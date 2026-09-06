Excellent. Day 7 is where ACTDRS starts becoming intelligent.

Until now, ACTDRS has been using fixed rules.

Example:

if cpu > 90:
    status = "HIGH"

This is not AI.

Today, we'll begin preparing ACTDRS to make decisions based on multiple factors, which is the foundation of an AI-driven cybersecurity system.

ACTDRS Progress
✅ Phase 1 : Environment Setup
✅ Phase 2 : Monitoring Engine
✅ Phase 3 : Threat Detection
🟡 Phase 4 : Response Engine
▶ Phase 5 : AI Decision Engine (Start Today)
First, understand the difference
Rule-Based System

Suppose:

CPU = 95%

Rule:

IF CPU > 90

↓

HIGH

Simple.

AI-Based System

Now imagine:

CPU = 95%

RAM = 85%

Unknown Process

Network Spike

USB Inserted

AI thinks:

All these events happened together

↓

Looks like ransomware

↓

Confidence = 93%

↓

Recommend Isolating Computer

Notice the difference?

AI looks at multiple features together, not just one value.

Today's Architecture
Monitoring Engine

↓

Threat Detection

↓

AI Decision Engine

↓

Response Engine
Step 1 – Create AI Folder

You already have an ai folder.

Inside it create:

decision_engine.py

Your project becomes:

ACTDRS/

ai/

    decision_engine.py
Step 2 – Paste This Code

Open

ai/decision_engine.py

Paste:

def decide(cpu_status, ram_status, disk_status):

    if cpu_status == "HIGH" and ram_status == "HIGH":

        return "POSSIBLE_ATTACK"

    elif cpu_status == "HIGH":

        return "CHECK_CPU"

    elif disk_status == "HIGH":

        return "CHECK_DISK"

    else:

        return "NORMAL"

Save the file.

What does this function do?

Suppose ACTDRS gets:

CPU = HIGH

RAM = HIGH

Disk = NORMAL

Call:

decision = decide("HIGH", "HIGH", "NORMAL")

Output:

POSSIBLE_ATTACK

Suppose:

CPU = HIGH

RAM = NORMAL

Disk = NORMAL

Output:

CHECK_CPU

Suppose:

Everything Normal

Output:

NORMAL
Step 3 – Connect It

Open

monitoring/system_monitor.py

At the top, add:

from ai.decision_engine import decide
Step 4 – Use the Decision Engine

After you've calculated:

cpu_status
ram_status
disk_status

Add:

decision = decide(
    cpu_status,
    ram_status,
    disk_status
)

print(f"AI Decision : {decision}")
What happens?

Example output:

CPU : HIGH

RAM : HIGH

Disk : NORMAL

AI Decision : POSSIBLE_ATTACK
Why are we doing this?

Because tomorrow we will stop responding directly to CPU or RAM alerts.

Instead:

Current flow:

CPU HIGH

↓

Response

New flow:

CPU HIGH

RAM HIGH

Disk NORMAL

↓

Decision Engine

↓

POSSIBLE_ATTACK

↓

Response Engine

This is much closer to how real cybersecurity platforms work.

Is this really AI?

No.

This is an important point.

Today's decision_engine.py is still rule-based logic.

We're calling it the "AI Decision Engine" because we're building the architecture that will later accept predictions from a machine learning model.

Later, we can replace this:

if cpu_status == "HIGH" and ram_status == "HIGH":
    return "POSSIBLE_ATTACK"

with:

prediction = model.predict(features)

without changing the rest of the project.

That's why we separated it into its own module.

Day 7 Learning Outcome

By the end of today, your ACTDRS will have four independent modules:

Monitoring Engine
        │
        ▼
Threat Detection Engine
        │
        ▼
Decision Engine
        │
        ▼
Response Engine

This modular architecture is exactly what you'll use in your SIH presentation diagram.

Before we continue to Day 8

I want you to complete these steps and make sure your program prints something like:

CPU Status : NORMAL
RAM Status : NORMAL
Disk Status : NORMAL

AI Decision : NORMAL

or

CPU Status : HIGH
RAM Status : HIGH
Disk Status : NORMAL

AI Decision : POSSIBLE_ATTACK

Once this is working, Day 8 will introduce persistent logging with SQLite, so every decision and response is recorded for later analysis—an essential feature for any cybersecurity platform.
