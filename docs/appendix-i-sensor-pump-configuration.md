# Appendix i — Additional Sensor & Pump Configuration

This appendix covers two scenarios: (1) correcting a mismatch between sensor and pump assignments, and (2) enabling optional sensors that are not active by default. All configuration is done through the web interface — no code editing required.

---

## Part A — Fixing a Sensor/Pump Mismatch

By default, **Sensor 1** is paired with **Pump 1** and **Sensor 2** is paired with **Pump 2** in the plant system configuration. If the sensor connected near a plant is controlling the wrong pump, use one of the following solutions.

### Solution 1 — Physical Swap *(Easiest)*

Move the moisture sensor cable so that it connects to the qwiic port on the same side as the pump serving that plant. This requires no configuration changes.

### Solution 2 — Reassign in the Configuration Page

**Step 1.** Connect to the AgXRP Wi-Fi network and open **http://192.168.4.1/configure** in your browser.

**Step 2.** Scroll down to the **Plant Systems** section.

**Step 3.** For each plant system, change the **Sensor Index** and **Pump Index** values so that the correct sensor is paired with the correct pump.

- For example, if Sensor 1 is near the plant watered by Pump 2, set Plant System 1 to use Sensor Index `1` and Pump Index `2`.

![Image A1 — Plant Systems section of the Configuration page, showing Sensor Index and Pump Index fields](images/placeholder-screenshot-config-plant-systems-swap.svg)
*Image A1 — Plant Systems section showing Sensor Index and Pump Index fields*

**Step 4.** Click **Save Configuration**, then click **Reboot to Apply Changes**.

**Step 5.** Reconnect to the AgXRP Wi-Fi network and verify on the Dashboard that the correct sensor and pump are now paired together.

---

## Part B — Enabling Optional Sensors

The AgXRP supports additional sensors beyond the soil moisture sensors. These include the **CO2 sensor (SCD4x)**, **Light sensor (VEML)**, **Spectral sensor (AS7343)**, and **OLED display**. By default, only the soil moisture sensors and basic configuration are active. Follow these steps to enable additional sensors.

### Step 1 — Connect the Sensor Hardware

Daisy-chain the additional sensor(s) to one of the moisture sensors by connecting a qwiic cable between the moisture sensor's open (right-hand) qwiic connector and the new sensor's qwiic connector.

![Image A2 — Daisy-chain of sensors from qwiic port 0 (bus 0)](images/placeholder-missing.svg)
*Image A2 — Daisy-chain of sensors from qwiic port 0 (bus 0)*

!!! note
    All sensors daisy-chained from the same qwiic port share the same I2C bus. Sensors connected from **qwiic 0** are on **Bus 0**; sensors connected from **qwiic 1** are on **Bus 1**. Remember which bus you connected the new sensor to — you will need this in the next step.

---

### Step 2 — Enable the Sensor on the Configuration Page

**Step 1.** Connect to the AgXRP Wi-Fi network and open **http://192.168.4.1/configure** in your browser.

**Step 2.** Find the section for the sensor you are adding:

| Sensor | Configuration Section |
|--------|-----------------------|
| CO2 (SCD4x) | **CO2 Sensor (SCD4x)** |
| Light (VEML) | **Light Sensor (VEML)** |
| Spectral (AS7343) | **Spectral Sensor (AS7343)** |
| OLED Display | **OLED Screen** |

**Step 3.** Set the sensor to **Enabled**.

**Step 4.** Set the **Bus** to match the qwiic port the sensor is daisy-chained from:

- If you connected to **qwiic 0** → select **Bus 0**
- If you connected to **qwiic 1** → select **Bus 1**

![Image A3 — Enabling the CO2 sensor and selecting Bus 0 on the Configuration page](images/placeholder-screenshot-config-enable-sensor.svg)
*Image A3 — Enabling the CO2 sensor and selecting Bus 0*

**Step 5.** Click **Save Configuration**, then click **Reboot to Apply Changes**.

**Step 6.** Reconnect to the AgXRP Wi-Fi network and check the Dashboard. The new sensor's readings should now appear in the Sensor Data section.

![Image A4 — Dashboard showing newly enabled CO2, temperature, and humidity readings](images/placeholder-screenshot-dashboard-new-sensors.svg)
*Image A4 — Dashboard showing newly enabled CO2, temperature, and humidity readings*

---

## Part C — Adding a Second Soil Sensor or Pump

If you want to add a second soil moisture sensor or a second pump, you can configure these on the Configuration page as well.

### Adding a Second Soil Sensor

**Step 1.** Physically connect the second soil sensor to the other qwiic port on the board (e.g., if Sensor 1 is on **qwiic 1**, connect Sensor 2 to **qwiic 0**).

**Step 2.** Open **http://192.168.4.1/configure** and scroll to the **Soil Sensors** section.

**Step 3.** Find the second soil sensor entry and set it to **Enabled**. Set the **Bus** to match the qwiic port you connected it to, and make sure the **Sensor Index** is set to `2`.

**Step 4.** Click **Save Configuration**, then **Reboot to Apply Changes**.

### Adding a Second Pump and Plant System

**Step 1.** Physically connect the second pump to the remaining motor port on the board.

**Step 2.** Open **http://192.168.4.1/configure** and scroll to the **Pumps** section.

**Step 3.** Find the second pump entry and set it to **Enabled**. Set the **Pump Index** to `2`.

**Step 4.** Scroll to the **Plant Systems** section and configure a second plant system that pairs **Sensor 2** with **Pump 2**. Set the check interval, threshold, duration, and effort as desired.

**Step 5.** Click **Save Configuration**, then **Reboot to Apply Changes**.

!!! tip
    For help calibrating moisture thresholds for the new plant system, see [Tutorial 5 — Moisture Sensor Calibration](tutorial-5-moisture-sensor-calibration.md). For a more detailed walkthrough of adding sensors and pumps, see [Tutorial 3 — Additional Sensors & Pumps](tutorial-3-additional-sensors-and-pumps.md).
