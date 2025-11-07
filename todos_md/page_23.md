<!-- URL: page_23 -->

Skip to content

Edit this page

# Pytest Plugin

## ``langsmith.pytest\_plugin ¶

LangSmith Pytest hooks.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `pytest_addoption` | Set a boolean flag for LangSmith output. |
| `pytest_cmdline_preparse` | Call immediately after command line options are parsed (pytest v7). |
| `pytest_load_initial_conftests` | Handle args in pytest v8+. |
| `pytest_runtest_call` | Apply LangSmith tracking to tests marked with @pytest.mark.langsmith. |
| `pytest_report_teststatus` | Remove the short test-status character outputs ("./F"). |
| `pytest_configure` | Register the 'langsmith' marker. |

### ``LangSmithPlugin ¶

Plugin for rendering LangSmith results.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize. |
| `pytest_collection_finish` | Call after collection phase is completed and session.items is populated. |
| `add_process_to_test_suite` | Group a test case with its test suite. |
| `update_process_status` | Update test results. |
| `pytest_runtest_logstart` | Initialize live display when first test starts. |
| `generate_tables` | Generate a collection of tables—one per suite. |
| `pytest_configure` | Disable warning reporting and show no warnings in output. |
| `pytest_sessionfinish` | Stop Rich Live rendering at the end of the session. |

#### ``\_\_init\_\_ ¶

```
__init__()
```

Initialize.

#### ``pytest\_collection\_finish ¶

```
pytest_collection_finish(session)
```

Call after collection phase is completed and session.items is populated.

#### ``add\_process\_to\_test\_suite ¶

```
add_process_to_test_suite(test_suite, process_id)
```

Group a test case with its test suite.

#### ``update\_process\_status ¶

```
update_process_status(process_id, status)
```

Update test results.

#### ``pytest\_runtest\_logstart ¶

```
pytest_runtest_logstart(nodeid)
```

Initialize live display when first test starts.

#### ``generate\_tables ¶

```
generate_tables()
```

Generate a collection of tables—one per suite.

Returns a 'Group' object so it can be rendered simultaneously by Rich Live.

#### ``pytest\_configure ¶

```
pytest_configure(config)
```

Disable warning reporting and show no warnings in output.

#### ``pytest\_sessionfinish ¶

```
pytest_sessionfinish(session)
```

Stop Rich Live rendering at the end of the session.

### ``pytest\_addoption ¶

```
pytest_addoption(parser)
```

Set a boolean flag for LangSmith output.

Skip if --langsmith-output is already defined.

### ``pytest\_cmdline\_preparse ¶

```
pytest_cmdline_preparse(config, args)
```

Call immediately after command line options are parsed (pytest v7).

### ``pytest\_load\_initial\_conftests ¶

```
pytest_load_initial_conftests(args)
```

Handle args in pytest v8+.

### ``pytest\_runtest\_call ¶

```
pytest_runtest_call(item)
```

Apply LangSmith tracking to tests marked with @pytest.mark.langsmith.

### ``pytest\_report\_teststatus ¶

```
pytest_report_teststatus(report, config)
```

Remove the short test-status character outputs ("./F").

### ``pytest\_configure ¶

```
pytest_configure(config)
```

Register the 'langsmith' marker.

Back to top