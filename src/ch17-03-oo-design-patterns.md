## מימוש דפוס תכנון מונחה-עצמים

*דפוס מצבים* הינו דפוס תכנון מונחה-עצמים. עיקר הדפוס הוא שאנו מגדירים קבוצה של מצבים פנימיים בהם ערך יכול להימצא. המצבים מיוצגים על-ידי קבוצה של *אובייקטי מצב*, וההתנהגות של הערך משתנה בהתבסס על מצבו. אנו מתעתדים לעבוד עם דוגמא של מבנה של הודעת בלוג שבו יש שדה לאכסון של מצב, שיהיה אחד מאובייקטי המצב "draft", "review", ו- "published".

אובייקטי המצב משתפים פונקציונאליות: בראסט, כמובן, אנו משתמשים במבנים ובתכונות ולא בעצמים ובירושה. כל אובייקט מצב אחראי להתנהגות שלו ולשליטה על מעבר שלו בין מצבים. הערך שמאכסן את אובייקט המצב אינו יודע דבר אודות ההתנהגות השונה של המצבים או על המעברים בין מצבים.

היתרון שבשימוש בדפוס המצבים הוא שכאשר הדרישות העסקיות על התכנית משתנות, אין צורך לשנות את הקוד של הערך שמאכסן את המצב או את הקוד שמשתמש בערך. כל שיהיה לעשות הוא לעדכן את הקוד בתוך אחד מאובייקטי המצב כדי לשנות את החוקיות לפיו הוא פועל או, אולי, להוסיף אובייקטי מצב נוספים.

ראשית, אנו הולכים לממש את דפוס המצבים בצורה מונחת-עצמים מסורתית, ואחר-כך ננקוט בגישה טבעית יותר למימוש בראסט. הבה נעמיק בנושא על-ידי מימוש הדרגתי של דרך-עבודה של הודעת בלוג תוך שימוש בדפוס המצבים.

הפונקציונאליות הסופית תראה כך:

1. הודעת בלוק מתחילה כטיוטא ריקה.
2. כאשר טיוטא גמורה, מועברת בקשת עריכה.
3. כאשר ההודעה מאושרת, היא מתפרסמת.
4. רק הודעות בלוג שפורסמו מחזירות תוכן להדפסה, זאת בכדי למנוע הדפסה בטעות של תוכן שלו אושר.

לכל ניסיון אחר לשנות הודעות לא צריכה להיות שום השפעה. למשל, אם ננסה לאשר טיוטא של הודעת בלוג לפני שביקשנו עריכה, ההודעה צריכה להישאר במצב של טיוטא שלא פורסמה.

