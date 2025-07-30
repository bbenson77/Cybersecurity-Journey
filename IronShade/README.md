# IronShade - TryHackMe Challenge
**Objective**: Documenting my analysis and learnings from the IronShade room.

## Task 1 - Identify Machine ID

- **Command used**:
- ```bash
  cat /etc/machine-id

  Output:
  dc7c8ac5c09a4bbfaf3d09d399f10d96

What I learned:
Every Linux system has a unique ID stored at etc/machine-id

![machine-id](./machine-id.png)

## Task 2 - Identify Backdoor User Account
- **Command used**:
- ```bash
  cat /etc/passwd

What I was looking for:
I reviewed all user accounts defined on the system to identify any unusual or suspicious users.

What I found:
A suspicious user account named microservice existed, which stands out since it's not part of the default Linux users.

![backdoor-user](./backdoor-user.png)

What I Learned:
The etc/passwd file contains all user accounts on a Linux system. Attackers often create new accounts for persistence, and spotting unfamiliar names like microservice can reveal backdoors.

## Task 3 - Detect Malicious Cronjob
- **Command used**:
- ```bash
  sudo cat /var/spool/cron/crontabs/root

![cronjob-root-persistence](./cronjob-root-persistence.png)

What I was looking for:
I reviewed scheduled tasks under root to identify any jobs set to automatically execute malicious scripts at boot, which is a known attacker tactic for persistence.

What I found:
A suspicious cronjob set to execute /home/mircroservice/printer_app every time the machine reboots, likely tied to the mircroservice backdoor user.

## Task 4 - Identify Hidden Process
- **Command Used**:
- ```bash
  ps aux | grep -E "/tmp|/dev|/home/mircroservice|\.\/|printer"

![suspicious-process](./suspicious-process-strokes.png)

What I was looking for:
I scanned all active processes running from suspicious locations (like /tmp, /home/mircroservice) that might belong to a backdoor account or be disguised as a system process.

What I found:
A process named .strokes running from a hidden directory:
/home/mircroservice/.tmp/.strokes

## Task 5 - Count Processes from Backdoor Directory
- **Command Used**:
- ```bash
  ps aux | grep -E "/tmp|/dev|/home/mircroservice|\.\/|printer"

![suspicious-process](./suspicious-process-strokes.png)

What I was looking for:
I searched all active processes to determine how many were running from within the backdoor user’s home directory: /home/mircroservice.

What I found:
Two suspicious processes:

/home/mircroservice/.tmp/.strokes

/home/mircroservice/printer_app

Both were owned by root and running from a non-standard, attacker-created directory.

Correct answer: 2

## Task 6 - Identify Hidden File from the Root (/) Directory 
- **Command Used**:
- ```bash
  sudo ls -la /

![hidden-systmdfile](./hidden-systemd-file-in-root-dir.png)

What I was looking for:
I searched for hidden files located directly under the root of the filesystem (/) — not the /root user's home directory. These files can signal stealthy attacker implants or dropped payloads.

What I found:
A file named .systemd, which is attempting to blend in with legitimate system services. Its location in / and hidden nature is highly suspicious.

## Task 7 - Identify Suspicious Services 
- **Command used**:
- ```bash
  ls /sys/fs/cgroup/systemd/system.slice/

  
![services-in-system](./services-in-system-slice.png)

What I was looking for:
I inspected services listed in the system.slice cgroup, which may include stealthy or manually dropped service files that don’t appear via standard systemctl listing.

What I found:
Two suspicious services:

backup.service – Not a legitimate system component and likely used to disguise a backdoor.

strokes.service – Matches the previously identified .strokes binary used for persistence.

These services do not belong to standard Linux operations and are likely attacker-created.

Correct answer: 
backup.service, strokes.service


