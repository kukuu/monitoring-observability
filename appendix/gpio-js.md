# Context: gpio.js

GPIOController Class Code Analysis


## GPIOController class: Asophisticated hardware abstraction layer (HAL).

This `GPIOController` is a production-grade **Hardware Abstraction Layer (HAL)**. Its primary achievement is making the entire application **hardware-agnostic**. The core logic (`ConveyorSimulator`, `WorkerSystem`) calls `gpio.setLed(...)` without needing to know or care if it's controlling a real light on a Pi or just a log message in a console. This design enables:

*   **Development on any machine** (Windows, Mac, Linux).

*   **Robust unit testing** without requiring physical hardware.

*   **Seamless deployment** to a Raspberry Pi without code changes.

*   **Graceful degradation** if hardware fails or is unavailable.



| Line of Code / Code Block | Purpose | Explanation |
| :--- | :--- | :--- |
| `class GPIOController { ... }` | **Hardware Abstraction Layer** | Defines a class that encapsulates all hardware interaction, providing a unified interface that works identically on a real Raspberry Pi and in simulation/testing environments. |
| `this.mockMode = process.env.NODE_ENV === 'test' || process.platform !== 'linux' || !Gpio.accessible;` | **Environment Detection** | The core logic that automatically determines the operational mode. It sets `mockMode = true` if: 1. The app is running in a test environment, 2. The OS is not Linux (so not a Pi), or 3. The `onoff` library reports hardware is not accessible. This enables seamless development on a PC. |
| `this.leds = {};` | **Hardware State Storage** | Initializes an empty object that will store the control interfaces (real or mock) for all the LEDs used in the system. |
| `this._initGPIO();` | **Automatic Initialization** | Called from the constructor to automatically set up the GPIO based on the detected `mockMode`. The application doesn't need to call this manually. |
| `_initGPIO()` | **Initialization Router** | A private method that acts as a router. It checks `this.mockMode` and calls the appropriate setup method (`_setupMockGPIO` or `_setupRealGPIO`). |
| `_setupMockGPIO() { ... }` | **Simulation Environment Setup** | Configures the system for software-only operation. It creates mock objects that simulate real GPIO pins but just log their actions to the console. |
| `createMockLed(name)` | **Mock Object Factory** | A helper function that generates a consistent mock LED object. Each mock has `writeSync`, `readSync`, and `unexport` methods that mimic the real `Gpio` object's API. |
| `if (process.env.NODE_ENV === 'test') { ... }` | **Test Framework Integration** | This block adds special Jest mocking capabilities *only* when running in a test environment. It allows unit tests to spy on and make assertions about which LEDs were turned on/off. |
| `_setupRealGPIO() { ... }` | **Physical Hardware Setup** | Configures the system to control real Raspberry Pi GPIO pins. It initializes the `onoff.Gpio` objects for each LED pin, defining them as outputs. |
| `Object.values(this.leds).forEach(led => led.writeSync(0));` | **Hardware Safety Initialization** | A critical safety step. It ensures all physical LEDs are turned **off** (0) immediately upon startup to avoid leaving them in an unknown state. |
| `this.setLed('beltActive', true);` | **Default State Setup** | Turns on the "belt active" LED by default after initialization, providing a visual indicator that the system is powered on and ready. |
| `try { ... } catch (err) { this.mockMode = true; ... }` | **Hardware Fallback Mechanism** | Wraps the real hardware setup in a try-catch block. If anything fails (e.g., no Pi detected despite being on Linux), it gracefully falls back to mock mode, ensuring the application doesn't crash. |
| `setLed(pin, state) { ... }` | **Unified Control Interface** | The primary public method. It provides a consistent way to turn an LED on or off, abstracting away whether it's controlling real hardware or a mock. It validates the pin name and converts the boolean `state` to a hardware value (`1` or `0`). |
| `getLedState(pin) { ... }` | **State Query Interface** | Provides a way to read the current state (on/off) of an LED. This is useful for diagnostics and for the Prometheus metrics endpoint to report hardware status. |
| `cleanup() { ... }` | **Resource Management & Safety** | A crucial method for graceful shutdown. It ensures all physical LEDs are turned off and their GPIO pins are properly released (`unexport()`ed) to prevent leaving the hardware in an unstable state. |
| `const gpioInstance = new GPIOController();` | **Singleton Pattern** | Creates a single, shared instance of the `GPIOController` for the entire application. This ensures all parts of the app use the same hardware state and configuration. |
| `if (process.env.NODE_ENV === 'test') { ... }` | **Test-Specific Augmentation** | This block adds special helper methods *only* to the singleton instance when running in a test environment. These methods are not present in production, keeping the core class clean. |
| `_resetForTests()` | **Test Isolation Helper** | A method added for tests to reset the GPIO state to a known condition between test runs, ensuring tests don't interfere with each other. |
| `_getMockCalls(pin)` | **Test Assertion Helper** | A method added for tests to inspect the "call history" of a mock LED. This allows tests to make assertions like "verify the beltActive LED was turned on 3 times." |
| `module.exports = gpioInstance;` | **Singleton Export** | Exports the single, already-initialized instance of the GPIO controller. Other files import this instance, not the class itself, ensuring the singleton pattern is enforced. |

