<!-- URL: page_72 -->

Skip to content

Edit this page

# Run Helpers

## ``langsmith.run\_helpers ¶

Decorator for creating a run tree from functions.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `get_current_run_tree` | Get the current run tree. |
| `get_tracing_context` | Get the current tracing context. |
| `tracing_context` | Set the tracing context for a block of code. |
| `is_traceable_function` | Check if a function is @traceable decorated. |
| `ensure_traceable` | Ensure that a function is traceable. |
| `is_async` | Inspect function or wrapped function to see if it is async. |
| `traceable` | Trace a function with langsmith. |
| `as_runnable` | Convert a function wrapped by the LangSmith @traceable decorator to a Runnable. |

### ``LangSmithExtra ¶

Bases: `TypedDict`

Any additional info to be injected into the run dynamically.

#### ``name`instance-attribute`¶

```
name: str | None
```

Optional name for the run.

#### ``reference\_example\_id`instance-attribute`¶

```
reference_example_id: ID_TYPE | None
```

Optional ID of a reference example.

#### ``run\_extra`instance-attribute`¶

```
run_extra: dict | None
```

Optional additional run information.

#### ``parent`instance-attribute`¶

```
parent: RunTree | str | Mapping | None
```

Optional parent run, can be a RunTree, string, or mapping.

#### ``run\_tree`instance-attribute`¶

```
run_tree: RunTree | None
```

Optional run tree (deprecated).

#### ``project\_name`instance-attribute`¶

```
project_name: str | None
```

Optional name of the project.

#### ``metadata`instance-attribute`¶

```
metadata: dict[str, Any] | None
```

Optional metadata for the run.

#### ``tags`instance-attribute`¶

```
tags: list[str] | None
```

Optional list of tags for the run.

#### ``run\_id`instance-attribute`¶

```
run_id: ID_TYPE | None
```

Optional ID for the run.

#### ``client`instance-attribute`¶

```
client: Client | None
```

Optional LangSmith client.

#### ``on\_end`instance-attribute`¶

```
on_end: Callable[[RunTree], Any] | None
```

Optional callback function to be called after the run ends and is sent.

### ``SupportsLangsmithExtra ¶

Bases: `Protocol`, `Generic[P, R]`

