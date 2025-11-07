<!-- URL: page_174 -->

Skip to content

Edit this page

# Checkpointing

## ``langgraph.checkpoint.base ¶

| FUNCTION | DESCRIPTION |
| --- | --- |
| `create_checkpoint` | Create a checkpoint for the given channels. |

### ``CheckpointMetadata ¶

Bases: `TypedDict`

Metadata associated with a checkpoint.

#### ``source`instance-attribute`¶

```
source: Literal['input', 'loop', 'update', 'fork']
```

The source of the checkpoint.

- `"input"`: The checkpoint was created from an input to invoke/stream/batch.
- `"loop"`: The checkpoint was created from inside the pregel loop.
- `"update"`: The checkpoint was created from a manual state update.
- `"fork"`: The checkpoint was created as a copy of another checkpoint.

#### ``step`instance-attribute`¶

```
step: int
```

The step number of the checkpoint.

`-1` for the first `"input"` checkpoint.
`0` for the first `"loop"` checkpoint.
`...` for the `nth` checkpoint afterwards.

#### ``parents`instance-attribute`¶

```
parents: dict[str, str]
```

The IDs of the parent checkpoints.

Mapping from checkpoint namespace to checkpoint ID.

### ``Checkpoint ¶

Bases: `TypedDict`

State snapshot at a given point in time.

#### ``v`instance-attribute`¶

```
v: int
```

The version of the checkpoint format. Currently 1.

#### ``id`instance-attribute`¶

```
id: str
```

The ID of the checkpoint. This is both unique and monotonically
increasing, so can be used for sorting checkpoints from first to last.

#### ``ts`instance-attribute`¶

```
ts: str
```

The timestamp of the checkpoint in ISO 8601 format.

#### ``channel\_values`instance-attribute`¶

```
channel_values: dict[str, Any]
```

The values of the channels at the time of the checkpoint.
Mapping from channel name to deserialized channel snapshot value.

#### ``channel\_versions`instance-attribute`¶

```
channel_versions: ChannelVersions
```

The versions of the channels at the time of the checkpoint.
The keys are channel names and the values are monotonically increasing
version strings for each channel.

#### ``versions\_seen`instance-attribute`¶

```
versions_seen: dict[str, ChannelVersions]
```

Map from node ID to map from channel name to version seen.
This keeps track of the versions of the channels that each node has seen.
Used to determine which nodes to execute next.

#### ``updated\_channels`instance-attribute`¶

```
updated_channels: list[str] | None
```

The channels that were updated in this checkpoint.

### ``BaseCheckpointSaver ¶

Bases: `Generic[V]`

Base class for creating a graph checkpointer.

Checkpointers allow LangGraph agents to persist their state
within and across multiple interactions.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `serde` | Serializer for encoding/decoding checkpoints.<br>**TYPE:**`SerializerProtocol` |

Note

When creating a custom checkpoint saver, consider implementing async
versions to avoid blocking the main thread.

| METHOD | DESCRIPTION |
| --- | --- |
| `get` | Fetch a checkpoint using the given configuration. |
| `get_tuple` | Fetch a checkpoint tuple using the given configuration. |
| `list` | List checkpoints that match the given criteria. |
| `put` | Store a checkpoint with its configuration and metadata. |
| `put_writes` | Store intermediate writes linked to a checkpoint. |
| `delete_thread` | Delete all checkpoints and writes associated with a specific thread ID. |
| `aget` | Asynchronously fetch a checkpoint using the given configuration. |
| `aget_tuple` | Asynchronously fetch a checkpoint tuple using the given configuration. |
| `alist` | Asynchronously list checkpoints that match the given criteria. |
| `aput` | Asynchronously store a checkpoint with its configuration and metadata. |
| `aput_writes` | Asynchronously store intermediate writes linked to a checkpoint. |
| `adelete_thread` | Delete all checkpoints and writes associated with a specific thread ID. |
| `get_next_version` | Generate the next version ID for a channel. |

#### ``config\_specs`property`¶

```
config_specs: list
```

Define the configuration options for the checkpoint saver.

| RETURNS | DESCRIPTION |
| --- | --- |
| `list` | List of configuration field specs.<br>**TYPE:**`list` |

#### ``get ¶

```
get(config: RunnableConfig) -> Checkpoint | None
```

Fetch a checkpoint using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Checkpoint | None` | The requested checkpoint, or `None` if not found. |

#### ``get\_tuple ¶

```
get_tuple(config: RunnableConfig) -> CheckpointTuple | None
```

Fetch a checkpoint tuple using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple | None` | The requested checkpoint tuple, or `None` if not found. |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Implement this method in your custom checkpoint saver. |

#### ``list ¶

```
list(
    config: RunnableConfig | None,
    *,
    filter: dict[str, Any] | None = None,
    before: RunnableConfig | None = None,
    limit: int | None = None,
) -> Iterator[CheckpointTuple]
```

List checkpoints that match the given criteria.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Base configuration for filtering checkpoints.<br>**TYPE:**`RunnableConfig | None` |
| `filter` | Additional filtering criteria.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `before` | List checkpoints created before this configuration.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `limit` | Maximum number of checkpoints to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Iterator[CheckpointTuple]` | Iterator of matching checkpoint tuples. |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Implement this method in your custom checkpoint saver. |

#### ``put ¶

```
put(
    config: RunnableConfig,
    checkpoint: Checkpoint,
    metadata: CheckpointMetadata,
    new_versions: ChannelVersions,
) -> RunnableConfig
```

Store a checkpoint with its configuration and metadata.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration for the checkpoint.<br>**TYPE:**`RunnableConfig` |
| `checkpoint` | The checkpoint to store.<br>**TYPE:**`Checkpoint` |
| `metadata` | Additional metadata for the checkpoint.<br>**TYPE:**`CheckpointMetadata` |
| `new_versions` | New channel versions as of this write.<br>**TYPE:**`ChannelVersions` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableConfig` | Updated configuration after storing the checkpoint.<br>**TYPE:**`RunnableConfig` |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Implement this method in your custom checkpoint saver. |

#### ``put\_writes ¶

```
put_writes(
    config: RunnableConfig,
    writes: Sequence[tuple[str, Any]],
    task_id: str,
    task_path: str = "",
) -> None
```

Store intermediate writes linked to a checkpoint.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration of the related checkpoint.<br>**TYPE:**`RunnableConfig` |
| `writes` | List of writes to store.<br>**TYPE:**`Sequence[tuple[str, Any]]` |
| `task_id` | Identifier for the task creating the writes.<br>**TYPE:**`str` |
| `task_path` | Path of the task creating the writes.<br>**TYPE:**`str`**DEFAULT:**`''` |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Implement this method in your custom checkpoint saver. |

#### ``delete\_thread ¶

```
delete_thread(thread_id: str) -> None
```

Delete all checkpoints and writes associated with a specific thread ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID whose checkpoints should be deleted.<br>**TYPE:**`str` |

#### ``aget`async`¶

```
aget(config: RunnableConfig) -> Checkpoint | None
```

Asynchronously fetch a checkpoint using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Checkpoint | None` | The requested checkpoint, or `None` if not found. |

#### ``aget\_tuple`async`¶

```
aget_tuple(config: RunnableConfig) -> CheckpointTuple | None
```

Asynchronously fetch a checkpoint tuple using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple | None` | The requested checkpoint tuple, or `None` if not found. |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Implement this method in your custom checkpoint saver. |

#### ``alist`async`¶

```
alist(
    config: RunnableConfig | None,
    *,
    filter: dict[str, Any] | None = None,
    before: RunnableConfig | None = None,
    limit: int | None = None,
) -> AsyncIterator[CheckpointTuple]
```

Asynchronously list checkpoints that match the given criteria.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Base configuration for filtering checkpoints.<br>**TYPE:**`RunnableConfig | None` |
| `filter` | Additional filtering criteria for metadata.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `before` | List checkpoints created before this configuration.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `limit` | Maximum number of checkpoints to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[CheckpointTuple]` | Async iterator of matching checkpoint tuples. |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Implement this method in your custom checkpoint saver. |

#### ``aput`async`¶

```
aput(
    config: RunnableConfig,
    checkpoint: Checkpoint,
    metadata: CheckpointMetadata,
    new_versions: ChannelVersions,
) -> RunnableConfig
```

Asynchronously store a checkpoint with its configuration and metadata.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration for the checkpoint.<br>**TYPE:**`RunnableConfig` |
| `checkpoint` | The checkpoint to store.<br>**TYPE:**`Checkpoint` |
| `metadata` | Additional metadata for the checkpoint.<br>**TYPE:**`CheckpointMetadata` |
| `new_versions` | New channel versions as of this write.<br>**TYPE:**`ChannelVersions` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableConfig` | Updated configuration after storing the checkpoint.<br>**TYPE:**`RunnableConfig` |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Implement this method in your custom checkpoint saver. |

