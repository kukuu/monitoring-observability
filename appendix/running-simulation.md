# Installation and  Simulation Guide

- Install dependencies

```
npm install sqlite3 onoff prom-client express

```


## Run Simulation, Visualisation Dashboard & GPIO Tests


- Start the server:

```
node index.js

```

- Terminal  (curl):


```
curl -X POST http://localhost:5000/api/simulate \
  -H "Content-Type: application/json" \
  -d '{"steps": 100}'

```


- Access the web interface at:

 - Simulation: http://localhost:5000
   
 - Reset Simulation: POST http://localhost:5000/api/reset

 - Metrics: http://localhost:5000/metrics
   
 - Dashboard: http://localhost:5000/dashboard

- How Dashboard Works: https://github.com/kukuu/raspberry-pie-digital/blob/main/conveyor-belt-nodejs-JS/docs/simulation-control-dashboad.md

- Unit tests
```

npm test test/unit/gpio.test.js

npm run test:integration

# Hardware integration test (mock mode)
npm test test/integration/hardware.test.js

# Physical GPIO test (on Raspberry Pi)
node scripts/test-gpio.js
```



- Browser Console Tests: https://github.com/kukuu/raspberry-pie-digital/blob/main/appendix/browser-console-test.md

- Browser Console Response: https://github.com/kukuu/raspberry-pie-digital/blob/main/appendix/Browser-console-response.md


### All endpoints

1. POST /api/simulate

2. POST /api/reset

3. GET /metrics

4. GET / (web interface)




