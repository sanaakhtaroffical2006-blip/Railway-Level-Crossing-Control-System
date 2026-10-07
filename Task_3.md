## Task 3 — Identify Constraint Violations

| ID | Constraint | Violation scenario (variable values) | What went wrong, and how do we know the constraint is violated? |
|----|------------|--------------------------------------|------------------------------------------------------------------|
| V1 | `Train_Present → ¬Barrier_Open` | `Train_Present = TRUE`, `Barrier_Open = TRUE` | A train is on the crossing but the barrier is up (e.g. a stuck open command). Substituting: `TRUE → ¬TRUE = TRUE → FALSE = FALSE`. Cars can drive onto the tracks. |
| V2 | `(Barrier_Closing ∨ Barrier_Closed) → Warning_Active` | `Barrier_Closing = TRUE`, `Warning_Active = FALSE` | The barrier is descending silently because the lights/alarm failed to switch on. The premise is TRUE and the conclusion FALSE, so the implication is FALSE. Drivers get no warning. |
| V3 | `Train_Present → Barrier_Closed` | `Train_Present = TRUE`, `Barrier_Closed = FALSE` (barrier stuck half-way) | The train is in the crossing but the barrier never reached the closed position. `TRUE → FALSE = FALSE`. A gap remains for vehicles to enter. |
| V4 | `Barrier_Opening → (Train_Cleared ∧ ¬Train_Present)` | `Barrier_Opening = TRUE`, `Train_Cleared = FALSE`, `Train_Present = TRUE` | A timer opened the barrier before the tail-end sensor confirmed clearance. The conclusion `FALSE ∧ FALSE = FALSE`, so the implication fails. Traffic could hit the last carriages. |
| V5 | `(Barrier_Closing ∨ Barrier_Closed) → Road_Signal_Red` | `Barrier_Closed = TRUE`, `Road_Signal_Red = FALSE` (signal shows green) | The barrier is down but the road signal says "go". Premise TRUE, conclusion FALSE. Drivers receive contradictory instructions and may force the barrier. |
| V6 | `Sensor_Failure → (Fail_Safe ∧ Barrier_Closed ∧ Warning_Active)` | `Sensor_Failure = TRUE`, `Fail_Safe = FALSE`, `Barrier_Closed = FALSE`, `Warning_Active = FALSE` | The approach sensor is dead but the system keeps running normally with the barrier up. The conclusion is FALSE, so a train could arrive undetected. |
| V7 | `Barrier_Fault → (Alert_Sent ∧ Road_Signal_Red)` | `Barrier_Fault = TRUE`, `Alert_Sent = FALSE`, `Road_Signal_Red = FALSE` | The motor jams, but no alert reaches the control center and the signal stays green. Operators are unaware and traffic keeps entering. |
| V8 | `Comm_Loss → (Fail_Safe ∧ Alert_Sent)` | `Comm_Loss = TRUE`, `Fail_Safe = FALSE`, `Alert_Sent = FALSE` | The data link drops and the system silently continues in normal mode. Nobody can monitor or override it, and the required safe state is not entered. |
| V9 | `Sensor_Conflict → (Barrier_Closed ∧ Warning_Active)` | `Sensor_Conflict = TRUE`, `Barrier_Closed = FALSE`, `Warning_Active = FALSE` | One sensor says "occupied" and another says "clear"; the logic trusts "clear" and leaves the barrier up. The system should have assumed the worst case. |
| V10 | `(Emergency ∧ ¬Manual_Override) → (Barrier_Closed ∧ Road_Signal_Red)` | `Emergency = TRUE`, `Manual_Override = FALSE`, `Barrier_Closed = FALSE` | An emergency is active and no operator has overridden, yet the barrier reopens automatically. Premise TRUE, conclusion FALSE. |
| V11 | `Train_Permitted → Barrier_Closed` | `Train_Permitted = TRUE`, `Barrier_Closed = FALSE` | The train is given a green signal while the barrier is still up or faulty. The train proceeds into an unprotected crossing. |
| V12 | `¬(Barrier_Opening ∧ Barrier_Closing)` | `Barrier_Opening = TRUE`, `Barrier_Closing = TRUE` | Two contradictory commands are active, e.g. a software race between the "train cleared" and "train approaching" events. The conjunction is TRUE, so its negation is FALSE. |

### How violations are detected

1. Read the current state of every variable from sensors, actuators and the control unit.
2. Evaluate each formal expression against those values.
3. If any expression evaluates to `FALSE`, the corresponding constraint is violated.
4. Expected system response: enter fail-safe mode, alert the control center, and log the event.
