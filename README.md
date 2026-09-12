# IceFlow — Plant OS

> Live box-count monitoring and telemetry portal for commercial ice plants.

IceFlow provides legal proof-of-delivery box accounting, optical proximity gate telemetry at the loading dock, individual customer contract pricing, and real-time plant floor HMI visualization across mobile and desktop.

![IceFlow Banner](https://raw.githubusercontent.com/mrayees3042-arch/ice-flow/main/preview.png)

## Features

- **DOCK-02 Optical Proximity Telemetry:** Real-time counting via optical infrared proximity sensor at the vehicle loading ramp with 1,200 ms debounce filtering (filtering conveyor bounce and damp box wobble). Power cut / outage buffer simulation with automatic sync upon reconnect.
- **Yield & Box Economics:** Real-time accounting based on 50 kg commercial ice blocks (1 crushed block = 2 boxes of 25 kg each). Eliminates abstract "units" in favor of concrete box tallies and block crush metrics.
- **Individual Customer Contract Pricing:** Flexible per-customer billing (e.g. ₹75 to ₹90 / box contract rates) replacing flat rates.
- **Operator Station & Van Batching:** Dedicated touch UI for dock operators with 1-tap customer selection, live van load accumulation, vehicle seal & close dispatch button, manual bypass, and instant undo.
- **Verified Customer Statement:** One-click legal proof-of-delivery statement for customer accounts with timestamps, box count, block equivalents, and agreed contract rates.
- **Interactive Plant Floor HMI:** Animated SVG simulation of gantry crane (`CR-02`), ice crusher (`IC-01`), conveyor (`CV-03`), box packing (`PK-01`), and cold-chain sealed load-out vans (`DOCK-02`).
- **Owner Dashboard & Orders:** Dynamic box dispatch chart, customizable date windows (7D, 14D, 30D), customer volume/revenue breakdowns, and live order book with CSV export.
- **Role-based Authentication:** Instant switching between **Plant Owner**, **Operator**, and **Service Admin** views.

## Architecture & Integration Points

The application is engineered with explicit swap points for enterprise deployment:

1. **Authentication:** Replace the demo `LoginScreen` session with Supabase, Firebase, or Clerk authentication.
2. **Plant Historical API:** Connect `buildData` and `buildOrders` to your backend REST/GraphQL endpoints:
   - `GET /api/plants/{plantId}/daily?days=60`
   - `GET /api/plants/{plantId}/orders`
3. **Live Box Stream:** Replace `useBoxStream` with an SSE or MQTT-over-WebSocket client subscribed to `plant/{plantId}/box_done` (`{ ts, ci, debounced }`).
4. **Edge Telemetry:** Bind `dev` state to real ESP32 / industrial gateway heartbeat telemetry (RSSI, offline buffer depth, power source).

## Getting Started

Because IceFlow is built using standalone React 18, Tailwind CSS, and Babel, no complex build tools or Node modules are required to run it locally:

1. Clone the repository:
   ```bash
   git clone https://github.com/mrayees3042-arch/ice-flow.git
   cd ice-flow
   ```
2. Open `index.html` directly in any modern web browser or serve via a lightweight HTTP server:
   ```bash
   npx serve .
   # or Python:
   python -m http.server 3000
   ```
3. Visit `http://localhost:3000` (or open the file directly).

## PWA Support

IceFlow includes manifest and meta tags for standalone progressive web app installation:
- On iOS (Safari): tap **Share** &rarr; **Add to Home Screen**.
- On Android (Chrome): tap the three dots menu &rarr; **Install App** / **Add to Home screen**.

## License

MIT License.
