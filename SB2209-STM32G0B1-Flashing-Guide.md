# SB2209 STM32G0B1 Complete Flashing Guide

**IMPORTANT: Follow Esoterical's CANBus Guide, NOT the BTT documentation.**

Based on Esoterical's CANBus Guide:
- https://canbus.esoterical.online/toolhead_flashing.html
- https://canbus.esoterical.online/toolhead_flashing/common_hardware/BigTreeTech%20SB2209%20and%20SB2240/README.html
- https://canbus.esoterical.online/mainboard_flashing/common_hardware/BigTreeTech%20Manta%20M8P%20v2.html

---

## Prerequisites

- 120Ω termination jumper installed on SB2209
- 120Ω termination jumper installed on Manta M8P V2
- USB cable for initial flashing
- CAN cable ready for final connection

---

## Phase 0: Flash Manta M8P V2 USB-CAN Bridge Firmware

The Manta M8P V2 acts as the USB-CAN bridge. Its CAN bus speed **must match** all devices on the bus (1000000).

### Step 0.1: Build Manta Firmware

```bash
cd ~/klipper
make menuconfig
```

**Manta M8P V2 menuconfig settings:**

| Setting | Value |
|---------|-------|
| Enable extra low-level configuration options | [*] Enabled |
| Micro-controller Architecture | STMicroelectronics STM32 |
| Processor model | STM32H723 |
| Bootloader offset | 128KiB bootloader |
| Clock Reference | 25 MHz crystal |
| Communication interface | USB to CAN bus bridge (USB on PA11/PA12) |
| CAN bus interface | CAN bus (on PD0/PD1) |
| CAN bus speed | 1000000 |

```bash
make clean
make
```

### Step 0.2: Enter DFU Mode on Manta

1. Hold BOOT button on Manta M8P V2
2. Tap RESET while holding BOOT
3. Release BOOT
4. Verify: `lsusb` shows `0483:df11 STMicroelectronics STM Device in DFU Mode`

### Step 0.3: Flash Manta

```bash
sudo dfu-util -R -a 0 -s 0x08020000:mass-erase:force:leave -D ~/klipper/out/klipper.bin -d 0483:df11
```

### Step 0.4: Verify Manta

After reset, verify USB-CAN bridge is working:

```bash
lsusb | grep "1d50:606f"
```

Should show: `OpenMoko, Inc. Geschwister Schneider CAN adapter`

Verify can0 interface:

```bash
ip link show can0
```

---

## Phase 1: Flash Katapult via USB DFU

### Step 1.1: Enter DFU Mode

1. Install USB power jumper on SB2209
2. Connect USB from Pi/CB2 to SB2209
3. Hold both RESET and BOOT buttons
4. Release RESET first, then release BOOT
5. Verify: `lsusb` shows `0483:df11 STMicroelectronics STM Device in DFU Mode`

### Step 1.2: Build Katapult

```bash
cd ~/katapult
make menuconfig
```

**Katapult menuconfig settings:**

| Setting | Value |
|---------|-------|
| Micro-controller Architecture | STMicroelectronics STM32 |
| Processor model | STM32G0B1 |
| Build Katapult deployment application | 8KiB bootloader |
| Clock Reference | 8 MHz crystal |
| Communication interface | CAN bus (on PB0/PB1) |
| Application start offset | 8KiB offset |
| CAN bus speed | 1000000 |
| GPIO pins to set on bootloader entry | *(leave empty)* |
| Support bootloader entry on rapid double click of reset button | [*] Enabled |
| Enable bootloader entry on button (or gpio) state | [ ] Disabled |
| Enable Status LED | [*] Enabled |
| Status LED GPIO Pin | PA13 |

```bash
make clean
make
```

### Step 1.3: Flash Katapult

```bash
sudo dfu-util -R -a 0 -s 0x08000000:mass-erase:force:leave -D ~/katapult/out/katapult.bin -d 0483:df11
```

Expected output includes "File downloaded successfully". An error after this is normal.

---

## Phase 2: Verify Katapult on CAN

### Step 2.1: Switch to CAN

1. Power off printer completely
2. Remove USB cable from SB2209
3. Remove USB power jumper from SB2209
4. Connect CAN cable (power + data)
5. Power on printer

### Step 2.2: Query CAN Bus

```bash
python3 ~/katapult/scripts/flashtool.py -i can0 -q
```

Should show UUID with "Application: Katapult"

If not visible, double-click reset button on SB2209 (LED should flash), then query again.

**Record the UUID** - you'll need it for the next steps.

---

## Phase 3: Flash Klipper via CAN

### Step 3.1: Build Klipper

```bash
cd ~/klipper
make menuconfig
```

**Klipper menuconfig settings:**

| Setting | Value |
|---------|-------|
| Enable extra low-level configuration options | [*] Enabled |
| Micro-controller Architecture | STMicroelectronics STM32 |
| Processor model | STM32G0B1 |
| Bootloader offset | 8KiB bootloader |
| Clock Reference | 8 MHz crystal |
| Communication interface | CAN bus (on PB0/PB1) |
| CAN bus speed | 1000000 |

```bash
make clean
make
```

### Step 3.2: Stop Klipper

```bash
sudo service klipper stop
```

### Step 3.3: Flash Klipper via Katapult

```bash
python3 ~/katapult/scripts/flashtool.py -i can0 -f ~/klipper/out/klipper.bin -u <UUID_FROM_STEP_2.2>
```

---

## Phase 4: Verify and Configure

### Step 4.1: Verify Klipper

```bash
~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0
```

Should show same UUID with "Application: Klipper"

### Step 4.2: Update printer.cfg

```ini
[mcu EBBCan]
canbus_uuid: <UUID>
```

### Step 4.3: Start Klipper

```bash
sudo service klipper start
```

---

## Troubleshooting

### No UUID visible after Phase 2

- Double-click reset button on SB2209 to enter bootloader mode (LED should flash)
- Verify 120Ω termination jumper is installed
- Check CAN wiring continuity
- Verify CAN bus speed matches (1000000)

### Katapult LED flashing but no CAN response

- **Verify CAN baud rate matches on ALL devices** - Manta, SB2209, and Eddy must all use 1000000
- Check CAN-H and CAN-L are not swapped
- Verify can0 interface is up: `ip link show can0`
- Check for bus errors: `ip -s -d link show can0`
- Check Manta firmware CAN speed: `cat ~/klipper/config.manta | grep CANBUS_FREQUENCY`

### References

- [Esoterical's CANBus Guide](https://canbus.esoterical.online/)
- [BigTreeTech EBB GitHub](https://github.com/bigtreetech/EBB)
- [Dr. Klipper Wiki - EBB SB2209](https://drklipper.de/doku.php?id=klipper_faq%3Aflash_guide%3Astm32g0b1%3Aebb_sb2209_can)
