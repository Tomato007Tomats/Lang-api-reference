<!-- URL: page_135 -->

Skip to content

Edit this page

# Schemas

## ``langsmith.schemas ¶

Schemas for the LangSmith API.

### ``Attachments`module-attribute`¶

```
Attachments = dict[str, tuple[str, bytes] | Attachment | tuple[str, Path]]
```

Attachments associated with the run.
Each entry is a tuple of (mime\_type, bytes), or (mime\_type, file\_path)

### ``Attachment ¶

Bases: `NamedTuple`

Annotated type that will be stored as an attachment if used.

Examples:

```
.. code-block:: python

    from langsmith import traceable
    from langsmith.schemas import Attachment

    @traceable
    def my_function(bar: int, my_val: Attachment):
        # my_val will be stored as an attachment
        # bar will be stored as inputs
        return bar
```

### ``BinaryIOLike ¶

Bases: `Protocol`

Protocol for binary IO-like objects.

| METHOD | DESCRIPTION |
| --- | --- |
| `read` | Read function. |
| `seek` | Seek function. |
| `getvalue` | Get value function. |

#### ``read ¶

```
read(size: int = -1) -> bytes
```

Read function.

#### ``seek ¶

```
seek(offset: int, whence: int = 0) -> int
```

Seek function.

#### ``getvalue ¶

```
getvalue() -> bytes
```

Get value function.

### ``ExampleBase ¶

Bases: `BaseModel`

Example base model.

#### ``Config ¶

Configuration class for the schema.

### ``ExampleCreate ¶

Bases: `BaseModel`

Example upload with attachments.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize from dict. |

#### ``\_\_init\_\_ ¶

```
__init__(**data)
```

Initialize from dict.

### ``ExampleUpsertWithAttachments ¶

Bases: `ExampleCreate`

Example create with attachments.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize from dict. |

#### ``\_\_init\_\_ ¶

```
__init__(**data)
```

Initialize from dict.

### ``AttachmentInfo ¶

Bases: `TypedDict`

Info for an attachment.

### ``Example ¶

Bases: `ExampleBase`

Example model.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize a Dataset object. |
| `__repr__` | Return a string representation of the RunBase object. |

#### ``attachments`class-attribute``instance-attribute`¶

```
attachments: dict[str, AttachmentInfo] | None = Field(default=None)
```

Dictionary with attachment names as keys and a tuple of the S3 url
and a reader of the data for the file.

#### ``url`property`¶

```
url: str | None
```

URL of this run within the app.

#### ``Config ¶

Configuration class for the schema.

#### ``\_\_init\_\_ ¶

```
__init__(
    _host_url: str | None = None, _tenant_id: UUID | None = None, **kwargs: Any
) -> None
```

Initialize a Dataset object.

#### ``\_\_repr\_\_ ¶

```
__repr__()
```

Return a string representation of the RunBase object.

### ``ExampleSearch ¶

Bases: `ExampleBase`

Example returned via search.

#### ``Config ¶

Configuration class for the schema.

### ``AttachmentsOperations ¶

Bases: `BaseModel`

Operations to perform on attachments.

### ``ExampleUpdate ¶

Bases: `BaseModel`

Example update with attachments.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize from dict. |

#### ``Config ¶

Configuration class for the schema.

#### ``\_\_init\_\_ ¶

```
__init__(**data)
```

Initialize from dict.

### ``DataType ¶

Bases: `str`, `Enum`

Enum for dataset data types.

### ``DatasetBase ¶

Bases: `BaseModel`

Dataset base model.

#### ``Config ¶

Configuration class for the schema.

### ``DatasetTransformation ¶

Bases: `TypedDict`

Schema for dataset transformations.

### ``Dataset ¶

Bases: `DatasetBase`

Dataset ORM model.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize a Dataset object. |

#### ``url`property`¶

```
url: str | None
```

URL of this run within the app.

#### ``Config ¶

Configuration class for the schema.

#### ``\_\_init\_\_ ¶

```
__init__(
    _host_url: str | None = None,
    _tenant_id: UUID | None = None,
    _public_path: str | None = None,
    **kwargs: Any,
) -> None
```

Initialize a Dataset object.

### ``DatasetVersion ¶

Bases: `BaseModel`

Class representing a dataset version.

### ``RunBase ¶

Bases: `BaseModel`

Base Run schema.

A Run is a span representing a single unit of work or operation within your LLM app.
This could be a single call to an LLM or chain, to a prompt formatting call,
to a runnable lambda invocation. If you are familiar with OpenTelemetry,
you can think of a run as a span.

| METHOD | DESCRIPTION |
| --- | --- |
| `__repr__` | Return a string representation of the RunBase object. |

#### ``id`instance-attribute`¶

```
id: UUID
```

Unique identifier for the run.

#### ``name`instance-attribute`¶

```
name: str
```

Human-readable name for the run.

#### ``start\_time`instance-attribute`¶

```
start_time: datetime
```

Start time of the run.

#### ``run\_type`instance-attribute`¶

```
run_type: str
```

The type of run, such as tool, chain, llm, retriever,
embedding, prompt, parser.

#### ``end\_time`class-attribute``instance-attribute`¶

```
end_time: datetime | None = None
```

End time of the run, if applicable.

#### ``extra`class-attribute``instance-attribute`¶

```
extra: dict | None = Field(default_factory=_default_extra)
```

Additional metadata or settings related to the run.

#### ``error`class-attribute``instance-attribute`¶

```
error: str | None = None
```

Error message, if the run encountered any issues.

#### ``serialized`class-attribute``instance-attribute`¶

```
serialized: dict | None = None
```

Serialized object that executed the run for potential reuse.

#### ``events`class-attribute``instance-attribute`¶

```
events: list[dict] | None = None
```

List of events associated with the run, like
start and end events.

#### ``inputs`class-attribute``instance-attribute`¶

```
inputs: dict = Field(default_factory=dict)
```

Inputs used for the run.

#### ``outputs`class-attribute``instance-attribute`¶

```
outputs: dict | None = None
```

Outputs generated by the run, if any.

#### ``reference\_example\_id`class-attribute``instance-attribute`¶

```
reference_example_id: UUID | None = None
```

Reference to an example that this run may be based on.

#### ``parent\_run\_id`class-attribute``instance-attribute`¶

```
parent_run_id: UUID | None = None
```

Identifier for a parent run, if this run is a sub-run.

#### ``tags`class-attribute``instance-attribute`¶

```
tags: list[str] | None = None
```

Tags for categorizing or annotating the run.

#### ``attachments`class-attribute``instance-attribute`¶

```
attachments: Attachments | dict[str, AttachmentInfo] = Field(default_factory=dict)
```

Attachments associated with the run.
Each entry is a tuple of (mime\_type, bytes).

#### ``metadata`property`¶

```
metadata: dict[str, Any]
```

Retrieve the metadata (if any).

#### ``revision\_id`property`¶

```
revision_id: UUID | None
```

Retrieve the revision ID (if any).

#### ``latency`property`¶

```
latency: float | None
```

Latency in seconds.

#### ``Config ¶

Configuration class for the schema.

#### ``\_\_repr\_\_ ¶

```
__repr__()
```

Return a string representation of the RunBase object.

