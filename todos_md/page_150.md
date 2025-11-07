<!-- URL: page_150 -->

Skip to content

Edit this page

# Storage ¶

`langchain-classic` documentation

These docs cover the `langchain-classic` package. This package will be maintained for security vulnerabilities until December 2026. Users are encouraged to migrate to the `langchain` package for the latest features and improvements. See docs for `langchain`

## ``langchain\_classic.storage ¶

Implementations of key-value stores and storage helpers.

Module provides implementations of various key-value stores that conform
to a simple key-value interface.

The primary goal of these storages is to support implementation of caching.

### ``EncoderBackedStore ¶

Bases: `BaseStore[K, V]`

Wraps a store with key and value encoders/decoders.

Examples that uses JSON for encoding/decoding:

```
import json

def key_encoder(key: int) -> str:
    return json.dumps(key)

def value_serializer(value: float) -> str:
    return json.dumps(value)

def value_deserializer(serialized_value: str) -> float:
    return json.loads(serialized_value)

# Create an instance of the abstract store
abstract_store = MyCustomStore()

# Create an instance of the encoder-backed store
store = EncoderBackedStore(
    store=abstract_store,
    key_encoder=key_encoder,
    value_serializer=value_serializer,
    value_deserializer=value_deserializer,
)

# Use the encoder-backed store methods
store.mset([(1, 3.14), (2, 2.718)])
values = store.mget([1, 2])  # Retrieves [3.14, 2.718]
store.mdelete([1, 2])  # Deletes the keys 1 and 2
```

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize an `EncodedStore`. |
| `mget` | Get the values associated with the given keys. |
| `amget` | Async get the values associated with the given keys. |
| `mset` | Set the values for the given keys. |
| `amset` | Async set the values for the given keys. |
| `mdelete` | Delete the given keys and their associated values. |
| `amdelete` | Async delete the given keys and their associated values. |
| `yield_keys` | Get an iterator over keys that match the given prefix. |
| `ayield_keys` | Async get an iterator over keys that match the given prefix. |

#### ``\_\_init\_\_ ¶

```
__init__(
    store: BaseStore[str, Any],
    key_encoder: Callable[[K], str],
    value_serializer: Callable[[V], bytes],
    value_deserializer: Callable[[Any], V],
) -> None
```

Initialize an `EncodedStore`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `store` | The underlying byte store to wrap.<br>**TYPE:**`BaseStore[str, Any]` |
| `key_encoder` | Function to encode keys from type `K` to strings.<br>**TYPE:**`Callable[[K], str]` |
| `value_serializer` | Function to serialize values from type `V` to bytes.<br>**TYPE:**`Callable[[V], bytes]` |
| `value_deserializer` | Function to deserialize bytes back to type V.<br>**TYPE:**`Callable[[Any], V]` |

#### ``mget ¶

```
mget(keys: Sequence[K]) -> list[V | None]
```

Get the values associated with the given keys.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `keys` | A sequence of keys.<br>**TYPE:**`Sequence[K]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[V | None]` | A sequence of optional values associated with the keys. |
| `list[V | None]` | If a key is not found, the corresponding value will be `None`. |

#### ``amget`async`¶

```
amget(keys: Sequence[K]) -> list[V | None]
```

Async get the values associated with the given keys.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `keys` | A sequence of keys.<br>**TYPE:**`Sequence[K]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[V | None]` | A sequence of optional values associated with the keys. |
| `list[V | None]` | If a key is not found, the corresponding value will be `None`. |

#### ``mset ¶

```
mset(key_value_pairs: Sequence[tuple[K, V]]) -> None
```

Set the values for the given keys.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `key_value_pairs` | A sequence of key-value pairs.<br>**TYPE:**`Sequence[tuple[K, V]]` |

#### ``amset`async`¶

```
amset(key_value_pairs: Sequence[tuple[K, V]]) -> None
```

Async set the values for the given keys.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `key_value_pairs` | A sequence of key-value pairs.<br>**TYPE:**`Sequence[tuple[K, V]]` |

