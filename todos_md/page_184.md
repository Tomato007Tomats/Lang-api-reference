<!-- URL: page_184 -->

Skip to content

Edit this page

# Language models

## ``langchain\_core.language\_models ¶

Language models.

LangChain has two main classes to work with language models: chat models and
"old-fashioned" LLMs.

**Chat models**

Language models that use a sequence of messages as inputs and return chat messages
as outputs (as opposed to using plain text).

Chat models support the assignment of distinct roles to conversation messages, helping
to distinguish messages from the AI, users, and instructions such as system messages.

The key abstraction for chat models is `BaseChatModel`. Implementations should inherit
from this class.

See existing chat model integrations.

**LLMs**

Language models that takes a string as input and returns a string.
These are traditionally older models (newer models generally are chat models).

Although the underlying models are string in, string out, the LangChain wrappers also
allow these models to take messages as input. This gives them the same interface as
chat models. When messages are passed in as input, they will be formatted into a string
under the hood before being passed to the underlying model.

### ``BaseChatModel ¶

Bases: `BaseLanguageModel[AIMessage]`, `ABC`

<code class="doc-symbol doc-symbol-heading doc-symbol-class"></code>            <span class="doc doc-object-name doc-class-name">BaseChatModel</span> (<code>langchain_core.language_models.BaseChatModel</code>)")<code class="doc-symbol doc-symbol-heading doc-symbol-class"></code>            <span class="doc doc-object-name doc-class-name">BaseLanguageModel</span> (<code>langchain_core.language_models.base.BaseLanguageModel</code>)")<code class="doc-symbol doc-symbol-heading doc-symbol-class"></code>            <span class="doc doc-object-name doc-class-name">langchain_core.runnables.base.RunnableSerializable</span><code class="doc-symbol doc-symbol-heading doc-symbol-class"></code>            <span class="doc doc-object-name doc-class-name">langchain_core.load.serializable.Serializable</span><code class="doc-symbol doc-symbol-heading doc-symbol-class"></code>            <span class="doc doc-object-name doc-class-name">langchain_core.runnables.base.Runnable</span>

BaseChatModel BaseLanguageModel RunnableSerializable Serializable Runnable

Base class for chat models.

Key imperative methods

Methods that actually call the underlying model.

This table provides a brief overview of the main imperative methods. Please see the base `Runnable` reference for full documentation.

| Method | Input | Output | Description |
| --- | --- | --- | --- |
| `invoke` | `str` \| `list[dict | tuple | BaseMessage]` \| `PromptValue` | `BaseMessage` | A single chat model call. |
| `ainvoke` | `'''` | `BaseMessage` | Defaults to running `invoke` in an async executor. |
| `stream` | `'''` | `Iterator[BaseMessageChunk]` | Defaults to yielding output of `invoke`. |
| `astream` | `'''` | `AsyncIterator[BaseMessageChunk]` | Defaults to yielding output of `ainvoke`. |
| `astream_events` | `'''` | `AsyncIterator[StreamEvent]` | Event types: `on_chat_model_start`, `on_chat_model_stream`, `on_chat_model_end`. |
| `batch` | `list[''']` | `list[BaseMessage]` | Defaults to running `invoke` in concurrent threads. |
| `abatch` | `list[''']` | `list[BaseMessage]` | Defaults to running `ainvoke` in concurrent threads. |
| `batch_as_completed` | `list[''']` | `Iterator[tuple[int, Union[BaseMessage, Exception]]]` | Defaults to running `invoke` in concurrent threads. |
| `abatch_as_completed` | `list[''']` | `AsyncIterator[tuple[int, Union[BaseMessage, Exception]]]` | Defaults to running `ainvoke` in concurrent threads. |

Key declarative methods

Methods for creating another `Runnable` using the chat model.

This table provides a brief overview of the main declarative methods. Please see the reference for each method for full documentation.

| Method | Description |
| --- | --- |
| `bind_tools` | Create chat model that can call tools. |
| `with_structured_output` | Create wrapper that structures model output using schema. |
| `with_retry` | Create wrapper that retries model calls on failure. |
| `with_fallbacks` | Create wrapper that falls back to other models on failure. |
| `configurable_fields` | Specify init args of the model that can be configured at runtime via the `RunnableConfig`. |
| `configurable_alternatives` | Specify alternative models which can be swapped in at runtime via the `RunnableConfig`. |

Creating custom chat model

Custom chat model implementations should inherit from this class.
Please reference the table below for information about which
methods and properties are required or optional for implementations.

| Method/Property | Description | Required |
| --- | --- | --- |
| `_generate` | Use to generate a chat result from a prompt | Required |
| `_llm_type` (property) | Used to uniquely identify the type of the model. Used for logging. | Required |
| `_identifying_params` (property) | Represent model parameterization for tracing purposes. | Optional |
| `_stream` | Use to implement streaming | Optional |
| `_agenerate` | Use to implement a native async method | Optional |
| `_astream` | Use to implement async version of `_stream` | Optional |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_name` | Get the name of the `Runnable`. |
| `get_input_schema` | Get a Pydantic model that can be used to validate input to the `Runnable`. |
| `get_input_jsonschema` | Get a JSON schema that represents the input to the `Runnable`. |
| `get_output_schema` | Get a Pydantic model that can be used to validate output to the `Runnable`. |
| `get_output_jsonschema` | Get a JSON schema that represents the output of the `Runnable`. |
| `config_schema` | The type of config this `Runnable` accepts specified as a Pydantic model. |
| `get_config_jsonschema` | Get a JSON schema that represents the config of the `Runnable`. |
| `get_graph` | Return a graph representation of this `Runnable`. |
| `get_prompts` | Return a list of prompts used by this `Runnable`. |
| `__or__` | Runnable "or" operator. |
| `__ror__` | Runnable "reverse-or" operator. |
| `pipe` | Pipe `Runnable` objects. |
| `pick` | Pick keys from the output `dict` of this `Runnable`. |
| `assign` | Assigns new fields to the `dict` output of this `Runnable`. |
| `batch` | Default implementation runs invoke in parallel using a thread pool executor. |
| `batch_as_completed` | Run `invoke` in parallel on a list of inputs. |
| `abatch` | Default implementation runs `ainvoke` in parallel using `asyncio.gather`. |
| `abatch_as_completed` | Run `ainvoke` in parallel on a list of inputs. |
| `astream_log` | Stream all output from a `Runnable`, as reported to the callback system. |
| `astream_events` | Generate a stream of events. |
| `transform` | Transform inputs to outputs. |
| `atransform` | Transform inputs to outputs. |
| `bind` | Bind arguments to a `Runnable`, returning a new `Runnable`. |
| `with_config` | Bind config to a `Runnable`, returning a new `Runnable`. |
| `with_listeners` | Bind lifecycle listeners to a `Runnable`, returning a new `Runnable`. |
| `with_alisteners` | Bind async lifecycle listeners to a `Runnable`. |
| `with_types` | Bind input and output types to a `Runnable`, returning a new `Runnable`. |
| `with_retry` | Create a new `Runnable` that retries the original `Runnable` on exceptions. |
| `map` | Return a new `Runnable` that maps a list of inputs to a list of outputs. |
| `with_fallbacks` | Add fallbacks to a `Runnable`, returning a new `Runnable`. |
| `as_tool` | Create a `BaseTool` from a `Runnable`. |
| `__init__` |  |
| `is_lc_serializable` | Is this class serializable? |
| `get_lc_namespace` | Get the namespace of the LangChain object. |
| `lc_id` | Return a unique identifier for this class for serialization purposes. |
| `to_json` | Serialize the `Runnable` to JSON. |
| `to_json_not_implemented` | Serialize a "not implemented" object. |
| `configurable_fields` | Configure particular `Runnable` fields at runtime. |
| `configurable_alternatives` | Configure alternatives for `Runnable` objects that can be set at runtime. |
| `set_verbose` | If verbose is `None`, set it. |
| `get_token_ids` | Return the ordered IDs of the tokens in a text. |
| `get_num_tokens` | Get the number of tokens present in the text. |
| `get_num_tokens_from_messages` | Get the number of tokens in the messages. |
| `invoke` | Transform a single input into an output. |
| `ainvoke` | Transform a single input into an output. |
| `stream` | Default implementation of `stream`, which calls `invoke`. |
| `astream` | Default implementation of `astream`, which calls `ainvoke`. |
| `generate` | Pass a sequence of prompts to the model and return model generations. |
| `agenerate` | Asynchronously pass a sequence of prompts to a model and return generations. |
| `generate_prompt` | Pass a sequence of prompts to the model and return model generations. |
| `agenerate_prompt` | Asynchronously pass a sequence of prompts and return model generations. |
| `dict` | Return a dictionary of the LLM. |
| `bind_tools` | Bind tools to the model. |
| `with_structured_output` | Model wrapper that returns outputs formatted to match the given schema. |

#### ``name`class-attribute``instance-attribute`¶

```
name: str | None = None
```

The name of the `Runnable`. Used for debugging and tracing.

#### ``InputType`property`¶

```
InputType: TypeAlias
```

Get the input type for this `Runnable`.

#### ``input\_schema`property`¶

```
input_schema: type[BaseModel]
```

The type of input this `Runnable` accepts specified as a Pydantic model.

#### ``output\_schema`property`¶

```
output_schema: type[BaseModel]
```

Output schema.

The type of output this `Runnable` produces specified as a Pydantic model.

#### ``config\_specs`property`¶

```
config_specs: list[ConfigurableFieldSpec]
```

List configurable fields for this `Runnable`.

#### ``lc\_secrets`property`¶

```
lc_secrets: dict[str, str]
```

A map of constructor argument names to secret ids.

For example, `{"openai_api_key": "OPENAI_API_KEY"}`

#### ``lc\_attributes`property`¶

```
lc_attributes: dict
```

List of attribute names that should be included in the serialized kwargs.

These attributes must be accepted by the constructor.

Default is an empty dictionary.

#### ``cache`class-attribute``instance-attribute`¶

```
cache: BaseCache | bool | None = Field(default=None, exclude=True)
```

Whether to cache the response.

- If `True`, will use the global cache.
- If `False`, will not use a cache
- If `None`, will use the global cache if it's set, otherwise no cache.
- If instance of `BaseCache`, will use the provided cache.

Caching is not currently supported for streaming methods of models.

#### ``verbose`class-attribute``instance-attribute`¶

```
verbose: bool = Field(default_factory=_get_verbosity, exclude=True, repr=False)
```

Whether to print out response text.

#### ``callbacks`class-attribute``instance-attribute`¶

```
callbacks: Callbacks = Field(default=None, exclude=True)
```

Callbacks to add to the run trace.

#### ``tags`class-attribute``instance-attribute`¶

```
tags: list[str] | None = Field(default=None, exclude=True)
```

Tags to add to the run trace.

#### ``metadata`class-attribute``instance-attribute`¶

```
metadata: dict[str, Any] | None = Field(default=None, exclude=True)
```

Metadata to add to the run trace.

#### ``custom\_get\_token\_ids`class-attribute``instance-attribute`¶

```
custom_get_token_ids: Callable[[str], list[int]] | None = Field(
    default=None, exclude=True
)
```

Optional encoder to use for counting tokens.

#### ``rate\_limiter`class-attribute``instance-attribute`¶

```
rate_limiter: BaseRateLimiter | None = Field(default=None, exclude=True)
```

An optional rate limiter to use for limiting the number of requests.

#### ``disable\_streaming`class-attribute``instance-attribute`¶

```
disable_streaming: bool | Literal['tool_calling'] = False
```

Whether to disable streaming for this model.

If streaming is bypassed, then `stream`/`astream`/`astream_events` will
defer to `invoke`/`ainvoke`.

- If `True`, will always bypass streaming case.
- If `'tool_calling'`, will bypass streaming case only when the model is called
with a `tools` keyword argument. In other words, LangChain will automatically
switch to non-streaming behavior (`invoke`) only when the tools argument is
provided. This offers the best of both worlds.
- If `False` (Default), will always use streaming case if available.

The main reason for this flag is that code might be written using `stream` and
a user may want to swap out a given model for another model whose the implementation
does not properly support streaming.

#### ``output\_version`class-attribute``instance-attribute`¶

```
output_version: str | None = Field(
    default_factory=from_env("LC_OUTPUT_VERSION", default=None)
)
```

Version of `AIMessage` output format to store in message content.

`AIMessage.content_blocks` will lazily parse the contents of `content` into a
standard format. This flag can be used to additionally store the standard format
in message content, e.g., for serialization purposes.

Supported values:

- `'v0'`: provider-specific format in content (can lazily-parse with
`content_blocks`)
- `'v1'`: standardized format in content (consistent with `content_blocks`)

Partner packages (e.g.,
`langchain-openai`) can also use this
field to roll out new content formats in a backward-compatible way.

Added in version 1.0

#### ``OutputType`property`¶

```
OutputType: Any
```

Get the output type for this `Runnable`.

#### ``profile`property`¶

```
profile: ModelProfile
```

Return profiling information for the model.

This property relies on the `langchain-model-profiles` package to retrieve chat
model capabilities, such as context window sizes and supported features.

| RAISES | DESCRIPTION |
| --- | --- |
| `ImportError` | If `langchain-model-profiles` is not installed. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelProfile` | A `ModelProfile` object containing profiling information for the model. |

#### ``get\_name ¶

```
get_name(suffix: str | None = None, *, name: str | None = None) -> str
```

Get the name of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `suffix` ¶ "Copy anchor link to this section for reference") | An optional suffix to append to the name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| ##### `name` ¶ "Copy anchor link to this section for reference") | An optional name to use instead of the `Runnable`'s name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | The name of the `Runnable`. |

#### ``get\_input\_schema ¶

```
get_input_schema(config: RunnableConfig | None = None) -> type[BaseModel]
```

Get a Pydantic model that can be used to validate input to the `Runnable`.

`Runnable` objects that leverage the `configurable_fields` and
`configurable_alternatives` methods will have a dynamic input schema that
depends on which configuration the `Runnable` is invoked with.

This method allows to get an input schema for a specific configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `config` ¶ "Copy anchor link to this section for reference") | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate input. |

#### ``get\_input\_jsonschema ¶

```
get_input_jsonschema(config: RunnableConfig | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the input to the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `config` ¶ "Copy anchor link to this section for reference") | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the input to the `Runnable`. |

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

runnable = RunnableLambda(add_one)

print(runnable.get_input_jsonschema())
```

Added in version 0.3.0

#### ``get\_output\_schema ¶

```
get_output_schema(config: RunnableConfig | None = None) -> type[BaseModel]
```

Get a Pydantic model that can be used to validate output to the `Runnable`.

`Runnable` objects that leverage the `configurable_fields` and
`configurable_alternatives` methods will have a dynamic output schema that
depends on which configuration the `Runnable` is invoked with.

This method allows to get an output schema for a specific configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `config` ¶ "Copy anchor link to this section for reference") | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate output. |

#### ``get\_output\_jsonschema ¶

```
get_output_jsonschema(config: RunnableConfig | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the output of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `config` ¶ "Copy anchor link to this section for reference") | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the output of the `Runnable`. |

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

runnable = RunnableLambda(add_one)

print(runnable.get_output_jsonschema())
```

Added in version 0.3.0

#### ``config\_schema ¶

```
config_schema(*, include: Sequence[str] | None = None) -> type[BaseModel]
```

The type of config this `Runnable` accepts specified as a Pydantic model.

To mark a field as configurable, see the `configurable_fields`
and `configurable_alternatives` methods.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `include` ¶ "Copy anchor link to this section for reference") | A list of fields to include in the config schema.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate config. |

#### ``get\_config\_jsonschema ¶

```
get_config_jsonschema(*, include: Sequence[str] | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the config of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `include` ¶ "Copy anchor link to this section for reference") | A list of fields to include in the config schema.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the config of the `Runnable`. |

Added in version 0.3.0

#### ``get\_graph ¶

```
get_graph(config: RunnableConfig | None = None) -> Graph
```

Return a graph representation of this `Runnable`.

#### ``get\_prompts ¶

```
get_prompts(config: RunnableConfig | None = None) -> list[BasePromptTemplate]
```

Return a list of prompts used by this `Runnable`.

#### ``\_\_or\_\_ ¶

```
__or__(
    other: Runnable[Any, Other]
    | Callable[[Iterator[Any]], Iterator[Other]]
    | Callable[[AsyncIterator[Any]], AsyncIterator[Other]]
    | Callable[[Any], Other]
    | Mapping[str, Runnable[Any, Other] | Callable[[Any], Other] | Any],
) -> RunnableSerializable[Input, Other]
```

Runnable "or" operator.

Compose this `Runnable` with another object to create a
`RunnableSequence`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `other` ¶ "Copy anchor link to this section for reference") | Another `Runnable` or a `Runnable`-like object.<br>**TYPE:**`Runnable[Any, Other] | Callable[[Iterator[Any]], Iterator[Other]] | Callable[[AsyncIterator[Any]], AsyncIterator[Other]] | Callable[[Any], Other] | Mapping[str, Runnable[Any, Other] | Callable[[Any], Other] | Any]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Other]` | A new `Runnable`. |

#### ``\_\_ror\_\_ ¶

```
__ror__(
    other: Runnable[Other, Any]
    | Callable[[Iterator[Other]], Iterator[Any]]
    | Callable[[AsyncIterator[Other]], AsyncIterator[Any]]
    | Callable[[Other], Any]
    | Mapping[str, Runnable[Other, Any] | Callable[[Other], Any] | Any],
) -> RunnableSerializable[Other, Output]
```

Runnable "reverse-or" operator.

Compose this `Runnable` with another object to create a
`RunnableSequence`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `other` ¶ "Copy anchor link to this section for reference") | Another `Runnable` or a `Runnable`-like object.<br>**TYPE:**`Runnable[Other, Any] | Callable[[Iterator[Other]], Iterator[Any]] | Callable[[AsyncIterator[Other]], AsyncIterator[Any]] | Callable[[Other], Any] | Mapping[str, Runnable[Other, Any] | Callable[[Other], Any] | Any]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Other, Output]` | A new `Runnable`. |

#### ``pipe ¶

```
pipe(
    *others: Runnable[Any, Other] | Callable[[Any], Other], name: str | None = None
) -> RunnableSerializable[Input, Other]
```

Pipe `Runnable` objects.

Compose this `Runnable` with `Runnable`-like objects to make a
`RunnableSequence`.

Equivalent to `RunnableSequence(self, *others)` or `self | others[0] | ...`

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

def mul_two(x: int) -> int:
    return x * 2

runnable_1 = RunnableLambda(add_one)
runnable_2 = RunnableLambda(mul_two)
sequence = runnable_1.pipe(runnable_2)
# Or equivalently:
# sequence = runnable_1 | runnable_2
# sequence = RunnableSequence(first=runnable_1, last=runnable_2)
sequence.invoke(1)
await sequence.ainvoke(1)
# -> 4

sequence.batch([1, 2, 3])
await sequence.abatch([1, 2, 3])
# -> [4, 6, 8]
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `*others` ¶ "Copy anchor link to this section for reference") | Other `Runnable` or `Runnable`-like objects to compose<br>**TYPE:**`Runnable[Any, Other] | Callable[[Any], Other]`**DEFAULT:**`()` |
| ##### `name` ¶ "Copy anchor link to this section for reference") | An optional name for the resulting `RunnableSequence`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Other]` | A new `Runnable`. |

#### ``pick ¶

```
pick(keys: str | list[str]) -> RunnableSerializable[Any, Any]
```

Pick keys from the output `dict` of this `Runnable`.

Pick a single key:

```
import json

from langchain_core.runnables import RunnableLambda, RunnableMap

as_str = RunnableLambda(str)
as_json = RunnableLambda(json.loads)
chain = RunnableMap(str=as_str, json=as_json)

chain.invoke("[1, 2, 3]")
# -> {"str": "[1, 2, 3]", "json": [1, 2, 3]}

json_only_chain = chain.pick("json")
json_only_chain.invoke("[1, 2, 3]")
# -> [1, 2, 3]
```

Pick a list of keys:

```
from typing import Any

import json

from langchain_core.runnables import RunnableLambda, RunnableMap

as_str = RunnableLambda(str)
as_json = RunnableLambda(json.loads)

def as_bytes(x: Any) -> bytes:
    return bytes(x, "utf-8")

chain = RunnableMap(str=as_str, json=as_json, bytes=RunnableLambda(as_bytes))

chain.invoke("[1, 2, 3]")
# -> {"str": "[1, 2, 3]", "json": [1, 2, 3], "bytes": b"[1, 2, 3]"}

json_and_bytes_chain = chain.pick(["json", "bytes"])
json_and_bytes_chain.invoke("[1, 2, 3]")
# -> {"json": [1, 2, 3], "bytes": b"[1, 2, 3]"}
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `keys` ¶ "Copy anchor link to this section for reference") | A key or list of keys to pick from the output dict.<br>**TYPE:**`str | list[str]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Any, Any]` | a new `Runnable`. |

#### ``assign ¶

```
assign(
    **kwargs: Runnable[dict[str, Any], Any]
    | Callable[[dict[str, Any]], Any]
    | Mapping[str, Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any]],
) -> RunnableSerializable[Any, Any]
```

Assigns new fields to the `dict` output of this `Runnable`.

```
from langchain_core.language_models.fake import FakeStreamingListLLM
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import SystemMessagePromptTemplate
from langchain_core.runnables import Runnable
from operator import itemgetter

prompt = (
    SystemMessagePromptTemplate.from_template("You are a nice assistant.")
    + "{question}"
)
model = FakeStreamingListLLM(responses=["foo-lish"])

chain: Runnable = prompt | model | {"str": StrOutputParser()}

chain_with_assign = chain.assign(hello=itemgetter("str") | model)

print(chain_with_assign.input_schema.model_json_schema())
# {'title': 'PromptInput', 'type': 'object', 'properties':
{'question': {'title': 'Question', 'type': 'string'}}}
print(chain_with_assign.output_schema.model_json_schema())
# {'title': 'RunnableSequenceOutput', 'type': 'object', 'properties':
{'str': {'title': 'Str',
'type': 'string'}, 'hello': {'title': 'Hello', 'type': 'string'}}}
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | A mapping of keys to `Runnable` or `Runnable`-like objects<br>that will be invoked with the entire output dict of this `Runnable`.<br>**TYPE:**`Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any] | Mapping[str, Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any]]`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Any, Any]` | A new `Runnable`. |

#### ``batch ¶

```
batch(
    inputs: list[Input],
    config: RunnableConfig | list[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> list[Output]
```

Default implementation runs invoke in parallel using a thread pool executor.

The default implementation of batch works well for IO bound runnables.

Subclasses must override this method if they can batch more efficiently;
e.g., if the underlying `Runnable` uses an API which supports a batch mode.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `inputs` ¶ "Copy anchor link to this section for reference") | A list of inputs to the `Runnable`.<br>**TYPE:**`list[Input]` |
| ##### `config` ¶ "Copy anchor link to this section for reference") | A config to use when invoking the `Runnable`. The config supports<br>standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work<br>to do in parallel, and other keys. Please refer to the<br>`RunnableConfig` for more details.<br>**TYPE:**`RunnableConfig | list[RunnableConfig] | None`**DEFAULT:**`None` |
| ##### `return_exceptions` ¶ "Copy anchor link to this section for reference") | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Output]` | A list of outputs from the `Runnable`. |

#### ``batch\_as\_completed ¶

```
batch_as_completed(
    inputs: Sequence[Input],
    config: RunnableConfig | Sequence[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> Iterator[tuple[int, Output | Exception]]
```

Run `invoke` in parallel on a list of inputs.

Yields results as they complete.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `inputs` ¶ "Copy anchor link to this section for reference") | A list of inputs to the `Runnable`.<br>**TYPE:**`Sequence[Input]` |
| ##### `config` ¶ "Copy anchor link to this section for reference") | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | Sequence[RunnableConfig] | None`**DEFAULT:**`None` |
| ##### `return_exceptions` ¶ "Copy anchor link to this section for reference") | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `tuple[int, Output | Exception]` | Tuples of the index of the input and the output from the `Runnable`. |

#### ``abatch`async`¶

```
abatch(
    inputs: list[Input],
    config: RunnableConfig | list[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> list[Output]
```

Default implementation runs `ainvoke` in parallel using `asyncio.gather`.

The default implementation of `batch` works well for IO bound runnables.

Subclasses must override this method if they can batch more efficiently;
e.g., if the underlying `Runnable` uses an API which supports a batch mode.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `inputs` ¶ "Copy anchor link to this section for reference") | A list of inputs to the `Runnable`.<br>**TYPE:**`list[Input]` |
| ##### `config` ¶ "Copy anchor link to this section for reference") | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | list[RunnableConfig] | None`**DEFAULT:**`None` |
| ##### `return_exceptions` ¶ "Copy anchor link to this section for reference") | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Output]` | A list of outputs from the `Runnable`. |

#### ``abatch\_as\_completed`async`¶

```
abatch_as_completed(
    inputs: Sequence[Input],
    config: RunnableConfig | Sequence[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> AsyncIterator[tuple[int, Output | Exception]]
```

Run `ainvoke` in parallel on a list of inputs.

Yields results as they complete.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `inputs` ¶ "Copy anchor link to this section for reference") | A list of inputs to the `Runnable`.<br>**TYPE:**`Sequence[Input]` |
| ##### `config` ¶ "Copy anchor link to this section for reference") | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | Sequence[RunnableConfig] | None`**DEFAULT:**`None` |
| ##### `return_exceptions` ¶ "Copy anchor link to this section for reference") | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[tuple[int, Output | Exception]]` | A tuple of the index of the input and the output from the `Runnable`. |

#### ``astream\_log`async`¶

```
astream_log(
    input: Any,
    config: RunnableConfig | None = None,
    *,
    diff: bool = True,
    with_streamed_output_list: bool = True,
    include_names: Sequence[str] | None = None,
    include_types: Sequence[str] | None = None,
    include_tags: Sequence[str] | None = None,
    exclude_names: Sequence[str] | None = None,
    exclude_types: Sequence[str] | None = None,
    exclude_tags: Sequence[str] | None = None,
    **kwargs: Any,
) -> AsyncIterator[RunLogPatch] | AsyncIterator[RunLog]
```

Stream all output from a `Runnable`, as reported to the callback system.

This includes all inner runs of LLMs, Retrievers, Tools, etc.

Output is streamed as Log objects, which include a list of
Jsonpatch ops that describe how the state of the run has changed in each
step, and the final state of the run.

The Jsonpatch ops can be applied in order to construct state.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `input` ¶ "Copy anchor link to this section for reference") | The input to the `Runnable`.<br>**TYPE:**`Any` |
| ##### `config` ¶ "Copy anchor link to this section for reference") | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| ##### `diff` ¶ "Copy anchor link to this section for reference") | Whether to yield diffs between each step or the current state.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| ##### `with_streamed_output_list` ¶ "Copy anchor link to this section for reference") | Whether to yield the `streamed_output` list.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| ##### `include_names` ¶ "Copy anchor link to this section for reference") | Only include logs with these names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| ##### `include_types` ¶ "Copy anchor link to this section for reference") | Only include logs with these types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| ##### `include_tags` ¶ "Copy anchor link to this section for reference") | Only include logs with these tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| ##### `exclude_names` ¶ "Copy anchor link to this section for reference") | Exclude logs with these names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| ##### `exclude_types` ¶ "Copy anchor link to this section for reference") | Exclude logs with these types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| ##### `exclude_tags` ¶ "Copy anchor link to this section for reference") | Exclude logs with these tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[RunLogPatch] | AsyncIterator[RunLog]` | A `RunLogPatch` or `RunLog` object. |

#### ``astream\_events`async`¶

```
astream_events(
    input: Any,
    config: RunnableConfig | None = None,
    *,
    version: Literal["v1", "v2"] = "v2",
    include_names: Sequence[str] | None = None,
    include_types: Sequence[str] | None = None,
    include_tags: Sequence[str] | None = None,
    exclude_names: Sequence[str] | None = None,
    exclude_types: Sequence[str] | None = None,
    exclude_tags: Sequence[str] | None = None,
    **kwargs: Any,
) -> AsyncIterator[StreamEvent]
```

Generate a stream of events.

Use to create an iterator over `StreamEvent` that provide real-time information
about the progress of the `Runnable`, including `StreamEvent` from intermediate
results.

A `StreamEvent` is a dictionary with the following schema:

- `event`: Event names are of the format:
`on_[runnable_type]_(start|stream|end)`.
- `name`: The name of the `Runnable` that generated the event.
- `run_id`: Randomly generated ID associated with the given execution of the
`Runnable` that emitted the event. A child `Runnable` that gets invoked as
part of the execution of a parent `Runnable` is assigned its own unique ID.
- `parent_ids`: The IDs of the parent runnables that generated the event. The
root `Runnable` will have an empty list. The order of the parent IDs is from
the root to the immediate parent. Only available for v2 version of the API.
The v1 version of the API will return an empty list.
- `tags`: The tags of the `Runnable` that generated the event.
- `metadata`: The metadata of the `Runnable` that generated the event.
- `data`: The data associated with the event. The contents of this field
depend on the type of event. See the table below for more details.

Below is a table that illustrates some events that might be emitted by various
chains. Metadata fields have been omitted from the table for brevity.
Chain definitions have been included after the table.

Note

This reference table is for the v2 version of the schema.

| event | name | chunk | input | output |
| --- | --- | --- | --- | --- |
| `on_chat_model_start` | `'[model name]'` |  | `{"messages": [[SystemMessage, HumanMessage]]}` |  |
| `on_chat_model_stream` | `'[model name]'` | `AIMessageChunk(content="hello")` |  |  |
| `on_chat_model_end` | `'[model name]'` |  | `{"messages": [[SystemMessage, HumanMessage]]}` | `AIMessageChunk(content="hello world")` |
| `on_llm_start` | `'[model name]'` |  | `{'input': 'hello'}` |  |
| `on_llm_stream` | `'[model name]'` | `'Hello'` |  |  |
| `on_llm_end` | `'[model name]'` |  | `'Hello human!'` |  |
| `on_chain_start` | `'format_docs'` |  |  |  |
| `on_chain_stream` | `'format_docs'` | `'hello world!, goodbye world!'` |  |  |
| `on_chain_end` | `'format_docs'` |  | `[Document(...)]` | `'hello world!, goodbye world!'` |
| `on_tool_start` | `'some_tool'` |  | `{"x": 1, "y": "2"}` |  |
| `on_tool_end` | `'some_tool'` |  |  | `{"x": 1, "y": "2"}` |
| `on_retriever_start` | `'[retriever name]'` |  | `{"query": "hello"}` |  |
| `on_retriever_end` | `'[retriever name]'` |  | `{"query": "hello"}` | `[Document(...), ..]` |
| `on_prompt_start` | `'[template_name]'` |  | `{"question": "hello"}` |  |
| `on_prompt_end` | `'[template_name]'` |  | `{"question": "hello"}` | `ChatPromptValue(messages: [SystemMessage, ...])` |

