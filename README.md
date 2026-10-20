# Topics in Marine Science: Sentinel Glider Data

Python notebooks for the Rutgers University **Topics in Marine Science** course, built around data from the **Sentinel** glider program, a Rutgers / Teledyne Webb Research effort to fly the first autonomous underwater vehicle around the globe.

Everything here runs in **Google Colab** in your web browser. You don't need to install anything on your computer, and you don't need any coding experience to start.

---

## Quick start

1. Sign in to a Google account (your Rutgers Google account works).
2. Pick a notebook from the table below and click its **Open in Colab** badge.
3. In Colab, go to **File → Save a copy in Drive**. That gives you your own copy to edit; changes to the original aren't saved.
4. Run the cells from top to bottom: click ▶ next to a cell or press **Shift + Enter**.

New to Python or Jupyter notebooks? Start with **Activity 1**. It covers the basics before loading any data.

---

## The notebooks

Work through them roughly in order. Each one builds on skills from the last.

| # | Notebook | What you'll do | Data |
|---|---|---|---|
| 1 | **Python basics + first glider data**<br>[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JGradone/Topics-Sentinel/blob/main/Rutgers_Topics_in_Marine_Science_Activity_1_Sentinel_Data.ipynb) | Python crash course (math, variables, lists, NumPy arrays), then load a table with `pandas` and compare CTD data from a Sentinel and an original Slocum glider flown side by side | `Sentinel_Slocum_CTD_Comparison.txt` (Cape Cod Bay) |
| 2 | **Flight diagnostics**<br>[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JGradone/Topics-Sentinel/blob/main/Rutgers_Topics_in_Marine_Science_Activity_2_Sentinel_Data.ipynb) | Load Sentinel flight data (depth, pitch, roll, heading) and explore how the glider flies | `Sentinel_CCBay_Flight_Data.txt` (Cape Cod Bay test flight, Jan 2025) |
| 3 | **Glider metrics**<br>[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JGradone/Topics-Sentinel/blob/main/Rutgers_Topics_in_Marine_Science_Activity_3_Sentinel_Data_ipynb.ipynb) | Group work: calculate horizontal speed and dive/climb speed | `Sentinel_CCBay_Flight_Data.txt` |
| 4 | **Deep flight evaluation**<br>[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JGradone/Topics-Sentinel/blob/main/Rutgers_Topics_in_Marine_Science_Activity_4_Sentinel_Data.ipynb) | Map and compare a Sentinel and a standard Slocum glider on deep dives. The files include buoyancy pump, battery, and CTD data | `Sentinel_Deep_Data.txt`, `Slocum_Deep_Data.txt` (off San Diego, Feb 2025) |
| 5 | **Three Sentinels**<br>[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JGradone/Topics-Sentinel/blob/main/Rutgers_Topics_in_Marine_Science_Activity_5_Sentinel_Data_04082025.ipynb) | Compare three Sentinel gliders, using pre-labelled dives, climbs, and profiles | `Sentinel_00.txt`, `Sentinel_01.txt`, `Sentinel_02.txt` (off San Diego, Mar 2025) |
| ★ | **Redwing deep dive (start here for the Redwing mission)**<br>[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JGradone/Topics-Sentinel/blob/main/Redwing_ERDDAP.ipynb) | Download Redwing's full real-time mission straight from ERDDAP. Engineering: buoyancy engine, pitch, battery, vacuum, currents. Science: profiles, sections, T–S diagrams, dive vs. climb (thermal lag) | Live from the [Rutgers glider ERDDAP server](https://slocum-data.marine.rutgers.edu/erddap/) |
| | Redwing real-time (in-class version, Fall 2025)<br>[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JGradone/Topics-Sentinel/blob/main/Redwing_Real_Time.ipynb) | The shorter notebook used in class, including a comparison of glider temperature with satellite SST | ERDDAP (glider) + NOAA CoastWatch (satellite SST) |