### ``Run ¶

Bases: `RunBase`

Run schema when loading from the DB.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize a Run object. |
| `__repr__` | Return a string representation of the RunBase object. |

#### ``session\_id`class-attribute``instance-attribute`¶

```
session_id: UUID | None = None
```

The project ID this run belongs to.

#### ``child\_run\_ids`class-attribute``instance-attribute`¶

```
child_run_ids: list[UUID] | None = None
```

Deprecated: The child run IDs of this run.

#### ``child\_runs`class-attribute``instance-attribute`¶

```
child_runs: list[Run] | None = None
```

The child runs of this run, if instructed to load using the client
These are not populated by default, as it is a heavier query to make.

#### ``feedback\_stats`class-attribute``instance-attribute`¶

```
feedback_stats: dict[str, Any] | None = None
```

Feedback stats for this run.

#### ``app\_path`class-attribute``instance-attribute`¶

```
app_path: str | None = None
```

Relative URL path of this run within the app.

#### ``manifest\_id`class-attribute``instance-attribute`¶

```
manifest_id: UUID | None = None
```

Unique ID of the serialized object for this run.

#### ``status`class-attribute``instance-attribute`¶

```
status: str | None = None
```

Status of the run (e.g., 'success').

#### ``prompt\_tokens`class-attribute``instance-attribute`¶

```
prompt_tokens: int | None = None
```

Number of tokens used for the prompt.

#### ``completion\_tokens`class-attribute``instance-attribute`¶

```
completion_tokens: int | None = None
```

Number of tokens generated as output.

#### ``total\_tokens`class-attribute``instance-attribute`¶

```
total_tokens: int | None = None
```

Total tokens for prompt and completion.

#### ``prompt\_token\_details`class-attribute``instance-attribute`¶

```
prompt_token_details: dict[str, int] | None = None
```

Breakdown of prompt (input) token counts.

Does _not_ need to sum to full prompt token count.

#### ``completion\_token\_details`class-attribute``instance-attribute`¶

```
completion_token_details: dict[str, int] | None = None
```

Breakdown of completion (output) token counts.

Does _not_ need to sum to full completion token count.

#### ``first\_token\_time`class-attribute``instance-attribute`¶

```
first_token_time: datetime | None = None
```

Time the first token was processed.

#### ``total\_cost`class-attribute``instance-attribute`¶

```
total_cost: Decimal | None = None
```

The total estimated LLM cost associated with the completion tokens.

#### ``prompt\_cost`class-attribute``instance-attribute`¶

```
prompt_cost: Decimal | None = None
```

The estimated cost associated with the prompt (input) tokens.

#### ``completion\_cost`class-attribute``instance-attribute`¶

```
completion_cost: Decimal | None = None
```

The estimated cost associated with the completion tokens.

#### ``prompt\_cost\_details`class-attribute``instance-attribute`¶

```
prompt_cost_details: dict[str, Decimal] | None = None
```

Breakdown of prompt (input) token costs.

Does _not_ need to sum to full prompt token cost.

#### ``completion\_cost\_details`class-attribute``instance-attribute`¶

```
completion_cost_details: dict[str, Decimal] | None = None
```

Breakdown of completion (output) token costs.

Does _not_ need to sum to full completion token cost.

#### ``parent\_run\_ids`class-attribute``instance-attribute`¶

```
parent_run_ids: list[UUID] | None = None
```

List of parent run IDs.

#### ``trace\_id`instance-attribute`¶

```
trace_id: UUID
```

Unique ID assigned to every run within this nested trace.

#### ``dotted\_order`class-attribute``instance-attribute`¶

```
dotted_order: str = Field(default='')
```

Dotted order for the run.

This is a string composed of {time}{run-uuid}.\* so that a trace can be
sorted in the order it was executed.

Example

- Parent: 20230914T223155647Z1b64098b-4ab7-43f6-afee-992304f198d8
- Children:
- 20230914T223155647Z1b64098b-4ab7-43f6-afee-992304f198d8.20230914T223155649Z809ed3a2-0172-4f4d-8a02-a64e9b7a0f8a
- 20230915T223155647Z1b64098b-4ab7-43f6-afee-992304f198d8.20230914T223155650Zc8d9f4c5-6c5a-4b2d-9b1c-3d9d7a7c5c7c

#### ``in\_dataset`class-attribute``instance-attribute`¶

```
in_dataset: bool | None = None
```

Whether this run is in a dataset.

#### ``url`property`¶

```
url: str | None
```

URL of this run within the app.

#### ``input\_tokens`property`¶

```
input_tokens: int | None
```

Alias for prompt\_tokens.

#### ``output\_tokens`property`¶

```
output_tokens: int | None
```

Alias for completion\_tokens.

#### ``input\_cost`property`¶

```
input_cost: Decimal | None
```

Alias for prompt\_cost.

#### ``output\_cost`property`¶

```
output_cost: Decimal | None
```

Alias for completion\_cost.

#### ``input\_token\_details`property`¶

```
input_token_details: dict[str, int] | None
```

Alias for prompt\_token\_details.

#### ``output\_token\_details`property`¶

```
output_token_details: dict[str, int] | None
```

Alias for output\_token\_details.

#### ``input\_cost\_details`property`¶

```
input_cost_details: dict[str, Decimal] | None
```

Alias for prompt\_cost\_details.

#### ``output\_cost\_details`property`¶

```
output_cost_details: dict[str, Decimal] | None
```

Alias for completion\_cost\_details.

#### ``id`instance-attribute`¶

```
id: UUID
```

Unique identifier for the run.

#### ``name`instance-attribute`¶

```
name: str
```

Human-readable name for the run.

#### ``start\_time`instance-attribute`¶

```
start_time: datetime
```

Start time of the run.

#### ``run\_type`instance-attribute`¶

```
run_type: str
```

The type of run, such as tool, chain, llm, retriever,
embedding, prompt, parser.

#### ``end\_time`class-attribute``instance-attribute`¶

```
end_time: datetime | None = None
```

End time of the run, if applicable.

#### ``extra`class-attribute``instance-attribute`¶

```
extra: dict | None = Field(default_factory=_default_extra)
```

Additional metadata or settings related to the run.

#### ``error`class-attribute``instance-attribute`¶

```
error: str | None = None
```

Error message, if the run encountered any issues.

#### ``serialized`class-attribute``instance-attribute`¶

```
serialized: dict | None = None
```

Serialized object that executed the run for potential reuse.

#### ``events`class-attribute``instance-attribute`¶

```
events: list[dict] | None = None
```

List of events associated with the run, like
start and end events.

#### ``inputs`class-attribute``instance-attribute`¶

```
inputs: dict = Field(default_factory=dict)
```

Inputs used for the run.

#### ``outputs`class-attribute``instance-attribute`¶

```
outputs: dict | None = None
```

Outputs generated by the run, if any.

#### ``reference\_example\_id`class-attribute``instance-attribute`¶

```
reference_example_id: UUID | None = None
```

Reference to an example that this run may be based on.

#### ``parent\_run\_id`class-attribute``instance-attribute`¶

```
parent_run_id: UUID | None = None
```

Identifier for a parent run, if this run is a sub-run.