רשימה 17-11 מציגה תזרים עבודה זה בצורת קוד: זוהי דוגמת שימוש ב-API שנממש במכולת ספריה בשם `blog`. קוד זה עוד לא יעבור קומפילציה מכיוון שלא ממשנו את המכולה `blog`.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch17-oop/listing-17-11/src/main.rs:all}}
```


<span class="caption">רשימה 17-11: קוד המדגים את ההתנהגות הרצויה עבור המכולה `blog`</span>

אנחנו מעוניינים לאפשר למשתמש ליצור טיוטאת הודעת בלוג חדשה באמצעות `Post::new`. אנחנו רוצים גם לאפשר הוספת טקסט להודעת הבלוג. אם ננסה לקבל את תוכן ההודעה מייד, לפני שהיא אושרה, אז צריך לא לקבל שום טקסט בגלל שההודעה עדיין במצב טיוטא. הוספנו כמה שורות `assert_eq!` בקוד למטרת הדגמה. מקרה מבחן מצויין לכך יהיה לטעון שטיוטא להודעת בלוג מחזירה מחרוזת ריקה מהמתודה `content`, אבל אנו לא נכתוב מבחנים כאלה עבור דוגמא זו.

עכשיו, אנו רוצים לאפשר בקשה לעריכה של ההודעה, ואנחנו רוצים ש-`content` תחזיר מחרוזת ריקה בזמן שאנו מחכים לעריכה. כאשר ההודעה מקבלת אישור, עליה להתפרסם, ולכן כאשר קוראים ל-`content` יש לקבל את הטקסט של ההודעה.

שימו לב שהטיפוס היחיד מהמכולה איתו אנו באים במגע הוא הטיפוס `Post`. טיפוס זה ישתמש בדפוס המצבים ויאכסן ערך שיהיה אחד משלושה אובייקטי מצב אפשריים המייצגים את המצבים השונים בהם הודעה יכולה להימצא -- טיוטא, ממתין לעריכה, או מפורסם. המעברים בין המצבים ינוהלו פנימית כחלק ממימוש הטיפוס `Post`. המצבים משתנים בתגובה לקריאות למתודות שמשתמשי הספריה מבצעים על מופע של `Post`, אבל הם לא צריכים לנהל את מעברי המצבים ישירות. בנוסף, משתמשים אינם יכולים לבצע פעולות פעולות שגויות בקשר למצבים, כמו לפרסם הודעה לפני שהיא עברה עריכה.

### הגדרת `Post` ויצירת מופע חדש במצב טיוטא

הבה נתחיל במלאכת מימוש הספריה! אנחנו יודעים שאנו צריכים את המבנה הפומבי `Post` שצריך לכלול תוכן, ולכן נתחיל עם הגדרת המבנה והפונקציה הפומבית המשוייכת `new` ליצירת מופע של `Post`, כפי שמוצג ברשימה 17-12. בנוסף, ניצור גם את התכונה הפרטית `State` שתגדיר את ההתנהגות שכל אובייקטי המצב עבור `Post` חייבים לספק.

כך המבנה `Post` יכלול אובייקט תכונה של `Box<dyn State>` בתוך `Option<T>` בשדה פרטי בשם `state` בכדי לאכסן את אובייקט המצב. עוד מעט תראו מדוע השימוש ב-`Option<T>` הכרחי.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch17-oop/listing-17-12/src/lib.rs}}
```


<span class="caption">רשימה 17-12: הגדרת המבנה `Post` והפונקציה `new` שיוצרת מופע חדש של `Post`, התכונה `State`, ומבנה `Draft`</span>

התכונה `State` מגדירה התנהגות משותפת עבור מצבי הודעה שונים. אובייקטי המצב הם `Draft`, `PendingReview`, ו- `Published`, והם יממשו כולם את התכונה `State`. לעת עתה, לתכונה אין אף מתודה, ואנו מתחילים רק עם הגדרת המצב `Draft` כיוון שזהו המצב ההתחלתי של כל הודעה.

כאשר אנו יוצרים מופע חדש של `Post`, אנו מאתחלים את השדה `state` לערך `Some` המכיל `Box`. `Box` זה מצביע אל מופע חדש של המבנה `Draft`. כך מובטח שבכל פעם שניצור מופע חדש של `Post`, המצב התחילי שלו יהיה טיוטא. כיוון שהשדה `state` של `Post` הוא פרטי, אין דרך ליצור מופע של `Post` במצב אחר! בפונקציה `Post::new` אנו מאתחלים את השדה `content` עם מחרוזת ריקה חדשה.

### אכסון הטקסט של של תוכן ההודעה

ברשימה 17-11 ראינו שאנחנו רוצים להיות מסוגלים לקרוא למתודה בשם `add_text` ולהעביר לה `&str` שאז מוסף כתוכן הטקסט של הודעת הבלוג. אנו מממשים זאת כמתודה, במקום לחשוף את השדה `content` כ- `pub`, כך שיותר מאוחר נוכל לממש מתודה שתשלוט על אופן הקריאה של הדאטה שבשדה `content`. המתודה `add_text` די ברורה מאליה, אז הבה נוסיף את המימוש ברשימה 17-13 לבלוק `impl
Post`:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch17-oop/listing-17-13/src/lib.rs:here}}
```


<span class="caption">רשימה 17-13: מימוש המתודה `add_text` שמוסיפה טקסט לשדה `content` של הודעה</span>

המתודה `add_text` מקבלת הפניה ברת-שינוי ל- `self` בגלל שאנחנו משנים את המופע של `Post` שעליו קוראים ל-`add_text`. לאחר מכן אנו קוראים ל-`push_str` על המחרוזת ב- `content` ומעבירים את `text` כארגומנט אשר יתווסף ל- `content`. התנהגות זו אינה תלויה במצב בו ההודעה נמצאת, ולכן היא לא מהווה חלק מדפוס המצבים. המתודה `add_text` כלל אינה באה במגע עם השדה `state`, אבל היא חלק מההתנהגות שאנחנו רוצים לספק.

### ווידוא שהתוכן של טיוטת הודעה הוא ריק

לאחר קריאה ל- `add_text` והוספת תוכן להודעה שלנו, אנחנו עדיין רוצים שהמתודה `content` תחזיר חיתוך מחרוזת ריק כיוון שההודעה עדיין במצב של טיוטה, כפי שמוצג בשורה 7 של רשימה 17-11. לבינתיים, הבה נממש את המתודה `content` בדרך הפשוטה ביותר שתבטיח דרישה זו: תמיד להחזיר חיתוך מחרוזת ריק. כמובן שנשנה זאת ברגע שנממש את היכולת לשנות את המצב של ההודעה כך שתוכל להתפרסם. עד כה, הודעות יכולות להיות אך ורק במצב טיוטא, ולכן תוכן ההודעה תמיד יהיה ריק. רשימה 17-14 מראה מימוש שומר-מקום זה:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch17-oop/listing-17-14/src/lib.rs:here}}
```


