# Tools and Actions
**Session Time:** 120 minutes  
**Format:** Instructor-Led  

---

## Prerequisites

- Completion of Sessions 1–6  
- Understanding of the tool layer as an agent component (Sessions 2 and 3)  
- Familiarity with tool-call orchestration as one of the LLM's three roles (Session 3)  
- Understanding of tool usage guidance as a system prompt component (Session 6)  

---

## Learning Objectives

By the end of this session, students will be able to:

1. **List** common categories of tools that agents use for actions such as web searches, data retrieval, and system integrations.
2. **Connect** an agent to an external tool in a no-code platform and configure it safely.
3. **Assess** when to integrate human-in-the-loop interventions in tool-based workflows and define clear escalation and approval points for high-risk actions.

> Tools are where agents stop generating text and start affecting the world. This session is about understanding that boundary — what tools can do, how to configure them responsibly, and when a human must remain in the decision loop.

---

## Session Breakdown

| # | Segment | Duration |
|---|---------|----------|
| 1 | The Tool Landscape: Categories and Capabilities | 15 min |
| 2 | Connecting and Configuring Tools Safely | 15 min |
| 3 | Human-in-the-Loop: When Agents Should Not Act Alone | 15 min |
| 4 | Break | 10 min |
| 5 | Lab | 60 min |
| 6 | Wrap-Up & Reflection | 5 min |
|   | **Total** | **120 min** |

---

## 1. The Tool Landscape: Categories and Capabilities (15 min)

### Tools Are the Agent's Hands

<img width="547" height="260" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/a32440d5-e8f4-4f9f-a908-b48f2e331840" />


### Tool Category 1: Search and Information Retrieval

These tools give the agent access to information that exists outside its training data — current, specific, or proprietary knowledge that the LLM cannot generate from memory alone.

<img width="695" height="176" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/88fb764f-1991-404a-8163-d941cf83ed01" />

**Risk profile:** Generally low. These tools read data; they do not modify it. The primary risks are data exposure (retrieved content may be sensitive) and hallucination masking (an agent that retrieves incorrect data may present it confidently as fact).

**Configuration principle:** Where possible, provide read-only credentials. An agent that only needs to read from a database should never be given write access.

---

### Tool Category 2: Data Processing and Computation

These tools give the agent the ability to perform calculations, transformations, and analyses that the LLM alone cannot reliably execute — particularly for numerical precision, large datasets, or structured data manipulation.

<img width="692" height="179" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/4feea3bb-fe65-4b82-ba12-17d5f8776418" />


**Risk profile:** Medium. Code execution introduces the risk of unintended side effects if the sandbox is not properly isolated. Spreadsheet write access can overwrite or corrupt business data.

**Configuration principle:** Code interpreters must run in isolated sandboxes with no access to the host filesystem or network unless explicitly required. Write access to production data should require an additional confirmation step.

---

### Tool Category 3: Communication and Notification

These tools allow the agent to send messages, notifications, and alerts — making it an active participant in organizational communication rather than a passive generator of text.

<img width="692" height="124" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/13addfa0-674f-41d6-b3ac-612612974c0a" />


**Risk profile:** High. Communication tools have direct external impact. An agent that sends emails or messages autonomously can cause reputational, legal, or operational harm if it acts incorrectly. The effects are often irreversible — a sent email cannot be unsent.

**Configuration principle:** Communication tools should default to a draft-and-review pattern unless the message type is templated, low-risk, and the recipient population is well-defined. Mass communications (to mailing lists, multiple Slack channels, or bulk SMS) should always require human approval.

---

### Tool Category 4: System Actions and Write Operations

These tools enable the agent to modify state in external systems — creating records, updating data, triggering workflows, or executing transactions. They are the highest-capability and highest-risk category of tools.

<img width="688" height="212" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/1efdc534-6773-43ca-9c76-1326389bbefa" />


