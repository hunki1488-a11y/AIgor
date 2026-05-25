# AgentOrchestration #4214 - SDK configurable request timeout

Issue: https://github.com/orchestration-agent/AgentOrchestration/issues/4214
Bounty: $5000
Local branch: `/tmp/AgentOrchestration-4214` `codex/sdk-configurable-timeout`
Local commit: `fca1bbb0a8dba246839ed9f6bc00dc29b8625b41`

## Problem

`OrchestratorClient._request()` calls `urlopen(req)` without a timeout. If an
endpoint accepts a connection and never replies, worker startup or CI can hang
until an external watchdog kills the process.

## Patch

- Adds `OrchestratorClient.DEFAULT_TIMEOUT = 30.0`.
- Adds a constructor option: `OrchestratorClient(timeout=...)`.
- Adds `AO_REQUEST_TIMEOUT` as an environment override.
- Validates timeout values as finite positive numbers.
- Passes `timeout=self.timeout` into every `urlopen()` request.
- Adds network-free regression tests using monkeypatched `urlopen`.

## Verification

```text
.venv/bin/python -m pytest tests/test_sdk_client.py -q
........                                                                 [100%]
8 passed in 0.03s

.venv/bin/python -m flake8 src/sdk/client.py tests/test_sdk_client.py
# passed

python3 -m py_compile src/sdk/client.py tests/test_sdk_client.py
# passed

git diff --check
# passed
```

## Patch File

https://github.com/hunki1488-a11y/AIgor/blob/main/submissions/agentorchestration-4214-sdk-timeout.patch

## Submission Status

Ready for PR/claim after explicit confirmation to star the repo, fork/open PR,
and comment `/attempt` + `/claim` on the bounty issue.