<span class="caption">רשימה 17-14: הוספת מימוש שומר-מקום עבור המתודה `content` על המבנה `Post` שתמיד מחזיר חיתוך מחרוזת ריק</span>

עם מימוש זה של המתודה `content`, כל הקוד המופיע ברשימה 17-11 עד שורה 7 עובד כנדרש.

### בקשת עריכה של ההודעה משנה את מצב ההודעה

עכשיו, אנחנו צריכים להוסיף פונקציונאליות עבור בקשה לעריכה של ההודעה, ובקשה זו צריכה לשנות את מצב ההודעה מ- `Draft` ל- `PendingReview`. רשימה 17-15 מציגה קוד זה:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch17-oop/listing-17-15/src/lib.rs:here}}
```


<span class="caption">רשימה 17-15: מימוש המתודה `request_review` על המבנה `Post` והתכונה `State`</span>

אנחנו מממשים עבור `Post` מתודה פומבית בשם `request_review` שמקבלת הפניה ברת-שינוי ל- `self`. ואז אנחנו קוראים למתודה פנימית בשם `request_review` על המצב הנוכחי של `Post`, והמתודה הפנימית הזאת מכלה את המצב הנוכחי ומחזירה מצב חדש.

אנחנו מוסיפים את המתודה `request_review` לתכונה `State`; כל הטיפוסים שמממשים את התכונה יהיו חיימים למממש את המתודה `request_review`. שימו לב שבמקום להשתמש ב- `self`, `&self`, או ב-`&mut self` כפרמטר הראשון למתודה, אנחנו משתמשים ב- `self: Box<Self>`. תחביר זה משמעו שהמתודה תקיפה רק כאשר קורים לה על `Box` המכילה את הטיפוס. תחביר זה לוקח בעלות על ה- `Box<Self>`, ומבטל את המצב הישן כך שניתן להעביר את המצב של ה-`Post` למצב חדש.

על מנת לכלות את המצב הישן, המתודה `request_review` צריכה לקחת בעלות על ערך המצב. כאן נכנס לתמונה השימוש ב- `Option` בשדה `state` של `Post`: אנו קוראים למתודה `take` על מנת לקחת את ערך ה- `Some` מתוך השדה `state` ולהשאיר `None` במקומו, כי ראסט לא מאפשרת שדות נטולי ערכים במבנים. כך אנחנו יכולים להעביר את הערך `state` מחוץ ל-`Post` במקום לשאול אותו. ואז נוכל לעדכן את הערך של `state` לתוצאת פעולה זו.

עלינו לעדכן את `state` ל- `None` באופן זמני במקום לעדכן אותו ישירות באמצעות קוד כמו `self.state = self.state.request_review();` בשביל לקבל בעלות על הערך ב- `state`. דבר זה מבטיח ש- `Post` לא יכול להשתמש בערך הישן של `state` לאחר שהעברנו אותו למצב חדש.

המתודה `request_review` על `Draft` מחזירה מופע חדש, בתוך קופסה, של המבנה `PendingReview`, שמייצג את המצב כאשר הודעה ממתינה לעריכה. המבנה `PendingReview` גם מממש את המתודה `request_review` אבל לא מבצע שום מעבר. במקום זאת הוא רק מחזיר את עצמו, כיוון שכאשר אנחנו מבצעים בקשת עריכה על הודעה שכבר במצב של `PendingReview`, היא צריכה פשוט להישאר באותו מצב.

כעת נוכל להתחיל לראות את היתורנות של דפוס המצבים: המתודה `request_review` על `Post` היא אותה מתודה, בלי קשר לערך ה- `state` שלה. כל מצב אחראי על החוקיות שלו.

נשאיר את מימוש המתודה `content` על `Post` כמות שהיא, דהיינו להחזיר חיתוך מחרוזת ריק. עכשיו `Post` יכול להיות במצב `PendingReview` וגם במצב `Draft`, אבל אנחנו רוצים את אותה ההתנהגות במצב `PendingReview`. בשלב זה רשימה 17-11 עובדת כהלכה עד שורה 10!

<!-- Old headings. Do not remove or links may break. -->
<a id="adding-the-approve-method-that-changes-the-behavior-of-content"></a>

### הוספת `approve` כדי לשנות את ההתנהגות של `content`

המתודה `approve` תהיה דומה למתודה `request_review`: היא תעדכן את `state` לערך מתאים לפי החוקיות בדבר קריאה לאישור של המצב הנוכחי, כמוצג ברשימה 17-16:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch17-oop/listing-17-16/src/lib.rs:here}}
```


