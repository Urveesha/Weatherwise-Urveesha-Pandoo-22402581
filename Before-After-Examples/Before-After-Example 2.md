---
title: "Before/After Example 2: Temperature Visyalisation Function and Precipitation visualisation Function"
author: "Urveesha Pandoo"
---


## Initial Prompt

```
Write two Python functions that visualize weather forecast data: one for temperature using a line chart, and one for precipitation using a bar chart.
```

## Initial AI-Generated Code (Before)

```python
def create_temperature_visualisation(weather_data):
    plt.plot(weather_data['dates'], weather_data['temperature'])
    plt.show()

def create_precipitation_visualisation(weather_data):
    plt.bar(weather_data['dates'], weather_data['precipitation'])
    plt.show()

```

## My Analysis of the Initial Code

Problems and Explanation:
1. Assumes ideal input-	Assumes input is already extracted and formatted
2. No axis labels or titles-	Poor chart readability
3. No layout adjustment-	Labels might overlap
4. No customization-	No color or style choices
5. No flexibility-	Cannot be reused or integrated into a larger UI

## My Follow-up Prompt

```
Please rewrite the functions to:

1. Extract date, temp_max, temp_min, and precipitation from structured weather_data.

2. Add titles, axis labels, and grid lines.

3. Use color and styling to improve readability.

4. Allow output_type='figure' to return the plot instead of showing it immediately.
```

## Improved AI-Generated Code (After)

```python
# --- TEMPERATURE VISUALIZATION FUNCTION ---
def create_temperature_visualisation(weather_data, output_type='display'):
    """
    Plots a line chart showing max and min temperature forecast.

    Args:
        weather_data (dict): Dictionary containing weather forecast with 'forecast' list.
        output_type (str): 'display' shows the plot, 'figure' returns the figure object.

    Returns:
        matplotlib.figure.Figure or None
    """
    dates, temp_max, temp_min = _extract_forecast_data(weather_data, ['date', 'temp_max', 'temp_min'])
    fig, ax = plt.subplots(figsize=(10, 5))
    ax.plot(dates, temp_max, label='Max Temp (°C)', marker='o', color='crimson')
    ax.plot(dates, temp_min, label='Min Temp (°C)', marker='o', color='royalblue')
    ax.fill_between(dates, temp_min, temp_max, color='lightgray', alpha=0.3)
    ax.set_title(f"Temperature Forecast for {weather_data.get('location', 'Unknown')}")
    ax.set_xlabel("Date")
    ax.set_ylabel("Temperature (°C)")
    ax.legend()
    ax.grid(True, linestyle='--', alpha=0.5)
    plt.xticks(rotation=45)
    plt.tight_layout()
    return fig if output_type == 'figure' else plt.show()

# --- PRECIPITATION VISUALIZATION FUNCTION ---
def create_precipitation_visualisation(weather_data, output_type='display'):
    """
    Plots a bar chart showing daily precipitation forecast.

    Args:
        weather_data (dict): Dictionary containing weather forecast with 'forecast' list.
        output_type (str): 'display' to show the plot or 'figure' to return it.

    Returns:
        matplotlib.figure.Figure or None
    """
    forecast = weather_data.get('forecast', [])
    if forecast:
        dates, precipitation = zip(*[
            (day.get('date', 'Unknown'), day.get('precipitation') or 0)
            for day in forecast if 'date' in day
        ])
    else:
        dates, precipitation = ([], [])
    fig, ax = plt.subplots(figsize=(10, 5))
    ax.bar(dates, precipitation, color='deepskyblue', width=0.6)
    ax.set_title(f"Precipitation Forecast for {weather_data.get('location', 'Unknown')}")
    ax.set_xlabel("Date")
    ax.set_ylabel("Precipitation (mm)")
    ax.grid(axis='y', linestyle='--', alpha=0.5)
    plt.xticks(rotation=45)
    plt.tight_layout()
    return fig if output_type == 'figure' else plt.show()
```

## Why My Prompting Strategy Was Effective

Strengths and Why It Helped
1. Focused on visual clarity- Clear labeling, colors, and layout improve user comprehension
2. Reusability-	The output_type parameter makes the functions reusable in dashboards
3. Thoughtful defaults-	Gracefully handles missing precipitation data
4. Goal-oriented-	Prompt aimed to produce code suitable for both scripts and notebooks
