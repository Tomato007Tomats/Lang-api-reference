<!-- URL: page_132 -->

Skip to content

Edit this page

# Chat models ¶

## ``langchain.chat\_models ¶

Entrypoint to using chat models in LangChain.

Reference docs

This page contains **reference documentation** for chat models. See
the docs for conceptual
guides, tutorials, and examples on using chat models.

### ``init\_chat\_model ¶

```
init_chat_model(
    model: str | None = None,
    *,
    model_provider: str | None = None,
    configurable_fields: Literal["any"] | list[str] | tuple[str, ...] | None = None,
    config_prefix: str | None = None,
    **kwargs: Any,
) -> BaseChatModel | _ConfigurableModel
```

Initialize a chat model from any supported provider using a unified interface.

**Two main use cases:**

1. **Fixed model** – specify the model upfront and get a ready-to-use chat model.
2. **Configurable model** – choose to specify parameters (including model name) at
    runtime via `config`. Makes it easy to switch between models/providers without
    changing your code

Note

Requires the integration package for the chosen model provider to be installed.

See the `model_provider` parameter below for specific package names
(e.g., `pip install langchain-openai`).

Refer to the provider integration's API reference
for supported model parameters to use as `**kwargs`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| #### `model` ¶ "Copy anchor link to this section for reference") | The name or ID of the model, e.g. `'o3-mini'`, `'claude-sonnet-4-5-20250929'`.<br>You can also specify model and model provider in a single argument using<br>`'{model_provider}:{model}'` format, e.g. `'openai:o1'`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| #### `model_provider` ¶ "Copy anchor link to this section for reference") | The model provider if not specified as part of the model arg<br>(see above).<br>Supported `model_provider` values and the corresponding integration package<br>are:<br>- `openai` -\> `langchain-openai`<br>- `anthropic` -\> `langchain-anthropic`<br>- `azure_openai` -\> `langchain-openai`<br>- `azure_ai` -\> `langchain-azure-ai`<br>- `google_vertexai` -\> `langchain-google-vertexai`<br>- `google_genai` -\> `langchain-google-genai`<br>- `bedrock` -\> `langchain-aws`<br>- `bedrock_converse` -\> `langchain-aws`<br>- `cohere` -\> `langchain-cohere`<br>- `fireworks` -\> `langchain-fireworks`<br>- `together` -\> `langchain-together`<br>- `mistralai` -\> `langchain-mistralai`<br>- `huggingface` -\> `langchain-huggingface`<br>- `groq` -\> `langchain-groq`<br>- `ollama` -\> `langchain-ollama`<br>- `google_anthropic_vertex` -\> `langchain-google-vertexai`<br>- `deepseek` -\> `langchain-deepseek`<br>- `ibm` -\> `langchain-ibm`<br>- `nvidia` -\> `langchain-nvidia-ai-endpoints`<br>- `xai` -\> `langchain-xai`<br>- `perplexity` -\> `langchain-perplexity`<br>Will attempt to infer `model_provider` from model if not specified. The<br>following providers will be inferred based on these model prefixes:<br>- `gpt-...` \| `o1...` \| `o3...` -\> `openai`<br>- `claude...` -\> `anthropic`<br>- `amazon...` -\> `bedrock`<br>- `gemini...` -\> `google_vertexai`<br>- `command...` -\> `cohere`<br>- `accounts/fireworks...` -\> `fireworks`<br>- `mistral...` -\> `mistralai`<br>- `deepseek...` -\> `deepseek`<br>- `grok...` -\> `xai`<br>- `sonar...` -\> `perplexity`<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| #### `configurable_fields` ¶ "Copy anchor link to this section for reference") | Which model parameters are configurable at runtime:<br>- `None`: No configurable fields (i.e., a fixed model).<br>- `'any'`: All fields are configurable. **See security note below.**<br>- `list[str] | Tuple[str, ...]`: Specified fields are configurable.<br>Fields are assumed to have `config_prefix` stripped if a `config_prefix` is<br>specified.<br>If `model` is specified, then defaults to `None`.<br>If `model` is not specified, then defaults to `("model", "model_provider")`.<br>Security note<br>Setting `configurable_fields="any"` means fields like `api_key`,<br>`base_url`, etc., can be altered at runtime, potentially redirecting<br>model requests to a different service/user.<br>Make sure that if you're accepting untrusted configurations that you<br>enumerate the `configurable_fields=(...)` explicitly.<br>**TYPE:**`Literal['any'] | list[str] | tuple[str, ...] | None`**DEFAULT:**`None` |
| #### `config_prefix` ¶ "Copy anchor link to this section for reference") | Optional prefix for configuration keys.<br>Useful when you have multiple configurable models in the same application.<br>If `'config_prefix'` is a non-empty string then `model` will be configurable<br>at runtime via the `config"configurable"` keys.<br>See examples below.<br>If `'config_prefix'` is an empty string then model will be configurable via<br>`config"configurable"`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| #### `**kwargs` ¶ "Copy anchor link to this section for reference") | Additional model-specific keyword args to pass to the underlying<br>chat model's `__init__` method. Common parameters include:<br>- `temperature`: Model temperature for controlling randomness.<br>- `max_tokens`: Maximum number of output tokens.<br>- `timeout`: Maximum time (in seconds) to wait for a response.<br>- `max_retries`: Maximum number of retry attempts for failed requests.<br>- `base_url`: Custom API endpoint URL.<br>- `rate_limiter`: A<br>`BaseRateLimiter`<br>instance to control request rate.<br>Refer to the specific model provider's<br>integration reference<br>for all available parameters.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `BaseChatModel | _ConfigurableModel` | A `BaseChatModel` corresponding to the `model_name` and `model_provider`<br>specified if configurability is inferred to be `False`. If configurable, a<br>chat model emulator that initializes the underlying model at runtime once a<br>config is passed in. |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If `model_provider` cannot be inferred or isn't supported. |
| `ImportError` | If the model provider integration package is not installed. |

