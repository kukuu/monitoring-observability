# Conveyor Belt Execution Steps 

## Development steps
-  The simulation initializes a 10-slot conveyor belt with 3 worker pairs.
-  Components arrive randomly (1/3 chance each for A, B, or empty).
-  Workers collect components (1 per hand), assemble product C (4-step process), and place it back.
-  GPIO LEDs indicate active workers (requires wiringPi on Pi).
-  Metrics are exposed on /metrics for Prometheus.
-  To run:
```
npm install && node index.js

```
 - Tests:
```
npm test

```

- Production:

i. containerize with Docker, deploy with PM2. 

ii. Grafana dashboard visualizes throughput and component statistics.

iii. Assumes Node 14+, works cross-platform with GPIO simulation on non-Pi systems.


## Conveyor Belt Simulation - Node.js and JS solution

https://github.com/kukuu/raspberry-pie-digital/blob/main/conveyor-belt-nodejs-JS.md
