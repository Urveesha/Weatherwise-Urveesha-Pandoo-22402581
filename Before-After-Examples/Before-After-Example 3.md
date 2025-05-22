---
title: "Before/After Example: User Interface"
author: "Urveesha Pandoo"
---


## Initial Prompt

```
Create an interactive weather app in a Jupyter notebook using IPyWidgets that:

- Takes user input (city)

- Accepts natural language weather questions

- Displays forecast charts (temperature and precipitation)

- Shows responses based on current and forecasted weather
```

## Initial AI-Generated Code (Before)

```python
import ipywidgets as widgets
from IPython.display import display

city_input = widgets.Text(placeholder='Enter city')
submit_button = widgets.Button(description='Submit')

def on_click(b):
    print("Fetching weather for:", city_input.value)

submit_button.on_click(on_click)
display(city_input, submit_button)
```

## My Analysis of the Initial Code

Problems and 	Explanation
1. Minimal interface	Only handles city input, not forecast or natural questions
2. No layout formatting	Buttons, input fields lack structure or design
4. No error handling	No feedback if input is invalid or API fails
5. No connection to visualization	No temperature or precipitation plotting
6. No stylistic design	UI is functional but lacks visual appeal
   
## My Follow-up Prompt

```
Please design a UI with:

1. Separate input boxes for city and natural language questions

2. Buttons to: "Ask Weather", "Show Temperature", "Show Precipitation"

3. Color-coded buttons for better appearance (Ask = Blue, Temperature = Green, Precipitation = Orange for quick recognition)

4. Custom button colors for better visual feedback and branding

5. Visual output area for displaying results and plots

6. Error handling for missing input or failed API calls

7. Responsive layout using VBox and HBox
```

## Improved AI-Generated Code (After)

```python
# --- IPYWIDGETS UI INTEGRATION ---

# Widgets
location_input = widgets.Text(
    description='📍 Location:',
    placeholder='e.g., Sydney',
    style={'description_width': 'initial'}
)

question_input = widgets.Text(
    description='💬 Ask:',
    placeholder='e.g., Will it rain in Berlin tomorrow?',
    style={'description_width': 'initial'}
)

ask_button = widgets.Button(
    description='Ask Weather',
    button_style='',
    layout=widgets.Layout(width='150px')
)
ask_button.style.button_color = '#3498db'

temp_button = widgets.Button(
    description='Show Temperature',
    button_style='success',
    layout=widgets.Layout(width='150px')
)

precip_button = widgets.Button(
    description='Show Precipitation',
    button_style='',
    layout=widgets.Layout(width='180px')
)
precip_button.style.button_color = '#f39c12'

output_area = widgets.Output()

# --- HANDLERS FOR BUTTONS ---
def on_ask_clicked(b):
    with output_area:
        clear_output()
        question = question_input.value.strip()
        parsed = parse_weather_question(question)
        if not parsed['location']:
            print("❌ Please specify a location.")
            return
        try:
            data = get_weather_data(parsed['location'])
            print(generate_weather_response(parsed, data))
        except Exception as e:
            print(f"⚠️ {e}")

def on_temp_clicked(b):
    with output_area:
        clear_output()
        loc = location_input.value.strip()
        if not loc:
            print("❌ Enter a location.")
            return
        try:
            data = get_weather_data(loc)
            create_temperature_visualisation(data)
        except Exception as e:
            print(f"⚠️ {e}")

def on_precip_clicked(b):
    with output_area:
        clear_output()
        loc = location_input.value.strip()
        if not loc:
            print("❌ Enter a location.")
            return
        try:
            data = get_weather_data(loc)
            create_precipitation_visualisation(data)
        except Exception as e:
            print(f"⚠️ {e}")

# Link buttons to handlers
ask_button.on_click(on_ask_clicked)
temp_button.on_click(on_temp_clicked)
precip_button.on_click(on_precip_clicked)

# --- LAYOUT AND DISPLAY ---
ui = widgets.VBox([
    widgets.HTML("<h2>☁️ Weather Assistant</h2>"),
    question_input,
    ask_button,
    widgets.HTML("<hr>"),
    location_input,
    widgets.HBox([temp_button, precip_button]),
    output_area
])

display(ui)
```

## Why My Prompting Strategy Was Effective

Prompt Strength	and Why It Helped
1. UI structure	Defined a clean visual layout using VBox and HBox
2. Realistic questions	Allowed natural phrasing like “Will it rain in Paris tomorrow?”
3. Clear button roles	Separated weather response and chart generation actions
4. Visual appeal	Used colors and spacing to improve user interaction
5. Robust behavior	Handlers validate input and catch exceptions
6. Visual appeal — Used custom colors (blue, green, orange) to make each button's function visually distinct, improving UX and clarity.

