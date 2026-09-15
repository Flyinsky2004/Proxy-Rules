# Repository Guidelines

## Project Structure & Module Organization

This repository stores proxy, direct, process, and blocking rules as configuration data.

- Root `*.txt` files are provider lists in Clash/Surge-style `payload` or line-oriented formats.
- `hosts` contains exact IPv4/IPv6 mappings used for local domain blocking.
- `sing-box/*.json` contains sing-box rule-set equivalents; keep the matching rule-set name aligned with its root counterpart (for example, `proxy-rules.txt` and `sing-box/proxy-rules.json`).
- There is no application source, generated build directory, or dedicated test tree.

## Build, Test, and Development Commands

No build step or package manager is required. Before submitting changes, run lightweight syntax checks:

```sh
jq empty sing-box/*.json       # Parse every sing-box rule set
git diff --check                # Detect whitespace errors
git status --short               # Review the complete change set
```

For text providers, inspect the changed file and confirm each entry follows the existing format (`payload:` with indented values, or a valid `DOMAIN-*`/`PROCESS-NAME` line). If a consumer-specific validator is available in your proxy client, import the changed provider there as an additional check.

## Coding Style & Naming Conventions

Use UTF-8 text, LF line endings, and the existing two- or four-space indentation of each file. Preserve comments and section headers when editing large lists. Use lowercase, descriptive, hyphen-separated filenames; name sing-box JSON after the corresponding root provider. Keep domains canonical (no accidental schemes or paths), process names exactly as reported by the operating system, and JSON formatted with two-space indentation. Avoid unrelated reformatting or duplicate entries.

## Testing Guidelines

There is no automated test suite or coverage target. Treat parsing and semantic review as tests: run `jq empty`, check that rule objects contain the expected `version` and `rules` keys, and verify additions are present in the intended provider without changing unrelated providers. For `hosts`, review both `0.0.0.0` and `::1` entries where dual-stack blocking is intended.

## Commit & Pull Request Guidelines

History favors short, imperative summaries (for example, `Add Telegram process to proxy process rules`) and occasional `fix:` prefixes. Follow that style, keep one logical change per commit, and mention the affected provider. Pull requests should explain the domains/processes changed, identify matching text and sing-box updates, include validation performed, and call out any intentional duplicates or compatibility considerations. Never include credentials, private endpoints, or unrelated machine-specific files.
