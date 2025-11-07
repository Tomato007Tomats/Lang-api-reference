<!-- URL: page_133 -->

Skip to content

Edit this page

# Client

## ``langsmith.client ¶

Client for interacting with the LangSmith API.

Use the client to customize API keys / workspace connections, SSL certs,
etc. for tracing.

Also used to create, read, update, and delete LangSmith resources
such as runs (~trace spans), datasets, examples (~records),
feedback (~metrics), projects (tracer sessions/groups), etc.

For detailed API documentation, visit: https://docs.smith.langchain.com/.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `close_session` | Close the session. |
| `convert_prompt_to_openai_format` | Convert a prompt to OpenAI format. |
| `convert_prompt_to_anthropic_format` | Convert a prompt to Anthropic format. |
| `dump_model` | Dump model depending on pydantic version. |
| `prep_obj_for_push` | Format the object so its Prompt Hub compatible. |

### ``Client ¶

Client for interacting with the LangSmith API.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize a Client instance. |
| `__repr__` | Return a string representation of the instance with a link to the URL. |
| `request_with_retries` | Send a request with retries. |
| `upload_dataframe` | Upload a dataframe as individual examples to the LangSmith API. |
| `upload_csv` | Upload a CSV file to the LangSmith API. |
| `create_run` | Persist a run to the LangSmith API. |
| `batch_ingest_runs` | Batch ingest/upsert multiple runs in the Langsmith system. |
| `multipart_ingest` | Batch ingest/upsert multiple runs in the Langsmith system. |
| `update_run` | Update a run in the LangSmith API. |
| `flush_compressed_traces` | Force flush the currently buffered compressed runs. |
| `flush` | Flush either queue or compressed buffer, depending on mode. |
| `read_run` | Read a run from the LangSmith API. |
| `list_runs` | List runs from the LangSmith API. |
| `get_run_stats` | Get aggregate statistics over queried runs. |
| `get_run_url` | Get the URL for a run. |
| `share_run` | Get a share link for a run. |
| `unshare_run` | Delete share link for a run. |
| `read_run_shared_link` | Retrieve the shared link for a specific run. |
| `run_is_shared` | Get share state for a run. |
| `read_shared_run` | Get shared runs. |
| `list_shared_runs` | Get shared runs. |
| `read_dataset_shared_schema` | Retrieve the shared schema of a dataset. |
| `share_dataset` | Get a share link for a dataset. |
| `unshare_dataset` | Delete share link for a dataset. |
| `read_shared_dataset` | Get shared datasets. |
| `list_shared_examples` | Get shared examples. |
| `list_shared_projects` | List shared projects. |
| `create_project` | Create a project on the LangSmith API. |
| `update_project` | Update a LangSmith project. |
| `read_project` | Read a project from the LangSmith API. |
| `has_project` | Check if a project exists. |
| `get_test_results` | Read the record-level information from an experiment into a Pandas DF. |
| `list_projects` | List projects from the LangSmith API. |
| `delete_project` | Delete a project from LangSmith. |
| `create_dataset` | Create a dataset in the LangSmith API. |
| `has_dataset` | Check whether a dataset exists in your tenant. |
| `read_dataset` | Read a dataset from the LangSmith API. |
| `diff_dataset_versions` | Get the difference between two versions of a dataset. |
| `read_dataset_openai_finetuning` | Download a dataset in OpenAI Jsonl format and load it as a list of dicts. |
| `list_datasets` | List the datasets on the LangSmith API. |
| `delete_dataset` | Delete a dataset from the LangSmith API. |
| `update_dataset_tag` | Update the tags of a dataset. |
| `list_dataset_versions` | List dataset versions. |
| `read_dataset_version` | Get dataset version by as\_of or exact tag. |
| `clone_public_dataset` | Clone a public dataset to your own langsmith tenant. |
| `create_llm_example` | Add an example (row) to an LLM-type dataset. |
| `create_chat_example` | Add an example (row) to a Chat-type dataset. |
| `create_example_from_run` | Add an example (row) to a dataset from a run. |
| `update_examples_multipart` | Update examples using multipart. |
| `upload_examples_multipart` | Upload examples using multipart. |
| `upsert_examples_multipart` | Upsert examples. |
| `create_examples` | Create examples in a dataset. |
| `create_example` | Create a dataset example in the LangSmith API. |
| `read_example` | Read an example from the LangSmith API. |
| `list_examples` | Retrieve the example rows of the specified dataset. |
| `index_dataset` | Enable dataset indexing. Examples are indexed by their inputs. |
| `sync_indexed_dataset` | Sync dataset index. This already happens automatically every 5 minutes, but you can call this to force a sync. |
| `similar_examples` | Retrieve the dataset examples whose inputs best match the current inputs. |
| `update_example` | Update a specific example. |
| `update_examples` | Update multiple examples. |
| `delete_example` | Delete an example by ID. |
| `delete_examples` | Delete multiple examples by ID. |
| `list_dataset_splits` | Get the splits for a dataset. |
| `update_dataset_splits` | Update the splits for a dataset. |
| `evaluate_run` | Evaluate a run. |
| `aevaluate_run` | Evaluate a run asynchronously. |
| `create_feedback` | Create feedback for a run. |
| `update_feedback` | Update a feedback in the LangSmith API. |
| `read_feedback` | Read a feedback from the LangSmith API. |
| `list_feedback` | List the feedback objects on the LangSmith API. |
| `delete_feedback` | Delete a feedback by ID. |
| `create_feedback_from_token` | Create feedback from a presigned token or URL. |
| `create_presigned_feedback_token` | Create a pre-signed URL to send feedback data to. |
| `create_presigned_feedback_tokens` | Create a pre-signed URL to send feedback data to. |
| `list_presigned_feedback_tokens` | List the feedback ingest tokens for a run. |
| `list_annotation_queues` | List the annotation queues on the LangSmith API. |
| `create_annotation_queue` | Create an annotation queue on the LangSmith API. |
| `read_annotation_queue` | Read an annotation queue with the specified queue ID. |
| `update_annotation_queue` | Update an annotation queue with the specified queue\_id. |
| `delete_annotation_queue` | Delete an annotation queue with the specified queue ID. |
| `add_runs_to_annotation_queue` | Add runs to an annotation queue with the specified queue ID. |
| `delete_run_from_annotation_queue` | Delete a run from an annotation queue with the specified queue ID and run ID. |
| `get_run_from_annotation_queue` | Get a run from an annotation queue at the specified index. |
| `create_comparative_experiment` | Create a comparative experiment on the LangSmith API. |
| `arun_on_dataset` | Asynchronously run the Chain or language model on a dataset. |
| `run_on_dataset` | Run the Chain or language model on a dataset. |
| `like_prompt` | Like a prompt. |
| `unlike_prompt` | Unlike a prompt. |
| `list_prompts` | List prompts with pagination. |
| `get_prompt` | Get a specific prompt by its identifier. |
| `create_prompt` | Create a new prompt. |
| `create_commit` | Create a commit for an existing prompt. |
| `update_prompt` | Update a prompt's metadata. |
| `delete_prompt` | Delete a prompt. |
| `pull_prompt_commit` | Pull a prompt object from the LangSmith API. |
| `list_prompt_commits` | List commits for a given prompt. |
| `pull_prompt` | Pull a prompt and return it as a LangChain PromptTemplate. |
| `push_prompt` | Push a prompt to the LangSmith API. |
| `cleanup` | Manually trigger cleanup of the background thread. |
| `evaluate` | Evaluate a target system on a given dataset. |
| `aevaluate` | Evaluate an async target system on a given dataset. |
| `get_experiment_results` | Get results for an experiment, including experiment session aggregated stats and experiment runs for each dataset example. |
| `generate_insights` | Generate Insights over your agent chat histories. |
| `poll_insights` | Poll the status of an Insights report. |

#### ``api\_key`property``writable`¶

```
api_key: str | None
```

Return the API key used for authentication.

#### ``workspace\_id`property``writable`¶

```
workspace_id: str | None
```

Return the workspace ID used for API requests.

#### ``info`property`¶

```
info: LangSmithInfo
```

Get the information about the LangSmith API.

| RETURNS | DESCRIPTION |
| --- | --- |
| `LangSmithInfo` | ls\_schemas.LangSmithInfo: The information about the LangSmith API, or None if the API is<br>not available. |

#### ``\_\_init\_\_ ¶

```
__init__(
    api_url: str | None = None,
    *,
    api_key: str | None = None,
    retry_config: Retry | None = None,
    timeout_ms: int | tuple[int, int] | None = None,
    web_url: str | None = None,
    session: Session | None = None,
    auto_batch_tracing: bool = True,
    anonymizer: Callable[[dict], dict] | None = None,
    hide_inputs: Callable[[dict], dict] | bool | None = None,
    hide_outputs: Callable[[dict], dict] | bool | None = None,
    hide_metadata: Callable[[dict], dict] | bool | None = None,
    process_buffered_run_ops: Callable[[Sequence[dict]], Sequence[dict]] | None = None,
    run_ops_buffer_size: int | None = None,
    run_ops_buffer_timeout_ms: float | None = None,
    info: dict | LangSmithInfo | None = None,
    api_urls: dict[str, str] | None = None,
    otel_tracer_provider: TracerProvider | None = None,
    otel_enabled: bool | None = None,
    tracing_sampling_rate: float | None = None,
    workspace_id: str | None = None,
    max_batch_size_bytes: int | None = None,
) -> None
```

Initialize a Client instance.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `api_url` | URL for the LangSmith API. Defaults to the LANGCHAIN\_ENDPOINT<br>environment variable or https://api.smith.langchain.com if not set.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `api_key` | API key for the LangSmith API. Defaults to the LANGCHAIN\_API\_KEY<br>environment variable.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `retry_config` | Retry configuration for the HTTPAdapter.<br>**TYPE:**`Retry | None`**DEFAULT:**`None` |
| `timeout_ms` | Timeout for the HTTPAdapter. Can also be a 2-tuple of<br>(connect timeout, read timeout) to set them separately.<br>**TYPE:**`int | Tuple[int, int] | None`**DEFAULT:**`None` |
| `web_url` | URL for the LangSmith web app. Default is auto-inferred from<br>the ENDPOINT.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `session` | The session to use for requests. If None, a new session will be<br>created.<br>**TYPE:**`Session | None`**DEFAULT:**`None` |
| `auto_batch_tracing` | Whether to automatically batch tracing.<br>**TYPE:**`bool, default=True`**DEFAULT:**`True` |
| `anonymizer` | A function applied for masking serialized run inputs and outputs,<br>before sending to the API.<br>**TYPE:**`Callable[[dict], dict] | None`**DEFAULT:**`None` |
| `hide_inputs` | Whether to hide run inputs when tracing with this client.<br>If True, hides the entire inputs. If a function, applied to<br>all run inputs when creating runs.<br>**TYPE:**`Callable[[dict], dict] | bool | None`**DEFAULT:**`None` |
| `hide_outputs` | Whether to hide run outputs when tracing with this client.<br>If True, hides the entire outputs. If a function, applied to<br>all run outputs when creating runs.<br>**TYPE:**`Callable[[dict], dict] | bool | None`**DEFAULT:**`None` |
| `hide_metadata` | Whether to hide run metadata when tracing with this client.<br>If True, hides the entire metadata. If a function, applied to<br>all run metadata when creating runs.<br>**TYPE:**`Callable[[dict], dict] | bool | None`**DEFAULT:**`None` |
| `process_buffered_run_ops` | A function applied to buffered run operations<br>that allows for modification of the raw run dicts before they are converted to multipart and compressed.<br>This is useful specifically for high throughput tracing where you need to apply a rate-limited API or other<br>costly process to the runs before they are sent to the API. Note that the buffer will only flush automatically<br>when run\_ops\_buffer\_size is reached or a new run is added to the buffer after run\_ops\_buffer\_timeout\_ms<br>has elapsed - it will not flush outside of these conditions unless you manually<br>call client.flush(), so be sure to do this before your code exits.<br>**TYPE:**`Callable[[Sequence[dict]], Sequence[dict]] | None`**DEFAULT:**`None` |
| `run_ops_buffer_size` | Maximum number of run operations to collect in the buffer before applying<br>process\_buffered\_run\_ops and sending to the API. Required when process\_buffered\_run\_ops is provided.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `run_ops_buffer_timeout_ms` | Maximum time in milliseconds to wait before flushing the run ops buffer<br>when new runs are added. Defaults to 5000. Only used when process\_buffered\_run\_ops is provided.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `info` | The information about the LangSmith API.<br>If not provided, it will be fetched from the API.<br>**TYPE:**`LangSmithInfo | None`**DEFAULT:**`None` |
| `api_urls` | A dictionary of write API URLs and their corresponding API keys.<br>Useful for multi-tenant setups. Data is only read from the first<br>URL in the dictionary. However, ONLY Runs are written (POST and PATCH)<br>to all URLs in the dictionary. Feedback, sessions, datasets, examples,<br>annotation queues and evaluation results are only written to the first.<br>**TYPE:**`Dict[str, str] | None`**DEFAULT:**`None` |
| `otel_tracer_provider` | Optional tracer provider for OpenTelemetry integration.<br>If not provided, a LangSmith-specific tracer provider will be used.<br>**TYPE:**`TracerProvider | None`**DEFAULT:**`None` |
| `tracing_sampling_rate` | The sampling rate for tracing. If provided,<br>overrides the LANGCHAIN\_TRACING\_SAMPLING\_RATE environment variable.<br>Should be a float between 0 and 1, where 1 means trace everything<br>and 0 means trace nothing.<br>**TYPE:**`float | None`**DEFAULT:**`None` |
| `workspace_id` | The workspace ID. Required for org-scoped API keys.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `max_batch_size_bytes` | The maximum size of a batch of runs in bytes.<br>If not provided, the default is set by the server.<br>**TYPE:**`int | None`**DEFAULT:**`None` |

| RAISES | DESCRIPTION |
| --- | --- |
| `LangSmithUserError` | If the API key is not provided when using the hosted service.<br>If both api\_url and api\_urls are provided. |

#### ``\_\_repr\_\_ ¶

```
__repr__() -> str
```

Return a string representation of the instance with a link to the URL.

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | The string representation of the instance.<br>**TYPE:**`str` |

#### ``request\_with\_retries ¶

```
request_with_retries(
    method: Literal["GET", "POST", "PUT", "PATCH", "DELETE"],
    pathname: str,
    *,
    request_kwargs: Mapping | None = None,
    stop_after_attempt: int = 1,
    retry_on: Sequence[type[BaseException]] | None = None,
    to_ignore: Sequence[type[BaseException]] | None = None,
    handle_response: Callable[[Response, int], Any] | None = None,
    _context: str = "",
    **kwargs: Any,
) -> Response
```

Send a request with retries.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `method` | The HTTP request method.<br>**TYPE:**`str` |
| `pathname` | The pathname of the request URL. Will be appended to the API URL.<br>**TYPE:**`str` |
| `request_kwargs` | Additional request parameters.<br>**TYPE:**`Mapping`**DEFAULT:**`None` |
| `stop_after_attempt` | The number of attempts to make.<br>**TYPE:**`int, default=1`**DEFAULT:**`1` |
| `retry_on` | The exceptions to retry on. In addition to:<br>\[LangSmithConnectionError, LangSmithAPIError\].<br>**TYPE:**`Sequence[Type[BaseException]] | None`**DEFAULT:**`None` |
| `to_ignore` | The exceptions to ignore / pass on.<br>**TYPE:**`Sequence[Type[BaseException]] | None`**DEFAULT:**`None` |
| `handle_response` | A function to handle the response and return whether to continue retrying.<br>**TYPE:**`Callable[[Response, int], Any] | None`**DEFAULT:**`None` |
| `_context` | The context of the request.<br>**TYPE:**`str, default=""`**DEFAULT:**`''` |
| `**kwargs` | Additional keyword arguments to pass to the request.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Response` | requests.Response: The response object. |

| RAISES | DESCRIPTION |
| --- | --- |
| `LangSmithAPIError` | If a server error occurs. |
| `LangSmithUserError` | If the request fails. |
| `LangSmithConnectionError` | If a connection error occurs. |
| `LangSmithError` | If the request fails. |

#### ``upload\_dataframe ¶

```
upload_dataframe(
    df: DataFrame,
    name: str,
    input_keys: Sequence[str],
    output_keys: Sequence[str],
    *,
    description: str | None = None,
    data_type: DataType | None = kv,
) -> Dataset
```

Upload a dataframe as individual examples to the LangSmith API.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `df` | The dataframe to upload.<br>**TYPE:**`DataFrame` |
| `name` | The name of the dataset.<br>**TYPE:**`str` |
| `input_keys` | The input keys.<br>**TYPE:**`Sequence[str]` |
| `output_keys` | The output keys.<br>**TYPE:**`Sequence[str]` |
| `description` | The description of the dataset.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `data_type` | The data type of the dataset.<br>**TYPE:**`DataType | None`**DEFAULT:**`kv` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Dataset` | The uploaded dataset.<br>**TYPE:**`Dataset` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If the csv\_file is not a string or tuple. |

Examples:

