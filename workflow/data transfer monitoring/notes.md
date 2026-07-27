First, let's see how data travels.

Suppose:

Computer A  ───────────────►  Computer B

You send a PDF file.

The file is not sent all at once.

It is divided into many packets.

Example:

Packet 1
Packet 2
Packet 3
Packet 4
Packet 5
...

Each packet travels through:

Computer A

↓

Router

↓

ISP

↓

Internet

↓

Router

↓

Computer B
Where can an attacker attack?

There are several possibilities.

Case 1: The attacker attacks Computer A

Example:

Malware steals the file before it is sent.

ACTDRS on Computer A can detect:

Suspicious process
Unknown application reading many files
Unusual outbound connections

Possible response:

Kill the malicious process.
Alert the user.
Block the network connection.
Case 2: The attacker intercepts packets while they travel

Imagine:

Computer A

↓

Internet

↓

💀 Hacker

↓

Computer B

This is called a Man-in-the-Middle (MITM) attack.

Now your question is:

Can ACTDRS stop this?

The answer depends on where ACTDRS is installed.

Scenario A

ACTDRS is installed only on Computer A.

Computer A (ACTDRS)

↓

Internet

↓

Hacker

↓

Computer B

Here, ACTDRS cannot see what happens in the middle of the Internet.

It can only observe what leaves Computer A.

Scenario B

ACTDRS is installed on both computers.

Computer A (ACTDRS)

↓

Internet

↓

Hacker

↓

Computer B (ACTDRS)

Now:

Computer A can monitor what it sends.

Computer B can verify what it receives.

If:

packets are modified,
or unexpected behavior occurs,

Computer B can detect inconsistencies.

However, it still cannot see the attacker directly unless there is additional network monitoring.

Scenario C (This is the advanced version)

Install ACTDRS on the network gateway or firewall.

Computer A

↓

Switch

↓

Router

↓

ACTDRS Gateway

↓

Internet

↓

Computer B

Now ACTDRS can inspect network traffic before it leaves or enters the network.

This is much more powerful.

What about milliseconds?

You asked:

"Data transfers in milliseconds. Is that enough time for a hacker?"

Good observation.

The answer is:

Modern cybersecurity systems don't wait until the entire file is transferred.

They inspect traffic continuously.

Imagine packets like this:

Packet 1

↓

Analyze

↓

Packet 2

↓

Analyze

↓

Packet 3

↓

Analyze

Detection happens while traffic is flowing, not only after the transfer finishes.

Can AI inspect every packet?

For a small network or a college project:

Yes, if you limit the amount of traffic.

For example:

Monitor only SSH.
Monitor only HTTP.
Monitor only FTP.
Monitor only your lab network.

For a large enterprise with thousands of devices:

Analyzing every packet with AI in real time becomes very expensive. Commercial systems use specialized hardware, optimized software, and layered detection instead of sending every packet through a large AI model.

What should ACTDRS actually detect?

There are two different things.

Host-based threats

These happen on the computer.

Examples:

Malware starts.
SSH brute force.
Suspicious process.
High CPU usage.
Unauthorized file access.

ACTDRS can monitor these directly.

Network-based threats

These happen while data is traveling.

Examples:

Port scanning.
DDoS.
Man-in-the-middle attempts.
Packet flooding.
Suspicious connections.

To detect these, ACTDRS needs a network monitoring component that captures and analyzes network traffic.

I think your project should evolve

Originally, we described ACTDRS like this:

Linux

↓

Logs

↓

AI

↓

Alert

After your question, I'd improve the architecture.

                Linux Computer

        ┌────────────┬────────────┐
        │            │            │
        ▼            ▼            ▼
    System Logs   Processes   Network Packets
        │            │            │
        └────────────┴────────────┘
                     │
             AI Detection Engine
                     │
          Detect + Analyze + Decide
                     │
         ┌───────────┴───────────┐
         ▼                       ▼
  Host Response          Network Response
         │                       │
 Kill Process             Block IP / Alert

Now your project protects both the host and its network activity, which is much closer to how real endpoint detection and response (EDR) and network detection and response (NDR) systems work.

One final point

You asked whether ACTDRS should detect only computer threats or also attacks during data transfer.

The answer is:

A host-only ACTDRS protects the computer itself.
An ACTDRS with network monitoring can also analyze traffic flowing into and out of the computer.
An ACTDRS installed at a gateway can inspect traffic for multiple devices on a network.

For your project, I recommend the second approach: build ACTDRS as a host-based system with integrated network monitoring. That is technically achievable for a college project, demonstrates both Linux and networking skills, and answers exactly the concern you raised about threats during data transfe
