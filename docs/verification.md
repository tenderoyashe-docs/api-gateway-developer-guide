# Verification plan and test evidence

This page separates **documented expected behavior** from **observed test results**. Do not claim the full gateway example has been verified until Docker checks have been performed.

## Local upstream tests

Run:

```bash
python -m unittest discover -s tests -v
```

The unit tests exercise `GET /health`, `GET /items`, valid item creation, and invalid request handling.

## Gateway smoke checks

These checks run in the `docs-ci` GitHub Actions workflow on every push. To run them locally, with Docker running:

```bash
docker compose up -d --build
bash scripts/smoke-test.sh
```

| Test | Expected | Observed |
| --- | --- | --- |
| Authenticated GET `/inventory/items` | 200 | 200 — PASS |
| GET without API key | 401 | 401 — PASS |
| GET with invalid API key | 401 | 401 — PASS |
| POST valid item | 201 | 201 — PASS |
| POST without `name` | 400 | 400 — PASS |
| Unknown upstream endpoint | 404 | 404 — PASS |
| Authenticated health check | 200 | 200 — PASS |

**Run details:** 9 November 2026, [GitHub Actions docs-ci run #42709452481](https://github.com/tenderoyashe-docs/api-gateway-developer-guide/actions/runs/42709452481), Kong 3.9.4, Docker 29.4.2, Docker Compose 5.1.3. Both the validation and gateway smoke-test jobs passed.

These are observed results from the linked GitHub Actions run. Check future CI runs after changing the application, gateway configuration or documentation.

## Review checklist

- Commands can be copied from Markdown and run without undocumented setup.
- Every endpoint in the reference appears in `openapi.yaml`.
- The declared status codes match the upstream handler and gateway behavior.
- Examples distinguish gateway errors from upstream errors.
- All keys and URLs are labeled as local demonstration values.
- Link paths and document navigation are valid.

## Environment limitations

This repository provides a runnable configuration, but a passing Python test suite **does not prove** the gateway routes or authentication work. Those must be verified with Docker.
