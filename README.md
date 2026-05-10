#  Project Documentation & AI Integration - Person Management System

##  שימוש בכלי AI 

בפרויקט זה נעשה שימוש מתקדם בכלי בינה מלאכותית כגון: **GitHub Copilot, Gemini, GPT, ClaudAI** אשר שימשו כ־**Senior Architectural Partner** לאורך כל שלבי הפיתוח.

העבודה התבססה על גישת **AI-Augmented Development**, שבה ה־AI שולב כחלק אינטגרלי מקבלת ההחלטות ההנדסיות:

- **Refactoring Strategy:** תכנון מעבר משדה `FullName` יחיד לארכיטקטורה מבוססת שמות מפוצלים (`FirstName`, `LastName`) תוך שמירה על **Data Integrity** וביצוע **Backward Compatibility**.

- **Database Migration Logic:** כתיבת סקריפטים של **Raw SQL** לצורך ביצוע מניפולציה על נתונים היסטוריים בתוך EF Core Migrations, במטרה למנוע **Data Loss**.

- **Prompt Engineering:** שימוש ב־**Copilot Instructions** מוגדרים מראש לאכיפת סטנדרטים של קוד נקי (**Clean Code**), שמירה על **Naming Conventions** והאצת תהליכי הפיתוח.

---

#  דוגמה לשיפור קוד: מנוע חיפוש חסין (Resilient Search)

אחד השיפורים המרכזיים שבוצעו בסיוע AI היה שדרוג מנגנון החיפוש במערכת מחיפוש בסיסי למנוע חכם.

##  הבעיה

מנגנון החיפוש הבסיסי נכשל במקרים נפוצים של:

- **Case Sensitivity** (רגישות לאותיות גדולות/קטנות).

- **Keyboard Mismatch:** הקלדה בשפה שגויה  
  (לדוגמה: המשתמש הקליד `nav` כשהתכוון ל־`משה`).

---

## ✅ הפתרון בסיוע AI

ה־AI סייע בתכנון ומימוש אלגוריתם **Mirror Keyboard** (מקלדת הפוכה) בתוך שכבת ה־Service:

```csharp
// Virtual keyboard translation before querying the database
var translatedFilter = TranslateMirrorKeyboard(cleanFilter);

query = query.Where(p =>
    p.FirstName.ToLower().Contains(cleanFilter) ||
    p.LastName.ToLower().Contains(cleanFilter) ||
    p.FirstName.ToLower().Contains(translatedFilter) ||
    p.LastName.ToLower().Contains(translatedFilter));
```

### 🎯 התוצאה

מנוע חיפוש **Resilient** החסין לטעויות הקלדה ומעניק חוויית משתמש (**UX**) גבוהה.

---

#  החלטה תכנונית: שימוש ב־Interface (IService)

בפרויקט יושמו עקרונות ה־SOLID  
(ובפרט **Dependency Inversion**)  
על ידי יצירת ה־Interface בשם:

```csharp
IPersonService
```

## הנימוקים המרכזיים להחלטה

### Decoupling (הפרדת צימודים)

הפרדה מוחלטת בין שכבת ה־Controller לבין שכבת ה־Data Access.

ה־Controller אינו מודע למימוש הספציפי, מה שמאפשר החלפת תשתית בקלות.

---

### Testability (יכולת בדיקה)

שימוש ב־Interface מאפשר לבצע Mocking בקלות לצורך כתיבת Unit Tests ללא צורך בחיבור פעיל למסד נתונים.

---

### Maintainability (תחזוקתיות)

ריכוז הלוגיקה העסקית ב־Service מאפשר שינויים עתידיים  
(כמו החלפת רכיב ה־PDF או שינוי שיטת אחסון התמונות)  
במיקום אחד, מבלי להשפיע על ה־API החיצוני.

---

#  עבודה עם Copilot Instructions

כדי להבטיח רמת קוד גבוהה, הוגדרו "הנחיות מערכת" (System Prompts) ששימשו כמעקה בטיחות ל־AI.

---

## Architectural Role

הנחיית ה־AI לפעול כ־Senior .NET Architect לקבלת הצעות קוד בסטנדרט Enterprise.

---

## Performance First

דרישה לשימוש ב־`AsNoTracking()` בשאילתות שליפה (Read-only) לשיפור ה־Memory Footprint.

```csharp
_context.People
    .AsNoTracking()
    .Where(x => x.IsActive)
```

---

## Formatting Guardrails

שימוש ב־Markdown וב־LaTeX להסברים טכניים ונוסחאות לוגיות בתוך תיעוד הקוד.

---

# 📂 סיכום שלבי הפיתוח

## חלק א'

---המימוש הבסיסי המציג יכולותCore (CRUD, PDF, File Upload).

## חלק ב'

ביצוע Refactoring מקיף לפיצול שדות השם, הוספת סטטוס פעיל/לא־פעיל, הטמעת מנגנון חיפוש חכם.

---

## חלק ג'
תיעוד תכנוני ושימוש מושכל ב-AI כחלק מדרישות החובה.
