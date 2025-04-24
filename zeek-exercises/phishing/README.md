# Zeek Exercise: Phishing Attempt (Task 3)

**Objective:** Investigate a phishing attempt by analyzing captured network traffic for malicious sources, domains, and downloaded artifacts.  
The case was assigned to me. Inspect the PCAP and retrieve the artefacts to confirm this alert is a true positive.

---

## 📂 PCAP Used:
`phishing.pcap`

---

## 🔧 Commands Used:
```bash
zeek -Cr phishing.pcap
cat conn.log | zeek-cut id.orig_h | sort | uniq -c
cat http.log | zeek-cut uri host
zeek -Cr phishing.pcap hash-demo.zeek
cat files.log | zeek-cut mime_type md5

