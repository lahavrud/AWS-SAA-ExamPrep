שירות מנוהל שמחלק את תעבורת הגולשים הנכנסת בין מספר יעדים (כמו שרתי [[EC2]], קונטיינרים או כתובות IP) במספר [[Availability Zones]].

## 1. ארבעת סוגי ה-Load Balancers

בבחינה, השאלה תמיד תתמקד בבחירת הסוג הנכון לפי הצרכים הטכניים:

|**סוג**|**שכבת רשת (OSI)**|**מאפיינים ושימושים מרכזיים**|
|---|---|---|
|**[[ALB]] (Application)**|**Layer 7** (HTTP/S)|תומך בניתוב לפי נתיב (`/users`) או דומיין. אידיאלי למיקרו-שירותים.|
|**[[NLB]] (Network)**|**Layer 4** (TCP/UDP)|לביצועים קיצוניים, שיהוי (Latency) נמוך מאוד וכתובות IP קבועות.|
|**[[GLB]] (Gateway)**|**Layer 3** (IP)|מיועד לניהול מכשירי צד-ג' כמו Firewalls או IDS/IPS וירטואליים.|
|**[[CLB]] (Classic)**|Layer 4/7|טכנולוגיה ישנה (Legacy). כמעט ולא מופיע בבחינה, למעט מקרים ישנים.|

---

## 2. מושגי יסוד ורכיבים

- **[[Target Groups]]:** קבוצות של משאבים שאליהם ה-ELB מעביר את התעבורה. שרת יכול להיות שייך ליותר מ-Target Group אחת.
    
- **[[Health Checks]]:** ה-Load Balancer בודק את השרתים באופן קבוע. אם שרת לא מגיב, ה-ELB מפסיק להעביר אליו תעבורה עד שיחזור להיות "בריא".
    
- **[[Listeners]]:** החוקים שמגדירים על איזה פורט ופרוטוקול ה-Load Balancer מאזין (למשל: HTTPS בפורט 443).
    

---

## 3. תכונות מתקדמות למבחן

### [[Sticky Sessions]] (Session Affinity)

מאפשר "לקשור" משתמש ספציפי לשרת ספציפי לאורך כל הסשן שלו. שימושי אם האפליקציה שומרת מידע מקומי על השרת (אם כי ההמלצה היא תמיד לעבוד Stateless).

### [[SSL/TLS Termination]]

ה-Load Balancer יכול לנהל את תעודות ה-SSL. הפיענוח (Decryption) קורה ב-ELB, והתעבורה לשרתים הפנימיים עוברת ב-HTTP רגיל, מה שחוסך משאבי CPU מהשרתים.

### [[Cross-Zone Load Balancing]]

מבטיח שהתעבורה תחולק באופן שווה בין **כל** השרתים בכל ה-AZs, ללא קשר לכמה שרתים יש בכל אזור בודד. (ב-ALB זה מופעל כברירת מחדל, ב-NLB צריך להפעיל ידנית).

---

## 🚩 נקודות קריטיות למבחן (Exam Tips)

- **שילוב עם [[ASG]]:** ה-ELB וה-Auto Scaling Group עובדים יחד. ה-ASG מוסיף שרתים, וה-ELB מחלק ביניהם את העומס.
    
- **Internal vs Internet-facing:** ניתן להקים Load Balancer ציבורי (עם Public IP) או פנימי בלבד (בתוך ה-VPC).
    
- **טיפ לזיהוי מהיר:**
    
    - אם השאלה מדברת על **HTTP/HTTPS** או **Microservices** -> התשובה היא **[[ALB]]**.
        
    - אם השאלה מדברת על **Extreme Performance**, **Static IP** או **UDP** -> התשובה היא **[[NLB]]**.
        
    - אם השאלה מדברת על **Third-party Firewalls** -> התשובה היא **[[GLB]]**.
        
- **Security Groups:** זכור שה-Security Group של ה-EC2 חייב לאפשר תנועה נכנסת מה-Security Group של ה-Load Balancer.
    

---

**האם תרצה שנעבור לסכם את נושא ה-[[Route 53]] (ה-DNS של AWS) או שנצלול לעומק של נושא האחסון עם [[EBS]]?**