#### ``aput\_writes`async`¶

```
aput_writes(
    config: RunnableConfig,
    writes: Sequence[tuple[str, Any]],
    task_id: str,
    task_path: str = "",
) -> None
```

Asynchronously store intermediate writes linked to a checkpoint.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration of the related checkpoint.<br>**TYPE:**`RunnableConfig` |
| `writes` | List of writes to store.<br>**TYPE:**`Sequence[tuple[str, Any]]` |
| `task_id` | Identifier for the task creating the writes.<br>**TYPE:**`str` |
| `task_path` | Path of the task creating the writes.<br>**TYPE:**`str`**DEFAULT:**`''` |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Implement this method in your custom checkpoint saver. |

#### ``adelete\_thread`async`¶

```
adelete_thread(thread_id: str) -> None
```

Delete all checkpoints and writes associated with a specific thread ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID whose checkpoints should be deleted.<br>**TYPE:**`str` |

#### ``get\_next\_version ¶

```
get_next_version(current: V | None, channel: None) -> V
```

Generate the next version ID for a channel.

Default is to use integer versions, incrementing by `1`. If you override, you can use `str`/`int`/`float`
versions, as long as they are monotonically increasing.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `current` | The current version identifier (`int`, `float`, or `str`).<br>**TYPE:**`V | None` |
| `channel` | Deprecated argument, kept for backwards compatibility.<br>**TYPE:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `V` | The next version identifier, which must be increasing.<br>**TYPE:**`V` |

### ``create\_checkpoint ¶

```
create_checkpoint(
    checkpoint: Checkpoint,
    channels: Mapping[str, ChannelProtocol] | None,
    step: int,
    *,
    id: str | None = None,
) -> Checkpoint
```

Create a checkpoint for the given channels.

## ``langgraph.checkpoint.serde.base ¶

### ``SerializerProtocol ¶

Bases: `Protocol`

Protocol for serialization and deserialization of objects.

- `dumps`: Serialize an object to bytes.
- `dumps_typed`: Serialize an object to a tuple `(type, bytes)`.
- `loads`: Deserialize an object from bytes.
- `loads_typed`: Deserialize an object from a tuple `(type, bytes)`.

Valid implementations include the `pickle`, `json` and `orjson` modules.

### ``CipherProtocol ¶

Bases: `Protocol`

Protocol for encryption and decryption of data.
\- `encrypt`: Encrypt plaintext.
\- `decrypt`: Decrypt ciphertext.

| METHOD | DESCRIPTION |
| --- | --- |
| `encrypt` | Encrypt plaintext. Returns a tuple (cipher name, ciphertext). |
| `decrypt` | Decrypt ciphertext. Returns the plaintext. |

#### ``encrypt ¶

```
encrypt(plaintext: bytes) -> tuple[str, bytes]
```

Encrypt plaintext. Returns a tuple (cipher name, ciphertext).

#### ``decrypt ¶

```
decrypt(ciphername: str, ciphertext: bytes) -> bytes
```

Decrypt ciphertext. Returns the plaintext.

## ``langgraph.checkpoint.serde.jsonplus ¶

### ``JsonPlusSerializer ¶

Bases: `SerializerProtocol`

Serializer that uses ormsgpack, with optional fallbacks.

Security note: this serializer is intended for use within the BaseCheckpointSaver
class and called within the Pregel loop. It should not be used on untrusted
python objects. If an attacker can write directly to your checkpoint database,
they may be able to trigger code execution when data is deserialized.

## ``langgraph.checkpoint.serde.encrypted ¶

### ``EncryptedSerializer ¶

Bases: `SerializerProtocol`

Serializer that encrypts and decrypts data using an encryption protocol.

| METHOD | DESCRIPTION |
| --- | --- |
| `dumps_typed` | Serialize an object to a tuple `(type, bytes)` and encrypt the bytes. |
| `from_pycryptodome_aes` | Create an `EncryptedSerializer` using AES encryption. |

#### ``dumps\_typed ¶

```
dumps_typed(obj: Any) -> tuple[str, bytes]
```

Serialize an object to a tuple `(type, bytes)` and encrypt the bytes.

#### ``from\_pycryptodome\_aes`classmethod`¶

```
from_pycryptodome_aes(
    serde: SerializerProtocol = JsonPlusSerializer(), **kwargs: Any
) -> EncryptedSerializer
```

Create an `EncryptedSerializer` using AES encryption.

## ``langgraph.checkpoint.memory ¶

### ``InMemorySaver ¶

Bases: `BaseCheckpointSaver[str]`, `AbstractContextManager`, `AbstractAsyncContextManager`

An in-memory checkpoint saver.

This checkpoint saver stores checkpoints in memory using a defaultdict.

Note

Only use `InMemorySaver` for debugging or testing purposes.
For production use cases we recommend installing langgraph-checkpoint-postgres and using `PostgresSaver` / `AsyncPostgresSaver`.

If you are using LangSmith Deployment, no checkpointer needs to be specified. The correct managed checkpointer will be used automatically.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `serde` | The serializer to use for serializing and deserializing checkpoints.<br>**TYPE:**`SerializerProtocol | None`**DEFAULT:**`None` |

Examples:

```
    import asyncio

    from langgraph.checkpoint.memory import InMemorySaver
    from langgraph.graph import StateGraph

    builder = StateGraph(int)
    builder.add_node("add_one", lambda x: x + 1)
    builder.set_entry_point("add_one")
    builder.set_finish_point("add_one")

    memory = InMemorySaver()
    graph = builder.compile(checkpointer=memory)
    coro = graph.ainvoke(1, {"configurable": {"thread_id": "thread-1"}})
    asyncio.run(coro)  # Output: 2
```

| METHOD | DESCRIPTION |
| --- | --- |
| `get_tuple` | Get a checkpoint tuple from the in-memory storage. |
| `list` | List checkpoints from the in-memory storage. |
| `put` | Save a checkpoint to the in-memory storage. |
| `put_writes` | Save a list of writes to the in-memory storage. |
| `delete_thread` | Delete all checkpoints and writes associated with a thread ID. |
| `aget_tuple` | Asynchronous version of `get_tuple`. |
| `alist` | Asynchronous version of `list`. |
| `aput` | Asynchronous version of `put`. |
| `aput_writes` | Asynchronous version of `put_writes`. |
| `adelete_thread` | Delete all checkpoints and writes associated with a thread ID. |
| `get_next_version` | Generate the next version ID for a channel. |
| `get` | Fetch a checkpoint using the given configuration. |
| `aget` | Asynchronously fetch a checkpoint using the given configuration. |

#### ``config\_specs`property`¶

```
config_specs: list
```

Define the configuration options for the checkpoint saver.

| RETURNS | DESCRIPTION |
| --- | --- |
| `list` | List of configuration field specs.<br>**TYPE:**`list` |

#### ``get\_tuple ¶

```
get_tuple(config: RunnableConfig) -> CheckpointTuple | None
```

Get a checkpoint tuple from the in-memory storage.

This method retrieves a checkpoint tuple from the in-memory storage based on the
provided config. If the config contains a `checkpoint_id` key, the checkpoint with
the matching thread ID and timestamp is retrieved. Otherwise, the latest checkpoint
for the given thread ID is retrieved.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to use for retrieving the checkpoint.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple | None` | The retrieved checkpoint tuple, or None if no matching checkpoint was found. |

#### ``list ¶

```
list(
    config: RunnableConfig | None,
    *,
    filter: dict[str, Any] | None = None,
    before: RunnableConfig | None = None,
    limit: int | None = None,
) -> Iterator[CheckpointTuple]
```

List checkpoints from the in-memory storage.

This method retrieves a list of checkpoint tuples from the in-memory storage based
on the provided criteria.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Base configuration for filtering checkpoints.<br>**TYPE:**`RunnableConfig | None` |
| `filter` | Additional filtering criteria for metadata.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `before` | List checkpoints created before this configuration.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `limit` | Maximum number of checkpoints to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple` | An iterator of matching checkpoint tuples. |

#### ``put ¶

