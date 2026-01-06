

## Project Title

**Satellite‑Based Water Balance Recharge Model for Bangladesh (2022)**

---

## Description

This repository contains a **production‑ready Google Earth Engine (GEE) implementation** of a national‑scale **potential vertical groundwater recharge model** for Bangladesh. The model applies a physically consistent annual water‑balance formulation using satellite and land‑surface model datasets.

The recharge estimate is derived as a residual of precipitation, evapotranspiration, runoff, and seasonal soil‑moisture storage gain:

> **R = max(P − ET − Q − dS⁺, 0)**

The model is designed for **national screening, spatial comparison, and hydro‑climatic analysis**, not for site‑specific groundwater budgeting.

---

## Scientific Basis

The model follows a classical FAO‑style vertical water balance framework and integrates:

* Satellite precipitation (IMERG)
* Satellite evapotranspiration (MODIS MOD16)
* Land‑surface runoff (GLDAS NOAH)
* Seasonal soil‑moisture storage dynamics

All components are harmonized to a common spatial grid and annual temporal scale.

---

## Datasets Used

| Component               | Dataset              | Resolution         | Notes                    |
| ----------------------- | -------------------- | ------------------ | ------------------------ |
| Precipitation (P)       | GPM IMERG V07        | ~10 km             | Half‑hourly accumulation |
| Evapotranspiration (ET) | MODIS MOD16A2 v061   | 500 m (aggregated) | QC‑filtered              |
| Runoff (Q)              | GLDAS NOAH v2.1      | 0.25°              | Accumulator differencing |
| Storage Change (dS⁺)    | GLDAS RootMoist_inst | 0.25°              | Seasonal soil wetting    |
| Boundary                | USDOS LSIB           | Vector             | National boundary        |

---

## Methodology

### 1. Precipitation (P)

* IMERG half‑hourly precipitation summed directly
* No temporal scaling applied
* Output: annual precipitation depth (mm)

### 2. Evapotranspiration (ET)

* MODIS MOD16A2 ET product
* QC masking applied (ET_QC = 0)
* Scaled using official factor (0.1)
* Aggregated to annual total

### 3. Runoff (Q)

* GLDAS surface and subsurface runoff accumulators (Qs_acc, Qsb_acc)
* Timestep runoff computed via **temporal differencing**
* Annual runoff depth derived by summation

### 4. Storage Gain (dS⁺)

* Root‑zone soil moisture (RootMoist_inst)
* Dry‑season (Feb–Mar) vs wet‑season (Jul–Sep) mean
* Only positive storage changes retained
* Interpreted as seasonal soil wetting, not aquifer storage

### 5. Recharge Calculation

```text
R = max(P − ET − Q − dS⁺, 0)
```

The result represents **potential vertical recharge**, not actual aquifer recharge.

---

## Spatial and Temporal Resolution

* Temporal scale: Annual (calendar year 2022)
* Spatial resolution: ~10 km (IMERG grid)
* All components explicitly reprojected to a common grid

---

## Outputs

* Annual recharge raster (mm/year)
* Individual water‑balance components (P, ET, Q, dS⁺)
* National‑scale zonal statistics (mean, min, max, std. dev.)

---

## Intended Use

✔ National groundwater recharge screening
✔ Climate variability and spatial pattern analysis
✔ Water‑resources planning support

✖ Local aquifer recharge estimation
✖ Engineering‑scale groundwater modeling

---

## Limitations

* Floodplain‑river interactions are not explicitly modeled
* Irrigation return flow is not included
* Recharge represents an **upper‑bound potential**, not observed recharge
* Soil moisture is used as a proxy for seasonal storage, not vadose‑zone thickness

---

## Requirements

* Google Earth Engine account
* GEE Code Editor (JavaScript API)

No local software installation is required.

---

## How to Use

1. Open the Google Earth Engine Code Editor
2. Paste the script from this repository
3. Run the script
4. Inspect map layers and console outputs
5. Export results to Google Drive if needed


