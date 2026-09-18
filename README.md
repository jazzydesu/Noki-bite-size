# Noki — Sunset & Sunrise Quality Prediction App

Noki is your personal sunset/sunrise quality predictor that tells you exactly how beautiful your next twilight moment will be. Whether you're planning a romantic evening walk, a photography session, or just want to know if today's sunset will be worth watching, Noki gives you the real answer: a simple 0-100 score with detailed explanations of why.

## Preview Images

![Preview 1](frontend/public/preview-1.png)
![Preview 2](frontend/public/preview-2.png)
![Preview 3](frontend/public/preview-3.png)

kinda cool right? :3

## The Two Parts

There is the terminal part — the Python backend (`sunset_score/`) that runs in a terminal, takes weather data, and produces the score. That is the engine.

Then there is the actual app — the React frontend (`frontend/`) with CSS, UI design, mobile layout, and the interface people actually touch. That is what gets deployed. The terminal produces the prediction. The app makes it visible.

Both exist. They are separate. They work together.

## App Flowchart

```mermaid
flowchart TD
  subgraph group_experience["User Experience"]
    node_react_entry["React Entry<br/>[main.jsx]"]
    node_app_shell["App Shell<br/>[App.jsx]"]
    node_home_page["Home Screen<br/>[Home.jsx]"]
    node_forecast_page["Forecast Screen<br/>[Forecast.jsx]"]
    node_saved_page["Saved Spots<br/>[Saved.jsx]"]
    node_settings_page["Settings Screen<br/>[Settings.jsx]"]
    node_location_picker["Location Picker<br/>[LocationPicker.jsx]"]
  end

  subgraph group_frontend_state["Frontend State"]
    node_app_context["App Context<br/>[AppContext.jsx]"]
    node_device_storage[("Device Storage<br/>[AppContext.jsx]")]
    node_frontend_client["Score Client<br/>[scorer.js]"]
  end

  subgraph group_prediction["Prediction Backend"]
    node_api_endpoint["Score API<br/>[api.py]"]
    node_weather_client["Weather Client<br/>[weather_client.py]"]
    node_score_engine["Six-Factor Scorer<br/>[scorer.py]"]
    node_forecast_builder["Forecast Builder<br/>[api.py]"]
    node_time_formatter["Time Formatter<br/>[cli.py]"]
    node_score_cache[("Score Cache<br/>[cache.py]")]
  end

  subgraph group_integrations["External Integrations"]
    node_open_meteo["Open-Meteo"]
    node_place_geocoder["Place Geocoder"]
    node_browser_location["Device Location"]
  end

  node_user(("Sky Watcher"))

  node_user -->|"uses"| node_app_shell
  node_react_entry -->|"renders"| node_app_shell
  node_app_shell -->|"provides state"| node_app_context
  node_app_shell -->|"routes"| node_home_page
  node_app_shell -->|"routes"| node_forecast_page
  node_app_shell -->|"routes"| node_saved_page
  node_app_shell -->|"routes"| node_settings_page
  node_home_page -->|"opens picker"| node_location_picker
  node_forecast_page -->|"opens picker"| node_location_picker
  node_location_picker -->|"selects location"| node_app_context
  node_app_context -->|"requests coordinates"| node_browser_location
  node_app_context -->|"resolves place"| node_place_geocoder
  node_app_context -->|"reads preferences"| node_device_storage
  node_app_context -->|"writes spots"| node_device_storage
  node_app_context -->|"fetches score"| node_frontend_client
  node_frontend_client -->|"requests forecast"| node_api_endpoint
  node_frontend_client -->|"returns result"| node_app_context
  node_api_endpoint -->|"reads cache"| node_score_cache
  node_api_endpoint -->|"fetches weather"| node_weather_client
  node_weather_client -->|"requests conditions"| node_open_meteo
  node_api_endpoint -->|"scores event"| node_score_engine
  node_api_endpoint -->|"builds outlook"| node_forecast_builder
  node_api_endpoint -->|"formats time"| node_time_formatter
  node_api_endpoint -->|"saves score"| node_score_cache
  node_home_page -->|"displays result"| node_app_context
  node_forecast_page -->|"reads forecast"| node_app_context

  click node_react_entry "https://github.com/jazzydesu/noki/blob/main/frontend/src/main.jsx"
  click node_app_shell "https://github.com/jazzydesu/noki/blob/main/frontend/src/App.jsx"
  click node_home_page "https://github.com/jazzydesu/noki/blob/main/frontend/src/pages/Home.jsx"
  click node_forecast_page "https://github.com/jazzydesu/noki/blob/main/frontend/src/pages/Forecast.jsx"
  click node_saved_page "https://github.com/jazzydesu/noki/blob/main/frontend/src/pages/Saved.jsx"
  click node_settings_page "https://github.com/jazzydesu/noki/blob/main/frontend/src/pages/Settings.jsx"
  click node_location_picker "https://github.com/jazzydesu/noki/blob/main/frontend/src/LocationPicker.jsx"
  click node_app_context "https://github.com/jazzydesu/noki/blob/main/frontend/src/context/AppContext.jsx"
  click node_device_storage "https://github.com/jazzydesu/noki/blob/main/frontend/src/context/AppContext.jsx"
  click node_frontend_client "https://github.com/jazzydesu/noki/blob/main/frontend/src/scorer.js"
  click node_api_endpoint "https://github.com/jazzydesu/noki/blob/main/src/sunset_score/api.py"
  click node_weather_client "https://github.com/jazzydesu/noki/blob/main/src/sunset_score/weather_client.py"
  click node_score_engine "https://github.com/jazzydesu/noki/blob/main/src/sunset_score/scorer.py"
  click node_forecast_builder "https://github.com/jazzydesu/noki/blob/main/src/sunset_score/api.py"
  click node_time_formatter "https://github.com/jazzydesu/noki/blob/main/src/sunset_score/cli.py"
  click node_score_cache "https://github.com/jazzydesu/noki/blob/main/src/sunset_score/cache.py"

  classDef toneNeutral fill:#f8fafc,stroke:#334155,stroke-width:1.5px,color:#0f172a
  classDef toneBlue fill:#dbeafe,stroke:#2563eb,stroke-width:1.5px,color:#172554
  classDef toneAmber fill:#fef3c7,stroke:#d97706,stroke-width:1.5px,color:#78350f
  classDef toneMint fill:#dcfce7,stroke:#16a34a,stroke-width:1.5px,color:#14532d
  classDef toneRose fill:#ffe4e6,stroke:#e11d48,stroke-width:1.5px,color:#881337
  classDef toneIndigo fill:#e0e7ff,stroke:#4f46e5,stroke-width:1.5px,color:#312e81
  classDef toneTeal fill:#ccfbf1,stroke:#0f766e,stroke-width:1.5px,color:#134e4a
  classDef toneTeal fill:#ccfbf1,stroke:#0f766e,stroke-width:1.5px,color:#134e4a
  classDef node_react_entry,node_app_shell,node_home_page,node_forecast_page,node_saved_page,node_settings_page,node_location_picker toneBlue
  classDef node_app_context,node_device_storage,node_frontend_client toneAmber
  classDef node_api_endpoint,node_weather_client,node_score_engine,node_forecast_builder,node_time_formatter,node_score_cache toneMint
  classDef node_open_meteo,node_place_geocoder,node_browser_location toneRose
  classDef node_user toneIndigo
```

