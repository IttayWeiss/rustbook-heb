## כיצד לכתוב מבחנים

מבחנים הם פונקציות בראסט שמוודאות שקוד המטרה מתפקד בדרך הצפויה. בגוף של פונקצית מבחן בדרך-כלל מבצעים שלוש פעולות אלה:

1. אתחול דאטה או מצבים.
2. הרצת הקוד שרוצים לבדוק.
3. ווידוא שהתוצאה הצפויה התקבלה.

הבה נראה את היכולות שראסט מספקת במיוחד לכתיבת מבחנים לביצוע פעולות אלה, שכוללות את התכונה `test`, כמה מקרואים, ואת התכונה `should_panic`.

### האנטומיה של פונקצית מבחן

בצורתה הפשוטה ביותר, מבחן בראסט הוא פונקציה שמבוארת בתכונה `test`. תכונות אלה (attributes) הן מטה-דאטה אודות פיסות של קוד בראסט; דוגמא אחת היא התכונה `derive` בה השתמשנו עם מבנים בפרק 5. על מנת לשנות פונקציה לפונקצית מבחן, יש להוסיף את השורה `#[test]` לפני שורת ה-`fn`. כאשר מריצים את המבחנים עם פקודת `cargo test`, ראסט בונה קובץ בינרי להרצת המבחנים שמריץ את הפונקציות המבוארות ומדווח עבור כל פונקציה אם היא עברה בהצלחה או כשלה.

בכל פעם שאנו יוצרים עם קארגו פרוייקט ספריה חדש, מודול מבחנים עם פונקצית מבחן מיוצר אוטומטית עבורנו. מודול זה מספק שלד לכתיבת מבחנים, כך שאין צורך לחפש ולמצוא מה המבנה והתחביר של פונקציות מבחן בכל פעם שמתחילים פרוייקט חדש. ניתן להוסיף כמה פונקציות מבחן וכמה מודולי מבחן שרוצים!

אנו נחקור כמה אספקטים של דרך הפעולה של מבחנים על-ידי התנסות עם שלד המבחן לפני שממש נבדוק קוד. לאחר מכן נכתוב מבחנים אמיתיים שקוראים לפיסת קוד שכתבנו ומוודאים את תקינות פעולתו.

הבה ניצור פרוייקט ספריה חדש בשם `adder` שמחבר שני מספרים:

```console
$ cargo new adder --lib
     Created library `adder` project
$ cd adder
```

התוכן של הקובץ _src/lib.rs_ בספרית ה-`adder` שלכם צריך להראות כמוצג ברשימה 11-1.

<span class="filename">Filename: src/lib.rs</span>

<!-- manual-regeneration
cd listings/ch11-writing-automated-tests
rm -rf listing-11-01
cargo new listing-11-01 --lib --name adder
cd listing-11-01
cargo test
git co output.txt
cd ../../..
-->

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-01/src/lib.rs}}
```

<span class="caption">רשימה 11-1: המודול ופונקצית המבחן הנוצרים אוטומטית על-ידי `cargo new`</span>

לעת זו, התעלמו משתי השורות הראשונות והתבוננו בפונקציה. שימו לב לביאור `#[test]`: תכונה זו מציינת שזו פונקציית מבחן, כך שמריץ הבחנים ידע להריץ פונקציה זו כמבחן. ייתכן וייהו לנו במודול `tests` גם פונקציות שאינן פונקציות מבחן, למשל כדי לאתחל מצבים נפוצים או לבצע פעולות שכיחות, ולכן תמיד יש לציין אלו פונקציות הן פונקציות מבחן.

הגוף של פונקצית המבחן משתמש במקרו `assert_eq!` כדי לוודא ש-`result`, שמכילה את התוצאה של הוספת 2 ל-2, שווה ל-4. זוהי התבנית הכללית לפונקציות מבחן. הבה נריץ את הפונקציה ונראה שהיא עוברת בהצלחה.

