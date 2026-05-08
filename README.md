# Industrial IoT — Node-RED, MQTT, Modbus

> Nine labs of industrial-IoT work — building dashboards, wiring up MQTT brokers, talking to PLCs over Modbus, and bridging shop-floor data into web-friendly endpoints.

![Status](https://img.shields.io/badge/status-complete-brightgreen)
![Course](https://img.shields.io/badge/course-IOT555-blue)
![Tool](https://img.shields.io/badge/tool-Node--RED-red)
![Comms](https://img.shields.io/badge/comms-MQTT%20%7C%20Modbus-green)

This repo collects deliverables from **IOT555 — Industrial Internet of Things** at Seneca Polytechnic. It covers Node-RED dashboards, MQTT topic design, Modbus RTU/TCP integration, and the data-bridge patterns that turn legacy PLC I/O into live web telemetry. The capstone of the course was the dashboard work that ended up driving the final TPJ653 capstone — same toolchain, real production deployment.

---

## What's inside

### Lab progression

| Lab | Focus |
| --- | --- |
| 1–2 | Node-RED fundamentals — flows, nodes, debug, deployment |
| 3 | Dashboard nodes — gauges, charts, controls |
| 4 | MQTT — broker, topic design, retained vs live messages |
| 5 | Modbus RTU + TCP — talking to PLC tags from Node-RED |
| 6 | Data persistence — file, CSV, SQLite |
| 7 | Edge computing — running Node-RED on Raspberry Pi |
| 8 | Authentication, HTTPS, basic security hardening |
| 9 | Integration — putting it all together |

### Capstone-style integration

Lab 9 wired everything into a small end-to-end pipeline: a PLC's I/O exposed over Modbus → Node-RED reads tags → publishes to an MQTT broker → dashboard subscribes for live display, while a logging path writes to CSV for later analysis. This same pattern was used for real on the **TPJ653 capstone** — see [Filling-capping-sorting-capstone](https://github.com/harpreetsingh52004/Filling-capping-sorting-capstone) for the production version.

---

## Tech Stack

| Layer | Technology |
| --- | --- |
| **Runtime** | Node-RED on local laptop / Raspberry Pi |
| **Comms** | MQTT (Mosquitto broker), Modbus RTU + Modbus TCP |
| **Dashboard** | Node-RED dashboard nodes (gauges, charts, switches, text) |
| **Persistence** | CSV files + SQLite for time-series logging |
| **PLC integration** | CLICK PLC, Siemens S7, Allen-Bradley CompactLogix (across labs) |

---

## Patterns I leaned on

- **MQTT topic design.** Hierarchical topics (`site/cell/station/metric`) so the dashboard can subscribe to broad slices or specific data points without rebuilding flows
- **Retained messages for state, live for events.** Status flags published with `retain: true` so the dashboard knows the latest state on connect; sequential events published live, no retain
- **Modbus polling without spamming the broker.** Fixed-interval polling on the PLC side, change-on-value publishing on the broker side — small bandwidth, fresh data
- **Dashboards as configuration, not code.** Layouts and theming kept in flow JSON so the dashboard moves with the project
- **Operator inputs round-trip.** Setpoints written from the dashboard go back into the PLC over Modbus and re-read for confirmation — no silent failures

---

## What I learned

- **Most "IoT" complexity is data design, not code.** A clean topic taxonomy and a clear notion of state vs. event makes everything downstream easier.
- **Dashboards earn their keep at commissioning, not in production.** The real value of a Node-RED dashboard isn't day-to-day monitoring — it's having something visible during commissioning so you can see what the PLC actually thinks is happening.
- **Modbus is fine.** It's old, but it's everywhere, and you can do real work with it. The spec is small enough to read in an afternoon.
- **Edge first, cloud later.** Running Node-RED on a Pi at the edge gives you a solid, working system before any cloud questions show up. Most of the value is local.

---

📫 **Harpreet Singh** — [harpreetsingh.cloud](https://harpreetsingh.cloud) · [GitHub](https://github.com/harpreetsingh52004)
