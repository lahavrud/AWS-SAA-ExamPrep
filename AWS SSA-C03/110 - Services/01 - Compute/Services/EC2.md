Amazon Elastic Compute Cloud (EC2) הוא שירות הליבה של AWS למחשוב בענן. בבחינה, הדגש הוא על בחירת סוג השרת הנכון, מודל הרכישה המשתלם ביותר, וארכיטקטורה של זמינות גבוהה.

---

## 1. יסודות השרת והקמה

- **[[AMI]] (Amazon Machine Image):** ה"אימג'" שממנו נבנה השרת. כולל OS, תוכנות וקונפיגורציית אחסון.
     ^c8dfd1
- **[[User Data]]:** סקריפט Bootstrap שרץ פעם אחת בלבד בהפעלה הראשונה של השרת. משמש להתקנת עדכונים ותוכנות.
    
- **Instance Metadata:** מידע על השרת (Networking, Instance ID, Security וכו') הנגיש מתוך השרת בכתובת `http://169.254.169.254/latest/meta-data/`. (חשוב: לא להתבלבל עם User Data).
    

---

## 2. משפחות וסוגי שרתים (Instance Types)

^b63ede

השמות בנויים במבנה: `m5d.large` (משפחה -> דור -> תכונה -> גודל).

| **משפחה**                 | **שם קוד**  | **מקרה בוחן (Use Case)**                              |
| ------------------------- | ----------- | ----------------------------------------------------- |
| **General Purpose**       | **T, M**    | איזון בין CPU/RAM. אתרי אינטרנט, סביבות פיתוח.        |
| **Compute Optimized**     | **C**       | עיבוד כבד, קידוד וידאו, Machine Learning (Inference). |
| **Memory Optimized**      | **R, X**    | מסדי נתונים (In-memory), Caching (Redis), SAP HANA.   |
| **Storage Optimized**     | **I, D, H** | מערכות קבצים מבוזרות, NoSQL, High IOPS.               |
| **Accelerated Computing** | **P, G**    | חישובים גרפיים (GPU), אימון מודלים של AI.             |

- **T-Class:** שרתים מסוג **Burstable**. הם צוברים "Credits" כשהם במנוחה ומשתמשים בהם כשיש עומס רגעי.
    

---

## 3. מודלים של רכישה (Cost Optimization)

^327926

זהו נושא קריטי לבחינה תחת פרק ה-Cost Optimization.

| **מודל**               | **תיאור**                      | **מתי נבחר?**                            |
| ---------------------- | ------------------------------ | ---------------------------------------- |
| **On-Demand**          | תשלום לפי שניה. הכי יקר.       | עומסים לא צפויים, אפליקציות חדשות.       |
| **Reserved Instances** | התחייבות (1/3 שנים).           | עומס עבודה קבוע וידוע (Steady-state).    |
| **Savings Plans**      | התחייבות לסכום הוצאה ($).      | גמישות בין סוגי שרתים ואזורים (Regions). |
| **Spot Instances**     | רכישת עודפי קיבולת (90% הנחה). | עומסים שיכולים להיקטע (Batch jobs).      |
| **Dedicated Hosts**    | שרת פיזי שלם ללקוח אחד.        | רגולציה מחמירה או רישיונות תוכנה (BYOL). |

---

## 4. אחסון ורשת (Storage & Networking)

- **אחסון:**
    
    - **[[Amazon EBS|EBS]] (Elastic Block Store):** כונן רשת (Network Drive). הנתונים נשמרים גם כשהשרת כבוי. ניתן להעביר בין שרתים באותו ה-AZ.
        
    - **[[Instance Store]]:** אחסון פיזי מחובר (Ephemeral). מהיר מאוד אך **הנתונים אובדים** אם השרת נעצר/נמחק.
        
    - **[[EFS]]:** מערכת קבצים מנוהלת שניתן לחבר למאות שרתים במקביל.
        
- **רשת:**
    
    - **[[Security Groups]]:** זה ה-Firewall ברמת השרת. **Stateful** (חוק בכניסה מאשר אוטומטית את היציאה).
        
    - **[[ENI]] (Elastic Network Interface):** כרטיס רשת וירטואלי. ניתן להעביר ENI משרת אחד לשני לצורך Disaster Recovery.
        
    - **[[EIP]] (Elastic IP):** כתובת IPv4 קבועה שאינה משתנה כשהשרת עוצר.
        

---

## 5. זמינות גבוהה וגדילה (Scaling & HA)

כדי לבנות מערכת "Elastic", נשתמש ב:

1. **[[Elastic Load Balancing|ELB]] (Elastic Load Balancer):**
    
    - **[[ALB]] (Application):** שכבה 7 (HTTP). תומך בנתיבים (Paths) ודומיינים.
        
    - **[[NLB]] (Network):** שכבה 4 (TCP). לביצועים קיצוניים וכתובות IP קבועות.
        
2. **[[Auto Scaling Group|ASG]] (Auto Scaling Group):** מוסיף/מסיר שרתים לפי מדדים (CPU, Network).
    
    - **Launch Template:** מגדיר "איך" ייראה השרת החדש (AMI, Type).
        
3. **[[Placement Groups]]:**
    
    - _Cluster:_ שרתים קרובים (למהירות רשת).
        
    - _Spread:_ שרתים על חומרה נפרדת (לשרידות).
        
    - _Partition:_ חלוקה לקבוצות לוגיות (Hadoop/Kafka).
        

---

## טיפים זהב למבחן

- אם מבקשים **מקסימום ביצועי רשת** בין שרתים -> **Placement Group: Cluster**.
    
- אם מבקשים **חיסכון מקסימלי** לעבודה שיכולה להיעצר -> **Spot Instances**.
    
- אם שרת לא מצליח להתחבר לאינטרנט -> בדוק את ה-**[[Security Groups]]** ואת ה-[[Route Table]] ב-[[VPC]].
    
- זכור: **[[EBS]] Snapshots** הם Incremental ונשמרים ב-S3.
