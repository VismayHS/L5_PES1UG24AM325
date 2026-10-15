# Issues Found — `wildlife_sightings.py`

Line numbers refer to the original file. Ordered by severity (high and medium first).

| Issue | Type | Line(s) | Description | Fix Approach |
|-------|------|---------|-------------|--------------|
| Weak MD5 hash (Bandit B324) | Security (High) | 15 | `hashlib.md5` is a broken hash algorithm; Bandit flags it as High severity | Use `hashlib.sha256` instead |
| SQL injection (Bandit B608) | Security (Medium) | 9 | INSERT query built with `str.format`, so a value like `O'Brien's owl` breaks the query and crafted input could change it | Use `?` placeholders and pass the values as a tuple to `execute()` |
| Mutable default argument (Pylint W0102) | Bug | 8 | `extras={}` is created once and shared across every call that omits it, so totals leak between calls | Default to `None` and create a new dict inside the function |
| Shadowed global `conn` (Pylint W0621) | Warning | 4, 8, 18, 26 | A module-level `conn` is shadowed by the `conn` parameter in every function; the DB is also created as a side effect of importing the module | Build the connection in `create_db()`, called from `main()` |
| `assert` used for a runtime check (Bandit B101) | Security (Low) | 36 | `assert` statements are removed when Python runs with `-O`, so the check silently disappears | Replace with an `if` check that raises `RuntimeError` |
| Code runs on import | Bug | 40 | `main()` is called at module level, so importing the file runs the demo | Wrap in `if __name__ == "__main__":` |
| Non-snake_case names (Pylint C0103) | Convention | 8, 14, 18 | `logSighting`, `observerId`, `zoneSummary` break PEP 8 naming | Rename to `log_sighting`, `observer_id`, `zone_summary` |
| Missing docstrings (Pylint C0114/C0116) | Convention | 1, 8, 14, 18, 26, 32 | No module or function docstrings | Add docstrings |
| Unnecessary `else` after `return` (Pylint R1705) | Refactor | 20–23 | `else` branch is redundant after `return` | Remove `else` and de-indent |
| Format string instead of f-string (Pylint C0209) | Convention | 9 | `str.format` used to build a string | Resolved by the parameterised query fix |
| Lines too long (Flake8 E501 / Pylint C0301) | Style | 5, 9, 19, 27 | Lines exceed 79 characters (Flake8) / 100 characters (Pylint) | Split the SQL strings and calls across lines |
| Ambiguous variable name `l` (Flake8 E741) | Style | 28 | `l` is easily confused with `1` and `I` | Rename to `least_seen` |
| Missing whitespace around operator (Flake8 E225) | Style | 28 | `l=min(...)` has no spaces around `=` | Write `least_seen = min(...)` |
