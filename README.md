# ESP32 RFID Attendance System

An RFID-based attendance system built with an ESP32, MFRC522 RFID reader, 20×4 I2C LCD, push button, status LEDs, and Google Sheets integration.

This project contains three Arduino sketches, progressing from basic RFID testing to a Google Sheets attendance system.

## Features

- Read RFID card/keychain UIDs through the Serial Monitor.
- Display RFID information and button state on a 20×4 I2C LCD.
- Register RFID cards and record attendance through Google Sheets.
- Display attendance status, name, date, and check-in/check-out times on the LCD.
- Use green and red LEDs to indicate attendance results.
- Switch between **Attendance** and **Registration** modes using a push button.

## Hardware Required

- ESP32 development board
- MFRC522 RFID reader
- RFID cards or keychains
- 20×4 LCD with I2C backpack
- Push button
- Green LED and red LED (with suitable current-limiting resistors)
- Jumper wires and USB cable

## Repository Contents

```text
Attendance sheet/
├── 01_Test_RFID-RC522_Serial/
│   └── 01_Test_RFID-RC522_Serial.ino
├── 02_Test_RFID-RC522_LCD-20x4_Button/
│   └── 02_Test_RFID-RC522_LCD-20x4_Button.ino
└── 03_ESP32_RFID_Google_Spreadsheet_Attendance/
    └── 03_ESP32_RFID_Google_Spreadsheet_Attendance.ino
```

### 1. RFID UID Test
Reads an RFID card UID and prints it to the Serial Monitor.

### 2. RFID + LCD + Button Test
Displays the detected UID and button state on the 20×4 I2C LCD.

### 3. Google Sheets Attendance System
Connects the ESP32 to Wi-Fi and sends RFID UIDs to a Google Apps Script web app. The script handles registration and attendance records, while the LCD and LEDs provide feedback.

## Pin Connections

The sketches use the following GPIO assignments. Confirm that they match your specific ESP32 board before wiring.

| Component | ESP32 GPIO |
|---|---:|
| MFRC522 SDA / SS | GPIO 5 |
| MFRC522 RST | GPIO 4 |
| Push button | GPIO 15 |
| Green LED | GPIO 25 |
| Red LED | GPIO 26 |
| I2C LCD SDA | Board-specific I2C SDA |
| I2C LCD SCL | Board-specific I2C SCL |

The MFRC522 uses SPI. Connect its SCK, MOSI, MISO, 3.3V, and GND pins to the appropriate ESP32 SPI pins and power connections. The LCD code uses I2C address `0x27`; scan the I2C bus if your display uses a different address.

**Important:** Power the MFRC522 from **3.3V**, not 5V. ESP32 GPIOs are not 5V tolerant.

## Required Arduino Libraries

Install these libraries using the Arduino IDE Library Manager:

- **MFRC522** by Miguel Balboa
- **LiquidCrystal I2C** (a compatible library providing `LiquidCrystal_I2C.h`)

`SPI`, `WiFi`, and `HTTPClient` are supplied by the ESP32 Arduino board package.

## Setup

1. Install the ESP32 board support package in Arduino IDE.
2. Install the required libraries.
3. Open and upload one sketch at a time, starting with the UID test.
4. Check the Serial Monitor at **115200 baud** for the first sketch.
5. For the Google Sheets sketch, update the Wi-Fi SSID and password in the code.
6. Deploy a Google Apps Script as a web app and configure its URL in `Web_App_URL`.
7. Configure the spreadsheet and Apps Script to support the expected registration and attendance request/response format.
8. Upload the third sketch and test registration before recording attendance.

## Attendance Mode

- **Attendance mode:** Scan a registered card to record attendance. Depending on the Apps Script response, the system displays check-in or check-out information.
- **Registration mode:** Press the button to switch modes, then scan a card to register its UID through the Google Apps Script.

The exact spreadsheet columns and registration rules depend on your Google Apps Script implementation.

## Troubleshooting

- **Card not detected:** Check MFRC522 wiring, 3.3V power, SPI pins, and library installation.
- **LCD blank:** Check SDA/SCL wiring, I2C address, contrast, and power.
- **Wi-Fi connection fails:** Verify the SSID and password.
- **Google Sheets request fails:** Verify the deployed Apps Script URL, web app access permissions, and server-side response format.
- **LEDs do not work:** Confirm polarity, GPIO wiring, and current-limiting resistors.


## Disclaimer

This project is intended for learning and prototyping. Test the wiring and Google Sheets workflow before using it for real attendance records.
