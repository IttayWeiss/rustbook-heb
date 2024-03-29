## אחסון רשימות ערכים בווקטורים

טיפוס האוסף הראשון בו נדון הוא `Vec<T>`, או בשמו הנפוץ, פשוט _וקטור_. וקטור אינו אלא מבנה נתונים המאפשר לנו לאחסן בו ערך אחד או יותר, כאשר כל ערכיו יאוחסנו זה לצד זה בזיכרון. בראסט, וקטורים מסוגלים לאחסן אך ורק ערכים מאותו הטיפוס. וקטורים עשוים להיות שימושיים כל אימת שנרצה לאחסן רשימת פריטים כלשהי, כגון שורות טקסט מקובץ, או מחירי מוצרים בסופרמרקט.

### יצירת וקטור חדש

כדי ליצור וקטור ריק חדש, אנו קוראים לפונקציה `Vec::new`, כפי שמציגה רשימה 8-1:

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-01/src/main.rs:here}}
```

<span class="caption">רשימה 8-1: יצירת וקטור חדש וריק שיחזיק ערכים מטיפוס `i32`</span>

שימו לב שהוספנו כאן ביאור טיפוס. מכיוון שאנחנו לא "דוחפים" (קרי, מכניסים) ערכים לווקטור הזה, ראסט לא יודעת איזה טיפוס של פריטים אנחנו מתכוונים לאחסן. זו נקודה חשובה. וקטורים מיושמים באמצעות ג’נריקס; אנו נסקור כיצד להשתמש בג’נריקס עם טיפוסים משלכם בפרק 10. לעת עתה, דעו שהטיפוס `Vec<T>` המסופק על ידי הספרייה הסטנדרטית יכול להכיל כל טיפוס. כאשר אנו יוצרים וקטור כדי לאחסון בו ערכים מטיפוס מסוים, אנו יכולים לציין זאת בסוגריים משולשים. ברשימה 8-1, אמרנו לראסט שה-`Vec<T>` שהגדרנו כעת יכיל פריטים מטיפוס `i32`.

לעתים קרובות ניצור `Vec<T>` עם מספר מסוים של ערכים התחלתיים, וראסט תסיק מהם את הטיפוס שברצונך לאחסן, כך שלעתים רחוקות בלבד נידרש לעשות זאת באופן מפורש. לנוחיות המשתמש, ראסט מספקת את המאקרו `vec!`, שיוצר וקטור חדש שמחזיק את הערכים המצוינים בעת הגדרתו. רשימה 8-2 יוצרת `Vec<i32>` חדש שמכיל את הערכים `1`, `2` ו-`3` מטיפוס המספרים השלמים `i32`, כי זהו סוג ברירת המחדל של המספרים השלמים, כפי שראינו בסעיף [“Data Types”][data-types]<!-- ignore --> בפרק 3.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-02/src/main.rs:here}}
```

<span class="caption">רשימה 8-2: יצירת וקטור חדש המכיל ערכים</span>

כאן, מכיוון שצוינו ערכי `i32` התחלתיים, ראסט יכולה להסיק שטיפוס הערכים של `v` הוא `i32`, ולכן ביאור הטיפוס המפורש אינו נחוץ. בהמשך נבחן כיצד לעדכן וקטור קיים.

### עדכון וקטור

