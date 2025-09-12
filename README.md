# tousek-garage-st61-esphome
# Smart Garage Control for Tousek ST-61 with ESPHome + Home Assistant

This project explains how to make a **Tousek ST-61 garage opener** smart using an  
**AA60 ESP32 relay board** with **ESPHome**.  
The setup allows full control of **two garage doors** directly from **Home Assistant**, with real door feedback via the ST-61 expansion slot.

---

## Hardware

- AA60 ESP32 RS485 Modbus 4-Channel Relay Module https://de.aliexpress.com/item/1005009209598465.html
- 12V DC power supply 
- JST VH3.96 4-pin female connectors with wires  https://de.aliexpress.com/item/1005008286152035.html
- Tousek ST-61 garage
- Connection wires relay to the Tousek module 
 

---

## Wiring

This setup is designed for **two Tousek ST-61 control units**, each operating a **two-wing garage door**.  
For every unit, the ESP32 controls both operation modes:  

- **Normal Operation (Pin 32)** → opens/closes both wings fully  
- **Pedestrian Operation (Pin 34)** → opens only one wing (partial opening)  

That means all **four relays** of the ESP32 board are used:  
two relays per ST-61 unit (Normal + Pedestrian).  

---

### Relays → ST-61 input terminals (30–34)

| Relay   | ESP32 GPIO | ST-61 Pins       | Function                                  |
|---------|------------|------------------|-------------------------------------------|
| Relay 1 | GPIO23     | 30 (COM) + 32    | Unit A – Full Door (both wings)           |
| Relay 2 | GPIO5      | 30 (COM) + 34    | Unit A – Pedestrian (one wing)            |
| Relay 3 | GPIO4      | 30 (COM) + 32    | Unit B – Full Door (both wings)           |
| Relay 4 | GPIO13     | 30 (COM) + 34    | Unit B – Pedestrian (one wing)            |

⚠️ Each ST-61 unit has its own **pin 30 (Common)** – do not mix them.  
⚠️ Relays 1–2 are wired to **Unit A**, Relays 3–4 to **Unit B**.  

---

### Inputs → ST-61 expansion slots

| ESP32 GPIO | Function             | Description                                |
|------------|----------------------|--------------------------------------------|
| GPIO25     | Unit A – Status A    | Feedback from ST-61 expansion slot         |
| GPIO26     | Unit A – Status B    | Feedback from ST-61 expansion slot         |
| GPIO27     | Unit B – Status A    | Feedback from ST-61 expansion slot         |
| GPIO33     | Unit B – Status B    | Feedback from ST-61 expansion slot         |

These inputs provide the **real-time wing status** from each ST-61 unit.  
ESPHome uses these signals in the `cover` entities so Home Assistant always knows if a door is **fully open, closed, or partially open**.

---

## Activating the ST-61 Additional Module

In the ST-61 menu go to:

**Peripherals → Additional module**

Available options:  
- Courtyard/control lamp  
- Status display 1  
- Status display 2  

✅ To use ESPHome feedback, select **Status display 2**.  

---

## ESPHome Configuration

See [esp-garage.yaml](esp-garage.yaml) for the full configuration.  

The AA60 ESP32 relay board can be connected directly to your computer via **USB-C**.  
Flashing is very simple using **ESPHome Web**:

👉 [https://web.esphome.io](https://web.esphome.io)

1. Open the link in your browser (Chrome/Edge recommended).  
2. Plug the relay board into your computer with a USB-C cable.  
3. Select the device in the browser and upload the ESPHome firmware (`esp-garage.yaml`).  
4. After flashing, the board will connect to your Wi-Fi and appear in Home Assistant.
---
## Safety Notes

- Only connect to the **low-voltage terminals (30–34)** of the ST-61.  
- Do **not** connect ESP32 or relays to **230V mains**.  
- Both relays can share the **common (30)** pin safely.  
- ⚠️ **Electricity is dangerous**: Always switch off power to the ST-61 and ESP32 before doing any wiring.  
- Double-check polarity and wiring before re-powering the system.  
- Incorrect wiring may damage the ST-61, the ESP32, or even cause electric hazards.  
- This project is provided **as-is, without warranty**.  
- ⚠️ **Use at your own risk** – you are fully responsible for safety, compliance, and correct installation.  

---

## Credits

- Hardware: [AA60 ESP32 Relay Board] https://de.aliexpress.com/item/1005009209598465.html
- JST Cables: [VH3.96 Connectors] https://de.aliexpress.com/item/1005008286152035.html
- ESPHome: [https://esphome.io](https://esphome.io)  
- Home Assistant: [https://www.home-assistant.io](https://www.home-assistant.io)
- Pinout research and documentation: **Helmi Beh** 🙏  

