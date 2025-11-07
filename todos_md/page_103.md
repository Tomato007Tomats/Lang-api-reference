<!-- URL: page_103 -->

Skip to content

Edit this page

# Utilities

## ``langsmith.utils ¶

Generic utility functions.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `tracing_is_enabled` | Return True if tracing is enabled. |
| `test_tracking_is_disabled` | Return True if testing is enabled. |
| `xor_args` | Validate specified keyword args are mutually exclusive. |
| `raise_for_status_with_text` | Raise an error with the response text. |
| `get_enum_value` | Get the value of a string enum. |
| `log_once` | Log a message at the specified level, but only once. |
| `get_messages_from_inputs` | Extract messages from the given inputs dictionary. |
| `get_message_generation_from_outputs` | Retrieve the message generation from the given outputs. |
| `get_prompt_from_inputs` | Retrieve the prompt from the given inputs. |
| `get_llm_generation_from_outputs` | Get the LLM generation from the outputs. |
| `get_docker_compose_command` | Get the correct docker compose command for this system. |
| `convert_langchain_message` | Convert a LangChain message to an example. |
| `is_base_message_like` | Check if the given object is similar to BaseMessage. |
| `is_env_var_truish` | Check if the given environment variable is truish. |
| `get_env_var` | Retrieve an environment variable from a list of namespaces. |
| `get_tracer_project` | Get the project name for a LangSmith tracer. |
| `filter_logs` | Temporarily adds specified filters to a logger. |
| `get_cache_dir` | Get the testing cache directory. |
| `filter_request_headers` | Filter request headers based on ignore\_hosts and allow\_hosts. |
| `with_cache` | Use a cache for requests. |
| `with_optional_cache` | Use a cache for requests. |
| `deepish_copy` | Deep copy a value with a compromise for uncopyable objects. |
| `is_version_greater_or_equal` | Check if the current version is greater or equal to the target version. |
| `parse_prompt_identifier` | Parse a string in the format of owner/name:hash, name:hash, owner/name, or name. |
| `get_api_url` | Get the LangSmith API URL from the environment or the given value. |
| `get_api_key` | Get the API key from the environment or the given value. |
| `get_workspace_id` | Get workspace ID. |
| `get_host_url` | Get the host URL based on the web URL or API URL. |
| `is_truish` | Check if the value is truish. |

### ``LangSmithError ¶

Bases: `Exception`

An error occurred while communicating with the LangSmith API.

### ``LangSmithAPIError ¶

Bases: `LangSmithError`

Internal server error while communicating with LangSmith.

### ``LangSmithRequestTimeout ¶

Bases: `LangSmithError`

Client took too long to send request body.

### ``LangSmithUserError ¶

Bases: `LangSmithError`

User error caused an exception when communicating with LangSmith.

### ``LangSmithRateLimitError ¶

Bases: `LangSmithError`

You have exceeded the rate limit for the LangSmith API.

### ``LangSmithAuthError ¶

Bases: `LangSmithError`

Couldn't authenticate with the LangSmith API.

### ``LangSmithNotFoundError ¶

Bases: `LangSmithError`

Couldn't find the requested resource.

### ``LangSmithConflictError ¶

Bases: `LangSmithError`

The resource already exists.

### ``LangSmithConnectionError ¶

Bases: `LangSmithError`

Couldn't connect to the LangSmith API.

### ``LangSmithExceptionGroup ¶

Bases: `LangSmithError`

Port of ExceptionGroup for Py < 3.11.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize. |

#### ``\_\_init\_\_ ¶

```
__init__(*args: Any, exceptions: Sequence[Exception], **kwargs: Any) -> None
```

Initialize.

### ``LangSmithWarning ¶

Bases: `UserWarning`

Base class for warnings.

### ``LangSmithMissingAPIKeyWarning ¶

Bases: `LangSmithWarning`

Warning for missing API key.

### ``FilterPoolFullWarning ¶

Bases: `Filter`

Filter urrllib3 warnings logged when the connection pool isn't reused.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize the FilterPoolFullWarning filter. |
| `filter` | urllib3.connectionpool:Connection pool is full, discarding connection: ... |

#### ``\_\_init\_\_ ¶

```
__init__(name: str = '', host: str = '') -> None
```

Initialize the FilterPoolFullWarning filter.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `name` | The name of the filter. Defaults to "".<br>**TYPE:**`str`**DEFAULT:**`''` |
| `host` | The host to filter. Defaults to "".<br>**TYPE:**`str`**DEFAULT:**`''` |

#### ``filter ¶

```
filter(record) -> bool
```

urllib3.connectionpool:Connection pool is full, discarding connection: ...

### ``FilterLangSmithRetry ¶

