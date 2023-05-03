## הגדרה ואתחול של מבנים

מבנים הם די דומים למרצפים, עליהם דיברנו בסעיף [טיפוס המרצף][tuples]<!--
ignore --> , היות ושניהם מאגדים יחדיו כמה ערכים הקשורים זה לזה. כמו רצפים, הפיסות השונות של מבנה יכולות להיות מטיפוסים שונים. אך בניגוד למרצפים, במבנה יש לתת שם לכל פיסת דאטה בכדי לבר במפורש מה משמעות כל ערך. הוספת שמות אלה משמעותה שמבנים גמישים יותר מרצפים: אין צורך להסתמך על הסדר בו מופיע הדאטה בכדי לבצע השמה, או בכדי לגשת לערכים של מופע מסוים של המבנה.

כדי להגדיר מבנה, יש לכתוב את מילת המפתח `struct` ואז לתת שם למבנה כולו. ראוי שהשם הניתן למבנה יתאר את החשיבות של איגוד פיסות הדאטה המסוימות הללו יחדיו. אז, בתוך סוגריים מסולסלים, מגדירים את השמות והטיפוסים של פיסות הדאטה, להם קוראים _שדות_. לדוגמא, רשימה 5-1 מראה מבנה המאחסן מידע אודות חשבון משתמש.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-01/src/main.rs:here}}
```

<span class="caption">רשימה 5-1: הגדרה של המבנה `User`</span>

כדי להשתמש במבנה אחרי שהגדרנו אותו, ניתן ליצור _מופע_ (instance) של המבנה ע"י הזנת ערכים קונקרטיים עבור כל אחד מהשדות. אנחנו יוצרים מופע על-ידי ציון שם המבנה, ואחריו סוגריים מסולסלים המכילים זוגות מפתח וערך, כאשר המפתחות הם שמות השדות, והערכים הם הדאטה שאנחנו רוצים לאכסן בשדות אלה. אין צורך לציין את השדות באותו הסדר בו הכרזנו עליהם במבנה.
במילים אחרות, הגדרת המבנה עצמו היא כמו תבנית כללית עבור הטיפוס, ומופעים ממלאים את התבנית עם דאטה ספציפי. כך נוצרים ערכים של הטיפוס. למשל, אנחנו יכולים להכריז על משתמש ספציפי כפי שמוצג ברשימה 5-2.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-02/src/main.rs:here}}
```

<span class="caption">רשימה 5-2: יצירת מופע של המבנה `User`</span>

על-מנת לגשת לערך ספציפי במבנה, ניתן להשתמש בביאור הנקודה. למשל, כדי לגשת לכתובת האימייל של משתמש, נוכל לכתוב `user1.email`. אם המופע בר-שינוי, אז נוכל גם לשנות ערך באמצעות ביאור הנקודה והשמה לתוך השדה הנדון. רשימה 5-3 מראה איך לשנות את הערך בשדה `email` של מופע בר-שינוי של `User`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-03/src/main.rs:here}}
```

<span class="caption">רשימה 5-3: שינוי הערך בשדה `email` של מופע של `User`</span>

שימו לב שהמופע כולו חייב להיות בר-שינוי; ראסט אינה מאפשרת לנו לסמן רק חלק מהשדות כברי-שינוי. כמו עם כל ביטוי, אנחנו יכולים ליצור מופע חדש של מבנה כביטוי האחרון בגוף פונקציה, ו ערך זה יהיה הערך המוחזר מהפונקציה.

רשימה 5-4 מציגה את הפונקציה `build_user` אשר מחזירה מופע של `User` עם אימייל ושם משתמש. השדה `active` מקבל את הערך `true`, והשדה `sign_in_count` מקבל את הערך `1`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-04/src/main.rs:here}}
```

<span class="caption">רשימה 5-4: הפונקציה `build_user` אשר מקבלת אימייל ושם משתמש ומחזירה מופע של `User`</span>

