# Sprout Track

A self-hosted baby activity tracker. Log feedings, diapers, sleep, measurements, milestones, medicine, and more — with a full calendar, reports, growth charts, and push notifications.

## First Run

On first access, the **Setup Wizard** will guide you through:

1. **Family setup** — family name and URL slug
2. **Security setup** — system-wide PIN or per-caretaker PINs
3. **Baby setup** — name, birth date, and feeding/diaper thresholds

Click **OPEN WEB UI** on the addon page, or navigate to `http://<your-ha-host>:3000`.

Default admin password for the Family Manager (`/family-manager`): **`admin`**

## Configuration

| Option | Default | Description |
|--------|---------|-------------|
| `auth_life` | `86400` | How long a login session stays valid, in seconds (default: 24 hours) |
| `idle_time` | `604800` | How long before an idle session is logged out, in seconds (default: 7 days) |
| `enable_notifications` | `true` | Enable push notification infrastructure. Notifications must also be configured in the Family Manager after HTTPS is set up. |

## Data Storage

The SQLite database is stored at `/share/sprout-track/` on your Home Assistant host. This directory persists across addon updates and restarts.

- `baby-tracker.db` — main application database
- `baby-tracker-logs.db` — API request logs (if logging is enabled)

**Back up your data** regularly using the backup option in the Family Manager settings, or include `/share/sprout-track/` in your HA backup configuration.

## HA Sidebar Shortcut

To add Sprout Track to the Home Assistant sidebar, add the following to your `configuration.yaml` and restart HA:

```yaml
panel_iframe:
  sprout_track:
    title: Sprout Track
    icon: mdi:baby-carriage
    url: http://homeassistant.local:3000
    require_admin: false
```

Replace `homeassistant.local` with your HA host IP or hostname if needed.

## Push Notifications

Push notifications require HTTPS. To enable them:

1. Set up HTTPS access (via Cloudflare Tunnel or a reverse proxy with a valid certificate)
2. Open the Family Manager at `/family-manager` → **Settings** → **Push Notifications**
3. Generate VAPID keys and configure the notification schedule
4. Add a HA automation to trigger the notification check every minute (see below)

### Triggering notifications from HA

Add to `configuration.yaml`:

```yaml
rest_command:
  sprout_track_notification_check:
    url: "http://homeassistant.local:3000/api/notifications/cron"
    method: POST
    headers:
      Authorization: "Bearer YOUR_NOTIFICATION_CRON_SECRET"
      Content-Type: "application/json"
```

Find `NOTIFICATION_CRON_SECRET` in `/share/sprout-track/.env`.

Then create a HA automation with a **Time Pattern** trigger set to every 1 minute (`/1`) that calls `rest_command.sprout_track_notification_check`.

## Cloudflare Tunnel (Remote Access)

If you use the [Cloudflare addon](https://github.com/brenner-tobias/addon-cloudflared) for remote access, add a public hostname pointing to `http://homeassistant.local:3000` in the Zero Trust dashboard.

## Support

- GitHub: [https://github.com/jeffest-jeff/sprout-track-ha](https://github.com/jeffest-jeff/sprout-track-ha)
- Upstream project: [https://github.com/Oak-and-Sprout/sprout-track](https://github.com/Oak-and-Sprout/sprout-track)