Bases: `Filter`

Filter for retries from this lib.

| METHOD | DESCRIPTION |
| --- | --- |
| `filter` | Filter retries from this library. |

#### ``filter ¶

```
filter(record) -> bool
```

Filter retries from this library.

### ``LangSmithRetry ¶

Bases: `Retry`

Wrapper to filter logs with this name.

### ``ContextThreadPoolExecutor ¶

Bases: `ThreadPoolExecutor`

ThreadPoolExecutor that copies the context to the child thread.

| METHOD | DESCRIPTION |
| --- | --- |
| `submit` | Submit a function to the executor. |
| `map` | Return an iterator equivalent to stdlib map. |

#### ``submit ¶

```
submit(func: Callable[P, T], *args: args, **kwargs: kwargs) -> Future[T]
```

Submit a function to the executor.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `func` | The function to submit.<br>**TYPE:**`Callable[..., T]` |
| `*args` | The positional arguments to the function.<br>**TYPE:**`Any`**DEFAULT:**`()` |
| `**kwargs` | The keyword arguments to the function.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Future[T]` | Future\[T\]: The future for the function. |

#### ``map ¶

```
map(
    fn: Callable[..., T],
    *iterables: Iterable[Any],
    timeout: float | None = None,
    chunksize: int = 1,
) -> Iterator[T]
```

Return an iterator equivalent to stdlib map.

Each function will receive its own copy of the context from the parent thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `fn` | A callable that will take as many arguments as there are<br>passed iterables.<br>**TYPE:**`Callable[..., T]` |
| `timeout` | The maximum number of seconds to wait. If None, then there<br>is no limit on the wait time.<br>**TYPE:**`float | None`**DEFAULT:**`None` |
| `chunksize` | The size of the chunks the iterable will be broken into<br>before being passed to a child process. This argument is only<br>used by ProcessPoolExecutor; it is ignored by<br>ThreadPoolExecutor.<br>**TYPE:**`int`**DEFAULT:**`1` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Iterator[T]` | An iterator equivalent to: map(func, \*iterables) but the calls may |
| `Iterator[T]` | be evaluated out-of-order. |

| RAISES | DESCRIPTION |
| --- | --- |
| `TimeoutError` | If the entire result iterator could not be generated<br>before the given timeout. |
| `Exception` | If fn(\*args) raises for any values. |

### ``tracing\_is\_enabled ¶

```
tracing_is_enabled(ctx: dict | None = None) -> bool | Literal['local']
```

Return True if tracing is enabled.

### ``test\_tracking\_is\_disabled ¶

```
test_tracking_is_disabled() -> bool
```

Return True if testing is enabled.

### ``xor\_args ¶

```
xor_args(*arg_groups: tuple[str, ...]) -> Callable
```

Validate specified keyword args are mutually exclusive.

### ``raise\_for\_status\_with\_text ¶

```
raise_for_status_with_text(response: Response | Response) -> None
```

Raise an error with the response text.

### ``get\_enum\_value ¶

```
get_enum_value(enu: Enum | str) -> str
```

Get the value of a string enum.

### ``log\_once`cached`¶

```
log_once(level: int, message: str) -> None
```

Log a message at the specified level, but only once.

### ``get\_messages\_from\_inputs ¶

```
get_messages_from_inputs(inputs: Mapping[str, Any]) -> list[dict[str, Any]]
```

Extract messages from the given inputs dictionary.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | The inputs dictionary.<br>**TYPE:**`Mapping[str, Any]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[dict[str, Any]]` | List\[Dict\[str, Any\]\]: A list of dictionaries representing<br>the extracted messages. |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If no message(s) are found in the inputs dictionary. |

### ``get\_message\_generation\_from\_outputs ¶

```
get_message_generation_from_outputs(outputs: Mapping[str, Any]) -> dict[str, Any]
```

Retrieve the message generation from the given outputs.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `outputs` | The outputs dictionary.<br>**TYPE:**`Mapping[str, Any]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | Dict\[str, Any\]: The message generation. |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If no generations are found or if multiple generations are present. |

### ``get\_prompt\_from\_inputs ¶

```
get_prompt_from_inputs(inputs: Mapping[str, Any]) -> str
```

Retrieve the prompt from the given inputs.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | The inputs dictionary.<br>**TYPE:**`Mapping[str, Any]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | The prompt.<br>**TYPE:**`str` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If the prompt is not found or if multiple prompts are present. |

### ``get\_llm\_generation\_from\_outputs ¶

```
get_llm_generation_from_outputs(outputs: Mapping[str, Any]) -> str
```

Get the LLM generation from the outputs.

### ``get\_docker\_compose\_command`cached`¶

```
get_docker_compose_command() -> list[str]
```

