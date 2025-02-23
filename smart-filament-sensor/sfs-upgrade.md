# Smart Filament Sensor (SFS 2.0) Upgrade

## Overview
This phase of the upgrade adds the **BTT Smart Filament Sensor (SFS 2.0)** to the **Troodon 2.0 Pro**. The **SFS 2.0** improves filament detection by monitoring both **filament presence** and **extrusion movement**, helping to prevent failed prints.

## Why Upgrade?
The **original filament sensor** on the **Troodon 2.0 Pro** was a simple switch-based sensor that only detected whether filament was present. The **BTT SFS 2.0**, however, offers:
- **Runout detection** (detects when filament runs out)
- **Extrusion detection** (detects if filament is moving properly)
- **Higher reliability** compared to basic sensors

## Wiring

The **Manta M8P** has a dedicated **Fil-DET (ADC IN) port**, allowing for a direct **4-pin connection** to the **SFS 2.0** without additional modifications.

| SFS 2.0 Wire | Manta M8P Pin |
|-------------|-------------|
| **Red (5V)** | **5V (Fil-DET Port)** |
| **Black (GND)** | **GND (Fil-DET Port)** |
| **Blue (Switch Sensor)** | **PF0 (Fil-DET Port)** |
| **Green (Motion Sensor)** | **PC15 (Fil-DET Port)** |

Since the **SFS 2.0** can be plugged directly into the **Fil-DET (ADC IN) port**, there is no need for split cables or custom wiring.

## Mounting
The **Smart Filament Sensor** will be mounted near the filament path, ideally at the **rear of the printer** in place of the existing runout sensor.

## Firmware Configuration
To enable the **Smart Filament Sensor (SFS 2.0)** in **Klipper**, add the following configuration:

```ini
[filament_motion_sensor SFS]
detection_mode: runout
switch_pin: PF0
motion_pin: PC15
pause_on_runout: True
runout_gcode: M600
extruder: extruder
```

## Summary
- The **Manta M8P** has a **dedicated Fil-DET (ADC IN) port**, making wiring simple.
- The **SFS 2.0** connects directly to this port with a 4-pin connection.
- The **SFS 2.0** will be mounted near the filament path.
- **Klipper configuration** is straightforward, using **PF0 for runout detection** and **PC15 for extrusion movement detection**.