#### ``tags`class-attribute``instance-attribute`¶

```
tags: list[str] | None = None
```

Tags for categorizing or annotating the run.

#### ``attachments`class-attribute``instance-attribute`¶

```
attachments: Attachments | dict[str, AttachmentInfo] = Field(default_factory=dict)
```

Attachments associated with the run.
Each entry is a tuple of (mime\_type, bytes).

#### ``metadata`property`¶

```
metadata: dict[str, Any]
```

Retrieve the metadata (if any).

#### ``revision\_id`property`¶

```
revision_id: UUID | None
```

Retrieve the revision ID (if any).

#### ``latency`property`¶

```
latency: float | None
```

Latency in seconds.

#### ``Config ¶

Configuration class for the schema.

#### ``\_\_init\_\_ ¶

```
__init__(_host_url: str | None = None, **kwargs: Any) -> None
```

Initialize a Run object.

#### ``\_\_repr\_\_ ¶

```
__repr__()
```

Return a string representation of the RunBase object.

### ``RunTypeEnum ¶

Bases: `str`, `Enum`

(Deprecated) Enum for run types. Use string directly.

### ``RunLikeDict ¶

Bases: `TypedDict`

Run-like dictionary, for type-hinting.

### ``RunWithAnnotationQueueInfo ¶

Bases: `RunBase`

Run schema with annotation queue info.

| METHOD | DESCRIPTION |
| --- | --- |
| `__repr__` | Return a string representation of the RunBase object. |

#### ``last\_reviewed\_time`class-attribute``instance-attribute`¶

```
last_reviewed_time: datetime | None = None
```

The last time this run was reviewed.

#### ``added\_at`class-attribute``instance-attribute`¶

```
added_at: datetime | None = None
```

The time this run was added to the queue.

#### ``id`instance-attribute`¶

```
id: UUID
```

Unique identifier for the run.

#### ``name`instance-attribute`¶

```
name: str
```

Human-readable name for the run.

#### ``start\_time`instance-attribute`¶

```
start_time: datetime
```

Start time of the run.

#### ``run\_type`instance-attribute`¶

```
run_type: str
```

The type of run, such as tool, chain, llm, retriever,
embedding, prompt, parser.

#### ``end\_time`class-attribute``instance-attribute`¶

```
end_time: datetime | None = None
```

End time of the run, if applicable.

#### ``extra`class-attribute``instance-attribute`¶

```
extra: dict | None = Field(default_factory=_default_extra)
```

Additional metadata or settings related to the run.

#### ``error`class-attribute``instance-attribute`¶

```
error: str | None = None
```

Error message, if the run encountered any issues.

#### ``serialized`class-attribute``instance-attribute`¶

```
serialized: dict | None = None
```

Serialized object that executed the run for potential reuse.

#### ``events`class-attribute``instance-attribute`¶

```
events: list[dict] | None = None
```

List of events associated with the run, like
start and end events.

#### ``inputs`class-attribute``instance-attribute`¶

```
inputs: dict = Field(default_factory=dict)
```

Inputs used for the run.

#### ``outputs`class-attribute``instance-attribute`¶

```
outputs: dict | None = None
```

Outputs generated by the run, if any.

#### ``reference\_example\_id`class-attribute``instance-attribute`¶

```
reference_example_id: UUID | None = None
```

Reference to an example that this run may be based on.

#### ``parent\_run\_id`class-attribute``instance-attribute`¶

```
parent_run_id: UUID | None = None
```

Identifier for a parent run, if this run is a sub-run.

#### ``tags`class-attribute``instance-attribute`¶

```
tags: list[str] | None = None
```

Tags for categorizing or annotating the run.

#### ``attachments`class-attribute``instance-attribute`¶

```
attachments: Attachments | dict[str, AttachmentInfo] = Field(default_factory=dict)
```

Attachments associated with the run.
Each entry is a tuple of (mime\_type, bytes).

#### ``metadata`property`¶

```
metadata: dict[str, Any]
```

Retrieve the metadata (if any).

#### ``revision\_id`property`¶

```
revision_id: UUID | None
```

Retrieve the revision ID (if any).

#### ``latency`property`¶

```
latency: float | None
```

Latency in seconds.

#### ``Config ¶

Configuration class for the schema.

#### ``\_\_repr\_\_ ¶

```
__repr__()
```

Return a string representation of the RunBase object.

### ``FeedbackSourceBase ¶

Bases: `BaseModel`

Base class for feedback sources.

This represents whether feedback is submitted from the API, model, human labeler,
etc.

#### ``type`instance-attribute`¶

```
type: str
```

The type of the feedback source.

#### ``metadata`class-attribute``instance-attribute`¶

```
metadata: dict[str, Any] | None = Field(default_factory=dict)
```

Additional metadata for the feedback source.

#### ``user\_id`class-attribute``instance-attribute`¶

```
user_id: UUID | str | None = None
```

The user ID associated with the feedback source.

#### ``user\_name`class-attribute``instance-attribute`¶

```
user_name: str | None = None
```

The user name associated with the feedback source.

### ``APIFeedbackSource ¶

Bases: `FeedbackSourceBase`

API feedback source.

#### ``type`class-attribute``instance-attribute`¶

```
type: Literal['api'] = 'api'
```

The type of the feedback source.

#### ``metadata`class-attribute``instance-attribute`¶

```
metadata: dict[str, Any] | None = Field(default_factory=dict)
```

Additional metadata for the feedback source.

#### ``user\_id`class-attribute``instance-attribute`¶

```
user_id: UUID | str | None = None
```

The user ID associated with the feedback source.

#### ``user\_name`class-attribute``instance-attribute`¶

```
user_name: str | None = None
```

The user name associated with the feedback source.

### ``ModelFeedbackSource ¶

Bases: `FeedbackSourceBase`

Model feedback source.

#### ``type`class-attribute``instance-attribute`¶

```
type: Literal['model'] = 'model'
```

The type of the feedback source.

#### ``metadata`class-attribute``instance-attribute`¶

```
metadata: dict[str, Any] | None = Field(default_factory=dict)
```

Additional metadata for the feedback source.

#### ``user\_id`class-attribute``instance-attribute`¶

```
user_id: UUID | str | None = None
```

The user ID associated with the feedback source.

#### ``user\_name`class-attribute``instance-attribute`¶

```
user_name: str | None = None
```

The user name associated with the feedback source.

### ``FeedbackSourceType ¶

Bases: `Enum`

Feedback source type.

#### ``API`class-attribute``instance-attribute`¶

```
API = 'api'
```

General feedback submitted from the API.

#### ``MODEL`class-attribute``instance-attribute`¶

```
MODEL = 'model'
```

Model-assisted feedback.

### ``FeedbackBase ¶

Bases: `BaseModel`

Feedback schema.

#### ``id`instance-attribute`¶

```
id: UUID
```

The unique ID of the feedback.

#### ``created\_at`class-attribute``instance-attribute`¶

```
created_at: datetime | None = None
```

The time the feedback was created.

#### ``modified\_at`class-attribute``instance-attribute`¶

```
modified_at: datetime | None = None
```

The time the feedback was last modified.

#### ``run\_id`instance-attribute`¶

```
run_id: UUID | None
```

The associated run ID this feedback is logged for.

