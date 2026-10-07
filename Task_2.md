## Task 2 — Formalize Constraints

### Propositional variables

| Variable | Meaning |
|----------|---------|
| `Train_Present` | A train is currently in the crossing |
| `Train_Cleared` | Sensors confirm the train has completely cleared the crossing |
| `Barrier_Open` | Barrier is fully open |
| `Barrier_Closed` | Barrier is fully closed |
| `Barrier_Closing` | Barrier is moving to the closed position |
| `Barrier_Opening` | Barrier is moving to the open position |
| `Warning_Active` | Warning lights and audible alarm are on |
| `Road_Signal_Red` | Road traffic signal shows red |
| `Sensor_Failure` | A train-detection sensor has failed |
| `Barrier_Fault` | A barrier fault has been detected |
| `Comm_Loss` | Communication with control center is lost |
| `Sensor_Conflict` | Sensors give contradictory readings |
| `Emergency` | Emergency condition is active |
| `Manual_Override` | An authorised operator has overridden the system |
| `Fail_Safe` | System is in fail-safe mode |
| `Alert_Sent` | Alert has been sent to the control center |
| `Train_Permitted` | Train has been given the green signal to pass |

### Formal expressions

| ID | Formal expression |
|----|-------------------|
| C1 | `Train_Present → ¬Barrier_Open` |
| C2 | `(Barrier_Closing ∨ Barrier_Closed) → Warning_Active` |
| C3 | `Train_Present → Barrier_Closed` |
| C4 | `Barrier_Opening → (Train_Cleared ∧ ¬Train_Present)` |
| C5 | `(Barrier_Closing ∨ Barrier_Closed) → Road_Signal_Red` |
| C6 | `Sensor_Failure → (Fail_Safe ∧ Barrier_Closed ∧ Warning_Active)` |
| C7 | `Barrier_Fault → (Alert_Sent ∧ Road_Signal_Red)` |
| C8 | `Comm_Loss → (Fail_Safe ∧ Alert_Sent)` |
| C9 | `Sensor_Conflict → (Barrier_Closed ∧ Warning_Active)` |
| C10 | `(Emergency ∧ ¬Manual_Override) → (Barrier_Closed ∧ Road_Signal_Red)` |
| C11 | `Train_Permitted → Barrier_Closed` |
| C12 | `¬(Barrier_Opening ∧ Barrier_Closing)` |

> **Reading an implication:** `A → B` is violated only when `A = TRUE` and `B = FALSE`.
> Equivalent form: `A → B ≡ ¬A ∨ B`.

---
