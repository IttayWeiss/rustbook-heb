## הכנסת מסלולים למתחם באמצעות מילת המפתח `use`

הצורך לציין מסלול מדויק עבור כל קריאה לפונקציה יכול להיות חוזרני ולא נוח. ברשימה 7-7, בין אם אנו משתמשים במסלול האבסולוטי או היחסי לפונקציה `add_to_waitlist`, בכל פעם שרצינו לקרוא ל-`add_to_waitlist` היה עלינו לציין גם את `front_of_house` ו-`hosting`. למרבה המזל, ישנה דרך פשוטה יותר: ניתן ליצור קיצור דרך למסלול באמצעות מילת המפתח `use` פעם אחת, ואז להשתמש בשם המקוצר באותו המתחם.

ברשימה 7-11 אנחנו מכניסים את המודול `crate::front_of_house::hosting` למתחם של הפונקציה `eat_at_restaurant` כך שאנחנו רק צריכים לציין `hosting::add_to_waitlist` בשביל לקרוא לפונקציה `add_to_waitlist` ב-`eat_at_restaurant`.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-11/src/lib.rs}}
```

<span class="caption">רשימה 7-11: הכנסת מודול למתחם באמצעות `use`</span>

הוספת `use` ומסלול במתחם מסוים דומה ליצירת symbolic link במערכת הקבצים. ע"י הוספת `use crate::front_of_house::hosting` בבסיס המכולה, `hosting` הופך להיות שם תקף במתחם, כאילו שהמודול `hosting` עצמו הוגדר בבסיס המכולה. מסלולים המוכנסים למתחם עם `use` עונים לדרישות הפרטיות, כמו כל מסלול אחר.

שימו לב כי מילת המפתח `use` יוצרת את קיצור הדרך רק במתחם בו היא מופיעה. רשימה 7-12 מעבירה את הפונקציה `eat_at_restaurant` למודול-בן חדש בשם `customer`, שהוא מתחם שונה מן המתחם בו מופיעה פקודת ה-`use`, ולכן גוף הפונקציה לא עובר קומפילציה:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness,does_not_compile,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-12/src/lib.rs}}
```

<span class="caption">רשימה 7-12: פקודת `use` תקפה רק במתחם בו היא מופיעה</span>

שגיאת הקומפילציה מראה שקיצור הדרך אינו תקף עוד בתוך המודול `customer`:

```console
{{#include ../listings/ch07-managing-growing-projects/listing-07-12/output.txt}}
```

שימו לב שמתקבלת גם אזהרה המודיעה כי `use` כבר לא נמצא במתחם שלו! כדי לתקן את המצב, יש להעביר גם את ה-`use` לתוך המודול `customer`, או להפנות אל קיצור הדרך במודול-האב באמצעות `super::hosting` מתוך מודול-הבן `customer`.

### יצירת מסלולי `use` אידיאומטים

בהקשר לרשימה 7-11, יתכן ותהיתם מדוע כתבנו `use
crate::front_of_house::hosting` ולאחר מכן קראנו ל-`hosting::add_to_waitlist` ב-`eat_at_restaurant` במקום לציין את מסלול ה-`use` כל הדרך אל `add_to_waitlist` כדי להשיג את אותה התוצאה, כמו ברשימה 7-13.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-13/src/lib.rs}}
```

<span class="caption">רשימה 7-13: הכנסת הפונקציה `add_to_waitlist` למתחם באמצעות `use`, בצורה שאינה אידאומטית</span>

למרות שגם רשימה 7-11 וגם רשימה 7-13 משיגות את אותה המטרה, רשימה 7-11 היא הדרך האידאומטית להכנסת פונקציה למתחם באמצעות `use`. המשמעות של הכנסת מודול-האב של הפונקציה למתחם באמצעות `use` היא שעלינו לציין את מודול-האב בעת קריאה לפונקציה. ציון מודול-האב בזמן קריאה לפונקציה מבהיר את העובדה שהפונקציה אינה מוגדרת לוקאלית, וזאת תוך חזרה מינימלית על המסלול המלא. מהקוד ברשימה 7-13 לא ברור היכן הפונקציה `add_to_waitlist` מוגדרת.

מצד שני, כאשר מכניסים למתחם מבנים, מבחרים, ושאר עצמים באמצעות `use`, הדרך האידאומטית היא לציין את המסלול המלא. רשימה 7-14 מראה את הדרך האידאומטית להכניס את המבנה `HashMap` מהספריה הסטנדרטית לתוך המתחם של מכולה בינארית.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-14/src/main.rs}}
```