הפקודה `cargo test` מריצה את כל פונקציות המבחן בפרוייקט, כפי שרואים ברשימה 11-2.

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-01/output.txt}}
```

<span class="caption">רשימה 11-2: הפלט המתקבל מהרצת פונקצית המבחן המיוצרת אוטומטית</span>

קארגו קימפל והריץ את המבחן. אנו רואים את השורה `running 1 test`. השורה הבאה מציגה את השם של פונקצית המבחן, קריא `it_works`, ושהתוצאת המבחן היא `ok`. משמעות הסיכום הכללי: `test result: ok.` היא שכל פונקציות המבחן עברו בהצלחה, והחלק `1 passed; 0
failed` מונה את מספר המבחנים הכולל שעברו ונכשלו.

ניתן לסמן מבחן ולגרום לכך שהוא לא יורץ בסיטואציות ספציפיות; נדון בכך בסעיף
[“התעלמות ממבחנים מסויימים אלא אם נדרש אחרת”][ignoring]<!-- ignore --> מאוחר יותר בפרק זה. כיוון שלא עשינו זאת כאן, הסיכום מורה על `0 ignored`. ניתן גם להעביר ארגומנט לפקודה `cargo test` כדי להריץ בדיקות שהשם שלהם תואם למחרוזת; זה נקרא _סינון_ ונדון בסעיף [“הרצת תת-קבוצה של בחינות לפי שם”][subset]<!-- ignore -->. כמו כן, לא סיננו את המבחנים להרצה, ולכן הסיכום מראה `0 filtered out`.

הנתון `0 measured` הוא סטטיסטיקה למבחני אמת מידה (benchmarking) שמודדים יעילות.
מבחני אמת מידה, בזמן כתיבת ספר זה, קיימים רק בגרסה הלילית של ראסט. תוכלו לקרוא את [תיעוד מבחני אמת מידה][bench] כדי ללמוד עוד על כך.

החלק הבא בפלט הרצת המבחן המתחיל ב-`Doc-tests adder` הוא עבור התוצאות של מבחני תיעוד. עוד לא כתבנו מבחני תיעוד, אולם ראסט יכולה לקמפל כל קוד לדוגמא שמופיע בתיעוד ה-API שלנו.
תכונה זו עוזרת לשמור על התיעוד של הקוד ועל הקוד עצמו מסונכרנים! נראה כיצד לכתוב מבחני תיעוד בסעיף [“הערות תיעוד כמבחנים”][doc-comments]<!-- ignore --> בפרק 14. לעת זו, פשוט נתעלם מהפלט ה-`Doc-tests`.

הבה נתחיל להתאים את המבחן לצרכנו שלנו. קודם כל נשנה את שם הפונקציה מ-`it_works` לשם אחר, כמו `exploration`, כך:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-01-changing-test-name/src/lib.rs}}
```

ואז נריץ שוב את `cargo test`. עכשיו הפלט מראה `exploration` במקום `it_works`:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-01-changing-test-name/output.txt}}
```

כעת נוסיף מבחן נוסף, אבל הפעם נכוון לכך שהמבחן יכשל! מבחנים נכשלים כאשר דבר מה בפונקצית המבחן נכנה לפאניקה. כל מבחן מורץ בפתיל חדש, וכאשר הפתיל המרכזי רואה שפתיל של מבחן מת, הוא מסמן את הבדיקה ככושלת. בפרק 9 הסברנו שהדרך הקלה ביותר להיכנס לפאניקה היא לקרוא למקרו `panic!`. הקלידו את המבחן החדש כפונקציה בשם `another`, כך שהקובץ _src/lib.rs_ יראה כמוצג ברשימה 11-3.

<span class="filename">Filename: src/lib.rs</span>

```rust,panics,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-03/src/lib.rs:here}}
```

<span class="caption">רשימה 11-3: הוספת מבחן שני שנכשל כיוון שהוא קורא למקרו `panic!`</span>

הריצו שוב את המבחנים באמצעות `cargo test`. הפלט צריך להארות כמו ברשימה 11-4, המראה שמבחן ה-`exploration` שלנו עבר בעוד `another` נכשל.

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-03/output.txt}}
```

<span class="caption">רשימה 11-4: תוצאות הרצת מבחנים עם מבחן אחד שצלח ואחד שכשל</span>

