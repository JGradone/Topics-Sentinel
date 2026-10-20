# Topics in Marine Science: Sentinel Glider Data

Colab notebooks for the Rutgers University **Topics in Marine Science** course. You'll learn Python by exploring real data from **Redwing**, a Slocum glider flying the **Sentinel** mission. Sentinel is a Rutgers / Teledyne Webb Research effort to fly the first autonomous underwater vehicle around the globe.

Everything here runs in **Google Colab** in your web browser. You don't need to install anything on your computer, and you don't need any coding experience to start.

---

## Quick start

1. Sign in to a Google account (your Rutgers Google account works).
2. Click an **Open in Colab** badge below.
3. In Colab, go to **File → Save a copy in Drive**. That gives you your own copy to edit; changes to the original aren't saved.
4. Run the cells from top to bottom: click ▶ next to a cell or press **Shift + Enter**.

---

## The notebooks

| # | Notebook | What you'll do |
|---|---|---|
| 1 | **Intro to Python and gliders**<br>[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JGradone/Topics-Sentinel/blob/main/Activity_1_Intro_to_Python_and_Gliders.ipynb) | Python crash course (math, variables, strings, lists, NumPy arrays), then download your first Redwing data from ERDDAP with `load_glider_data()`, look at it with `pandas`, and make your first plots |
| 2 | **Redwing glider data deep dive**<br>[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JGradone/Topics-Sentinel/blob/main/Activity_2_Redwing_Glider_Data.ipynb) | Download Redwing's whole mission. **Engineering:** buoyancy engine, pitch, battery, vacuum, currents. **Science:** profiles, depth–time sections, T–S diagrams, dive vs. climb (thermal lag). Ends with group exercises |

Do Activity 1 first. Activity 2 uses the same `load_glider_data()` function and builds on what you learned.

---

## Background you'll want

### What is a glider?
A **Slocum glider** is a torpedo-shaped robot about 2 m long that flies through the ocean without a propeller. It pumps oil in and out of an external bladder to become heavier or lighter than seawater, and its wings turn that sinking and rising into forward motion. It flies a sawtooth ("yo") pattern, here down to ~1000 m and back. At the surface it gets a GPS fix and sends data home by satellite. A single glider can stay at sea for months.

### Engineering vs. science data
Gliders record two kinds of data:

* **Engineering (flight) data** describe the vehicle: depth, pitch, roll, heading, buoyancy pump volume, battery position, battery charge, internal vacuum.
* **Science data** describe the ocean, mostly from the **CTD** (Conductivity–Temperature–Depth sensor). From conductivity, temperature, and pressure we calculate salinity and density.

### Variable names
Slocum variable names start with a prefix that tells you where they come from:

| Prefix | Meaning | Examples |
|---|---|---|
| `m_` | **measured** by the flight computer | `m_depth`, `m_pitch`, `m_battery`, `m_de_oil_vol` |
| `c_` | **commanded**: what the glider was told to do | `c_heading`, `c_de_oil_vol` |
| `sci_` | measured by the **science** computer / sensors | `sci_water_temp`, `sci_water_cond`, `sci_water_pressure` |
| `x_` | derived onboard | `x_de_oil_flux` |

Watch the units. Angles (`m_pitch`, `m_heading`) are in **radians**, `sci_water_pressure` is in **bar** (×10 for decibars ≈ metres), and `sci_water_cond` is in **S/m** (×10 for mS/cm, which is what `gsw` expects).

### Where the data come from
Both notebooks pull data live from the [Rutgers glider ERDDAP server](https://slocum-data.marine.rutgers.edu/erddap/), so there are no files to download. Redwing's dataset ID is `redwing-20251011T1511-trajectory-raw-rt`, and you can browse every variable [here](https://slocum-data.marine.rutgers.edu/erddap/tabledap/redwing-20251011T1511-trajectory-raw-rt.html).

`load_glider_data()` works for **any** glider on that server. Give it a different `dataset_id` to try another one.

---

## Python packages used

Colab already includes `numpy`, `pandas`, and `matplotlib`. The first code cell of each notebook installs the rest (this takes a few seconds):

| Package | What it's for |
|---|---|
| [`erddapy`](https://ioos.github.io/erddapy/) | Requesting data from ERDDAP servers |
| [`gsw`](https://teos-10.github.io/GSW-Python/) | Seawater properties (salinity, density) using TEOS-10 (Activity 2) |
| [`cmocean`](https://matplotlib.org/cmocean/) | Colormaps made for oceanography (`thermal`, `haline`, `dense`, ...) (Activity 2) |

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `NameError: name 'df' is not defined` (or any variable) | You skipped a cell. Run the cells above it, or use **Runtime → Run before**. |
| `ModuleNotFoundError: No module named 'erddapy'` (or `gsw`, `cmocean`) | Run the `pip install` cell at the top of the notebook. |
| Things are acting strangely | **Runtime → Restart session and run all**. |
| `HTTPError` 502 / 503 from a data server | The server is busy. Wait a minute and run the cell again. |
| SSL `CERTIFICATE_VERIFY_FAILED` from the Rutgers ERDDAP server | The notebooks handle this automatically. If you write your own code, copy the `read_csv_from_url()` function. |
| My edits disappeared | You were editing the original from GitHub. Use **File → Save a copy in Drive** first. |

---

## Ideas for projects

* Pick any glider on the [Rutgers ERDDAP server](https://slocum-data.marine.rutgers.edu/erddap/tabledap/index.html) and run Activity 2 on it.
* Use Redwing's battery data to estimate how long a full-depth mission can last.
* Map where Redwing met the strongest currents and compare with the Gulf Stream's position.
* Measure CTD thermal lag by comparing dive and climb profiles. When does it matter most?
* Compare Redwing's near-surface temperature with satellite sea surface temperature, for example NOAA's [Blended SST](https://coastwatch.noaa.gov/erddap/griddap/noaacwBLENDEDsstDNDaily.html), which is also on ERDDAP.

---

## Repository contents

```
Activity_1_Intro_to_Python_and_Gliders.ipynb   Python basics + first Redwing data
Activity_2_Redwing_Glider_Data.ipynb           Redwing engineering + science deep dive
README.md                                      This guide
```

Older notebooks from the 2025 course (Sentinel test flights in Cape Cod Bay and off San Diego) are still in the git history.

---

*Course materials by Joe Gradone, Rutgers University. Questions? Open an issue on this repository or ask in class.*
