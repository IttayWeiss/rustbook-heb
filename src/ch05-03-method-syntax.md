## תחביר מתודות

_מתודות_ דומות מאוד לפונקציות: מגדירים אותן עם מילת המפתח `fn` שלאחריה שם; הן יכולות לקבל פרמטרים ולהחזיר ערך, והן מכילות קוד שמבוצע כאשר הן נקראות על-ידי קוד ממקום אחר. בניגוד לפונקציות, מתודות מוגדרות בהקשר של מבנה (או מבחר, או תכונה, כפי שנראה [בפרק 6][enums]<!-- ignore --> וכן [בפרק 17][trait-objects]<!-- ignore -->, בהתאמה), והפרמטר הראשון שלהן הוא תמיד `self`, שמייצג מופע של המבנה עליו המתודה נקראת.

### הגדרת מתודות

הבה נשנה את הפונקציה `area` שמקבלת מופע של `Rectangle` כפרמטר. נהפוך אותה כעת למתודה המוגדרת על המבנה `Rectangle`, כמוצג ברשימה 5-13.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-13/src/main.rs}}
```

<span class="caption">רשימה 5-13: הגדרת המתודה `area` על המבנה `Rectangle`</span>

כדי להגדיר את הפונקציה בהקשר של `Rectangle`, נתחיל בלוק `impl` (יישום, implementation) עבור `Rectangle`. כל מה שבתוך בלוק `impl` זה יהיה משויך לטיפוס `Rectangle`. לאחר-מכן, אנו מעבירים את הפונקציה `area` לאזור התחום ע"י הסוגריים המסולסלים של ה-`impl` ומשנים, בחותם המתודה, את הפרמטר הראשון (ובמקרה זה, גם היחיד) ל-`self`, ומשתמשים ב-`self` בגוף המתודה. בפונקציה `main`, היכן שקראנו לפונקציה `area` והעברנו את `rect1` כארגומנט, נוכל כעת להשתמש _בתחביר המתודות_ כדי לקרוא למתודה `area` על המופע הנוכחי של המבנה `Rectangle`. תחביר המתודות משייך את המופע למתודה: מוסיפים נקודה אחרי המופע, אחריה את שם המתודה, ואז סוגריים וארגומנטים, אם ישנם.

בחותם של `area`, אנו משתמשים ב-`&self` במקום ב- `rectangle: &Rectangle`. ה- `&self` הוא בעצם קיצור עבור `self: &Self`. בתוך בלוק `impl`, הטיפוס `Self` הוא כינוי לטיפוס עבורו בלוק ה-`impl` מיושם, או במילים פשוטות – מיושם על עצמו. בשפת ראסט, מתודות מחויבות לקבל, כפרמטר הראשון בחותם, פרמטר בשם `self` מטיפוס `Self`, או `self` על דרך הקיצור.
שימו לב שאנחנו עדיין צריכים להשתמש ב- `&` לפני ה- `self` המקוצר, כדי לציין שמתודה זו שואלת את המופע של `Self`, בדיוק כפי שעשינו עם `rectangle: &Rectangle`. בדיוק כמו עם משתנים אחרים, מתודות יכולות לקחת בעלות על `self`, לשאול את `self` בצורה מנועת-שינוי (כפי שעשינו כאן), או לשאול את `self` בצורה ברת-שינוי, בדיוק כמו עם כל פרמטר אחר.

בחרנו להשתמש ב- `&self` כאן מאותה הסיבה שהשתמשנו ב- `&Rectangle` בגרסת הפונקציה: אנחנו לא רוצים לקחת בעלות על המופע. כל שאנו רוצים הוא לקרוא את הדאטה במבנה, לא לכתוב אליו. אם היינו רוצים, כחלק מפעולת המתודה, לשנות את המופע עליו המתודה נקראת, היינו משתמשים ב- `&mut self` כפרמטר הראשון. זה די נדיר שמתודה לוקחת בעלות על המופע של עצמה, קרי, ש-`self` מופיע ללא הסימן `&`. עושים שימוש בטכניקה זו במקרים בהם רוצה המתכנת שהמתודה תהפוך את `self` לטיפוס מסוג אחר, ולכן יש למנוע מהקורא למתודה מלהשתמש במופע המקורי לאחר השינוי.

שתי סיבות חשובות לשימוש במתודות במקום בפונקציות היא, ראשית, היכולת להשתמש בתחביר המתודות, ושנית, העובדה שאין צורך לחזור על הטיפוס של `self` בחותמים של מתודות. עם זאת, הסיבה העיקרית לכך היא סיבה ארגונית – מיקמנו בתוך בלוק ה- `impl` את כל מה שניתן לעשות עם מופע של טיפוס נתון. כך, חסכנו למשתמשים עתידיים של הקוד שלנו את הצורך לנבור בנפרד בתיעוד הקוד בכדי לתור אחר יכולות של `Rectangle` .

שימו לב שאנחנו יכולים לתת למתודה את אותו השם כמו אחד מהשדות במבנה. למשל, ניתן להגדיר מתודה על `Rectangle` בשם `width`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-06-method-field-interaction/src/main.rs:here}}
```