במקום `ok`, השורה `test tests::another` מראה `FAILED`. שני סעיפים חדשים מופיעים בין התוצאות האינדיבידואליות והסיכום: הראשון מציג את פרטי הסיבה לכל מבחן שנכשל. במקרה זה, אנו למדים ש-`another` נכשל בגלל שהוא `panicked at 'Make this test fail'` בשורה 10 בקובץ _src/lib.rs_. הסעיף הבא מציג רק את שמות המבחנים שכשלו, מידע מועיל כאשר ישנם מבחנים רבים ולכן גם פרטים מרובים בפלט הרצת המבחן. ניתן להשתמש בשם של מבחן שכשל על מנת להריץ רק אותו כדי להקל על פתרון הבעיה; נדון בעוד דרכים להרצת מבחנים בסעיף
[“שליטה על אופן הרצת מבחנים”][controlling-how-tests-are-run]<!-- ignore
-->.

שורת הסיכום בסוף הפלט מודיעה: בסה"כ, תוצאת המבחן היא `FAILED`. היו לנו מבחן מוצלח אחד ומבחן כושל אחד.

כעת שראיתם איך נראים תוצאות הרצת מבחן בסיטואציות שונות, הבה נכיר כמה מקרואים מלבד `panic!` שמועילים בכתיבת מבחנים.

### בדיקת תוצאות באמצעות המקרו `assert!`

המקרו `assert!`, אשר מסופק על-ידי הספריה הסטנדרטית, שימושי במקרים בהם רוצים לוודא שתנאי מסוים מוערך ל-`true`. מספקים למקרו `assert!` ארגומנט שמוערך לערך בוליאני. אם הערך הוא `true`, שום דבר לא קורה והמבחן ממשיך. אם הערך הוא `false`, המקרו `assert!` קורא ל-`panic!` כדי לגרום למבחן להכשל. שימוש במקרו `assert!` עוזר לנו לבדוק שהקוד שלנו מתפקד כמו שאנחנו רוצים.

בפרק 5, ברשימה 5-15, השתמשנו במבנה `Rectangle` ובמתודה `can_hold`, שמוצגים כאן שוב ברשימה 11-5. הבה נמקם קוד זה בקובץ _src/lib.rs_, ואז נכתוב כמה מבחנים עבורו תוך שימוש במקרו `assert!`.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-05/src/lib.rs:here}}
```

<span class="caption">רשימה 11-5: שימוש במבנה `Rectangle` ובמתודה `can_hold` מפרק 5</span>

המתודה `can_hold` מחזירה ערך בוליאני, כך שזו הזדמנות מצוינת להשתמש במקרו `assert!`. ברשימה 11-6, אנו כותבים מבחן שמפעיל את המתודה `can_hold` על-ידי יצירת מופע של `Rectangle` עם רוחב 8 וגובה 7, ואנו מוודאים שהוא יכול להכיל מופע שני של `Rectangle` עם רוחב 5 וגובה 1.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-06/src/lib.rs:here}}
```

<span class="caption">רשימה 11-6: מבחן עבור `can_hold` הבודק האם מלבן גדול יכול להכיל מלבן קטן יותר.</span>

שימו לב שהוספנו שורה חדשה בתוך המודול `tests`: `use super::*;`.
המודול `tests` הוא מודול רגיל ולכן עוקב אחר חוקי הנראות הרגילים אותם סקרנו בפרק 7 בסעיף
[“מסלולים להפנית עצבים בעץ המודולים”][paths-for-referring-to-an-item-in-the-module-tree]<!-- ignore -->. כיוון שהמודול `tests` הוא מודול פנימי, יש להכניס את הקוד מהמודול החיצוני לתוך המתחם של המודול הפנימי. אנו משתמשים ב-\* במקרה זה כדי שכל מה שמוגדר במודול החיצוני יהיה זמין במודול `tests`.

השם שבחרנו למבחן הוא `larger_can_hold_smaller`, ויצרנו את שני המופעים של `Rectangle` שאנו צריכים. אז קראנו למקרו `assert!` והעברנו אליו את תוצאת הקריאה ל-`larger.can_hold(&smaller)`. ביטוי זה אמור להחזיר `true`, כך שהמבחן אמור לעבור בהצלחה. הבה נגלה אם כך הוא!

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-06/output.txt}}
```

אכן, המבחן עבר בהצלחה! הבה נוסיף עוד מבחן, הפעם כזה הטוען שמלבן קטן לא יכול להכיל מלבן גדול יותר:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-02-adding-another-rectangle-test/src/lib.rs:here}}
```

