# Working in this repository

This repository holds no product code. It holds three others as git
submodules — `contracts/` (ats-strike-contract), `ats-strike/` (server and
pipeline) and `ats-strike-client/` — and the tests that need more than one of
them.

- **Change code where it lives.** A fix to the server, the client or the
  contract is a commit in that repository plus the new pointer here, never an
  edit kept in a submodule.
- **Not recursive.** The submodules' own `contracts/` stay unchecked out;
  `pyproject.toml` explains why every member uses the top one.
- **No test may need an API key, a network or a terminal.** Fake at the
  boundary: the test double replaces the HTTP client inside a real
  `ModelSelector`, so request assembly and structured output stay under test.
  cv-eval is not a pytest test; only its scoring functions are.
- **The test set is for scoring only.** Explore and tune prompts on your own
  data, never on `tests/cv_quality/jds`. `requirements.json` and `stripped/`
  files are the yardstick earlier runs were scored with: never rewrite one.
- `uv` for everything: `uv sync`, `uv run pytest`, `uv lock`.
- Do not commit or push. Changes are left in the working tree for review.