In addition to the standard events, users can also dispatch custom events (see example below).

Custom events will be only be surfaced with in the v2 version of the API!

A custom event has following format:

| Attribute | Type | Description |
| --- | --- | --- |
| `name` | `str` | A user defined name for the event. |
| `data` | `Any` | The data associated with the event. This can be anything, though we suggest making it JSON serializable. |

Here are declarations associated with the standard events shown above:

`format_docs`:

```
def format_docs(docs: list[Document]) -> str:
    '''Format the docs.'''
    return ", ".join([doc.page_content for doc in docs])

format_docs = RunnableLambda(format_docs)
```

`some_tool`:

```
@tool
def some_tool(x: int, y: str) -> dict:
    '''Some_tool.'''
    return {"x": x, "y": y}
```

`prompt`:

```
template = ChatPromptTemplate.from_messages(
    [\
        ("system", "You are Cat Agent 007"),\
        ("human", "{question}"),\
    ]
).with_config({"run_name": "my_template", "tags": ["my_template"]})
```

For instance:

```
from langchain_core.runnables import RunnableLambda

async def reverse(s: str) -> str:
    return s[::-1]

chain = RunnableLambda(func=reverse)

events = [event async for event in chain.astream_events("hello", version="v2")]

# Will produce the following events
# (run_id, and parent_ids has been omitted for brevity):
[\
    {\
        "data": {"input": "hello"},\
        "event": "on_chain_start",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
    {\
        "data": {"chunk": "olleh"},\
        "event": "on_chain_stream",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
    {\
        "data": {"output": "olleh"},\
        "event": "on_chain_end",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
]
```

Example: Dispatch Custom Event

```
from langchain_core.callbacks.manager import (
    adispatch_custom_event,
)
from langchain_core.runnables import RunnableLambda, RunnableConfig
import asyncio

async def slow_thing(some_input: str, config: RunnableConfig) -> str:
    """Do something that takes a long time."""
    await asyncio.sleep(1) # Placeholder for some slow operation
    await adispatch_custom_event(
        "progress_event",
        {"message": "Finished step 1 of 3"},
        config=config # Must be included for python < 3.10
    )
    await asyncio.sleep(1) # Placeholder for some slow operation
    await adispatch_custom_event(
        "progress_event",
        {"message": "Finished step 2 of 3"},
        config=config # Must be included for python < 3.10
    )
    await asyncio.sleep(1) # Placeholder for some slow operation
    return "Done"

slow_thing = RunnableLambda(slow_thing)

async for event in slow_thing.astream_events("some_input", version="v2"):
    print(event)
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `input` ¶ "Copy anchor link to this section for reference") | The input to the `Runnable`.<br>**TYPE:**`Any` |
| ##### `config` ¶ "Copy anchor link to this section for reference") | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| ##### `version` ¶ "Copy anchor link to this section for reference") | The version of the schema to use either `'v2'` or `'v1'`.<br>Users should use `'v2'`.<br>`'v1'` is for backwards compatibility and will be deprecated<br>in `0.4.0`.<br>No default will be assigned until the API is stabilized.<br>custom events will only be surfaced in `'v2'`.<br>**TYPE:**`Literal['v1', 'v2']`**DEFAULT:**`'v2'` |
| ##### `include_names` ¶ "Copy anchor link to this section for reference") | Only include events from `Runnable` objects with matching names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| ##### `include_types` ¶ "Copy anchor link to this section for reference") | Only include events from `Runnable` objects with matching types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| ##### `include_tags` ¶ "Copy anchor link to this section for reference") | Only include events from `Runnable` objects with matching tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| ##### `exclude_names` ¶ "Copy anchor link to this section for reference") | Exclude events from `Runnable` objects with matching names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| ##### `exclude_types` ¶ "Copy anchor link to this section for reference") | Exclude events from `Runnable` objects with matching types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| ##### `exclude_tags` ¶ "Copy anchor link to this section for reference") | Exclude events from `Runnable` objects with matching tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Additional keyword arguments to pass to the `Runnable`.<br>These will be passed to `astream_log` as this implementation<br>of `astream_events` is built on top of `astream_log`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[StreamEvent]` | An async stream of `StreamEvent`. |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | If the version is not `'v1'` or `'v2'`. |

#### ``transform ¶

```
transform(
    input: Iterator[Input], config: RunnableConfig | None = None, **kwargs: Any | None
) -> Iterator[Output]
```

Transform inputs to outputs.

Default implementation of transform, which buffers input and calls `astream`.

Subclasses must override this method if they can start producing output while
input is still being generated.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `input` ¶ "Copy anchor link to this section for reference") | An iterator of inputs to the `Runnable`.<br>**TYPE:**`Iterator[Input]` |
| ##### `config` ¶ "Copy anchor link to this section for reference") | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``atransform`async`¶

```
atransform(
    input: AsyncIterator[Input],
    config: RunnableConfig | None = None,
    **kwargs: Any | None,
) -> AsyncIterator[Output]
```

Transform inputs to outputs.

Default implementation of atransform, which buffers input and calls `astream`.

Subclasses must override this method if they can start producing output while
input is still being generated.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `input` ¶ "Copy anchor link to this section for reference") | An async iterator of inputs to the `Runnable`.<br>**TYPE:**`AsyncIterator[Input]` |
| ##### `config` ¶ "Copy anchor link to this section for reference") | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[Output]` | The output of the `Runnable`. |

#### ``bind ¶

```
bind(**kwargs: Any) -> Runnable[Input, Output]
```

Bind arguments to a `Runnable`, returning a new `Runnable`.

Useful when a `Runnable` in a chain requires an argument that is not
in the output of the previous `Runnable` or included in the user input.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | The arguments to bind to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the arguments bound. |

Example

```
from langchain_ollama import ChatOllama
from langchain_core.output_parsers import StrOutputParser

model = ChatOllama(model="llama3.1")

# Without bind
chain = model | StrOutputParser()

chain.invoke("Repeat quoted words exactly: 'One two three four five.'")
# Output is 'One two three four five.'

# With bind
chain = model.bind(stop=["three"]) | StrOutputParser()

chain.invoke("Repeat quoted words exactly: 'One two three four five.'")
# Output is 'One two'
```

#### ``with\_config ¶

```
with_config(
    config: RunnableConfig | None = None, **kwargs: Any
) -> Runnable[Input, Output]
```

Bind config to a `Runnable`, returning a new `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `config` ¶ "Copy anchor link to this section for reference") | The config to bind to the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the config bound. |

#### ``with\_listeners ¶

```
with_listeners(
    *,
    on_start: Callable[[Run], None]
    | Callable[[Run, RunnableConfig], None]
    | None = None,
    on_end: Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None = None,
    on_error: Callable[[Run], None]
    | Callable[[Run, RunnableConfig], None]
    | None = None,
) -> Runnable[Input, Output]
```

Bind lifecycle listeners to a `Runnable`, returning a new `Runnable`.

The Run object contains information about the run, including its `id`,
`type`, `input`, `output`, `error`, `start_time`, `end_time`, and
any tags or metadata added to the run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `on_start` ¶ "Copy anchor link to this section for reference") | Called before the `Runnable` starts running, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |
| ##### `on_end` ¶ "Copy anchor link to this section for reference") | Called after the `Runnable` finishes running, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |
| ##### `on_error` ¶ "Copy anchor link to this section for reference") | Called if the `Runnable` throws an error, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the listeners bound. |

Example

```
from langchain_core.runnables import RunnableLambda
from langchain_core.tracers.schemas import Run

import time

def test_runnable(time_to_sleep: int):
    time.sleep(time_to_sleep)

def fn_start(run_obj: Run):
    print("start_time:", run_obj.start_time)

def fn_end(run_obj: Run):
    print("end_time:", run_obj.end_time)

chain = RunnableLambda(test_runnable).with_listeners(
    on_start=fn_start, on_end=fn_end
)
chain.invoke(2)
```

#### ``with\_alisteners ¶

```
with_alisteners(
    *,
    on_start: AsyncListener | None = None,
    on_end: AsyncListener | None = None,
    on_error: AsyncListener | None = None,
) -> Runnable[Input, Output]
```

Bind async lifecycle listeners to a `Runnable`.

Returns a new `Runnable`.

The Run object contains information about the run, including its `id`,
`type`, `input`, `output`, `error`, `start_time`, `end_time`, and
any tags or metadata added to the run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `on_start` ¶ "Copy anchor link to this section for reference") | Called asynchronously before the `Runnable` starts running,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |
| ##### `on_end` ¶ "Copy anchor link to this section for reference") | Called asynchronously after the `Runnable` finishes running,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |
| ##### `on_error` ¶ "Copy anchor link to this section for reference") | Called asynchronously if the `Runnable` throws an error,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the listeners bound. |

Example

```
from langchain_core.runnables import RunnableLambda, Runnable
from datetime import datetime, timezone
import time
import asyncio

def format_t(timestamp: float) -> str:
    return datetime.fromtimestamp(timestamp, tz=timezone.utc).isoformat()

async def test_runnable(time_to_sleep: int):
    print(f"Runnable[{time_to_sleep}s]: starts at {format_t(time.time())}")
    await asyncio.sleep(time_to_sleep)
    print(f"Runnable[{time_to_sleep}s]: ends at {format_t(time.time())}")

async def fn_start(run_obj: Runnable):
    print(f"on start callback starts at {format_t(time.time())}")
    await asyncio.sleep(3)
    print(f"on start callback ends at {format_t(time.time())}")

async def fn_end(run_obj: Runnable):
    print(f"on end callback starts at {format_t(time.time())}")
    await asyncio.sleep(2)
    print(f"on end callback ends at {format_t(time.time())}")

runnable = RunnableLambda(test_runnable).with_alisteners(
    on_start=fn_start,
    on_end=fn_end
)
async def concurrent_runs():
    await asyncio.gather(runnable.ainvoke(2), runnable.ainvoke(3))

asyncio.run(concurrent_runs())
Result:
on start callback starts at 2025-03-01T07:05:22.875378+00:00
on start callback starts at 2025-03-01T07:05:22.875495+00:00
on start callback ends at 2025-03-01T07:05:25.878862+00:00
on start callback ends at 2025-03-01T07:05:25.878947+00:00
Runnable[2s]: starts at 2025-03-01T07:05:25.879392+00:00
Runnable[3s]: starts at 2025-03-01T07:05:25.879804+00:00
Runnable[2s]: ends at 2025-03-01T07:05:27.881998+00:00
on end callback starts at 2025-03-01T07:05:27.882360+00:00
Runnable[3s]: ends at 2025-03-01T07:05:28.881737+00:00
on end callback starts at 2025-03-01T07:05:28.882428+00:00
on end callback ends at 2025-03-01T07:05:29.883893+00:00
on end callback ends at 2025-03-01T07:05:30.884831+00:00
```

#### ``with\_types ¶

```
with_types(
    *, input_type: type[Input] | None = None, output_type: type[Output] | None = None
) -> Runnable[Input, Output]
```

Bind input and output types to a `Runnable`, returning a new `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `input_type` ¶ "Copy anchor link to this section for reference") | The input type to bind to the `Runnable`.<br>**TYPE:**`type[Input] | None`**DEFAULT:**`None` |
| ##### `output_type` ¶ "Copy anchor link to this section for reference") | The output type to bind to the `Runnable`.<br>**TYPE:**`type[Output] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the types bound. |

#### ``with\_retry ¶

```
with_retry(
    *,
    retry_if_exception_type: tuple[type[BaseException], ...] = (Exception,),
    wait_exponential_jitter: bool = True,
    exponential_jitter_params: ExponentialJitterParams | None = None,
    stop_after_attempt: int = 3,
) -> Runnable[Input, Output]
```

Create a new `Runnable` that retries the original `Runnable` on exceptions.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `retry_if_exception_type` ¶ "Copy anchor link to this section for reference") | A tuple of exception types to retry on.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| ##### `wait_exponential_jitter` ¶ "Copy anchor link to this section for reference") | Whether to add jitter to the wait<br>time between retries.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| ##### `stop_after_attempt` ¶ "Copy anchor link to this section for reference") | The maximum number of attempts to make before<br>giving up.<br>**TYPE:**`int`**DEFAULT:**`3` |
| ##### `exponential_jitter_params` ¶ "Copy anchor link to this section for reference") | Parameters for<br>`tenacity.wait_exponential_jitter`. Namely: `initial`, `max`,<br>`exp_base`, and `jitter` (all `float` values).<br>**TYPE:**`ExponentialJitterParams | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new Runnable that retries the original Runnable on exceptions. |

Example

```
from langchain_core.runnables import RunnableLambda

count = 0

def _lambda(x: int) -> None:
    global count
    count = count + 1
    if x == 1:
        raise ValueError("x is 1")
    else:
        pass

runnable = RunnableLambda(_lambda)
try:
    runnable.with_retry(
        stop_after_attempt=2,
        retry_if_exception_type=(ValueError,),
    ).invoke(1)
except ValueError:
    pass

assert count == 2
```

#### ``map ¶

```
map() -> Runnable[list[Input], list[Output]]
```

Return a new `Runnable` that maps a list of inputs to a list of outputs.

Calls `invoke` with each input.

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[list[Input], list[Output]]` | A new `Runnable` that maps a list of inputs to a list of outputs. |

Example

```
from langchain_core.runnables import RunnableLambda

def _lambda(x: int) -> int:
    return x + 1

runnable = RunnableLambda(_lambda)
print(runnable.map().invoke([1, 2, 3]))  # [2, 3, 4]
```

#### ``with\_fallbacks ¶

```
with_fallbacks(
    fallbacks: Sequence[Runnable[Input, Output]],
    *,
    exceptions_to_handle: tuple[type[BaseException], ...] = (Exception,),
    exception_key: str | None = None,
) -> RunnableWithFallbacks[Input, Output]
```

Add fallbacks to a `Runnable`, returning a new `Runnable`.

The new `Runnable` will try the original `Runnable`, and then each fallback
in order, upon failures.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `fallbacks` ¶ "Copy anchor link to this section for reference") | A sequence of runnables to try if the original `Runnable`<br>fails.<br>**TYPE:**`Sequence[Runnable[Input, Output]]` |
| ##### `exceptions_to_handle` ¶ "Copy anchor link to this section for reference") | A tuple of exception types to handle.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| ##### `exception_key` ¶ "Copy anchor link to this section for reference") | If `string` is specified then handled exceptions will be<br>passed to fallbacks as part of the input under the specified key.<br>If `None`, exceptions will not be passed to fallbacks.<br>If used, the base `Runnable` and its fallbacks must accept a<br>dictionary as input.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableWithFallbacks[Input, Output]` | A new `Runnable` that will try the original `Runnable`, and then each<br>Fallback in order, upon failures. |

Example

```
from typing import Iterator

from langchain_core.runnables import RunnableGenerator

def _generate_immediate_error(input: Iterator) -> Iterator[str]:
    raise ValueError()
    yield ""

def _generate(input: Iterator) -> Iterator[str]:
    yield from "foo bar"

runnable = RunnableGenerator(_generate_immediate_error).with_fallbacks(
    [RunnableGenerator(_generate)]
)
print("".join(runnable.stream({})))  # foo bar
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `fallbacks` ¶ "Copy anchor link to this section for reference") | A sequence of runnables to try if the original `Runnable`<br>fails.<br>**TYPE:**`Sequence[Runnable[Input, Output]]` |
| ##### `exceptions_to_handle` ¶ "Copy anchor link to this section for reference") | A tuple of exception types to handle.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| ##### `exception_key` ¶ "Copy anchor link to this section for reference") | If `string` is specified then handled exceptions will be<br>passed to fallbacks as part of the input under the specified key.<br>If `None`, exceptions will not be passed to fallbacks.<br>If used, the base `Runnable` and its fallbacks must accept a<br>dictionary as input.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableWithFallbacks[Input, Output]` | A new `Runnable` that will try the original `Runnable`, and then each<br>Fallback in order, upon failures. |

#### ``as\_tool ¶

```
as_tool(
    args_schema: type[BaseModel] | None = None,
    *,
    name: str | None = None,
    description: str | None = None,
    arg_types: dict[str, type] | None = None,
) -> BaseTool
```

Create a `BaseTool` from a `Runnable`.

`as_tool` will instantiate a `BaseTool` with a name, description, and
`args_schema` from a `Runnable`. Where possible, schemas are inferred
from `runnable.get_input_schema`. Alternatively (e.g., if the
`Runnable` takes a dict as input and the specific dict keys are not typed),
the schema can be specified directly with `args_schema`. You can also
pass `arg_types` to just specify the required arguments and their types.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `args_schema` ¶ "Copy anchor link to this section for reference") | The schema for the tool.<br>**TYPE:**`type[BaseModel] | None`**DEFAULT:**`None` |
| ##### `name` ¶ "Copy anchor link to this section for reference") | The name of the tool.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| ##### `description` ¶ "Copy anchor link to this section for reference") | The description of the tool.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| ##### `arg_types` ¶ "Copy anchor link to this section for reference") | A dictionary of argument names to types.<br>**TYPE:**`dict[str, type] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `BaseTool` | A `BaseTool` instance. |

Typed dict input:

```
from typing_extensions import TypedDict
from langchain_core.runnables import RunnableLambda

class Args(TypedDict):
    a: int
    b: list[int]

def f(x: Args) -> str:
    return str(x["a"] * max(x["b"]))

runnable = RunnableLambda(f)
as_tool = runnable.as_tool()
as_tool.invoke({"a": 3, "b": [1, 2]})
```

`dict` input, specifying schema via `args_schema`:

```
from typing import Any
from pydantic import BaseModel, Field
from langchain_core.runnables import RunnableLambda

def f(x: dict[str, Any]) -> str:
    return str(x["a"] * max(x["b"]))

class FSchema(BaseModel):
    """Apply a function to an integer and list of integers."""

    a: int = Field(..., description="Integer")
    b: list[int] = Field(..., description="List of ints")

runnable = RunnableLambda(f)
as_tool = runnable.as_tool(FSchema)
as_tool.invoke({"a": 3, "b": [1, 2]})
```

`dict` input, specifying schema via `arg_types`:

```
from typing import Any
from langchain_core.runnables import RunnableLambda

def f(x: dict[str, Any]) -> str:
    return str(x["a"] * max(x["b"]))

runnable = RunnableLambda(f)
as_tool = runnable.as_tool(arg_types={"a": int, "b": list[int]})
as_tool.invoke({"a": 3, "b": [1, 2]})
```

String input:

```
from langchain_core.runnables import RunnableLambda

def f(x: str) -> str:
    return x + "a"

def g(x: str) -> str:
    return x + "z"

runnable = RunnableLambda(f) | g
as_tool = runnable.as_tool()
as_tool.invoke("b")
```

#### ``\_\_init\_\_ ¶

```
__init__(*args: Any, **kwargs: Any) -> None
```

#### ``is\_lc\_serializable`classmethod`¶

```
is_lc_serializable() -> bool
```

Is this class serializable?

By design, even if a class inherits from `Serializable`, it is not serializable
by default. This is to prevent accidental serialization of objects that should
not be serialized.

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | Whether the class is serializable. Default is `False`. |

#### ``get\_lc\_namespace`classmethod`¶

```
get_lc_namespace() -> list[str]
```

Get the namespace of the LangChain object.

For example, if the class is `langchain.llms.openai.OpenAI`, then the
namespace is `["langchain", "llms", "openai"]`

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[str]` | The namespace. |

#### ``lc\_id`classmethod`¶

```
lc_id() -> list[str]
```

Return a unique identifier for this class for serialization purposes.

The unique identifier is a list of strings that describes the path
to the object.

For example, for the class `langchain.llms.openai.OpenAI`, the id is
`["langchain", "llms", "openai", "OpenAI"]`.

#### ``to\_json ¶

```
to_json() -> SerializedConstructor | SerializedNotImplemented
```

Serialize the `Runnable` to JSON.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedConstructor | SerializedNotImplemented` | A JSON-serializable representation of the `Runnable`. |

#### ``to\_json\_not\_implemented ¶

```
to_json_not_implemented() -> SerializedNotImplemented
```

Serialize a "not implemented" object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedNotImplemented` | `SerializedNotImplemented`. |

#### ``configurable\_fields ¶

```
configurable_fields(
    **kwargs: AnyConfigurableField,
) -> RunnableSerializable[Input, Output]
```

Configure particular `Runnable` fields at runtime.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | A dictionary of `ConfigurableField` instances to configure.<br>**TYPE:**`AnyConfigurableField`**DEFAULT:**`{}` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If a configuration key is not found in the `Runnable`. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Output]` | A new `Runnable` with the fields configured. |

```
from langchain_core.runnables import ConfigurableField
from langchain_openai import ChatOpenAI

model = ChatOpenAI(max_tokens=20).configurable_fields(
    max_tokens=ConfigurableField(
        id="output_token_number",
        name="Max tokens in the output",
        description="The maximum number of tokens in the output",
    )
)

# max_tokens = 20
print("max_tokens_20: ", model.invoke("tell me something about chess").content)

# max_tokens = 200
print(
    "max_tokens_200: ",
    model.with_config(configurable={"output_token_number": 200})
    .invoke("tell me something about chess")
    .content,
)
```

#### ``configurable\_alternatives ¶

```
configurable_alternatives(
    which: ConfigurableField,
    *,
    default_key: str = "default",
    prefix_keys: bool = False,
    **kwargs: Runnable[Input, Output] | Callable[[], Runnable[Input, Output]],
) -> RunnableSerializable[Input, Output]
```

Configure alternatives for `Runnable` objects that can be set at runtime.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `which` ¶ "Copy anchor link to this section for reference") | The `ConfigurableField` instance that will be used to select the<br>alternative.<br>**TYPE:**`ConfigurableField` |
| ##### `default_key` ¶ "Copy anchor link to this section for reference") | The default key to use if no alternative is selected.<br>**TYPE:**`str`**DEFAULT:**`'default'` |
| ##### `prefix_keys` ¶ "Copy anchor link to this section for reference") | Whether to prefix the keys with the `ConfigurableField` id.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | A dictionary of keys to `Runnable` instances or callables that<br>return `Runnable` instances.<br>**TYPE:**`Runnable[Input, Output] | Callable[[], Runnable[Input, Output]]`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Output]` | A new `Runnable` with the alternatives configured. |

```
from langchain_anthropic import ChatAnthropic
from langchain_core.runnables.utils import ConfigurableField
from langchain_openai import ChatOpenAI

model = ChatAnthropic(
    model_name="claude-sonnet-4-5-20250929"
).configurable_alternatives(
    ConfigurableField(id="llm"),
    default_key="anthropic",
    openai=ChatOpenAI(),
)

# uses the default model ChatAnthropic
print(model.invoke("which organization created you?").content)

# uses ChatOpenAI
print(
    model.with_config(configurable={"llm": "openai"})
    .invoke("which organization created you?")
    .content
)
```

#### ``set\_verbose ¶

```
set_verbose(verbose: bool | None) -> bool
```

If verbose is `None`, set it.

This allows users to pass in `None` as verbose to access the global setting.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `verbose` ¶ "Copy anchor link to this section for reference") | The verbosity setting to use.<br>**TYPE:**`bool | None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | The verbosity setting to use. |

#### ``get\_token\_ids ¶

```
get_token_ids(text: str) -> list[int]
```

Return the ordered IDs of the tokens in a text.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `text` ¶ "Copy anchor link to this section for reference") | The string input to tokenize.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[int]` | A list of IDs corresponding to the tokens in the text, in order they occur<br>in the text. |

#### ``get\_num\_tokens ¶

```
get_num_tokens(text: str) -> int
```

Get the number of tokens present in the text.

Useful for checking if an input fits in a model's context window.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `text` ¶ "Copy anchor link to this section for reference") | The string input to tokenize.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `int` | The integer number of tokens in the text. |

#### ``get\_num\_tokens\_from\_messages ¶

```
get_num_tokens_from_messages(
    messages: list[BaseMessage], tools: Sequence | None = None
) -> int
```

Get the number of tokens in the messages.

Useful for checking if an input fits in a model's context window.

Note

The base implementation of `get_num_tokens_from_messages` ignores tool
schemas.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `messages` ¶ "Copy anchor link to this section for reference") | The message inputs to tokenize.<br>**TYPE:**`list[BaseMessage]` |
| ##### `tools` ¶ "Copy anchor link to this section for reference") | If provided, sequence of dict, `BaseModel`, function, or<br>`BaseTool` objects to be converted to tool schemas.<br>**TYPE:**`Sequence | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `int` | The sum of the number of tokens across the messages. |

#### ``invoke ¶

```
invoke(
    input: LanguageModelInput,
    config: RunnableConfig | None = None,
    *,
    stop: list[str] | None = None,
    **kwargs: Any,
) -> AIMessage
```

Transform a single input into an output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `input` ¶ "Copy anchor link to this section for reference") | The input to the `Runnable`.<br>**TYPE:**`Input` |
| ##### `config` ¶ "Copy anchor link to this section for reference") | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``ainvoke`async`¶

```
ainvoke(
    input: LanguageModelInput,
    config: RunnableConfig | None = None,
    *,
    stop: list[str] | None = None,
    **kwargs: Any,
) -> AIMessage
```

Transform a single input into an output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `input` ¶ "Copy anchor link to this section for reference") | The input to the `Runnable`.<br>**TYPE:**`Input` |
| ##### `config` ¶ "Copy anchor link to this section for reference") | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``stream ¶

```
stream(
    input: LanguageModelInput,
    config: RunnableConfig | None = None,
    *,
    stop: list[str] | None = None,
    **kwargs: Any,
) -> Iterator[AIMessageChunk]
```

Default implementation of `stream`, which calls `invoke`.

Subclasses must override this method if they support streaming output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `input` ¶ "Copy anchor link to this section for reference") | The input to the `Runnable`.<br>**TYPE:**`Input` |
| ##### `config` ¶ "Copy anchor link to this section for reference") | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``astream`async`¶

```
astream(
    input: LanguageModelInput,
    config: RunnableConfig | None = None,
    *,
    stop: list[str] | None = None,
    **kwargs: Any,
) -> AsyncIterator[AIMessageChunk]
```

Default implementation of `astream`, which calls `ainvoke`.

Subclasses must override this method if they support streaming output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `input` ¶ "Copy anchor link to this section for reference") | The input to the `Runnable`.<br>**TYPE:**`Input` |
| ##### `config` ¶ "Copy anchor link to this section for reference") | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[Output]` | The output of the `Runnable`. |

#### ``generate ¶

```
generate(
    messages: list[list[BaseMessage]],
    stop: list[str] | None = None,
    callbacks: Callbacks = None,
    *,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    run_name: str | None = None,
    run_id: UUID | None = None,
    **kwargs: Any,
) -> LLMResult
```

Pass a sequence of prompts to the model and return model generations.

This method should make use of batched calls for models that expose a batched
API.

Use this method when you want to:

1. Take advantage of batched calls,
2. Need more output from the model than just the top generated value,
3. Are building chains that are agnostic to the underlying language model
    type (e.g., pure text completion models vs chat models).

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `messages` ¶ "Copy anchor link to this section for reference") | List of list of messages.<br>**TYPE:**`list[list[BaseMessage]]` |
| ##### `stop` ¶ "Copy anchor link to this section for reference") | Stop words to use when generating. Model output is cut off at the<br>first occurrence of any of these substrings.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| ##### `callbacks` ¶ "Copy anchor link to this section for reference") | `Callbacks` to pass through. Used for executing additional<br>functionality, such as logging or streaming, throughout generation.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| ##### `tags` ¶ "Copy anchor link to this section for reference") | The tags to apply.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| ##### `metadata` ¶ "Copy anchor link to this section for reference") | The metadata to apply.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| ##### `run_name` ¶ "Copy anchor link to this section for reference") | The name of the run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| ##### `run_id` ¶ "Copy anchor link to this section for reference") | The ID of the run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Arbitrary additional keyword arguments. These are usually passed<br>to the model provider API call.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LLMResult` | An `LLMResult`, which contains a list of candidate `Generations` for each<br>input prompt and additional model provider-specific output. |

#### ``agenerate`async`¶

```
agenerate(
    messages: list[list[BaseMessage]],
    stop: list[str] | None = None,
    callbacks: Callbacks = None,
    *,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    run_name: str | None = None,
    run_id: UUID | None = None,
    **kwargs: Any,
) -> LLMResult
```

Asynchronously pass a sequence of prompts to a model and return generations.

This method should make use of batched calls for models that expose a batched
API.

Use this method when you want to:

1. Take advantage of batched calls,
2. Need more output from the model than just the top generated value,
3. Are building chains that are agnostic to the underlying language model
    type (e.g., pure text completion models vs chat models).

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `messages` ¶ "Copy anchor link to this section for reference") | List of list of messages.<br>**TYPE:**`list[list[BaseMessage]]` |
| ##### `stop` ¶ "Copy anchor link to this section for reference") | Stop words to use when generating. Model output is cut off at the<br>first occurrence of any of these substrings.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| ##### `callbacks` ¶ "Copy anchor link to this section for reference") | `Callbacks` to pass through. Used for executing additional<br>functionality, such as logging or streaming, throughout generation.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| ##### `tags` ¶ "Copy anchor link to this section for reference") | The tags to apply.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| ##### `metadata` ¶ "Copy anchor link to this section for reference") | The metadata to apply.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| ##### `run_name` ¶ "Copy anchor link to this section for reference") | The name of the run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| ##### `run_id` ¶ "Copy anchor link to this section for reference") | The ID of the run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Arbitrary additional keyword arguments. These are usually passed<br>to the model provider API call.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LLMResult` | An `LLMResult`, which contains a list of candidate `Generations` for each<br>input prompt and additional model provider-specific output. |

#### ``generate\_prompt ¶

```
generate_prompt(
    prompts: list[PromptValue],
    stop: list[str] | None = None,
    callbacks: Callbacks = None,
    **kwargs: Any,
) -> LLMResult
```

Pass a sequence of prompts to the model and return model generations.