**Risk profile:** Very high. Write operations are often irreversible or difficult to undo. A financial transaction processed in error may require manual reconciliation. A provisioning action that grants excessive permissions creates a security vulnerability. A CRM record updated with incorrect data can propagate errors through downstream systems.

**Configuration principle:** Write tools must have clearly defined scope (which records, which fields, which value ranges), reversibility assessment (can this be undone?), and approval thresholds (above what value or impact does this require human sign-off?).

---

### Tool Category 5: Human Handoff and Escalation

These tools are often overlooked in tool taxonomies but are architecturally essential. They formalize the moment at which the agent transfers control — to a human, to another agent, or to an external review process.

<img width="694" height="164" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/ed49b7c6-e2f5-4053-9c4a-ce0ed14478ce" />


**Risk profile:** Low — these tools reduce autonomous action, they do not increase it. The risk of not having them is far greater than the risk of having them.

**Configuration principle:** Every agent should have at least one escalation path. An agent with no mechanism to involve a human is an agent with no fallback when it fails.

---

### The Tool Risk Matrix

Summarizing the five categories by risk and reversibility:

<img width="700" height="257" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/b8023116-50c1-48dd-b672-41cabfb6190e" />

---

## 2. Connecting and Configuring Tools Safely (15 min)

### From Concept to Connection: What Tool Configuration Actually Involves

When a platform documentation says "connect your agent to Google Sheets," what does that actually mean architecturally? Understanding the underlying components of a tool connection prevents misconfiguration and security gaps.

A tool connection has four components:

<img width="697" height="273" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/4065695f-6411-4dbb-a420-68124c702253" />


### Authentication Patterns

The method of authentication determines the security boundary of the tool. There are three common patterns:

#### API Key Authentication

A static key is stored in the agent's configuration and passed in the request header. Simple and widely supported, but carries risk: if the key is exposed (through logs, error messages, or prompt injection), it grants full access to whoever has it.

```
Configuration:
  TOOL: google_sheets
  API_KEY: [stored in environment variable, never in the prompt]
  SCOPE: read + write (spreadsheets only)
```

**Best practice:** Store API keys as environment variables, never in the system prompt or tool schema. Rotate keys on a defined schedule. Use the minimum permission scope the tool requires.

#### OAuth 2.0

The agent authenticates as a specific user or service account through a delegated authorization flow. More secure than API keys because tokens are scoped, short-lived, and can be revoked without changing the underlying credential.

**Best practice:** Use service accounts with the minimum required permissions, not personal user accounts. Log which service account is being used for each tool call — this creates an auditable trail.

#### Webhook Authentication

The tool is triggered by an incoming HTTP request to a URL. The agent sends a request with a shared secret in the header; the receiving service validates the secret before executing the action.

**Best practice:** Always validate webhook signatures. Never expose webhook URLs publicly without authentication — an unauthenticated webhook is an open door to uncontrolled execution.

### Schema Definition: How the LLM Understands a Tool

The tool schema is the structured description the LLM reads to understand what a tool does and how to call it. A well-written schema is the difference between an agent that uses tools correctly and one that calls them with wrong parameters, in the wrong order, or in the wrong situations.

**Schema components:**

```json
{
  "name": "update_order_status",
  "description": "Updates the status of a customer order in the order management system. 
                  Use this only after verifying the customer's identity and confirming 
                  the new status is valid. Do not use to cancel orders — use 
                  cancel_order instead.",
  "parameters": {
    "order_id": {
      "type": "string",
      "description": "The unique order identifier, format: ORD-XXXXXX"
    },
    "new_status": {
      "type": "string",
      "enum": ["processing", "shipped", "delivered", "on_hold"],
      "description": "The new status to assign. Cannot set to 'cancelled' via this tool."
    },
    "reason": {
      "type": "string",
      "description": "Brief explanation for the status change, for audit logging."
    }
  },
  "required": ["order_id", "new_status", "reason"]
}
```

**Schema writing principles:**