<span class="caption">רשימה 7-14: הכנסת `HashMap` למתחם בדרך אידאומטית</span>

אין סיבה של ממש מאחורי אידאומה זו: זוהי פשוט המוסכמה שהתגבשה מעצמה; הקהילה התרגלה לקרוא ולכתוב קוד ראסט בצורה זו.

היוצא מן הכלל לאידאומה זו הוא מקרה בו אנחנו מכניסים למתחם שני עצמים בעלי אותו שם באמצעות פקודת `use`, כיוון שראסט לא מאפשרת זאת. רשימה 7-15 מראה כיצד להכניס למתחם שני טיפוסי `Result` להם יש את אותו שם אבל מודולי-אב שונים, וכיצד להפנות אליהם.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-15/src/lib.rs:here}}
```

<span class="caption">רשימה 7-15: הכנסת שני טיפוסים בעלי אותו שם לאותו המתחם מצריכה שימוש במודולי-האב שלהם.</span>

כפי שאתם רואים, שימוש במודולי-האב מבדיל בין שני טיפוסי ה- `Result`. לו היינו מציינים `use std::fmt::Result` ו-`use std::io::Result`, אז היו לנו שני טיפוסי `Result` באותו המתחם, וראסט לא היתה יודעת לאיזה מהם אנו מתכוונים כאשר אנו מפנים ל- `Result`.

### אספקת שמות חדשים באמצעות מילת המפתח `as`

ישנו פיתרון נוסף לבעיה של הכנסת שני טיפוסים עם אותו שם למתחם אחד באמצעות `use`: אחרי המסלול, ניתן לכתוב `as` ולציין שם מקומי חדש, או _כינוי_, עבור הטיפוס. רשימה 7-16 מציגה דרך נוספת לכתוב את הקוד מרשימה 7-15 על-ידי שינוי השם של אחד מטיפוסי ה-`Result` תוך שימוש ב-`as`.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-16/src/lib.rs:here}}
```

<span class="caption">רשימה 7-16: שינוי שם של טיפוס בזמן הכנסתו למתחם באמצעות מילת המפתח `as`</span>

בפקודת ה-`use` השניה בחרנו את השם החדש `IoResult` עבור הטיפוס `std::io::Result` כדי להימנע מההתנגשות עם ה-`Result` מ-`std::fmt` שגם אותו הכנסנו למתחם. גם רשימה 7-15 וגם רשימה 7-16 נחשבות אידאומטיות, ולכן הבחירה באיזה סגנון להשתמש נמצאת בידיכם!

### יצוא מחדש של שמות באמצעות `pub use`

כאשר מכניסים שם לתוך מתחם באמצעות מילת המפתח `use`, השם במתחם הוא פרטי. על מנת לאפשר לקוד שקורא לקוד שלנו להפנות לשם הזה כאילו הוא היה מוגדר במתחם של הקוד, ניתן לשלב את `pub` עם `use`. טכניקה זו נקראת _יצוא מחדש_ משום שאנחנו מכניסים עצם למתחם אבל גם הופכים את העצם זמין בשביל שאחרים יוכלו להכניסו למתחם שלהם.

