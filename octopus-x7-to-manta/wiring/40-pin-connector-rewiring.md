# 40-Pin Connector Rewiring

This document outlines the changes required to migrate from the **Octopus X7** mainboard to the **Manta M8P**.

## Overview
The original wiring uses a **40-pin connector** to route multiple signals, including motor, endstop, and sensor connections. This chart provides a **pin-to-pin mapping** of only the **necessary** rewired connections.

### Filament Sensor (as built)
- The **stock Troodon filament sensor** line on the 40-pin connector (PC2) is **retired and physically removed** (2026-10-12). Its 3-wire extension was pulled out of the harness; nothing in the Klipper config reads it.
- The **BTT SFS 2.0** does not go through the 40-pin connector. Its two leads plug directly into the Manta's endstop row: **Switch Sensor → M5-STOP (PF0)**, **Motion Sensor → M6-STOP (PC15)**. Both are JST-XH 3-pin plugs.
- Verified live 2026-10-12: with filament pulled out of the SFS, `sfs_switch` reports `filament_detected: False`.

For full pin mapping, see `40pin-to-manta.csv`.