## What Noki Actually Does

Noki takes several pieces of weather data that normally don't mean much on their own and puts them together into one prediction.

It looks at six factors:

- Total cloud cover
- Mid/high-level cloud structure
- Low cloud cover near the horizon
- Humidity
- Visibility
- Atmospheric timing and conditions

The point is not simply to produce a number.

If Noki gives you 85/100, you should be able to look at the result and understand why.

Maybe there is enough high cloud to catch the light.

Maybe the lower atmosphere is clear enough to leave the horizon visible.

Maybe visibility is excellent.

Or maybe the conditions are terrible and Noki tells you not to bother.

That's the idea.

## Features

- Real-time location
- Six-factor sunset/sunrise analysis
- 0–100 quality prediction
- Explanations behind the prediction
- Mobile-optimized interface
- No account required

## Project Structure

- `frontend/` - React app (CSS, UI, mobile layout)
- `src/` - Python backend (terminal tool, scoring engine)
- `tests/` - Test suite

## Quick Start

```bash
# Terminal backend
$env:PYTHONPATH="src"
uvicorn sunset_score.api:app --reload

# Frontend app
cd frontend
npm run dev
```

## Setup

1. Create and activate a virtual environment:

   On macOS/Linux:
   ```bash
   python -m venv venv
   source venv/bin/activate
   ```

   On Windows (PowerShell):
   ```bash
   python -m venv venv
   .\venv\Scripts\Activate.ps1
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Set pythonpath:
   ```bash
   export PYTHONPATH=src
   ```

## Run the API Server

```powershell
$env:PYTHONPATH="src"
uvicorn sunset_score.api:app --reload
```

Check that the server is running:

```bash
curl http://127.0.0.1:8000/health
```

Request a sunset score:

```bash
curl "http://127.0.0.1:8000/score?lat=37.7749&lon=-122.4194"
```

An invalid latitude returns a validation error:

```bash
curl "http://127.0.0.1:8000/score?lat=91&lon=-122.4194"
```

Score responses are cached locally for 60 minutes by rounded location. The
cache is stored in `sunset_score.db`, which is created automatically and is
safe to delete whenever you want to clear it.

## How to Run

Pass latitude and longitude as command-line arguments to the CLI module:

```bash
python -m sunset_score.cli 37.7749 -122.4194
```

Example Output:
```text
Sunset Quality Prediction
-------------------------
Sunset Time: 7:52 PM
Score: 85/100

Notes:
 - 50% total cloud cover is in the productive range
 - Good mid/high cloud canvas (60%) structure for color
 - Low cloud cover is light (10%), keeping the horizon clear
 - Optimal humidity (45%) for vibrant colors
 - Excellent visibility (20.0 km)
```

## How to Run Tests

Run `pytest` from the project root:

```bash
pytest
```
