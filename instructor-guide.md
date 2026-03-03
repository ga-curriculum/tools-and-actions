# Tools and Actions: Instructor Guide  

**Module Time:** 120 minutes  

---

## What This Module Is About

This module marks a turning point in the course.

Up to this point, learners have understood:

- What agents are  
- How reasoning works  
- How prompts shape behavior  
- How architecture structures autonomy  

Now we move from:

> Designing how agents think  
to  
> Controlling how agents act.

This is the session where agents stop being advisors and start being operators.

Without tools, an agent generates text.  
With tools, it can:

- Search the web  
- Read or update a spreadsheet  
- Send an email  
- Trigger a workflow  
- Modify a record in a system  

That shift introduces real-world consequence.

This module is not about integrations.  
It is about controlled agency.

Learners should leave understanding:

- The different categories of tools  
- The risk profile of each category  
- How to configure tools safely in no-code platforms  
- When humans must remain in the loop  
- Why autonomy must be scoped — not assumed  

---

## Core Objectives

By the end of this module, learners should be able to:

- Identify and categorize common agent tools  
- Assess risk by reversibility and impact  
- Configure a tool safely in a no-code environment  
- Restrict scope using least-privilege principles  
- Define clear approval thresholds  
- Architect a deliberate human-in-the-loop design  

---

## Must-Hit Topics

- Tools as execution layer  
- Five tool categories  
- Risk levels and reversibility  
- Tool descriptions as behavioral control  
- Minimum access configuration  
- Failure handling strategy  
- Approval gates  
- Escalation design  
- Responsible autonomy  

---

## 1. The Tool Landscape: Categories and Capabilities  
**Time:** ~20 minutes  

### Purpose

Help learners understand:

> Tools define the real-world impact surface of the agent.

An LLM mistake may generate wrong text.  
A tool mistake may change a system permanently.

Frame this clearly before diving into categories.

---

### Teaching Strategy

For each category:

- Ask: What could go wrong?
- Ask: Is it reversible?
- Ask: Who owns the risk?

Make this interactive.  
Have learners answer verbally before revealing the “risk level.”

---

### Category 1: Search and Information Retrieval  
**Risk Level: Low**

Examples:
- Web search  
- Reading Google Drive documents  
- Querying a database  
- Pulling live inventory data  

Teaching analogy:

> Giving the agent a library card.

It can read — but not change.

### Key Teaching Points

- These tools are read-only  
- Primary risk is misinformation  
- Access must still be restricted  

Ask:

- If the agent retrieves incorrect data, what happens next?
- Should it have access to all company documents?

---

### Category 2: Data Processing and Calculation  
**Risk Level: Medium**

Examples:
- Updating a spreadsheet  
- Extracting information from a PDF  
- Performing refund calculations  

Teaching analogy:

> Giving the agent a calculator and shared spreadsheet access.

Changes are real and visible to others.

### Key Teaching Points

- Write access introduces risk  
- Testing should begin in read-only mode  
- Changes propagate quickly  

Ask:

- What happens if it writes incorrect data?
- Who notices first?

---

### Category 3: Communication and Notification  
**Risk Level: High**

Examples:
- Sending emails  
- Posting in Slack  
- Scheduling calendar invites  
- Sending SMS  

Teaching analogy:

> Giving the agent your inbox.

Once sent → cannot be undone.

### Critical Teaching Moment

Ask:

- What is the reputational blast radius of a wrong email?
- Should this default to draft mode?

Emphasize:

> External communication should default to draft + review.

---

## Category 4: System Actions (Create, Update, Delete)  
**Risk Level: Very High**

Examples:
- Updating CRM records  
- Triggering workflows  
- Processing refunds  
- Deleting files  

Teaching analogy:

> Giving the agent system administrator access.

Every action changes reality.

### Key Teaching Points

Before enabling these tools, answer:

1. What exactly can it change?
2. Up to what threshold can it act alone?
3. What requires approval?

---

## Category 5: Human Handoff and Escalation  
**Risk Level: Low (Risk-Reducing)**

