## הפרדת מודולים לקבצים שונים

עד כה, הדוגמאות בפרק זה הגדירו מספר מודולים בקובץ אחד. כאשר מודולים הופכים לארוכים יותר, יתכן ותרצו להעביר את ההגדרות בהם לקובץ נפרד על מנת להקל על ההתמצאות בקוד.

למשל, הבה נתחיל עם הקוד ברשימה 7-17, בו יש מספר מודולי מסעדות. במקום לרכז את כל המודולים בקובץ בסיס המכולה, אנו נוציא מודולים לתוך קבצים נפרדים. במקרה זה, בסיס המכולה הוא _src/lib.rs_, אבל דרך עבודה זו שאנו מדגימים תקפה גם למכולות בינאריות, בהן קובץ בסיס המכולה הוא _src/main.rs_.

ראשית, נוציא את המודול `front_of_house` לקובץ משלו. הסירו את הקוד שבתוך הסוגריים המסולסלים של המודול `front_of_house`, והשאירו רק את ההכרזה `mod front_of_house;`, כך ש- _src/lib.rs_ יכיל את הקוד המוצג ברשימה 7-21. שימו לב שהקוד לא יעבור קומפילציה עד שניצור את הקובץ _src/front_of_house.rs_ ברשימה 7-22.W

<span class="filename">Filename: src/lib.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-21-and-22/src/lib.rs}}
```

<span class="caption">רשימה 7-21: הכרזה על המודול `front_of_house` שגופו ימצא בקובץ _src/front_of_house.rs_</span>

עכשיו, צרו קובץ בשם _src/front_of_house.rs_ ומקמו בתוכו את הקוד שהיה בסוגריים המסולסלים, כפי שמוצג ברשימה 7-22. הקומפיילר יודע לחפש את הקובץ הזה בגלל שהוא מצא בבסיס המכולה את ההכרזה על המודול `front_of_house`.

<span class="filename">Filename: src/front_of_house.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-21-and-22/src/front_of_house.rs}}
```

<span class="caption">רשימה 7-22: הגדרות פנימיות של המודול `front_of_house` בקובץ _src/front_of_house.rs_</span>

שימו לב שיש לטעון קובץ באמצעות הכרזת `mod` רק _פעם אחת_ בעץ המודולים שלכם. ברגע שהקומפיילר יודע שהקובץ הוא חלק מהפרוייקט (ויודע את המקום בעץ המודולים בו הקוד נמצא, לפי המיקום של פקודת ה-`mod`), קבצים אחרים בפרוייקט צריכים להפנות לקוד מהקובץ הנטען תוך שימוש במסלול המצביע למקום בו הוא הוגדר, כפי שהוסבר בסעיף ["מסלולים להפניות של עצמים בעץ המודולים"][paths]<!-- ignore --> . במילים אחרות, `mod` היא פקודה _שונה_ מ-"include" שמוכרת לכם, אולי, משפות תכנות אחרות.

עכשיו נפנה למיצוי המודול `hosting` לקובץ משלו. התהליך שונה במעט משום ש- `hosting` הוא מודול-בן של `front_of_house`, ולא של מודול הבסיס. נמקם את הקובץ עבור `hosting` בתיקיה חדשה בשם של מודול-האב בעץ המודולים, במקרה זה _src/front_of_house/_.

בכדי להתחיל להעביר את `hosting`, נשנה את _src/front_of_house.rs_ כך שיכיל רק את ההכרזה על המודול `hosting`:

<span class="filename">Filename: src/front_of_house.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/no-listing-02-extracting-hosting/src/front_of_house.rs}}
```

ואז ניצור את הספריה _src/front_of_house_ ואת הקובץ _hosting.rs_ כך שיכיל את ההגדרות במודול `hosting`:

<span class="filename">Filename: src/front_of_house/hosting.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/no-listing-02-extracting-hosting/src/front_of_house/hosting.rs}}
```

