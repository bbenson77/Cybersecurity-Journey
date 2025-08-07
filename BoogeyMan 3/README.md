# Boogeyman 3 - TryHackMe Challenge

## Objective:
Documenting my full investigation of the **initial payload execution** in the Boogeyman 3 challenge, while building real-world SOC skills using Kibana and Winlogbeat data.

---

## 🧠 Task 1 - Identify the Process That Executed the Initial Stage 1 Payload

**Command / Query Used:**
```kql
process.parent.name: "explorer.exe"
```

![task1](./Screenshots/task1.png)  ![task1answer](./Screenshots/task1correct.png)

 The Struggle:
At first, I thought the answer would obviously be:

A PowerShell command using -enc or -c

Or setup.exe launched from the .iso

Or even the RAT tool screenconnect.windowsclient.exe

I tested 6+ different PIDs including:

7116 (from fodhelper.exe)

1600 (for setup.exe)

1752 (screenconnect)

... all of them were wrong.

So I went back to square one and thought:
"What would a real attacker use to disguise a script-based payload?"

That’s when I remembered mshta.exe from a previous TryHackMe room.

## Task 2 - Find the Full Command-Line Value of the Stage 1 Payload Execution

**Command/Query Used:**
```kql
process.parent.pid: 6392

```
![task2](./Screenshots/task2.png)  ![task2answer](./Screenshots/task2correct.png)

 What I Did:
I knew from Task 1 that the process responsible for stage 1 execution was mshta.exe and its PID was 6392.

To investigate what child processes it launched and how, I filtered the dataset using:
process.parent.pid: 6392

This shows any command that was directly launched by mshta.exe.
My Reasoning:
When I saw the TryHackMe question ask about a command "attempting to implant a file to another location," I knew it had to involve a tool like xcopy.exe, robocopy, or something used for data movement.

One entry stood out: "C:\Windows\System32\xcopy.exe" /s /i /e /h D:\review.dat C:\Users\EVAN~1.HUT\AppData\Local\Temp\review.dat

I recognized:

xcopy.exe = used to copy files and directories

/s /i /e /h = common switches attackers use for stealthy recursive file copying

review.dat = suspicious, likely payload or extracted data

EVAN~1.HUT = 8.3 short filename for EVAN-HUTCHINSON, showing it’s a real user profile

This task taught me how to correlate a parent-child process chain in a timeline using Winlogbeat logs in Kibana. By filtering with process.parent.pid, I was able to isolate what was launched right after mshta.exe ran. This is critical in real-world malware forensics when tracking staged payload execution and lateral movement.
















