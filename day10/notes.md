🔐 ACTDRS — Day 10
Network Connection Monitoring
Today's goal

We want ACTDRS to see active network connections and capture:

Process
   ↓
Protocol
   ↓
Source IP
Source Port
   ↓
Destination IP
Destination Port

For example:

TCP
192.168.1.10:52341
        ↓
142.250.72.14:443

Meaning:

Source IP = your machine's IP
Source Port = temporary port used by your machine
Destination IP = remote machine/server
Destination Port = service being contacted
443 = HTTPS
Step 1 — Check your current project

You already have this structure:

ACTDRS/
├── ai/
├── analyzer/
├── collector/
├── config/
├── dashboard/
├── database/
├── detection/
├── docs/
├── logs/
├── monitoring/
├── reports/
├── response/
├── tests/
└── actdrs.db

We're going to create a network monitoring module rather than putting everything into system_monitor.py.

Go to:

ACTDRS/monitoring/

Create a new file:

network_monitor.py
Step 2 — Put this code in network_monitor.py
import psutil


def monitor_network_connections():

    connections = psutil.net_connections(kind="inet")

    print("\n===== NETWORK CONNECTIONS =====")

    for connection in connections:

        if connection.laddr:

            source_ip = connection.laddr.ip
            source_port = connection.laddr.port

        else:
            source_ip = "N/A"
            source_port = "N/A"

        if connection.raddr:

            destination_ip = connection.raddr.ip
            destination_port = connection.raddr.port

        else:
            destination_ip = "N/A"
            destination_port = "N/A"

        print("--------------------------------")
        print(f"Status          : {connection.status}")
        print(f"Source IP       : {source_ip}")
        print(f"Source Port     : {source_port}")
        print(f"Destination IP  : {destination_ip}")
        print(f"Destination Port: {destination_port}")
        print(f"PID             : {connection.pid}")

Save:

Ctrl + S

Step 3 — Test only this module

Don't connect it to your main ACTDRS yet.

From:

~/ACTDRS

run:

python3 -c "from monitoring.network_monitor import monitor_network_connections; monitor_network_connections()"

You should get something resembling:

===== NETWORK CONNECTIONS =====
--------------------------------
Status          : ESTABLISHED
Source IP       : 127.0.0.1
Source Port     : 54321
Destination IP  : 142.250.x.x
Destination Port: 443
PID             : 1234

You may also see:

LISTEN
TIME_WAIT
NONE
N/A

That's normal.

Don't worry if you don't see exactly the same IPs or ports.

Your connections depend on what programs are currently communicating over the network.

Step 4 — Important security point

Do not assume every external connection is an attack.

For example:

Destination Port: 443

does not mean malicious.

Port 443 normally means HTTPS.

Likewise, seeing an unfamiliar destination IP doesn't automatically mean an attack.

Our ACTDRS will eventually need to analyze behavior, not simply say:

"Unknown IP = attack."

That's an important distinction between a useful security-monitoring project and a toy project.

Step 5 — What we're building toward

Today's module is only the visibility layer:

             NETWORK
                │
                ▼
       psutil.net_connections()
                │
                ▼
        Network Monitor
                │
       ┌────────┼────────┐
       ▼        ▼        ▼
    Source   Destination  PID
      IP         IP
      │          │
    Port        Port
       └────┬─────┘
            ▼
       Network Event
            ▼
      Threat Analysis
            ▼
       AI Decision
            ▼
     Security Event DB

We're deliberately doing this in stages.

Do not modify system_monitor.py yet.

Your only task now

Create:

monitoring/network_monitor.py

Paste the code above and run:

python3 -c "from monitoring.network_monitor import monitor_network_connections; monitor_network_connections()"

Send me the terminal output.

I'll check it, then we'll do Day 10 — Step 2: identify which process owns each network connection.
