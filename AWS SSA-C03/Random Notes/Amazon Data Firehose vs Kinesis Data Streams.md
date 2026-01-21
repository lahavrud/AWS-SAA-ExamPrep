ההבדל המהותי ביותר הוא בין **אגירה וצריכה עצמאית** (Streams) לבין **הזרמה והגשה מנוהלת** (Firehose).

---

## [[Kinesis Data Streams]] (ה"אגם")

שירות המיועד לקליטת נתונים בזמן אמת ושמירתם לצורך עיבוד על ידי צרכנים שונים.

- **מודל פעולה (Pull):** המידע זורם לאגם ונשמר שם. הצרכן (למשל [[Lambda]]) הוא "הדייג" שבא להוציא את המידע מתי שנוח לו.
    
- **ניהול (Shards):** דורש ניהול של "נתיבים" (Shards) כדי לקבוע את רוחב הפס (אלא אם משתמשים במודל On-demand).
    
- **זמן שמירה (Retention):** הנתונים נשמרים באגם בין 24 שעות ל-365 ימים.
    
- **עיבוד א-סינכרוני:** הצרכנים עובדים באופן עצמאי. אם צרכן אחד איטי, זה לא מפריע לנתונים להמשיך להיכנס לאגם.
    
- **שימוש עיקרי:** אפליקציות הדורשות עיבוד מורכב, שמירת סדר האירועים, או צריכה של אותו מידע על ידי מספר אפליקציות שונות בו-זמנית.
    

---

## 🚰 [[Amazon Data Firehose]] (ה"ברז" / צינור)

שירות המיועד להזרמת נתונים ישירות ליעדים (Destinations) עם מינימום ניהול.

- **מודל פעולה (Push):** המידע זורם בצינור ונדחף ישירות ליעד (כמו [[S3]] או Redshift).
    
- **ניהול (Serverless):** אין צורך בניהול Shards. השירות גדל אוטומטית לפי כמות המידע.
    
- **זמן שמירה:** אין. ברגע שהמידע עבר בצינור והגיע ליעד, הוא נעלם מה-Firehose.
    
- **עיבוד סינכרוני (Blocking):** אם מוסיפים [[Lambda]] לשינוי המידע (Transformation), הצינור "מחכה" לסיום העיבוד לפני שהוא ממשיך ליעד.
    
- **שימוש עיקרי:** הזרמת לוגים (Logs), הזרמת מידע ל-[[Data Lake]] ב-[[S3]], וביצוע שינויים פשוטים בפורמט המידע תוך כדי תנועה.
    

---

## 📊 טבלת השוואה (The Difference Maker)

| **מאפיין**          | **[[Kinesis Data Streams]] (אגם)** | **[[Amazon Data Firehose]] (ברז)** |
| ------------------- | ---------------------------------- | ---------------------------------- |
| **אינטראקציה**      | הצרכן מושך מידע (**Pull**)         | השירות דוחף ליעד (**Push**)        |
| **אחסון**           | נשמר עד שנה (Persistent)           | אין שמירה (Streaming only)         |
| **ניהול**           | דורש הגדרת Shards                  | מנוהל לחלוטין (Serverless)         |
| **עיבוד**           | עצמאי וא-סינכרוני                  | סינכרוני (חלק מהצינור)             |
| **שיהוי (Latency)** | זמן אמת (מתחת לשנייה)              | סביב 60-90 שניות (בגלל ה-Buffer)   |

---

## 💡 לקח למבחן (SAA-C03)

> [!important] מתי נבחר מה?
> 
> 1. אם השאלה מדברת על **מספר צרכנים** שצריכים את אותו המידע או על **עיבוד מותאם אישית ומורכב** -> בחר ב-**Streams**.
>     
> 2. אם השאלה מדברת על **הזרמה פשוטה** ליעדים כמו S3/OpenSearch עם **מינימום מאמץ ניהולי** -> בחר ב-**Firehose**.
>     
> 3. אם יש **עיבוד ארוך (5 דקות+)** -> Firehose יישבר בגלל מנגנון ה-Buffer, ולכן חובה להשתמש ב-**Streams** עם Lambda Consumer.
>     

---
