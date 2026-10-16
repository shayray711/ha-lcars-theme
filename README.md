# LCARS Themes for Home Assistant

Four dark, Star Trek LCARS-style themes for Home Assistant dashboards, including a Pi-hole-style console look. Condensed Antonio font, all-caps labels, pill buttons and curved "elbow" panels, written from scratch.

| Theme | Look |
|---|---|
| **Picard LCARS** | Black background with soft blues, golds and reds |
| **TNG LCARS** | Black background with orange, peach, lavender and light blue |
| **LCARS Console** | Black background in the Pi-hole LCARS colours: blue, peach, salmon and pink, with cyan, red, amber and green tiles |
| **Star Trek LCARS** | The Console layout in classic TNG colours (orange, peach, lavender, light blue) with thicker elbows and pill-shaped tiles and buttons |

![Star Trek LCARS on a Home Assistant dashboard](images/star-trek-lcars.png)

*Star Trek LCARS: the same dashboard in orange, peach and lavender.*


![LCARS Console on a Home Assistant dashboard](images/lcars-console-home.png)

*LCARS Console: the Home dashboard with coloured stat tiles, elbow-style panel headings and the readout strip.*

![LCARS Console on a floor page](images/lcars-console-floor.png)


All themes have:

- The condensed **Antonio** font, self-hosted on your Home Assistant (nothing loads from Google).
- **ALL-CAPS** headings and labels.
- **Pill-shaped** tiles and buttons with a coloured end cap.
- **LCARS "elbows"**: a thick coloured side bar and top bar on each panel, joined by a large curved corner.
- **Colour families** you can give individual panels (network, Pi-hole and battery are built in), plus large LCARS-style number readouts.

Everything here was written from scratch. There are no Star Trek logos, artwork or copied stylesheets.

---

## Requirements

- Home Assistant 2024.10 or newer
- [card-mod](https://github.com/thomasloven/lovelace-card-mod) (install it through HACS → Frontend). It's needed for the shapes and all-caps text. Without it you still get the colours and font.
- Themes enabled in `configuration.yaml`:

  ```yaml
  frontend:
    themes: !include_dir_merge_named themes
  ```

  If you already have a `frontend:` block, add the `themes:` line under it. Don't add a second `frontend:`.

## Installation

### With HACS

1. HACS → ⋮ → **Custom repositories** → add this repository's URL with type **Theme**.
2. Find **LCARS Themes (Picard & TNG)** in HACS and click **Download**.
3. Install the font (below), then in **Developer tools → Actions**, run `frontend.reload_themes`.

### Manually

1. Copy `themes/picard-lcars.yaml` to `/config/themes/picard-lcars/picard-lcars.yaml`.
2. Install the font (below).
3. Run `frontend.reload_themes`, or restart Home Assistant.

### Font (both methods)

HACS doesn't install fonts for themes, so this step is manual:

1. Copy the `www/fonts/antonio/` folder from this repository to `/config/www/fonts/antonio/`. If `/config/www` didn't exist before, restart Home Assistant once.
2. Go to **Settings → Dashboards → ⋮ → Resources → Add resource**:
   - URL: `/local/fonts/antonio/antonio.css?v=1`
   - Type: **Stylesheet**
3. Refresh your browser.

## Using the theme

Click your **user name** at the bottom of the sidebar. On the **General** tab, under **Theme**, choose **Picard LCARS** or **TNG LCARS**, and set the mode to **Dark**. This is saved per browser and device.

If a dashboard view has a fixed `theme:` set, it overrides your profile choice. Remove it from the view to use the theme from your profile.

### Colour families and big readouts

Every LCARS shape takes its colour from `--lcars-accent`. To put a card in a colour family, add:

```yaml
card_mod:
  style: |
    ha-card { --lcars-accent: var(--lcars-network); }
```

Built-in families: `--lcars-network`, `--lcars-pihole` and `--lcars-battery`. Each has a lighter partner (`--lcars-network-2` and so on) and there's an alert colour, `--lcars-alert`.

To make a large LCARS number readout from an **Entity** card:

```yaml
type: entity
entity: sensor.example_battery
name: Example
card_mod:
  style: |
    ha-card { --lcars-accent: var(--lcars-battery); }
    .info .value { font-size: var(--lcars-readout-size, inherit); line-height: 1; color: var(--lcars-accent, inherit); }
    .info .measurement { color: var(--lcars-battery-2, inherit); }
```

These styles use `var(..., inherit)` fallbacks, so the same cards look normal when someone uses a non-LCARS theme.

## Credits and licences

- Inspired by (not copied from) the community **Pi-hole LCARS themes**, and by the LCARS look of *Star Trek: The Next Generation* and *Star Trek: Picard*. This is an independent fan project, not affiliated with or endorsed by CBS Studios, Paramount or the Pi-hole project. "Star Trek" and "LCARS" are trademarks of their respective owners and are used here descriptively.
- **Antonio** font © The Antonio Project Authors, licensed under the SIL Open Font License 1.1 (see `www/fonts/antonio/OFL.txt`).
- Uses [card-mod](https://github.com/thomasloven/lovelace-card-mod) by Thomas Lovén.

## LCARS Console (Pi-hole style)

The third theme is meant for a dashboard that looks like the Pi-hole LCARS web interface.

- It sets the palette, the Antonio font, slim coloured panel edges, elbow-style headings and dark text boxes and pop-ups.
- It also defines colour variables you can use in your own cards, with a normal fallback for other themes: `--lcars-tile-cyan`, `--lcars-tile-red`, `--lcars-tile-amber`, `--lcars-tile-green`, `--lcars-tile-text`, and banner variables `--lcars-banner-*`.
- For the coloured stat tiles you need [button-card](https://github.com/custom-cards/button-card). Example:

  ```yaml
  type: custom:button-card
  name: Inside temperature
  entity: sensor.example_temperature
  show_state: true
  styles:
    card:
      - background: var(--lcars-tile-cyan, #2a3350)
      - color: var(--lcars-tile-text, #e8ecf6)
      - border-radius: 6px
  ```
- This repository is the **theme only**. The dashboard layouts depend on each person's own entities, so they aren't included.
- Optional: [apexcharts-card](https://github.com/RomRider/apexcharts-card) for bar charts and donuts.
- Home Assistant doesn't let a theme restyle the sidebar into big coloured buttons, so that part of the Pi-hole look isn't possible with a theme alone.
