🎯 Day 8 Goal

Store every scan permanently:

Time
CPU usage
RAM usage
Disk usage
AI decision

So later you can answer:

When did the attack happen?
How many alerts occurred today?
Which system had the highest CPU usage?
What actions were taken?

This is essential for SIH because judges expect a cybersecurity product to maintain audit logs.

Current Architecture
from database.database_manager import (
    create_database,
    insert_log
)
Day 8 Architecture
create_database()
Step 1: Create Database Folder

Inside ACTDRS create:

print("======= ACTDRS System Monitor =======")

create_database()

while True:
Step 2: Create Database File

Inside database/ create:

print(f"\nAI Decision : {decision}")

Your structure becomes:

insert_log(
    cpu,
    memory.percent,
    disk.percent,
    decision
)
Step 3: Paste This Code

Open database_manager.py and paste:

python3 monitoring/system_monitor.py

Save it.

What does this do?
create_database()

Creates a database file:

Ctrl + C

and a table:

Column	Purpose
id	Unique record number
timestamp	Time of scan
cpu	CPU usage
ram	RAM usage
disk	Disk usage
decision	AI decision
insert_log(...)

Adds one new row every time ACTDRS scans the system.

Example:

Time	CPU	RAM	Decision
2026-07-29 09:15:01	22	41	NORMAL
2026-07-29 09:15:05	95	92	POSSIBLE_ATTACK
Step 4: Connect Database to ACTDRS

Open:

ls

Add this import near the top:

actdrs.db
Step 5: Initialize Database

Add this line before while True::

sqlite3 actdrs.db

Example:

SELECT * FROM system_logs;

This creates the database automatically when ACTDRS starts.

Step 6: Save Logs Every Scan

After this line:

1|2026-07-29 09:15:01|22.0|41.0|35.0|NORMAL
2|2026-07-29 09:15:05|95.0|92.0|35.0|POSSIBLE_ATTACK

add:

.exit
Final Flow
sudo apt install sqlite3 -y
Step 7: Run the Program

In the terminal:

sqlite3 actdrs.db "SELECT * FROM system_logs;"

Let it run for 15–20 seconds, then stop with:

Step 8: Verify Database Was Created

In the terminal:

You should see:

✅ This means SQLite is working.

Step 9: View the Stored Logs

Run:

Inside SQLite type:

You should see rows similar to:

To exit:

If sqlite3 command is not found

Install it once:

What Have You Built Today?

You now have a real forensic logging system.

Your ACTDRS can answer questions such as:

What happened at 2 PM?
How many attacks occurred today?
Which scans were suspicious?
What was the AI decision history?

This is a feature many student projects miss.