There are two copies of Activity 5. `..._Activity_5_Sentinel_Data_04082025.ipynb` is the newer version (April 8, 2025). The older `..._Activity_5_Sentinel_Data.ipynb` is kept for reference.

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
* **Activities 1–5** read small `.txt` (CSV) files stored in this repository. The notebooks load them directly from GitHub, so you don't need to download anything.
* **The Redwing notebooks** pull data live from the [Rutgers glider ERDDAP server](https://slocum-data.marine.rutgers.edu/erddap/). Redwing's dataset ID is `redwing-20251011T1511-trajectory-raw-rt`. You can browse every variable [here](https://slocum-data.marine.rutgers.edu/erddap/tabledap/redwing-20251011T1511-trajectory-raw-rt.html). The `load_glider_data()` function in `Redwing_ERDDAP.ipynb` works for **any** glider on that server if you give it a different dataset ID.

---

## Python packages used

Colab already includes `numpy`, `pandas`, `matplotlib`, and `xarray`. Some notebooks install a few extra packages in their first cells (this takes a few seconds):

| Package | What it's for |
|---|---|
| [`erddapy`](https://ioos.github.io/erddapy/) | Requesting data from ERDDAP servers |
| [`gsw`](https://teos-10.github.io/GSW-Python/) | Seawater properties (salinity, density) using TEOS-10 |
| [`cmocean`](https://matplotlib.org/cmocean/) | Colormaps made for oceanography (`thermal`, `haline`, `dense`, ...) |
| [`geopy`](https://geopy.readthedocs.io/) | Distances between latitude/longitude points (Activities 3–4) |
| [`cartopy`](https://scitools.org.uk/cartopy/) | Maps with coastlines (Activity 4) |

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `NameError: name 'df' is not defined` (or any variable) | You skipped a cell. Run the cells above it, or use **Runtime → Run before**. |
| `ModuleNotFoundError: No module named 'erddapy'` (or `gsw`, `cmocean`) | Run the `pip install` cell at the top of the notebook. |
| Things are acting strangely | **Runtime → Restart session and run all**. |
| `HTTPError` 502 / 503 from a data server | The server is busy. Wait a minute and run the cell again. |
| SSL `CERTIFICATE_VERIFY_FAILED` from the Rutgers ERDDAP server | `Redwing_ERDDAP.ipynb` handles this automatically. If you write your own code, copy its `read_csv_from_url()` function. |
| My edits disappeared | You were editing the original from GitHub. Use **File → Save a copy in Drive** first. |

---

## Ideas for projects

* Pick any glider on the [Rutgers ERDDAP server](https://slocum-data.marine.rutgers.edu/erddap/tabledap/index.html) and run the Redwing notebook on it.
* Use Redwing's battery data to estimate how long a full-depth mission can last.
* Map where Redwing met the strongest currents and compare with the Gulf Stream's position.
* Measure CTD thermal lag by comparing dive and climb profiles. When does it matter most?
* Compare glider near-surface temperature with satellite SST (see `Redwing_Real_Time.ipynb`).

---

## Repository contents

```
Rutgers_Topics_in_Marine_Science_Activity_*.ipynb   Course activities 1–5
Redwing_ERDDAP.ipynb                                Redwing engineering + science deep dive (ERDDAP)
Redwing_Real_Time.ipynb                             Fall 2025 in-class Redwing notebook (includes satellite SST)
Sentinel_Slocum_CTD_Comparison.txt                  Activity 1 data
Sentinel_CCBay_Flight_Data.txt                      Activities 2–3 data
Sentinel_Deep_Data.txt, Slocum_Deep_Data.txt        Activity 4 data
Sentinel_00.txt, Sentinel_01.txt, Sentinel_02.txt   Activity 5 data
```

---

*Course materials by Joe Gradone, Rutgers University. Questions? Open an issue on this repository or ask in class.*
