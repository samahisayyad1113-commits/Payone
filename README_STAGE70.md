# PayOne Stage 70 — Payout Alerts & Incident Management

This incremental package adds persistent operational alerts for payout incidents.

## Download and inspect
- Migration: `backend/src/main/resources/db/migration/V22__payout_operational_alerts.sql`
- Service: `backend/src/main/java/com/payone/payoutprovider/alerts/PayoutAlertService.java`
- Admin controller: `backend/src/main/java/com/payone/payoutprovider/alerts/PayoutAlertAdminController.java`
- Static checker: `scripts/check_stage70_alerts.py`
- Report: `docs/STAGE70_PAYOUT_ALERTS.md`

## Alert types
- `UNKNOWN_PAYOUTS`: unknown transfer outcomes exceed configured threshold.
- `RECOVERY_QUEUE_AGE`: oldest open/in-progress recovery task exceeds age threshold.
- `WEBHOOK_FAILURES`: rejected or failed webhook events exceed a 15-minute threshold.

The default alert evaluation thresholds are 1 unknown payout, 900 seconds of recovery queue age, and 5 webhook failures in 15 minutes. They can be passed to the evaluate endpoint; this stage does not persist threshold configuration.

## API endpoints
- `POST /api/v1/admin/payout-alerts/evaluate`
- `GET /api/v1/admin/payout-alerts`
- `POST /api/v1/admin/payout-alerts/{id}/acknowledge`
- `POST /api/v1/admin/payout-alerts/{id}/resolve`

All endpoints must be protected by administrator authentication. Production should derive the actor from the authenticated principal rather than trusting the request body.

## Verification status
The 8 static contract checks pass. Maven compilation, database migration execution, PostgreSQL integration tests, and end-to-end tests have not been run. No email/SMS/Slack notifications are sent. No transfer is submitted or retried by this stage.
