<!-- URL: page_78 -->

Skip to content

Edit this page

# Serialization

## ``langchain\_core.load.dump.dumpd ¶

```
dumpd(obj: Any) -> Any
```

Return a dict representation of an object.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `obj` | The object to dump.<br>**TYPE:**`Any` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Any` | Dictionary that can be serialized to json using `json.dumps`. |

## ``langchain\_core.load.dump.dumps ¶

```
dumps(obj: Any, *, pretty: bool = False, **kwargs: Any) -> str
```

Return a JSON string representation of an object.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `obj` | The object to dump.<br>**TYPE:**`Any` |
| `pretty` | Whether to pretty print the json. If `True`, the json will be<br>indented with 2 spaces (if no indent is provided as part of `kwargs`).<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional arguments to pass to `json.dumps`<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | A JSON string representation of the object. |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If `default` is passed as a kwarg. |

## ``langchain\_core.load.load.load ¶

```
load(
    obj: Any,
    *,
    secrets_map: dict[str, str] | None = None,
    valid_namespaces: list[str] | None = None,
    secrets_from_env: bool = True,
    additional_import_mappings: dict[tuple[str, ...], tuple[str, ...]] | None = None,
    ignore_unserializable_fields: bool = False,
) -> Any
```

Revive a LangChain class from a JSON object.

Use this if you already have a parsed JSON object,
eg. from `json.load` or `orjson.loads`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `obj` | The object to load.<br>**TYPE:**`Any` |
| `secrets_map` | A map of secrets to load. If a secret is not found in<br>the map, it will be loaded from the environment if `secrets_from_env`<br>is True.<br>**TYPE:**`dict[str, str] | None`**DEFAULT:**`None` |
| `valid_namespaces` | A list of additional namespaces (modules)<br>to allow to be deserialized.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `secrets_from_env` | Whether to load secrets from the environment.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `additional_import_mappings` | A dictionary of additional namespace mappings<br>You can use this to override default mappings or add new mappings.<br>**TYPE:**`dict[tuple[str, ...], tuple[str, ...]] | None`**DEFAULT:**`None` |
| `ignore_unserializable_fields` | Whether to ignore unserializable fields.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Any` | Revived LangChain objects. |

## ``langchain\_core.load.load.loads ¶

```
loads(
    text: str,
    *,
    secrets_map: dict[str, str] | None = None,
    valid_namespaces: list[str] | None = None,
    secrets_from_env: bool = True,
    additional_import_mappings: dict[tuple[str, ...], tuple[str, ...]] | None = None,
    ignore_unserializable_fields: bool = False,
) -> Any
```

Revive a LangChain class from a JSON string.

Equivalent to `load(json.loads(text))`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `text` | The string to load.<br>**TYPE:**`str` |
| `secrets_map` | A map of secrets to load. If a secret is not found in<br>the map, it will be loaded from the environment if `secrets_from_env`<br>is True.<br>**TYPE:**`dict[str, str] | None`**DEFAULT:**`None` |
| `valid_namespaces` | A list of additional namespaces (modules)<br>to allow to be deserialized.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `secrets_from_env` | Whether to load secrets from the environment.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `additional_import_mappings` | A dictionary of additional namespace mappings<br>You can use this to override default mappings or add new mappings.<br>**TYPE:**`dict[tuple[str, ...], tuple[str, ...]] | None`**DEFAULT:**`None` |
| `ignore_unserializable_fields` | Whether to ignore unserializable fields.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Any` | Revived LangChain objects. |

## ``langchain\_core.load.serializable.Serializable ¶

Bases: `BaseModel`, `ABC`

Serializable base class.

This class is used to serialize objects to JSON.

It relies on the following methods and properties:

- `is_lc_serializable`: Is this class serializable?
By design, even if a class inherits from `Serializable`, it is not serializable
by default. This is to prevent accidental serialization of objects that should
not be serialized.
- `get_lc_namespace`: Get the namespace of the LangChain object.

During deserialization, this namespace is used to identify
the correct class to instantiate.

Please see the `Reviver` class in `langchain_core.load.load` for more details.
During deserialization an additional mapping is handle classes that have moved
or been renamed across package versions.

- `lc_secrets`: A map of constructor argument names to secret ids.

- `lc_attributes`: List of additional attribute names that should be included
as part of the serialized representation.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` |  |
| `is_lc_serializable` | Is this class serializable? |
| `get_lc_namespace` | Get the namespace of the LangChain object. |
| `lc_id` | Return a unique identifier for this class for serialization purposes. |
| `to_json` | Serialize the object to JSON. |
| `to_json_not_implemented` | Serialize a "not implemented" object. |

### ``lc\_secrets`property`¶

```
lc_secrets: dict[str, str]
```

A map of constructor argument names to secret ids.

For example, `{"openai_api_key": "OPENAI_API_KEY"}`

### ``lc\_attributes`property`¶

```
lc_attributes: dict
```

List of attribute names that should be included in the serialized kwargs.

These attributes must be accepted by the constructor.

Default is an empty dictionary.

### ``\_\_init\_\_ ¶

```
__init__(*args: Any, **kwargs: Any) -> None
```

### ``is\_lc\_serializable`classmethod`¶

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

### ``get\_lc\_namespace`classmethod`¶

```
get_lc_namespace() -> list[str]
```

Get the namespace of the LangChain object.

For example, if the class is `langchain.llms.openai.OpenAI`, then the
namespace is `["langchain", "llms", "openai"]`

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[str]` | The namespace. |

### ``lc\_id`classmethod`¶

```
lc_id() -> list[str]
```

Return a unique identifier for this class for serialization purposes.

The unique identifier is a list of strings that describes the path
to the object.

For example, for the class `langchain.llms.openai.OpenAI`, the id is
`["langchain", "llms", "openai", "OpenAI"]`.

### ``to\_json ¶

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

### ``to\_json\_not\_implemented ¶

```
to_json_not_implemented() -> SerializedNotImplemented
```

Serialize a "not implemented" object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedNotImplemented` | `SerializedNotImplemented`. |

Back to top