```
put(
    config: RunnableConfig,
    checkpoint: Checkpoint,
    metadata: CheckpointMetadata,
    new_versions: ChannelVersions,
) -> RunnableConfig
```

Save a checkpoint to the in-memory storage.

This method saves a checkpoint to the in-memory storage. The checkpoint is associated
with the provided config.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to associate with the checkpoint.<br>**TYPE:**`RunnableConfig` |
| `checkpoint` | The checkpoint to save.<br>**TYPE:**`Checkpoint` |
| `metadata` | Additional metadata to save with the checkpoint.<br>**TYPE:**`CheckpointMetadata` |
| `new_versions` | New versions as of this write<br>**TYPE:**`ChannelVersions` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableConfig` | The updated config containing the saved checkpoint's timestamp.<br>**TYPE:**`RunnableConfig` |

#### ``put\_writes ¶

```
put_writes(
    config: RunnableConfig,
    writes: Sequence[tuple[str, Any]],
    task_id: str,
    task_path: str = "",
) -> None
```

Save a list of writes to the in-memory storage.

This method saves a list of writes to the in-memory storage. The writes are associated
with the provided config.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to associate with the writes.<br>**TYPE:**`RunnableConfig` |
| `writes` | The writes to save.<br>**TYPE:**`Sequence[tuple[str, Any]]` |
| `task_id` | Identifier for the task creating the writes.<br>**TYPE:**`str` |
| `task_path` | Path of the task creating the writes.<br>**TYPE:**`str`**DEFAULT:**`''` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableConfig` | The updated config containing the saved writes' timestamp.<br>**TYPE:**`None` |

#### ``delete\_thread ¶

```
delete_thread(thread_id: str) -> None
```

Delete all checkpoints and writes associated with a thread ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to delete.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

#### ``aget\_tuple`async`¶

```
aget_tuple(config: RunnableConfig) -> CheckpointTuple | None
```

Asynchronous version of `get_tuple`.

This method is an asynchronous wrapper around `get_tuple` that runs the synchronous
method in a separate thread using asyncio.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to use for retrieving the checkpoint.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple | None` | The retrieved checkpoint tuple, or None if no matching checkpoint was found. |

#### ``alist`async`¶

```
alist(
    config: RunnableConfig | None,
    *,
    filter: dict[str, Any] | None = None,
    before: RunnableConfig | None = None,
    limit: int | None = None,
) -> AsyncIterator[CheckpointTuple]
```

Asynchronous version of `list`.

This method is an asynchronous wrapper around `list` that runs the synchronous
method in a separate thread using asyncio.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to use for listing the checkpoints.<br>**TYPE:**`RunnableConfig | None` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[CheckpointTuple]` | An asynchronous iterator of checkpoint tuples. |

#### ``aput`async`¶

```
aput(
    config: RunnableConfig,
    checkpoint: Checkpoint,
    metadata: CheckpointMetadata,
    new_versions: ChannelVersions,
) -> RunnableConfig
```

Asynchronous version of `put`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to associate with the checkpoint.<br>**TYPE:**`RunnableConfig` |
| `checkpoint` | The checkpoint to save.<br>**TYPE:**`Checkpoint` |
| `metadata` | Additional metadata to save with the checkpoint.<br>**TYPE:**`CheckpointMetadata` |
| `new_versions` | New versions as of this write<br>**TYPE:**`ChannelVersions` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableConfig` | The updated config containing the saved checkpoint's timestamp.<br>**TYPE:**`RunnableConfig` |

#### ``aput\_writes`async`¶

```
aput_writes(
    config: RunnableConfig,
    writes: Sequence[tuple[str, Any]],
    task_id: str,
    task_path: str = "",
) -> None
```

Asynchronous version of `put_writes`.

This method is an asynchronous wrapper around `put_writes` that runs the synchronous
method in a separate thread using asyncio.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to associate with the writes.<br>**TYPE:**`RunnableConfig` |
| `writes` | The writes to save, each as a (channel, value) pair.<br>**TYPE:**`Sequence[tuple[str, Any]]` |
| `task_id` | Identifier for the task creating the writes.<br>**TYPE:**`str` |
| `task_path` | Path of the task creating the writes.<br>**TYPE:**`str`**DEFAULT:**`''` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

#### ``adelete\_thread`async`¶

```
adelete_thread(thread_id: str) -> None
```

Delete all checkpoints and writes associated with a thread ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to delete.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

#### ``get\_next\_version ¶

```
get_next_version(current: str | None, channel: None) -> str
```

Generate the next version ID for a channel.

Default is to use integer versions, incrementing by `1`. If you override, you can use `str`/`int`/`float`
versions, as long as they are monotonically increasing.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `current` | The current version identifier (`int`, `float`, or `str`).<br>**TYPE:**`V | None` |
| `channel` | Deprecated argument, kept for backwards compatibility.<br>**TYPE:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `V` | The next version identifier, which must be increasing.<br>**TYPE:**`V` |

#### ``get ¶

```
get(config: RunnableConfig) -> Checkpoint | None
```

Fetch a checkpoint using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Checkpoint | None` | The requested checkpoint, or `None` if not found. |

#### ``aget`async`¶

```
aget(config: RunnableConfig) -> Checkpoint | None
```

Asynchronously fetch a checkpoint using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Checkpoint | None` | The requested checkpoint, or `None` if not found. |

### ``PersistentDict ¶

Bases: `defaultdict`

Persistent dictionary with an API compatible with shelve and anydbm.

The dict is kept in memory, so the dictionary operations run as fast as
a regular dictionary.

Write to disk is delayed until close or sync (similar to gdbm's fast mode).

Input file format is automatically discovered.
Output file format is selectable between pickle, json, and csv.
All three serialization formats are backed by fast C implementations.

Adapted from https://code.activestate.com/recipes/576642-persistent-dict-with-multiple-standard-file-format/

| METHOD | DESCRIPTION |
| --- | --- |
| `sync` | Write dict to disk |

#### ``sync ¶

```
sync() -> None
```

Write dict to disk

## ``langgraph.checkpoint.sqlite ¶

### ``SqliteSaver ¶

Bases: `BaseCheckpointSaver[str]`

A checkpoint saver that stores checkpoints in a SQLite database.

Note

This class is meant for lightweight, synchronous use cases
(demos and small projects) and does not
scale to multiple threads.
For a similar sqlite saver with `async` support,
consider using AsyncSqliteSaver.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `conn` | The SQLite database connection.<br>**TYPE:**`Connection` |
| `serde` | The serializer to use for serializing and deserializing checkpoints. Defaults to JsonPlusSerializerCompat.<br>**TYPE:**`Optional[SerializerProtocol]`**DEFAULT:**`None` |

Examples:

```
>>> import sqlite3
>>> from langgraph.checkpoint.sqlite import SqliteSaver
>>> from langgraph.graph import StateGraph
>>>
>>> builder = StateGraph(int)
>>> builder.add_node("add_one", lambda x: x + 1)
>>> builder.set_entry_point("add_one")
>>> builder.set_finish_point("add_one")
>>> # Create a new SqliteSaver instance
>>> # Note: check_same_thread=False is OK as the implementation uses a lock
>>> # to ensure thread safety.
>>> conn = sqlite3.connect("checkpoints.sqlite", check_same_thread=False)
>>> memory = SqliteSaver(conn)
>>> graph = builder.compile(checkpointer=memory)
>>> config = {"configurable": {"thread_id": "1"}}
>>> graph.get_state(config)
>>> result = graph.invoke(3, config)
>>> graph.get_state(config)
StateSnapshot(values=4, next=(), config={'configurable': {'thread_id': '1', 'checkpoint_ns': '', 'checkpoint_id': '0c62ca34-ac19-445d-bbb0-5b4984975b2a'}}, parent_config=None)
```

| METHOD | DESCRIPTION |
| --- | --- |
| `from_conn_string` | Create a new SqliteSaver instance from a connection string. |
| `setup` | Set up the checkpoint database. |
| `cursor` | Get a cursor for the SQLite database. |
| `get_tuple` | Get a checkpoint tuple from the database. |
| `list` | List checkpoints from the database. |
| `put` | Save a checkpoint to the database. |
| `put_writes` | Store intermediate writes linked to a checkpoint. |
| `delete_thread` | Delete all checkpoints and writes associated with a thread ID. |
| `aget_tuple` | Get a checkpoint tuple from the database asynchronously. |
| `alist` | List checkpoints from the database asynchronously. |
| `aput` | Save a checkpoint to the database asynchronously. |
| `get_next_version` | Generate the next version ID for a channel. |
| `get` | Fetch a checkpoint using the given configuration. |
| `aget` | Asynchronously fetch a checkpoint using the given configuration. |
| `aput_writes` | Asynchronously store intermediate writes linked to a checkpoint. |
| `adelete_thread` | Delete all checkpoints and writes associated with a specific thread ID. |

#### ``config\_specs`property`¶

```
config_specs: list
```

Define the configuration options for the checkpoint saver.

| RETURNS | DESCRIPTION |
| --- | --- |
| `list` | List of configuration field specs.<br>**TYPE:**`list` |

#### ``from\_conn\_string`classmethod`¶

```
from_conn_string(conn_string: str) -> Iterator[SqliteSaver]
```

Create a new SqliteSaver instance from a connection string.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `conn_string` | The SQLite connection string.<br>**TYPE:**`str` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `SqliteSaver` | A new SqliteSaver instance.<br>**TYPE::**`SqliteSaver` |

Examples:

```
In memory:

    with SqliteSaver.from_conn_string(":memory:") as memory:
        ...

