# 🧺 Washing Machine Controller (FSM-Based Design)

## 📘 Overview
This project implements a **Finite State Machine (FSM)** for an **Automatic Washing Machine Controller** using **Verilog HDL**.  
It models the sequential behavior of a washing machine — handling operations like door checking, water filling, detergent addition, washing cycle, draining, and spinning.

---

## ⚙️ Features
- ✅ FSM-based design for structured operation control  
- ✅ Fully synchronous with clock and reset  
- ✅ Safety door lock mechanism  
- ✅ Step-by-step washing sequence  
- ✅ Simulation testbench included (`new_test`)  
- ✅ Real-time signal monitoring via `$monitor`  

---

## 🧩 Module Description

### **Top-Level Module:** `washing_machine_controller`

| **Port** | **Direction** | **Description** |
|-----------|---------------|-----------------|
| `clk` | Input | System clock |
| `reset` | Input | Active-low asynchronous reset |
| `door_close` | Input | Door closed sensor |
| `start` | Input | Start button |
| `filled` | Input | Water filled sensor |
| `detergent_added` | Input | Detergent added flag |
| `cycle_timeout` | Input | Washing cycle completion signal |
| `drained` | Input | Drain complete signal |
| `spin_timeout` | Input | Spin complete signal |
| `door_lock` | Output | Door lock signal |
| `motor_on` | Output | Motor control |
| `fill_value_on` | Output | Water fill valve control |
| `drain_value_on` | Output | Drain valve control |
| `done` | Output | Washing complete flag |
| `soap_wash` | Output | Soap wash phase active |
| `water_wash` | Output | Water rinse phase active |

---

## 🧠 FSM Design

### **State Encoding**

| **State Name** | **Binary Code** | **Description** |
|----------------|----------------|----------------|
| `check_door` | `000` | Verify door closed and start pressed |
| `fill_water` | `001` | Fill the drum with water |
| `add_detergent` | `010` | Add detergent and prepare for wash |
| `cycle` | `011` | Perform washing cycle |
| `drain_water` | `100` | Drain used water |
| `spin` | `101` | Spin-dry clothes |
| — | — | Cycle ends and returns to idle |

---

### **State Diagram (Mermaid Syntax)**

```mermaid
stateDiagram-v2
    [*] --> CHECK_DOOR
    CHECK_DOOR --> FILL_WATER: start && door_close
    FILL_WATER --> ADD_DETERGENT: filled && !soap_wash
    ADD_DETERGENT --> CYCLE: detergent_added
    CYCLE --> DRAIN_WATER: cycle_timeout
    DRAIN_WATER --> FILL_WATER: drained && !water_wash
    DRAIN_WATER --> SPIN: drained && water_wash
    SPIN --> CHECK_DOOR: spin_timeout
    CHECK_DOOR --> [*]: done
