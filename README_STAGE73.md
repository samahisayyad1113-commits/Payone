# PayOne Stage 73 — Unified Build Fixes & Automated Integration Testing

This package extends Stage 72 with a repeatable integration verification gate, CI static checks, and an end-to-end test plan.

## Run locally

```bash
python3 scripts/check_stage73_integration.py
bash scripts/run_stage73_integration_gate.sh
```

The full gate exits with HOLD/failure if Maven is unavailable or build/tests fail. Install Maven 3.9+ (or configure a real Maven Wrapper) and Docker for Testcontainers-based PostgreSQL tests.

## What this stage verifies statically

- Spring Boot/Java 21 Maven project and Testcontainers dependencies exist.
- Flyway versions V1–V23 are unique and contiguous.
- Java test suite and PostgreSQL integration tests exist.
- CI config includes PostgreSQL and Maven verification and invokes Stage 73 checks.
- Payout live mode is not explicitly enabled by default.

## What it does not claim

Static checks are not a successful build. In the environment used to prepare this ZIP, Java 21 is available but Maven is unavailable, so Java compilation, JUnit/Testcontainers execution, Flyway runtime migration, and provider sandbox calls remain unverified. See `docs/STAGE73_BUILD_VERIFICATION_REPORT.md` and `docs/STAGE73_INTEGRATION_TEST_PLAN.md`.

Keep live payouts disabled until the full build and integration suite passes, provider sandbox evidence is recorded, and all applicable security, financial, provider, and regulatory reviews are approved.
