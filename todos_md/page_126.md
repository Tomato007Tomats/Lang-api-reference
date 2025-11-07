<!-- URL: page_126 -->

Skip to content

Edit this page

# LangSmith SDK reference

![PyPI - Version](https://pypi.org/project/langsmith/#history)![PyPI - License](https://opensource.org/licenses/MIT)![PyPI - Downloads](https://pypistats.org/packages/langsmith)

Welcome to the LangSmith Python SDK reference docs! These pages detail the core interfaces you will use when building with LangSmith's Observability and Evaluations tools.

For user guides, tutorials, and conceptual overviews, please visit the LangSmith documentation.

Work in progress

This page is a work in progress, and we appreciate your patience as we continue to expand and improve the content.

## Quick Reference ¶

| Class/function | Description |
| --- | --- |
| `Client` | Synchronous client for interacting with the LangSmith API. |
| `AsyncClient` | Asynchronous client for interacting with the LangSmith API. |
| `traceable` | Wrapper/decorator for tracing any function. |
| `@pytest.mark.langsmith` | LangSmith `pytest` integration. |
| `wrap_openai` | Wrapper for OpenAI client, adds LangSmith tracing. |
| `wrap_anthropic` | Wrapper for Anthropic client, adds LangSmith tracing. |

## Core APIs ¶

The primary interfaces for the LangSmith SDK.

- `Client`: Synchronous client for the LangSmith API.
- `AsyncClient`: Asynchronous client for the LangSmith API.
- Run Helpers: Functions like `traceable`, `trace`, and tracing context management.
- Run Trees: Tree structure for representing runs and nested runs.
- Evaluation: Tools for evaluating functions and models on datasets.

## Additional APIs ¶

- Schemas: Data schemas and type definitions.
- Utilities: Utility classes including error types and thread pool executors.
- Wrappers: Tracing wrappers for popular LLM providers.
- Anonymizer: Tools for anonymizing sensitive data.
- Testing: Testing utilities and pytest integration.
- Expect API: Assertions and expectations for testing.

Back to top