#### ``trace\_id`instance-attribute`¶

```
trace_id: UUID | None
```

The associated trace ID this feedback is logged for.

#### ``key`instance-attribute`¶

```
key: str
```

The metric name, tag, or aspect to provide feedback on.

#### ``score`class-attribute``instance-attribute`¶

```
score: SCORE_TYPE = None
```

Value or score to assign the run.

#### ``value`class-attribute``instance-attribute`¶

```
value: VALUE_TYPE = None
```

The display value, tag or other value for the feedback if not a metric.

#### ``comment`class-attribute``instance-attribute`¶

```
comment: str | None = None
```

Comment or explanation for the feedback.

#### ``correction`class-attribute``instance-attribute`¶

```
correction: str | dict | None = None
```

Correction for the run.

#### ``feedback\_source`class-attribute``instance-attribute`¶

```
feedback_source: FeedbackSourceBase | None = None
```

The source of the feedback.

#### ``session\_id`class-attribute``instance-attribute`¶

```
session_id: UUID | None = None
```

The associated project ID (Session = Project) this feedback is logged for.

#### ``comparative\_experiment\_id`class-attribute``instance-attribute`¶

```
comparative_experiment_id: UUID | None = None
```

If logged within a 'comparative experiment', this is the ID of the experiment.

#### ``feedback\_group\_id`class-attribute``instance-attribute`¶

```
feedback_group_id: UUID | None = None
```

For preference scoring, this group ID is shared across feedbacks for each

run in the group that was being compared.

#### ``extra`class-attribute``instance-attribute`¶

```
extra: dict | None = None
```

The metadata of the feedback.

#### ``Config ¶

Configuration class for the schema.

### ``FeedbackCategory ¶

Bases: `TypedDict`

Specific value and label pair for feedback.

#### ``value`instance-attribute`¶

```
value: float
```

The numeric value associated with this feedback category.

#### ``label`instance-attribute`¶

```
label: str | None
```

An optional label to interpret the value for this feedback category.

### ``FeedbackConfig ¶

Bases: `TypedDict`

Represents _how_ a feedback value ought to be interpreted.

#### ``type`instance-attribute`¶

```
type: Literal['continuous', 'categorical', 'freeform']
```

The type of feedback.

#### ``min`instance-attribute`¶

```
min: float | None
```

The minimum value for continuous feedback.

#### ``max`instance-attribute`¶

```
max: float | None
```

The maximum value for continuous feedback.

#### ``categories`instance-attribute`¶

```
categories: list[FeedbackCategory] | None
```

If feedback is categorical, this defines the valid categories the server will accept.
Not applicable to continuous or freeform feedback types.

### ``FeedbackCreate ¶

Bases: `FeedbackBase`

Schema used for creating feedback.

#### ``feedback\_source`instance-attribute`¶

```
feedback_source: FeedbackSourceBase
```

The source of the feedback.

#### ``feedback\_config`class-attribute``instance-attribute`¶

```
feedback_config: FeedbackConfig | None = None
```

The config for the feedback

#### ``id`instance-attribute`¶

```
id: UUID
```

The unique ID of the feedback.

#### ``created\_at`class-attribute``instance-attribute`¶

```
created_at: datetime | None = None
```

The time the feedback was created.

#### ``modified\_at`class-attribute``instance-attribute`¶

```
modified_at: datetime | None = None
```

The time the feedback was last modified.

#### ``run\_id`instance-attribute`¶

```
run_id: UUID | None
```

The associated run ID this feedback is logged for.

#### ``trace\_id`instance-attribute`¶

```
trace_id: UUID | None
```

The associated trace ID this feedback is logged for.

#### ``key`instance-attribute`¶

```
key: str
```

The metric name, tag, or aspect to provide feedback on.

#### ``score`class-attribute``instance-attribute`¶

```
score: SCORE_TYPE = None
```

Value or score to assign the run.

#### ``value`class-attribute``instance-attribute`¶

```
value: VALUE_TYPE = None
```

The display value, tag or other value for the feedback if not a metric.

#### ``comment`class-attribute``instance-attribute`¶

```
comment: str | None = None
```

Comment or explanation for the feedback.

#### ``correction`class-attribute``instance-attribute`¶

```
correction: str | dict | None = None
```

Correction for the run.

#### ``session\_id`class-attribute``instance-attribute`¶

```
session_id: UUID | None = None
```

The associated project ID (Session = Project) this feedback is logged for.

#### ``comparative\_experiment\_id`class-attribute``instance-attribute`¶

```
comparative_experiment_id: UUID | None = None
```

If logged within a 'comparative experiment', this is the ID of the experiment.

#### ``feedback\_group\_id`class-attribute``instance-attribute`¶

```
feedback_group_id: UUID | None = None
```

For preference scoring, this group ID is shared across feedbacks for each

run in the group that was being compared.

#### ``extra`class-attribute``instance-attribute`¶

```
extra: dict | None = None
```

The metadata of the feedback.

#### ``Config ¶

Configuration class for the schema.

### ``Feedback ¶

Bases: `FeedbackBase`

Schema for getting feedback.

#### ``id`instance-attribute`¶

```
id: UUID
```

The unique ID of the feedback.

#### ``created\_at`instance-attribute`¶

```
created_at: datetime
```

The time the feedback was created.

#### ``modified\_at`instance-attribute`¶

```
modified_at: datetime
```

The time the feedback was last modified.

#### ``feedback\_source`class-attribute``instance-attribute`¶

```
feedback_source: FeedbackSourceBase | None = None
```

The source of the feedback. In this case

#### ``run\_id`instance-attribute`¶

```
run_id: UUID | None
```

The associated run ID this feedback is logged for.

#### ``trace\_id`instance-attribute`¶

```
trace_id: UUID | None
```

The associated trace ID this feedback is logged for.

#### ``key`instance-attribute`¶

```
key: str
```

The metric name, tag, or aspect to provide feedback on.

#### ``score`class-attribute``instance-attribute`¶

```
score: SCORE_TYPE = None
```

Value or score to assign the run.

#### ``value`class-attribute``instance-attribute`¶

```
value: VALUE_TYPE = None
```

The display value, tag or other value for the feedback if not a metric.

#### ``comment`class-attribute``instance-attribute`¶

```
comment: str | None = None
```

Comment or explanation for the feedback.

#### ``correction`class-attribute``instance-attribute`¶

```
correction: str | dict | None = None
```

Correction for the run.

#### ``session\_id`class-attribute``instance-attribute`¶

```
session_id: UUID | None = None
```

The associated project ID (Session = Project) this feedback is logged for.

#### ``comparative\_experiment\_id`class-attribute``instance-attribute`¶

```
comparative_experiment_id: UUID | None = None
```

If logged within a 'comparative experiment', this is the ID of the experiment.

#### ``feedback\_group\_id`class-attribute``instance-attribute`¶

```
feedback_group_id: UUID | None = None
```

For preference scoring, this group ID is shared across feedbacks for each

run in the group that was being compared.

#### ``extra`class-attribute``instance-attribute`¶

```
extra: dict | None = None
```

The metadata of the feedback.

#### ``Config ¶

Configuration class for the schema.

### ``TracerSession ¶