כדי ליצור וקטור ולאחר מכן להוסיף לו פריטים, נוכל להשתמש במתודה `push`, כפי שמוצג ברשימה 8-3.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-03/src/main.rs:here}}
```

<span class="caption">רשימה 8-3: שימוש בשיטת `push` בכדי להוסיף ערכים לווקטור קיים</span>

כמו בכל משתנה שאת ערכו אנו מעוניינים לשנות, עלינו להפוך אותו לבר-שינוי באמצעות מילת המפתח `mut`, כפי שראינו בפרק 3. המספרים שאנו מוסיפים לתוכו הם כולם מטיפוס `i32`. ראסט מסוגלת להסיק זאת מן הנתונים עצמם, כך שאנו לא נדרשים להוסיף את הביאור `Vec<i32>`.

### גישה וקריאת פריטים של וקטורים

ישנן שתי דרכים לגשת לערך המאוחסן בווקטור: באמצעות אינדקס, או באמצעות קריאה למתודת `get`. לשם הבהירות, בדוגמאות הבאות ביארנו את טיפוסי הערכים המוחזרים מפונקציות אלה.

רשימה 8-4 מציגה את שני אופני הגישה לערך המאוחסן בווקטור, אחד באמצעות תחביר אינדקס, ושני באמצעות מתודת `get`.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-04/src/main.rs:here}}
```

<span class="caption">רשימה 8-4: שימוש בתחביר אינדקס ובמתודת `get` כדי לגשת לפריט בווקטור</span>

שימו לב לכמה דברים כאן. ראשית, מכיוון שהפריטים המאוחסנים בווקטור ממוספרים החל מהמספר 0, אנו משתמשים בערך האינדקס `2` כדי לגשת אל הפריט השלישי. שנית, שימוש ב-&' ו-'[]' מעניק לנו הפניה לפריט המאוחסן באינדקס המצוין. שלישית, מתודת `get` מקבלת את האינדקס כארגומנט, ומחזירה ערך מטיפוס `Option<&T>`, כך שנוכל להשתמש בו עם ביטוי `match`.

הסיבה שראסט מספקת את שתי הדרכים הללו לגשת לפריט היא כדי לאפשר למשתמש לבחור כיצד התוכנית תתנהג בעת ניסיון גישה לפריט הנמצא אל מעבר לגבולות הווקטור. כדוגמה, בואו נראה מה קורה כשיש לנו וקטור בן חמישה פריטים, וננסה לגשת לפריט באינדקס 100. רשימה 8-5 מציגה שני ניסיונות כאלה.

```rust,should_panic,panics
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-05/src/main.rs:here}}
```

<span class="caption">רשימה 8-5: ניסיון גישה לפריט באינדקס 100 בווקטור בן חמישה פריטים</span>

בעת הריצה, המתודה הראשונה – שימוש בתחביר `[]` -- תגרום לתוכנית להיכנס לפאניקה מכיוון שהיא מתייחסת לפריט שאינו קיים. מומלץ לבחור במתודה זו כאשר אתה רוצה שהתוכנית שלך תתרסק בעת ניסיון לגשת לפריט מעבר לגבול טווח הווקטור.

לעומת זאת, כאשר למתודת `get` מועבר אינדקס שנמצא מחוץ לטווח, היא מחזירה `None` מבלי להיכנס לפאניקה. מומלץ לבחור במתודה זו אם ניסיון גישה לפריט הנמצא מעבר לטווח הווקטור עשוי להתרחש מדי פעם בנסיבות ריצה רגילות. כך יוכל הקוד לטפל ב-`Some(&element)` או `None`, כפי שראינו בפרק 6. לדוגמה, האינדקס יכול להגיע ממשתמש המזין מספר. אם יזין בטעות מספר גדול מדי, התוכנית תקבל ערך `None`, ויהיה ניתן לטפל במקרה בבעיה. תוכלו, למשל, לומר למשתמש כמה פריטים מכיל הווקטור הנוכחי, ולאפשר לו הזדמנות נוספת להזין ערך חוקי. זה פתרון ידידותי-למשתמש הרבה יותר מאשר לאפשר לתכנית לקרוס עקב שגיאת הקלדה!

