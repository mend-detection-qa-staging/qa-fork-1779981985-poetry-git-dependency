# poetry-git-dependency

Tests *Poetry VCS/git-sourced dependency*: a project where one dep is resolved from a GitHub
git URL rather than PyPI. There is no `.whl` file — this exercises the non-whl code path
in the Poetry resolver.

## Why this matters

The SCA-5540 bug and fix are entirely in the `versionPatternWhl` path. A git dep bypasses
that path entirely. This test confirms the non-whl resolver path is unaffected by the fix
and that git deps still appear in the dep tree.

## Dependencies

| Package | Source | Type |
|---|---|---|
| `requests` | PyPI | whl |
| `pyyaml` | PyPI | whl |
| `dateparser` | GitHub `v1.2.0` tag | git (no whl) |

## Key assertions

- All 3 direct deps appear in the tree
- PyPI deps have non-empty `sha1`; git dep may have null `sha1` (expected, no cache entry)
- Transitives from both PyPI and git source are resolved
