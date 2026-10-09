# PayOne Stage 71 — Alert Notification Delivery & Escalation

## Included
- PostgreSQL-backed email notification outbox (Flyway V23).
- Scheduled escalation enqueue for unresolved critical alerts after 15 minutes and warning alerts after 30 minutes.
- Unique key per alert/escalation/channel/recipient to suppress duplicate notifications.
- `FOR UPDATE SKIP LOCKED` queue claiming for multiple workers.
- SMTP email delivery via Spring Mail, disabled by default.
- Bounded exponential retry, dead-letter state, and safe error-class logging.
- Read-only admin summary endpoint: `GET /api/v1/admin/payout-alert-notifications/summary`.

## Configure email (opt-in)
Set `PAYONE_ALERT_EMAIL_ENABLED=true`, `PAYONE_ALERT_EMAIL_RECIPIENT` to an operations mailbox, and configure `SPRING_MAIL_HOST`, `SPRING_MAIL_PORT`, `SPRING_MAIL_USERNAME`, and `SPRING_MAIL_PASSWORD` through a secrets manager/environment. Do not commit credentials. TLS settings should match the SMTP provider. If mail is not configured, no messages are sent.

## Safety notes
- Notifications contain alert metadata only; they do not include beneficiary bank details or full payout payloads.
- Email is an operational signal, not proof of transfer status. Operators must use authenticated provider status and reconciliation workflows.
- This stage does not submit, retry, settle, or change payout records.
- Email delivery is at-least-once in crash edge cases; downstream mail systems may deliver duplicates. The outbox uniqueness constraint prevents duplicate queue rows, not guaranteed exactly-once email delivery.
- Review alert ownership, recipient access, SMTP rate limits, SPF/DKIM/DMARC, and incident response before enabling.

## Verification
Run `python3 scripts/check_stage71_alert_delivery.py`. Maven compilation, PostgreSQL migrations, scheduler behavior, and SMTP delivery have not been executed in this build environment.
