# Tutorial 2 — Dashboard & Configuration

This tutorial explains how to connect to the AgXRP web interface, read sensor data, control the water pump, set up automatic watering, and configure your device. This tutorial assumes you have one capacitive soil moisture sensor and one pump connected (the base kit). Adding more sensors and pumps is covered in [Tutorial 3](tutorial-3-additional-sensors-and-pumps.md).

!!! note "Before You Begin"
    Make sure you have completed [Tutorial 1 — Kit Contents, Assembly & Wiring](tutorial-1-software-setup.md) and that your AgXRP is powered on.

---

## Connecting to the AgXRP

**Step 1.** On your phone, tablet, or computer, open your Wi-Fi settings and connect to the AgXRP network:

- **Network name (SSID):** `AgXRP_SensorKit`
- **Password:** `sensor123`

**Step 2.** A web page should open automatically in your browser. If it does not, open a browser and go to: **http://192.168.4.1**

!!! note
    While connected to the AgXRP Wi-Fi network, your device will not have regular internet access. This is expected — the AgXRP creates its own local network for communication with the board.

---

## The Dashboard

The Dashboard is the home page of the AgXRP web interface. It is divided into three sections: sensor data at the top, manual pump controls in the middle, and the automatic watering controller at the bottom.

At the top of the page you will see navigation links to switch between pages: **Dashboard**, **Data**, and **Configure**.

---

### Sensor Data

The top section of the Dashboard displays live readings from all connected sensors. With the base kit, you will see a **Soil Moisture 1** reading from your capacitive soil sensor, reported in **pF (picofarads)**. The data updates automatically every few seconds.

Below the sensor readings, the Dashboard displays a **Date/Time** line and a **Last updated** indicator.

![Sensor data section showing soil moisture reading and date/time](images/dashboard-sensor-data.jpg)
*Sensor data section showing soil moisture reading and date/time*

#### How Date and Time Works

The XRP board does not have a battery-backed clock. Every time the board powers on, the date and time resets to the date the board was manufactured. However, two things can update the clock to the correct time:

1. **Log file recovery** — If you have CSV data logging enabled and data has been written to a log file, the board will set its clock to the most recent date and time found in the log file on boot. This is not perfectly accurate, but it keeps timestamps roughly in order.
2. **Browser sync** — As soon as a phone, laptop, or tablet connects to the Dashboard in a web browser, the board automatically syncs its clock to the date and time on that device. From that point on, the board's clock is accurate.

!!! warning "What Happens During a Power Outage"
    If the board loses power and restarts, the clock resets. If CSV logging is enabled, the clock will recover to the time of the last log entry — but any data logged between the restart and a browser reconnection will have timestamps from the last known time, not the actual current time. To get accurate timestamps again, simply connect to the Dashboard from any device with a web browser.

---

### Manual Pump Controls

Below the sensor data, the Dashboard shows manual controls for each water pump.

![Manual pump control section](images/dashboard-manual-pump-control.jpg)
*Manual pump control section*

The controls work as follows:

- **Pump Effort** — A number between **-1.0** and **1.0** that controls the motor speed.
    - `1.0` = full forward speed
    - `0.5` = half speed
    - `0.0` = stopped
    - Negative values (e.g., `-1.0`) spin the pump **backwards**. You would only use a negative value if the pump is pushing water in the wrong direction — flip the sign and the water will flow the other way.
- **Start Pump** (blue button) — Starts the pump at the effort value you entered.
- **Stop Pump** (red button) — Stops the pump immediately.

!!! tip "Checking That Your Sensor and Pump Are Paired Correctly"
    When you manually start a pump, the **blue LED** on the soil moisture sensor that is paired with that pump will turn on. If you click Start Pump and the LED on your sensor lights up, the sensor and pump are paired correctly. If the LED does not light up (or the wrong sensor lights up), you need to adjust the sensor and pump index numbers on the Configuration page.

---

### Automatic Watering Controller

The bottom section of the Dashboard lets you configure **automatic watering**. Each soil sensor and pump pair forms one **Plant System**. With the base kit, you will have one plant system: Sensor 1 + Pump 1.

![Automatic Watering Controller section](images/dashboard-automatic-water-control.jpg)
*Automatic Watering Controller section*

