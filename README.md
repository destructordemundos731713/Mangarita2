# 🛰️ Mangarita — NASA POWER Climate Explorer & Forecaster

**Mangarita** is an interactive web application built for the **NASA Space Apps Challenge**. It transforms real satellite and meteorological observations from the **NASA POWER (Prediction Of Worldwide Energy Resources)** API into global comparative dashboards, coordinate-based exploration, and mathematical climate projections.

🌐 **Live Web Demo:** [Click here to try Mangarita online](YOUR-LIVE-WEB-LINK-HERE) *(No installation required)*

---

## ✨ Key Features

* **Verified NASA Satellite Data (Zero Hallucinations):** Retrieves monthly meteorological observations from `2021` to the present directly from NASA's Langley Research Center (`power.larc.nasa.gov`), filtering out missing (`-999`) or annual summary records.
* **Preloaded Global Catalog & Local Reserve (`reserva_nasa.json`):** Comes pre-seeded with **75 global locations** spanning the Americas, Europe, Africa, Asia, Oceania, and Antarctica. Every new coordinate searched by a user is automatically appended to the local JSON reserve. If NASA's API is unreachable, Mangarita falls back to the nearest saved location using Haversine great-circle distance.
* **Dynamic Global Comparison Dashboard (Smart 9-Station Cap):**
  * Initializes with 4 distinct climate benchmarks: *Alaska (Arctic)*, *Bogota (Equatorial)*, *Madrid (Mediterranean)*, and *Sydney (Southern Hem.)*.
  * **Real-Time Learning:** Every coordinate queried in the search view is dynamically added to the main comparison dashboard via `/api/dashboard`.
  * **Anti-Saturation Filter:** Limits the comparative view to a maximum of **9 simultaneous locations**. When full, it protects user-searched coordinates by retiring the oldest initial benchmark stations first, followed by FIFO rotation.
* **Statistical Significance Testing:** Automatically computes net linear change (`2021–present`) across all tracked variables and runs a **Student's t-test** (`t > 2.01`, ~95% confidence) to distinguish statistically significant trends from normal seasonal noise.
* **Dual Mathematical Projection Engines:**
  1. **"See the future" (Server-Side Harmonic Regression):** Fits a linear trend plus annual and semi-annual seasonal cycles (`sin`/`cos` harmonics) via Ordinary Least Squares, plotting a **95% prediction interval** from `12` up to `60` months ahead while enforcing physical boundaries (e.g., humidity `0–100%`).
  2. **"Predict future years" (100% In-Browser Projection):** Computes a `36`-month forecast directly in JavaScript using deseasonalized linear trends and monthly anomalies—requiring zero server calls or internet connectivity.

---

## 📊 Tracked Atmospheric & Climate Variables

| Parameter Code | Variable Name | Unit | Physical Bounds Enforced |
| :--- | :--- | :--- | :--- |
| `T2M` | Atmospheric temperature | °C | Unbounded |
| `RH2M` | Relative humidity (2 m) | % | `0.0` to `100.0` |
| `WS10M` | Wind speed (10 m) | m/s | `≥ 0.0` |
| `PS` | Surface pressure | kPa | `≥ 0.0` |
| `PRECTOTCORR` | Precipitation rate | mm/day | `≥ 0.0` |
| `ALLSKY_SFC_SW_DWN` | Solar downward flux | W/m² | `≥ 0.0` *(auto-converted from kWh/m²/day or MJ/m²/day)* |

---

## 🚀 How to Run Locally

1. **Clone this repository:**
   ```bash
   git clone https://github.com
   cd YOUR-REPO-NAME
   ```

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the application:**
   ```bash
   python mangarita2.py
   ```
   *(Opens automatically in your default browser at `http://127.0.0`)*

### Command-Line Options
* `python mangarita2.py --no-download` — Starts immediately using only `reserva_nasa.json` without querying NASA on startup.
* `python mangarita2.py --update` — Re-downloads and refreshes all catalog and saved locations from NASA POWER.

---

## ☁️ Cloud Deployment Architecture
Mangarita is cloud-ready out of the box:
* Binds to `0.0.0.0` and dynamically reads the `PORT` environment variable (compatible with **Render**, **Hugging Face Spaces**, and **Railway**).
* Automatically detects cloud environments (`PORT` present) to suppress local browser launches and runs background catalog synchronization in a non-blocking daemon thread when `reserva_nasa.json` is already populated.

---

## 🛠️ Tech Stack
* **Backend:** Python 3 (`http.server.ThreadingHTTPServer`, `threading`, `requests`)
* **Math & Data Processing:** `NumPy`, `Pandas` (Ordinary Least Squares, Harmonic Regression, Haversine Distance, Student's t-test)
* **Frontend & Visualization:** Vanilla HTML5/CSS3/JS, Animated SVG Mascot, `Plotly.js`
