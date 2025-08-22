# Running the Simulation

- Install dependencies

```
npm install sqlite3 onoff prom-client express

```

- Run simulation

```
node index.js

```

- Test

```
npm test
```

- Deploy with PM2

```

pm2 start index.js --name "conveyor-belt"
pm2 save
pm2 startup

```

- Monitor with Grafana


i. Import grafana-dashboard.json

ii. View real-time metrics




  
### Running Conveyor belt simulation: Port 5000

- Run Simulation: POST http://localhost:5000/
- Reset Simulation: POST http://localhost:5000/api/reset
- Metrics: http://localhost:5000/metrics

### GPIO Tests

```
# Unit tests
npm test test/unit/gpio.test.js

npm run test:integration

# Hardware integration test (mock mode)
npm test test/integration/hardware.test.js

# Physical GPIO test (on Raspberry Pi)
node scripts/test-gpio.js
```

### Browser Console Tests

- https://github.com/kukuu/raspberry-pie-digital/blob/main/appendix/browser-console-test.md

### Browser Console Response

- https://github.com/kukuu/raspberry-pie-digital/blob/main/appendix/Browser-console-response.md

### Command Line Tests:

From Command Line (curl):


```
curl -X POST http://localhost:5000/api/simulate \
  -H "Content-Type: application/json" \
  -d '{"steps": 100}'

```

- Start the server:

```
node index.js

```
- Access the web interface at:

 Simulation: http://localhost:5000

 Metrics: http://localhost:5000/metrics



## All endpoints

1. POST /api/simulate

2. POST /api/reset

3. GET /metrics

4. GET / (web interface)




