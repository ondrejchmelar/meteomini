# Meteomini

ESP32-based sensor station that measures temperature, humidity, CO2, and VOC/NOx air quality, then uploads the data to [TMEP.cz](https://tmep.cz) and enters deep sleep for 10 minutes.

## Hardware

- ESP32
- BME688 — temperature, humidity, air quality
- SGP41 — VOC / NOx index
- SCD41 — CO2
- Battery with voltage divider on ADC pin 0
- Power enable pin 3 (cuts sensor power between measurements)

### Wiring (I2C)

| Signal                | ESP32 pin |
|-----------------------|-----------|
| SDA                   | 19        |
| SCL                   | 18        |
| Sensor power enable   | 3         |
| Battery voltage (ADC) | 0         |

## Prerequisites

### 1. Install Arduino IDE

Download and install [Arduino IDE 2.x](https://www.arduino.cc/en/software).

### 2. Add ESP32 board support

1. Open **File → Preferences**.
2. In *Additional boards manager URLs*, add:
   ```
   https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
   ```
3. Open **Tools → Board → Boards Manager**, search for `esp32` (by Espressif Systems) and install it.

## Installing Libraries

Open **Sketch → Include Library → Manage Libraries** and install each of the following:

| Search for                       | Author               |
|----------------------------------|----------------------|
| `Adafruit BME680`                | Adafruit             |
| `SparkFun SCD4x Arduino Library` | SparkFun Electronics |
| `Sensirion I2C SGP41`            | Sensirion            |
| `Sensirion Gas Index Algorithm`  | Sensirion            |
| `WiFiManager`                    | tzapu                |

The remaining libraries (`Wire`, `HTTPClient`, `esp_wifi`, `esp_sleep`, `Preferences`) are bundled with the ESP32 board package and require no separate installation.

## Configuration

Open `Meteomini+SGP41+SCD41+BME688` and set your TMEP.cz subdomain:

```cpp
String server_name = "http://YOUR_SUBDOMAIN.tmep.cz/?";
```

## Compiling and Uploading

1. In Arduino IDE, open `Meteomini_SGP41_SCD41_BME688.ino`
2. Select your board: **Tools → Board → ESP32 Arduino → ESP32 Dev Module** (or whichever variant you have).
3. Select the port: **Tools → Port** — pick the COM/tty port that appears when you plug in the board.
4. Click **Upload** (the right-arrow button).

The IDE compiles the sketch and flashes it to the board. Open **Tools → Serial Monitor** at 115200 baud to see live output.

### First boot — WiFi setup

On first run, the board creates a WiFi access point named **ESP32-Config**. Connect to it from your phone or laptop, enter your home WiFi credentials, and the board will save them and restart.