<span class="caption">רשימה 17-16: מימוש של המתודה `approve` על המבנה `Post` והתכונה `State`</span>

אנו מוסיפים את המתודה `approve` לתכונה `State` ומוסיפים מבנה חדש, המצב `Published`, שמממש את `State`.

דרך הפעולה של `PendingReview`דומה לזו של `request_review`. אם אנו קוראים למתודה `approve` על `Draft`, לא תהיה לכך השפעה כיוון ש-`approve` יחזיר `self`. כאשר קוראים ל- `approve` על `PendingReview`, מוחזר מופע חדש, בתוך קופסה, של המבנה `Published`. המבנה `Published` מממש את התכונה `State`, ועבור המתודות `request_review` ו-`approve` מוחזר הערך עצמו, בגלל שההודעה צריכה להישאר במצב `Published` במקרים אלה.

כעת אנחנו צריכים לעדכן את המתודה `content` על `Post`. אנחנו רוצים שהערך המוחזר מ- `content` יהיה תלוי במצב הנוכחי של `Post`, ולכן `Post` יהיה צריך להעביר אחריות למתודה `content` שמוגדרת על ה-`state` שלו, כפי שרואים ברשימה 17-17:

<span class="filename">Filename: src/lib.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch17-oop/listing-17-17/src/lib.rs:here}}
```


<span class="caption">רשימה 17-17: עדכון המתודה `content` על `Post` כדי להעביר אחריות למתודה `content` על `State`</span>

כיוון שהמטרה היא לשמור את כל החוקים האלה פנימיים למבנים שמממשים את `State`, אנו קוראים למתודה `content` על הערך ב-`state` ומעבירים את המופע של ההודעה (זאת אומרת `self`) כארגומנט. ואז אנחנו מחזירים את הערך שמוחזר מהקריאה למתודה `content` על הערך ב- `state`.

אנחנו קוראים למתודה `as_ref` על ה- `Option` כי אנחנו רוצים הפניה לערך שבתוך ה- `Option` ולא לקחת בעלות על הערך. כיוון ש-`state` הוא `Option<Box<dyn State>>`, כאשר אנו קוראים ל- `as_ref` אנו מקבלים חזרה ערך מטיפוס `Option<&Box<dyn
State>>`. לולא הקריאה ל- `as_ref` היינו מקבלים שגיאה כיוון שלא ניתן להזיז את הערך ב- `state` מחוץ לערך השאול מטיפוס `&self` של פרמטר הפונקציה.

לאחר מכן אנו קוראים למתודה `unwrap`, שבביטחון לא תגרום לפאניקה, בגלל שאנחנו יודעים שהמתודות על `Post` מוודאות ש- `state` תמיד יכיל ערך `Some` כאשר המתודות מסתיימות. זהו אחד המקרים עליהם דיברנו בסעיף ["מקרים בהם המתכנת יודע יותר מהקומפיילר"][more-info-than-rustc]<!-- ignore --> בפרק 9 כאשר ידוע לנו שערך `None` אינו אפשרי, אפילו שהקומפיילר לא יכול להסיק זאת.

בנקודה זו, כאשר אנו קוראים ל- `content` על `&Box<dyn State>`, כפיית די-הפניה תפעל על ה- `&` וה- `Box` כך שהמתודה `content` תיקרא לבסוף על הטיפוס שמממש את התכונה `State`. משמעות הדבר היא שאנחנו צריכים להוסיף את `content` להגדרת התכונה `State`, וזה המקום בו נממש את הלוגיקה אודות איזה תוכן להחזיר בהתאם למצב בו אנו נמצאים, כפי שרואים ברשימה 17-18:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch17-oop/listing-17-18/src/lib.rs:here}}
```


