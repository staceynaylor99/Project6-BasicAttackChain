Basic Attack Chain Detection (Project 6)

This project demonstrates how a simple attacker sequence appears in Windows logs.
It includes:

 1. Failed Logon Attempts

Captured using Windows Security Log (Event ID 4625).
Shows brute-force or password-guessing style failures.

 2. Enumeration

Commands like whoami and net user captured by Sysmon (Event ID 1).
Represents attacker recon on the local system.

 3. Privilege Escalation Attempt

net.exe localgroup administrators attackerUser /add
Shows a clear attempt to add a new admin user.

Files Included
DetectionNotes.md

Full analysis, written in plain language.

/screenshots

failed-logon.png

recon.png

privilege-escalation.png

Skills Demonstrated

Windows Event Log analysis

Sysmon investigation

Identifying attacker behaviors

Documenting findings like a SOC analyst

Mapping actions to ATT&CK techniques
