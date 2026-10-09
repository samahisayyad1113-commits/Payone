# Release Notes — Stage 65

- Added an opt-in RazorpayX test-mode HTTP client for create payout and lookup by provider payout ID.
- Added provider idempotency header, test-key guard, INR validation, and fail-closed handling of ambiguous outcomes.
- Added four unit tests and a static contract checker.
- No Cashfree HTTP adapter added in this stage; it remains a local simulator from Stage 63/64.
- No live transfers are enabled.
- Static checker is the only verification performed here. Maven compilation/tests and provider UAT were not run.