#### ``mdelete ¶

```
mdelete(keys: Sequence[K]) -> None
```

Delete the given keys and their associated values.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `keys` | A sequence of keys to delete.<br>**TYPE:**`Sequence[K]` |

#### ``amdelete`async`¶

```
amdelete(keys: Sequence[K]) -> None
```

Async delete the given keys and their associated values.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `keys` | A sequence of keys to delete.<br>**TYPE:**`Sequence[K]` |

#### ``yield\_keys ¶

```
yield_keys(*, prefix: str | None = None) -> Iterator[K] | Iterator[str]
```

Get an iterator over keys that match the given prefix.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `prefix` | The prefix to match.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `Iterator[K] | Iterator[str]` | Keys that match the given prefix. |

#### ``ayield\_keys`async`¶

```
ayield_keys(*, prefix: str | None = None) -> AsyncIterator[K] | AsyncIterator[str]
```

Async get an iterator over keys that match the given prefix.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `prefix` | The prefix to match.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[K] | AsyncIterator[str]` | Keys that match the given prefix. |

### ``LocalFileStore ¶

Bases: `ByteStore`

`BaseStore` interface that works on the local file system.

Examples:

Create a `LocalFileStore` instance and perform operations on it:

```
from langchain_classic.storage import LocalFileStore

# Instantiate the LocalFileStore with the root path
file_store = LocalFileStore("/path/to/root")

# Set values for keys
file_store.mset([("key1", b"value1"), ("key2", b"value2")])

# Get values for keys
values = file_store.mget(["key1", "key2"])  # Returns [b"value1", b"value2"]

# Delete keys
file_store.mdelete(["key1"])

# Iterate over keys
for key in file_store.yield_keys():
    print(key)  # noqa: T201
```

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Implement the `BaseStore` interface for the local file system. |
| `mget` | Get the values associated with the given keys. |
| `mset` | Set the values for the given keys. |
| `mdelete` | Delete the given keys and their associated values. |
| `yield_keys` | Get an iterator over keys that match the given prefix. |

#### ``\_\_init\_\_ ¶

```
__init__(
    root_path: str | Path,
    *,
    chmod_file: int | None = None,
    chmod_dir: int | None = None,
    update_atime: bool = False,
) -> None
```

Implement the `BaseStore` interface for the local file system.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `root_path` | The root path of the file store. All keys are interpreted as<br>paths relative to this root.<br>**TYPE:**`str | Path` |
| `chmod_file` | Sets permissions for newly created files, overriding the<br>current `umask` if needed.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `chmod_dir` | Sets permissions for newly created dirs, overriding the<br>current `umask` if needed.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `update_atime` | Updates the filesystem access time (but not the modified<br>time) when a file is read. This allows MRU/LRU cache policies to be<br>implemented for filesystems where access time updates are disabled.<br>**TYPE:**`bool`**DEFAULT:**`False` |

#### ``mget ¶

```
mget(keys: Sequence[str]) -> list[bytes | None]
```

Get the values associated with the given keys.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `keys` | A sequence of keys.<br>**TYPE:**`Sequence[str]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[bytes | None]` | A sequence of optional values associated with the keys. |
| `list[bytes | None]` | If a key is not found, the corresponding value will be `None`. |

#### ``mset ¶

```
mset(key_value_pairs: Sequence[tuple[str, bytes]]) -> None
```

Set the values for the given keys.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `key_value_pairs` | A sequence of key-value pairs.<br>**TYPE:**`Sequence[tuple[str, bytes]]` |

#### ``mdelete ¶

```
mdelete(keys: Sequence[str]) -> None
```

Delete the given keys and their associated values.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `keys` | A sequence of keys to delete.<br>**TYPE:**`Sequence[str]` |

#### ``yield\_keys ¶

```
yield_keys(prefix: str | None = None) -> Iterator[str]
```

Get an iterator over keys that match the given prefix.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `prefix` | The prefix to match.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `str` | Keys that match the given prefix. |

Back to top