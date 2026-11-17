# ats-integration-tests

The [ats-strike](https://github.com/gcontini/ats-strike) server and the
[ats-strike-client](https://github.com/gcontini/ats-strike-client), tested
together, over the [contract](https://github.com/gcontini/ats-strike-contract)
they share. Each repository tests itself alone; this one holds all three as
git submodules and checks that the combination their pointers name still
agrees about the wire format.

```text
contracts/           the ats-strike-contract repository
ats-strike/          the ats-strike repository: server and pipeline
ats-strike-client/   the ats-strike-client repository: the CLI
tests/
  test_integration.py   the client against two real servers, in one process
  cv_quality/           cv-eval: CV quality on a fixed set of job descriptions
```

## Running it

```bash
git clone https://github.com/gcontini/ats-integration-tests
cd ats-integration-tests
git submodule update --init      # not --recursive: one contracts/, the top one
uv sync
uv run pytest
```

No API key, no network: the model endpoint is faked under a real
`ModelSelector`, and the CV is compiled for real when `pdflatex` is on `PATH`.
CI also builds the client executable from `ats-strike-client/`, starts the
server from `ats-strike/` and renders a PDF through it.

## Testing a new server or client

A submodule pointer is the version under test. To test what was pushed:

```bash
git submodule update --remote ats-strike       # or ats-strike-client, contracts
uv lock                                        # if a member's dependencies changed
uv run pytest
```

and commit the new pointer, so CI tests the same combination.

## cv-eval

`tests/cv_quality/cv_eval.py` writes one CV per job description with the
server in `ats-strike/` and the client in `ats-strike-client/`, has the
cv-judge agent score them, and appends the averages to
`tests/cv_quality/results.csv`. It needs a model: put `.env` with the keys at
this repository's root. The `cv-eval` skill
(`.claude/skills/cv-eval/SKILL.md`) is the procedure.
