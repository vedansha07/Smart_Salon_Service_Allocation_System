# Project Idea — Smart Salon Queue & Service Allocation System

## 1. Background Problem

Traditional salon token systems follow a First-Come-First-Serve (FIFO) model.
In real environments this fails because services vary in duration and require different skill levels.

Examples:

* A 60-minute hair coloring blocks several 10-minute beard trims
* Senior stylists stay idle while junior barbers overload
* VIP customers manually interrupt the queue
* Missed customers require manual adjustment
* Staff breaks disturb order
* Waiting time becomes unpredictable

Therefore, a simple ordered queue cannot represent real salon operations.

---

## 2. Objective

Design a real-time backend scheduling system that dynamically assigns customers to compatible barbers using deterministic priority rules while maintaining fairness and minimizing idle time.

The system must behave like a resource allocation engine instead of a static waiting list.

---

## 3. Scope of the System

The system will:

* Accept walk-in customers requesting specific services
* Calculate a dynamic priority score
* Assign customers to available compatible barbers
* Continuously recalculate ordering after events
* Enforce token lifecycle rules
* Predict waiting time
* Support multiple barbers operating simultaneously

Focus is on backend logic and correctness of behavior rather than UI complexity.

---

## 4. Actors

### Customer

* Request a service token
* Mark VIP status
* Leave queue
* Rejoin if missed

### Barber (Operator)

* Call next customer
* Start service
* Finish service
* Pause or resume availability
* Transfer customer

### Admin

* Define services and durations
* Assign barber skills
* Configure priority rules
* View analytics

### System Scheduler (Internal Actor)

Automated decision engine responsible for queue ordering and assignment.

---

## 5. Constraints

The system must ensure:

1. A barber can only perform compatible services
2. A token exists in only one state at a time
3. Customers are not starved indefinitely
4. VIP priority does not permanently block normal customers
5. Queue automatically updates after interruptions
6. Multiple barbers operate concurrently without conflict

---

## 6. Priority Model

Each token receives a priority score:

priorityScore = servicePriority + VIPBonus + waitingTimeFactor

Where:

* Service priority depends on duration
* VIPBonus is fixed
* WaitingTimeFactor increases periodically to guarantee fairness

The scheduler always selects the highest compatible priority.

---

## 7. Token Lifecycle

States:

CREATED → WAITING → CALLED → SERVING → COMPLETED

Exception transitions:

* CALLED → MISSED (timeout)
* WAITING → CANCELLED
* MISSED → WAITING (rejoin)
* SERVING → REASSIGNED (barber unavailable)

Invalid transitions are not allowed.

---

## 8. Core Challenges Addressed

* Dynamic scheduling instead of static ordering
* Skill-based allocation
* Fairness between long and short services
* Handling missed customers
* Parallel barber coordination
* Maintaining queue consistency after disruptions

---

## 9. Expected Outcome

A functioning backend scheduling engine capable of dynamically controlling service order in a real salon environment while maintaining correct state transitions and allocation decisions.
