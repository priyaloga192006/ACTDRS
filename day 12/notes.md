We move to Day 12 — Repeated Network Connection Detection.

This is an important improvement. Right now ACTDRS evaluates connections individually:

Connection → Risk Score

But real suspicious behavior can be about frequency:

Same process
      ↓
Same destination
      ↓
Many connections in a short period
      ↓
Potentially suspicious behavior
Day 12 — Step 1: Create the connection tracker

Create this file:

ACTDRS/
└── analyzer/
    └── connection_tracker.py

Paste this entire code:

from collections import defaultdict
from datetime import datetime, timedelta


# Store connection timestamps
connection_history = defaultdict(list)


def track_connection(process_name, destination_ip):

    current_time = datetime.now()

    key = (process_name, destination_ip)

    # Add current connection
    connection_history[key].append(current_time)

    # Keep only connections from the last 60 seconds
    cutoff_time = current_time - timedelta(seconds=60)

    connection_history[key] = [
        timestamp
        for timestamp in connection_history[key]
        if timestamp >= cutoff_time
    ]

    connection_count = len(connection_history[key])

    # Determine whether connection frequency is suspicious
    if connection_count >= 10:

        status = "SUSPICIOUS"

    else:

        status = "NORMAL"

    return connection_count, status
What this does

For each:

Process + Destination IP

it remembers how many connections occurred during the last 60 seconds.

For example:

Firefox → Google
1 connection   → NORMAL
2 connections  → NORMAL
5 connections  → NORMAL
9 connections  → NORMAL
10+ connections → SUSPICIOUS

This is still a rule-based prototype, not machine learning. We're deliberately building the behavior-analysis foundation first.

Step 2 — Test it independently

Don't modify network_monitor.py yet.

Run:

python3 -c "from analyzer.connection_tracker import track_connection; print(track_connection('firefox','8.8.8.8')); print(track_connection('firefox','8.8.8.8'))"

You should get something similar to:

(1, 'NORMAL')
(2, 'NORMAL')

That proves the tracker is remembering previous connections.

Stop here.

Run that command and send me the output. Then we'll connect the tracker to your live network_monitor.py without breaking the code we've already completed.