<span class="caption">רשימה 17-18: הוספת המתודה `content` לתכונה `State`</span>

אנו מוסיפים מימוש ברירת מחדל עבור המתודה `content` שמחזיר חיתוך מחרוזת ריק. משתמע מכך שאנחנו לא צריכים לממש את `content` על המבנים `Draft` ו-`PendingReview`. המבנה `Published` יעקוף את המימוש של המתודה `content` ויחזיר את הערך ב-`post.content`.

שימו לב שיש צורך להשתמש בביאור משך חיים עבור מתודה זו, בהתאם לנידון בפרק 10. אנחנו לוקחים הפניה למופע של `post` כארגומנט ומחזירים הפניה לחלק מהמופע הזה, ולכן משך החיים של הערך ההפניה המוחזרת קשורה למשך החיים של הארגומנט מטיפוס `post` שמועבר למתודה.

ובזאת סיימנו -- רשימה 17-11 עובדת במלואה! מימשנו את דפוס המצבים עם חוקיות של תזרים עבודה של הודעות בלוג. הלוגיקה הקשורה לחוקים חיה באובייקטי המצבים ולא מפוזרת ברחבי המימוש של `Post`.

> #### מדוע לא להשתמש במבחר?
> 
> ייתכן ותהיתם מדוע לא השתמשנו במבחר עם ווראינטים עבור המצבים השונים של הודעות הבלוג. זהו בהחלט פתרון אפשרי, נסו זאת והשוו את התוצאה הסופית כדי וראו מה אתם מעדיפים! חיסרון אחד של שימוש במבחר הוא שכל מקום בוא מתבצעת בדיקה מול ווריאנטים של המבחר יצריך ביטוי `match`  או מנגנון דומה לטיפול בכל ווריאנט אפשרי. זה עלול להיות יותר רפטטיבי מאשר בפתרון המיישם אובייקטי תכונה.

### פשרות יעילות של דפוס המצבים

הראנו שראסט מסוגלת למממש את דפוס המצבים, שהו דפוס מונחה-העצמים, על מנת לאפסן את הסוגים השונים של התנהגות שהודעת בלוג צריכה לאפשר בכל מצב. המתודות שמוגדרות על `Post` לא יודעות דבר אודות התנהגויות אלה. בדרך בה ארגנו את הקוד עלינו לחפש רק במקום אחד כדי לדעת מהן הדרכים השונות בהן הודעה שפורסמה יכולה להתנהג: המימוש של התכונה `State` על המבנה `Published`.

אם היינו יוצרים מימוש אלטרנטיבי שלא משתמש בדפוס המצבים, היינו יכולים להשתשמ בביטויי `match` במתודות על `Post` או אפילו בקוד שב-`main` הבודק את המצב של ההודעה ומשנה התנהגות במקומות אלה. זה היה דורש מאיתנו להתבונן בכמה מקומות בקוד כדי להבין את כל ההשלכות שיש לכך שהודעה נמצאת במצב מפורסם! וזה ילך ויסתבך ככל שנוסיף עוד מצבים: כל אחד מביטויי ה-`match` האלה יצטרך לקבל זרוע נוספת.

