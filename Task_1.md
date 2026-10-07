# Lab Task 6 – Railway Level-Crossing Control System (ARLCCS)

**Role:** Software Verification Team
**Focus:** System requirements → formal constraints → violations → expected behavior

---

## Task 1 — Identify Constraints

| ID | Constraint (simple English) | Why the constraint is necessary |
|----|-----------------------------|---------------------------------|
| C1 | The barrier must not be open while a train is present in the crossing. | Road vehicles could enter the crossing while the train is passing, causing a collision. |
| C2 | Warning lights and the audible alarm must be active whenever the barrier is closing or closed. | Drivers and pedestrians must be warned before and while the road is blocked. |
| C3 | The barrier must be fully closed whenever a train is present in the crossing. | A partly closed barrier still leaves a gap for vehicles to enter. |
| C4 | The barrier may start opening only after the system has confirmed the train has completely cleared the crossing. | Opening early could let traffic hit the rear carriages of the train. |
| C5 | The road traffic signal must be red whenever the barrier is closing or closed. | A green signal in front of a closed barrier gives drivers contradictory instructions. |
| C6 | If a train-detection sensor fails, the system must enter fail-safe mode (barrier closed, warnings on). | Without sensor data the system cannot know if a train is coming, so it must assume the worst. |
| C7 | If a barrier fault is detected, the system must alert the control center and keep the road signal red. | Operators must be told immediately so they can send help or stop the train. |
| C8 | If communication with the control center is lost, the system must enter fail-safe mode and raise a local alert. | The system can no longer be monitored or overridden remotely, so it must default to the safest state. |
| C9 | If sensor readings conflict (e.g. "track clear" and "track occupied" at the same time), the system must treat the crossing as occupied (barrier closed, warnings on). | An incorrect "clear" reading is far more dangerous than a false "occupied" reading. |
| C10 | During an emergency, the barrier must stay closed and the road signal red until an authorised operator overrides. | Prevents automatic reopening while a hazard (derailment, stalled vehicle, etc.) exists. |
| C11 | A train may be given permission to pass (green signal) only if the barrier is confirmed closed. | The train must never be told to proceed into an unprotected crossing. |
| C12 | The system must never issue open and close commands to the barrier at the same time. | Conflicting commands can damage the barrier mechanism and leave it in an unknown state. |

---
