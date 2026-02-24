# AWS SWF — Mental Model (SAA-C03 Briefing)

Amazon **Simple Workflow Service (SWF)** is a *workflow coordination* service: it keeps **state**, **history**, and **task dispatching**, while **your code** (workers + deciders) executes the work and makes orchestration decisions.

> For new builds, AWS generally recommends considering **Step Functions** first; use **SWF** when you need deeper, code-driven orchestration or you’re dealing with legacy workflows. citeturn0search11

---

## 1) Core Concepts (map to “who does what”)

### Domain
A **namespace boundary** for everything: workflow types, activity types, executions, visibility queries.
- Think: “project/tenant boundary”.

### Workflow Execution
A **running instance** of a workflow type.
- Has an **Execution History** (event log).
- Can be **open** or **closed**.

### Workflow Type
The **template** of a workflow (name + version + defaults like timeouts).

### Activity Type
The **template** of an activity (name + version + defaults like timeouts).

### Decider
Your code that:
- Reads workflow **history**
- Decides next actions (schedule activity, start child workflow, complete workflow, fail workflow, set timers, etc.)
- Responds to **decision tasks**

### Worker
Your code that:
- Polls for **activity tasks**
- Executes the activity
- Reports completion/failure/heartbeats

### Task Lists
Queues that deciders and workers poll.
- If **task list names don’t match**, tasks “disappear” (they’re just on a different list).

### Visibility
Search-like APIs to list/count open/closed workflows; useful for operations.

---

## 2) The Control Loop (how SWF “runs” your system)

1. **Start workflow execution**
2. SWF creates a **DecisionTask** on a decision task list
3. **Decider polls** → receives a slice of history → decides what to do next
4. Decider responds with decisions (e.g., “schedule activity X”)
5. SWF schedules an **ActivityTask** on an activity task list
6. **Worker polls** → gets task → executes → reports completion/failure (+ heartbeat)
7. SWF logs events → creates another DecisionTask
8. Loop continues until decider **closes** the workflow

### Long-poll behavior
Workers/deciders typically use long polling; for example, `poll-for-activity-task` holds the request open up to **60 seconds** before returning empty. citeturn0search1

---

## 3) Timeouts (what breaks workflows in practice)

SWF has multiple timeout layers. Operationally, think of them like **guardrails**:

- **WorkflowExecution timeout**: max total runtime
- **DecisionTask timeout**: decider must respond in time
- **ActivityTask timeouts**:
  - **ScheduleToStart**: task must be picked up
  - **StartToClose**: task must finish after start
  - **Heartbeat**: worker must heartbeat periodically (for long work)

Most “mysterious failures” are actually timeouts + missing heartbeats.

---

## 4) When SWF shows up in SAA-C03 questions

SWF is usually tested conceptually:
- When to use **SWF vs Step Functions**
- Understanding **deciders/workers/task lists**
- Long-running/human-in-the-loop workflows (classic SWF positioning)

---

## 5) Quick “exam style” comparison

| Topic | SWF | Step Functions |
|---|---|---|
| Orchestration logic | **Code** (decider) | **State machine** (JSON) |
| Activities execution | Your workers | Integrations / Lambda / ECS / etc |
| Operational overhead | Higher (run workers/deciders) | Lower |
| Visualization | API/console visibility | Built-in workflow visualization |
| Fit | Legacy or custom coordination | Default choice for new workflows |

---

## 6) Practical rule of thumb

- If you want **managed orchestration with direct AWS integrations** → Step Functions.
- If you need **custom decision logic**, **legacy SWF**, or very specific worker/decider control → SWF.