```
.. code-block:: python

    from langsmith import Client
    import os
    import pandas as pd

    client = Client()

    df = pd.read_parquet("path/to/your/myfile.parquet")
    input_keys = ["column1", "column2"]  # replace with your input column names
    output_keys = ["output1", "output2"]  # replace with your output column names

    dataset = client.upload_dataframe(
        df=df,
        input_keys=input_keys,
        output_keys=output_keys,
        name="My Parquet Dataset",
        description="Dataset created from a parquet file",
        data_type="kv",  # The default
    )
```

#### ``upload\_csv ¶

```
upload_csv(
    csv_file: str | tuple[str, BytesIO],
    input_keys: Sequence[str],
    output_keys: Sequence[str],
    *,
    name: str | None = None,
    description: str | None = None,
    data_type: DataType | None = kv,
) -> Dataset
```

Upload a CSV file to the LangSmith API.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `csv_file` | The CSV file to upload. If a string, it should be the path<br>If a tuple, it should be a tuple containing the filename<br>and a BytesIO object.<br>**TYPE:**`str | Tuple[str, BytesIO]` |
| `input_keys` | The input keys.<br>**TYPE:**`Sequence[str]` |
| `output_keys` | The output keys.<br>**TYPE:**`Sequence[str]` |
| `name` | The name of the dataset.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `description` | The description of the dataset.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `data_type` | The data type of the dataset.<br>**TYPE:**`DataType | None`**DEFAULT:**`kv` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Dataset` | The uploaded dataset.<br>**TYPE:**`Dataset` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If the csv\_file is not a string or tuple. |

Examples:

```
.. code-block:: python

    from langsmith import Client
    import os

    client = Client()

    csv_file = "path/to/your/myfile.csv"
    input_keys = ["column1", "column2"]  # replace with your input column names
    output_keys = ["output1", "output2"]  # replace with your output column names

    dataset = client.upload_csv(
        csv_file=csv_file,
        input_keys=input_keys,
        output_keys=output_keys,
        name="My CSV Dataset",
        description="Dataset created from a CSV file",
        data_type="kv",  # The default
    )
```

#### ``create\_run ¶

```
create_run(
    name: str,
    inputs: dict[str, Any],
    run_type: RUN_TYPE_T,
    *,
    project_name: str | None = None,
    revision_id: str | None = None,
    dangerously_allow_filesystem: bool = False,
    api_key: str | None = None,
    api_url: str | None = None,
    **kwargs: Any,
) -> None
```

Persist a run to the LangSmith API.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `name` | The name of the run.<br>**TYPE:**`str` |
| `inputs` | The input values for the run.<br>**TYPE:**`Dict[str, Any]` |
| `run_type` | The type of the run, such as tool, chain, llm, retriever,<br>embedding, prompt, or parser.<br>**TYPE:**`str` |
| `project_name` | The project name of the run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `revision_id` | The revision ID of the run.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |
| `api_key` | The API key to use for this specific run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `api_url` | The API URL to use for this specific run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

| RAISES | DESCRIPTION |
| --- | --- |
| `LangSmithUserError` | If the API key is not provided when using the hosted service. |

Examples:

```
.. code-block:: python

    from langsmith import Client
    import datetime
    from uuid import uuid4

    client = Client()

    run_id = uuid4()
    client.create_run(
        id=run_id,
        project_name=project_name,
        name="test_run",
        run_type="llm",
        inputs={"prompt": "hello world"},
        outputs={"generation": "hi there"},
        start_time=datetime.datetime.now(datetime.timezone.utc),
        end_time=datetime.datetime.now(datetime.timezone.utc),
        hide_inputs=True,
        hide_outputs=True,
    )
```

#### ``batch\_ingest\_runs ¶

```
batch_ingest_runs(
    create: Sequence[Run | RunLikeDict | dict] | None = None,
    update: Sequence[Run | RunLikeDict | dict] | None = None,
    *,
    pre_sampled: bool = False,
) -> None
```

Batch ingest/upsert multiple runs in the Langsmith system.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `create` | A sequence of `Run` objects or equivalent dictionaries representing<br>runs to be created / posted.<br>**TYPE:**`Sequence[Run | RunLikeDict] | None`**DEFAULT:**`None` |
| `update` | A sequence of `Run` objects or equivalent dictionaries representing<br>runs that have already been created and should be updated / patched.<br>**TYPE:**`Sequence[Run | RunLikeDict] | None`**DEFAULT:**`None` |
| `pre_sampled` | Whether the runs have already been subject<br>to sampling, and therefore should not be sampled again.<br>Defaults to False.<br>**TYPE:**`bool, default=False`**DEFAULT:**`False` |

| RAISES | DESCRIPTION |
| --- | --- |
| `LangsmithAPIError` | If there is an error in the API request. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

Note

- The run objects MUST contain the dotted\_order and trace\_id fields
to be accepted by the API.

Examples:

```
.. code-block:: python

    from langsmith import Client
    import datetime
    from uuid import uuid4

    client = Client()
    _session = "__test_batch_ingest_runs"
    trace_id = uuid4()
    trace_id_2 = uuid4()
    run_id_2 = uuid4()
    current_time = datetime.datetime.now(datetime.timezone.utc).strftime(
        "%Y%m%dT%H%M%S%fZ"
    )
    later_time = (
        datetime.datetime.now(datetime.timezone.utc) + timedelta(seconds=1)
    ).strftime("%Y%m%dT%H%M%S%fZ")

    runs_to_create = [\
        {\
            "id": str(trace_id),\
            "session_name": _session,\
            "name": "run 1",\
            "run_type": "chain",\
            "dotted_order": f"{current_time}{str(trace_id)}",\
            "trace_id": str(trace_id),\
            "inputs": {"input1": 1, "input2": 2},\
            "outputs": {"output1": 3, "output2": 4},\
        },\
        {\
            "id": str(trace_id_2),\
            "session_name": _session,\
            "name": "run 3",\
            "run_type": "chain",\
            "dotted_order": f"{current_time}{str(trace_id_2)}",\
            "trace_id": str(trace_id_2),\
            "inputs": {"input1": 1, "input2": 2},\
            "error": "error",\
        },\
        {\
            "id": str(run_id_2),\
            "session_name": _session,\
            "name": "run 2",\
            "run_type": "chain",\
            "dotted_order": f"{current_time}{str(trace_id)}."\
            f"{later_time}{str(run_id_2)}",\
            "trace_id": str(trace_id),\
            "parent_run_id": str(trace_id),\
            "inputs": {"input1": 5, "input2": 6},\
        },\
    ]
    runs_to_update = [\
        {\
            "id": str(run_id_2),\
            "dotted_order": f"{current_time}{str(trace_id)}."\
            f"{later_time}{str(run_id_2)}",\
            "trace_id": str(trace_id),\
            "parent_run_id": str(trace_id),\
            "outputs": {"output1": 4, "output2": 5},\
        },\
    ]

    client.batch_ingest_runs(create=runs_to_create, update=runs_to_update)
```

#### ``multipart\_ingest ¶

```
multipart_ingest(
    create: Sequence[Run | RunLikeDict | dict] | None = None,
    update: Sequence[Run | RunLikeDict | dict] | None = None,
    *,
    pre_sampled: bool = False,
    dangerously_allow_filesystem: bool = False,
) -> None
```

Batch ingest/upsert multiple runs in the Langsmith system.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `create` | A sequence of `Run` objects or equivalent dictionaries representing<br>runs to be created / posted.<br>**TYPE:**`Sequence[Run | RunLikeDict] | None`**DEFAULT:**`None` |
| `update` | A sequence of `Run` objects or equivalent dictionaries representing<br>runs that have already been created and should be updated / patched.<br>**TYPE:**`Sequence[Run | RunLikeDict] | None`**DEFAULT:**`None` |
| `pre_sampled` | Whether the runs have already been subject<br>to sampling, and therefore should not be sampled again.<br>Defaults to False.<br>**TYPE:**`bool, default=False`**DEFAULT:**`False` |

| RAISES | DESCRIPTION |
| --- | --- |
| `LangsmithAPIError` | If there is an error in the API request. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

Note

- The run objects MUST contain the dotted\_order and trace\_id fields
to be accepted by the API.

Examples:

```
.. code-block:: python

    from langsmith import Client
    import datetime
    from uuid import uuid4

    client = Client()
    _session = "__test_batch_ingest_runs"
    trace_id = uuid4()
    trace_id_2 = uuid4()
    run_id_2 = uuid4()
    current_time = datetime.datetime.now(datetime.timezone.utc).strftime(
        "%Y%m%dT%H%M%S%fZ"
    )
    later_time = (
        datetime.datetime.now(datetime.timezone.utc) + timedelta(seconds=1)
    ).strftime("%Y%m%dT%H%M%S%fZ")

    runs_to_create = [\
        {\
            "id": str(trace_id),\
            "session_name": _session,\
            "name": "run 1",\
            "run_type": "chain",\
            "dotted_order": f"{current_time}{str(trace_id)}",\
            "trace_id": str(trace_id),\
            "inputs": {"input1": 1, "input2": 2},\
            "outputs": {"output1": 3, "output2": 4},\
        },\
        {\
            "id": str(trace_id_2),\
            "session_name": _session,\
            "name": "run 3",\
            "run_type": "chain",\
            "dotted_order": f"{current_time}{str(trace_id_2)}",\
            "trace_id": str(trace_id_2),\
            "inputs": {"input1": 1, "input2": 2},\
            "error": "error",\
        },\
        {\
            "id": str(run_id_2),\
            "session_name": _session,\
            "name": "run 2",\
            "run_type": "chain",\
            "dotted_order": f"{current_time}{str(trace_id)}."\
            f"{later_time}{str(run_id_2)}",\
            "trace_id": str(trace_id),\
            "parent_run_id": str(trace_id),\
            "inputs": {"input1": 5, "input2": 6},\
        },\
    ]
    runs_to_update = [\
        {\
            "id": str(run_id_2),\
            "dotted_order": f"{current_time}{str(trace_id)}."\
            f"{later_time}{str(run_id_2)}",\
            "trace_id": str(trace_id),\
            "parent_run_id": str(trace_id),\
            "outputs": {"output1": 4, "output2": 5},\
        },\
    ]

    client.multipart_ingest(create=runs_to_create, update=runs_to_update)
```

#### ``update\_run ¶

```
update_run(
    run_id: ID_TYPE,
    *,
    name: str | None = None,
    end_time: datetime | None = None,
    error: str | None = None,
    inputs: dict | None = None,
    outputs: dict | None = None,
    events: Sequence[dict] | None = None,
    extra: dict | None = None,
    tags: list[str] | None = None,
    attachments: Attachments | None = None,
    dangerously_allow_filesystem: bool = False,
    reference_example_id: str | UUID | None = None,
    api_key: str | None = None,
    api_url: str | None = None,
    **kwargs: Any,
) -> None
```

Update a run in the LangSmith API.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `run_id` | The ID of the run to update.<br>**TYPE:**`UUID | str` |
| `name` | The name of the run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `end_time` | The end time of the run.<br>**TYPE:**`datetime | None`**DEFAULT:**`None` |
| `error` | The error message of the run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `inputs` | The input values for the run.<br>**TYPE:**`Dict | None`**DEFAULT:**`None` |
| `outputs` | The output values for the run.<br>**TYPE:**`Dict | None`**DEFAULT:**`None` |
| `events` | The events for the run.<br>**TYPE:**`Sequence[dict] | None`**DEFAULT:**`None` |
| `extra` | The extra information for the run.<br>**TYPE:**`Dict | None`**DEFAULT:**`None` |
| `tags` | The tags for the run.<br>**TYPE:**`List[str] | None`**DEFAULT:**`None` |
| `attachments` | A dictionary of attachments to add to the run. The keys are the attachment names,<br>and the values are Attachment objects containing the data and mime type.<br>**TYPE:**`Dict[str, Attachment] | None`**DEFAULT:**`None` |
| `reference_example_id` | ID of the example<br>that was the source of the run inputs. Used for runs that were part of<br>an experiment.<br>**TYPE:**`str | UUID | None`**DEFAULT:**`None` |
| `api_key` | The API key to use for this specific run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `api_url` | The API URL to use for this specific run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `**kwargs` | Kwargs are ignored.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

Examples:

```
.. code-block:: python

    from langsmith import Client
    import datetime
    from uuid import uuid4

    client = Client()
    project_name = "__test_update_run"

    start_time = datetime.datetime.now()
    revision_id = uuid4()
    run: dict = dict(
        id=uuid4(),
        name="test_run",
        run_type="llm",
        inputs={"text": "hello world"},
        project_name=project_name,
        api_url=os.getenv("LANGCHAIN_ENDPOINT"),
        start_time=start_time,
        extra={"extra": "extra"},
        revision_id=revision_id,
    )
    # Create the run
    client.create_run(**run)
    run["outputs"] = {"output": ["Hi"]}
    run"extra" = "bar"
    run["name"] = "test_run_updated"
    # Update the run
    client.update_run(run["id"], **run)
```

#### ``flush\_compressed\_traces ¶

```
flush_compressed_traces(attempts: int = 3) -> None
```

Force flush the currently buffered compressed runs.

#### ``flush ¶

```
flush() -> None
```

Flush either queue or compressed buffer, depending on mode.

#### ``read\_run ¶

```
read_run(run_id: ID_TYPE, load_child_runs: bool = False) -> Run
```

Read a run from the LangSmith API.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `run_id` | The ID of the run to read.<br>**TYPE:**`UUID | str` |
| `load_child_runs` | Whether to load nested child runs.<br>**TYPE:**`bool, default=False`**DEFAULT:**`False` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Run` | The run read from the LangSmith API.<br>**TYPE:**`Run` |

Examples:

```
.. code-block:: python

    from langsmith import Client

    # Existing run
    run_id = "your-run-id"

    client = Client()
    stored_run = client.read_run(run_id)
```

#### ``list\_runs ¶

```
list_runs(
    *,
    project_id: ID_TYPE | Sequence[ID_TYPE] | None = None,
    project_name: str | Sequence[str] | None = None,
    run_type: str | None = None,
    trace_id: ID_TYPE | None = None,
    reference_example_id: ID_TYPE | None = None,
    query: str | None = None,
    filter: str | None = None,
    trace_filter: str | None = None,
    tree_filter: str | None = None,
    is_root: bool | None = None,
    parent_run_id: ID_TYPE | None = None,
    start_time: datetime | None = None,
    error: bool | None = None,
    run_ids: Sequence[ID_TYPE] | None = None,
    select: Sequence[str] | None = None,
    limit: int | None = None,
    **kwargs: Any,
) -> Iterator[Run]
```

List runs from the LangSmith API.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `project_id` | The ID(s) of the project to filter by.<br>**TYPE:**`UUID | str, Sequence[UUID | str] | None`**DEFAULT:**`None` |
| `project_name` | The name(s) of the project to filter by.<br>**TYPE:**`str | Sequence[str] | None`**DEFAULT:**`None` |
| `run_type` | The type of the runs to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `trace_id` | The ID of the trace to filter by.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |
| `reference_example_id` | The ID of the reference example to filter by.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |
| `query` | The query string to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `filter` | The filter string to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `trace_filter` | Filter to apply to the ROOT run in the trace tree. This is meant to<br>be used in conjunction with the regular `filter` parameter to let you<br>filter runs by attributes of the root run within a trace.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `tree_filter` | Filter to apply to OTHER runs in the trace tree, including<br>sibling and child runs. This is meant to be used in conjunction with<br>the regular `filter` parameter to let you filter runs by attributes<br>of any run within a trace.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `is_root` | Whether to filter by root runs.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `parent_run_id` | The ID of the parent run to filter by.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |
| `start_time` | The start time to filter by.<br>**TYPE:**`datetime | None`**DEFAULT:**`None` |
| `error` | Whether to filter by error status.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `run_ids` | The IDs of the runs to filter by.<br>**TYPE:**`Sequence[UUID | str] | None`**DEFAULT:**`None` |
| `select` | The fields to select.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `limit` | The maximum number of runs to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `Run` | The runs. |

Examples:

.. code-block:: python

