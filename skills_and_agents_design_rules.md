

**What you are really asking**

You are asking: what is the discipline of workflow design itself — before we touch any specific step. Because if we do not know the criteria, we will keep producing steps that are individually reasonable but structurally incomplete or disconnected, which is exactly the problem we identified in the 15-technique system.

---

**The domain and expertise required**

Designing a workflow that converts to both skills and autonomous agents requires competence in three domains simultaneously. Missing any one produces a workflow that works in one mode and fails in the others.

**Domain 1 — The subject matter domain**
For this workflow: Canadian refugee law, IRB procedural rules, RAD appeal standards, the governing guidelines and their caselaw. Without this, the steps will be logically structured but legally wrong — they will miss obligations, misclassify rules, or produce outputs that cannot survive a RAD submission.

**Domain 2 — The information processing domain**
How AI agents read, extract, retain, and transfer information across sessions. This includes context limits, attention degradation, session statelesness, handoff protocols, and the distinction between what an agent can determine from text versus what requires human judgment. Without this, the workflow will be correct on paper but fail in execution — steps will lose information between them, agents will drift, and the output will be unreliable across cases.

**Domain 3 — Workflow engineering**
The structure of a workflow itself — how steps are defined, connected, and validated. This is what your question is directly about.

---

**The necessary elements of every step**

A step is not complete unless it has all of the following. If any element is missing, the step will either fail in execution or disconnect from the steps around it.

**1 — A single clear purpose**
Every step does one thing. Not two things done together for efficiency. One thing. If a step has two purposes, it must be split. The reason: when a step fails, you need to know exactly what failed. A step with two purposes produces ambiguous failures.

**2 — A defined input**
What exactly does this step receive before it begins? From where? In what format? An input that is vaguely described produces a step that works when the previous step happened to produce the right thing and fails when it did not — invisibly.

**3 — A defined output**
What exactly does this step produce when it completes? In what format? Stored where? Named how? The output of every step is the input of the next. If the output is not precisely defined, the connection between steps is a assumption, not a design.

**4 — A completion condition**
How does this step know it is done? This is different from the output. The output describes what was produced. The completion condition describes what must be true before the step can close. Without a completion condition, a step ends when the agent decides it is done — which means it ends at different points in different sessions.

**5 — A failure mode**
What can go wrong in this step, and what happens when it does? Does the step stop and wait for human input? Does it flag and continue? Does it return to a prior step? A step without a defined failure mode produces silent failures — the workflow continues as if the step succeeded when it did not.

**6 — A connection forward**
What does the next step need from this step, and does this step's output satisfy that need precisely? This is the connection test. Every step must be designed by looking at what it receives and what the step after it requires. A step designed in isolation from its neighbors will always have a gap at one or both boundaries.

**7 — A human involvement point (if any)**
If this step requires a human decision, that decision must be named, placed, and defined. What specifically does the human decide? What does the agent present to enable that decision? What format is the handoff? An unplaced human involvement point becomes either an interruption the workflow was not designed to handle, or a judgment the agent makes on its own.

**8 — A scope boundary**
What does this step explicitly not do? This is as important as what it does. Without a scope boundary, steps expand into each other's territory — especially when an agent encounters something adjacent to its task and addresses it without being asked. Scope boundaries prevent this and keep the steps clean.

---

**How steps connect to each other — the connection criteria**

A workflow is not a list of steps. It is a chain where every link is a defined handoff. A handoff is correct only when:

- The output format of step N matches the input format of step N+1 exactly
- The completion condition of step N guarantees the input condition of step N+1 is satisfied
- The failure mode of step N specifies what step N+1 receives when step N did not fully succeed
- The scope boundary of step N does not leave a gap that step N+1 assumes was covered

If any of these four handoff conditions is not met, there is a gap between the steps. The workflow will cross the gap when everything goes well and fall into it when anything goes wrong.

---

**The criteria for evaluating a completed workflow**

Once a workflow is designed, it must pass five tests before it is ready to build:

**Test 1 — Coverage**
Does the sequence of steps cover the entire task from first input to final output with no uncovered territory?

**Test 2 — Non-redundancy**
Does any step duplicate work that another step already did or will do?

**Test 3 — Handoff integrity**
For every adjacent pair of steps, does the output of the first satisfy the input requirement of the second precisely?

**Test 4 — Human involvement placement**
Are all human involvement points named, placed, and defined? Are there any points where human judgment is required but the workflow sends it to an agent?

**Test 5 — Failure resilience**
If any single step fails completely, does the workflow have a defined response, or does the failure propagate silently to the final output?

---

**What this means for R1 specifically**

The problem we identified — Step 2 floating between steps without a clear mechanism — is a violation of element 1 (single clear purpose) and element 6 (connection forward). Step 2 as written is not a step. It is a definition that belongs inside another step. The fact that it sat between two steps as if it were standalone is exactly the kind of structural incompleteness this discipline is designed to prevent.

Before we write any more protocol, every step we design must answer all eight elements. And before we call R1's protocol complete, it must pass all five tests.

---

