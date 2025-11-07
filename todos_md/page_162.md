<!-- URL: page_162 -->

Skip to content

Edit this page

# `langchain-mcp-adapters` ¶

![PyPI - Version](https://pypi.org/project/langchain-mcp-adapters/#history)![PyPI - License](https://opensource.org/licenses/MIT)![PyPI - Downloads](https://pypistats.org/packages/langchain-mcp-adapters)

Reference documentation for the `langchain-mcp-adapters` package.

## ``langchain\_mcp\_adapters.client ¶

Client for connecting to multiple MCP servers and loading LangChain tools/resources.

This module provides the `MultiServerMCPClient` class for managing connections to multiple
MCP servers and loading tools, prompts, and resources from them.

### ``MultiServerMCPClient ¶

Client for connecting to multiple MCP servers.

Loads LangChain-compatible tools, prompts and resources from MCP servers.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize a `MultiServerMCPClient` with MCP servers connections. |
| `session` | Connect to an MCP server and initialize a session. |
| `get_tools` | Get a list of all tools from all connected servers. |
| `get_prompt` | Get a prompt from a given MCP server. |
| `get_resources` | Get resources from a given MCP server. |
| `__aenter__` | Async context manager entry point. |
| `__aexit__` | Async context manager exit point. |

#### ``\_\_init\_\_ ¶

```
__init__(
    connections: dict[str, Connection] | None = None,
    *,
    callbacks: Callbacks | None = None,
    tool_interceptors: list[ToolCallInterceptor] | None = None,
) -> None
```

Initialize a `MultiServerMCPClient` with MCP servers connections.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `connections` | A `dict` mapping server names to connection configurations. If<br>`None`, no initial connections are established.<br>**TYPE:**`dict[str, Connection] | None`**DEFAULT:**`None` |
| `callbacks` | Optional callbacks for handling notifications and events.<br>**TYPE:**`Callbacks | None`**DEFAULT:**`None` |
| `tool_interceptors` | Optional list of tool call interceptors for modifying<br>requests and responses.<br>**TYPE:**`list[ToolCallInterceptor] | None`**DEFAULT:**`None` |

Basic usage (starting a new session on each tool call)

```
from langchain_mcp_adapters.client import MultiServerMCPClient

client = MultiServerMCPClient(
    {
        "math": {
            "command": "python",
            # Make sure to update to the full absolute path to your
            # math_server.py file
            "args": ["/path/to/math_server.py"],
            "transport": "stdio",
        },
        "weather": {
            # Make sure you start your weather server on port 8000
            "url": "http://localhost:8000/mcp",
            "transport": "streamable_http",
        }
    }
)
all_tools = await client.get_tools()
```

Explicitly starting a session

```
from langchain_mcp_adapters.client import MultiServerMCPClient
from langchain_mcp_adapters.tools import load_mcp_tools

client = MultiServerMCPClient({...})
async with client.session("math") as session:
    tools = await load_mcp_tools(session)
```

#### ``session`async`¶

```
session(
    server_name: str, *, auto_initialize: bool = True
) -> AsyncIterator[ClientSession]
```

Connect to an MCP server and initialize a session.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `server_name` | Name to identify this server connection<br>**TYPE:**`str` |
| `auto_initialize` | Whether to automatically initialize the session<br>**TYPE:**`bool`**DEFAULT:**`True` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If the server name is not found in the connections |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[ClientSession]` | An initialized `ClientSession` |

#### ``get\_tools`async`¶

```
get_tools(*, server_name: str | None = None) -> list[BaseTool]
```

Get a list of all tools from all connected servers.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `server_name` | Optional name of the server to get tools from.<br>If `None`, all tools from all servers will be returned.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

Note

A new session will be created for each tool call

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[BaseTool]` | A list of LangChain tools |

#### ``get\_prompt`async`¶

```
get_prompt(
    server_name: str, prompt_name: str, *, arguments: dict[str, Any] | None = None
) -> list[HumanMessage | AIMessage]
```

Get a prompt from a given MCP server.

#### ``get\_resources`async`¶

```
get_resources(server_name: str, *, uris: str | list[str] | None = None) -> list[Blob]
```

Get resources from a given MCP server.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `server_name` | Name of the server to get resources from<br>**TYPE:**`str` |
| `uris` | Optional resource URI or list of URIs to load. If not provided,<br>all resources will be loaded.<br>**TYPE:**`str | list[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Blob]` | A list of LangChain Blob objects. |

#### ``\_\_aenter\_\_`async`¶

```
__aenter__() -> MultiServerMCPClient
```

Async context manager entry point.

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Context manager support has been removed. |

#### ``\_\_aexit\_\_ ¶

```
__aexit__(
    exc_type: type[BaseException] | None,
    exc_val: BaseException | None,
    exc_tb: TracebackType | None,
) -> None
```

Async context manager exit point.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `exc_type` | Exception type if an exception occurred.<br>**TYPE:**`type[BaseException] | None` |
| `exc_val` | Exception value if an exception occurred.<br>**TYPE:**`BaseException | None` |
| `exc_tb` | Exception traceback if an exception occurred.<br>**TYPE:**`TracebackType | None` |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Context manager support has been removed. |

## ``langchain\_mcp\_adapters.tools ¶

Tools adapter for converting MCP tools to LangChain tools.

This module provides functionality to convert MCP tools into LangChain-compatible
tools, handle tool execution, and manage tool conversion between the two formats.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `load_mcp_tools` | Load all available MCP tools and convert them to LangChain tools. |

### ``load\_mcp\_tools`async`¶

```
load_mcp_tools(
    session: ClientSession | None,
    *,
    connection: Connection | None = None,
    callbacks: Callbacks | None = None,
    tool_interceptors: list[ToolCallInterceptor] | None = None,
    server_name: str | None = None,
) -> list[BaseTool]
```

Load all available MCP tools and convert them to LangChain tools.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `session` | The MCP client session. If `None`, connection must be provided.<br>**TYPE:**`ClientSession | None` |
| `connection` | Connection config to create a new session if session is `None`.<br>**TYPE:**`Connection | None`**DEFAULT:**`None` |
| `callbacks` | Optional `Callbacks` for handling notifications and events.<br>**TYPE:**`Callbacks | None`**DEFAULT:**`None` |
| `tool_interceptors` | Optional list of interceptors for tool call processing.<br>**TYPE:**`list[ToolCallInterceptor] | None`**DEFAULT:**`None` |
| `server_name` | Name of the server these tools belong to.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[BaseTool]` | List of LangChain tools.<br>Tool annotations are returned as part of the tool metadata object. |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If neither session nor connection is provided. |

## ``langchain\_mcp\_adapters.prompts ¶

Prompts adapter for converting MCP prompts to LangChain messages.

This module provides functionality to convert MCP prompt messages into LangChain
message objects, handling both user and assistant message types.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `load_mcp_prompt` | Load MCP prompt and convert to LangChain messages. |

### ``load\_mcp\_prompt`async`¶

```
load_mcp_prompt(
    session: ClientSession, name: str, *, arguments: dict[str, Any] | None = None
) -> list[HumanMessage | AIMessage]
```

Load MCP prompt and convert to LangChain messages.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `session` | The MCP client session.<br>**TYPE:**`ClientSession` |
| `name` | Name of the prompt to load.<br>**TYPE:**`str` |
| `arguments` | Optional arguments to pass to the prompt.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[HumanMessage | AIMessage]` | A list of LangChain messages<br>converted from the MCP prompt. |

## ``langchain\_mcp\_adapters.resources ¶

Resources adapter for converting MCP resources to LangChain Blob objects.

This module provides functionality to convert MCP resources into LangChain Blob
objects, handling both text and binary resource content types.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `load_mcp_resources` | Load MCP resources and convert them to LangChain Blob objects. |

### ``load\_mcp\_resources`async`¶

```
load_mcp_resources(
    session: ClientSession, *, uris: str | list[str] | None = None
) -> list[Blob]
```

Load MCP resources and convert them to LangChain Blob objects.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `session` | MCP client session.<br>**TYPE:**`ClientSession` |
| `uris` | List of URIs to load. If `None`, all resources will be loaded.<br>Note<br>Dynamic resources will NOT be loaded when `None` is specified,<br>as they require parameters and are ignored by the MCP SDK's<br>`session.list_resources()` method.<br>**TYPE:**`str | list[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Blob]` | A list of LangChain Blob objects. |

| RAISES | DESCRIPTION |
| --- | --- |
| `RuntimeError` | If an error occurs while fetching a resource. |

Back to top