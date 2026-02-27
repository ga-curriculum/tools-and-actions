# Tools and Actions: Instructor Guide  

**Module Time:** 120 minutes  

---

## What This Module Is About

This module transitions learners from:

> Designing how agents think → to controlling how agents act.

Up to this point, learners understand:

- What agents are (Session 1)  
- How reasoning loops operate (Session 3)  
- How architecture shapes autonomy (Session 2)  
- How prompts govern behavior across time (Session 6)  

This session introduces a critical shift:

> Tools are the boundary between cognition and consequence.

Without tools, agents generate text.  
With tools, agents modify systems, send communications, move money, and trigger workflows.

This module is about:

- Categorizing tool types  
- Understanding tool risk levels  
- Configuring tools safely  
- Designing approval gates and escalation  
- Preventing uncontrolled agency  

Learners should leave understanding:

- That tools are capability multipliers  
- That tool configuration is a security decision  
- That autonomy must be calibrated per action  
- That every agent must have a fallback path  

---

## Core Objectives

By the end of this module, learners should be able to:

- Categorize tools by capability and risk profile  
- Explain why read/write boundaries matter  
- Design a properly scoped tool schema  
- Implement least-privilege configuration  
- Define escalation and approval thresholds  
- Architect human-in-the-loop intervention deliberately  

---

## Must-Hit Topics

- Tools as execution layer  
- Five tool categories  
- Risk matrix & reversibility  
- Authentication patterns  
- Schema design principles  
- Least privilege architecture  
- Error handling strategy  
- Human-in-the-loop triggers  
- Approval gate pattern  
- Controlled autonomy  

---

# 1. Tools as Execution Architecture  
**Time:** ~15 minutes  

## Purpose

Shift learners from:

> “Tools are integrations”  

to:

> “Tools define the real-world impact surface of the agent.”

An agent’s reasoning layer can fail safely.  
A tool layer failure can cause financial loss, security breach, or reputational damage.

---

## Key Teaching Points

### Tools Are the Agent’s Hands

Reinforce prior concept:

- LLM = Brain  
- Tools = Hands  
- Prompt = Behavioral Control Layer  
- Orchestrator = Execution Supervisor  

Without tools → simulation  
With tools → real-world consequence  

Ask learners:

- What is the first irreversible action your current agent could take?
- Who approved that design?

---

# 2. The Tool Landscape: Categories and Capabilities  
**Time:** ~20 minutes  

This section should be interactive.

As you introduce each category, ask:

- What is the blast radius if this tool misfires?
- Is it reversible?
- Who owns the risk?

---

## Category 1: Search & Retrieval (Low Risk)

### Characteristics
- Read-only  
- No state modification  
- External or internal knowledge access  

### Risks
- Data leakage  
- Incorrect retrieval  
- Hallucination masking  

**Teaching Prompt**

“If this tool gives wrong data, what happens?”

Emphasize:

> Retrieval errors corrupt reasoning.

---

## Category 2: Data Processing & Computation (Medium Risk)

### Characteristics
- Executes logic  
- Performs math  
- Transforms data  

### Risks
- Sandbox escape  
- Data corruption  
- Spreadsheet overwrites  

Ask:

- Should this have write access to production?
- Where should sandbox boundaries exist?

---

## Category 3: Communication & Notification (High Risk)

### Characteristics
- Sends messages  
- Creates events  
- Notifies external parties  

### Risks
- Irreversible messaging  
- Legal exposure  
- Brand damage  

Critical discussion:

> An email cannot be unsent.  
> Should this default to draft mode?

---

## Category 4: System Actions & Write Operations (Very High Risk)

### Characteristics
- Modifies records  
- Processes payments  
- Grants permissions  

### Risks
- Financial loss  
- Security vulnerability  
- Regulatory violations  

Ask:

- What is the undo cost?
- Who audits these actions?

---

## Category 5: Human Escalation (Low Risk, High Value)

### Characteristics
- Pauses automation  
- Routes to human  
- Enables approval gates  

Teaching point:

> Lack of escalation is a design failure.

---

## Tool Risk Matrix Discussion

Have learners fill in:

| Category | Reversible? | Needs Approval? | Owner |
|----------|-------------|----------------|-------|

This becomes input for the lab.

---

# 3. Connecting and Configuring Tools Safely  
**Time:** ~20 minutes  

## Purpose

Demystify “connect your tool.”

Tool configuration has four architectural layers:

1. Authentication  
2. Schema  
3. Scope restriction  
4. Error handling  

---

## Authentication Patterns

Discuss:

- API keys  
- OAuth  
- Webhooks  

Ask:

- Where should credentials live?
- What happens if logs leak?

Reinforce:

> Credentials never belong in prompts.

---

## Schema as Behavioral Constraint

Explain that schema is:

> Prompt-level guardrail for tool invocation.

Critical principles:

- Verb-based naming  
- Clear negative guidance  
- Enum constraints  
- Required fields  

Ask learners:

“What happens if `new_status` is free text?”

---

## Least Privilege Architecture

Principle:

> Minimize blast radius.

Interactive prompt:

- What permission would you remove first from your current agent?

---

## Error Handling Protocol

Three failure classes:

- Transient  
- Data mismatch  
- Authorization failure  

Ask:

- Should the agent retry?
- Should it escalate?
- Should it refuse?

---

# 4. Human-in-the-Loop Architecture  
**Time:** ~20 minutes  

## Purpose

Reframe autonomy.

Autonomy is not binary.  
It is scoped per action.

---

## Four Escalation Triggers

### 1. Impact Threshold  
Define financial or operational boundary.

### 2. Irreversibility  
Hard-to-undo actions require approval.

### 3. Ambiguity  
Low confidence → escalate.

### 4. Sensitive Context  
Legal, regulatory, emotional scenarios.

Ask:

- Which trigger applies most in your industry?

---

## Approval Gate Pattern

Structured flow:

Agent prepares action →  
Calls approval tool →  
Human approves →  
Agent executes  

Have learners design one approval gate for their lab agent.

---

# 5. Break  
**Time:** ~10 minutes  

Reflection prompts:

- Where is your agent over-permissioned?
- Where is approval missing?
- What action worries you most?

---

# 6. Lab Transition  
**Time:** ~60 minutes  

Frame:

> “You are not connecting tools.  
> You are defining controlled agency.”

Learners must:

- Choose 3 tool categories  
- Define risk profile  
- Write a scoped schema  
- Define authentication pattern  
- Define error handling behavior  
- Define at least one approval threshold  
- Define one escalation path  

Encourage production mindset.

---

# Wrap-Up & Course Connection  
**Time:** ~5 minutes  

## Key Takeaways

- Tools define impact surface  
- Risk varies by category  
- Least privilege reduces blast radius  
- Schema design controls invocation  
- Escalation is a feature, not a failure  
- Autonomy must be earned  

> The most dangerous agent is not the smartest.  
> It is the one with unchecked permissions.

---

## Reflection Questions

- Which tool category is highest risk in your organization?
- What is currently over-permissioned?
- What action should never be autonomous?
- Where would an approval gate reduce risk most?

---

## Next Steps

Next module: **Memory and Learning**

Learners will move from execution control to state persistence — exploring how agents remember, adapt, and improve across sessions.