עם דפוס המצבים, המתודות על `Post` והמקומות בהם אנו משתמשים ב- `Post` לא צריכים ביטויי `match`, וכדי להוסיף מצב חדש, כל שיש לעשות הוא להוסיף מבנה חדש וליישם את מתודות התכונה על מבנה זה.

המימוש באמצעות דפוס מצבים קל להרחבה בכדי להוסיף פונקציונאליות. כדי לראות את הפשטות של תחזוק קוד שמשתמש בדפוס מצבים, נסו כמה מההצעות הבאות:

* הוסיפו מתודת `reject` שמשנה את מצב ההודעה מ-`PendingReview` חזרה ל-`Draft`.
* דרשו שתי קריאות ל- `approve` לפני שהמצב משתנה ל-`Published`.
* אפשרו למשתמשים להוסיף טקסט רק כאשר הודעה נמצאת במצב `Draft`. רמז: וודאו שאובייקט המצב אחראי למה שיכול להשתנות בקשר לתוכן אבל לא אחראי לשינויים של ה-`Post`.

מגרעה אחת של דפוס המצבים היא שמשום שהמצבים מממשים את המעברים בין מצבים, כמה מהמצבים מקושרים אחד עם השני. אם נרצה להוסיף מצב נוסף בין `PendingReview` ו- `Published`, כמו למשל `Scheduled`, נאלץ לשנות את הקוד ב- `PendingReview` כך שיעבור למצב `Scheduled` במקום. אם `PendingReview` לא היה צריך להשתנות בעקבות הוספה של מצב חדש, אז היתה פחות עבודת תחזוק, אבל זה מצריך מעבר לדפוס תכנון אחר.

מגרעה נוספת היא ששכפלנו חלק מהלוגיקה התפעולית. כדי להימנע מחלק מהשכפול, אנו יכולים לנסות מימושי ברירת מחדל עבור המתודות `request_review` ו-`approve` על התכונה `State` שמחזירים `self`; אולם, זה יפר את בטיחות האובייקטים, משום שהתכונה לא יודעת איזה טיפוס קונקרטי ה- `self` יהיה. אנחנו רוצים לאפשר להשתמש ב- `State` כבאובייקט תכונה, ולכן חייבים שהמתודות שלו יהיו בטוחות לאובייקטים.

כפילות נוספת כוללת את המימושים הדומים זה לזה של המתודות `request_review` ו- `approve` על `Post`. שתי המתודות מעבירות את האחריות למימוש של אותה המתודה על הערך בשדה `state` של ה- `Option` וקובעות את הערך החדש של השדה `state` להיות התוצאה. לו היו לנו הרבה מתודות על `Post` שעוקבות אחד דפוס זה, יתכן ועדיף להגדיר מאקרו שייתר את השכפול (ראו סעיף [“Macros”][macros]<!-- ignore --> בפרק 19).

על-ידי מימוש דפוס המצבים בדיוק כמו שהוג מוגדר עבור שפות מונחות-עצמים אנחנו לא מנצלים עד תום את החוזקות של ראסט. הבה נביט בכמה שינויים שניתן בבצע למכולה `blog` שיכולים להפוך מצבים לא תקינים ומעברי מצבים לא תקינים לשגיאות בזמן הקומפילציה.

#### קידוד מצבים והתנהגות כטיפוסים

נראה לכם כיצד לחשוב אחרת על דפוס המצבים ולקבל מערכת שונה של פשרות יעילות. במקום לאפסן את המצבים והמעברים במלואם כך שלקוד חיצוני אין גישה אליהם בכלל, נקודד את המצבים לתוך טיפוסים שונים. בעקבות זאת, מערכת בדיקת הטיפוסים של ראסט תמנע נסיונות לשימוש בהודעות במצטב טיוטא במקרים בהם מותרות רק הודעות מפורסמות על-ידי שגיאת קומפילציה.

