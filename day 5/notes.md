Day 5 Goal

Yesterday we created:

detection/
    threat_detector.py

It can identify whether CPU, RAM, or Disk usage is normal or high.

Today we'll connect it with the Monitoring Engine.

What happens today?

Currently:

CPU = 96%

↓

Display 96%

After Day 5:

CPU = 96%

↓

Threat Detector

↓

HIGH

↓

Generate Alert
Step 1

Open

monitoring/system_monitor.py
Step 2

At the top of the file, below the existing imports, add:

from detection.threat_detector import (
    detect_cpu,
    detect_ram,
    detect_disk
)

Your imports should now look like:

import psutil
import time

from detection.threat_detector import (
    detect_cpu,
    detect_ram,
    detect_disk
)
Step 3

Find this section:

print(f"CPU Usage : {cpu}%")

Replace it with:

cpu_status = detect_cpu(cpu)

print(f"CPU Usage : {cpu}%")
print(f"CPU Status : {cpu_status}")

Now replace

print(f"RAM Usage : {memory.percent}%")

with

ram_status = detect_ram(memory.percent)

print(f"RAM Usage : {memory.percent}%")
print(f"RAM Status : {ram_status}")

Replace

print(f"Disk Usage : {disk.percent}%")

with

disk_status = detect_disk(disk.percent)

print(f"Disk Usage : {disk.percent}%")
print(f"Disk Status : {disk_status}")
Step 4

Now add alerts.

Immediately after:

print(f"CPU Status : {cpu_status}")

add:

if cpu_status == "HIGH":
    print("🚨 ALERT : High CPU Usage Detected!")

Do the same for RAM:

if ram_status == "HIGH":
    print("🚨 ALERT : High RAM Usage Detected!")

And for Disk:

if disk_status == "HIGH":
    print("🚨 ALERT : Disk Almost Full!")
What happens now?

Suppose:

CPU = 18%

Output:

CPU Usage : 18%
CPU Status : NORMAL

Suppose:

CPU = 97%

Output:

CPU Usage : 97%
CPU Status : HIGH

🚨 ALERT : High CPU Usage Detected!

Now system_monitor.py is no longer making decisions.

Instead:

system_monitor.py
        │
        ▼
threat_detector.py
        │
        ▼
HIGH / NORMAL
        │
        ▼
Display Alert

This separation is exactly how professional software is designed.

Why is this important?

Right now, threat_detector.py only uses simple rules:

if cpu > 90:
    return "HIGH"

Later, we can replace only this file with an AI model:

CPU
RAM
Disk
Processes
Logs
        │
        ▼
Machine Learning Model
        │
        ▼
Threat Probability = 94%

Notice something important:

We won't need to rewrite system_monitor.py.

Only the detection engine changes.

This is called modular architecture, and it's one of the main reasons we separated monitoring and detection into different modules.
