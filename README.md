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
| 2 | Connecting and Configuring Tools Safely | 20 min |
| 3 | Human-in-the-Loop: When Agents Should Not Act Alone | 10 min |
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

**Think of it like:** giving the agent a library card and internet access. It can read anything it has been given access to — but it cannot change anything yet.

**Risk level: Low.** These tools only read — they do not modify anything. The main risk is if the agent reads incorrect information and presents it as fact.

**Key rule:** Only give the agent access to the information it actually needs. If it only needs to read sales data, don't give it access to HR records.


---

### Tool Category 2: Data Processing and Computation

These tools give the agent the ability to perform calculations, transformations, and analyses that the LLM alone cannot reliably execute — particularly for numerical precision, large datasets, or structured data manipulation.

<img width="692" height="179" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/4feea3bb-fe65-4b82-ba12-17d5f8776418" />

**Think of it like:** giving the agent a calculator and a spreadsheet. It can do the maths and update the data — but any changes it makes to a shared document are real and immediate.

**Risk level: Medium.** If the agent writes incorrect data to a shared spreadsheet, it affects everyone who uses it.

**Key rule:** Start with read-only access. Add write access only after you have verified the agent is working correctly in tests.

---

### Tool Category 3: Communication and Notification

These tools allow the agent to send messages, notifications, and alerts — making it an active participant in organizational communication rather than a passive generator of text.

<img width="692" height="124" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/13addfa0-674f-41d6-b3ac-612612974c0a" />

**Think of it like:** giving the agent access to your inbox and messaging apps. Once it sends something, you cannot unsend it.

**Risk level: High.** A message sent to the wrong person, with the wrong content, or at the wrong time can damage trust, create legal issues, or violate privacy — and the effect is usually irreversible.

**Key rule:** For anything going to external contacts or large groups, the agent should prepare a draft for a human to review before sending. Only automate sending for templated, low-risk messages to a well-defined audience.

---

### Tool Category 4: System Actions and Write Operations

These tools enable the agent to modify state in external systems — creating records, updating data, triggering workflows, or executing transactions. They are the highest-capability and highest-risk category of tools.

<img width="688" height="212" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/1efdc534-6773-43ca-9c76-1326389bbefa" />


**Think of it like:** giving the agent the ability to update your company's systems directly. Every change is real, immediate, and affects other people and processes.

**Risk level: Very high.** A wrong update to a customer record, an accidental refund, or a mistaken deletion can have serious business consequences — and may be impossible to undo.

**Key rule:** Before giving an agent this kind of access, answer three questions: What exactly is it allowed to change? Up to what value or impact can it act alone? What must a human approve first?

---

### Tool Category 5: Human Handoff and Escalation

These tools are often overlooked in tool taxonomies but are architecturally essential. They formalize the moment at which the agent transfers control — to a human, to another agent, or to an external review process.

<img width="694" height="164" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/ed49b7c6-e2f5-4053-9c4a-ce0ed14478ce" />


**Think of it like:** a well-trained employee who knows when to stop and ask their manager. This is not a limitation — it is what makes the system safe and trustworthy.

**Risk level: Low.** These tools reduce the chance of mistakes. Every agent must have at least one escalation path.


---

### The Tool Risk Matrix

Summarizing the five categories by risk and reversibility:

<img width="700" height="257" alt="image" src="https://git.generalassemb.ly/user-attachments/assets/b8023116-50c1-48dd-b672-41cabfb6190e" />

---

## 2. Connecting and Configuring Tools Safely (20 min)

### What "Connecting a Tool" Actually Means

When you connect an agent to a tool in a platform like Flowise, GPT Builder, or Relevance AI, you are doing three things:

1. **Telling the agent what the tool does** — so it knows when to use it
2. **Giving the agent permission to access it** — through a connection or login
3. **Setting the rules for how it can be used** — what it can and cannot do

---

### Step 1: Telling the Agent What the Tool Does

In no-code platforms, every tool has a name and a description. The agent reads this description to decide whether to use the tool in a given situation.

A good description answers two questions: *what does it do?* and *when should I use it?* It is also useful to say when it should NOT be used.

**Weak description — agent may misuse the tool:**
> "Google Sheets tool"

**Strong description — agent knows exactly when and how to use it:**
> "Use this tool to read the current list of open sales leads from the Sales Pipeline spreadsheet. Use it when the user asks about lead status or pipeline updates. Do not use it to write or update any data."

The description is not just documentation — it is an instruction the agent follows every time it decides whether to use the tool. Vague descriptions produce unpredictable behavior.

