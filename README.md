# home-assistant

Blueprints for Home Assistant automations.

## Nibe F470 Heat Pump Blueprints

Automation blueprints for the Nibe F470 R 3x400V heat pump connected via the [myUplink](https://www.home-assistant.io/integrations/myuplink/) integration, with [Nordpool](https://www.home-assistant.io/integrations/nordpool/) electricity price integration.

### Blueprints

#### 1. Electricity Price Optimization
**File:** `blueprints/automation/nibe_f470/electricity_price_optimization.yaml`

Adjusts the heating curve offset based on Nordpool spot prices:
- **Cheap electricity** — increases heating offset (pre-heats using thermal mass)
- **Expensive electricity** — reduces heating offset (coasts on stored heat)
- **Cold weather override** — disables price optimization when outdoor temp drops below a threshold to ensure adequate heating

#### 2. Temperature Scheduling
**File:** `blueprints/automation/nibe_f470/temperature_scheduling.yaml`

Time-based heating curve adjustment with comfort/economy modes:
- Separate weekday and weekend schedules
- Daytime economy mode for weekdays (when at work)
- Optional presence detection — switches to away mode when nobody is home

#### 3. Hot Water Price Optimization
**File:** `blueprints/automation/nibe_f470/hot_water_scheduling.yaml`

Optimizes hot water heating based on electricity prices:
- Heats to higher temperatures during cheap hours (energy storage)
- Reduces target temperatures during expensive hours
- **Boost windows** before configurable times (e.g., morning/evening showers)

#### 5. Fireplace Mode
**File:** `blueprints/automation/nibe_f470/fireplace_mode.yaml`

Toggles air conditioning into fireplace mode via a dashboard button:
- Sets AC mode to value 3 (fireplace) when toggled on, back to 0 (normal) when off
- Uses an `input_boolean` helper as a simple on/off switch
- Optional auto-disable after a configurable duration (default 3 hours)

#### 4. Notifications & Monitoring
**File:** `blueprints/automation/nibe_f470/notifications_monitoring.yaml`

Monitors heat pump health and sends alerts for:
- Alarm state changes (critical priority)
- Compressor running at high frequency for too long
- Compressor stopped unexpectedly during cold weather
- High power consumption (possible backup heater activation)
- Large supply/return temperature difference (possible circulation issue)

### Installation

1. Copy the `blueprints/` directory into your Home Assistant config directory
2. Restart Home Assistant or reload automations
3. Go to **Settings > Automations & Scenes > Blueprints**
4. Find the Nibe F470 blueprints and create automations from them
5. Select your myUplink and Nordpool entities and configure thresholds

### Requirements

- [myUplink integration](https://www.home-assistant.io/integrations/myuplink/) configured with your Nibe F470
- [Nordpool integration](https://www.home-assistant.io/integrations/nordpool/) for price-based automations
- A notification service for monitoring alerts
- An `input_boolean` helper for fireplace mode toggle

---

## Toshiba AC Heat Pump Blueprints

Automation blueprints for Toshiba heat pumps controlled via the [toshiba_ac custom integration](https://github.com/h4de5/home-assistant-toshiba_ac). Supports all HVAC modes (heat, cool, auto, dry, fan_only) with temperature range 17–30°C.

### Blueprints

#### 1. Temperature Scheduling
**File:** `blueprints/automation/toshiba_ac/temperature_scheduling.yaml`

Time-based target temperature adjustment with comfort/economy modes:
- Separate weekday and weekend schedules
- Daytime economy mode for weekdays (when at work)
- Optional presence detection — switches to away temperature when nobody is home
- Only updates when temperature actually needs to change

#### 2. Climate Mode Switching
**File:** `blueprints/automation/toshiba_ac/climate_mode_switching.yaml`

Automatically switches between heating and cooling based on outdoor temperature:
- Configurable heat/cool thresholds with deadband zone
- Delays to prevent rapid mode switching
- Sets appropriate target temperature for each mode
- Optional auto-off when outdoor temperature is comfortable

#### 3. Electricity Price Optimization
**File:** `blueprints/automation/toshiba_ac/electricity_price_optimization.yaml`

Adjusts target temperature based on Nordpool electricity spot prices:
- **Cheap electricity** — pre-heats/pre-cools using thermal mass
- **Expensive electricity** — coasts on stored thermal energy
- Adapts offset direction based on current HVAC mode (heat vs cool)
- **Cold weather override** — disables price optimization when outdoor temp is too low

#### 4. Notifications & Monitoring
**File:** `blueprints/automation/toshiba_ac/notifications_monitoring.yaml`

Monitors heat pump health and sends alerts for:
- HVAC mode changes (optional)
- Extreme outdoor temperature warnings (cold and hot)
- High power consumption
- Monthly filter cleaning reminders

### Installation

1. Copy the `blueprints/` directory into your Home Assistant config directory
2. Restart Home Assistant or reload automations
3. Go to **Settings > Automations & Scenes > Blueprints**
4. Find the Toshiba AC blueprints and create automations from them
5. Select your Toshiba AC climate entity and configure thresholds

### Requirements

- [Toshiba AC integration](https://github.com/h4de5/home-assistant-toshiba_ac) configured with your heat pump
- [Nordpool integration](https://www.home-assistant.io/integrations/nordpool/) for price-based optimization
- An outdoor temperature sensor (or template sensor from AC's outdoor_temperature attribute)
- A notification service for monitoring alerts