יהיה זה הגיוני לקרוא לפרמטרים של הפונקציה באותם השמות כמו שדות המבנה, אבל הצורך לחזור על שמות השדות `email` ו- `username` הוא קצת מייגע. אם למבנה היו עוד שדות, הצורך לחזור על כל אחד ואחד משמות השדות היה כבר מעצבן. למרבה המזל, קיים קיצור זמין ונוח!

<!-- Old heading. Do not remove or links may break. -->

<a id="using-the-field-init-shorthand-when-variables-and-fields-have-the-same-name"></a>

### שימוש באתחול שדות מקוצר

בגלל ששמות הפרמטרים ושמות השדות של המבנה ברשימה 5-4 זהים, ניתן להשתמש בתחביר _אתחול שדות מקוצר_ כדי לשכתב את `build_user` כך שתתנהג בדיוק באותו אופן, אך ללא החזרה על `username` ו- `email`, כפי שמוצג ברשימה 5-5.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-05/src/main.rs:here}}
```

<span class="caption">רשימה 5-5: הפונקציה `build_user` שמשתמשת באתחול שדות מקוצר כיוון שלפרמטרים `username` ו-`email` יש את אותם השמות כמו שדות המבנה</span>

אנחנו יוצרים כאן מופע חדש של המבנה `User`, שיש לו שדה בשם `email`. אנחנו רוצים לקבוע את הערך של השדה `email` להיות הערך בפרמטר `email` של הפונקציה `build_user`. משום שלשדה `email` ולפרמטר `email` יש את אותו השם, כל שצריך לעשות זה לכתוב `email`, במקום `email: email`.

### יצירת מופעים של מבנה ממופעים אחרים שלו באמצעות תחביר עדכון מבנה

לעיתים קרובות יש צורך ליצור מופע חדש של מבנה שכולל את הערכים ממבנה אחר כפי שהם, כאשר רק כמה מהם דורשים שינוי. ניתן לעשות זאת באמצעות _תחביר עדכון מבנה_.

ראשית, ברשימה 5-6 אנו מראים כיצד ליצור מופע חדש של `User` ב- `user2` בצורה הרגילה, ללא תחביר העדכון. אנו קובעים ערך חדש ל- `email` אבל שאר השדות מקבלים את ערכיהם מהמופע `user1` שיצרנו ברשימה 5-2.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-06/src/main.rs:here}}
```

<span class="caption">רשימה 5-6: יצירת מופע חדש של `User` תוך שימוש באחד מהערכים של `user1`</span>

