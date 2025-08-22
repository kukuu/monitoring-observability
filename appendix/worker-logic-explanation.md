#  Worker Logic Explanation

**1. Constructor Initialization**

hands: Array representing the worker’s hands (max 2 items, initially [null, null]).

assemblyTime: Tracks remaining steps to assemble C (starts at 0 = idle).

**2. pickOrPlace(slot) Method**

- Step 1: Check if Busy Assembling

If assemblyTime > 0:

Decrements assemblyTime (counts down).

Returns immediately (worker can’t act while assembling).

- Step 2: Assemble C if Possible

i. If hands hold both 'A' and 'B':

a. Sets assemblyTime = 4 (takes 4 steps to assemble).

b. Clears hands ([null, null]).

c. Returns (assembly starts; product C is implied but not tracked here).

- Step 3: Pick Up Components

i. If the slot has a component ('A'/'B') and the worker isn’t already holding it:

a. Finds an empty hand (first null in hands).

b. Picks up the component (updates hands).

c. Sets slot = null (removes component from the belt).

- Key Behaviors

- Idle Worker: Picks up components when free.

- Busy Worker: Counts down assemblyTime; ignores the belt.

- Assembly: Triggered automatically when holding A+B; takes 4 steps.

Example Sequential Workflow

i. Pickup: Worker picks A from belt → hands = ['A', null].

ii. Pickup: Worker picks B from belt → hands = ['A', 'B'].

Assemble: Starts 4-step timer → assemblyTime = 4.

i. Complete: After 4 steps, hands reset → [null, null].
