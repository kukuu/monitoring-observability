# Conveyor Belt CLASS 

_Explanation_:

Here’s a step-by-step breakdown of what the ConveyorBelt class does:

**1. Constructor Initialization**
Creates a conveyor belt with length slots (default: 10), all initially empty (null).

- Initializes:

i. workers: An empty array (placeholder for 3 worker pairs, though currently unassigned).

ii. unusedA/unusedB: Counters for unused components (starting at 0).

iii. productsC: Counter for assembled products (initially 0).

**2. moveBelt() Method**

- Step 1: Add a New Item

- Generates a random item:

33% chance of 'A', 33% chance of 'B', 33% chance of null (empty).

**Step 2: Update the Belt**

- Removes the last item (using pop()).

- Adds the new item to the front (using unshift()), simulating belt movement.

**Step 3: Track Unused Components**

- Checks the last slot (before it was removed by pop()):

i. If it was 'A', increments unusedA.

ii. If it was 'B', increments unusedB.

**Key Purpose**

- Simulates a conveyor belt moving components (A/B) forward.

- Tracks unused components that "fell off" the belt’s end (for efficiency metrics).
