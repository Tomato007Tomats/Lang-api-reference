<!-- URL: page_105 -->

Skip to content

Edit this page

# Wrappers

## ``langsmith.wrappers ¶

This module provides convenient tracing wrappers for popular libraries.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `wrap_anthropic` | Patch the Anthropic client to make it traceable. |
| `wrap_openai` | Patch the OpenAI client to make it traceable. |

### ``wrap\_anthropic ¶

```
wrap_anthropic(client: C, *, tracing_extra: TracingExtra | None = None) -> C
```

Patch the Anthropic client to make it traceable.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `client` | The client to patch.<br>**TYPE:**`Anthropic | AsyncAnthropic` |
| `tracing_extra` | Extra tracing information.<br>Defaults to None.<br>**TYPE:**`TracingExtra | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `C` | Union\[Anthropic, AsyncAnthropic\]: The patched client. |

Example:

```
.. code-block:: python

    import anthropic
    from langsmith import wrappers

    client = wrappers.wrap_anthropic(anthropic.Anthropic())

    # Use Anthropic client same as you normally would:
    system = "You are a helpful assistant."
    messages = [\
        {\
            "role": "user",\
            "content": "What physics breakthroughs do you predict will happen by 2300?",\
        }\
    ]
    completion = client.messages.create(
        model="claude-3-5-sonnet-latest",
        messages=messages,
        max_tokens=1000,
        system=system,
    )
    print(completion.content)

    # You can also use the streaming context manager:
    with client.messages.stream(
        model="claude-3-5-sonnet-latest",
        messages=messages,
        max_tokens=1000,
        system=system,
    ) as stream:
        for text in stream.text_stream:
            print(text, end="", flush=True)
        message = stream.get_final_message()
```

### ``wrap\_openai ¶

```
wrap_openai(
    client: C,
    *,
    tracing_extra: TracingExtra | None = None,
    chat_name: str = "ChatOpenAI",
    completions_name: str = "OpenAI",
) -> C
```

Patch the OpenAI client to make it traceable.

Supports

- Chat and Responses API's
- Sync and async OpenAI clients
- create() and parse() methods
- with and without streaming

| PARAMETER | DESCRIPTION |
| --- | --- |
| `client` | The client to patch.<br>**TYPE:**`OpenAI | AsyncOpenAI` |
| `tracing_extra` | Extra tracing information.<br>Defaults to None.<br>**TYPE:**`TracingExtra | None`**DEFAULT:**`None` |
| `chat_name` | The run name for the chat completions endpoint.<br>Defaults to "ChatOpenAI".<br>**TYPE:**`str`**DEFAULT:**`'ChatOpenAI'` |
| `completions_name` | The run name for the completions endpoint.<br>Defaults to "OpenAI".<br>**TYPE:**`str`**DEFAULT:**`'OpenAI'` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `C` | Union\[OpenAI, AsyncOpenAI\]: The patched client. |

Example:

```
.. code-block:: python

    import openai
    from langsmith import wrappers

    # Use OpenAI client same as you normally would.
    client = wrappers.wrap_openai(openai.OpenAI())

    # Chat API:
    messages = [\
        {"role": "system", "content": "You are a helpful assistant."},\
        {\
            "role": "user",\
            "content": "What physics breakthroughs do you predict will happen by 2300?",\
        },\
    ]
    completion = client.chat.completions.create(
        model="gpt-4o-mini", messages=messages
    )
    print(completion.choices[0].message.content)

    # Responses API:
    response = client.responses.create(
        model="gpt-4o-mini",
        messages=messages,
    )
    print(response.output_text)
```

.. versionchanged:: 0.3.16

```
Support for Responses API added.
```

Back to top