בגלל שהתוצאה הנכונה של המתודה `can_hold` במקרה זה היא `false`, יש לשלול את התוצאה לפני העברתה למקרו `assert!`. בצורה זו, המבחן יעבור בהצלחה במידה ו-`can_hold` תחזיר `false`:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-02-adding-another-rectangle-test/output.txt}}
```

שני מבחנים העוברים בהצלחה! כעת נראה מה יקרה לתוצאות המבחנים שלנו אם נציג באג בקוד שלנו. נשנה את המימוש של המתודה `can_hold` על-ידי החלפת סימן הגדול-שווה בסימן קטן-שווה בזמן השוואת הרוחב:

```rust,not_desired_behavior,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-03-introducing-a-bug/src/lib.rs:here}}
```

הרצת המבחנים כעת נראה כך:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-03-introducing-a-bug/output.txt}}
```

המבחנים שלנו הצליחו לצוד את הבאג! כיוון ש-`larger.width` הוא 8 ו-`smaller.width` הוא 5, ההשוואה בין הרוחבים ב-`can_hold` מחזירה `false`: 8 אינו קטן מ-5.

### בדיקת שוויון באמצעות המקרואים `assert_eq!` ו-`assert_ne!`

דרך נפוצה לוודא פונקציונאליות הוא לבדוק שתוצאה המופקת מהקוד שווה לתוצאה מפורשת שאנו מצפים שהקוד יחזיר. ניתן לעשות זאת על-ידי העברת ביטוי הכולל את האופרטור `==` למקרו `assert!`. אולם, בדיקה מסוג זה היא כל-כך נפוצה שהספריה הסטנדרטית מספקת צמד מקרואים -- `assert_eq!` ו-`assert_ne!` -- כדי לבצע פעולה זו בנוחות. מקרואים אלה משווים שני ארגומנטים ובודקים שוויון או אי-שוויון, בהתאמה. הם גם ידפיסו את שני הערכים במידה והם נכשלים, וכך קל יותר לראות _מדוע_ המבחן נכשל; בניגוד לכך שהמקרו `assert!` רק מציין שהתקבל `false` עבור ביטוי ה-`==`, ללא הדפסת הערכים שהובילו לתוצאה זו.

ברשימה 11-7, אנו כותבים פונקציה בשם `add_two` שמוסיפה 2 לפרמטר שלה, ואז אנו בודקים את הפונקציה באמצעות המקרו `assert_eq!`.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-07/src/lib.rs}}
```

<span class="caption">רשימה 11-7: בדיקת הפונקציה `add_two` תוך שימוש במקרו `assert_eq!`</span>

הבה נבדוק שהמבחן עובר בהצלחה!

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-07/output.txt}}
```

אנו מעבירים את הערך `4` כארגומנט ל-`assert_eq!`, שאכן שווה לתוצאה של הקריאה ל-`add_two(2)`. השורה של המבחן הזה היא `test tests::it_adds_two ...
ok`, `ok` מציין שהמבחן עבר בהצלחה!

הבה נייצר באג בקוד שלנו כדי לראות כיצד נראה `assert_eq!` במצב של כישלון. שנו את המימוש של הפונקציה `add_two` כך שתוסיף את הערך `3`:

```rust,not_desired_behavior,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-04-bug-in-add-two/src/lib.rs:here}}
```

