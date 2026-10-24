# Family Hub — iPad-first Home Assistant dashboard

This folder is the start of a **personal family command center** built around the Daylight Calendar Card already in this repository. It is meant to run in Home Assistant on a 9th-generation iPad today, then on a larger Raspberry Pi-connected touchscreen later **without rewriting the dashboard**.

The calendar card itself remains untouched. We compose it with Home Assistant's built-in weather, to-do, thermostat and camera cards. The configuration uses a responsive Sections view: the calendar spans two sections on larger screens, and the layout stacks as space gets tighter.

## What is included

- `dashboard.yaml` — a complete three-view Lovelace dashboard: **Family**, **Security**, and **Home**.
- Week-at-a-glance family calendar, interactive chores/reminders and grocery lists.
- Daily weather, Nest thermostat placeholder and Ring camera placeholder.
- iPad/iPhone compatibility through the Home Assistant companion app or a browser; same URL on a future Pi kiosk.

**This is a configuration scaffold, not a finished live installation.** Device and entity IDs are examples and must be replaced in your own Home Assistant instance. The project does not contain account credentials, and this commit does not connect to Ring, Google, or Nest.

## Before you start

1. Run **Home Assistant** on an always-on machine (Pi, mini PC, NAS or VM). The iPad is only the display, not the server. Do not buy a 27-inch monitor for this first phase.
2. Install **Daylight Calendar Card** through HACS or as a manual JavaScript resource (see the repo's original [README](../README.md)). Its registered card type is `custom:daylight-calendar-card`. Don't load both the upstream and forked JS resources at once.
3. In **Settings → Devices & services**, set up the integrations you actually use:
   - Google Calendar (or another calendar integration) for your calendar entities.
   - A weather provider with a `weather.*` entity.
   - Ring for supported camera/event entities. The cloud-based integration has device- and subscription-dependent limitations.
   - Nest **only if you have a supported Nest thermostat**. Google Device Access setup may require a fee.
   - **Local To-do** (or any to-do integration) for a new `Family Tasks` list; Home Assistant's Shopping List usually provides `todo.shopping_list`.
4. In **Developer Tools → States**, locate the actual `calendar.*`, `todo.*`, `weather.*`, `climate.*` and `camera.*` entity IDs. Replace the example IDs in `dashboard.yaml`. Remove any card for a device you don't own.

## Use it on your iPad

**Option A — easiest for a first test (storage-managed dashboard):**

1. In Home Assistant, create a new empty dashboard called **Family Hub**.
2. Open that dashboard, edit it, open the **Raw configuration editor** from the dashboard's menu, and paste the contents of [dashboard.yaml](dashboard.yaml).
3. Save. Visit the dashboard in the Home Assistant iPad app or Safari, then switch the iPad to landscape orientation.
4. Use the iOS app's kiosk/full-screen options as desired. Keep the iPad charging safely for permanent wall use.

**Option B — configuration as code (YAML-mode dashboard):**

Copy this folder to your Home Assistant config directory, so the dashboard is located at `<config>/family-hub/dashboard.yaml`. Add a new YAML-mode dashboard to `configuration.yaml`:

```yaml
lovelace:
  dashboards:
    family-hub:
      mode: yaml
      title: Family Hub
      icon: mdi:tablet-dashboard
      show_in_sidebar: true
      filename: family-hub/dashboard.yaml
```

If you already have a `lovelace:` section, **merge** these keys into it rather than defining another one. Check configuration and restart Home Assistant as appropriate for a YAML dashboard update. Don't copy the full `dashboard.yaml` under `configuration.yaml` directly.

## Ring and Nest caveats

- The included Ring camera card uses `camera_view: auto` to avoid maintaining a continuous cloud stream. Tapping the camera opens Home Assistant's camera details; whether live view works depends on the model and integration. Replace the sample `camera.front_door_live_view` with your actual Ring camera entity.
- Ring is cloud-dependent and two-way audio in Home Assistant's live view is not supported by the official integration. Recordings can require Ring Protect.
- Nest climate controls require a compatible thermostat and the Nest integration to be configured. If you have Nest devices but **no thermostat**, remove the `thermostat` cards until we know which entities you have.
- **Do not commit** private camera URLs, long-lived Home Assistant tokens, passwords, Ring credentials, Google OAuth secrets or `secrets.yaml` to this repository. Its GitHub visibility may be public. Keep those values in Home Assistant.

## Future development

- Confirm the specific Ring devices, available Nest devices, and real Home Assistant entity IDs.
- Tune the calendar/card proportions after viewing the layout on the iPad.
- Add doorbell-triggered notifications and optional camera popups after event entity and notification target names are known.
- Add kid-friendly chores, reminders, and meal planning.
- Switch the same dashboard to a 24–27-inch touchscreen with a Raspberry Pi kiosk browser.

Useful references: [Home Assistant Sections](https://www.home-assistant.io/dashboards/sections/), [To-do list card](https://www.home-assistant.io/dashboards/todo-list/), [Ring integration](https://www.home-assistant.io/integrations/ring/), [Nest integration](https://www.home-assistant.io/integrations/nest/).