To disk:

    with SqliteSaver.from_conn_string("checkpoints.sqlite") as memory:
        ...
```

#### ``setup ¶

```
setup() -> None
```

Set up the checkpoint database.

This method creates the necessary tables in the SQLite database if they don't
already exist. It is called automatically when needed and should not be called
directly by the user.

#### ``cursor ¶

```
cursor(transaction: bool = True) -> Iterator[Cursor]
```

Get a cursor for the SQLite database.

This method returns a cursor for the SQLite database. It is used internally
by the SqliteSaver and should not be called directly by the user.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `transaction` | Whether to commit the transaction when the cursor is closed. Defaults to True.<br>**TYPE:**`bool`**DEFAULT:**`True` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `Cursor` | sqlite3.Cursor: A cursor for the SQLite database. |

#### ``get\_tuple ¶

```
get_tuple(config: RunnableConfig) -> CheckpointTuple | None
```

Get a checkpoint tuple from the database.

This method retrieves a checkpoint tuple from the SQLite database based on the
provided config. If the config contains a `checkpoint_id` key, the checkpoint with
the matching thread ID and checkpoint ID is retrieved. Otherwise, the latest checkpoint
for the given thread ID is retrieved.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to use for retrieving the checkpoint.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple | None` | The retrieved checkpoint tuple, or None if no matching checkpoint was found. |

Examples:

```
Basic:
>>> config = {"configurable": {"thread_id": "1"}}
>>> checkpoint_tuple = memory.get_tuple(config)
>>> print(checkpoint_tuple)
CheckpointTuple(...)

With checkpoint ID:

>>> config = {
...    "configurable": {
...        "thread_id": "1",
...        "checkpoint_ns": "",
...        "checkpoint_id": "1ef4f797-8335-6428-8001-8a1503f9b875",
...    }
... }
>>> checkpoint_tuple = memory.get_tuple(config)
>>> print(checkpoint_tuple)
CheckpointTuple(...)
```

#### ``list ¶

```
list(
    config: RunnableConfig | None,
    *,
    filter: dict[str, Any] | None = None,
    before: RunnableConfig | None = None,
    limit: int | None = None,
) -> Iterator[CheckpointTuple]
```

List checkpoints from the database.

This method retrieves a list of checkpoint tuples from the SQLite database based
on the provided config. The checkpoints are ordered by checkpoint ID in descending order (newest first).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to use for listing the checkpoints.<br>**TYPE:**`RunnableConfig | None` |
| `filter` | Additional filtering criteria for metadata.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `before` | If provided, only checkpoints before the specified checkpoint ID are returned.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `limit` | The maximum number of checkpoints to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple` | An iterator of checkpoint tuples. |

Examples:

```
>>> from langgraph.checkpoint.sqlite import SqliteSaver
>>> with SqliteSaver.from_conn_string(":memory:") as memory:
... # Run a graph, then list the checkpoints
>>>     config = {"configurable": {"thread_id": "1"}}
>>>     checkpoints = list(memory.list(config, limit=2))
>>> print(checkpoints)
[CheckpointTuple(...), CheckpointTuple(...)]
```

```
>>> config = {"configurable": {"thread_id": "1"}}
>>> before = {"configurable": {"checkpoint_id": "1ef4f797-8335-6428-8001-8a1503f9b875"}}
>>> with SqliteSaver.from_conn_string(":memory:") as memory:
... # Run a graph, then list the checkpoints
>>>     checkpoints = list(memory.list(config, before=before))
>>> print(checkpoints)
[CheckpointTuple(...), ...]
```

#### ``put ¶

```
put(
    config: RunnableConfig,
    checkpoint: Checkpoint,
    metadata: CheckpointMetadata,
    new_versions: ChannelVersions,
) -> RunnableConfig
```

Save a checkpoint to the database.

This method saves a checkpoint to the SQLite database. The checkpoint is associated
with the provided config and its parent config (if any).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to associate with the checkpoint.<br>**TYPE:**`RunnableConfig` |
| `checkpoint` | The checkpoint to save.<br>**TYPE:**`Checkpoint` |
| `metadata` | Additional metadata to save with the checkpoint.<br>**TYPE:**`CheckpointMetadata` |
| `new_versions` | New channel versions as of this write.<br>**TYPE:**`ChannelVersions` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableConfig` | Updated configuration after storing the checkpoint.<br>**TYPE:**`RunnableConfig` |

Examples:

```
>>> from langgraph.checkpoint.sqlite import SqliteSaver
>>> with SqliteSaver.from_conn_string(":memory:") as memory:
>>>     config = {"configurable": {"thread_id": "1", "checkpoint_ns": ""}}
>>>     checkpoint = {"ts": "2024-05-04T06:32:42.235444+00:00", "id": "1ef4f797-8335-6428-8001-8a1503f9b875", "channel_values": {"key": "value"}}
>>>     saved_config = memory.put(config, checkpoint, {"source": "input", "step": 1, "writes": {"key": "value"}}, {})
>>> print(saved_config)
{'configurable': {'thread_id': '1', 'checkpoint_ns': '', 'checkpoint_id': '1ef4f797-8335-6428-8001-8a1503f9b875'}}
```

#### ``put\_writes ¶

```
put_writes(
    config: RunnableConfig,
    writes: Sequence[tuple[str, Any]],
    task_id: str,
    task_path: str = "",
) -> None
```

Store intermediate writes linked to a checkpoint.

This method saves intermediate writes associated with a checkpoint to the SQLite database.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration of the related checkpoint.<br>**TYPE:**`RunnableConfig` |
| `writes` | List of writes to store, each as (channel, value) pair.<br>**TYPE:**`Sequence[tuple[str, Any]]` |
| `task_id` | Identifier for the task creating the writes.<br>**TYPE:**`str` |
| `task_path` | Path of the task creating the writes.<br>**TYPE:**`str`**DEFAULT:**`''` |

#### ``delete\_thread ¶

```
delete_thread(thread_id: str) -> None
```

Delete all checkpoints and writes associated with a thread ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to delete.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

#### ``aget\_tuple`async`¶

```
aget_tuple(config: RunnableConfig) -> CheckpointTuple | None
```

Get a checkpoint tuple from the database asynchronously.

Note

This async method is not supported by the SqliteSaver class.
Use get\_tuple() instead, or consider using AsyncSqliteSaver.

#### ``alist`async`¶

```
alist(
    config: RunnableConfig | None,
    *,
    filter: dict[str, Any] | None = None,
    before: RunnableConfig | None = None,
    limit: int | None = None,
) -> AsyncIterator[CheckpointTuple]
```

List checkpoints from the database asynchronously.

Note

This async method is not supported by the SqliteSaver class.
Use list() instead, or consider using AsyncSqliteSaver.

#### ``aput`async`¶

```
aput(
    config: RunnableConfig,
    checkpoint: Checkpoint,
    metadata: CheckpointMetadata,
    new_versions: ChannelVersions,
) -> RunnableConfig
```

Save a checkpoint to the database asynchronously.

Note

This async method is not supported by the SqliteSaver class.
Use put() instead, or consider using AsyncSqliteSaver.

#### ``get\_next\_version ¶

```
get_next_version(current: str | None, channel: None) -> str
```

Generate the next version ID for a channel.

