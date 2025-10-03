# Repository Guidelines

## Dos and Don’ts

- Do run the quick smoke test (see "Build, Test, and Development Commands") before sending changes so the local script workflow is exercised without downloading remote data.
- Do ensure generated outputs listed in `.gitignore` (for example `daily/` and `nrd-*days*.txt`) are not staged for commit.
- By default, focus changes in `nrd-list-downloader.sh`; update other files when the change clearly requires it (docs, ignore rules, CI, etc.).
- Don’t commit WhoisDS credentials or NRD data archives; rely on environment variables and ignore rules instead.

## Project Structure and Module Organization

- Root contains the executable shell script `nrd-list-downloader.sh`, the user guide `README.md`, license information, and `.gitignore` rules that exclude generated NRD lists.
- CI runs via GitHub Actions (`.github/workflows/ci.yml`).
- The script writes generated lists to `nrd-<range>days-<tier>.txt` and per-day directories under `daily/`; these paths are ignored by default but exist locally after runs.
- There are no additional source modules, tests, or data assets beyond the shell script.

## Build, Test, and Development Commands

- Primary workflow: run `./nrd-list-downloader.sh` from the repository root to download and aggregate NRD lists. This requires outbound HTTPS access to WhoisDS and the commands listed in the README dependencies.
- Quick verification without network traffic: run `DAY_RANGE=0 ./nrd-list-downloader.sh` to generate an empty aggregate file while exercising IO paths.
- Static analysis before pushing: `shellcheck -x nrd-list-downloader.sh`; use `bash -n nrd-list-downloader.sh` for a quick syntax sanity check.
- Formatting: `shfmt -i 4 -ci -sr -w nrd-list-downloader.sh` (preserve four-space indentation).
- Paid tier access relies on exporting `PAID_WHOISDS_USERNAME` and `PAID_WHOISDS_PASSWORD` in the shell session prior to running the script.

## Coding Style and Naming Conventions

- Use Bash with `set -e` for fail-fast behavior; keep new utilities within the existing script unless modularization is explicitly requested.
- Follow the prevailing four-space indentation and brace placement (`function name() { ... }`).
- Preserve helper naming such as `echo.Green`/`echo.Red` (from ColorEchoForShell) for colored output; introduce new helpers only when broadly reused.
- Keep comments concise with `#` at column 1 or aligned with existing style; prefer English prose for code comments.
- Avoid introducing external dependencies; continue leveraging coreutils commands already vetted in README dependencies.

## Testing Guidelines

- There is no automated test suite; rely on manual runs of the script.
- For regression checks, start with the smoke test above to validate control flow and file handling, then execute with the intended range once network access is available.
- When modifying paid account handling, test both free (`PAID_WHOISDS_*` unset) and paid (variables set) paths to ensure conditional logic stays correct.

## Commit and Pull Request Guidelines

- Keep commits focused and follow the existing style: imperative, capitalized subjects without trailing periods (e.g., `Fix link to AdGuard Home`).
- Include a brief body explaining what changed and why when the intent is not obvious; wrap at roughly 72 characters.
- Reference related issues using GitHub syntax (`close #1`) when applicable.
- Ensure the script passes `bash -n` and your targeted runtime checks before submitting a PR; mention the executed commands in the PR description.
- No PR template exists, so summarize scope, testing, and any follow-up steps directly in the PR body.

## Safety and Permissions

- It is safe to read sources, adjust small portions of `nrd-list-downloader.sh`, and run `bash -n` or the smoke test locally.
- Avoid committing generated NRD files, modifying remote data URLs without verification, or storing secrets in the repository.
- Treat outbound downloads as potentially disruptive; coordinate before triggering large `DAY_RANGE` runs in shared environments.