אם, במקום זאת, נמקם את _hosting.rs_ בתיקייה _src_, הקומפיילר יצפה למצוא את הקוד של _hosting.rs_ במודול `hosting` שמוכרז בבסיס המכולה, ולא שמוכרז כמודול-בן של `front_of_house`. כללי הקומפיילר לגבי באילו קבצים לחפש את הקוד עבור מודולים נתונים, מובילים לכך שמבנה עץ המודולים שנוצר דומה למבנה התיקיות בפרויקט.

> ### מסלולי קבצים אלטרנטיבים
>
> עד כה הצגנו את מסלולי הקבצים האידיאומטים ביותר בשימוש הקומפיילר של ראסט, אבל ראסט תומכת גם בסגנון ישן יותר של מסלולים. עבור מודול בשם `front_of_house` המוכרז בבסיס המכולה, הקומפיילר יחפש את הקוד של המודול במקומות הבאים:
>
> - בקובץ _src/front_of_house.rs_ (כפי שכבר ראינו)
> - בקובץ _src/front_of_house/mod.rs_ (סגנון ישן יותר, שעדיין נתמך)
>
> עבור מודול בשם `hosting` שהוא תת-מודול של `front_of_house`, הקומפיילר יחפש את הקוד של המודול במקומות האלה:
>
> - בקובץ _src/front_of_house/hosting.rs_ (כפי שכבר ראינו)
> - בקובץ _src/front_of_house/hosting/mod.rs_ (סגנון ישן יותר, שעדיין נתמך)
>
> במידה ותשתמשו בשני הסגנונות עבור אותו מודול, תקבלו שגיאת קומפילציה. כן ניתן לערב את שני הסגנונות עבור מודולים שונים באותו הפרוייקט, אבל זה עלול להיות מבלבל עבור מפתחים שינסו לנווט את הקוד שלכם.
>
> המגרעה המרכזית של הסגנון שמשתמש בקבצים בשם _mod.rs_ היא שהפרוייקט שלכם עשוי להכיל הרבה קבצים בשם _mod.rs_, מה שעלול לגרום לבלבול כאשר כמה מהקבצים האלה פתוחים לעריכה בעורך הטקסט שלכם באותו זמן.

ובכן, העברנו את הקוד של כל אחד מהמודולים לקובץ נפרד, ועץ המודולים נשאר כשהיה. הקריאות לפונקציות ב-`eat_at_restaurant` ימשיכו לעבוד ללא כל צורך בהתאמות, אפילו שההגדרות נמצאות בקבצים שונים. טכניקה זו מאפשרת לכם להזיז מודולים לקבצים משלהם, במידת הצורך.

שימו לב גם שהפקודה `pub use crate::front_of_house::hosting` בקובץ _src/lib.rs_ לא השתנתה, וכן שלשימוש ב-`use` אין כל השפעה על אלו קבצים עוברים קומפילציה כחלק מהמכולה. מילת המפתח `mod` מכריזה על מודולים, וראסט מחפשת בקובץ בעל אותו שם כמו השם של המודול את הקוד של המודול.

## סיכום

ראסט מאפשרת לכם לפצל חבילה לכמה מכולות, ומכולה לכמה מודולים. כך, על-ידי ציון מסלולים אבסולוטיים או יחסיים, ניתן להפנות ממודול אחד לפריטים המוגדרים במודול אחר. ניתן להכניס מסלולים אלה למתחם באמצעות פקודת `use`, וכך להשתמש במסלול קצר יותר לשימוש חוזר עבור פריט מסויים במתחם. כברירת מחדל, המתחם של מודול הוא פרטי, אך ניתן להגדירו כפומבי על-ידי הוספת מילת המפתח `pub`.

בפרק הבא נתבונן בכמה טיפוסי נתונים מאגדים (collection data types) שנמצאים בספריה הסטנדרטית וזמינים לשימושכם כחלק מהקוד שלכם, שכעת ביכולתכם לארגן למופת.

[paths]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
