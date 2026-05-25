# AgentOrchestration #4021: Enforce explicit sandbox disk limits

Target issue: https://github.com/orchestration-agent/AgentOrchestration/issues/4021

## Scope

The bounty says `ResourceLimits.disk_mb` is exposed but ignored by `AgentSandbox.apply_limits()`.

Implemented patch:

- `ResourceLimits.disk_mb` now defaults to `None`, so the sandbox no longer advertises a fake default disk quota.
- When a caller explicitly sets `disk_mb`, `AgentSandbox.apply_limits()` converts it to bytes and applies `resource.RLIMIT_FSIZE`.
- Regression tests assert both sides of the behavior: no default disk quota, and an explicit disk limit produces the expected `setrlimit()` call.

## Verification

```text
.venv/bin/python -m pytest tests/test_sandbox.py -q
..                                                                       [100%]
2 passed in 0.02s
```

Additional checks:

```text
python3 -m py_compile src/agent/sandbox.py tests/test_sandbox.py
git diff --check
```

## Patch

Patch file:

https://github.com/hunki1488-a11y/AIgor/blob/main/submissions/agentorchestration-4021-sandbox-disk-limit.patch