הריצו שוב פעם את המבחנים:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-04-bug-in-add-two/output.txt}}
```

המבחן שלנו הצליח ללכוד את הבאג! המבחן `it_adds_two` נכשל, וההודעה הנלוות אומרת לנו שהווידוא שכשל הוא `` assertion failed: `(left == right)` `` וכן מה האם הערכים `left` ו-`right`. הודעה זו מסייעת לנו במלאכת תיקון הבאג: הארגומנט `left` היה שווה ל-`4` אבל הארגומנט `right`, שהכיל את התוצאה מ-`add_two(2)`, היה שווה ל-`5`. תוכלו לשער לעצמכם את יעילות ההודעות האלה במיוחד כאשר מדובר במקרי מבחן רבים.

שימו לב שבשפות תכנות מסויימות ומערכות בדיקות שונות, הפרמטרים עבור ווידואי שוויון נקראים `expected` ו-`actual`, והסדר בהם הם מופיעים חשוב. אולם, בראסט, הם נקראים `left` ו-`right`, והסדר בו אנו מספקים את הערך הצפוי לעומת הערך המחושב אינו משנה. ניתן לכתוב את הווידוא לעיל גם כ-`assert_eq!(add_two(2), 4)`, ובמקרה של כישלון תודפס אותה ההודעה: `` assertion failed: `(left == right)` ``.

המקרו `assert_ne!` עובר בהצלחה אם שני הערכים המועברים אליו שונים זה מזה, והוא כושל אחרת. מקרו זה יעיל במיוחד במקרים בהם אנחנו לא בטוחים מה _יהיה_ הערך, אבל אנחנו יודעים בוודאות מה הערך _לא יהיה_.
למשל, אם אנו בודקים פונקציה שמובטח שהיא משנה את הפלט שלה בדרך כלשהיא, אבל בדיוק כיצד הערך משתנה תלוי ביום בשבוע בו אנו מריצים את הבדיקה, יתכן שהווידוא הטוב ביותר הוא שהפלט של הפונקציה שונה מהקלט.

מתחת למכסה המנוע, המקרואים `assert_eq!` ו-`assert_ne!` משתמשים באופרטורים `==` ו-`!=`, בהתאמה. כאשר הווידואים כושלים, מקרואים אלה מדפיסים את הארגומנטים שלהם בפורמט של דה-באג, זאת אומרת שהערכים שמושווים זה לזה חייבים לממש את התכונות `PartialEq` ו-`Debug`. כל הטיפוסים הפרימיטיבים ורוב הטיפוסים בספריה הסטנדרטית מממשים תכונות אלה. עבור מבנים ומבחרים שאתם מגדירים בעצמכם, תצטרכו לממש את `PartialEq` על מנת לוודא שוויון עבור טיפוסים אלה. תצטרכו גם לממש את `Debug` כדי להדפיס את הערכים במקרה של כישלון. כיוון ששתי התכונות את תכונות נגזרות, כפי שהוזכר ברשימה 5-12 שבפרק 5, בדרך-כלל כל שיש לעשות הוא להוסיף את הביאור `#[derive(PartialEq, Debug)]` על הגדרת המבנה או המבחר. עבור פרטים נוספים, פנו לנספח C, [“תכונות נגזרות,”][derivable-traits]<!-- ignore --> שם תוכלו לקרוא עוד אודות תכונות אלה ואחרות.

### הוספת הודעות שגיאה מותאמות

ניתן להוסיף, כארגומנט אופציונאלי למקרואים `assert!`, `assert_eq!` ו-`assert_ne!`, הודעת שגיאה מותאמת שתודפס עם הודעת השגיאה במקרה של כישלון. כל ארגומנטים שהם המועברים אחרי הארגומנטים ההכרחיים מועברים למקרו `format!` (בו דנו בסעיף [“שרשור עם הפעולה `+` או עם מקרו הפרמוט”][concatenation-with-the--operator-or-the-format-macro]<!-- ignore --> בפרק 8), כך שניתן לעביר מחרוזת פרמוט הכוללת מחזיקי מקום `{}` וערכים עבור מחזיקי מקום אלה. הודעות מותאמות הן שימושיות כדי לתעד את משמעות הווידוא; במקרה של כישלון, יהיה קונטקסט רחב יותר להבנת הבעיה.

למשל, נניח שיש לנו פונקציה שמברכת אנשים לשלום בשמם ואנו רוצים לבדוק שהשם שאנחנו מעבירים לפונקציה מופיע בפלא:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-05-greeter/src/lib.rs}}
```

הדרישות מתכנית זו לא סוכמו מראש, ואנחנו די משוכנעים שהטקס `Hello` בתחילת הברכה ישתנה בעתיד. החלטנו שאנחנו לא רוצים להצטרך לעדכן את פונקצית המבחן כאשר הדרישות ישתנו, ולכן במקום לבדוק שוויון מדוייק לערך המוחזר מהפונקציה `greeting`, רק נוודא שהפלט כולל את טקסט הקלט.

כעת, הבה נייצר באג בקוד על-ידי שינוי `greeting` כך שלא תכלול את `name` כדי לראות כיצד נראית הודעת השגיאה המסופקת כברירת מחדל:

```rust,not_desired_behavior,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-06-greeter-with-bug/src/lib.rs:here}}
```

הרצת מקרה מבחן זה נראית כך:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-06-greeter-with-bug/output.txt}}
```

