# Conveyor Belt Simulation 

**Node.js Solution**

(With Persistence, GPIO, Tests, Monitoring & Deployment)

## Implementation Summary

This solution simulates a conveyor belt with 3 worker pairs, where components (A/B) are randomly placed (⅓ chance each). Workers pick components, assemble product C (takes 4 steps), and place it back. The system includes:

- SQLite persistence (stores simulation results)

- GPIO integration (LEDs indicate worker activity on Raspberry Pi)

- Unit & integration tests (Jest)

- Prometheus + Grafana for real-time monitoring

- PM2 for production deployment

### Folder Structure 
```

/conveyor-belt  
├── index.js                # Main app & API  
├── lib/  
│   ├── conveyor.js         # Belt logic & simulation  
│   ├── workers.js          # Worker behavior  
│   ├── gpio.js             # GPIO control (falls back to mock if not on Pi)  
│   └── db.js               # SQLite persistence  
├── test/  
│   ├── unit/               # Unit tests (Jest)  
│   └── integration/        # API & DB tests  
├── config/  
│   ├── prometheus.yml      # Prometheus scraping config  
│   └── grafana-dashboard.json # Pre-configured dashboard  
├── package.json            # Dependencies & scripts  
└── README.md               # Setup instructions


``` 
