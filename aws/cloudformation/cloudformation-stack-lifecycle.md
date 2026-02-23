# CloudFormation Stack Lifecycle Runbook

*Last updated: 2026-02-23T17:49:52.489743 UTC*

## Overview

This runbook describes the lifecycle states of an AWS CloudFormation
stack and the operational actions associated with each state.

------------------------------------------------------------------------

## Common Stack Statuses

  -------------------------------------------------------------------------------
  Status                        Meaning                     Action
  ----------------------------- --------------------------- ---------------------
  CREATE_IN_PROGRESS            Stack is being created      Monitor events

  CREATE_COMPLETE               Stack created successfully  No action required

  CREATE_FAILED                 Creation failed             Inspect events

  UPDATE_IN_PROGRESS            Stack update in progress    Monitor

  UPDATE_COMPLETE               Update successful           No action required

  UPDATE_ROLLBACK_IN_PROGRESS   Update failed, rollback     Monitor
                                started                     

  UPDATE_ROLLBACK_FAILED        Rollback failed             Manual intervention
                                                            required

  ROLLBACK_COMPLETE             Stack creation failed and   Delete and recreate
                                rolled back                 

  DELETE_FAILED                 Stack deletion failed       Investigate resource
                                                            dependency
  -------------------------------------------------------------------------------

------------------------------------------------------------------------

## Inspect Stack Events

``` bash
aws cloudformation describe-stack-events --stack-name <stack-name>
```

Focus on: - `ResourceStatus` - `ResourceStatusReason` - IAM permission
errors - Resource already exists errors

------------------------------------------------------------------------

## Safe Operational Practices

-   Always review Change Sets before update
-   Enable termination protection for critical stacks
-   Use stack policies for production stacks
