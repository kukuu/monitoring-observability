# Raspberry Pie Digital

A practical Node.js cum JavaScript steps to building a digital Raspberry Pie - a ToDo exercise  for building and testing a digital experience. 

## Practical step-by-step guide using JavaScript (Node.js) on a Raspberry Pi to build and test a simple ToDo App 

**A -  Using API REST end-point** 

_1. Set Up Raspberry Pi_

- Flash OS: Install Raspberry Pi OS (Lite or Desktop) using Raspberry Pi Imager.

- Enable SSH: Use sudo raspi-config to enable SSH for remote access.

- Update: Run sudo apt update && sudo apt upgrade -y.

_2. Install Node.js_

```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
node -v  # Verify installation (v18+)

```
_3. Create a Simple ToDo App_

**Backend (Express.js)**

i. Initialis project

```
mkdir todo-app && cd todo-app
npm init -y
npm install express body-parser

```

ii. Create server.js:

```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
app.use(bodyParser.json());

let tasks = [];

// Add task
app.post('/tasks', (req, res) => {
  tasks.push(req.body.task);
  res.send(`Added: ${req.body.task}`);
});

// List tasks
app.get('/tasks', (req, res) => {
  res.json(tasks);
});

app.listen(3000, () => console.log('Server running on port 3000'));

```
**Frontend (HTML/JS)**

- Create public/index.html:

1. 
```
<!DOCTYPE html>
<html>
<body>
  <h1>ToDo App</h1>
  <input id="taskInput" placeholder="New task">
  <button onclick="addTask()">Add</button>
  <ul id="taskList"></ul>

  <script>
    async function addTask() {
      const task = document.getElementById('taskInput').value;
      await fetch('http://localhost:3000/tasks', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ task })
      });
      loadTasks();
    }

    async function loadTasks() {
      const response = await fetch('http://localhost:3000/tasks');
      const tasks = await response.json();
      document.getElementById('taskList').innerHTML = tasks.map(t => `<li>${t}</li>`).join('');
    }
    loadTasks();
  </script>
</body>
</html>
```

**Serve static files in server.js:**

```
app.use(express.static('public'));
```

_4. Run and Test_

i. Start the Server

```
node server.js

```

ii. Access the app:

Open a browser on the Pi (or another device on the same network) and navigate to:

```
http://<RASPBERRY_PI_IP>:3000

```
NB: Use:

```

hostname -I
```
to find the Pi’s IP.


_5. Extensions for Students_

i.  Add Persistence: Save tasks to a file or SQLite database.

ii. GPIO Integration: Use onoff npm package to trigger an LED when a task is added (for hardware interaction).

GPIO (General Purpose Input/Output) is a programmable interface on Raspberry Pi (and other hardware) that allows you to connect and control external electronic components (e.g., LEDs, sensors) by reading/writing digital signals (high/low voltage).

(Example: Use GPIO to turn an LED on/off when a ToDo task is completed!).

- Deploy: Use nginx as a reverse proxy to expose the app to the internet.

**Extended work on Hardware: Extend the above work to integrate GPIO injection to turn the LED on and Off.**

_Practical Steps to Integrate GPIO with the ToDo App (Raspberry Pi)_

i.  Install GPIO Library:

```
npm install onoff  # Install Node.js GPIO library

```

ii. Initialize GPIO in server.js

```
const { Gpio } = require('onoff');
const led = new Gpio(17, 'out'); // LED on GPIO pin 17

```

iii.  Modify the PATCH Endpoint to Toggle LED (Patching) by updating **the /tasks/:id** route in server.js:

```
app.patch('/tasks/:id', (req, res) => {
  const task = tasks.find(t => t.id === parseInt(req.params.id));
  if (task) {
    task.completed = !task.completed;
    led.writeSync(task.completed ? 1 : 0); // Turn LED on/off
    res.json(task);
  } else {
    res.status(404).send('Task not found');
  }
});
```

iv. Clean Up GPIO on Exit

Add this to server.js to prevent hardware issues:

```
process.on('SIGINT', () => {
  led.unexport(); // Clean up GPIO
  process.exit();
});
```


**Hardware Setup**

- Connect LED to GPIO Pin 17:

- Anode (Long leg) → GPIO 17

- Cathode (Short leg) → Ground (GND) via a 220Ω resistor.

- Power Up:

```

node server.js  # Start the server

```

**Testing**

i. Toggle a task in the UI → LED turns on (completed) or off (incomplete).

ii. Observe the LED reacting to task state changes.

**Key Code Injection Points**

i. GPIO Setup (Top of server.js).

ii. LED Control (Inside PATCH endpoint).

iii. Cleanup (SIGINT handler).

(This bridges software (Node.js) and hardware (LED) for tangible feedback!)

**B -  Using a Custom JSON Object (list)**
