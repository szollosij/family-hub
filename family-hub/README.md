# Family Hub — iPad-first dashboard

A calmer, Skylight-inspired Home Assistant dashboard: **calendar first, helpful information second**. This configuration builds on the Daylight Calendar Card in this repository without modifying its JavaScript. Your 9th-gen iPad displays the dashboard today; a larger touchscreen can open the same dashboard later.

## Design

- **Calendar:** an uncluttered light monthly calendar takes two of three columns on larger screens.
- **At a glance:** a soft sage-tinted rail with weather, reminders, groceries and an optional Nest summary.
- **Security:** Ring camera view is isolated from the everyday calendar.
- **Home:** thermostat and family lists.
- **Responsive:** Home Assistant's Sections layout reflows the cards on smaller screens. On the iPad, use landscape and collapse the navigation sidebar to give the calendar more room.

The color palette is warm ivory (`#F8F7F3`), sage (`#E8F0EA`), charcoal green (`#263C37`) and muted blue. Standard Home Assistant cards still inherit the active HA theme. For closer control, an **optional** `theme.yaml` is provided.

## Calendar palette (important)

The default Daylight Calendar Card header uses Home Assistant's `--primary-color`, which may be bright cyan even when the surrounding dashboard is light. Our dashboard **overrides this directly** in the card configuration:

```yaml
header_color: "#ECF1EB"      # muted sage header
header_text_color: "#263C37" # charcoal text/buttons
header_background_opacity: 0
grid_color: "#E4EAE3"
colors:
  calendar.family: "#7DA48D" # family event chips
event_color_mode: left-tint
event_tint_opacity: 24
today_style:
  background_color: "#DCEAE3"
  opacity: 0.55
  border_color: "#7FA58D"
  border_width: 1
```

**Pulling code on GitHub does not update the running dashboard.** After any change to `dashboard.yaml`, copy the latest file into Home Assistant's **dashboard-level** Raw configuration editor again and save. If you just want the color fix, open the existing Daylight card code editor and add those appearance keys without replacing its current calendar entity.

The Home Assistant application header (dark slate in the setup screenshot) is separate. To make that background warm too, install and select the optional `theme.yaml` explained below.

**Yellow `Entity not found` warnings** while editing represent not-yet-configured optional Nest, Ring, and to-do entities. Home Assistant shows conditional cards in edit mode even when their conditions normally hide them; click **Done** to see the normal view. Once you connect a service, replace its example entity ID with its real ID.

## Install into the Family Hub dashboard you already created

1. **Install the original Daylight Calendar Card in HACS** (already done during setup). Hard-refresh the browser after installation.
2. Open **Family Hub** in Home Assistant and select the dashboard's edit pencil.
3. Open the top-right **⋮ menu → Raw configuration editor**. On some versions, the raw editor option is under the edit menu.
4. Copy the complete contents of [dashboard.yaml](dashboard.yaml) and paste it into the **dashboard-level** raw configuration editor, replacing the existing Family Hub YAML. **Don't paste the whole file into a Tile card's code editor.**
5. Save and exit edit mode. The old HACS update-status tile on the current Family Hub view will be replaced by the new layout. Your separate **Overview** dashboard is not affected.
6. Confirm that your actual calendar entity is `calendar.family`. Find the exact ID by opening **Developer Tools → States**, filtering for `calendar.`, then updating the `entities` list if necessary.

The entire [dashboard.yaml](dashboard.yaml) is designed to be copied directly into the raw editor. It is **not** `configuration.yaml`.

## Connect features as you go

These sample IDs are placeholders. Find the real ones in Developer Tools → States and update the YAML.

| Feature | Example entity | Needed |
|---|---|---|
| Family calendar | `calendar.family` | Yes, replace if yours differs |
| Weather | `weather.forecast_home` | Optional |
| Reminders and chores | `todo.family_tasks` | Optional |
| Groceries | `todo.shopping_list` | Optional |
| Nest thermostat | `climate.living_room` | Optional, if you own a thermostat |
| Ring live-view camera | `camera.front_door_live_view` | Optional, device-specific |

**Optional cards automatically hide** in normal dashboard view until their example entity has a usable state. Weather, reminders, Ring and Nest have friendly explanatory placeholders. Home Assistant shows conditional cards **even when hidden** during dashboard edit mode; leave edit mode to see the clean design.

To enable reminders, create a `Family Tasks` list using Home Assistant's **Local To-do** integration. For weather, add a weather integration and update its entity. Your Ring live view is on demand; Home Assistant may not support continuous streaming for your device. Nest setup is separate and may require Google's one-time Device Access registration.

## Optional sage + ivory theme

If you want the whole Home Assistant interface to follow the design palette (not only our lightly tinted section), copy [theme.yaml](theme.yaml) to:

```
<HA-config>/themes/family_hub.yaml
```

In `configuration.yaml` under the existing `frontend:` block, enable themes (if not already enabled):

```yaml
frontend:
  themes: !include_dir_merge_named themes
```

Restart Home Assistant and select **Family Hub Warm** from your Home Assistant profile's theme setting. Do not make duplicate `frontend:` blocks.

This is optional; start without the theme first to keep setup simple.

## Future

When you move to a 24–27-inch touch display, run a fullscreen browser on the Raspberry Pi pointing at Home Assistant. The dashboard remains on your Home Assistant server and does not need rewriting.

**Privacy:** Never commit Ring credentials, camera private URLs, Google OAuth secrets, Home Assistant access tokens or `secrets.yaml` here. Your GitHub repository is public unless you make it private.
