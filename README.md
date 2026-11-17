# Zephra — Weather Intelligence Dashboard

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-blue?style=for-the-badge&logo=github)](https://mrchandrayee.github.io/zephra-weather-dashboard/)
[![HTML](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)](https://developer.mozilla.org) [![CSS](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)](https://developer.mozilla.org) [![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](./LICENSE)

A responsive vanilla HTML/CSS/JavaScript recreation based on the supplied 5.23-second screen recording (720×1280, 30 fps).

## Run
1. Extract `zephra-weather-dashboard.zip`.
2. Open `index.html` in Chrome, Edge, Firefox or Safari.
3. No npm, build step or external service is required.

Optional local server:
```bash
python3 -m http.server 8000
```
Open `http://localhost:8000`.

## What the video shows

The product is a dark weather dashboard branded **Zephra**. The visible location is **Mainland Portugal**, with the map and forecast focused on **Lisbon**. The dashboard shows **10 Nov 2026, 09:40**.

### Overview
- Current conditions: 15°C, showers, 95% rain
- Wind status: 17 km/h
- UV index: 0.97 (low)
- Humidity: 91%
- Visibility: 24 km
- Feels like 13°C
- Sunrise 07:13; sunset 17:28
- 7-day forecast list
- Animated precipitation / weather condition map

### Radar
- Mainland Portugal map with radar overlay and rain-intensity legend
- Rain / Temperature / Wind / Cloud layer selector
- Play / pause and time scrubber
- Zoom controls and center-on-location action
- Rain readings by city: Lisbon, Sintra, Setúbal, Coimbra, Guarda, Braga, Évora, Porto and Faro
- Rain intensity legend: Light, Moderate, Heavy, Extreme

### Forecast
- Hour by hour, Next 10 days and Rain views
- Day selector: Today, Tomorrow, Thursday, Friday and Saturday
- 24-hour temperature curve
- Hourly condition strip
- 10-day list and rainfall intensity charts

### Other navigation
- Maps
- Alerts (sample yellow rain warning for Lisbon)
- History (24 hours / 7 days / 30 days)
- Settings for units, location and notifications

Also includes sidebar collapse, light/dark appearance, responsive layout and keyboard Space to play/pause the radar animation.

## Source code visible in the video

The code editor is labelled `src/wx.ts`. The readable function is `rainAt`, which estimates rain rate at a point by summing contributions from moving rain cells:

```ts
// rain rate (mm/h) at a point, from a set of live cells:
// each one drifts on the wind, swells, peaks and fades
export function rainAt(
  cells: Cell[], lat: number, lon: number, t: number,
) {
  let v = 0;
  for (const c of cells) {
    const env = Math.sin(Math.PI * (t - c.ts) / (c.te - c.ts));
    const dt = (t - c.ts) / HOUR;
    const dy = (lat - (c.lat + c.vLat * dt)) / c.ry;
    const dx = ((lon - (c.lon + c.vLon * dt)) * COSLAT) / c.rx;
    const d2 = dx * dx + dy * dy;
    if (d2 < 9) v += c.peak * Math.pow(env, 0.8) * Math.exp(-d2);
  }
  return v;
}
```

The recording only displays this function excerpt, not the full original TypeScript application. The runnable project recreates the visible dashboard and simulates the rain layer locally; it does not claim to recover the unseen original repository.

## Demo limitations
- Weather values, forecasts, warnings, map boundaries and rain cells are illustrative sample data based on the visible UI.
- The radar animation is a local CSS simulation; it is not connected to a meteorological feed or live radar provider.
- No GPS, geolocation, weather API, alerts service or forecast backend is connected.
- Changing the location changes the displayed label only; it does not fetch weather for that location.

## 👨‍💻 Author

**mrchandrayee** · [GitHub](https://github.com/mrchandrayee)

## 📄 License

[MIT License](./LICENSE)
