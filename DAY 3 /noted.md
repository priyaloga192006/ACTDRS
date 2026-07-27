Excellent. Before Day 3, here's where we should be:

✅ Ubuntu installed
✅ Python installed
✅ pip installed
✅ Git installed
✅ VS Code installed
✅ ACTDRS folder created
✅ Basic project folders created
✅ monitoring folder with Python files (or ready to create)
Day 3 Goal

Today we'll build the Monitoring Engine.

Until now, each monitor worked separately:

CPU Monitor

RAM Monitor

Disk Monitor

Process Monitor

A real cybersecurity system doesn't run four separate programs manually.

It runs one engine that monitors everything continuously.

Today's Architecture
             Monitoring Engine
                     │
      ┌──────────────┼──────────────┐
      │              │              │
 CPU Monitor    RAM Monitor   Disk Monitor
                     │
               Process Monitor
                     │
                     ▼
             Display Information

This is the heart of ACTDRS.

Step 1: Create a new file

Inside the monitoring folder, create:

system_monitor.py

Your folder becomes:

monitoring/

cpu_monitor.py

ram_monitor.py

disk_monitor.py

process_monitor.py

system_monitor.py
Step 2: First Version

Paste this code into system_monitor.py.

import psutil

print("========= ACTDRS System Monitor =========")

cpu = psutil.cpu_percent(interval=1)

memory = psutil.virtual_memory()

disk = psutil.disk_usage('/')

print(f"CPU Usage  : {cpu}%")

print(f"RAM Usage  : {memory.percent}%")

print(f"Disk Usage : {disk.percent}%")

Save it.

Step 3: Run it

Open Terminal.

Go to ACTDRS.

cd ~/ACTDRS

Run

python3 monitoring/system_monitor.py

Example output

========= ACTDRS System Monitor =========

CPU Usage : 14%

RAM Usage : 39%

Disk Usage : 58%

Congratulations 🎉

This is your first cybersecurity monitoring tool.

Step 4: Add Running Processes

Replace the program with this:

import psutil

print("========= ACTDRS =========")

cpu = psutil.cpu_percent(interval=1)

memory = psutil.virtual_memory()

disk = psutil.disk_usage('/')

print(f"CPU Usage : {cpu}%")

print(f"RAM Usage : {memory.percent}%")

print(f"Disk Usage : {disk.percent}%")

print("\nRunning Processes\n")

for process in psutil.process_iter(['pid','name']):

    print(process.info)

Now it shows

CPU
RAM
Disk
Running Processes
Step 5: Make It Continuous

Right now,

the program runs once and stops.

We want

Collect

↓

Wait

↓

Collect Again

↓

Wait

↓

Collect Again

Add

import time

Then

while True:

    cpu = psutil.cpu_percent(interval=1)

    memory = psutil.virtual_memory()

    disk = psutil.disk_usage('/')

    print("---------------------")

    print(cpu)

    print(memory.percent)

    print(disk.percent)

    time.sleep(3)

Now it refreshes every 3 seconds.

This is what SOC software does.

Step 6: Why are we doing this?

Imagine ransomware starts encrypting files.

Before ACTDRS can stop it,

it must notice

CPU ↑

Disk Activity ↑

Unknown Process ↑

Without monitoring,

AI has nothing to analyze.

So monitoring is the eyes of ACTDRS.

Step 7: Save Logs

Create a new folder (if it doesn't already exist):

logs/

Now modify the program so it writes monitoring data into a file like:

system.log

Example log:

2026-07-26 10:30:15

CPU : 18%

RAM : 42%

Disk : 58%

------------------

This is very important because later the AI will learn from these logs instead of only using live data.

Step 8: Project Progress

Current architecture:

ACTDRS

│

├── Monitoring Engine ✅

├── AI Engine ⏳

├── Decision Engine ⏳

├── Response Engine ⏳

├── Dashboard ⏳

└── Notification ⏳

We've completed approximately 20% of the core architecture.
