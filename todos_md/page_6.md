<!-- URL: page_6 -->

Skip to content

Edit this page

# Base tests ¶

## ``langchain\_tests.unit\_tests.chat\_models.ChatModelTests ¶

Bases: `BaseStandardTests`

Base class for chat model tests.

| METHOD | DESCRIPTION |
| --- | --- |
| `model` | Model fixture. |
| `my_adder_tool` | Adder tool fixture. |
| `test_no_overrides_DO_NOT_OVERRIDE` | Test that no standard tests are overridden. |

### ``chat\_model\_class`abstractmethod``property`¶

```
chat_model_class: type[BaseChatModel]
```

The chat model class to test, e.g., `ChatParrotLink`.

### ``chat\_model\_params`property`¶

```
chat_model_params: dict
```

Initialization parameters for the chat model.

### ``standard\_chat\_model\_params`property`¶

```
standard_chat_model_params: dict
```

Standard chat model parameters.

### ``has\_tool\_calling`property`¶

```
has_tool_calling: bool
```

Whether the model supports tool calling.

### ``tool\_choice\_value`property`¶

```
tool_choice_value: str | None
```

(None or str) to use for tool choice when used in tests.

### ``has\_tool\_choice`property`¶

```
has_tool_choice: bool
```

Whether the model supports tool calling.

### ``has\_structured\_output`property`¶

```
has_structured_output: bool
```

Whether the chat model supports structured output.

### ``structured\_output\_kwargs`property`¶

```
structured_output_kwargs: dict
```

If specified, additional kwargs for `with_structured_output`.

### ``supports\_json\_mode`property`¶

```
supports_json_mode: bool
```

Whether the chat model supports JSON mode.

### ``supports\_image\_inputs`property`¶

```
supports_image_inputs: bool
```

Supports image inputs.

Whether the chat model supports image inputs, defaults to
`False`.

### ``supports\_image\_urls`property`¶

```
supports_image_urls: bool
```

Supports image inputs from URLs.

Whether the chat model supports image inputs from URLs, defaults to
`False`.

### ``supports\_pdf\_inputs`property`¶

```
supports_pdf_inputs: bool
```

Whether the chat model supports PDF inputs, defaults to `False`.

### ``supports\_audio\_inputs`property`¶

```
supports_audio_inputs: bool
```

Supports audio inputs.

Whether the chat model supports audio inputs, defaults to `False`.

### ``supports\_video\_inputs`property`¶

```
supports_video_inputs: bool
```

Supports video inputs.

Whether the chat model supports video inputs, defaults to `False`.

No current tests are written for this feature.

### ``returns\_usage\_metadata`property`¶

```
returns_usage_metadata: bool
```

Returns usage metadata.

Whether the chat model returns usage metadata on invoke and streaming
responses.

### ``supports\_anthropic\_inputs`property`¶

```
supports_anthropic_inputs: bool
```

Whether the chat model supports Anthropic-style inputs.

### ``supports\_image\_tool\_message`property`¶

```
supports_image_tool_message: bool
```

Supports image `ToolMessage` objects.

Whether the chat model supports `ToolMessage` objects that include image
content.

### ``supports\_pdf\_tool\_message`property`¶

```
supports_pdf_tool_message: bool
```

Supports PDF `ToolMessage` objects.

Whether the chat model supports `ToolMessage` objects that include PDF
content.

### ``enable\_vcr\_tests`property`¶

```
enable_vcr_tests: bool
```

Whether to enable VCR tests for the chat model.

Warning

See `enable_vcr_tests` dropdown `above <ChatModelTests>` for more
information.

### ``supported\_usage\_metadata\_details`property`¶

```
supported_usage_metadata_details: dict[\
    Literal["invoke", "stream"],\
    list[\
        Literal[\
            "audio_input",\
            "audio_output",\
            "reasoning_output",\
            "cache_read_input",\
            "cache_creation_input",\
        ]\
    ],\
]
```

Supported usage metadata details.

What usage metadata details are emitted in invoke and stream. Only needs to be
overridden if these details are returned by the model.

### ``model ¶

```
model(request: Any) -> BaseChatModel
```

Model fixture.

### ``my\_adder\_tool ¶

```
my_adder_tool() -> BaseTool
```

Adder tool fixture.

### ``test\_no\_overrides\_DO\_NOT\_OVERRIDE ¶

```
test_no_overrides_DO_NOT_OVERRIDE() -> None
```

Test that no standard tests are overridden.

## ``langchain\_tests.unit\_tests.embeddings.EmbeddingsTests ¶

Bases: `BaseStandardTests`

Embeddings tests base class.

| METHOD | DESCRIPTION |
| --- | --- |
| `model` | Embeddings model fixture. |
| `test_no_overrides_DO_NOT_OVERRIDE` | Test that no standard tests are overridden. |

### ``embeddings\_class`abstractmethod``property`¶

```
embeddings_class: type[Embeddings]
```

Embeddings class.

### ``embedding\_model\_params`property`¶

```
embedding_model_params: dict
```

Embeddings model parameters.

### ``model ¶

```
model() -> Embeddings
```

Embeddings model fixture.

### ``test\_no\_overrides\_DO\_NOT\_OVERRIDE ¶

```
test_no_overrides_DO_NOT_OVERRIDE() -> None
```

Test that no standard tests are overridden.

## ``langchain\_tests.unit\_tests.tools.ToolsTests ¶

Bases: `BaseStandardTests`

Base class for testing tools.

This won't show in the documentation, but the docstrings will be inherited by
subclasses.

| METHOD | DESCRIPTION |
| --- | --- |
| `tool` | Tool fixture. |
| `test_no_overrides_DO_NOT_OVERRIDE` | Test that no standard tests are overridden. |

### ``tool\_constructor`abstractmethod``property`¶

```
tool_constructor: type[BaseTool] | BaseTool
```

Returns a class or instance of a tool to be tested.

### ``tool\_constructor\_params`property`¶

```
tool_constructor_params: dict
```

Returns a dictionary of parameters to pass to the tool constructor.

### ``tool\_invoke\_params\_example`property`¶

```
tool_invoke_params_example: dict
```

Returns a dictionary representing the "args" of an example tool call.

This should NOT be a `ToolCall` dict - it should not have
`{"name", "id", "args"}` keys.

### ``tool ¶

```
tool() -> BaseTool
```

Tool fixture.

### ``test\_no\_overrides\_DO\_NOT\_OVERRIDE ¶

```
test_no_overrides_DO_NOT_OVERRIDE() -> None
```

Test that no standard tests are overridden.

## ``langchain\_tests.base.BaseStandardTests ¶

Base class for standard tests.

| METHOD | DESCRIPTION |
| --- | --- |
| `test_no_overrides_DO_NOT_OVERRIDE` | Test that no standard tests are overridden. |

### ``test\_no\_overrides\_DO\_NOT\_OVERRIDE ¶

```
test_no_overrides_DO_NOT_OVERRIDE() -> None
```

Test that no standard tests are overridden.

Back to top