# 🧭 User guide

A tour of everything you can do in the CLIMO dashboard (`CLIMO_backend.html`).

> **Tip:** Everything in the left sidebar is an **opt-in layer**. Switch a toggle on to show the layer and its controls. Switch it off and everything that layer drew is cleared.

---

## Top bar

| Control | What it does |
|---|---|
| 🔍 **Search** | Type a city, station or state. Matching stations appear instantly. After about 400 ms, any locality in India is also searched through OpenStreetMap. Picking a station opens its drawer. Picking a place drops a red pin and shows live weather for it. |
| 🟢 **Stations online** | Live count of how many observatories loaded |
| 📄 **Export Report** | Downloads a CSV of every station's current reading and risk score |
| 📍 **My Location** | Uses GPS to name your exact area (e.g. *Model Colony, Shivajinagar, Pune*), fetches the weather at your exact coordinates, and opens it in the drawer with the nearest official station |
| 🌐 **Language** | Choose one of 16 languages. Your choice is remembered. |

---

## Map layers (sidebar)

### 🔵 Stations
Shows the 90-station network. Nearby markers group into numbered clusters, which split apart as you zoom in. The icon shows the sensor type. Click **ⓘ** in the drawer to see the legend.

### 🟠 Risk Heatmap
Click **🌡️ Temp**, **💧 Humidity** or **🌧️ Rainfall** to draw a live heatmap from current station readings. Click the active button again to turn it off.

### 🔴 Buffer Analysis
- Draws dashed **15 km** buffers around every station whose risk is **≥ 34**. Click a buffer for *Buffer Insights*: area, risk, and how many stations it overlaps.
- **Search Any Location** (or tap 📍) draws **5/10/15/20 km** rings around any place and opens a live weather and risk card.

### 🟣 Live Weather (State Level)
Pick **Temperature**, **Rainfall**, **Humidity** or **Wind Speed** to colour every state. An animated atmosphere (rain streaks, wind particles, heat shimmer, humidity haze) is drawn over the map. Click a state for its detailed panel.

### 🩵 Weather-Safe Route
1. Set a **Start**: type a place, tap 📍 for GPS, or tap 📌 and then click the map.
2. Set a **Destination**: type a place or pin it.
3. Click **🧭 Find weather-safe route**.

CLIMO draws the **shortest route** (red, dashed) and checks live rainfall every 25 km along it. If it finds storm cells, it asks for a **weather-safe route** (teal) that avoids them. A comparison card shows the extra minutes and kilometres, and lists each hazard. **✕ Close route** clears everything.

### 🔴 Alert Notification
1. Enter your mobile number.
2. Tap 📍 so CLIMO knows which station to watch.
3. Choose a threshold: **Moderate 34+**, **High 50+** or **Severe 66+**.
4. Click **💾 Save & Enable**. Optionally click **🔔 Allow browser alerts**.

**🧪 Simulate high-risk alert** runs the full alert pipeline on demand. SMS delivery is currently simulated (see [SECURITY.md](../SECURITY.md#sms-alerts)).

### Top Alerts Across States
The six stations with the highest live risk (score ≥ 20). Click a card to fly to that station and open its drawer.

---

## Base maps
Click the **layers icon** (◈) under the zoom buttons to switch between **OpenStreetMap** and **Satellite**.

---

## Station drawer

| Section | Details |
|---|---|
| Header | State, sensor type, station name, IMD code |
| Current conditions | WMO weather label and icon |
| Nearest official station | Distance to the nearest other station. Tap it to draw the road route and open that station. |
| Past hour · Now · Next 24 hours | Hourly icon, temperature and rain probability. The *Now* tile uses the live reading. |
| 7-Day Forecast | Daily max/min temperature and rain probability, always starting today (IST) |
| Metrics | Temperature, humidity, rain this hour, wind |
| Regional Flood Risk | Score from 0 to 100 with a coloured bar, and the 24 h rainfall behind it |
| 💬 Local feedback | Post what you see on the ground (waterlogging, blocked road, power cut, all clear). Posts appear in the drawer for this session. |

---

## Weather Time Explorer

Click the bar at the top-left of the map to expand it.

| Row | Range | Use |
|---|---|---|
| **Top slider** | 48 h of history → 10-day forecast, hourly | Drag it, or press ▶, to replay or preview risk across every station. Alerts and buffers update as it moves. |
| **Bottom slider** | **2000 → 2030**, yearly | Shows the climate heatmap for that year. ▶ animates it. The label changes from `OBSERVED` to `FORECAST (LSTM)` at 2026. |

Collapsing the explorer also hides the climate heatmap and its legend.

---

## 🤖 CLIMO Copilot

Open it with the 💬 button. Try:

| Ask | You get |
|---|---|
| `weather in Nagpur` | Current weather and advice for that place |
| `will it rain near me?` | Uses GPS, then gives a forecast for your area |
| `weather in Pune tomorrow` / `in 2 hours` / `tonight` | Forecast for that time |
| `any flood risk?` / `highest risk locations` | The top 5 stations by risk. The map flies to the worst one. |
| `rainfall outlook` | Stations with the most rain right now |
| `nearest station` | The closest official observatory to you |
| `help` | Everything Copilot can do |

The quick chips (*Today's weather*, *Rainfall outlook*, *Nearest station*, *Map help*) send common questions with one tap.

---

## Click anywhere

When no tool is waiting for a map click, tapping any point on the map drops a pin there, looks up the place name, and opens live weather and risk for that exact spot.

---

## Mobile tips

- **☰** opens the layers drawer. Tap the map to close it.
- The time explorer docks at the bottom of the screen.
- In the Android app, **Export Report** hands the CSV to the system so it can be saved or shared.
