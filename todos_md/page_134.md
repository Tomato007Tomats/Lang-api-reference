<!-- URL: page_134 -->

Skip to content

Edit this page

# LangSmith Deployment SDK reference ¶

(Formerly LangGraph Platform)

![PyPI - Version](https://pypi.org/project/langgraph-sdk/#history)![PyPI - License](https://opensource.org/licenses/MIT)![PyPI - Downloads](https://pypistats.org/packages/langgraph-sdk)

## ``langgraph\_sdk.client ¶

The LangGraph client implementations connect to the LangGraph API.

This module provides both asynchronous ( get\_client(url="http://localhost:2024")) or LangGraphClient)
and synchronous ( get\_sync\_client(url="http://localhost:2024")) or SyncLanggraphClient)
clients to interacting with the LangGraph API's core resources such as
Assistants, Threads, Runs, and Cron jobs, as well as its persistent
document Store.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `get_client` | Create and configure a LangGraphClient. |
| `get_sync_client` | Get a synchronous LangGraphClient instance. |

### ``LangGraphClient ¶

Top-level client for LangGraph API.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `assistants` | Manages versioned configuration for your graphs. |
| `threads` | Handles (potentially) multi-turn interactions, such as conversational threads. |
| `runs` | Controls individual invocations of the graph. |
| `crons` | Manages scheduled operations. |
| `store` | Interfaces with persistent, shared data storage. |

| METHOD | DESCRIPTION |
| --- | --- |
| `__aenter__` | Enter the async context manager. |
| `__aexit__` | Exit the async context manager. |
| `aclose` | Close the underlying HTTP client. |

#### ``\_\_aenter\_\_`async`¶

```
__aenter__() -> LangGraphClient
```

Enter the async context manager.

#### ``\_\_aexit\_\_`async`¶

```
__aexit__(
    exc_type: type[BaseException] | None,
    exc_val: BaseException | None,
    exc_tb: TracebackType | None,
) -> None
```

Exit the async context manager.

#### ``aclose`async`¶

```
aclose() -> None
```

Close the underlying HTTP client.

### ``HttpClient ¶

Handle async requests to the LangGraph API.