Bases: `BaseModel`

TracerSession schema for the API.

Sessions are also referred to as "Projects" in the UI.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize a Run object. |

#### ``id`instance-attribute`¶

```
id: UUID
```

The ID of the project.

#### ``start\_time`class-attribute``instance-attribute`¶

```
start_time: datetime = Field(default_factory=lambda: now(utc))
```

The time the project was created.

#### ``end\_time`class-attribute``instance-attribute`¶

```
end_time: datetime | None = None
```

The time the project was ended.

#### ``description`class-attribute``instance-attribute`¶

```
description: str | None = None
```

The description of the project.

#### ``name`class-attribute``instance-attribute`¶

```
name: str | None = None
```

The name of the session.

#### ``extra`class-attribute``instance-attribute`¶

```
extra: dict[str, Any] | None = None
```

Extra metadata for the project.

#### ``tenant\_id`instance-attribute`¶

```
tenant_id: UUID
```

The tenant ID this project belongs to.

#### ``reference\_dataset\_id`instance-attribute`¶

```
reference_dataset_id: UUID | None
```

The reference dataset IDs this project's runs were generated on.

#### ``url`property`¶

```
url: str | None
```

URL of this run within the app.

#### ``metadata`property`¶

```
metadata: dict[str, Any]
```

Retrieve the metadata (if any).

#### ``tags`property`¶

```
tags: list[str]
```

Retrieve the tags (if any).

#### ``\_\_init\_\_ ¶

```
__init__(_host_url: str | None = None, **kwargs: Any) -> None
```

Initialize a Run object.

### ``TracerSessionResult ¶

Bases: `TracerSession`

A project, hydrated with additional information.

Sessions are also referred to as "Projects" in the UI.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize a Run object. |

#### ``run\_count`instance-attribute`¶

```
run_count: int | None
```

The number of runs in the project.

#### ``latency\_p50`instance-attribute`¶

```
latency_p50: timedelta | None
```

The median (50th percentile) latency for the project.

#### ``latency\_p99`instance-attribute`¶

```
latency_p99: timedelta | None
```

The 99th percentile latency for the project.

#### ``total\_tokens`instance-attribute`¶

```
total_tokens: int | None
```

The total number of tokens consumed in the project.

#### ``prompt\_tokens`instance-attribute`¶

```
prompt_tokens: int | None
```

The total number of prompt tokens consumed in the project.

#### ``completion\_tokens`instance-attribute`¶

```
completion_tokens: int | None
```

The total number of completion tokens consumed in the project.

#### ``last\_run\_start\_time`instance-attribute`¶

```
last_run_start_time: datetime | None
```

The start time of the last run in the project.

#### ``feedback\_stats`instance-attribute`¶

```
feedback_stats: dict[str, Any] | None
```

Feedback stats for the project.

#### ``session\_feedback\_stats`instance-attribute`¶

```
session_feedback_stats: dict[str, Any] | None
```

Summary feedback stats for the project.

#### ``run\_facets`instance-attribute`¶

```
run_facets: list[dict[str, Any]] | None
```

Facets for the runs in the project.

#### ``total\_cost`instance-attribute`¶

```
total_cost: Decimal | None
```

The total estimated LLM cost associated with the completion tokens.

#### ``prompt\_cost`instance-attribute`¶

```
prompt_cost: Decimal | None
```

The estimated cost associated with the prompt (input) tokens.

#### ``completion\_cost`instance-attribute`¶

```
completion_cost: Decimal | None
```

The estimated cost associated with the completion tokens.

#### ``first\_token\_p50`instance-attribute`¶

```
first_token_p50: timedelta | None
```

The median (50th percentile) time to process the first token.

#### ``first\_token\_p99`instance-attribute`¶

```
first_token_p99: timedelta | None
```

The 99th percentile time to process the first token.

#### ``error\_rate`instance-attribute`¶

```
error_rate: float | None
```

The error rate for the project.

#### ``id`instance-attribute`¶

```
id: UUID
```

The ID of the project.

#### ``start\_time`class-attribute``instance-attribute`¶

```
start_time: datetime = Field(default_factory=lambda: now(utc))
```

The time the project was created.

#### ``end\_time`class-attribute``instance-attribute`¶

```
end_time: datetime | None = None
```

The time the project was ended.

#### ``description`class-attribute``instance-attribute`¶

```
description: str | None = None
```

The description of the project.

#### ``name`class-attribute``instance-attribute`¶

```
name: str | None = None
```

The name of the session.

#### ``extra`class-attribute``instance-attribute`¶

```
extra: dict[str, Any] | None = None
```

Extra metadata for the project.

#### ``tenant\_id`instance-attribute`¶

```
tenant_id: UUID
```

The tenant ID this project belongs to.

#### ``reference\_dataset\_id`instance-attribute`¶

```
reference_dataset_id: UUID | None
```

The reference dataset IDs this project's runs were generated on.

#### ``url`property`¶

```
url: str | None
```

URL of this run within the app.

#### ``metadata`property`¶

```
metadata: dict[str, Any]
```

Retrieve the metadata (if any).

#### ``tags`property`¶

```
tags: list[str]
```

Retrieve the tags (if any).

#### ``\_\_init\_\_ ¶

```
__init__(_host_url: str | None = None, **kwargs: Any) -> None
```

Initialize a Run object.

### ``BaseMessageLike ¶

Bases: `Protocol`

A protocol representing objects similar to BaseMessage.

#### ``content`instance-attribute`¶

```
content: str
```

The content of the message.

#### ``additional\_kwargs`instance-attribute`¶

```
additional_kwargs: dict[Any, Any]
```

Additional keyword arguments associated with the message.

#### ``type`property`¶

```
type: str
```

Type of the Message, used for serialization.

### ``DatasetShareSchema ¶

Bases: `TypedDict`

Represents the schema for a dataset share.

#### ``dataset\_id`instance-attribute`¶

```
dataset_id: UUID
```

The ID of the dataset.

#### ``share\_token`instance-attribute`¶

```
share_token: UUID
```

The token for sharing the dataset.

#### ``url`instance-attribute`¶

```
url: str
```

The URL of the shared dataset.

### ``AnnotationQueue ¶

Bases: `BaseModel`

Represents an annotation queue.

#### ``id`instance-attribute`¶

```
id: UUID
```

The unique identifier of the annotation queue.

#### ``name`instance-attribute`¶

```
name: str
```

The name of the annotation queue.

#### ``description`class-attribute``instance-attribute`¶

```
description: str | None = None
```

An optional description of the annotation queue.

#### ``created\_at`class-attribute``instance-attribute`¶

```
created_at: datetime = Field(default_factory=lambda: now(utc))
```

The timestamp when the annotation queue was created.

#### ``updated\_at`class-attribute``instance-attribute`¶

```
updated_at: datetime = Field(default_factory=lambda: now(utc))
```

The timestamp when the annotation queue was last updated.

#### ``tenant\_id`instance-attribute`¶

```
tenant_id: UUID
```

The ID of the tenant associated with the annotation queue.

### ``AnnotationQueueWithDetails ¶

Bases: `AnnotationQueue`

Represents an annotation queue with details.

#### ``rubric\_instructions`class-attribute``instance-attribute`¶