Get the correct docker compose command for this system.

### ``convert\_langchain\_message ¶

```
convert_langchain_message(message: BaseMessageLike) -> dict
```

Convert a LangChain message to an example.

### ``is\_base\_message\_like ¶

```
is_base_message_like(obj: object) -> bool
```

Check if the given object is similar to BaseMessage.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `obj` | The object to check.<br>**TYPE:**`object` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | True if the object is similar to BaseMessage, False otherwise.<br>**TYPE:**`bool` |

### ``is\_env\_var\_truish ¶

```
is_env_var_truish(value: str | None) -> bool
```

Check if the given environment variable is truish.

### ``get\_env\_var`cached`¶

```
get_env_var(
    name: str,
    default: str | None = None,
    *,
    namespaces: tuple = ("LANGSMITH", "LANGCHAIN"),
) -> str | None
```

Retrieve an environment variable from a list of namespaces.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `name` | The name of the environment variable.<br>**TYPE:**`str` |
| `default` | The default value to return if the<br>environment variable is not found. Defaults to None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `namespaces` | A tuple of namespaces to search for the<br>environment variable. Defaults to ("LANGSMITH", "LANGCHAINs").<br>**TYPE:**`Tuple`**DEFAULT:**`('LANGSMITH', 'LANGCHAIN')` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str | None` | Optional\[str\]: The value of the environment variable if found,<br>otherwise the default value. |

### ``get\_tracer\_project`cached`¶

```
get_tracer_project(return_default_value=True) -> str | None
```

Get the project name for a LangSmith tracer.

### ``filter\_logs ¶

```
filter_logs(logger: Logger, filters: Sequence[Filter]) -> Generator[None, None, None]
```

Temporarily adds specified filters to a logger.

Parameters:
\- logger: The logger to which the filters will be added.
\- filters: A sequence of logging.Filter objects to be temporarily added
to the logger.

### ``get\_cache\_dir ¶

```
get_cache_dir(cache: str | None) -> str | None
```

Get the testing cache directory.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `cache` | The cache path.<br>**TYPE:**`str | None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str | None` | Optional\[str\]: The cache path if provided, otherwise the value |
| `str | None` | from the LANGSMITH\_TEST\_CACHE environment variable. |

### ``filter\_request\_headers ¶

```
filter_request_headers(
    request: Any,
    *,
    ignore_hosts: Sequence[str] | None = None,
    allow_hosts: Sequence[str] | None = None,
) -> Any
```

Filter request headers based on ignore\_hosts and allow\_hosts.

### ``with\_cache ¶

```
with_cache(
    path: str | Path,
    ignore_hosts: Sequence[str] | None = None,
    allow_hosts: Sequence[str] | None = None,
) -> Generator[None, None, None]
```

Use a cache for requests.

### ``with\_optional\_cache ¶

```
with_optional_cache(
    path: str | Path | None,
    ignore_hosts: Sequence[str] | None = None,
    allow_hosts: Sequence[str] | None = None,
) -> Generator[None, None, None]
```

Use a cache for requests.

### ``deepish\_copy ¶

```
deepish_copy(val: T) -> T
```

Deep copy a value with a compromise for uncopyable objects.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `val` | The value to be deep copied.<br>**TYPE:**`T` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `T` | The deep copied value. |

### ``is\_version\_greater\_or\_equal ¶

```
is_version_greater_or_equal(current_version: str, target_version: str) -> bool
```

Check if the current version is greater or equal to the target version.

### ``parse\_prompt\_identifier ¶

```
parse_prompt_identifier(identifier: str) -> tuple[str, str, str]
```

Parse a string in the format of owner/name:hash, name:hash, owner/name, or name.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `identifier` | The prompt identifier to parse.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `tuple[str, str, str]` | Tuple\[str, str, str\]: A tuple containing (owner, name, hash). |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If the identifier doesn't match the expected formats. |

### ``get\_api\_url ¶

```
get_api_url(api_url: str | None) -> str
```

Get the LangSmith API URL from the environment or the given value.

### ``get\_api\_key ¶

```
get_api_key(api_key: str | None) -> str | None
```

Get the API key from the environment or the given value.

### ``get\_workspace\_id ¶

```
get_workspace_id(workspace_id: str | None) -> str | None
```

Get workspace ID.

### ``get\_host\_url`cached`¶

```
get_host_url(web_url: str | None, api_url: str)
```

Get the host URL based on the web URL or API URL.

### ``is\_truish ¶

```
is_truish(val: Any) -> bool
```

Check if the value is truish.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `val` | The value to check.<br>**TYPE:**`Any` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | True if the value is truish, False otherwise.<br>**TYPE:**`bool` |

Back to top