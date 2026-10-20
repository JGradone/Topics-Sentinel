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
| 1 | **Intro to Python and gliders**<br>[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JGradone/Topics-Sentinel/blob/main/Activity_1_Intro_to_Python_and_Gliders.ipynb) | Python crash course (math, variables, strings, lists, NumPy arrays), then download three days of Redwing data from ERDDAP with `load_glider_data()`, look at it with `pandas`, and make your first plots |
| 2 | **Redwing engineering data**<br>[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JGradone/Topics-Sentinel/blob/main/Activity_2_Redwing_Engineering.ipynb) | How the glider flies: buoyancy engine, pitch, heading and fin (measured vs. commanded), vertical speed over the mission. Glider health: vacuum and battery. The ocean currents it fought. Ends with group exercises |
| 3 | **Redwing science data**<br>[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JGradone/Topics-Sentinel/blob/main/Activity_3_Redwing_Science.ipynb) | What the CTD measured: quality-control flags, whole-mission temperature/salinity/density sections, the Gulf Stream front, T–S diagrams and water masses, and dive vs. climb (sensor thermal lag). Ends with group exercises |

Do Activity 1 first. Activities 2 and 3 can be done in either order (or by different groups at the same time). All three use exactly the same `load_glider_data()` function.

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
All three notebooks pull data live from the [Rutgers glider ERDDAP server](https://slocum-data.marine.rutgers.edu/erddap/), so there are no files to download.

During a mission a glider only sends a small sample of its data home by satellite (**real-time** data). We use the **delayed-mode** data: the complete files stored on the glider, with every measurement (about one per second), processed after they were retrieved. Redwing has two delayed-mode datasets:

| Name in the code | Dataset ID | What's in it | Used in |
|---|---|---|---|
| `ENGINEERING_ID` | [`redwing-20251011T1511-trajectory-raw-delayed`](https://slocum-data.marine.rutgers.edu/erddap/tabledap/redwing-20251011T1511-trajectory-raw-delayed.html) | everything the glider logged: flight data plus raw CTD, Oct 2025 – May 2026 | Activities 1 and 2 |
| `SCIENCE_ID` | [`redwing-20251011T1511-profile-sci-delayed`](https://slocum-data.marine.rutgers.edu/erddap/tabledap/redwing-20251011T1511-profile-sci-delayed.html) | processed CTD profiles with salinity, density and QARTOD quality flags, Oct 2025 – Jan 2026 | Activity 3 |

The delayed-mode engineering files don't include the battery variables, so Activity 2 gets those from the real-time dataset, [`redwing-20251011T1511-trajectory-raw-rt`](https://slocum-data.marine.rutgers.edu/erddap/tabledap/redwing-20251011T1511-trajectory-raw-rt.html).

Because the full-resolution data are millions of rows, the notebooks use ERDDAP **server-side filters** through `load_glider_data(..., filters=[...])`. For example, `'m_depth!=NaN'` keeps only rows with a depth, and `'orderByMean("profile_id,pressure/5")'` averages each profile into 5 dbar bins before sending it.

`load_glider_data()` works for **any** glider on that server. Give it a different dataset ID to try another one.

---

## Python packages used

Colab already includes `numpy`, `pandas`, and `matplotlib`. The first code cell of each notebook installs the rest (this takes a few seconds):

| Package | What it's for |
|---|---|
| [`erddapy`](https://ioos.github.io/erddapy/) | Requesting data from ERDDAP servers |
| [`cmocean`](https://matplotlib.org/cmocean/) | Colormaps made for oceanography (`thermal`, `haline`, `dense`, ...) (Activities 2 and 3) |
| [`gsw`](https://teos-10.github.io/GSW-Python/) | Seawater properties (density, conservative temperature) using TEOS-10 (Activity 3) |

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

* Pick any glider on the [Rutgers ERDDAP server](https://slocum-data.marine.rutgers.edu/erddap/tabledap/index.html) and run Activities 2 and 3 on it.
* Use Redwing's battery data to estimate how long a full-depth mission can last.
* Map where Redwing met the strongest currents and compare with the Gulf Stream's position.
* Measure CTD thermal lag by comparing dive and climb profiles. When does it matter most?
* Track the mixed layer deepening from fall into winter.
* Map how thick the Eighteen Degree Water layer was along the track.
* Compare real-time and delayed-mode data for the same day. What did the glider leave out?
* Compare Redwing's near-surface temperature with satellite sea surface temperature, for example NOAA's [Blended SST](https://coastwatch.noaa.gov/erddap/griddap/noaacwBLENDEDsstDNDaily.html), which is also on ERDDAP.

---

## Repository contents

```
Activity_1_Intro_to_Python_and_Gliders.ipynb   Python basics + first Redwing data
Activity_2_Redwing_Engineering.ipynb           How Redwing flew, its health, and the currents it met
Activity_3_Redwing_Science.ipynb               What Redwing's CTD measured in the ocean
README.md                                      This guide
```

Older notebooks from the 2025 course (Sentinel test flights in Cape Cod Bay and off San Diego) are still in the git history.

---

*Course materials by Joe Gradone, Rutgers University. Questions? Open an issue on this repository or ask in class.*
