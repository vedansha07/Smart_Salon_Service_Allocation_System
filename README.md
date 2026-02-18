# Smart Salon Queue & Service Allocation System

A real-time backend scheduling engine that dynamically assigns walk-in customers to compatible barbers based on service type, priority rules, and live availability.

Unlike traditional token systems (FIFO), this system continuously recalculates service order to minimize waiting time and prevent service starvation.

---

## Key Concept

Customers do not receive fixed positions in line.

The system continuously decides:

**Which customer should be served next by which barber at this moment**

The queue behaves like a scheduling engine rather than a waiting list.

---

## System Features

### Dynamic Allocation

* Skill-based barber assignment
* Automatic reassignment when barber unavailable
* Parallel multi-barber handling

### Fair Priority Scheduling

Priority Score = Service Weight + VIP Bonus + Waiting Time Factor

Prevents long services or normal customers from starving.

### Real-Time Queue Updates

* Reorders after every event
* Handles missed customers
* Supports emergency/VIP insertion

### Token Lifecycle Control

CREATED → WAITING → CALLED → SERVING → COMPLETED

With exception flows:
MISSED, CANCELLED, REASSIGNED

---

## Actors

* Customer
* Barber (Operator)
* Admin
* System Scheduler (automated decision engine)

---

## System Diagrams

### Use Case Diagram

![Use Case](./useCaseDiagram.md)

### Sequence Diagram

![Sequence](./sequenceDiagram.md)

### Class Diagram

![Class](./classDiagram.md)

### ER Diagram

![ER](./ErDiagram.md)

---

## Backend Responsibilities

* Scheduling engine
* Priority calculation
* Compatibility validation
* State transition enforcement
* Waiting time prediction
* Event driven reallocation

---

## Project Focus

This project emphasizes backend system design, OOP architecture, and process correctness over UI complexity.

---

## Tech Stack (Planned)

Backend: Node.js / Express
Database: PostgreSQL
Realtime: WebSockets
Frontend: Minimal control dashboard

---

## Expected Outcome

A working operational queue system capable of dynamically controlling service order in a real salon environment while maintaining correctness of state transitions and allocation decisions.
