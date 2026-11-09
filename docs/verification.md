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
| Authenticated GET `/inventory/items` | 200 | Not yet recorded |
| GET without API key | 401 | Not yet recorded |
| GET with invalid API key | 401 | Not yet recorded |
| POST valid item | 201 | Not yet recorded |
| POST without `name` | 400 | Not yet recorded |
| Unknown upstream endpoint | 404 | Not yet recorded |
| Authenticated health check | 200 | Not yet recorded |

**Run details:** Not yet recorded.

After the first successful `docs-ci` run, replace each `Not yet recorded` with the observed result and record the workflow run link, date, Docker and Kong versions.

## Review checklist

- Commands can be copied from Markdown and run without undocumented setup.
- Every endpoint in the reference appears in `openapi.yaml`.
- The declared status codes match the upstream handler and gateway behavior.
- Examples distinguish gateway errors from upstream errors.
- All keys and URLs are labeled as local demonstration values.
- Link paths and document navigation are valid.

## Environment limitations

This repository provides a runnable configuration, but a passing Python test suite **does not prove** the gateway routes or authentication work. Those must be verified with Docker.
