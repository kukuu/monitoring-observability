# Context: conveyor.js

Here is  table explaining the provided code from the `ConveyorSimulator` class.

## Block A: ConveyorSimulator Class Code Analysis

| Line of Code / Code Block | Purpose | Explanation |
| :--- | :--- | :--- |
| `const { Gpio } = require('onoff');` | **Hardware Library Import** | Imports the `onoff` library, which provides a Node.js interface to control GPIO pins on a Raspberry Pi. This is only functional on a Pi itself. |
| `const gpio = require('./gpio');` | **Hardware Abstraction Import** | Imports the custom `gpio` module. This module contains the logic to abstract hardware control, allowing the same code to run on a Pi (using `onoff`) or in simulation mode (using mocks). |
| `class ConveyorSimulator { ... }` | **Core Simulation Class** | Defines the main class that models the entire conveyor belt system, its state, and its behavior. This is the brain of the operation. |
| `constructor() { ... }` | **Object Initialization** | Initializes a new instance of the conveyor simulator, setting all its properties to a default starting state. |
| `this.slots = Array(10).fill(null);` | **Belt Representation** | Creates an array of 10 elements, all initially `null`. This array **models the physical conveyor belt**, where each element represents a "slot" that can hold a component (`A`, `B`), a product (`C`), or be empty (`null`). |
| `this.unusedA = 0; this.unusedB = 0;` | **Waste Tracking** | Initializes counters for components that fall off the end of the belt without being used. This is a key performance metric. |
| `this.productsC = 0;` | **Production Tracking** | Initializes a counter for successfully manufactured products. This is the primary output metric. |
| `this.stepCount = 0;` | **Operation Tracking** | A simple counter to track how many simulation steps have been executed. |
| `this.isStopped = false;` | **State Flag** | A boolean flag to track if the conveyor belt is in a normal running state (`false`) or has been stopped by an emergency signal (`true`). |
| `if (!gpio.mockMode) { this.initializeGPIO(); }` | **Hardware Setup Check** | Checks if the application is running in simulation mode (`gpio.mockMode = true`) or on real hardware. If on real hardware, it calls the method to set up GPIO pins. |
| `initializeGPIO() { ... }` | **Hardware Configuration** | A method to set up the physical hardware interaction. This is only called when *not* in mock mode. |
| `this.emergencyStop = new Gpio(24, 'in', 'both');` | **Emergency Stop Button** | Configures physical **GPIO pin 24 as an input** to listen for a signal from an emergency stop button. The `'both'` parameter means it will watch for both rising and falling voltage changes. |
| `this.emergencyStop.watch(...);` | **Interrupt Handler** | Sets up an interrupt handler on the emergency stop pin. This function runs immediately whenever the pin's state changes, allowing for instant reaction to a stop command. |
| `handleEmergencyStop()` | **Safety Function** | This function (its body isn't shown here but implied) would contain the logic to immediately halt all conveyor movement and set `this.isStopped = true` for safety. |
| `simulateStep(workers) { ... }` | **Main Simulation Loop** | This is the **core method** that advances the entire simulation by one unit of time (one "step" or "tick"). It models the entire cycle of the conveyor belt. |
| `if (this.isStopped) return ...;` | **Safety Check** | The first thing the step does is check the emergency stop flag. If the belt is stopped, it returns the current state without doing any work. |
| `this.stepCount++;` | **Step Counter** | Increments the counter that keeps track of how many steps have been simulated. |
| `const newItem = this.generateComponent();` | **Component Generation** | Creates a new component (`A`, `B`) or nothing (`null`) at the "start" of the belt, simulating a loading station. |
| `this.slots.pop(); this.slots.unshift(newItem);` | **Belt Movement Simulation** | This is how the belt's movement is modeled. `pop()` removes the last item (which falls off the end), and `unshift()` adds the new item to the beginning. This effectively moves every item one slot down the belt. |
| `trackUnusedComponents()` | **Waste Calculation** | Checks the last slot (the one that was just `pop()`ped off). If it contained a component, it increments the appropriate waste counter (`unusedA` or `unusedB`). |
| `workers.forEach(...) { this.processWorker(...) }` | **Worker Processing** | For each worker in the system, it calls `processWorker` to allow them to interact with the components on the belt. |
| `processWorker(worker, workerIndex) { ... }` | **Worker Interaction Logic** | This method contains the logic for how a worker interacts with the belt. It typically iterates over the slots the worker can reach, allowing them to pick up components or place products. |
| `worker.pickOrPlace(this.slots[i])` | **Worker Action** | Calls a method on the worker object to decide what to do with a specific slot on the belt. The worker's logic (e.g., its internal state) determines if it picks up a component or places a finished product. |
| `placeProduct(workerIndex) { ... }` | **Product Finalization** | Called when a worker signals it has finished a product. It finds an empty slot on the belt and places the product (`'C'`) there, increments the product counter, and updates the hardware indicator. |
| `updateGPIOFeedback(workers) { ... }` | **Hardware Status Update** | This method updates the physical LEDs based on the current simulation state. It turns worker LEDs on/off based on if they are active and creates a blinking effect for the belt activity LED. |
| `gpio.setLed('beltActive', false); ... setTimeout(...)` | **Belt Activity Indicator** | Creates a visual "heartbeat" or "blink" on the belt activity LED every 10 steps. This provides physical feedback that the system is running, even if no other changes are happening. |


## Block B: ConveyorSimulator Code Analysis

This table includes `ConveyorSimulator` Class Code Analysis.


| Line of Code / Code Block | Purpose | Explanation |
| :--- | :--- | :--- |
| `handleEmergencyStop() { ... }` | **Safety Protocol Execution** | The function that is called by the GPIO interrupt when the emergency stop button is pressed. It contains the immediate actions required for a safe shutdown. |
| `this.isStopped = true;` | **State Freeze** | Sets the internal state flag to `true`, which is checked in `simulateStep()` to halt all further simulation logic until the system is reset. |
| `gpio.setLed('beltActive', false);` | **Visual Hazard Indication** | Turns off the "belt active" LED to provide immediate visual confirmation that the belt has stopped. |
| `[1, 2, 3].forEach(i => gpio.setLed(`worker${i}`, false));` | **Clear Activity Indicators** | Turns off all worker LEDs to indicate that all assembly activity has ceased. |
| `console.error('EMERGENCY STOP ACTIVATED');` | **Logging & Alerting** | Logs a high-priority error message to the console. In a production system, this could be connected to an alerting or notification service. |
| `reset() { ... }` | **System Reinitialization** | A public method to return the entire simulation to its original, default state. This is called by the `/api/reset` endpoint. |
| `this.slots = Array(10).fill(null); ...` | **State Reset** | Re-initializes all key properties (`slots`, `unusedA`, `unusedB`, `productsC`, `stepCount`, `isStopped`) to their starting values. |
| `gpio.setLed('beltActive', true);` | **Hardware State Reset** | Turns the "belt active" LED back on, providing a visual cue that the system is ready to start again after a reset. |
| `getState() { ... }` | **Data Snapshot Provider** | A public method that returns a complete snapshot of the current simulation state. This is the primary method used by API endpoints to report status. |
| `return { slots: [...this.slots], ... };` | **State Object Creation** | Creates a new object containing a *copy* of the current state (using the spread operator `...` for the `slots` array to avoid reference issues). This object is what gets sent as JSON via the API. |
| `efficiency: this.calculateEfficiency(),` | **KPI Calculation** | Includes the current efficiency rating in the state snapshot. This is a crucial business metric for the dashboard. |
| `timestamp: new Date().toISOString()` | **State Timestamp** | Adds a standardized timestamp to the state object, which is essential for tracking when events occurred in the simulation history. |
| `calculateEfficiency() { ... }` | **Performance Metric Calculation** | A helper method that calculates the production efficiency as a percentage. Formula: `(Successful Products / Total Components Processed) * 100`. |
| `const total = this.unusedA + this.unusedB + this.productsC;` | **Total Components Processed** | The denominator for the efficiency calculation. It sums successfully used components (which became products) and wasted components. |
| `shutdown() { ... }` | **Resource Cleanup** | A method designed to be called when the application is closing. Its purpose is to properly release hardware resources to avoid leaving the Raspberry Pi in an unstable state. |
| `this.emergencyStop.unexport();` | **GPIO Pin Cleanup** | If the emergency stop GPIO pin was initialized, this method releases it. This is a critical step to free the pin for future use and avoid errors on the next startup. |
| `gpio.cleanup();` | **Abstracted Hardware Cleanup** | Calls a cleanup function on the abstracted GPIO module. This function would handle turning off all LEDs and releasing all pins, whether they are real or mocked. |
| `module.exports = { ConveyorSimulator };` | **Class Export** | Makes the `ConveyorSimulator` class available for import by other files (like `index.js`), enabling its use in the larger application. |
