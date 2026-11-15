# Mangarita — NASA POWER Climate Explorer

Mangarita is a web application created for the **NASA Space Apps Challenge**. It downloads and analyzes real monthly satellite and meteorological data from the **NASA POWER API** (from 2021 to the present) to compare global climates, explore any coordinate on Earth, and calculate mathematical trend projections.

**Live Web Demo:** [Open Mangarita in your browser](YOUR-LIVE-WEB-LINK-HERE) *(No installation required)*

---

## How it works

1. **Real NASA Data & Local Cache (`reserva_nasa.json`):**
   Mangarita fetches real monthly records directly from `power.larc.nasa.gov` and saves them locally. It comes preloaded with 75 global locations. If there is no internet connection or the NASA API is unreachable, the app keeps working offline using the saved reserve and finds the nearest available location using Haversine great-circle distance.

2. **Dynamic Global Comparison (Up to 9 Locations):**
   The main dashboard starts by comparing 4 distinct climate zones (*Alaska, Bogota, Madrid, and Sydney*). Every time you search for a new coordinate, it is automatically added to the comparison charts. To keep the charts readable, the dashboard holds a maximum of 9 locations: when full, it removes the oldest initial benchmark station first so your searched locations stay visible.

3. **Statistical Significance Testing:**
   Automatically calculates net linear change (`2021–present`) across all variables and applies a Student's t-test (`t > 2.01`, ~95% confidence) to separate real long-term trends from normal seasonal noise.

4. **Two Future Projection Tools:**
   - **See the future (Server-side):** Uses ordinary least squares (linear trend + annual and semi-annual sine/cosine cycles) to project up to 60 months ahead with a 95% prediction range, respecting physical limits (such as 0–100% for humidity).
   - **Predict future years (In-browser):** Calculates a 36-month projection directly in JavaScript using the deseasonalized linear trend and monthly anomalies, working completely offline.

---

## Variables Tracked

- **T2M:** Atmospheric temperature (°C)
- **RH2M:** Relative humidity at 2 m (%)
- **WS10M:** Wind speed at 10 m (m/s)
- **PS:** Surface pressure (kPa)
- **PRECTOTCORR:** Precipitation rate (mm/day)
- **ALLSKY_SFC_SW_DWN:** Solar downward flux (W/m²)

---

## Run it on your computer

1. Clone the repository and enter the folder:
   ```bash
   git clone https://github.com
   cd Mangarita2
   ```

2. Install the required libraries:
   ```bash
   pip install -r requirements.txt
   ```

3. Start the program:
   ```bash
   python mangarita2.py
   ```
   Your browser will open automatically at `http://127.0.0`.

### Extra options
- `python mangarita2.py --no-download` — Runs immediately using only the local `reserva_nasa.json` file.
- `python mangarita2.py --update` — Re-downloads the catalog from NASA to get the latest available months.

---

## Built with
Python (`http.server`, `requests`), `NumPy`, `Pandas`, `Plotly`, and vanilla HTML/CSS/JavaScript.
