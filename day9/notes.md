Great. Day 9 starts now.

Today we are going to upgrade ACTDRS from simply recording system numbers to recording security events.

Day 9 goal

Right now your database records:

CPU | RAM | Disk | Decision

For example:

21.3 | 64.8 | 34.9 | NORMAL

But a cybersecurity system needs to know what happened.

Eventually we want records like:

Time | Event | Severity | Details | Decision
------------------------------------------------
20:30 | HIGH_CPU | MEDIUM | CPU 92% | CHECK_CPU
20:31 | SUSPICIOUS_PROCESS | HIGH | unknown process | POSSIBLE_ATTACK
20:32 | NETWORK_SCAN | HIGH | many ports contacted | BLOCK

We are not building all of that today. We'll build it step by step.

DAY 9 — STEP 1: Design the Security Event Table

First, we need a separate table in SQLite.

Your existing table:

system_logs

will remain untouched.

We'll create a new table:

security_events

It will eventually contain:

id
timestamp
event_type
severity
description
decision
Why a separate table?

Because these are different types of information:

System logs

"CPU was 21%."

Security events

"CPU exceeded the suspicious threshold."

That separation will become useful later when we build the dashboard and threat investigation system.

Step 1A — Open the database file

In VS Code:

ACTDRS
 └── database
      └── database_manager.py

Open:

database_manager.py

Do NOT delete your existing code.

At the bottom of the file, after your existing insert_log() function, we're going to add a new function.

Paste this:

def create_security_events_table():

    conn = sqlite3.connect(DB_NAME)
    cursor = conn.cursor()

    cursor.execute("""
        CREATE TABLE IF NOT EXISTS security_events(
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            timestamp TEXT,
            event_type TEXT,
            severity TEXT,
            description TEXT,
            decision TEXT
        )
    """)

    conn.commit()
    conn.close()
Step 1B — What this function does

When ACTDRS calls:

create_security_events_table()

SQLite creates:

security_events

with these columns:

Column	Purpose
id	Unique event number
timestamp	When event happened
event_type	Type of security event
severity	LOW / MEDIUM / HIGH
description	What happened
decision	What ACTDRS decided

For example:

1 | 20:35:12 | HIGH_CPU | MEDIUM | CPU reached 92% | CHECK_CPU
Step 1C — Save

Press:

Ctrl + S

That's all we change for this step.

Important

Do NOT modify system_monitor.py yet.

We first need to make sure the new table can be created successfully.

Step 2 — Test the function

We need to call the function once.

In the Ubuntu terminal, make sure you're at:

/home/priya/ACTDRS

You can check:

pwd

Then run:

python3 -c "from database.database_manager import create_security_events_table; create_security_events_table(); print('Security events table created successfully')"
Expected output
Security events table created successfully

If you get that, don't do anything else yet.

Send me the terminal output/screenshot.

Then I'll give you Day 9 Step 2, where we verify that security_events actually exists in actdrs.db.

We are deliberately doing this one step at a time so we don't break the working Day 8 system.
