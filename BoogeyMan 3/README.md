# Boogeyman 3 - TryHackMe Challenge

## Objective:
Documenting my full investigation of the **initial payload execution** in the Boogeyman 3 challenge, while building real-world SOC skills using Kibana and Winlogbeat data.

---

## 🧠 Task 1 - Identify the Process That Executed the Initial Stage 1 Payload

### Command / Query Used:
```kql
process.parent.name: "explorer.exe"