Adds additional error messaging & content handling above the
provided httpx client.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `client` | Underlying HTTPX async client.<br>**TYPE:**`AsyncClient` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get` | Send a `GET` request. |
| `post` | Send a `POST` request. |
| `put` | Send a `PUT` request. |
| `patch` | Send a `PATCH` request. |
| `delete` | Send a `DELETE` request. |
| `request_reconnect` | Send a request that automatically reconnects to Location header. |
| `stream` | Stream results using SSE. |

#### ``get`async`¶

```
get(
    path: str,
    *,
    params: QueryParamTypes | None = None,
    headers: Mapping[str, str] | None = None,
    on_response: Callable[[Response], None] | None = None,
) -> Any
```

Send a `GET` request.

#### ``post`async`¶

```
post(
    path: str,
    *,
    json: dict[str, Any] | list | None,
    params: QueryParamTypes | None = None,
    headers: Mapping[str, str] | None = None,
    on_response: Callable[[Response], None] | None = None,
) -> Any
```

Send a `POST` request.

#### ``put`async`¶

```
put(
    path: str,
    *,
    json: dict,
    params: QueryParamTypes | None = None,
    headers: Mapping[str, str] | None = None,
    on_response: Callable[[Response], None] | None = None,
) -> Any
```

Send a `PUT` request.

#### ``patch`async`¶

```
patch(
    path: str,
    *,
    json: dict,
    params: QueryParamTypes | None = None,
    headers: Mapping[str, str] | None = None,
    on_response: Callable[[Response], None] | None = None,
) -> Any
```

Send a `PATCH` request.

#### ``delete`async`¶

```
delete(
    path: str,
    *,
    json: Any | None = None,
    params: QueryParamTypes | None = None,
    headers: Mapping[str, str] | None = None,
    on_response: Callable[[Response], None] | None = None,
) -> None
```

Send a `DELETE` request.

#### ``request\_reconnect`async`¶

```
request_reconnect(
    path: str,
    method: str,
    *,
    json: dict[str, Any] | None = None,
    params: QueryParamTypes | None = None,
    headers: Mapping[str, str] | None = None,
    on_response: Callable[[Response], None] | None = None,
    reconnect_limit: int = 5,
) -> Any
```

Send a request that automatically reconnects to Location header.

#### ``stream`async`¶

```
stream(
    path: str,
    method: str,
    *,
    json: dict[str, Any] | None = None,
    params: QueryParamTypes | None = None,
    headers: Mapping[str, str] | None = None,
    on_response: Callable[[Response], None] | None = None,
) -> AsyncIterator[StreamPart]
```

Stream results using SSE.

### ``AssistantsClient ¶

Client for managing assistants in LangGraph.

This class provides methods to interact with assistants,
which are versioned configurations of your graph.

Example

```
client = get_client(url="http://localhost:2024")
assistant = await client.assistants.get("assistant_id_123")
```

| METHOD | DESCRIPTION |
| --- | --- |
| `get` | Get an assistant by ID. |
| `get_graph` | Get the graph of an assistant by ID. |
| `get_schemas` | Get the schemas of an assistant by ID. |
| `get_subgraphs` | Get the schemas of an assistant by ID. |
| `create` | Create a new assistant. |
| `update` | Update an assistant. |
| `delete` | Delete an assistant. |
| `search` | Search for assistants. |
| `count` | Count assistants matching filters. |
| `get_versions` | List all versions of an assistant. |
| `set_latest` | Change the version of an assistant. |

#### ``get`async`¶

```
get(
    assistant_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Assistant
```

Get an assistant by ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The ID of the assistant to get.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Assistant` | Assistant Object.<br>**TYPE:**`Assistant` |

Example Usage

```
assistant = await client.assistants.get(
    assistant_id="my_assistant_id"
)
print(assistant)
```

```
----------------------------------------------------

{
    'assistant_id': 'my_assistant_id',
    'graph_id': 'agent',
    'created_at': '2024-06-25T17:10:33.109781+00:00',
    'updated_at': '2024-06-25T17:10:33.109781+00:00',
    'config': {},
    'metadata': {'created_by': 'system'},
    'version': 1,
    'name': 'my_assistant'
}
```

#### ``get\_graph`async`¶

```
get_graph(
    assistant_id: str,
    *,
    xray: int | bool = False,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> dict[str, list[dict[str, Any]]]
```

Get the graph of an assistant by ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The ID of the assistant to get the graph of.<br>**TYPE:**`str` |
| `xray` | Include graph representation of subgraphs. If an integer value is provided, only subgraphs with a depth less than or equal to the value will be included.<br>**TYPE:**`int | bool`**DEFAULT:**`False` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Graph` | The graph information for the assistant in JSON format.<br>**TYPE:**`dict[str, list[dict[str, Any]]]` |

Example Usage

```
client = get_client(url="http://localhost:2024")
graph_info = await client.assistants.get_graph(
    assistant_id="my_assistant_id"
)
print(graph_info)
```

```
--------------------------------------------------------------------------------------------------------------------------

{
    'nodes':
        [\
            {'id': '__start__', 'type': 'schema', 'data': '__start__'},\
            {'id': '__end__', 'type': 'schema', 'data': '__end__'},\
            {'id': 'agent','type': 'runnable','data': {'id': ['langgraph', 'utils', 'RunnableCallable'],'name': 'agent'}},\
        ],
    'edges':
        [\
            {'source': '__start__', 'target': 'agent'},\
            {'source': 'agent','target': '__end__'}\
        ]
}
```

#### ``get\_schemas`async`¶

```
get_schemas(
    assistant_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> GraphSchema
```

Get the schemas of an assistant by ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The ID of the assistant to get the schema of.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `GraphSchema` | The graph schema for the assistant.<br>**TYPE:**`GraphSchema` |

Example Usage

```
client = get_client(url="http://localhost:2024")
schema = await client.assistants.get_schemas(
    assistant_id="my_assistant_id"
)
print(schema)
```

```
----------------------------------------------------------------------------------------------------------------------------

{
    'graph_id': 'agent',
    'state_schema':
        {
            'title': 'LangGraphInput',
            '$ref': '#/definitions/AgentState',
            'definitions':
                {
                    'BaseMessage':
                        {
                            'title': 'BaseMessage',
                            'description': 'Base abstract Message class. Messages are the inputs and outputs of ChatModels.',
                            'type': 'object',
                            'properties':
                                {
                                 'content':
                                    {
                                        'title': 'Content',
                                        'anyOf': [\
                                            {'type': 'string'},\
                                            {'type': 'array','items': {'anyOf': [{'type': 'string'}, {'type': 'object'}]}}\
                                        ]
                                    },
                                'additional_kwargs':
                                    {
                                        'title': 'Additional Kwargs',
                                        'type': 'object'
                                    },
                                'response_metadata':
                                    {
                                        'title': 'Response Metadata',
                                        'type': 'object'
                                    },
                                'type':
                                    {
                                        'title': 'Type',
                                        'type': 'string'
                                    },
                                'name':
                                    {
                                        'title': 'Name',
                                        'type': 'string'
                                    },
                                'id':
                                    {
                                        'title': 'Id',
                                        'type': 'string'
                                    }
                                },
                            'required': ['content', 'type']
                        },
                    'AgentState':
                        {
                            'title': 'AgentState',
                            'type': 'object',
                            'properties':
                                {
                                    'messages':
                                        {
                                            'title': 'Messages',
                                            'type': 'array',
                                            'items': {'$ref': '#/definitions/BaseMessage'}
                                        }
                                },
                            'required': ['messages']
                        }
                }
        },
    'context_schema':
        {
            'title': 'Context',
            'type': 'object',
            'properties':
                {
                    'model_name':
                        {
                            'title': 'Model Name',
                            'enum': ['anthropic', 'openai'],
                            'type': 'string'
                        }
                }
        }
}
```

#### ``get\_subgraphs`async`¶

```
get_subgraphs(
    assistant_id: str,
    namespace: str | None = None,
    recurse: bool = False,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Subgraphs
```

Get the schemas of an assistant by ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The ID of the assistant to get the schema of.<br>**TYPE:**`str` |
| `namespace` | Optional namespace to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `recurse` | Whether to recursively get subgraphs.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Subgraphs` | The graph schema for the assistant.<br>**TYPE:**`Subgraphs` |

#### ``create`async`¶

```
create(
    graph_id: str | None,
    config: Config | None = None,
    *,
    context: Context | None = None,
    metadata: Json = None,
    assistant_id: str | None = None,
    if_exists: OnConflictBehavior | None = None,
    name: str | None = None,
    headers: Mapping[str, str] | None = None,
    description: str | None = None,
    params: QueryParamTypes | None = None,
) -> Assistant
```

Create a new assistant.

Useful when graph is configurable and you want to create different assistants based on different configurations.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `graph_id` | The ID of the graph the assistant should use. The graph ID is normally set in your langgraph.json configuration.<br>**TYPE:**`str | None` |
| `config` | Configuration to use for the graph.<br>**TYPE:**`Config | None`**DEFAULT:**`None` |
| `metadata` | Metadata to add to assistant.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `context` | Static context to add to the assistant.<br>Added in version 0.6.0<br>**TYPE:**`Context | None`**DEFAULT:**`None` |
| `assistant_id` | Assistant ID to use, will default to a random UUID if not provided.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `if_exists` | How to handle duplicate creation. Defaults to 'raise' under the hood.<br>Must be either 'raise' (raise error if duplicate), or 'do\_nothing' (return existing assistant).<br>**TYPE:**`OnConflictBehavior | None`**DEFAULT:**`None` |
| `name` | The name of the assistant. Defaults to 'Untitled' under the hood.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `description` | Optional description of the assistant.<br>The description field is available for langgraph-api server version>=0.0.45<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Assistant` | The created assistant.<br>**TYPE:**`Assistant` |

Example Usage

```
client = get_client(url="http://localhost:2024")
assistant = await client.assistants.create(
    graph_id="agent",
    context={"model_name": "openai"},
    metadata={"number":1},
    assistant_id="my-assistant-id",
    if_exists="do_nothing",
    name="my_name"
)
```

#### ``update`async`¶

```
update(
    assistant_id: str,
    *,
    graph_id: str | None = None,
    config: Config | None = None,
    context: Context | None = None,
    metadata: Json = None,
    name: str | None = None,
    headers: Mapping[str, str] | None = None,
    description: str | None = None,
    params: QueryParamTypes | None = None,
) -> Assistant
```

Update an assistant.

Use this to point to a different graph, update the configuration, or change the metadata of an assistant.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | Assistant to update.<br>**TYPE:**`str` |
| `graph_id` | The ID of the graph the assistant should use.<br>The graph ID is normally set in your langgraph.json configuration. If `None`, assistant will keep pointing to same graph.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `config` | Configuration to use for the graph.<br>**TYPE:**`Config | None`**DEFAULT:**`None` |
| `context` | Static context to add to the assistant.<br>Added in version 0.6.0<br>**TYPE:**`Context | None`**DEFAULT:**`None` |
| `metadata` | Metadata to merge with existing assistant metadata.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `name` | The new name for the assistant.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `description` | Optional description of the assistant.<br>The description field is available for langgraph-api server version>=0.0.45<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Assistant` | The updated assistant. |

Example Usage

```
client = get_client(url="http://localhost:2024")
assistant = await client.assistants.update(
    assistant_id='e280dad7-8618-443f-87f1-8e41841c180f',
    graph_id="other-graph",
    context={"model_name": "anthropic"},
    metadata={"number":2}
)
```

#### ``delete`async`¶

```
delete(
    assistant_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Delete an assistant.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The assistant ID to delete.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_client(url="http://localhost:2024")
await client.assistants.delete(
    assistant_id="my_assistant_id"
)
```

#### ``search`async`¶

```
search(
    *,
    metadata: Json = None,
    graph_id: str | None = None,
    limit: int = 10,
    offset: int = 0,
    sort_by: AssistantSortBy | None = None,
    sort_order: SortOrder | None = None,
    select: list[AssistantSelectField] | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> list[Assistant]
```

Search for assistants.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `metadata` | Metadata to filter by. Exact match filter for each KV pair.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `graph_id` | The ID of the graph to filter by.<br>The graph ID is normally set in your langgraph.json configuration.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `limit` | The maximum number of results to return.<br>**TYPE:**`int`**DEFAULT:**`10` |
| `offset` | The number of results to skip.<br>**TYPE:**`int`**DEFAULT:**`0` |
| `sort_by` | The field to sort by.<br>**TYPE:**`AssistantSortBy | None`**DEFAULT:**`None` |
| `sort_order` | The order to sort by.<br>**TYPE:**`SortOrder | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Assistant]` | A list of assistants. |

Example Usage

```
client = get_client(url="http://localhost:2024")
assistants = await client.assistants.search(
    metadata = {"name":"my_name"},
    graph_id="my_graph_id",
    limit=5,
    offset=5
)
```

#### ``count`async`¶

```
count(
    *,
    metadata: Json = None,
    graph_id: str | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> int
```

Count assistants matching filters.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `metadata` | Metadata to filter by. Exact match for each key/value.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `graph_id` | Optional graph id to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `int` | Number of assistants matching the criteria.<br>**TYPE:**`int` |

#### ``get\_versions`async`¶

```
get_versions(
    assistant_id: str,
    metadata: Json = None,
    limit: int = 10,
    offset: int = 0,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> list[AssistantVersion]
```

List all versions of an assistant.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The assistant ID to get versions for.<br>**TYPE:**`str` |
| `metadata` | Metadata to filter versions by. Exact match filter for each KV pair.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `limit` | The maximum number of versions to return.<br>**TYPE:**`int`**DEFAULT:**`10` |
| `offset` | The number of versions to skip.<br>**TYPE:**`int`**DEFAULT:**`0` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[AssistantVersion]` | A list of assistant versions. |

Example Usage

```
client = get_client(url="http://localhost:2024")
assistant_versions = await client.assistants.get_versions(
    assistant_id="my_assistant_id"
)
```

#### ``set\_latest`async`¶

```
set_latest(
    assistant_id: str,
    version: int,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Assistant
```

Change the version of an assistant.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The assistant ID to delete.<br>**TYPE:**`str` |
| `version` | The version to change to.<br>**TYPE:**`int` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Assistant` | Assistant Object. |

Example Usage

```
client = get_client(url="http://localhost:2024")
new_version_assistant = await client.assistants.set_latest(
    assistant_id="my_assistant_id",
    version=3
)
```

### ``ThreadsClient ¶

Client for managing threads in LangGraph.

A thread maintains the state of a graph across multiple interactions/invocations (aka runs).
It accumulates and persists the graph's state, allowing for continuity between separate
invocations of the graph.

Example

```
client = get_client(url="http://localhost:2024"))
new_thread = await client.threads.create(metadata={"user_id": "123"})
```

| METHOD | DESCRIPTION |
| --- | --- |
| `get` | Get a thread by ID. |
| `create` | Create a new thread. |
| `update` | Update a thread. |
| `delete` | Delete a thread. |
| `search` | Search for threads. |
| `count` | Count threads matching filters. |
| `copy` | Copy a thread. |
| `get_state` | Get the state of a thread. |
| `update_state` | Update the state of a thread. |
| `get_history` | Get the state history of a thread. |
| `join_stream` | Get a stream of events for a thread. |

#### ``get`async`¶

```
get(
    thread_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Thread
```

Get a thread by ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The ID of the thread to get.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Thread` | Thread object. |

Example Usage

```
client = get_client(url="http://localhost:2024")
thread = await client.threads.get(
    thread_id="my_thread_id"
)
print(thread)
```

```
-----------------------------------------------------

{
    'thread_id': 'my_thread_id',
    'created_at': '2024-07-18T18:35:15.540834+00:00',
    'updated_at': '2024-07-18T18:35:15.540834+00:00',
    'metadata': {'graph_id': 'agent'}
}
```

#### ``create`async`¶

```
create(
    *,
    metadata: Json = None,
    thread_id: str | None = None,
    if_exists: OnConflictBehavior | None = None,
    supersteps: Sequence[dict[str, Sequence[dict[str, Any]]]] | None = None,
    graph_id: str | None = None,
    ttl: int | Mapping[str, Any] | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Thread
```

Create a new thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `metadata` | Metadata to add to thread.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `thread_id` | ID of thread.<br>If `None`, ID will be a randomly generated UUID.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `if_exists` | How to handle duplicate creation. Defaults to 'raise' under the hood.<br>Must be either 'raise' (raise error if duplicate), or 'do\_nothing' (return existing thread).<br>**TYPE:**`OnConflictBehavior | None`**DEFAULT:**`None` |
| `supersteps` | Apply a list of supersteps when creating a thread, each containing a sequence of updates.<br>Each update has `values` or `command` and `as_node`. Used for copying a thread between deployments.<br>**TYPE:**`Sequence[dict[str, Sequence[dict[str, Any]]]] | None`**DEFAULT:**`None` |
| `graph_id` | Optional graph ID to associate with the thread.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `ttl` | Optional time-to-live in minutes for the thread. You can pass an<br>integer (minutes) or a mapping with keys `ttl` and optional<br>`strategy` (defaults to "delete").<br>**TYPE:**`int | Mapping[str, Any] | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Thread` | The created thread. |

Example Usage

```
client = get_client(url="http://localhost:2024")
thread = await client.threads.create(
    metadata={"number":1},
    thread_id="my-thread-id",
    if_exists="raise"
)
```

#### ``update`async`¶

```
update(
    thread_id: str,
    *,
    metadata: Mapping[str, Any],
    ttl: int | Mapping[str, Any] | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Thread
```

Update a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | ID of thread to update.<br>**TYPE:**`str` |
| `metadata` | Metadata to merge with existing thread metadata.<br>**TYPE:**`Mapping[str, Any]` |
| `ttl` | Optional time-to-live in minutes for the thread. You can pass an<br>integer (minutes) or a mapping with keys `ttl` and optional<br>`strategy` (defaults to "delete").<br>**TYPE:**`int | Mapping[str, Any] | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Thread` | The created thread. |

Example Usage

```
client = get_client(url="http://localhost:2024")
thread = await client.threads.update(
    thread_id="my-thread-id",
    metadata={"number":1},
    ttl=43_200,
)
```

#### ``delete`async`¶

```
delete(
    thread_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Delete a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The ID of the thread to delete.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_client(url="http://localhost2024)
await client.threads.delete(
    thread_id="my_thread_id"
)
```

#### ``search`async`¶

```
search(
    *,
    metadata: Json = None,
    values: Json = None,
    ids: Sequence[str] | None = None,
    status: ThreadStatus | None = None,
    limit: int = 10,
    offset: int = 0,
    sort_by: ThreadSortBy | None = None,
    sort_order: SortOrder | None = None,
    select: list[ThreadSelectField] | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> list[Thread]
```

Search for threads.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `metadata` | Thread metadata to filter on.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `values` | State values to filter on.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `ids` | List of thread IDs to filter by.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `status` | Thread status to filter on.<br>Must be one of 'idle', 'busy', 'interrupted' or 'error'.<br>**TYPE:**`ThreadStatus | None`**DEFAULT:**`None` |
| `limit` | Limit on number of threads to return.<br>**TYPE:**`int`**DEFAULT:**`10` |
| `offset` | Offset in threads table to start search from.<br>**TYPE:**`int`**DEFAULT:**`0` |
| `sort_by` | Sort by field.<br>**TYPE:**`ThreadSortBy | None`**DEFAULT:**`None` |
| `sort_order` | Sort order.<br>**TYPE:**`SortOrder | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Thread]` | List of the threads matching the search parameters. |

Example Usage

```
client = get_client(url="http://localhost:2024")
threads = await client.threads.search(
    metadata={"number":1},
    status="interrupted",
    limit=15,
    offset=5
)
```

#### ``count`async`¶

```
count(
    *,
    metadata: Json = None,
    values: Json = None,
    status: ThreadStatus | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> int
```

Count threads matching filters.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `metadata` | Thread metadata to filter on.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `values` | State values to filter on.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `status` | Thread status to filter on.<br>**TYPE:**`ThreadStatus | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `int` | Number of threads matching the criteria.<br>**TYPE:**`int` |

#### ``copy`async`¶

```
copy(
    thread_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Copy a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The ID of the thread to copy.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_client(url="http://localhost:2024)
await client.threads.copy(
    thread_id="my_thread_id"
)
```

#### ``get\_state`async`¶

```
get_state(
    thread_id: str,
    checkpoint: Checkpoint | None = None,
    checkpoint_id: str | None = None,
    *,
    subgraphs: bool = False,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> ThreadState
```

Get the state of a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The ID of the thread to get the state of.<br>**TYPE:**`str` |
| `checkpoint` | The checkpoint to get the state of.<br>**TYPE:**`Checkpoint | None`**DEFAULT:**`None` |
| `checkpoint_id` | (deprecated) The checkpoint ID to get the state of.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `subgraphs` | Include subgraphs states.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ThreadState` | The thread of the state. |

Example Usage

```
client = get_client(url="http://localhost:2024)
thread_state = await client.threads.get_state(
    thread_id="my_thread_id",
    checkpoint_id="my_checkpoint_id"
)
print(thread_state)
```

```
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

{
    'values': {
        'messages': [\
            {\
                'content': 'how are you?',\
                'additional_kwargs': {},\
                'response_metadata': {},\
                'type': 'human',\
                'name': None,\
                'id': 'fe0a5778-cfe9-42ee-b807-0adaa1873c10',\
                'example': False\
            },\
            {\
                'content': "I'm doing well, thanks for asking! I'm an AI assistant created by Anthropic to be helpful, honest, and harmless.",\
                'additional_kwargs': {},\
                'response_metadata': {},\
                'type': 'ai',\
                'name': None,\
                'id': 'run-159b782c-b679-4830-83c6-cef87798fe8b',\
                'example': False,\
                'tool_calls': [],\
                'invalid_tool_calls': [],\
                'usage_metadata': None\
            }\
        ]
    },
    'next': [],
    'checkpoint':
        {
            'thread_id': 'e2496803-ecd5-4e0c-a779-3226296181c2',
            'checkpoint_ns': '',
            'checkpoint_id': '1ef4a9b8-e6fb-67b1-8001-abd5184439d1'
        }
    'metadata':
        {
            'step': 1,
            'run_id': '1ef4a9b8-d7da-679a-a45a-872054341df2',
            'source': 'loop',
            'writes':
                {
                    'agent':
                        {
                            'messages': [\
                                {\
                                    'id': 'run-159b782c-b679-4830-83c6-cef87798fe8b',\
                                    'name': None,\
                                    'type': 'ai',\
                                    'content': "I'm doing well, thanks for asking! I'm an AI assistant created by Anthropic to be helpful, honest, and harmless.",\
                                    'example': False,\
                                    'tool_calls': [],\
                                    'usage_metadata': None,\
                                    'additional_kwargs': {},\
                                    'response_metadata': {},\
                                    'invalid_tool_calls': []\
                                }\
                            ]
                        }
                },
    'user_id': None,
    'graph_id': 'agent',
    'thread_id': 'e2496803-ecd5-4e0c-a779-3226296181c2',
    'created_by': 'system',
    'assistant_id': 'fe096781-5601-53d2-b2f6-0d3403f7e9ca'},
    'created_at': '2024-07-25T15:35:44.184703+00:00',
    'parent_config':
        {
            'thread_id': 'e2496803-ecd5-4e0c-a779-3226296181c2',
            'checkpoint_ns': '',
            'checkpoint_id': '1ef4a9b8-d80d-6fa7-8000-9300467fad0f'
        }
}
```

#### ``update\_state`async`¶

```
update_state(
    thread_id: str,
    values: dict[str, Any] | Sequence[dict] | None,
    *,
    as_node: str | None = None,
    checkpoint: Checkpoint | None = None,
    checkpoint_id: str | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> ThreadUpdateStateResponse
```

Update the state of a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The ID of the thread to update.<br>**TYPE:**`str` |
| `values` | The values to update the state with.<br>**TYPE:**`dict[str, Any] | Sequence[dict] | None` |
| `as_node` | Update the state as if this node had just executed.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `checkpoint` | The checkpoint to update the state of.<br>**TYPE:**`Checkpoint | None`**DEFAULT:**`None` |
| `checkpoint_id` | (deprecated) The checkpoint ID to update the state of.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ThreadUpdateStateResponse` | Response after updating a thread's state. |

Example Usage

```
client = get_client(url="http://localhost:2024)
response = await client.threads.update_state(
    thread_id="my_thread_id",
    values={"messages":[{"role": "user", "content": "hello!"}]},
    as_node="my_node",
)
print(response)
```

```
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

{
    'checkpoint': {
        'thread_id': 'e2496803-ecd5-4e0c-a779-3226296181c2',
        'checkpoint_ns': '',
        'checkpoint_id': '1ef4a9b8-e6fb-67b1-8001-abd5184439d1',
        'checkpoint_map': {}
    }
}
```

#### ``get\_history`async`¶

```
get_history(
    thread_id: str,
    *,
    limit: int = 10,
    before: str | Checkpoint | None = None,
    metadata: Mapping[str, Any] | None = None,
    checkpoint: Checkpoint | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> list[ThreadState]
```

Get the state history of a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The ID of the thread to get the state history for.<br>**TYPE:**`str` |
| `checkpoint` | Return states for this subgraph. If empty defaults to root.<br>**TYPE:**`Checkpoint | None`**DEFAULT:**`None` |
| `limit` | The maximum number of states to return.<br>**TYPE:**`int`**DEFAULT:**`10` |
| `before` | Return states before this checkpoint.<br>**TYPE:**`str | Checkpoint | None`**DEFAULT:**`None` |
| `metadata` | Filter states by metadata key-value pairs.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[ThreadState]` | The state history of the thread. |

Example Usage

```
client = get_client(url="http://localhost:2024)
thread_state = await client.threads.get_history(
    thread_id="my_thread_id",
    limit=5,
)
```

#### ``join\_stream`async`¶

```
join_stream(
    thread_id: str,
    *,
    last_event_id: str | None = None,
    stream_mode: ThreadStreamMode | Sequence[ThreadStreamMode] = "run_modes",
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> AsyncIterator[StreamPart]
```

Get a stream of events for a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The ID of the thread to get the stream for.<br>**TYPE:**`str` |
| `last_event_id` | The ID of the last event to get.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[StreamPart]` | An iterator of stream parts. |

Example Usage

```
for chunk in client.threads.join_stream(
    thread_id="my_thread_id",
    last_event_id="my_event_id",
):
    print(chunk)
```

### ``RunsClient ¶

Client for managing runs in LangGraph.

A run is a single assistant invocation with optional input, config, context, and metadata.
This client manages runs, which can be stateful (on threads) or stateless.

Example

```
client = get_client(url="http://localhost:2024")
run = await client.runs.create(assistant_id="asst_123", thread_id="thread_456", input={"query": "Hello"})
```

| METHOD | DESCRIPTION |
| --- | --- |
| `stream` | Create a run and stream the results. |
| `create` | Create a background run. |
| `create_batch` | Create a batch of stateless background runs. |
| `wait` | Create a run, wait until it finishes and return the final state. |
| `list` | List runs. |
| `get` | Get a run. |
| `cancel` | Get a run. |
| `join` | Block until a run is done. Returns the final state of the thread. |
| `join_stream` | Stream output from a run in real-time, until the run is done. |
| `delete` | Delete a run. |

#### ``stream ¶

```
stream(
    thread_id: str | None,
    assistant_id: str,
    *,
    input: Mapping[str, Any] | None = None,
    command: Command | None = None,
    stream_mode: StreamMode | Sequence[StreamMode] = "values",
    stream_subgraphs: bool = False,
    stream_resumable: bool = False,
    metadata: Mapping[str, Any] | None = None,
    config: Config | None = None,
    context: Context | None = None,
    checkpoint: Checkpoint | None = None,
    checkpoint_id: str | None = None,
    checkpoint_during: bool | None = None,
    interrupt_before: All | Sequence[str] | None = None,
    interrupt_after: All | Sequence[str] | None = None,
    feedback_keys: Sequence[str] | None = None,
    on_disconnect: DisconnectMode | None = None,
    on_completion: OnCompletionBehavior | None = None,
    webhook: str | None = None,
    multitask_strategy: MultitaskStrategy | None = None,
    if_not_exists: IfNotExists | None = None,
    after_seconds: int | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
    on_run_created: Callable[[RunCreateMetadata], None] | None = None,
    durability: Durability | None = None,
) -> AsyncIterator[StreamPart]
```

Create a run and stream the results.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | the thread ID to assign to the thread.<br>If `None` will create a stateless run.<br>**TYPE:**`str | None` |
| `assistant_id` | The assistant ID or graph name to stream from.<br>If using graph name, will default to first assistant created from that graph.<br>**TYPE:**`str` |
| `input` | The input to the graph.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `command` | A command to execute. Cannot be combined with input.<br>**TYPE:**`Command | None`**DEFAULT:**`None` |
| `stream_mode` | The stream mode(s) to use.<br>**TYPE:**`StreamMode | Sequence[StreamMode]`**DEFAULT:**`'values'` |
| `stream_subgraphs` | Whether to stream output from subgraphs.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `stream_resumable` | Whether the stream is considered resumable.<br>If true, the stream can be resumed and replayed in its entirety even after disconnection.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `metadata` | Metadata to assign to the run.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `config` | The configuration for the assistant.<br>**TYPE:**`Config | None`**DEFAULT:**`None` |
| `context` | Static context to add to the assistant.<br>Added in version 0.6.0<br>**TYPE:**`Context | None`**DEFAULT:**`None` |
| `checkpoint` | The checkpoint to resume from.<br>**TYPE:**`Checkpoint | None`**DEFAULT:**`None` |
| `checkpoint_during` | (deprecated) Whether to checkpoint during the run (or only at the end/interruption).<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `interrupt_before` | Nodes to interrupt immediately before they get executed.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | Nodes to Nodes to interrupt immediately after they get executed.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `feedback_keys` | Feedback keys to assign to run.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `on_disconnect` | The disconnect mode to use.<br>Must be one of 'cancel' or 'continue'.<br>**TYPE:**`DisconnectMode | None`**DEFAULT:**`None` |
| `on_completion` | Whether to delete or keep the thread created for a stateless run.<br>Must be one of 'delete' or 'keep'.<br>**TYPE:**`OnCompletionBehavior | None`**DEFAULT:**`None` |
| `webhook` | Webhook to call after LangGraph API call is done.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `multitask_strategy` | Multitask strategy to use.<br>Must be one of 'reject', 'interrupt', 'rollback', or 'enqueue'.<br>**TYPE:**`MultitaskStrategy | None`**DEFAULT:**`None` |
| `if_not_exists` | How to handle missing thread. Defaults to 'reject'.<br>Must be either 'reject' (raise error if missing), or 'create' (create new thread).<br>**TYPE:**`IfNotExists | None`**DEFAULT:**`None` |
| `after_seconds` | The number of seconds to wait before starting the run.<br>Use to schedule future runs.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |
| `on_run_created` | Callback when a run is created.<br>**TYPE:**`Callable[[RunCreateMetadata], None] | None`**DEFAULT:**`None` |
| `durability` | The durability to use for the run. Values are "sync", "async", or "exit".<br>"async" means checkpoints are persisted async while next graph step executes, replaces checkpoint\_during=True<br>"sync" means checkpoints are persisted sync after graph step executes, replaces checkpoint\_during=False<br>"exit" means checkpoints are only persisted when the run exits, does not save intermediate steps<br>**TYPE:**`Durability | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[StreamPart]` | Asynchronous iterator of stream results. |

Example Usage

```
client = get_client(url="http://localhost:2024)
async for chunk in client.runs.stream(
    thread_id=None,
    assistant_id="agent",
    input={"messages": [{"role": "user", "content": "how are you?"}]},
    stream_mode=["values","debug"],
    metadata={"name":"my_run"},
    context={"model_name": "anthropic"},
    interrupt_before=["node_to_stop_before_1","node_to_stop_before_2"],
    interrupt_after=["node_to_stop_after_1","node_to_stop_after_2"],
    feedback_keys=["my_feedback_key_1","my_feedback_key_2"],
    webhook="https://my.fake.webhook.com",
    multitask_strategy="interrupt"
):
    print(chunk)
```

```
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

StreamPart(event='metadata', data={'run_id': '1ef4a9b8-d7da-679a-a45a-872054341df2'})
StreamPart(event='values', data={'messages': [{'content': 'how are you?', 'additional_kwargs': {}, 'response_metadata': {}, 'type': 'human', 'name': None, 'id': 'fe0a5778-cfe9-42ee-b807-0adaa1873c10', 'example': False}]})
StreamPart(event='values', data={'messages': [{'content': 'how are you?', 'additional_kwargs': {}, 'response_metadata': {}, 'type': 'human', 'name': None, 'id': 'fe0a5778-cfe9-42ee-b807-0adaa1873c10', 'example': False}, {'content': "I'm doing well, thanks for asking! I'm an AI assistant created by Anthropic to be helpful, honest, and harmless.", 'additional_kwargs': {}, 'response_metadata': {}, 'type': 'ai', 'name': None, 'id': 'run-159b782c-b679-4830-83c6-cef87798fe8b', 'example': False, 'tool_calls': [], 'invalid_tool_calls': [], 'usage_metadata': None}]})
StreamPart(event='end', data=None)
```

#### ``create`async`¶

```
create(
    thread_id: str | None,
    assistant_id: str,
    *,
    input: Mapping[str, Any] | None = None,
    command: Command | None = None,
    stream_mode: StreamMode | Sequence[StreamMode] = "values",
    stream_subgraphs: bool = False,
    stream_resumable: bool = False,
    metadata: Mapping[str, Any] | None = None,
    config: Config | None = None,
    context: Context | None = None,
    checkpoint: Checkpoint | None = None,
    checkpoint_id: str | None = None,
    checkpoint_during: bool | None = None,
    interrupt_before: All | Sequence[str] | None = None,
    interrupt_after: All | Sequence[str] | None = None,
    webhook: str | None = None,
    multitask_strategy: MultitaskStrategy | None = None,
    if_not_exists: IfNotExists | None = None,
    on_completion: OnCompletionBehavior | None = None,
    after_seconds: int | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
    on_run_created: Callable[[RunCreateMetadata], None] | None = None,
    durability: Durability | None = None,
) -> Run
```

Create a background run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | the thread ID to assign to the thread.<br>If `None` will create a stateless run.<br>**TYPE:**`str | None` |
| `assistant_id` | The assistant ID or graph name to stream from.<br>If using graph name, will default to first assistant created from that graph.<br>**TYPE:**`str` |
| `input` | The input to the graph.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `command` | A command to execute. Cannot be combined with input.<br>**TYPE:**`Command | None`**DEFAULT:**`None` |
| `stream_mode` | The stream mode(s) to use.<br>**TYPE:**`StreamMode | Sequence[StreamMode]`**DEFAULT:**`'values'` |
| `stream_subgraphs` | Whether to stream output from subgraphs.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `stream_resumable` | Whether the stream is considered resumable.<br>If true, the stream can be resumed and replayed in its entirety even after disconnection.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `metadata` | Metadata to assign to the run.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `config` | The configuration for the assistant.<br>**TYPE:**`Config | None`**DEFAULT:**`None` |
| `context` | Static context to add to the assistant.<br>Added in version 0.6.0<br>**TYPE:**`Context | None`**DEFAULT:**`None` |
| `checkpoint` | The checkpoint to resume from.<br>**TYPE:**`Checkpoint | None`**DEFAULT:**`None` |
| `checkpoint_during` | (deprecated) Whether to checkpoint during the run (or only at the end/interruption).<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `interrupt_before` | Nodes to interrupt immediately before they get executed.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | Nodes to Nodes to interrupt immediately after they get executed.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `webhook` | Webhook to call after LangGraph API call is done.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `multitask_strategy` | Multitask strategy to use.<br>Must be one of 'reject', 'interrupt', 'rollback', or 'enqueue'.<br>**TYPE:**`MultitaskStrategy | None`**DEFAULT:**`None` |
| `on_completion` | Whether to delete or keep the thread created for a stateless run.<br>Must be one of 'delete' or 'keep'.<br>**TYPE:**`OnCompletionBehavior | None`**DEFAULT:**`None` |
| `if_not_exists` | How to handle missing thread. Defaults to 'reject'.<br>Must be either 'reject' (raise error if missing), or 'create' (create new thread).<br>**TYPE:**`IfNotExists | None`**DEFAULT:**`None` |
| `after_seconds` | The number of seconds to wait before starting the run.<br>Use to schedule future runs.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `on_run_created` | Optional callback to call when a run is created.<br>**TYPE:**`Callable[[RunCreateMetadata], None] | None`**DEFAULT:**`None` |
| `durability` | The durability to use for the run. Values are "sync", "async", or "exit".<br>"async" means checkpoints are persisted async while next graph step executes, replaces checkpoint\_during=True<br>"sync" means checkpoints are persisted sync after graph step executes, replaces checkpoint\_during=False<br>"exit" means checkpoints are only persisted when the run exits, does not save intermediate steps<br>**TYPE:**`Durability | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Run` | The created background run. |

Example Usage

```
background_run = await client.runs.create(
    thread_id="my_thread_id",
    assistant_id="my_assistant_id",
    input={"messages": [{"role": "user", "content": "hello!"}]},
    metadata={"name":"my_run"},
    context={"model_name": "openai"},
    interrupt_before=["node_to_stop_before_1","node_to_stop_before_2"],
    interrupt_after=["node_to_stop_after_1","node_to_stop_after_2"],
    webhook="https://my.fake.webhook.com",
    multitask_strategy="interrupt"
)
print(background_run)
```

```
--------------------------------------------------------------------------------

{
    'run_id': 'my_run_id',
    'thread_id': 'my_thread_id',
    'assistant_id': 'my_assistant_id',
    'created_at': '2024-07-25T15:35:42.598503+00:00',
    'updated_at': '2024-07-25T15:35:42.598503+00:00',
    'metadata': {},
    'status': 'pending',
    'kwargs':
        {
            'input':
                {
                    'messages': [\
                        {\
                            'role': 'user',\
                            'content': 'how are you?'\
                        }\
                    ]
                },
            'config':
                {
                    'metadata':
                        {
                            'created_by': 'system'
                        },
                    'configurable':
                        {
                            'run_id': 'my_run_id',
                            'user_id': None,
                            'graph_id': 'agent',
                            'thread_id': 'my_thread_id',
                            'checkpoint_id': None,
                            'assistant_id': 'my_assistant_id'
                        },
                },
            'context':
                {
                    'model_name': 'openai'
                }
            'webhook': "https://my.fake.webhook.com",
            'temporary': False,
            'stream_mode': ['values'],
            'feedback_keys': None,
            'interrupt_after': ["node_to_stop_after_1","node_to_stop_after_2"],
            'interrupt_before': ["node_to_stop_before_1","node_to_stop_before_2"]
        },
    'multitask_strategy': 'interrupt'
}
```

#### ``create\_batch`async`¶

```
create_batch(
    payloads: list[RunCreate],
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> list[Run]
```

Create a batch of stateless background runs.

#### ``wait`async`¶

```
wait(
    thread_id: str | None,
    assistant_id: str,
    *,
    input: Mapping[str, Any] | None = None,
    command: Command | None = None,
    metadata: Mapping[str, Any] | None = None,
    config: Config | None = None,
    context: Context | None = None,
    checkpoint: Checkpoint | None = None,
    checkpoint_id: str | None = None,
    checkpoint_during: bool | None = None,
    interrupt_before: All | Sequence[str] | None = None,
    interrupt_after: All | Sequence[str] | None = None,
    webhook: str | None = None,
    on_disconnect: DisconnectMode | None = None,
    on_completion: OnCompletionBehavior | None = None,
    multitask_strategy: MultitaskStrategy | None = None,
    if_not_exists: IfNotExists | None = None,
    after_seconds: int | None = None,
    raise_error: bool = True,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
    on_run_created: Callable[[RunCreateMetadata], None] | None = None,
    durability: Durability | None = None,
) -> list[dict] | dict[str, Any]
```

Create a run, wait until it finishes and return the final state.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | the thread ID to create the run on.<br>If `None` will create a stateless run.<br>**TYPE:**`str | None` |
| `assistant_id` | The assistant ID or graph name to run.<br>If using graph name, will default to first assistant created from that graph.<br>**TYPE:**`str` |
| `input` | The input to the graph.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `command` | A command to execute. Cannot be combined with input.<br>**TYPE:**`Command | None`**DEFAULT:**`None` |
| `metadata` | Metadata to assign to the run.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `config` | The configuration for the assistant.<br>**TYPE:**`Config | None`**DEFAULT:**`None` |
| `context` | Static context to add to the assistant.<br>Added in version 0.6.0<br>**TYPE:**`Context | None`**DEFAULT:**`None` |
| `checkpoint` | The checkpoint to resume from.<br>**TYPE:**`Checkpoint | None`**DEFAULT:**`None` |
| `checkpoint_during` | (deprecated) Whether to checkpoint during the run (or only at the end/interruption).<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `interrupt_before` | Nodes to interrupt immediately before they get executed.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | Nodes to Nodes to interrupt immediately after they get executed.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `webhook` | Webhook to call after LangGraph API call is done.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `on_disconnect` | The disconnect mode to use.<br>Must be one of 'cancel' or 'continue'.<br>**TYPE:**`DisconnectMode | None`**DEFAULT:**`None` |
| `on_completion` | Whether to delete or keep the thread created for a stateless run.<br>Must be one of 'delete' or 'keep'.<br>**TYPE:**`OnCompletionBehavior | None`**DEFAULT:**`None` |
| `multitask_strategy` | Multitask strategy to use.<br>Must be one of 'reject', 'interrupt', 'rollback', or 'enqueue'.<br>**TYPE:**`MultitaskStrategy | None`**DEFAULT:**`None` |
| `if_not_exists` | How to handle missing thread. Defaults to 'reject'.<br>Must be either 'reject' (raise error if missing), or 'create' (create new thread).<br>**TYPE:**`IfNotExists | None`**DEFAULT:**`None` |
| `after_seconds` | The number of seconds to wait before starting the run.<br>Use to schedule future runs.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `on_run_created` | Optional callback to call when a run is created.<br>**TYPE:**`Callable[[RunCreateMetadata], None] | None`**DEFAULT:**`None` |
| `durability` | The durability to use for the run. Values are "sync", "async", or "exit".<br>"async" means checkpoints are persisted async while next graph step executes, replaces checkpoint\_during=True<br>"sync" means checkpoints are persisted sync after graph step executes, replaces checkpoint\_during=False<br>"exit" means checkpoints are only persisted when the run exits, does not save intermediate steps<br>**TYPE:**`Durability | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[dict] | dict[str, Any]` | The output of the run. |

Example Usage

```
client = get_client(url="http://localhost:2024")
final_state_of_run = await client.runs.wait(
    thread_id=None,
    assistant_id="agent",
    input={"messages": [{"role": "user", "content": "how are you?"}]},
    metadata={"name":"my_run"},
    context={"model_name": "anthropic"},
    interrupt_before=["node_to_stop_before_1","node_to_stop_before_2"],
    interrupt_after=["node_to_stop_after_1","node_to_stop_after_2"],
    webhook="https://my.fake.webhook.com",
    multitask_strategy="interrupt"
)
print(final_state_of_run)
```

```
-------------------------------------------------------------------------------------------------------------------------------------------

{
    'messages': [\
        {\
            'content': 'how are you?',\
            'additional_kwargs': {},\
            'response_metadata': {},\
            'type': 'human',\
            'name': None,\
            'id': 'f51a862c-62fe-4866-863b-b0863e8ad78a',\
            'example': False\
        },\
        {\
            'content': "I'm doing well, thanks for asking! I'm an AI assistant created by Anthropic to be helpful, honest, and harmless.",\
            'additional_kwargs': {},\
            'response_metadata': {},\
            'type': 'ai',\
            'name': None,\
            'id': 'run-bf1cd3c6-768f-4c16-b62d-ba6f17ad8b36',\
            'example': False,\
            'tool_calls': [],\
            'invalid_tool_calls': [],\
            'usage_metadata': None\
        }\
    ]
}
```

#### ``list`async`¶

```
list(
    thread_id: str,
    *,
    limit: int = 10,
    offset: int = 0,
    status: RunStatus | None = None,
    select: list[RunSelectField] | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> list[Run]
```

List runs.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to list runs for.<br>**TYPE:**`str` |
| `limit` | The maximum number of results to return.<br>**TYPE:**`int`**DEFAULT:**`10` |
| `offset` | The number of results to skip.<br>**TYPE:**`int`**DEFAULT:**`0` |
| `status` | The status of the run to filter by.<br>**TYPE:**`RunStatus | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Run]` | The runs for the thread. |

Example Usage

```
client = get_client(url="http://localhost:2024")
await client.runs.list(
    thread_id="thread_id",
    limit=5,
    offset=5,
)
```

#### ``get`async`¶

```
get(
    thread_id: str,
    run_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Run
```

Get a run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to get.<br>**TYPE:**`str` |
| `run_id` | The run ID to get.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Run` | `Run` object. |

Example Usage

```
client = get_client(url="http://localhost:2024")
run = await client.runs.get(
    thread_id="thread_id_to_delete",
    run_id="run_id_to_delete",
)
```

#### ``cancel`async`¶

```
cancel(
    thread_id: str,
    run_id: str,
    *,
    wait: bool = False,
    action: CancelAction = "interrupt",
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Get a run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to cancel.<br>**TYPE:**`str` |
| `run_id` | The run ID to cancel.<br>**TYPE:**`str` |
| `wait` | Whether to wait until run has completed.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `action` | Action to take when cancelling the run. Possible values<br>are `interrupt` or `rollback`. Default is `interrupt`.<br>**TYPE:**`CancelAction`**DEFAULT:**`'interrupt'` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_client(url="http://localhost:2024")
await client.runs.cancel(
    thread_id="thread_id_to_cancel",
    run_id="run_id_to_cancel",
    wait=True,
    action="interrupt"
)
```

#### ``join`async`¶

```
join(
    thread_id: str,
    run_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> dict
```

Block until a run is done. Returns the final state of the thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to join.<br>**TYPE:**`str` |
| `run_id` | The run ID to join.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict` | `None` |

Example Usage

```
client = get_client(url="http://localhost:2024")
result =await client.runs.join(
    thread_id="thread_id_to_join",
    run_id="run_id_to_join"
)
```

#### ``join\_stream ¶

```
join_stream(
    thread_id: str,
    run_id: str,
    *,
    cancel_on_disconnect: bool = False,
    stream_mode: StreamMode | Sequence[StreamMode] | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
    last_event_id: str | None = None,
) -> AsyncIterator[StreamPart]
```

Stream output from a run in real-time, until the run is done.
Output is not buffered, so any output produced before this call will
not be received here.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to join.<br>**TYPE:**`str` |
| `run_id` | The run ID to join.<br>**TYPE:**`str` |
| `cancel_on_disconnect` | Whether to cancel the run when the stream is disconnected.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `stream_mode` | The stream mode(s) to use. Must be a subset of the stream modes passed<br>when creating the run. Background runs default to having the union of all<br>stream modes.<br>**TYPE:**`StreamMode | Sequence[StreamMode] | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |
| `last_event_id` | The last event ID to use for the stream.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[StreamPart]` | The stream of parts. |

Example Usage

```
client = get_client(url="http://localhost:2024")
async for part in client.runs.join_stream(
    thread_id="thread_id_to_join",
    run_id="run_id_to_join",
    stream_mode=["values", "debug"]
):
    print(part)
```

#### ``delete`async`¶

```
delete(
    thread_id: str,
    run_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Delete a run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to delete.<br>**TYPE:**`str` |
| `run_id` | The run ID to delete.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_client(url="http://localhost:2024")
await client.runs.delete(
    thread_id="thread_id_to_delete",
    run_id="run_id_to_delete"
)
```

### ``CronClient ¶

Client for managing recurrent runs (cron jobs) in LangGraph.

A run is a single invocation of an assistant with optional input, config, and context.
This client allows scheduling recurring runs to occur automatically.

Example Usage

```
client = get_client(url="http://localhost:2024"))
cron_job = await client.crons.create_for_thread(
    thread_id="thread_123",
    assistant_id="asst_456",
    schedule="0 9 * * *",
    input={"message": "Daily update"}
)
```

Feature Availability

The crons client functionality is not supported on all licenses.
Please check the relevant license documentation for the most up-to-date
details on feature availability.

| METHOD | DESCRIPTION |
| --- | --- |
| `create_for_thread` | Create a cron job for a thread. |
| `create` | Create a cron run. |
| `delete` | Delete a cron. |
| `search` | Get a list of cron jobs. |
| `count` | Count cron jobs matching filters. |

#### ``create\_for\_thread`async`¶

```
create_for_thread(
    thread_id: str,
    assistant_id: str,
    *,
    schedule: str,
    input: Mapping[str, Any] | None = None,
    metadata: Mapping[str, Any] | None = None,
    config: Config | None = None,
    context: Context | None = None,
    checkpoint_during: bool | None = None,
    interrupt_before: All | list[str] | None = None,
    interrupt_after: All | list[str] | None = None,
    webhook: str | None = None,
    multitask_strategy: str | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Run
```

Create a cron job for a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | the thread ID to run the cron job on.<br>**TYPE:**`str` |
| `assistant_id` | The assistant ID or graph name to use for the cron job.<br>If using graph name, will default to first assistant created from that graph.<br>**TYPE:**`str` |
| `schedule` | The cron schedule to execute this job on.<br>**TYPE:**`str` |
| `input` | The input to the graph.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `metadata` | Metadata to assign to the cron job runs.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `config` | The configuration for the assistant.<br>**TYPE:**`Config | None`**DEFAULT:**`None` |
| `context` | Static context to add to the assistant.<br>Added in version 0.6.0<br>**TYPE:**`Context | None`**DEFAULT:**`None` |
| `checkpoint_during` | Whether to checkpoint during the run (or only at the end/interruption).<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `interrupt_before` | Nodes to interrupt immediately before they get executed.<br>**TYPE:**`All | list[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | Nodes to Nodes to interrupt immediately after they get executed.<br>**TYPE:**`All | list[str] | None`**DEFAULT:**`None` |
| `webhook` | Webhook to call after LangGraph API call is done.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `multitask_strategy` | Multitask strategy to use.<br>Must be one of 'reject', 'interrupt', 'rollback', or 'enqueue'.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Run` | The cron run. |

Example Usage

```
client = get_client(url="http://localhost:2024")
cron_run = await client.crons.create_for_thread(
    thread_id="my-thread-id",
    assistant_id="agent",
    schedule="27 15 * * *",
    input={"messages": [{"role": "user", "content": "hello!"}]},
    metadata={"name":"my_run"},
    context={"model_name": "openai"},
    interrupt_before=["node_to_stop_before_1","node_to_stop_before_2"],
    interrupt_after=["node_to_stop_after_1","node_to_stop_after_2"],
    webhook="https://my.fake.webhook.com",
    multitask_strategy="interrupt"
)
```

#### ``create`async`¶

```
create(
    assistant_id: str,
    *,
    schedule: str,
    input: Mapping[str, Any] | None = None,
    metadata: Mapping[str, Any] | None = None,
    config: Config | None = None,
    context: Context | None = None,
    checkpoint_during: bool | None = None,
    interrupt_before: All | list[str] | None = None,
    interrupt_after: All | list[str] | None = None,
    webhook: str | None = None,
    multitask_strategy: str | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Run
```

Create a cron run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The assistant ID or graph name to use for the cron job.<br>If using graph name, will default to first assistant created from that graph.<br>**TYPE:**`str` |
| `schedule` | The cron schedule to execute this job on.<br>**TYPE:**`str` |
| `input` | The input to the graph.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `metadata` | Metadata to assign to the cron job runs.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `config` | The configuration for the assistant.<br>**TYPE:**`Config | None`**DEFAULT:**`None` |
| `context` | Static context to add to the assistant.<br>Added in version 0.6.0<br>**TYPE:**`Context | None`**DEFAULT:**`None` |
| `checkpoint_during` | Whether to checkpoint during the run (or only at the end/interruption).<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `interrupt_before` | Nodes to interrupt immediately before they get executed.<br>**TYPE:**`All | list[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | Nodes to Nodes to interrupt immediately after they get executed.<br>**TYPE:**`All | list[str] | None`**DEFAULT:**`None` |
| `webhook` | Webhook to call after LangGraph API call is done.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `multitask_strategy` | Multitask strategy to use.<br>Must be one of 'reject', 'interrupt', 'rollback', or 'enqueue'.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Run` | The cron run. |

Example Usage

```
client = get_client(url="http://localhost:2024")
cron_run = client.crons.create(
    assistant_id="agent",
    schedule="27 15 * * *",
    input={"messages": [{"role": "user", "content": "hello!"}]},
    metadata={"name":"my_run"},
    context={"model_name": "openai"},
    interrupt_before=["node_to_stop_before_1","node_to_stop_before_2"],
    interrupt_after=["node_to_stop_after_1","node_to_stop_after_2"],
    webhook="https://my.fake.webhook.com",
    multitask_strategy="interrupt"
)
```

#### ``delete`async`¶

```
delete(
    cron_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Delete a cron.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `cron_id` | The cron ID to delete.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_client(url="http://localhost:2024")
await client.crons.delete(
    cron_id="cron_to_delete"
)
```

#### ``search`async`¶

```
search(
    *,
    assistant_id: str | None = None,
    thread_id: str | None = None,
    limit: int = 10,
    offset: int = 0,
    sort_by: CronSortBy | None = None,
    sort_order: SortOrder | None = None,
    select: list[CronSelectField] | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> list[Cron]
```

Get a list of cron jobs.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The assistant ID or graph name to search for.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `thread_id` | the thread ID to search for.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `limit` | The maximum number of results to return.<br>**TYPE:**`int`**DEFAULT:**`10` |
| `offset` | The number of results to skip.<br>**TYPE:**`int`**DEFAULT:**`0` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Cron]` | The list of cron jobs returned by the search, |

Example Usage

```
client = get_client(url="http://localhost:2024")
cron_jobs = await client.crons.search(
    assistant_id="my_assistant_id",
    thread_id="my_thread_id",
    limit=5,
    offset=5,
)
print(cron_jobs)
```

```
----------------------------------------------------------

[\
    {\
        'cron_id': '1ef3cefa-4c09-6926-96d0-3dc97fd5e39b',\
        'assistant_id': 'my_assistant_id',\
        'thread_id': 'my_thread_id',\
        'user_id': None,\
        'payload':\
            {\
                'input': {'start_time': ''},\
                'schedule': '4 * * * *',\
                'assistant_id': 'my_assistant_id'\
            },\
        'schedule': '4 * * * *',\
        'next_run_date': '2024-07-25T17:04:00+00:00',\
        'end_time': None,\
        'created_at': '2024-07-08T06:02:23.073257+00:00',\
        'updated_at': '2024-07-08T06:02:23.073257+00:00'\
    }\
]
```

#### ``count`async`¶

```
count(
    *,
    assistant_id: str | None = None,
    thread_id: str | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> int
```

Count cron jobs matching filters.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | Assistant ID to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `thread_id` | Thread ID to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `int` | Number of crons matching the criteria.<br>**TYPE:**`int` |

### ``StoreClient ¶

Client for interacting with the graph's shared storage.

The Store provides a key-value storage system for persisting data across graph executions,
allowing for stateful operations and data sharing across threads.

Example

```
client = get_client(url="http://localhost:2024")
await client.store.put_item(["users", "user123"], "mem-123451342", {"name": "Alice", "score": 100})
```

| METHOD | DESCRIPTION |
| --- | --- |
| `put_item` | Store or update an item. |
| `get_item` | Retrieve a single item. |
| `delete_item` | Delete an item. |
| `search_items` | Search for items within a namespace prefix. |
| `list_namespaces` | List namespaces with optional match conditions. |

#### ``put\_item`async`¶

```
put_item(
    namespace: Sequence[str],
    /,
    key: str,
    value: Mapping[str, Any],
    index: Literal[False] | list[str] | None = None,
    ttl: int | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Store or update an item.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `namespace` | A list of strings representing the namespace path.<br>**TYPE:**`Sequence[str]` |
| `key` | The unique identifier for the item within the namespace.<br>**TYPE:**`str` |
| `value` | A dictionary containing the item's data.<br>**TYPE:**`Mapping[str, Any]` |
| `index` | Controls search indexing - None (use defaults), False (disable), or list of field paths to index.<br>**TYPE:**`Literal[False] | list[str] | None`**DEFAULT:**`None` |
| `ttl` | Optional time-to-live in minutes for the item, or None for no expiration.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_client(url="http://localhost:2024")
await client.store.put_item(
    ["documents", "user123"],
    key="item456",
    value={"title": "My Document", "content": "Hello World"}
)
```

#### ``get\_item`async`¶

```
get_item(
    namespace: Sequence[str],
    /,
    key: str,
    *,
    refresh_ttl: bool | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Item
```

Retrieve a single item.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `key` | The unique identifier for the item.<br>**TYPE:**`str` |
| `namespace` | Optional list of strings representing the namespace path.<br>**TYPE:**`Sequence[str]` |
| `refresh_ttl` | Whether to refresh the TTL on this read operation. If `None`, uses the store's default behavior.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Item` | The retrieved item.<br>**TYPE:**`Item` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Item` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`Item` |

Example Usage

```
client = get_client(url="http://localhost:2024")
item = await client.store.get_item(
    ["documents", "user123"],
    key="item456",
)
print(item)
```

```
----------------------------------------------------------------

{
    'namespace': ['documents', 'user123'],
    'key': 'item456',
    'value': {'title': 'My Document', 'content': 'Hello World'},
    'created_at': '2024-07-30T12:00:00Z',
    'updated_at': '2024-07-30T12:00:00Z'
}
```

#### ``delete\_item`async`¶

```
delete_item(
    namespace: Sequence[str],
    /,
    key: str,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Delete an item.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `key` | The unique identifier for the item.<br>**TYPE:**`str` |
| `namespace` | Optional list of strings representing the namespace path.<br>**TYPE:**`Sequence[str]` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_client(url="http://localhost:2024")
await client.store.delete_item(
    ["documents", "user123"],
    key="item456",
)
```

#### ``search\_items`async`¶

```
search_items(
    namespace_prefix: Sequence[str],
    /,
    filter: Mapping[str, Any] | None = None,
    limit: int = 10,
    offset: int = 0,
    query: str | None = None,
    refresh_ttl: bool | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> SearchItemsResponse
```

Search for items within a namespace prefix.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `namespace_prefix` | List of strings representing the namespace prefix.<br>**TYPE:**`Sequence[str]` |
| `filter` | Optional dictionary of key-value pairs to filter results.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `limit` | Maximum number of items to return (default is 10).<br>**TYPE:**`int`**DEFAULT:**`10` |
| `offset` | Number of items to skip before returning results (default is 0).<br>**TYPE:**`int`**DEFAULT:**`0` |
| `query` | Optional query for natural language search.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `refresh_ttl` | Whether to refresh the TTL on items returned by this search. If `None`, uses the store's default behavior.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `SearchItemsResponse` | A list of items matching the search criteria. |

Example Usage

```
client = get_client(url="http://localhost:2024")
items = await client.store.search_items(
    ["documents"],
    filter={"author": "John Doe"},
    limit=5,
    offset=0
)
print(items)
```

```
----------------------------------------------------------------

{
    "items": [\
        {\
            "namespace": ["documents", "user123"],\
            "key": "item789",\
            "value": {\
                "title": "Another Document",\
                "author": "John Doe"\
            },\
            "created_at": "2024-07-30T12:00:00Z",\
            "updated_at": "2024-07-30T12:00:00Z"\
        },\
        # ... additional items ...\
    ]
}
```

#### ``list\_namespaces`async`¶

```
list_namespaces(
    prefix: list[str] | None = None,
    suffix: list[str] | None = None,
    max_depth: int | None = None,
    limit: int = 100,
    offset: int = 0,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> ListNamespaceResponse
```

List namespaces with optional match conditions.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `prefix` | Optional list of strings representing the prefix to filter namespaces.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `suffix` | Optional list of strings representing the suffix to filter namespaces.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `max_depth` | Optional integer specifying the maximum depth of namespaces to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `limit` | Maximum number of namespaces to return (default is 100).<br>**TYPE:**`int`**DEFAULT:**`100` |
| `offset` | Number of namespaces to skip before returning results (default is 0).<br>**TYPE:**`int`**DEFAULT:**`0` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ListNamespaceResponse` | A list of namespaces matching the criteria. |

Example Usage

```
client = get_client(url="http://localhost:2024")
namespaces = await client.store.list_namespaces(
    prefix=["documents"],
    max_depth=3,
    limit=10,
    offset=0
)
print(namespaces)

----------------------------------------------------------------

[\
    ["documents", "user123", "reports"],\
    ["documents", "user456", "invoices"],\
    ...\
]
```

### ``SyncLangGraphClient ¶

Synchronous client for interacting with the LangGraph API.

This class provides synchronous access to LangGraph API endpoints for managing
assistants, threads, runs, cron jobs, and data storage.

Example

```
client = get_sync_client(url="http://localhost:2024")
assistant = client.assistants.get("asst_123")
```

| METHOD | DESCRIPTION |
| --- | --- |
| `__enter__` | Enter the sync context manager. |
| `__exit__` | Exit the sync context manager. |
| `close` | Close the underlying HTTP client. |

#### ``\_\_enter\_\_ ¶

```
__enter__() -> SyncLangGraphClient
```

Enter the sync context manager.

#### ``\_\_exit\_\_ ¶

```
__exit__(
    exc_type: type[BaseException] | None,
    exc_val: BaseException | None,
    exc_tb: TracebackType | None,
) -> None
```

Exit the sync context manager.

#### ``close ¶

```
close() -> None
```

Close the underlying HTTP client.

### ``SyncHttpClient ¶

Handle synchronous requests to the LangGraph API.

Provides error messaging and content handling enhancements above the
underlying httpx client, mirroring the interface of HttpClient
but for sync usage.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `client` | Underlying HTTPX sync client.<br>**TYPE:**`Client` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get` | Send a `GET` request. |
| `post` | Send a `POST` request. |
| `put` | Send a `PUT` request. |
| `patch` | Send a `PATCH` request. |
| `delete` | Send a `DELETE` request. |
| `request_reconnect` | Send a request that automatically reconnects to Location header. |
| `stream` | Stream the results of a request using SSE. |

#### ``get ¶

```
get(
    path: str,
    *,
    params: QueryParamTypes | None = None,
    headers: Mapping[str, str] | None = None,
    on_response: Callable[[Response], None] | None = None,
) -> Any
```

Send a `GET` request.

#### ``post ¶

```
post(
    path: str,
    *,
    json: dict[str, Any] | list | None,
    params: QueryParamTypes | None = None,
    headers: Mapping[str, str] | None = None,
    on_response: Callable[[Response], None] | None = None,
) -> Any
```

Send a `POST` request.

#### ``put ¶

```
put(
    path: str,
    *,
    json: dict,
    params: QueryParamTypes | None = None,
    headers: Mapping[str, str] | None = None,
    on_response: Callable[[Response], None] | None = None,
) -> Any
```

Send a `PUT` request.

#### ``patch ¶

```
patch(
    path: str,
    *,
    json: dict,
    params: QueryParamTypes | None = None,
    headers: Mapping[str, str] | None = None,
    on_response: Callable[[Response], None] | None = None,
) -> Any
```

Send a `PATCH` request.

#### ``delete ¶

```
delete(
    path: str,
    *,
    json: Any | None = None,
    params: QueryParamTypes | None = None,
    headers: Mapping[str, str] | None = None,
    on_response: Callable[[Response], None] | None = None,
) -> None
```

Send a `DELETE` request.

#### ``request\_reconnect ¶

```
request_reconnect(
    path: str,
    method: str,
    *,
    json: dict[str, Any] | None = None,
    params: QueryParamTypes | None = None,
    headers: Mapping[str, str] | None = None,
    on_response: Callable[[Response], None] | None = None,
    reconnect_limit: int = 5,
) -> Any
```

Send a request that automatically reconnects to Location header.

#### ``stream ¶

```
stream(
    path: str,
    method: str,
    *,
    json: dict[str, Any] | None = None,
    params: QueryParamTypes | None = None,
    headers: Mapping[str, str] | None = None,
    on_response: Callable[[Response], None] | None = None,
) -> Iterator[StreamPart]
```

Stream the results of a request using SSE.

### ``SyncAssistantsClient ¶

Client for managing assistants in LangGraph synchronously.

This class provides methods to interact with assistants, which are versioned configurations of your graph.

Example

```
client = get_sync_client(url="http://localhost:2024")
assistant = client.assistants.get("assistant_id_123")
```

| METHOD | DESCRIPTION |
| --- | --- |
| `get` | Get an assistant by ID. |
| `get_graph` | Get the graph of an assistant by ID. |
| `get_schemas` | Get the schemas of an assistant by ID. |
| `get_subgraphs` | Get the schemas of an assistant by ID. |
| `create` | Create a new assistant. |
| `update` | Update an assistant. |
| `delete` | Delete an assistant. |
| `search` | Search for assistants. |
| `count` | Count assistants matching filters. |
| `get_versions` | List all versions of an assistant. |
| `set_latest` | Change the version of an assistant. |

#### ``get ¶

```
get(
    assistant_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Assistant
```

Get an assistant by ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The ID of the assistant to get OR the name of the graph (to use the default assistant).<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Assistant` | `Assistant` Object. |

Example Usage

```
assistant = client.assistants.get(
    assistant_id="my_assistant_id"
)
print(assistant)
```

```
----------------------------------------------------

{
    'assistant_id': 'my_assistant_id',
    'graph_id': 'agent',
    'created_at': '2024-06-25T17:10:33.109781+00:00',
    'updated_at': '2024-06-25T17:10:33.109781+00:00',
    'config': {},
    'context': {},
    'metadata': {'created_by': 'system'}
}
```

#### ``get\_graph ¶

```
get_graph(
    assistant_id: str,
    *,
    xray: int | bool = False,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> dict[str, list[dict[str, Any]]]
```

Get the graph of an assistant by ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The ID of the assistant to get the graph of.<br>**TYPE:**`str` |
| `xray` | Include graph representation of subgraphs. If an integer value is provided, only subgraphs with a depth less than or equal to the value will be included.<br>**TYPE:**`int | bool`**DEFAULT:**`False` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, list[dict[str, Any]]]` | The graph information for the assistant in JSON format. |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
graph_info = client.assistants.get_graph(
    assistant_id="my_assistant_id"
)
print(graph_info)

--------------------------------------------------------------------------------------------------------------------------

{
    'nodes':
        [\
            {'id': '__start__', 'type': 'schema', 'data': '__start__'},\
            {'id': '__end__', 'type': 'schema', 'data': '__end__'},\
            {'id': 'agent','type': 'runnable','data': {'id': ['langgraph', 'utils', 'RunnableCallable'],'name': 'agent'}},\
        ],
    'edges':
        [\
            {'source': '__start__', 'target': 'agent'},\
            {'source': 'agent','target': '__end__'}\
        ]
}
```

#### ``get\_schemas ¶

```
get_schemas(
    assistant_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> GraphSchema
```

Get the schemas of an assistant by ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The ID of the assistant to get the schema of.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `GraphSchema` | The graph schema for the assistant.<br>**TYPE:**`GraphSchema` |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
schema = client.assistants.get_schemas(
    assistant_id="my_assistant_id"
)
print(schema)
```

```
----------------------------------------------------------------------------------------------------------------------------

{
    'graph_id': 'agent',
    'state_schema':
        {
            'title': 'LangGraphInput',
            '$ref': '#/definitions/AgentState',
            'definitions':
                {
                    'BaseMessage':
                        {
                            'title': 'BaseMessage',
                            'description': 'Base abstract Message class. Messages are the inputs and outputs of ChatModels.',
                            'type': 'object',
                            'properties':
                                {
                                 'content':
                                    {
                                        'title': 'Content',
                                        'anyOf': [\
                                            {'type': 'string'},\
                                            {'type': 'array','items': {'anyOf': [{'type': 'string'}, {'type': 'object'}]}}\
                                        ]
                                    },
                                'additional_kwargs':
                                    {
                                        'title': 'Additional Kwargs',
                                        'type': 'object'
                                    },
                                'response_metadata':
                                    {
                                        'title': 'Response Metadata',
                                        'type': 'object'
                                    },
                                'type':
                                    {
                                        'title': 'Type',
                                        'type': 'string'
                                    },
                                'name':
                                    {
                                        'title': 'Name',
                                        'type': 'string'
                                    },
                                'id':
                                    {
                                        'title': 'Id',
                                        'type': 'string'
                                    }
                                },
                            'required': ['content', 'type']
                        },
                    'AgentState':
                        {
                            'title': 'AgentState',
                            'type': 'object',
                            'properties':
                                {
                                    'messages':
                                        {
                                            'title': 'Messages',
                                            'type': 'array',
                                            'items': {'$ref': '#/definitions/BaseMessage'}
                                        }
                                },
                            'required': ['messages']
                        }
                }
        },
    'config_schema':
        {
            'title': 'Configurable',
            'type': 'object',
            'properties':
                {
                    'model_name':
                        {
                            'title': 'Model Name',
                            'enum': ['anthropic', 'openai'],
                            'type': 'string'
                        }
                }
        },
    'context_schema':
        {
            'title': 'Context',
            'type': 'object',
            'properties':
                {
                    'model_name':
                        {
                            'title': 'Model Name',
                            'enum': ['anthropic', 'openai'],
                            'type': 'string'
                        }
                }
        }
}
```

#### ``get\_subgraphs ¶

```
get_subgraphs(
    assistant_id: str,
    namespace: str | None = None,
    recurse: bool = False,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Subgraphs
```

Get the schemas of an assistant by ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The ID of the assistant to get the schema of.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Subgraphs` | The graph schema for the assistant.<br>**TYPE:**`Subgraphs` |

#### ``create ¶

```
create(
    graph_id: str | None,
    config: Config | None = None,
    *,
    context: Context | None = None,
    metadata: Json = None,
    assistant_id: str | None = None,
    if_exists: OnConflictBehavior | None = None,
    name: str | None = None,
    headers: Mapping[str, str] | None = None,
    description: str | None = None,
    params: QueryParamTypes | None = None,
) -> Assistant
```

Create a new assistant.

Useful when graph is configurable and you want to create different assistants based on different configurations.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `graph_id` | The ID of the graph the assistant should use. The graph ID is normally set in your langgraph.json configuration.<br>**TYPE:**`str | None` |
| `config` | Configuration to use for the graph.<br>**TYPE:**`Config | None`**DEFAULT:**`None` |
| `context` | Static context to add to the assistant.<br>Added in version 0.6.0<br>**TYPE:**`Context | None`**DEFAULT:**`None` |
| `metadata` | Metadata to add to assistant.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `assistant_id` | Assistant ID to use, will default to a random UUID if not provided.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `if_exists` | How to handle duplicate creation. Defaults to 'raise' under the hood.<br>Must be either 'raise' (raise error if duplicate), or 'do\_nothing' (return existing assistant).<br>**TYPE:**`OnConflictBehavior | None`**DEFAULT:**`None` |
| `name` | The name of the assistant. Defaults to 'Untitled' under the hood.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `description` | Optional description of the assistant.<br>The description field is available for langgraph-api server version>=0.0.45<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Assistant` | The created assistant. |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
assistant = client.assistants.create(
    graph_id="agent",
    context={"model_name": "openai"},
    metadata={"number":1},
    assistant_id="my-assistant-id",
    if_exists="do_nothing",
    name="my_name"
)
```

#### ``update ¶

```
update(
    assistant_id: str,
    *,
    graph_id: str | None = None,
    config: Config | None = None,
    context: Context | None = None,
    metadata: Json = None,
    name: str | None = None,
    headers: Mapping[str, str] | None = None,
    description: str | None = None,
    params: QueryParamTypes | None = None,
) -> Assistant
```

Update an assistant.

Use this to point to a different graph, update the configuration, or change the metadata of an assistant.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | Assistant to update.<br>**TYPE:**`str` |
| `graph_id` | The ID of the graph the assistant should use.<br>The graph ID is normally set in your langgraph.json configuration. If `None`, assistant will keep pointing to same graph.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `config` | Configuration to use for the graph.<br>**TYPE:**`Config | None`**DEFAULT:**`None` |
| `context` | Static context to add to the assistant.<br>Added in version 0.6.0<br>**TYPE:**`Context | None`**DEFAULT:**`None` |
| `metadata` | Metadata to merge with existing assistant metadata.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `name` | The new name for the assistant.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `description` | Optional description of the assistant.<br>The description field is available for langgraph-api server version>=0.0.45<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Assistant` | The updated assistant. |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
assistant = client.assistants.update(
    assistant_id='e280dad7-8618-443f-87f1-8e41841c180f',
    graph_id="other-graph",
    context={"model_name": "anthropic"},
    metadata={"number":2}
)
```

#### ``delete ¶

```
delete(
    assistant_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Delete an assistant.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The assistant ID to delete.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
client.assistants.delete(
    assistant_id="my_assistant_id"
)
```

#### ``search ¶

```
search(
    *,
    metadata: Json = None,
    graph_id: str | None = None,
    limit: int = 10,
    offset: int = 0,
    sort_by: AssistantSortBy | None = None,
    sort_order: SortOrder | None = None,
    select: list[AssistantSelectField] | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> list[Assistant]
```

Search for assistants.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `metadata` | Metadata to filter by. Exact match filter for each KV pair.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `graph_id` | The ID of the graph to filter by.<br>The graph ID is normally set in your langgraph.json configuration.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `limit` | The maximum number of results to return.<br>**TYPE:**`int`**DEFAULT:**`10` |
| `offset` | The number of results to skip.<br>**TYPE:**`int`**DEFAULT:**`0` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Assistant]` | A list of assistants. |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
assistants = client.assistants.search(
    metadata = {"name":"my_name"},
    graph_id="my_graph_id",
    limit=5,
    offset=5
)
```

#### ``count ¶

```
count(
    *,
    metadata: Json = None,
    graph_id: str | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> int
```

Count assistants matching filters.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `metadata` | Metadata to filter by. Exact match for each key/value.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `graph_id` | Optional graph id to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `int` | Number of assistants matching the criteria.<br>**TYPE:**`int` |

#### ``get\_versions ¶

```
get_versions(
    assistant_id: str,
    metadata: Json = None,
    limit: int = 10,
    offset: int = 0,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> list[AssistantVersion]
```

List all versions of an assistant.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The assistant ID to get versions for.<br>**TYPE:**`str` |
| `metadata` | Metadata to filter versions by. Exact match filter for each KV pair.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `limit` | The maximum number of versions to return.<br>**TYPE:**`int`**DEFAULT:**`10` |
| `offset` | The number of versions to skip.<br>**TYPE:**`int`**DEFAULT:**`0` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[AssistantVersion]` | A list of assistants. |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
assistant_versions = client.assistants.get_versions(
    assistant_id="my_assistant_id"
)
```

#### ``set\_latest ¶

```
set_latest(
    assistant_id: str,
    version: int,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Assistant
```

Change the version of an assistant.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The assistant ID to delete.<br>**TYPE:**`str` |
| `version` | The version to change to.<br>**TYPE:**`int` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Assistant` | `Assistant` Object. |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
new_version_assistant = client.assistants.set_latest(
    assistant_id="my_assistant_id",
    version=3
)
```

### ``SyncThreadsClient ¶

Synchronous client for managing threads in LangGraph.

This class provides methods to create, retrieve, and manage threads,
which represent conversations or stateful interactions.

Example

```
client = get_sync_client(url="http://localhost:2024")
thread = client.threads.create(metadata={"user_id": "123"})
```

| METHOD | DESCRIPTION |
| --- | --- |
| `get` | Get a thread by ID. |
| `create` | Create a new thread. |
| `update` | Update a thread. |
| `delete` | Delete a thread. |
| `search` | Search for threads. |
| `count` | Count threads matching filters. |
| `copy` | Copy a thread. |
| `get_state` | Get the state of a thread. |
| `update_state` | Update the state of a thread. |
| `get_history` | Get the state history of a thread. |
| `join_stream` | Get a stream of events for a thread. |

#### ``get ¶

```
get(
    thread_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Thread
```

Get a thread by ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The ID of the thread to get.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Thread` | `Thread` object. |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
thread = client.threads.get(
    thread_id="my_thread_id"
)
print(thread)
```

```
-----------------------------------------------------

{
    'thread_id': 'my_thread_id',
    'created_at': '2024-07-18T18:35:15.540834+00:00',
    'updated_at': '2024-07-18T18:35:15.540834+00:00',
    'metadata': {'graph_id': 'agent'}
}
```

#### ``create ¶

```
create(
    *,
    metadata: Json = None,
    thread_id: str | None = None,
    if_exists: OnConflictBehavior | None = None,
    supersteps: Sequence[dict[str, Sequence[dict[str, Any]]]] | None = None,
    graph_id: str | None = None,
    ttl: int | Mapping[str, Any] | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Thread
```

Create a new thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `metadata` | Metadata to add to thread.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `thread_id` | ID of thread.<br>If `None`, ID will be a randomly generated UUID.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `if_exists` | How to handle duplicate creation. Defaults to 'raise' under the hood.<br>Must be either 'raise' (raise error if duplicate), or 'do\_nothing' (return existing thread).<br>**TYPE:**`OnConflictBehavior | None`**DEFAULT:**`None` |
| `supersteps` | Apply a list of supersteps when creating a thread, each containing a sequence of updates.<br>Each update has `values` or `command` and `as_node`. Used for copying a thread between deployments.<br>**TYPE:**`Sequence[dict[str, Sequence[dict[str, Any]]]] | None`**DEFAULT:**`None` |
| `graph_id` | Optional graph ID to associate with the thread.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `ttl` | Optional time-to-live in minutes for the thread. You can pass an<br>integer (minutes) or a mapping with keys `ttl` and optional<br>`strategy` (defaults to "delete").<br>**TYPE:**`int | Mapping[str, Any] | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Thread` | The created `Thread`. |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
thread = client.threads.create(
    metadata={"number":1},
    thread_id="my-thread-id",
    if_exists="raise"
)
```

)

#### ``update ¶

```
update(
    thread_id: str,
    *,
    metadata: Mapping[str, Any],
    ttl: int | Mapping[str, Any] | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Thread
```

Update a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | ID of thread to update.<br>**TYPE:**`str` |
| `metadata` | Metadata to merge with existing thread metadata.<br>**TYPE:**`Mapping[str, Any]` |
| `ttl` | Optional time-to-live in minutes for the thread. You can pass an<br>integer (minutes) or a mapping with keys `ttl` and optional<br>`strategy` (defaults to "delete").<br>**TYPE:**`int | Mapping[str, Any] | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Thread` | The created `Thread`. |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
thread = client.threads.update(
    thread_id="my-thread-id",
    metadata={"number":1},
    ttl=43_200,
)
```

#### ``delete ¶

```
delete(
    thread_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Delete a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The ID of the thread to delete.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client.threads.delete(
    thread_id="my_thread_id"
)
```

#### ``search ¶

```
search(
    *,
    metadata: Json = None,
    values: Json = None,
    ids: Sequence[str] | None = None,
    status: ThreadStatus | None = None,
    limit: int = 10,
    offset: int = 0,
    sort_by: ThreadSortBy | None = None,
    sort_order: SortOrder | None = None,
    select: list[ThreadSelectField] | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> list[Thread]
```

Search for threads.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `metadata` | Thread metadata to filter on.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `values` | State values to filter on.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `ids` | List of thread IDs to filter by.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `status` | Thread status to filter on.<br>Must be one of 'idle', 'busy', 'interrupted' or 'error'.<br>**TYPE:**`ThreadStatus | None`**DEFAULT:**`None` |
| `limit` | Limit on number of threads to return.<br>**TYPE:**`int`**DEFAULT:**`10` |
| `offset` | Offset in threads table to start search from.<br>**TYPE:**`int`**DEFAULT:**`0` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Thread]` | List of the threads matching the search parameters. |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
threads = client.threads.search(
    metadata={"number":1},
    status="interrupted",
    limit=15,
    offset=5
)
```

#### ``count ¶

```
count(
    *,
    metadata: Json = None,
    values: Json = None,
    status: ThreadStatus | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> int
```

Count threads matching filters.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `metadata` | Thread metadata to filter on.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `values` | State values to filter on.<br>**TYPE:**`Json`**DEFAULT:**`None` |
| `status` | Thread status to filter on.<br>**TYPE:**`ThreadStatus | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `int` | Number of threads matching the criteria.<br>**TYPE:**`int` |

#### ``copy ¶

```
copy(
    thread_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Copy a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The ID of the thread to copy.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
client.threads.copy(
    thread_id="my_thread_id"
)
```

#### ``get\_state ¶

```
get_state(
    thread_id: str,
    checkpoint: Checkpoint | None = None,
    checkpoint_id: str | None = None,
    *,
    subgraphs: bool = False,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> ThreadState
```

Get the state of a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The ID of the thread to get the state of.<br>**TYPE:**`str` |
| `checkpoint` | The checkpoint to get the state of.<br>**TYPE:**`Checkpoint | None`**DEFAULT:**`None` |
| `subgraphs` | Include subgraphs states.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ThreadState` | The thread of the state. |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
thread_state = client.threads.get_state(
    thread_id="my_thread_id",
    checkpoint_id="my_checkpoint_id"
)
print(thread_state)
```

```
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

{
    'values': {
        'messages': [\
            {\
                'content': 'how are you?',\
                'additional_kwargs': {},\
                'response_metadata': {},\
                'type': 'human',\
                'name': None,\
                'id': 'fe0a5778-cfe9-42ee-b807-0adaa1873c10',\
                'example': False\
            },\
            {\
                'content': "I'm doing well, thanks for asking! I'm an AI assistant created by Anthropic to be helpful, honest, and harmless.",\
                'additional_kwargs': {},\
                'response_metadata': {},\
                'type': 'ai',\
                'name': None,\
                'id': 'run-159b782c-b679-4830-83c6-cef87798fe8b',\
                'example': False,\
                'tool_calls': [],\
                'invalid_tool_calls': [],\
                'usage_metadata': None\
            }\
        ]
    },
    'next': [],
    'checkpoint':
        {
            'thread_id': 'e2496803-ecd5-4e0c-a779-3226296181c2',
            'checkpoint_ns': '',
            'checkpoint_id': '1ef4a9b8-e6fb-67b1-8001-abd5184439d1'
        }
    'metadata':
        {
            'step': 1,
            'run_id': '1ef4a9b8-d7da-679a-a45a-872054341df2',
            'source': 'loop',
            'writes':
                {
                    'agent':
                        {
                            'messages': [\
                                {\
                                    'id': 'run-159b782c-b679-4830-83c6-cef87798fe8b',\
                                    'name': None,\
                                    'type': 'ai',\
                                    'content': "I'm doing well, thanks for asking! I'm an AI assistant created by Anthropic to be helpful, honest, and harmless.",\
                                    'example': False,\
                                    'tool_calls': [],\
                                    'usage_metadata': None,\
                                    'additional_kwargs': {},\
                                    'response_metadata': {},\
                                    'invalid_tool_calls': []\
                                }\
                            ]
                        }
                },
    'user_id': None,
    'graph_id': 'agent',
    'thread_id': 'e2496803-ecd5-4e0c-a779-3226296181c2',
    'created_by': 'system',
    'assistant_id': 'fe096781-5601-53d2-b2f6-0d3403f7e9ca'},
    'created_at': '2024-07-25T15:35:44.184703+00:00',
    'parent_config':
        {
            'thread_id': 'e2496803-ecd5-4e0c-a779-3226296181c2',
            'checkpoint_ns': '',
            'checkpoint_id': '1ef4a9b8-d80d-6fa7-8000-9300467fad0f'
        }
}
```

#### ``update\_state ¶

```
update_state(
    thread_id: str,
    values: dict[str, Any] | Sequence[dict] | None,
    *,
    as_node: str | None = None,
    checkpoint: Checkpoint | None = None,
    checkpoint_id: str | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> ThreadUpdateStateResponse
```

Update the state of a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The ID of the thread to update.<br>**TYPE:**`str` |
| `values` | The values to update the state with.<br>**TYPE:**`dict[str, Any] | Sequence[dict] | None` |
| `as_node` | Update the state as if this node had just executed.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `checkpoint` | The checkpoint to update the state of.<br>**TYPE:**`Checkpoint | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ThreadUpdateStateResponse` | Response after updating a thread's state. |

Example Usage

```
response = await client.threads.update_state(
    thread_id="my_thread_id",
    values={"messages":[{"role": "user", "content": "hello!"}]},
    as_node="my_node",
)
print(response)

----------------------------------------------------------------------------------------------------------------------------------------------------------------------

{
    'checkpoint': {
        'thread_id': 'e2496803-ecd5-4e0c-a779-3226296181c2',
        'checkpoint_ns': '',
        'checkpoint_id': '1ef4a9b8-e6fb-67b1-8001-abd5184439d1',
        'checkpoint_map': {}
    }
}
```

#### ``get\_history ¶

```
get_history(
    thread_id: str,
    *,
    limit: int = 10,
    before: str | Checkpoint | None = None,
    metadata: Mapping[str, Any] | None = None,
    checkpoint: Checkpoint | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> list[ThreadState]
```

Get the state history of a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The ID of the thread to get the state history for.<br>**TYPE:**`str` |
| `checkpoint` | Return states for this subgraph. If empty defaults to root.<br>**TYPE:**`Checkpoint | None`**DEFAULT:**`None` |
| `limit` | The maximum number of states to return.<br>**TYPE:**`int`**DEFAULT:**`10` |
| `before` | Return states before this checkpoint.<br>**TYPE:**`str | Checkpoint | None`**DEFAULT:**`None` |
| `metadata` | Filter states by metadata key-value pairs.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[ThreadState]` | The state history of the `Thread`. |

Example Usage

```
thread_state = client.threads.get_history(
    thread_id="my_thread_id",
    limit=5,
    before="my_timestamp",
    metadata={"name":"my_name"}
)
```

#### ``join\_stream ¶

```
join_stream(
    thread_id: str,
    *,
    stream_mode: ThreadStreamMode | Sequence[ThreadStreamMode] = "run_modes",
    last_event_id: str | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Iterator[StreamPart]
```

Get a stream of events for a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The ID of the thread to get the stream for.<br>**TYPE:**`str` |
| `last_event_id` | The ID of the last event to get.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Iterator[StreamPart]` | An iterator of stream parts. |

Example Usage

```
for chunk in client.threads.join_stream(
    thread_id="my_thread_id",
    last_event_id="my_event_id",
    stream_mode="run_modes",
):
    print(chunk)
```

### ``SyncRunsClient ¶

Synchronous client for managing runs in LangGraph.

This class provides methods to create, retrieve, and manage runs, which represent
individual executions of graphs.

Example

```
client = get_sync_client(url="http://localhost:2024")
run = client.runs.create(thread_id="thread_123", assistant_id="asst_456")
```

| METHOD | DESCRIPTION |
| --- | --- |
| `stream` | Create a run and stream the results. |
| `create` | Create a background run. |
| `create_batch` | Create a batch of stateless background runs. |
| `wait` | Create a run, wait until it finishes and return the final state. |
| `list` | List runs. |
| `get` | Get a run. |
| `cancel` | Get a run. |
| `join` | Block until a run is done. Returns the final state of the thread. |
| `join_stream` | Stream output from a run in real-time, until the run is done. |
| `delete` | Delete a run. |

#### ``stream ¶

```
stream(
    thread_id: str | None,
    assistant_id: str,
    *,
    input: Mapping[str, Any] | None = None,
    command: Command | None = None,
    stream_mode: StreamMode | Sequence[StreamMode] = "values",
    stream_subgraphs: bool = False,
    stream_resumable: bool = False,
    metadata: Mapping[str, Any] | None = None,
    config: Config | None = None,
    context: Context | None = None,
    checkpoint: Checkpoint | None = None,
    checkpoint_id: str | None = None,
    checkpoint_during: bool | None = None,
    interrupt_before: All | Sequence[str] | None = None,
    interrupt_after: All | Sequence[str] | None = None,
    feedback_keys: Sequence[str] | None = None,
    on_disconnect: DisconnectMode | None = None,
    on_completion: OnCompletionBehavior | None = None,
    webhook: str | None = None,
    multitask_strategy: MultitaskStrategy | None = None,
    if_not_exists: IfNotExists | None = None,
    after_seconds: int | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
    on_run_created: Callable[[RunCreateMetadata], None] | None = None,
    durability: Durability | None = None,
) -> Iterator[StreamPart]
```

Create a run and stream the results.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | the thread ID to assign to the thread.<br>If `None` will create a stateless run.<br>**TYPE:**`str | None` |
| `assistant_id` | The assistant ID or graph name to stream from.<br>If using graph name, will default to first assistant created from that graph.<br>**TYPE:**`str` |
| `input` | The input to the graph.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `command` | The command to execute.<br>**TYPE:**`Command | None`**DEFAULT:**`None` |
| `stream_mode` | The stream mode(s) to use.<br>**TYPE:**`StreamMode | Sequence[StreamMode]`**DEFAULT:**`'values'` |
| `stream_subgraphs` | Whether to stream output from subgraphs.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `stream_resumable` | Whether the stream is considered resumable.<br>If true, the stream can be resumed and replayed in its entirety even after disconnection.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `metadata` | Metadata to assign to the run.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `config` | The configuration for the assistant.<br>**TYPE:**`Config | None`**DEFAULT:**`None` |
| `context` | Static context to add to the assistant.<br>Added in version 0.6.0<br>**TYPE:**`Context | None`**DEFAULT:**`None` |
| `checkpoint` | The checkpoint to resume from.<br>**TYPE:**`Checkpoint | None`**DEFAULT:**`None` |
| `checkpoint_during` | (deprecated) Whether to checkpoint during the run (or only at the end/interruption).<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `interrupt_before` | Nodes to interrupt immediately before they get executed.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | Nodes to Nodes to interrupt immediately after they get executed.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `feedback_keys` | Feedback keys to assign to run.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `on_disconnect` | The disconnect mode to use.<br>Must be one of 'cancel' or 'continue'.<br>**TYPE:**`DisconnectMode | None`**DEFAULT:**`None` |
| `on_completion` | Whether to delete or keep the thread created for a stateless run.<br>Must be one of 'delete' or 'keep'.<br>**TYPE:**`OnCompletionBehavior | None`**DEFAULT:**`None` |
| `webhook` | Webhook to call after LangGraph API call is done.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `multitask_strategy` | Multitask strategy to use.<br>Must be one of 'reject', 'interrupt', 'rollback', or 'enqueue'.<br>**TYPE:**`MultitaskStrategy | None`**DEFAULT:**`None` |
| `if_not_exists` | How to handle missing thread. Defaults to 'reject'.<br>Must be either 'reject' (raise error if missing), or 'create' (create new thread).<br>**TYPE:**`IfNotExists | None`**DEFAULT:**`None` |
| `after_seconds` | The number of seconds to wait before starting the run.<br>Use to schedule future runs.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `on_run_created` | Optional callback to call when a run is created.<br>**TYPE:**`Callable[[RunCreateMetadata], None] | None`**DEFAULT:**`None` |
| `durability` | The durability to use for the run. Values are "sync", "async", or "exit".<br>"async" means checkpoints are persisted async while next graph step executes, replaces checkpoint\_during=True<br>"sync" means checkpoints are persisted sync after graph step executes, replaces checkpoint\_during=False<br>"exit" means checkpoints are only persisted when the run exits, does not save intermediate steps<br>**TYPE:**`Durability | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Iterator[StreamPart]` | Iterator of stream results. |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
async for chunk in client.runs.stream(
    thread_id=None,
    assistant_id="agent",
    input={"messages": [{"role": "user", "content": "how are you?"}]},
    stream_mode=["values","debug"],
    metadata={"name":"my_run"},
    context={"model_name": "anthropic"},
    interrupt_before=["node_to_stop_before_1","node_to_stop_before_2"],
    interrupt_after=["node_to_stop_after_1","node_to_stop_after_2"],
    feedback_keys=["my_feedback_key_1","my_feedback_key_2"],
    webhook="https://my.fake.webhook.com",
    multitask_strategy="interrupt"
):
    print(chunk)
```

```
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

StreamPart(event='metadata', data={'run_id': '1ef4a9b8-d7da-679a-a45a-872054341df2'})
StreamPart(event='values', data={'messages': [{'content': 'how are you?', 'additional_kwargs': {}, 'response_metadata': {}, 'type': 'human', 'name': None, 'id': 'fe0a5778-cfe9-42ee-b807-0adaa1873c10', 'example': False}]})
StreamPart(event='values', data={'messages': [{'content': 'how are you?', 'additional_kwargs': {}, 'response_metadata': {}, 'type': 'human', 'name': None, 'id': 'fe0a5778-cfe9-42ee-b807-0adaa1873c10', 'example': False}, {'content': "I'm doing well, thanks for asking! I'm an AI assistant created by Anthropic to be helpful, honest, and harmless.", 'additional_kwargs': {}, 'response_metadata': {}, 'type': 'ai', 'name': None, 'id': 'run-159b782c-b679-4830-83c6-cef87798fe8b', 'example': False, 'tool_calls': [], 'invalid_tool_calls': [], 'usage_metadata': None}]})
StreamPart(event='end', data=None)
```

#### ``create ¶

```
create(
    thread_id: str | None,
    assistant_id: str,
    *,
    input: Mapping[str, Any] | None = None,
    command: Command | None = None,
    stream_mode: StreamMode | Sequence[StreamMode] = "values",
    stream_subgraphs: bool = False,
    stream_resumable: bool = False,
    metadata: Mapping[str, Any] | None = None,
    config: Config | None = None,
    context: Context | None = None,
    checkpoint: Checkpoint | None = None,
    checkpoint_id: str | None = None,
    checkpoint_during: bool | None = None,
    interrupt_before: All | Sequence[str] | None = None,
    interrupt_after: All | Sequence[str] | None = None,
    webhook: str | None = None,
    multitask_strategy: MultitaskStrategy | None = None,
    if_not_exists: IfNotExists | None = None,
    on_completion: OnCompletionBehavior | None = None,
    after_seconds: int | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
    on_run_created: Callable[[RunCreateMetadata], None] | None = None,
    durability: Durability | None = None,
) -> Run
```

Create a background run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | the thread ID to assign to the thread.<br>If `None` will create a stateless run.<br>**TYPE:**`str | None` |
| `assistant_id` | The assistant ID or graph name to stream from.<br>If using graph name, will default to first assistant created from that graph.<br>**TYPE:**`str` |
| `input` | The input to the graph.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `command` | The command to execute.<br>**TYPE:**`Command | None`**DEFAULT:**`None` |
| `stream_mode` | The stream mode(s) to use.<br>**TYPE:**`StreamMode | Sequence[StreamMode]`**DEFAULT:**`'values'` |
| `stream_subgraphs` | Whether to stream output from subgraphs.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `stream_resumable` | Whether the stream is considered resumable.<br>If true, the stream can be resumed and replayed in its entirety even after disconnection.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `metadata` | Metadata to assign to the run.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `config` | The configuration for the assistant.<br>**TYPE:**`Config | None`**DEFAULT:**`None` |
| `context` | Static context to add to the assistant.<br>Added in version 0.6.0<br>**TYPE:**`Context | None`**DEFAULT:**`None` |
| `checkpoint` | The checkpoint to resume from.<br>**TYPE:**`Checkpoint | None`**DEFAULT:**`None` |
| `checkpoint_during` | (deprecated) Whether to checkpoint during the run (or only at the end/interruption).<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `interrupt_before` | Nodes to interrupt immediately before they get executed.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | Nodes to Nodes to interrupt immediately after they get executed.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `webhook` | Webhook to call after LangGraph API call is done.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `multitask_strategy` | Multitask strategy to use.<br>Must be one of 'reject', 'interrupt', 'rollback', or 'enqueue'.<br>**TYPE:**`MultitaskStrategy | None`**DEFAULT:**`None` |
| `on_completion` | Whether to delete or keep the thread created for a stateless run.<br>Must be one of 'delete' or 'keep'.<br>**TYPE:**`OnCompletionBehavior | None`**DEFAULT:**`None` |
| `if_not_exists` | How to handle missing thread. Defaults to 'reject'.<br>Must be either 'reject' (raise error if missing), or 'create' (create new thread).<br>**TYPE:**`IfNotExists | None`**DEFAULT:**`None` |
| `after_seconds` | The number of seconds to wait before starting the run.<br>Use to schedule future runs.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `on_run_created` | Optional callback to call when a run is created.<br>**TYPE:**`Callable[[RunCreateMetadata], None] | None`**DEFAULT:**`None` |
| `durability` | The durability to use for the run. Values are "sync", "async", or "exit".<br>"async" means checkpoints are persisted async while next graph step executes, replaces checkpoint\_during=True<br>"sync" means checkpoints are persisted sync after graph step executes, replaces checkpoint\_during=False<br>"exit" means checkpoints are only persisted when the run exits, does not save intermediate steps<br>**TYPE:**`Durability | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Run` | The created background `Run`. |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
background_run = client.runs.create(
    thread_id="my_thread_id",
    assistant_id="my_assistant_id",
    input={"messages": [{"role": "user", "content": "hello!"}]},
    metadata={"name":"my_run"},
    context={"model_name": "openai"},
    interrupt_before=["node_to_stop_before_1","node_to_stop_before_2"],
    interrupt_after=["node_to_stop_after_1","node_to_stop_after_2"],
    webhook="https://my.fake.webhook.com",
    multitask_strategy="interrupt"
)
print(background_run)
```

```
--------------------------------------------------------------------------------

{
    'run_id': 'my_run_id',
    'thread_id': 'my_thread_id',
    'assistant_id': 'my_assistant_id',
    'created_at': '2024-07-25T15:35:42.598503+00:00',
    'updated_at': '2024-07-25T15:35:42.598503+00:00',
    'metadata': {},
    'status': 'pending',
    'kwargs':
        {
            'input':
                {
                    'messages': [\
                        {\
                            'role': 'user',\
                            'content': 'how are you?'\
                        }\
                    ]
                },
            'config':
                {
                    'metadata':
                        {
                            'created_by': 'system'
                        },
                    'configurable':
                        {
                            'run_id': 'my_run_id',
                            'user_id': None,
                            'graph_id': 'agent',
                            'thread_id': 'my_thread_id',
                            'checkpoint_id': None,
                            'assistant_id': 'my_assistant_id'
                        }
                },
            'context':
                {
                    'model_name': 'openai'
                },
            'webhook': "https://my.fake.webhook.com",
            'temporary': False,
            'stream_mode': ['values'],
            'feedback_keys': None,
            'interrupt_after': ["node_to_stop_after_1","node_to_stop_after_2"],
            'interrupt_before': ["node_to_stop_before_1","node_to_stop_before_2"]
        },
    'multitask_strategy': 'interrupt'
}
```

#### ``create\_batch ¶

```
create_batch(
    payloads: list[RunCreate],
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> list[Run]
```

Create a batch of stateless background runs.

#### ``wait ¶

```
wait(
    thread_id: str | None,
    assistant_id: str,
    *,
    input: Mapping[str, Any] | None = None,
    command: Command | None = None,
    metadata: Mapping[str, Any] | None = None,
    config: Config | None = None,
    context: Context | None = None,
    checkpoint_during: bool | None = None,
    checkpoint: Checkpoint | None = None,
    checkpoint_id: str | None = None,
    interrupt_before: All | Sequence[str] | None = None,
    interrupt_after: All | Sequence[str] | None = None,
    webhook: str | None = None,
    on_disconnect: DisconnectMode | None = None,
    on_completion: OnCompletionBehavior | None = None,
    multitask_strategy: MultitaskStrategy | None = None,
    if_not_exists: IfNotExists | None = None,
    after_seconds: int | None = None,
    raise_error: bool = True,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
    on_run_created: Callable[[RunCreateMetadata], None] | None = None,
    durability: Durability | None = None,
) -> list[dict] | dict[str, Any]
```

Create a run, wait until it finishes and return the final state.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | the thread ID to create the run on.<br>If `None` will create a stateless run.<br>**TYPE:**`str | None` |
| `assistant_id` | The assistant ID or graph name to run.<br>If using graph name, will default to first assistant created from that graph.<br>**TYPE:**`str` |
| `input` | The input to the graph.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `command` | The command to execute.<br>**TYPE:**`Command | None`**DEFAULT:**`None` |
| `metadata` | Metadata to assign to the run.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `config` | The configuration for the assistant.<br>**TYPE:**`Config | None`**DEFAULT:**`None` |
| `context` | Static context to add to the assistant.<br>Added in version 0.6.0<br>**TYPE:**`Context | None`**DEFAULT:**`None` |
| `checkpoint` | The checkpoint to resume from.<br>**TYPE:**`Checkpoint | None`**DEFAULT:**`None` |
| `checkpoint_during` | (deprecated) Whether to checkpoint during the run (or only at the end/interruption).<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `interrupt_before` | Nodes to interrupt immediately before they get executed.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | Nodes to Nodes to interrupt immediately after they get executed.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `webhook` | Webhook to call after LangGraph API call is done.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `on_disconnect` | The disconnect mode to use.<br>Must be one of 'cancel' or 'continue'.<br>**TYPE:**`DisconnectMode | None`**DEFAULT:**`None` |
| `on_completion` | Whether to delete or keep the thread created for a stateless run.<br>Must be one of 'delete' or 'keep'.<br>**TYPE:**`OnCompletionBehavior | None`**DEFAULT:**`None` |
| `multitask_strategy` | Multitask strategy to use.<br>Must be one of 'reject', 'interrupt', 'rollback', or 'enqueue'.<br>**TYPE:**`MultitaskStrategy | None`**DEFAULT:**`None` |
| `if_not_exists` | How to handle missing thread. Defaults to 'reject'.<br>Must be either 'reject' (raise error if missing), or 'create' (create new thread).<br>**TYPE:**`IfNotExists | None`**DEFAULT:**`None` |
| `after_seconds` | The number of seconds to wait before starting the run.<br>Use to schedule future runs.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `raise_error` | Whether to raise an error if the run fails.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `on_run_created` | Optional callback to call when a run is created.<br>**TYPE:**`Callable[[RunCreateMetadata], None] | None`**DEFAULT:**`None` |
| `durability` | The durability to use for the run. Values are "sync", "async", or "exit".<br>"async" means checkpoints are persisted async while next graph step executes, replaces checkpoint\_during=True<br>"sync" means checkpoints are persisted sync after graph step executes, replaces checkpoint\_during=False<br>"exit" means checkpoints are only persisted when the run exits, does not save intermediate steps<br>**TYPE:**`Durability | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[dict] | dict[str, Any]` | The output of the `Run`. |

Example Usage

```
final_state_of_run = client.runs.wait(
    thread_id=None,
    assistant_id="agent",
    input={"messages": [{"role": "user", "content": "how are you?"}]},
    metadata={"name":"my_run"},
    context={"model_name": "anthropic"},
    interrupt_before=["node_to_stop_before_1","node_to_stop_before_2"],
    interrupt_after=["node_to_stop_after_1","node_to_stop_after_2"],
    webhook="https://my.fake.webhook.com",
    multitask_strategy="interrupt"
)
print(final_state_of_run)
```

```
-------------------------------------------------------------------------------------------------------------------------------------------

{
    'messages': [\
        {\
            'content': 'how are you?',\
            'additional_kwargs': {},\
            'response_metadata': {},\
            'type': 'human',\
            'name': None,\
            'id': 'f51a862c-62fe-4866-863b-b0863e8ad78a',\
            'example': False\
        },\
        {\
            'content': "I'm doing well, thanks for asking! I'm an AI assistant created by Anthropic to be helpful, honest, and harmless.",\
            'additional_kwargs': {},\
            'response_metadata': {},\
            'type': 'ai',\
            'name': None,\
            'id': 'run-bf1cd3c6-768f-4c16-b62d-ba6f17ad8b36',\
            'example': False,\
            'tool_calls': [],\
            'invalid_tool_calls': [],\
            'usage_metadata': None\
        }\
    ]
}
```

#### ``list ¶

```
list(
    thread_id: str,
    *,
    limit: int = 10,
    offset: int = 0,
    status: RunStatus | None = None,
    select: list[RunSelectField] | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> list[Run]
```

List runs.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to list runs for.<br>**TYPE:**`str` |
| `limit` | The maximum number of results to return.<br>**TYPE:**`int`**DEFAULT:**`10` |
| `offset` | The number of results to skip.<br>**TYPE:**`int`**DEFAULT:**`0` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Run]` | The runs for the thread. |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
client.runs.list(
    thread_id="thread_id",
    limit=5,
    offset=5,
)
```

#### ``get ¶

```
get(
    thread_id: str,
    run_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Run
```

Get a run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to get.<br>**TYPE:**`str` |
| `run_id` | The run ID to get.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Run` | `Run` object. |

Example Usage

```
run = client.runs.get(
    thread_id="thread_id_to_delete",
    run_id="run_id_to_delete",
)
```

#### ``cancel ¶

```
cancel(
    thread_id: str,
    run_id: str,
    *,
    wait: bool = False,
    action: CancelAction = "interrupt",
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Get a run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to cancel.<br>**TYPE:**`str` |
| `run_id` | The run ID to cancel.<br>**TYPE:**`str` |
| `wait` | Whether to wait until run has completed.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `action` | Action to take when cancelling the run. Possible values<br>are `interrupt` or `rollback`. Default is `interrupt`.<br>**TYPE:**`CancelAction`**DEFAULT:**`'interrupt'` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
client.runs.cancel(
    thread_id="thread_id_to_cancel",
    run_id="run_id_to_cancel",
    wait=True,
    action="interrupt"
)
```

#### ``join ¶

```
join(
    thread_id: str,
    run_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> dict
```

Block until a run is done. Returns the final state of the thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to join.<br>**TYPE:**`str` |
| `run_id` | The run ID to join.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict` | `None` |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
client.runs.join(
    thread_id="thread_id_to_join",
    run_id="run_id_to_join"
)
```

#### ``join\_stream ¶

```
join_stream(
    thread_id: str,
    run_id: str,
    *,
    cancel_on_disconnect: bool = False,
    stream_mode: StreamMode | Sequence[StreamMode] | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
    last_event_id: str | None = None,
) -> Iterator[StreamPart]
```

Stream output from a run in real-time, until the run is done.
Output is not buffered, so any output produced before this call will
not be received here.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to join.<br>**TYPE:**`str` |
| `run_id` | The run ID to join.<br>**TYPE:**`str` |
| `stream_mode` | The stream mode(s) to use. Must be a subset of the stream modes passed<br>when creating the run. Background runs default to having the union of all<br>stream modes.<br>**TYPE:**`StreamMode | Sequence[StreamMode] | None`**DEFAULT:**`None` |
| `cancel_on_disconnect` | Whether to cancel the run when the stream is disconnected.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |
| `last_event_id` | The last event ID to use for the stream.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Iterator[StreamPart]` | `None` |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
client.runs.join_stream(
    thread_id="thread_id_to_join",
    run_id="run_id_to_join",
    stream_mode=["values", "debug"]
)
```

#### ``delete ¶

```
delete(
    thread_id: str,
    run_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Delete a run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to delete.<br>**TYPE:**`str` |
| `run_id` | The run ID to delete.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_sync_client(url="http://localhost:2024")
client.runs.delete(
    thread_id="thread_id_to_delete",
    run_id="run_id_to_delete"
)
```

### ``SyncCronClient ¶

Synchronous client for managing cron jobs in LangGraph.

This class provides methods to create and manage scheduled tasks (cron jobs) for automated graph executions.

Example

```
client = get_sync_client(url="http://localhost:8123")
cron_job = client.crons.create_for_thread(thread_id="thread_123", assistant_id="asst_456", schedule="0 * * * *")
```

Feature Availability

The crons client functionality is not supported on all licenses.
Please check the relevant license documentation for the most up-to-date
details on feature availability.

| METHOD | DESCRIPTION |
| --- | --- |
| `create_for_thread` | Create a cron job for a thread. |
| `create` | Create a cron run. |
| `delete` | Delete a cron. |
| `search` | Get a list of cron jobs. |
| `count` | Count cron jobs matching filters. |

#### ``create\_for\_thread ¶

```
create_for_thread(
    thread_id: str,
    assistant_id: str,
    *,
    schedule: str,
    input: Mapping[str, Any] | None = None,
    metadata: Mapping[str, Any] | None = None,
    config: Config | None = None,
    context: Context | None = None,
    checkpoint_during: bool | None = None,
    interrupt_before: All | list[str] | None = None,
    interrupt_after: All | list[str] | None = None,
    webhook: str | None = None,
    multitask_strategy: str | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Run
```

Create a cron job for a thread.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | the thread ID to run the cron job on.<br>**TYPE:**`str` |
| `assistant_id` | The assistant ID or graph name to use for the cron job.<br>If using graph name, will default to first assistant created from that graph.<br>**TYPE:**`str` |
| `schedule` | The cron schedule to execute this job on.<br>**TYPE:**`str` |
| `input` | The input to the graph.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `metadata` | Metadata to assign to the cron job runs.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `config` | The configuration for the assistant.<br>**TYPE:**`Config | None`**DEFAULT:**`None` |
| `context` | Static context to add to the assistant.<br>Added in version 0.6.0<br>**TYPE:**`Context | None`**DEFAULT:**`None` |
| `checkpoint_during` | Whether to checkpoint during the run (or only at the end/interruption).<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `interrupt_before` | Nodes to interrupt immediately before they get executed.<br>**TYPE:**`All | list[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | Nodes to Nodes to interrupt immediately after they get executed.<br>**TYPE:**`All | list[str] | None`**DEFAULT:**`None` |
| `webhook` | Webhook to call after LangGraph API call is done.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `multitask_strategy` | Multitask strategy to use.<br>Must be one of 'reject', 'interrupt', 'rollback', or 'enqueue'.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Run` | The cron `Run`. |

Example Usage

```
client = get_sync_client(url="http://localhost:8123")
cron_run = client.crons.create_for_thread(
    thread_id="my-thread-id",
    assistant_id="agent",
    schedule="27 15 * * *",
    input={"messages": [{"role": "user", "content": "hello!"}]},
    metadata={"name":"my_run"},
    context={"model_name": "openai"},
    interrupt_before=["node_to_stop_before_1","node_to_stop_before_2"],
    interrupt_after=["node_to_stop_after_1","node_to_stop_after_2"],
    webhook="https://my.fake.webhook.com",
    multitask_strategy="interrupt"
)
```

#### ``create ¶

```
create(
    assistant_id: str,
    *,
    schedule: str,
    input: Mapping[str, Any] | None = None,
    metadata: Mapping[str, Any] | None = None,
    config: Config | None = None,
    context: Context | None = None,
    checkpoint_during: bool | None = None,
    interrupt_before: All | list[str] | None = None,
    interrupt_after: All | list[str] | None = None,
    webhook: str | None = None,
    multitask_strategy: str | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Run
```

Create a cron run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The assistant ID or graph name to use for the cron job.<br>If using graph name, will default to first assistant created from that graph.<br>**TYPE:**`str` |
| `schedule` | The cron schedule to execute this job on.<br>**TYPE:**`str` |
| `input` | The input to the graph.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `metadata` | Metadata to assign to the cron job runs.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `config` | The configuration for the assistant.<br>**TYPE:**`Config | None`**DEFAULT:**`None` |
| `context` | Static context to add to the assistant.<br>Added in version 0.6.0<br>**TYPE:**`Context | None`**DEFAULT:**`None` |
| `checkpoint_during` | Whether to checkpoint during the run (or only at the end/interruption).<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `interrupt_before` | Nodes to interrupt immediately before they get executed.<br>**TYPE:**`All | list[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | Nodes to Nodes to interrupt immediately after they get executed.<br>**TYPE:**`All | list[str] | None`**DEFAULT:**`None` |
| `webhook` | Webhook to call after LangGraph API call is done.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `multitask_strategy` | Multitask strategy to use.<br>Must be one of 'reject', 'interrupt', 'rollback', or 'enqueue'.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Run` | The cron `Run`. |

Example Usage

```
client = get_sync_client(url="http://localhost:8123")
cron_run = client.crons.create(
    assistant_id="agent",
    schedule="27 15 * * *",
    input={"messages": [{"role": "user", "content": "hello!"}]},
    metadata={"name":"my_run"},
    context={"model_name": "openai"},
    checkpoint_during=True,
    interrupt_before=["node_to_stop_before_1","node_to_stop_before_2"],
    interrupt_after=["node_to_stop_after_1","node_to_stop_after_2"],
    webhook="https://my.fake.webhook.com",
    multitask_strategy="interrupt"
)
```

#### ``delete ¶

```
delete(
    cron_id: str,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Delete a cron.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `cron_id` | The cron ID to delete.<br>**TYPE:**`str` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_sync_client(url="http://localhost:8123")
client.crons.delete(
    cron_id="cron_to_delete"
)
```

#### ``search ¶

```
search(
    *,
    assistant_id: str | None = None,
    thread_id: str | None = None,
    limit: int = 10,
    offset: int = 0,
    sort_by: CronSortBy | None = None,
    sort_order: SortOrder | None = None,
    select: list[CronSelectField] | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> list[Cron]
```

Get a list of cron jobs.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | The assistant ID or graph name to search for.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `thread_id` | the thread ID to search for.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `limit` | The maximum number of results to return.<br>**TYPE:**`int`**DEFAULT:**`10` |
| `offset` | The number of results to skip.<br>**TYPE:**`int`**DEFAULT:**`0` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Cron]` | The list of cron jobs returned by the search, |

Example Usage

```
client = get_sync_client(url="http://localhost:8123")
cron_jobs = client.crons.search(
    assistant_id="my_assistant_id",
    thread_id="my_thread_id",
    limit=5,
    offset=5,
)
print(cron_jobs)
```

```
----------------------------------------------------------

[\
    {\
        'cron_id': '1ef3cefa-4c09-6926-96d0-3dc97fd5e39b',\
        'assistant_id': 'my_assistant_id',\
        'thread_id': 'my_thread_id',\
        'user_id': None,\
        'payload':\
            {\
                'input': {'start_time': ''},\
                'schedule': '4 * * * *',\
                'assistant_id': 'my_assistant_id'\
            },\
        'schedule': '4 * * * *',\
        'next_run_date': '2024-07-25T17:04:00+00:00',\
        'end_time': None,\
        'created_at': '2024-07-08T06:02:23.073257+00:00',\
        'updated_at': '2024-07-08T06:02:23.073257+00:00'\
    }\
]
```

#### ``count ¶

```
count(
    *,
    assistant_id: str | None = None,
    thread_id: str | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> int
```

Count cron jobs matching filters.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `assistant_id` | Assistant ID to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `thread_id` | Thread ID to filter by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `int` | Number of crons matching the criteria.<br>**TYPE:**`int` |

### ``SyncStoreClient ¶

A client for synchronous operations on a key-value store.

Provides methods to interact with a remote key-value store, allowing
storage and retrieval of items within namespaced hierarchies.

Example

```
client = get_sync_client(url="http://localhost:2024"))
client.store.put_item(["users", "profiles"], "user123", {"name": "Alice", "age": 30})
```

| METHOD | DESCRIPTION |
| --- | --- |
| `put_item` | Store or update an item. |
| `get_item` | Retrieve a single item. |
| `delete_item` | Delete an item. |
| `search_items` | Search for items within a namespace prefix. |
| `list_namespaces` | List namespaces with optional match conditions. |

#### ``put\_item ¶

```
put_item(
    namespace: Sequence[str],
    /,
    key: str,
    value: Mapping[str, Any],
    index: Literal[False] | list[str] | None = None,
    ttl: int | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Store or update an item.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `namespace` | A list of strings representing the namespace path.<br>**TYPE:**`Sequence[str]` |
| `key` | The unique identifier for the item within the namespace.<br>**TYPE:**`str` |
| `value` | A dictionary containing the item's data.<br>**TYPE:**`Mapping[str, Any]` |
| `index` | Controls search indexing - None (use defaults), False (disable), or list of field paths to index.<br>**TYPE:**`Literal[False] | list[str] | None`**DEFAULT:**`None` |
| `ttl` | Optional time-to-live in minutes for the item, or None for no expiration.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_sync_client(url="http://localhost:8123")
client.store.put_item(
    ["documents", "user123"],
    key="item456",
    value={"title": "My Document", "content": "Hello World"}
)
```

#### ``get\_item ¶

```
get_item(
    namespace: Sequence[str],
    /,
    key: str,
    *,
    refresh_ttl: bool | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> Item
```

Retrieve a single item.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `key` | The unique identifier for the item.<br>**TYPE:**`str` |
| `namespace` | Optional list of strings representing the namespace path.<br>**TYPE:**`Sequence[str]` |
| `refresh_ttl` | Whether to refresh the TTL on this read operation. If `None`, uses the store's default behavior.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Item` | The retrieved item. |

Example Usage

```
client = get_sync_client(url="http://localhost:8123")
item = client.store.get_item(
    ["documents", "user123"],
    key="item456",
)
print(item)
```

```
----------------------------------------------------------------

{
    'namespace': ['documents', 'user123'],
    'key': 'item456',
    'value': {'title': 'My Document', 'content': 'Hello World'},
    'created_at': '2024-07-30T12:00:00Z',
    'updated_at': '2024-07-30T12:00:00Z'
}
```

#### ``delete\_item ¶

```
delete_item(
    namespace: Sequence[str],
    /,
    key: str,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> None
```

Delete an item.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `key` | The unique identifier for the item.<br>**TYPE:**`str` |
| `namespace` | Optional list of strings representing the namespace path.<br>**TYPE:**`Sequence[str]` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | `None` |

Example Usage

```
client = get_sync_client(url="http://localhost:8123")
client.store.delete_item(
    ["documents", "user123"],
    key="item456",
)
```

#### ``search\_items ¶

```
search_items(
    namespace_prefix: Sequence[str],
    /,
    filter: Mapping[str, Any] | None = None,
    limit: int = 10,
    offset: int = 0,
    query: str | None = None,
    refresh_ttl: bool | None = None,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> SearchItemsResponse
```

Search for items within a namespace prefix.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `namespace_prefix` | List of strings representing the namespace prefix.<br>**TYPE:**`Sequence[str]` |
| `filter` | Optional dictionary of key-value pairs to filter results.<br>**TYPE:**`Mapping[str, Any] | None`**DEFAULT:**`None` |
| `limit` | Maximum number of items to return (default is 10).<br>**TYPE:**`int`**DEFAULT:**`10` |
| `offset` | Number of items to skip before returning results (default is 0).<br>**TYPE:**`int`**DEFAULT:**`0` |
| `query` | Optional query for natural language search.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `refresh_ttl` | Whether to refresh the TTL on items returned by this search. If `None`, uses the store's default behavior.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `params` | Optional query parameters to include with the request.<br>**TYPE:**`QueryParamTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `SearchItemsResponse` | A list of items matching the search criteria. |

Example Usage

```
client = get_sync_client(url="http://localhost:8123")
items = client.store.search_items(
    ["documents"],
    filter={"author": "John Doe"},
    limit=5,
    offset=0
)
print(items)
```

```
----------------------------------------------------------------

{
    "items": [\
        {\
            "namespace": ["documents", "user123"],\
            "key": "item789",\
            "value": {\
                "title": "Another Document",\
                "author": "John Doe"\
            },\
            "created_at": "2024-07-30T12:00:00Z",\
            "updated_at": "2024-07-30T12:00:00Z"\
        },\
        # ... additional items ...\
    ]
}
```

#### ``list\_namespaces ¶

```
list_namespaces(
    prefix: list[str] | None = None,
    suffix: list[str] | None = None,
    max_depth: int | None = None,
    limit: int = 100,
    offset: int = 0,
    *,
    headers: Mapping[str, str] | None = None,
    params: QueryParamTypes | None = None,
) -> ListNamespaceResponse
```

List namespaces with optional match conditions.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `prefix` | Optional list of strings representing the prefix to filter namespaces.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `suffix` | Optional list of strings representing the suffix to filter namespaces.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `max_depth` | Optional integer specifying the maximum depth of namespaces to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `limit` | Maximum number of namespaces to return (default is 100).<br>**TYPE:**`int`**DEFAULT:**`100` |
| `offset` | Number of namespaces to skip before returning results (default is 0).<br>**TYPE:**`int`**DEFAULT:**`0` |
| `headers` | Optional custom headers to include with the request.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ListNamespaceResponse` | A list of namespaces matching the criteria. |

Example Usage

```
client = get_sync_client(url="http://localhost:8123")
namespaces = client.store.list_namespaces(
    prefix=["documents"],
    max_depth=3,
    limit=10,
    offset=0
)
print(namespaces)
```

```
----------------------------------------------------------------

[\
    ["documents", "user123", "reports"],\
    ["documents", "user456", "invoices"],\
    ...\
]
```

### ``get\_client ¶

```
get_client(
    *,
    url: str | None = None,
    api_key: str | None = None,
    headers: Mapping[str, str] | None = None,
    timeout: TimeoutTypes | None = None,
) -> LangGraphClient
```

Create and configure a LangGraphClient.

The client provides programmatic access to LangSmith Deployment. It supports
both remote servers and local in-process connections (when running inside a LangGraph server).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `url` | Base URL of the LangGraph API.<br>– If `None`, the client first attempts an in-process connection via ASGI transport.<br>If that fails, it falls back to `http://localhost:8123`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `api_key` | API key for authentication. If omitted, the client reads from environment<br>variables in the following order:<br>1\. Function argument<br>2\. `LANGGRAPH_API_KEY`<br>3\. `LANGSMITH_API_KEY`<br>4\. `LANGCHAIN_API_KEY`<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Additional HTTP headers to include in requests. Merged with authentication headers.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `timeout` | HTTP timeout configuration. May be:<br>– `httpx.Timeout` instance<br>– float (total seconds)<br>– tuple `(connect, read, write, pool)` in seconds<br>Defaults: connect=5, read=300, write=300, pool=5.<br>**TYPE:**`TimeoutTypes | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `LangGraphClient` | A top-level client exposing sub-clients for assistants, threads,<br>runs, and cron operations.<br>**TYPE:**`LangGraphClient` |

Connect to a remote server:

```
from langgraph_sdk import get_client

# get top-level LangGraphClient
client = get_client(url="http://localhost:8123")

# example usage: client.<model>.<method_name>()
assistants = await client.assistants.get(assistant_id="some_uuid")
```

Connect in-process to a running LangGraph server:

```
from langgraph_sdk import get_client

client = get_client(url=None)

async def my_node(...):
    subagent_result = await client.runs.wait(
        thread_id=None,
        assistant_id="agent",
        input={"messages": [{"role": "user", "content": "Foo"}]},
    )
```

### ``get\_sync\_client ¶

```
get_sync_client(
    *,
    url: str | None = None,
    api_key: str | None = None,
    headers: Mapping[str, str] | None = None,
    timeout: TimeoutTypes | None = None,
) -> SyncLangGraphClient
```

Get a synchronous LangGraphClient instance.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `url` | The URL of the LangGraph API.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `api_key` | The API key. If not provided, it will be read from the environment.<br>Precedence:<br>1\. explicit argument<br>2\. LANGGRAPH\_API\_KEY<br>3\. LANGSMITH\_API\_KEY<br>4\. LANGCHAIN\_API\_KEY<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Optional custom headers<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |
| `timeout` | Optional timeout configuration for the HTTP client.<br>Accepts an httpx.Timeout instance, a float (seconds), or a tuple of timeouts.<br>Tuple format is (connect, read, write, pool)<br>If not provided, defaults to connect=5s, read=300s, write=300s, and pool=5s.<br>**TYPE:**`TimeoutTypes | None`**DEFAULT:**`None` |

Returns:
SyncLangGraphClient: The top-level synchronous client for accessing AssistantsClient,
ThreadsClient, RunsClient, and CronClient.

Example

```
from langgraph_sdk import get_sync_client

# get top-level synchronous LangGraphClient
client = get_sync_client(url="http://localhost:8123")

# example usage: client.<model>.<method_name>()
assistant = client.assistants.get(assistant_id="some_uuid")
```

## ``langgraph\_sdk.schema ¶

Data models for interacting with the LangGraph API.

### ``Json`module-attribute`¶

```
Json = dict[str, Any] | None
```

Represents a JSON-like structure, which can be None or a dictionary with string keys and any values.

### ``RunStatus`module-attribute`¶

```
RunStatus = Literal['pending', 'running', 'error', 'success', 'timeout', 'interrupted']
```

Represents the status of a run:
\- "pending": The run is waiting to start.
\- "running": The run is currently executing.
\- "error": The run encountered an error and stopped.
\- "success": The run completed successfully.
\- "timeout": The run exceeded its time limit.
\- "interrupted": The run was manually stopped or interrupted.

### ``ThreadStatus`module-attribute`¶

```
ThreadStatus = Literal['idle', 'busy', 'interrupted', 'error']
```

Represents the status of a thread:
\- "idle": The thread is not currently processing any task.
\- "busy": The thread is actively processing a task.
\- "interrupted": The thread's execution was interrupted.
\- "error": An exception occurred during task processing.

### ``ThreadStreamMode`module-attribute`¶

```
ThreadStreamMode = Literal['run_modes', 'lifecycle', 'state_update']
```

Defines the mode of streaming:
\- "run\_modes": Stream the same events as the runs on thread, as well as run\_done events.
\- "lifecycle": Stream only run start/end events.
\- "state\_update": Stream state updates on the thread.

### ``StreamMode`module-attribute`¶

```
StreamMode = Literal[\
    "values",\
    "messages",\
    "updates",\
    "events",\
    "tasks",\
    "checkpoints",\
    "debug",\
    "custom",\
    "messages-tuple",\
]
```

Defines the mode of streaming:
\- "values": Stream only the values.
\- "messages": Stream complete messages.
\- "updates": Stream updates to the state.
\- "events": Stream events occurring during execution.
\- "checkpoints": Stream checkpoints as they are created.
\- "tasks": Stream task start and finish events.
\- "debug": Stream detailed debug information.
\- "custom": Stream custom events.

### ``DisconnectMode`module-attribute`¶

```
DisconnectMode = Literal['cancel', 'continue']
```

Specifies behavior on disconnection:
\- "cancel": Cancel the operation on disconnection.
\- "continue": Continue the operation even if disconnected.

### ``MultitaskStrategy`module-attribute`¶

```
MultitaskStrategy = Literal['reject', 'interrupt', 'rollback', 'enqueue']
```

Defines how to handle multiple tasks:
\- "reject": Reject new tasks when busy.
\- "interrupt": Interrupt current task for new ones.
\- "rollback": Roll back current task and start new one.
\- "enqueue": Queue new tasks for later execution.

### ``OnConflictBehavior`module-attribute`¶

```
OnConflictBehavior = Literal['raise', 'do_nothing']
```

Specifies behavior on conflict:
\- "raise": Raise an exception when a conflict occurs.
\- "do\_nothing": Ignore conflicts and proceed.

### ``OnCompletionBehavior`module-attribute`¶

```
OnCompletionBehavior = Literal['delete', 'keep']
```

Defines action after completion:
\- "delete": Delete resources after completion.
\- "keep": Retain resources after completion.

### ``Durability`module-attribute`¶

```
Durability = Literal['sync', 'async', 'exit']
```

Durability mode for the graph execution.
\- `"sync"`: Changes are persisted synchronously before the next step starts.
\- `"async"`: Changes are persisted asynchronously while the next step executes.
\- `"exit"`: Changes are persisted only when the graph exits.

### ``All`module-attribute`¶

```
All = Literal['*']
```

Represents a wildcard or 'all' selector.

### ``IfNotExists`module-attribute`¶

```
IfNotExists = Literal['create', 'reject']
```

Specifies behavior if the thread doesn't exist:
\- "create": Create a new thread if it doesn't exist.
\- "reject": Reject the operation if the thread doesn't exist.

### ``CancelAction`module-attribute`¶

```
CancelAction = Literal['interrupt', 'rollback']
```

Action to take when cancelling the run.
\- "interrupt": Simply cancel the run.
\- "rollback": Cancel the run. Then delete the run and associated checkpoints.

### ``AssistantSortBy`module-attribute`¶

```
AssistantSortBy = Literal[\
    "assistant_id", "graph_id", "name", "created_at", "updated_at"\
]
```

The field to sort by.

### ``ThreadSortBy`module-attribute`¶

```
ThreadSortBy = Literal['thread_id', 'status', 'created_at', 'updated_at']
```

The field to sort by.

### ``CronSortBy`module-attribute`¶

```
CronSortBy = Literal[\
    "cron_id", "assistant_id", "thread_id", "created_at", "updated_at", "next_run_date"\
]
```

The field to sort by.

### ``SortOrder`module-attribute`¶

```
SortOrder = Literal['asc', 'desc']
```

The order to sort by.

### ``Config ¶

Bases: `TypedDict`

Configuration options for a call.

#### ``tags`instance-attribute`¶

```
tags: list[str]
```

Tags for this call and any sub-calls (eg. a Chain calling an LLM).
You can use these to filter calls.

#### ``recursion\_limit`instance-attribute`¶

```
recursion_limit: int
```

Maximum number of times a call can recurse. If not provided, defaults to 25.

#### ``configurable`instance-attribute`¶

```
configurable: dict[str, Any]
```

Runtime values for attributes previously made configurable on this Runnable,
or sub-Runnables, through .configurable\_fields() or .configurable\_alternatives().
Check .output\_schema() for a description of the attributes that have been made
configurable.

### ``Checkpoint ¶

Bases: `TypedDict`

Represents a checkpoint in the execution process.

#### ``thread\_id`instance-attribute`¶

```
thread_id: str
```

Unique identifier for the thread associated with this checkpoint.

#### ``checkpoint\_ns`instance-attribute`¶

```
checkpoint_ns: str
```

Namespace for the checkpoint; used internally to manage subgraph state.

#### ``checkpoint\_id`instance-attribute`¶

```
checkpoint_id: str | None
```

Optional unique identifier for the checkpoint itself.

#### ``checkpoint\_map`instance-attribute`¶

```
checkpoint_map: dict[str, Any] | None
```

Optional dictionary containing checkpoint-specific data.

### ``GraphSchema ¶

Bases: `TypedDict`

Defines the structure and properties of a graph.

#### ``graph\_id`instance-attribute`¶

```
graph_id: str
```

The ID of the graph.

#### ``input\_schema`instance-attribute`¶

```
input_schema: dict | None
```

The schema for the graph input.
Missing if unable to generate JSON schema from graph.

#### ``output\_schema`instance-attribute`¶

```
output_schema: dict | None
```

The schema for the graph output.
Missing if unable to generate JSON schema from graph.

#### ``state\_schema`instance-attribute`¶

```
state_schema: dict | None
```

The schema for the graph state.
Missing if unable to generate JSON schema from graph.

#### ``config\_schema`instance-attribute`¶

```
config_schema: dict | None
```

The schema for the graph config.
Missing if unable to generate JSON schema from graph.

#### ``context\_schema`instance-attribute`¶

```
context_schema: dict | None
```

The schema for the graph context.
Missing if unable to generate JSON schema from graph.

### ``AssistantBase ¶

Bases: `TypedDict`

Base model for an assistant.

#### ``assistant\_id`instance-attribute`¶

```
assistant_id: str
```

The ID of the assistant.

#### ``graph\_id`instance-attribute`¶

```
graph_id: str
```

The ID of the graph.

#### ``config`instance-attribute`¶

```
config: Config
```

The assistant config.

#### ``context`instance-attribute`¶

```
context: Context
```

The static context of the assistant.

#### ``created\_at`instance-attribute`¶

```
created_at: datetime
```

The time the assistant was created.

#### ``metadata`instance-attribute`¶

```
metadata: Json
```

The assistant metadata.

#### ``version`instance-attribute`¶

```
version: int
```

The version of the assistant

#### ``name`instance-attribute`¶

```
name: str
```

The name of the assistant

#### ``description`instance-attribute`¶

```
description: str | None
```

The description of the assistant

### ``AssistantVersion ¶

Bases: `AssistantBase`

Represents a specific version of an assistant.

#### ``assistant\_id`instance-attribute`¶

```
assistant_id: str
```

The ID of the assistant.

#### ``graph\_id`instance-attribute`¶

```
graph_id: str
```

The ID of the graph.

#### ``config`instance-attribute`¶

```
config: Config
```

The assistant config.

#### ``context`instance-attribute`¶

```
context: Context
```

The static context of the assistant.

#### ``created\_at`instance-attribute`¶

```
created_at: datetime
```

The time the assistant was created.

#### ``metadata`instance-attribute`¶

```
metadata: Json
```

The assistant metadata.

#### ``version`instance-attribute`¶

```
version: int
```

The version of the assistant

#### ``name`instance-attribute`¶

```
name: str
```

The name of the assistant

#### ``description`instance-attribute`¶

```
description: str | None
```

The description of the assistant

### ``Assistant ¶

Bases: `AssistantBase`

Represents an assistant with additional properties.

#### ``updated\_at`instance-attribute`¶

```
updated_at: datetime
```

The last time the assistant was updated.

#### ``assistant\_id`instance-attribute`¶

```
assistant_id: str
```

The ID of the assistant.

#### ``graph\_id`instance-attribute`¶

```
graph_id: str
```

The ID of the graph.

#### ``config`instance-attribute`¶

```
config: Config
```

The assistant config.

#### ``context`instance-attribute`¶

```
context: Context
```

The static context of the assistant.

#### ``created\_at`instance-attribute`¶

```
created_at: datetime
```

The time the assistant was created.

#### ``metadata`instance-attribute`¶

```
metadata: Json
```

The assistant metadata.

#### ``version`instance-attribute`¶

```
version: int
```

The version of the assistant

#### ``name`instance-attribute`¶

```
name: str
```

The name of the assistant

#### ``description`instance-attribute`¶

```
description: str | None
```

The description of the assistant

### ``Interrupt ¶

Bases: `TypedDict`

Represents an interruption in the execution flow.

#### ``value`instance-attribute`¶

```
value: Any
```

The value associated with the interrupt.

#### ``id`instance-attribute`¶

```
id: str
```

The ID of the interrupt. Can be used to resume the interrupt.

### ``Thread ¶

Bases: `TypedDict`

Represents a conversation thread.

#### ``thread\_id`instance-attribute`¶

```
thread_id: str
```

The ID of the thread.

#### ``created\_at`instance-attribute`¶

```
created_at: datetime
```

The time the thread was created.

#### ``updated\_at`instance-attribute`¶

```
updated_at: datetime
```

The last time the thread was updated.

#### ``metadata`instance-attribute`¶

```
metadata: Json
```

The thread metadata.

#### ``status`instance-attribute`¶

```
status: ThreadStatus
```

The status of the thread, one of 'idle', 'busy', 'interrupted'.

#### ``values`instance-attribute`¶

```
values: Json
```

The current state of the thread.

#### ``interrupts`instance-attribute`¶

```
interrupts: dict[str, list[Interrupt]]
```

Mapping of task ids to interrupts that were raised in that task.

### ``ThreadTask ¶

Bases: `TypedDict`

Represents a task within a thread.

### ``ThreadState ¶

Bases: `TypedDict`

Represents the state of a thread.

#### ``values`instance-attribute`¶

```
values: list[dict] | dict[str, Any]
```

The state values.

#### ``next`instance-attribute`¶

```
next: Sequence[str]
```

The next nodes to execute. If empty, the thread is done until new input is
received.

#### ``checkpoint`instance-attribute`¶

```
checkpoint: Checkpoint
```

The ID of the checkpoint.

#### ``metadata`instance-attribute`¶

```
metadata: Json
```

Metadata for this state

#### ``created\_at`instance-attribute`¶

```
created_at: str | None
```

Timestamp of state creation

#### ``parent\_checkpoint`instance-attribute`¶

```
parent_checkpoint: Checkpoint | None
```

The ID of the parent checkpoint. If missing, this is the root checkpoint.

#### ``tasks`instance-attribute`¶

```
tasks: Sequence[ThreadTask]
```

Tasks to execute in this step. If already attempted, may contain an error.

#### ``interrupts`instance-attribute`¶

```
interrupts: list[Interrupt]
```

Interrupts which were thrown in this thread.

### ``ThreadUpdateStateResponse ¶

Bases: `TypedDict`

Represents the response from updating a thread's state.

#### ``checkpoint`instance-attribute`¶

```
checkpoint: Checkpoint
```

Checkpoint of the latest state.

### ``Run ¶

Bases: `TypedDict`

Represents a single execution run.

#### ``run\_id`instance-attribute`¶

```
run_id: str
```

The ID of the run.

#### ``thread\_id`instance-attribute`¶

```
thread_id: str
```

The ID of the thread.

#### ``assistant\_id`instance-attribute`¶

```
assistant_id: str
```

The assistant that was used for this run.

#### ``created\_at`instance-attribute`¶

```
created_at: datetime
```

The time the run was created.

#### ``updated\_at`instance-attribute`¶

```
updated_at: datetime
```

The last time the run was updated.

#### ``status`instance-attribute`¶

```
status: RunStatus
```

The status of the run. One of 'pending', 'running', "error", 'success', "timeout", "interrupted".

#### ``metadata`instance-attribute`¶

```
metadata: Json
```

The run metadata.

#### ``multitask\_strategy`instance-attribute`¶

```
multitask_strategy: MultitaskStrategy
```

Strategy to handle concurrent runs on the same thread.

### ``Cron ¶

Bases: `TypedDict`

Represents a scheduled task.

#### ``cron\_id`instance-attribute`¶

```
cron_id: str
```

The ID of the cron.

#### ``assistant\_id`instance-attribute`¶

```
assistant_id: str
```

The ID of the assistant.

#### ``thread\_id`instance-attribute`¶

```
thread_id: str | None
```

The ID of the thread.

#### ``end\_time`instance-attribute`¶

```
end_time: datetime | None
```

The end date to stop running the cron.

#### ``schedule`instance-attribute`¶

```
schedule: str
```

The schedule to run, cron format.

#### ``created\_at`instance-attribute`¶

```
created_at: datetime
```

The time the cron was created.

#### ``updated\_at`instance-attribute`¶

```
updated_at: datetime
```

The last time the cron was updated.

#### ``payload`instance-attribute`¶

```
payload: dict
```

The run payload to use for creating new run.

#### ``user\_id`instance-attribute`¶

```
user_id: str | None
```

The user ID of the cron.

#### ``next\_run\_date`instance-attribute`¶

```
next_run_date: datetime | None
```

The next run date of the cron.

#### ``metadata`instance-attribute`¶

```
metadata: dict
```

The metadata of the cron.

### ``RunCreate ¶

Bases: `TypedDict`

Defines the parameters for initiating a background run.

#### ``thread\_id`instance-attribute`¶

```
thread_id: str | None
```

The identifier of the thread to run. If not provided, the run is stateless.

#### ``assistant\_id`instance-attribute`¶

```
assistant_id: str
```

The identifier of the assistant to use for this run.

#### ``input`instance-attribute`¶

```
input: dict | None
```

Initial input data for the run.

#### ``metadata`instance-attribute`¶

```
metadata: dict | None
```

Additional metadata to associate with the run.

#### ``config`instance-attribute`¶

```
config: Config | None
```

Configuration options for the run.

#### ``context`instance-attribute`¶

```
context: Context | None
```

The static context of the run.

#### ``checkpoint\_id`instance-attribute`¶

```
checkpoint_id: str | None
```

The identifier of a checkpoint to resume from.

#### ``interrupt\_before`instance-attribute`¶

```
interrupt_before: list[str] | None
```

List of node names to interrupt execution before.

#### ``interrupt\_after`instance-attribute`¶

```
interrupt_after: list[str] | None
```

List of node names to interrupt execution after.

#### ``webhook`instance-attribute`¶

```
webhook: str | None
```

URL to send webhook notifications about the run's progress.

#### ``multitask\_strategy`instance-attribute`¶

```
multitask_strategy: MultitaskStrategy | None
```

Strategy for handling concurrent runs on the same thread.

### ``Item ¶

Bases: `TypedDict`

Represents a single document or data entry in the graph's Store.

Items are used to store cross-thread memories.

#### ``namespace`instance-attribute`¶

```
namespace: list[str]
```

The namespace of the item. A namespace is analogous to a document's directory.

#### ``key`instance-attribute`¶

```
key: str
```

The unique identifier of the item within its namespace.

In general, keys needn't be globally unique.

#### ``value`instance-attribute`¶

```
value: dict[str, Any]
```

The value stored in the item. This is the document itself.

#### ``created\_at`instance-attribute`¶

```
created_at: datetime
```

The timestamp when the item was created.

#### ``updated\_at`instance-attribute`¶

```
updated_at: datetime
```

The timestamp when the item was last updated.

### ``ListNamespaceResponse ¶

Bases: `TypedDict`

Response structure for listing namespaces.

#### ``namespaces`instance-attribute`¶

```
namespaces: list[list[str]]
```

A list of namespace paths, where each path is a list of strings.

### ``SearchItem ¶

Bases: `Item`

Item with an optional relevance score from search operations.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `score` | Relevance/similarity score. Included when<br>searching a compatible store with a natural language query.<br>**TYPE:**`Optional[float]` |

#### ``namespace`instance-attribute`¶

```
namespace: list[str]
```

The namespace of the item. A namespace is analogous to a document's directory.

#### ``key`instance-attribute`¶

```
key: str
```

The unique identifier of the item within its namespace.

In general, keys needn't be globally unique.

#### ``value`instance-attribute`¶

```
value: dict[str, Any]
```

The value stored in the item. This is the document itself.

#### ``created\_at`instance-attribute`¶

```
created_at: datetime
```

The timestamp when the item was created.

#### ``updated\_at`instance-attribute`¶

```
updated_at: datetime
```

The timestamp when the item was last updated.

### ``SearchItemsResponse ¶

Bases: `TypedDict`

Response structure for searching items.

#### ``items`instance-attribute`¶

```
items: list[SearchItem]
```

A list of items matching the search criteria.

### ``StreamPart ¶

Bases: `NamedTuple`

Represents a part of a stream response.

#### ``event`instance-attribute`¶

```
event: str
```

The type of event for this stream part.

#### ``data`instance-attribute`¶

```
data: dict
```

The data payload associated with the event.

### ``Send ¶

Bases: `TypedDict`

Represents a message to be sent to a specific node in the graph.

This type is used to explicitly send messages to nodes in the graph, typically
used within Command objects to control graph execution flow.

#### ``node`instance-attribute`¶

```
node: str
```

The name of the target node to send the message to.

#### ``input`instance-attribute`¶

```
input: dict[str, Any] | None
```

Optional dictionary containing the input data to be passed to the node.

If None, the node will be called with no input.

### ``Command ¶

Bases: `TypedDict`

Represents one or more commands to control graph execution flow and state.

This type defines the control commands that can be returned by nodes to influence
graph execution. It lets you navigate to other nodes, update graph state,
and resume from interruptions.

#### ``goto`instance-attribute`¶

```
goto: Send | str | Sequence[Send | str]
```

Specifies where execution should continue. Can be:

- A string node name to navigate to
- A Send object to execute a node with specific input
- A sequence of node names or Send objects to execute in order

#### ``update`instance-attribute`¶

```
update: dict[str, Any] | Sequence[tuple[str, Any]]
```

Updates to apply to the graph's state. Can be:

- A dictionary of state updates to merge
- A sequence of (key, value) tuples for ordered updates

#### ``resume`instance-attribute`¶

```
resume: Any
```

Value to resume execution with after an interruption.
Used in conjunction with interrupt() to implement control flow.

### ``RunCreateMetadata ¶

Bases: `TypedDict`

Metadata for a run creation request.

#### ``run\_id`instance-attribute`¶

```
run_id: str
```

The ID of the run.

#### ``thread\_id`instance-attribute`¶

```
thread_id: str | None
```

The ID of the thread.

## ``langgraph\_sdk.auth ¶

### ``Auth ¶

Add custom authentication and authorization management to your LangGraph application.

The Auth class provides a unified system for handling authentication and
authorization in LangGraph applications. It supports custom user authentication
protocols and fine-grained authorization rules for different resources and
actions.

To use, create a separate python file and add the path to the file to your
LangGraph API configuration file (`langgraph.json`). Within that file, create
an instance of the Auth class and register authentication and authorization
handlers as needed.

Example `langgraph.json` file:

```
{
  "dependencies": ["."],
  "graphs": {
    "agent": "./my_agent/agent.py:graph"
  },
  "env": ".env",
  "auth": {
    "path": "./auth.py:my_auth"
  }
```

Then the LangGraph server will load your auth file and run it server-side whenever a request comes in.

Basic Usage

```
from langgraph_sdk import Auth

my_auth = Auth()

async def verify_token(token: str) -> str:
    # Verify token and return user_id
    # This would typically be a call to your auth server
    return "user_id"

@auth.authenticate
async def authenticate(authorization: str) -> str:
    # Verify token and return user_id
    result = await verify_token(authorization)
    if result != "user_id":
        raise Auth.exceptions.HTTPException(
            status_code=401, detail="Unauthorized"
        )
    return result

# Global fallback handler
@auth.on
async def authorize_default(params: Auth.on.value):
    return False # Reject all requests (default behavior)

@auth.on.threads.create
async def authorize_thread_create(params: Auth.on.threads.create.value):
    # Allow the allowed user to create a thread
    assert params.get("metadata", {}).get("owner") == "allowed_user"

@auth.on.store
async def authorize_store(ctx: Auth.types.AuthContext, value: Auth.types.on):
    assert ctx.user.identity in value["namespace"], "Not authorized"
```

Request Processing Flow

1. Authentication (your `@auth.authenticate` handler) is performed first on **every request**
2. For authorization, the most specific matching handler is called:
   - If a handler exists for the exact resource and action, it is used (e.g., `@auth.on.threads.create`)
   - Otherwise, if a handler exists for the resource with any action, it is used (e.g., `@auth.on.threads`)
   - Finally, if no specific handlers match, the global handler is used (e.g., `@auth.on`)
   - If no global handler is set, the request is accepted

This allows you to set default behavior with a global handler while
overriding specific routes as needed.

| METHOD | DESCRIPTION |
| --- | --- |
| `authenticate` | Register an authentication handler function. |

#### ``types`class-attribute``instance-attribute`¶

```
types = types
```

Reference to auth type definitions.

Provides access to all type definitions used in the auth system,
like ThreadsCreate, AssistantsRead, etc.

#### ``exceptions`class-attribute``instance-attribute`¶

```
exceptions = exceptions
```

Reference to auth exception definitions.

Provides access to all exception definitions used in the auth system,
like HTTPException, etc.

#### ``on`instance-attribute`¶

```
on = _On(self)
```

Entry point for authorization handlers that control access to specific resources.

The on class provides a flexible way to define authorization rules for different
resources and actions in your application. It supports three main usage patterns:

1. Global handlers that run for all resources and actions
2. Resource-specific handlers that run for all actions on a resource
3. Resource and action specific handlers for fine-grained control

Each handler must be an async function that accepts two parameters

- ctx (AuthContext): Contains request context and authenticated user info
- value: The data being authorized (type varies by endpoint)

The handler should return one of:

```
- None or True: Accept the request
- False: Reject with 403 error
- FilterType: Apply filtering rules to the response
```

Examples

Global handler for all requests:

```
@auth.on
async def reject_unhandled_requests(ctx: AuthContext, value: Any) -> None:
    print(f"Request to {ctx.path} by {ctx.user.identity}")
    return False
```

Resource-specific handler. This would take precedence over the global handler
for all actions on the `threads` resource:

```
@auth.on.threads
async def check_thread_access(ctx: AuthContext, value: Any) -> bool:
    # Allow access only to threads created by the user
    return value.get("created_by") == ctx.user.identity
```

Resource and action specific handler:

```
@auth.on.threads.delete
async def prevent_thread_deletion(ctx: AuthContext, value: Any) -> bool:
    # Only admins can delete threads
    return "admin" in ctx.user.permissions
```

Multiple resources or actions:

```
@auth.on(resources=["threads", "runs"], actions=["create", "update"])
async def rate_limit_writes(ctx: AuthContext, value: Any) -> bool:
    # Implement rate limiting for write operations
    return await check_rate_limit(ctx.user.identity)
```

Auth for the `store` resource is a bit different since its structure is developer defined.
You typically want to enforce user creds in the namespace.

```
@auth.on.store
async def check_store_access(ctx: AuthContext, value: Auth.types.on) -> bool:
    # Assuming you structure your store like (store.aput((user_id, application_context), key, value))
    assert value"namespace" == ctx.user.identity
```

#### ``authenticate ¶

```
authenticate(fn: AH) -> AH
```

Register an authentication handler function.

The authentication handler is responsible for verifying credentials
and returning user scopes. It can accept any of the following parameters
by name:

```
- request (Request): The raw ASGI request object
- path (str): The request path, e.g., "/threads/abcd-1234-abcd-1234/runs/abcd-1234-abcd-1234/stream"
- method (str): The HTTP method, e.g., "GET"
- path_params (dict[str, str]): URL path parameters, e.g., {"thread_id": "abcd-1234-abcd-1234", "run_id": "abcd-1234-abcd-1234"}
- query_params (dict[str, str]): URL query parameters, e.g., {"stream": "true"}
- headers (dict[bytes, bytes]): Request headers
- authorization (str | None): The Authorization header value (e.g., "Bearer <token>")
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `fn` | The authentication handler function to register.<br>Must return a representation of the user. This could be a:<br>\- string (the user id)<br>\- dict containing {"identity": str, "permissions": list\[str\]}<br>\- or an object with identity and permissions properties<br>Permissions can be optionally used by your handlers downstream.<br>**TYPE:**`AH` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `AH` | The registered handler function. |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If an authentication handler is already registered. |

Examples

Basic token authentication:

```
@auth.authenticate
async def authenticate(authorization: str) -> str:
    user_id = verify_token(authorization)
    return user_id
```

Accept the full request context:

```
@auth.authenticate
async def authenticate(
    method: str,
    path: str,
    headers: dict[str, bytes]
) -> str:
    user = await verify_request(method, path, headers)
    return user
```

Return user name and permissions:

```
@auth.authenticate
async def authenticate(
    method: str,
    path: str,
    headers: dict[str, bytes]
) -> Auth.types.MinimalUserDict:
    permissions, user = await verify_request(method, path, headers)
    # Permissions could be things like ["runs:read", "runs:write", "threads:read", "threads:write"]
    return {
        "identity": user["id"],
        "permissions": permissions,
        "display_name": user["name"],
    }
```

## ``langgraph\_sdk.auth.types ¶

Authentication and authorization types for LangGraph.

This module defines the core types used for authentication, authorization, and
request handling in LangGraph. It includes user protocols, authentication contexts,
and typed dictionaries for various API operations.

Note

All typing.TypedDict classes use total=False to make all fields typing.Optional by default.

### ``RunStatus`module-attribute`¶

```
RunStatus = Literal['pending', 'error', 'success', 'timeout', 'interrupted']
```

Status of a run execution.

Values

- pending: Run is queued or in progress
- error: Run failed with an error
- success: Run completed successfully
- timeout: Run exceeded time limit
- interrupted: Run was manually interrupted

### ``MultitaskStrategy`module-attribute`¶

```
MultitaskStrategy = Literal['reject', 'rollback', 'interrupt', 'enqueue']
```

Strategy for handling multiple concurrent tasks.

Values

- reject: Reject new tasks while one is in progress
- rollback: Cancel current task and start new one
- interrupt: Interrupt current task and start new one
- enqueue: Queue new tasks to run after current one

### ``OnConflictBehavior`module-attribute`¶

```
OnConflictBehavior = Literal['raise', 'do_nothing']
```

Behavior when encountering conflicts.

Values

- raise: Raise an exception on conflict
- do\_nothing: Silently ignore conflicts

### ``IfNotExists`module-attribute`¶

```
IfNotExists = Literal['create', 'reject']
```

Behavior when an entity doesn't exist.

Values

- create: Create the entity
- reject: Reject the operation

### ``FilterType`module-attribute`¶

```
FilterType = (
    dict[\
        str,\
        str\
        | dict[Literal["$eq", "$contains"], str]\
        | dict[Literal["$contains"], list[str]],\
    ]
    | dict[str, str]
)
```

Response type for authorization handlers.

Supports exact matches and operators

- Exact match shorthand: {"field": "value"}
- Exact match: {"field": {"$eq": "value"}}
- Contains (membership): {"field": {"$contains": "value"}}
- Contains (subset containment): {"field": {"$contains": \["value1", "value2"\]}}

Subset containment is only supported by newer versions of the LangGraph dev server;
install langgraph-runtime-inmem >= 0.14.1 to use this filter variant.

Examples

Simple exact match filter for the resource owner:

```
filter = {"owner": "user-abcd123"}
```

Explicit version of the exact match filter:

```
filter = {"owner": {"$eq": "user-abcd123"}}
```

Containment (membership of a single element):

```
filter = {"participants": {"$contains": "user-abcd123"}}
```

Containment (subset containment; all values must be present, but order doesn't matter):

```
filter = {"participants": {"$contains": ["user-abcd123", "user-efgh456"]}}
```

Combining filters (treated as a logical `AND`):

```
filter = {"owner": "user-abcd123", "participants": {"$contains": "user-efgh456"}}
```

### ``ThreadStatus`module-attribute`¶

```
ThreadStatus = Literal['idle', 'busy', 'interrupted', 'error']
```

Status of a thread.

Values

- idle: Thread is available for work
- busy: Thread is currently processing
- interrupted: Thread was interrupted
- error: Thread encountered an error

### ``MetadataInput`module-attribute`¶

```
MetadataInput = dict[str, Any]
```

Type for arbitrary metadata attached to entities.

Allows storing custom key-value pairs with any entity.
Keys must be strings, values can be any JSON-serializable type.

Examples

```
metadata = {
    "created_by": "user123",
    "priority": 1,
    "tags": ["important", "urgent"]
}
```

### ``HandlerResult`module-attribute`¶

```
HandlerResult = None | bool | FilterType
```

The result of a handler can be:
\\* None \| True: accept the request.
\\* False: reject the request with a 403 error
\\* FilterType: filter to apply

### ``Authenticator`module-attribute`¶

```
Authenticator = Callable[\
    ..., Awaitable[MinimalUser | str | BaseUser | MinimalUserDict | Mapping[str, Any],]\
]
```

Type for authentication functions.

An authenticator can return either:
1\. A string (user\_id)
2\. A dict containing {"identity": str, "permissions": list\[str\]}
3\. An object with identity and permissions properties

Permissions can be used downstream by your authorization logic to determine
access permissions to different resources.

The authenticate decorator will automatically inject any of the following parameters
by name if they are included in your function signature:

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | The raw ASGI request object<br>**TYPE:**`Request` |
| `body` | The parsed request body<br>**TYPE:**`dict` |
| `path` | The request path<br>**TYPE:**`str` |
| `method` | The HTTP method (GET, POST, etc.)<br>**TYPE:**`str` |
| `path_params` | URL path parameters<br>**TYPE:**`dict[str, str] | None` |
| `query_params` | URL query parameters<br>**TYPE:**`dict[str, str] | None` |
| `headers` | Request headers<br>**TYPE:**`dict[str, bytes] | None` |
| `authorization` | The Authorization header value (e.g. "Bearer ")<br>**TYPE:**`str | None` |

Examples

Basic authentication with token:

```
from langgraph_sdk import Auth

auth = Auth()

@auth.authenticate
async def authenticate1(authorization: str) -> Auth.types.MinimalUserDict:
    return await get_user(authorization)
```

Authentication with multiple parameters:

```
@auth.authenticate
async def authenticate2(
    method: str,
    path: str,
    headers: dict[str, bytes]
) -> Auth.types.MinimalUserDict:
    # Custom auth logic using method, path and headers
    user = verify_request(method, path, headers)
    return user
```

Accepting the raw ASGI request:

```
MY_SECRET = "my-secret-key"
@auth.authenticate
async def get_current_user(request: Request) -> Auth.types.MinimalUserDict:
    try:
        token = (request.headers.get("authorization") or "").split(" ", 1)[1]
        payload = jwt.decode(token, MY_SECRET, algorithms=["HS256"])
    except (IndexError, InvalidTokenError):
        raise HTTPException(
            status_code=401,
            detail="Invalid token",
            headers={"WWW-Authenticate": "Bearer"},
        )

    async with httpx.AsyncClient() as client:
        response = await client.get(
            f"https://api.myauth-provider.com/auth/v1/user",
            headers={"Authorization": f"Bearer {MY_SECRET}"}
        )
        if response.status_code != 200:
            raise HTTPException(status_code=401, detail="User not found")

        user_data = response.json()
        return {
            "identity": user_data["id"],
            "display_name": user_data.get("name"),
            "permissions": user_data.get("permissions", []),
            "is_authenticated": True,
        }
```

### ``MinimalUser ¶

Bases: `Protocol`

User objects must at least expose the identity property.

#### ``identity`property`¶

```
identity: str
```

The unique identifier for the user.

This could be a username, email, or any other unique identifier used
to distinguish between different users in the system.

### ``MinimalUserDict ¶

Bases: `TypedDict`

The dictionary representation of a user.

#### ``identity`instance-attribute`¶

```
identity: Required[str]
```

The required unique identifier for the user.

#### ``display\_name`instance-attribute`¶

```
display_name: str
```

The typing.Optional display name for the user.

#### ``is\_authenticated`instance-attribute`¶

```
is_authenticated: bool
```

Whether the user is authenticated. Defaults to True.

#### ``permissions`instance-attribute`¶

```
permissions: Sequence[str]
```

A list of permissions associated with the user.

You can use these in your `@auth.on` authorization logic to determine
access permissions to different resources.

### ``BaseUser ¶

Bases: `Protocol`

The base ASGI user protocol

| METHOD | DESCRIPTION |
| --- | --- |
| `__getitem__` | Get a key from your minimal user dict. |
| `__contains__` | Check if a property exists. |
| `__iter__` | Iterate over the keys of the user. |

#### ``is\_authenticated`property`¶

```
is_authenticated: bool
```

Whether the user is authenticated.

#### ``display\_name`property`¶

```
display_name: str
```

The display name of the user.

#### ``identity`property`¶

```
identity: str
```

The unique identifier for the user.

#### ``permissions`property`¶

```
permissions: Sequence[str]
```

The permissions associated with the user.

#### ``\_\_getitem\_\_ ¶

```
__getitem__(key)
```

Get a key from your minimal user dict.

#### ``\_\_contains\_\_ ¶

```
__contains__(key)
```

Check if a property exists.

#### ``\_\_iter\_\_ ¶

```
__iter__()
```

Iterate over the keys of the user.

### ``StudioUser ¶

A user object that's populated from authenticated requests from the LangGraph studio.

Note: Studio auth can be disabled in your `langgraph.json` config.

```
{
  "auth": {
    "disable_studio_auth": true
  }
}
```

You can use `isinstance` checks in your authorization handlers (`@auth.on`) to control access specifically
for developers accessing the instance from the LangGraph Studio UI.

Examples

```
@auth.on
async def allow_developers(ctx: Auth.types.AuthContext, value: Any) -> None:
    if isinstance(ctx.user, Auth.types.StudioUser):
        return None
    ...
    return False
```

### ``BaseAuthContext`dataclass`¶

Base class for authentication context.

Provides the fundamental authentication information needed for
authorization decisions.

#### ``permissions`instance-attribute`¶

```
permissions: Sequence[str]
```

The permissions granted to the authenticated user.

#### ``user`instance-attribute`¶

```
user: BaseUser
```

The authenticated user.

### ``AuthContext`dataclass`¶

Bases: `BaseAuthContext`

Complete authentication context with resource and action information.

Extends BaseAuthContext with specific resource and action being accessed,
allowing for fine-grained access control decisions.

#### ``resource`instance-attribute`¶

```
resource: Literal['runs', 'threads', 'crons', 'assistants', 'store']
```

The resource being accessed.

#### ``action`instance-attribute`¶

```
action: Literal[\
    "create",\
    "read",\
    "update",\
    "delete",\
    "search",\
    "create_run",\
    "put",\
    "get",\
    "list_namespaces",\
]
```

The action being performed on the resource.

Most resources support the following actions:
\- create: Create a new resource
\- read: Read information about a resource
\- update: Update an existing resource
\- delete: Delete a resource
\- search: Search for resources

The store supports the following actions:
\- put: Add or update a document in the store
\- get: Get a document from the store
\- list\_namespaces: List the namespaces in the store

#### ``permissions`instance-attribute`¶

```
permissions: Sequence[str]
```

The permissions granted to the authenticated user.

#### ``user`instance-attribute`¶

```
user: BaseUser
```

The authenticated user.

### ``ThreadTTL ¶

Bases: `TypedDict`

Time-to-live configuration for a thread.

Matches the OpenAPI schema where TTL is represented as an object with
an optional strategy and a time value in minutes.

#### ``strategy`instance-attribute`¶

```
strategy: Literal['delete']
```

TTL strategy. Currently only 'delete' is supported.

#### ``ttl`instance-attribute`¶

```
ttl: int
```

Time-to-live in minutes from now until the thread should be swept.

### ``ThreadsCreate ¶

Bases: `TypedDict`

Parameters for creating a new thread.

Examples

```
create_params = {
    "thread_id": UUID("123e4567-e89b-12d3-a456-426614174000"),
    "metadata": {"owner": "user123"},
    "if_exists": "do_nothing"
}
```

#### ``thread\_id`instance-attribute`¶

```
thread_id: UUID
```

Unique identifier for the thread.

#### ``metadata`instance-attribute`¶

```
metadata: MetadataInput
```

typing.Optional metadata to attach to the thread.

#### ``if\_exists`instance-attribute`¶

```
if_exists: OnConflictBehavior
```

Behavior when a thread with the same ID already exists.

#### ``ttl`instance-attribute`¶

```
ttl: ThreadTTL
```

Optional TTL configuration for the thread.

### ``ThreadsRead ¶

Bases: `TypedDict`

Parameters for reading thread state or run information.

This type is used in three contexts:
1\. Reading thread, thread version, or thread state information: Only thread\_id is provided
2\. Reading run information: Both thread\_id and run\_id are provided

#### ``thread\_id`instance-attribute`¶

```
thread_id: UUID
```

Unique identifier for the thread.

#### ``run\_id`instance-attribute`¶

```
run_id: UUID | None
```

Run ID to filter by. Only used when reading run information within a thread.

### ``ThreadsUpdate ¶

Bases: `TypedDict`

Parameters for updating a thread or run.

Called for updates to a thread, thread version, or run
cancellation.

#### ``thread\_id`instance-attribute`¶

```
thread_id: UUID
```

Unique identifier for the thread.

#### ``metadata`instance-attribute`¶

```
metadata: MetadataInput
```

typing.Optional metadata to update.

#### ``action`instance-attribute`¶

```
action: Literal['interrupt', 'rollback'] | None
```

typing.Optional action to perform on the thread.

### ``ThreadsDelete ¶

Bases: `TypedDict`

Parameters for deleting a thread.

Called for deletes to a thread, thread version, or run

#### ``thread\_id`instance-attribute`¶

```
thread_id: UUID
```

Unique identifier for the thread.

#### ``run\_id`instance-attribute`¶

```
run_id: UUID | None
```

typing.Optional run ID to filter by.

### ``ThreadsSearch ¶

Bases: `TypedDict`

Parameters for searching threads.

Called for searches to threads or runs.

#### ``metadata`instance-attribute`¶

```
metadata: MetadataInput
```

typing.Optional metadata to filter by.

#### ``values`instance-attribute`¶

```
values: MetadataInput
```

typing.Optional values to filter by.

#### ``status`instance-attribute`¶

```
status: ThreadStatus | None
```

typing.Optional status to filter by.

#### ``limit`instance-attribute`¶

```
limit: int
```

Maximum number of results to return.

#### ``offset`instance-attribute`¶

```
offset: int
```

Offset for pagination.

#### ``ids`instance-attribute`¶

```
ids: Sequence[UUID] | None
```

typing.Optional list of thread IDs to filter by.

#### ``thread\_id`instance-attribute`¶

```
thread_id: UUID | None
```

typing.Optional thread ID to filter by.

### ``RunsCreate ¶

Bases: `TypedDict`

Payload for creating a run.

Examples

```
create_params = {
    "assistant_id": UUID("123e4567-e89b-12d3-a456-426614174000"),
    "thread_id": UUID("123e4567-e89b-12d3-a456-426614174001"),
    "run_id": UUID("123e4567-e89b-12d3-a456-426614174002"),
    "status": "pending",
    "metadata": {"owner": "user123"},
    "prevent_insert_if_inflight": True,
    "multitask_strategy": "reject",
    "if_not_exists": "create",
    "after_seconds": 10,
    "kwargs": {"key": "value"},
    "action": "interrupt"
}
```

#### ``assistant\_id`instance-attribute`¶

```
assistant_id: UUID | None
```

typing.Optional assistant ID to use for this run.

#### ``thread\_id`instance-attribute`¶

```
thread_id: UUID | None
```

typing.Optional thread ID to use for this run.

#### ``run\_id`instance-attribute`¶

```
run_id: UUID | None
```

typing.Optional run ID to use for this run.

#### ``status`instance-attribute`¶

```
status: RunStatus | None
```

typing.Optional status for this run.

#### ``metadata`instance-attribute`¶

```
metadata: MetadataInput
```

typing.Optional metadata for the run.

#### ``prevent\_insert\_if\_inflight`instance-attribute`¶

```
prevent_insert_if_inflight: bool
```

Prevent inserting a new run if one is already in flight.

#### ``multitask\_strategy`instance-attribute`¶

```
multitask_strategy: MultitaskStrategy
```

Multitask strategy for this run.

#### ``if\_not\_exists`instance-attribute`¶

```
if_not_exists: IfNotExists
```

IfNotExists for this run.

#### ``after\_seconds`instance-attribute`¶

```
after_seconds: int
```

Number of seconds to wait before creating the run.

#### ``kwargs`instance-attribute`¶

```
kwargs: dict[str, Any]
```

Keyword arguments to pass to the run.

#### ``action`instance-attribute`¶

```
action: Literal['interrupt', 'rollback'] | None
```

Action to take if updating an existing run.

### ``AssistantsCreate ¶

Bases: `TypedDict`

Payload for creating an assistant.

Examples

```
create_params = {
    "assistant_id": UUID("123e4567-e89b-12d3-a456-426614174000"),
    "graph_id": "graph123",
    "config": {"tags": ["tag1", "tag2"]},
    "context": {"key": "value"},
    "metadata": {"owner": "user123"},
    "if_exists": "do_nothing",
    "name": "Assistant 1"
}
```

#### ``assistant\_id`instance-attribute`¶

```
assistant_id: UUID
```

Unique identifier for the assistant.

#### ``graph\_id`instance-attribute`¶

```
graph_id: str
```

Graph ID to use for this assistant.

#### ``config`instance-attribute`¶

```
config: dict[str, Any]
```

typing.Optional configuration for the assistant.

#### ``metadata`instance-attribute`¶

```
metadata: MetadataInput
```

typing.Optional metadata to attach to the assistant.

#### ``if\_exists`instance-attribute`¶

```
if_exists: OnConflictBehavior
```

Behavior when an assistant with the same ID already exists.

#### ``name`instance-attribute`¶

```
name: str
```

Name of the assistant.

### ``AssistantsRead ¶

Bases: `TypedDict`

Payload for reading an assistant.

Examples

```
read_params = {
    "assistant_id": UUID("123e4567-e89b-12d3-a456-426614174000"),
    "metadata": {"owner": "user123"}
}
```

#### ``assistant\_id`instance-attribute`¶

```
assistant_id: UUID
```

Unique identifier for the assistant.

#### ``metadata`instance-attribute`¶

```
metadata: MetadataInput
```

typing.Optional metadata to filter by.

### ``AssistantsUpdate ¶

Bases: `TypedDict`

Payload for updating an assistant.

Examples

```
update_params = {
    "assistant_id": UUID("123e4567-e89b-12d3-a456-426614174000"),
    "graph_id": "graph123",
    "config": {"tags": ["tag1", "tag2"]},
    "context": {"key": "value"},
    "metadata": {"owner": "user123"},
    "name": "Assistant 1",
    "version": 1
}
```

#### ``assistant\_id`instance-attribute`¶

```
assistant_id: UUID
```

Unique identifier for the assistant.

#### ``graph\_id`instance-attribute`¶

```
graph_id: str | None
```

typing.Optional graph ID to update.

#### ``config`instance-attribute`¶

```
config: dict[str, Any]
```

typing.Optional configuration to update.

#### ``context`instance-attribute`¶

```
context: dict[str, Any]
```

The static context of the assistant.

#### ``metadata`instance-attribute`¶

```
metadata: MetadataInput
```

typing.Optional metadata to update.

#### ``name`instance-attribute`¶

```
name: str | None
```

typing.Optional name to update.

#### ``version`instance-attribute`¶

```
version: int | None
```

typing.Optional version to update.

### ``AssistantsDelete ¶

Bases: `TypedDict`

Payload for deleting an assistant.

Examples

```
delete_params = {
    "assistant_id": UUID("123e4567-e89b-12d3-a456-426614174000")
}
```

#### ``assistant\_id`instance-attribute`¶

```
assistant_id: UUID
```

Unique identifier for the assistant.

### ``AssistantsSearch ¶

Bases: `TypedDict`

Payload for searching assistants.

Examples

```
search_params = {
    "graph_id": "graph123",
    "metadata": {"owner": "user123"},
    "limit": 10,
    "offset": 0
}
```

#### ``graph\_id`instance-attribute`¶

```
graph_id: str | None
```

typing.Optional graph ID to filter by.

#### ``metadata`instance-attribute`¶

```
metadata: MetadataInput
```

typing.Optional metadata to filter by.

#### ``limit`instance-attribute`¶

```
limit: int
```

Maximum number of results to return.

#### ``offset`instance-attribute`¶

```
offset: int
```

Offset for pagination.

### ``CronsCreate ¶

Bases: `TypedDict`

Payload for creating a cron job.

Examples

```
create_params = {
    "payload": {"key": "value"},
    "schedule": "0 0 * * *",
    "cron_id": UUID("123e4567-e89b-12d3-a456-426614174000"),
    "thread_id": UUID("123e4567-e89b-12d3-a456-426614174001"),
    "user_id": "user123",
    "end_time": datetime(2024, 3, 16, 10, 0, 0)
}
```

#### ``payload`instance-attribute`¶

```
payload: dict[str, Any]
```

Payload for the cron job.

#### ``schedule`instance-attribute`¶

```
schedule: str
```

Schedule for the cron job.

#### ``cron\_id`instance-attribute`¶

```
cron_id: UUID | None
```

typing.Optional unique identifier for the cron job.

#### ``thread\_id`instance-attribute`¶

```
thread_id: UUID | None
```

typing.Optional thread ID to use for this cron job.

#### ``user\_id`instance-attribute`¶

```
user_id: str | None
```

typing.Optional user ID to use for this cron job.

#### ``end\_time`instance-attribute`¶

```
end_time: datetime | None
```

typing.Optional end time for the cron job.

### ``CronsDelete ¶

Bases: `TypedDict`

Payload for deleting a cron job.

Examples

```
delete_params = {
    "cron_id": UUID("123e4567-e89b-12d3-a456-426614174000")
}
```

#### ``cron\_id`instance-attribute`¶

```
cron_id: UUID
```

Unique identifier for the cron job.

### ``CronsRead ¶

Bases: `TypedDict`

Payload for reading a cron job.

Examples

```
read_params = {
    "cron_id": UUID("123e4567-e89b-12d3-a456-426614174000")
}
```

#### ``cron\_id`instance-attribute`¶

```
cron_id: UUID
```

Unique identifier for the cron job.

### ``CronsUpdate ¶

Bases: `TypedDict`

Payload for updating a cron job.

Examples

```
update_params = {
    "cron_id": UUID("123e4567-e89b-12d3-a456-426614174000"),
    "payload": {"key": "value"},
    "schedule": "0 0 * * *"
}
```

#### ``cron\_id`instance-attribute`¶

```
cron_id: UUID
```

Unique identifier for the cron job.

#### ``payload`instance-attribute`¶

```
payload: dict[str, Any] | None
```

typing.Optional payload to update.

#### ``schedule`instance-attribute`¶

```
schedule: str | None
```

typing.Optional schedule to update.

### ``CronsSearch ¶

Bases: `TypedDict`

Payload for searching cron jobs.

Examples

```
search_params = {
    "assistant_id": UUID("123e4567-e89b-12d3-a456-426614174000"),
    "thread_id": UUID("123e4567-e89b-12d3-a456-426614174001"),
    "limit": 10,
    "offset": 0
}
```

#### ``assistant\_id`instance-attribute`¶

```
assistant_id: UUID | None
```

typing.Optional assistant ID to filter by.

#### ``thread\_id`instance-attribute`¶

```
thread_id: UUID | None
```

typing.Optional thread ID to filter by.

#### ``limit`instance-attribute`¶

```
limit: int
```

Maximum number of results to return.

#### ``offset`instance-attribute`¶

```
offset: int
```

Offset for pagination.

### ``StoreGet ¶

Bases: `TypedDict`

Operation to retrieve a specific item by its namespace and key.

#### ``namespace`instance-attribute`¶

```
namespace: tuple[str, ...]
```

Hierarchical path that uniquely identifies the item's location.

#### ``key`instance-attribute`¶

```
key: str
```

Unique identifier for the item within its specific namespace.

### ``StoreSearch ¶

Bases: `TypedDict`

Operation to search for items within a specified namespace hierarchy.

#### ``namespace`instance-attribute`¶

```
namespace: tuple[str, ...]
```

Prefix filter for defining the search scope.

#### ``filter`instance-attribute`¶

```
filter: dict[str, Any] | None
```

Key-value pairs for filtering results based on exact matches or comparison operators.

#### ``limit`instance-attribute`¶

```
limit: int
```

Maximum number of items to return in the search results.

#### ``offset`instance-attribute`¶

```
offset: int
```

Number of matching items to skip for pagination.

#### ``query`instance-attribute`¶

```
query: str | None
```

Naturalj language search query for semantic search capabilities.

### ``StoreListNamespaces ¶

Bases: `TypedDict`

Operation to list and filter namespaces in the store.

#### ``namespace`instance-attribute`¶

```
namespace: tuple[str, ...] | None
```

Prefix filter namespaces.

#### ``suffix`instance-attribute`¶

```
suffix: tuple[str, ...] | None
```

Optional conditions for filtering namespaces.

#### ``max\_depth`instance-attribute`¶

```
max_depth: int | None
```

Maximum depth of namespace hierarchy to return.

Note

Namespaces deeper than this level will be truncated.

#### ``limit`instance-attribute`¶

```
limit: int
```

Maximum number of namespaces to return.

#### ``offset`instance-attribute`¶

```
offset: int
```

Number of namespaces to skip for pagination.

### ``StorePut ¶

Bases: `TypedDict`

Operation to store, update, or delete an item in the store.

#### ``namespace`instance-attribute`¶

```
namespace: tuple[str, ...]
```

Hierarchical path that identifies the location of the item.

#### ``key`instance-attribute`¶

```
key: str
```

Unique identifier for the item within its namespace.

#### ``value`instance-attribute`¶

```
value: dict[str, Any] | None
```

The data to store, or `None` to mark the item for deletion.

#### ``index`instance-attribute`¶

```
index: Literal[False] | list[str] | None
```

Optional index configuration for full-text search.

### ``StoreDelete ¶

Bases: `TypedDict`

Operation to delete an item from the store.

#### ``namespace`instance-attribute`¶

```
namespace: tuple[str, ...]
```

Hierarchical path that uniquely identifies the item's location.

#### ``key`instance-attribute`¶

```
key: str
```

Unique identifier for the item within its specific namespace.

### ``on ¶

Namespace for type definitions of different API operations.

This class organizes type definitions for create, read, update, delete,
and search operations across different resources (threads, assistants, crons).

Usage

```
from langgraph_sdk import Auth

auth = Auth()

@auth.on
def handle_all(params: Auth.on.value):
    raise Exception("Not authorized")

@auth.on.threads.create
def handle_thread_create(params: Auth.on.threads.create.value):
    # Handle thread creation
    pass

@auth.on.assistants.search
def handle_assistant_search(params: Auth.on.assistants.search.value):
    # Handle assistant search
    pass
```

#### ``threads ¶

Types for thread-related operations.

##### ``create ¶

Type for thread creation parameters.

##### ``create\_run ¶

Type for creating or streaming a run.

##### ``read ¶

Type for thread read parameters.

##### ``update ¶

Type for thread update parameters.

##### ``delete ¶

Type for thread deletion parameters.

##### ``search ¶

Type for thread search parameters.

#### ``assistants ¶

Types for assistant-related operations.

##### ``create ¶

Type for assistant creation parameters.

##### ``read ¶

Type for assistant read parameters.

##### ``update ¶

Type for assistant update parameters.

##### ``delete ¶

Type for assistant deletion parameters.

##### ``search ¶

Type for assistant search parameters.

#### ``crons ¶

Types for cron-related operations.

##### ``create ¶

Type for cron creation parameters.

##### ``read ¶

Type for cron read parameters.

##### ``update ¶

Type for cron update parameters.

##### ``delete ¶

Type for cron deletion parameters.

##### ``search ¶

Type for cron search parameters.

#### ``store ¶

Types for store-related operations.

##### ``put ¶

Type for store put parameters.

##### ``get ¶

Type for store get parameters.

##### ``search ¶

Type for store search parameters.

##### ``delete ¶

Type for store delete parameters.

##### ``list\_namespaces ¶

Type for store list namespaces parameters.

## ``langgraph\_sdk.auth.exceptions ¶

Exceptions used in the auth system.

### ``HTTPException ¶

Bases: `Exception`

HTTP exception that you can raise to return a specific HTTP error response.

Since this is defined in the auth module, we default to a 401 status code.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `status_code` | HTTP status code for the error. Defaults to 401 "Unauthorized".<br>**TYPE:**`int`**DEFAULT:**`401` |
| `detail` | Detailed error message. If `None`, uses a default<br>message based on the status code.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `headers` | Additional HTTP headers to include in the error response.<br>**TYPE:**`Mapping[str, str] | None`**DEFAULT:**`None` |

Example

Default:

```
raise HTTPException()
# HTTPException(status_code=401, detail='Unauthorized')
```

Add headers:

```
raise HTTPException(headers={"X-Custom-Header": "Custom Value"})
# HTTPException(status_code=401, detail='Unauthorized', headers={"WWW-Authenticate": "Bearer"})
```

Custom error:

```
raise HTTPException(status_code=404, detail="Not found")
```

Back to top