HFOS GOVERNING DOCUMENT (V1)
Household Financial Operating System — Runtime Specification  
Version: V1
Author: TheRealT0B
Scope: Tier‑1 Blueprint → Tier‑2 Runtime Governance
Purpose: Ensure deterministic, rebuildable, rule‑driven financial operations inside the HFOS Copilot Task runtime.

1. SYSTEM OVERVIEW
System Name: Household Financial Operating System (HFOS)
Version: V1

Purpose:  
HFOS is a structured, rule‑driven financial system where:

Core HFOS entities are strictly defined

Task generation is deterministic and rule‑based

Copilot enhances UX but cannot alter HFOS logic

HFOS is fully rebuildable from GitHub blueprints + local encrypted backup

HFOS is designed to be:

Deterministic

Auditable

Rebuildable

Version‑controlled

AI‑assisted but rule‑governed

2. CORE HFOS ENTITIES (NON‑NEGOTIABLE)
These entities must match the HFOS schema stored in /schema/ in GitHub.
Copilot may not create new entity types or modify required fields.

Code
{
  "entities": {
    "credit_card": {
      "required_fields": [
        "name",
        "issuer",
        "annualFee",
        "status",
        "statement_close_date",
        "due_date",
        "apr",
        "autopay_enabled"
      ]
    },

    "account": {
      "required_fields": [
        "accountName",
        "type",
        "institution",
        "balance",
        "asOfDate"
      ]
    },

    "task": {
      "required_fields": [
        "name",
        "type",
        "due_date",
        "priority",
        "status",
        "linked_entity"
      ]
    }
  }
}
Notes:

Field names follow HFOS schema naming conventions.

No new entity types may be introduced without a version increment.

No required field may be renamed, removed, or repurposed.

3. CORE CONSTRAINTS (DO NOT VIOLATE)
HFOS runtime must obey:

No new entities may be created outside the HFOS schema

No required fields may be removed or renamed

Entity relationships must remain consistent

Tasks must always reference a valid HFOS entity

Dates must never be inferred without explicit rules

All recurring behavior must originate from HFOS rules

Copilot may not modify schema or logic

Copilot may not delete tasks without explanation

Copilot may not infer financial data

These constraints ensure HFOS remains deterministic and rebuildable.

4. TASK GENERATION RULES (HFOS DETERMINISTIC ENGINE)
These rules generate tasks inside the HFOS Task Runtime.
They must be applied exactly as written.

4.1 Credit Card Payment Rule
Code
{
  "rule": "credit_card_payment",
  "trigger": "5 days before due_date",
  "action": "create_task",
  "task": {
    "name": "Pay {card_name}",
    "type": "payment",
    "priority": "high",
    "linked_entity": "credit_card"
  }
}
4.2 Statement Close Utilization Check
Code
{
  "rule": "utilization_check",
  "trigger": "3 days before statement_close_date",
  "action": "create_task",
  "task": {
    "name": "Check utilization {card_name}",
    "type": "review",
    "priority": "medium"
  }
}
4.3 Weekly HFOS Review
Code
{
  "rule": "weekly_review",
  "trigger": "every Friday",
  "action": "create_task",
  "task": {
    "name": "Weekly HFOS Review",
    "type": "review",
    "priority": "medium"
  }
}
4.4 Monthly Snapshot + Backup
Code
{
  "rule": "monthly_snapshot",
  "trigger": "last day of month",
  "action": "create_task",
  "task": {
    "name": "Record Net Worth + Export HFOS Backup",
    "type": "audit",
    "priority": "high"
  }
}
5. HFOS FINANCIAL CONSTRAINTS (SYSTEM TRUTHS)
Code
{
  "constraints": [
    "Credit card utilization per card < 10% at statement close",
    "Total utilization < 5%",
    "All credit cards must have a payment task before due date",
    "No missed due dates allowed",
    "Weekly cash flow must remain positive"
  ]
}
These constraints guide HFOS task generation and alerting.

6. ALLOWED COPILOT BEHAVIOR (FLEX ZONE)
Copilot MAY:
Create dashboards, summaries, and UI enhancements

Suggest optional tasks (must be labeled “suggestion”)

Add temporary helper fields (must not persist without approval)

Group or reorganize UI components

Improve readability and UX

Copilot MUST NOT:
Modify HFOS schema

Rename entities or fields

Change task generation rules

Infer financial data

Delete tasks without explanation

Create new entity types

Alter required fields

This ensures HFOS remains stable and predictable.

7. TASK NAMING CONVENTIONS (STRICT)
Payment Tasks
Pay {card_name}

Review Tasks
Check utilization {card_name}

Weekly HFOS Review

Audit Tasks
Record Net Worth + Export HFOS Backup

Naming must remain consistent for rebuildability and rule matching.

8. AUDIT & SELF‑INSPECTION COMMANDS
Copilot must support:

“Output full HFOS schema in JSON.”

“List all active HFOS task‑generation rules.”

“List all HFOS entities and their fields.”

“Identify deviations from HFOS spec.”

“List all active tasks grouped by entity.”

These commands ensure drift detection and system integrity.

9. HFOS REBUILD PROTOCOL (FAILSAFE)
If HFOS runtime becomes corrupted:

Recreate HFOS entities:

credit_card

account

task

Apply HFOS schema exactly as defined in GitHub

Reapply all task‑generation rules

Restore balances & transactions from encrypted local backup

Regenerate tasks using rules only

Target rebuild time: < 60 minutes

This ensures HFOS is fully recoverable.

10. VERSION CONTROL RULES
All structural changes require version increment

Copilot must not change behavior without explicit instruction

Each version must include:

Version number

Change description

Impact

Example:

Code
V2.3
- Added cash flow forecasting engine
- Impact: new weekly projection task
11. DATA MANAGEMENT POLICY
Sensitive Data Policy
No personal financial data stored in GitHub

GitHub contains:

Schemas

Rules

Prompts

Documentation

Backup Policy
Monthly encrypted export required

Must include:

Balances

Accounts

Transactions

Task states

Backups are stored locally in Tier‑3 encrypted JSON.

12. HFOS CONTROL PROMPT (IMPORTANT)
Use this to initialize or correct HFOS behavior:

Code
You are operating inside the Household Financial Operating System (HFOS).

You MUST follow all defined HFOS schema, rules, and constraints.
You may NOT modify core entities or task logic.

Before making structural changes:
- Explain the change
- Confirm compliance with HFOS rules

Focus on:
- Deterministic task generation
- Maintaining HFOS financial constraints
- Enhancing usability without altering structure

END OF HFOS GOVERNING DOCUMENT (V1)
