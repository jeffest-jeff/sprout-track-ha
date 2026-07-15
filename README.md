# Sprout Track — Home Assistant Addon

A Home Assistant addon for [Sprout Track](https://github.com/Oak-and-Sprout/sprout-track), a self-hosted baby activity tracker.

## What it does

Runs the Sprout Track web app as a Home Assistant addon. Log feedings, diapers, sleep, measurements, milestones, medicine, and more — with a full calendar, reports, growth charts, and push notifications. Data is stored persistently in `/share/sprout-track/` on your HA host.

## Installation

1. In Home Assistant, go to **Settings → Add-ons → Add-on Store**
2. Click the three-dot menu → **Repositories**
3. Add: `https://github.com/jeffest-jeff/sprout-track-ha`
4. Find **Sprout Track** in the store and install it
5. Start the addon and click **OPEN WEB UI**

## First Run

The Setup Wizard will guide you through creating your family, setting a security PIN, and adding your baby's profile.

Default Family Manager password: `admin` — change this on first login at `/family-manager`.

## Sidebar

Add Sprout Track to the HA sidebar by adding this to `configuration.yaml` and restarting HA:

```yaml
panel_iframe:
  sprout_track:
    title: Sprout Track
    icon: mdi:baby-carriage
    url: http://homeassistant.local:3333
    require_admin: false
```

## Push Notifications

Push notifications require HTTPS (e.g. via the Cloudflare Tunnel addon). Once HTTPS is set up:

1. Go to `/family-manager` → Settings → Push Notifications → generate VAPID keys
2. Subscribe each device you want to receive notifications
3. Add a HA automation to trigger the notification check every minute:

```yaml
# configuration.yaml
rest_command:
  sprout_track_notification_check:
    url: "http://homeassistant.local:3333/api/notifications/cron"
    method: POST
    headers:
      Authorization: "Bearer YOUR_NOTIFICATION_CRON_SECRET"
      Content-Type: "application/json"
```

Find `NOTIFICATION_CRON_SECRET` in `/share/sprout-track/.env`.

## Configuration Options

| Option | Default | Description |
|--------|---------|-------------|
| `auth_life` | `86400` | Session lifetime in seconds (default: 24 hours) |
| `idle_time` | `604800` | Idle logout timeout in seconds (default: 7 days) |
| `enable_notifications` | `true` | Enable push notification infrastructure |

## Data

- Database: `/share/sprout-track/baby-tracker.db`
- Secrets/config: `/share/sprout-track/.env`

Include `/share/sprout-track/` in your HA backup configuration to protect your data.

## Upstream

This addon wraps the [Sprout Track](https://github.com/Oak-and-Sprout/sprout-track) project and pulls the latest release image from Docker Hub (`sprouttrack/sprout-track:latest`). No affiliation with the upstream project.

---

> This addon and repository were developed with the assistance of [Claude Code](https://claude.ai/code) by Anthropic.
