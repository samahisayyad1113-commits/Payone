# PayOne Stage 76 — Build Verification

Start with `docs/STAGE76_BUILD_VERIFICATION.md`.

1. Run `python3 scripts/check_stage76_structure.py`.
2. Run `bash scripts/run_stage76_build_verification.sh` in an environment with JDK 21 and Maven 3.9+.
3. Review the generated `build-verification/verification-summary.txt` and full Maven log.

Do not enable live payouts on the basis of structural checks alone. Provider, database, ledger, security, and compliance verification remain required.
