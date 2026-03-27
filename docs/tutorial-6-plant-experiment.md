# Tutorial 6 — Plant Experiment

With your AgXRP system fully set up and calibrated, you are ready to design and run a plant growth experiment. This tutorial provides guidance on obtaining seeds, setting up lighting, and running your experiment with the AgXRP monitoring system.

!!! note "Before You Begin"
    Make sure you have completed Tutorials 1–5. Your AgXRP system should be wired, the pumps primed, and the moisture sensors calibrated before starting a plant experiment.

## What You'll Need

| Item | Notes |
|------|-------|
| Seeds | Wisconsin Fast Plants recommended — see Step 1 |
| Gardening pots | One per plant system |
| Potting soil | Already dried and calibrated from Tutorial 5 |
| Light source | Window, grow light, or outdoor location — see Step 2 |

---

## Step 1 — Obtain Seeds

Many seed varieties are available, but **Wisconsin Fast Plants** are strongly recommended for classroom experiments due to their rapid growth cycle (approximately 14 days from germination to flowering).

Purchase seeds from a supplier. Wisconsin Fast Plants from Carolina Biological Supply are recommended:

[Carolina Biological Supply — Wisconsin Fast Plants](https://www.carolina.com/browse/product-search-results?q=seed&tab=p&product.type=Product&product.brandName=wisconsin-fast-plants&facetFields=product.brandName)

---

## Step 2 — Set Up Lighting

Choose a lighting source for your plants. Options include:

- A classroom window with consistent natural light.
- An outdoor location (weather permitting).
- Dedicated plant growth lights *(recommended for most reliable results)*.

!!! tip
    If using growth lights, position them approximately 2–4 inches above the top of the plants and keep them on for 16–24 hours per day for best results with Fast Plants.

---

## Step 3 — Plant Your Seeds

Plant your seeds according to the seed supplier's instructions. Place the pots so that each pot's moisture sensor is fully inserted into the soil and the pump tube outlet is positioned to water that pot.

---

## Step 4 — Take Measurements

Take regular measurements throughout the experiment. Suggested measurements include:

- Plant height
- Day of germination and first flowering
- Number of leaves
- Leaf area
- Leaf color or overall plant appearance

!!! tip
    Use the AgXRP Dashboard and Data page to monitor soil moisture readings over time. Download the CSV log files to compare against your plant growth observations and understand the relationship between watering and growth.

---

## Step 5 — Explore and Experiment

Get creative! Use additional equipment and tools available to you to explore new variables and questions. For example, you could compare plant growth under different watering thresholds, light conditions, or soil types.

![Example AgXRP plant experiment](images/placeholder-missing.svg)
*Example AgXRP plant experiment*

---

!!! info "Moon Experiment"
    To conduct a plant experiment that simulates lunar conditions, follow the lesson plans at: `_____________________________`

    *(Link to be filled in before distributing this guide.)*

---

## Troubleshooting

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Seeds not germinating after several days | Seeds too old, too deep, or too dry | Check the supplier's recommended planting depth. Verify the AgXRP is watering correctly by monitoring the Dashboard. |
| Plant wilting despite automatic watering | Moisture threshold set too low, or pump not delivering enough water | Check the soil moisture reading on the Dashboard. If it isn't rising after a watering cycle, increase **Pump Duration** or check that the pump tube is delivering water directly to the pot. |
| Soil moisture reading not changing after watering | Sensor not fully inserted, or water not reaching the sensor | Make sure the sensor's sensing area is fully submerged in soil. Check that the pump tube outlet is positioned near the sensor and not draining out the bottom of the pot immediately. |
| Soil stays wet and never triggers watering | Moisture threshold set too high | Lower the **Soil Moisture Threshold** on the Dashboard. Re-run the calibration in Tutorial 5 if needed. |
| Pump activates but plant still dries out quickly | Pot drains too fast, or **Pump Duration** too short | Increase **Pump Duration** to deliver more water per cycle, or reduce **Check Interval** so the system waters more frequently. |