בעזרת תחביר עדכון מבנה, ניתן להשיג את אותה התוצאה בפחות קוד, כפי שנראה ברשימה 5-7. התחביר `..` משמעו ששאר השדות במופע החדש, אלה שלא נעשה בהם שימוש מפורש, יקבלו את אותם הערכים כמו בשדות המקבילים של המופע הקיים.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-07/src/main.rs:here}}
```

<span class="caption">רשימה 5-7: שימוש בתחביר עדכון מבנה כדי לקבוע ערך חדש לשדה `email` של המופע `User` ולקבוע את ערכי שאר השדות לפי הערכים ב-`user1`</span>

הקוד ברשימה 5-7 מייצר מופע ב- `user2` בעל ערך שונה לשדה `email` אבל עם אותם ערכים כמו ב- `user1` עבור השדות `username`, `active`, ו- `sign_in_count`. הנוטציה `..user1` חייבת להופיע בשורה האחרונה כדי לציין שכל השדות הנותרים צריכים לקבל את ערכם מהשדות התואמים ב- `user1`, אבל אנחנו חופשיים לקבוע ערכים לכל מספר שהוא של שדות, ובכל סדר שנרצה, ללא תלות בסדר של השדות בהגדרת המבנה.

שימו לב שהתחביר לעדכון מבנה משתמש בסימן `=` כמו השמה; זאת משום שהוא מזיז את הדאטה, בדיוק כפי שראינו בסעיף [אינטראקציה בין משתנים ודאטה באמצעות הזזה][move]<!-- ignore --> . בדוגמה זו, לא ניתן להשתמש ב-`user1` לאחר יצירת `user2` כיוון שה- `String` בשדה `username` של `user1` הוזזה לתוך `user2`. אילו היינו נותנים ל- `user2` ערך חדש של `String` גם עבור `email` וגם עבור `username`, וכך רק משתמשים בערכים של `active` וב- `sign_in_count` מ- `user1`, אז `user1` עדיין יהיה תקף לאחר יצירת `user2`. הטיפוסים `active` ו- `sign_in_count` מיישמים שניהם את התכונה `Copy`, ולכן ההתנהגות עליה דיברנו בסעיף [דאטה שכולו במחסנית: העתקה][copy]<!-- ignore --> בתוקף.

### שימוש במבני מרצף ללא שמות לשדות כדי ליצור טיפוסים שונים

ראסט גם תומכת במבנים הדומים מאוד למרצפים, ונקראים _מבני רצף_ (tuple structs). למבני מרצף יש את המשמעות הנוספת שניתנת להם מהשם של המבנה, אבל לשדות שלהם אין שמות משלהם; רק טיפוסי השדות קיימים. מבני מרצף הם שימושיים כאשר רוצים לתת שם למרצף בכללותו ובכך להפוך את המרצף לטיפוס דאטה השונה ממרצפים אחרים; או, כאשר מתן שם לכל שדה כמו במבנה רגיל, יהיה מיותר או מוגזם.

כדי להגדיר מבנה מרצף, יש להתחיל עם מילת המפתח `struct` ועם שם המבנה, ולהמשיך עם הטיפוסים במרצף. למשל, כאן אנו מגדירים ומשתמשים בשני מבני המרצף `Color` ו-`Point`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-01-tuple-structs/src/main.rs}}
```

שימו לב שהערכים `black` ו-`origin` הם מטיפוס שונה כי הם מופעים של מבני מרצף שונים. כל מבנה שתגדירו הוא טיפוס בפני עצמו, אפילו אם השדות בשני המבנים זהים. לדוגמא, פונקציה שלוקחת פרמטר מטיפוס `Color` לא תקבל מופע של `Point` כפרמטר, אפילו ששני הטיפוסים מורכבים משלושה ערכים מטיפוס `i32`. מלבד זאת, מבני רצף דומים מאוד למרצפים בכך שניתן לפרק אותם לרכיביהם. בנוסף, ניתן גם להשתמש בביאור הנקודה `.` ולאחריה אינדקס בכדי לגשת לרכיב מסויים.

### מבנים דמויי-unit ללא כל שדות