This method creates a new version identifier for a channel based on its current version.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `current` | The current version identifier of the channel.<br>**TYPE:**`Optional[str]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | The next version identifier, which is guaranteed to be monotonically increasing.<br>**TYPE:**`str` |

#### ``get ¶

```
get(config: RunnableConfig) -> Checkpoint | None
```

Fetch a checkpoint using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Checkpoint | None` | The requested checkpoint, or `None` if not found. |

#### ``aget`async`¶

```
aget(config: RunnableConfig) -> Checkpoint | None
```

Asynchronously fetch a checkpoint using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Checkpoint | None` | The requested checkpoint, or `None` if not found. |

#### ``aput\_writes`async`¶

```
aput_writes(
    config: RunnableConfig,
    writes: Sequence[tuple[str, Any]],
    task_id: str,
    task_path: str = "",
) -> None
```

Asynchronously store intermediate writes linked to a checkpoint.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration of the related checkpoint.<br>**TYPE:**`RunnableConfig` |
| `writes` | List of writes to store.<br>**TYPE:**`Sequence[tuple[str, Any]]` |
| `task_id` | Identifier for the task creating the writes.<br>**TYPE:**`str` |
| `task_path` | Path of the task creating the writes.<br>**TYPE:**`str`**DEFAULT:**`''` |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Implement this method in your custom checkpoint saver. |

#### ``adelete\_thread`async`¶

```
adelete_thread(thread_id: str) -> None
```

Delete all checkpoints and writes associated with a specific thread ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID whose checkpoints should be deleted.<br>**TYPE:**`str` |

## ``langgraph.checkpoint.sqlite.aio ¶

### ``AsyncSqliteSaver ¶

Bases: `BaseCheckpointSaver[str]`

An asynchronous checkpoint saver that stores checkpoints in a SQLite database.

This class provides an asynchronous interface for saving and retrieving checkpoints
using a SQLite database. It's designed for use in asynchronous environments and
offers better performance for I/O-bound operations compared to synchronous alternatives.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `conn` | The asynchronous SQLite database connection.<br>**TYPE:**`Connection` |
| `serde` | The serializer used for encoding/decoding checkpoints.<br>**TYPE:**`SerializerProtocol` |

Tip

Requires the aiosqlite package.
Install it with `pip install aiosqlite`.

Warning

While this class supports asynchronous checkpointing, it is not recommended
for production workloads due to limitations in SQLite's write performance.
For production use, consider a more robust database like PostgreSQL.

Tip

Remember to **close the database connection** after executing your code,
otherwise, you may see the graph "hang" after execution (since the program
will not exit until the connection is closed).

The easiest way is to use the `async with` statement as shown in the examples.

```
async with AsyncSqliteSaver.from_conn_string("checkpoints.sqlite") as saver:
    # Your code here
    graph = builder.compile(checkpointer=saver)
    config = {"configurable": {"thread_id": "thread-1"}}
    async for event in graph.astream_events(..., config, version="v1"):
        print(event)
```

Examples:

Usage within StateGraph:

```
>>> import asyncio
>>>
>>> from langgraph.checkpoint.sqlite.aio import AsyncSqliteSaver
>>> from langgraph.graph import StateGraph
>>>
>>> async def main():
>>>     builder = StateGraph(int)
>>>     builder.add_node("add_one", lambda x: x + 1)
>>>     builder.set_entry_point("add_one")
>>>     builder.set_finish_point("add_one")
>>>     async with AsyncSqliteSaver.from_conn_string("checkpoints.db") as memory:
>>>         graph = builder.compile(checkpointer=memory)
>>>         coro = graph.ainvoke(1, {"configurable": {"thread_id": "thread-1"}})
>>>         print(await asyncio.gather(coro))
>>>
>>> asyncio.run(main())
Output: [2]
```

Raw usage:

```
>>> import asyncio
>>> import aiosqlite
>>> from langgraph.checkpoint.sqlite.aio import AsyncSqliteSaver
>>>
>>> async def main():
>>>     async with aiosqlite.connect("checkpoints.db") as conn:
...         saver = AsyncSqliteSaver(conn)
...         config = {"configurable": {"thread_id": "1", "checkpoint_ns": ""}}
...         checkpoint = {"ts": "2023-05-03T10:00:00Z", "data": {"key": "value"}, "id": "0c62ca34-ac19-445d-bbb0-5b4984975b2a"}
...         saved_config = await saver.aput(config, checkpoint, {}, {})
...         print(saved_config)
>>> asyncio.run(main())
{'configurable': {'thread_id': '1', 'checkpoint_ns': '', 'checkpoint_id': '0c62ca34-ac19-445d-bbb0-5b4984975b2a'}}
```

| METHOD | DESCRIPTION |
| --- | --- |
| `from_conn_string` | Create a new AsyncSqliteSaver instance from a connection string. |
| `get_tuple` | Get a checkpoint tuple from the database. |
| `list` | List checkpoints from the database asynchronously. |
| `put` | Save a checkpoint to the database. |
| `put_writes` | Store intermediate writes linked to a checkpoint. |
| `delete_thread` | Delete all checkpoints and writes associated with a thread ID. |
| `setup` | Set up the checkpoint database asynchronously. |
| `aget_tuple` | Get a checkpoint tuple from the database asynchronously. |
| `alist` | List checkpoints from the database asynchronously. |
| `aput` | Save a checkpoint to the database asynchronously. |
| `aput_writes` | Store intermediate writes linked to a checkpoint asynchronously. |
| `adelete_thread` | Delete all checkpoints and writes associated with a thread ID. |
| `get_next_version` | Generate the next version ID for a channel. |
| `get` | Fetch a checkpoint using the given configuration. |
| `aget` | Asynchronously fetch a checkpoint using the given configuration. |

#### ``config\_specs`property`¶

```
config_specs: list
```

Define the configuration options for the checkpoint saver.

| RETURNS | DESCRIPTION |
| --- | --- |
| `list` | List of configuration field specs.<br>**TYPE:**`list` |

#### ``from\_conn\_string`async``classmethod`¶

```
from_conn_string(conn_string: str) -> AsyncIterator[AsyncSqliteSaver]
```

Create a new AsyncSqliteSaver instance from a connection string.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `conn_string` | The SQLite connection string.<br>**TYPE:**`str` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncSqliteSaver` | A new AsyncSqliteSaver instance.<br>**TYPE::**`AsyncIterator[AsyncSqliteSaver]` |

#### ``get\_tuple ¶

```
get_tuple(config: RunnableConfig) -> CheckpointTuple | None
```

Get a checkpoint tuple from the database.

This method retrieves a checkpoint tuple from the SQLite database based on the
provided config. If the config contains a `checkpoint_id` key, the checkpoint with
the matching thread ID and checkpoint ID is retrieved. Otherwise, the latest checkpoint
for the given thread ID is retrieved.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to use for retrieving the checkpoint.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple | None` | The retrieved checkpoint tuple, or None if no matching checkpoint was found. |

#### ``list ¶

```
list(
    config: RunnableConfig | None,
    *,
    filter: dict[str, Any] | None = None,
    before: RunnableConfig | None = None,
    limit: int | None = None,
) -> Iterator[CheckpointTuple]
```

List checkpoints from the database asynchronously.

This method retrieves a list of checkpoint tuples from the SQLite database based
on the provided config. The checkpoints are ordered by checkpoint ID in descending order (newest first).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Base configuration for filtering checkpoints.<br>**TYPE:**`RunnableConfig | None` |
| `filter` | Additional filtering criteria for metadata.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `before` | If provided, only checkpoints before the specified checkpoint ID are returned.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `limit` | Maximum number of checkpoints to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple` | An iterator of matching checkpoint tuples. |

#### ``put ¶

```
put(
    config: RunnableConfig,
    checkpoint: Checkpoint,
    metadata: CheckpointMetadata,
    new_versions: ChannelVersions,
) -> RunnableConfig
```

Save a checkpoint to the database.