כאשר לתוכנית מועברת הפניה תקינה, בודק ההשאלות אוכף את כללי הבעלות וההשאלה (כפי שנסקרו בפרק 4) בכדי להבטיח שהפניה זו, כמו כל הפניה אחרת לתוכן הווקטור, אכן יישארו בתוקף. זכור את הכלל לפיו לא ניתן להחזיק בהפניות ברות-שינוי ומנועות-שינוי באותו המתחם. רשימה 8-6 מדגימה כלל זה. אנו מחזיקים בהפניה מנועת-שינוי לפריט הראשון בווקטור, ומנסים להוסיף פריט חדש לסופו. אם ננסה לגשת לפריט זה בהמשך הפונקציה, התוכנית לא תעבוד:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-06/src/main.rs:here}}
```

<span class="caption">רשימה 8-6: ניסיון להוסיף פריט לוקטור תוך החזקת הפניה לפריט</span> ניסיון קומפילציה של קוד זה יפיק את השגיאה הבאה:

```console
{{#include ../listings/ch08-common-collections/listing-08-06/output.txt}}
```

הקוד ברשימה 8-6 עשוי להיראות כאילו הוא אמור לעבוד: מדוע הפניה לפריט הראשון צריכה להדאיג עצמה עם שינויים המתרחשים בסוף הווקטור? שגיאה זו נובעת מהאופן שבו וקטורים פועלים: מעצם הגדרתם, וקטורים מאחסנים את הפריטים שלהם בזיכרון זה לצד זה. לכן, יתכן מצב בו הוספת פריט חדש לסופו של הווקטור תדרוש הקצאת חלקת זיכרון חדשה והעתקת הפריטים הקיימים אליה. במקרה כזה, ההפניה לפריט הראשון תהווה הפניה למיקום בזיכרון 
שאינו תקף יותר. כללי ההשאלה מונעים היתכנות מצב עגום זה.

הערה: כדי לקרוא עוד אותוד פרטי המימוש של הטיפוס `Vec<T>` פנו ל-[“The Rustonomicon”][nomicon].

### איטרציה על הפריטים המאוחסנים בווקטור

הבה נניח שברצוננו לגשת לכל אחד מן הפריטים שבווקטור, אחד אחרי השני. במקום להשתמש בתחביר אינדקס בכדי לגשת אל הפריטים ישירות, זה אחר זה, אנו יכולים לעבור עליהם באיטרציה. רשימה 8-7 מראה כיצד להשתמש בלולאת `for` כדי ליצור הפניות מנועות-שינוי לכל פריט מטיפוס `i32` המאוחסן בווקטור, ולהדפיס אותו אל המסך.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-07/src/main.rs:here}}
```

<span class="caption">רשימה 8-7: הדפסת כל פריטי הווקטור באמצעות איטרציה עם לולאת `for`</span>

עם הפניות ברות-שינוי, אנו יכולים לבצע איטרציה על הווקטור (בר-שינוי אף הוא) בכדי לבצע שינויים בכל פריט ופריט. כך למשל, לולאת ה-'for' ברשימה 8-8 תוסיף '50' לערכו של כל פריט.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-08/src/main.rs:here}}
```

<span class="caption">רישום 8-8: איטרציה באמצעות הפניות ברות-שינוי לפריטים בווקטור</span>

כדי לשנות את הערך אליו מתייחסת ההפניה ברת-השינוי, עלינו להשתמש באופרטור דה-ההפניה, `*`, כדי להגיע לערך המאוחסן ב-`i`. רק אז נוכל להשתמש באופרטור `+=` בכדי לשנותו. נדון בהרחבה באופרטור דה-ההפניה בפרק 15, בסעיף [“מעקב אחר מצביע לעבר הערך באמצעות אופרטור הדה-הפניה”][deref]<!-- ignore -->

בזכות בודק ההשאלה, איטרציה על וקטור תהיה תמיד בטוחה, תהא היא ברת-שינוי או מנועת-שינוי. אם ננסה להכניס או להסיר פריטים בגופי לולאות ה-`for` שברשימות 8-7 ו-8-8, נקבל שגיאת קומפילציה הדומה לזו שהפיק הקוד ברשימה 8-6. לולאת ה-`for` מחזיקה בהפניה לוקטור המונעת שינוי בו-זמני של הווקטור כולו.

### שימוש במבחר לאחסון טיפוסים מרובים

וקטורים יכולים לאחסן רק ערכים מאותו הטיפוס. זה יכול להיות לא-נוח; קיימים בהחלט מקרים בהם נדרש אחסון רשימה של פריטים מטיפוסים שונים. למרבה המזל, וריאנטים של מבחר מוגדרים, כזכור, כטיפוס של אותו המבחר. כלומר, כאשר אנו זקוקים לטיפוס אחד ויחיד בכדי לייצג פריטים מטיפוסים שונים, אנו יכולים להגדיר ולהשתמש במבחר!

לדוגמה, נניח שאנו רוצים לקבל ערכים משורה בגיליון אלקטרוני, בה חלק מן העמודות בשורה מכילות מספרים שלמים, חלקן מכילות מספרי נקודה צפה, וחלקן מחרוזות. נוכל להגדיר מבחר שהווריאנטים שלו יחזיקו את טיפוסי הערכים השונים. כך, כל הווריאנטים הללו יחשבו לאותו הטיפוס - זה של המבחר תחתיו הם מוגדרים. כך נוכל ליצור וקטור שטיפוסו הוא המבחר הרצוי; בכך, למעשה, יוכל הווקטור להכיל טיפוסים שונים. רשימה 8-9 מדגימה זאת:

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-09/src/main.rs:here}}
```

