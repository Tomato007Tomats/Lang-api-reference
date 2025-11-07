<!-- URL: page_27 -->

Skip to content

Edit this page

# Anonymizer ¶

## ``langsmith.anonymizer ¶

| FUNCTION | DESCRIPTION |
| --- | --- |
| `create_anonymizer` | Create an anonymizer function. |

### ``StringNode ¶

Bases: `TypedDict`

String node extracted from the data.

#### ``value`instance-attribute`¶

```
value: str
```

String value.

#### ``path`instance-attribute`¶

```
path: list[str | int]
```

Path to the string node in the data.

### ``StringNodeProcessor ¶

Processes a list of string nodes for masking.

| METHOD | DESCRIPTION |
| --- | --- |
| `mask_nodes` | Accept and return a list of string nodes to be masked. |

#### ``mask\_nodes`abstractmethod`¶

```
mask_nodes(nodes: list[StringNode]) -> list[StringNode]
```

Accept and return a list of string nodes to be masked.

### ``ReplacerOptions ¶

Bases: `TypedDict`

Configuration options for replacing sensitive data.

#### ``max\_depth`instance-attribute`¶

```
max_depth: int | None
```

Maximum depth to traverse to to extract string nodes.

#### ``deep\_clone`instance-attribute`¶

```
deep_clone: bool | None
```

Deep clone the data before replacing.

### ``StringNodeRule ¶

Bases: `TypedDict`

Declarative rule used for replacing sensitive data.

#### ``pattern`instance-attribute`¶

```
pattern: Pattern
```

Regex pattern to match.

#### ``replace`instance-attribute`¶

```
replace: str | None
```

Replacement value. Defaults to `[redacted]` if not specified.

### ``RuleNodeProcessor ¶

Bases: `StringNodeProcessor`

String node processor that uses a list of rules to replace sensitive data.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize the processor with a list of rules. |
| `mask_nodes` | Mask nodes using the rules. |

#### ``rules`instance-attribute`¶

```
rules: list[StringNodeRule] = [\
    {\
        "pattern": rule["pattern"]\
        if isinstance(rule["pattern"], Pattern)\
        else compile(rule["pattern"]),\
        "replace": rule["replace"] if isinstance(get("replace"), str) else "[redacted]",\
    }\
    for rule in rules\
]
```

List of rules to apply for replacing sensitive data.

Each rule is a StringNodeRule, which contains a regex pattern to match
and an optional replacement string.

#### ``\_\_init\_\_ ¶

```
__init__(rules: list[StringNodeRule])
```

Initialize the processor with a list of rules.

#### ``mask\_nodes ¶

```
mask_nodes(nodes: list[StringNode]) -> list[StringNode]
```

Mask nodes using the rules.

### ``CallableNodeProcessor ¶

Bases: `StringNodeProcessor`

String node processor that uses a callable function to replace sensitive data.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize the processor with a callable function. |
| `mask_nodes` | Mask nodes using the callable function. |

#### ``func`instance-attribute`¶

```
func: Callable[[str], str] | Callable[[str, list[Union[str, int]]], str] = func
```

The callable function used to replace sensitive data.

It can be either a function that takes a single string argument and returns a string,
or a function that takes a string and a list of path elements (strings or integers)
and returns a string.

#### ``accepts\_path`instance-attribute`¶

```
accepts_path: bool = len(parameters) == 2
```

Indicates whether the callable function accepts a path argument.

If True, the function expects two arguments: the string to be processed and the path to that string.
If False, the function expects only the string to be processed.

#### ``\_\_init\_\_ ¶

```
__init__(func: Callable[[str], str] | Callable[[str, list[Union[str, int]]], str])
```

Initialize the processor with a callable function.

#### ``mask\_nodes ¶

```
mask_nodes(nodes: list[StringNode]) -> list[StringNode]
```

Mask nodes using the callable function.

### ``create\_anonymizer ¶

```
create_anonymizer(
    replacer: ReplacerType, *, max_depth: int | None = None
) -> Callable[[Any], Any]
```

Create an anonymizer function.

Back to top