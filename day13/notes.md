ACTDRS — Day 13

Day 12 is complete. We now move into the next capability.

Day 13 Goal — Combine Multiple Risk Signals

So far, ACTDRS can independently detect:

CPU/RAM/Disk anomalies
        ↓
Network anomalies
        ↓
Suspicious ports
        ↓
Repeated connections
        ↓
Risk scores

The problem is that these signals are still somewhat separate.

A real SOC-style system should be able to say:

"Several suspicious signals are happening together, so the overall risk is higher."

Example

Imagine:

Process: unknown.exe

Network risk       = 40
Repeated connection = +30
Suspicious port     = +20
--------------------------------
Overall risk        = 90

ACTDRS can then classify the activity as:

90 → HIGH

This is the beginning of behavior correlation.

Day 13 — Step 1: Create the Risk Correlation Engine

Create this file:

ACTDRS/
└── analyzer/
    └── risk_correlator.py
Paste this entire program
# ============================================================
# ACTDRS - RISK CORRELATION ENGINE
# ============================================================


def calculate_overall_risk(
    network_risk,
    frequency_status,
    port_status
):

    # Start with the existing network risk
    overall_score = network_risk

    reasons = []


    # --------------------------------------------------------
    # CONNECTION FREQUENCY
    # --------------------------------------------------------

    if frequency_status == "SUSPICIOUS":

        overall_score += 20

        reasons.append(
            "High connection frequency"
        )


    # --------------------------------------------------------
    # SUSPICIOUS PORT
    # --------------------------------------------------------

    if port_status == "SUSPICIOUS":

        overall_score += 20

        reasons.append(
            "Suspicious destination port"
        )


    # --------------------------------------------------------
    # LIMIT SCORE TO 100
    # --------------------------------------------------------

    if overall_score > 100:

        overall_score = 100


    # --------------------------------------------------------
    # DETERMINE OVERALL RISK LEVEL
    # --------------------------------------------------------

    if overall_score >= 70:

        risk_level = "HIGH"

    elif overall_score >= 40:

        risk_level = "MEDIUM"

    else:

        risk_level = "LOW"


    # --------------------------------------------------------
    # RETURN RESULT
    # --------------------------------------------------------

    return overall_score, risk_level, reasons
What this module does

It combines three signals:

Signal	Meaning
network_risk	Existing network risk score
frequency_status	Whether repeated connections are suspicious
port_status	Whether destination port is suspicious

For example:

Network risk = 40
Frequency    = SUSPICIOUS
Port         = SUSPICIOUS

The engine calculates:

40
+20 frequency
+20 port
---------
80 HIGH

So instead of looking at one indicator independently, ACTDRS starts correlating multiple indicators.

Why are we doing this?

This is an important distinction:

Before Day 13
Network connection
       ↓
Risk = 40
After Day 13
Network connection
       ↓
Network risk = 40
       +
Repeated connection
       +
Suspicious port
       ↓
Overall risk = 80
       ↓
HIGH

That's much closer to how a security analytics system should reason.

Day 13 — Step 2

Don't connect this to network_monitor.py yet.

First test the new module independently.

From:

~/ACTDRS

run:

python3 -c "from analyzer.risk_correlator import calculate_overall_risk; print(calculate_overall_risk(20, 'NORMAL', 'NORMAL')); print(calculate_overall_risk(40, 'SUSPICIOUS', 'NORMAL')); print(calculate_overall_risk(60, 'SUSPICIOUS', 'SUSPICIOUS'))"

You should get results similar to:

(20, 'LOW', [])
(60, 'MEDIUM', ['High connection frequency'])
(100, 'HIGH', ['High connection frequency', 'Suspicious destination port'])

The exact output should follow the code above.

⚠️ One important thing

Don't modify your existing:

network_monitor.py
connection_tracker.py
network_analyzer.py
database_manager.py

yet.

We're following the same safe approach we used earlier:

Create → Test independently → Integrate → Test → Database → GitHub

Your current position
Day 12 ✅
   ↓
Day 13
   ↓
Step 1 — Create risk_correlator.py  ← DONE after pasting
   ↓
Step 2 — Independent test            ← YOU ARE HERE
   ↓
Step 3 — Integrate with network_monitor.py
   ↓
Step 4 — Security-event correlation
   ↓
Step 5 — End-to-end test
   ↓
Step 6 — GitHub

Run the one test command above and send me the output.