#### Controller Fields

| Field | Description |
|-------|-------------|
| **Check Interval (hours)** | How often the system checks the soil moisture. For example, `0.5` means it checks every 30 minutes. `1.0` means it checks every hour. |
| **Soil Moisture Threshold** | The moisture reading (in pF for capacitive sensors) below which the system will trigger the pump. If the sensor reads lower than this value, the soil is considered too dry. |
| **Pump Duration (seconds)** | How long the pump runs each time it is triggered. |
| **Pump Effort (-1.0 to 1.0)** | The motor speed used during automatic watering. Typically set to `1.0` for full speed. |
| **Enable Controller** | Radio buttons to turn automatic watering **Enabled** or **Disabled** for this plant system. |

After changing any of these values, click **Update Settings** to save them. The settings take effect immediately — no reboot is required.

#### How Automatic Watering Works

The automatic watering system uses a simple feedback loop:

1. At the interval you set (e.g., every 30 minutes), the system reads the soil moisture from the sensor.
2. If the moisture reading is **below the threshold**, the pump activates for the duration you configured.
3. After the pump runs, the system waits until the next check interval and reads the sensor again.
4. This repeats continuously, keeping the soil moisture near your target level.

#### Understanding Hysteresis

The controller also has a **hysteresis** setting (configured on the Configuration page). Hysteresis prevents the pump from rapidly cycling on and off when the soil moisture is hovering right around the threshold.

Here is how it works: once the moisture drops **below the threshold**, the system flags that plant as needing water and activates the pump. After watering, the system will **not stop flagging** that plant as needing water until the moisture rises above the **threshold plus the hysteresis value**. This creates a buffer zone that prevents the pump from triggering again immediately if the moisture is only slightly above the threshold after watering.

For example, if your threshold is 300 pF and your hysteresis is 20 pF, the pump will activate when moisture drops below 300 pF, but the system will not consider the plant "watered enough" until the moisture rises above 320 pF.

!!! note
    Finding the right threshold for your soil and plant takes some experimentation. [Tutorial 5 — Moisture Sensor Calibration](tutorial-5-moisture-sensor-calibration.md) walks you through a calibration procedure to determine the best value for your setup.

---

## The Configuration Page

Navigate to **http://192.168.4.1/configure** or click **Configure** in the navigation bar.

If you built and connected the AgXRP according to the instructions in Tutorial 1, **you should not need to change anything on this page** to get started — the defaults are designed for the base kit with one soil sensor and one pump. The two things you might want to change right away are:

- **Wi-Fi password** — if you want something other than `sensor123`.
- **CSV logging settings** — if you want to record sensor data to a file (see the [CSV Logger](#csv-logger) section below).

The rest of this section explains what each setting does, in the order they appear on the page, so you can come back here when you need to make changes.

---

### Access Point (Wi-Fi Settings)

This is the first section on the Configuration page. It lets you change the Wi-Fi network name and password that the AgXRP broadcasts.

| Field | Description |
|-------|-------------|
| **SSID** | The Wi-Fi network name the AgXRP broadcasts. Change this if you are using multiple AgXRP kits at the same time — each must have a unique name. |
| **Password** | The Wi-Fi password. Must be at least 8 characters. |

![Access Point configuration](images/configure-access-point.jpg)
*Access Point configuration*

!!! warning
    If you change the SSID or password, you will need to reconnect to the new network name after the board reboots.

---

### I2C Bus Configuration

The XRP board has two I2C buses for connecting sensors. Each bus corresponds to a physical qwiic port on the board.

- **Bus 0** — Corresponds to the **qwiic 0** port on the board.
- **Bus 1** — Corresponds to the **qwiic 1** port on the board.

![I2C Bus Configuration section](images/configure-i2c-bus.jpg)
*I2C Bus Configuration section*

Enable the bus that has a sensor connected to it. If you only have one sensor plugged into one port, you only need that bus enabled. The other can be disabled.

---

### Controller

This section lets you enable or disable the automatic watering controller globally. When disabled, no plant systems will water automatically, but you can still control pumps manually from the Dashboard.

![Controller enable/disable section](images/configure-controller-enable.jpg)
*Controller enable/disable section*

---

### Pumps

Configure up to 4 water pumps. You will see an entry for Pump 1.

| Field | What to Set | Description |
|-------|-------------|-------------|
| **Enabled** | Yes | The pump must be enabled for it to appear on the Dashboard. |
| **Pump Index** | `1` | This number identifies the pump and is used to pair it with a sensor in the plant system. |
| **CSV Filename** | `water_pump_1_log.csv` | An optional log file that records every time this pump activates, including the timestamp, duration, and soil moisture at the time. |
| **Max Duration (seconds)** | `60` | A safety limit — the pump will automatically stop after running this long, even if told to run longer. |

![Pump configuration](images/configure-pump-enable.jpg)
*Pump configuration*

---

### Plant Systems

This is where you pair a soil sensor with a pump for automatic watering. Each **Plant System** links a sensor index to a pump index and defines the watering rules for that pair.

The key thing is that the **Sensor Index** and **Pump Index** in the plant system must match the index numbers you assigned to the sensor and pump in their respective sections. With the base kit, Sensor 1 is paired with Pump 1.

![Plant System configuration](images/configure-plant-system.jpg)
*Plant System configuration*

!!! tip "How to Verify the Pairing"
    Go back to the Dashboard and manually start the pump. If the blue LED on your soil sensor turns on, the pairing is correct. If it does not, check that the sensor index and pump index numbers match between the Soil Sensors section, the Pumps section, and the Plant Systems section.

#### Fixing a Sensor/Pump Mismatch

If the wrong pump activates for a given plant (e.g., Sensor 1 is near Plant A but Pump 2 is the one watering it), you have two options:

**Option 1 — Physical swap (easiest):** Unplug the moisture sensor cable and move it to the qwiic port on the same side of the board as the pump that serves that plant. No configuration changes needed.

**Option 2 — Reassign in software:** In the Plant Systems section, change the **Sensor Index** and **Pump Index** values so the correct sensor is paired with the correct pump. For example, if Sensor 1 should control Pump 2, set that plant system to Sensor Index `1` and Pump Index `2`. Click **Save Configuration**, then **Reboot to Apply Changes**.

---

### Sensor Update Interval

Set how frequently (in seconds) the board reads new data from all sensors and updates the Dashboard. The default is `2` seconds.

![Sensor Update Interval configuration](images/configure-sensor-update-period.jpg)
*Sensor Update Interval configuration*

---

### Additional Sensors (CO2, Spectral, Light)

The AgXRP supports several additional sensor types beyond the soil moisture sensor. With the base kit, you do not need to change any of these settings — they are all disabled by default. These sensor types are for optional accessories covered in [Tutorial 3](tutorial-3-additional-sensors-and-pumps.md).

The sections that appear here are:

- **CO2 Sensor (SCD4x)** — For measuring carbon dioxide, temperature, and humidity.
- **Spectral Sensor (AS7343)** — For measuring blue, green, red, and near-infrared light channels.
- **Light Sensor (VEML)** — For measuring ambient light intensity in lux.

Each has an **Enabled** toggle and a **Bus** selector to indicate which I2C bus the sensor is connected to.

![Additional sensor configuration sections](images/configure-additional-sensors.jpg)
*Additional sensor configuration sections (CO2, Spectral, Light)*

---

### Soil Sensors

Configure up to 4 soil moisture sensors. With the base kit, you will see an entry for Soil Sensor 1. Here is what each field means:

| Field | What to Set | Description |
|-------|-------------|-------------|
| **Enabled** | Yes | The sensor must be enabled for it to appear on the Dashboard. |
| **Sensor Index** | `1` | This number is how the sensor is identified on the Dashboard and how it gets paired with a pump in the plant system. |
| **Bus** | `0` or `1` | Set this to match the qwiic port the sensor is plugged into. If you used **qwiic 0**, set this to **0**. If you used **qwiic 1**, set this to **1**. |
| **I2C Address** | `0x37` | Leave this at the default. This is the hardware address of the capacitive soil sensor. |
| **Type** | Capacitive | Must match the physical sensor type. The base kit includes a capacitive sensor. |

![Soil sensor configuration fields](images/configure-soil-sensor.jpg)
*Soil sensor configuration fields*

---

### OLED Screen

Enable this if you have a qwiic-connected OLED display. Select which I2C bus it is connected to. This is an optional accessory not included in the base kit.

![OLED Screen configuration](images/configure-oled.jpg)
*OLED Screen configuration*

---

### CSV Logger

The AgXRP can record sensor data to a CSV file on the board. This is useful for tracking moisture levels and other readings over time.

| Field | Description |
|-------|-------------|
| **Enabled** | Turn CSV logging on or off. Disabled by default. |
| **Filename** | Name of the log file (default: `sensor_log.csv`). |
| **Period (ms)** | How often a row is written, in milliseconds. `5000` = every 5 seconds. |
| **Max Rows** | After this many rows, the log file rotates — the old file is renamed with a `.bak` extension and a new file starts. This prevents the log from filling up the board's storage. |

![CSV Logger configuration](images/configure-csv-file.jpg)
*CSV Logger configuration*

In addition to the sensor log, each pump also has its own log file (configured in the Pumps section). The pump log records every watering event with the timestamp, how long the pump ran, and the soil moisture at the time of watering.

---

### Saving and Applying Changes

At the bottom of the Configuration page:

1. Click **Save Configuration** to write your changes to the board.
2. Click **Reboot to Apply Changes** to restart the board with the new settings.
3. After the board reboots, reconnect to the AgXRP Wi-Fi network and navigate to **http://192.168.4.1**.

!!! warning
    The board must reboot for configuration changes to take effect. Any active pumps will be stopped safely before the reboot.

---

## The Data Page

Navigate to **http://192.168.4.1/data** or click **Data** in the navigation bar.

![Data Viewer page](images/data-viewer.jpg)
*Data Viewer page*

The Data page lets you view and download the CSV log files stored on the board. Two types of log files may be available:

- **Sensor Log** — Contains timestamped readings from all enabled sensors (soil moisture, CO2, light, etc.). This file is only created if you enabled the **CSV Logger** on the Configuration page.
- **Water Pump Logs** — One file per pump, recording each watering event with the timestamp, duration, and soil moisture at the time of watering. These are created automatically when the pump has a CSV filename configured.

### Viewing Data

Click on a log file name to display its contents in a table. The most recent entries appear at the top.

### Downloading Data

Click the green **Download CSV** button above the data table to download the file to your device. You can open it in any spreadsheet application (Excel, Google Sheets, etc.) for further analysis.

!!! note
    If no log files appear, make sure CSV logging is enabled on the Configuration page and that the board has been running long enough to collect data.

---

## Troubleshooting

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Board does not appear as a Wi-Fi network | Board is not powered on, or firmware is missing | Check that the power supply is connected and the power switch is ON. If the network still does not appear, the firmware may need to be reflashed — see [Tutorial 1](tutorial-1-software-setup.md#reflashing-the-firmware-if-needed). |
| Cannot connect to the Wi-Fi network | Wrong password | The default password is `sensor123`. If you changed the password and forgot it, reflash the firmware to reset to defaults. |
| Web page does not load | Browser not navigating to correct address | Go to **http://192.168.4.1** manually. Make sure you are still connected to the AgXRP Wi-Fi. |
| Soil sensor does not appear on Dashboard | Sensor not enabled or wrong bus | Go to Configuration, check that the soil sensor is enabled and the bus number matches the qwiic port you used. Save and reboot. |
| Pump does not respond | Pump not enabled or wrong index | Go to Configuration, check that the pump is enabled. Also check that the motor cable is firmly connected. |
| Blue LED does not light up when pump starts | Sensor/pump pairing mismatch | Make sure the sensor index and pump index match in the Soil Sensors, Pumps, and Plant Systems sections of the Configuration page. |
| Timestamps are wrong after a power outage | Clock reset on reboot | Connect to the Dashboard from any device with a web browser — the clock will automatically sync. |

---

## Next Steps

- Proceed to [Tutorial 3 — Additional Sensors & Pumps](tutorial-3-additional-sensors-and-pumps.md) if you want to add more sensors or pumps.
- Proceed to [Tutorial 4 — Pump Calibration](tutorial-4-pump-calibration.md) to prime and test your water pump.
- Proceed to [Tutorial 5 — Moisture Sensor Calibration](tutorial-5-moisture-sensor-calibration.md) to find the right moisture threshold for your soil.