כל שתוצאה זו אומרת זה שהווידוא נכשל ובאיזו שורה ארעה השגיאה. הודעה שימושית יותר גם תדפיס את הערך מהפונקציה `greeting`. הבה נוסיף הודעת שגיאה מותאמת המורכבת ממחורזת פרמוט עם מחזיק מקום אליו נשלח את הערך שקיבלנו חזאה מהפונקציה `greeting`:

```rust,ignore
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-07-custom-failure-message/src/lib.rs:here}}
```

כעת כשמריצים את מקרה המבחן מקבלים הודעת שגיאה אינפורמטיבית יותר:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-07-custom-failure-message/output.txt}}
```

אנחנו רואים את הערך עצמו שקיבלנו כפלט בבדיקה, וזה מסייע להבין מה השתבש.

### בדיקת כניסה לפאניקה באמצעות `should_panic`

בנוסף לבדיקת ערכים מוחזרים, חשוב שאפשר יהיה לבדוק שהקוד שלנו מטפל כצפוי בשגיאות. למשל, זכרו את הטיפוס `Guess` שייצרנו בפרק 9, רשימה 9-13. קוד אחר שמשתמש ב-`Guess` מסתמך על הביטחון שמופעי `Guess` תמיד יכילו ערכים בין 1 ל-100. ניתן לכתוב מקרה מבחן שמוודא שניסיון ליצור מופע של `Guess` עם ערך מחוץ לטווח זה יכנס לפאניקה.

עושים זאת על-ידי הוספת התכונה `should_panic` לפונקצית המבחן. המבחן עובר בהצלחה אם קוד המבחן נכנס לפאניקה; המבחן נכשל אם פונקצית המבחן לא נכנסת לפאניקה.

רשימה 11-8 מציגה מקרה מבחן שבודק שתנאי השגיאה של `Guess::new` מתקיימים כאשר אנו מצפים לכך.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-08/src/lib.rs}}
```

<span class="caption">רשימה 11-8: בדיקה שתנאי מסוים גורר כניסה לפאניקה</span>

אנו ממקמים את התכונה `#[should_panic]` לאחר התכונה `#[test]` ולפני פונקצית המבחן המיועדת. הבה נתבונן בתוצאה כאשר המבחן עובר בהצלחה:

```console
{{#include ../listings/ch11-writing-automated-tests/listing-11-08/output.txt}}
```

נראה טוב! כעת, ניצור באג בקוד על-ידי הסרת התנאי שגורם לפונקציה `new` להיכנס לפאניקה במידה והערך גדול מ-100:

```rust,not_desired_behavior,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-08-guess-with-bug/src/lib.rs:here}}
```

כאשר מריצים את הבדיקה עכשיו מרשימה 11-8, היא נכשלת:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-08-guess-with-bug/output.txt}}
```

לא מקבלים הודעת שגיאה מאוד משמעותית במקרה זה, אבל כשמסתכלים על פונקצית המבחן רואים שהיא מבוארת עם `#[should_panic]`. הכישלון שקיבלנו נובע מכך שהקוד בפונקצית המבחן לא נכנס לפאניקה.

