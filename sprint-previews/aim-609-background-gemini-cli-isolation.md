# Sprint Preview: BrianV1981/aim#609 Background Gemini CLI Isolation

Lead: https://github.com/BrianV1981/aim/issues/609

## Proposed Paid Sprint

Price: `250 USD` for a focused audit/report, or `500 USD` for PR delivery.

Turnaround: `48h` after scope confirmation.

Deliverable: isolate background Gemini CLI calls from the operator's primary Gemini CLI home/history and unblock headless execution.

## Current Finding

The issue is high-fit because it is specific, already diagnosed, and has zero comment competition at review time.

Current `aim_core/reasoning_utils.py` still sets two environment variables before invoking `gemini -p - -o json -y` from `memory-wiki`:

- `GEMINI_CLI_TMP_DIR`
- `GEMINI_CLI_DISABLE_CHECKPOINT`

The issue states those are ignored by current Gemini CLI. That leaves the background summarizer at risk of writing daemon chunk calls into the operator's normal Gemini CLI history.

## Patch Shape

The prepared patch:

- adds a daemon-specific `GEMINI_CLI_HOME` under the system temp directory;
- creates the nested `.gemini` directory before invocation;
- sets `GEMINI_CLI_TRUST_WORKSPACE=true` for the controlled `memory-wiki` working directory;
- sets a distinct `GEMINI_CLI_SURFACE=aim-background-reasoning` marker;
- points `GEMINI_CLI_TRUSTED_FOLDERS_PATH` into the isolated home;
- removes the two obsolete env vars;
- adds a regression test asserting the env/cwd passed to `subprocess.run`.

Public patch preview: https://github.com/hunki1488-a11y/AIgor/blob/main/submissions/aim-609-background-gemini-cli-isolation.patch

## Verification

Local verification in `/tmp/aim`:

```sh
python3 tests/test_reasoning_utils_background_cli.py
python3 -m py_compile aim_core/reasoning_utils.py tests/test_reasoning_utils_background_cli.py
git diff --check
```

Result: targeted test passed, compile passed, no whitespace errors.

`pytest` is not installed in the local system Python, so the full suite was not run.

## Acceptance Criteria

- Background `generate_reasoning` calls do not use the operator's normal Gemini CLI home.
- The headless background daemon does not stop on an interactive workspace-trust prompt for the controlled `memory-wiki` cwd.
- The regression test fails if obsolete env vars are reintroduced or if isolation env is removed.
