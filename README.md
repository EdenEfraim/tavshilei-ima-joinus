# 🍳 תבשילי אמא — דף גיוס עובדים

> מסמך זה מסכם את כל הארכיטקטורה, החיבורים, ותהליך העבודה של הפרויקט.
> שמור אותו — הוא עונה על כל שאלה שתצוץ בעוד שבוע.

---

## 🌐 כתובות חשובות

| שם | כתובת |
|----|--------|
| **האתר בפרודוקציה** | https://joinus.imatavshil.com |
| **Vercel (hosting)** | https://vercel.com |
| **GitHub (קוד)** | https://github.com/EdenEfraim/tavshilei-ima-joinus |
| **Formspree (טפסים)** | https://formspree.io |

---

## 🏗️ ארכיטקטורה — איך הכול מחובר

```
[מישהו סורק QR]
        ↓
joinus.imatavshil.com   ← subdomain בוויקס (CNAME)
        ↓
Vercel (hosting חינמי)   ← מגיש את index.html
        ↓
דף הנחיתה נפתח בנייד
        ↓
משתמש ממלא טופס ולוחץ "שלח"
        ↓
Formspree (שירות טפסים חינמי)
        ↓
מייל מגיע אליך עם פרטי המועמד
```

---

## 📦 שירותים ותפקידם

### 1. GitHub — `EdenEfraim/tavshilei-ima-joinus`
- שמור את **קוד המקור** של הפרויקט
- כל שינוי עובר דרכו
- שני branches:
  - `dev` — לעבודה ובדיקה
  - `main` — פרודוקציה (מה שהציבור רואה)

### 2. Vercel — Hosting
- מאזין ל-GitHub אוטומטית
- כל `push` ל-`dev` → בונה **Preview URL** (לבדיקה בלבד)
- כל `push` ל-`main` → מעדכן את **joinus.imatavshil.com**
- חינמי לחלוטין לפרויקטים קטנים

### 3. Wix DNS — Subdomain
- הדומיין `imatavshil.com` מנוהל בוויקס
- נוספה רשומת CNAME:
  - Host: `joinus`
  - Value: `cname.vercel-dns.com`
- זה מה שגורם ל-`joinus.imatavshil.com` להצביע על Vercel

### 4. Formspree — טפסים
- מקבל את ההגשות מהטופס באתר
- שולח מייל לכתובת שהגדרת
- ניהול ב: https://formspree.io
- ה-endpoint מוטמע ב-`index.html` (שורה עם `action="https://formspree.io/f/..."`)

---

## 🔄 תהליך עבודה יומיומי

### רוצה לשנות משהו באתר?

```
1. פתח Claude Code
2. תגיד לו לעבור ל-dev:
   "switch to dev branch"

3. תאר את השינוי שרוצה:
   "שנה את טקסט השכר / הוסף שדה / עדכן את הצבעים..."

4. Claude Code עושה את השינוי ומעלה:
   git push → לוקח 30 שניות

5. Vercel שולח לך Preview URL (במייל או ב-Vercel dashboard)
   → כתובת זמנית, רק אתה רואה אותה

6. בדקת ואתה מרוצה? תגיד ל-Claude Code:
   "merge to main and push"
   → האתר מתעדכן תוך דקה
```

### תרשים הזרימה:

```
Claude Code עושה שינוי
        ↓
git push → dev branch
        ↓
Vercel בונה Preview URL אוטומטי
        ↓
    בדקת? מרוצה?
    ↙           ↘
  כן             לא
   ↓              ↓
merge main     תיקון נוסף
   ↓           ב-dev
פרודוקציה
מתעדכנת
```

---

## 📁 מבנה הקבצים

```
tavshilei-ima-joinus/
│
├── index.html          ← כל האתר (HTML + CSS + JS בקובץ אחד)
└── README.md           ← המסמך הזה
```

כל האתר הוא **קובץ אחד בלבד** — `index.html`. אין build process, אין dependencies, אין node_modules. פשוט וקל לתחזוקה.

---

## ✏️ סוגי שינויים נפוצים

### שינוי טקסט
```
תגיד ל-Claude Code:
"שנה את הטקסט '45–50 ₪' ל-'50–55 ₪' ב-index.html"
```

### שינוי צבעים
```
"שנה את צבע הכפתור מ-#9A5047 ל-#7A3A33"
```

### הוספת שדה לטופס
```
"הוסף שדה 'ניסיון קודם' לטופס, select עם אפשרויות: אין / עד שנה / שנה+"
```

### שינוי תמונה / לוגו
```
"הוסף את הלוגו הזה בראש הדף" + צרף קובץ תמונה
```

---

## 🔑 איפה כל החשבונות

| שירות | איפה להיכנס |
|--------|------------|
| Vercel | vercel.com עם אותו מייל שנרשמת |
| GitHub | github.com/EdenEfraim |
| Formspree | formspree.io עם המייל שרשמת |
| Wix DNS | manage.wix.com → Domains → imatavshil.com → DNS Records |

---

## 🆘 פתרון בעיות נפוצות

**האתר לא מתעדכן אחרי push:**
→ בדוק ב-Vercel תחת Deployments שהבילד הצליח (✅ ירוק)
→ אם אדום — שלח ל-Claude Code את הלינק לשגיאה

**הטופס לא שולח מייל:**
→ בדוק ב-formspree.io שה-form פעיל
→ בדוק שה-endpoint ב-index.html תקין

**joinus.imatavshil.com לא נפתח:**
→ בדוק ב-Vercel שה-domain מוגדר (Settings → Domains)
→ בדוק ב-Wix שרשומת ה-CNAME קיימת

---

*נוצר ב-Claude • אפריל 2026*