מקרי מבחן שמשתמשים ב-`should_panic` יכולים לסבול מחוסר בהירות. מבחן מסוג `should_panic` יעבור בהצלחה גם אם הוא נכנס לפאניקה אבל מסיבה שונה מזו לה אנו מצפים. על מנת להפוך את מבחני `should_panic` ליותר ברורים, ניתן להוסיף פרמטר `expected` אופציונאלי לביאור ה- `should_panic`. מריץ המבחנים יוודא שהודעות הכישלון יכילו את הטקסט הנלווה. למשל, התבוננו בקוד המעודכן עבור `Guess` ברשימה 11-9, שם הפונקציה `new` נכנסת לפאניקה עם הודעות שגיאה שונות בהתאם להאם הערך גדול מידי או קטן מידי.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/listing-11-09/src/lib.rs:here}}
```

<span class="caption">רשימה 11-9: בדיקה עבור כניסה לפאניקה עם הודעת פאניקה שמכילה מחרוזת ספציפית</span>

בדיקה זו תעבור בהצלחה כיוון שהערך שאנו שמים עבור הפרמטר `expected` של התכונה `should_panic` הוא תת-מחרוזת של ההודעה שהפונקציה `Guess::new` נכנסת לפאניקה עבורו. היינו יכולים לציין את כל הודעת הפאניקה לא אנו מצפים, שבמקרה זה היא `Guess value must be less than or equal to
100, got 200.` החלטה איזו תת-מחרוזת לציין תלויה בעד כמה הודעת הפאניקה דינאמית וכמה מדוייקת פונקצית המבחן צריכה להיות. במקרה זה, תת-מחרוזת של הודעת הפאניקה מספיקה כדי לוודא שהקוד בפונקצית המבחן מריץ את המקרה `else if value > 100`.

כדי לראות מה קורה כאשר מבחן מסוג `should_panic` עם הודעת `expected` נכשל, הבה ניצור שוב באג מכוון בקוד שלנו על-ידי החלפת גופי הבלוקים של המקרים `if value < 1` ו-`else if value > 100`:

```rust,ignore,not_desired_behavior
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-09-guess-with-panic-msg-bug/src/lib.rs:here}}
```

הפעם, כאשר מרצים את המבחן הזה, הוא נכשל:

```console
{{#include ../listings/ch11-writing-automated-tests/no-listing-09-guess-with-panic-msg-bug/output.txt}}
```

הודעת הכישלון מציינת שהמבחן אכן נכנס לפאניקה, כפי שציפינו, אולם הודעת הפאניקה לא כללה את המחרוזת `'Guess value must be less than or equal to 100'`. הודעת הפאניקה שכן קיבלנו במקרה זה היתה `Guess value must be greater than or equal to 1, got 200.` כך אפשר להתחיל להבין היכן הבאג!

### שימוש בטיפוס `Result<T, E>` במבחנים

עד כה, כל המבחנים שלנו נכנסו לפאניקה כאשר הם כשלו. ניתן גם לכתוב מבחנים שמשתמשים ב-`Result<T, E>`! הינה המבחן מרשימה 11-1, משוכתב כך שהוא משתמש ב-`Result<T,
E>` ומחזיר `Err` במקום להיכנס לפאניקה:

```rust,noplayground
{{#rustdoc_include ../listings/ch11-writing-automated-tests/no-listing-10-result-in-tests/src/lib.rs}}
```

לפונקציה `it_works` יש עכשיו את `Result<(), String>` כטיפוס הערך המוחזר. בגוף של הפונקציה, במקום לקרוא למקרו `assert_eq!`, אנו מחזירים `Ok(())` כאשר המבחן עובר בהצלחה ו-`Err` עם ערך פנימי מטיפוס `String` במקרה של כישלון.

כתיבת מבחנים שמחזירים `Result<T, E>` מאפשרת שימוש באופרטור סימן השאלה בגוף המבחנים, מה שמספק דרך נוחה לכתוב מבחנים שאמורים להיכשל במידה ואיזושהיא פעולה פנימית להם מחזירה את הוריאנט `Err`.

לא ניתן להשתמש בביאור `#[should_panic]` עבור מבחנים שמחזירים `Result<T,
E>`. כדי לוודא שפעולה מחזירה את הוריאנט `Err`, _אל_ תשמשו באופרטור סימן השאלה על ערך ה-`Result<T, E>`. במקום זאת, השתמשו ב-`assert!(value.is_err())`.

כעת משאתם מכירים כמה דרכים לכתיבת מבחנים, הבה נתבונן במתרחש כאשר מריצים את המבחנים ונכיר את האפשרויות השונות שעומדות לשרותינו כאשר משתמשים ב-`cargo test`.

[concatenation-with-the--operator-or-the-format-macro]: ch08-02-strings.html#concatenation-with-the--operator-or-the-format-macro
[bench]: ../unstable-book/library-features/test.html
[ignoring]: ch11-02-running-tests.html#ignoring-some-tests-unless-specifically-requested
[subset]: ch11-02-running-tests.html#running-a-subset-of-tests-by-name
[controlling-how-tests-are-run]: ch11-02-running-tests.html#controlling-how-tests-are-run
[derivable-traits]: appendix-03-derivable-traits.html
[doc-comments]: ch14-02-publishing-to-crates-io.html#documentation-comments-as-tests
[paths-for-referring-to-an-item-in-the-module-tree]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
