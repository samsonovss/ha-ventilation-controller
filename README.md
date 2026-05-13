# PID Window Controller

Custom Home Assistant integration for controlling a window/cover by PID.

## Features

- Select an indoor temperature sensor in the UI
- Select the controlled cover/window in the UI
- Optional outdoor temperature sensor
- PID control of cover position from 0–100%
- Adjustable target temperature
- Separate summer/winter PID coefficients
- Optional summer/winter logic based on outdoor temperature
- Autotune with a real stepped window test
- Optional per-device calibration for non-linear actuator travel

## Settings

Available as entities in the **Settings** section:

- `Target temperature`
- `PID Profile Mode` (`auto` / `winter` / `summer`)
- `Winter Kp / Ki / Kd`
- `Summer Kp / Ki / Kd`
- `Adaptive outdoor factor`
- `Adaptive rate factor`
- `Update interval`
- `Autotune sample seconds`
- `Outdoor lock enabled`
- `Outdoor summer limit`
- `Outdoor lock threshold`
- `Calibration points`

### Calibration points

Optional text field with points in the form:

`10:0.5,20:1.0,30:1.5,50:2.5,100:12.0`

This maps PID output percent to the real opening of the window/actuator.

## Control logic

- `auto` profile:
  - uses outdoor temperature when available
  - otherwise falls back to indoor trend / target temperature
- `Outdoor summer limit` switches the controller into summer logic at warm outdoor temperatures
- `Outdoor lock threshold` clamps opening more aggressively when it is too warm outside
- Autotune disables normal PID during the test, moves the window through 25 / 50 / 75 / 100%, samples response, then restores normal control

## Status

This is an initial HACS-ready version focused on the core PID controller.

## Installation

### HACS

1. Open HACS.
2. Add this repository as a custom integration repository.
3. Install **PID Window Controller**.
4. Restart Home Assistant.
5. Go to **Settings → Devices & services → Add integration**.
6. Select the indoor temperature sensor and the target cover.

### Manual installation

Copy `custom_components/pid_window` to `/config/custom_components/pid_window` and restart Home Assistant.

## Entities

- `switch` to enable/disable the controller
- `number` entities for target temperature and tuning settings
- `text` entity for calibration points
- `sensor` entities for controller state and live values
- `button` for autotune

## Notes

- The component is designed for a window with 0–100% position control.
- The outdoor sensor is optional.
- If the outdoor sensor is present, it can be used to limit window opening in warm weather.
- The autotune is a stepped response test, not a quick coefficient guess.
