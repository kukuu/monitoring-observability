
# Architecture

## Key Decisions

- Modular Separation:

Conveyor logic handles only belt state

Workers manage their own FSM (Finite State Machine)

GPIO abstracted for cross-platform support

- Data Flow

https://github.com/kukuu/raspberry-pie-digital/blob/main/conveyor-belt-nodejs-JS/conveyor-belt-flow-diagram.png

## Data Architecture

```

┌───────────────────────────────────────────────────────────────────────────────┐
│                   CONVEYOR BELT SIMULATION ARCHITECTURE                       │
└───────────────────────────────────────────────────────────────────────────────┘

                      ┌───────────────────────────────┐
                      │      Requirement Analysis     │
                      │  - Component Flow Rules       │
                      │  - Worker Constraints         │
                      │  - Output Metrics             │
                      └───────────────┬───────────────┘
                                      │
                                      ▼
┌───────────────────────┐  ┌───────────────────────┐  ┌───────────────────────┐
│   Conveyor Algorithm  │  │    Worker Algorithm   │  │    GPIO Interface     │
│                       │  │                       │  │                       │
│  • Slot Management    │◀─┤  • Pick/Place Logic   │◀─┤  • Hardware Abstraction
│  • Component Generation│  │  • Assembly Timing   │  │  • Mock Mode          │
│  • Belt Rotation      │  │  • Hand State Machine │  └───────────────────────┘
└──────────┬────────────┘  └──────────┬────────────┘
           │                          │
           ▼                          ▼
┌───────────────────────┐  ┌───────────────────────┐
│   Simulation Engine   │  │     Data Persistence   │
│                       │  │                       │
│  • Time-step Control  ├─▶│  • SQLite Operations  │
│  • State Propagation  │  │  • Metric Recording   │
└──────────┬────────────┘  └───────────────────────┘
           │
           ▼
┌───────────────────────┐  ┌───────────────────────┐
│       API Layer       │  │      Monitoring       │
│                       │  │                       │
│  • REST Endpoints     │  │  • Prometheus Metrics │
│  • Request Validation ├─▶│  • Grafana Dashboard  │
└──────────┬────────────┘  └───────────────────────┘
           │
           ▼
┌───────────────────────┐
│      Client UI        │
│                       │
│  • Real-time Viz      │
│  • Control Interface  │
└───────────────────────┘

```

## Key Decision Tree for Algorithms:
1. Component Generation → [Random?] → Yes: 1/3 probability each (A/B/empty)

2. Worker Action → [Assembling?] → Yes: Decrement timer

             │→ No → [Has A+B?] → Yes: Start assembly

             │→ No → [Slot has component?] → Yes: Pick if valid

3. Product Placement → [Assembly complete?] → Yes: Find empty even slot

4. Metrics Collection → [Step complete?] → Yes: Update OEE calculations

## Dependency Graph:

conveyor.js → workers.js → gpio.js

    ↓             ↓
simulation.js ← db.js

    ↓
api.js → {metrics.js, client/}



## Optimisation Paths

1. Slot indexing (O(1) access)

2. Worker zone partitioning

3. Hardware-accelerated GPIO

## Failure Mitigation

1. Circuit breakers in API calls

2. Exponential backoff for hardware

3. Statistical process control