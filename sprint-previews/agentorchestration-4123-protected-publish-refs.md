# AgentOrchestration #4123 - protected publish refs

Issue: https://github.com/orchestration-agent/AgentOrchestration/issues/4123
Bounty: $7000
Local branch: `/tmp/AgentOrchestration-4123` `codex/enforce-protected-publish-refs`
Local commit: `55b3f61f43eaae9710b5b374b57060e93267db08`

## Problem

Manual package publish jobs can target unprotected refs. That allows package
builds to run from code that has not passed the repository release controls.

## Patch

- Adds `.github/workflows/publish.yml` with a package publish flow.
- Runs `scripts/check_publish_ref.py` before dependency install, build, and
  PyPI publishing.
- Accepts protected branch refs using GitHub's `GITHUB_REF_PROTECTED` context.
- Accepts release tags only when they match `vX.Y.Z` policy and `git tag -v`
  verifies the tag signature.
- Ignores manual dispatch input ref-style overrides and uses only
  GitHub-provided `GITHUB_REF*` environment values.
- Adds regression tests for protected branch, unprotected branch, manual input
  override rejection, signed tag acceptance, unsigned tag rejection, and
  non-release tag rejection.

## Verification

```text
.venv/bin/python -m pytest tests/test_publish_ref_guard.py -q
.......                                                                  [100%]
7 passed in 0.01s

.venv/bin/python -m flake8 scripts/check_publish_ref.py tests/test_publish_ref_guard.py
# passed

python3 -m py_compile scripts/check_publish_ref.py tests/test_publish_ref_guard.py
# passed

git diff --check
# passed
```

## Patch File

https://github.com/hunki1488-a11y/AIgor/blob/main/submissions/agentorchestration-4123-protected-publish-refs.patch

## Submission Status

Ready for PR/claim after explicit confirmation to star the repo, fork/open PR,
and comment `/attempt` + `/claim` on the bounty issue.