This method should make use of batched calls for models that expose a batched
API.

Use this method when you want to:

1. Take advantage of batched calls,
2. Need more output from the model than just the top generated value,
3. Are building chains that are agnostic to the underlying language model
    type (e.g., pure text completion models vs chat models).

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `prompts` ¶ "Copy anchor link to this section for reference") | List of `PromptValue` objects. A `PromptValue` is an object that<br>can be converted to match the format of any language model (string for<br>pure text generation models and `BaseMessage` objects for chat models).<br>**TYPE:**`list[PromptValue]` |
| ##### `stop` ¶ "Copy anchor link to this section for reference") | Stop words to use when generating. Model output is cut off at the<br>first occurrence of any of these substrings.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| ##### `callbacks` ¶ "Copy anchor link to this section for reference") | `Callbacks` to pass through. Used for executing additional<br>functionality, such as logging or streaming, throughout generation.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Arbitrary additional keyword arguments. These are usually passed<br>to the model provider API call.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LLMResult` | An `LLMResult`, which contains a list of candidate `Generation` objects for<br>each input prompt and additional model provider-specific output. |

#### ``agenerate\_prompt`async`¶

```
agenerate_prompt(
    prompts: list[PromptValue],
    stop: list[str] | None = None,
    callbacks: Callbacks = None,
    **kwargs: Any,
) -> LLMResult
```

Asynchronously pass a sequence of prompts and return model generations.

This method should make use of batched calls for models that expose a batched
API.

Use this method when you want to:

1. Take advantage of batched calls,
2. Need more output from the model than just the top generated value,
3. Are building chains that are agnostic to the underlying language model
    type (e.g., pure text completion models vs chat models).

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `prompts` ¶ "Copy anchor link to this section for reference") | List of `PromptValue` objects. A `PromptValue` is an object that<br>can be converted to match the format of any language model (string for<br>pure text generation models and `BaseMessage` objects for chat models).<br>**TYPE:**`list[PromptValue]` |
| ##### `stop` ¶ "Copy anchor link to this section for reference") | Stop words to use when generating. Model output is cut off at the<br>first occurrence of any of these substrings.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| ##### `callbacks` ¶ "Copy anchor link to this section for reference") | `Callbacks` to pass through. Used for executing additional<br>functionality, such as logging or streaming, throughout generation.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Arbitrary additional keyword arguments. These are usually passed<br>to the model provider API call.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LLMResult` | An `LLMResult`, which contains a list of candidate `Generation` objects for<br>each input prompt and additional model provider-specific output. |

#### ``dict ¶

```
dict(**kwargs: Any) -> dict
```

Return a dictionary of the LLM.

#### ``bind\_tools ¶

```
bind_tools(
    tools: Sequence[Dict[str, Any] | type | Callable | BaseTool],
    *,
    tool_choice: str | None = None,
    **kwargs: Any,
) -> Runnable[LanguageModelInput, AIMessage]
```

Bind tools to the model.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `tools` ¶ "Copy anchor link to this section for reference") | Sequence of tools to bind to the model.<br>**TYPE:**`Sequence[Dict[str, Any] | type | Callable | BaseTool]` |
| ##### `tool_choice` ¶ "Copy anchor link to this section for reference") | The tool to use. If "any" then any tool can be used.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[LanguageModelInput, AIMessage]` | A Runnable that returns a message. |

#### ``with\_structured\_output ¶

```
with_structured_output(
    schema: Dict | type, *, include_raw: bool = False, **kwargs: Any
) -> Runnable[LanguageModelInput, Dict | BaseModel]
```

Model wrapper that returns outputs formatted to match the given schema.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `schema` ¶ "Copy anchor link to this section for reference") | The output schema. Can be passed in as:<br>- an OpenAI function/tool schema,<br>- a JSON Schema,<br>- a `TypedDict` class,<br>- or a Pydantic class.<br>If `schema` is a Pydantic class then the model output will be a<br>Pydantic instance of that class, and the model-generated fields will be<br>validated by the Pydantic class. Otherwise the model output will be a<br>dict and will not be validated.<br>See `langchain_core.utils.function_calling.convert_to_openai_tool` for<br>more on how to properly specify types and descriptions of schema fields<br>when specifying a Pydantic or `TypedDict` class.<br>**TYPE:**`Dict | type` |
| ##### `include_raw` ¶ "Copy anchor link to this section for reference") | If `False` then only the parsed structured output is returned. If<br>an error occurs during model output parsing it will be raised. If `True`<br>then both the raw model response (a `BaseMessage`) and the parsed model<br>response will be returned. If an error occurs during output parsing it<br>will be caught and returned as well.<br>The final output is always a `dict` with keys `'raw'`, `'parsed'`, and<br>`'parsing_error'`.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If there are any unsupported `kwargs`. |
| `NotImplementedError` | If the model does not implement<br>`with_structured_output()`. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[LanguageModelInput, Dict | BaseModel]` | A `Runnable` that takes same inputs as a<br>`langchain_core.language_models.chat.BaseChatModel`. If `include_raw` is<br>`False` and `schema` is a Pydantic class, `Runnable` outputs an instance<br>of `schema` (i.e., a Pydantic object). Otherwise, if `include_raw` is<br>`False` then `Runnable` outputs a `dict`.<br>If `include_raw` is `True`, then `Runnable` outputs a `dict` with keys:<br>- `'raw'`: `BaseMessage`<br>- `'parsed'`: `None` if there was a parsing error, otherwise the type<br>depends on the `schema` as described above.<br>- `'parsing_error'`: `BaseException | None` |

Example: Pydantic schema (`include_raw=False`):

```
from pydantic import BaseModel

class AnswerWithJustification(BaseModel):
    '''An answer to the user question along with justification for the answer.'''

    answer: str
    justification: str

model = ChatModel(model="model-name", temperature=0)
structured_model = model.with_structured_output(AnswerWithJustification)

structured_model.invoke(
    "What weighs more a pound of bricks or a pound of feathers"
)

# -> AnswerWithJustification(
#     answer='They weigh the same',
#     justification='Both a pound of bricks and a pound of feathers weigh one pound. The weight is the same, but the volume or density of the objects may differ.'
# )
```

Example: Pydantic schema (`include_raw=True`):

```
from pydantic import BaseModel

class AnswerWithJustification(BaseModel):
    '''An answer to the user question along with justification for the answer.'''

    answer: str
    justification: str

model = ChatModel(model="model-name", temperature=0)
structured_model = model.with_structured_output(
    AnswerWithJustification, include_raw=True
)

structured_model.invoke(
    "What weighs more a pound of bricks or a pound of feathers"
)
# -> {
#     'raw': AIMessage(content='', additional_kwargs={'tool_calls': [{'id': 'call_Ao02pnFYXD6GN1yzc0uXPsvF', 'function': {'arguments': '{"answer":"They weigh the same.","justification":"Both a pound of bricks and a pound of feathers weigh one pound. The weight is the same, but the volume or density of the objects may differ."}', 'name': 'AnswerWithJustification'}, 'type': 'function'}]}),
#     'parsed': AnswerWithJustification(answer='They weigh the same.', justification='Both a pound of bricks and a pound of feathers weigh one pound. The weight is the same, but the volume or density of the objects may differ.'),
#     'parsing_error': None
# }
```

Example: `dict` schema (`include_raw=False`):

```
from pydantic import BaseModel
from langchain_core.utils.function_calling import convert_to_openai_tool

class AnswerWithJustification(BaseModel):
    '''An answer to the user question along with justification for the answer.'''

    answer: str
    justification: str

dict_schema = convert_to_openai_tool(AnswerWithJustification)
model = ChatModel(model="model-name", temperature=0)
structured_model = model.with_structured_output(dict_schema)

structured_model.invoke(
    "What weighs more a pound of bricks or a pound of feathers"
)
# -> {
#     'answer': 'They weigh the same',
#     'justification': 'Both a pound of bricks and a pound of feathers weigh one pound. The weight is the same, but the volume and density of the two substances differ.'
# }
```

Behavior changed in 0.2.26

Added support for TypedDict class.

## ``langchain\_core.messages ¶

**Messages** are objects used in prompts and chat conversations.

### ``BaseMessage ¶

Bases: `Serializable`

Base abstract message class.

Messages are the inputs and outputs of a chat model.

Examples include `HumanMessage`,
`AIMessage`, and
`SystemMessage`.

| METHOD | DESCRIPTION |
| --- | --- |
| `lc_id` | Return a unique identifier for this class for serialization purposes. |
| `to_json` | Serialize the object to JSON. |
| `to_json_not_implemented` | Serialize a "not implemented" object. |
| `__init__` | Initialize a `BaseMessage`. |
| `is_lc_serializable` | `BaseMessage` is serializable. |
| `get_lc_namespace` | Get the namespace of the LangChain object. |
| `__add__` | Concatenate this message with another message. |
| `pretty_repr` | Get a pretty representation of the message. |
| `pretty_print` | Print a pretty representation of the message. |

#### ``lc\_secrets`property`¶

```
lc_secrets: dict[str, str]
```

A map of constructor argument names to secret ids.

For example, `{"openai_api_key": "OPENAI_API_KEY"}`

#### ``lc\_attributes`property`¶

```
lc_attributes: dict
```

List of attribute names that should be included in the serialized kwargs.

These attributes must be accepted by the constructor.

Default is an empty dictionary.

#### ``content`instance-attribute`¶

```
content: str | list[str | dict]
```

The contents of the message.

#### ``additional\_kwargs`class-attribute``instance-attribute`¶

```
additional_kwargs: dict = Field(default_factory=dict)
```

Reserved for additional payload data associated with the message.

For example, for a message from an AI, this could include tool calls as
encoded by the model provider.

#### ``response\_metadata`class-attribute``instance-attribute`¶

```
response_metadata: dict = Field(default_factory=dict)
```

Examples: response headers, logprobs, token counts, model name.

#### ``type`instance-attribute`¶

```
type: str
```

The type of the message. Must be a string that is unique to the message type.

The purpose of this field is to allow for easy identification of the message type
when deserializing messages.

#### ``name`class-attribute``instance-attribute`¶

```
name: str | None = None
```

An optional name for the message.

This can be used to provide a human-readable name for the message.

Usage of this field is optional, and whether it's used or not is up to the
model implementation.

#### ``id`class-attribute``instance-attribute`¶

```
id: str | None = Field(default=None, coerce_numbers_to_str=True)
```

An optional unique identifier for the message.

This should ideally be provided by the provider/model which created the message.

#### ``content\_blocks`property`¶

```
content_blocks: list[ContentBlock]
```

Load content blocks from the message content.

Added in version 1.0.0

#### ``text`property`¶

```
text: TextAccessor
```

Get the text content of the message as a string.

Can be used as both property (`message.text`) and method (`message.text()`).

Deprecated

As of `langchain-core` 1.0.0, calling `.text()` as a method is deprecated.
Use `.text` as a property instead. This method will be removed in 2.0.0.

| RETURNS | DESCRIPTION |
| --- | --- |
| `TextAccessor` | The text content of the message. |

#### ``lc\_id`classmethod`¶

```
lc_id() -> list[str]
```

Return a unique identifier for this class for serialization purposes.

The unique identifier is a list of strings that describes the path
to the object.

For example, for the class `langchain.llms.openai.OpenAI`, the id is
`["langchain", "llms", "openai", "OpenAI"]`.

#### ``to\_json ¶

```
to_json() -> SerializedConstructor | SerializedNotImplemented
```

Serialize the object to JSON.

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If the class has deprecated attributes. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedConstructor | SerializedNotImplemented` | A JSON serializable object or a `SerializedNotImplemented` object. |

#### ``to\_json\_not\_implemented ¶

```
to_json_not_implemented() -> SerializedNotImplemented
```

Serialize a "not implemented" object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedNotImplemented` | `SerializedNotImplemented`. |

#### ``\_\_init\_\_ ¶

```
__init__(
    content: str | list[str | dict] | None = None,
    content_blocks: list[ContentBlock] | None = None,
    **kwargs: Any,
) -> None
```

Initialize a `BaseMessage`.

Specify `content` as positional arg or `content_blocks` for typing.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `content` ¶ "Copy anchor link to this section for reference") | The contents of the message.<br>**TYPE:**`str | list[str | dict] | None`**DEFAULT:**`None` |
| ##### `content_blocks` ¶ "Copy anchor link to this section for reference") | Typed standard content.<br>**TYPE:**`list[ContentBlock] | None`**DEFAULT:**`None` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Additional arguments to pass to the parent class.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``is\_lc\_serializable`classmethod`¶

```
is_lc_serializable() -> bool
```

`BaseMessage` is serializable.

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | True |

#### ``get\_lc\_namespace`classmethod`¶

```
get_lc_namespace() -> list[str]
```

Get the namespace of the LangChain object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[str]` | `["langchain", "schema", "messages"]` |

#### ``\_\_add\_\_ ¶

```
__add__(other: Any) -> ChatPromptTemplate
```

Concatenate this message with another message.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `other` ¶ "Copy anchor link to this section for reference") | Another message to concatenate with this one.<br>**TYPE:**`Any` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ChatPromptTemplate` | A ChatPromptTemplate containing both messages. |

#### ``pretty\_repr ¶

```
pretty_repr(html: bool = False) -> str
```

Get a pretty representation of the message.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `html` ¶ "Copy anchor link to this section for reference") | Whether to format the message as HTML. If `True`, the message will be<br>formatted with HTML tags.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | A pretty representation of the message. |

#### ``pretty\_print ¶

```
pretty_print() -> None
```

Print a pretty representation of the message.

### ``BaseMessageChunk ¶

Bases: `BaseMessage`

Message chunk, which can be concatenated with other Message chunks.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize a `BaseMessage`. |
| `is_lc_serializable` | `BaseMessage` is serializable. |
| `get_lc_namespace` | Get the namespace of the LangChain object. |
| `lc_id` | Return a unique identifier for this class for serialization purposes. |
| `to_json` | Serialize the object to JSON. |
| `to_json_not_implemented` | Serialize a "not implemented" object. |
| `pretty_repr` | Get a pretty representation of the message. |
| `pretty_print` | Print a pretty representation of the message. |
| `__add__` | Message chunks support concatenation with other message chunks. |

#### ``lc\_secrets`property`¶

```
lc_secrets: dict[str, str]
```

A map of constructor argument names to secret ids.

For example, `{"openai_api_key": "OPENAI_API_KEY"}`

#### ``lc\_attributes`property`¶

```
lc_attributes: dict
```

List of attribute names that should be included in the serialized kwargs.

These attributes must be accepted by the constructor.

Default is an empty dictionary.

#### ``content`instance-attribute`¶

```
content: str | list[str | dict]
```

The contents of the message.

#### ``additional\_kwargs`class-attribute``instance-attribute`¶

```
additional_kwargs: dict = Field(default_factory=dict)
```

Reserved for additional payload data associated with the message.

For example, for a message from an AI, this could include tool calls as
encoded by the model provider.

#### ``response\_metadata`class-attribute``instance-attribute`¶

```
response_metadata: dict = Field(default_factory=dict)
```

Examples: response headers, logprobs, token counts, model name.

#### ``type`instance-attribute`¶

```
type: str
```

The type of the message. Must be a string that is unique to the message type.

The purpose of this field is to allow for easy identification of the message type
when deserializing messages.

#### ``name`class-attribute``instance-attribute`¶

```
name: str | None = None
```

An optional name for the message.

This can be used to provide a human-readable name for the message.

Usage of this field is optional, and whether it's used or not is up to the
model implementation.

#### ``id`class-attribute``instance-attribute`¶

```
id: str | None = Field(default=None, coerce_numbers_to_str=True)
```

An optional unique identifier for the message.

This should ideally be provided by the provider/model which created the message.

#### ``content\_blocks`property`¶

```
content_blocks: list[ContentBlock]
```

Load content blocks from the message content.

Added in version 1.0.0

#### ``text`property`¶

```
text: TextAccessor
```

Get the text content of the message as a string.

Can be used as both property (`message.text`) and method (`message.text()`).

Deprecated

As of `langchain-core` 1.0.0, calling `.text()` as a method is deprecated.
Use `.text` as a property instead. This method will be removed in 2.0.0.

| RETURNS | DESCRIPTION |
| --- | --- |
| `TextAccessor` | The text content of the message. |

#### ``\_\_init\_\_ ¶

```
__init__(
    content: str | list[str | dict] | None = None,
    content_blocks: list[ContentBlock] | None = None,
    **kwargs: Any,
) -> None
```

Initialize a `BaseMessage`.

Specify `content` as positional arg or `content_blocks` for typing.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `content` ¶ "Copy anchor link to this section for reference") | The contents of the message.<br>**TYPE:**`str | list[str | dict] | None`**DEFAULT:**`None` |
| ##### `content_blocks` ¶ "Copy anchor link to this section for reference") | Typed standard content.<br>**TYPE:**`list[ContentBlock] | None`**DEFAULT:**`None` |
| ##### `**kwargs` ¶ "Copy anchor link to this section for reference") | Additional arguments to pass to the parent class.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``is\_lc\_serializable`classmethod`¶

```
is_lc_serializable() -> bool
```

`BaseMessage` is serializable.

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | True |

#### ``get\_lc\_namespace`classmethod`¶

```
get_lc_namespace() -> list[str]
```

Get the namespace of the LangChain object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[str]` | `["langchain", "schema", "messages"]` |

#### ``lc\_id`classmethod`¶

```
lc_id() -> list[str]
```

Return a unique identifier for this class for serialization purposes.

The unique identifier is a list of strings that describes the path
to the object.

For example, for the class `langchain.llms.openai.OpenAI`, the id is
`["langchain", "llms", "openai", "OpenAI"]`.

#### ``to\_json ¶

```
to_json() -> SerializedConstructor | SerializedNotImplemented
```

Serialize the object to JSON.

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If the class has deprecated attributes. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedConstructor | SerializedNotImplemented` | A JSON serializable object or a `SerializedNotImplemented` object. |

#### ``to\_json\_not\_implemented ¶

```
to_json_not_implemented() -> SerializedNotImplemented
```

Serialize a "not implemented" object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedNotImplemented` | `SerializedNotImplemented`. |

#### ``pretty\_repr ¶

```
pretty_repr(html: bool = False) -> str
```

Get a pretty representation of the message.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `html` ¶ "Copy anchor link to this section for reference") | Whether to format the message as HTML. If `True`, the message will be<br>formatted with HTML tags.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | A pretty representation of the message. |

#### ``pretty\_print ¶

```
pretty_print() -> None
```

Print a pretty representation of the message.

#### ``\_\_add\_\_ ¶

```
__add__(other: Any) -> BaseMessageChunk
```

Message chunks support concatenation with other message chunks.

This functionality is useful to combine message chunks yielded from
a streaming model into a complete message.

| PARAMETER | DESCRIPTION |
| --- | --- |
| ##### `other` ¶ "Copy anchor link to this section for reference") | Another message chunk to concatenate with this one.<br>**TYPE:**`Any` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `BaseMessageChunk` | A new message chunk that is the concatenation of this message chunk |
| `BaseMessageChunk` | and the other message chunk. |

| RAISES | DESCRIPTION |
| --- | --- |
| `TypeError` | If the other object is not a message chunk. |

For example,

`AIMessageChunk(content="Hello") + AIMessageChunk(content=" World")`

will give `AIMessageChunk(content="Hello World")`

## ``langchain\_core.language\_models.fake\_chat\_models ¶

Fake chat models for testing purposes.

### ``GenericFakeChatModel ¶

Bases: `BaseChatModel`

Generic fake chat model that can be used to test the chat model interface.

- Chat model should be usable in both sync and async tests
- Invokes `on_llm_new_token` to allow for testing of callback related code for new
tokens.
- Includes logic to break messages into message chunk to facilitate testing of
streaming.

| METHOD | DESCRIPTION |
| --- | --- |
| `get_name` | Get the name of the `Runnable`. |
| `get_input_schema` | Get a Pydantic model that can be used to validate input to the `Runnable`. |
| `get_input_jsonschema` | Get a JSON schema that represents the input to the `Runnable`. |
| `get_output_schema` | Get a Pydantic model that can be used to validate output to the `Runnable`. |
| `get_output_jsonschema` | Get a JSON schema that represents the output of the `Runnable`. |
| `config_schema` | The type of config this `Runnable` accepts specified as a Pydantic model. |
| `get_config_jsonschema` | Get a JSON schema that represents the config of the `Runnable`. |
| `get_graph` | Return a graph representation of this `Runnable`. |
| `get_prompts` | Return a list of prompts used by this `Runnable`. |
| `__or__` | Runnable "or" operator. |
| `__ror__` | Runnable "reverse-or" operator. |
| `pipe` | Pipe `Runnable` objects. |
| `pick` | Pick keys from the output `dict` of this `Runnable`. |
| `assign` | Assigns new fields to the `dict` output of this `Runnable`. |
| `invoke` | Transform a single input into an output. |
| `ainvoke` | Transform a single input into an output. |
| `batch` | Default implementation runs invoke in parallel using a thread pool executor. |
| `batch_as_completed` | Run `invoke` in parallel on a list of inputs. |
| `abatch` | Default implementation runs `ainvoke` in parallel using `asyncio.gather`. |
| `abatch_as_completed` | Run `ainvoke` in parallel on a list of inputs. |
| `stream` | Default implementation of `stream`, which calls `invoke`. |
| `astream` | Default implementation of `astream`, which calls `ainvoke`. |
| `astream_log` | Stream all output from a `Runnable`, as reported to the callback system. |
| `astream_events` | Generate a stream of events. |
| `transform` | Transform inputs to outputs. |
| `atransform` | Transform inputs to outputs. |
| `bind` | Bind arguments to a `Runnable`, returning a new `Runnable`. |
| `with_config` | Bind config to a `Runnable`, returning a new `Runnable`. |
| `with_listeners` | Bind lifecycle listeners to a `Runnable`, returning a new `Runnable`. |
| `with_alisteners` | Bind async lifecycle listeners to a `Runnable`. |
| `with_types` | Bind input and output types to a `Runnable`, returning a new `Runnable`. |
| `with_retry` | Create a new `Runnable` that retries the original `Runnable` on exceptions. |
| `map` | Return a new `Runnable` that maps a list of inputs to a list of outputs. |
| `with_fallbacks` | Add fallbacks to a `Runnable`, returning a new `Runnable`. |
| `as_tool` | Create a `BaseTool` from a `Runnable`. |
| `__init__` |  |
| `is_lc_serializable` | Is this class serializable? |
| `get_lc_namespace` | Get the namespace of the LangChain object. |
| `lc_id` | Return a unique identifier for this class for serialization purposes. |
| `to_json` | Serialize the `Runnable` to JSON. |
| `to_json_not_implemented` | Serialize a "not implemented" object. |
| `configurable_fields` | Configure particular `Runnable` fields at runtime. |
| `configurable_alternatives` | Configure alternatives for `Runnable` objects that can be set at runtime. |
| `set_verbose` | If verbose is `None`, set it. |
| `generate_prompt` | Pass a sequence of prompts to the model and return model generations. |
| `agenerate_prompt` | Asynchronously pass a sequence of prompts and return model generations. |
| `with_structured_output` | Model wrapper that returns outputs formatted to match the given schema. |
| `get_token_ids` | Return the ordered IDs of the tokens in a text. |
| `get_num_tokens` | Get the number of tokens present in the text. |
| `get_num_tokens_from_messages` | Get the number of tokens in the messages. |
| `generate` | Pass a sequence of prompts to the model and return model generations. |
| `agenerate` | Asynchronously pass a sequence of prompts to a model and return generations. |
| `dict` | Return a dictionary of the LLM. |
| `bind_tools` | Bind tools to the model. |

#### ``messages`instance-attribute`¶

```
messages: Iterator[AIMessage | str]
```

Get an iterator over messages.

This can be expanded to accept other types like Callables / dicts / strings
to make the interface more generic if needed.

Note

if you want to pass a list, you can use `iter` to convert it to an iterator.

Warning

Streaming is not implemented yet. We should try to implement it in the future by
delegating to invoke and then breaking the resulting output into message chunks.

#### ``name`class-attribute``instance-attribute`¶

```
name: str | None = None
```

The name of the `Runnable`. Used for debugging and tracing.

#### ``InputType`property`¶

```
InputType: TypeAlias
```

Get the input type for this `Runnable`.

#### ``OutputType`property`¶

```
OutputType: Any
```

Get the output type for this `Runnable`.

#### ``input\_schema`property`¶

```
input_schema: type[BaseModel]
```

The type of input this `Runnable` accepts specified as a Pydantic model.

#### ``output\_schema`property`¶

```
output_schema: type[BaseModel]
```

Output schema.

The type of output this `Runnable` produces specified as a Pydantic model.

#### ``config\_specs`property`¶

```
config_specs: list[ConfigurableFieldSpec]
```

List configurable fields for this `Runnable`.

#### ``lc\_secrets`property`¶

```
lc_secrets: dict[str, str]
```

A map of constructor argument names to secret ids.

For example, `{"openai_api_key": "OPENAI_API_KEY"}`

#### ``lc\_attributes`property`¶

```
lc_attributes: dict
```

List of attribute names that should be included in the serialized kwargs.

These attributes must be accepted by the constructor.

Default is an empty dictionary.

#### ``cache`class-attribute``instance-attribute`¶

```
cache: BaseCache | bool | None = Field(default=None, exclude=True)
```

Whether to cache the response.

- If `True`, will use the global cache.
- If `False`, will not use a cache
- If `None`, will use the global cache if it's set, otherwise no cache.
- If instance of `BaseCache`, will use the provided cache.

Caching is not currently supported for streaming methods of models.

#### ``verbose`class-attribute``instance-attribute`¶

```
verbose: bool = Field(default_factory=_get_verbosity, exclude=True, repr=False)
```

Whether to print out response text.

#### ``callbacks`class-attribute``instance-attribute`¶

```
callbacks: Callbacks = Field(default=None, exclude=True)
```

Callbacks to add to the run trace.

#### ``tags`class-attribute``instance-attribute`¶

```
tags: list[str] | None = Field(default=None, exclude=True)
```

Tags to add to the run trace.

#### ``metadata`class-attribute``instance-attribute`¶

```
metadata: dict[str, Any] | None = Field(default=None, exclude=True)
```

Metadata to add to the run trace.

#### ``custom\_get\_token\_ids`class-attribute``instance-attribute`¶

```
custom_get_token_ids: Callable[[str], list[int]] | None = Field(
    default=None, exclude=True
)
```

Optional encoder to use for counting tokens.

#### ``rate\_limiter`class-attribute``instance-attribute`¶

```
rate_limiter: BaseRateLimiter | None = Field(default=None, exclude=True)
```

An optional rate limiter to use for limiting the number of requests.

#### ``disable\_streaming`class-attribute``instance-attribute`¶

```
disable_streaming: bool | Literal['tool_calling'] = False
```

Whether to disable streaming for this model.

If streaming is bypassed, then `stream`/`astream`/`astream_events` will
defer to `invoke`/`ainvoke`.

- If `True`, will always bypass streaming case.
- If `'tool_calling'`, will bypass streaming case only when the model is called
with a `tools` keyword argument. In other words, LangChain will automatically
switch to non-streaming behavior (`invoke`) only when the tools argument is
provided. This offers the best of both worlds.
- If `False` (Default), will always use streaming case if available.

The main reason for this flag is that code might be written using `stream` and
a user may want to swap out a given model for another model whose the implementation
does not properly support streaming.

#### ``output\_version`class-attribute``instance-attribute`¶

```
output_version: str | None = Field(
    default_factory=from_env("LC_OUTPUT_VERSION", default=None)
)
```

Version of `AIMessage` output format to store in message content.

`AIMessage.content_blocks` will lazily parse the contents of `content` into a
standard format. This flag can be used to additionally store the standard format
in message content, e.g., for serialization purposes.

Supported values:

- `'v0'`: provider-specific format in content (can lazily-parse with
`content_blocks`)
- `'v1'`: standardized format in content (consistent with `content_blocks`)

Partner packages (e.g.,
`langchain-openai`) can also use this
field to roll out new content formats in a backward-compatible way.

Added in version 1.0

#### ``profile`property`¶

```
profile: ModelProfile
```

Return profiling information for the model.

This property relies on the `langchain-model-profiles` package to retrieve chat
model capabilities, such as context window sizes and supported features.

