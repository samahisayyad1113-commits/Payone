# PayOne Stage 72

Start with `docs/STAGE72_PRODUCTION_READINESS.md` and `docs/STAGE72_TEST_MATRIX.md`.

Run the static preflight: `python3 scripts/check_stage72_release_gate.py`
Run the complete release gate: `bash scripts/run_stage72_release_gate.sh`.

The complete gate is expected to remain HOLD until Maven verification and the external database/provider/security evidence are available. Do not use live credentials or enable real-money payouts for local testing.
