<!-- URL: page_149 -->

Skip to content

Edit this page

# `langchain-model-profiles` ¶

![PyPI - Version](https://pypi.org/project/langchain-model-profiles/#history)![PyPI - License](https://opensource.org/licenses/MIT)![PyPI - Downloads](https://pypistats.org/packages/langchain-model-profiles)

Reference documentation for the `langchain-model-profiles` package.

Beta package

This package is currently in development and the API is subject to change.

## ``langchain\_model\_profiles ¶

Model profiles entrypoint.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `get_model_profile` | Get the model capabilities for a given model. |

### ``ModelProfile ¶

Bases: `TypedDict`

Model profile.

#### ``max\_input\_tokens`instance-attribute`¶

```
max_input_tokens: int
```

Maximum context window (tokens)

#### ``image\_inputs`instance-attribute`¶

```
image_inputs: bool
```

Whether image inputs are supported.

#### ``image\_url\_inputs`instance-attribute`¶

```
image_url_inputs: bool
```

Whether image URL inputs
are supported.

#### ``pdf\_inputs`instance-attribute`¶

```
pdf_inputs: bool
```

Whether PDF inputs
are supported.

#### ``audio\_inputs`instance-attribute`¶

```
audio_inputs: bool
```

Whether audio inputs
are supported.

#### ``video\_inputs`instance-attribute`¶

```
video_inputs: bool
```

Whether video inputs
are supported.

#### ``image\_tool\_message`instance-attribute`¶

```
image_tool_message: bool
```

TODO: description.

#### ``pdf\_tool\_message`instance-attribute`¶

```
pdf_tool_message: bool
```

TODO: description.

#### ``max\_output\_tokens`instance-attribute`¶

```
max_output_tokens: int
```

Maximum output tokens

#### ``reasoning\_output`instance-attribute`¶

```
reasoning_output: bool
```

Whether the model supports reasoning / chain-of-thought

#### ``image\_outputs`instance-attribute`¶

```
image_outputs: bool
```

Whether image outputs
are supported.

#### ``audio\_outputs`instance-attribute`¶

```
audio_outputs: bool
```

Whether audio outputs
are supported.

#### ``video\_outputs`instance-attribute`¶

```
video_outputs: bool
```

Whether video outputs
are supported.

#### ``tool\_calling`instance-attribute`¶

```
tool_calling: bool
```

Whether the model supports tool calling

#### ``tool\_choice`instance-attribute`¶

```
tool_choice: bool
```

Whether the model supports tool choice

#### ``structured\_output`instance-attribute`¶

```
structured_output: bool
```

Whether the model supports a native structured output
feature

### ``get\_model\_profile ¶

```
get_model_profile(provider: str, model: str) -> ModelProfile | None
```

Get the model capabilities for a given model.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `provider` | Identifier for provider (e.g., `'openai'`, `'anthropic'`).<br>**TYPE:**`str` |
| `model` | Identifier for model (e.g., `'gpt-5'`,<br>`'claude-sonnet-4-5-20250929'`).<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelProfile | None` | The model capabilities or `None` if not found in the data. |

Back to top