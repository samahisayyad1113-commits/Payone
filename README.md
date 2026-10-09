# PayOne — Stage 52 Unified Repository (Integration Baseline)

This repository consolidates the Stage 37 backend baseline, Stage 38–42 full-snapshot updates, and Stage 43–51 incremental modules into one directory tree.

## What this is
- One consolidated source tree for backend, Flutter customer-app components, admin UI, database migrations, scripts, and stage documentation.
- A starting point for integration review and build verification.
- A test-mode/development baseline, **not a production-certified application**.

## Repository layout
- `backend/` — Spring Boot backend, PostgreSQL/Flyway migrations, tests
- `flutter/` — customer-app source/components
- `admin-ui/` — admin portal static UI
- `docs/` — stage notes, contracts, and integration audit
- `scripts/` — local checks and UAT helper scripts
- `docker-compose.yml` — local development services
- `.env.example` — configuration template; do not place real secrets in source control

## Local verification
1. Install JDK 21, Maven 3.9+, Docker Compose, and (for Flutter work) a compatible Flutter SDK.
2. Copy `.env.example` to `.env` and fill only test credentials. Never use live gateway secrets for local testing.
3. Run the repository structural check:
   ```bash
   bash scripts/check_repo.sh
   ```
4. Run backend tests from `backend/`:
   ```bash
   mvn test
   ```
5. Run application integration/UAT tests against a disposable PostgreSQL instance before relying on any end-to-end flow.

The included `backend/mvnw` script is not a self-contained Maven Wrapper; use installed Maven unless a real Maven Wrapper is added.

## Critical integration status
The source files have been overlaid in stage order, but this consolidation has **not** been verified by a complete Maven build, Flutter build, Docker startup, or end-to-end test run in this environment. Presence of files does not prove that every module is correctly wired together. Review `docs/STAGE52_INTEGRATION_AUDIT.md` before further development.

## Payment-only and wallet models
PayOne is intended to support both:
1. **Payment-only** transactions through an external payment gateway.
2. **Stored-value wallet** top-ups and spending, behind a disabled-by-default launch gate until the exact business model, RBI requirements, authorization/regulated partner arrangements, and operational controls are confirmed.

Do not enable real-money wallet credits or debits for customers based only on this code bundle. Payment gateway integration by itself is not authorization to operate a stored-value wallet. Keep test-mode credentials and test transactions separate from live systems.

## Suggested next milestone
Stage 53 should establish a reproducible build, run the full unit/integration suite, verify Flyway migrations against a clean database, and resolve the critical findings in the Stage 52 integration audit before additional feature development.


## Stage 53 verification additions
Run `bash scripts/verify_stage53.sh` to perform the local verification steps available in your environment. The CI workflow is in `.github/workflows/backend-verification.yml`. Read `docs/STAGE53_VERIFICATION_REPORT.md` for known release blockers. A complete build and integration test must be run in an environment with Maven and PostgreSQL/Docker; do not infer test success from the existence of these scripts.


## Stage 54 financial contract fixes
Stage 54 adds migration V13 and updates financial posting/settlement services to align with the V1 ledger schema. Read `docs/STAGE54_FINANCIAL_FIXES.md` for assumptions and required integration tests. These changes have not been validated by a live PostgreSQL test or a successful Maven build.


## Stage 55 financial integrity tests
PostgreSQL/Testcontainers tests were added for posting idempotency, rollback, and settlement posting. Read `docs/STAGE55_FINANCIAL_INTEGRITY_TESTS.md`, especially the Flyway V7 checksum warning. Tests have not been run in this artifact-building environment.


## Stage 56 security hardening
Admin API routes now require `ROLE_ADMIN`; customer payment-intent input is validated; webhook HMAC verifier tests were added. Read `docs/STAGE56_SECURITY_HARDENING.md` for required deployment settings and limitations. Build/tests were not run in the artifact-building environment.


## Stage 57 merchant/agent commissions
Adds partner registry, commission rules, idempotent accrual, admin APIs, and a PostgreSQL/Testcontainers test. Read `docs/STAGE57_MERCHANT_AGENT_COMMISSION.md` for limitations. Build and database tests remain unverified.

## Stage 58: Commission settlement & payout orchestration

See `docs/STAGE58_COMMISSION_SETTLEMENT.md` for the payout-batch schema, admin API outline, reconciliation safeguards, and verification limitations. The code records payout intent only; it does not transfer money.


## Stage 64: Live provider adapter scaffold
This stage adds fail-closed live-payout flags and readiness criteria. It does not send live transfers; see `docs/STAGE64_LIVE_PROVIDER_ADAPTER_SCAFFOLD.md`. Run `python3 scripts/check_stage64.py` for static checks.

## Stage 65: RazorpayX test-mode HTTP adapter

Adds `RazorpayXTestModeHttpClient`, an opt-in HTTP adapter for test-mode credentials only. See `docs/STAGE65_RAZORPAYX_TESTMODE_ADAPTER.md` and run `python3 scripts/check_stage65.py`. It is not auto-wired and does not enable live transfers. Maven/JUnit tests and provider sandbox UAT remain unverified.

## Stage 66 — Cashfree Payouts sandbox adapter
See `docs/STAGE66_CASHFREE_PAYOUTS_TESTMODE.md`. The adapter is sandbox-only, not wired into the production router, and not verified against a live Cashfree sandbox. Run `python3 scripts/check_stage66.py` for static checks. Do not enable real-money payouts.


## Stage 67
See `docs/STAGE67_UNIFIED_MULTI_PROVIDER.md` for the unified payout dispatch scaffold, limitations, and verification status.

## Stage 69 — Payout Monitoring

Read-only payout monitoring endpoints and dashboard are available in `admin-ui/payout-monitoring.html`. See `docs/STAGE69_PAYOUT_MONITORING.md`. Keep these endpoints behind the admin role policy. The dashboard does not initiate transfers. Static contract checks: `python3 scripts/check_stage69_contracts.py`.
