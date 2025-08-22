# Browser Console Response

```
Promise {<pending>}[[Prototype]]: Promise[[PromiseState]]: "fulfilled"[[PromiseResult]]: undefined
VM2553:17 === Simulation Results ===
VM2553:18 Products Manufactured: 61
VM2553:19 Unused Component A: 16
VM2553:20 Unused Component B: 0
VM2553:21 Efficiency: 79.22%
VM2553:23 
=== Last 5 Steps ===
VM2553:25 Step 96: Slots: [B, , C, , , , C, , , C] State: RUNNING
VM2553:25 Step 97: Slots: [B, , , C, , , , C, , ] State: RUNNING
VM2553:25 Step 98: Slots: [C, , , , C, , , , C, ] State: RUNNING
VM2553:25 Step 99: Slots: [A, C, , , , C, , , , C] State: RUNNING
VM2553:25 Step 100: Slots: [C, , C, , , , C, , , ] State: RUNNING
VM2553:31 
=== Full Response ===
VM2553:32 {
  "success": true,
  "stepsCompleted": 100,
  "productsC": 61,
  "unusedA": 16,
  "unusedB": 0,
  "efficiency": "79.22",
  "lastSteps": [
    {
      "slots": [
        "B",
        null,
        "C",
        null,
        null,
        null,
        "C",
        null,
        null,
        "C"
      ],
      "unusedA": 16,
      "unusedB": 0,
      "productsC": 59,
      "isStopped": false,
      "efficiency": "78.67",
      "timestamp": "2025-08-18T11:27:36.965Z"
    },
    {
      "slots": [
        "B",
        null,
        null,
        "C",
        null,
        null,
        null,
        "C",
        null,
        null
      ],
      "unusedA": 16,
      "unusedB": 0,
      "productsC": 59,
      "isStopped": false,
      "efficiency": "78.67",
      "timestamp": "2025-08-18T11:27:36.965Z"
    },
    {
      "slots": [
        "C",
        null,
        null,
        null,
        "C",
        null,
        null,
        null,
        "C",
        null
      ],
      "unusedA": 16,
      "unusedB": 0,
      "productsC": 60,
      "isStopped": false,
      "efficiency": "78.95",
      "timestamp": "2025-08-18T11:27:36.965Z"
    },
    {
      "slots": [
        "A",
        "C",
        null,
        null,
        null,
        "C",
        null,
        null,
        null,
        "C"
      ],
      "unusedA": 16,
      "unusedB": 0,
      "productsC": 60,
      "isStopped": false,
      "efficiency": "78.95",
      "timestamp": "2025-08-18T11:27:36.965Z"
    },
    {
      "slots": [
        "C",
        null,
        "C",
        null,
        null,
        null,
        "C",
        null,
        null,
        null
      ],
      "unusedA": 16,
      "unusedB": 0,
      "productsC": 61,
      "isStopped": false,
      "efficiency": "79.22",
      "timestamp": "2025-08-18T11:27:36.966Z"
    }
  ]
}

//VM2553:1 Fetch finished loading: POST "http://localhost:5000/api/simulate". (anonymous) @ VM2553:1

```
