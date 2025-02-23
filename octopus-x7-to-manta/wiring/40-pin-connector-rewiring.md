# 40-Pin Connector Rewiring

This document outlines the changes required to migrate from the **Octopus X7** mainboard to the **Manta M8P**.

## Overview
The original wiring uses a **40-pin connector** to route multiple signals, including motor, endstop, and sensor connections. This chart provides a **pin-to-pin mapping** of only the **necessary** rewired connections.

### Changes for Smart Filament Sensor (SFS 2.0)
- Instead of connecting to **Fil-DET**, the **Switch Sensor** is now connected to **PF0**, and the **Motion Sensor** is connected to **PC15**.
- Both signals are **JST-XH 4-Pin** connections.

For full pin mapping, see `40pin-to-manta.csv`.