הבה נתבונן בחלק הראשון של הפונקציה `main` ברשימה 17-11:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch17-oop/listing-17-11/src/main.rs:here}}
```

אנחנו עדיין מאפשרים יצירת הודעות חדשות במצב טיוטא על-ידי שימוש ב-`Post::new` ואת היכולת להוסיף טקסט לתוכן ההודעה. אבל במקום להשתמש במתודה `content` על טיוטת הודעה שמחזירה מחרוזת ריקה, במימוש שלנו עכשיו להודעות במצב טיוטא כלל לא תהיה את המתודה `content`. באופן זה, אם ננסה לגשת לתוכן טיוטת הודעה, נקבל שגיאת קומפילציה האומרת לנו שהמתודה אינה קיימת. כתוצאה מכך יהיה זה בלתי אפשרי להציג בטעות את התוכן של טיוטות, מכיוון שהקוד לא יעבור קומפילציה. רשימה 17-19 מראה את ההגדרות של המבנה `Post` ושל המבנה `DraftPost`, וכן מתודות עבור כל אחד מהם:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch17-oop/listing-17-19/src/lib.rs}}
```


<span class="caption">רשימה 17-19: המבנה `Post` יחד עם המתודה `content` והמבנה `DraftPost` ללא המתודה `content`</span>

לשני המבנים, `Post` ו- `DraftPost`, יש שדה פרטי בשם `content` שמאכסן את הטקסט של הודעת הבלוג. למבנים אין את השדה `state` כיוון שאנחנו מעבירים את קידוד המצב לטיפוסים של המבנים. המבנה `Post` ייצג הודעה מפורספת, ויש לו את המתודה `content` שמחזריה את `content`.

עדיין יש לנו את הפונקציה `Post::new`, אבל במקום להחזיר מופע של `Post`, היא מחזירה מופע של `DraftPost`. היות והשדה `content` הוא פרטי וכיוון שאין אף פונקציה שמחזירה ערך מטיפוס `Post`, לא ניתן, בשלב זה, ליצור מופע של `Post`.

למבנה `DraftPost` יש את המתודה `add_text`, כך שניתן להוסיף טקסט ל-`content` כמקודם, אבל שימו לב שהמתודה `content` לא מוגדרת עובר `DraftPost`! בשלב זה, התכנית מבטיחה שכל ההודעות יתחילו כטיוטות, ושהתוכן של הודעות במצב טיוטא אינו ניתן להצגה. כל ניסיון לעקוף מגבלות אלה יגרור שגיאת קומפילציה.

#### מימוש מעברים בין מצבים כתמורות בין טיפוסים

אם כן, כיצד נאפשר גישה לתוכן של הודעה שפורסמה? אנחנו רוצים לחייב את הכלל לפיו טיוטת הודעה חייבת לעבור עריכה ואישור לפני שהיא מתפרסמת. אסור שהודעה במצב של המתנה לעריכה תספק תוכן. הבה נממש אילוצים אלה על-ידי הוספת מבנה נוסף, `PendingReviewPost`, ונגדיר את המתודה `request_review` על `DraftPost` כך שתחזיר ערך מטיפוס `PendingReviewPost`, ונגדיר את המתודה `approve` על `PendingReviewPost` כך שתחזיר ערך מטיפוס `Post`, כפי שמוצג ברשימה 17-20:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch17-oop/listing-17-20/src/lib.rs:here}}
```


<span class="caption">רשימה 17-20: המבנה `PendingReviewPost` שמיוצר על-ידי קריאה למתודה `request_review` על המבנה `DraftPost`, והמתודה `approve` שמחזירה ערך מטיפוס `PendingReviewPost` למופע `Post` מפורסם</span>

המתודות `request_review` ו- `approve` לוקחות בעלות על `self`, ולכן מכלות את המופעים של `DraftPost` ושל `PendingReviewPost` ומתמירות אותם למופעים של `PendingReviewPost` ושל `Post` מפורסם, בהתאמה. בצורה זו, לא יהיו שום מופעים מתדלדלים של `DraftPost` לאחר שקוראים למתודה `request_review` עליהם. למבנה `PendingReviewPost` אין מתודת `content` שמוגדרת עליו, ולכן כל ניסיון לקרוא את תוכנו יגרור שגיאת קומפילציה, וכמו כן עבור `DraftPost`. כיוון שהדרך היחידה לקבל מופע של `Post` שעליו כן מוגדרת המתודה `content` היא לקרוא למתודה `approve` על מופע של `PendingReviewPost`, והיות והדרך היחידה לקבל מופע של `PendingReviewPost` היא לקרוא למתודה `request_review` על מופע של `DraftPost`, הצלחנו לקודד את תזרים העבודה של הבלוג לתוך מערכת הטיפוסים.

אבל, עלינו לעשות עוד כמה שינויים קלים לקוד בפונקציה `main`. המתודות `request_review` ו-`approve` מחזירות מופעים חדשים במקום לשנות את המופעים של המבנה עליהם קוראים אותן, ולכן צריך להוסיף יותר השמות מהצורה `let post =` כדי לאכסן את המופעים המוחזרים. בנוסף, הבדיקות אודות התוכן של הודעות במצבים של טיוטא והמתנה לעריכה הפכו לחסרות מובן, ולמעת האמת כלל לא צריך אותן יותר: כבר אי אפשר לקמפל קוד שמנסה להשתמש בתוכן של הודעות במצבים אלה. הקוד המעודכן לפונקציה `main` מוצג ברשימה 17-21:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch17-oop/listing-17-21/src/main.rs}}
```