This method saves a checkpoint to the SQLite database. The checkpoint is associated
with the provided config and its parent config (if any).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to associate with the checkpoint.<br>**TYPE:**`RunnableConfig` |
| `checkpoint` | The checkpoint to save.<br>**TYPE:**`Checkpoint` |
| `metadata` | Additional metadata to save with the checkpoint.<br>**TYPE:**`CheckpointMetadata` |
| `new_versions` | New channel versions as of this write.<br>**TYPE:**`ChannelVersions` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableConfig` | Updated configuration after storing the checkpoint.<br>**TYPE:**`RunnableConfig` |

#### ``put\_writes ¶

```
put_writes(
    config: RunnableConfig,
    writes: Sequence[tuple[str, Any]],
    task_id: str,
    task_path: str = "",
) -> None
```

Store intermediate writes linked to a checkpoint.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration of the related checkpoint.<br>**TYPE:**`RunnableConfig` |
| `writes` | List of writes to store.<br>**TYPE:**`Sequence[tuple[str, Any]]` |
| `task_id` | Identifier for the task creating the writes.<br>**TYPE:**`str` |
| `task_path` | Path of the task creating the writes.<br>**TYPE:**`str`**DEFAULT:**`''` |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Implement this method in your custom checkpoint saver. |

#### ``delete\_thread ¶

```
delete_thread(thread_id: str) -> None
```

Delete all checkpoints and writes associated with a thread ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to delete.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

#### ``setup`async`¶

```
setup() -> None
```

Set up the checkpoint database asynchronously.

This method creates the necessary tables in the SQLite database if they don't
already exist. It is called automatically when needed and should not be called
directly by the user.

#### ``aget\_tuple`async`¶

```
aget_tuple(config: RunnableConfig) -> CheckpointTuple | None
```

Get a checkpoint tuple from the database asynchronously.

This method retrieves a checkpoint tuple from the SQLite database based on the
provided config. If the config contains a `checkpoint_id` key, the checkpoint with
the matching thread ID and checkpoint ID is retrieved. Otherwise, the latest checkpoint
for the given thread ID is retrieved.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to use for retrieving the checkpoint.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple | None` | The retrieved checkpoint tuple, or None if no matching checkpoint was found. |

#### ``alist`async`¶

```
alist(
    config: RunnableConfig | None,
    *,
    filter: dict[str, Any] | None = None,
    before: RunnableConfig | None = None,
    limit: int | None = None,
) -> AsyncIterator[CheckpointTuple]
```

List checkpoints from the database asynchronously.

This method retrieves a list of checkpoint tuples from the SQLite database based
on the provided config. The checkpoints are ordered by checkpoint ID in descending order (newest first).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Base configuration for filtering checkpoints.<br>**TYPE:**`RunnableConfig | None` |
| `filter` | Additional filtering criteria for metadata.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `before` | If provided, only checkpoints before the specified checkpoint ID are returned.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `limit` | Maximum number of checkpoints to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[CheckpointTuple]` | An asynchronous iterator of matching checkpoint tuples. |

#### ``aput`async`¶

```
aput(
    config: RunnableConfig,
    checkpoint: Checkpoint,
    metadata: CheckpointMetadata,
    new_versions: ChannelVersions,
) -> RunnableConfig
```

Save a checkpoint to the database asynchronously.

This method saves a checkpoint to the SQLite database. The checkpoint is associated
with the provided config and its parent config (if any).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to associate with the checkpoint.<br>**TYPE:**`RunnableConfig` |
| `checkpoint` | The checkpoint to save.<br>**TYPE:**`Checkpoint` |
| `metadata` | Additional metadata to save with the checkpoint.<br>**TYPE:**`CheckpointMetadata` |
| `new_versions` | New channel versions as of this write.<br>**TYPE:**`ChannelVersions` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableConfig` | Updated configuration after storing the checkpoint.<br>**TYPE:**`RunnableConfig` |

#### ``aput\_writes`async`¶

```
aput_writes(
    config: RunnableConfig,
    writes: Sequence[tuple[str, Any]],
    task_id: str,
    task_path: str = "",
) -> None
```

Store intermediate writes linked to a checkpoint asynchronously.

This method saves intermediate writes associated with a checkpoint to the database.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration of the related checkpoint.<br>**TYPE:**`RunnableConfig` |
| `writes` | List of writes to store, each as (channel, value) pair.<br>**TYPE:**`Sequence[tuple[str, Any]]` |
| `task_id` | Identifier for the task creating the writes.<br>**TYPE:**`str` |
| `task_path` | Path of the task creating the writes.<br>**TYPE:**`str`**DEFAULT:**`''` |

#### ``adelete\_thread`async`¶

```
adelete_thread(thread_id: str) -> None
```

Delete all checkpoints and writes associated with a thread ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to delete.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

#### ``get\_next\_version ¶

```
get_next_version(current: str | None, channel: None) -> str
```

Generate the next version ID for a channel.

This method creates a new version identifier for a channel based on its current version.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `current` | The current version identifier of the channel.<br>**TYPE:**`Optional[str]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | The next version identifier, which is guaranteed to be monotonically increasing.<br>**TYPE:**`str` |

#### ``get ¶

```
get(config: RunnableConfig) -> Checkpoint | None
```

Fetch a checkpoint using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Checkpoint | None` | The requested checkpoint, or `None` if not found. |

#### ``aget`async`¶

```
aget(config: RunnableConfig) -> Checkpoint | None
```

Asynchronously fetch a checkpoint using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Checkpoint | None` | The requested checkpoint, or `None` if not found. |

## ``langgraph.checkpoint.postgres ¶

### ``PostgresSaver ¶

Bases: `BasePostgresSaver`

Checkpointer that stores checkpoints in a Postgres database.

| METHOD | DESCRIPTION |
| --- | --- |
| `from_conn_string` | Create a new PostgresSaver instance from a connection string. |
| `setup` | Set up the checkpoint database asynchronously. |
| `list` | List checkpoints from the database. |
| `get_tuple` | Get a checkpoint tuple from the database. |
| `put` | Save a checkpoint to the database. |
| `put_writes` | Store intermediate writes linked to a checkpoint. |
| `delete_thread` | Delete all checkpoints and writes associated with a thread ID. |
| `get` | Fetch a checkpoint using the given configuration. |
| `aget` | Asynchronously fetch a checkpoint using the given configuration. |
| `aget_tuple` | Asynchronously fetch a checkpoint tuple using the given configuration. |
| `alist` | Asynchronously list checkpoints that match the given criteria. |
| `aput` | Asynchronously store a checkpoint with its configuration and metadata. |
| `aput_writes` | Asynchronously store intermediate writes linked to a checkpoint. |
| `adelete_thread` | Delete all checkpoints and writes associated with a specific thread ID. |
| `get_next_version` | Generate the next version ID for a channel. |

#### ``config\_specs`property`¶

```
config_specs: list
```

Define the configuration options for the checkpoint saver.

| RETURNS | DESCRIPTION |
| --- | --- |
| `list` | List of configuration field specs.<br>**TYPE:**`list` |

#### ``from\_conn\_string`classmethod`¶

```
from_conn_string(
    conn_string: str, *, pipeline: bool = False
) -> Iterator[PostgresSaver]
```

Create a new PostgresSaver instance from a connection string.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `conn_string` | The Postgres connection info string.<br>**TYPE:**`str` |
| `pipeline` | whether to use Pipeline<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `PostgresSaver` | A new PostgresSaver instance.<br>**TYPE:**`Iterator[PostgresSaver]` |

#### ``setup ¶

```
setup() -> None
```

Set up the checkpoint database asynchronously.

This method creates the necessary tables in the Postgres database if they don't
already exist and runs database migrations. It MUST be called directly by the user
the first time checkpointer is used.

#### ``list ¶

```
list(
    config: RunnableConfig | None,
    *,
    filter: dict[str, Any] | None = None,
    before: RunnableConfig | None = None,
    limit: int | None = None,
) -> Iterator[CheckpointTuple]
```

List checkpoints from the database.

This method retrieves a list of checkpoint tuples from the Postgres database based
on the provided config. The checkpoints are ordered by checkpoint ID in descending order (newest first).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to use for listing the checkpoints.<br>**TYPE:**`RunnableConfig | None` |
| `filter` | Additional filtering criteria for metadata.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `before` | If provided, only checkpoints before the specified checkpoint ID are returned.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `limit` | The maximum number of checkpoints to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple` | An iterator of checkpoint tuples. |

Examples:

```
>>> from langgraph.checkpoint.postgres import PostgresSaver
>>> DB_URI = "postgres://postgres:postgres@localhost:5432/postgres?sslmode=disable"
>>> with PostgresSaver.from_conn_string(DB_URI) as memory:
... # Run a graph, then list the checkpoints
>>>     config = {"configurable": {"thread_id": "1"}}
>>>     checkpoints = list(memory.list(config, limit=2))
>>> print(checkpoints)
[CheckpointTuple(...), CheckpointTuple(...)]
```

```
>>> config = {"configurable": {"thread_id": "1"}}
>>> before = {"configurable": {"checkpoint_id": "1ef4f797-8335-6428-8001-8a1503f9b875"}}
>>> with PostgresSaver.from_conn_string(DB_URI) as memory:
... # Run a graph, then list the checkpoints
>>>     checkpoints = list(memory.list(config, before=before))
>>> print(checkpoints)
[CheckpointTuple(...), ...]
```

