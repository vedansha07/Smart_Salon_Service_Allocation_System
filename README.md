# Smart Salon Queue & Service Allocation System

A real-time backend scheduling engine that dynamically assigns walk-in customers to compatible barbers based on service type, priority rules, and live availability.

Unlike traditional FIFO token systems, the queue continuously recalculates order after every event (service finish, VIP entry, barber break, etc.).

---

## Core Idea

Customers don’t get a fixed position in line.

The system continuously decides:

**Which customer should be served next by which barber at this moment**

This turns the queue into a scheduling engine instead of a waiting list.

---

## Actors

* Customer
* Barber (Operator)
* Admin
* System Scheduler (automated decision engine)

---

## System Behaviour Overview

### Use Case

```mermaid
flowchart LR

Customer --> RequestToken[Request Service Token]
Customer --> MarkVIP[Mark VIP]
Customer --> LeaveQueue[Leave Queue]
Customer --> Rejoin[Rejoin After Missed]

Barber --> CallNext[Call Next Customer]
Barber --> StartService[Start Service]
Barber --> FinishService[Finish Service]
Barber --> Pause[Pause Availability]
Barber --> Transfer[Transfer Customer]

Admin --> ManageServices[Add/Update Services]
Admin --> AssignSkills[Assign Barber Skills]
Admin --> ConfigureRules[Configure Priority Rules]

RequestToken --> Scheduler
FinishService --> Scheduler
Pause --> Scheduler
Transfer --> Scheduler
Rejoin --> Scheduler

Scheduler --> Recalculate[Recalculate Queue Order]
Scheduler --> AssignBarber[Assign Compatible Barber]
```

---

### Main Flow

```mermaid
sequenceDiagram
actor Customer
participant Controller
participant TokenService
participant PriorityPolicy
participant QueueManager
participant Scheduler
participant Barber
participant DB

Customer->>Controller: Request service
Controller->>TokenService: Create token
TokenService->>PriorityPolicy: Calculate priority
PriorityPolicy-->>TokenService: Score

TokenService->>DB: Save token (WAITING)
TokenService->>QueueManager: Insert token
TokenService->>Scheduler: Trigger allocation

Scheduler->>DB: Find available barber
Scheduler->>DB: Update token -> CALLED
Scheduler-->>Barber: Notify

Barber->>Controller: Start service
Controller->>DB: SERVING

Barber->>Controller: Finish service
Controller->>DB: COMPLETED
Controller->>Scheduler: Next allocation
```

---

### Backend Architecture

```mermaid
classDiagram

class Token
class Service
class Barber
class QueueManager
class Scheduler
class PriorityPolicy
class CompatibilityChecker
class TokenStateMachine
class NotificationService

Scheduler --> QueueManager
Scheduler --> PriorityPolicy
Scheduler --> CompatibilityChecker
Scheduler --> TokenStateMachine
QueueManager --> Token
Token --> Service
Barber --> Service
```

---

### Database Structure

```mermaid
erDiagram

CUSTOMERS ||--o{ TOKENS : creates
SERVICES ||--o{ TOKENS : requested_in
TOKENS ||--o{ QUEUE_EVENTS : logs
BARBERS ||--o{ BARBER_SESSIONS : handles
TOKENS ||--|| BARBER_SESSIONS : served_by
```

---

## Scheduling Logic

Priority Score:

```
priorityScore = servicePriority + VIPBonus + waitingTimeFactor
```

Ensures:

* Fairness
* No starvation
* Efficient barber utilization

---

## Token Lifecycle

```
CREATED → WAITING → CALLED → SERVING → COMPLETED
           ↓
        MISSED / CANCELLED / REASSIGNED
```

---

## Project Focus

This project emphasizes backend system design:

* State machine modelling
* Scheduling logic
* Resource allocation
* Event driven updates
* Data consistency

UI is only a control panel for triggering backend behaviour.

---

## Planned Tech Stack

Backend: Node.js / Express
Database: MySQL
Realtime: WebSockets
Frontend: Minimal dashboard
