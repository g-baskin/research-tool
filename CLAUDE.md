# Research Tool

This repository packages one LLM agent skill, `deep-research-suite`, plus a small Python helper CLI for no-API web/news research and lawful public-source OSINT evidence gathering.

## Structure

- `skills/deep-research-suite/SKILL.md` is the skill entrypoint and contains the research/OSINT workflow instructions.
- `skills/deep-research-suite/scripts/research` is the only executable helper; it gathers raw web/news/fetched-page material for an agent to synthesize.
- `research-reports/`, `research-raw.md`, `osint-raw.md`, and `*.raw.md` are generated local outputs and are gitignored.
- `NOTICE.md` records attribution for the adapted helper/workflow.
- `.gg/commands/commit.md` defines the local commit workflow for this repo.

## Runtime and dependencies

- The helper is a PEP 723 inline Python script with `requires-python = ">=3.11"`.
- The helper runs through `uv run --script` via its shebang.
- The only declared third-party dependency is `ddgs>=9.0.0` in `skills/deep-research-suite/scripts/research`.
- There is no project-level `pyproject.toml`, lockfile, package manifest, test suite, build config, or CI config.

## Useful commands

- Smoke-check the helper CLI:
  ```bash
  uv run --script skills/deep-research-suite/scripts/research --help
  ```
- Run a basic search:
  ```bash
  uv run --script skills/deep-research-suite/scripts/research "<query>"
  ```
- Run web + news search and fetch top pages:
  ```bash
  uv run --script skills/deep-research-suite/scripts/research --full \
    "<topic> overview" \
    "<topic> risks alternatives" \
    --max 8 --fetch-top 3 --output research-raw.md
  ```
- Fetch known URLs directly:
  ```bash
  uv run --script skills/deep-research-suite/scripts/research --fetch "https://example.com"
  ```

## Skill installation notes

- The installable skill folder is `skills/deep-research-suite/`; copy the whole folder, not just `SKILL.md`, so the helper script is available too.
- For Claude Code personal install, the destination is `~/.claude/skills/deep-research-suite/`.
- If `~/.claude/skills/` did not exist when Claude Code started, restart Claude Code after creating it so the new skill directory is watched.
