## 1.6.1

- Fix: notification cron job (timer-based reminders) was never installed or started — `run.sh` now runs `notification:cron:setup` and starts `crond` when notifications are enabled, matching upstream's `docker-startup.sh`
- Fix: generated secrets (`ENC_HASH`, `NOTIFICATION_CRON_SECRET`, etc.) were written to `/app/env/.env`, which is wiped on every container recreate. Moved to `/share/sprout-track/env/.env` so they persist alongside the database they protect. Previously, any addon rebuild silently broke decryption of the admin password and VAPID keys.

## 1.0.0

- Initial release of standalone HA addon
- Pulls latest Sprout Track image from Docker Hub (sprouttrack/sprout-track:latest)
- Persistent database storage at /share/sprout-track/
- "Open Web UI" button on the addon page
- Push notification support via HTTPS + VAPID