Examples:
- Sending approval requests  
- Routing to a human  
- Creating summary for review  

Teaching analogy:

> A trained employee who knows when to ask their manager.

### Core Message

Every agent must have an escalation path.

An agent without escalation is overconfident by design.

---

## Tool Risk Table (Discuss Live)

| Category | Risk Level | Reversible? | Needs Approval? |
|-----------|------------|--------------|-----------------|
| Search & Retrieval | Low | N/A | No |
| Data Processing | Medium | Sometimes | Sometimes |
| Communication | High | Rarely | Often |
| System Actions | Very High | Rarely/Never | Yes |
| Escalation | Low | N/A | No |

Have learners reflect:

- Which category exists in their lab agent?
- Which one worries them most?

---

# 2. Connecting and Configuring Tools Safely  
**Time:** ~15 minutes  

## Purpose

Demystify what “connect a tool” actually means.

Connecting a tool involves three decisions:

1. What it does  
2. What it can access  
3. What it cannot do  

---

## Step 1: Tool Description as Behavioral Control

The description is not documentation.  
It is instruction.

Show contrast:

Weak:
> "Google Sheets Tool"

Strong:
> "Use this tool to read the Sales Pipeline spreadsheet when the user asks about lead status. Do not use it to write or modify data."

Ask:

- What happens if the description is vague?
- How does that affect predictability?

---

## Step 2: Granting Permission

Permissions may be:

- OAuth login  
- API key  
- Prebuilt connector  

### Critical Rule

Grant minimum access.

Ask:

- Does it need full Drive access?
- Does it need to send emails — or only draft?

---

## Step 3: Scope Restriction

Show safe configuration examples:

| Tool | Risky | Safer |
|------|--------|--------|
| Google Sheets | All sheets | One specific sheet |
| Gmail | Send enabled | Draft only |
| Slack | All channels | One channel |
| Workflow tool | All workflows | One workflow |

Reinforce:

> Scope defines blast radius.

---

## Step 4: Failure Handling

Ask:

What should happen if:

- The connection times out?
- The data comes back empty?
- The API rejects the request?

Define:

- Retry once  
- Inform user  
- Escalate for critical tools  

Failure policy should be written before launch.

---

# 3. Human-in-the-Loop: When Agents Should Not Act Alone  
**Time:** ~10 minutes  

## Purpose

Reframe autonomy.

Autonomy is not about removing humans.  
It is about defining boundaries clearly.

---

## Four Human Triggers

### 1. High Impact

Define thresholds:

| Action | Autonomous | Approval Required |
|--------|------------|------------------|
| Refund | ≤ $200 | > $200 |
| Email | 1 recipient template | Multi-recipient custom |

These are business decisions — not AI decisions.

---

### 2. Irreversible Actions

Ask:

- Can this be undone easily?

If no → approval required.

---

### 3. Ambiguity

If intent unclear → clarify or escalate.

Agents should not guess.

---

### 4. Sensitive Context

Legal, health, safety, distress.

Human empathy > automation.

---

## Three Human Patterns

1. Draft & Review  
2. Approval Gate  
3. Full Escalation  

Clarify:

Human-in-the-loop is not weakness.  
It is responsible system design.

---

# 4. Break  
**Time:** ~10 minutes  

---

# 5. Lab: Tool Integration Challenge  

---

# Wrap-Up & Course Connection  
**Time:** ~5 minutes  

## Key Takeaways

- Tools create real-world impact  
- Risk increases with write access  
- Scope restriction limits damage  
- Descriptions guide behavior  
- Human checkpoints are essential  
- Autonomy must be scoped  

> The danger is not intelligence.  
> The danger is unchecked execution.

---

## Reflection Questions

- Which tool category is highest risk in your organization?
- What action should never be autonomous?
- Where would an approval gate reduce risk most?
- What is currently over-permissioned?

---

## Next Session

**Memory and Learning**

Next, learners will explore how agents persist state, summarize interactions, and improve over time — without losing control.