כאן, אנחנו בוחרים שהמתודה `width` תחזיר `true` אם הערך שבשדה `width` של המופע גדול מ-`0`, ו-`false` אם הערך הוא `0`: ניתן להשתמש בשדה בתוך מתודה בעלת אותו שם לכל מטרה. בפונקציה `main`, כאשר אנחנו קוראים ל- `rect1.width` עם סוגריים עגולים מיד לאחר-מכן, ראסט יודעת שהכוונה היא למתודה `width`. אם לא משתמשים בסוגריים, ראסט תדע שהכוונה היא לשדה `width`.

לעיתים קרובות (אך לא תמיד), כאשר אנו נותנים למתודה את אותו השם כמו אחד השדות במבנה, זה מכיוון שאנו רוצים שהיא תחזיר את הערך של השדה המדובר, ותו לא. מתודות כאלה מכונות _getters_, ובניגוד לשפות אחרות, ראסט לא מיישמת אותן באופן אוטומטי. מתודות `getters` כאלה הן יעילות כי הן מאפשרות לשדות להיות פרטיים בעוד המתודה היא פומבית, וכך מתאפשרת גישת קריאה-בלבד לשדה כחלק מה-API הפומבי של הטיפוס. אנחנו נדון במשמעות של פומביות ופרטיות, וכיצד להצהיר על שדות ומתודות כפומביות או פרטיות, [בפרק 7][public]<!-- ignore -->.

> ### איפה האופרטור `<-`?
>
> ב-C וב-++C, משתמשים בשני אופרטורים שונים לקריאה למתודות: משתמשים ב- `.` כאשר קוראים למתודה ישירות על אובייקט, וב- `->` כאשר קוראים למתודה על מצביע לאובייקט, כך שנדרשת קודם דה-הפניה למצביע. במילים אחרות, אם `object` הוא מצביע, אז `object->something()` שקול ל- `(*object).something()`.
>
> לראסט אין מקבילה לאופרטור `->`; במקום זאת, לראסט יש תכונה שנקראת _הפנייה ודה-הפניה אוטומטיים_. קריאה למתודה היא אחד המקומות הבודדים בראסט בה מיושמת תכונה זו.
>
> כך זה עובד: כאשר אתם קוראים למתודה באמצעות `object.something()`, ראסט מוסיפה אוטומטית או `&`, `&mut` או `*`, כך ש- `object` תואם את החותם של המתודה. במילים אחרות, האפשרויות הבאות שקולות:
>
> <!-- CAN'T EXTRACT SEE BUG https://github.com/rust-lang/mdBook/issues/1127 -->
>
> ```rust
> # #[derive(Debug,Copy,Clone)]
> # struct Point {
> #     x: f64,
> #     y: f64,
> # }
> #
> # impl Point {
> #    fn distance(&self, other: &Point) -> f64 {
> #        let x_squared = f64::powi(other.x - self.x, 2);
> #        let y_squared = f64::powi(other.y - self.y, 2);
> #
> #        f64::sqrt(x_squared + y_squared)
> #    }
> # }
> # let p1 = Point { x: 0.0, y: 0.0 };
> # let p2 = Point { x: 5.0, y: 6.5 };
> p1.distance(&p2);
> (&p1).distance(&p2);
> ```
>
> האופציה הראשונה נקייה וברורה יותר לקריאה. יכולת זו לבצע הפנייה אוטומטית היא אפשרית משום שלכל מתודה יש אובייקט ברור עליו היא מיושמת (קרי, האובייקט המקבל, receiver) – זהו הטיפוס של `self`. בהינתן האובייקט המקבל ושם המתודה, ראסט יכולה לפענח בוודאות האם המתודה היא מתודה קוראת (`&self`), מתודה משנה (`&mut self`), או מתודה צורכת (`self`). במילים פשוטות, בראסט ,השאלות מבוצעות בצורה מרומזת בזמן קריאה למתודות. זו סיבה מרכזית מדוע עיקרון הבעלות הוא קל לשימוש ויישום בשימוש יום-יומי.

