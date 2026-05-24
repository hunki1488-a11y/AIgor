# Sprint Preview: github-unified-mcp#174 Runtime Parity

Lead: https://github.com/vinicius-ssantos/github-unified-mcp/issues/174

## Proposed Paid Sprint

Price: `250 USD`

Turnaround: `48h` after scope confirmation

Deliverable: focused PR that closes the Python runtime parity slice from PR 2+3 cleanup.

## Current Finding

The issue asks for CI/CD hardening and production cleanup. A low-risk first slice is already visible:

- GitHub Actions test/release jobs use Python `3.11`.
- Render production pins Python `3.13.5`.
- Render build currently installs `.[dev,redis,observability]`, which pulls development tooling into production deploys.

That means CI can pass against a different minor Python version than production, and production installs test/release dependencies it does not need.

## Patch Shape

The prepared local patch does three things:

- Update `.github/workflows/ci.yml`, `.github/workflows/preview.yml`, and `.github/workflows/release.yml` from Python `3.11` to `3.13`.
- Change `render.yaml` build command from `pip install -e ".[dev,redis,observability]"` to `pip install -e ".[redis,observability]"`.
- Add a short `docs/CI_CD.md` note explaining why CI uses Python `3.13` and why Render excludes dev extras.

## Verification

Static verification completed:

```sh
rg -n "python-version: '3\\.11'|\\[dev,redis,observability\\]" .github/workflows render.yaml
```

Expected result: no output.

## Acceptance Criteria

- CI, preview, and release workflows use the same Python minor line as production Render.
- Render build installs runtime extras only.
- CI/CD docs explain the split between CI/release tooling and runtime deploy dependencies.
