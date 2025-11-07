<!-- URL: page_141 -->

Skip to content

Edit this page

# Hub ¶

`langchain-classic` documentation

These docs cover the `langchain-classic` package. This package will be maintained for security vulnerabilities until December 2026. Users are encouraged to migrate to the `langchain` package for the latest features and improvements. See docs for `langchain`

## ``langchain\_classic.hub ¶

Interface with the LangChain Hub.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `push` | Push an object to the hub and returns the URL it can be viewed at in a browser. |
| `pull` | Pull an object from the hub and returns it as a LangChain object. |

### ``push ¶

```
push(
    repo_full_name: str,
    object: Any,
    *,
    api_url: str | None = None,
    api_key: str | None = None,
    parent_commit_hash: str | None = None,
    new_repo_is_public: bool = False,
    new_repo_description: str | None = None,
    readme: str | None = None,
    tags: Sequence[str] | None = None,
) -> str
```

Push an object to the hub and returns the URL it can be viewed at in a browser.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `repo_full_name` | The full name of the prompt to push to in the format of<br>`owner/prompt_name` or `prompt_name`.<br>**TYPE:**`str` |
| `object` | The LangChain object to serialize and push to the hub.<br>**TYPE:**`Any` |
| `api_url` | The URL of the LangChain Hub API. Defaults to the hosted API service<br>if you have an API key set, or a localhost instance if not.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `api_key` | The API key to use to authenticate with the LangChain Hub API.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `parent_commit_hash` | The commit hash of the parent commit to push to. Defaults<br>to the latest commit automatically.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `new_repo_is_public` | Whether the prompt should be public.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `new_repo_description` | The description of the prompt.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `readme` | README content for the repository.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `tags` | Tags to associate with the prompt.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | URL where the pushed object can be viewed in a browser. |

### ``pull ¶

```
pull(
    owner_repo_commit: str,
    *,
    include_model: bool | None = None,
    api_url: str | None = None,
    api_key: str | None = None,
) -> Any
```

Pull an object from the hub and returns it as a LangChain object.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `owner_repo_commit` | The full name of the prompt to pull from in the format of<br>`owner/prompt_name:commit_hash` or `owner/prompt_name`<br>or just `prompt_name` if it's your own prompt.<br>**TYPE:**`str` |
| `include_model` | Whether to include the model configuration in the pulled prompt.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `api_url` | The URL of the LangChain Hub API. Defaults to the hosted API service<br>if you have an API key set, or a localhost instance if not.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `api_key` | The API key to use to authenticate with the LangChain Hub API.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Any` | The pulled LangChain object. |

Back to top