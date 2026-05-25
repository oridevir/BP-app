# מעקב לחץ דם — מרפאת ד"ר דביר

## סקירת הפרויקט

אפליקציית Web חד-קובץ למעקב אחר מדידות לחץ דם. מיועדת למטופלים של מרפאת ד"ר דביר — הרופא שולח את הקובץ (או לינק) למטופל דרך וואטסאפ, המטופל מזין מדידות במשך מספר ימים, ולבסוף שולח ריכוז בחזרה לרופא.

## מחסנית טכנולוגית

- **קובץ יחיד**: `BPapp.html` — HTML + CSS + JavaScript, ללא תלויות חיצוניות
- **אחסון**: `localStorage` (עם fallback ל-`sessionStorage`)
- **פריסה**: GitHub Pages — `https://oridevir.github.io/BP-app/BPapp.html`
- **ריפו**: `https://github.com/oridevir/BP-app`
- **שפת ממשק**: עברית, RTL

## מבנה האפליקציה

### מסכים
| מזהה | תפקיד |
|---|---|
| `#scr-reg` | מסך רישום — מילוי פרטי מטופל (ת.ז., שם, טלפון, מייל) |
| `#scr-main` | הזנת מדידה — סיסטולי / דיאסטולי / דופק + תאריך ושעה |
| `#scr-hist` | היסטוריה — טבלת מדידות, ממוצעים, ייצוא CSV, שליחה לרופא |

### ניווט
ניווט תחתון קבוע (`#bot-nav`) עם שתי לשוניות: "הזן מדידה" ו"ריכוז מדידות".

### אחסון נתונים
```
localStorage['bp_user'] = { id, fn, ln, ph, em }
localStorage['bp_meas'] = [ { id, ts, sys, dia, pul }, ... ]
```

## פונקציות JavaScript מרכזיות

| פונקציה | תפקיד |
|---|---|
| `initStorage()` | בודק localStorage, נופל ל-sessionStorage אם חסום |
| `registerUser()` | אימות ושמירת פרטי מטופל, מעבר לאפליקציה |
| `launchApp()` | הצגת header + nav, הצגת אזהרת iOS אם רלוונטי |
| `saveMeasurement()` | אימות וולידציה, שמירת מדידה עם timestamp |
| `renderHist()` | בניית טבלת היסטוריה עם ממוצעים בfooter |
| `exportCSV()` | ייצוא כל המדידות לקובץ CSV עם BOM לעברית ב-Excel |
| `sendWhatsApp()` | בניית הודעת טקסט מפורמטת ופתיחת wa.me |
| `resetApp()` | מחיקת כל הנתונים (אישור כפול) + reload |
| `lsSet(key, val)` | כתיבה בטוחה ל-storage (try/catch) |
| `lsGet(key, def)` | קריאה בטוחה מ-storage עם ברירת מחדל |

## בעיות iOS/WhatsApp — פתרון מיושם

### הבעיה
וואטסאפ ב-iPhone פותח קבצים ב-WKWebView פנימי שבו `localStorage` **חסום לחלוטין**. כתיבה אליו זורקת `QuotaExceededError` — הכפתור נראה כאינו מגיב.

### הפתרון
1. **`initStorage()`** רץ מיד בטעינה — בודק localStorage, עובר ל-sessionStorage אם נכשל
2. **`_persists`** — דגל בוליאני: `true` רק אם localStorage עובד
3. **באנר כחול** (`#ios-banner`) — מוצג ב-iPhone עם הנחיות: "פתח בספארי ← הוסף למסך הבית"
4. **רצועת אזהרה** (`#storage-warn-strip`) — מוצגת לאורך כל השימוש אם האחסון אינו קבוע
5. **Toast אזהרה** — מופיע 7 שניות לאחר רישום אם האחסון אינו קבוע

### פתרון ארוך-טווח למטופל עם iPhone
פתח מוואטסאפ בספארי ← הוסף למסך הבית ← פתח תמיד מהאייקון. הגדרה חד-פעמית.

## זרימת פיתוח

```bash
# עריכה מקומית
# פתח BPapp.html ישירות בדפדפן לבדיקה

# Push לגיטהאב (מפעיל GitHub Pages אוטומטית)
git add BPapp.html
git commit -m "תיאור השינוי"
git push origin main
```

## פרטי מרפאה

- **רופא**: ד"ר דביר
- **לוגו**: `https://drdevir.com/Images/logo.jpg` (עם fallback emoji 🏥)
- **וואטסאפ רופא**: `972555588571` (בשימוש בפונקציית sendWhatsApp)
