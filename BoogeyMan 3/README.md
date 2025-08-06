# Boogeyman 3 - TryHackMe Challenge

## Objective:
Documenting my full investigation of the **initial payload execution** in the Boogeyman 3 challenge, while building real-world SOC skills using Kibana and Winlogbeat data.

---

## 🧠 Task 1 - Identify the Process That Executed the Initial Stage 1 Payload

**Command / Query Used:**
```kql
process.parent.name: "explorer.exe"


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









