# Run GPIO Tests

```
# Unit tests
npm test test/unit/gpio.test.js

npm run test:integration

# Hardware integration test (mock mode)
npm test test/integration/hardware.test.js

# Physical GPIO test (on Raspberry Pi)
node scripts/test-gpio.js

```

## Expected Output:

- Non-Pi environments: Automatically falls back to mock mode

- Raspberry Pi: Controls actual GPIO pins 17, 27, 22, 23

- Test LEDs should blink in sequence during hardware test

##  Key Safety Features:

- Automatic mock mode detection

- Graceful cleanup on exit

- Hardware-independent test suite

- Visual feedback for all critical states

- NB:

## Best Practice:

- Added proper environment detection (typeof jest !== 'undefined')

- Ensured Jest mocks only exist in test environment

- Maintained All Functionality:

- Preserved all GPIO operations (setLed, getLedState, cleanup)

- Kept all logging functionality intact

- Maintained belt LED activation during initialization

## Integration Tests:

- Proper mock tracking with Jest in test environment

- Clear test assertions that verify hardware behavior

- Proper test isolation with _resetForTests()

## Cross-Environment Compatibility:

- Works in production (node index.js)

- Works in test environment (npm test)

- Maintains consistent behavior in both

