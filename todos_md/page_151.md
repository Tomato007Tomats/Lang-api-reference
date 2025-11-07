<!-- URL: page_151 -->

Skip to content

Edit this page

# Cache integration tests ¶

## ``langchain\_tests.integration\_tests.SyncCacheTestSuite ¶

Bases: `BaseStandardTests`

Test suite for checking the `BaseCache` API of a caching layer for LLMs.

This test suite verifies the basic caching API of a caching layer for LLMs.

The test suite is designed for synchronous caching layers.

Implementers should subclass this test suite and provide a fixture
that returns an empty cache for each test.

| METHOD | DESCRIPTION |
| --- | --- |
| `test_no_overrides_DO_NOT_OVERRIDE` | Test that no standard tests are overridden. |
| `cache` | Get the cache class to test. |
| `get_sample_prompt` | Return a sample prompt for testing. |
| `get_sample_llm_string` | Return a sample LLM string for testing. |
| `get_sample_generation` | Return a sample `Generation` object for testing. |
| `test_cache_is_empty` | Test that the cache is empty. |
| `test_update_cache` | Test updating the cache. |
| `test_cache_still_empty` | Test that the cache is still empty. |
| `test_clear_cache` | Test clearing the cache. |
| `test_cache_miss` | Test cache miss. |
| `test_cache_hit` | Test cache hit. |
| `test_update_cache_with_multiple_generations` | Test updating the cache with multiple `Generation` objects. |

### ``test\_no\_overrides\_DO\_NOT\_OVERRIDE ¶

```
test_no_overrides_DO_NOT_OVERRIDE() -> None
```

Test that no standard tests are overridden.

### ``cache`abstractmethod`¶

```
cache() -> BaseCache
```

Get the cache class to test.

The returned cache should be EMPTY.

### ``get\_sample\_prompt ¶

```
get_sample_prompt() -> str
```

Return a sample prompt for testing.

### ``get\_sample\_llm\_string ¶

```
get_sample_llm_string() -> str
```

Return a sample LLM string for testing.

### ``get\_sample\_generation ¶

```
get_sample_generation() -> Generation
```

Return a sample `Generation` object for testing.

### ``test\_cache\_is\_empty ¶

```
test_cache_is_empty(cache: BaseCache) -> None
```

Test that the cache is empty.

### ``test\_update\_cache ¶

```
test_update_cache(cache: BaseCache) -> None
```

Test updating the cache.

### ``test\_cache\_still\_empty ¶

```
test_cache_still_empty(cache: BaseCache) -> None
```

Test that the cache is still empty.

This test should follow a test that updates the cache.

This just verifies that the fixture is set up properly to be empty after each
test.

### ``test\_clear\_cache ¶

```
test_clear_cache(cache: BaseCache) -> None
```

Test clearing the cache.

### ``test\_cache\_miss ¶

```
test_cache_miss(cache: BaseCache) -> None
```

Test cache miss.

### ``test\_cache\_hit ¶

```
test_cache_hit(cache: BaseCache) -> None
```

Test cache hit.

### ``test\_update\_cache\_with\_multiple\_generations ¶

```
test_update_cache_with_multiple_generations(cache: BaseCache) -> None
```

Test updating the cache with multiple `Generation` objects.

## ``langchain\_tests.integration\_tests.AsyncCacheTestSuite ¶

Bases: `BaseStandardTests`

Test suite for checking the `BaseCache` API of a caching layer for LLMs.

Verifies the basic caching API of a caching layer for LLMs.

The test suite is designed for synchronous caching layers.

Implementers should subclass this test suite and provide a fixture that returns an
empty cache for each test.

| METHOD | DESCRIPTION |
| --- | --- |
| `test_no_overrides_DO_NOT_OVERRIDE` | Test that no standard tests are overridden. |
| `cache` | Get the cache class to test. |
| `get_sample_prompt` | Return a sample prompt for testing. |
| `get_sample_llm_string` | Return a sample LLM string for testing. |
| `get_sample_generation` | Return a sample `Generation` object for testing. |
| `test_cache_is_empty` | Test that the cache is empty. |
| `test_update_cache` | Test updating the cache. |
| `test_cache_still_empty` | Test that the cache is still empty. |
| `test_clear_cache` | Test clearing the cache. |
| `test_cache_miss` | Test cache miss. |
| `test_cache_hit` | Test cache hit. |
| `test_update_cache_with_multiple_generations` | Test updating the cache with multiple `Generation` objects. |

### ``test\_no\_overrides\_DO\_NOT\_OVERRIDE ¶

```
test_no_overrides_DO_NOT_OVERRIDE() -> None
```

Test that no standard tests are overridden.

### ``cache`abstractmethod``async`¶

```
cache() -> BaseCache
```

Get the cache class to test.

The returned cache should be EMPTY.

### ``get\_sample\_prompt ¶

```
get_sample_prompt() -> str
```

Return a sample prompt for testing.

### ``get\_sample\_llm\_string ¶

```
get_sample_llm_string() -> str
```

Return a sample LLM string for testing.

### ``get\_sample\_generation ¶

```
get_sample_generation() -> Generation
```

Return a sample `Generation` object for testing.

### ``test\_cache\_is\_empty`async`¶

```
test_cache_is_empty(cache: BaseCache) -> None
```

Test that the cache is empty.

### ``test\_update\_cache`async`¶

```
test_update_cache(cache: BaseCache) -> None
```

Test updating the cache.

### ``test\_cache\_still\_empty`async`¶

```
test_cache_still_empty(cache: BaseCache) -> None
```

Test that the cache is still empty.

This test should follow a test that updates the cache.

This just verifies that the fixture is set up properly to be empty after each
test.

### ``test\_clear\_cache`async`¶

```
test_clear_cache(cache: BaseCache) -> None
```

Test clearing the cache.

### ``test\_cache\_miss`async`¶

```
test_cache_miss(cache: BaseCache) -> None
```

Test cache miss.

### ``test\_cache\_hit`async`¶

```
test_cache_hit(cache: BaseCache) -> None
```

Test cache hit.

### ``test\_update\_cache\_with\_multiple\_generations`async`¶

```
test_update_cache_with_multiple_generations(cache: BaseCache) -> None
```

Test updating the cache with multiple `Generation` objects.

Back to top