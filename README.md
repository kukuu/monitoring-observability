# Monitoring and Observabiity

Deploying Prometheus and Grafana.

## Monitoring and Logging

1. Prometheus and Grafana:

i. Deploy Prometheus and Grafana using Helm:

```
helm install prometheus prometheus-community/prometheus
helm install grafana grafana/grafana

```

ii. Configure Prometheus to scrape metrics from the backend.

### Prometheus Configuration 

In here we  configure Prometheus to scrape metrics from the Node.js backend in the application described in Step 6. This includes setting up Prometheus to monitor the backend service and visualize the metrics in Grafana.

#### Steps:

**1. Instrument the Backend for Prometheus:**

i. Install the prom-client library to expose metrics in the Node.js backend:

```
npm install prom-client
```

ii. Update the backend code (backend/src/index.ts) to expose metrics: 

```
import express from 'express';
import { collectDefaultMetrics, register } from 'prom-client';

const app = express();
const port = 3001;

// Collect default metrics (CPU, memory, etc.)
collectDefaultMetrics();

app.get('/api', (req, res) => {
  res.json({ message: 'Hello from the backend!' });
});

// Expose metrics endpoint
app.get('/metrics', async (req, res) => {
  res.set('Content-Type', register.contentType);
  res.end(await register.metrics());
});

app.listen(port, () => {
  console.log(`Backend running on http://localhost:${port}`);
});
```

**2. Create a Prometheus Configuration File:**

i. Create a prometheus.yml file to define the scraping configuration:

```
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node-backend'
    static_configs:
      - targets: ['backend:3001']  # Kubernetes service name for the backend

```
**3. Deploy Prometheus in Kubernetes:**

i. Create a Kubernetes ConfigMap for the prometheus.yml file: 

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: prometheus-config
data:
  prometheus.yml: |
    global:
      scrape_interval: 15s

    scrape_configs:
      - job_name: 'node-backend'
        static_configs:
          - targets: ['backend:3001']
```

ii. Apply the ConfigMap: 

```
kubectl apply -f k8s/prometheus-config.yaml
```

iii. Deploy Prometheus using a Kubernetes Deployment:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: prometheus
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: prometheus
    spec:
      containers:
      - name: prometheus
        image: prom/prometheus
        args:
          - '--config.file=/etc/prometheus/prometheus.yml'
        ports:
        - containerPort: 9090
        volumeMounts:
        - name: prometheus-config
          mountPath: /etc/prometheus
      volumes:
      - name: prometheus-config
        configMap:
          name: prometheus-config
```
iv. Apply the Deployment:

```
kubectl apply -f k8s/prometheus-deployment.yaml
```

v. Expose Prometheus via a Service:

```
apiVersion: v1
kind: Service
metadata:
  name: prometheus
spec:
  type: NodePort
  ports:
  - port: 9090
    targetPort: 9090
  selector:
    app: prometheus

```

vi. Apply the Service:

```
kubectl apply -f k8s/prometheus-service.yaml
```


**4. Access Prometheus Dashboard:**

i. Open the Prometheus dashboard by accessing the service:

```
kubectl port-forward svc/prometheus 9090:9090
```

ii. Visit


http://localhost:9090 in your browser to view metrics.

5. Visualize Metrics in Grafana:

i. Deploy Grafana in Kubernetes:

```
helm install grafana grafana/grafana
```

ii. Access Grafana and add Prometheus as a data source: https://github.com/kukuu/deployment/blob/main/grafana-dashboard.md

iii. Create dashboards to visualize metrics like request rates, error rates, and system performance.


### ELK Stack for Logging:

i. Deploy Elasticsearch, Logstash, and Kibana:

```
helm install elasticsearch elastic/elasticsearch
helm install kibana elastic/kibana
```

ii. Send logs from the backend to Elasticsearch.

### Visualize Metrics in Grafana:

Create dashboards in Grafana to monitor application performance.
