Progress So Far:
ACTDRS
│
├── ✅ Day 1 : Environment Setup
├── ✅ Day 2 : Project Structure
├── ✅ Day 3 : Monitoring Engine
│      ├── CPU Monitor
│      ├── RAM Monitor
│      ├── Disk Monitor
│      ├── Process Monitor
│      └── Continuous Monitoring
│
└── ▶ Day 4 : Threat Detection Engine
Day 4 Architecture:
          Monitoring Engine
                  │
                  ▼
          Collect System Data
                  │
                  ▼
      Threat Detection Engine
                  │
        ┌─────────┴─────────┐
        │                   │
     Normal            Suspicious
        │                   │
 Continue Monitoring    Generate Alert
