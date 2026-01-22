קטגוריה: #Storage #CloudComputing

תיאור קצר: משפחת שירותי מערכות קבצים מנוהלות המאפשרת להריץ אפליקציות בסביבתן הטבעית (Windows, Linux, HPC) עם ביצועים גבוהים.

---

## סקירה כללית

Amazon FSx חוסך את הניהול של שרתי קבצים, עדכוני גרסה וגיבויים ידניים. השירות מציע ארבעה פתרונות המותאמים לעומסי עבודה שונים:

---

## סוגי ה-FSx

### 1. FSx for Windows File Server

^a5cd40

- **פרוטוקול:** SMB (Server Message Block).
    
- **שימושים עיקריים:** * שיתוף קבצים ארגוני (Home Directories).
    
    - אפליקציות Windows (כמו .NET או SQL Server).
        
    - אינטגרציה עם Microsoft Active Directory.
        
- **תכונות מפתח:** Data Deduplication (חיסכון בשטח אחסון), תמיכה ב-Windows ACLs.


### 2. FSx for Lustre

^395183

- **פרוטוקול:** POSIX-compliant.
    
- **שימושים עיקריים:** * High Performance Computing (HPC).
    
    - עיבוד נתונים מהיר (ML, Rendering).
        
- **תכונות מפתח:** אינטגרציה ישירה עם **S3** - טעינת נתונים מ-S3 ועיבודם במהירות אדירה.
    
- **סוגי אחסון:** Scratch (זמני/זול) ו-Persistent (עמיד לטווח ארוך).


### 3. FSx for NetApp ONTAP

- **פרוטוקול:** Multi-protocol (NFS, SMB, iSCSI).
    
- **שימושים עיקריים:** הגירה של ארגונים המשתמשים ב-NetApp ב-Data Center שלהם (Lift & Shift).
    
- **תכונות מפתח:** תמיכה ב-Snapshots, Cloning ויכולות דחיסה מתקדמות.


### 4. FSx for OpenZFS

- **פרוטוקול:** NFS.
    
- **שימושים עיקריים:** אפליקציות לינוקס הדורשות Latency נמוך מאוד (מתחת למילי-שנייה).
    
- **תכונות מפתח:** מבוסס על מערכת הקבצים ZFS, תומך ב-Copy-on-write ובביצועים גבוהים במיוחד.


---

## 📊 טבלת החלטה למבחן (Decision Matrix)

|**הצורך המרכזי**|**הפתרון הנבחר**|**מילת מפתח במבחן**|
|---|---|---|
|**שיתוף קבצים ב-Windows**|FSx for Windows|SMB, Active Directory|
|**חישובים כבדים / AI**|FSx for Lustre|HPC, S3 Integration|
|**מעבר מ-NetApp קיים**|FSx for NetApp ONTAP|iSCSI, ONTAP Features|
|**NFS מהיר מאוד ללינוקס**|FSx for OpenZFS|ZFS, Ultra-low latency|

---

## 💡 דגשים ארכיטקטוניים (Architectural Notes)

> [!info] Deployment
> 
> - **Single-AZ:** זול יותר, מתאים לסביבות פיתוח או Scratch.
>     
> - **Multi-AZ:** שכפול נתונים בין AZs שונים, חובה ליישומים קריטיים (Production).
>     

> [!warning] Security
> 
> - הנתונים מוצפנים במנוחה (At rest) באמצעות **KMS**.
>     
> - השירות מוגן בתוך ה-VPC באמצעות Security Groups.
>     