| RAISES | DESCRIPTION |
| --- | --- |
| `ImportError` | If `langchain-model-profiles` is not installed. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelProfile` | A `ModelProfile` object containing profiling information for the model. |

#### ``get\_name ¶

```
get_name(suffix: str | None = None, *, name: str | None = None) -> str
```

Get the name of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `suffix` | An optional suffix to append to the name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `name` | An optional name to use instead of the `Runnable`'s name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | The name of the `Runnable`. |

#### ``get\_input\_schema ¶

```
get_input_schema(config: RunnableConfig | None = None) -> type[BaseModel]
```

Get a Pydantic model that can be used to validate input to the `Runnable`.

`Runnable` objects that leverage the `configurable_fields` and
`configurable_alternatives` methods will have a dynamic input schema that
depends on which configuration the `Runnable` is invoked with.

This method allows to get an input schema for a specific configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate input. |

#### ``get\_input\_jsonschema ¶

```
get_input_jsonschema(config: RunnableConfig | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the input to the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the input to the `Runnable`. |

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

runnable = RunnableLambda(add_one)

print(runnable.get_input_jsonschema())
```

Added in version 0.3.0

#### ``get\_output\_schema ¶

```
get_output_schema(config: RunnableConfig | None = None) -> type[BaseModel]
```

Get a Pydantic model that can be used to validate output to the `Runnable`.

`Runnable` objects that leverage the `configurable_fields` and
`configurable_alternatives` methods will have a dynamic output schema that
depends on which configuration the `Runnable` is invoked with.

This method allows to get an output schema for a specific configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate output. |

#### ``get\_output\_jsonschema ¶

```
get_output_jsonschema(config: RunnableConfig | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the output of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the output of the `Runnable`. |

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

runnable = RunnableLambda(add_one)

print(runnable.get_output_jsonschema())
```

Added in version 0.3.0

#### ``config\_schema ¶

```
config_schema(*, include: Sequence[str] | None = None) -> type[BaseModel]
```

The type of config this `Runnable` accepts specified as a Pydantic model.

To mark a field as configurable, see the `configurable_fields`
and `configurable_alternatives` methods.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `include` | A list of fields to include in the config schema.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate config. |

#### ``get\_config\_jsonschema ¶

```
get_config_jsonschema(*, include: Sequence[str] | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the config of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `include` | A list of fields to include in the config schema.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the config of the `Runnable`. |

Added in version 0.3.0

#### ``get\_graph ¶

```
get_graph(config: RunnableConfig | None = None) -> Graph
```

Return a graph representation of this `Runnable`.

#### ``get\_prompts ¶

```
get_prompts(config: RunnableConfig | None = None) -> list[BasePromptTemplate]
```

Return a list of prompts used by this `Runnable`.

#### ``\_\_or\_\_ ¶

```
__or__(
    other: Runnable[Any, Other]
    | Callable[[Iterator[Any]], Iterator[Other]]
    | Callable[[AsyncIterator[Any]], AsyncIterator[Other]]
    | Callable[[Any], Other]
    | Mapping[str, Runnable[Any, Other] | Callable[[Any], Other] | Any],
) -> RunnableSerializable[Input, Other]
```

Runnable "or" operator.

Compose this `Runnable` with another object to create a
`RunnableSequence`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `other` | Another `Runnable` or a `Runnable`-like object.<br>**TYPE:**`Runnable[Any, Other] | Callable[[Iterator[Any]], Iterator[Other]] | Callable[[AsyncIterator[Any]], AsyncIterator[Other]] | Callable[[Any], Other] | Mapping[str, Runnable[Any, Other] | Callable[[Any], Other] | Any]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Other]` | A new `Runnable`. |

#### ``\_\_ror\_\_ ¶

```
__ror__(
    other: Runnable[Other, Any]
    | Callable[[Iterator[Other]], Iterator[Any]]
    | Callable[[AsyncIterator[Other]], AsyncIterator[Any]]
    | Callable[[Other], Any]
    | Mapping[str, Runnable[Other, Any] | Callable[[Other], Any] | Any],
) -> RunnableSerializable[Other, Output]
```

Runnable "reverse-or" operator.

Compose this `Runnable` with another object to create a
`RunnableSequence`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `other` | Another `Runnable` or a `Runnable`-like object.<br>**TYPE:**`Runnable[Other, Any] | Callable[[Iterator[Other]], Iterator[Any]] | Callable[[AsyncIterator[Other]], AsyncIterator[Any]] | Callable[[Other], Any] | Mapping[str, Runnable[Other, Any] | Callable[[Other], Any] | Any]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Other, Output]` | A new `Runnable`. |

#### ``pipe ¶

```
pipe(
    *others: Runnable[Any, Other] | Callable[[Any], Other], name: str | None = None
) -> RunnableSerializable[Input, Other]
```

Pipe `Runnable` objects.

Compose this `Runnable` with `Runnable`-like objects to make a
`RunnableSequence`.

Equivalent to `RunnableSequence(self, *others)` or `self | others[0] | ...`

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

def mul_two(x: int) -> int:
    return x * 2

runnable_1 = RunnableLambda(add_one)
runnable_2 = RunnableLambda(mul_two)
sequence = runnable_1.pipe(runnable_2)
# Or equivalently:
# sequence = runnable_1 | runnable_2
# sequence = RunnableSequence(first=runnable_1, last=runnable_2)
sequence.invoke(1)
await sequence.ainvoke(1)
# -> 4

sequence.batch([1, 2, 3])
await sequence.abatch([1, 2, 3])
# -> [4, 6, 8]
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `*others` | Other `Runnable` or `Runnable`-like objects to compose<br>**TYPE:**`Runnable[Any, Other] | Callable[[Any], Other]`**DEFAULT:**`()` |
| `name` | An optional name for the resulting `RunnableSequence`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Other]` | A new `Runnable`. |

#### ``pick ¶

```
pick(keys: str | list[str]) -> RunnableSerializable[Any, Any]
```

Pick keys from the output `dict` of this `Runnable`.

Pick a single key:

```
import json

from langchain_core.runnables import RunnableLambda, RunnableMap

as_str = RunnableLambda(str)
as_json = RunnableLambda(json.loads)
chain = RunnableMap(str=as_str, json=as_json)

chain.invoke("[1, 2, 3]")
# -> {"str": "[1, 2, 3]", "json": [1, 2, 3]}

json_only_chain = chain.pick("json")
json_only_chain.invoke("[1, 2, 3]")
# -> [1, 2, 3]
```

Pick a list of keys:

```
from typing import Any

import json

from langchain_core.runnables import RunnableLambda, RunnableMap

as_str = RunnableLambda(str)
as_json = RunnableLambda(json.loads)

def as_bytes(x: Any) -> bytes:
    return bytes(x, "utf-8")

chain = RunnableMap(str=as_str, json=as_json, bytes=RunnableLambda(as_bytes))

chain.invoke("[1, 2, 3]")
# -> {"str": "[1, 2, 3]", "json": [1, 2, 3], "bytes": b"[1, 2, 3]"}

json_and_bytes_chain = chain.pick(["json", "bytes"])
json_and_bytes_chain.invoke("[1, 2, 3]")
# -> {"json": [1, 2, 3], "bytes": b"[1, 2, 3]"}
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `keys` | A key or list of keys to pick from the output dict.<br>**TYPE:**`str | list[str]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Any, Any]` | a new `Runnable`. |

#### ``assign ¶

```
assign(
    **kwargs: Runnable[dict[str, Any], Any]
    | Callable[[dict[str, Any]], Any]
    | Mapping[str, Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any]],
) -> RunnableSerializable[Any, Any]
```

Assigns new fields to the `dict` output of this `Runnable`.

```
from langchain_core.language_models.fake import FakeStreamingListLLM
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import SystemMessagePromptTemplate
from langchain_core.runnables import Runnable
from operator import itemgetter

prompt = (
    SystemMessagePromptTemplate.from_template("You are a nice assistant.")
    + "{question}"
)
model = FakeStreamingListLLM(responses=["foo-lish"])

chain: Runnable = prompt | model | {"str": StrOutputParser()}

chain_with_assign = chain.assign(hello=itemgetter("str") | model)

print(chain_with_assign.input_schema.model_json_schema())
# {'title': 'PromptInput', 'type': 'object', 'properties':
{'question': {'title': 'Question', 'type': 'string'}}}
print(chain_with_assign.output_schema.model_json_schema())
# {'title': 'RunnableSequenceOutput', 'type': 'object', 'properties':
{'str': {'title': 'Str',
'type': 'string'}, 'hello': {'title': 'Hello', 'type': 'string'}}}
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**kwargs` | A mapping of keys to `Runnable` or `Runnable`-like objects<br>that will be invoked with the entire output dict of this `Runnable`.<br>**TYPE:**`Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any] | Mapping[str, Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any]]`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Any, Any]` | A new `Runnable`. |

#### ``invoke ¶

```
invoke(
    input: LanguageModelInput,
    config: RunnableConfig | None = None,
    *,
    stop: list[str] | None = None,
    **kwargs: Any,
) -> AIMessage
```

Transform a single input into an output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``ainvoke`async`¶

```
ainvoke(
    input: LanguageModelInput,
    config: RunnableConfig | None = None,
    *,
    stop: list[str] | None = None,
    **kwargs: Any,
) -> AIMessage
```

Transform a single input into an output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``batch ¶

```
batch(
    inputs: list[Input],
    config: RunnableConfig | list[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> list[Output]
```

Default implementation runs invoke in parallel using a thread pool executor.

The default implementation of batch works well for IO bound runnables.

Subclasses must override this method if they can batch more efficiently;
e.g., if the underlying `Runnable` uses an API which supports a batch mode.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`list[Input]` |
| `config` | A config to use when invoking the `Runnable`. The config supports<br>standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work<br>to do in parallel, and other keys. Please refer to the<br>`RunnableConfig` for more details.<br>**TYPE:**`RunnableConfig | list[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Output]` | A list of outputs from the `Runnable`. |

#### ``batch\_as\_completed ¶

```
batch_as_completed(
    inputs: Sequence[Input],
    config: RunnableConfig | Sequence[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> Iterator[tuple[int, Output | Exception]]
```

Run `invoke` in parallel on a list of inputs.

Yields results as they complete.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`Sequence[Input]` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | Sequence[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `tuple[int, Output | Exception]` | Tuples of the index of the input and the output from the `Runnable`. |

#### ``abatch`async`¶

```
abatch(
    inputs: list[Input],
    config: RunnableConfig | list[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> list[Output]
```

Default implementation runs `ainvoke` in parallel using `asyncio.gather`.

The default implementation of `batch` works well for IO bound runnables.

Subclasses must override this method if they can batch more efficiently;
e.g., if the underlying `Runnable` uses an API which supports a batch mode.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`list[Input]` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | list[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Output]` | A list of outputs from the `Runnable`. |

#### ``abatch\_as\_completed`async`¶

```
abatch_as_completed(
    inputs: Sequence[Input],
    config: RunnableConfig | Sequence[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> AsyncIterator[tuple[int, Output | Exception]]
```

Run `ainvoke` in parallel on a list of inputs.

Yields results as they complete.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`Sequence[Input]` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | Sequence[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[tuple[int, Output | Exception]]` | A tuple of the index of the input and the output from the `Runnable`. |

#### ``stream ¶

```
stream(
    input: LanguageModelInput,
    config: RunnableConfig | None = None,
    *,
    stop: list[str] | None = None,
    **kwargs: Any,
) -> Iterator[AIMessageChunk]
```

Default implementation of `stream`, which calls `invoke`.

Subclasses must override this method if they support streaming output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``astream`async`¶

```
astream(
    input: LanguageModelInput,
    config: RunnableConfig | None = None,
    *,
    stop: list[str] | None = None,
    **kwargs: Any,
) -> AsyncIterator[AIMessageChunk]
```

Default implementation of `astream`, which calls `ainvoke`.

Subclasses must override this method if they support streaming output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[Output]` | The output of the `Runnable`. |

#### ``astream\_log`async`¶

```
astream_log(
    input: Any,
    config: RunnableConfig | None = None,
    *,
    diff: bool = True,
    with_streamed_output_list: bool = True,
    include_names: Sequence[str] | None = None,
    include_types: Sequence[str] | None = None,
    include_tags: Sequence[str] | None = None,
    exclude_names: Sequence[str] | None = None,
    exclude_types: Sequence[str] | None = None,
    exclude_tags: Sequence[str] | None = None,
    **kwargs: Any,
) -> AsyncIterator[RunLogPatch] | AsyncIterator[RunLog]
```

Stream all output from a `Runnable`, as reported to the callback system.

This includes all inner runs of LLMs, Retrievers, Tools, etc.

Output is streamed as Log objects, which include a list of
Jsonpatch ops that describe how the state of the run has changed in each
step, and the final state of the run.

The Jsonpatch ops can be applied in order to construct state.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Any` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `diff` | Whether to yield diffs between each step or the current state.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `with_streamed_output_list` | Whether to yield the `streamed_output` list.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `include_names` | Only include logs with these names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_types` | Only include logs with these types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_tags` | Only include logs with these tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_names` | Exclude logs with these names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_types` | Exclude logs with these types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_tags` | Exclude logs with these tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[RunLogPatch] | AsyncIterator[RunLog]` | A `RunLogPatch` or `RunLog` object. |

#### ``astream\_events`async`¶

```
astream_events(
    input: Any,
    config: RunnableConfig | None = None,
    *,
    version: Literal["v1", "v2"] = "v2",
    include_names: Sequence[str] | None = None,
    include_types: Sequence[str] | None = None,
    include_tags: Sequence[str] | None = None,
    exclude_names: Sequence[str] | None = None,
    exclude_types: Sequence[str] | None = None,
    exclude_tags: Sequence[str] | None = None,
    **kwargs: Any,
) -> AsyncIterator[StreamEvent]
```

Generate a stream of events.

Use to create an iterator over `StreamEvent` that provide real-time information
about the progress of the `Runnable`, including `StreamEvent` from intermediate
results.

A `StreamEvent` is a dictionary with the following schema:

- `event`: Event names are of the format:
`on_[runnable_type]_(start|stream|end)`.
- `name`: The name of the `Runnable` that generated the event.
- `run_id`: Randomly generated ID associated with the given execution of the
`Runnable` that emitted the event. A child `Runnable` that gets invoked as
part of the execution of a parent `Runnable` is assigned its own unique ID.
- `parent_ids`: The IDs of the parent runnables that generated the event. The
root `Runnable` will have an empty list. The order of the parent IDs is from
the root to the immediate parent. Only available for v2 version of the API.
The v1 version of the API will return an empty list.
- `tags`: The tags of the `Runnable` that generated the event.
- `metadata`: The metadata of the `Runnable` that generated the event.
- `data`: The data associated with the event. The contents of this field
depend on the type of event. See the table below for more details.

Below is a table that illustrates some events that might be emitted by various
chains. Metadata fields have been omitted from the table for brevity.
Chain definitions have been included after the table.

Note

This reference table is for the v2 version of the schema.

| event | name | chunk | input | output |
| --- | --- | --- | --- | --- |
| `on_chat_model_start` | `'[model name]'` |  | `{"messages": [[SystemMessage, HumanMessage]]}` |  |
| `on_chat_model_stream` | `'[model name]'` | `AIMessageChunk(content="hello")` |  |  |
| `on_chat_model_end` | `'[model name]'` |  | `{"messages": [[SystemMessage, HumanMessage]]}` | `AIMessageChunk(content="hello world")` |
| `on_llm_start` | `'[model name]'` |  | `{'input': 'hello'}` |  |
| `on_llm_stream` | `'[model name]'` | `'Hello'` |  |  |
| `on_llm_end` | `'[model name]'` |  | `'Hello human!'` |  |
| `on_chain_start` | `'format_docs'` |  |  |  |
| `on_chain_stream` | `'format_docs'` | `'hello world!, goodbye world!'` |  |  |
| `on_chain_end` | `'format_docs'` |  | `[Document(...)]` | `'hello world!, goodbye world!'` |
| `on_tool_start` | `'some_tool'` |  | `{"x": 1, "y": "2"}` |  |
| `on_tool_end` | `'some_tool'` |  |  | `{"x": 1, "y": "2"}` |
| `on_retriever_start` | `'[retriever name]'` |  | `{"query": "hello"}` |  |
| `on_retriever_end` | `'[retriever name]'` |  | `{"query": "hello"}` | `[Document(...), ..]` |
| `on_prompt_start` | `'[template_name]'` |  | `{"question": "hello"}` |  |
| `on_prompt_end` | `'[template_name]'` |  | `{"question": "hello"}` | `ChatPromptValue(messages: [SystemMessage, ...])` |

In addition to the standard events, users can also dispatch custom events (see example below).

Custom events will be only be surfaced with in the v2 version of the API!

A custom event has following format:

| Attribute | Type | Description |
| --- | --- | --- |
| `name` | `str` | A user defined name for the event. |
| `data` | `Any` | The data associated with the event. This can be anything, though we suggest making it JSON serializable. |

Here are declarations associated with the standard events shown above:

`format_docs`:

```
def format_docs(docs: list[Document]) -> str:
    '''Format the docs.'''
    return ", ".join([doc.page_content for doc in docs])

format_docs = RunnableLambda(format_docs)
```

`some_tool`:

```
@tool
def some_tool(x: int, y: str) -> dict:
    '''Some_tool.'''
    return {"x": x, "y": y}
```

`prompt`:

```
template = ChatPromptTemplate.from_messages(
    [\
        ("system", "You are Cat Agent 007"),\
        ("human", "{question}"),\
    ]
).with_config({"run_name": "my_template", "tags": ["my_template"]})
```

For instance:

```
from langchain_core.runnables import RunnableLambda

async def reverse(s: str) -> str:
    return s[::-1]

chain = RunnableLambda(func=reverse)

events = [event async for event in chain.astream_events("hello", version="v2")]

# Will produce the following events
# (run_id, and parent_ids has been omitted for brevity):
[\
    {\
        "data": {"input": "hello"},\
        "event": "on_chain_start",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
    {\
        "data": {"chunk": "olleh"},\
        "event": "on_chain_stream",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
    {\
        "data": {"output": "olleh"},\
        "event": "on_chain_end",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
]
```

Example: Dispatch Custom Event

```
from langchain_core.callbacks.manager import (
    adispatch_custom_event,
)
from langchain_core.runnables import RunnableLambda, RunnableConfig
import asyncio

async def slow_thing(some_input: str, config: RunnableConfig) -> str:
    """Do something that takes a long time."""
    await asyncio.sleep(1) # Placeholder for some slow operation
    await adispatch_custom_event(
        "progress_event",
        {"message": "Finished step 1 of 3"},
        config=config # Must be included for python < 3.10
    )
    await asyncio.sleep(1) # Placeholder for some slow operation
    await adispatch_custom_event(
        "progress_event",
        {"message": "Finished step 2 of 3"},
        config=config # Must be included for python < 3.10
    )
    await asyncio.sleep(1) # Placeholder for some slow operation
    return "Done"

slow_thing = RunnableLambda(slow_thing)

async for event in slow_thing.astream_events("some_input", version="v2"):
    print(event)
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Any` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `version` | The version of the schema to use either `'v2'` or `'v1'`.<br>Users should use `'v2'`.<br>`'v1'` is for backwards compatibility and will be deprecated<br>in `0.4.0`.<br>No default will be assigned until the API is stabilized.<br>custom events will only be surfaced in `'v2'`.<br>**TYPE:**`Literal['v1', 'v2']`**DEFAULT:**`'v2'` |
| `include_names` | Only include events from `Runnable` objects with matching names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_types` | Only include events from `Runnable` objects with matching types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_tags` | Only include events from `Runnable` objects with matching tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_names` | Exclude events from `Runnable` objects with matching names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_types` | Exclude events from `Runnable` objects with matching types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_tags` | Exclude events from `Runnable` objects with matching tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>These will be passed to `astream_log` as this implementation<br>of `astream_events` is built on top of `astream_log`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[StreamEvent]` | An async stream of `StreamEvent`. |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | If the version is not `'v1'` or `'v2'`. |

#### ``transform ¶

```
transform(
    input: Iterator[Input], config: RunnableConfig | None = None, **kwargs: Any | None
) -> Iterator[Output]
```

Transform inputs to outputs.

Default implementation of transform, which buffers input and calls `astream`.

Subclasses must override this method if they can start producing output while
input is still being generated.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | An iterator of inputs to the `Runnable`.<br>**TYPE:**`Iterator[Input]` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``atransform`async`¶

```
atransform(
    input: AsyncIterator[Input],
    config: RunnableConfig | None = None,
    **kwargs: Any | None,
) -> AsyncIterator[Output]
```

Transform inputs to outputs.

Default implementation of atransform, which buffers input and calls `astream`.

Subclasses must override this method if they can start producing output while
input is still being generated.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | An async iterator of inputs to the `Runnable`.<br>**TYPE:**`AsyncIterator[Input]` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[Output]` | The output of the `Runnable`. |

#### ``bind ¶

```
bind(**kwargs: Any) -> Runnable[Input, Output]
```

Bind arguments to a `Runnable`, returning a new `Runnable`.

Useful when a `Runnable` in a chain requires an argument that is not
in the output of the previous `Runnable` or included in the user input.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**kwargs` | The arguments to bind to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the arguments bound. |

Example

```
from langchain_ollama import ChatOllama
from langchain_core.output_parsers import StrOutputParser

model = ChatOllama(model="llama3.1")

# Without bind
chain = model | StrOutputParser()

chain.invoke("Repeat quoted words exactly: 'One two three four five.'")
# Output is 'One two three four five.'

# With bind
chain = model.bind(stop=["three"]) | StrOutputParser()

chain.invoke("Repeat quoted words exactly: 'One two three four five.'")
# Output is 'One two'
```

#### ``with\_config ¶

```
with_config(
    config: RunnableConfig | None = None, **kwargs: Any
) -> Runnable[Input, Output]
```

Bind config to a `Runnable`, returning a new `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to bind to the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the config bound. |

#### ``with\_listeners ¶

```
with_listeners(
    *,
    on_start: Callable[[Run], None]
    | Callable[[Run, RunnableConfig], None]
    | None = None,
    on_end: Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None = None,
    on_error: Callable[[Run], None]
    | Callable[[Run, RunnableConfig], None]
    | None = None,
) -> Runnable[Input, Output]
```

Bind lifecycle listeners to a `Runnable`, returning a new `Runnable`.

The Run object contains information about the run, including its `id`,
`type`, `input`, `output`, `error`, `start_time`, `end_time`, and
any tags or metadata added to the run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `on_start` | Called before the `Runnable` starts running, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |
| `on_end` | Called after the `Runnable` finishes running, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |
| `on_error` | Called if the `Runnable` throws an error, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the listeners bound. |

Example

```
from langchain_core.runnables import RunnableLambda
from langchain_core.tracers.schemas import Run

import time

def test_runnable(time_to_sleep: int):
    time.sleep(time_to_sleep)

def fn_start(run_obj: Run):
    print("start_time:", run_obj.start_time)

def fn_end(run_obj: Run):
    print("end_time:", run_obj.end_time)

chain = RunnableLambda(test_runnable).with_listeners(
    on_start=fn_start, on_end=fn_end
)
chain.invoke(2)
```

#### ``with\_alisteners ¶

```
with_alisteners(
    *,
    on_start: AsyncListener | None = None,
    on_end: AsyncListener | None = None,
    on_error: AsyncListener | None = None,
) -> Runnable[Input, Output]
```

Bind async lifecycle listeners to a `Runnable`.

Returns a new `Runnable`.

The Run object contains information about the run, including its `id`,
`type`, `input`, `output`, `error`, `start_time`, `end_time`, and
any tags or metadata added to the run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `on_start` | Called asynchronously before the `Runnable` starts running,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |
| `on_end` | Called asynchronously after the `Runnable` finishes running,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |
| `on_error` | Called asynchronously if the `Runnable` throws an error,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the listeners bound. |

Example

```
from langchain_core.runnables import RunnableLambda, Runnable
from datetime import datetime, timezone
import time
import asyncio

def format_t(timestamp: float) -> str:
    return datetime.fromtimestamp(timestamp, tz=timezone.utc).isoformat()

async def test_runnable(time_to_sleep: int):
    print(f"Runnable[{time_to_sleep}s]: starts at {format_t(time.time())}")
    await asyncio.sleep(time_to_sleep)
    print(f"Runnable[{time_to_sleep}s]: ends at {format_t(time.time())}")

async def fn_start(run_obj: Runnable):
    print(f"on start callback starts at {format_t(time.time())}")
    await asyncio.sleep(3)
    print(f"on start callback ends at {format_t(time.time())}")

async def fn_end(run_obj: Runnable):
    print(f"on end callback starts at {format_t(time.time())}")
    await asyncio.sleep(2)
    print(f"on end callback ends at {format_t(time.time())}")

runnable = RunnableLambda(test_runnable).with_alisteners(
    on_start=fn_start,
    on_end=fn_end
)
async def concurrent_runs():
    await asyncio.gather(runnable.ainvoke(2), runnable.ainvoke(3))

asyncio.run(concurrent_runs())
Result:
on start callback starts at 2025-03-01T07:05:22.875378+00:00
on start callback starts at 2025-03-01T07:05:22.875495+00:00
on start callback ends at 2025-03-01T07:05:25.878862+00:00
on start callback ends at 2025-03-01T07:05:25.878947+00:00
Runnable[2s]: starts at 2025-03-01T07:05:25.879392+00:00
Runnable[3s]: starts at 2025-03-01T07:05:25.879804+00:00
Runnable[2s]: ends at 2025-03-01T07:05:27.881998+00:00
on end callback starts at 2025-03-01T07:05:27.882360+00:00
Runnable[3s]: ends at 2025-03-01T07:05:28.881737+00:00
on end callback starts at 2025-03-01T07:05:28.882428+00:00
on end callback ends at 2025-03-01T07:05:29.883893+00:00
on end callback ends at 2025-03-01T07:05:30.884831+00:00
```

#### ``with\_types ¶

```
with_types(
    *, input_type: type[Input] | None = None, output_type: type[Output] | None = None
) -> Runnable[Input, Output]
```

Bind input and output types to a `Runnable`, returning a new `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input_type` | The input type to bind to the `Runnable`.<br>**TYPE:**`type[Input] | None`**DEFAULT:**`None` |
| `output_type` | The output type to bind to the `Runnable`.<br>**TYPE:**`type[Output] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the types bound. |

#### ``with\_retry ¶

```
with_retry(
    *,
    retry_if_exception_type: tuple[type[BaseException], ...] = (Exception,),
    wait_exponential_jitter: bool = True,
    exponential_jitter_params: ExponentialJitterParams | None = None,
    stop_after_attempt: int = 3,
) -> Runnable[Input, Output]
```

Create a new `Runnable` that retries the original `Runnable` on exceptions.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `retry_if_exception_type` | A tuple of exception types to retry on.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| `wait_exponential_jitter` | Whether to add jitter to the wait<br>time between retries.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `stop_after_attempt` | The maximum number of attempts to make before<br>giving up.<br>**TYPE:**`int`**DEFAULT:**`3` |
| `exponential_jitter_params` | Parameters for<br>`tenacity.wait_exponential_jitter`. Namely: `initial`, `max`,<br>`exp_base`, and `jitter` (all `float` values).<br>**TYPE:**`ExponentialJitterParams | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new Runnable that retries the original Runnable on exceptions. |

Example

```
from langchain_core.runnables import RunnableLambda

count = 0

def _lambda(x: int) -> None:
    global count
    count = count + 1
    if x == 1:
        raise ValueError("x is 1")
    else:
        pass

runnable = RunnableLambda(_lambda)
try:
    runnable.with_retry(
        stop_after_attempt=2,
        retry_if_exception_type=(ValueError,),
    ).invoke(1)
except ValueError:
    pass

assert count == 2
```

#### ``map ¶

```
map() -> Runnable[list[Input], list[Output]]
```

Return a new `Runnable` that maps a list of inputs to a list of outputs.

Calls `invoke` with each input.

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[list[Input], list[Output]]` | A new `Runnable` that maps a list of inputs to a list of outputs. |

Example

```
from langchain_core.runnables import RunnableLambda

def _lambda(x: int) -> int:
    return x + 1

runnable = RunnableLambda(_lambda)
print(runnable.map().invoke([1, 2, 3]))  # [2, 3, 4]
```

#### ``with\_fallbacks ¶

```
with_fallbacks(
    fallbacks: Sequence[Runnable[Input, Output]],
    *,
    exceptions_to_handle: tuple[type[BaseException], ...] = (Exception,),
    exception_key: str | None = None,
) -> RunnableWithFallbacks[Input, Output]
```

Add fallbacks to a `Runnable`, returning a new `Runnable`.

The new `Runnable` will try the original `Runnable`, and then each fallback
in order, upon failures.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `fallbacks` | A sequence of runnables to try if the original `Runnable`<br>fails.<br>**TYPE:**`Sequence[Runnable[Input, Output]]` |
| `exceptions_to_handle` | A tuple of exception types to handle.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| `exception_key` | If `string` is specified then handled exceptions will be<br>passed to fallbacks as part of the input under the specified key.<br>If `None`, exceptions will not be passed to fallbacks.<br>If used, the base `Runnable` and its fallbacks must accept a<br>dictionary as input.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableWithFallbacks[Input, Output]` | A new `Runnable` that will try the original `Runnable`, and then each<br>Fallback in order, upon failures. |

Example

```
from typing import Iterator

from langchain_core.runnables import RunnableGenerator

def _generate_immediate_error(input: Iterator) -> Iterator[str]:
    raise ValueError()
    yield ""

def _generate(input: Iterator) -> Iterator[str]:
    yield from "foo bar"

runnable = RunnableGenerator(_generate_immediate_error).with_fallbacks(
    [RunnableGenerator(_generate)]
)
print("".join(runnable.stream({})))  # foo bar
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `fallbacks` | A sequence of runnables to try if the original `Runnable`<br>fails.<br>**TYPE:**`Sequence[Runnable[Input, Output]]` |
| `exceptions_to_handle` | A tuple of exception types to handle.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| `exception_key` | If `string` is specified then handled exceptions will be<br>passed to fallbacks as part of the input under the specified key.<br>If `None`, exceptions will not be passed to fallbacks.<br>If used, the base `Runnable` and its fallbacks must accept a<br>dictionary as input.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableWithFallbacks[Input, Output]` | A new `Runnable` that will try the original `Runnable`, and then each<br>Fallback in order, upon failures. |

#### ``as\_tool ¶

```
as_tool(
    args_schema: type[BaseModel] | None = None,
    *,
    name: str | None = None,
    description: str | None = None,
    arg_types: dict[str, type] | None = None,
) -> BaseTool
```

Create a `BaseTool` from a `Runnable`.

`as_tool` will instantiate a `BaseTool` with a name, description, and
`args_schema` from a `Runnable`. Where possible, schemas are inferred
from `runnable.get_input_schema`. Alternatively (e.g., if the
`Runnable` takes a dict as input and the specific dict keys are not typed),
the schema can be specified directly with `args_schema`. You can also
pass `arg_types` to just specify the required arguments and their types.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `args_schema` | The schema for the tool.<br>**TYPE:**`type[BaseModel] | None`**DEFAULT:**`None` |
| `name` | The name of the tool.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `description` | The description of the tool.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `arg_types` | A dictionary of argument names to types.<br>**TYPE:**`dict[str, type] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `BaseTool` | A `BaseTool` instance. |

Typed dict input:

```
from typing_extensions import TypedDict
from langchain_core.runnables import RunnableLambda

class Args(TypedDict):
    a: int
    b: list[int]

def f(x: Args) -> str:
    return str(x["a"] * max(x["b"]))

runnable = RunnableLambda(f)
as_tool = runnable.as_tool()
as_tool.invoke({"a": 3, "b": [1, 2]})
```

`dict` input, specifying schema via `args_schema`:

```
from typing import Any
from pydantic import BaseModel, Field
from langchain_core.runnables import RunnableLambda

def f(x: dict[str, Any]) -> str:
    return str(x["a"] * max(x["b"]))

class FSchema(BaseModel):
    """Apply a function to an integer and list of integers."""

    a: int = Field(..., description="Integer")
    b: list[int] = Field(..., description="List of ints")

runnable = RunnableLambda(f)
as_tool = runnable.as_tool(FSchema)
as_tool.invoke({"a": 3, "b": [1, 2]})
```

`dict` input, specifying schema via `arg_types`:

```
from typing import Any
from langchain_core.runnables import RunnableLambda

def f(x: dict[str, Any]) -> str:
    return str(x["a"] * max(x["b"]))

runnable = RunnableLambda(f)
as_tool = runnable.as_tool(arg_types={"a": int, "b": list[int]})
as_tool.invoke({"a": 3, "b": [1, 2]})
```

String input:

```
from langchain_core.runnables import RunnableLambda

def f(x: str) -> str:
    return x + "a"

def g(x: str) -> str:
    return x + "z"

runnable = RunnableLambda(f) | g
as_tool = runnable.as_tool()
as_tool.invoke("b")
```

#### ``\_\_init\_\_ ¶

```
__init__(*args: Any, **kwargs: Any) -> None
```

#### ``is\_lc\_serializable`classmethod`¶

```
is_lc_serializable() -> bool
```

Is this class serializable?

By design, even if a class inherits from `Serializable`, it is not serializable
by default. This is to prevent accidental serialization of objects that should
not be serialized.

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | Whether the class is serializable. Default is `False`. |

#### ``get\_lc\_namespace`classmethod`¶

```
get_lc_namespace() -> list[str]
```

Get the namespace of the LangChain object.

For example, if the class is `langchain.llms.openai.OpenAI`, then the
namespace is `["langchain", "llms", "openai"]`

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[str]` | The namespace. |

#### ``lc\_id`classmethod`¶

```
lc_id() -> list[str]
```

Return a unique identifier for this class for serialization purposes.

The unique identifier is a list of strings that describes the path
to the object.

For example, for the class `langchain.llms.openai.OpenAI`, the id is
`["langchain", "llms", "openai", "OpenAI"]`.

#### ``to\_json ¶

```
to_json() -> SerializedConstructor | SerializedNotImplemented
```

Serialize the `Runnable` to JSON.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedConstructor | SerializedNotImplemented` | A JSON-serializable representation of the `Runnable`. |

#### ``to\_json\_not\_implemented ¶

```
to_json_not_implemented() -> SerializedNotImplemented
```

Serialize a "not implemented" object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedNotImplemented` | `SerializedNotImplemented`. |

#### ``configurable\_fields ¶

```
configurable_fields(
    **kwargs: AnyConfigurableField,
) -> RunnableSerializable[Input, Output]
```

Configure particular `Runnable` fields at runtime.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**kwargs` | A dictionary of `ConfigurableField` instances to configure.<br>**TYPE:**`AnyConfigurableField`**DEFAULT:**`{}` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If a configuration key is not found in the `Runnable`. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Output]` | A new `Runnable` with the fields configured. |

```
from langchain_core.runnables import ConfigurableField
from langchain_openai import ChatOpenAI

model = ChatOpenAI(max_tokens=20).configurable_fields(
    max_tokens=ConfigurableField(
        id="output_token_number",
        name="Max tokens in the output",
        description="The maximum number of tokens in the output",
    )
)

# max_tokens = 20
print("max_tokens_20: ", model.invoke("tell me something about chess").content)

# max_tokens = 200
print(
    "max_tokens_200: ",
    model.with_config(configurable={"output_token_number": 200})
    .invoke("tell me something about chess")
    .content,
)
```

#### ``configurable\_alternatives ¶

```
configurable_alternatives(
    which: ConfigurableField,
    *,
    default_key: str = "default",
    prefix_keys: bool = False,
    **kwargs: Runnable[Input, Output] | Callable[[], Runnable[Input, Output]],
) -> RunnableSerializable[Input, Output]
```

Configure alternatives for `Runnable` objects that can be set at runtime.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `which` | The `ConfigurableField` instance that will be used to select the<br>alternative.<br>**TYPE:**`ConfigurableField` |
| `default_key` | The default key to use if no alternative is selected.<br>**TYPE:**`str`**DEFAULT:**`'default'` |
| `prefix_keys` | Whether to prefix the keys with the `ConfigurableField` id.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | A dictionary of keys to `Runnable` instances or callables that<br>return `Runnable` instances.<br>**TYPE:**`Runnable[Input, Output] | Callable[[], Runnable[Input, Output]]`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Output]` | A new `Runnable` with the alternatives configured. |

```
from langchain_anthropic import ChatAnthropic
from langchain_core.runnables.utils import ConfigurableField
from langchain_openai import ChatOpenAI

model = ChatAnthropic(
    model_name="claude-sonnet-4-5-20250929"
).configurable_alternatives(
    ConfigurableField(id="llm"),
    default_key="anthropic",
    openai=ChatOpenAI(),
)

# uses the default model ChatAnthropic
print(model.invoke("which organization created you?").content)

# uses ChatOpenAI
print(
    model.with_config(configurable={"llm": "openai"})
    .invoke("which organization created you?")
    .content
)
```

#### ``set\_verbose ¶

```
set_verbose(verbose: bool | None) -> bool
```

If verbose is `None`, set it.

This allows users to pass in `None` as verbose to access the global setting.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `verbose` | The verbosity setting to use.<br>**TYPE:**`bool | None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | The verbosity setting to use. |

#### ``generate\_prompt ¶

```
generate_prompt(
    prompts: list[PromptValue],
    stop: list[str] | None = None,
    callbacks: Callbacks = None,
    **kwargs: Any,
) -> LLMResult
```

Pass a sequence of prompts to the model and return model generations.

This method should make use of batched calls for models that expose a batched
API.

Use this method when you want to:

1. Take advantage of batched calls,
2. Need more output from the model than just the top generated value,
3. Are building chains that are agnostic to the underlying language model
    type (e.g., pure text completion models vs chat models).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `prompts` | List of `PromptValue` objects. A `PromptValue` is an object that<br>can be converted to match the format of any language model (string for<br>pure text generation models and `BaseMessage` objects for chat models).<br>**TYPE:**`list[PromptValue]` |
| `stop` | Stop words to use when generating. Model output is cut off at the<br>first occurrence of any of these substrings.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `callbacks` | `Callbacks` to pass through. Used for executing additional<br>functionality, such as logging or streaming, throughout generation.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| `**kwargs` | Arbitrary additional keyword arguments. These are usually passed<br>to the model provider API call.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LLMResult` | An `LLMResult`, which contains a list of candidate `Generation` objects for<br>each input prompt and additional model provider-specific output. |

#### ``agenerate\_prompt`async`¶

```
agenerate_prompt(
    prompts: list[PromptValue],
    stop: list[str] | None = None,
    callbacks: Callbacks = None,
    **kwargs: Any,
) -> LLMResult
```

Asynchronously pass a sequence of prompts and return model generations.

This method should make use of batched calls for models that expose a batched
API.

Use this method when you want to:

1. Take advantage of batched calls,
2. Need more output from the model than just the top generated value,
3. Are building chains that are agnostic to the underlying language model
    type (e.g., pure text completion models vs chat models).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `prompts` | List of `PromptValue` objects. A `PromptValue` is an object that<br>can be converted to match the format of any language model (string for<br>pure text generation models and `BaseMessage` objects for chat models).<br>**TYPE:**`list[PromptValue]` |
| `stop` | Stop words to use when generating. Model output is cut off at the<br>first occurrence of any of these substrings.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `callbacks` | `Callbacks` to pass through. Used for executing additional<br>functionality, such as logging or streaming, throughout generation.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| `**kwargs` | Arbitrary additional keyword arguments. These are usually passed<br>to the model provider API call.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LLMResult` | An `LLMResult`, which contains a list of candidate `Generation` objects for<br>each input prompt and additional model provider-specific output. |

#### ``with\_structured\_output ¶

```
with_structured_output(
    schema: Dict | type, *, include_raw: bool = False, **kwargs: Any
) -> Runnable[LanguageModelInput, Dict | BaseModel]
```

Model wrapper that returns outputs formatted to match the given schema.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `schema` | The output schema. Can be passed in as:<br>- an OpenAI function/tool schema,<br>- a JSON Schema,<br>- a `TypedDict` class,<br>- or a Pydantic class.<br>If `schema` is a Pydantic class then the model output will be a<br>Pydantic instance of that class, and the model-generated fields will be<br>validated by the Pydantic class. Otherwise the model output will be a<br>dict and will not be validated.<br>See `langchain_core.utils.function_calling.convert_to_openai_tool` for<br>more on how to properly specify types and descriptions of schema fields<br>when specifying a Pydantic or `TypedDict` class.<br>**TYPE:**`Dict | type` |
| `include_raw` | If `False` then only the parsed structured output is returned. If<br>an error occurs during model output parsing it will be raised. If `True`<br>then both the raw model response (a `BaseMessage`) and the parsed model<br>response will be returned. If an error occurs during output parsing it<br>will be caught and returned as well.<br>The final output is always a `dict` with keys `'raw'`, `'parsed'`, and<br>`'parsing_error'`.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If there are any unsupported `kwargs`. |
| `NotImplementedError` | If the model does not implement<br>`with_structured_output()`. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[LanguageModelInput, Dict | BaseModel]` | A `Runnable` that takes same inputs as a<br>`langchain_core.language_models.chat.BaseChatModel`. If `include_raw` is<br>`False` and `schema` is a Pydantic class, `Runnable` outputs an instance<br>of `schema` (i.e., a Pydantic object). Otherwise, if `include_raw` is<br>`False` then `Runnable` outputs a `dict`.<br>If `include_raw` is `True`, then `Runnable` outputs a `dict` with keys:<br>- `'raw'`: `BaseMessage`<br>- `'parsed'`: `None` if there was a parsing error, otherwise the type<br>depends on the `schema` as described above.<br>- `'parsing_error'`: `BaseException | None` |

Example: Pydantic schema (`include_raw=False`):

```
from pydantic import BaseModel

class AnswerWithJustification(BaseModel):
    '''An answer to the user question along with justification for the answer.'''

    answer: str
    justification: str

model = ChatModel(model="model-name", temperature=0)
structured_model = model.with_structured_output(AnswerWithJustification)

structured_model.invoke(
    "What weighs more a pound of bricks or a pound of feathers"
)

# -> AnswerWithJustification(
#     answer='They weigh the same',
#     justification='Both a pound of bricks and a pound of feathers weigh one pound. The weight is the same, but the volume or density of the objects may differ.'
# )
```

Example: Pydantic schema (`include_raw=True`):

```
from pydantic import BaseModel

class AnswerWithJustification(BaseModel):
    '''An answer to the user question along with justification for the answer.'''

    answer: str
    justification: str

model = ChatModel(model="model-name", temperature=0)
structured_model = model.with_structured_output(
    AnswerWithJustification, include_raw=True
)

structured_model.invoke(
    "What weighs more a pound of bricks or a pound of feathers"
)
# -> {
#     'raw': AIMessage(content='', additional_kwargs={'tool_calls': [{'id': 'call_Ao02pnFYXD6GN1yzc0uXPsvF', 'function': {'arguments': '{"answer":"They weigh the same.","justification":"Both a pound of bricks and a pound of feathers weigh one pound. The weight is the same, but the volume or density of the objects may differ."}', 'name': 'AnswerWithJustification'}, 'type': 'function'}]}),
#     'parsed': AnswerWithJustification(answer='They weigh the same.', justification='Both a pound of bricks and a pound of feathers weigh one pound. The weight is the same, but the volume or density of the objects may differ.'),
#     'parsing_error': None
# }
```

Example: `dict` schema (`include_raw=False`):

```
from pydantic import BaseModel
from langchain_core.utils.function_calling import convert_to_openai_tool

class AnswerWithJustification(BaseModel):
    '''An answer to the user question along with justification for the answer.'''

    answer: str
    justification: str

dict_schema = convert_to_openai_tool(AnswerWithJustification)
model = ChatModel(model="model-name", temperature=0)
structured_model = model.with_structured_output(dict_schema)

structured_model.invoke(
    "What weighs more a pound of bricks or a pound of feathers"
)
# -> {
#     'answer': 'They weigh the same',
#     'justification': 'Both a pound of bricks and a pound of feathers weigh one pound. The weight is the same, but the volume and density of the two substances differ.'
# }
```

Behavior changed in 0.2.26

Added support for TypedDict class.

#### ``get\_token\_ids ¶

```
get_token_ids(text: str) -> list[int]
```

Return the ordered IDs of the tokens in a text.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `text` | The string input to tokenize.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[int]` | A list of IDs corresponding to the tokens in the text, in order they occur<br>in the text. |

#### ``get\_num\_tokens ¶

```
get_num_tokens(text: str) -> int
```

Get the number of tokens present in the text.

Useful for checking if an input fits in a model's context window.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `text` | The string input to tokenize.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `int` | The integer number of tokens in the text. |

#### ``get\_num\_tokens\_from\_messages ¶

```
get_num_tokens_from_messages(
    messages: list[BaseMessage], tools: Sequence | None = None
) -> int
```

Get the number of tokens in the messages.

Useful for checking if an input fits in a model's context window.

Note

The base implementation of `get_num_tokens_from_messages` ignores tool
schemas.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `messages` | The message inputs to tokenize.<br>**TYPE:**`list[BaseMessage]` |
| `tools` | If provided, sequence of dict, `BaseModel`, function, or<br>`BaseTool` objects to be converted to tool schemas.<br>**TYPE:**`Sequence | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `int` | The sum of the number of tokens across the messages. |

#### ``generate ¶

```
generate(
    messages: list[list[BaseMessage]],
    stop: list[str] | None = None,
    callbacks: Callbacks = None,
    *,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    run_name: str | None = None,
    run_id: UUID | None = None,
    **kwargs: Any,
) -> LLMResult
```

Pass a sequence of prompts to the model and return model generations.

This method should make use of batched calls for models that expose a batched
API.

Use this method when you want to:

1. Take advantage of batched calls,
2. Need more output from the model than just the top generated value,
3. Are building chains that are agnostic to the underlying language model
    type (e.g., pure text completion models vs chat models).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `messages` | List of list of messages.<br>**TYPE:**`list[list[BaseMessage]]` |
| `stop` | Stop words to use when generating. Model output is cut off at the<br>first occurrence of any of these substrings.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `callbacks` | `Callbacks` to pass through. Used for executing additional<br>functionality, such as logging or streaming, throughout generation.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| `tags` | The tags to apply.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `metadata` | The metadata to apply.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `run_name` | The name of the run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `run_id` | The ID of the run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Arbitrary additional keyword arguments. These are usually passed<br>to the model provider API call.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LLMResult` | An `LLMResult`, which contains a list of candidate `Generations` for each<br>input prompt and additional model provider-specific output. |

#### ``agenerate`async`¶

```
agenerate(
    messages: list[list[BaseMessage]],
    stop: list[str] | None = None,
    callbacks: Callbacks = None,
    *,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    run_name: str | None = None,
    run_id: UUID | None = None,
    **kwargs: Any,
) -> LLMResult
```

Asynchronously pass a sequence of prompts to a model and return generations.

This method should make use of batched calls for models that expose a batched
API.

Use this method when you want to:

1. Take advantage of batched calls,
2. Need more output from the model than just the top generated value,
3. Are building chains that are agnostic to the underlying language model
    type (e.g., pure text completion models vs chat models).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `messages` | List of list of messages.<br>**TYPE:**`list[list[BaseMessage]]` |
| `stop` | Stop words to use when generating. Model output is cut off at the<br>first occurrence of any of these substrings.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `callbacks` | `Callbacks` to pass through. Used for executing additional<br>functionality, such as logging or streaming, throughout generation.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| `tags` | The tags to apply.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `metadata` | The metadata to apply.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `run_name` | The name of the run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `run_id` | The ID of the run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Arbitrary additional keyword arguments. These are usually passed<br>to the model provider API call.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LLMResult` | An `LLMResult`, which contains a list of candidate `Generations` for each<br>input prompt and additional model provider-specific output. |

#### ``dict ¶

```
dict(**kwargs: Any) -> dict
```

Return a dictionary of the LLM.

#### ``bind\_tools ¶

```
bind_tools(
    tools: Sequence[Dict[str, Any] | type | Callable | BaseTool],
    *,
    tool_choice: str | None = None,
    **kwargs: Any,
) -> Runnable[LanguageModelInput, AIMessage]
```

Bind tools to the model.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `tools` | Sequence of tools to bind to the model.<br>**TYPE:**`Sequence[Dict[str, Any] | type | Callable | BaseTool]` |
| `tool_choice` | The tool to use. If "any" then any tool can be used.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[LanguageModelInput, AIMessage]` | A Runnable that returns a message. |

### ``ParrotFakeChatModel ¶

Bases: `BaseChatModel`

Generic fake chat model that can be used to test the chat model interface.

- Chat model should be usable in both sync and async tests

| METHOD | DESCRIPTION |
| --- | --- |
| `get_name` | Get the name of the `Runnable`. |
| `get_input_schema` | Get a Pydantic model that can be used to validate input to the `Runnable`. |
| `get_input_jsonschema` | Get a JSON schema that represents the input to the `Runnable`. |
| `get_output_schema` | Get a Pydantic model that can be used to validate output to the `Runnable`. |
| `get_output_jsonschema` | Get a JSON schema that represents the output of the `Runnable`. |
| `config_schema` | The type of config this `Runnable` accepts specified as a Pydantic model. |
| `get_config_jsonschema` | Get a JSON schema that represents the config of the `Runnable`. |
| `get_graph` | Return a graph representation of this `Runnable`. |
| `get_prompts` | Return a list of prompts used by this `Runnable`. |
| `__or__` | Runnable "or" operator. |
| `__ror__` | Runnable "reverse-or" operator. |
| `pipe` | Pipe `Runnable` objects. |
| `pick` | Pick keys from the output `dict` of this `Runnable`. |
| `assign` | Assigns new fields to the `dict` output of this `Runnable`. |
| `invoke` | Transform a single input into an output. |
| `ainvoke` | Transform a single input into an output. |
| `batch` | Default implementation runs invoke in parallel using a thread pool executor. |
| `batch_as_completed` | Run `invoke` in parallel on a list of inputs. |
| `abatch` | Default implementation runs `ainvoke` in parallel using `asyncio.gather`. |
| `abatch_as_completed` | Run `ainvoke` in parallel on a list of inputs. |
| `stream` | Default implementation of `stream`, which calls `invoke`. |
| `astream` | Default implementation of `astream`, which calls `ainvoke`. |
| `astream_log` | Stream all output from a `Runnable`, as reported to the callback system. |
| `astream_events` | Generate a stream of events. |
| `transform` | Transform inputs to outputs. |
| `atransform` | Transform inputs to outputs. |
| `bind` | Bind arguments to a `Runnable`, returning a new `Runnable`. |
| `with_config` | Bind config to a `Runnable`, returning a new `Runnable`. |
| `with_listeners` | Bind lifecycle listeners to a `Runnable`, returning a new `Runnable`. |
| `with_alisteners` | Bind async lifecycle listeners to a `Runnable`. |
| `with_types` | Bind input and output types to a `Runnable`, returning a new `Runnable`. |
| `with_retry` | Create a new `Runnable` that retries the original `Runnable` on exceptions. |
| `map` | Return a new `Runnable` that maps a list of inputs to a list of outputs. |
| `with_fallbacks` | Add fallbacks to a `Runnable`, returning a new `Runnable`. |
| `as_tool` | Create a `BaseTool` from a `Runnable`. |
| `__init__` |  |
| `is_lc_serializable` | Is this class serializable? |
| `get_lc_namespace` | Get the namespace of the LangChain object. |
| `lc_id` | Return a unique identifier for this class for serialization purposes. |
| `to_json` | Serialize the `Runnable` to JSON. |
| `to_json_not_implemented` | Serialize a "not implemented" object. |
| `configurable_fields` | Configure particular `Runnable` fields at runtime. |
| `configurable_alternatives` | Configure alternatives for `Runnable` objects that can be set at runtime. |
| `set_verbose` | If verbose is `None`, set it. |
| `generate_prompt` | Pass a sequence of prompts to the model and return model generations. |
| `agenerate_prompt` | Asynchronously pass a sequence of prompts and return model generations. |
| `with_structured_output` | Model wrapper that returns outputs formatted to match the given schema. |
| `get_token_ids` | Return the ordered IDs of the tokens in a text. |
| `get_num_tokens` | Get the number of tokens present in the text. |
| `get_num_tokens_from_messages` | Get the number of tokens in the messages. |
| `generate` | Pass a sequence of prompts to the model and return model generations. |
| `agenerate` | Asynchronously pass a sequence of prompts to a model and return generations. |
| `dict` | Return a dictionary of the LLM. |
| `bind_tools` | Bind tools to the model. |

#### ``name`class-attribute``instance-attribute`¶

```
name: str | None = None
```

The name of the `Runnable`. Used for debugging and tracing.

#### ``InputType`property`¶

```
InputType: TypeAlias
```

Get the input type for this `Runnable`.

#### ``OutputType`property`¶

```
OutputType: Any
```

Get the output type for this `Runnable`.

#### ``input\_schema`property`¶

```
input_schema: type[BaseModel]
```

The type of input this `Runnable` accepts specified as a Pydantic model.

#### ``output\_schema`property`¶

```
output_schema: type[BaseModel]
```

Output schema.

The type of output this `Runnable` produces specified as a Pydantic model.

#### ``config\_specs`property`¶

```
config_specs: list[ConfigurableFieldSpec]
```

List configurable fields for this `Runnable`.

#### ``lc\_secrets`property`¶

```
lc_secrets: dict[str, str]
```

A map of constructor argument names to secret ids.

For example, `{"openai_api_key": "OPENAI_API_KEY"}`

#### ``lc\_attributes`property`¶

```
lc_attributes: dict
```

List of attribute names that should be included in the serialized kwargs.

These attributes must be accepted by the constructor.

Default is an empty dictionary.

#### ``cache`class-attribute``instance-attribute`¶

```
cache: BaseCache | bool | None = Field(default=None, exclude=True)
```

Whether to cache the response.

- If `True`, will use the global cache.
- If `False`, will not use a cache
- If `None`, will use the global cache if it's set, otherwise no cache.
- If instance of `BaseCache`, will use the provided cache.

Caching is not currently supported for streaming methods of models.

#### ``verbose`class-attribute``instance-attribute`¶

```
verbose: bool = Field(default_factory=_get_verbosity, exclude=True, repr=False)
```

Whether to print out response text.

#### ``callbacks`class-attribute``instance-attribute`¶

```
callbacks: Callbacks = Field(default=None, exclude=True)
```

Callbacks to add to the run trace.

#### ``tags`class-attribute``instance-attribute`¶

```
tags: list[str] | None = Field(default=None, exclude=True)
```

Tags to add to the run trace.

#### ``metadata`class-attribute``instance-attribute`¶

```
metadata: dict[str, Any] | None = Field(default=None, exclude=True)
```

Metadata to add to the run trace.

#### ``custom\_get\_token\_ids`class-attribute``instance-attribute`¶

```
custom_get_token_ids: Callable[[str], list[int]] | None = Field(
    default=None, exclude=True
)
```

Optional encoder to use for counting tokens.

#### ``rate\_limiter`class-attribute``instance-attribute`¶

```
rate_limiter: BaseRateLimiter | None = Field(default=None, exclude=True)
```

An optional rate limiter to use for limiting the number of requests.

#### ``disable\_streaming`class-attribute``instance-attribute`¶

```
disable_streaming: bool | Literal['tool_calling'] = False
```

Whether to disable streaming for this model.

If streaming is bypassed, then `stream`/`astream`/`astream_events` will
defer to `invoke`/`ainvoke`.

- If `True`, will always bypass streaming case.
- If `'tool_calling'`, will bypass streaming case only when the model is called
with a `tools` keyword argument. In other words, LangChain will automatically
switch to non-streaming behavior (`invoke`) only when the tools argument is
provided. This offers the best of both worlds.
- If `False` (Default), will always use streaming case if available.

The main reason for this flag is that code might be written using `stream` and
a user may want to swap out a given model for another model whose the implementation
does not properly support streaming.

#### ``output\_version`class-attribute``instance-attribute`¶

```
output_version: str | None = Field(
    default_factory=from_env("LC_OUTPUT_VERSION", default=None)
)
```

Version of `AIMessage` output format to store in message content.

`AIMessage.content_blocks` will lazily parse the contents of `content` into a
standard format. This flag can be used to additionally store the standard format
in message content, e.g., for serialization purposes.

Supported values:

- `'v0'`: provider-specific format in content (can lazily-parse with
`content_blocks`)
- `'v1'`: standardized format in content (consistent with `content_blocks`)

Partner packages (e.g.,
`langchain-openai`) can also use this
field to roll out new content formats in a backward-compatible way.

Added in version 1.0

#### ``profile`property`¶

```
profile: ModelProfile
```

Return profiling information for the model.

This property relies on the `langchain-model-profiles` package to retrieve chat
model capabilities, such as context window sizes and supported features.

| RAISES | DESCRIPTION |
| --- | --- |
| `ImportError` | If `langchain-model-profiles` is not installed. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelProfile` | A `ModelProfile` object containing profiling information for the model. |

#### ``get\_name ¶

```
get_name(suffix: str | None = None, *, name: str | None = None) -> str
```

Get the name of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `suffix` | An optional suffix to append to the name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `name` | An optional name to use instead of the `Runnable`'s name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | The name of the `Runnable`. |

#### ``get\_input\_schema ¶

```
get_input_schema(config: RunnableConfig | None = None) -> type[BaseModel]
```

Get a Pydantic model that can be used to validate input to the `Runnable`.

`Runnable` objects that leverage the `configurable_fields` and
`configurable_alternatives` methods will have a dynamic input schema that
depends on which configuration the `Runnable` is invoked with.

This method allows to get an input schema for a specific configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate input. |

#### ``get\_input\_jsonschema ¶

```
get_input_jsonschema(config: RunnableConfig | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the input to the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the input to the `Runnable`. |

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

runnable = RunnableLambda(add_one)

print(runnable.get_input_jsonschema())
```

Added in version 0.3.0

#### ``get\_output\_schema ¶

```
get_output_schema(config: RunnableConfig | None = None) -> type[BaseModel]
```

Get a Pydantic model that can be used to validate output to the `Runnable`.

`Runnable` objects that leverage the `configurable_fields` and
`configurable_alternatives` methods will have a dynamic output schema that
depends on which configuration the `Runnable` is invoked with.

This method allows to get an output schema for a specific configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate output. |

#### ``get\_output\_jsonschema ¶

```
get_output_jsonschema(config: RunnableConfig | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the output of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the output of the `Runnable`. |

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

runnable = RunnableLambda(add_one)

print(runnable.get_output_jsonschema())
```

Added in version 0.3.0

#### ``config\_schema ¶

```
config_schema(*, include: Sequence[str] | None = None) -> type[BaseModel]
```

The type of config this `Runnable` accepts specified as a Pydantic model.

To mark a field as configurable, see the `configurable_fields`
and `configurable_alternatives` methods.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `include` | A list of fields to include in the config schema.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate config. |

#### ``get\_config\_jsonschema ¶

```
get_config_jsonschema(*, include: Sequence[str] | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the config of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `include` | A list of fields to include in the config schema.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the config of the `Runnable`. |

Added in version 0.3.0

#### ``get\_graph ¶

```
get_graph(config: RunnableConfig | None = None) -> Graph
```

Return a graph representation of this `Runnable`.

#### ``get\_prompts ¶

```
get_prompts(config: RunnableConfig | None = None) -> list[BasePromptTemplate]
```

Return a list of prompts used by this `Runnable`.

#### ``\_\_or\_\_ ¶

```
__or__(
    other: Runnable[Any, Other]
    | Callable[[Iterator[Any]], Iterator[Other]]
    | Callable[[AsyncIterator[Any]], AsyncIterator[Other]]
    | Callable[[Any], Other]
    | Mapping[str, Runnable[Any, Other] | Callable[[Any], Other] | Any],
) -> RunnableSerializable[Input, Other]
```

Runnable "or" operator.

Compose this `Runnable` with another object to create a
`RunnableSequence`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `other` | Another `Runnable` or a `Runnable`-like object.<br>**TYPE:**`Runnable[Any, Other] | Callable[[Iterator[Any]], Iterator[Other]] | Callable[[AsyncIterator[Any]], AsyncIterator[Other]] | Callable[[Any], Other] | Mapping[str, Runnable[Any, Other] | Callable[[Any], Other] | Any]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Other]` | A new `Runnable`. |

#### ``\_\_ror\_\_ ¶

```
__ror__(
    other: Runnable[Other, Any]
    | Callable[[Iterator[Other]], Iterator[Any]]
    | Callable[[AsyncIterator[Other]], AsyncIterator[Any]]
    | Callable[[Other], Any]
    | Mapping[str, Runnable[Other, Any] | Callable[[Other], Any] | Any],
) -> RunnableSerializable[Other, Output]
```

Runnable "reverse-or" operator.

Compose this `Runnable` with another object to create a
`RunnableSequence`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `other` | Another `Runnable` or a `Runnable`-like object.<br>**TYPE:**`Runnable[Other, Any] | Callable[[Iterator[Other]], Iterator[Any]] | Callable[[AsyncIterator[Other]], AsyncIterator[Any]] | Callable[[Other], Any] | Mapping[str, Runnable[Other, Any] | Callable[[Other], Any] | Any]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Other, Output]` | A new `Runnable`. |

#### ``pipe ¶

```
pipe(
    *others: Runnable[Any, Other] | Callable[[Any], Other], name: str | None = None
) -> RunnableSerializable[Input, Other]
```

Pipe `Runnable` objects.

Compose this `Runnable` with `Runnable`-like objects to make a
`RunnableSequence`.

Equivalent to `RunnableSequence(self, *others)` or `self | others[0] | ...`

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

def mul_two(x: int) -> int:
    return x * 2

runnable_1 = RunnableLambda(add_one)
runnable_2 = RunnableLambda(mul_two)
sequence = runnable_1.pipe(runnable_2)
# Or equivalently:
# sequence = runnable_1 | runnable_2
# sequence = RunnableSequence(first=runnable_1, last=runnable_2)
sequence.invoke(1)
await sequence.ainvoke(1)
# -> 4

sequence.batch([1, 2, 3])
await sequence.abatch([1, 2, 3])
# -> [4, 6, 8]
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `*others` | Other `Runnable` or `Runnable`-like objects to compose<br>**TYPE:**`Runnable[Any, Other] | Callable[[Any], Other]`**DEFAULT:**`()` |
| `name` | An optional name for the resulting `RunnableSequence`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Other]` | A new `Runnable`. |

#### ``pick ¶

```
pick(keys: str | list[str]) -> RunnableSerializable[Any, Any]
```

Pick keys from the output `dict` of this `Runnable`.

Pick a single key:

```
import json

from langchain_core.runnables import RunnableLambda, RunnableMap

as_str = RunnableLambda(str)
as_json = RunnableLambda(json.loads)
chain = RunnableMap(str=as_str, json=as_json)

chain.invoke("[1, 2, 3]")
# -> {"str": "[1, 2, 3]", "json": [1, 2, 3]}

json_only_chain = chain.pick("json")
json_only_chain.invoke("[1, 2, 3]")
# -> [1, 2, 3]
```

Pick a list of keys:

```
from typing import Any

import json

from langchain_core.runnables import RunnableLambda, RunnableMap

as_str = RunnableLambda(str)
as_json = RunnableLambda(json.loads)

def as_bytes(x: Any) -> bytes:
    return bytes(x, "utf-8")

chain = RunnableMap(str=as_str, json=as_json, bytes=RunnableLambda(as_bytes))

chain.invoke("[1, 2, 3]")
# -> {"str": "[1, 2, 3]", "json": [1, 2, 3], "bytes": b"[1, 2, 3]"}

json_and_bytes_chain = chain.pick(["json", "bytes"])
json_and_bytes_chain.invoke("[1, 2, 3]")
# -> {"json": [1, 2, 3], "bytes": b"[1, 2, 3]"}
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `keys` | A key or list of keys to pick from the output dict.<br>**TYPE:**`str | list[str]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Any, Any]` | a new `Runnable`. |

#### ``assign ¶

```
assign(
    **kwargs: Runnable[dict[str, Any], Any]
    | Callable[[dict[str, Any]], Any]
    | Mapping[str, Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any]],
) -> RunnableSerializable[Any, Any]
```

Assigns new fields to the `dict` output of this `Runnable`.

```
from langchain_core.language_models.fake import FakeStreamingListLLM
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import SystemMessagePromptTemplate
from langchain_core.runnables import Runnable
from operator import itemgetter

prompt = (
    SystemMessagePromptTemplate.from_template("You are a nice assistant.")
    + "{question}"
)
model = FakeStreamingListLLM(responses=["foo-lish"])

chain: Runnable = prompt | model | {"str": StrOutputParser()}

chain_with_assign = chain.assign(hello=itemgetter("str") | model)

print(chain_with_assign.input_schema.model_json_schema())
# {'title': 'PromptInput', 'type': 'object', 'properties':
{'question': {'title': 'Question', 'type': 'string'}}}
print(chain_with_assign.output_schema.model_json_schema())
# {'title': 'RunnableSequenceOutput', 'type': 'object', 'properties':
{'str': {'title': 'Str',
'type': 'string'}, 'hello': {'title': 'Hello', 'type': 'string'}}}
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**kwargs` | A mapping of keys to `Runnable` or `Runnable`-like objects<br>that will be invoked with the entire output dict of this `Runnable`.<br>**TYPE:**`Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any] | Mapping[str, Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any]]`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Any, Any]` | A new `Runnable`. |

#### ``invoke ¶

```
invoke(
    input: LanguageModelInput,
    config: RunnableConfig | None = None,
    *,
    stop: list[str] | None = None,
    **kwargs: Any,
) -> AIMessage
```

Transform a single input into an output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``ainvoke`async`¶

```
ainvoke(
    input: LanguageModelInput,
    config: RunnableConfig | None = None,
    *,
    stop: list[str] | None = None,
    **kwargs: Any,
) -> AIMessage
```

Transform a single input into an output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``batch ¶

```
batch(
    inputs: list[Input],
    config: RunnableConfig | list[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> list[Output]
```

Default implementation runs invoke in parallel using a thread pool executor.

The default implementation of batch works well for IO bound runnables.

Subclasses must override this method if they can batch more efficiently;
e.g., if the underlying `Runnable` uses an API which supports a batch mode.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`list[Input]` |
| `config` | A config to use when invoking the `Runnable`. The config supports<br>standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work<br>to do in parallel, and other keys. Please refer to the<br>`RunnableConfig` for more details.<br>**TYPE:**`RunnableConfig | list[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Output]` | A list of outputs from the `Runnable`. |

#### ``batch\_as\_completed ¶

```
batch_as_completed(
    inputs: Sequence[Input],
    config: RunnableConfig | Sequence[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> Iterator[tuple[int, Output | Exception]]
```

Run `invoke` in parallel on a list of inputs.

Yields results as they complete.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`Sequence[Input]` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | Sequence[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `tuple[int, Output | Exception]` | Tuples of the index of the input and the output from the `Runnable`. |

#### ``abatch`async`¶

```
abatch(
    inputs: list[Input],
    config: RunnableConfig | list[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> list[Output]
```

Default implementation runs `ainvoke` in parallel using `asyncio.gather`.

The default implementation of `batch` works well for IO bound runnables.

Subclasses must override this method if they can batch more efficiently;
e.g., if the underlying `Runnable` uses an API which supports a batch mode.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`list[Input]` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | list[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Output]` | A list of outputs from the `Runnable`. |

#### ``abatch\_as\_completed`async`¶

```
abatch_as_completed(
    inputs: Sequence[Input],
    config: RunnableConfig | Sequence[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> AsyncIterator[tuple[int, Output | Exception]]
```

Run `ainvoke` in parallel on a list of inputs.

Yields results as they complete.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`Sequence[Input]` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | Sequence[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[tuple[int, Output | Exception]]` | A tuple of the index of the input and the output from the `Runnable`. |

#### ``stream ¶

```
stream(
    input: LanguageModelInput,
    config: RunnableConfig | None = None,
    *,
    stop: list[str] | None = None,
    **kwargs: Any,
) -> Iterator[AIMessageChunk]
```

Default implementation of `stream`, which calls `invoke`.

Subclasses must override this method if they support streaming output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``astream`async`¶

```
astream(
    input: LanguageModelInput,
    config: RunnableConfig | None = None,
    *,
    stop: list[str] | None = None,
    **kwargs: Any,
) -> AsyncIterator[AIMessageChunk]
```

Default implementation of `astream`, which calls `ainvoke`.

Subclasses must override this method if they support streaming output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[Output]` | The output of the `Runnable`. |

#### ``astream\_log`async`¶

```
astream_log(
    input: Any,
    config: RunnableConfig | None = None,
    *,
    diff: bool = True,
    with_streamed_output_list: bool = True,
    include_names: Sequence[str] | None = None,
    include_types: Sequence[str] | None = None,
    include_tags: Sequence[str] | None = None,
    exclude_names: Sequence[str] | None = None,
    exclude_types: Sequence[str] | None = None,
    exclude_tags: Sequence[str] | None = None,
    **kwargs: Any,
) -> AsyncIterator[RunLogPatch] | AsyncIterator[RunLog]
```

Stream all output from a `Runnable`, as reported to the callback system.

This includes all inner runs of LLMs, Retrievers, Tools, etc.

Output is streamed as Log objects, which include a list of
Jsonpatch ops that describe how the state of the run has changed in each
step, and the final state of the run.

The Jsonpatch ops can be applied in order to construct state.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Any` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `diff` | Whether to yield diffs between each step or the current state.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `with_streamed_output_list` | Whether to yield the `streamed_output` list.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `include_names` | Only include logs with these names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_types` | Only include logs with these types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_tags` | Only include logs with these tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_names` | Exclude logs with these names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_types` | Exclude logs with these types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_tags` | Exclude logs with these tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[RunLogPatch] | AsyncIterator[RunLog]` | A `RunLogPatch` or `RunLog` object. |

#### ``astream\_events`async`¶

```
astream_events(
    input: Any,
    config: RunnableConfig | None = None,
    *,
    version: Literal["v1", "v2"] = "v2",
    include_names: Sequence[str] | None = None,
    include_types: Sequence[str] | None = None,
    include_tags: Sequence[str] | None = None,
    exclude_names: Sequence[str] | None = None,
    exclude_types: Sequence[str] | None = None,
    exclude_tags: Sequence[str] | None = None,
    **kwargs: Any,
) -> AsyncIterator[StreamEvent]
```

Generate a stream of events.

Use to create an iterator over `StreamEvent` that provide real-time information
about the progress of the `Runnable`, including `StreamEvent` from intermediate
results.

A `StreamEvent` is a dictionary with the following schema:

- `event`: Event names are of the format:
`on_[runnable_type]_(start|stream|end)`.
- `name`: The name of the `Runnable` that generated the event.
- `run_id`: Randomly generated ID associated with the given execution of the
`Runnable` that emitted the event. A child `Runnable` that gets invoked as
part of the execution of a parent `Runnable` is assigned its own unique ID.
- `parent_ids`: The IDs of the parent runnables that generated the event. The
root `Runnable` will have an empty list. The order of the parent IDs is from
the root to the immediate parent. Only available for v2 version of the API.
The v1 version of the API will return an empty list.
- `tags`: The tags of the `Runnable` that generated the event.
- `metadata`: The metadata of the `Runnable` that generated the event.
- `data`: The data associated with the event. The contents of this field
depend on the type of event. See the table below for more details.

Below is a table that illustrates some events that might be emitted by various
chains. Metadata fields have been omitted from the table for brevity.
Chain definitions have been included after the table.

Note

This reference table is for the v2 version of the schema.

| event | name | chunk | input | output |
| --- | --- | --- | --- | --- |
| `on_chat_model_start` | `'[model name]'` |  | `{"messages": [[SystemMessage, HumanMessage]]}` |  |
| `on_chat_model_stream` | `'[model name]'` | `AIMessageChunk(content="hello")` |  |  |
| `on_chat_model_end` | `'[model name]'` |  | `{"messages": [[SystemMessage, HumanMessage]]}` | `AIMessageChunk(content="hello world")` |
| `on_llm_start` | `'[model name]'` |  | `{'input': 'hello'}` |  |
| `on_llm_stream` | `'[model name]'` | `'Hello'` |  |  |
| `on_llm_end` | `'[model name]'` |  | `'Hello human!'` |  |
| `on_chain_start` | `'format_docs'` |  |  |  |
| `on_chain_stream` | `'format_docs'` | `'hello world!, goodbye world!'` |  |  |
| `on_chain_end` | `'format_docs'` |  | `[Document(...)]` | `'hello world!, goodbye world!'` |
| `on_tool_start` | `'some_tool'` |  | `{"x": 1, "y": "2"}` |  |
| `on_tool_end` | `'some_tool'` |  |  | `{"x": 1, "y": "2"}` |
| `on_retriever_start` | `'[retriever name]'` |  | `{"query": "hello"}` |  |
| `on_retriever_end` | `'[retriever name]'` |  | `{"query": "hello"}` | `[Document(...), ..]` |
| `on_prompt_start` | `'[template_name]'` |  | `{"question": "hello"}` |  |
| `on_prompt_end` | `'[template_name]'` |  | `{"question": "hello"}` | `ChatPromptValue(messages: [SystemMessage, ...])` |

In addition to the standard events, users can also dispatch custom events (see example below).

Custom events will be only be surfaced with in the v2 version of the API!

A custom event has following format:

| Attribute | Type | Description |
| --- | --- | --- |
| `name` | `str` | A user defined name for the event. |
| `data` | `Any` | The data associated with the event. This can be anything, though we suggest making it JSON serializable. |

Here are declarations associated with the standard events shown above:

`format_docs`:

```
def format_docs(docs: list[Document]) -> str:
    '''Format the docs.'''
    return ", ".join([doc.page_content for doc in docs])

format_docs = RunnableLambda(format_docs)
```

`some_tool`:

```
@tool
def some_tool(x: int, y: str) -> dict:
    '''Some_tool.'''
    return {"x": x, "y": y}
```

`prompt`:

```
template = ChatPromptTemplate.from_messages(
    [\
        ("system", "You are Cat Agent 007"),\
        ("human", "{question}"),\
    ]
).with_config({"run_name": "my_template", "tags": ["my_template"]})
```

For instance:

```
from langchain_core.runnables import RunnableLambda

async def reverse(s: str) -> str:
    return s[::-1]

chain = RunnableLambda(func=reverse)

events = [event async for event in chain.astream_events("hello", version="v2")]

# Will produce the following events
# (run_id, and parent_ids has been omitted for brevity):
[\
    {\
        "data": {"input": "hello"},\
        "event": "on_chain_start",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
    {\
        "data": {"chunk": "olleh"},\
        "event": "on_chain_stream",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
    {\
        "data": {"output": "olleh"},\
        "event": "on_chain_end",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
]
```

Example: Dispatch Custom Event

```
from langchain_core.callbacks.manager import (
    adispatch_custom_event,
)
from langchain_core.runnables import RunnableLambda, RunnableConfig
import asyncio

async def slow_thing(some_input: str, config: RunnableConfig) -> str:
    """Do something that takes a long time."""
    await asyncio.sleep(1) # Placeholder for some slow operation
    await adispatch_custom_event(
        "progress_event",
        {"message": "Finished step 1 of 3"},
        config=config # Must be included for python < 3.10
    )
    await asyncio.sleep(1) # Placeholder for some slow operation
    await adispatch_custom_event(
        "progress_event",
        {"message": "Finished step 2 of 3"},
        config=config # Must be included for python < 3.10
    )
    await asyncio.sleep(1) # Placeholder for some slow operation
    return "Done"

slow_thing = RunnableLambda(slow_thing)

async for event in slow_thing.astream_events("some_input", version="v2"):
    print(event)
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Any` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `version` | The version of the schema to use either `'v2'` or `'v1'`.<br>Users should use `'v2'`.<br>`'v1'` is for backwards compatibility and will be deprecated<br>in `0.4.0`.<br>No default will be assigned until the API is stabilized.<br>custom events will only be surfaced in `'v2'`.<br>**TYPE:**`Literal['v1', 'v2']`**DEFAULT:**`'v2'` |
| `include_names` | Only include events from `Runnable` objects with matching names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_types` | Only include events from `Runnable` objects with matching types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_tags` | Only include events from `Runnable` objects with matching tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_names` | Exclude events from `Runnable` objects with matching names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_types` | Exclude events from `Runnable` objects with matching types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_tags` | Exclude events from `Runnable` objects with matching tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>These will be passed to `astream_log` as this implementation<br>of `astream_events` is built on top of `astream_log`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[StreamEvent]` | An async stream of `StreamEvent`. |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | If the version is not `'v1'` or `'v2'`. |

#### ``transform ¶

```
transform(
    input: Iterator[Input], config: RunnableConfig | None = None, **kwargs: Any | None
) -> Iterator[Output]
```

Transform inputs to outputs.

Default implementation of transform, which buffers input and calls `astream`.

Subclasses must override this method if they can start producing output while
input is still being generated.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | An iterator of inputs to the `Runnable`.<br>**TYPE:**`Iterator[Input]` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``atransform`async`¶

```
atransform(
    input: AsyncIterator[Input],
    config: RunnableConfig | None = None,
    **kwargs: Any | None,
) -> AsyncIterator[Output]
```

Transform inputs to outputs.

Default implementation of atransform, which buffers input and calls `astream`.

Subclasses must override this method if they can start producing output while
input is still being generated.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | An async iterator of inputs to the `Runnable`.<br>**TYPE:**`AsyncIterator[Input]` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[Output]` | The output of the `Runnable`. |

#### ``bind ¶

```
bind(**kwargs: Any) -> Runnable[Input, Output]
```

Bind arguments to a `Runnable`, returning a new `Runnable`.

Useful when a `Runnable` in a chain requires an argument that is not
in the output of the previous `Runnable` or included in the user input.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**kwargs` | The arguments to bind to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the arguments bound. |

Example

```
from langchain_ollama import ChatOllama
from langchain_core.output_parsers import StrOutputParser

model = ChatOllama(model="llama3.1")

# Without bind
chain = model | StrOutputParser()

chain.invoke("Repeat quoted words exactly: 'One two three four five.'")
# Output is 'One two three four five.'

# With bind
chain = model.bind(stop=["three"]) | StrOutputParser()

chain.invoke("Repeat quoted words exactly: 'One two three four five.'")
# Output is 'One two'
```

#### ``with\_config ¶

```
with_config(
    config: RunnableConfig | None = None, **kwargs: Any
) -> Runnable[Input, Output]
```

Bind config to a `Runnable`, returning a new `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to bind to the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the config bound. |

#### ``with\_listeners ¶

```
with_listeners(
    *,
    on_start: Callable[[Run], None]
    | Callable[[Run, RunnableConfig], None]
    | None = None,
    on_end: Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None = None,
    on_error: Callable[[Run], None]
    | Callable[[Run, RunnableConfig], None]
    | None = None,
) -> Runnable[Input, Output]
```

Bind lifecycle listeners to a `Runnable`, returning a new `Runnable`.

The Run object contains information about the run, including its `id`,
`type`, `input`, `output`, `error`, `start_time`, `end_time`, and
any tags or metadata added to the run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `on_start` | Called before the `Runnable` starts running, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |
| `on_end` | Called after the `Runnable` finishes running, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |
| `on_error` | Called if the `Runnable` throws an error, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the listeners bound. |

Example

```
from langchain_core.runnables import RunnableLambda
from langchain_core.tracers.schemas import Run

import time

def test_runnable(time_to_sleep: int):
    time.sleep(time_to_sleep)

def fn_start(run_obj: Run):
    print("start_time:", run_obj.start_time)

def fn_end(run_obj: Run):
    print("end_time:", run_obj.end_time)

chain = RunnableLambda(test_runnable).with_listeners(
    on_start=fn_start, on_end=fn_end
)
chain.invoke(2)
```

#### ``with\_alisteners ¶

```
with_alisteners(
    *,
    on_start: AsyncListener | None = None,
    on_end: AsyncListener | None = None,
    on_error: AsyncListener | None = None,
) -> Runnable[Input, Output]
```

Bind async lifecycle listeners to a `Runnable`.

Returns a new `Runnable`.

The Run object contains information about the run, including its `id`,
`type`, `input`, `output`, `error`, `start_time`, `end_time`, and
any tags or metadata added to the run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `on_start` | Called asynchronously before the `Runnable` starts running,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |
| `on_end` | Called asynchronously after the `Runnable` finishes running,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |
| `on_error` | Called asynchronously if the `Runnable` throws an error,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the listeners bound. |

Example

```
from langchain_core.runnables import RunnableLambda, Runnable
from datetime import datetime, timezone
import time
import asyncio

def format_t(timestamp: float) -> str:
    return datetime.fromtimestamp(timestamp, tz=timezone.utc).isoformat()

async def test_runnable(time_to_sleep: int):
    print(f"Runnable[{time_to_sleep}s]: starts at {format_t(time.time())}")
    await asyncio.sleep(time_to_sleep)
    print(f"Runnable[{time_to_sleep}s]: ends at {format_t(time.time())}")

async def fn_start(run_obj: Runnable):
    print(f"on start callback starts at {format_t(time.time())}")
    await asyncio.sleep(3)
    print(f"on start callback ends at {format_t(time.time())}")

async def fn_end(run_obj: Runnable):
    print(f"on end callback starts at {format_t(time.time())}")
    await asyncio.sleep(2)
    print(f"on end callback ends at {format_t(time.time())}")

runnable = RunnableLambda(test_runnable).with_alisteners(
    on_start=fn_start,
    on_end=fn_end
)
async def concurrent_runs():
    await asyncio.gather(runnable.ainvoke(2), runnable.ainvoke(3))

asyncio.run(concurrent_runs())
Result:
on start callback starts at 2025-03-01T07:05:22.875378+00:00
on start callback starts at 2025-03-01T07:05:22.875495+00:00
on start callback ends at 2025-03-01T07:05:25.878862+00:00
on start callback ends at 2025-03-01T07:05:25.878947+00:00
Runnable[2s]: starts at 2025-03-01T07:05:25.879392+00:00
Runnable[3s]: starts at 2025-03-01T07:05:25.879804+00:00
Runnable[2s]: ends at 2025-03-01T07:05:27.881998+00:00
on end callback starts at 2025-03-01T07:05:27.882360+00:00
Runnable[3s]: ends at 2025-03-01T07:05:28.881737+00:00
on end callback starts at 2025-03-01T07:05:28.882428+00:00
on end callback ends at 2025-03-01T07:05:29.883893+00:00
on end callback ends at 2025-03-01T07:05:30.884831+00:00
```

#### ``with\_types ¶

```
with_types(
    *, input_type: type[Input] | None = None, output_type: type[Output] | None = None
) -> Runnable[Input, Output]
```

Bind input and output types to a `Runnable`, returning a new `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input_type` | The input type to bind to the `Runnable`.<br>**TYPE:**`type[Input] | None`**DEFAULT:**`None` |
| `output_type` | The output type to bind to the `Runnable`.<br>**TYPE:**`type[Output] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the types bound. |

#### ``with\_retry ¶

```
with_retry(
    *,
    retry_if_exception_type: tuple[type[BaseException], ...] = (Exception,),
    wait_exponential_jitter: bool = True,
    exponential_jitter_params: ExponentialJitterParams | None = None,
    stop_after_attempt: int = 3,
) -> Runnable[Input, Output]
```

Create a new `Runnable` that retries the original `Runnable` on exceptions.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `retry_if_exception_type` | A tuple of exception types to retry on.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| `wait_exponential_jitter` | Whether to add jitter to the wait<br>time between retries.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `stop_after_attempt` | The maximum number of attempts to make before<br>giving up.<br>**TYPE:**`int`**DEFAULT:**`3` |
| `exponential_jitter_params` | Parameters for<br>`tenacity.wait_exponential_jitter`. Namely: `initial`, `max`,<br>`exp_base`, and `jitter` (all `float` values).<br>**TYPE:**`ExponentialJitterParams | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new Runnable that retries the original Runnable on exceptions. |

Example

```
from langchain_core.runnables import RunnableLambda

count = 0

def _lambda(x: int) -> None:
    global count
    count = count + 1
    if x == 1:
        raise ValueError("x is 1")
    else:
        pass

runnable = RunnableLambda(_lambda)
try:
    runnable.with_retry(
        stop_after_attempt=2,
        retry_if_exception_type=(ValueError,),
    ).invoke(1)
except ValueError:
    pass

assert count == 2
```

#### ``map ¶

```
map() -> Runnable[list[Input], list[Output]]
```

Return a new `Runnable` that maps a list of inputs to a list of outputs.

Calls `invoke` with each input.

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[list[Input], list[Output]]` | A new `Runnable` that maps a list of inputs to a list of outputs. |

Example

```
from langchain_core.runnables import RunnableLambda

def _lambda(x: int) -> int:
    return x + 1

runnable = RunnableLambda(_lambda)
print(runnable.map().invoke([1, 2, 3]))  # [2, 3, 4]
```

#### ``with\_fallbacks ¶

```
with_fallbacks(
    fallbacks: Sequence[Runnable[Input, Output]],
    *,
    exceptions_to_handle: tuple[type[BaseException], ...] = (Exception,),
    exception_key: str | None = None,
) -> RunnableWithFallbacks[Input, Output]
```

Add fallbacks to a `Runnable`, returning a new `Runnable`.

The new `Runnable` will try the original `Runnable`, and then each fallback
in order, upon failures.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `fallbacks` | A sequence of runnables to try if the original `Runnable`<br>fails.<br>**TYPE:**`Sequence[Runnable[Input, Output]]` |
| `exceptions_to_handle` | A tuple of exception types to handle.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| `exception_key` | If `string` is specified then handled exceptions will be<br>passed to fallbacks as part of the input under the specified key.<br>If `None`, exceptions will not be passed to fallbacks.<br>If used, the base `Runnable` and its fallbacks must accept a<br>dictionary as input.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableWithFallbacks[Input, Output]` | A new `Runnable` that will try the original `Runnable`, and then each<br>Fallback in order, upon failures. |

Example

```
from typing import Iterator

from langchain_core.runnables import RunnableGenerator

def _generate_immediate_error(input: Iterator) -> Iterator[str]:
    raise ValueError()
    yield ""

def _generate(input: Iterator) -> Iterator[str]:
    yield from "foo bar"

runnable = RunnableGenerator(_generate_immediate_error).with_fallbacks(
    [RunnableGenerator(_generate)]
)
print("".join(runnable.stream({})))  # foo bar
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `fallbacks` | A sequence of runnables to try if the original `Runnable`<br>fails.<br>**TYPE:**`Sequence[Runnable[Input, Output]]` |
| `exceptions_to_handle` | A tuple of exception types to handle.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| `exception_key` | If `string` is specified then handled exceptions will be<br>passed to fallbacks as part of the input under the specified key.<br>If `None`, exceptions will not be passed to fallbacks.<br>If used, the base `Runnable` and its fallbacks must accept a<br>dictionary as input.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableWithFallbacks[Input, Output]` | A new `Runnable` that will try the original `Runnable`, and then each<br>Fallback in order, upon failures. |

#### ``as\_tool ¶

```
as_tool(
    args_schema: type[BaseModel] | None = None,
    *,
    name: str | None = None,
    description: str | None = None,
    arg_types: dict[str, type] | None = None,
) -> BaseTool
```

Create a `BaseTool` from a `Runnable`.

`as_tool` will instantiate a `BaseTool` with a name, description, and
`args_schema` from a `Runnable`. Where possible, schemas are inferred
from `runnable.get_input_schema`. Alternatively (e.g., if the
`Runnable` takes a dict as input and the specific dict keys are not typed),
the schema can be specified directly with `args_schema`. You can also
pass `arg_types` to just specify the required arguments and their types.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `args_schema` | The schema for the tool.<br>**TYPE:**`type[BaseModel] | None`**DEFAULT:**`None` |
| `name` | The name of the tool.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `description` | The description of the tool.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `arg_types` | A dictionary of argument names to types.<br>**TYPE:**`dict[str, type] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `BaseTool` | A `BaseTool` instance. |

Typed dict input:

```
from typing_extensions import TypedDict
from langchain_core.runnables import RunnableLambda

class Args(TypedDict):
    a: int
    b: list[int]

def f(x: Args) -> str:
    return str(x["a"] * max(x["b"]))

runnable = RunnableLambda(f)
as_tool = runnable.as_tool()
as_tool.invoke({"a": 3, "b": [1, 2]})
```

`dict` input, specifying schema via `args_schema`:

```
from typing import Any
from pydantic import BaseModel, Field
from langchain_core.runnables import RunnableLambda

def f(x: dict[str, Any]) -> str:
    return str(x["a"] * max(x["b"]))

class FSchema(BaseModel):
    """Apply a function to an integer and list of integers."""

    a: int = Field(..., description="Integer")
    b: list[int] = Field(..., description="List of ints")

runnable = RunnableLambda(f)
as_tool = runnable.as_tool(FSchema)
as_tool.invoke({"a": 3, "b": [1, 2]})
```

`dict` input, specifying schema via `arg_types`:

```
from typing import Any
from langchain_core.runnables import RunnableLambda

def f(x: dict[str, Any]) -> str:
    return str(x["a"] * max(x["b"]))

runnable = RunnableLambda(f)
as_tool = runnable.as_tool(arg_types={"a": int, "b": list[int]})
as_tool.invoke({"a": 3, "b": [1, 2]})
```

String input:

```
from langchain_core.runnables import RunnableLambda

def f(x: str) -> str:
    return x + "a"

def g(x: str) -> str:
    return x + "z"

runnable = RunnableLambda(f) | g
as_tool = runnable.as_tool()
as_tool.invoke("b")
```

#### ``\_\_init\_\_ ¶

```
__init__(*args: Any, **kwargs: Any) -> None
```

#### ``is\_lc\_serializable`classmethod`¶

```
is_lc_serializable() -> bool
```

Is this class serializable?

By design, even if a class inherits from `Serializable`, it is not serializable
by default. This is to prevent accidental serialization of objects that should
not be serialized.

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | Whether the class is serializable. Default is `False`. |

#### ``get\_lc\_namespace`classmethod`¶

```
get_lc_namespace() -> list[str]
```

Get the namespace of the LangChain object.

For example, if the class is `langchain.llms.openai.OpenAI`, then the
namespace is `["langchain", "llms", "openai"]`

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[str]` | The namespace. |

#### ``lc\_id`classmethod`¶

```
lc_id() -> list[str]
```

Return a unique identifier for this class for serialization purposes.

The unique identifier is a list of strings that describes the path
to the object.

For example, for the class `langchain.llms.openai.OpenAI`, the id is
`["langchain", "llms", "openai", "OpenAI"]`.

#### ``to\_json ¶

```
to_json() -> SerializedConstructor | SerializedNotImplemented
```

Serialize the `Runnable` to JSON.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedConstructor | SerializedNotImplemented` | A JSON-serializable representation of the `Runnable`. |

#### ``to\_json\_not\_implemented ¶

```
to_json_not_implemented() -> SerializedNotImplemented
```

Serialize a "not implemented" object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedNotImplemented` | `SerializedNotImplemented`. |

#### ``configurable\_fields ¶

```
configurable_fields(
    **kwargs: AnyConfigurableField,
) -> RunnableSerializable[Input, Output]
```

Configure particular `Runnable` fields at runtime.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**kwargs` | A dictionary of `ConfigurableField` instances to configure.<br>**TYPE:**`AnyConfigurableField`**DEFAULT:**`{}` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If a configuration key is not found in the `Runnable`. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Output]` | A new `Runnable` with the fields configured. |

```
from langchain_core.runnables import ConfigurableField
from langchain_openai import ChatOpenAI

model = ChatOpenAI(max_tokens=20).configurable_fields(
    max_tokens=ConfigurableField(
        id="output_token_number",
        name="Max tokens in the output",
        description="The maximum number of tokens in the output",
    )
)

# max_tokens = 20
print("max_tokens_20: ", model.invoke("tell me something about chess").content)

# max_tokens = 200
print(
    "max_tokens_200: ",
    model.with_config(configurable={"output_token_number": 200})
    .invoke("tell me something about chess")
    .content,
)
```

#### ``configurable\_alternatives ¶

```
configurable_alternatives(
    which: ConfigurableField,
    *,
    default_key: str = "default",
    prefix_keys: bool = False,
    **kwargs: Runnable[Input, Output] | Callable[[], Runnable[Input, Output]],
) -> RunnableSerializable[Input, Output]
```

Configure alternatives for `Runnable` objects that can be set at runtime.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `which` | The `ConfigurableField` instance that will be used to select the<br>alternative.<br>**TYPE:**`ConfigurableField` |
| `default_key` | The default key to use if no alternative is selected.<br>**TYPE:**`str`**DEFAULT:**`'default'` |
| `prefix_keys` | Whether to prefix the keys with the `ConfigurableField` id.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | A dictionary of keys to `Runnable` instances or callables that<br>return `Runnable` instances.<br>**TYPE:**`Runnable[Input, Output] | Callable[[], Runnable[Input, Output]]`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Output]` | A new `Runnable` with the alternatives configured. |

```
from langchain_anthropic import ChatAnthropic
from langchain_core.runnables.utils import ConfigurableField
from langchain_openai import ChatOpenAI

model = ChatAnthropic(
    model_name="claude-sonnet-4-5-20250929"
).configurable_alternatives(
    ConfigurableField(id="llm"),
    default_key="anthropic",
    openai=ChatOpenAI(),
)

# uses the default model ChatAnthropic
print(model.invoke("which organization created you?").content)

# uses ChatOpenAI
print(
    model.with_config(configurable={"llm": "openai"})
    .invoke("which organization created you?")
    .content
)
```

#### ``set\_verbose ¶

```
set_verbose(verbose: bool | None) -> bool
```

If verbose is `None`, set it.

This allows users to pass in `None` as verbose to access the global setting.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `verbose` | The verbosity setting to use.<br>**TYPE:**`bool | None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | The verbosity setting to use. |

#### ``generate\_prompt ¶

```
generate_prompt(
    prompts: list[PromptValue],
    stop: list[str] | None = None,
    callbacks: Callbacks = None,
    **kwargs: Any,
) -> LLMResult
```

Pass a sequence of prompts to the model and return model generations.

This method should make use of batched calls for models that expose a batched
API.

Use this method when you want to:

1. Take advantage of batched calls,
2. Need more output from the model than just the top generated value,
3. Are building chains that are agnostic to the underlying language model
    type (e.g., pure text completion models vs chat models).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `prompts` | List of `PromptValue` objects. A `PromptValue` is an object that<br>can be converted to match the format of any language model (string for<br>pure text generation models and `BaseMessage` objects for chat models).<br>**TYPE:**`list[PromptValue]` |
| `stop` | Stop words to use when generating. Model output is cut off at the<br>first occurrence of any of these substrings.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `callbacks` | `Callbacks` to pass through. Used for executing additional<br>functionality, such as logging or streaming, throughout generation.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| `**kwargs` | Arbitrary additional keyword arguments. These are usually passed<br>to the model provider API call.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LLMResult` | An `LLMResult`, which contains a list of candidate `Generation` objects for<br>each input prompt and additional model provider-specific output. |

#### ``agenerate\_prompt`async`¶

```
agenerate_prompt(
    prompts: list[PromptValue],
    stop: list[str] | None = None,
    callbacks: Callbacks = None,
    **kwargs: Any,
) -> LLMResult
```

Asynchronously pass a sequence of prompts and return model generations.

This method should make use of batched calls for models that expose a batched
API.

Use this method when you want to:

1. Take advantage of batched calls,
2. Need more output from the model than just the top generated value,
3. Are building chains that are agnostic to the underlying language model
    type (e.g., pure text completion models vs chat models).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `prompts` | List of `PromptValue` objects. A `PromptValue` is an object that<br>can be converted to match the format of any language model (string for<br>pure text generation models and `BaseMessage` objects for chat models).<br>**TYPE:**`list[PromptValue]` |
| `stop` | Stop words to use when generating. Model output is cut off at the<br>first occurrence of any of these substrings.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `callbacks` | `Callbacks` to pass through. Used for executing additional<br>functionality, such as logging or streaming, throughout generation.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| `**kwargs` | Arbitrary additional keyword arguments. These are usually passed<br>to the model provider API call.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LLMResult` | An `LLMResult`, which contains a list of candidate `Generation` objects for<br>each input prompt and additional model provider-specific output. |

#### ``with\_structured\_output ¶

```
with_structured_output(
    schema: Dict | type, *, include_raw: bool = False, **kwargs: Any
) -> Runnable[LanguageModelInput, Dict | BaseModel]
```

Model wrapper that returns outputs formatted to match the given schema.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `schema` | The output schema. Can be passed in as:<br>- an OpenAI function/tool schema,<br>- a JSON Schema,<br>- a `TypedDict` class,<br>- or a Pydantic class.<br>If `schema` is a Pydantic class then the model output will be a<br>Pydantic instance of that class, and the model-generated fields will be<br>validated by the Pydantic class. Otherwise the model output will be a<br>dict and will not be validated.<br>See `langchain_core.utils.function_calling.convert_to_openai_tool` for<br>more on how to properly specify types and descriptions of schema fields<br>when specifying a Pydantic or `TypedDict` class.<br>**TYPE:**`Dict | type` |
| `include_raw` | If `False` then only the parsed structured output is returned. If<br>an error occurs during model output parsing it will be raised. If `True`<br>then both the raw model response (a `BaseMessage`) and the parsed model<br>response will be returned. If an error occurs during output parsing it<br>will be caught and returned as well.<br>The final output is always a `dict` with keys `'raw'`, `'parsed'`, and<br>`'parsing_error'`.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If there are any unsupported `kwargs`. |
| `NotImplementedError` | If the model does not implement<br>`with_structured_output()`. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[LanguageModelInput, Dict | BaseModel]` | A `Runnable` that takes same inputs as a<br>`langchain_core.language_models.chat.BaseChatModel`. If `include_raw` is<br>`False` and `schema` is a Pydantic class, `Runnable` outputs an instance<br>of `schema` (i.e., a Pydantic object). Otherwise, if `include_raw` is<br>`False` then `Runnable` outputs a `dict`.<br>If `include_raw` is `True`, then `Runnable` outputs a `dict` with keys:<br>- `'raw'`: `BaseMessage`<br>- `'parsed'`: `None` if there was a parsing error, otherwise the type<br>depends on the `schema` as described above.<br>- `'parsing_error'`: `BaseException | None` |

Example: Pydantic schema (`include_raw=False`):

```
from pydantic import BaseModel

class AnswerWithJustification(BaseModel):
    '''An answer to the user question along with justification for the answer.'''

    answer: str
    justification: str

model = ChatModel(model="model-name", temperature=0)
structured_model = model.with_structured_output(AnswerWithJustification)

structured_model.invoke(
    "What weighs more a pound of bricks or a pound of feathers"
)

# -> AnswerWithJustification(
#     answer='They weigh the same',
#     justification='Both a pound of bricks and a pound of feathers weigh one pound. The weight is the same, but the volume or density of the objects may differ.'
# )
```

Example: Pydantic schema (`include_raw=True`):

```
from pydantic import BaseModel

class AnswerWithJustification(BaseModel):
    '''An answer to the user question along with justification for the answer.'''

    answer: str
    justification: str

model = ChatModel(model="model-name", temperature=0)
structured_model = model.with_structured_output(
    AnswerWithJustification, include_raw=True
)

structured_model.invoke(
    "What weighs more a pound of bricks or a pound of feathers"
)
# -> {
#     'raw': AIMessage(content='', additional_kwargs={'tool_calls': [{'id': 'call_Ao02pnFYXD6GN1yzc0uXPsvF', 'function': {'arguments': '{"answer":"They weigh the same.","justification":"Both a pound of bricks and a pound of feathers weigh one pound. The weight is the same, but the volume or density of the objects may differ."}', 'name': 'AnswerWithJustification'}, 'type': 'function'}]}),
#     'parsed': AnswerWithJustification(answer='They weigh the same.', justification='Both a pound of bricks and a pound of feathers weigh one pound. The weight is the same, but the volume or density of the objects may differ.'),
#     'parsing_error': None
# }
```

Example: `dict` schema (`include_raw=False`):

```
from pydantic import BaseModel
from langchain_core.utils.function_calling import convert_to_openai_tool

class AnswerWithJustification(BaseModel):
    '''An answer to the user question along with justification for the answer.'''

    answer: str
    justification: str

dict_schema = convert_to_openai_tool(AnswerWithJustification)
model = ChatModel(model="model-name", temperature=0)
structured_model = model.with_structured_output(dict_schema)

structured_model.invoke(
    "What weighs more a pound of bricks or a pound of feathers"
)
# -> {
#     'answer': 'They weigh the same',
#     'justification': 'Both a pound of bricks and a pound of feathers weigh one pound. The weight is the same, but the volume and density of the two substances differ.'
# }
```

Behavior changed in 0.2.26

Added support for TypedDict class.

#### ``get\_token\_ids ¶

```
get_token_ids(text: str) -> list[int]
```

Return the ordered IDs of the tokens in a text.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `text` | The string input to tokenize.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[int]` | A list of IDs corresponding to the tokens in the text, in order they occur<br>in the text. |

#### ``get\_num\_tokens ¶

```
get_num_tokens(text: str) -> int
```

Get the number of tokens present in the text.

Useful for checking if an input fits in a model's context window.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `text` | The string input to tokenize.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `int` | The integer number of tokens in the text. |

#### ``get\_num\_tokens\_from\_messages ¶

```
get_num_tokens_from_messages(
    messages: list[BaseMessage], tools: Sequence | None = None
) -> int
```

Get the number of tokens in the messages.

Useful for checking if an input fits in a model's context window.

Note

The base implementation of `get_num_tokens_from_messages` ignores tool
schemas.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `messages` | The message inputs to tokenize.<br>**TYPE:**`list[BaseMessage]` |
| `tools` | If provided, sequence of dict, `BaseModel`, function, or<br>`BaseTool` objects to be converted to tool schemas.<br>**TYPE:**`Sequence | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `int` | The sum of the number of tokens across the messages. |

#### ``generate ¶

```
generate(
    messages: list[list[BaseMessage]],
    stop: list[str] | None = None,
    callbacks: Callbacks = None,
    *,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    run_name: str | None = None,
    run_id: UUID | None = None,
    **kwargs: Any,
) -> LLMResult
```

Pass a sequence of prompts to the model and return model generations.

This method should make use of batched calls for models that expose a batched
API.

Use this method when you want to:

1. Take advantage of batched calls,
2. Need more output from the model than just the top generated value,
3. Are building chains that are agnostic to the underlying language model
    type (e.g., pure text completion models vs chat models).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `messages` | List of list of messages.<br>**TYPE:**`list[list[BaseMessage]]` |
| `stop` | Stop words to use when generating. Model output is cut off at the<br>first occurrence of any of these substrings.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `callbacks` | `Callbacks` to pass through. Used for executing additional<br>functionality, such as logging or streaming, throughout generation.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| `tags` | The tags to apply.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `metadata` | The metadata to apply.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `run_name` | The name of the run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `run_id` | The ID of the run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Arbitrary additional keyword arguments. These are usually passed<br>to the model provider API call.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LLMResult` | An `LLMResult`, which contains a list of candidate `Generations` for each<br>input prompt and additional model provider-specific output. |

#### ``agenerate`async`¶

```
agenerate(
    messages: list[list[BaseMessage]],
    stop: list[str] | None = None,
    callbacks: Callbacks = None,
    *,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    run_name: str | None = None,
    run_id: UUID | None = None,
    **kwargs: Any,
) -> LLMResult
```

Asynchronously pass a sequence of prompts to a model and return generations.

This method should make use of batched calls for models that expose a batched
API.

Use this method when you want to:

1. Take advantage of batched calls,
2. Need more output from the model than just the top generated value,
3. Are building chains that are agnostic to the underlying language model
    type (e.g., pure text completion models vs chat models).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `messages` | List of list of messages.<br>**TYPE:**`list[list[BaseMessage]]` |
| `stop` | Stop words to use when generating. Model output is cut off at the<br>first occurrence of any of these substrings.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `callbacks` | `Callbacks` to pass through. Used for executing additional<br>functionality, such as logging or streaming, throughout generation.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| `tags` | The tags to apply.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `metadata` | The metadata to apply.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `run_name` | The name of the run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `run_id` | The ID of the run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Arbitrary additional keyword arguments. These are usually passed<br>to the model provider API call.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LLMResult` | An `LLMResult`, which contains a list of candidate `Generations` for each<br>input prompt and additional model provider-specific output. |

#### ``dict ¶

```
dict(**kwargs: Any) -> dict
```

Return a dictionary of the LLM.

#### ``bind\_tools ¶

```
bind_tools(
    tools: Sequence[Dict[str, Any] | type | Callable | BaseTool],
    *,
    tool_choice: str | None = None,
    **kwargs: Any,
) -> Runnable[LanguageModelInput, AIMessage]
```

Bind tools to the model.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `tools` | Sequence of tools to bind to the model.<br>**TYPE:**`Sequence[Dict[str, Any] | type | Callable | BaseTool]` |
| `tool_choice` | The tool to use. If "any" then any tool can be used.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[LanguageModelInput, AIMessage]` | A Runnable that returns a message. |

## ``langchain\_core.language\_models.base ¶

Base language models class.

### ``LanguageModelInput`module-attribute`¶

```
LanguageModelInput = PromptValue | str | Sequence[MessageLikeRepresentation]
```

Input to a language model.

### ``LanguageModelOutput`module-attribute`¶

```
LanguageModelOutput = BaseMessage | str
```

Output from a language model.

### ``LanguageModelLike`module-attribute`¶

```
LanguageModelLike = Runnable[LanguageModelInput, LanguageModelOutput]
```

Input/output interface for a language model.

### ``BaseLanguageModel ¶

Bases: `RunnableSerializable[LanguageModelInput, LanguageModelOutputVar]`, `ABC`

Abstract base class for interfacing with language models.

All language model wrappers inherited from `BaseLanguageModel`.

| METHOD | DESCRIPTION |
| --- | --- |
| `set_verbose` | If verbose is `None`, set it. |
| `generate_prompt` | Pass a sequence of prompts to the model and return model generations. |
| `agenerate_prompt` | Asynchronously pass a sequence of prompts and return model generations. |
| `with_structured_output` | Not implemented on this class. |
| `get_token_ids` | Return the ordered IDs of the tokens in a text. |
| `get_num_tokens` | Get the number of tokens present in the text. |
| `get_num_tokens_from_messages` | Get the number of tokens in the messages. |
| `get_name` | Get the name of the `Runnable`. |
| `get_input_schema` | Get a Pydantic model that can be used to validate input to the `Runnable`. |
| `get_input_jsonschema` | Get a JSON schema that represents the input to the `Runnable`. |
| `get_output_schema` | Get a Pydantic model that can be used to validate output to the `Runnable`. |
| `get_output_jsonschema` | Get a JSON schema that represents the output of the `Runnable`. |
| `config_schema` | The type of config this `Runnable` accepts specified as a Pydantic model. |
| `get_config_jsonschema` | Get a JSON schema that represents the config of the `Runnable`. |
| `get_graph` | Return a graph representation of this `Runnable`. |
| `get_prompts` | Return a list of prompts used by this `Runnable`. |
| `__or__` | Runnable "or" operator. |
| `__ror__` | Runnable "reverse-or" operator. |
| `pipe` | Pipe `Runnable` objects. |
| `pick` | Pick keys from the output `dict` of this `Runnable`. |
| `assign` | Assigns new fields to the `dict` output of this `Runnable`. |
| `invoke` | Transform a single input into an output. |
| `ainvoke` | Transform a single input into an output. |
| `batch` | Default implementation runs invoke in parallel using a thread pool executor. |
| `batch_as_completed` | Run `invoke` in parallel on a list of inputs. |
| `abatch` | Default implementation runs `ainvoke` in parallel using `asyncio.gather`. |
| `abatch_as_completed` | Run `ainvoke` in parallel on a list of inputs. |
| `stream` | Default implementation of `stream`, which calls `invoke`. |
| `astream` | Default implementation of `astream`, which calls `ainvoke`. |
| `astream_log` | Stream all output from a `Runnable`, as reported to the callback system. |
| `astream_events` | Generate a stream of events. |
| `transform` | Transform inputs to outputs. |
| `atransform` | Transform inputs to outputs. |
| `bind` | Bind arguments to a `Runnable`, returning a new `Runnable`. |
| `with_config` | Bind config to a `Runnable`, returning a new `Runnable`. |
| `with_listeners` | Bind lifecycle listeners to a `Runnable`, returning a new `Runnable`. |
| `with_alisteners` | Bind async lifecycle listeners to a `Runnable`. |
| `with_types` | Bind input and output types to a `Runnable`, returning a new `Runnable`. |
| `with_retry` | Create a new `Runnable` that retries the original `Runnable` on exceptions. |
| `map` | Return a new `Runnable` that maps a list of inputs to a list of outputs. |
| `with_fallbacks` | Add fallbacks to a `Runnable`, returning a new `Runnable`. |
| `as_tool` | Create a `BaseTool` from a `Runnable`. |
| `__init__` |  |
| `is_lc_serializable` | Is this class serializable? |
| `get_lc_namespace` | Get the namespace of the LangChain object. |
| `lc_id` | Return a unique identifier for this class for serialization purposes. |
| `to_json` | Serialize the `Runnable` to JSON. |
| `to_json_not_implemented` | Serialize a "not implemented" object. |
| `configurable_fields` | Configure particular `Runnable` fields at runtime. |
| `configurable_alternatives` | Configure alternatives for `Runnable` objects that can be set at runtime. |

#### ``cache`class-attribute``instance-attribute`¶

```
cache: BaseCache | bool | None = Field(default=None, exclude=True)
```

Whether to cache the response.

- If `True`, will use the global cache.
- If `False`, will not use a cache
- If `None`, will use the global cache if it's set, otherwise no cache.
- If instance of `BaseCache`, will use the provided cache.

Caching is not currently supported for streaming methods of models.

#### ``verbose`class-attribute``instance-attribute`¶

```
verbose: bool = Field(default_factory=_get_verbosity, exclude=True, repr=False)
```

Whether to print out response text.

#### ``callbacks`class-attribute``instance-attribute`¶

```
callbacks: Callbacks = Field(default=None, exclude=True)
```

Callbacks to add to the run trace.

#### ``tags`class-attribute``instance-attribute`¶

```
tags: list[str] | None = Field(default=None, exclude=True)
```

Tags to add to the run trace.

#### ``metadata`class-attribute``instance-attribute`¶

```
metadata: dict[str, Any] | None = Field(default=None, exclude=True)
```

Metadata to add to the run trace.

#### ``custom\_get\_token\_ids`class-attribute``instance-attribute`¶

```
custom_get_token_ids: Callable[[str], list[int]] | None = Field(
    default=None, exclude=True
)
```

Optional encoder to use for counting tokens.

#### ``InputType`property`¶

```
InputType: TypeAlias
```

Get the input type for this `Runnable`.

#### ``name`class-attribute``instance-attribute`¶

```
name: str | None = None
```

The name of the `Runnable`. Used for debugging and tracing.

#### ``OutputType`property`¶

```
OutputType: type[Output]
```

Output Type.

The type of output this `Runnable` produces specified as a type annotation.

| RAISES | DESCRIPTION |
| --- | --- |
| `TypeError` | If the output type cannot be inferred. |

#### ``input\_schema`property`¶

```
input_schema: type[BaseModel]
```

The type of input this `Runnable` accepts specified as a Pydantic model.

#### ``output\_schema`property`¶

```
output_schema: type[BaseModel]
```

Output schema.

The type of output this `Runnable` produces specified as a Pydantic model.

#### ``config\_specs`property`¶

```
config_specs: list[ConfigurableFieldSpec]
```

List configurable fields for this `Runnable`.

#### ``lc\_secrets`property`¶

```
lc_secrets: dict[str, str]
```

A map of constructor argument names to secret ids.

For example, `{"openai_api_key": "OPENAI_API_KEY"}`

#### ``lc\_attributes`property`¶

```
lc_attributes: dict
```

List of attribute names that should be included in the serialized kwargs.

These attributes must be accepted by the constructor.

Default is an empty dictionary.

#### ``set\_verbose ¶

```
set_verbose(verbose: bool | None) -> bool
```

If verbose is `None`, set it.

This allows users to pass in `None` as verbose to access the global setting.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `verbose` | The verbosity setting to use.<br>**TYPE:**`bool | None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | The verbosity setting to use. |

#### ``generate\_prompt`abstractmethod`¶

```
generate_prompt(
    prompts: list[PromptValue],
    stop: list[str] | None = None,
    callbacks: Callbacks = None,
    **kwargs: Any,
) -> LLMResult
```

Pass a sequence of prompts to the model and return model generations.

This method should make use of batched calls for models that expose a batched
API.

Use this method when you want to:

1. Take advantage of batched calls,
2. Need more output from the model than just the top generated value,
3. Are building chains that are agnostic to the underlying language model
    type (e.g., pure text completion models vs chat models).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `prompts` | List of `PromptValue` objects. A `PromptValue` is an object that<br>can be converted to match the format of any language model (string for<br>pure text generation models and `BaseMessage` objects for chat models).<br>**TYPE:**`list[PromptValue]` |
| `stop` | Stop words to use when generating. Model output is cut off at the<br>first occurrence of any of these substrings.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `callbacks` | `Callbacks` to pass through. Used for executing additional<br>functionality, such as logging or streaming, throughout generation.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| `**kwargs` | Arbitrary additional keyword arguments. These are usually passed<br>to the model provider API call.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LLMResult` | An `LLMResult`, which contains a list of candidate `Generation` objects for<br>each input prompt and additional model provider-specific output. |

#### ``agenerate\_prompt`abstractmethod``async`¶

```
agenerate_prompt(
    prompts: list[PromptValue],
    stop: list[str] | None = None,
    callbacks: Callbacks = None,
    **kwargs: Any,
) -> LLMResult
```

Asynchronously pass a sequence of prompts and return model generations.

This method should make use of batched calls for models that expose a batched
API.

Use this method when you want to:

1. Take advantage of batched calls,
2. Need more output from the model than just the top generated value,
3. Are building chains that are agnostic to the underlying language model
    type (e.g., pure text completion models vs chat models).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `prompts` | List of `PromptValue` objects. A `PromptValue` is an object that<br>can be converted to match the format of any language model (string for<br>pure text generation models and `BaseMessage` objects for chat models).<br>**TYPE:**`list[PromptValue]` |
| `stop` | Stop words to use when generating. Model output is cut off at the<br>first occurrence of any of these substrings.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `callbacks` | `Callbacks` to pass through. Used for executing additional<br>functionality, such as logging or streaming, throughout generation.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| `**kwargs` | Arbitrary additional keyword arguments. These are usually passed<br>to the model provider API call.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LLMResult` | An `LLMResult`, which contains a list of candidate `Generation` objects for<br>each input prompt and additional model provider-specific output. |

#### ``with\_structured\_output ¶

```
with_structured_output(
    schema: dict | type, **kwargs: Any
) -> Runnable[LanguageModelInput, dict | BaseModel]
```

Not implemented on this class.

#### ``get\_token\_ids ¶

```
get_token_ids(text: str) -> list[int]
```

Return the ordered IDs of the tokens in a text.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `text` | The string input to tokenize.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[int]` | A list of IDs corresponding to the tokens in the text, in order they occur<br>in the text. |

#### ``get\_num\_tokens ¶

```
get_num_tokens(text: str) -> int
```

Get the number of tokens present in the text.

Useful for checking if an input fits in a model's context window.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `text` | The string input to tokenize.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `int` | The integer number of tokens in the text. |

#### ``get\_num\_tokens\_from\_messages ¶

```
get_num_tokens_from_messages(
    messages: list[BaseMessage], tools: Sequence | None = None
) -> int
```

Get the number of tokens in the messages.

Useful for checking if an input fits in a model's context window.

Note

The base implementation of `get_num_tokens_from_messages` ignores tool
schemas.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `messages` | The message inputs to tokenize.<br>**TYPE:**`list[BaseMessage]` |
| `tools` | If provided, sequence of dict, `BaseModel`, function, or<br>`BaseTool` objects to be converted to tool schemas.<br>**TYPE:**`Sequence | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `int` | The sum of the number of tokens across the messages. |

#### ``get\_name ¶

```
get_name(suffix: str | None = None, *, name: str | None = None) -> str
```

Get the name of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `suffix` | An optional suffix to append to the name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `name` | An optional name to use instead of the `Runnable`'s name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | The name of the `Runnable`. |

#### ``get\_input\_schema ¶

```
get_input_schema(config: RunnableConfig | None = None) -> type[BaseModel]
```

Get a Pydantic model that can be used to validate input to the `Runnable`.

`Runnable` objects that leverage the `configurable_fields` and
`configurable_alternatives` methods will have a dynamic input schema that
depends on which configuration the `Runnable` is invoked with.

This method allows to get an input schema for a specific configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate input. |

#### ``get\_input\_jsonschema ¶

```
get_input_jsonschema(config: RunnableConfig | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the input to the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the input to the `Runnable`. |

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

runnable = RunnableLambda(add_one)

print(runnable.get_input_jsonschema())
```

Added in version 0.3.0

#### ``get\_output\_schema ¶

```
get_output_schema(config: RunnableConfig | None = None) -> type[BaseModel]
```

Get a Pydantic model that can be used to validate output to the `Runnable`.

`Runnable` objects that leverage the `configurable_fields` and
`configurable_alternatives` methods will have a dynamic output schema that
depends on which configuration the `Runnable` is invoked with.

This method allows to get an output schema for a specific configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate output. |

#### ``get\_output\_jsonschema ¶

```
get_output_jsonschema(config: RunnableConfig | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the output of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the output of the `Runnable`. |

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

runnable = RunnableLambda(add_one)

print(runnable.get_output_jsonschema())
```

Added in version 0.3.0

#### ``config\_schema ¶

```
config_schema(*, include: Sequence[str] | None = None) -> type[BaseModel]
```

The type of config this `Runnable` accepts specified as a Pydantic model.

To mark a field as configurable, see the `configurable_fields`
and `configurable_alternatives` methods.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `include` | A list of fields to include in the config schema.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate config. |

#### ``get\_config\_jsonschema ¶

```
get_config_jsonschema(*, include: Sequence[str] | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the config of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `include` | A list of fields to include in the config schema.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the config of the `Runnable`. |

Added in version 0.3.0

#### ``get\_graph ¶

```
get_graph(config: RunnableConfig | None = None) -> Graph
```

Return a graph representation of this `Runnable`.

#### ``get\_prompts ¶

```
get_prompts(config: RunnableConfig | None = None) -> list[BasePromptTemplate]
```

Return a list of prompts used by this `Runnable`.

#### ``\_\_or\_\_ ¶

```
__or__(
    other: Runnable[Any, Other]
    | Callable[[Iterator[Any]], Iterator[Other]]
    | Callable[[AsyncIterator[Any]], AsyncIterator[Other]]
    | Callable[[Any], Other]
    | Mapping[str, Runnable[Any, Other] | Callable[[Any], Other] | Any],
) -> RunnableSerializable[Input, Other]
```

Runnable "or" operator.

Compose this `Runnable` with another object to create a
`RunnableSequence`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `other` | Another `Runnable` or a `Runnable`-like object.<br>**TYPE:**`Runnable[Any, Other] | Callable[[Iterator[Any]], Iterator[Other]] | Callable[[AsyncIterator[Any]], AsyncIterator[Other]] | Callable[[Any], Other] | Mapping[str, Runnable[Any, Other] | Callable[[Any], Other] | Any]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Other]` | A new `Runnable`. |

#### ``\_\_ror\_\_ ¶

```
__ror__(
    other: Runnable[Other, Any]
    | Callable[[Iterator[Other]], Iterator[Any]]
    | Callable[[AsyncIterator[Other]], AsyncIterator[Any]]
    | Callable[[Other], Any]
    | Mapping[str, Runnable[Other, Any] | Callable[[Other], Any] | Any],
) -> RunnableSerializable[Other, Output]
```

Runnable "reverse-or" operator.

Compose this `Runnable` with another object to create a
`RunnableSequence`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `other` | Another `Runnable` or a `Runnable`-like object.<br>**TYPE:**`Runnable[Other, Any] | Callable[[Iterator[Other]], Iterator[Any]] | Callable[[AsyncIterator[Other]], AsyncIterator[Any]] | Callable[[Other], Any] | Mapping[str, Runnable[Other, Any] | Callable[[Other], Any] | Any]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Other, Output]` | A new `Runnable`. |

#### ``pipe ¶

```
pipe(
    *others: Runnable[Any, Other] | Callable[[Any], Other], name: str | None = None
) -> RunnableSerializable[Input, Other]
```

Pipe `Runnable` objects.

Compose this `Runnable` with `Runnable`-like objects to make a
`RunnableSequence`.

Equivalent to `RunnableSequence(self, *others)` or `self | others[0] | ...`

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

def mul_two(x: int) -> int:
    return x * 2

runnable_1 = RunnableLambda(add_one)
runnable_2 = RunnableLambda(mul_two)
sequence = runnable_1.pipe(runnable_2)
# Or equivalently:
# sequence = runnable_1 | runnable_2
# sequence = RunnableSequence(first=runnable_1, last=runnable_2)
sequence.invoke(1)
await sequence.ainvoke(1)
# -> 4

sequence.batch([1, 2, 3])
await sequence.abatch([1, 2, 3])
# -> [4, 6, 8]
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `*others` | Other `Runnable` or `Runnable`-like objects to compose<br>**TYPE:**`Runnable[Any, Other] | Callable[[Any], Other]`**DEFAULT:**`()` |
| `name` | An optional name for the resulting `RunnableSequence`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Other]` | A new `Runnable`. |

#### ``pick ¶

```
pick(keys: str | list[str]) -> RunnableSerializable[Any, Any]
```

Pick keys from the output `dict` of this `Runnable`.

Pick a single key:

```
import json

from langchain_core.runnables import RunnableLambda, RunnableMap

as_str = RunnableLambda(str)
as_json = RunnableLambda(json.loads)
chain = RunnableMap(str=as_str, json=as_json)

chain.invoke("[1, 2, 3]")
# -> {"str": "[1, 2, 3]", "json": [1, 2, 3]}

json_only_chain = chain.pick("json")
json_only_chain.invoke("[1, 2, 3]")
# -> [1, 2, 3]
```

Pick a list of keys:

```
from typing import Any

import json

from langchain_core.runnables import RunnableLambda, RunnableMap

as_str = RunnableLambda(str)
as_json = RunnableLambda(json.loads)

def as_bytes(x: Any) -> bytes:
    return bytes(x, "utf-8")

chain = RunnableMap(str=as_str, json=as_json, bytes=RunnableLambda(as_bytes))

chain.invoke("[1, 2, 3]")
# -> {"str": "[1, 2, 3]", "json": [1, 2, 3], "bytes": b"[1, 2, 3]"}

json_and_bytes_chain = chain.pick(["json", "bytes"])
json_and_bytes_chain.invoke("[1, 2, 3]")
# -> {"json": [1, 2, 3], "bytes": b"[1, 2, 3]"}
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `keys` | A key or list of keys to pick from the output dict.<br>**TYPE:**`str | list[str]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Any, Any]` | a new `Runnable`. |

#### ``assign ¶

```
assign(
    **kwargs: Runnable[dict[str, Any], Any]
    | Callable[[dict[str, Any]], Any]
    | Mapping[str, Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any]],
) -> RunnableSerializable[Any, Any]
```

Assigns new fields to the `dict` output of this `Runnable`.

```
from langchain_core.language_models.fake import FakeStreamingListLLM
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import SystemMessagePromptTemplate
from langchain_core.runnables import Runnable
from operator import itemgetter

prompt = (
    SystemMessagePromptTemplate.from_template("You are a nice assistant.")
    + "{question}"
)
model = FakeStreamingListLLM(responses=["foo-lish"])

chain: Runnable = prompt | model | {"str": StrOutputParser()}

chain_with_assign = chain.assign(hello=itemgetter("str") | model)

print(chain_with_assign.input_schema.model_json_schema())
# {'title': 'PromptInput', 'type': 'object', 'properties':
{'question': {'title': 'Question', 'type': 'string'}}}
print(chain_with_assign.output_schema.model_json_schema())
# {'title': 'RunnableSequenceOutput', 'type': 'object', 'properties':
{'str': {'title': 'Str',
'type': 'string'}, 'hello': {'title': 'Hello', 'type': 'string'}}}
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**kwargs` | A mapping of keys to `Runnable` or `Runnable`-like objects<br>that will be invoked with the entire output dict of this `Runnable`.<br>**TYPE:**`Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any] | Mapping[str, Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any]]`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Any, Any]` | A new `Runnable`. |

#### ``invoke`abstractmethod`¶

```
invoke(input: Input, config: RunnableConfig | None = None, **kwargs: Any) -> Output
```

Transform a single input into an output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``ainvoke`async`¶

```
ainvoke(input: Input, config: RunnableConfig | None = None, **kwargs: Any) -> Output
```

Transform a single input into an output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``batch ¶

```
batch(
    inputs: list[Input],
    config: RunnableConfig | list[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> list[Output]
```

Default implementation runs invoke in parallel using a thread pool executor.

The default implementation of batch works well for IO bound runnables.

Subclasses must override this method if they can batch more efficiently;
e.g., if the underlying `Runnable` uses an API which supports a batch mode.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`list[Input]` |
| `config` | A config to use when invoking the `Runnable`. The config supports<br>standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work<br>to do in parallel, and other keys. Please refer to the<br>`RunnableConfig` for more details.<br>**TYPE:**`RunnableConfig | list[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Output]` | A list of outputs from the `Runnable`. |

#### ``batch\_as\_completed ¶

```
batch_as_completed(
    inputs: Sequence[Input],
    config: RunnableConfig | Sequence[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> Iterator[tuple[int, Output | Exception]]
```

Run `invoke` in parallel on a list of inputs.

Yields results as they complete.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`Sequence[Input]` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | Sequence[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `tuple[int, Output | Exception]` | Tuples of the index of the input and the output from the `Runnable`. |

#### ``abatch`async`¶

```
abatch(
    inputs: list[Input],
    config: RunnableConfig | list[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> list[Output]
```

Default implementation runs `ainvoke` in parallel using `asyncio.gather`.

The default implementation of `batch` works well for IO bound runnables.

Subclasses must override this method if they can batch more efficiently;
e.g., if the underlying `Runnable` uses an API which supports a batch mode.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`list[Input]` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | list[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Output]` | A list of outputs from the `Runnable`. |

#### ``abatch\_as\_completed`async`¶

```
abatch_as_completed(
    inputs: Sequence[Input],
    config: RunnableConfig | Sequence[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> AsyncIterator[tuple[int, Output | Exception]]
```

Run `ainvoke` in parallel on a list of inputs.

Yields results as they complete.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`Sequence[Input]` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | Sequence[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[tuple[int, Output | Exception]]` | A tuple of the index of the input and the output from the `Runnable`. |

#### ``stream ¶

```
stream(
    input: Input, config: RunnableConfig | None = None, **kwargs: Any | None
) -> Iterator[Output]
```

Default implementation of `stream`, which calls `invoke`.

Subclasses must override this method if they support streaming output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``astream`async`¶

```
astream(
    input: Input, config: RunnableConfig | None = None, **kwargs: Any | None
) -> AsyncIterator[Output]
```

Default implementation of `astream`, which calls `ainvoke`.

Subclasses must override this method if they support streaming output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[Output]` | The output of the `Runnable`. |

#### ``astream\_log`async`¶

```
astream_log(
    input: Any,
    config: RunnableConfig | None = None,
    *,
    diff: bool = True,
    with_streamed_output_list: bool = True,
    include_names: Sequence[str] | None = None,
    include_types: Sequence[str] | None = None,
    include_tags: Sequence[str] | None = None,
    exclude_names: Sequence[str] | None = None,
    exclude_types: Sequence[str] | None = None,
    exclude_tags: Sequence[str] | None = None,
    **kwargs: Any,
) -> AsyncIterator[RunLogPatch] | AsyncIterator[RunLog]
```

Stream all output from a `Runnable`, as reported to the callback system.

This includes all inner runs of LLMs, Retrievers, Tools, etc.

Output is streamed as Log objects, which include a list of
Jsonpatch ops that describe how the state of the run has changed in each
step, and the final state of the run.

The Jsonpatch ops can be applied in order to construct state.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Any` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `diff` | Whether to yield diffs between each step or the current state.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `with_streamed_output_list` | Whether to yield the `streamed_output` list.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `include_names` | Only include logs with these names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_types` | Only include logs with these types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_tags` | Only include logs with these tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_names` | Exclude logs with these names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_types` | Exclude logs with these types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_tags` | Exclude logs with these tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[RunLogPatch] | AsyncIterator[RunLog]` | A `RunLogPatch` or `RunLog` object. |

#### ``astream\_events`async`¶

```
astream_events(
    input: Any,
    config: RunnableConfig | None = None,
    *,
    version: Literal["v1", "v2"] = "v2",
    include_names: Sequence[str] | None = None,
    include_types: Sequence[str] | None = None,
    include_tags: Sequence[str] | None = None,
    exclude_names: Sequence[str] | None = None,
    exclude_types: Sequence[str] | None = None,
    exclude_tags: Sequence[str] | None = None,
    **kwargs: Any,
) -> AsyncIterator[StreamEvent]
```

Generate a stream of events.

Use to create an iterator over `StreamEvent` that provide real-time information
about the progress of the `Runnable`, including `StreamEvent` from intermediate
results.

A `StreamEvent` is a dictionary with the following schema:

- `event`: Event names are of the format:
`on_[runnable_type]_(start|stream|end)`.
- `name`: The name of the `Runnable` that generated the event.
- `run_id`: Randomly generated ID associated with the given execution of the
`Runnable` that emitted the event. A child `Runnable` that gets invoked as
part of the execution of a parent `Runnable` is assigned its own unique ID.
- `parent_ids`: The IDs of the parent runnables that generated the event. The
root `Runnable` will have an empty list. The order of the parent IDs is from
the root to the immediate parent. Only available for v2 version of the API.
The v1 version of the API will return an empty list.
- `tags`: The tags of the `Runnable` that generated the event.
- `metadata`: The metadata of the `Runnable` that generated the event.
- `data`: The data associated with the event. The contents of this field
depend on the type of event. See the table below for more details.

Below is a table that illustrates some events that might be emitted by various
chains. Metadata fields have been omitted from the table for brevity.
Chain definitions have been included after the table.

Note

This reference table is for the v2 version of the schema.

| event | name | chunk | input | output |
| --- | --- | --- | --- | --- |
| `on_chat_model_start` | `'[model name]'` |  | `{"messages": [[SystemMessage, HumanMessage]]}` |  |
| `on_chat_model_stream` | `'[model name]'` | `AIMessageChunk(content="hello")` |  |  |
| `on_chat_model_end` | `'[model name]'` |  | `{"messages": [[SystemMessage, HumanMessage]]}` | `AIMessageChunk(content="hello world")` |
| `on_llm_start` | `'[model name]'` |  | `{'input': 'hello'}` |  |
| `on_llm_stream` | `'[model name]'` | `'Hello'` |  |  |
| `on_llm_end` | `'[model name]'` |  | `'Hello human!'` |  |
| `on_chain_start` | `'format_docs'` |  |  |  |
| `on_chain_stream` | `'format_docs'` | `'hello world!, goodbye world!'` |  |  |
| `on_chain_end` | `'format_docs'` |  | `[Document(...)]` | `'hello world!, goodbye world!'` |
| `on_tool_start` | `'some_tool'` |  | `{"x": 1, "y": "2"}` |  |
| `on_tool_end` | `'some_tool'` |  |  | `{"x": 1, "y": "2"}` |
| `on_retriever_start` | `'[retriever name]'` |  | `{"query": "hello"}` |  |
| `on_retriever_end` | `'[retriever name]'` |  | `{"query": "hello"}` | `[Document(...), ..]` |
| `on_prompt_start` | `'[template_name]'` |  | `{"question": "hello"}` |  |
| `on_prompt_end` | `'[template_name]'` |  | `{"question": "hello"}` | `ChatPromptValue(messages: [SystemMessage, ...])` |

In addition to the standard events, users can also dispatch custom events (see example below).

Custom events will be only be surfaced with in the v2 version of the API!

A custom event has following format:

| Attribute | Type | Description |
| --- | --- | --- |
| `name` | `str` | A user defined name for the event. |
| `data` | `Any` | The data associated with the event. This can be anything, though we suggest making it JSON serializable. |

Here are declarations associated with the standard events shown above:

`format_docs`:

```
def format_docs(docs: list[Document]) -> str:
    '''Format the docs.'''
    return ", ".join([doc.page_content for doc in docs])

format_docs = RunnableLambda(format_docs)
```

`some_tool`:

```
@tool
def some_tool(x: int, y: str) -> dict:
    '''Some_tool.'''
    return {"x": x, "y": y}
```

`prompt`:

```
template = ChatPromptTemplate.from_messages(
    [\
        ("system", "You are Cat Agent 007"),\
        ("human", "{question}"),\
    ]
).with_config({"run_name": "my_template", "tags": ["my_template"]})
```

For instance:

```
from langchain_core.runnables import RunnableLambda

async def reverse(s: str) -> str:
    return s[::-1]

chain = RunnableLambda(func=reverse)

events = [event async for event in chain.astream_events("hello", version="v2")]

# Will produce the following events
# (run_id, and parent_ids has been omitted for brevity):
[\
    {\
        "data": {"input": "hello"},\
        "event": "on_chain_start",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
    {\
        "data": {"chunk": "olleh"},\
        "event": "on_chain_stream",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
    {\
        "data": {"output": "olleh"},\
        "event": "on_chain_end",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
]
```

Example: Dispatch Custom Event

```
from langchain_core.callbacks.manager import (
    adispatch_custom_event,
)
from langchain_core.runnables import RunnableLambda, RunnableConfig
import asyncio

async def slow_thing(some_input: str, config: RunnableConfig) -> str:
    """Do something that takes a long time."""
    await asyncio.sleep(1) # Placeholder for some slow operation
    await adispatch_custom_event(
        "progress_event",
        {"message": "Finished step 1 of 3"},
        config=config # Must be included for python < 3.10
    )
    await asyncio.sleep(1) # Placeholder for some slow operation
    await adispatch_custom_event(
        "progress_event",
        {"message": "Finished step 2 of 3"},
        config=config # Must be included for python < 3.10
    )
    await asyncio.sleep(1) # Placeholder for some slow operation
    return "Done"

slow_thing = RunnableLambda(slow_thing)

async for event in slow_thing.astream_events("some_input", version="v2"):
    print(event)
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Any` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `version` | The version of the schema to use either `'v2'` or `'v1'`.<br>Users should use `'v2'`.<br>`'v1'` is for backwards compatibility and will be deprecated<br>in `0.4.0`.<br>No default will be assigned until the API is stabilized.<br>custom events will only be surfaced in `'v2'`.<br>**TYPE:**`Literal['v1', 'v2']`**DEFAULT:**`'v2'` |
| `include_names` | Only include events from `Runnable` objects with matching names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_types` | Only include events from `Runnable` objects with matching types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_tags` | Only include events from `Runnable` objects with matching tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_names` | Exclude events from `Runnable` objects with matching names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_types` | Exclude events from `Runnable` objects with matching types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_tags` | Exclude events from `Runnable` objects with matching tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>These will be passed to `astream_log` as this implementation<br>of `astream_events` is built on top of `astream_log`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[StreamEvent]` | An async stream of `StreamEvent`. |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | If the version is not `'v1'` or `'v2'`. |

#### ``transform ¶

```
transform(
    input: Iterator[Input], config: RunnableConfig | None = None, **kwargs: Any | None
) -> Iterator[Output]
```

Transform inputs to outputs.

Default implementation of transform, which buffers input and calls `astream`.

Subclasses must override this method if they can start producing output while
input is still being generated.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | An iterator of inputs to the `Runnable`.<br>**TYPE:**`Iterator[Input]` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``atransform`async`¶

```
atransform(
    input: AsyncIterator[Input],
    config: RunnableConfig | None = None,
    **kwargs: Any | None,
) -> AsyncIterator[Output]
```

Transform inputs to outputs.

Default implementation of atransform, which buffers input and calls `astream`.

Subclasses must override this method if they can start producing output while
input is still being generated.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | An async iterator of inputs to the `Runnable`.<br>**TYPE:**`AsyncIterator[Input]` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[Output]` | The output of the `Runnable`. |

#### ``bind ¶

```
bind(**kwargs: Any) -> Runnable[Input, Output]
```

Bind arguments to a `Runnable`, returning a new `Runnable`.

Useful when a `Runnable` in a chain requires an argument that is not
in the output of the previous `Runnable` or included in the user input.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**kwargs` | The arguments to bind to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the arguments bound. |

Example

```
from langchain_ollama import ChatOllama
from langchain_core.output_parsers import StrOutputParser

model = ChatOllama(model="llama3.1")

# Without bind
chain = model | StrOutputParser()

chain.invoke("Repeat quoted words exactly: 'One two three four five.'")
# Output is 'One two three four five.'

# With bind
chain = model.bind(stop=["three"]) | StrOutputParser()

chain.invoke("Repeat quoted words exactly: 'One two three four five.'")
# Output is 'One two'
```

#### ``with\_config ¶

```
with_config(
    config: RunnableConfig | None = None, **kwargs: Any
) -> Runnable[Input, Output]
```

Bind config to a `Runnable`, returning a new `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to bind to the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the config bound. |

#### ``with\_listeners ¶

```
with_listeners(
    *,
    on_start: Callable[[Run], None]
    | Callable[[Run, RunnableConfig], None]
    | None = None,
    on_end: Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None = None,
    on_error: Callable[[Run], None]
    | Callable[[Run, RunnableConfig], None]
    | None = None,
) -> Runnable[Input, Output]
```

Bind lifecycle listeners to a `Runnable`, returning a new `Runnable`.

The Run object contains information about the run, including its `id`,
`type`, `input`, `output`, `error`, `start_time`, `end_time`, and
any tags or metadata added to the run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `on_start` | Called before the `Runnable` starts running, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |
| `on_end` | Called after the `Runnable` finishes running, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |
| `on_error` | Called if the `Runnable` throws an error, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the listeners bound. |

Example

```
from langchain_core.runnables import RunnableLambda
from langchain_core.tracers.schemas import Run

import time

def test_runnable(time_to_sleep: int):
    time.sleep(time_to_sleep)

def fn_start(run_obj: Run):
    print("start_time:", run_obj.start_time)

def fn_end(run_obj: Run):
    print("end_time:", run_obj.end_time)

chain = RunnableLambda(test_runnable).with_listeners(
    on_start=fn_start, on_end=fn_end
)
chain.invoke(2)
```

#### ``with\_alisteners ¶

```
with_alisteners(
    *,
    on_start: AsyncListener | None = None,
    on_end: AsyncListener | None = None,
    on_error: AsyncListener | None = None,
) -> Runnable[Input, Output]
```

Bind async lifecycle listeners to a `Runnable`.

Returns a new `Runnable`.

The Run object contains information about the run, including its `id`,
`type`, `input`, `output`, `error`, `start_time`, `end_time`, and
any tags or metadata added to the run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `on_start` | Called asynchronously before the `Runnable` starts running,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |
| `on_end` | Called asynchronously after the `Runnable` finishes running,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |
| `on_error` | Called asynchronously if the `Runnable` throws an error,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the listeners bound. |

Example

```
from langchain_core.runnables import RunnableLambda, Runnable
from datetime import datetime, timezone
import time
import asyncio

def format_t(timestamp: float) -> str:
    return datetime.fromtimestamp(timestamp, tz=timezone.utc).isoformat()

async def test_runnable(time_to_sleep: int):
    print(f"Runnable[{time_to_sleep}s]: starts at {format_t(time.time())}")
    await asyncio.sleep(time_to_sleep)
    print(f"Runnable[{time_to_sleep}s]: ends at {format_t(time.time())}")

async def fn_start(run_obj: Runnable):
    print(f"on start callback starts at {format_t(time.time())}")
    await asyncio.sleep(3)
    print(f"on start callback ends at {format_t(time.time())}")

async def fn_end(run_obj: Runnable):
    print(f"on end callback starts at {format_t(time.time())}")
    await asyncio.sleep(2)
    print(f"on end callback ends at {format_t(time.time())}")

runnable = RunnableLambda(test_runnable).with_alisteners(
    on_start=fn_start,
    on_end=fn_end
)
async def concurrent_runs():
    await asyncio.gather(runnable.ainvoke(2), runnable.ainvoke(3))

asyncio.run(concurrent_runs())
Result:
on start callback starts at 2025-03-01T07:05:22.875378+00:00
on start callback starts at 2025-03-01T07:05:22.875495+00:00
on start callback ends at 2025-03-01T07:05:25.878862+00:00
on start callback ends at 2025-03-01T07:05:25.878947+00:00
Runnable[2s]: starts at 2025-03-01T07:05:25.879392+00:00
Runnable[3s]: starts at 2025-03-01T07:05:25.879804+00:00
Runnable[2s]: ends at 2025-03-01T07:05:27.881998+00:00
on end callback starts at 2025-03-01T07:05:27.882360+00:00
Runnable[3s]: ends at 2025-03-01T07:05:28.881737+00:00
on end callback starts at 2025-03-01T07:05:28.882428+00:00
on end callback ends at 2025-03-01T07:05:29.883893+00:00
on end callback ends at 2025-03-01T07:05:30.884831+00:00
```

#### ``with\_types ¶

```
with_types(
    *, input_type: type[Input] | None = None, output_type: type[Output] | None = None
) -> Runnable[Input, Output]
```

Bind input and output types to a `Runnable`, returning a new `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input_type` | The input type to bind to the `Runnable`.<br>**TYPE:**`type[Input] | None`**DEFAULT:**`None` |
| `output_type` | The output type to bind to the `Runnable`.<br>**TYPE:**`type[Output] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the types bound. |

#### ``with\_retry ¶

```
with_retry(
    *,
    retry_if_exception_type: tuple[type[BaseException], ...] = (Exception,),
    wait_exponential_jitter: bool = True,
    exponential_jitter_params: ExponentialJitterParams | None = None,
    stop_after_attempt: int = 3,
) -> Runnable[Input, Output]
```

Create a new `Runnable` that retries the original `Runnable` on exceptions.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `retry_if_exception_type` | A tuple of exception types to retry on.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| `wait_exponential_jitter` | Whether to add jitter to the wait<br>time between retries.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `stop_after_attempt` | The maximum number of attempts to make before<br>giving up.<br>**TYPE:**`int`**DEFAULT:**`3` |
| `exponential_jitter_params` | Parameters for<br>`tenacity.wait_exponential_jitter`. Namely: `initial`, `max`,<br>`exp_base`, and `jitter` (all `float` values).<br>**TYPE:**`ExponentialJitterParams | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new Runnable that retries the original Runnable on exceptions. |

Example

```
from langchain_core.runnables import RunnableLambda

count = 0

def _lambda(x: int) -> None:
    global count
    count = count + 1
    if x == 1:
        raise ValueError("x is 1")
    else:
        pass

runnable = RunnableLambda(_lambda)
try:
    runnable.with_retry(
        stop_after_attempt=2,
        retry_if_exception_type=(ValueError,),
    ).invoke(1)
except ValueError:
    pass

assert count == 2
```

#### ``map ¶

```
map() -> Runnable[list[Input], list[Output]]
```

Return a new `Runnable` that maps a list of inputs to a list of outputs.

Calls `invoke` with each input.

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[list[Input], list[Output]]` | A new `Runnable` that maps a list of inputs to a list of outputs. |

Example

```
from langchain_core.runnables import RunnableLambda

def _lambda(x: int) -> int:
    return x + 1

runnable = RunnableLambda(_lambda)
print(runnable.map().invoke([1, 2, 3]))  # [2, 3, 4]
```

#### ``with\_fallbacks ¶

```
with_fallbacks(
    fallbacks: Sequence[Runnable[Input, Output]],
    *,
    exceptions_to_handle: tuple[type[BaseException], ...] = (Exception,),
    exception_key: str | None = None,
) -> RunnableWithFallbacks[Input, Output]
```

Add fallbacks to a `Runnable`, returning a new `Runnable`.

The new `Runnable` will try the original `Runnable`, and then each fallback
in order, upon failures.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `fallbacks` | A sequence of runnables to try if the original `Runnable`<br>fails.<br>**TYPE:**`Sequence[Runnable[Input, Output]]` |
| `exceptions_to_handle` | A tuple of exception types to handle.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| `exception_key` | If `string` is specified then handled exceptions will be<br>passed to fallbacks as part of the input under the specified key.<br>If `None`, exceptions will not be passed to fallbacks.<br>If used, the base `Runnable` and its fallbacks must accept a<br>dictionary as input.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableWithFallbacks[Input, Output]` | A new `Runnable` that will try the original `Runnable`, and then each<br>Fallback in order, upon failures. |

Example

```
from typing import Iterator

from langchain_core.runnables import RunnableGenerator

def _generate_immediate_error(input: Iterator) -> Iterator[str]:
    raise ValueError()
    yield ""

def _generate(input: Iterator) -> Iterator[str]:
    yield from "foo bar"

runnable = RunnableGenerator(_generate_immediate_error).with_fallbacks(
    [RunnableGenerator(_generate)]
)
print("".join(runnable.stream({})))  # foo bar
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `fallbacks` | A sequence of runnables to try if the original `Runnable`<br>fails.<br>**TYPE:**`Sequence[Runnable[Input, Output]]` |
| `exceptions_to_handle` | A tuple of exception types to handle.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| `exception_key` | If `string` is specified then handled exceptions will be<br>passed to fallbacks as part of the input under the specified key.<br>If `None`, exceptions will not be passed to fallbacks.<br>If used, the base `Runnable` and its fallbacks must accept a<br>dictionary as input.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableWithFallbacks[Input, Output]` | A new `Runnable` that will try the original `Runnable`, and then each<br>Fallback in order, upon failures. |

#### ``as\_tool ¶

```
as_tool(
    args_schema: type[BaseModel] | None = None,
    *,
    name: str | None = None,
    description: str | None = None,
    arg_types: dict[str, type] | None = None,
) -> BaseTool
```

Create a `BaseTool` from a `Runnable`.

`as_tool` will instantiate a `BaseTool` with a name, description, and
`args_schema` from a `Runnable`. Where possible, schemas are inferred
from `runnable.get_input_schema`. Alternatively (e.g., if the
`Runnable` takes a dict as input and the specific dict keys are not typed),
the schema can be specified directly with `args_schema`. You can also
pass `arg_types` to just specify the required arguments and their types.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `args_schema` | The schema for the tool.<br>**TYPE:**`type[BaseModel] | None`**DEFAULT:**`None` |
| `name` | The name of the tool.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `description` | The description of the tool.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `arg_types` | A dictionary of argument names to types.<br>**TYPE:**`dict[str, type] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `BaseTool` | A `BaseTool` instance. |

Typed dict input:

```
from typing_extensions import TypedDict
from langchain_core.runnables import RunnableLambda

class Args(TypedDict):
    a: int
    b: list[int]

def f(x: Args) -> str:
    return str(x["a"] * max(x["b"]))

runnable = RunnableLambda(f)
as_tool = runnable.as_tool()
as_tool.invoke({"a": 3, "b": [1, 2]})
```

`dict` input, specifying schema via `args_schema`:

```
from typing import Any
from pydantic import BaseModel, Field
from langchain_core.runnables import RunnableLambda

def f(x: dict[str, Any]) -> str:
    return str(x["a"] * max(x["b"]))

class FSchema(BaseModel):
    """Apply a function to an integer and list of integers."""

    a: int = Field(..., description="Integer")
    b: list[int] = Field(..., description="List of ints")

runnable = RunnableLambda(f)
as_tool = runnable.as_tool(FSchema)
as_tool.invoke({"a": 3, "b": [1, 2]})
```

`dict` input, specifying schema via `arg_types`:

```
from typing import Any
from langchain_core.runnables import RunnableLambda

def f(x: dict[str, Any]) -> str:
    return str(x["a"] * max(x["b"]))

runnable = RunnableLambda(f)
as_tool = runnable.as_tool(arg_types={"a": int, "b": list[int]})
as_tool.invoke({"a": 3, "b": [1, 2]})
```

String input:

```
from langchain_core.runnables import RunnableLambda

def f(x: str) -> str:
    return x + "a"

def g(x: str) -> str:
    return x + "z"

runnable = RunnableLambda(f) | g
as_tool = runnable.as_tool()
as_tool.invoke("b")
```

#### ``\_\_init\_\_ ¶

```
__init__(*args: Any, **kwargs: Any) -> None
```

#### ``is\_lc\_serializable`classmethod`¶

```
is_lc_serializable() -> bool
```

Is this class serializable?

By design, even if a class inherits from `Serializable`, it is not serializable
by default. This is to prevent accidental serialization of objects that should
not be serialized.

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | Whether the class is serializable. Default is `False`. |

#### ``get\_lc\_namespace`classmethod`¶

```
get_lc_namespace() -> list[str]
```

Get the namespace of the LangChain object.

For example, if the class is `langchain.llms.openai.OpenAI`, then the
namespace is `["langchain", "llms", "openai"]`

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[str]` | The namespace. |

#### ``lc\_id`classmethod`¶

```
lc_id() -> list[str]
```

Return a unique identifier for this class for serialization purposes.

The unique identifier is a list of strings that describes the path
to the object.

For example, for the class `langchain.llms.openai.OpenAI`, the id is
`["langchain", "llms", "openai", "OpenAI"]`.

#### ``to\_json ¶

```
to_json() -> SerializedConstructor | SerializedNotImplemented
```

Serialize the `Runnable` to JSON.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedConstructor | SerializedNotImplemented` | A JSON-serializable representation of the `Runnable`. |

#### ``to\_json\_not\_implemented ¶

```
to_json_not_implemented() -> SerializedNotImplemented
```

Serialize a "not implemented" object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedNotImplemented` | `SerializedNotImplemented`. |

#### ``configurable\_fields ¶

```
configurable_fields(
    **kwargs: AnyConfigurableField,
) -> RunnableSerializable[Input, Output]
```

Configure particular `Runnable` fields at runtime.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**kwargs` | A dictionary of `ConfigurableField` instances to configure.<br>**TYPE:**`AnyConfigurableField`**DEFAULT:**`{}` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If a configuration key is not found in the `Runnable`. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Output]` | A new `Runnable` with the fields configured. |

```
from langchain_core.runnables import ConfigurableField
from langchain_openai import ChatOpenAI

model = ChatOpenAI(max_tokens=20).configurable_fields(
    max_tokens=ConfigurableField(
        id="output_token_number",
        name="Max tokens in the output",
        description="The maximum number of tokens in the output",
    )
)

# max_tokens = 20
print("max_tokens_20: ", model.invoke("tell me something about chess").content)

# max_tokens = 200
print(
    "max_tokens_200: ",
    model.with_config(configurable={"output_token_number": 200})
    .invoke("tell me something about chess")
    .content,
)
```

#### ``configurable\_alternatives ¶

```
configurable_alternatives(
    which: ConfigurableField,
    *,
    default_key: str = "default",
    prefix_keys: bool = False,
    **kwargs: Runnable[Input, Output] | Callable[[], Runnable[Input, Output]],
) -> RunnableSerializable[Input, Output]
```

Configure alternatives for `Runnable` objects that can be set at runtime.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `which` | The `ConfigurableField` instance that will be used to select the<br>alternative.<br>**TYPE:**`ConfigurableField` |
| `default_key` | The default key to use if no alternative is selected.<br>**TYPE:**`str`**DEFAULT:**`'default'` |
| `prefix_keys` | Whether to prefix the keys with the `ConfigurableField` id.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | A dictionary of keys to `Runnable` instances or callables that<br>return `Runnable` instances.<br>**TYPE:**`Runnable[Input, Output] | Callable[[], Runnable[Input, Output]]`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Output]` | A new `Runnable` with the alternatives configured. |

```
from langchain_anthropic import ChatAnthropic
from langchain_core.runnables.utils import ConfigurableField
from langchain_openai import ChatOpenAI

model = ChatAnthropic(
    model_name="claude-sonnet-4-5-20250929"
).configurable_alternatives(
    ConfigurableField(id="llm"),
    default_key="anthropic",
    openai=ChatOpenAI(),
)

# uses the default model ChatAnthropic
print(model.invoke("which organization created you?").content)

# uses ChatOpenAI
print(
    model.with_config(configurable={"llm": "openai"})
    .invoke("which organization created you?")
    .content
)
```

### ``LangSmithParams ¶

Bases: `TypedDict`

LangSmith parameters for tracing.

#### ``ls\_provider`instance-attribute`¶

```
ls_provider: str
```

Provider of the model.

#### ``ls\_model\_name`instance-attribute`¶

```
ls_model_name: str
```

Name of the model.

#### ``ls\_model\_type`instance-attribute`¶

```
ls_model_type: Literal['chat', 'llm']
```

Type of the model. Should be 'chat' or 'llm'.

#### ``ls\_temperature`instance-attribute`¶

```
ls_temperature: float | None
```

Temperature for generation.

#### ``ls\_max\_tokens`instance-attribute`¶

```
ls_max_tokens: int | None
```

Max tokens for generation.

#### ``ls\_stop`instance-attribute`¶

```
ls_stop: list[str] | None
```

Stop words for generation.

Back to top