## 2026.7.0

- Addon version now uses CalVer (`YYYY.M.PATCH`, matching Home Assistant core's own scheme) instead of mirroring upstream Sprout Track's version number. Previously the addon version tracked upstream's 1:1 (e.g. 1.6.0 = 1.6.0), but wrapper-only fixes in 1.6.1/1.6.2 bumped the addon past upstream's actual version (still 1.6.0), breaking that assumption and risking a real collision later. The addon version no longer implies any particular upstream version — check the changelog for which upstream release a given addon version bundles.

## 1.6.2

- Fix: `run-notification-cron.sh` (and other bundled scripts) hardcode `/app/.env` as their default env file location, which no longer exists after 1.6.1 moved secrets to `/share/sprout-track/env/.env`. Cron would fail every run with "NOTIFICATION_CRON_SECRET is not set". `run.sh` now symlinks `/app/.env` to the persisted file so these scripts resolve it transparently.

## 1.6.1

- Fix: notification cron job (timer-based reminders) was never installed or started — `run.sh` now runs `notification:cron:setup` and starts `crond` when notifications are enabled, matching upstream's `docker-startup.sh`
- Fix: generated secrets (`ENC_HASH`, `NOTIFICATION_CRON_SECRET`, etc.) were written to `/app/env/.env`, which is wiped on every container recreate. Moved to `/share/sprout-track/env/.env` so they persist alongside the database they protect. Previously, any addon rebuild silently broke decryption of the admin password and VAPID keys.

## 1.0.0

- Initial release of standalone HA addon
- Pulls latest Sprout Track image from Docker Hub (sprouttrack/sprout-track:latest)
- Persistent database storage at /share/sprout-track/
- "Open Web UI" button on the addon page
- Push notification support via HTTPS + VAPID