- **Name:** Use a verb-noun format that describes what the tool does, not what it is (`update_order_status`, not `order_management`)
- **Description:** Include when to use it AND when not to use it — the LLM makes better tool selection decisions with negative guidance
- **Parameters:** Use enums to constrain valid values wherever possible; free-text parameters introduce unpredictability
- **Required fields:** Mark every field the tool cannot function without as required — this prevents silent failures from missing parameters

### Scope Restriction: The Principle of Least Privilege

The principle of least privilege states that any system — including an agent — should have access only to the resources and capabilities it needs to perform its defined function, and nothing more.

In practice, this means:

| Instead of... | Configure... |
|---|---|
| Full CRM read/write access | Read access to customer records; write access to interaction logs only |
| Full Gmail access | Send-only access, restricted to approved recipient domains |
| Full database access | Read-only access to the specific tables the agent needs |
| Admin-level API credentials | A service account with the minimum permission set |

Scope restriction is not about distrust — it is about containing the blast radius when something goes wrong. An agent with minimal permissions that behaves unexpectedly causes minimal damage. An agent with admin permissions that behaves unexpectedly can be catastrophic.

### Error Handling: What Happens When a Tool Fails

Tool failures are not edge cases — they are routine occurrences in production systems. APIs rate-limit, networks time out, external services have downtime, and parameters that passed validation in testing fail against live data. An agent that has no defined behavior for tool failures will either loop indefinitely, hallucinate a result, or crash.

Every tool integration should define responses for three failure modes:

**1. Transient failure (temporary unavailability):**
```
Policy: Retry once after 5 seconds. 
If retry fails: inform the customer of a temporary delay, 
log the failure, and offer to follow up via email.
```

**2. Data error (tool returns unexpected format or empty result):**
```
Policy: Do not assume. 
If order_lookup returns null: do not tell the customer the order doesn't exist — 
ask them to verify the order number and try once more. 
If the second attempt fails: escalate.
```

**3. Authorization error (tool returns 401 or 403):**
```
Policy: Do not retry. 
Log the authentication failure as a security event. 
Inform the customer that you are unable to access this information currently 
and escalate to a human agent.
```

---

## 3. Human-in-the-Loop: When Agents Should Not Act Alone (15 min)

### The Autonomy Decision Is Not Binary

A common framing error is treating human-in-the-loop as an on/off switch: either the agent is autonomous, or a human does the work. In practice, the human-in-the-loop question is more nuanced: **for which specific actions, above which thresholds, in which contexts, does this agent need human authorization before proceeding?**

The answer is different for every tool, every organization, and every risk environment. The goal of this segment is to provide a framework for making that determination deliberately, rather than by default or omission.

### Four Triggers for Human-in-the-Loop Intervention

#### Trigger 1: Impact Threshold

Define a quantitative boundary above which autonomous action is not permitted, regardless of the agent's confidence.

| Tool | Autonomous Threshold | Requires Approval Above |
|---|---|---|
| Refund processor | Up to $200 | $200 |
| Purchase order | Up to $1,000 | $1,000 |
| Email send | Single recipient, templated content | Multiple recipients or custom content |
| Account permission change | Read access grants | Write or admin access grants |
| Data deletion | Soft delete (recoverable) | Hard delete (permanent) |

Thresholds should be set by the business stakeholder responsible for the process — not by the AI team. The AI team can implement the threshold; the business owner must define it.

#### Trigger 2: Irreversibility

Any action that cannot be undone or is difficult to reverse should require human confirmation before execution.

**Irreversible or hard-to-reverse actions:**
- Sending external communications (emails, SMS, notifications)
- Permanent data deletion
- Financial transactions (once settled)
- Account terminations
- Provisioning actions (especially permission grants)

**A useful test:** "If this action turns out to be wrong, how much effort is required to undo it?" Actions where the answer is "significant effort" or "impossible" should have human approval gates.

#### Trigger 3: Confidence and Ambiguity