רשימה 7-17 מראה את הקוד מרשימה 7-11 בהחלפה של `use` בבסיס המודול ל-`pub use`.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-17/src/lib.rs}}
```

<span class="caption">רשימה 7-17: הפיכת שם לזמין לכל קוד, מכל מתחם, באמצעות `pub use`</span>

לפני שינוי זה, קוד חיצוני היה צריך לקרוא לפונקציה `add_to_waitlist` תוך שימוש במסלול `restaurant::front_of_house::hosting::add_to_waitlist()`. השימוש ב- `pub
use` גרם ליצוא מחדש של המודול `hosting` מבסיס המודול, ולכן קוד חיצוני יכול כעת פשוט להשתמש במסלול `restaurant::hosting::add_to_waitlist()`.

יצוא מחדש יעיל כאשר המבנה הפנימי של הקוד שלכם שונה מהאופן בו סביר שמתכנתים אחרים, שרק קוראים לקוד שלכם, יחשבו אודות מבנה הקוד שלכם. לדוגמא, במטפורת המסעדה, האנשים המנהלים את המסעדה חושבים במושגים של "קדמת הבית" ו-"אחורי הבית." אבל סביר מאוד שלקוחות המבקרים במסעדה לא יחשבו במונחים אלה. באמצעות `pub use` יש ביכולתנו לכתוב את הקוד במבנה מסוים הנוח לנו, בעוד אנו חושפים מבנה אחר לשימוש ציבורי. בכך אנו בונים ספריה מאורגנת היטב גם עבור מתכנתים המפתחים את הספריה, וגם עבור מתכנתים המשתמשים בה. אנו נראה דוגמה נוספת לשימוש ב- `pub use`, והשפעתו על תיעוד המכולה, בסעיף ["יצוא של API פומבי נוח באמצעות `pub use`"][ch14-pub-use]<!-- ignore --> בפרק 14.

### שימוש בחבילות חיצוניות

בפרק 2 כתבנו פרויקט משחק ניחוש מספר שהשתמש בחבילה החיצונית `rand` כדי לעבוד עם מספרים אקראיים. על מנת להשתמש ב-`rand` בפרוייקט שלנו, הוספנו את השורה הבאה לקובץ _Cargo.toml_:

<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch02-00-guessing-game-tutorial.md
* ch14-03-cargo-workspaces.md
-->

<span class="filename">Filename: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-02/Cargo.toml:9:}}
```

הוספת `rand` כתלות ב-_Cargo.toml_ אומרת לקארגו להוריד את `rand` ואת כל התלותות שלה מ-[crates.io](https://crates.io/) ולהפוך את `rand` זמינה עבור הפרוייקט שלנו.

ואז, כדי להכניס הגדרות מ-`rand` למתחם של החבילה שלנו, הוספנו שורת `use` שמתחילה עם שם החבילה, דהיינו `rand`, וציינו את שמות העצמים שרצינו להכניס למתחם. זכרו שבסעיף ["יצירת מספר אקראי"][rand]<!-- ignore --> בפרק 2, הכנסנו את התכונה `Rng` למתחם וקראנו לפונקציה `rand::thread_rng`:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-03/src/main.rs:ch07-04}}
```

חברי קהילת ראסט בנו חבילות רבות והן זמינות ב-[crates.io](https://crates.io/). יבוא כל אחת מהן לשימוש בחבילות משלכם מצריכה בדיוק את שתי הפעולות האלה: ציון החבילות לשימוש בקובץ _Cargo.toml_ של החבילה שלכם, ושימוש ב-`use` כדי להכניס את העצמים מהחבילות האלה לתוך המתחם שלכם.

שימו לב שהספריה הסטנדרטית `std` גם היא מכולה חיצונית לחבילה שלנו. בגלל שהספריה הסטנדרטית מושקת ביחד עם שפת ראסט, אין צורך לשנות את את _Cargo.toml_ כדי שיכיל את `std`. אבל כן צריך להפנות אליה באמצעות `use` כדי להכניס עצמים ממנה אל המתחם בחבילה שלנו. למשל, עם `HashMap` עלינו להשתמש בשורה:

```rust
use std::collections::HashMap;
```

זהו מסלול אבסולוטי המתחיל עם `std`, שם המכולה של הספריה הסטנדרטית.

### שימוש במסלולים מקוננים כדי לפשט רשימות `use` ארוכות

כאשר אנו משתמשים בעצמים רבים המוגדרים באותה מכולה או באותו מודול, ציון כל עצם ועצם בפני עצמו עלול למלא שורות רבות בקבצים שלנו. לדוגמא, שתי פקודות ה-`use` הבאות שהופיעו ברשימה 2-4 במשחק ניחוש המספר שלנו מכניסות עצמים מ-`std` למתחם:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/no-listing-01-use-std-unnested/src/main.rs:here}}
```

