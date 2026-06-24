# AGENTS.md

## Quick Reference

**Project**: AstrBot plugin integrating Suwayomi-Server for manga search, reading, downloads, and subscription updates.
**Language**: Python 3.12+ | **Package manager**: uv | **Framework**: AstrBot plugin system

## Commands

```bash
# Unit tests (26 tests, no network needed)
uv run pytest tests/test_models.py tests/test_client.py tests/test_subscription.py -v

# Integration tests (requires live Suwayomi-Server)
uv run pytest tests/test_live_api.py -v -s
# Custom server: SUWAYOMI_URL=http://host:9330 uv run pytest tests/test_live_api.py -v -s

# All tests
uv run pytest -v

# Syntax check
python -c "import ast; ast.parse(open('main.py', encoding='utf-8').read()); print('OK')"
```

## Architecture

```
main.py (SuwayomiPlugin)
  ├── suwayomi/client.py (SuwayomiClient - async GraphQL HTTP)
  ├── suwayomi/models.py (Source, Manga, Chapter, SearchResult dataclasses)
  └── utils/subscription.py (SubscriptionManager - AstrBot KV storage)
```

- `main.py`: Plugin entry, all 9 commands under `@filter.command_group("漫画")`, background update loop
- `suwayomi/client.py`: All Suwayomi interaction via `POST /api/graphql`; supports none/basic/jwt auth
- `suwayomi/models.py`: Pure dataclasses with `from_dict()` factory methods
- `utils/subscription.py`: Persists subscriptions via AstrBot's `get_kv_data()`/`put_kv_data()`

## Critical Quirks

1. **Source ID is string, not int**: Suwayomi's `source` field is `LongString` scalar. GraphQL vars must be `$sid:LongString!`, values must be strings like `"524579092615598717"`.

2. **API returns numbers as strings**: JSON fields like `"id": "287"` need explicit `int()`/`float()` conversion in `from_dict()`.

3. **Use `filter` not `condition` for title search**: `condition: {title: "..."}` is exact match only. Use `filter: {title: {includes: "..."}}` for substring search.

4. **`Long` type doesn't exist**: Suwayomi GraphQL rejects `Long` type declarations. Use `LongString`.

5. **Source ID `"0"` crashes searches**: Local source causes NullPointerException. Skip it when iterating sources.

6. **Background task startup**: Use `@filter.on_astrbot_loaded()` not `__init__` for `asyncio.create_task()` - event loop may not be running during plugin load.

7. **All command args are strings**: AstrBot passes raw strings, not typed values. Explicit `float()`/`int()` conversion required in command handlers.

8. **Duplicate chapter numbers**: Some manga have multiple chapters with same number (e.g., appendices). Plugin detects this and prompts users to use `id:xxx` syntax for disambiguation.

## Adding New Commands

1. Add method to `SuwayomiPlugin` class in `main.py`
2. Decorate with `@manga_group.command("命令名")`
3. First param: `event: AstrMessageEvent`
4. Return text: `yield event.plain_result(...)`
5. Return rich media: `yield event.chain_result([...])`
6. Docstring = user-facing help text
7. User prompts use `「漫画 命令名」` format (with space)

## File Conventions

- `metadata.yaml`: AstrBot plugin metadata (name, version, platforms)
- `_conf_schema.json`: AstrBot WebUI config form schema
- `requirements.txt`: Runtime deps (currently just `aiohttp>=3.9.0`)
- `pyproject.toml`: Dev deps (pytest, pytest-asyncio)
- Tests in `tests/` - unit tests are synchronous or use `@pytest.mark.asyncio`
- `test_live_api.py`: Integration tests, skipped by default, need live server
