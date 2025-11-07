<!-- URL: page_104 -->

Skip to content

Edit this page

# Middleware

## ``langsmith.middleware ¶

Middleware for making it easier to do distributed tracing.

### ``TracingMiddleware ¶

Middleware for propagating distributed tracing context using LangSmith.

This middleware checks for the 'langsmith-trace' header and propagates the
tracing context if present. It does not start new traces by default.
It is designed to work with ASGI applications.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `app` | The ASGI application being wrapped. |

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize the middleware. |
| `__call__` | Handle incoming requests and propagate tracing context if applicable. |

#### ``\_\_init\_\_ ¶

```
__init__(app)
```

Initialize the middleware.

#### ``\_\_call\_\_`async`¶

```
__call__(scope: dict, receive, send)
```

Handle incoming requests and propagate tracing context if applicable.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `scope` | A dict containing ASGI connection scope.<br>**TYPE:**`dict` |
| `receive` | An awaitable callable for receiving ASGI events. |
| `send` | An awaitable callable for sending ASGI events. |

If the request is HTTP and contains the 'langsmith-trace' header,
it propagates the tracing context before calling the wrapped application.
Otherwise, it calls the application directly without modifying the context.

Back to top