במקום זאת, ניתן להשתמש במסלולים מקוננים כדי להכניס למתחם בשורה אחת את אותם העצמים. אנו עושים זאת ע"י ציון החלק המשותף של המסלול, ולאחריו שני נקודותיים, ובעקבותיהם סוגריים מסולסלים סביב רשימת חלקי המסלולים השונים, כפי שמוצג ברשימה 7-18.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-18/src/main.rs:here}}
```

<span class="caption">רשימה 7-18: ציון מסלול מקונן על מנת להכניס למתחם מספר עצמים בעלי מסלול תחילי משותף</span>

בתכניות גדולות יותר, הכנסה למתחם של עצמים רבים מאותה מכולה או אותו מודול תוך שימוש במסלולים מקוננים יכולה להקטין משמעותית את מספר פקודות ה-`use` הנפרדות!

ניתן להשתמש במסלול מקונן בכל רמה במסלול. זוהי יכולת מועילה כאשר משלבים שתי פקודות `use` שמשתפות תת-מסלול. למשל, רשימה 7-19 מראה שתי פקודות `use`: האחת מכניסה את `std::io` למתחם והשניה מכניסה את `std::io::Write` למתחם.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-19/src/lib.rs}}
```

<span class="caption">רשימה 7-19: שתי פקודות `use` כאשר האחת היא תת-מסלול של השניה</span>

החלק המשותף לשני המסלולים הוא `std::io`, וזהו המסלול המלא הראשון. על מנת למזג את שני המסלולים האלה לכדי שימוש יחיד ב-`use`, ניתן להשתמש ב-`self` במסלול המקונן, כפי שרואים ברשימה 7-20.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-20/src/lib.rs}}
```

<span class="caption">רשימה 7-20: שילוב המסלולים מרשימה 7-19 לשימוש יחיד בפקודת `use`</span>

שורה זו מכניסה גם את `std::io` וגם את `std::io::Write` למתחם.

### אופרטור הגלוב (glob)

אם אנחנו רוצים להכניס למתחם את _כל_ העצמים הפומביים שמוגדרים במסלול מסויים, נוכל לציין מסלול זה ולאחריו להשתמש באופרטור הגלוב `*`:

```rust
use std::collections::*;
```

פקודת `use` זו מכניסה למתחם הנוכחי את כל העצמים הפומביים שמוגדרים ב-`std::collections`. היזהרו כאשר אתם משתמשים באופרטור גלוב! גלוב יכול להקשות עלינו להבין אלו שמות נמצאים במתחם והיכן הוגדר עצם זה או אחר בו אנו משתמשים.

לרוב משתמשים באופרטור גלוב כאשר מבצעים בדיקות, כדי להכניס את כל מה שאמור להיבדק תחת מודול, לרוב בשם `tests`; נדון בכך בסעיף ["כיצד לכתוב בדיקות"][writing-tests]<!-- ignore --> בפרק 11. לעיתים גם משתמשים באופרטור גלוב כחלק מדפוס הפרליוד: ראו [תיעוד הספריה הסטנדרטית](../std/prelude/index.html#other-preludes)<!-- ignore -->
לפרטים נוספים אודות דפוס זה.

[ch14-pub-use]: ch14-02-publishing-to-crates-io.html#exporting-a-convenient-public-api-with-pub-use
[rand]: ch02-00-guessing-game-tutorial.html#generating-a-random-number
[writing-tests]: ch11-01-writing-tests.html#how-to-write-tests