ניתן גם להגדיר מבנים בלי אף שדה כלל! מבנים כאלה נקראים _מבנים דמויי-unit_ מכיוון שהם מתנהגים באופן דומה ל- `()`, טיפוס היחידה שהזכרנו בסעיף [טיפוס הרצף][tuples]<!-- ignore --> . מבנים דמויי-unit יכולים להיות שימושיים כאשר אנו רוצים ליישם תכונה עבור טיפוס מסויים, אבל אין לנו דאטה שרוצים לאכסן בטיפוס עצמו. אנו נדון בתכונות בפרק 10. הינה דוגמא להכרזה ואתחול של מבנה דמוי-unit בשם `AlwaysEqual`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-04-unit-like-structs/src/main.rs}}
```

כדי להגדיר את `AlwaysEqual` אנו משתמשים במילת המפתח `struct`, שם המבנה הרצוי, ומייד אחריו נקודה-פסיק. אין צורך בסוגריים (מסולסלים או עגולים)! אח"כ אנחנו יכולים ליצור מופע של `AlwaysEqual` במשתנה `subject` בדרך דומה: שימוש בשם שהגדרנו, ללא כל סוגריים. דמיינו שבשלב מאוחר יותר נממש התנהגות עבור טיפוס זה כך שכל מופע של `AlwaysEqual` תמיד שווה לכל מופע של כל טיפוס אחר, למשל בכדי ליצור תוצאה ידועה מראש למטרת בדיקות. לא נצטרך שום דאטה כדי ליישם התנהגות זו! תראו בפרק 10 כיצד להגדיר תכונות וליישם אותן עבור כל טיפוס שהוא, כולל מבנים דמויי-unit.

> ### בעלות על דאטה במבנה
>
> בהגדרה של המבנה `User` ברשימה 5-1, השתמשנו ב-`String`, טיפוס עם בעלות על הדאטה שלו, ולא בטיפוס חיתוך מחרוזת `&str`. זוהי בחירה מכוונת, שנעשתה כיוון שאנחנו רוצים שכל מופע של מבנה זה יהיה הבעלים של הדאטה שלו, ושהדאטה יהיה תקף כל עוד המבנה כולו תקף.
>
> מבנים יכולים גם לאחסן הפניות לדאטה שבבעלותו של מישהו אחר, אךכדי לעשות זאת יש להשתמש _במשך-חיים _ (lifetime), תכונה של ראסט בה נדון בפרק 10. משך חיים מבטיח שהדאטה אליו מפנים מתוך מופע של מבנה תקף כל עוד המופע תקף. הבה נניח שאתם מנסים לאחסן הפנייה במבנה ללא שימוש במשך חיים, כמו בדוגמא הבאה; שאינה עובדת:
>
> <span class="filename">Filename: src/main.rs</span>
>
> <!-- CAN'T EXTRACT SEE https://github.com/rust-lang/mdBook/issues/1127 -->
>
> ```rust,ignore,does_not_compile
> struct User {
>     active: bool,
>     username: &str,
>     email: &str,
>     sign_in_count: u64,
> }
>
> fn main() {
>     let user1 = User {
>         active: true,
>         username: "someusername123",
>         email: "someone@example.com",
>         sign_in_count: 1,
>     };
> }
> ```
>
> הקומפיילר יתלונן שהוא צריך מצייני משך חיים:
>
> ```console
> $ cargo run
>    Compiling structs v0.1.0 (file:///projects/structs)
> error[E0106]: missing lifetime specifier
>  --> src/main.rs:3:15
>   |
> 3 |     username: &str,
>   |               ^ expected named lifetime parameter
>   |
> help: consider introducing a named lifetime parameter
>   |
> 1 ~ struct User<'a> {
> 2 |     active: bool,
> 3 ~     username: &'a str,
>   |
>
> error[E0106]: missing lifetime specifier
>  --> src/main.rs:4:12
>   |
> 4 |     email: &str,
>   |            ^ expected named lifetime parameter
>   |
> help: consider introducing a named lifetime parameter
>   |
> 1 ~ struct User<'a> {
> 2 |     active: bool,
> 3 |     username: &str,
> 4 ~     email: &'a str,
>   |
>
> For more information about this error, try `rustc --explain E0106`.
> error: could not compile `structs` due to 2 previous errors
> ```
>
> בפרק 10, נראה כיצד לתקן שגיאות אלה כך שתוכלו לאחסן הפניות במבנים, אבל לבינתיים נתקן בעיות כגון אלה תוך שימוש בטיפוסים עם בעלות כמו `String` במקום הפניות כמו `&str`.

<!-- manual-regeneration
for the error above
after running update-rustc.sh:
pbcopy < listings/ch05-using-structs-to-structure-related-data/no-listing-02-reference-in-struct/output.txt
paste above
add `> ` before every line -->

[tuples]: ch03-02-data-types.html#the-tuple-type
[tuples]: ch03-02-data-types.html#the-tuple-type
[move]: ch04-01-what-is-ownership.html#variables-and-data-interacting-with-move
[copy]: ch04-01-what-is-ownership.html#stack-only-data-copy