```
rubric_instructions: str | None = None
```

The rubric instructions for the annotation queue.

#### ``id`instance-attribute`¶

```
id: UUID
```

The unique identifier of the annotation queue.

#### ``name`instance-attribute`¶

```
name: str
```

The name of the annotation queue.

#### ``description`class-attribute``instance-attribute`¶

```
description: str | None = None
```

An optional description of the annotation queue.

#### ``created\_at`class-attribute``instance-attribute`¶

```
created_at: datetime = Field(default_factory=lambda: now(utc))
```

The timestamp when the annotation queue was created.

#### ``updated\_at`class-attribute``instance-attribute`¶

```
updated_at: datetime = Field(default_factory=lambda: now(utc))
```

The timestamp when the annotation queue was last updated.

#### ``tenant\_id`instance-attribute`¶

```
tenant_id: UUID
```

The ID of the tenant associated with the annotation queue.

### ``BatchIngestConfig ¶

Bases: `TypedDict`

Configuration for batch ingestion.

#### ``use\_multipart\_endpoint`instance-attribute`¶

```
use_multipart_endpoint: bool
```

Whether to use the multipart endpoint for batch ingestion.

#### ``scale\_up\_qsize\_trigger`instance-attribute`¶

```
scale_up_qsize_trigger: int
```

The queue size threshold that triggers scaling up.

#### ``scale\_up\_nthreads\_limit`instance-attribute`¶

```
scale_up_nthreads_limit: int
```

The maximum number of threads to scale up to.

#### ``scale\_down\_nempty\_trigger`instance-attribute`¶

```
scale_down_nempty_trigger: int
```

The number of empty threads that triggers scaling down.

#### ``size\_limit`instance-attribute`¶

```
size_limit: int
```

The maximum size limit for the batch.

#### ``size\_limit\_bytes`instance-attribute`¶

```
size_limit_bytes: int | None
```

The maximum size limit in bytes for the batch.

### ``LangSmithInfo ¶

Bases: `BaseModel`

Information about the LangSmith server.

#### ``version`class-attribute``instance-attribute`¶

```
version: str = ''
```

The version of the LangSmith server.

#### ``license\_expiration\_time`class-attribute``instance-attribute`¶

```
license_expiration_time: datetime | None = None
```

The time the license will expire.

#### ``batch\_ingest\_config`class-attribute``instance-attribute`¶

```
batch_ingest_config: BatchIngestConfig | None = None
```

The instance flags.

### ``LangSmithSettings ¶

Bases: `BaseModel`

Settings for the LangSmith tenant.

#### ``id`instance-attribute`¶

```
id: str
```

The ID of the tenant.

#### ``display\_name`instance-attribute`¶

```
display_name: str
```

The display name of the tenant.

#### ``created\_at`instance-attribute`¶

```
created_at: datetime
```

The creation time of the tenant.

### ``FeedbackIngestToken ¶

Bases: `BaseModel`

Represents the schema for a feedback ingest token.

#### ``id`instance-attribute`¶

```
id: UUID
```

The ID of the feedback ingest token.

#### ``url`instance-attribute`¶

```
url: str
```

The URL to GET when logging the feedback.

#### ``expires\_at`instance-attribute`¶

```
expires_at: datetime
```

The expiration time of the token.

### ``RunEvent ¶

Bases: `TypedDict`

Run event schema.

#### ``name`instance-attribute`¶

```
name: str
```

Type of event.

#### ``time`instance-attribute`¶

```
time: datetime | str
```

Time of the event.

#### ``kwargs`instance-attribute`¶

```
kwargs: dict[str, Any] | None
```

Additional metadata for the event.

### ``TimeDeltaInput ¶

Bases: `TypedDict`

Timedelta input schema.

#### ``days`instance-attribute`¶

```
days: int
```

Number of days.

#### ``hours`instance-attribute`¶

```
hours: int
```

Number of hours.

#### ``minutes`instance-attribute`¶

```
minutes: int
```

Number of minutes.

### ``DatasetDiffInfo ¶

Bases: `BaseModel`

Represents the difference information between two datasets.

#### ``examples\_modified`instance-attribute`¶

```
examples_modified: list[UUID]
```

A list of UUIDs representing the modified examples.

#### ``examples\_added`instance-attribute`¶

```
examples_added: list[UUID]
```

A list of UUIDs representing the added examples.

#### ``examples\_removed`instance-attribute`¶

```
examples_removed: list[UUID]
```

A list of UUIDs representing the removed examples.

### ``ComparativeExperiment ¶

Bases: `BaseModel`

Represents a comparative experiment.

This information summarizes evaluation results comparing
two or more models on a given dataset.

#### ``id`instance-attribute`¶

```
id: UUID
```

The unique identifier for the comparative experiment.

#### ``name`class-attribute``instance-attribute`¶

```
name: str | None = None
```

The optional name of the comparative experiment.

#### ``description`class-attribute``instance-attribute`¶

```
description: str | None = None
```

An optional description of the comparative experiment.

#### ``tenant\_id`instance-attribute`¶

```
tenant_id: UUID
```

The identifier of the tenant associated with this experiment.

#### ``created\_at`instance-attribute`¶

```
created_at: datetime
```

The timestamp when the comparative experiment was created.

#### ``modified\_at`instance-attribute`¶

```
modified_at: datetime
```

The timestamp when the comparative experiment was last modified.

#### ``reference\_dataset\_id`instance-attribute`¶

```
reference_dataset_id: UUID
```

The identifier of the reference dataset used in this experiment.

#### ``extra`class-attribute``instance-attribute`¶

```
extra: dict[str, Any] | None = None
```

Optional additional information about the experiment.

#### ``experiments\_info`class-attribute``instance-attribute`¶

```
experiments_info: list[dict] | None = None
```

Optional list of dictionaries containing information about individual experiments.

#### ``feedback\_stats`class-attribute``instance-attribute`¶

```
feedback_stats: dict[str, Any] | None = None
```

Optional dictionary containing feedback statistics for the experiment.

#### ``metadata`property`¶

```
metadata: dict[str, Any]
```

Retrieve the metadata (if any).

### ``PromptCommit ¶

Bases: `BaseModel`

Represents a Prompt with a manifest.

#### ``owner`instance-attribute`¶

```
owner: str
```

The handle of the owner of the prompt.

#### ``repo`instance-attribute`¶

```
repo: str
```

The name of the prompt.

#### ``commit\_hash`instance-attribute`¶

```
commit_hash: str
```

The commit hash of the prompt.

#### ``manifest`instance-attribute`¶

```
manifest: dict[str, Any]
```

The manifest of the prompt.

#### ``examples`instance-attribute`¶

```
examples: list[dict]
```

The list of examples.

### ``ListedPromptCommit ¶

Bases: `BaseModel`

Represents a listed prompt commit with associated metadata.

#### ``id`instance-attribute`¶

```
id: UUID
```

The unique identifier for the prompt commit.

#### ``owner`instance-attribute`¶

```
owner: str
```

The owner of the prompt commit.

#### ``repo`instance-attribute`¶

```
repo: str
```

The repository name of the prompt commit.

#### ``manifest\_id`class-attribute``instance-attribute`¶

```
manifest_id: UUID | None = None
```

