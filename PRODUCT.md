# PRODUCT.md — Business & Product Context

## Official project title

**Thai:** โครงการความร่วมมือในการประยุกต์ใช้เทคโนโลยี AI และ Machine Learning เพื่อส่งเสริมอุตสาหกรรมกระจายเสียง กิจการโทรทัศน์ และกิจการโทรคมนาคม ในพื้นที่ชายขอบ

**English:** The Application and Cooperation of AI and Machine Learning Technologies in the Border Area to Support the National Broadcasting and Telecommunication Commission

**Funder:** NBTC (กทปส) Research and Development Fund — Section 52(2) | Signed: 7 March 2025

---

## What this is

A fiber-optic intrusion detection system for Thailand's border. Buried fiber detects and classifies ground-coupled vibrations from illegal crossings, contraband drops, and related activity. The system upgrades from "something happened at position X" (signal detection) to "a human / vehicle / animal crossed at position X" (intelligent interpretation).

## Partners and stakeholders

| Role | Party |
|------|-------|
| Research institution | KMITL — International Aviation Industry College |
| PI | Asst. Prof. Dr. Soemsak Yuyuen (Dean), `soemsak.yo@kmitl.ac.th` |
| End user | Royal Thai Army — กองกำลังสุรศักดิ์มนตรี (Suranaree Military Force) |
| Funder | NBTC (กทปส) Research Fund, Section 52(2) |
| Coordinator | Kantima Imsri, `kantima.im@kmitl.ac.th` |

## Problem

Thailand has ~5,600 km of border with Myanmar (2,400 km), Laos (1,800 km), Cambodia (800 km), Malaysia (600 km). Natural terrain channels are used for illegal immigration, weapons/drug smuggling, and human trafficking. Current monitoring: insufficient personnel, outdated equipment, no automated coverage of natural terrain. MOU with military area owner required before any field work begins.

## Event taxonomy

| Label | Description |
|---|---|
| `human_footstep` | Single person or small group on foot |
| `vehicle_light` | Motorcycle, ATV, light truck |
| `vehicle_heavy` | Truck, heavy machinery |
| `digging` | Manual or mechanical excavation |
| `dropped_object` | Impact from a dropped item (contraband drop) |
| `animal` | Wildlife movement (deer, boar, etc.) |
| `environmental` | Rain, wind, distant road traffic — background / negative class |

## Pilot deployment

- **Location:** กองกำลังสุรศักดิ์มนตรี border area (exact coordinates withheld — sensitive)
- **Scope:** 5 km fiber run, 3 candidate points pre-selected
- **Prerequisites:** MOU signed, field survey completed, soil/terrain assessment done

## Output to operators

- Web dashboard + mobile alert app, real-time display
- RBAC access control (role-based)
- Alert format: position along fiber + event class + confidence
- Historical data management

## Success metrics (from NBTC proposal)

- 1 working AI/ML detection system deployed
- ≥ 90% of trained operators able to use the system independently
- 8 training sessions × 50 personnel

## Project timeline (12 months from grant approval)

| Quarter | Milestone |
|---------|-----------|
| Q1 | Literature review, equipment procurement |
| Q1–Q2 | Real-field data collection, training database |
| Q2–Q3 | AI model development (FPGA/Neural Network) |
| Q3 | Web app + mobile alert application |
| Q3–Q4 | On-site testing and data collection |
| Q4 | Field deployment, operator training, final report |

## Project risks (from NBTC proposal)

1. Hardware damage in harsh field environment
2. False alarms from non-target vibrations (rain, wind, traffic)
3. Environmental factors (temperature, humidity) reducing model accuracy
4. Soil density variation affecting signal propagation
5. Installation errors affecting alert reliability

---

## Key technical decisions locked

- **Sensing:** Φ-OTDR DAS (Rayleigh backscattering) — confirmed
- **Inference hardware:** FPGA (low-latency, high-frequency operation)
- **Dataset:** Built from scratch — no public Thai fiber vibration dataset exists
- **Training:** Supervised learning on real-field recordings (walking, vehicle, animal, background)
