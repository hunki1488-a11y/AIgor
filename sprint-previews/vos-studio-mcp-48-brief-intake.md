# VOS Studio MCP #48 Brief Intake Sprint Preview

Target issue: https://github.com/vinicius-ssantos/vos-studio-mcp/issues/48

Public patch: https://github.com/hunki1488-a11y/AIgor/blob/main/submissions/vos-studio-mcp-48-brief-intake.patch

## Scope

This preview implements the `prepare_creative_brief` / brief intake wizard as a deterministic, compact MCP tool.

Included:

- `BriefIntakeInput`, `BriefQuestion`, and `BriefIntakeResponse` Pydantic schemas
- `prepare_creative_brief` service with no provider calls and no paid side effects
- MCP tool registration in `tools/__init__.py`
- missing-information extraction for objective, timing/budget, and claims/compliance gaps
- compact sprint handoff payload for `create_creative_sprint`
- service tests and tools-layer registration/delegation test

## Acceptance Criteria Coverage

- New Pydantic input/output schemas: covered.
- Deterministic structured output: covered by pure composition service.
- MCP tool registered in the server/tool layer: covered.
- Compact output aligned with ADR-0011: covered by bounded lists and handoff summary.
- Tests for minimal/missing information and constrained brand/compliance inputs: covered.
- No provider execution or paid side effect: covered by service design.

## Verification

Commands run locally against a fresh clone:

```bash
.venv/bin/python -m pytest tests/services/test_brief_service.py tests/tools/test_tools_layer.py -q
.venv/bin/python -m py_compile src/vos_studio_mcp/schemas/brief.py src/vos_studio_mcp/services/brief_service.py src/vos_studio_mcp/tools/prepare_creative_brief.py tests/services/test_brief_service.py tests/tools/test_tools_layer.py
git diff --check
```

Result:

```text
23 passed in 0.59s
py_compile passed
git diff --check passed
```

## Paid Sprint Shape

Low-risk paid scope:

- 250 USD: patch cleanup, maintainer-aligned acceptance notes, and PR-ready review pass.
- 500 USD: open PR, adjust implementation to maintainer preferences, and handle first review iteration.

Turnaround target: 48 hours after scope confirmation.
