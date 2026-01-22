קטגוריה: #Migration #DisasterRecovery

שירות מנוהל להתאוששות מאסון המאפשר שכפול מתמשך של שרתים (פיזיים או וירטואליים) ל-AWS בעלות מינימלית.

## מנגנון הפעולה
1. **Replication Agent:** מותקן על שרת המקור (On-prem/Cloud).
2. **Staging Area:** אזור ב-AWS (VPC ייעודי) הכולל משאבים זולים (שרתי EC2 קטנים ו-EBS) שקולטים את השכפול ברמת הבלוק.
3. **Continuous Replication:** הנתונים מסונכרנים כל הזמן (מביא ל-RPO נמוך).
4. **Recovery Job:** בעת אסון, DRS הופך את הנתונים מה-Staging Area לשרתים חזקים (Target Instances) ב-VPC המרכזי.

## מדדי ביצוע (Metrics)
* **RPO (Recovery Point Objective):** בדרך כלל **שניות**. (כמות המידע שנאבד).
* **RTO (Recovery Time Objective):** בדרך כלל **דקות**. (הזמן שלוקח להחזיר את המערכת לפעולה).
* *לפירוט המושגים, ראה:* [[Disaster Recovery Strategies]]

## דגשים למבחן (SAA-C03)
- **Cost-Effective:** הפתרון המשתלם ביותר ל-DR כי לא מחזיקים שרתים חזקים דולקים ב-AWS בשגרה.
- **Drill:** ניתן להריץ בדיקות DR (Drills) מבלי להפסיק את השכפול השוטף.
- **Failback:** תומך בהחזרת הנתונים המעודכנים מ-AWS לאתר המקור אחרי שהתקלה תוקנה.
- **שימוש ב-Snapshot:** ניתן לבחור נקודת זמן ספציפית להתאוששות (למשל לפני התקפת Ransomware).

## קשרים לאסטרטגיות
- DRS מיישם לרוב אסטרטגיה של **Pilot Light** (או Warm Standby במקרים מסוימים).
- *ראה השוואה:* [[Disaster Recovery Strategies#📊 טבלת השוואה למבחן (Cheat Sheet)|Pilot Light vs Warm Standby]]