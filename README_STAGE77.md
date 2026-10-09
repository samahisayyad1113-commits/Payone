# PayOne Stage 77 — Maven Bootstrap & Build Verification

This stage replaces the prior `backend/mvnw` prerequisite stub with a small bootstrap script that uses system Maven when present or downloads a pinned Apache Maven 3.9.11 distribution into the user's Maven cache. If it downloads the archive, it obtains the adjacent Apache `.sha512` file and verifies the archive before extraction.

## Run offline wrapper contract checks

```bash
python3 scripts/check_stage77_wrapper.py
```

These checks are static only and do not compile Java.

## Run the actual build

```bash
bash scripts/run_stage77_build_verification.sh
```

Requirements: JDK 21, outbound HTTPS access to the Apache Maven archive when Maven is not already installed, and enough disk space. Testcontainers-based tests may additionally require a working Docker daemon. The command saves its output in `build-verification-stage77/`.

## Limitations

- The wrapper is a lightweight bootstrap script, not the official generated Maven Wrapper distribution.
- Downloading Maven depends on the archive URL and network availability at run time.
- This stage does not claim that the Java source compiles, migrations apply, or integration tests pass.
- Review and test the wrapper in your own environment before relying on it for releases.
- Keep real-money payouts disabled until build, database, provider sandbox, security, and ledger tests pass.
