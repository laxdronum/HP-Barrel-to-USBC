# HP Laptop USB-C PD Charging Adapter (Blue Tip Mod)

This repository documents the design and assembly of a custom adapter allowing HP laptops with proprietary "Blue Tip" (4.5mm x 3.0mm) connectors to utilize USB-C Power Delivery (PD) charging sources.

![Finished Project](assets/real_product.jpg)
*Figure 1: Assembled adapter.*

## Project Objective
Standard DC jack converters often fail to charge HP laptops or trigger an "Unknown AC Adapter" error due to HP's proprietary "Smart Pin" signal identification. This project addresses this by:
1.  Negotiating a 20V output from a USB-C PD source.
2.  Spoofing the analog ID signal to simulate a genuine HP power supply.
3.  Housing the circuitry in a custom 3D-printed enclosure.

## Technical Implementation

### Power Delivery Triggering
The circuit utilizes a PD trigger module to request a fixed **20V** profile from the USB-C charger. While HP laptops nominally operate at 19.5V, the 20V standard falls within the safe input voltage tolerance of the device's power regulation circuitry.

### Smart Pin Spoofing
HP power supplies identify their wattage capacity via an analog signal on the central ID pin. This project replicates that signal using a pull-up resistor connected between the **V+ (20V)** line and the **ID Pin**.

![Wiring Diagram](assets/wiring_diagram.png)
*Figure 2: Circuit wiring diagram.*

### Connector Pinout & Safety Warning
The HP "Blue Tip" (4.5mm) connector consists of three connection points.

* **Outer Sleeve:** Ground (GND)
* **Inner Ring:** V+ (19.5V / 20V)
* **Center Pin:** ID (Smart Pin Signal)

> **⚠️ CRITICAL WARNING:** Replacement cable wire colors are **NOT standard** and vary significantly between manufacturers.
> 
> **Do not rely on color codes.** You **MUST** use a multimeter in continuity mode to identify which wire corresponds to the Inner Ring, Outer Sleeve, and Center Pin before soldering. Failing to do so may reverse polarity and damage your laptop.

### Resistor Values
To emulate specific wattage ratings, the corresponding resistor must be soldered between the V+ line and the ID Pin.

| Target Power Rating | Required Resistor |
| :--- | :--- |
| **45W** | **~470kΩ - 510kΩ** |
| 65W | ~383kΩ |
| 90W | ~294kΩ |

*Technical reference and signal logic derived from [Parkytowers](http://www.parkytowers.me.uk/thin/hp/adapter/inside.shtml).*

## Bill of Materials
* **1x** USB-C PD Trigger Module (20V configuration)
* **1x** Resistor (See table above for values)
* **1x** HP Blue Tip Replacement Cable (4.5mm x 3.0mm)
* **1x** 3D Printed Enclosure
* **4x** M3 ISO7380-1 Screws (**Length: 10mm**)

## 3D Enclosure
A custom enclosure designed in FreeCAD is provided to secure the components. The design consists of two parts.

![FreeCAD Render](assets/freecad_render.png)
*Figure 3: 3D model render.*

* **Source Files:** `stl/HP-PDM-Bottom.stl` `stl/HP-PDM-Top.stl`
* **Recommended Material:** PLA or PETG
* **Infill:** 100%

## Assembly Instructions

1.  **Placement:** Insert the PD trigger board into its designated slot within the enclosure. Secure it in place using a small amount of hot glue.
2.  **Cable Strain Relief:** Insert the replacement cable's strain relief section into the enclosure, aligning it with the PD board's output pads accordingly.
3.  **Verification (Crucial Step):** Use a multimeter to confirm which wire is V+, GND, and ID. Label them if necessary.
4.  **Wire Prep:** Carefully trim the wires to the appropriate length and strip the insulation.
5.  **Soldering (Main Power):**
    * Solder the **V+ (Inner Ring)** wire to the **VCC/V+** pad on the PD board.
    * Solder the **GND (Outer Sleeve)** wire to the **GND** pad on the board.
6.  **Soldering (ID Resistor):**
    * Insulate the resistor legs using heat shrink tubing.
    * Solder one leg of the resistor to the **VCC/V+** pad.
    * Solder the other leg to the **ID (Center Pin)** wire.
7.  **Insulation & Securing:** Ensure all exposed connections are insulated. Use hot glue to cover the pads and secure wires against vibration.
8.  **Final Assembly:** Close the enclosure lid and fasten it using the four M3x10mm screws.

> **✅ Final Voltage Check:** Before plugging the adapter into your laptop for the first time:
> 1. Connect the adapter to your USB-C charger.
> 2. Use a multimeter to measure the voltage at the HP tip.
> 3. Verify you have **+20V** on the Inner Ring and **0V (GND)** on the Outer Sleeve.
> 4. Only proceed if the polarity and voltage are correct.

## Disclaimer
This documentation is provided for educational purposes. This project involves hardware modification and electrical wiring. Incorrect assembly may result in damage to the host device. Implementation is at the user's own risk.
