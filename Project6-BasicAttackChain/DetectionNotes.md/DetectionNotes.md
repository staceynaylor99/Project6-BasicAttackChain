Detection Notes – Basic Attack Chain Investigation (Failed Logon → Enumeration → Priv-Escalation)
Overview

This small investigation shows how Windows logs can reveal an attacker’s early steps on a machine.
I simulated three things:

Failed logon attempts

User enumeration using whoami and net user

A privilege escalation attempt using net.exe localgroup administrators attackerUser /add

These steps mimic a basic attack chain that SOC analysts look for.

1. Failed Logon (Event ID 4625)

Windows Security logs recorded multiple failed logon attempts.

Key details

Event ID: 4625

Failure Reason: Unknown username or bad password

User targeted: staceynaylor

Process: svchost.exe

Source Address: 127.0.0.1 (local machine)

Why it matters

Repeated failed logons can be signs of:

password guessing

brute-force attempts

a user struggling to authenticate

malware probing accounts

It is often the first signal something suspicious is happening.

2. Enumeration Activity (Sysmon Event ID 1 – ProcessCreate)

Sysmon recorded commands used to gather system information.

Commands captured

whoami.exe

net.exe user

Key log details

Parent process: powershell.exe

User: staceynaylor

CommandLine values showed exactly what was run

Hashes captured for executable integrity

Why it matters

These commands are commonly used by attackers to learn:

which account they’re operating under

what users exist

potential privilege paths

They are normal admin tools — but also common recon tools in attacks.

3. Privilege Escalation Attempt (Sysmon Event ID 1)

I ran a command to simulate an attacker trying to elevate privileges:

net.exe localgroup administrators attackerUser /add

Sysmon showed

Process: C:\Windows\System32\net.exe

CommandLine: clearly showing the user addition attempt

Parent process: powershell.exe

Integrity Level: Medium

User: staceynaylor

Why it matters

This event is extremely important because:

Attackers often add themselves to “Administrators”

This gives full system control

Sysmon showing the exact command makes the activity easy to detect

This single log would typically trigger alerts in real SOC environments.

Conclusion

This simulation created a small but realistic attack chain:

Failed attempts to access an account

Information gathering via normal Windows tools

A privilege escalation attempt using net.exe

A SOC analyst would look for this sequence to detect early-stage compromise.

This project builds skills in:

reading Windows Security logs

analyzing Sysmon process activity

understanding how normal tools become attacker tools

writing clear detection notes