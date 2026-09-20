Day 14 — Step 1: Unified Threat Classification
Right now you can detect:
CPU       → HIGH
RAM       → NORMAL
Disk      → NORMAL
Port      → SUSPICIOUS
Frequency → SUSPICIOUS
But ACTDRS needs to answer:
"What kind of threat does all this evidence represent, and how severe is it?"

So we'll create:
ACTDRS/
└── detection/
    ├── threat_detector.py       ← KEEP EXISTING
    └── threat_classifier.py     ← NEW
The relationship becomes:
CPU/RAM/Disk ──────────┐
                       │
Suspicious Port ───────┤
                       ├──→ Threat Classifier
Connection Frequency ──┤
                       │
Overall Risk ──────────┘
                              ↓
                       Threat Type
                              ↓
                         Severity
1. Create the new file
Run:
cd /home/priya/ACTDRS
nano detection/threat_classifier.py
Paste this entire program:
# ============================================================
# ACTDRS - UNIFIED THREAT CLASSIFIER
# ============================================================


def classify_threat(
    cpu_status,
    ram_status,
    disk_status,
    port_status,
    frequency_status,
    overall_score
):
    """
    Combine multiple security signals and classify
    the overall threat condition.
    """

    reasons = []
    threat_type = "NORMAL"
    severity = "LOW"

    # --------------------------------------------------------
    # SYSTEM RESOURCE THREATS
    # --------------------------------------------------------

    if cpu_status == "HIGH":
        reasons.append("High CPU usage")

    if ram_status == "HIGH":
        reasons.append("High RAM usage")

    if disk_status == "HIGH":
        reasons.append("High disk usage")


    # --------------------------------------------------------
    # NETWORK THREATS
    # --------------------------------------------------------

    if port_status == "SUSPICIOUS":
        reasons.append("Suspicious destination port")

    if frequency_status == "SUSPICIOUS":
        reasons.append("Repeated network connection")


    # --------------------------------------------------------
    # THREAT TYPE
    # --------------------------------------------------------

    if port_status == "SUSPICIOUS" and frequency_status == "SUSPICIOUS":

        threat_type = "SUSPICIOUS_NETWORK_ACTIVITY"

    elif port_status == "SUSPICIOUS":

        threat_type = "SUSPICIOUS_NETWORK_PORT"

    elif frequency_status == "SUSPICIOUS":

        threat_type = "REPEATED_NETWORK_ACTIVITY"

    elif (
        cpu_status == "HIGH"
        or ram_status == "HIGH"
        or disk_status == "HIGH"
    ):

        threat_type = "ABNORMAL_SYSTEM_ACTIVITY"

    else:

        threat_type = "NORMAL_ACTIVITY"


    # --------------------------------------------------------
    # SEVERITY
    # --------------------------------------------------------

    if overall_score >= 70:

        severity = "HIGH"

    elif overall_score >= 40:

        severity = "MEDIUM"

    else:

        severity = "LOW"


    # --------------------------------------------------------
    # RETURN CLASSIFICATION
    # --------------------------------------------------------

    return threat_type, severity, reasons

Save:
Ctrl + O → Enter → Ctrl + X
2. Test the new classifier
Run:
python3 -c "from detection.threat_classifier import classify_threat; print(classify_threat('NORMAL','NORMAL','NORMAL','NORMAL','NORMAL',20)); print(classify_threat('NORMAL','NORMAL','NORMAL','SUSPICIOUS','NORMAL',50)); print(classify_threat('HIGH','NORMAL','NORMAL','SUSPICIOUS','SUSPICIOUS',90))"
You should get approximately:
('NORMAL_ACTIVITY', 'LOW', [])
('SUSPICIOUS_NETWORK_PORT', 'MEDIUM', ['Suspicious destination port'])
('SUSPICIOUS_NETWORK_ACTIVITY', 'HIGH', ['High CPU usage', 'Suspicious destination port', 'Repeated network connection'])
Why this layer matters
Your existing threat_detector.py answers individual questions:
"Is the CPU high?"

"Is this port suspicious?"

The new threat_classifier.py answers the larger question:
"Given all the evidence, what security condition is occurring?"

That's why we're adding it rather than replacing your existing detector.
