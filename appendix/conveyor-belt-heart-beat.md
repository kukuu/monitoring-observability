# Conveyor Belt "Heartbeat"

```
updateGPIOFeedback(workers) {
    workers.forEach((worker, i) => {
      gpio.setLed(`worker${i + 1}`, worker.isAssembling());
    });

    if (this.stepCount % 10 === 0) {
      gpio.setLed('beltActive', false);
      setTimeout(() => gpio.setLed('beltActive', true), 100);
    }
  }

```



This function synchronizes the physical (or simulated) LED indicators with the current state of the simulation, providing real-time visual feedback. It achieves two things:

i. Worker Status Lights: It continuously updates LEDs to show which workers are actively assembling a product.

ii. Belt "Heartbeat": It makes the main belt LED blink briefly every 10 steps. This provides a crucial visual indication that the simulation is actually running and processing steps, even if the workers are idle, preventing the illusion that the system has frozen.


## Context

i. workers.forEach((worker, i) => { ... });: This loop iterates over each worker in the system.

ii. gpio.setLed(worker${i + 1}, worker.isAssembling());:

For each worker, it sets their dedicated LED to be on (true) if the worker's isAssembling() method returns true (meaning they are busy building a product), or off (false) if they are idle. This gives an immediate visual status of each worker's activity.

iii. if (this.stepCount % 10 === 0) { ... }: This condition checks if the current step count is a multiple of 10 (e.g., step 10, 20, 30...). It creates a periodic event.

iv. gpio.setLed('beltActive', false);: When the condition is met, it immediately turns the "Belt Active" LED off.

v. setTimeout(() => gpio.setLed('beltActive', true), 100);: It then sets a timer to turn the same LED back on after 100 milliseconds.



