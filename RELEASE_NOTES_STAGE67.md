# Stage 67 release notes

Added a shared payout dispatch service and V20 migration for persistent provider selection, idempotency, request fingerprints, beneficiary reference hashing, and dispatch status. Ambiguous network outcomes are recorded as UNKNOWN and blind resubmission is blocked.

Static checks are included. Maven compilation, PostgreSQL integration, Spring bean wiring, provider sandbox requests, and full end-to-end behavior remain unverified. Live transfers must remain disabled.