```
# List all runs in a project
project_runs = client.list_runs(project_name="<your_project>")

# List LLM and Chat runs in the last 24 hours
todays_llm_runs = client.list_runs(
    project_name="<your_project>",
    start_time=datetime.now() - timedelta(days=1),
    run_type="llm",
)

# List root traces in a project
root_runs = client.list_runs(project_name="<your_project>", is_root=1)

# List runs without errors
correct_runs = client.list_runs(project_name="<your_project>", error=False)

# List runs and only return their inputs/outputs (to speed up the query)
input_output_runs = client.list_runs(
    project_name="<your_project>", select=["inputs", "outputs"]
)

# List runs by run ID
run_ids = [\
    "a36092d2-4ad5-4fb4-9c0d-0dba9a2ed836",\
    "9398e6be-964f-4aa4-8ae9-ad78cd4b7074",\
]
selected_runs = client.list_runs(id=run_ids)

# List all "chain" type runs that took more than 10 seconds and had
# `total_tokens` greater than 5000
chain_runs = client.list_runs(
    project_name="<your_project>",
    filter='and(eq(run_type, "chain"), gt(latency, 10), gt(total_tokens, 5000))',
)

# List all runs called "extractor" whose root of the trace was assigned feedback "user_score" score of 1
good_extractor_runs = client.list_runs(
    project_name="<your_project>",
    filter='eq(name, "extractor")',
    trace_filter='and(eq(feedback_key, "user_score"), eq(feedback_score, 1))',
)

# List all runs that started after a specific timestamp and either have "error" not equal to null or a "Correctness" feedback score equal to 0
complex_runs = client.list_runs(
    project_name="<your_project>",
    filter='and(gt(start_time, "2023-07-15T12:34:56Z"), or(neq(error, null), and(eq(feedback_key, "Correctness"), eq(feedback_score, 0.0))))',
)

# List all runs where `tags` include "experimental" or "beta" and `latency` is greater than 2 seconds
tagged_runs = client.list_runs(
    project_name="<your_project>",
    filter='and(or(has(tags, "experimental"), has(tags, "beta")), gt(latency, 2))',
)
```

#### ``get\_run\_stats ¶

```
get_run_stats(
    *,
    id: list[ID_TYPE] | None = None,
    trace: ID_TYPE | None = None,
    parent_run: ID_TYPE | None = None,
    run_type: str | None = None,
    project_names: list[str] | None = None,
    project_ids: list[ID_TYPE] | None = None,
    reference_example_ids: list[ID_TYPE] | None = None,
    start_time: str | None = None,
    end_time: str | None = None,
    error: bool | None = None,
    query: str | None = None,
    filter: str | None = None,
    trace_filter: str | None = None,
    tree_filter: str | None = None,
    is_root: bool | None = None,
    data_source_type: str | None = None,
) -> dict[str, Any]
```

Get aggregate statistics over queried runs.

Takes in similar query parameters to `list_runs` and returns statistics
based on the runs that match the query.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `id` | List of run IDs to filter by.<br>**TYPE:**`List[UUID | str] | None`**DEFAULT:**`None` |
| `trace` | Trace ID to filter by.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |
| `parent_run` | Parent run ID to filter by.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |
| `run_type` | Run type to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `project_names` | List of project names to filter by.<br>**TYPE:**`List[str] | None`**DEFAULT:**`None` |
| `project_ids` | List of project IDs to filter by.<br>**TYPE:**`List[UUID | str] | None`**DEFAULT:**`None` |
| `reference_example_ids` | List of reference example IDs to filter by.<br>**TYPE:**`List[UUID | str] | None`**DEFAULT:**`None` |
| `start_time` | Start time to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `end_time` | End time to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `error` | Filter by error status.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `query` | Query string to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `filter` | Filter string to apply.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `trace_filter` | Trace filter string to apply.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `tree_filter` | Tree filter string to apply.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `is_root` | Filter by root run status.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `data_source_type` | Data source type to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | Dict\[str, Any\]: A dictionary containing the run statistics. |

#### ``get\_run\_url ¶

```
get_run_url(
    *, run: RunBase, project_name: str | None = None, project_id: ID_TYPE | None = None
) -> str
```

Get the URL for a run.

Not recommended for use within your agent runtime.
More for use interacting with runs after the fact
for data analysis or ETL workloads.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `run` | The run.<br>**TYPE:**`RunBase` |
| `project_name` | The name of the project.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `project_id` | The ID of the project.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | The URL for the run.<br>**TYPE:**`str` |

#### ``share\_run ¶

```
share_run(run_id: ID_TYPE, *, share_id: ID_TYPE | None = None) -> str
```

Get a share link for a run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `run_id` | The ID of the run to share.<br>**TYPE:**`UUID | str` |
| `share_id` | Custom share ID.<br>If not provided, a random UUID will be generated.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | The URL of the shared run.<br>**TYPE:**`str` |

#### ``unshare\_run ¶

```
unshare_run(run_id: ID_TYPE) -> None
```

Delete share link for a run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `run_id` | The ID of the run to unshare.<br>**TYPE:**`UUID | str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

#### ``read\_run\_shared\_link ¶

```
read_run_shared_link(run_id: ID_TYPE) -> str | None
```

Retrieve the shared link for a specific run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `run_id` | The ID of the run.<br>**TYPE:**`UUID | str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str | None` | Optional\[str\]: The shared link for the run, or None if the link is not |
| `str | None` | available. |

#### ``run\_is\_shared ¶

```
run_is_shared(run_id: ID_TYPE) -> bool
```

Get share state for a run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `run_id` | The ID of the run.<br>**TYPE:**`UUID | str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | True if the run is shared, False otherwise.<br>**TYPE:**`bool` |

#### ``read\_shared\_run ¶

```
read_shared_run(share_token: ID_TYPE | str, run_id: ID_TYPE | None = None) -> Run
```

Get shared runs.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `share_token` | The share token or URL of the shared run.<br>**TYPE:**`UUID | str` |
| `run_id` | The ID of the specific run to retrieve.<br>If not provided, the full shared run will be returned.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Run` | The shared run.<br>**TYPE:**`Run` |

#### ``list\_shared\_runs ¶

```
list_shared_runs(
    share_token: ID_TYPE | str, run_ids: list[str] | None = None
) -> Iterator[Run]
```

Get shared runs.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `share_token` | The share token or URL of the shared run.<br>**TYPE:**`UUID | str` |
| `run_ids` | A list of run IDs to filter the results by.<br>**TYPE:**`List[str] | None`**DEFAULT:**`None` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `Run` | A shared run. |

#### ``read\_dataset\_shared\_schema ¶

```
read_dataset_shared_schema(
    dataset_id: ID_TYPE | None = None, *, dataset_name: str | None = None
) -> DatasetShareSchema
```

Retrieve the shared schema of a dataset.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `dataset_id` | The ID of the dataset.<br>Either `dataset_id` or `dataset_name` must be given.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |
| `dataset_name` | The name of the dataset.<br>Either `dataset_id` or `dataset_name` must be given.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `DatasetShareSchema` | ls\_schemas.DatasetShareSchema: The shared schema of the dataset. |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If neither `dataset_id` nor `dataset_name` is given. |

#### ``share\_dataset ¶

```
share_dataset(
    dataset_id: ID_TYPE | None = None, *, dataset_name: str | None = None
) -> DatasetShareSchema
```

Get a share link for a dataset.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `dataset_id` | The ID of the dataset.<br>Either `dataset_id` or `dataset_name` must be given.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |
| `dataset_name` | The name of the dataset.<br>Either `dataset_id` or `dataset_name` must be given.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `DatasetShareSchema` | ls\_schemas.DatasetShareSchema: The shared schema of the dataset. |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If neither `dataset_id` nor `dataset_name` is given. |

#### ``unshare\_dataset ¶

```
unshare_dataset(dataset_id: ID_TYPE) -> None
```

Delete share link for a dataset.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `dataset_id` | The ID of the dataset to unshare.<br>**TYPE:**`UUID | str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

#### ``read\_shared\_dataset ¶

```
read_shared_dataset(share_token: str) -> Dataset
```

Get shared datasets.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `share_token` | The share token or URL of the shared dataset.<br>**TYPE:**`UUID | str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Dataset` | The shared dataset.<br>**TYPE:**`Dataset` |

#### ``list\_shared\_examples ¶

```
list_shared_examples(
    share_token: str,
    *,
    example_ids: list[ID_TYPE] | None = None,
    limit: int | None = None,
) -> Iterator[Example]
```

Get shared examples.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `share_token` | The share token or URL of the shared dataset.<br>**TYPE:**`UUID | str` |
| `example_ids` | The IDs of the examples to filter by. Defaults to None.<br>**TYPE:**`List[UUID, str] | None`**DEFAULT:**`None` |
| `limit` | Maximum number of examples to return, by default None.<br>**TYPE:**`int | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Iterator[Example]` | List\[ls\_schemas.Example\]: The list of shared examples. |

#### ``list\_shared\_projects ¶

```
list_shared_projects(
    *,
    dataset_share_token: str,
    project_ids: list[ID_TYPE] | None = None,
    name: str | None = None,
    name_contains: str | None = None,
    limit: int | None = None,
) -> Iterator[TracerSessionResult]
```

List shared projects.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `dataset_share_token` | The share token of the dataset.<br>**TYPE:**`str` |
| `project_ids` | List of project IDs to filter the results, by default None.<br>**TYPE:**`List[UUID | str] | None`**DEFAULT:**`None` |
| `name` | Name of the project to filter the results, by default None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `name_contains` | Substring to search for in project names, by default None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `limit` | Maximum number of projects to return, by default None.<br>**TYPE:**`int | None`**DEFAULT:**`None` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `TracerSessionResult` | The shared projects. |

#### ``create\_project ¶

```
create_project(
    project_name: str,
    *,
    description: str | None = None,
    metadata: dict | None = None,
    upsert: bool = False,
    project_extra: dict | None = None,
    reference_dataset_id: ID_TYPE | None = None,
) -> TracerSession
```

