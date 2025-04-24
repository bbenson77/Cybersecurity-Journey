# Zeek Exercise: Anomalous DNS (Task 2)

**Objective:** Investigate a DNS tunneling alert and determine if it’s a true positive by analyzing the PCAP file with Zeek.

---

## 📂 PCAP Used:
`dns-tunneling.pcap`

---

## 🔧 Commands Used:
```bash
zeek -C -r dns-tunneling.pcap
cat dns.log | grep ":" | wc -l
cat dns.log | less
cat dns.log | zeek-cut qtype_name | grep "AAAA" | uniq -c
cat conn.log | zeek-cut duration | sort -r | head -n 1
cat dns.log | zeek-cut query |rev | cut -d '.' -f 1-2 | rev | head
cat conn.log | zeek-cut id.orig_h | sort | uniq -c



## 📸 Screenshot Output

![AAAA Count](./dns-aaa-count.png)
Linked terminal screenshot to README






