# Context: workers.js

## WorkerSystem Class Code Analysis

Here is a table explaining the provided code from the `WorkerSystem` class.

| Line of Code / Code Block | Purpose | Explanation |
| :--- | :--- | :--- |
| `class WorkerSystem { ... }` | **Worker Agent Definition** | Defines a class that models an individual worker station in the manufacturing process. Each instance represents a single worker with their own state and capabilities. |
| `constructor(id) { ... }` | **Worker Initialization** | Initializes a new worker agent with a unique identifier and a default empty state. |
| `this.id = id;` | **Unique Identifier** | Stores the worker's ID (e.g., 1, 2, 3). This is used for logging, GPIO pin mapping, and distinguishing between workers in the system. |
| `this.hands = [null, null];` | **Component Inventory** | An array that represents the worker's two "hands." Each hand can hold a component (`'A'`, `'B'`) or be empty (`null`). This is the core of the worker's internal state. |
| `this.assemblyTime = 0;` | **Assembly Progress Timer** | A counter that tracks how many simulation steps are left to complete the assembly of a product. `0` means not assembling, `>0` means currently working. |
| `this.productsCompleted = 0;` | **Production Output Counter** | Tracks the total number of products this specific worker has successfully manufactured. A key performance metric. |
| `initializeGpio() { ... }` | **Hardware Setup for Worker** | A method to set up the physical LED for this specific worker. It is only called if the system is not in mock mode. |
| `this.led = new Gpio(17 + this.id, 'out');` | **Dedicated LED Pin Assignment** | Dynamically assigns a GPIO pin for the worker's LED based on its ID. For example, Worker 1 gets pin 18 (17+1), Worker 2 gets pin 19 (17+2), etc. The pin is configured as an output. |
| `pickOrPlace(slot) { ... }` | **Main Decision Logic** | This is the **core AI method** for the worker. It decides what action to take (pick, place, assemble, or do nothing) based on its own state and the contents of the belt slot it's evaluating. It returns the new value for the belt slot. |
| `if (this.isAssembling()) { ... }` | **Priority: Continue Assembly** | The highest priority. If the worker is currently assembling a product, it decrements the timer and returns the slot unchanged (the worker is busy and can't interact). |
| `if (this.hasBothComponents()) { ... }` | **Priority: Start Assembly** | The second priority. If the worker has both components (A and B) in hand, it starts the assembly process, consuming the components and starting a timer. |
| `if (this.canPickComponent(slot)) { ... }` | **Priority: Pick Up Component** | The third priority. If the slot contains a component the worker needs and can hold, it picks it up, removing it from the belt. |
| `if (this.hasCompletedProduct() && slot === null) { ... }` | **Priority: Place Finished Product** | The final priority. If the worker has just finished a product *and* the belt slot is empty, it places the product (`'C'`) onto the belt. |
| `canPickComponent(component) { ... }` | **Pickup Validation Rules** | Encapsulates the rules for a valid pickup: 1. The item must be a valid component (`A` or `B`). 2. The worker must have an empty hand. 3. The worker must not already have that same component (preventing two `A`s or two `B`s). |
| `pickComponent(component) { ... }` | **Component Acquisition** | Executes the act of picking up a component: finds the first empty hand, puts the component in it, and returns `null` to show the belt slot is now empty. |
| `hasBothComponents()` | **Component Check** | Returns `true` if the worker has one `'A'` and one `'B'` in their hands, meaning they are ready to start assembling. |
| `startAssembly()` | **Begin Product Creation** | Initiates the assembly process: clears the worker's hands, sets the `assemblyTime` to a fixed duration (4 steps), and turns on their status LED. |
| `isAssembling()` | **Activity Status Check** | Returns `true` if the assembly timer is greater than zero, meaning the worker is currently busy and cannot interact with the belt. |
| `hasCompletedProduct()` | **Product Readiness Check** | Returns `true` only when `assemblyTime === 1`, meaning the product will be finished on the *next* call to `pickOrPlace`. This allows the system to check for an empty slot to place it. |
| `placeProduct()` | **Product Finalization** | Completes the assembly cycle: resets the timer, increments the product counter, turns off the status LED, and returns `'C'` to be placed on the belt. |
| `reset()` | **State Reinitialization** | Returns the worker to its initial state: empty hands, no active assembly, and LED turned off. |
| `getStatus()` | **Status Snapshot Provider** | Returns a detailed object representing the current state of the worker. This is used by the API to report on each worker's activity. |
| `setWorkerLed(state)` | **Hardware Status Indicator** | Abstracts the control of the worker's LED. It tries to use the direct GPIO object first but falls back to the abstracted `gpio.setLed` function if needed, ensuring it works in both real and mock modes. |
| `module.exports = { WorkerSystem };` | **Class Export** | Makes the `WorkerSystem` class available for import by other files, allowing the main application to create multiple worker instances. |
