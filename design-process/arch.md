# System Architecture
How is the switch going to work?

## Power & Thermal Management
- Main Power Input: 12V DC Barrel Jack on rear panel.
- Power Protection Circuitry: Input reverse-polarity protection (P-channel MOSFET / ideal diode) and inrush current / eFuse protection.
- Active Thermal Control: 4-pin PWM fan header with MOSFET driver, controlled with CM GPIO based on temp sensors.

### Power Delivery Rails:
- 5V Buck Regulator: Powers Compute Module, USB, and SATA drive
- 3.3V Buck Regulator (High Current): Powers Switch, Ethernet PHYs and wifi transmission power spikes
- 1.2V Regulators: Core voltages for Ethernet switch and PHY ICs.

## Compute Module & System Hardware
- Host Processor: Raspberry Pi Compute Module CM5.
- System Clocks & Timing: Onboard I2C Real-Time Clock (RTC) IC with coin cell for offline timekeeping.
- Hardware Monitoring: Onboard I2C temperature sensors near PCIe switch and power regulators for system health telemetry.
- Front Panel Reset Switch: Recessed pinhole button connected to CM reset line.

## PCIe & Storage Topology
- PCIe Switch IC: Splits the single Compute Module PCIe lane into two downstream channels (with clock buffer for PCIe REFCLK).
  - Branch 1: M.2 Key E slot for Wi-Fi 6 cards (includes routed W_DISABLE1# / W_DISABLE2# GPIO lines).
  - Branch 2: PCIe-to-SATA controller for SATA SSD/HDD storage.
- Storage & OS Boot Options:
  - Onboard eMMC support on Compute Module.
  - MicroSD card slot for Lite modules or secondary storage.
  - Rear-panel eMMC Flashing Port: USB-C slave port with recessed pinhole push-button to trigger rpiboot mode.

## Network & Switch Architecture
- KSZ9897 Switch IC: Managed 7-port Gigabit Ethernet switch core configured via I2C/SPI pin-strapping resistors.
- Front Panel LAN Switch Ports (Ports 1–4): Direct copper Gigabit RJ45 ports from KSZ9897 internal PHYs for local network devices.
- Host Link (Port 5): Directly connected to Compute Module
- Rear Panel Dedicated WAN / Management Port (Port 7): RGMII interface connected to an external Gigabit Ethernet PHY (e.g., RTL8211) driving a 5th RJ45 port on the rear panel.

## Visual Feedback & Front Panel I/O
- Display Output: Full-sized HDMI port connected directly to CM video output.
- Serial Debugging: Dedicated front-panel USB-C port backed by an onboard USB-to-UART bridge IC (e.g., CP2102N / FT230X) connected to CM primary UART.
- Status LED Matrix ("Blinkenlights"):
  - Network LEDs: Individual discrete LEDs for Link/Activity and Speed (1000M vs 10/100) per RJ45 port.
  - System & Activity LEDs: Dedicated discrete indicators for 12V / 5V / 3.3V / 1.2V Power Rails, CM Heartbeat, Storage Read/Write (SATA/eMMC), and Wi-Fi RX/TX.
- OLED: Shows quick stats (user configurable).

## Enclosure & Physical Interfaces
- Front Panel: 4x RJ45 LAN Ports, Full-Size HDMI, USB-C Console Port, Recessed Reset Button, Status LED Matrix, OLED.
- Rear Panel: 1x RJ45 WAN/Uplink Port, 12V DC Jack, USB-C eMMC Flash Port + Pin-hole Boot Button, PWM Fan Exhaust, 2x–3x SMA Antenna Bulkhead Cutouts for Wi-Fi pigtails.
- Chassis & Grounding: PCB mounting holes tied to Earth Ground via an RC filter network (1MΩ / 10nF) near RJ45 jacks for ESD protection.
