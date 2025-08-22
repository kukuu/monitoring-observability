# Verification Steps:
## Test Original Functionality:

```
curl -X POST http://localhost:5000/api/simulate -H "Content-Type: application/json" -d '{"steps":10}'
```

Expected: Should return same JSON response as before

## Check GPIO Metrics:

```
curl http://localhost:5000/metrics | grep gpio_status
```

Expected output:

1. gpio_status{pin="belt_active"} 1

2. gpio_status{pin="worker_1"} 0

3. gpio_status{pin="worker_2"} 0 

4. gpio_status{pin="worker_3"} 0

## Hardware Test (on Raspberry Pi):

```
watch -n 1 'gpio readall'  # Monitor pin states in real-time
```
