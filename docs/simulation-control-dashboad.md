# Features

1. Data Fetching: The dashboard  fetches data from the /api/simulation-history endpoint

2. Real-time Updates: Click "Refresh Data" to get the latest simulation results

3. Historical Data: Shows a history of simulation runs with timestamps

4. Current State: Displays the current state of the conveyor system

5. Visualization: Updates the bar chart with historical data

6. Navigation: "Back to Control" button returns to the main control page

## How It Works:

1. When the dashboard loads, it automatically fetches data from the API

2. The chart is populated with historical simulation data

3. The metrics display shows the current system state

4. The history list shows details of previous simulation runs

5. Users can refresh the data at any time to get the latest results

PS: The dashboard now shares data with the control page through the Express API, providing a comprehensive view of the simulation history and current system state.
