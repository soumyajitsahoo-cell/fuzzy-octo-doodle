# fuzzy-octo-doodle
# Minimalist Glass New Tab

A standalone, zero-dependency new tab interface built with vanilla HTML, Tailwind CSS, and optical liquid glass styling over Hokusai's *The Great Wave off Kanagawa*.

## Optical & Visual Architecture

The visual layer is derived from the Convx / Kyant0 Android AGSL liquid glass system rather than standard web frosted glass:

- **White Luminance Lift**: Real glass reflects ambient illumination instead of absorbing it. Rather than dark-tinted smoke (`rgba(0,0,0,0.6)`), the surface uses a +15% white lift (`rgba(255, 255, 255, 0.15)` dark / `rgba(245, 245, 247, 0.32)` light), keeping the underlying artwork vibrant.
- **Low Blur Density + High Chrominance**: Uses an 8px (`3dp`) blur radius combined with `saturate(170%)` and `brightness(106%)`. Background brushstrokes and textures remain legible through the glass without turning into milky opacity.
- **Dual-Rim Edge Boundary**: Simulates physical perimeter refraction through a 1px border (`rgba(255, 255, 255, 0.18)`), an inset specular top pluck (`inset 0 1.2px 1px 0 rgba(255, 255, 255, 0.40)`), and an inset chamfer bevel (`inset 0 -1px 1px 0 rgba(0, 0, 0, 0.35)`).
- **Selection Architecture**: Global `user-select: none` across widgets, clock, and tiles prevents accidental browser selection rectangles during clicks and drags. Native OS text selection and highlight are strictly preserved in editable input fields (`#search-input`, `#cmd-input`, modals).
- **Motion Restraint**: Zero rubbery wobbles. Interactive buttons feature a physical 100ms tactile press response (`scale(0.96)`). Modals enter via a 180ms ease-out bridge (`scale(0.96)` to `scale(1)`). Hover transforms are gated behind `@media (hover: hover) and (pointer: fine)` to prevent sticky states on touch devices, and background animations honor `@media (prefers-reduced-motion: reduce)`.

## Features

- **Hero Clock**: Tabular numeric display with instant 12/24-hour switching via click.
- **Omnibox Search**:
  - Localhost & Port Recognition: Queries matching `localhost`, `127.0.0.1`, or `host:port` route directly via `http://` instead of searching.
  - Quick Engine Switching: Cycle search engines (Google, Perplexity, DuckDuckGo, GitHub, YouTube) by pressing `Tab` inside the search bar or typing engine prefix hotkeys (`g `, `p `, `d `, `gh `, `yt `).
  - Live Suggestion Dropdown: Debounced suggestions fetched directly via JSONP with keyboard arrow selection.
- **Shortcut Grid**:
  - Real-time website favicon extraction via Google S2 with DuckDuckGo fallback and letter avatar generation.
  - In-place modal editor to add, modify, or remove bookmarks.
- **Weather Widget**:
  - Open-Meteo integration (no API key required).
  - Geolocation auto-detection with BigDataCloud reverse geocoding, IP fallback, and manual city search modal.
  - Temperature, daily highs/lows, condition icon, and humidity.
- **Reading List**:
  - Local scratchpad to store URLs for later reading with hostname extraction and quick-delete controls.
- **Command Palette (`Cmd/Ctrl+K`)**:
  - Quick action launcher for Zen Focus mode, Theme toggle, and bookmark navigation.
  - Integrated math engine evaluating arithmetic, roots, and trigonometry (`144 * 12`, `sqrt(256)`, `pi * 4`). Pressing Enter copies the evaluation result to the clipboard.

## Keyboard Shortcuts

| Shortcut | Action |
| --- | --- |
| `Cmd + K` / `Ctrl + K` | Open Command Palette & Calculator |
| `/` | Focus search bar (when outside input fields) |
| `Tab` | Cycle search engine (while inside search bar) |
| `1` – `8` | Open shortcut bookmark 1 through 8 directly |
| `Escape` | Close active modal, dropdown suggestions, or clear search input |
| `Arrow Up` / `Arrow Down` | Navigate search suggestions or command palette items |
| `Enter` | Submit query, launch selected suggestion, or copy math calculation |

## Local Storage Keys

All configuration is client-side and offline-first:

- `newtab_theme`: `'dark'` or `'light'`
- `newtab_focus_mode`: `'true'` or `'false'` (hides bottom widgets)
- `newtab_24hour`: `'true'` or `'false'`
- `newtab_engine_idx`: active search engine index (`0`–`4`)
- `newtab_shortcuts`: JSON array of custom shortcut objects
- `newtab_reading_list`: JSON array of saved reading links
- `newtab_weather_lat`, `newtab_weather_lon`, `newtab_weather_city`, `newtab_weather_cache`: cached weather parameters

## Setup & Deployment

The page has no build step and requires no local package manager:

1. Open `index.html` directly in any Chromium, WebKit, or Gecko browser.
2. Ensure `background.jpg` remains in the same directory.
3. To configure as a default new tab in Chrome/Edge, load the directory as an unpacked extension or use a new tab redirect extension pointing to the local file path.
