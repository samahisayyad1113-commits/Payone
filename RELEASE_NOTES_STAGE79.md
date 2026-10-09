# PayOne Stage 79 — CI Evidence Collection

- Split source compilation from the full Maven verification step.
- Added a PostgreSQL service for CI tests.
- Added a static workflow checker and local preflight script.
- Configured build/test report artifact upload with `if: always()` so failure evidence is retained.
- Added run instructions and caveats.

Verification boundary: local static checks only. GitHub Actions execution, Maven compilation, JUnit tests, and PostgreSQL integration tests have not been run in this artifact-building environment.