Create a project on the LangSmith API.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `project_name` | The name of the project.<br>**TYPE:**`str` |
| `project_extra` | Additional project information.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |
| `metadata` | Additional metadata to associate with the project.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |
| `description` | The description of the project.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `upsert` | Whether to update the project if it already exists.<br>**TYPE:**`bool, default=False`**DEFAULT:**`False` |
| `reference_dataset_id` | The ID of the reference dataset to associate with the project.<br>**TYPE:**`Optional[Union[UUID, str]`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `TracerSession` | The created project.<br>**TYPE:**`TracerSession` |\
\
#### ``update\_project ¶\
\
```\
update_project(\
    project_id: ID_TYPE,\
    *,\
    name: str | None = None,\
    description: str | None = None,\
    metadata: dict | None = None,\
    project_extra: dict | None = None,\
    end_time: datetime | None = None,\
) -> TracerSession\
```\
\
Update a LangSmith project.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `project_id` | The ID of the project to update.<br>**TYPE:**`UUID | str` |\
| `name` | The new name to give the project. This is only valid if the project<br>has been assigned an end\_time, meaning it has been completed/closed.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `description` | The new description to give the project.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `metadata` | Additional metadata to associate with the project.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |\
| `project_extra` | Additional project information.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |\
| `end_time` | The time the project was completed.<br>**TYPE:**`datetime | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `TracerSession` | The updated project.<br>**TYPE:**`TracerSession` |\
\
#### ``read\_project ¶\
\
```\
read_project(\
    *,\
    project_id: str | None = None,\
    project_name: str | None = None,\
    include_stats: bool = False,\
) -> TracerSessionResult\
```\
\
Read a project from the LangSmith API.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `project_id` | The ID of the project to read.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `project_name` | The name of the project to read.<br>Only one of project\_id or project\_name may be given.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `include_stats` | Whether to include a project's aggregate statistics in the response.<br>**TYPE:**`bool, default=False`**DEFAULT:**`False` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `TracerSessionResult` | The project.<br>**TYPE:**`TracerSessionResult` |\
\
#### ``has\_project ¶\
\
```\
has_project(project_name: str, *, project_id: str | None = None) -> bool\
```\
\
Check if a project exists.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `project_name` | The name of the project to check for.<br>**TYPE:**`str` |\
| `project_id` | The ID of the project to check for.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `bool` | Whether the project exists.<br>**TYPE:**`bool` |\
\
#### ``get\_test\_results ¶\
\
```\
get_test_results(\
    *, project_id: ID_TYPE | None = None, project_name: str | None = None\
) -> DataFrame\
```\
\
Read the record-level information from an experiment into a Pandas DF.\
\
Note: this will fetch whatever data exists in the DB. Results are not\
immediately available in the DB upon evaluation run completion.\
\
Feedback score values will be returned as an average across all runs for\
the experiment. Note that non-numeric feedback scores will be omitted.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `project_id` | The ID of the project.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `project_name` | The name of the project.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `DataFrame` | pd.DataFrame: A dataframe containing the test results. |\
\
#### ``list\_projects ¶\
\
```\
list_projects(\
    project_ids: list[ID_TYPE] | None = None,\
    name: str | None = None,\
    name_contains: str | None = None,\
    reference_dataset_id: ID_TYPE | None = None,\
    reference_dataset_name: str | None = None,\
    reference_free: bool | None = None,\
    include_stats: bool | None = None,\
    dataset_version: str | None = None,\
    limit: int | None = None,\
    metadata: dict[str, Any] | None = None,\
) -> Iterator[TracerSessionResult]\
```\
\
List projects from the LangSmith API.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `project_ids` | A list of project IDs to filter by, by default None<br>**TYPE:**`List[UUID | str] | None`**DEFAULT:**`None` |\
| `name` | The name of the project to filter by, by default None<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `name_contains` | A string to search for in the project name, by default None<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `reference_dataset_id` | A dataset ID to filter by, by default None<br>**TYPE:**`List[UUID | str] | None`**DEFAULT:**`None` |\
| `reference_dataset_name` | The name of the reference dataset to filter by, by default None<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `reference_free` | Whether to filter for only projects not associated with a dataset.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |\
| `limit` | The maximum number of projects to return, by default None<br>**TYPE:**`int | None`**DEFAULT:**`None` |\
| `metadata` | Metadata to filter by.<br>**TYPE:**`Dict[str, Any] | None`**DEFAULT:**`None` |\
\
| YIELDS | DESCRIPTION |\
| --- | --- |\
| `TracerSessionResult` | The projects. |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `ValueError` | If both reference\_dataset\_id and reference\_dataset\_name are given. |\
\
#### ``delete\_project ¶\
\
```\
delete_project(\
    *, project_name: str | None = None, project_id: str | None = None\
) -> None\
```\
\
Delete a project from LangSmith.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `project_name` | The name of the project to delete.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `project_id` | The ID of the project to delete.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `None` | None |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `ValueError` | If neither project\_name or project\_id is provided. |\
\
#### ``create\_dataset ¶\
\
```\
create_dataset(\
    dataset_name: str,\
    *,\
    description: str | None = None,\
    data_type: DataType = kv,\
    inputs_schema: dict[str, Any] | None = None,\
    outputs_schema: dict[str, Any] | None = None,\
    transformations: list[DatasetTransformation] | None = None,\
    metadata: dict | None = None,\
) -> Dataset\
```\
\
Create a dataset in the LangSmith API.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_name` | The name of the dataset.<br>**TYPE:**`str` |\
| `description` | The description of the dataset.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `data_type` | The data type of the dataset.<br>**TYPE:**`DataType, default=DataType.kv`**DEFAULT:**`kv` |\
| `inputs_schema` | The schema definition for the inputs of the dataset.<br>**TYPE:**`Dict[str, Any] | None`**DEFAULT:**`None` |\
| `outputs_schema` | The schema definition for the outputs of the dataset.<br>**TYPE:**`Dict[str, Any] | None`**DEFAULT:**`None` |\
| `transformations` | A list of transformations to apply to the dataset.<br>**TYPE:**`List[DatasetTransformation] | None`**DEFAULT:**`None` |\
| `metadata` | Additional metadata to associate with the dataset.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Dataset` | The created dataset.<br>**TYPE:**`Dataset` |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `HTTPError` | If the request to create the dataset fails. |\
\
#### ``has\_dataset ¶\
\
```\
has_dataset(\
    *, dataset_name: str | None = None, dataset_id: ID_TYPE | None = None\
) -> bool\
```\
\
Check whether a dataset exists in your tenant.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_name` | The name of the dataset to check.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `dataset_id` | The ID of the dataset to check.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `bool` | Whether the dataset exists.<br>**TYPE:**`bool` |\
\
#### ``read\_dataset ¶\
\
```\
read_dataset(\
    *, dataset_name: str | None = None, dataset_id: ID_TYPE | None = None\
) -> Dataset\
```\
\
Read a dataset from the LangSmith API.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_name` | The name of the dataset to read.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `dataset_id` | The ID of the dataset to read.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Dataset` | The dataset.<br>**TYPE:**`Dataset` |\
\
#### ``diff\_dataset\_versions ¶\
\
```\
diff_dataset_versions(\
    dataset_id: ID_TYPE | None = None,\
    *,\
    dataset_name: str | None = None,\
    from_version: str | datetime,\
    to_version: str | datetime,\
) -> DatasetDiffInfo\
```\
\
Get the difference between two versions of a dataset.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_id` | The ID of the dataset.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `from_version` | The starting version for the diff.<br>**TYPE:**`str | datetime` |\
| `to_version` | The ending version for the diff.<br>**TYPE:**`str | datetime` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `DatasetDiffInfo` | The difference between the two versions of the dataset.<br>**TYPE:**`DatasetDiffInfo` |\
\
Examples:\
\
.. code-block:: python\
\
```\
# Get the difference between two tagged versions of a dataset\
from_version = "prod"\
to_version = "dev"\
diff = client.diff_dataset_versions(\
    dataset_name="my-dataset",\
    from_version=from_version,\
    to_version=to_version,\
)\
\
# Get the difference between two timestamped versions of a dataset\
from_version = datetime.datetime(2024, 1, 1)\
to_version = datetime.datetime(2024, 2, 1)\
diff = client.diff_dataset_versions(\
    dataset_name="my-dataset",\
    from_version=from_version,\
    to_version=to_version,\
)\
```\
\
#### ``read\_dataset\_openai\_finetuning ¶\
\
```\
read_dataset_openai_finetuning(\
    dataset_id: ID_TYPE | None = None, *, dataset_name: str | None = None\
) -> list\
```\
\
Download a dataset in OpenAI Jsonl format and load it as a list of dicts.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_id` | The ID of the dataset to download.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset to download.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `list` | list\[dict\]: The dataset loaded as a list of dicts. |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `ValueError` | If neither dataset\_id nor dataset\_name is provided. |\
\
#### ``list\_datasets ¶\
\
```\
list_datasets(\
    *,\
    dataset_ids: list[ID_TYPE] | None = None,\
    data_type: str | None = None,\
    dataset_name: str | None = None,\
    dataset_name_contains: str | None = None,\
    metadata: dict[str, Any] | None = None,\
    limit: int | None = None,\
) -> Iterator[Dataset]\
```\
\
List the datasets on the LangSmith API.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_ids` | A list of dataset IDs to filter the results by.<br>**TYPE:**`List[UUID | str] | None`**DEFAULT:**`None` |\
| `data_type` | The data type of the datasets to filter the results by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset to filter the results by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `dataset_name_contains` | A substring to search for in the dataset names.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `metadata` | A dictionary of metadata to filter the results by.<br>**TYPE:**`Dict[str, Any] | None`**DEFAULT:**`None` |\
| `limit` | The maximum number of datasets to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |\
\
| YIELDS | DESCRIPTION |\
| --- | --- |\
| `Dataset` | The datasets. |\
\
#### ``delete\_dataset ¶\
\
```\
delete_dataset(\
    *, dataset_id: ID_TYPE | None = None, dataset_name: str | None = None\
) -> None\
```\
\
Delete a dataset from the LangSmith API.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_id` | The ID of the dataset to delete.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset to delete.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `None` | None |\
\
#### ``update\_dataset\_tag ¶\
\
```\
update_dataset_tag(\
    *,\
    dataset_id: ID_TYPE | None = None,\
    dataset_name: str | None = None,\
    as_of: datetime,\
    tag: str,\
) -> None\
```\
\
Update the tags of a dataset.\
\
If the tag is already assigned to a different version of this dataset,\
the tag will be moved to the new version. The as\_of parameter is used to\
determine which version of the dataset to apply the new tags to.\
It must be an exact version of the dataset to succeed. You can\
use the read\_dataset\_version method to find the exact version\
to apply the tags to.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_id` | The ID of the dataset to update.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset to update.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `as_of` | The timestamp of the dataset to apply the new tags to.<br>**TYPE:**`datetime` |\
| `tag` | The new tag to apply to the dataset.<br>**TYPE:**`str` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `None` | None |\
\
Examples:\
\
.. code-block:: python\
\
```\
dataset_name = "my-dataset"\
# Get the version of a dataset <= a given timestamp\
dataset_version = client.read_dataset_version(\
    dataset_name=dataset_name, as_of=datetime.datetime(2024, 1, 1)\
)\
# Assign that version a new tag\
client.update_dataset_tags(\
    dataset_name="my-dataset",\
    as_of=dataset_version.as_of,\
    tag="prod",\
)\
```\
\
#### ``list\_dataset\_versions ¶\
\
```\
list_dataset_versions(\
    *,\
    dataset_id: ID_TYPE | None = None,\
    dataset_name: str | None = None,\
    search: str | None = None,\
    limit: int | None = None,\
) -> Iterator[DatasetVersion]\
```\
\
List dataset versions.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_id` | The ID of the dataset.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `search` | The search query.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `limit` | The maximum number of versions to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |\
\
| YIELDS | DESCRIPTION |\
| --- | --- |\
| `DatasetVersion` | The dataset versions. |\
\
#### ``read\_dataset\_version ¶\
\
```\
read_dataset_version(\
    *,\
    dataset_id: ID_TYPE | None = None,\
    dataset_name: str | None = None,\
    as_of: datetime | None = None,\
    tag: str | None = None,\
) -> DatasetVersion\
```\
\
Get dataset version by as\_of or exact tag.\
\
Ues this to resolve the nearest version to a given timestamp or for a given tag.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_id` | The ID of the dataset.<br>**TYPE:**`ID_TYPE | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `as_of` | The timestamp of the dataset<br>to retrieve.<br>**TYPE:**`datetime | None`**DEFAULT:**`None` |\
| `tag` | The tag of the dataset to retrieve.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `DatasetVersion` | The dataset version.<br>**TYPE:**`DatasetVersion` |\
\
Examples:\
\
.. code-block:: python\
\
```\
# Get the latest version of a dataset\
client.read_dataset_version(dataset_name="my-dataset", tag="latest")\
\
# Get the version of a dataset <= a given timestamp\
client.read_dataset_version(\
    dataset_name="my-dataset",\
    as_of=datetime.datetime(2024, 1, 1),\
)\
\
# Get the version of a dataset with a specific tag\
client.read_dataset_version(dataset_name="my-dataset", tag="prod")\
```\
\
#### ``clone\_public\_dataset ¶\
\
```\
clone_public_dataset(\
    token_or_url: str,\
    *,\
    source_api_url: str | None = None,\
    dataset_name: str | None = None,\
) -> Dataset\
```\
\
Clone a public dataset to your own langsmith tenant.\
\
This operation is idempotent. If you already have a dataset with the given name,\
this function will do nothing.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `token_or_url` | The token of the public dataset to clone.<br>**TYPE:**`str` |\
| `source_api_url` | The URL of the langsmith server where the data is hosted.<br>Defaults to the API URL of your current client.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset to create in your tenant.<br>Defaults to the name of the public dataset.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Dataset` | The cloned dataset.<br>**TYPE:**`Dataset` |\
\
#### ``create\_llm\_example ¶\
\
```\
create_llm_example(\
    prompt: str,\
    generation: str | None = None,\
    dataset_id: ID_TYPE | None = None,\
    dataset_name: str | None = None,\
    created_at: datetime | None = None,\
) -> Example\
```\
\
Add an example (row) to an LLM-type dataset.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `prompt` | The input prompt for the example.<br>**TYPE:**`str` |\
| `generation` | The output generation for the example.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `dataset_id` | The ID of the dataset.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `created_at` | The creation timestamp of the example.<br>**TYPE:**`datetime | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Example` | The created example<br>**TYPE:**`Example` |\
\
#### ``create\_chat\_example ¶\
\
```\
create_chat_example(\
    messages: list[Mapping[str, Any] | BaseMessageLike],\
    generations: Mapping[str, Any] | BaseMessageLike | None = None,\
    dataset_id: ID_TYPE | None = None,\
    dataset_name: str | None = None,\
    created_at: datetime | None = None,\
) -> Example\
```\
\
Add an example (row) to a Chat-type dataset.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `messages` | The input messages for the example.<br>**TYPE:**`List[Mapping[str, Any] | BaseMessageLike]` |\
| `generations` | The output messages for the example.<br>**TYPE:**`Mapping[str, Any] | BaseMessageLike | None`**DEFAULT:**`None` |\
| `dataset_id` | The ID of the dataset.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `created_at` | The creation timestamp of the example.<br>**TYPE:**`datetime | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Example` | The created example<br>**TYPE:**`Example` |\
\
#### ``create\_example\_from\_run ¶\
\
```\
create_example_from_run(\
    run: Run,\
    dataset_id: ID_TYPE | None = None,\
    dataset_name: str | None = None,\
    created_at: datetime | None = None,\
) -> Example\
```\
\
Add an example (row) to a dataset from a run.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `run` | The run to create an example from.<br>**TYPE:**`Run` |\
| `dataset_id` | The ID of the dataset.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `created_at` | The creation timestamp of the example.<br>**TYPE:**`datetime | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Example` | The created example<br>**TYPE:**`Example` |\
\
#### ``update\_examples\_multipart ¶\
\
```\
update_examples_multipart(\
    *,\
    dataset_id: ID_TYPE,\
    updates: list[ExampleUpdate] | None = None,\
    dangerously_allow_filesystem: bool = False,\
) -> UpsertExamplesResponse\
```\
\
Update examples using multipart.\
\
.. deprecated:: 0.3.9\
\
```\
Use Client.update_examples instead. Will be removed in 0.4.0.\
```\
\
#### ``upload\_examples\_multipart ¶\
\
```\
upload_examples_multipart(\
    *,\
    dataset_id: ID_TYPE,\
    uploads: list[ExampleCreate] | None = None,\
    dangerously_allow_filesystem: bool = False,\
) -> UpsertExamplesResponse\
```\
\
Upload examples using multipart.\
\
.. deprecated:: 0.3.9\
\
```\
Use Client.create_examples instead. Will be removed in 0.4.0.\
```\
\
#### ``upsert\_examples\_multipart ¶\
\
```\
upsert_examples_multipart(\
    *,\
    upserts: list[ExampleUpsertWithAttachments] | None = None,\
    dangerously_allow_filesystem: bool = False,\
) -> UpsertExamplesResponse\
```\
\
Upsert examples.\
\
.. deprecated:: 0.3.9\
\
```\
Use Client.create_examples and Client.update_examples instead. Will be\
removed in 0.4.0.\
```\
\
#### ``create\_examples ¶\
\
```\
create_examples(\
    *,\
    dataset_name: str | None = None,\
    dataset_id: ID_TYPE | None = None,\
    examples: Sequence[ExampleCreate | dict] | None = None,\
    dangerously_allow_filesystem: bool = False,\
    max_concurrency: Annotated[int, Field(ge=1, le=3)] = 1,\
    **kwargs: Any,\
) -> UpsertExamplesResponse | dict[str, Any]\
```\
\
Create examples in a dataset.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_name` | The name of the dataset to create the examples in. Must specify exactly<br>one of dataset\_name or dataset\_id.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `dataset_id` | The ID of the dataset to create the examples in. Must specify exactly<br>one of dataset\_name or dataset\_id<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `examples` | The examples to create.<br>**TYPE:**`Sequence[ExampleCreate | dict]`**DEFAULT:**`None` |\
| `dangerously_allow_filesystem` | Whether to allow uploading files from the filesystem.<br>**TYPE:**`bool`**DEFAULT:**`False` |\
| `**kwargs` | Legacy keyword args. Should not be specified if 'examples' is specified.<br>- inputs (Sequence\[Mapping\[str, Any\]\]): The input values for the examples.<br>- outputs (Optional\[Sequence\[Optional\[Mapping\[str, Any\]\]\]\]): The output values for the examples.<br>- metadata (Optional\[Sequence\[Optional\[Mapping\[str, Any\]\]\]\]): The metadata for the examples.<br>- splits (Optional\[Sequence\[Optional\[str \| List\[str\]\]\]\]): The splits for the examples, which are divisions of your dataset such as 'train', 'test', or 'validation'.<br>- source\_run\_ids (Optional\[Sequence\[Optional\[Union\[UUID, str\]\]\]\]): The IDs of the source runs associated with the examples.<br>- ids (Optional\[Sequence\[Union\[UUID, str\]\]\]): The IDs of the examples.<br>**TYPE:**`Any`**DEFAULT:**`{}` |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `ValueError` | If 'examples' and legacy args are both provided. |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `UpsertExamplesResponse | dict[str, Any]` | The LangSmith JSON response. Includes 'count' and 'example\_ids'. |\
\
.. versionchanged:: 0.3.11\
\
```\
Updated to take argument 'examples', a single list where each\
element is the full example to create. This should be used instead of the\
legacy 'inputs', 'outputs', etc. arguments which split each examples\
attributes across arguments.\
\
Updated to support creating examples with attachments.\
```\
\
Example\
\
.. code-block:: python\
\
```\
from langsmith import Client\
\
client = Client()\
\
dataset = client.create_dataset("agent-qa")\
\
examples = [\
    {\
        "inputs": {"question": "what's an agent"},\
        "outputs": {"answer": "an agent is..."},\
        "metadata": {"difficulty": "easy"},\
    },\
    {\
        "inputs": {\
            "question": "can you explain the agent architecture in this diagram?"\
        },\
        "outputs": {"answer": "this diagram shows..."},\
        "attachments": {"diagram": {"mime_type": "image/png", "data": b"..."}},\
        "metadata": {"difficulty": "medium"},\
    },\
    # more examples...\
]\
\
response = client.create_examples(dataset_name="agent-qa", examples=examples)\
# -> {"example_ids": ...\
```\
\
#### ``create\_example [¶\
\
```\
create_example(\
    inputs: Mapping[str, Any] | None = None,\
    dataset_id: ID_TYPE | None = None,\
    dataset_name: str | None = None,\
    created_at: datetime | None = None,\
    outputs: Mapping[str, Any] | None = None,\
    metadata: Mapping[str, Any] | None = None,\
    split: str | list[str] | None = None,\
    example_id: ID_TYPE | None = None,\
    source_run_id: ID_TYPE | None = None,\
    use_source_run_io: bool = False,\
    use_source_run_attachments: list[str] | None = None,\
    attachments: Attachments | None = None,\
) -> Example\
```\
\
Create a dataset example in the LangSmith API.\
\
Examples are rows in a dataset, containing the inputs\
and expected outputs (or other reference information)\
for a model or chain.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `inputs` | The input values for the example.<br>**TYPE:**`Mapping[str, Any]`**DEFAULT:**`None` |\
| `dataset_id` | The ID of the dataset to create the example in.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset to create the example in.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `created_at` | The creation timestamp of the example.<br>**TYPE:**`datetime | None`**DEFAULT:**`None` |\
| `outputs` | The output values for the example.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |\
| `metadata` | The metadata for the example.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |\
| `split` | The splits for the example, which are divisions<br>of your dataset such as 'train', 'test', or 'validation'.<br>**TYPE:**`str | List[str] | None`**DEFAULT:**`None` |\
| `example_id` | The ID of the example to create. If not provided, a new<br>example will be created.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `source_run_id` | The ID of the source run associated with this example.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `use_source_run_io` | Whether to use the inputs, outputs, and attachments from the source run.<br>**TYPE:**`bool`**DEFAULT:**`False` |\
| `use_source_run_attachments` | Which attachments to use from the source run. If use\_source\_run\_io<br>is True, all attachments will be used regardless of this param.<br>**TYPE:**`List[str] | None`**DEFAULT:**`None` |\
| `attachments` | The attachments for the example.<br>**TYPE:**`Attachments | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Example` | The created example.<br>**TYPE:**`Example` |\
\
#### ``read\_example ¶\
\
```\
read_example(example_id: ID_TYPE, *, as_of: datetime | None = None) -> Example\
```\
\
Read an example from the LangSmith API.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `example_id` | The ID of the example to read.<br>**TYPE:**`UUID | str` |\
| `as_of` | The dataset version tag OR<br>timestamp to retrieve the example as of.<br>Response examples will only be those that were present at the time<br>of the tagged (or timestamped) version.<br>**TYPE:**`datetime | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Example` | The example.<br>**TYPE:**`Example` |\
\
#### ``list\_examples ¶\
\
```\
list_examples(\
    dataset_id: ID_TYPE | None = None,\
    dataset_name: str | None = None,\
    example_ids: Sequence[ID_TYPE] | None = None,\
    as_of: datetime | str | None = None,\
    splits: Sequence[str] | None = None,\
    inline_s3_urls: bool = True,\
    *,\
    offset: int = 0,\
    limit: int | None = None,\
    metadata: dict | None = None,\
    filter: str | None = None,\
    include_attachments: bool = False,\
    **kwargs: Any,\
) -> Iterator[Example]\
```\
\
Retrieve the example rows of the specified dataset.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_id` | The ID of the dataset to filter by.<br>Defaults to None.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset to filter by.<br>Defaults to None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `example_ids` | The IDs of the examples to filter by.<br>Defaults to None.<br>**TYPE:**`Optional[Sequence[Union[UUID, str]]`**DEFAULT:**`None` |\
| `as_of` | The dataset version tag OR<br>timestamp to retrieve the examples as of.<br>Response examples will only be those that were present at the time<br>of the tagged (or timestamped) version.<br>**TYPE:**`datetime | str | None`**DEFAULT:**`None` |\
| `splits` | A list of dataset splits, which are<br>divisions of your dataset such as 'train', 'test', or 'validation'.<br>Returns examples only from the specified splits.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |\
| `inline_s3_urls` | Whether to inline S3 URLs.<br>Defaults to True.<br>**TYPE:**`bool, default=True`**DEFAULT:**`True` |\
| `offset` | The offset to start from. Defaults to 0.<br>**TYPE:**`int, default=0`**DEFAULT:**`0` |\
| `limit` | The maximum number of examples to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |\
| `metadata` | A dictionary of metadata to filter by.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |\
| `filter` | A structured filter string to apply to<br>the examples.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `include_attachments` | Whether to include the<br>attachments in the response. Defaults to False.<br>**TYPE:**`bool, default=False`**DEFAULT:**`False` |\
| `**kwargs` | Additional keyword arguments are ignored.<br>**TYPE:**`Any`**DEFAULT:**`{}` |\
\
| YIELDS | DESCRIPTION |\
| --- | --- |\
| `Example` | The examples. |\
\
Examples:\
\
List all examples for a dataset:\
\
.. code-block:: python\
\
```\
from langsmith import Client\
\
client = Client()\
\
# By Dataset ID\
examples = client.list_examples(\
    dataset_id="c9ace0d8-a82c-4b6c-13d2-83401d68e9ab"\
)\
# By Dataset Name\
examples = client.list_examples(dataset_name="My Test Dataset")\
```\
\
List examples by id\
\
.. code-block:: python\
\
```\
example_ids = [\
    "734fc6a0-c187-4266-9721-90b7a025751a",\
    "d6b4c1b9-6160-4d63-9b61-b034c585074f",\
    "4d31df4e-f9c3-4a6e-8b6c-65701c2fed13",\
]\
examples = client.list_examples(example_ids=example_ids)\
```\
\
List examples by metadata\
\
.. code-block:: python\
\
```\
examples = client.list_examples(\
    dataset_name=dataset_name, metadata={"foo": "bar"}\
)\
```\
\
List examples by structured filter\
\
.. code-block:: python\
\
```\
examples = client.list_examples(\
    dataset_name=dataset_name,\
    filter='and(not(has(metadata, \'{"foo": "bar"}\')), exists(metadata, "tenant_id"))',\
)\
```\
\
#### ``index\_dataset ¶\
\
```\
index_dataset(*, dataset_id: ID_TYPE, tag: str = 'latest', **kwargs: Any) -> None\
```\
\
Enable dataset indexing. Examples are indexed by their inputs.\
\
This enables searching for similar examples by inputs with\
`client.similar_examples()`.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_id` | The ID of the dataset to index.<br>**TYPE:**`UUID | str` |\
| `tag` | The version of the dataset to index. If 'latest'<br>then any updates to the dataset (additions, updates, deletions of<br>examples) will be reflected in the index.<br>**TYPE:**`str | None`**DEFAULT:**`'latest'` |\
| `**kwargs` | Additional keyword arguments to pass as part of request body.<br>**TYPE:**`Any`**DEFAULT:**`{}` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `None` | None |\
\
#### ``sync\_indexed\_dataset ¶\
\
```\
sync_indexed_dataset(*, dataset_id: ID_TYPE, **kwargs: Any) -> None\
```\
\
Sync dataset index. This already happens automatically every 5 minutes, but you can call this to force a sync.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_id` | The ID of the dataset to sync.<br>**TYPE:**`UUID | str` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `None` | None |\
\
#### ``similar\_examples ¶\
\
```\
similar_examples(\
    inputs: dict,\
    /,\
    *,\
    limit: int,\
    dataset_id: ID_TYPE,\
    filter: str | None = None,\
    **kwargs: Any,\
) -> list[ExampleSearch]\
```\
\
Retrieve the dataset examples whose inputs best match the current inputs.\
\
**Note**: Must have few-shot indexing enabled for the dataset. See\
`client.index_dataset()`.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `inputs` | The inputs to use as a search query. Must match the dataset<br>input schema. Must be JSON serializable.<br>**TYPE:**`dict` |\
| `limit` | The maximum number of examples to return.<br>**TYPE:**`int` |\
| `dataset_id` | The ID of the dataset to search over.<br>**TYPE:**`UUID | str` |\
| `filter` | A filter string to apply to the search results. Uses<br>the same syntax as the `filter` parameter in `list_runs()`. Only a subset<br>of operations are supported. Defaults to None.<br>For example, you can use `and(eq(metadata.some_tag, 'some_value'), neq(metadata.env, 'dev'))`<br>to filter only examples where some\_tag has some\_value, and the environment is not dev.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `**kwargs` | Additional keyword arguments to pass as part of request body.<br>**TYPE:**`Any`**DEFAULT:**`{}` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `list[ExampleSearch]` | list\[ExampleSearch\]: List of ExampleSearch objects. |\
\
Examples:\
\
.. code-block:: python\
\
```\
from langsmith import Client\
\
client = Client()\
client.similar_examples(\
    {"question": "When would i use the runnable generator"},\
    limit=3,\
    dataset_id="...",\
)\
```\
\
.. code-block:: python\
\
```\
[\
    ExampleSearch(\
        inputs={\
            "question": "How do I cache a Chat model? What caches can I use?"\
        },\
        outputs={\
            "answer": "You can use LangChain's caching layer for Chat Models. This can save you money by reducing the number of API calls you make to the LLM provider, if you're often requesting the same completion multiple times, and speed up your application.\n\nfrom langchain.cache import InMemoryCache\nlangchain.llm_cache = InMemoryCache()\n\n# The first time, it is not yet in cache, so it should take longer\nllm.predict('Tell me a joke')\n\nYou can also use SQLite Cache which uses a SQLite database:\n\nrm .langchain.db\n\nfrom langchain.cache import SQLiteCache\nlangchain.llm_cache = SQLiteCache(database_path=\".langchain.db\")\n\n# The first time, it is not yet in cache, so it should take longer\nllm.predict('Tell me a joke') \n"\
        },\
        metadata=None,\
        id=UUID("b2ddd1c4-dff6-49ae-8544-f48e39053398"),\
        dataset_id=UUID("01b6ce0f-bfb6-4f48-bbb8-f19272135d40"),\
    ),\
    ExampleSearch(\
        inputs={"question": "What's a runnable lambda?"},\
        outputs={\
            "answer": "A runnable lambda is an object that implements LangChain's `Runnable` interface and runs a callbale (i.e., a function). Note the function must accept a single argument."\
        },\
        metadata=None,\
        id=UUID("f94104a7-2434-4ba7-8293-6a283f4860b4"),\
        dataset_id=UUID("01b6ce0f-bfb6-4f48-bbb8-f19272135d40"),\
    ),\
    ExampleSearch(\
        inputs={"question": "Show me how to use RecursiveURLLoader"},\
        outputs={\
            "answer": 'The RecursiveURLLoader comes from the langchain.document_loaders.recursive_url_loader module. Here\'s an example of how to use it:\n\nfrom langchain.document_loaders.recursive_url_loader import RecursiveUrlLoader\n\n# Create an instance of RecursiveUrlLoader with the URL you want to load\nloader = RecursiveUrlLoader(url="https://example.com")\n\n# Load all child links from the URL page\nchild_links = loader.load()\n\n# Print the child links\nfor link in child_links:\n    print(link)\n\nMake sure to replace "https://example.com" with the actual URL you want to load. The load() method returns a list of child links found on the URL page. You can iterate over this list to access each child link.'\
        },\
        metadata=None,\
        id=UUID("0308ea70-a803-4181-a37d-39e95f138f8c"),\
        dataset_id=UUID("01b6ce0f-bfb6-4f48-bbb8-f19272135d40"),\
    ),\
]\
```\
\
#### ``update\_example ¶\
\
```\
update_example(\
    example_id: ID_TYPE,\
    *,\
    inputs: dict[str, Any] | None = None,\
    outputs: Mapping[str, Any] | None = None,\
    metadata: dict | None = None,\
    split: str | list[str] | None = None,\
    dataset_id: ID_TYPE | None = None,\
    attachments_operations: AttachmentsOperations | None = None,\
    attachments: Attachments | None = None,\
) -> dict[str, Any]\
```\
\
Update a specific example.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `example_id` | The ID of the example to update.<br>**TYPE:**`UUID | str` |\
| `inputs` | The input values to update.<br>**TYPE:**`Dict[str, Any] | None`**DEFAULT:**`None` |\
| `outputs` | The output values to update.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |\
| `metadata` | The metadata to update.<br>**TYPE:**`Dict | None`**DEFAULT:**`None` |\
| `split` | The dataset split to update, such as<br>'train', 'test', or 'validation'.<br>**TYPE:**`str | List[str] | None`**DEFAULT:**`None` |\
| `dataset_id` | The ID of the dataset to update.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `attachments_operations` | The attachments operations to perform.<br>**TYPE:**`AttachmentsOperations | None`**DEFAULT:**`None` |\
| `attachments` | The attachments to add to the example.<br>**TYPE:**`Attachments | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `dict[str, Any]` | Dict\[str, Any\]: The updated example. |\
\
#### ``update\_examples ¶\
\
```\
update_examples(\
    *,\
    dataset_name: str | None = None,\
    dataset_id: ID_TYPE | None = None,\
    updates: Sequence[ExampleUpdate | dict] | None = None,\
    dangerously_allow_filesystem: bool = False,\
    **kwargs: Any,\
) -> dict[str, Any]\
```\
\
Update multiple examples.\
\
Examples are expected to all be part of the same dataset.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_name` | The name of the dataset to update. Should specify exactly one of<br>'dataset\_name' or 'dataset\_id'.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `dataset_id` | The ID of the dataset to update. Should specify exactly one of<br>'dataset\_name' or 'dataset\_id'.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `updates` | The example updates. Overwrites any specified fields and does not<br>update any unspecified fields.<br>**TYPE:**`Sequence[ExampleUpdate | dict] | None`**DEFAULT:**`None` |\
| `dangerously_allow_filesystem` | Whether to allow using filesystem paths as attachments.<br>**TYPE:**`bool`**DEFAULT:**`False` |\
| `**kwargs` | Legacy keyword args. Should not be specified if 'updates' is specified.<br>- example\_ids (Sequence\[UUID \| str\]): The IDs of the examples to update.<br>- inputs (Sequence\[dict \| None\] \| None): The input values for the examples.<br>- outputs (Sequence\[dict \| None\] \| None): The output values for the examples.<br>- metadata (Sequence\[dict \| None\] \| None): The metadata for the examples.<br>- splits (Sequence\[str \| list\[str\] \| None\] \| None): The splits for the examples, which are divisions of your dataset such as 'train', 'test', or 'validation'.<br>- attachments\_operations (Sequence\[AttachmentsOperations \| None\] \| None): The operations to perform on the attachments.<br>- dataset\_ids (Sequence\[UUID \| str\] \| None): The IDs of the datasets to move the examples to.<br>**TYPE:**`Any`**DEFAULT:**`{}` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `dict[str, Any]` | The LangSmith JSON response. Includes 'message', 'count', and 'example\_ids'. |\
\
.. versionchanged:: 0.3.9\
\
```\
 Updated to ...\
```\
\
Example:\
\
```\
 .. code-block:: python\
\
     from langsmith import Client\
\
     client = Client()\
\
     dataset = client.create_dataset("agent-qa")\
\
     examples = [\
         {\
             "inputs": {"question": "what's an agent"},\
             "outputs": {"answer": "an agent is..."},\
             "metadata": {"difficulty": "easy"},\
         },\
         {\
             "inputs": {\
                 "question": "can you explain the agent architecture in this diagram?"\
             },\
             "outputs": {"answer": "this diagram shows..."},\
             "attachments": {"diagram": {"mime_type": "image/png", "data": b"..."}},\
             "metadata": {"difficulty": "medium"},\
         },\
         # more examples...\
     ]\
\
     response = client.create_examples(dataset_name="agent-qa", examples=examples)\
     example_ids = response["example_ids"]\
\
     updates = [\
         {\
             "id": example_ids[0],\
             "inputs": {"question": "what isn't an agent"},\
             "outputs": {"answer": "an agent is not..."},\
         },\
         {\
             "id": example_ids[1],\
             "attachments_operations": [\
                 {"rename": {"diagram": "agent_diagram"}, "retain": []}\
             ],\
         },\
     ]\
     response = client.update_examples(dataset_name="agent-qa", updates=updates)\
     # -> {"example_ids": ...\
```\
\
#### ``delete\_example [¶\
\
```\
delete_example(example_id: ID_TYPE) -> None\
```\
\
Delete an example by ID.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `example_id` | The ID of the example to delete.<br>**TYPE:**`UUID | str` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `None` | None |\
\
#### ``delete\_examples ¶\
\
```\
delete_examples(example_ids: Sequence[ID_TYPE]) -> None\
```\
\
Delete multiple examples by ID.\
\
###### Parameters ¶\
\
example\_ids : Sequence\[ID\_TYPE\]\
The IDs of the examples to delete.\
\
#### ``list\_dataset\_splits ¶\
\
```\
list_dataset_splits(\
    *,\
    dataset_id: ID_TYPE | None = None,\
    dataset_name: str | None = None,\
    as_of: str | datetime | None = None,\
) -> list[str]\
```\
\
Get the splits for a dataset.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_id` | The ID of the dataset.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `as_of` | The version<br>of the dataset to retrieve splits for. Can be a timestamp or a<br>string tag. Defaults to "latest".<br>**TYPE:**`str | datetime | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `list[str]` | List\[str\]: The names of this dataset's splits. |\
\
#### ``update\_dataset\_splits ¶\
\
```\
update_dataset_splits(\
    *,\
    dataset_id: ID_TYPE | None = None,\
    dataset_name: str | None = None,\
    split_name: str,\
    example_ids: list[ID_TYPE],\
    remove: bool = False,\
) -> None\
```\
\
Update the splits for a dataset.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `dataset_id` | The ID of the dataset to update.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `dataset_name` | The name of the dataset to update.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `split_name` | The name of the split to update.<br>**TYPE:**`str` |\
| `example_ids` | The IDs of the examples to add to or<br>remove from the split.<br>**TYPE:**`List[UUID | str]` |\
| `remove` | If True, remove the examples from the split.<br>If False, add the examples to the split. Defaults to False.<br>**TYPE:**`bool | None`**DEFAULT:**`False` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `None` | None |\
\
#### ``evaluate\_run ¶\
\
```\
evaluate_run(\
    run: Run | RunBase | str | UUID,\
    evaluator: RunEvaluator,\
    *,\
    source_info: dict[str, Any] | None = None,\
    reference_example: Example | str | dict | UUID | None = None,\
    load_child_runs: bool = False,\
) -> EvaluationResult\
```\
\
Evaluate a run.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `run` | The run to evaluate.<br>**TYPE:**`Run | RunBase | str | UUID` |\
| `evaluator` | The evaluator to use.<br>**TYPE:**`RunEvaluator` |\
| `source_info` | Additional information about the source of the evaluation to log<br>as feedback metadata.<br>**TYPE:**`Dict[str, Any] | None`**DEFAULT:**`None` |\
| `reference_example` | The example to use as a reference for the evaluation.<br>If not provided, the run's reference example will be used.<br>**TYPE:**`Example | str | dict | UUID | None`**DEFAULT:**`None` |\
| `load_child_runs` | Whether to load child runs when resolving the run ID.<br>**TYPE:**`bool, default=False`**DEFAULT:**`False` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Feedback` | The feedback object created by the evaluation.<br>**TYPE:**`EvaluationResult` |\
\
#### ``aevaluate\_run`async`¶\
\
```\
aevaluate_run(\
    run: Run | str | UUID,\
    evaluator: RunEvaluator,\
    *,\
    source_info: dict[str, Any] | None = None,\
    reference_example: Example | str | dict | UUID | None = None,\
    load_child_runs: bool = False,\
) -> EvaluationResult\
```\
\
Evaluate a run asynchronously.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `run` | The run to evaluate.<br>**TYPE:**`Run | str | UUID` |\
| `evaluator` | The evaluator to use.<br>**TYPE:**`RunEvaluator` |\
| `source_info` | Additional information about the source of the evaluation to log<br>as feedback metadata.<br>**TYPE:**`Dict[str, Any] | None`**DEFAULT:**`None` |\
| `reference_example` | The example to use as a reference for the evaluation.<br>If not provided, the run's reference example will be used.<br>**TYPE:**`Example | str | dict | UUID | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `EvaluationResult` | The evaluation result object created by the evaluation.<br>**TYPE:**`EvaluationResult` |\
\
#### ``create\_feedback ¶\
\
```\
create_feedback(\
    run_id: ID_TYPE | None = None,\
    key: str = "unnamed",\
    *,\
    score: float | int | bool | None = None,\
    value: str | dict | None = None,\
    trace_id: ID_TYPE | None = None,\
    correction: dict | None = None,\
    comment: str | None = None,\
    source_info: dict[str, Any] | None = None,\
    feedback_source_type: FeedbackSourceType | str = API,\
    source_run_id: ID_TYPE | None = None,\
    feedback_id: ID_TYPE | None = None,\
    feedback_config: FeedbackConfig | None = None,\
    stop_after_attempt: int = 10,\
    project_id: ID_TYPE | None = None,\
    comparative_experiment_id: ID_TYPE | None = None,\
    feedback_group_id: ID_TYPE | None = None,\
    extra: dict | None = None,\
    error: bool | None = None,\
    **kwargs: Any,\
) -> Feedback\
```\
\
Create feedback for a run.\
\
**NOTE**: To enable feedback to be batch uploaded in the background you must\
specify trace\_id. _We highly encourage this for latency-sensitive environments._\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `key` | The name of the feedback metric.<br>**TYPE:**`str`**DEFAULT:**`'unnamed'` |\
| `score` | The score to rate this run on the metric or aspect.<br>**TYPE:**`float | int | bool | None`**DEFAULT:**`None` |\
| `value` | The display value or non-numeric value for this feedback.<br>**TYPE:**`float | int | bool | str | dict | None`**DEFAULT:**`None` |\
| `run_id` | The ID of the run to provide feedback for. At least one of run\_id,<br>trace\_id, or project\_id must be specified.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `trace_id` | The ID of the trace (i.e. root parent run) of the run to provide<br>feedback for (specified by run\_id). If run\_id and trace\_id are the<br>same, only trace\_id needs to be specified. **NOTE**: trace\_id is<br>required feedback ingestion to be batched and backgrounded.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `correction` | The proper ground truth for this run.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |\
| `comment` | A comment about this feedback, such as a justification for the score or<br>chain-of-thought trajectory for an LLM judge.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `source_info` | Information about the source of this feedback.<br>**TYPE:**`Dict[str, Any] | None`**DEFAULT:**`None` |\
| `feedback_source_type` | The type of feedback source, such as model (for model-generated feedback)<br>or API.<br>**TYPE:**`FeedbackSourceType | str`**DEFAULT:**`API` |\
| `source_run_id` | The ID of the run that generated this feedback, if a "model" type.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `feedback_id` | The ID of the feedback to create. If not provided, a random UUID will be<br>generated.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `feedback_config` | The configuration specifying how to interpret feedback with this key.<br>Examples include continuous (with min/max bounds), categorical,<br>or freeform.<br>**TYPE:**`FeedbackConfig | None`**DEFAULT:**`None` |\
| `stop_after_attempt` | The number of times to retry the request before giving up.<br>**TYPE:**`int, default=10`**DEFAULT:**`10` |\
| `project_id` | The ID of the project (or experiment) to provide feedback on. This is<br>used for creating summary metrics for experiments. Cannot specify<br>run\_id or trace\_id if project\_id is specified, and vice versa.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `comparative_experiment_id` | If this feedback was logged as a part of a comparative experiment, this<br>associates the feedback with that experiment.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `feedback_group_id` | When logging preferences, ranking runs, or other comparative feedback,<br>this is used to group feedback together.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `extra` | Metadata for the feedback.<br>**TYPE:**`Dict | None`**DEFAULT:**`None` |\
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Feedback` | The created feedback object.<br>**TYPE:**`Feedback` |\
\
Example\
\
.. code-block:: python\
\
```\
from langsmith import trace, traceable, Client\
\
@traceable\
def foo(x):\
    return {"y": x * 2}\
\
@traceable\
def bar(y):\
    return {"z": y - 1}\
\
client = Client()\
\
inputs = {"x": 1}\
with trace(name="foobar", inputs=inputs) as root_run:\
    result = foo(**inputs)\
    result = bar(**result)\
    root_run.outputs = result\
    trace_id = root_run.id\
    child_runs = root_run.child_runs\
\
# Provide feedback for a trace (a.k.a. a root run)\
client.create_feedback(\
    key="user_feedback",\
    score=1,\
    trace_id=trace_id,\
)\
\
# Provide feedback for a child run\
foo_run_id = run for run in child_runs if run.name == "foo".id\
client.create_feedback(\
    key="correctness",\
    score=0,\
    run_id=foo_run_id,\
    # trace_id= is optional but recommended to enable batched and backgrounded\
    # feedback ingestion.\
    trace_id=trace_id,\
)\
```\
\
#### ``update\_feedback ¶\
\
```\
update_feedback(\
    feedback_id: ID_TYPE,\
    *,\
    score: float | int | bool | None = None,\
    value: float | int | bool | str | dict | None = None,\
    correction: dict | None = None,\
    comment: str | None = None,\
) -> None\
```\
\
Update a feedback in the LangSmith API.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `feedback_id` | The ID of the feedback to update.<br>**TYPE:**`UUID | str` |\
| `score` | The score to update the feedback with.<br>**TYPE:**`float | int | bool | None`**DEFAULT:**`None` |\
| `value` | The value to update the feedback with.<br>**TYPE:**`float | int | bool | str | dict | None`**DEFAULT:**`None` |\
| `correction` | The correction to update the feedback with.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |\
| `comment` | The comment to update the feedback with.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `None` | None |\
\
#### ``read\_feedback ¶\
\
```\
read_feedback(feedback_id: ID_TYPE) -> Feedback\
```\
\
Read a feedback from the LangSmith API.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `feedback_id` | The ID of the feedback to read.<br>**TYPE:**`UUID | str` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Feedback` | The feedback.<br>**TYPE:**`Feedback` |\
\
#### ``list\_feedback ¶\
\
```\
list_feedback(\
    *,\
    run_ids: Sequence[ID_TYPE] | None = None,\
    feedback_key: Sequence[str] | None = None,\
    feedback_source_type: Sequence[FeedbackSourceType] | None = None,\
    limit: int | None = None,\
    **kwargs: Any,\
) -> Iterator[Feedback]\
```\
\
List the feedback objects on the LangSmith API.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `run_ids` | The IDs of the runs to filter by.<br>**TYPE:**`Sequence[UUID | str] | None`**DEFAULT:**`None` |\
| `feedback_key` | The feedback key(s) to filter by. Examples: 'correctness'<br>The query performs a union of all feedback keys.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |\
| `feedback_source_type` | The type of feedback source, such as model or API.<br>**TYPE:**`Sequence[FeedbackSourceType] | None`**DEFAULT:**`None` |\
| `limit` | The maximum number of feedback to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |\
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |\
\
| YIELDS | DESCRIPTION |\
| --- | --- |\
| `Feedback` | The feedback objects. |\
\
#### ``delete\_feedback ¶\
\
```\
delete_feedback(feedback_id: ID_TYPE) -> None\
```\
\
Delete a feedback by ID.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `feedback_id` | The ID of the feedback to delete.<br>**TYPE:**`UUID | str` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `None` | None |\
\
#### ``create\_feedback\_from\_token ¶\
\
```\
create_feedback_from_token(\
    token_or_url: str | UUID,\
    score: float | int | bool | None = None,\
    *,\
    value: float | int | bool | str | dict | None = None,\
    correction: dict | None = None,\
    comment: str | None = None,\
    metadata: dict | None = None,\
) -> None\
```\
\
Create feedback from a presigned token or URL.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `token_or_url` | The token or URL from which to create<br>feedback.<br>**TYPE:**`str | UUID` |\
| `score` | The score of the feedback.<br>Defaults to None.<br>**TYPE:**`float | int | bool | None`**DEFAULT:**`None` |\
| `value` | The value of the<br>feedback. Defaults to None.<br>**TYPE:**`float | int | bool | str | dict | None`**DEFAULT:**`None` |\
| `correction` | The correction of the feedback.<br>Defaults to None.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |\
| `comment` | The comment of the feedback. Defaults<br>to None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `metadata` | Additional metadata for the feedback.<br>Defaults to None.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `ValueError` | If the source API URL is invalid. |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `None` | None |\
\
#### ``create\_presigned\_feedback\_token ¶\
\
```\
create_presigned_feedback_token(\
    run_id: ID_TYPE,\
    feedback_key: str,\
    *,\
    expiration: datetime | timedelta | None = None,\
    feedback_config: FeedbackConfig | None = None,\
    feedback_id: ID_TYPE | None = None,\
) -> FeedbackIngestToken\
```\
\
Create a pre-signed URL to send feedback data to.\
\
This is useful for giving browser-based clients a way to upload\
feedback data directly to LangSmith without accessing the\
API key.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `run_id` | The ID of the run.<br>**TYPE:**`UUID | str` |\
| `feedback_key` | The key of the feedback to create.<br>**TYPE:**`str` |\
| `expiration` | The expiration time of the pre-signed URL.<br>Either a datetime or a timedelta offset from now.<br>Default to 3 hours.<br>**TYPE:**`datetime | timedelta | None`**DEFAULT:**`None` |\
| `feedback_config` | If creating a feedback\_key for the first time,<br>this defines how the metric should be interpreted,<br>such as a continuous score (w/ optional bounds),<br>or distribution over categorical values.<br>**TYPE:**`FeedbackConfig | None`**DEFAULT:**`None` |\
| `feedback_id` | The ID of the feedback to create. If not provided, a new<br>feedback will be created.<br>**TYPE:**`Optional[Union[UUID, str]`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `FeedbackIngestToken` | The pre-signed URL for uploading feedback data.<br>**TYPE:**`FeedbackIngestToken` |\
\
#### ``create\_presigned\_feedback\_tokens ¶\
\
```\
create_presigned_feedback_tokens(\
    run_id: ID_TYPE,\
    feedback_keys: Sequence[str],\
    *,\
    expiration: datetime | timedelta | None = None,\
    feedback_configs: Sequence[FeedbackConfig | None] | None = None,\
) -> Sequence[FeedbackIngestToken]\
```\
\
Create a pre-signed URL to send feedback data to.\
\
This is useful for giving browser-based clients a way to upload\
feedback data directly to LangSmith without accessing the\
API key.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `run_id` | The ID of the run.<br>**TYPE:**`UUID | str` |\
| `feedback_keys` | The key of the feedback to create.<br>**TYPE:**`Sequence[str]` |\
| `expiration` | The expiration time of the pre-signed URL.<br>Either a datetime or a timedelta offset from now.<br>Default to 3 hours.<br>**TYPE:**`datetime | timedelta | None`**DEFAULT:**`None` |\
| `feedback_configs` | If creating a feedback\_key for the first time,<br>this defines how the metric should be interpreted,<br>such as a continuous score (w/ optional bounds),<br>or distribution over categorical values.<br>**TYPE:**`Sequence[FeedbackConfig | None] | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Sequence[FeedbackIngestToken]` | Sequence\[FeedbackIngestToken\]: The pre-signed URL for uploading feedback data. |\
\
#### ``list\_presigned\_feedback\_tokens ¶\
\
```\
list_presigned_feedback_tokens(\
    run_id: ID_TYPE, *, limit: int | None = None\
) -> Iterator[FeedbackIngestToken]\
```\
\
List the feedback ingest tokens for a run.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `run_id` | The ID of the run to filter by.<br>**TYPE:**`UUID | str` |\
| `limit` | The maximum number of tokens to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |\
\
| YIELDS | DESCRIPTION |\
| --- | --- |\
| `FeedbackIngestToken` | The feedback ingest tokens. |\
\
#### ``list\_annotation\_queues ¶\
\
```\
list_annotation_queues(\
    *,\
    queue_ids: list[ID_TYPE] | None = None,\
    name: str | None = None,\
    name_contains: str | None = None,\
    limit: int | None = None,\
) -> Iterator[AnnotationQueue]\
```\
\
List the annotation queues on the LangSmith API.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `queue_ids` | The IDs of the queues to filter by.<br>**TYPE:**`List[UUID | str] | None`**DEFAULT:**`None` |\
| `name` | The name of the queue to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `name_contains` | The substring that the queue name should contain.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `limit` | The maximum number of queues to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |\
\
| YIELDS | DESCRIPTION |\
| --- | --- |\
| `AnnotationQueue` | The annotation queues. |\
\
#### ``create\_annotation\_queue ¶\
\
```\
create_annotation_queue(\
    *,\
    name: str,\
    description: str | None = None,\
    queue_id: ID_TYPE | None = None,\
    rubric_instructions: str | None = None,\
) -> AnnotationQueueWithDetails\
```\
\
Create an annotation queue on the LangSmith API.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `name` | The name of the annotation queue.<br>**TYPE:**`str` |\
| `description` | The description of the annotation queue.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `queue_id` | The ID of the annotation queue.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `rubric_instructions` | The rubric instructions for the annotation queue.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `AnnotationQueue` | The created annotation queue object.<br>**TYPE:**`AnnotationQueueWithDetails` |\
\
#### ``read\_annotation\_queue ¶\
\
```\
read_annotation_queue(queue_id: ID_TYPE) -> AnnotationQueue\
```\
\
Read an annotation queue with the specified queue ID.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `queue_id` | The ID of the annotation queue to read.<br>**TYPE:**`UUID | str` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `AnnotationQueue` | The annotation queue object.<br>**TYPE:**`AnnotationQueue` |\
\
#### ``update\_annotation\_queue ¶\
\
```\
update_annotation_queue(\
    queue_id: ID_TYPE,\
    *,\
    name: str,\
    description: str | None = None,\
    rubric_instructions: str | None = None,\
) -> None\
```\
\
Update an annotation queue with the specified queue\_id.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `queue_id` | The ID of the annotation queue to update.<br>**TYPE:**`UUID | str` |\
| `name` | The new name for the annotation queue.<br>**TYPE:**`str` |\
| `description` | The new description for the<br>annotation queue. Defaults to None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `rubric_instructions` | The new rubric instructions for the<br>annotation queue. Defaults to None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `None` | None |\
\
#### ``delete\_annotation\_queue ¶\
\
```\
delete_annotation_queue(queue_id: ID_TYPE) -> None\
```\
\
Delete an annotation queue with the specified queue ID.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `queue_id` | The ID of the annotation queue to delete.<br>**TYPE:**`UUID | str` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `None` | None |\
\
#### ``add\_runs\_to\_annotation\_queue ¶\
\
```\
add_runs_to_annotation_queue(queue_id: ID_TYPE, *, run_ids: list[ID_TYPE]) -> None\
```\
\
Add runs to an annotation queue with the specified queue ID.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `queue_id` | The ID of the annotation queue.<br>**TYPE:**`UUID | str` |\
| `run_ids` | The IDs of the runs to be added to the annotation<br>queue.<br>**TYPE:**`List[UUID | str]` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `None` | None |\
\
#### ``delete\_run\_from\_annotation\_queue ¶\
\
```\
delete_run_from_annotation_queue(queue_id: ID_TYPE, *, run_id: ID_TYPE) -> None\
```\
\
Delete a run from an annotation queue with the specified queue ID and run ID.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `queue_id` | The ID of the annotation queue.<br>**TYPE:**`UUID | str` |\
| `run_id` | The ID of the run to be added to the annotation<br>queue.<br>**TYPE:**`UUID | str` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `None` | None |\
\
#### ``get\_run\_from\_annotation\_queue ¶\
\
```\
get_run_from_annotation_queue(\
    queue_id: ID_TYPE, *, index: int\
) -> RunWithAnnotationQueueInfo\
```\
\
Get a run from an annotation queue at the specified index.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `queue_id` | The ID of the annotation queue.<br>**TYPE:**`UUID | str` |\
| `index` | The index of the run to retrieve.<br>**TYPE:**`int` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `RunWithAnnotationQueueInfo` | The run at the specified index.<br>**TYPE:**`RunWithAnnotationQueueInfo` |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `LangSmithNotFoundError` | If the run is not found at the given index. |\
| `LangSmithError` | For other API-related errors. |\
\
#### ``create\_comparative\_experiment ¶\
\
```\
create_comparative_experiment(\
    name: str,\
    experiments: Sequence[ID_TYPE],\
    *,\
    reference_dataset: ID_TYPE | None = None,\
    description: str | None = None,\
    created_at: datetime | None = None,\
    metadata: dict[str, Any] | None = None,\
    id: ID_TYPE | None = None,\
) -> ComparativeExperiment\
```\
\
Create a comparative experiment on the LangSmith API.\
\
These experiments compare 2 or more experiment results over a shared dataset.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `name` | The name of the comparative experiment.<br>**TYPE:**`str` |\
| `experiments` | The IDs of the experiments to compare.<br>**TYPE:**`Sequence[UUID | str]` |\
| `reference_dataset` | The ID of the dataset these experiments are compared on.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
| `description` | The description of the comparative experiment.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `created_at` | The creation time of the comparative experiment.<br>**TYPE:**`datetime | None`**DEFAULT:**`None` |\
| `metadata` | Additional metadata for the comparative experiment.<br>**TYPE:**`Dict[str, Any] | None`**DEFAULT:**`None` |\
| `id` | The ID of the comparative experiment.<br>**TYPE:**`UUID | str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `ComparativeExperiment` | The created comparative experiment object.<br>**TYPE:**`ComparativeExperiment` |\
\
#### ``arun\_on\_dataset`async`¶\
\
```\
arun_on_dataset(\
    dataset_name: str,\
    llm_or_chain_factory: Any,\
    *,\
    evaluation: Any | None = None,\
    concurrency_level: int = 5,\
    project_name: str | None = None,\
    project_metadata: dict[str, Any] | None = None,\
    dataset_version: datetime | str | None = None,\
    verbose: bool = False,\
    input_mapper: Callable[[dict], Any] | None = None,\
    revision_id: str | None = None,\
    **kwargs: Any,\
) -> dict[str, Any]\
```\
\
Asynchronously run the Chain or language model on a dataset.\
\
.. deprecated:: 0.1.0\
\
This method is deprecated. Use :func:`langsmith.aevaluate` instead.\
\
#### ``run\_on\_dataset ¶\
\
```\
run_on_dataset(\
    dataset_name: str,\
    llm_or_chain_factory: Any,\
    *,\
    evaluation: Any | None = None,\
    concurrency_level: int = 5,\
    project_name: str | None = None,\
    project_metadata: dict[str, Any] | None = None,\
    dataset_version: datetime | str | None = None,\
    verbose: bool = False,\
    input_mapper: Callable[[dict], Any] | None = None,\
    revision_id: str | None = None,\
    **kwargs: Any,\
) -> dict[str, Any]\
```\
\
Run the Chain or language model on a dataset.\
\
.. deprecated:: 0.1.0\
\
This method is deprecated. Use :func:`langsmith.aevaluate` instead.\
\
#### ``like\_prompt ¶\
\
```\
like_prompt(prompt_identifier: str) -> dict[str, int]\
```\
\
Like a prompt.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `prompt_identifier` | The identifier of the prompt.<br>**TYPE:**`str` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `dict[str, int]` | Dict\[str, int\]: A dictionary with the key 'likes' and the count of likes as the value. |\
\
#### ``unlike\_prompt ¶\
\
```\
unlike_prompt(prompt_identifier: str) -> dict[str, int]\
```\
\
Unlike a prompt.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `prompt_identifier` | The identifier of the prompt.<br>**TYPE:**`str` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `dict[str, int]` | Dict\[str, int\]: A dictionary with the key 'likes' and the count of likes as the value. |\
\
#### ``list\_prompts ¶\
\
```\
list_prompts(\
    *,\
    limit: int = 100,\
    offset: int = 0,\
    is_public: bool | None = None,\
    is_archived: bool | None = False,\
    sort_field: PromptSortField = updated_at,\
    sort_direction: Literal["desc", "asc"] = "desc",\
    query: str | None = None,\
) -> ListPromptsResponse\
```\
\
List prompts with pagination.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `limit` | The maximum number of prompts to return. Defaults to 100.<br>**TYPE:**`int, default=100`**DEFAULT:**`100` |\
| `offset` | The number of prompts to skip. Defaults to 0.<br>**TYPE:**`int, default=0`**DEFAULT:**`0` |\
| `is_public` | Filter prompts by if they are public.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |\
| `is_archived` | Filter prompts by if they are archived.<br>**TYPE:**`bool | None`**DEFAULT:**`False` |\
| `sort_field` | The field to sort by.<br>Defaults to "updated\_at".<br>**TYPE:**`PromptSortField`**DEFAULT:**`updated_at` |\
| `sort_direction` | The order to sort by.<br>Defaults to "desc".<br>**TYPE:**`Literal["desc", "asc"], default="desc"`**DEFAULT:**`'desc'` |\
| `query` | Filter prompts by a search query.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `ListPromptsResponse` | A response object containing<br>**TYPE:**`ListPromptsResponse` |\
| `ListPromptsResponse` | the list of prompts. |\
\
#### ``get\_prompt ¶\
\
```\
get_prompt(prompt_identifier: str) -> Prompt | None\
```\
\
Get a specific prompt by its identifier.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `prompt_identifier` | The identifier of the prompt.<br>The identifier should be in the format "prompt\_name" or "owner/prompt\_name".<br>**TYPE:**`str` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Prompt | None` | Optional\[Prompt\]: The prompt object. |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `HTTPError` | If the prompt is not found or<br>another error occurs. |\
\
#### ``create\_prompt ¶\
\
```\
create_prompt(\
    prompt_identifier: str,\
    *,\
    description: str | None = None,\
    readme: str | None = None,\
    tags: Sequence[str] | None = None,\
    is_public: bool = False,\
) -> Prompt\
```\
\
Create a new prompt.\
\
Does not attach prompt object, just creates an empty prompt.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `prompt_identifier` | The identifier of the prompt.<br>The identifier should be in the formatof owner/name:hash, name:hash, owner/name, or name<br>**TYPE:**`str` |\
| `description` | A description of the prompt.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `readme` | A readme for the prompt.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `tags` | A list of tags for the prompt.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |\
| `is_public` | Whether the prompt should be public. Defaults to False.<br>**TYPE:**`bool`**DEFAULT:**`False` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Prompt` | The created prompt object.<br>**TYPE:**`Prompt` |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `ValueError` | If the current tenant is not the owner. |\
| `HTTPError` | If the server request fails. |\
\
#### ``create\_commit ¶\
\
```\
create_commit(\
    prompt_identifier: str, object: Any, *, parent_commit_hash: str | None = None\
) -> str\
```\
\
Create a commit for an existing prompt.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `prompt_identifier` | The identifier of the prompt.<br>**TYPE:**`str` |\
| `object` | The LangChain object to commit.<br>**TYPE:**`Any` |\
| `parent_commit_hash` | The hash of the parent commit.<br>Defaults to latest commit.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `str` | The url of the prompt commit.<br>**TYPE:**`str` |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `HTTPError` | If the server request fails. |\
| `ValueError` | If the prompt does not exist. |\
\
#### ``update\_prompt ¶\
\
```\
update_prompt(\
    prompt_identifier: str,\
    *,\
    description: str | None = None,\
    readme: str | None = None,\
    tags: Sequence[str] | None = None,\
    is_public: bool | None = None,\
    is_archived: bool | None = None,\
) -> dict[str, Any]\
```\
\
Update a prompt's metadata.\
\
To update the content of a prompt, use push\_prompt or create\_commit instead.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `prompt_identifier` | The identifier of the prompt to update.<br>**TYPE:**`str` |\
| `description` | New description for the prompt.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `readme` | New readme for the prompt.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `tags` | New list of tags for the prompt.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |\
| `is_public` | New public status for the prompt.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |\
| `is_archived` | New archived status for the prompt.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `dict[str, Any]` | Dict\[str, Any\]: The updated prompt data as returned by the server. |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `ValueError` | If the prompt\_identifier is empty. |\
| `HTTPError` | If the server request fails. |\
\
#### ``delete\_prompt ¶\
\
```\
delete_prompt(prompt_identifier: str) -> None\
```\
\
Delete a prompt.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `prompt_identifier` | The identifier of the prompt to delete.<br>**TYPE:**`str` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `bool` | True if the prompt was successfully deleted, False otherwise.<br>**TYPE:**`None` |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `ValueError` | If the current tenant is not the owner of the prompt. |\
\
#### ``pull\_prompt\_commit ¶\
\
```\
pull_prompt_commit(\
    prompt_identifier: str, *, include_model: bool | None = False\
) -> PromptCommit\
```\
\
Pull a prompt object from the LangSmith API.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `prompt_identifier` | The identifier of the prompt.<br>**TYPE:**`str` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `PromptCommit` | The prompt object.<br>**TYPE:**`PromptCommit` |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `ValueError` | If no commits are found for the prompt. |\
\
#### ``list\_prompt\_commits ¶\
\
```\
list_prompt_commits(\
    prompt_identifier: str,\
    *,\
    limit: int | None = None,\
    offset: int = 0,\
    include_model: bool = False,\
) -> Iterator[ListedPromptCommit]\
```\
\
List commits for a given prompt.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `prompt_identifier` | The identifier of the prompt in the format 'owner/repo\_name'.<br>**TYPE:**`str` |\
| `limit` | The maximum number of commits to return. If None, returns all commits. Defaults to None.<br>**TYPE:**`int | None`**DEFAULT:**`None` |\
| `offset` | The number of commits to skip before starting to return results. Defaults to 0.<br>**TYPE:**`int, default=0`**DEFAULT:**`0` |\
| `include_model` | Whether to include the model information in the commit data. Defaults to False.<br>**TYPE:**`bool, default=False`**DEFAULT:**`False` |\
\
| YIELDS | DESCRIPTION |\
| --- | --- |\
| `ListedPromptCommit` | A ListedPromptCommit object for each commit. |\
\
Note\
\
This method uses pagination to retrieve commits. It will make multiple API calls if necessary to retrieve all commits\
or up to the specified limit.\
\
#### ``pull\_prompt ¶\
\
```\
pull_prompt(prompt_identifier: str, *, include_model: bool | None = False) -> Any\
```\
\
Pull a prompt and return it as a LangChain PromptTemplate.\
\
This method requires `langchain-core `\_\_.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `prompt_identifier` | The identifier of the prompt.<br>**TYPE:**`str` |\
| `include_model` | Whether to include the model information in the prompt data.<br>**TYPE:**`Optional[bool], default=False`**DEFAULT:**`False` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `Any` | The prompt object in the specified format.<br>**TYPE:**`Any` |\
\
#### ``push\_prompt ¶\
\
```\
push_prompt(\
    prompt_identifier: str,\
    *,\
    object: Any | None = None,\
    parent_commit_hash: str = "latest",\
    is_public: bool | None = None,\
    description: str | None = None,\
    readme: str | None = None,\
    tags: Sequence[str] | None = None,\
) -> str\
```\
\
Push a prompt to the LangSmith API.\
\
Can be used to update prompt metadata or prompt content.\
\
If the prompt does not exist, it will be created.\
If the prompt exists, it will be updated.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `prompt_identifier` | The identifier of the prompt.<br>**TYPE:**`str` |\
| `object` | The LangChain object to push.<br>**TYPE:**`Any | None`**DEFAULT:**`None` |\
| `parent_commit_hash` | The parent commit hash.<br>Defaults to "latest".<br>**TYPE:**`str`**DEFAULT:**`'latest'` |\
| `is_public` | Whether the prompt should be public.<br>If None (default), the current visibility status is maintained for existing prompts.<br>For new prompts, None defaults to private.<br>Set to True to make public, or False to make private.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |\
| `description` | A description of the prompt.<br>Defaults to an empty string.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `readme` | A readme for the prompt.<br>Defaults to an empty string.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `tags` | A list of tags for the prompt.<br>Defaults to an empty list.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `str` | The URL of the prompt.<br>**TYPE:**`str` |\
\
#### ``cleanup ¶\
\
```\
cleanup() -> None\
```\
\
Manually trigger cleanup of the background thread.\
\
#### ``evaluate ¶\
\
```\
evaluate(\
    target: TARGET_T | Runnable | EXPERIMENT_T | tuple[EXPERIMENT_T, EXPERIMENT_T],\
    /,\
    data: DATA_T | None = None,\
    evaluators: Sequence[EVALUATOR_T] | Sequence[COMPARATIVE_EVALUATOR_T] | None = None,\
    summary_evaluators: Sequence[SUMMARY_EVALUATOR_T] | None = None,\
    metadata: dict | None = None,\
    experiment_prefix: str | None = None,\
    description: str | None = None,\
    max_concurrency: int | None = 0,\
    num_repetitions: int = 1,\
    blocking: bool = True,\
    experiment: EXPERIMENT_T | None = None,\
    upload_results: bool = True,\
    error_handling: Literal["log", "ignore"] = "log",\
    **kwargs: Any,\
) -> ExperimentResults | ComparativeExperimentResults\
```\
\
Evaluate a target system on a given dataset.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `target` | The target system or experiment(s) to evaluate. Can be a function<br>that takes a dict and returns a dict, a langchain Runnable, an<br>existing experiment ID, or a two-tuple of experiment IDs.<br>**TYPE:**`TARGET_T | Runnable | EXPERIMENT_T | Tuple[EXPERIMENT_T, EXPERIMENT_T]` |\
| `data` | The dataset to evaluate on. Can be a dataset name, a list of<br>examples, or a generator of examples.<br>**TYPE:**`DATA_T`**DEFAULT:**`None` |\
| `evaluators` | A list of evaluators to run on each example. The evaluator signature<br>depends on the target type. Default to None.<br>**TYPE:**`Sequence[EVALUATOR_T] | Sequence[COMPARATIVE_EVALUATOR_T] | None`**DEFAULT:**`None` |\
| `summary_evaluators` | A list of summary<br>evaluators to run on the entire dataset. Should not be specified if<br>comparing two existing experiments. Defaults to None.<br>**TYPE:**`Sequence[SUMMARY_EVALUATOR_T] | None`**DEFAULT:**`None` |\
| `metadata` | Metadata to attach to the experiment.<br>Defaults to None.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |\
| `experiment_prefix` | A prefix to provide for your experiment name.<br>Defaults to None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `description` | A free-form text description for the experiment.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `max_concurrency` | The maximum number of concurrent<br>evaluations to run. If None then no limit is set. If 0 then no concurrency.<br>Defaults to 0.<br>**TYPE:**`Optional[int], default=0`**DEFAULT:**`0` |\
| `blocking` | Whether to block until the evaluation is complete.<br>Defaults to True.<br>**TYPE:**`bool, default=True`**DEFAULT:**`True` |\
| `num_repetitions` | The number of times to run the evaluation.<br>Each item in the dataset will be run and evaluated this many times.<br>Defaults to 1.<br>**TYPE:**`int, default=1`**DEFAULT:**`1` |\
| `experiment` | An existing experiment to<br>extend. If provided, experiment\_prefix is ignored. For advanced<br>usage only. Should not be specified if target is an existing experiment or<br>two-tuple fo experiments.<br>**TYPE:**`EXPERIMENT_T | None`**DEFAULT:**`None` |\
| `upload_results` | Whether to upload the results to LangSmith.<br>Defaults to True.<br>**TYPE:**`bool, default=True`**DEFAULT:**`True` |\
| `error_handling` | How to handle individual run errors. 'log'<br>will trace the runs with the error message as part of the experiment,<br>'ignore' will not count the run as part of the experiment at all.<br>**TYPE:**`str, default="log"`**DEFAULT:**`'log'` |\
| `**kwargs` | Additional keyword arguments to pass to the evaluator.<br>**TYPE:**`Any`**DEFAULT:**`{}` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `ExperimentResults` | If target is a function, Runnable, or existing experiment.<br>**TYPE:**`ExperimentResults | ComparativeExperimentResults` |\
| `ComparativeExperimentResults` | If target is a two-tuple of existing experiments.<br>**TYPE:**`ExperimentResults | ComparativeExperimentResults` |\
\
Examples:\
\
Prepare the dataset:\
\
.. code-block:: python\
\
```\
from langsmith import Client\
\
client = Client()\
dataset = client.clone_public_dataset(\
    "https://smith.langchain.com/public/419dcab2-1d66-4b94-8901-0357ead390df/d"\
)\
dataset_name = "Evaluate Examples"\
```\
\
Basic usage:\
\
.. code-block:: python\
\
```\
def accuracy(outputs: dict, reference_outputs: dict) -> dict:\
    # Row-level evaluator for accuracy.\
    pred = outputs["response"]\
    expected = reference_outputs["answer"]\
    return {"score": expected.lower() == pred.lower()}\
```\
\
.. code-block:: python\
\
```\
def precision(outputs: list[dict], reference_outputs: list[dict]) -> dict:\
    # Experiment-level evaluator for precision.\
    # TP / (TP + FP)\
    predictions = [out["response"].lower() for out in outputs]\
    expected = [ref["answer"].lower() for ref in reference_outputs]\
    # yes and no are the only possible answers\
    tp = sum([p == e for p, e in zip(predictions, expected) if p == "yes"])\
    fp = sum([p == "yes" and e == "no" for p, e in zip(predictions, expected)])\
    return {"score": tp / (tp + fp)}\
\
def predict(inputs: dict) -> dict:\
    # This can be any function or just an API call to your app.\
    return {"response": "Yes"}\
\
results = client.evaluate(\
    predict,\
    data=dataset_name,\
    evaluators=[accuracy],\
    summary_evaluators=[precision],\
    experiment_prefix="My Experiment",\
    description="Evaluating the accuracy of a simple prediction model.",\
    metadata={\
        "my-prompt-version": "abcd-1234",\
    },\
)\
```\
\
Evaluating over only a subset of the examples\
\
.. code-block:: python\
\
```\
experiment_name = results.experiment_name\
examples = client.list_examples(dataset_name=dataset_name, limit=5)\
results = client.evaluate(\
    predict,\
    data=examples,\
    evaluators=[accuracy],\
    summary_evaluators=[precision],\
    experiment_prefix="My Experiment",\
    description="Just testing a subset synchronously.",\
)\
```\
\
Streaming each prediction to more easily + eagerly debug.\
\
.. code-block:: python\
\
```\
results = client.evaluate(\
    predict,\
    data=dataset_name,\
    evaluators=[accuracy],\
    summary_evaluators=[precision],\
    description="I don't even have to block!",\
    blocking=False,\
)\
for i, result in enumerate(results):  # doctest: +ELLIPSIS\
    pass\
```\
\
Using the `evaluate` API with an off-the-shelf LangChain evaluator:\
\
.. code-block:: python\
\
```\
from langsmith.evaluation import LangChainStringEvaluator\
from langchain.chat_models import init_chat_model\
\
def prepare_criteria_data(run: Run, example: Example):\
    return {\
        "prediction": run.outputs["output"],\
        "reference": example.outputs["answer"],\
        "input": str(example.inputs),\
    }\
\
results = client.evaluate(\
    predict,\
    data=dataset_name,\
    evaluators=[\
        accuracy,\
        LangChainStringEvaluator("embedding_distance"),\
        LangChainStringEvaluator(\
            "labeled_criteria",\
            config={\
                "criteria": {\
                    "usefulness": "The prediction is useful if it is correct"\
                    " and/or asks a useful followup question."\
                },\
                "llm": init_chat_model("gpt-4o"),\
            },\
            prepare_data=prepare_criteria_data,\
        ),\
    ],\
    description="Evaluating with off-the-shelf LangChain evaluators.",\
    summary_evaluators=[precision],\
)\
```\
\
View the evaluation results for experiment:...\
Evaluating a LangChain object:\
\
.. code-block:: python\
\
```\
from langchain_core.runnables import chain as as_runnable\
\
@as_runnable\
def nested_predict(inputs):\
    return {"response": "Yes"}\
\
@as_runnable\
def lc_predict(inputs):\
    return nested_predict.invoke(inputs)\
\
results = client.evaluate(\
    lc_predict,\
    data=dataset_name,\
    evaluators=[accuracy],\
    description="This time we're evaluating a LangChain object.",\
    summary_evaluators=[precision],\
)\
```\
\
Comparative evaluation:\
\
.. code-block:: python\
\
```\
results = client.evaluate(\
    # The target is a tuple of the experiment IDs to compare\
    target=(\
        "12345678-1234-1234-1234-123456789012",\
        "98765432-1234-1234-1234-123456789012",\
    ),\
    evaluators=[accuracy],\
    summary_evaluators=[precision],\
)\
```\
\
Evaluate an existing experiment:\
\
.. code-block:: python\
\
```\
results = client.evaluate(\
    # The target is the ID of the experiment we are evaluating\
    target="12345678-1234-1234-1234-123456789012",\
    evaluators=[accuracy],\
    summary_evaluators=[precision],\
)\
```\
\
.. versionadded:: 0.2.0\
\
#### ``aevaluate`async`¶\
\
```\
aevaluate(\
    target: ATARGET_T | AsyncIterable[dict] | Runnable | str | UUID | TracerSession,\
    /,\
    data: DATA_T | AsyncIterable[Example] | Iterable[Example] | None = None,\
    evaluators: Sequence[EVALUATOR_T | AEVALUATOR_T] | None = None,\
    summary_evaluators: Sequence[SUMMARY_EVALUATOR_T] | None = None,\
    metadata: dict | None = None,\
    experiment_prefix: str | None = None,\
    description: str | None = None,\
    max_concurrency: int | None = 0,\
    num_repetitions: int = 1,\
    blocking: bool = True,\
    experiment: TracerSession | str | UUID | None = None,\
    upload_results: bool = True,\
    error_handling: Literal["log", "ignore"] = "log",\
    **kwargs: Any,\
) -> AsyncExperimentResults\
```\
\
Evaluate an async target system on a given dataset.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `target` | The target system or experiment(s) to evaluate. Can be an async function<br>that takes a dict and returns a dict, a langchain Runnable, an<br>existing experiment ID, or a two-tuple of experiment IDs.<br>**TYPE:**`ATARGET_T | AsyncIterable[dict] | Runnable | str | UUID | TracerSession` |\
| `data` | The dataset to evaluate on. Can be a dataset name, a list of<br>examples, an async generator of examples, or an async iterable of examples.<br>**TYPE:**`DATA_T | AsyncIterable[Example]`**DEFAULT:**`None` |\
| `evaluators` | A list of evaluators to run<br>on each example. Defaults to None.<br>**TYPE:**`Sequence[EVALUATOR_T] | None`**DEFAULT:**`None` |\
| `summary_evaluators` | A list of summary<br>evaluators to run on the entire dataset. Defaults to None.<br>**TYPE:**`Sequence[SUMMARY_EVALUATOR_T] | None`**DEFAULT:**`None` |\
| `metadata` | Metadata to attach to the experiment.<br>Defaults to None.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |\
| `experiment_prefix` | A prefix to provide for your experiment name.<br>Defaults to None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `description` | A description of the experiment.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `max_concurrency` | The maximum number of concurrent<br>evaluations to run. If None then no limit is set. If 0 then no concurrency.<br>Defaults to 0.<br>**TYPE:**`Optional[int], default=0`**DEFAULT:**`0` |\
| `num_repetitions` | The number of times to run the evaluation.<br>Each item in the dataset will be run and evaluated this many times.<br>Defaults to 1.<br>**TYPE:**`int, default=1`**DEFAULT:**`1` |\
| `blocking` | Whether to block until the evaluation is complete.<br>Defaults to True.<br>**TYPE:**`bool, default=True`**DEFAULT:**`True` |\
| `experiment` | An existing experiment to<br>extend. If provided, experiment\_prefix is ignored. For advanced<br>usage only.<br>**TYPE:**`TracerSession | None`**DEFAULT:**`None` |\
| `upload_results` | Whether to upload the results to LangSmith.<br>Defaults to True.<br>**TYPE:**`bool, default=True`**DEFAULT:**`True` |\
| `error_handling` | How to handle individual run errors. 'log'<br>will trace the runs with the error message as part of the experiment,<br>'ignore' will not count the run as part of the experiment at all.<br>**TYPE:**`str, default="log"`**DEFAULT:**`'log'` |\
| `**kwargs` | Additional keyword arguments to pass to the evaluator.<br>**TYPE:**`Any`**DEFAULT:**`{}` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `AsyncExperimentResults` | AsyncIterator\[ExperimentResultRow\]: An async iterator over the experiment results. |\
\
Environment\
\
- LANGSMITH\_TEST\_CACHE: If set, API calls will be cached to disk to save time and\
cost during testing. Recommended to commit the cache files to your repository\
for faster CI/CD runs.\
Requires the 'langsmith\[vcr\]' package to be installed.\
\
Examples:\
\
Prepare the dataset:\
\
.. code-block:: python\
\
```\
import asyncio\
from langsmith import Client\
\
client = Client()\
dataset = client.clone_public_dataset(\
    "https://smith.langchain.com/public/419dcab2-1d66-4b94-8901-0357ead390df/d"\
)\
dataset_name = "Evaluate Examples"\
```\
\
Basic usage:\
\
.. code-block:: python\
\
```\
def accuracy(outputs: dict, reference_outputs: dict) -> dict:\
    # Row-level evaluator for accuracy.\
    pred = outputs["resposen"]\
    expected = reference_outputs["answer"]\
    return {"score": expected.lower() == pred.lower()}\
\
def precision(outputs: list[dict], reference_outputs: list[dict]) -> dict:\
    # Experiment-level evaluator for precision.\
    # TP / (TP + FP)\
    predictions = [out["response"].lower() for out in outputs]\
    expected = [ref["answer"].lower() for ref in reference_outputs]\
    # yes and no are the only possible answers\
    tp = sum([p == e for p, e in zip(predictions, expected) if p == "yes"])\
    fp = sum([p == "yes" and e == "no" for p, e in zip(predictions, expected)])\
    return {"score": tp / (tp + fp)}\
\
async def apredict(inputs: dict) -> dict:\
    # This can be any async function or just an API call to your app.\
    await asyncio.sleep(0.1)\
    return {"response": "Yes"}\
\
results = asyncio.run(\
    client.aevaluate(\
        apredict,\
        data=dataset_name,\
        evaluators=[accuracy],\
        summary_evaluators=[precision],\
        experiment_prefix="My Experiment",\
        description="Evaluate the accuracy of the model asynchronously.",\
        metadata={\
            "my-prompt-version": "abcd-1234",\
        },\
    )\
)\
```\
\
Evaluating over only a subset of the examples using an async generator:\
\
.. code-block:: python\
\
```\
async def example_generator():\
    examples = client.list_examples(dataset_name=dataset_name, limit=5)\
    for example in examples:\
        yield example\
\
results = asyncio.run(\
    client.aevaluate(\
        apredict,\
        data=example_generator(),\
        evaluators=[accuracy],\
        summary_evaluators=[precision],\
        experiment_prefix="My Subset Experiment",\
        description="Evaluate a subset of examples asynchronously.",\
    )\
)\
```\
\
Streaming each prediction to more easily + eagerly debug.\
\
.. code-block:: python\
\
```\
results = asyncio.run(\
    client.aevaluate(\
        apredict,\
        data=dataset_name,\
        evaluators=[accuracy],\
        summary_evaluators=[precision],\
        experiment_prefix="My Streaming Experiment",\
        description="Streaming predictions for debugging.",\
        blocking=False,\
    )\
)\
\
async def aenumerate(iterable):\
    async for elem in iterable:\
        print(elem)\
\
asyncio.run(aenumerate(results))\
```\
\
Running without concurrency:\
\
.. code-block:: python\
\
```\
results = asyncio.run(\
    client.aevaluate(\
        apredict,\
        data=dataset_name,\
        evaluators=[accuracy],\
        summary_evaluators=[precision],\
        experiment_prefix="My Experiment Without Concurrency",\
        description="This was run without concurrency.",\
        max_concurrency=0,\
    )\
)\
```\
\
Using Async evaluators:\
\
.. code-block:: python\
\
```\
async def helpfulness(outputs: dict) -> dict:\
    # Row-level evaluator for helpfulness.\
    await asyncio.sleep(5)  # Replace with your LLM API call\
    return {"score": outputs["output"] == "Yes"}\
\
results = asyncio.run(\
    client.aevaluate(\
        apredict,\
        data=dataset_name,\
        evaluators=[helpfulness],\
        summary_evaluators=[precision],\
        experiment_prefix="My Helpful Experiment",\
        description="Applying async evaluators example.",\
    )\
)\
```\
\
Evaluate an existing experiment:\
\
.. code-block:: python\
\
```\
results = asyncio.run(\
    client.aevaluate(\
        # The target is the ID of the experiment we are evaluating\
        target="419dcab2-1d66-4b94-8901-0357ead390df",\
        evaluators=[accuracy, helpfulness],\
        summary_evaluators=[precision],\
    )\
)\
```\
\
.. versionadded:: 0.2.0\
\
#### ``get\_experiment\_results ¶\
\
```\
get_experiment_results(\
    name: str | None = None,\
    project_id: UUID | None = None,\
    preview: bool = False,\
    comparative_experiment_id: UUID | None = None,\
    filters: dict[UUID, list[str]] | None = None,\
    limit: int | None = None,\
) -> ExperimentResults\
```\
\
Get results for an experiment, including experiment session aggregated stats and experiment runs for each dataset example.\
\
Experiment results may not be available immediately after the experiment is created.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `name` | The experiment name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `project_id` | Experiment's tracing project id, also called session\_id, can be found in the url of the LS experiment page<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |\
| `preview` | Whether to return lightweight preview data only. When True,<br>fetches inputs\_preview/outputs\_preview summaries instead of full inputs/outputs from S3 storage.<br>Faster and less bandwidth.<br>**TYPE:**`bool`**DEFAULT:**`False` |\
| `comparative_experiment_id` | Optional comparative experiment UUID for pairwise comparison experiment results.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |\
| `filters` | Optional filters to apply to results<br>**TYPE:**`dict[UUID, list[str]] | None`**DEFAULT:**`None` |\
| `limit` | Maximum number of results to return<br>**TYPE:**`int | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `ExperimentResults` | ExperimentResults with:<br>\- feedback\_stats: Combined feedback statistics including session-level feedback<br>\- run\_stats: Aggregated run statistics (latency, tokens, cost, etc.)<br>\- examples\_with\_runs: Iterator of ExampleWithRuns |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `ValueError` | If project not found for the given session\_id |\
\
Example\
\
.. code-block:: python\
\
```\
client = Client()\
results = client.get_experiment_results(\
    project_id="037ae90f-f297-4926-b93c-37d8abf6899f",\
)\
for example_with_runs in results["examples_with_runs"]:\
    print(example_with_runs.dict())\
\
# Access aggregated experiment statistics\
print(f"Total runs: {results'run_stats'}")\
print(f"Total cost: {results'run_stats'}")\
print(f"P50 latency: {results'run_stats'}")\
\
# Access feedback statistics\
print(f"Feedback stats: {results['feedback_stats']}")\
```\
\
#### ``generate\_insights ¶\
\
```\
generate_insights(\
    *,\
    chat_histories: list[list[dict]],\
    instructions: str = DEFAULT_INSTRUCTIONS,\
    name: str | None = None,\
    model: Literal["openai", "anthropic"] | None = None,\
    openai_api_key: str | None = None,\
    anthropic_api_key: str | None = None,\
) -> InsightsReport\
```\
\
Generate Insights over your agent chat histories.\
\
**NOTE**:\
\- Only available to Plus and higher tier LangSmith users.\
\- Insights Agent uses user's model API key. The cost of the report\
grows linearly with the number of chat histories you upload and the\
size of each history. For more see:\
https://docs.langchain.com/langsmith/insights\
\- This method will upload your chat histories as traces to LangSmith.\
\- If you pass in a model API key this will be set as a workspace secret\
meaning it will be usedin for evaluators and the playground.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `chat_histories` | A list of chat histories. Each chat history should be a<br>list of messages. We recommend formatting these as OpenAI messages with<br>a "role" and "content" key. Max length 1000 items.<br>**TYPE:**`list[list[dict]]` |\
| `instructions` | Instructions for the Insights agent. Should focus on what<br>your agent does and what types of insights you<br>want to generate.<br>**TYPE:**`str`**DEFAULT:**`DEFAULT_INSTRUCTIONS` |\
| `name` | Name for the generated Insights report.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `model` | Whether to use OpenAI or Anthropic models. This will impact the<br>cost of generating the Insights Report.<br>**TYPE:**`Literal['openai', 'anthropic'] | None`**DEFAULT:**`None` |\
| `openai_api_key` | OpenAI API key to use. Only needed if you have not already<br>stored this in LangSmith as a workspace secret.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
| `anthropic_api_key` | Anthropic API key to use. Only needed if you have not<br>already stored this in LangSmith as a workspace secret.<br>**TYPE:**`str | None`**DEFAULT:**`None` |\
\
Examples:\
\
```\
.. code-block:: python\
\
    import os\
    from langsmith import Client\
\
    client = client()\
\
    chat_histories = [\
        [\
            {"role": "user", "content": "how are you"},\
            {"role": "assistant", "content": "good!"},\
        ],\
        [\
            {"role": "user", "content": "do you like art"},\
            {"role": "assistant", "content": "only Tarkovsky"},\
        ],\
    ]\
\
    report = client.generate_insights(\
        chat_histories=chat_histories,\
        name="Conversation Topics",\
        instructions="What are the high-level topics of conversations users are having with the assistant?",\
        openai_api_key=os.environ["OPENAI_API_KEY"],\
    )\
\
    # client.poll_insights(report=report)\
```\
\
#### ``poll\_insights ¶\
\
```\
poll_insights(\
    *,\
    report: InsightsReport | None = None,\
    id: str | UUID | None = None,\
    project_id: str | UUID | None = None,\
    rate: int = 30,\
    timeout: int = 30 * 60,\
    verbose: bool = False,\
) -> InsightsReport\
```\
\
Poll the status of an Insights report.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `report` | THe InsightsReport.<br>**TYPE:**`InsightsReport | None`**DEFAULT:**`None` |\
| `id` | The Insights report ID. Should only specify if 'report' is not specified.<br>**TYPE:**`str | UUID | None`**DEFAULT:**`None` |\
| `project_id` | The Tracing project ID. Should only specify if 'report' is not specified.<br>**TYPE:**`str | UUID | None`**DEFAULT:**`None` |\
\
### ``close\_session ¶\
\
```\
close_session(session: Session) -> None\
```\
\
Close the session.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `session` | The session to close.<br>**TYPE:**`Session` |\
\
### ``convert\_prompt\_to\_openai\_format ¶\
\
```\
convert_prompt_to_openai_format(\
    messages: Any, model_kwargs: dict[str, Any] | None = None\
) -> dict\
```\
\
Convert a prompt to OpenAI format.\
\
Requires the `langchain_openai` package to be installed.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `messages` | The messages to convert.<br>**TYPE:**`Any` |\
| `model_kwargs` | Model configuration arguments including<br>`stop` and any other required arguments. Defaults to None.<br>**TYPE:**`Dict[str, Any] | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `dict` | The prompt in OpenAI format.<br>**TYPE:**`dict` |\
\
| RAISES | DESCRIPTION |\
| --- | --- |\
| `ImportError` | If the `langchain_openai` package is not installed. |\
| `LangSmithError` | If there is an error during the conversion process. |\
\
### ``convert\_prompt\_to\_anthropic\_format ¶\
\
```\
convert_prompt_to_anthropic_format(\
    messages: Any, model_kwargs: dict[str, Any] | None = None\
) -> dict\
```\
\
Convert a prompt to Anthropic format.\
\
Requires the `langchain_anthropic` package to be installed.\
\
| PARAMETER | DESCRIPTION |\
| --- | --- |\
| `messages` | The messages to convert.<br>**TYPE:**`Any` |\
| `model_kwargs` | Model configuration arguments including `model_name` and `stop`.<br>Defaults to None.<br>**TYPE:**`Dict[str, Any] | None`**DEFAULT:**`None` |\
\
| RETURNS | DESCRIPTION |\
| --- | --- |\
| `dict` | The prompt in Anthropic format.<br>**TYPE:**`dict` |\
\
### ``dump\_model ¶\
\
```\
dump_model(model) -> dict[str, Any]\
```\
\
Dump model depending on pydantic version.\
\
### ``prep\_obj\_for\_push ¶\
\
```\
prep_obj_for_push(obj: Any) -> Any\
```\
\
Format the object so its Prompt Hub compatible.\
\
Back to top