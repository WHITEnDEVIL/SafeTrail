# 🏔️ SafeTrail: Offline-First Tourist Safety & Incident Response Ecosystem



[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Flutter%20%7C%20React%20%7C%20Node.js-green.svg)](#tech-stack)
[![Architecture](https://img.shields.io/badge/Architecture-Offline--First%20%7C%20Edge--Native-orange.svg)](#system-architecture)

---

##  1. Project Overview

Every trekking season across high-altitude and deep-forest corridors (such as Himachal, Uttarakhand, and Ladakh), tourists face life-threatening risks due to sudden weather shifts, rugged cliffs, and complete cellular blackouts. Existing emergency mechanisms (such as dial 112) fail entirely without 4G/5G data connectivity, forcing disaster response forces (SDRF/NDRF) into blind, multi-day searches across 50 km² grids.

**SafeTrail** solves this by delivering an end-to-end, zero-hardware-overhead safety platform:
1. **Offline Tourist Mobile Client:** Runs on the tourist's existing smartphone to execute mathematical on-device geo-fencing against pre-cached trail vectors with zero network dependency.
2. **Multi-Tier Incident Dispatch Pipeline:** Transmits emergency telemetry via single-burst 2G SMS signaling and peer-to-peer Bluetooth Low Energy (BLE) store-carry-forward hops.
3. **Ranger & SDRF Command Console:** Equips emergency dispatchers with real-time incident queues, automated overdue hiker timers, and precise search coordinates, shrinking search radiuses down to a 50-meter perimeter.



##  2. System Architecture
>>
              │                 TOURIST MOBILE DEVICE                  │
              │  - GPS Receiver (NMEA Sentences / Location Stream)     │
              │  - Local Vector Cache (GeoJSON MBTiles)                │
              │  - Point-in-Polygon Engine (Ray-Casting Algorithm)     │
              │  - Encrypted Local SQLite DB                           │
              └───────────────────────────┬────────────────────────────┘
                                          │
                  ┌───────────────────────┴───────────────────────┐
                  │                                               │
           [Cellular Fringe]                               [Zero Network]
                  │                                               │
                  ▼                                               ▼
      ┌───────────────────────┐                       ┌───────────────────────┐
      │   Tier 1: 2G SMS      │                       │   Tier 2: BLE Mesh    │
      │   Signaling Burst     │                       │   Epidemic Hop        │
      │ (64-byte Base64 Hex)  │                       │ (Store-Carry-Forward) │
      └───────────┬───────────┘                       └───────────┬───────────┘
                  │                                               │
                  └───────────────────────┬───────────────────────┘
                                          │
                                          ▼
              ┌────────────────────────────────────────────────────────┐
              │            TRAILHEAD MICRO-SERVER / GATEWAY            │
              │  - Solar-Powered Standalone Edge Node                  │
              │  - Local Wi-Fi Direct Vector Map Distribution          │
              │  - Checkpoint Check-In / Check-Out Log Registry        │
              │  - Tier 3 Overdue Incident State Engine                │
              └───────────────────────────┬────────────────────────────┘
                                          │ (VSAT / Fiber / Radio)
                                          ▼
              ┌────────────────────────────────────────────────────────┐
              │             RANGER & SDRF COMMAND DASHBOARD            │
              │  - Real-Time Incident Priority Queue (Triage Score)    │
              │  - Leaflet / Mapbox Geospatial Dispatch Map            │
              │  - Verifiable Permit Audit Ledger                      │
              └────────────────────────────────────────────────────────┘


##  3. Key Technical Innovations

### A. Mathematical On-Device Geo-Fencing (Ray-Casting PIP)
Rather than making continuous cloud API calls, the client evaluates whether a hiker is within a designated boundary polygon using the **Ray-Casting (Even-Odd) Algorithm**:
* Projects an imaginary horizontal ray from the user's current coordinate $(x_0, y_0)$ to positive infinity.
* Counts intersections against edge segments connecting vertices $A(x_i, y_i)$ and $B(x_{i+1}, y_{i+1})$.
* An **odd number of intersections** confirms the user is within the safe trail zone; an **even number** indicates a boundary departure.
* **Optimization:** Incorporates an initial $\mathcal{O}(1)$ rectangular **Bounding Box (BBox)** filter to bypass trigonometric and edge arithmetic unless the hiker is within 200 meters of a perimeter.

### B. Multi-Tiered Fail-Safe SOS Pipeline
**Tier 1 (2G SMS Fallback):** When 4G data bands drop, the modem transmits a single-burst uncompressed SMS packet encoded in 64 characters:
  
  [Timestamp (4B) | Lat-Long (8B) | Battery% (1B) | Medical ID (2B) | Checksum (2B)]


**Tier 2 (Opportunistic BLE Epidemic Mesh): In complete blackouts, devices advertise encrypted distress beacons. Passing hikers automatically relay the packet along the trail corridor without requiring manual pairing or interaction.

**Tier 3 (Trailhead Overdue Protocol): If the phone battery is depleted due to extreme cold, the trailhead checkpost initiates an automated dispatch trigger when the pre-authorized permit window expires



**Team GeoVector
Nikhil Kumar — Team Lead & System Architect


Team Members: Frontend Mobile Lead, GIS & Geo-Fencing Engineer, Network & BLE Specialist, Incident Operations Coordinator, Feasibility & Policy Analyst.




