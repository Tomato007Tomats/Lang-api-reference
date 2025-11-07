<!-- URL: page_61 -->

Skip to content

Edit this page

# Tool unit tests ¶

## ``langchain\_tests.unit\_tests.ToolsUnitTests ¶

Bases: `ToolsTests`

Base class for tools unit tests.

| METHOD | DESCRIPTION |
| --- | --- |
| `test_no_overrides_DO_NOT_OVERRIDE` | Test that no standard tests are overridden. |
| `tool` | Tool fixture. |
| `test_init` | Test init. |
| `test_init_from_env` | Test that the tool can be initialized from environment variables. |
| `test_has_name` | Tests that the tool has a name attribute to pass to chat models. |
| `test_has_input_schema` | Tests that the tool has an input schema. |
| `test_input_schema_matches_invoke_params` | Tests that the provided example params match the declared input schema. |

### ``tool\_constructor`abstractmethod``property`¶

```
tool_constructor: type[BaseTool] | BaseTool
```

Returns a class or instance of a tool to be tested.

### ``tool\_constructor\_params`property`¶

```
tool_constructor_params: dict
```

Returns a dictionary of parameters to pass to the tool constructor.

### ``tool\_invoke\_params\_example`property`¶

```
tool_invoke_params_example: dict
```

Returns a dictionary representing the "args" of an example tool call.

This should NOT be a `ToolCall` dict - it should not have
`{"name", "id", "args"}` keys.

### ``init\_from\_env\_params`property`¶

```
init_from_env_params: tuple[dict, dict, dict]
```

Init from env params.

Return env vars, init args, and expected instance attrs for initializing
from env vars.

### ``test\_no\_overrides\_DO\_NOT\_OVERRIDE ¶

```
test_no_overrides_DO_NOT_OVERRIDE() -> None
```

Test that no standard tests are overridden.

### ``tool ¶

```
tool() -> BaseTool
```

Tool fixture.

### ``test\_init ¶

```
test_init() -> None
```

Test init.

Test that the tool can be initialized with `tool_constructor` and
`tool_constructor_params`. If this fails, check that the
keyword args defined in `tool_constructor_params` are valid.

### ``test\_init\_from\_env ¶

```
test_init_from_env() -> None
```

Test that the tool can be initialized from environment variables.

### ``test\_has\_name ¶

```
test_has_name(tool: BaseTool) -> None
```

Tests that the tool has a name attribute to pass to chat models.

If this fails, add a `name` parameter to your tool.

### ``test\_has\_input\_schema ¶

```
test_has_input_schema(tool: BaseTool) -> None
```

Tests that the tool has an input schema.

If this fails, add an `args_schema` to your tool.

See
this guide
and see how `CalculatorInput` is configured in the
`CustomCalculatorTool.args_schema` attribute

### ``test\_input\_schema\_matches\_invoke\_params ¶

```
test_input_schema_matches_invoke_params(tool: BaseTool) -> None
```

Tests that the provided example params match the declared input schema.

If this fails, update the `tool_invoke_params_example` attribute to match
the input schema (`args_schema`) of the tool.

Back to top