<span class="caption">רשימה 17-21: התאמות לפונקציה `main` על מנת להשתמש במימוש החדש לתזרים העבודה של הבלוג</span>

משמעות השינויים שהיה עלינו לבצע בפונקציה `main` כדי שההשמה ל- `post` תהיה תקינה היא שמימוש זה לא עוקב במדוייק אחר דפוס המצבים מונחה-העצמים: התמורות בין המצבים כבר לא מאופסנים במלואם בתוך המימוש של `Post`. אולם, הרווח שלנו הוא שמצבים לא תקינים הם פשוט בלתי-אפשריים תודות למערכת הטיפוסים ובדיקת הטיפוסם שמתרחשת בזמן הקומפילציה! דבר זה מבטיח שבאגים מסויימים, כמו הצגת תוכן של הודעות לא מפורסמות, יתגלו לפני שהקוד מושק למשתמש.

נסו לבצע את המשימות המוצאות בתחילת סעיף זה על מכולת ה-`blog` במצבה הנוכחי לאחר רשימה 17-21 כל מנת לחוות את דעתכם אודות התכנון של גרסה זו של הקוד. שימו לב שכמה ממשימות אלה ניתנות להשלמה מיידית ברמת התכנון.

ראינו שאפילו שניתן לממש בראסט עקרונות תכנות מונחה-עצמים, תבניות אחרות, כמו קידוד מצבים לתוך מערכת הטיפוסים, זמינות גם הן בראסט. לתבניות אלה יש פשרות יעילות שונות. למרות שאתם עשויים להיות בקיאים בתבניות מונחות-עצמים, עיצוב מחדש של הבעיה על מנת לנצל את התכונות של ראסט עשוי לספק יתרונות, כגון איתור באגים מסויימים בזמן הקומפילציה. תבניות מונחות-עצמים אינן בהכרח הפתרון היעיל ביותר בראסט הודות לתכונות מסויימות, כמו בעלות, שלא נמצאים בשפות מונחות-עצמים.

## סיכום

ללא תלות בתשובתכם לשאלה האם ראסט היא שפה מונחת-עצמים או לא, לאחר קריאת פרק זה אתם יודעים שניתן להשתמש באובייקטי תכונה כדי לקבל תכונות מונחות-עצמים בראסט. שיגור דינמי יכול לספק לקוד שלכם גמישות מסויימת בעלות מסויימת של יעילות זמן הריצה. ניתן להשתמש בגמישות זו כדי לממש תבניות תכנות מונחה-עצמים שישפרו את יכולת התחזוק של הקוד שלכם. לראסט יש גם תכונות אחרות, כמו בעלות, שנעדרות משפות מונחות-עצמים. אין הכרח שתבניות תכנות מונחה-עצמים תמיד תהיה הדרך הטובה ביותר לנצל את החוזקות של ראסט, אבל הן זמינות כאופציה.

כעת נפנה להתבונן בתבניות, תכונה אחרת של ראסט שמאפשרת גמישות רבה. כבר נתקלנו בהן במהלך הספר אבל עוד לא חווינו את מלוא יכולתן. הגיעה הזמן!

[more-info-than-rustc]: ch09-03-to-panic-or-not-to-panic.html#cases-in-which-you-have-more-information-than-the-compiler
[macros]: ch19-06-macros.html#macros