Initialize a non-configurable model

```
# pip install langchain langchain-openai langchain-anthropic langchain-google-vertexai

from langchain.chat_models import init_chat_model

o3_mini = init_chat_model("openai:o3-mini", temperature=0)
claude_sonnet = init_chat_model("anthropic:claude-sonnet-4-5-20250929", temperature=0)
gemini_2-5_flash = init_chat_model("google_vertexai:gemini-2.5-flash", temperature=0)

o3_mini.invoke("what's your name")
claude_sonnet.invoke("what's your name")
gemini_2-5_flash.invoke("what's your name")
```

Partially configurable model with no default

```
# pip install langchain langchain-openai langchain-anthropic

from langchain.chat_models import init_chat_model

# (We don't need to specify configurable=True if a model isn't specified.)
configurable_model = init_chat_model(temperature=0)

configurable_model.invoke("what's your name", config={"configurable": {"model": "gpt-4o"}})
# Use GPT-4o to generate the response

configurable_model.invoke(
    "what's your name",
    config={"configurable": {"model": "claude-sonnet-4-5-20250929"}},
)
```

Fully configurable model with a default

```
# pip install langchain langchain-openai langchain-anthropic

from langchain.chat_models import init_chat_model

configurable_model_with_default = init_chat_model(
    "openai:gpt-4o",
    configurable_fields="any",  # This allows us to configure other params like temperature, max_tokens, etc at runtime.
    config_prefix="foo",
    temperature=0,
)

configurable_model_with_default.invoke("what's your name")
# GPT-4o response with temperature 0 (as set in default)

configurable_model_with_default.invoke(
    "what's your name",
    config={
        "configurable": {
            "foo_model": "anthropic:claude-sonnet-4-5-20250929",
            "foo_temperature": 0.6,
        }
    },
)
# Override default to use Sonnet 4.5 with temperature 0.6 to generate response
```

Bind tools to a configurable model

You can call any chat model declarative methods on a configurable model in the
same way that you would with a normal model:

```
# pip install langchain langchain-openai langchain-anthropic

from langchain.chat_models import init_chat_model
from pydantic import BaseModel, Field

class GetWeather(BaseModel):
    '''Get the current weather in a given location'''

    location: str = Field(..., description="The city and state, e.g. San Francisco, CA")

class GetPopulation(BaseModel):
    '''Get the current population in a given location'''

    location: str = Field(..., description="The city and state, e.g. San Francisco, CA")

configurable_model = init_chat_model(
    "gpt-4o", configurable_fields=("model", "model_provider"), temperature=0
)

configurable_model_with_tools = configurable_model.bind_tools(
    [\
        GetWeather,\
        GetPopulation,\
    ]
)
configurable_model_with_tools.invoke(
    "Which city is hotter today and which is bigger: LA or NY?"
)
# Use GPT-4o

configurable_model_with_tools.invoke(
    "Which city is hotter today and which is bigger: LA or NY?",
    config={"configurable": {"model": "claude-sonnet-4-5-20250929"}},
)
# Use Sonnet 4.5
```

Back to top