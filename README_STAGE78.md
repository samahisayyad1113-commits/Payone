# PayOne Stage 78 — Build Environment Triage

Stage 78 records the first actual attempt to bootstrap Maven and distinguishes infrastructure failure from source-code failure. It adds an offline environment/CI check and a triage runner that classifies DNS/network failures separately from compiler/test failures.

## Run offline checks

```bash
python3 scripts/check_stage78_environment.py
```

## Attempt the build and capture diagnostics

```bash
bash scripts/run_stage78_build_triage.sh
```

Outputs are saved under `build-verification-stage78/`.

## Current known blocker

The Stage 77 build attempt failed before compilation because the environment could not resolve `archive.apache.org`. Java 21 was available, but Maven was not installed. See `docs/stage78/ACTUAL_BUILD_ATTEMPT.md`.

## CI route

The repository already includes `.github/workflows/backend-verification.yml`, which sets up Java 21 and runs static checks plus `mvn verify` on a hosted runner. A successful workflow run is still required before claiming that the backend compiles or its tests pass.

## Safety

This stage does not change payment or payout business logic. Keep real-money payouts disabled until a network-enabled build, database tests, provider sandbox tests, security review, and financial reconciliation checks have passed.
