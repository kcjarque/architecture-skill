---
name: "architecture"
description: Turn a business's CPSI (Context, Problem, Solution, Instructions) into an app architecture in the Module-Function-Process (MFP) format. Asks clarifying questions first, then builds. Use on /architecture.
---

# /architecture — CPSI → MFP

CPSI is the input. MFP (Module → Function → Process) is the output.

- **CPSI** captures the business: Context (full business info, team, volume, current tools, the process written as an arrow sequence, known rules, files/links), Problem, Solution, Instructions.
- **MFP** is the app architecture: Modules, each with Functions, each written as Input → Process → Output.

The reader is often a non-technical business owner or a beginner. Everything must be easy to read and recognizable as their own business.

## How it runs

**Step 1: Ask first. Do not build yet.**
Read everything given (CPSI, transcripts, files, screenshots, price lists). Then ask clarifying questions and STOP. Wait for the answers before writing anything else.

**Step 2: Build.**
After the user answers (or says "skip"), produce the output: the Overview, then the Module Details. Nothing else before them.

## Step 1: Clarifying questions

- Ask only questions whose answers change the architecture: team and roles, volume, which steps exist, approval rules, money rules (down payments, commissions, discounts, payment terms), and checking/counting frequency.
- Do not ask about anything the CPSI already answers.
- Ask 8–10 questions at most. Fewer is better.
- Group them by plain topic (Team, Volume, Orders, Money, Checking), never by module, since the user doesn't know the modules yet.
- Make every question multiple choice, with one option marked *(recommended)* as the default. Allow "pick all that apply" where it fits. The user can always type their own answer.
- Open with one line: "Before I build, I need a few answers. Reply with letters (e.g. 1a, 2b, 3a), or type 'skip' and I'll use the recommended answers."
- End there. Do not start the overview or modules in the same reply.

If the user says "skip" or "just build," use the recommended answers and list them under **Assumptions** at the end of the output.

## Step 2: The output

### Part A: Overview

In this order:

1. **In one line:** what the system does, in the owner's words.
   e.g. "A QR-based system so the artist always knows where every painting is, who has it, and whether it's paid."
2. **Quick terms:** define once, plainly:
   - **Status:** the stage a record is in right now.
   - **Handoff (→):** when one part of the system passes information to another.
   - **Gate:** when the system blocks a step because something is missing.
3. **The modules:** a table with columns Module | Nickname | What it does | Why you need it. The "why" ties to the client's own problem.
4. **How it connects:** a simple text diagram in a code block showing modules and what flows between them.
5. **The workflow:** one arrow line of the main steps, start to finish, using the client's real rules (e.g. "Reserve (50%)").
6. **The journey (status):** one arrow line of the main record's statuses.
7. **Who uses what:** one bullet per role, listing what they do in the system.
8. **A day in the life:** a short story (4–6 sentences) following one real-feeling item through every module, using a realistic Filipino name and the client's actual rules and numbers.
9. **Swimlane workflow diagram:** right after the overview, draw the main workflow as a visual swimlane diagram (use the inline visual/widget tool if available; otherwise an SVG or Mermaid `flowchart` with one `subgraph` per lane).
   - One horizontal lane per actor, labelled on the left: the customer first, then each role, then "System", then the Owner/approver last.
   - Every step sits in the lane of whoever does it. Rounded pills for customer actions, plain boxes for staff/system steps, diamonds for decisions (e.g. "In stock?", "Paid?", "Discount > 10%?").
   - Label decision branches "Yes"/"No", including the bad path (blocked, waiting, loop back).
   - Show the main loop (e.g. restock, rework) returning with "↻ back into …" instead of a long crossing arrow.
   - Box labels are 1–3 words, matching the step and status names used in the modules. Max ~16 nodes; leave out minor branches and mention them in one line below the diagram.
   - No arrow may cross through a box or a label. Reroute with L-shaped bends.

### Part B: Module Details

Every module in the EXACT format below, functions in the order work actually happens. No inline `[CONFIRM]` tags, since the questions were already answered.

```
Module: [Name] ([Nickname])

Responsibility: [One sentence: what it records/controls, from start to end]
Paper it owns: [The physical document, notebook, or form it replaces]
Who works here: [Roles, with what each does here in parentheses if needed]

CPS
- Context: [How this part works today, with real numbers]
- Problem: [What goes wrong here]
- Solution: [What this module does, one line]

---

### Function 1: [Verb + Noun]
*In short: [one plain-language sentence a noob understands].*

Input: [What comes in; name the source module for handoffs]
Process:
1. [Actor] [does one thing].
2. [Actor] [does one thing].
   - [Branch condition]: [what happens].
   - [Branch condition]: [what happens].

Output: [What is produced] → [Receiving module or person]

---
```

