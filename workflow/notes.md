ACTDRS Workflow
           Monitor
              │
              ▼
      Detect Suspicious Activity
              │
              ▼
      Analyze the Threat
              │
              ▼
     Decide the Best Action
              │
              ▼
      Respond Automatically
              │
              ▼
      Verify Recovery Success
              │
              ▼
      Learn & Improve

Let's go through each step.

Step 1: Monitor

The system continuously watches:

CPU
RAM
Disk
Processes
Services
Network
Login attempts
Logs

Example:

SSH Login Failed
CPU = 15%
Apache Running

At this point, no attack has been confirmed.

Step 2: Detect

Now the system asks:

"Is something abnormal happening?"

Example:

500 failed SSH logins

This is not normal.

Possible detection:

Possible Brute Force Attack

Detection is recognizing something unusual.

Step 3: Analyze

Now the AI asks:

What exactly is happening?

Example:

Failed logins

↓

Same IP

↓

500 attempts in 3 minutes

↓

Brute Force

It calculates:

Threat type
Severity
Confidence

Example:

Threat:
SSH Brute Force

Confidence:
96%
Step 4: Decide

Now the AI decides:

What should I do?

Decision examples:

If:

Apache stopped

Decision:

Restart Apache

If:

Malicious IP

Decision:

Block IP

If:

CPU = 100%

Decision:

Check running processes
Step 5: Respond (Self-Healing)

This is where self-healing begins.

Examples:

Restart service
Kill malicious process
Block IP
Disable compromised account
Quarantine a suspicious file
Notify administrator

This is the "healing" action.

Step 6: Verify

This is the most important step.

Example:

The system restarted Apache.

Now it asks:

Is Apache running now?

If yes:

Recovery Successful

If no:

Try another recovery

For example:

Restart failed

↓

Reload configuration

↓

Restart again

↓

Still failed

↓

Notify Admin

Without verification, the system cannot know whether it actually fixed the problem.

Step 7: Learn

Finally, the AI records:

Attack

↓

Solution

↓

Result

↓

Store Knowledge

Later, when a similar event occurs, it can respond faster or suggest a more appropriate action.

For a one-month project, this "learning" can be as simple as recording previous incidents and responses—it doesn't need to be a sophisticated machine learning system.

Example Scenario

Imagine someone tries to brute-force SSH.

Monitor
Watching SSH logs

↓

Detect
500 failed logins

↓

Analyze
Brute Force Attack
Confidence = 97%

↓

Decide
Block attacker IP

↓

Respond
Firewall blocks IP

↓

Verify
No more failed logins
SSH service healthy

↓

Learn
Store incident in database
Generate report
