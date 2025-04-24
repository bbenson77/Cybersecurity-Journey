# Zeek Exercise: Log4J Exploitation Attempt (Task 4)

**Objective:** Investigate network traffic for signs of Log4Shell exploitation by using Zeek scripts, signatures, and log analysis. Confirm the alert and identify artefacts linked to the exploitation attempt.

---

## 📂 PCAP and Scripts Used:
- `log4shell.pcapng`
- `detection-log4j.zeek`

---

## 🔧 Commands Used:
```bash
zeek -Cr log4shell.pcapng detection-log4j.zeek
cat signatures.log | zeek-cut sig_id | wc -l
cat http.log | zeek-cut user_agent|sort| uniq -c
cat log4j.log | zeek-cut value | head -n20


