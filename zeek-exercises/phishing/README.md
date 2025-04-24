# Zeek Exercise: Phishing Attempt (Task 3)

**Objective:** Investigate a phishing attempt by analyzing captured network traffic for malicious sources, domains, and downloaded artifacts.
The case was assigned to Me. Inspect the PCAP and retrieve the artefacts to confirm this alert is a true positive. 

---

## 📂 PCAP Used:
`phishing.pcap`

---

## 🔧 Commands Used:
(Commands and outputs will be added during the investigation)
zeek -Cr phishing.pcap
cat conn.log | zeek-cut id.orig_h | sort | uniq -c
cat http.log | zeek-cut uri host
---

## 📸 Screenshots:
(Screenshots of terminal outputs and findings will go here)

---

## 🧠 Analysis Summary:
This section will detail the analysis process including identification of phishing domains, malicious files, and associated threat artifacts.

---