### מתודות עם יותר פרמטרים

הבה נתרגל את השימוש במתודות באמצעות ישום מתודה שניה על המבנה `Rectangle`. הפעם אנו רוצים שמופע של `Rectangle` יקבל מופע אחר של `Rectangle` ויחזיר `true` אם ה-`Rectangle` השני מתאים לחלוטין בתוך `self` (ה-`Rectangle` הראשון); אחרת, יש להחזיר את הערך `false`. כלומר, ברגע שנגדיר את המתודה `can_hold`, נוכל לכתוב את התכנית ברשימה 5-14.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-14/src/main.rs}}
```

<span class="caption">רשימה 5-14: שימוש במתודה (שעוד לא נכתבה) `can_hold`</span>

הפלט המצופה יראה כך, משום שמימדי `rect2` קטנים מהמימדים של `rect1`, בעוד ש- `rect3` רחב יותר מ-`rect1`:

```text
Can rect1 hold rect2? true
Can rect1 hold rect3? false
```

אנו רוצים כעת להגדיר מתודה, ולכן נפנה לבלוק `impl Rectangle`. שם המתודה יהיה `can_hold`, והיא תקבל כפרמטר השאלה מנועת-שינוי למופע של `Rectangle`. אנחנו יכולים לדעת מה יהיה טיפוס הפרמטר על-ידי התבוננות בקוד שקורא למתודה: `rect1.can_hold(&rect2)` מעביר את `&rect2`, שהוא הפניה מנועת-שינוי ל-`rect2`, מופע של `Rectangle`. זה הגיוני מכיוון שאנחנו רק צריכים לקרוא מ- `rect2` (לו היינו צריכים לכתוב אליו, היה עלינו להשתמש בהפניה ברת-שינוי), ואנו רוצים ש- `main` תשמור על הבעלות של `rect2` כדי שנוכל להשתמש בו מאוחר יותר, בקריאה למתודה `can_hold`. הערך המוחזר מ- `can_hold` יהיה בוליאני, והיישום יוודא אם הרוחב והגובה של `self` גדולים מהרוחב והגובה של ה-`Rectangle` השני, בהתאמה. הבה נוסיף את המתודה החדשה `can_hold` לבלוק ה-`impl` מרשימה 5-13, כפי שמוצג ברשימה 5-15.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-15/src/main.rs:here}}
```

<span class="caption">רשימה 5-15: יישום של המתודה `can_hold` עבור `Rectangle` שמקבלת מופע שני של `Rectangle` כפרמטר</span>