Implementations of this Protocol accept an optional langsmith\_extra parameter.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `*args` | Variable length arguments. |
| `langsmith_extra` | Optional dictionary of<br>additional parameters for Langsmith.<br>**TYPE:**`OptionalLangSmithExtra` |\
| `**kwargs` | Keyword arguments. |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `R` | The return type of the callable. |\
\
| METHOD | DESCRIPTION |\
| --- | --- |\
| `__call__` | Call the instance when it is called as a function. |\
\
#### ``\_\_call\_\_ [¶\
\
```\
__call__(\
    *args: args, langsmith_extra: LangSmithExtra | None = None, **kwargs: kwargs\
) -> R\
```\
\
Call the instance when it is called as a function.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `*args` | Variable length argument list.<br>**TYPE:**`args`**DEFAULT:**`()` |\
| `langsmith_extra` | Optional dictionary containing additional<br>parameters specific to Langsmith.<br>**TYPE:**`LangSmithExtra | None`**DEFAULT:**`None` |\
| `**kwargs` | Arbitrary keyword arguments.<br>**TYPE:**`kwargs`**DEFAULT:**`{}` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `R` | The return value of the method.<br>**TYPE:**`R` |\
\
### ``trace ¶\
\
Manage a LangSmith run in context.\
\
This class can be used as both a synchronous and asynchronous context manager.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `name` | Name of the run.<br>**TYPE:**`str` |\
| `run_type` | Type of run (e.g., "chain", "llm", "tool"). Defaults to "chain".<br>**TYPE:**`RUN_TYPE_T`**DEFAULT:**`'chain'` |\
| `inputs` | Initial input data for the run. Defaults to None.<br>**TYPE:**`Dict | None`**DEFAULT:**`None` |\
| `project_name` | Project name to associate the run with. Defaults to None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `parent` | Parent run. Can be a RunTree, dotted order string, or tracing headers. Defaults to None.<br>**TYPE:**`RunTree | str | Mapping | None`**DEFAULT:**`None` |\
| `tags` | List of tags for the run. Defaults to None.<br>**TYPE:**`List[str] | None`**DEFAULT:**`None` |\
| `metadata` | Additional metadata for the run. Defaults to None.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |\
| `client` | LangSmith client for custom settings. Defaults to None.<br>**TYPE:**`Client | None`**DEFAULT:**`None` |\
| `run_id` | Preset identifier for the run. Defaults to None.<br>**TYPE:**`ID_TYPE | None`**DEFAULT:**`None` |\
| `reference_example_id` | Associates run with a dataset example. Only for root runs in evaluation. Defaults to None.<br>**TYPE:**`ID_TYPE | None`**DEFAULT:**`None` |\
| `exceptions_to_handle` | Exception types to ignore. Defaults to None.<br>**TYPE:**`Tuple[Type[BaseException], ...] | None`**DEFAULT:**`None` |\
| `extra` | Extra data to send to LangSmith. Use 'metadata' instead. Defaults to None.<br>**TYPE:**`Dict | None`**DEFAULT:**`None` |\
\
Examples:\
\
Synchronous usage:\
\
.. code-block:: python\
\
```\
>>> with trace("My Operation", run_type="tool", tags=["important"]) as run:\
...     result = "foo"  # Perform operation\
...     run.metadata["some-key"] = "some-value"\
...     run.end(outputs={"result": result})\
```\
\
Asynchronous usage:\
\
.. code-block:: python\
\
```\
>>> async def main():\
...     async with trace("Async Operation", run_type="tool", tags=["async"]) as run:\
...         result = "foo"  # Await async operation\
...         run.metadata["some-key"] = "some-value"\
...         # "end" just adds the outputs and sets error to None\
...         # The actual patching of the run happens when the context exits\
...         run.end(outputs={"result": result})\
>>> asyncio.run(main())\
```\
\
Handling specific exceptions:\
\
.. code-block:: python\
\
```\
>>> import pytest\
>>> import sys\
>>> with trace("Test", exceptions_to_handle=(pytest.skip.Exception,)):\
...     if sys.platform == "win32": # Just an example\
...         pytest.skip("Skipping test for windows")\
...     result = "foo"  # Perform test operation\
```\
\
| METHOD | DESCRIPTION |\
| --- | --- |\
| `__init__` | Initialize the trace context manager. |\
| `__enter__` | Enter the context manager synchronously. |\
| `__exit__` | Exit the context manager synchronously. |\
| `__aenter__` | Enter the context manager asynchronously. |\
| `__aexit__` | Exit the context manager asynchronously. |\
\
#### ``\_\_init\_\_ ¶\
\
```\
__init__(\
    name: str,\
    run_type: RUN_TYPE_T = "chain",\
    *,\
    inputs: dict | None = None,\
    extra: dict | None = None,\
    project_name: str | None = None,\
    parent: RunTree | str | Mapping | Literal["ignore"] | None = None,\
    tags: list[str] | None = None,\
    metadata: Mapping[str, Any] | None = None,\
    client: Client | None = None,\
    run_id: ID_TYPE | None = None,\
    reference_example_id: ID_TYPE | None = None,\
    exceptions_to_handle: tuple[type[BaseException], ...] | None = None,\
    attachments: Attachments | None = None,\
    **kwargs: Any,\
)\
```\
\
Initialize the trace context manager.\
\
Warns if unsupported kwargs are passed.\
\
#### ``\_\_enter\_\_ ¶\
\
```\
__enter__() -> RunTree\
```\
\
Enter the context manager synchronously.\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `RunTree` | run\_trees.RunTree: The newly created run. |\
\
#### ``\_\_exit\_\_ ¶\
\
```\
__exit__(\
    exc_type: type[BaseException] | None = None,\
    exc_value: BaseException | None = None,\
    traceback: TracebackType | None = None,\
) -> None\
```\
\
Exit the context manager synchronously.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `exc_type` | The type of the exception that occurred, if any.<br>**TYPE:**`type[BaseException] | None`**DEFAULT:**`None` |\
| `exc_value` | The exception instance that occurred, if any.<br>**TYPE:**`BaseException | None`**DEFAULT:**`None` |\
| `traceback` | The traceback object associated with the exception, if any.<br>**TYPE:**`TracebackType | None`**DEFAULT:**`None` |\
\
#### ``\_\_aenter\_\_`async`¶\
\
```\
__aenter__() -> RunTree\
```\
\
Enter the context manager asynchronously.\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `RunTree` | run\_trees.RunTree: The newly created run. |\
\
#### ``\_\_aexit\_\_`async`¶\
\
```\
__aexit__(\
    exc_type: type[BaseException] | None = None,\
    exc_value: BaseException | None = None,\
    traceback: TracebackType | None = None,\
) -> None\
```\
\
Exit the context manager asynchronously.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `exc_type` | The type of the exception that occurred, if any.<br>**TYPE:**`type[BaseException] | None`**DEFAULT:**`None` |\
| `exc_value` | The exception instance that occurred, if any.<br>**TYPE:**`BaseException | None`**DEFAULT:**`None` |\
| `traceback` | The traceback object associated with the exception, if any.<br>**TYPE:**`TracebackType | None`**DEFAULT:**`None` |\
\
### ``get\_current\_run\_tree ¶\
\
```\
get_current_run_tree() -> RunTree | None\
```\
\
Get the current run tree.\
\
### ``get\_tracing\_context ¶\
\
```\
get_tracing_context(context: Context | None = None) -> dict[str, Any]\
```\
\
Get the current tracing context.\
\
### ``tracing\_context ¶\
\
```\
tracing_context(\
    *,\
    project_name: str | None = None,\
    tags: list[str] | None = None,\
    metadata: dict[str, Any] | None = None,\
    parent: RunTree | Mapping | str | Literal[False] | None = None,\
    enabled: bool | Literal["local"] | None = None,\
    client: Client | None = None,\
    replicas: Sequence[WriteReplica] | None = None,\
    distributed_parent_id: str | None = None,\
    **kwargs: Any,\
) -> Generator[None, None, None]\
```\
\
Set the tracing context for a block of code.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `project_name` | The name of the project to log the run to. Defaults to None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `tags` | The tags to add to the run. Defaults to None.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |\
| `metadata` | The metadata to add to the run. Defaults to None.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |\
| `parent` | The parent run to use for the context. Can be a Run/RunTree object,<br>request headers (for distributed tracing), or the dotted order string.<br>Defaults to None.<br>**TYPE:**`RunTree | Mapping | str | Literal[False] | None`**DEFAULT:**`None` |\
| `client` | The client to use for logging the run to LangSmith. Defaults to None,<br>**TYPE:**`Client | None`**DEFAULT:**`None` |\
| `enabled` | Whether tracing is enabled. Defaults to None, meaning it will use the<br>current context value or environment variables.<br>**TYPE:**`bool | Literal['local'] | None`**DEFAULT:**`None` |\
| `replicas` | A sequence of WriteReplica dictionaries to send runs to.<br>Example: \{"api\_url": " [https://api.example.com", "api\_key": "key", "project\_name": "proj"}\]<br>or \[{"project\_name": "my\_experiment", "updates": {"reference\_example\_id": None}}\]<br>**TYPE:**`Sequence[WriteReplica] | None`**DEFAULT:**`None` |\
| `distributed_parent_id` | The distributed parent ID for distributed tracing. Defaults to None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
\
### ``is\_traceable\_function ¶\
\
```\
is_traceable_function(func: Any) -> TypeGuard[SupportsLangsmithExtra[P, R]]\
```\
\
Check if a function is @traceable decorated.\
\
### ``ensure\_traceable ¶\
\
```\
ensure_traceable(\
    func: Callable[P, R],\
    *,\
    name: str | None = None,\
    metadata: Mapping[str, Any] | None = None,\
    tags: list[str] | None = None,\
    client: Client | None = None,\
    reduce_fn: Callable[[Sequence], dict | str] | None = None,\
    project_name: str | None = None,\
    process_inputs: Callable[[dict], dict] | None = None,\
    process_outputs: Callable[..., dict] | None = None,\
    process_chunk: Callable | None = None,\
) -> SupportsLangsmithExtra[P, R]\
```\
\
Ensure that a function is traceable.\
\
### ``is\_async ¶\
\
```\
is_async(func: Callable) -> bool\
```\
\
Inspect function or wrapped function to see if it is async.\
\
### ``traceable ¶\
\
```\
traceable(*args: Any, **kwargs: Any) -> Callable | Callable[[Callable], Callable]\
```\
\
Trace a function with langsmith.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `run_type` | The type of run (span) to create. Examples: llm, chain, tool, prompt,<br>retriever, etc. Defaults to "chain". |\
| `name` | The name of the run. Defaults to the function name. |\
| `metadata` | The metadata to add to the run. Defaults to None. |\
| `tags` | The tags to add to the run. Defaults to None. |\
| `client` | The client to use for logging the run to LangSmith. Defaults to<br>None, which will use the default client. |\
| `reduce_fn` | A function to reduce the output of the function if the function<br>returns a generator. Defaults to None, which means the values will be<br>logged as a list. Note: if the iterator is never exhausted (e.g.<br>the function returns an infinite generator), this will never be<br>called, and the run itself will be stuck in a pending state. |\
| `project_name` | The name of the project to log the run to. Defaults to None,<br>which will use the default project. |\
| `process_inputs` | Custom serialization / processing function for inputs.<br>Defaults to None. |\
| `process_outputs` | Custom serialization / processing function for outputs.<br>Defaults to None. |\
| `dangerously_allow_filesystem` | Whether to allow filesystem access for attachments.<br>Defaults to False.<br>Traces that reference local filepaths will be uploaded to LangSmith.<br>In general, network-hosted applications should not be using this because<br>referenced files are usually on the user's machine, not the host machine. |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Callable | Callable[[Callable], Callable]` | Union\[Callable, Callable\[\[Callable\], Callable\]\]: The decorated function. |\
\
Note\
\
- Requires that LANGSMITH\_TRACING\_V2 be set to 'true' in the environment.\
\
Examples:\
\
Basic usage:\
\
.. code-block:: python\
\
```\
@traceable\
def my_function(x: float, y: float) -> float:\
    return x + y\
\
my_function(5, 6)\
\
@traceable\
async def my_async_function(query_params: dict) -> dict:\
    async with httpx.AsyncClient() as http_client:\
        response = await http_client.get(\
            "https://api.example.com/data",\
            params=query_params,\
        )\
        return response.json()\
\
asyncio.run(my_async_function({"param": "value"}))\
```\
\
Streaming data with a generator:\
\
.. code-block:: python\
\
```\
@traceable\
def my_generator(n: int) -> Iterable:\
    for i in range(n):\
        yield i\
\
for item in my_generator(5):\
    print(item)\
```\
\
Async streaming data:\
\
.. code-block:: python\
\
```\
@traceable\
async def my_async_generator(query_params: dict) -> Iterable:\
    async with httpx.AsyncClient() as http_client:\
        response = await http_client.get(\
            "https://api.example.com/data",\
            params=query_params,\
        )\
        for item in response.json():\
            yield item\
\
async def async_code():\
    async for item in my_async_generator({"param": "value"}):\
        print(item)\
\
asyncio.run(async_code())\
```\
\
Specifying a run type and name:\
\
.. code-block:: python\
\
```\
@traceable(name="CustomName", run_type="tool")\
def another_function(a: float, b: float) -> float:\
    return a * b\
\
another_function(5, 6)\
```\
\
Logging with custom metadata and tags:\
\
.. code-block:: python\
\
```\
@traceable(\
    metadata={"version": "1.0", "author": "John Doe"}, tags=["beta", "test"]\
)\
def tagged_function(x):\
    return x**2\
\
tagged_function(5)\
```\
\
Specifying a custom client and project name:\
\
.. code-block:: python\
\
```\
custom_client = Client(api_key="your_api_key")\
\
@traceable(client=custom_client, project_name="My Special Project")\
def project_specific_function(data):\
    return data\
\
project_specific_function({"data": "to process"})\
```\
\
Manually passing langsmith\_extra:\
\
.. code-block:: python\
\
```\
@traceable\
def manual_extra_function(x):\
    return x**2\
\
manual_extra_function(5, langsmith_extra={"metadata": {"version": "1.0"}})\
```\
\
### ``as\_runnable ¶\
\
```\
as_runnable(traceable_fn: Callable) -> Runnable\
```\
\
Convert a function wrapped by the LangSmith @traceable decorator to a Runnable.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `traceable_fn` | The function wrapped by the @traceable decorator.<br>**TYPE:**`Callable` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Runnable` | A Runnable object that maintains a consistent LangSmith<br>tracing context.<br>**TYPE:**`Runnable` |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `ImportError` | If langchain module is not installed. |\
| `ValueError` | If the provided function is not wrapped by the @traceable decorator. |\
\
Example\
\
> > > @traceable\
> > > ... def my\_function(input\_data):\
> > > ... # Function implementation\
> > > ... pass\
> > > runnable = as\_runnable(my\_function)\
\
Back to top