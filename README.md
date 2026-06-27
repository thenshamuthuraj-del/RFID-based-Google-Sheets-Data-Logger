# RFID-based-Google-Sheets-Data-Logger

# 📊 RFID-based Google Sheets Data Logger using ESP8266

A smart, Wi-Fi–enabled RFID system that **reads data blocks from RFID cards** and logs them to **Google Sheets** in real time via HTTPS.  
Displays status and results on a 16x2 I2C LCD, and prevents duplicate entries by checking scanned history.

Perfect as a proof of concept for:
- Smart attendance systems
- Inventory logging
- Perishable tracking in supermarkets
- Edge IoT data collection

---

## 📦 **Features**
✅ Reads multiple data blocks from each RFID tag  
✅ Publishes data securely over HTTPS to Google Sheets  
✅ Duplicate detection (delete or insert based on prior scan)  
✅ LCD shows live status (connected, publishing, success, failed)  
✅ Visual feedback with buzzer and LED  
✅ Fully written in C++ for ESP8266 (NodeMCU)

---

## 🛠 **Hardware Used**
- NodeMCU (ESP8266)
- MFRC522 RFID Reader
- RFID cards/tags
- I2C LCD 16x2 (address `0x27`)
- Buzzer
- LED (connected to `D8`)
- Jumper wires

---

## ⚙ **Workflow**
1. Connects to Wi-Fi → shows status on LCD.
2. Connects to Google Apps Script endpoint over HTTPS.
3. Waits for RFID tag:
   - Reads data from multiple blocks (`4,5,6,8,9`)
   - Sanitizes and combines data
4. Checks if the tag has been scanned before:
   - ✅ New tag → sends `insert_row` command to Google Sheets.
   - 🗑 Already scanned → sends `delete_row` command to remove it.
5. Shows result on LCD and toggles LED/buzzer.

---

## 🔒 **Secure Cloud Integration**
- Uses **HTTPSRedirect** library to POST JSON payload.
- Google Apps Script receives request, updates sheet.

---

## 🧰 **Libraries Used**
- `ESP8266WiFi.h` (Wi-Fi)
- `MFRC522.h` (RFID reader)
- `HTTPSRedirect.h` (HTTPS POST)
- `LiquidCrystal_I2C.h` (LCD display)
- `Wire.h`, `SPI.h`, `Arduino.h` (Arduino core)

> All can be installed via Arduino Library Manager.

---

## 📋 **Google Sheets Setup**
- Deploy a Google Apps Script Web App bound to a Google Sheet.
- Use script to handle `insert_row` and `delete_row` commands.
- Replace `GScriptId` with your script's ID.

---

## 📝 **RFID Block Reading**
Reads and sanitizes 5 RFID blocks:
| Block | Purpose                  |
|:----:|---------------------------|
|  4   | Roll number / ID (key)    |
|  5–9 | Name / extra fields       |

> Only printable ASCII characters are kept.

---