The optional identifier for the manifest associated with this commit.

#### ``repo\_id`class-attribute``instance-attribute`¶

```
repo_id: UUID | None = None
```

The optional identifier for the repository.

#### ``parent\_id`class-attribute``instance-attribute`¶

```
parent_id: UUID | None = None
```

The optional identifier for the parent commit.

#### ``commit\_hash`class-attribute``instance-attribute`¶

```
commit_hash: str | None = None
```

The optional hash of the commit.

#### ``created\_at`class-attribute``instance-attribute`¶

```
created_at: datetime | None = None
```

The optional timestamp when the commit was created.

#### ``updated\_at`class-attribute``instance-attribute`¶

```
updated_at: datetime | None = None
```

The optional timestamp when the commit was last updated.

#### ``example\_run\_ids`class-attribute``instance-attribute`¶

```
example_run_ids: list[UUID] | None = Field(default_factory=list)
```

A list of example run identifiers associated with this commit.

#### ``num\_downloads`class-attribute``instance-attribute`¶

```
num_downloads: int | None = 0
```

The number of times this commit has been downloaded.

#### ``num\_views`class-attribute``instance-attribute`¶

```
num_views: int | None = 0
```

The number of times this commit has been viewed.

#### ``parent\_commit\_hash`class-attribute``instance-attribute`¶

```
parent_commit_hash: str | None = None
```

The optional hash of the parent commit.

### ``Prompt ¶

Bases: `BaseModel`

Represents a Prompt with metadata.

#### ``repo\_handle`instance-attribute`¶

```
repo_handle: str
```

The name of the prompt.

#### ``description`class-attribute``instance-attribute`¶

```
description: str | None = None
```

The description of the prompt.

#### ``readme`class-attribute``instance-attribute`¶

```
readme: str | None = None
```

The README of the prompt.

#### ``id`instance-attribute`¶

```
id: str
```

The ID of the prompt.

#### ``tenant\_id`instance-attribute`¶

```
tenant_id: str
```

The tenant ID of the prompt owner.

#### ``created\_at`instance-attribute`¶

```
created_at: datetime
```

The creation time of the prompt.

#### ``updated\_at`instance-attribute`¶

```
updated_at: datetime
```

The last update time of the prompt.

#### ``is\_public`instance-attribute`¶

```
is_public: bool
```

Whether the prompt is public.

#### ``is\_archived`instance-attribute`¶

```
is_archived: bool
```

Whether the prompt is archived.

#### ``tags`instance-attribute`¶

```
tags: list[str]
```

The tags associated with the prompt.

#### ``original\_repo\_id`class-attribute``instance-attribute`¶

```
original_repo_id: str | None = None
```

The ID of the original prompt, if forked.

#### ``upstream\_repo\_id`class-attribute``instance-attribute`¶

```
upstream_repo_id: str | None = None
```

The ID of the upstream prompt, if forked.

#### ``owner`instance-attribute`¶

```
owner: str | None
```

The handle of the owner of the prompt.

#### ``full\_name`instance-attribute`¶

```
full_name: str
```

The full name of the prompt. (owner + repo\_handle)

#### ``num\_likes`instance-attribute`¶

```
num_likes: int
```

The number of likes.

#### ``num\_downloads`instance-attribute`¶

```
num_downloads: int
```

The number of downloads.

#### ``num\_views`instance-attribute`¶

```
num_views: int
```

The number of views.

#### ``liked\_by\_auth\_user`class-attribute``instance-attribute`¶

```
liked_by_auth_user: bool | None = None
```

Whether the prompt is liked by the authenticated user.

#### ``last\_commit\_hash`class-attribute``instance-attribute`¶

```
last_commit_hash: str | None = None
```

The hash of the last commit.

#### ``num\_commits`instance-attribute`¶

```
num_commits: int
```

The number of commits.

#### ``original\_repo\_full\_name`class-attribute``instance-attribute`¶

```
original_repo_full_name: str | None = None
```

The full name of the original prompt, if forked.

#### ``upstream\_repo\_full\_name`class-attribute``instance-attribute`¶

```
upstream_repo_full_name: str | None = None
```

The full name of the upstream prompt, if forked.

### ``ListPromptsResponse ¶

Bases: `BaseModel`

A list of prompts with metadata.

#### ``repos`instance-attribute`¶

```
repos: list[Prompt]
```

The list of prompts.

#### ``total`instance-attribute`¶

```
total: int
```

The total number of prompts.

### ``PromptSortField ¶

Bases: `str`, `Enum`

Enum for sorting fields for prompts.

#### ``num\_downloads`class-attribute``instance-attribute`¶

```
num_downloads = 'num_downloads'
```

Number of downloads.

#### ``num\_views`class-attribute``instance-attribute`¶

```
num_views = 'num_views'
```

Number of views.

#### ``updated\_at`class-attribute``instance-attribute`¶

```
updated_at = 'updated_at'
```

Last updated time.

#### ``num\_likes`class-attribute``instance-attribute`¶

```
num_likes = 'num_likes'
```

Number of likes.

### ``InputTokenDetails ¶

Bases: `TypedDict`

Breakdown of input token counts.

Does _not_ need to sum to full input token count. Does _not_ need to have all keys.

#### ``audio`instance-attribute`¶

```
audio: int
```

Audio input tokens.

#### ``cache\_creation`instance-attribute`¶

```
cache_creation: int
```

Input tokens that were cached and there was a cache miss.

Since there was a cache miss, the cache was created from these tokens.

#### ``cache\_read`instance-attribute`¶

```
cache_read: int
```

Input tokens that were cached and there was a cache hit.

Since there was a cache hit, the tokens were read from the cache. More precisely,
the model state given these tokens was read from the cache.

### ``OutputTokenDetails ¶

Bases: `TypedDict`

Breakdown of output token counts.

Does _not_ need to sum to full output token count. Does _not_ need to have all keys.

#### ``audio`instance-attribute`¶

```
audio: int
```

Audio output tokens.

#### ``reasoning`instance-attribute`¶

```
reasoning: int
```

Reasoning output tokens.

Tokens generated by the model in a chain of thought process (i.e. by OpenAI's o1
models) that are not returned as part of model output.

### ``InputCostDetails ¶

Bases: `TypedDict`

Breakdown of input token costs.

Does _not_ need to sum to full input cost. Does _not_ need to have all keys.

#### ``audio`instance-attribute`¶

```
audio: float
```

Cost of audio input tokens.

#### ``cache\_creation`instance-attribute`¶

```
cache_creation: float
```

Cost of input tokens that were cached and there was a cache miss.

Since there was a cache miss, the cache was created from these tokens.

#### ``cache\_read`instance-attribute`¶

```
cache_read: float
```

Cost of input tokens that were cached and there was a cache hit.

Since there was a cache hit, the tokens were read from the cache. More precisely,
the model state given these tokens was read from the cache.

### ``OutputCostDetails ¶

Bases: `TypedDict`

Breakdown of output token costs.

Does _not_ need to sum to full output cost. Does _not_ need to have all keys.

#### ``audio`instance-attribute`¶

```
audio: float
```

Cost of audio output tokens.

#### ``reasoning`instance-attribute`¶

```
reasoning: float
```