Repeat `---` + `### Function N` for every function. Repeat the whole block for every module.

### Part C: Assumptions (only if needed)

If the user skipped questions or left something unanswered, list each default used, one line each. Otherwise, leave this out.

## Quality rules

1. **Actor first, every step.** Each step starts with a role ("Sales", "Owner", "Cashier"), "The system", or "The customer". One action per step. No passive voice.
2. **Real numbers.** Use the figures and thresholds from the CPSI and the answers (30% rush, 10% discount limit, 50% down payment). Never invent one.
3. **Every decision is explicit.** Write each condition with its outcome, including the bad path: blocked, held for approval, or looped back ("go back to step 2").
4. **Named statuses in quotes.** When a function moves the record, a step sets the status (e.g., the system sets the status to "Quoted."). Keep status names identical across the overview and all modules.
5. **Gates.** When a step depends on another module (payment, approval, stock), the system checks it, blocks if missing, and shows what's missing. Release, delivery, and closing steps always check payment status.
6. **Arrow handoffs.** Outputs use `→` to name where things go ("Order total → Billing"). Inputs name their source ("Payment status from Billing"). If a module sends something to another module, the receiving function must list it as an Input.
7. **Paper it owns** is the real physical thing being replaced, so the owner recognizes it.
8. **Plain language.** A non-technical owner must be able to read it and say "yes, that's how we do it." No tech stack, tables, or code.
9. **5 functions or fewer per module.** If a module needs more, split it into two modules.

## Reference example (Module Details quality)

Module: Orders (The Counter)

Responsibility: Record, price, and track every order from inquiry to release.
Paper it owns: The order slip
Who works here: Sales staff, Owner (approvals)

CPS
- Context: 2 sales staff take 15–20 orders/day on Messenger and write them in a notebook.
- Problem: Orders get lost, prices aren't consistent, and status is unknown.
- Solution: One place to record, price, and track every order.

---

### Function 1: Create Order
*In short: write down a new order so it's never lost.*

Input: Customer name, contact, product, size, quantity, material
Process:
1. Sales opens a new order.
2. Sales enters the customer details (or picks an existing customer).
3. Sales selects the product and enters the size, quantity, and material.
4. The system generates the order number.
5. The system sets the status to "Inquiry."

Output: New order record (status: Inquiry)

---

### Function 2: Compute Quote
*In short: the system prices the order the same way every time.*

Input: Order details, price list, rush flag
Process:
1. The system pulls the base price from the price list.
2. The system multiplies it by size and quantity.
3. If rush is marked, the system adds 30%.
4. If Sales enters a discount:
   - 10% or less: applied right away.
   - More than 10%: sent to the Owner for approval and held until approved.
5. The system shows the final quote.
6. Sales sends the quote to the customer.
7. The system sets the status to "Quoted."

Output: Final quote amount → sent to the customer, and the order total → Billing

---

### Function 3: Approve Artwork
*In short: get the customer's yes on the design before printing.*

Input: Artwork file, customer confirmation
Process:
1. Sales uploads the artwork file to the order.
2. Sales sends the proof to the customer.
3. The customer replies: approve, or request changes.
4. If changes are requested: revise, upload again, and go back to step 2.
5. If approved: Sales records the approval with a date and time.
6. The system sets the status to "Artwork Approved."

Output: Approved artwork attached to the order

---

### Function 4: Release to Production
*In short: nothing goes to production without approved artwork and a down payment.*

Input: Approved artwork, payment status from Billing
Process:
1. The system checks that the artwork is approved.
2. The system checks Billing for the 50% down payment.
3. If either is missing, the system blocks the release and shows what's missing.
4. If both are complete, the system creates a job order.
5. If it's a rush order, the job order goes to the top of the queue.
6. The system sets the status to "In Production."

Output: Job order → Production

---

### Function 5: Track and Release Order
*In short: follow the order until the customer has it and has fully paid.*

Input: Status updates from Production, full payment status from Billing
Process:
1. Production updates the job stage; the order status follows it.
2. When Production marks it "Done," the system notifies the customer.
3. The customer pays the balance (recorded in Billing).
4. The system checks Billing for full payment.
   - Not fully paid: the system blocks the release and shows the balance.
   - Fully paid: go to step 5.
5. The customer picks up the order, or it's delivered.
6. Sales marks the order "Released."

Output: Closed order (status: Released), and a "Ready for pickup" message → Notifications
