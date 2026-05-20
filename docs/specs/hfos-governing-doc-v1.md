# HFOS GOVERNING DOCUMENT (V1)
**Household Financial Operating System — Runtime Specification**
Version: V1.4
Author: TheRealT0B
Scope: Tier-1 Blueprint → Tier-2 Runtime Governance
Last Updated: 2026-05-19

---

## 1. SYSTEM OVERVIEW

**System Name:** Household Financial Operating System (HFOS)
**Current Version:** V1.4

**Purpose:**
HFOS is a structured, rule-driven financial system where:
- Core HFOS entities are strictly defined
- Task generation is deterministic and rule-based
- Copilot enhances UX but cannot alter HFOS logic
- HFOS is fully rebuildable from GitHub blueprints + local encrypted backup

**Design Principles:**
- Deterministic
- Auditable
- Rebuildable
- Version-controlled
- AI-assisted but rule-governed

---

## 2. CORE HFOS ENTITIES (NON-NEGOTIABLE)

These entities must match the HFOS schema stored in `/schema/schema.json`.
Copilot may not create new entity types or modify required fields.

### Entity: credit_card
Required fields: `name`, `issuer`, `annualFee`, `status`, `statement_close_date`, `due_date`, `apr`, `autopay_enabled`

### Entity: account
Required fields: `accountName`, `type`, `institution`, `balance`, `asOfDate`

### Entity: task
Required fields: `name`, `type`, `due_date`, `priority`, `status`, `linked_entity`

### Entity: transaction
Required fields: `id`, `date`, `merchant`, `category`, `amount`

### Entity: user
Required fields: `id`, `name`, `role`, `chase524`, `spendPatterns`

### Entity: rewards
Required fields: `id`, `program`, `balance`, `pointValuation`

### Entity: goal
Required fields: `id`, `type`, `title`, `status`

**Rules:**
- Field names follow HFOS schema naming conventions exactly.
- No new entity types may be introduced without a version increment.
- No required field may be renamed, removed, or repurposed.

---

## 3. CORE CONSTRAINTS (DO NOT VIOLATE)

HFOS runtime must obey:
- No new entities may be created outside the HFOS schema
- No required fields may be removed or renamed
- Entity relationships must remain consistent
- Tasks must always reference a valid HFOS entity via `linked_entity` and `linked_id`
- Dates must never be inferred without explicit rules
- All recurring behavior must originate from HFOS rules
- Copilot may not modify schema or logic
- Copilot may not delete tasks without explanation
- Copilot may not infer financial data
- Credit card utilization per card must remain < 10% at statement close
- Total utilization must remain < 5%
- All credit cards must have a payment task before due date
- No missed due dates allowed
- Weekly cash flow must remain positive

---

## 4. TASK GENERATION RULES (HFOS DETERMINISTIC ENGINE)

### Rule 4.1 — Credit Card Payment
```json
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