Even within its defined scope, an agent should escalate when it is not confident that it has understood the situation correctly.

This is particularly important when:
- The user's intent is ambiguous and clarification has not resolved it
- Multiple valid interpretations of a request lead to significantly different actions
- The retrieved data contradicts the user's stated information
- The situation falls at the boundary of a defined constraint (e.g., a refund request that is 1 day outside the policy window)

The agent should be explicitly instructed — in the system prompt — to escalate in these cases rather than guess.

#### Trigger 4: Sensitive Context

Some situations warrant human involvement regardless of the action's technical scope or financial value.

**Examples:**
- A customer expresses significant distress or frustration
- A request involves a potential legal claim or regulatory issue
- A user's account shows signs of suspicious activity
- The request involves a protected characteristic (medical, legal, financial advice in regulated contexts)

These are not situations where an agent's technical capability is the limiting factor — they are situations where human judgment, empathy, and accountability are required.

### Designing the Escalation Path

Every agent needs at least one escalation path. The escalation path answers three questions:

**1. When does escalation trigger?**  
Define the conditions explicitly in the system prompt and in the tool configuration. Vague conditions ("when appropriate") produce inconsistent escalation behavior.

**2. What information is passed to the human?**  
The handoff must include everything the human needs to continue without starting over: the customer's identity, the issue summary, the actions already taken, the agent's recommended next step, and the reason for escalation.

**3. What does the customer experience during escalation?**  
The agent should inform the customer that escalation is happening, set an expectation for response time, and — where possible — provide a reference number. A customer who is escalated without explanation experiences the escalation as abandonment.

### The Approval Gate Pattern

For high-impact actions that do not require full escalation — the agent can proceed, but only after a human confirms — the approval gate pattern provides a middle path between full autonomy and full escalation.

```
AGENT:  "I'm ready to process a refund of $380 to Maria Silva's original 
         payment method for order #48291. This exceeds the standard refund 
         limit. Sending approval request to the operations team now."

→ approval_request tool called
→ Operations team member receives: amount, customer, order, agent's reasoning
→ Operations team member approves or rejects via approval interface
→ Agent receives response and proceeds or escalates accordingly
```

The approval gate preserves the efficiency benefit of agent automation for the majority of the workflow, while maintaining human control at the highest-risk decision point.

### Human-in-the-Loop Is Not a Failure

A persistent misconception in AI deployment is that human-in-the-loop requirements represent a failure of the AI to be "fully autonomous." This framing is wrong and dangerous.

Human-in-the-loop is a **design choice that reflects appropriate risk calibration**. An agent that acts autonomously in situations where it should not is not more capable — it is more dangerous. The organizations that deploy agents most successfully are those that design human checkpoints deliberately and treat them as permanent features of the system, not temporary scaffolding to be removed when the agent "matures."

Autonomy should be earned through demonstrated reliability at lower levels of scope, not assumed from the outset.

---

## 4. Break (10 min)

---

## 5. Lab (60 min)

*Lab details in the Lab Guide document.*

---

## 6. Wrap-Up & Reflection (5 min)

### Key Takeaway 
- Tools Expand Capability — and Blast Radius
- Schema Design Determines Agent Behavior
- Autonomy Must Be Calibrated, Not Maximized

> Next session: **Memory and Learning** 

---

## Resources

- [OpenAI — Function Calling and Tool Use](https://platform.openai.com/docs/guides/function-calling)
- [Anthropic — Tool Use Guide](https://docs.anthropic.com/en/docs/build-with-claude/tool-use)
- [LangChain — Tools and Toolkits](https://docs.langchain.com/docs/components/tools)
- OWASP — *Top 10 for Large Language Model Applications: Excessive Agency* (2023)
- [n8n — Credential Management](https://docs.n8n.io/credentials/)
- [Flowise — Tool Configuration Guide](https://docs.flowiseai.com/integrations/tools)
- NIST — *Principle of Least Privilege* (SP 800-53)