כאשר נריץ קוד זה עם הפונקציה `main` מרשימה 5-15, נקבל את הפלט הרצוי. מתודות יכולות לקבל כמה פרמטרים בצורה דומה לפונקציות. פשוט מוסיפים את הפרמטרים לחותם המתודה, לאחר הפרמטר הראשון, שהוא `self`.

### פונקציות משויכות

כל הפונקציות שמוגדרות בתוך בלוק `impl` נקראות _פונקציות משויכות _, משום שהן משויכות לטיפוס המצויין מיד לאחר מילת המפתח `impl`. אם הן לא צריכות מופע של הטיפוס בכדי לרוץ, ניתן להגדיר פונקציות משויכות גם ללא `self` כפרמטר הראשון שלהן (במקרה כזה, הן יהיו פונקציות, ולא מתודות). כבר השתמשנו בפונקציה אחת כזו: הפונקציה `String::from` המוגדרת על הטיפוס `String`.

לרוב משתמשים בפונקציות משויכות שאינן מתודות כפונקציות בונות (constructors) שמחזירות מופע חדש של המבנה. לפונקציות כאלה קוראים לרוב `new`, אבל חשוב לזכור ש-`new` אינו שם מיוחד ואינו בנוי לתוך השפה. למשל, אנחנו יכולים לבחור לספק פונקציה משויכת בשם `square` בעלת פרמטר יחיד, ולהשתמש בו עבור הרוחב והגובה, ובכך להקל על יצירת `Rectangle` מרובע במקום להידרש לציין את אותו הערך פעמיים:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-03-associated-functions/src/main.rs:here}}
```

מילת המפתח `Self` בערך ההחזרה ובגוף הפונקציה היא כינוי עבור הטיפוס המופיע לאחר מילת המפתח `impl`, ובמקרה שלנו זה `Rectangle`.

כדי לקרוא לפונקציה משויכת זו, משתמשים בסימן התחבירי `::` יחד עם שם המבנה; `let sq = Rectangle::square(3);` היא דוגמה לכך. פונקציה זו ממוקמת לפי המבנה: בתחביר `::` משתמשים גם עבור פונקציות משויכות וגם עבור מיקומים שנוצרים במודולים. במודולים נדון [בפרק 7][modules]<!-- ignore -->.

### ריבוי בלוקי `impl`

לכל מבנה יכולים להיות כמה בלוקים מסוג `impl`. למשל, הקוד ברשימה 5-15 שקול לקוד המופיע ברשימה 5-16, שבו כל מתודה נמצאת בבלוק `impl` משלה.

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-16/src/main.rs:here}}
```

<span class="caption">רשימה 5-16: שכתוב של רשימה 5-15 תוך שימוש בריבוי `impl`</span>

אמנם אין כאן סיבה אמיתית להפריד מתודות אלה לבלוקי `impl` שונים, אבל זהו תחביר תקני. בפרק 10, כאשר נדון בטיפוסים גנריים ובתכונות, נראה מצב בו ריבוי בלוקיי `impl` הופך לשימושי.

## סיכום

מבנים מאפשרים לכם ליצור טיפוסים בעלי משמעות עבור תחום הפעולה שלכם. באמצעות שימוש במבנים, ניתן לאגד פיסות דאטה נפרדות, אך קשורות זו לזו, לישות אחת בעלת שמות משמעותיים לדאטה כולו, ולמרכיבים השונים. בתוך בלוקי `impl`, ניתן להגדיר פונקציות המשויכות לטיפוס. מתודות הן סוג מסוים של פונקציות משויכות שמאפשרות לכם להגדיר את ההתנהגות של מופעים מסוג המבנה שלכם.

אבל מבנים אינם הדרך היחידה ליצור טיפוסים משלכם: הבה נפנה כעת לנושא של מבחרים בראסט בכדי להעשיר את תיבת הכלים שלכם.

[enums]: ch06-00-enums.html
[trait-objects]: ch17-02-trait-objects.md
[public]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html#exposing-paths-with-the-pub-keyword
[modules]: ch07-02-defining-modules-to-control-scope-and-privacy.html
