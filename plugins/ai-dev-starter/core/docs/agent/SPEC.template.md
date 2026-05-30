<!-- ai-dev-starter | core | v0.1.0 -->
# Functional Specification — {{PROJECT_NAME}}

What the system must do, per module. This is the source of truth for *behavior*.
Architecture (how) lives in `ARCHITECTURE.md`; library choices (with what) in `DECISIONS.md`.

## Vision

{{VISION_SUMMARY}}
<!-- Product, users/roles, problem solved, success criteria, explicit non-goals for v1. -->

## Roles & permissions

{{ROLES_TABLE}}
<!-- | Role | Description | Key permissions | -->

## Modules

<!-- Repeat this block per module. A module is "done" when a developer who never spoke to the
     user could implement it from this section alone. -->

### Module: {{MODULE_NAME}}

**Purpose:** {{MODULE_PURPOSE}}

**Entities:** {{ENTITIES_AND_KEY_FIELDS}}

**Operations:**

| Operation | Actor (role) | Description |
|-----------|--------------|-------------|
| {{OP}} | {{ROLE}} | {{DESC}} |

**Business rules (EARS notation):**

- WHEN {{trigger}} THE SYSTEM SHALL {{response}}.
- WHILE {{state}} WHEN {{trigger}} THE SYSTEM SHALL {{response}}.
- IF {{precondition}} THEN THE SYSTEM SHALL {{response}}.

**Relationships:** {{CROSS_MODULE_LINKS}}

---

## Domain primitives

Value types with validation/formatting rules — these seed the `domain-reviewer` agent.

| Primitive | Format / rule | Example |
|-----------|---------------|---------|
| {{NAME}} | {{RULE}} | {{EXAMPLE}} |

## Internationalization

{{I18N_NOTES}}
<!-- Locales supported, how strings are keyed, which fields are localized. -->