#### ``get\_tuple ¶

```
get_tuple(config: RunnableConfig) -> CheckpointTuple | None
```

Get a checkpoint tuple from the database.

This method retrieves a checkpoint tuple from the Postgres database based on the
provided config. If the config contains a `checkpoint_id` key, the checkpoint with
the matching thread ID and timestamp is retrieved. Otherwise, the latest checkpoint
for the given thread ID is retrieved.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to use for retrieving the checkpoint.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple | None` | The retrieved checkpoint tuple, or None if no matching checkpoint was found. |

Examples:

```
Basic:
>>> config = {"configurable": {"thread_id": "1"}}
>>> checkpoint_tuple = memory.get_tuple(config)
>>> print(checkpoint_tuple)
CheckpointTuple(...)

With timestamp:

>>> config = {
...    "configurable": {
...        "thread_id": "1",
...        "checkpoint_ns": "",
...        "checkpoint_id": "1ef4f797-8335-6428-8001-8a1503f9b875",
...    }
... }
>>> checkpoint_tuple = memory.get_tuple(config)
>>> print(checkpoint_tuple)
CheckpointTuple(...)
```

#### ``put ¶

```
put(
    config: RunnableConfig,
    checkpoint: Checkpoint,
    metadata: CheckpointMetadata,
    new_versions: ChannelVersions,
) -> RunnableConfig
```

Save a checkpoint to the database.

This method saves a checkpoint to the Postgres database. The checkpoint is associated
with the provided config and its parent config (if any).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to associate with the checkpoint.<br>**TYPE:**`RunnableConfig` |
| `checkpoint` | The checkpoint to save.<br>**TYPE:**`Checkpoint` |
| `metadata` | Additional metadata to save with the checkpoint.<br>**TYPE:**`CheckpointMetadata` |
| `new_versions` | New channel versions as of this write.<br>**TYPE:**`ChannelVersions` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableConfig` | Updated configuration after storing the checkpoint.<br>**TYPE:**`RunnableConfig` |

Examples:

```
>>> from langgraph.checkpoint.postgres import PostgresSaver
>>> DB_URI = "postgres://postgres:postgres@localhost:5432/postgres?sslmode=disable"
>>> with PostgresSaver.from_conn_string(DB_URI) as memory:
>>>     config = {"configurable": {"thread_id": "1", "checkpoint_ns": ""}}
>>>     checkpoint = {"ts": "2024-05-04T06:32:42.235444+00:00", "id": "1ef4f797-8335-6428-8001-8a1503f9b875", "channel_values": {"key": "value"}}
>>>     saved_config = memory.put(config, checkpoint, {"source": "input", "step": 1, "writes": {"key": "value"}}, {})
>>> print(saved_config)
{'configurable': {'thread_id': '1', 'checkpoint_ns': '', 'checkpoint_id': '1ef4f797-8335-6428-8001-8a1503f9b875'}}
```

#### ``put\_writes ¶

```
put_writes(
    config: RunnableConfig,
    writes: Sequence[tuple[str, Any]],
    task_id: str,
    task_path: str = "",
) -> None
```

Store intermediate writes linked to a checkpoint.

This method saves intermediate writes associated with a checkpoint to the Postgres database.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration of the related checkpoint.<br>**TYPE:**`RunnableConfig` |
| `writes` | List of writes to store.<br>**TYPE:**`Sequence[tuple[str, Any]]` |
| `task_id` | Identifier for the task creating the writes.<br>**TYPE:**`str` |

#### ``delete\_thread ¶

```
delete_thread(thread_id: str) -> None
```

Delete all checkpoints and writes associated with a thread ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to delete.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

#### ``get ¶

```
get(config: RunnableConfig) -> Checkpoint | None
```

Fetch a checkpoint using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Checkpoint | None` | The requested checkpoint, or `None` if not found. |

#### ``aget`async`¶

```
aget(config: RunnableConfig) -> Checkpoint | None
```

Asynchronously fetch a checkpoint using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Checkpoint | None` | The requested checkpoint, or `None` if not found. |

#### ``aget\_tuple`async`¶

```
aget_tuple(config: RunnableConfig) -> CheckpointTuple | None
```

Asynchronously fetch a checkpoint tuple using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple | None` | The requested checkpoint tuple, or `None` if not found. |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Implement this method in your custom checkpoint saver. |

#### ``alist`async`¶

```
alist(
    config: RunnableConfig | None,
    *,
    filter: dict[str, Any] | None = None,
    before: RunnableConfig | None = None,
    limit: int | None = None,
) -> AsyncIterator[CheckpointTuple]
```

Asynchronously list checkpoints that match the given criteria.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Base configuration for filtering checkpoints.<br>**TYPE:**`RunnableConfig | None` |
| `filter` | Additional filtering criteria for metadata.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `before` | List checkpoints created before this configuration.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `limit` | Maximum number of checkpoints to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[CheckpointTuple]` | Async iterator of matching checkpoint tuples. |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Implement this method in your custom checkpoint saver. |

#### ``aput`async`¶

```
aput(
    config: RunnableConfig,
    checkpoint: Checkpoint,
    metadata: CheckpointMetadata,
    new_versions: ChannelVersions,
) -> RunnableConfig
```

Asynchronously store a checkpoint with its configuration and metadata.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration for the checkpoint.<br>**TYPE:**`RunnableConfig` |
| `checkpoint` | The checkpoint to store.<br>**TYPE:**`Checkpoint` |
| `metadata` | Additional metadata for the checkpoint.<br>**TYPE:**`CheckpointMetadata` |
| `new_versions` | New channel versions as of this write.<br>**TYPE:**`ChannelVersions` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableConfig` | Updated configuration after storing the checkpoint.<br>**TYPE:**`RunnableConfig` |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Implement this method in your custom checkpoint saver. |

#### ``aput\_writes`async`¶

```
aput_writes(
    config: RunnableConfig,
    writes: Sequence[tuple[str, Any]],
    task_id: str,
    task_path: str = "",
) -> None
```

Asynchronously store intermediate writes linked to a checkpoint.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration of the related checkpoint.<br>**TYPE:**`RunnableConfig` |
| `writes` | List of writes to store.<br>**TYPE:**`Sequence[tuple[str, Any]]` |
| `task_id` | Identifier for the task creating the writes.<br>**TYPE:**`str` |
| `task_path` | Path of the task creating the writes.<br>**TYPE:**`str`**DEFAULT:**`''` |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | Implement this method in your custom checkpoint saver. |

#### ``adelete\_thread`async`¶

```
adelete_thread(thread_id: str) -> None
```

Delete all checkpoints and writes associated with a specific thread ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID whose checkpoints should be deleted.<br>**TYPE:**`str` |

#### ``get\_next\_version ¶

```
get_next_version(current: str | None, channel: None) -> str
```

Generate the next version ID for a channel.

Default is to use integer versions, incrementing by `1`. If you override, you can use `str`/`int`/`float`
versions, as long as they are monotonically increasing.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `current` | The current version identifier (`int`, `float`, or `str`).<br>**TYPE:**`V | None` |
| `channel` | Deprecated argument, kept for backwards compatibility.<br>**TYPE:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `V` | The next version identifier, which must be increasing.<br>**TYPE:**`V` |

## ``langgraph.checkpoint.postgres.aio ¶

### ``AsyncPostgresSaver ¶

Bases: `BasePostgresSaver`

Asynchronous checkpointer that stores checkpoints in a Postgres database.

| METHOD | DESCRIPTION |
| --- | --- |
| `from_conn_string` | Create a new AsyncPostgresSaver instance from a connection string. |
| `setup` | Set up the checkpoint database asynchronously. |
| `alist` | List checkpoints from the database asynchronously. |
| `aget_tuple` | Get a checkpoint tuple from the database asynchronously. |
| `aput` | Save a checkpoint to the database asynchronously. |
| `aput_writes` | Store intermediate writes linked to a checkpoint asynchronously. |
| `adelete_thread` | Delete all checkpoints and writes associated with a thread ID. |
| `list` | List checkpoints from the database. |
| `get_tuple` | Get a checkpoint tuple from the database. |
| `put` | Save a checkpoint to the database. |
| `put_writes` | Store intermediate writes linked to a checkpoint. |
| `delete_thread` | Delete all checkpoints and writes associated with a thread ID. |
| `get` | Fetch a checkpoint using the given configuration. |
| `aget` | Asynchronously fetch a checkpoint using the given configuration. |
| `get_next_version` | Generate the next version ID for a channel. |

#### ``config\_specs`property`¶

```
config_specs: list
```

Define the configuration options for the checkpoint saver.

| RETURNS | DESCRIPTION |
| --- | --- |
| `list` | List of configuration field specs.<br>**TYPE:**`list` |

#### ``from\_conn\_string`async``classmethod`¶

```
from_conn_string(
    conn_string: str, *, pipeline: bool = False, serde: SerializerProtocol | None = None
) -> AsyncIterator[AsyncPostgresSaver]
```

Create a new AsyncPostgresSaver instance from a connection string.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `conn_string` | The Postgres connection info string.<br>**TYPE:**`str` |
| `pipeline` | whether to use AsyncPipeline<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `AsyncPostgresSaver` | A new AsyncPostgresSaver instance.<br>**TYPE:**`AsyncIterator[AsyncPostgresSaver]` |

#### ``setup`async`¶

```
setup() -> None
```

Set up the checkpoint database asynchronously.

This method creates the necessary tables in the Postgres database if they don't
already exist and runs database migrations. It MUST be called directly by the user
the first time checkpointer is used.

#### ``alist`async`¶

```
alist(
    config: RunnableConfig | None,
    *,
    filter: dict[str, Any] | None = None,
    before: RunnableConfig | None = None,
    limit: int | None = None,
) -> AsyncIterator[CheckpointTuple]
```

List checkpoints from the database asynchronously.

This method retrieves a list of checkpoint tuples from the Postgres database based
on the provided config. The checkpoints are ordered by checkpoint ID in descending order (newest first).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Base configuration for filtering checkpoints.<br>**TYPE:**`RunnableConfig | None` |
| `filter` | Additional filtering criteria for metadata.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `before` | If provided, only checkpoints before the specified checkpoint ID are returned.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `limit` | Maximum number of checkpoints to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[CheckpointTuple]` | An asynchronous iterator of matching checkpoint tuples. |

