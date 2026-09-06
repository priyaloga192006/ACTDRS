Day 11 goal

Right now we detect:

Destination port → SUSPICIOUS / NORMAL

That's too weak by itself.

Instead, we'll calculate a simple risk score from multiple signals:

Suspicious port       +30
External destination  +20
Unknown process       +20
Repeated connection   +30
                       ───
                       Risk Score

Then:

0–29   → LOW
30–59  → MEDIUM
60–100 → HIGH

This is not machine learning yet. It's a transparent rule-based risk engine. That's actually better at this stage because we can explain exactly why ACTDRS produced an alert.

Step 1 — Create the risk analyzer

In VS Code create:

ACTDRS/
└── analyzer/
    └── network_analyzer.py

If analyzer already exists, just create network_analyzer.py inside it.

Paste:

def calculate_network_risk(
    port_status,
    process_name,
    destination_ip
):

    risk_score = 0
    reasons = []

    # Suspicious destination port
    if port_status == "SUSPICIOUS":
        risk_score += 30
        reasons.append("Suspicious destination port")

    # Unknown process
    if process_name == "Unknown":
        risk_score += 20
        reasons.append("Unknown process")

    # External destination
    if destination_ip not in ["N/A", "127.0.0.1", "::1", "localhost"]:
        risk_score += 20
        reasons.append("External destination")

    # Determine risk level
    if risk_score >= 60:
        risk_level = "HIGH"

    elif risk_score >= 30:
        risk_level = "MEDIUM"

    else:
        risk_level = "LOW"

    return risk_score, risk_level, reasons

Save:

Ctrl + S

Step 2 — Test the analyzer

Don't connect it to the whole ACTDRS yet.

Run:

python3 -c "from analyzer.network_analyzer import calculate_network_risk; print(calculate_network_risk('NORMAL', 'firefox', '142.250.72.14')); print(calculate_network_risk('SUSPICIOUS', 'Unknown', '8.8.8.8'))"

You should get results similar to:

(20, 'LOW', ['External destination'])

and:

(70, 'HIGH', ['Suspicious destination port', 'Unknown process', 'External destination'])

The exact IP doesn't matter for this test.

Why this is important

We're changing the logic from:

4444 → ATTACK ❌

to:

4444
 +
Unknown process
 +
External destination
        ↓
Risk Score = 70
        ↓
HIGH

That's much more defensible.

And eventually we'll extend it with:

Repeated connection attempts
Unusual destination
Connection frequency
Known malicious indicators
Process behavior

before making a high-confidence response.

Your task

Create:

analyzer/network_analyzer.py

Paste the code, save it, and run the test command.

Send me the output. Then we'll connect this risk analyzer to network_monitor.py in Day 11 — Step 2.
