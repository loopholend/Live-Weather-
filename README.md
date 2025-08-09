# Weatherly — HTML + CSS + JS Weather App

A single‑file weather application built with vanilla HTML, CSS, and JavaScript. It uses the free Open‑Meteo APIs (no API key required) to provide city search with suggestions, geolocation, unit toggling (°C/°F), and a dynamic light/dark UI that adapts to weather and user preference.

## Features

- City search with debounced autosuggestions
- “My location” via browser geolocation
- Current weather: temperature, feels like, description, humidity, wind speed/direction, pressure, precipitation
- Unit toggle: °C ↔ °F
- Theme modes: Auto, Dark, Light (persisted in localStorage)
- Dynamic background palette based on weather code and day/night
- Keyboard navigation in suggestions (Arrow Up/Down, Enter, Escape)
- Accessible semantics and focus states
- Zero dependencies, zero build step

## Live preview

- Open `index.html` in a modern browser, or serve over a small local server (recommended).

## Quick start

1) Download or clone the project.

2) Open `index.html` in your browser.

3) Recommended: serve over a local static server to avoid fetch/CORS issues some browsers impose on `file://` pages.

- Python
  ```bash
  python3 -m http.server 8000
  # Visit http://localhost:8000
  ```

- Node (http-server)
  ```bash
  npx http-server .
  # Visit the printed URL (e.g., http://127.0.0.1:8080)
  ```

- VS Code (Live Server)
  ```
  Install "Live Server" extension, then right-click index.html -> "Open with Live Server"
  ```

## Usage

- Start typing a city (e.g., “Indore”, “Paris”). Suggestions appear; use mouse or keyboard to select.
- Press Enter to pick the highlighted suggestion or the top result.
- Click “📍 My location” to use your current coordinates (you’ll be asked to allow location access).
- Toggle units with °C/°F buttons.
- Toggle theme with 🌓 Auto → 🌙 Dark → ☀️ Light (mode is saved across visits).

## Project structure

```
.
└── index.html   # All HTML, CSS, and JS in one file
```

## How it works

- Geocoding (city search)
  - Endpoint: `https://geocoding-api.open-meteo.com/v1/search?name={q}&count=8&language=en&format=json`
  - The app debounces input (300ms) and uses `AbortController` to cancel stale requests.

- Reverse geocoding (for “My location” label)
  - Endpoint: `https://geocoding-api.open-meteo.com/v1/reverse?latitude={lat}&longitude={lon}&count=1&language=en&format=json`

- Weather (current conditions)
  - Endpoint: `https://api.open-meteo.com/v1/forecast`
  - Params:
    - `latitude`, `longitude`
    - `current`: `temperature_2m,apparent_temperature,is_day,precipitation,weather_code,wind_speed_10m,wind_direction_10m,relative_humidity_2m,pressure_msl`
    - `temperature_unit`: `celsius` or `fahrenheit`
    - `wind_speed_unit`: `kmh` or `mph`
    - `precipitation_unit`: `mm`
    - `timezone`: `auto`

- Theming
  - Weather code maps to a readable description and a palette.
  - Theme mode:
    - Auto: uses weather palette; night palettes apply a dark text color and body dark class
    - Dark: forces a night palette
    - Light: forces a light palette

## Accessibility

- Semantic regions: header, main, footer
- ARIA attributes for listbox/options, status messages for “Searching…”/“No results”
- Focus-visible styles on interactive elements
- Keyboard support for suggestions

## Troubleshooting

- No suggestions appear
  - Ensure you have an internet connection.
  - Some browsers limit network requests from `file://` pages. Use a local server as recommended above.

- “My location” not working
  - Your browser might block or require permission for geolocation on unsecured origins. Use `http://localhost` or host on HTTPS.

- No results for specific cities
  - Try alternate spellings or include region (e.g., “Paris, France”) — the API is case-insensitive but benefits from disambiguation.

## Browser support

- Latest Chrome, Edge, Firefox, Safari
- Requires Fetch API and `AbortController` for best experience (fallback is graceful, but modern browsers are recommended)

## Customization

- Colors and spacing: edit CSS variables at the top of `index.html` (`:root` and `.theme-dark`).
- Debounce delay: search debounce is set to 300ms in JS — adjust as needed.
- Fields shown: update the “current” parameters in the weather fetch if you want to add/remove metrics.

## Deploy

- GitHub Pages, Netlify, Vercel, or any static host will work. Just serve `index.html` and any assets (this project is a single file).

## License

MIT — feel free to use and modify.
```