Cost of reasoning output tokens.

Tokens generated by the model in a chain of thought process (i.e. by OpenAI's o1
models) that are not returned as part of model output.

### ``UsageMetadata ¶

Bases: `TypedDict`

Usage metadata for a message, such as token counts.

This is a standard representation of token usage that is consistent across models.

#### ``input\_tokens`instance-attribute`¶

```
input_tokens: int
```

Count of input (or prompt) tokens. Sum of all input token types.

#### ``output\_tokens`instance-attribute`¶

```
output_tokens: int
```

Count of output (or completion) tokens. Sum of all output token types.

#### ``total\_tokens`instance-attribute`¶

```
total_tokens: int
```

Total token count. Sum of input\_tokens + output\_tokens.

#### ``input\_token\_details`instance-attribute`¶

```
input_token_details: NotRequired[InputTokenDetails]
```

Breakdown of input token counts.

Does _not_ need to sum to full input token count. Does _not_ need to have all keys.

#### ``output\_token\_details`instance-attribute`¶

```
output_token_details: NotRequired[OutputTokenDetails]
```

Breakdown of output token counts.

Does _not_ need to sum to full output token count. Does _not_ need to have all keys.

#### ``input\_cost`instance-attribute`¶

```
input_cost: NotRequired[float]
```

The cost of the input tokens.

#### ``output\_cost`instance-attribute`¶

```
output_cost: NotRequired[float]
```

The cost of the output tokens.

#### ``total\_cost`instance-attribute`¶

```
total_cost: NotRequired[float]
```

The total cost of the tokens.

#### ``input\_cost\_details`instance-attribute`¶

```
input_cost_details: NotRequired[InputCostDetails]
```

The cost details of the input tokens.

#### ``output\_cost\_details`instance-attribute`¶

```
output_cost_details: NotRequired[OutputCostDetails]
```

The cost details of the output tokens.

### ``ExtractedUsageMetadata ¶

Bases: `TypedDict`

Usage metadata dictionary extracted from a run.

Should be the same as UsageMetadata, but does not require all
keys to be present.

#### ``input\_tokens`instance-attribute`¶

```
input_tokens: int
```

The number of tokens used for the prompt.

#### ``output\_tokens`instance-attribute`¶

```
output_tokens: int
```

The number of tokens generated as output.

#### ``total\_tokens`instance-attribute`¶

```
total_tokens: int
```

The total number of tokens used.

#### ``input\_token\_details`instance-attribute`¶

```
input_token_details: InputTokenDetails
```

The details of the input tokens.

#### ``output\_token\_details`instance-attribute`¶

```
output_token_details: OutputTokenDetails
```

The details of the output tokens.

#### ``input\_cost`instance-attribute`¶

```
input_cost: float
```

The cost of the input tokens.

#### ``output\_cost`instance-attribute`¶

```
output_cost: float
```

The cost of the output tokens.

#### ``total\_cost`instance-attribute`¶

```
total_cost: float
```

The total cost of the tokens.

#### ``input\_cost\_details`instance-attribute`¶

```
input_cost_details: InputCostDetails
```

The cost details of the input tokens.

#### ``output\_cost\_details`instance-attribute`¶

```
output_cost_details: OutputCostDetails
```

The cost details of the output tokens.

### ``UpsertExamplesResponse ¶

Bases: `TypedDict`

Response object returned from the upsert\_examples\_multipart method.

#### ``count`instance-attribute`¶

```
count: int
```

The number of examples that were upserted.

#### ``example\_ids`instance-attribute`¶

```
example_ids: list[str]
```

The ids of the examples that were upserted.

### ``ExampleWithRuns ¶

Bases: `Example`

Example with runs.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize a Dataset object. |
| `__repr__` | Return a string representation of the RunBase object. |

#### ``runs`class-attribute``instance-attribute`¶

```
runs: list[Run] = Field(default_factory=list)
```

The runs of the example.

#### ``attachments`class-attribute``instance-attribute`¶

```
attachments: dict[str, AttachmentInfo] | None = Field(default=None)
```

Dictionary with attachment names as keys and a tuple of the S3 url
and a reader of the data for the file.

#### ``url`property`¶

```
url: str | None
```

URL of this run within the app.

#### ``Config ¶

Configuration class for the schema.

#### ``\_\_init\_\_ ¶

```
__init__(
    _host_url: str | None = None, _tenant_id: UUID | None = None, **kwargs: Any
) -> None
```

Initialize a Dataset object.

#### ``\_\_repr\_\_ ¶

```
__repr__()
```

Return a string representation of the RunBase object.

### ``ExperimentRunStats ¶

Bases: `TypedDict`

Run statistics for an experiment.

#### ``run\_count`instance-attribute`¶

```
run_count: int | None
```

The number of runs in the project.

#### ``latency\_p50`instance-attribute`¶

```
latency_p50: timedelta | None
```

The median (50th percentile) latency for the project.

#### ``latency\_p99`instance-attribute`¶

```
latency_p99: timedelta | None
```

The 99th percentile latency for the project.

#### ``total\_tokens`instance-attribute`¶

```
total_tokens: int | None
```

The total number of tokens consumed in the project.

#### ``prompt\_tokens`instance-attribute`¶

```
prompt_tokens: int | None
```

The total number of prompt tokens consumed in the project.

#### ``completion\_tokens`instance-attribute`¶

```
completion_tokens: int | None
```

The total number of completion tokens consumed in the project.

#### ``last\_run\_start\_time`instance-attribute`¶

```
last_run_start_time: datetime | None
```

The start time of the last run in the project.

#### ``run\_facets`instance-attribute`¶

```
run_facets: list[dict[str, Any]] | None
```

Facets for the runs in the project.

#### ``total\_cost`instance-attribute`¶

```
total_cost: Decimal | None
```

The total estimated LLM cost associated with the completion tokens.

#### ``prompt\_cost`instance-attribute`¶

```
prompt_cost: Decimal | None
```

The estimated cost associated with the prompt (input) tokens.

#### ``completion\_cost`instance-attribute`¶

```
completion_cost: Decimal | None
```

The estimated cost associated with the completion tokens.

#### ``first\_token\_p50`instance-attribute`¶

```
first_token_p50: timedelta | None
```

The median (50th percentile) time to process the first token.

#### ``first\_token\_p99`instance-attribute`¶

```
first_token_p99: timedelta | None
```

The 99th percentile time to process the first token.

#### ``error\_rate`instance-attribute`¶

```
error_rate: float | None
```

The error rate for the project.

### ``ExperimentResults ¶

Bases: `TypedDict`

Results container for experiment data with stats and examples.

Breaking change in v0.4.32:
The 'stats' field has been split into 'feedback\_stats' and 'run\_stats'.

#### ``feedback\_stats`instance-attribute`¶

```
feedback_stats: dict
```

Feedback statistics for the experiment.

#### ``run\_stats`instance-attribute`¶

```
run_stats: ExperimentRunStats
```

Run statistics (latency, token count, etc.).

### ``InsightsReport ¶

Bases: `BaseModel`

An Insights Report created by the Insights Agent over a tracing project.

#### ``link`property`¶

```
link: str
```

URL to view this Insights Report in LangSmith UI.

Back to top