#### ``aget\_tuple`async`¶

```
aget_tuple(config: RunnableConfig) -> CheckpointTuple | None
```

Get a checkpoint tuple from the database asynchronously.

This method retrieves a checkpoint tuple from the Postgres database based on the
provided config. If the config contains a `checkpoint_id` key, the checkpoint with
the matching thread ID and "checkpoint\_id" is retrieved. Otherwise, the latest checkpoint
for the given thread ID is retrieved.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to use for retrieving the checkpoint.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple | None` | The retrieved checkpoint tuple, or None if no matching checkpoint was found. |

#### ``aput`async`¶

```
aput(
    config: RunnableConfig,
    checkpoint: Checkpoint,
    metadata: CheckpointMetadata,
    new_versions: ChannelVersions,
) -> RunnableConfig
```

Save a checkpoint to the database asynchronously.

This method saves a checkpoint to the Postgres database. The checkpoint is associated
with the provided config and its parent config (if any).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to associate with the checkpoint.<br>**TYPE:**`RunnableConfig` |
| `checkpoint` | The checkpoint to save.<br>**TYPE:**`Checkpoint` |
| `metadata` | Additional metadata to save with the checkpoint.<br>**TYPE:**`CheckpointMetadata` |
| `new_versions` | New channel versions as of this write.<br>**TYPE:**`ChannelVersions` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableConfig` | Updated configuration after storing the checkpoint.<br>**TYPE:**`RunnableConfig` |

#### ``aput\_writes`async`¶

```
aput_writes(
    config: RunnableConfig,
    writes: Sequence[tuple[str, Any]],
    task_id: str,
    task_path: str = "",
) -> None
```

Store intermediate writes linked to a checkpoint asynchronously.

This method saves intermediate writes associated with a checkpoint to the database.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration of the related checkpoint.<br>**TYPE:**`RunnableConfig` |
| `writes` | List of writes to store, each as (channel, value) pair.<br>**TYPE:**`Sequence[tuple[str, Any]]` |
| `task_id` | Identifier for the task creating the writes.<br>**TYPE:**`str` |

#### ``adelete\_thread`async`¶

```
adelete_thread(thread_id: str) -> None
```

Delete all checkpoints and writes associated with a thread ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to delete.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

#### ``list ¶

```
list(
    config: RunnableConfig | None,
    *,
    filter: dict[str, Any] | None = None,
    before: RunnableConfig | None = None,
    limit: int | None = None,
) -> Iterator[CheckpointTuple]
```

List checkpoints from the database.

This method retrieves a list of checkpoint tuples from the Postgres database based
on the provided config. The checkpoints are ordered by checkpoint ID in descending order (newest first).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Base configuration for filtering checkpoints.<br>**TYPE:**`RunnableConfig | None` |
| `filter` | Additional filtering criteria for metadata.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `before` | If provided, only checkpoints before the specified checkpoint ID are returned.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `limit` | Maximum number of checkpoints to return.<br>**TYPE:**`int | None`**DEFAULT:**`None` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple` | An iterator of matching checkpoint tuples. |

#### ``get\_tuple ¶

```
get_tuple(config: RunnableConfig) -> CheckpointTuple | None
```

Get a checkpoint tuple from the database.

This method retrieves a checkpoint tuple from the Postgres database based on the
provided config. If the config contains a `checkpoint_id` key, the checkpoint with
the matching thread ID and "checkpoint\_id" is retrieved. Otherwise, the latest checkpoint
for the given thread ID is retrieved.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to use for retrieving the checkpoint.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `CheckpointTuple | None` | The retrieved checkpoint tuple, or None if no matching checkpoint was found. |

#### ``put ¶

```
put(
    config: RunnableConfig,
    checkpoint: Checkpoint,
    metadata: CheckpointMetadata,
    new_versions: ChannelVersions,
) -> RunnableConfig
```

Save a checkpoint to the database.

This method saves a checkpoint to the Postgres database. The checkpoint is associated
with the provided config and its parent config (if any).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to associate with the checkpoint.<br>**TYPE:**`RunnableConfig` |
| `checkpoint` | The checkpoint to save.<br>**TYPE:**`Checkpoint` |
| `metadata` | Additional metadata to save with the checkpoint.<br>**TYPE:**`CheckpointMetadata` |
| `new_versions` | New channel versions as of this write.<br>**TYPE:**`ChannelVersions` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableConfig` | Updated configuration after storing the checkpoint.<br>**TYPE:**`RunnableConfig` |

#### ``put\_writes ¶

```
put_writes(
    config: RunnableConfig,
    writes: Sequence[tuple[str, Any]],
    task_id: str,
    task_path: str = "",
) -> None
```

Store intermediate writes linked to a checkpoint.

This method saves intermediate writes associated with a checkpoint to the database.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration of the related checkpoint.<br>**TYPE:**`RunnableConfig` |
| `writes` | List of writes to store, each as (channel, value) pair.<br>**TYPE:**`Sequence[tuple[str, Any]]` |
| `task_id` | Identifier for the task creating the writes.<br>**TYPE:**`str` |
| `task_path` | Path of the task creating the writes.<br>**TYPE:**`str`**DEFAULT:**`''` |

#### ``delete\_thread ¶

```
delete_thread(thread_id: str) -> None
```

Delete all checkpoints and writes associated with a thread ID.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_id` | The thread ID to delete.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `None` | None |

#### ``get ¶

```
get(config: RunnableConfig) -> Checkpoint | None
```

Fetch a checkpoint using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Checkpoint | None` | The requested checkpoint, or `None` if not found. |

#### ``aget`async`¶

```
aget(config: RunnableConfig) -> Checkpoint | None
```

Asynchronously fetch a checkpoint using the given configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | Configuration specifying which checkpoint to retrieve.<br>**TYPE:**`RunnableConfig` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Checkpoint | None` | The requested checkpoint, or `None` if not found. |

#### ``get\_next\_version ¶

```
get_next_version(current: str | None, channel: None) -> str
```

Generate the next version ID for a channel.

Default is to use integer versions, incrementing by `1`. If you override, you can use `str`/`int`/`float`
versions, as long as they are monotonically increasing.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `current` | The current version identifier (`int`, `float`, or `str`).<br>**TYPE:**`V | None` |
| `channel` | Deprecated argument, kept for backwards compatibility.<br>**TYPE:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `V` | The next version identifier, which must be increasing.<br>**TYPE:**`V` |

Back to top