<span class="caption">רישום 8-9: הגדרת מבחר לאחסון ערכים מטיפוסים שונים בווקטור אחד</span>

ראסט צריכה לדעת אילו טיפוסים יכיל הווקטור כבר בזמן הקומפילציה; כך תדע בדיוק כמה זיכרון בערימה יידרש כדי לאחסן כל פריט. עלינו לבאר מפורשות גם אילו טיפוסים מותרים לאחסון בווקטור זה. לו היתה ראסט מאפשרת לוקטור להחזיק יותר מטיפוס אחד, היה עלול אחד או יותר מהם לגרום לשגיאות. כפי שראינו בפרק 6, שימוש במבחר בשילוב ביטוי `match` פירושו שראסט תבטיח כבר בזמן הקומפילציה שכל מקרה אפשרי יטופל.

עם זאת, אם אינכם יודעים בזמן הקומפילציה אילו טיפוסים בדיוק תידרש התוכנית שלכם לקבל ולאחסן, טכניקת המבחר לא תעבוד. במקום זאת, אתם יכולים להשתמש באובייקט תכונה, אותו נסקור בפרק 17.

כעת, לאחר שדנו בכמה מהדרכים הנפוצות ביותר להשתמש בווקטורים, ראו את [תעוד ה-API][vec-api]<!-- ignore --> למידע נוסף אודות כל המתודות השימושיות הרבות שהגדירה ב-`Vec<T>` הספרייה הסנטדרטית. לדוגמה, בנוסף ל-`push`, קיימת מתודת `pop`, המסירה ומחזירה את הפריט האחרון בווקטור.

### עזיבת וקטור עוזבת את הפריטים שבו

כמו כל `מבנה` אחר, וקטור נעזב כאשר הוא יוצא מהמתחם, כפי שמציגה רשימה 8-10.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-10/src/main.rs:here}}
```

<span class="caption">רשימה 8-10: המקום בו וקטור והפריטים שבו נעזבים </span>

כאשר הווקטור נעזב, התוכן שלו נעזב אף הוא. כלומר, המספרים השלמים שמאוחסנים בו יעזבו ולא יהיו זמינים יותר. בודק ההשאלה מבטיח שכל ההפניות לתוכנו של הווקטור מתבצעות רק בזמן שהווקטור עצמו תקף.

בואו נעבור לסוג האוסף הבא: `מחרוזת`!

[data-types]: ch03-02-data-types.html#data-types
[nomicon]: ../nomicon/vec/vec.html
[vec-api]: ../std/vec/struct.Vec.html
[deref]: ch15-02-deref.html#following-the-pointer-to-the-value-with-the-dereference-operator
