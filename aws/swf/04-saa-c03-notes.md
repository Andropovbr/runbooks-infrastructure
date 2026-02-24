# SWF Notes for SAA-C03 (What to memorize)

## What SWF *is*
- Managed **workflow coordination** service (state + history + task dispatch).
- You provide:
  - **Decider** (orchestration logic)
  - **Workers** (activities)

## Keywords that point to SWF in exam questions
- “decider”
- “workers”
- “task list”
- “workflow history”
- “long-running workflow with custom coordination”

## SWF vs Step Functions (high-level)
- Step Functions: managed state machine + easy integrations; recommended first for new apps. citeturn0search11
- SWF: more control; you write decider logic; more operational overhead.

## Typical gotchas
- Task list mismatch
- Timeouts/heartbeats
- Private subnet without egress (no VPC endpoint for SWF in most setups)
- Deprecated domain/type prevents new work

## “One-liner” mental model
SWF is an event-sourced workflow engine: **history drives decisions**; decisions schedule activities; workers execute and report back.