---

### Step 2: Giving the Agent Permission

Every tool connection requires some form of permission. In no-code platforms, this usually works in one of three ways:

**Connect your account (most common):** Click "Connect" and log in with your Google, Slack, or Zapier account. The platform stores a secure access permission on your behalf. You see this in Flowise, Relevance AI, and GPT Builder.

**Enter an API key:** Some tools require you to generate a key in the tool's settings and paste it into the platform. The key acts like a password that lets the agent access the tool.

**Use a pre-built connector:** Platforms like Zapier and Make have hundreds of pre-built app connections. You choose the app, log in, and the connector handles the rest.

**The safety rule that applies to all three:** Only grant the minimum level of access the agent needs. If it needs to read emails but not send them, configure it as read-only. If it needs one specific spreadsheet, don't give it access to your entire Google Drive.

---

### Step 3: Restricting Scope and Handling Failures

Once connected, restrict what the tool can actually do:

| Tool | Risky default | Safer configuration |
|---|---|---|
| Google Sheets | Access to all sheets in your Drive | One named spreadsheet only |
| Gmail / Outlook | Full send access | Draft-only, or approved contacts only |
| Slack | Post to any channel | One designated channel only |
| Zapier | Trigger any workflow | One specific, tested workflow only |
| CRM (HubSpot, Salesforce) | Read and write all records | Read-only, or write to one specific field |

**What happens when a tool fails?**

Tools break — connections time out, apps go down, data comes back in an unexpected format. Define what the agent should do before this happens:

- **Retry once, then stop** — for minor connection glitches
- **Tell the user and continue without the tool** — if the tool is optional for the task
- **Stop and escalate to a human** — for critical tools like payment processing or CRM updates

Define these fallback behaviors in the agent's instructions before going live — not after the first failure.

---

## 3. Human-in-the-Loop: When Agents Should Not Act Alone (10 min)

### The Autonomy Decision Is Not Binary

A common assumption is that the goal of an AI agent is to remove humans from the process entirely. This is the wrong goal. The right question is not "how do we eliminate the human?" — it is "at which specific moments does a human need to be involved?"

### Four Triggers That Always Require a Human

**Trigger 1: High-impact actions**
Define a clear limit above which the agent must pause and ask for approval:

| Action | Agent handles alone | Needs human approval |
|---|---|---|
| Processing a refund | Up to $200 | Over $200 |
| Sending an email | One recipient, templated content | Multiple recipients or custom content |
| Updating a record | Standard fields | Sensitive fields (payment, account tier) |
| Triggering a workflow | Internal, reversible | External or irreversible |

These limits are business decisions, not technical ones. The team responsible for the process defines the threshold — the platform enforces it.

**Trigger 2: Actions that cannot be undone**
Sending a mass email. Deleting a record permanently. Processing a settled payment. Ask: *"If this turns out to be wrong, how hard is it to fix?"* If the answer is "very hard" or "impossible," a human must approve before the agent acts.

**Trigger 3: Situations where the agent is not sure**
If a request is ambiguous, if two valid interpretations lead to very different actions, or if something doesn't add up — the agent should stop and ask rather than guess. An agent that guesses in ambiguous situations will eventually guess wrong.

**Trigger 4: Sensitive situations**
A customer who is distressed. A complaint involving a legal issue. A request touching on health or safety. These situations require human empathy and judgment — not automation.

### The Three Ways to Involve a Human

**Draft and Review:** Agent prepares everything; human approves before anything is sent or changed.
> Agent drafts a complaint response → human reviews → human clicks Send.

**Approval Gate:** Agent pauses at a specific step, notifies a human, and waits for the green light before continuing.
> Agent flags a refund over the limit → sends approval request to manager → processes only after approval is received.

**Full Escalation:** Agent hands the entire interaction to a human, with a full summary of everything that has happened so far.
> Agent cannot resolve the issue → creates a case summary → routes to a human agent with full context ready.

**A persistent misconception:** human-in-the-loop is not a failure of the AI to be "smart enough." It is a deliberate design choice that reflects appropriate risk management. The organizations that deploy agents most successfully are those that design human checkpoints intentionally — and treat them as permanent features, not temporary training wheels.

---

## 4. Break (10 min)

---

## 5. Lab (60 min)

https://pages.git.generalassemb.ly/Build-AI-Agents/Tools-and-Actions/

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


---

*General Assembly · Build AI Agents · BAA 16.0 · © 2026*
