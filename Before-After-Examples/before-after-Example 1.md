---
title: "Before/After Example: Get Weather Data Function"
author: "Urveesha Pandoo"
---


## Initial Prompt

```
Write a function that retrieves weather forecast data for a location using the Open-Meteo API. The function should support retrieving a forecast for up to 5 days.
```

## Initial AI-Generated Code (Before)

```python
def get_weather_data(location):
    import requests
    import datetime

    url = "https://api.open-meteo.com/v1/forecast"
    lat, lon = geocode_location(location)
    today = datetime.date.today().isoformat()
    end_date = (datetime.date.today() + datetime.timedelta(days=4)).isoformat()
    params = {
        'latitude': lat,
        'longitude': lon,
        'daily': 'temperature_2m_max,temperature_2m_min,precipitation_sum',
        'timezone': 'auto',
        'start_date': today,
        'end_date': end_date,
        'current_weather': True
    }
    response = requests.get(url, params=params)
    return response.json()

```

## My Analysis of the Initial Code

Problems and Explanation:
1. No error handling- Fails silently if the API or network fails
2. No input validation-	Could break if location is empty or invalid
3. Raw data return-	Returns raw JSON which is hard to work with
4. No structured forecast-	No post-processing of daily data
5. No documentation-	Lacks explanation of arguments or return values

## My Follow-up Prompt

```
Please improve the function to:

1. Add error handling and validation for the location.

2. Process and structure the forecast output for each day.

3. Support a forecast_days parameter (max 5).

4. Add clear docstrings explaining inputs and outputs.
```

## Improved AI-Generated Code (After)

```python
def get_weather_data(location: str, forecast_days: int = 5) -> Dict[str, Any]:
    """
    Retrieve structured weather forecast data from Open-Meteo API.

    Args:
        location (str): Location name (e.g., "Berlin").
        forecast_days (int): Number of forecast days (1–5). Defaults to 5.

    Returns:
        dict: Structured dictionary containing location info, current weather,
              and a list of daily forecast dictionaries.
    """
    import requests, datetime

    # Clamp forecast_days to [1, 5]
    forecast_days = min(max(forecast_days, 1), 5)

    # Convert location to coordinates
    lat, lon = geocode_location(location)

    # Calculate date range
    today = datetime.date.today()
    end_date = today + datetime.timedelta(days=forecast_days - 1)

    # Fetch data from Open-Meteo API
    raw_data = fetch_weather(lat, lon, today.isoformat(), end_date.isoformat())

    # Prepare structured result
    result = {
        'location': location,
        'latitude': lat,
        'longitude': lon,
        'current': raw_data.get('current_weather', {}),
        'forecast': []
    }

    # Extract daily forecast
    daily = raw_data.get('daily', {})
    for date, t_max, t_min, precip in zip(
        daily.get('time', []),
        daily.get('temperature_2m_max', []),
        daily.get('temperature_2m_min', []),
        daily.get('precipitation_sum', [])
    ):
        result['forecast'].append({
            'date': date,
            'temp_max': t_max,
            'temp_min': t_min,
            'precipitation': precip
        })

    return result
```

## Why My Prompting Strategy Was Effective

Strengths and	Why It Helped:
1. Clear expectations- I asked for specific, actionable improvements.
2. Technical detail-	Requested input validation, error handling, and data processing.
3. UX-conscious-	I wanted a user-friendly output rather than raw API data.
4. Documentation-	I ensured the function can be reused by others easily and more comprehensible.
