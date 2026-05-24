# AgentOrchestration #3873: Reject non-finite gauge values

Target issue: https://github.com/orchestration-agent/AgentOrchestration/issues/3873

## Scope

The bounty asks for finite-only gauge values in `src/common/metrics.py`.

Implemented patch:

- `MetricsCollector.gauge()` rejects `NaN`, `+inf`, and `-inf` using `math.isfinite`.
- Rejected values raise `ValueError` before mutating the gauge store.
- Existing gauge values are preserved if a later non-finite update is rejected.
- The collector lock is changed from `Lock` to `RLock` because the existing `test_timer` path calls `observe()` while `stop_timer()` already holds the collector lock.

## Verification

```text
.venv/bin/python -m pytest tests/test_metrics.py -q
........                                                                 [100%]
8 passed in 0.02s
```

Additional checks:

```text
python3 -m py_compile src/common/metrics.py tests/test_metrics.py
git diff --check
```

Full test suite note:

```text
.venv/bin/python -m pytest tests -q
```

The full suite currently fails during collection on an unrelated pre-existing import error:

```text
ImportError: cannot import name 'AgentStatus' from 'src.agent'
```

## Patch

Patch file:

https://github.com/hunki1488-a11y/AIgor/blob/main/submissions/agentorchestration-3873-finite-gauge.patch
