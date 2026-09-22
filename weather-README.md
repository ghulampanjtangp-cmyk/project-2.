# Isobar — Weather Lookup

A single-page weather app that looks up current conditions for any city using the [OpenWeatherMap Current Weather Data API](https://openweathermap.org/current). Built as one self-contained `index.html` — no build step, no framework, no dependencies beyond a Google Fonts link.

https://ghulampanjtangp-cmyk.github.io/project-1/

## What's included

- **City search** — type any city name and get its current weather.
- **Current conditions displayed:** temperature (°C), "feels like," humidity, wind speed, pressure, a text description of the condition, and a matching hand-drawn icon (clear / cloudy / rain / drizzle / thunderstorm / snow / mist).
- **Invalid city handling** — a city that doesn't match anything returns a clear inline message ("No city found matching '...'") instead of a broken or blank screen. A rejected/invalid API key and network failures are also handled with their own specific messages.
- **Last-updated timestamp** — shown using the `dt` field from the API response (the time OpenWeatherMap calculated that reading), not just the time you clicked search.
- **Visible API source** — OpenWeatherMap is credited in the page footer and linked, in addition to being documented below.

## API used

**OpenWeatherMap — Current Weather Data API**
Docs: https://openweathermap.org/current
Endpoint used:
```
GET https://api.openweathermap.org/data/2.5/weather?q={city}&units=metric&appid={API_KEY}
```

### Sample response

A successful response for a city (`q=Faisalabad`) looks like this (trimmed to the fields the app uses):

```json
{
  "coord": { "lon": 73.0798, "lat": 31.4187 },
  "weather": [
    { "id": 800, "main": "Clear", "description": "clear sky", "icon": "01d" }
  ],
  "main": {
    "temp": 34.2,
    "feels_like": 33.1,
    "humidity": 18,
    "pressure": 1005
  },
  "wind": { "speed": 3.1 },
  "dt": 1758540000,
  "sys": { "country": "PK" },
  "name": "Faisalabad",
  "cod": 200
}
```

An unknown city (`q=asdkjaskjd`) returns:

```json
{
  "cod": "404",
  "message": "city not found"
}
```

The app checks `res.ok` / the HTTP status on every request and shows a specific message for the `404` (unknown city) and `401` (bad API key) cases, rather than a generic error.

## Getting an API key

1. Create a free account at [openweathermap.org](https://openweathermap.org/api).
2. Under **API keys**, copy your default key (or generate a new one).
3. New keys can take a few minutes to a couple of hours to activate — if you get a 401 error right after signing up, wait a bit and try again.
4. Open the app, expand **"API key setup"** at the top, paste your key, and click **Save**. It's stored only in your browser's `localStorage` — it is never sent anywhere except directly to OpenWeatherMap's API.

The free tier (60 calls/minute) is more than enough for this app.

## Running it locally

Just open `index.html` in a browser. No build step needed.

To serve it instead (useful for testing on other devices on your network):
```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploying (for the live demo link)

**GitHub Pages:**
1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under "Build and deployment," set **Source** to "Deploy from a branch," pick `main` and `/ (root)`.
4. Save, wait a minute, then refresh — your live URL will be `https://<your-username>.github.io/<repo-name>/`.

Note: each visitor needs to paste their own free API key the first time they use the deployed app, since the key isn't baked into the file.

## Taking screenshots

Once running (locally or deployed):
- Capture the **empty/initial state** (before a search).
- Capture a **successful search** (e.g. a valid city with its full weather panel showing).
- Capture the **error state** by searching an invalid city name, e.g. `asdkjaskjd`.
- Save them into a `screenshots/` folder in the repo and reference them here, e.g.:
  ```markdown
  ![Empty state](screenshots/empty.png)
  ![Weather result](screenshots/result.png)
  ![Invalid city error](screenshots/error.png)
  ```

## Tech stack

Plain HTML, CSS, and vanilla JavaScript (`fetch`). Weather icons are hand-built inline SVGs (no external icon library or image requests), mapped from OpenWeatherMap's `weather[0].main` field. Fonts (Space Grotesk, IBM Plex Sans, IBM Plex Mono) load from Google Fonts.

## License

Use freely for your own project.
