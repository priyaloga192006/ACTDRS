Day 2 Goal

By the end of today, your project should:

✅ Monitor CPU usage
✅ Monitor RAM usage
✅ Monitor Disk usage
✅ Monitor Running Processes
✅ Display everything in real time
✅ Organize the project professionally
Step 1: ACTDRS Architecture

Today we'll implement only the first block.

            ACTDRS

        User Dashboard
              ▲
              │
      Response Engine
              ▲
              │
      AI Decision Engine
              ▲
              │
      Threat Detection
              ▲
              │
      System Monitoring   ← Today

Without System Monitoring, nothing above it can work.

Step 2: Project Structure

Inside your ACTDRS folder, create this structure:

ACTDRS/
│
├── app.py
├── requirements.txt
│
├── monitoring/
│      ├── cpu_monitor.py
│      ├── ram_monitor.py
│      ├── disk_monitor.py
│      └── process_monitor.py
│
├── logs/
│
├── config/
│
├── ai/
│
├── response/
│
└── dashboard/

Create the folders:

mkdir monitoring
mkdir ai
mkdir response
mkdir dashboard
Step 3: Install Required Libraries

Run these commands in the Ubuntu terminal (with your virtual environment activated):

pip install psutil

Then:

pip install flask

Then:

pip install pandas

We'll add machine learning libraries later when we actually build the AI model.

Step 4: Create the CPU Monitor

Create:

monitoring/cpu_monitor.py

Paste:

import psutil

cpu = psutil.cpu_percent(interval=1)

print(f"CPU Usage : {cpu}%")

Run:

python monitoring/cpu_monitor.py

Example output:

CPU Usage : 14%
Step 5: RAM Monitor

Create:

monitoring/ram_monitor.py
import psutil

memory = psutil.virtual_memory()

print(f"RAM Usage : {memory.percent}%")

Run:

python monitoring/ram_monitor.py

Example:

RAM Usage : 38%
Step 6: Disk Monitor

Create:

monitoring/disk_monitor.py
import psutil

disk = psutil.disk_usage('/')

print(f"Disk Usage : {disk.percent}%")

Example:

Disk Usage : 57%
Step 7: Process Monitor

Create:

monitoring/process_monitor.py
import psutil

for process in psutil.process_iter(['pid','name']):
    print(process.info)

Example:

{'pid':1,'name':'systemd'}
{'pid':945,'name':'firefox'}
{'pid':1124,'name':'python3'}
...

This gives ACTDRS visibility into what is running on the system.

Step 8: Why are we doing this?

Imagine ransomware starts encrypting files.

Before ACTDRS can respond, it must first notice unusual behavior.

It might observe:

CPU suddenly jumps from 10% to 95%
A previously unseen process starts running
RAM usage spikes
Disk activity becomes unusually high

These observations become the features that later feed into the AI model.
