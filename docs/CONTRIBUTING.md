# Contributing

Contributions are welcome. Please follow these steps.

## Adding a Blueprint

1. Read [CONVENTIONS.md](CONVENTIONS.md) first.
2. Place the file in the matching category under `blueprints/`. If no category fits, create one and update both `CONVENTIONS.md` and `README.md`.
3. Required fields: `name`, `description`, `domain`, `source_url`, `author`, `# version:` comment on line 1.
4. Add a row to the category table in `README.md` with an import badge.
5. Add an entry under `## Unreleased` in `CHANGELOG.md`.
6. CI must pass before merge.

## Commit Format

This project uses [Conventional Commits](https://www.conventionalcommits.org/):

| Type | When to use |
|------|-------------|
| `feat(<category>): add <short_name>` | New blueprint |
| `fix(<category>): <description>` | Bug fix in existing blueprint |
| `refactor(<category>): <description>` | Refactor without behavior change |
| `docs: <description>` | Documentation only |

## Releases

Release tags follow CalVer: `vYYYY.MM.DD`. Rename `## Unreleased` in `CHANGELOG.md` and add a new `## Unreleased` section above it.

## Reporting Issues

Use the GitHub Issue templates (bug report or blueprint request).
