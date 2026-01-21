---
test_source: "[[DojoTest 1]]"
domain: "[[Design Cost-Optimized Architectures\r]]"
error_type: "[[Knowledge Gap]]"
service: "[[NAT Gateway]]"
date: 2026-01-21
---

# טעות: מיגרציה מ-On Premise Windows ל-AWS

> [!failure] השאלה בקצרה
> איך מבצעים מיגרציה לשרתי On Premise Windows כך שיהיו עם high availability ו-low latency ל-Block Storage

### מה אני בחרתי?
בחרתי ב-[[FSx for Windows]] כי קישרתי את זה בהכרח לWindows File System.

### למה זו טעות?
IGW דורש Public IP והוא מאפשר תעבורה דו-כיוונית. השאלה ביקשה "מאובטח" ו-"Private".

### התשובה הנכונה: [[NAT Gateway]]
* **הסבר:** NAT Gateway מאפשר תעבורה יוצאת (Outbound) בלבד.
* **נקודה קריטית למבחן:** הוא חייב לשבת ב-Public Subnet כדי לעבוד.

### 💡 לקח לעתיד (Key Takeaway)
בכל פעם שמופיעה המילה **Private Subnet** + **Internet Access**, התשובה היא כמעט תמיד NAT Gateway או NAT Instance.