# TODO: To keep me under control!

## Component Selection & Part Numbering
Key Components to Finalize:
- [ ] PCIe Switch IC
- [ ] PCIe-to-SATA Controller
- [ ] Gigabit PHY
- [ ] USB-to-UART Bridge
- [ ] Buck Regulators & Load Switches (5V 3A+, 3.3V 4A+ 1.2V)
- [ ] RTC IC

## Schematic Capture
Organize logical schematic sheets:
- [ ] Block Diagram and Power Tree : High-level system connections and power distribution map.
- [ ] Compute Module Headers: CM5 high-density socket connections, power input, and GPIO breakouts.
- [ ] Power Management: 12V barrel input, protection (eFuse/MOSFET), 5V/3.3V/1.2V regulators, and SATA power load switch.
- [ ] PCIe Subsystem: PCIe switch IC, reference clock buffer/distribution, M.2 Key E slot, and SATA controller.
- [ ] Network Subsystem: KSZ9897 switch IC, pin-strapping resistors, magnetics, 4x front RJ45 jacks, Port 7 RGMII PHY, and rear RJ45 jack.
- [ ] I/O & Peripherals: Full-size HDMI, MicroSD card slot, USB-to-UART bridge, eMMC flash USB-C port, RTC, and 4-pin PWM fan driver.
- [ ] Blinkenlights & Control: Status LED matrix, OLED, shift registers (if used), and reset/boot buttons.

## Pinout & GPIO Verification
- [ ] Ensure all Compute Module GPIO pins are correct to avoid multiplexing conflicts.

## PCB Layout & High-Speed Routing Constraints
Because this design includes Gigabit Ethernet, HDMI, USB 2.0, and PCIe Gen 2/3 signals, routing requires careful attention to signal integrity. Use a minimum 4-layer or 6-layer PCB stackup with dedicated inner ground and power planes.

### Controlled Impedance:
- [ ] PCIe: 85Ω differential pairs.
- [ ] Ethernet: 90Ω differential pairs.
- [ ] USB: 90Ω differential pairs.
- [ ] HDMI: 100Ω differential pairs.
- [ ] RGMII: Length-matched trace sets for timing alignment between the PHY and KSZ9897.

### Thermal Design:
- [ ] Place large copper pours and thermal vias under high heat ICs (Compute Module, PCIe switch, KSZ9897 and regulators)
