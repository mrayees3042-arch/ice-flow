# IceFlow — Plant OS

> Live box-count monitoring and telemetry portal for commercial ice plants.

IceFlow provides box-level accounting, edge scale telemetry, customer sales mix analysis, and real-time plant floor visualization across mobile and desktop.

![IceFlow Banner](https://raw.githubusercontent.com/mrayees3042-arch/ice-flow/main/preview.png)

## Features

- **Live Edge Device Telemetry:** Real-time weighing and counting with RS-232 scale integration, tare tracking, power cut / outage buffer simulation, and automatic sync upon reconnect.
- **Interactive Plant Floor HMI:** Animated visual representation of gantry crane (`CR-02`), ice crusher (`IC-01`), conveyor (`CV-03`), box packing (`PK-01`), and cold-chain sealed load-out vans (`DOCK-02`).
- **Comprehensive Sales & Metrics:** Dynamic daily sales chart, customizable date windows (7D, 14D, 30D), customer volume/revenue breakdowns, and live order book with CSV export.
- **Operator Station View:** Dedicated mobile-friendly packing bench touch UI with 1-tap customer attribution, manual count overrides, and instant undo.
- **Role-based Authentication:** Instant switching between **Plant Owner**, **Operator**, and **Service Admin** views.

## Architecture & Integration Points

The application is engineered with explicit swap points for enterprise deployment:

1. **Authentication:** Replace the demo `LoginScreen` session with Supabase, Firebase, or Clerk authentication.
2. **Plant Historical API:** Connect `buildData` and `buildOrders` to your backend REST/GraphQL endpoints:
   - `GET /api/plants/{plantId}/daily?days=60`
   - `GET /api/plants/{plantId}/orders`
3. **Live Box Stream:** Replace `useBoxStream` with an SSE or MQTT-over-WebSocket client subscribed to `plant/{plantId}/box_done` (`{ ts, w_kg, units }`).
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
