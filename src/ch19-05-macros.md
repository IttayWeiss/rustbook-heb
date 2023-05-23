## מאקרואים

השתמשנו במקרואים לכל אורך הספר, אבל לא בחנו לעומק מהם מקרואים וכיצד הם עובדים. המושג *מקרו* (macro) מתייחס למשפחה של תכונות בראסט: *מקרואים הכרזתיים* עם `macro_rules!` ושלושה סוגים של *מקרואים תפעוליים*:

* מקרואים מותאמים מסוג `#[derive]` שמציינים קוד בתוספת האפיון `derive` בשימוש על מבנים ומבחרים
* מקרואים דמויי-אפיון שמגדירים אפיונים מותאמים שניתנים לשימוש על כל טיפוס
* מקרואים דמויי-פונקציה שנראים כמו קריאות לפונקציה אבל פועלים על האסימונים (tokens) שמצויינים כארגומנטים שלהם

אנו נדון בכל אחד מאלה לפי התור, אבל ראשית, הבה נבין מדוע בכלל יש צורך במאקרואים כאשר כבר יש לנו פונקציות.

### על ההבדל בין מקרואים לפונקציות

באופן בסיסי, מאקרואים הם דרך לכתוב קוד שכותב קוד, פעולה שידועה בשם *מטה-תכנות*. בנספח ג', אנו דנים באפיון `derive`, שמייצר עבורכם מימוש של כל מיני תכונות. כמו כן, השתמשנו, לכל אורך הספר, במקרואים `println!` ו-`vec!`. כל המקרואים האלה מתרחבים לכדי יותר קוד מהקוד שכותבים ידנית.

מטה-תכנות הוא שימושי כדי להקטין את כמות הקוד שצריך לכתוב ולתחזק, וזו גם אחת המטרות של פונקציות. אבל, למקרואים יש כמה יכולות שלפונקציות אין.

חותם פונקציה חייב להכריז על מספר הפרמטרים, והטיפוסים שלכם, שהפונקציה מקבלת. מקרו, לאומת זאת, יכול לקבל כל מספר של פרמטרים: ניתן לכתוב `println!("hello")` עם ארגומנט אחד, או `println!("hello {}", name)` עם שני ארגומנטים. בנוסף, מקרואים עוברים התרחבות לפני שהקומפיילר מפרש את הקוד, ולכן מקרו יכול, למשל, לממש תכונה של טיפוס נתון. פונקציה לא יכולה לבצע פעולה שכזאת משום שלפונקציה קוראים בזמן הריצה בעוד התכונה חייבת להיות ממומשת בזמן הקומפילציה.

חסרון של מימוש מקרו במקום פונקציה הוא שהגדרת מקרו היא יותר סבוכה מהגדרת פונקציה כיוון שהמטרה היא לכתוב קוד ראסט שבעצמו כותב עוד קוד ראסט. בשל כתיבת קוד עקיפה זו, הגדרות של מקרואים בדרך-כלל קשות יותר לקריאה, להבנה, ולתחזוק בהשוואה להגדרות של פונקציות.

הבדל חשוב נוסף בין מקרואים לפונקציות הוא שחייבים להגדיר מקרו, או להכניס אותו למתחם, *לפני* שקוראים לו בקובץ, בניגוד לפונקציות, אותן ניתן להגדיר בכל מקום ולקרוא להן מכל מקום.

### מקרואים הכרזתיים באמצעות `macro_rules!` עבור מטה-תכנות כללי

תצורת המקרו הנפוצה ביותר בראסט היא *מקרו הכרזתי*. למקרואים אלה לעיתים קוראים גם "מקרואים לפי דוגמא", "`macro_rules!` ", או פשוט "מקרואים" בבסיסם, מקרואים הכרזתיים מאפשרים לכם לכתוב דבר מה הדומה לביטוי `match` בראסט. כפי שנידון בפרק 6, ביטוי `match` הוא מבנה לבקרת זרימה שמקבל ביטוי, משווה את הערך המתקבל מהביטוי מול תבניות, ואז מריץ קוד לפי שיוך לתבנית. מקרו גם משווה ערך מול תביות שמשוייכות עם קוד מסויים: במקרה זה, הערך הוא קוד הראסט המפורש שמועבר למקרו; התבניות מושוות מול המבנה של קוד זה; והקוד המשוייך עם כל תבנית, כאשר יש התאמה, מחליף את הקוד המועבר למקרו. כל זה מתרחש במהלך הקומפילציה.

על מנת להגדיר מקרו, משתמשים ב- `macro_rules!`. הבה נראה כיצד להשתמש ב- `macro_rules!` על-ידי התבוננות באופן ההגדרה של המקרו `vec!`. פרק 8 כיסה את השימוש במקרו `vec!` כדי ליצור ווקטור חדש עם ערכים נתונים. למשל, המקרו הבא יוצר ווקטור חדש המכיל שלושה שלמים:

```rust
let v: Vec<u32> = vec![1, 2, 3];
```

ניתן גם להשתמש במקרו `vec!` כדי ליצור ווקטור של שני שלמים, או ווקטור של חמישה חיתוכי מחרוזת. לא ניתן לעשות זאת עם פונקציה משום שאי-אפשר לדעת מראש את מספר הערכים, או טיפוסם.

רשימה 19-28 מציגה הגדרה מפושטת מעט של המקרו `vec!`.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch19-advanced-features/listing-19-28/src/lib.rs}}
```


<span class="caption">רשימה 19-28: גרסה מפושטת `vec!`</span>

> הערה: ההגדרה של `vec!` בגרסתה הרשמית בספריה הסטנדרטית כוללת קוד לביצוע הקצאה מקדימה של כמות הזיכרון הדרוש. קוד זה הוא אופטימיזציה ממנה אנו מתעלמים פה, למען פשטות הדוגמא.

הביאור `#[macro_export]` מציין שמקרו זה צריך להיות זמין בכל מקום בו המכולה בה המקרו מוגדר מוכנסת למתחם. ללא ביאור זה, לא ניתן להכניס את המקרו למתחם.

מתחילים את הגדרת המקרו על-ידי כתיבת `macro_rules!` ולאחר מכן שם המקרו *ללא* סימן קריאה. לאחר השם, שבמקרה זה הוא `vec`, מופיעים סוגריים מסולסלים אשר מסמנים את גוף הגדרת המקרו.

המבנה של גוף ה-`vec` דומה למבנה של ביטוי `match`. כאן יש לנו זרוע אחת עם התבנית `( $( $x:expr ),* )`, ולאחריה `=>` והבלוק של הקוד המשוייך לתבנית זו. במידה והתבנית מתאימה, בלוק הקוד המשוייך יופק. בהתחשב בכך שזוהי התבנית היחידה במקרו, יש רק דרך תקפה אחת להתאמה; כל תבנית אחרת תוביל לשגיאה. למקרואים מורכבים יותר יהיו יותר מזרוע אחת.

התחביר של תבניות תקפות בהגדרות מקרואים שונה מתחביר התבניות בו דנו בפרק 18 מכיוון שתבניות מקרואים מותאמות מול קוד ראסט, ולא כנגד ערכים. הבה נעבור, פיסה-אחר-פיסה, על משמעות הקוד ברשימה 19-28; לתחביר תבניות המקרואים המלא, פנו [לתיעוד של ראסט][ref].

ראשית, אנו משתמשים בזוג סוגריים כדי להקיף את התבנית כולה. אנו משתמשים בסימן `$` כדי להכריז, במערכת המקרואים, על משתנה שיכיל את קוד הראסט עליו מתבצעת ההתאמה. סימן ה-<0>$</0> מבהיר שזהו משתנה מקרו בניגוד למשתנה רגיל של ראסט. לאחר מכן מגיעים סוגריים שתופסים ערכים שמתאימים לתבנית שבתוך הסוגריים, לשימוש בקוד להחלפה. בתוך `$()` נמצא `$x:expr`, שמתאים לכל ביטוי ראסט ונותן לביטוי את השם `$x`.

הפסיק שאחרי ה-`$()` מציין שהסימן המפורש פסיק כסימן מפריד יכול, אופציונאלית, להופיע אחרי הקוד שמותאם לקוד ב-`$()`. ה-`*` מציינת שהתבנית מתאימה לאפס, או יותר, ממה שמופיע אחרי ה-`*`.

כאשר קוראים למקרו זה עם `vec![1, 2, 3];`, התבנית `$x` מתאימה שלוש פעמים עם שלושת הביטויים `1`, `2`, ו-`3`.

הבה נעבור כעת בתבנית שבגוף של הקוד המשוייך לזרוע זו: `temp_vec.push()` בתוך `$()*` יופק עבור כל חלק שמתאים ל-`$()` בתבנית, אפס או יותר פעמים, בתלות בכמה פעמים התבנית הותאמה. ה-`$x` יוחלף עם כל אחד מהביטויים שהותאמו. כאשר קוראים למקרו זה עם `vec![1, 2, 3];`, הקוד שיופק שיחליף את קריאת המקרו יהיה:

```rust,ignore
{
    let mut temp_vec = Vec::new();
    temp_vec.push(1);
    temp_vec.push(2);
    temp_vec.push(3);
    temp_vec
}
```

הגדרנו מקרו שיכול לקבל כל מספר שהוא של ארגומנטים, לא משנה מאיזה טיפוס, ומייצר קוד שמייצר ווקטור שמכיל את הפרטים הנתונים.

כדי ללמוד עוד על כתיבת מקרואים, פנו לתיעוד ברשת, או למקורות אחרים, כמו [“The Little Book of Rust Macros”][tlborm] שיזם דניאל קיפ (Daniel Keep) והמשיך לכתוב לוקאס ווירת' (Lukas Wirth).

### מקרואים תפעוליים לייצור קוד ממאפיינים

התצורה השניה של מקרואים היא *מקרואים תפעוליים*, שפועלים יותר כמו פונקציות (והם סוג של פרוצדורה). מקרו תפעולי מקבל פיסת קוד כקלט, פועל על קוד זה, ומפיק קוד כלשהו כפלט במקום להתאים מול תבניות ולבצע החלפה של קוד בקוד אחר כמו שמקרו הכרזתי עושה. שלושת הסוגים של מקרואים תפעוליים הם גזירה מותאמת, דמוי-אפיון, ודמוי-פונקציה, וכולם פועלים בדרכים דומות.

כאשר יוצרים מקרו תפעולי, ההגדרות חייבות לשכון במכולה משלהן, בעלת טיפוס מכולה מיוחד. זה נחוץ מסיבות טכניות סבוכות שכולנו תקווה שיעלמו בעתיד. ברשימה 19-29, אנו מראים כיצד להגדיר מקרו תפעולי, כאשר `some_attribute` הוא תופס-מקום לשימוש בסוג ספציפי של מקרו.

<span class="filename">Filename: src/lib.rs</span>

```rust,ignore
use proc_macro;

#[some_attribute]
pub fn some_name(input: TokenStream) -> TokenStream {
}
```


<span class="caption">רשימה 19-29: דוגמא להגדרת מקרו תפעולי</span>

הפונקציה שמגדירה את המקרו התפעולי מקבלת את `TokenStream` כקלט ומפיק `TokenStream` כפלט. הטיפוס `TokenStream` מוגדר על-ידי המכולה `proc_macro` שכלולה עם ראסט ומייצגת סדרה של אסימונים. זהו לב המקרו: קוד המקור שעליו המקרו פועל הוא הקלט `TokenStream`, והקוד שהמקרו מפיק הוא הפלט `TokenStream`. לפונקציה יש גם אפיון שמציין איזה סוג של מקרו תפעולי יוצר. יכולים להיות כמה סוגים של מקרואים תפעוליים במכולה אחת.

הבה נתבונן בסוגים השונים של מקרואים תפעוליים. נתחיל עם מקרואים מסוג גזירה מותאמת, ולאחר מכן נסביר מהן השונויות הקטנות שבסוגים האחרים.

### כיצד לכתוב מקרו `derive` מותאם

הבה ניצור מכולה בשם `hello_macro` שמגדירה תכונה בשם `HelloMacro` עם פונקציה משוייכת אחת בשם `hello_macro`. במקום להכריח את המשתמשים שלנו לממש את התכונה `HelloMacro` עבור כל אחד מהטיפוסים שלהם, נספק מקרו תפעולי כדי שמשתמשים יוכלו לבאר את הטיפוסים שלהם עם `#[derive(HelloMacro)]` ולקבל מימוש ברירת-מחדל עבור הפונקציה `hello_macro`. מימוש ברירת-המחד ידפיס `hello_macro</0>! My name is
TypeName!` כאשר `TypeName` הוא השם של הטיפוס עליו התכונה הוגדרה. במילים אחרות, נכתוב מכולה שמאפשרת למתכנתים אחרים לכתוב קוד כמו ברשימה 19-30 תוך שימוש במכולה שלנו.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch19-advanced-features/listing-19-30/src/main.rs}}
```


<span class="caption">רשימה 19-30: הקוד שמשתמשים של המכולה שלנו יוכלו לכתוב תוך שימוש במקרו תפעולי</span>

קוד זה ידפיס `Hello, Macro! My name is Pancakes!` כאשר נסיים. הצעד הראשון הוא ליצור מכולת ספריה חדשה, כך:

```console
$ cargo new hello_macro --lib
```

לאחר מכן, נגדיר את התכונה `HelloMacro` ואת הפונקציות המשוייכות שלה:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch19-advanced-features/no-listing-20-impl-hellomacro-for-pancakes/hello_macro/src/lib.rs}}
```

יש לנו תכונה ואת הפונקציות שלה. בנקודה זו, המכולה שלנו תוכל לממש את התכונה כדי להשיג את הפונקציונאליות המצופה, כך:

```rust,ignore
{{#rustdoc_include ../listings/ch19-advanced-features/no-listing-20-impl-hellomacro-for-pancakes/pancakes/src/main.rs}}
```

אבל, משתמשים יאלצו לכתוב את בלוק המימוש לכל טיפוס עבורו הם רוצים להשתמש ב- `hello_macro`; אנחנו רוצים לחסוך להם את המאמץ הזה.

בנוסף, לא ניתן לספק לפונקציה `hello_macro` מימוש ברירת-מחדל שידפיס את השם של הטיפוס עליו התכונה ממומשת: לראסט אין יכולת התבוננות עצמית, ולכן אינה יכולה לחפש אחר שם טיפוס בזמן הריצה. אנחנו צריכים מקרו כדי להיות מסוגלים לייצר קוד בזמן הקומפילציה.

הצעד הבא הוא להגדיר את המקרו התפעולי. בזמן כתיבת שורות אלה, מקרואים תפעוליים חייבים להימצא במכולה משלהם. עם הזמן, יתכן ומגבלה זו תוסר. המוסכמה לבנית מכולות ומכולות מקרואים היא כדלהלן: עבור מקרו בשם `foo`, מכולת מקרו תפעולי נגזרתי מותאם נקראת `foo_derive`. הבה ניצור מכולה חדשה בשם `hello_macro_derive` בתוך הפרוייקט `hello_macro`:

```console
$ cargo new hello_macro_derive --lib
```

שתי המכולות שלנו קשורות הדוקות זו לזו, ולכן אנו יוצרים את מכולת המקרו התפעולי בתוך התיקיה של המכולה `hello_macro`. אם נשנה את שם התכונה ב-`hello_macro`, יהיה עלינו לשנות את המימוש של המקרו התפעולי ב-`hello_macro_derive` גם כן. שתי המכולות יצטרכו להיות מפורסמות בנפרד, ומתכנתים שישתמשו במכולות אלה יצטרכו להוסיף את שתיהן כתלותות ולהכניס את שתיהן למתחם. היינו יכולים, כחלופה, להגדיר שהמכולה `hello_macro` משתמשת ב-`hello_macro_derive` כתלות ולבצע יצוא מחדש של הקוד של המקרו התפעולי. אבל, הדרך בה בנינו את הפרוייקט מאפשרת למתכנתים להשתמש ב-`hello_macro` אפילו אם הם לא מעוניינים בפונקציונאליות ה-`derive`.

אנחנו צריכים להכריז על המכולה `hello_macro_derive` כמכולת מקרו תפעולי. בנוסף נצטרך גם פונקציונאליות מהמכולות `syn` ו- `quote`, כפי שתראו עוד רגע קט, ולכן אנחנו מוסיפים אותן כתלותות. הוסיפו את השורות הבאות לקובץ *Cargo.toml* עבור `hello_macro_derive`:

<span class="filename">Filename: hello_macro_derive/Cargo.toml</span>

```toml
{{#include ../listings/ch19-advanced-features/listing-19-31/hello_macro/hello_macro_derive/Cargo.toml:6:12}}
```

על מנת להתחיל להגדיר את המקרו התפעולי, מקמו את הקוד מרשימה 19-31 לתוך הקובץ *src/lib.rs* עבור המכולה `hello_macro_derive`. שימו לב שקוד זה לא יעבור קומפילציה לפני שנוסיף את ההגדרה עבור הפונקציה `impl_hello_macro`.

<span class="filename">Filename: hello_macro_derive/src/lib.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch19-advanced-features/listing-19-31/hello_macro/hello_macro_derive/src/lib.rs}}
```


<span class="caption">רשימה 19-31: קוד שרוב מכולות מקרואים תפעוליים ידרשו בשביל לעבד קוד ראסט</span>

שימו לב שפיצלנו את הקוד לפונקציה `hello_macro_derive`, שאחראית על הפרסינג של ה- `TokenStream`, והפונקציה `impl_hello_macro`, שאחראית על המרת עץ התחביר: פיצול זה הופך מקל על כתיבת מקרואים תפעוליים. הקוד בפונקציה חיצונית זו (`hello_macro_derive` במקרה זה) תהיה זהה כמעט לכל מכולת מקרו תפעולי שתראו, או שתכתבו. הקוד שאתם מספקים בגוף הפונקציה הפנימית (`impl_hello_macro` במקרה זה) תהיה שונה ותלויה במטרת המקרו התפעולי.

הצגנו שלוש מכולות חדשות: `proc_macro`, [`syn`][], ו-[`quote`][]. המכולה `proc_macro` מגיעה עם ראסט, ולכן לא היה צורך להוסיף אותה לרשימת התלותות ב-*Cargo.toml*. המכולה `proc_macro` היא ה-API של הקומפיילר שמאפשרת לנו לקרוא ולעבד קוד ראסט מתוך הקוד שלנו.

המכולה `syn` עושה פרסינג לקוד ראסט מתוך מחרוזת לתוך מבנה נתונים שעליו אנו מבצעים פעולות. המכולה `quote` הופכת מבני נתונים מסוג `syn` חזרה לקוד ראסט. מכולות אלה מקלות מאוד על ביצוע פרסינג על כל סוג של קוד ראסט שנוכל להיתקל בו: כתיבת פרסר מלא לראסט אינה עניין של מה בכך.

הפונקציה `hello_macro_derive` תיקרא כאשר משתמשי הספריה יציינו `#[derive(HelloMacro)]` על טיפוס כלשהו. זה מתאפשר מכיוון שביארנו כאן את הפונקציה `hello_macro_derive` עם `proc_macro_derive` וציינו את השם `HelloMacro`, שתואם את שם התכונה שלנו; זוהי המוסכמה אחריה רוב המקרואים התפעוליים עוקבים.

הפונקציה `hello_macro_derive` מתחילה בלהמיר את `input` מ-`TokenStream` למבנה נתונים שאנחנו יכולים לפרש ולבצע עליו פעולות. כאן `syn` נכנס לתמונה. הפונקציה `parse` ב-`syn` מקבלת `TokenStream` ומחזירה מבנה `DeriveInput` המייצג את קוד הראסט לאחר פרסינג. רשימה 19-32 מציגה את החלקים הרלוונטים של המבנה `DeriveInput` שאנו מקבלים לאחר ביצוע פרסינג למחרוזת `struct Pancakes;`:

```rust,ignore
DeriveInput {
    // --snip--

    ident: Ident {
        ident: "Pancakes",
        span: #0 bytes(95..103)
    },
    data: Struct(
        DataStruct {
            struct_token: Struct,
            fields: Unit,
            semi_token: Some(
                Semi
            )
        }
    )
}
```


<span class="caption">רשימה 19-32: המופע של `DeriveInput` שאנו מקבלים כשמבצעים פרסינג לקוד עם האפיון של המקרו ברשימה 19-30</span>

השדות במבנה זה מראים שקוד הראסט עליו עשינו פרסינג הוא מבנה יחידה עם ה-`ident` (identifier, קריא השם) של `Pancakes`. יש שדות נוספים במבנה זה עבור תיאור כל סוגי הקוד בראסי; פנו לתיעוד של `DeriveInput`</a> למידע נוסף.

בעוד זמן קצר נגדיר את הפונקציה `impl_hello_macro`, שם נבנה את קוד הראסט החדש שאנו רוצים לכלול. אבל לפני שנעשה זאת, שימוש לב שהפלט שמקרו הגזירה שלנו גם הוא `TokenStream`. ערך `TokenStream` מוחזר זה מוסף לקוד שמשתמשי המכולה שלנו כותבים, כך שכאשר אם מקמפלים את המכולה שלהם, הם יקבלו פונקציונאליות נוספת שאנו מספקים ב-`TokenStream` המשודרג.

יתכן ושמתם לב שאנו קוראים ל-`unwrap` כדי לגרום לפונקציה `hello_macro_derive` להיכנס לפאניקה במקרה והקריאה לפונקציה `syn::parse` נכשלת. זה הכרחי שהמקרו התפעולי שלנו יכנס לפאניקה במקרה של שגיאות כיוון שפונקציות מסוג `proc_macro_derive` חייבות להחזיר `TokenStream` ולא `Result` על מנת לענות לדרישות ה-API של מקרואים תפעוליים. פישטנו דוגמא זאת על-ידי שימוש ב-`unwrap`; בקוד להשקה, יש לספק הודעות שגיאה יותר ממוקדות באמצעות `panic!` או `expect` אודות מה שהשתבש.

כעת, משיש בידנו את הקוד להפוך את קוד הראסט המבואר מ-`TokenStream` למופע של `DeriveInput`, הבה ניצר את הקוד שמממש את התכונה `HelloMacro` על הטיפוס המבואר, כמוצג ברשימה 19-33.

<span class="filename">Filename: hello_macro_derive/src/lib.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch19-advanced-features/listing-19-33/hello_macro/hello_macro_derive/src/lib.rs:here}}
```


<span class="caption">רשימה 19-33: מימוש של התכונה `HelloMacro` תוך שימוש בקוד ראסט שעבר פרסינג</span>

אנו מקבלים, באמצעות `ast.ident`, מופע של המבנה `Ident` שמכיל את השם (identifier) של הטיפוס המבואר. המבנה ברשימה 19-32 מראה שכאשר אנו קוראים לפונקציה `impl_hello_macro` על הקוד מרשימה 19-30, ל-`ident` שמקבלים יהיה את הערך `"Pancakes"` בשדה `ident`. לכן, המשתנה `name` ברשימה 19-33 יכיל מופע של המבנה `Ident` אשר, כשהוא יודפס, יהיה המחרוזת `"Pancakes"`, שהיא השם של המבנה מרשימה 19-30.

המקרו `quote!` מאפשר לנו להגדיר את קוד הראסט שאנחנו רוצים להחזיר. הקומפיילר מצפה לקבל משהו שונה מהתוצאה שמסופקת ישירות מריצת המקרו `quote!`, ולכן עלינו להמיר את התוצאה ל-`TokenStream`. אנו עושים זאת על-ידי קריאה למתודה `into`, שמכלה את ייצוג הביניים הזה ומחזירה ערך מהטיפוס המבוקש, `TokenStream`.

המקרו `quote!` מספק גם כמה יכולות בנית טמפלטים מרשימות ביותר: ניתן לרשום `#name`, ואז `quote!` יחליף את זה עם הערך במשתנה `name`. ניתן אפילו לבצע חזרות באופן דומה לנעשה במקרואים רגילים. פנו לתיעוד ב-[the `quote` crate’s docs][quote-docs] לעוד מידע.

אנחנו רוצים שהמקרו התפעולי שלנו ייצר מימוש עבור התכונה `HelloMacro` שלנו עבור הטיפוס שהמשתמש מבאר, שאותו אנו יכולים לקבל דרך `#name`. למימוש התכונה יש את הפונקציה היחידה `hello_macro`, וגוף הפונקציה מכיל את הפונקציונאליות שאנחנו רוצים: הדפסת `Hello, Macro! My name is` ואז את שם הטיפוס המבואר.

המקרו `stringify!` בו אנו משתמשים כאן הוא מקרו שבנוי לתוך ראסט. הוא לוקח ביטוי של ראסט, כגון `1 + 2`, ובזמן הקומפילציה הופך את הביטוי למחרוזת מפורשת, כמו `"1 + 2"`. פעולה זו שונה ממה ש-`format!` עושה, או ממה ש-`println!` עושה. אלה מקרואים שמעריכים ביטוי ומחזירים את את התוצאה כ-`String`. יתכן שהקלט שב-`#name` מכיל ביטוי, ואותו יש להדפיס כמו שהוא, לכן אנו משתמשים ב-`stringify!`. שימוש ב-`stringify!`. גם חוסך הקצאת זיכרון על-ידי המרת `#name` למחרוזת מפורשת בזמן הקומפילציה.

בנקודה זו, `cargo build` צריך לעבוד בהצלחה גם ב-`hello_macro` וגם ב-`hello_macro_derive`. הבה נחבר מכולות אלה לקוד מרשימה 19-30 כדי לראות את המקרואים התפעוליים בפעולה! צרו פרוייקט בינארי חדש בתיקיית הפרוייקטים שלכם על-ידי הרצת `cargo new pancakes`. יש להוסיף את `hello_macro` ואת `hello_macro_derive` כתלותות בקובץ *Cargo.toml* של המכולה `pancakes`. אם אתם מפרסמים את הגרסאות שלכם של `hello_macro` ו-`hello_macro_derive` ל-[crates.io](https://crates.io/), הם יהיו תלותות רגילות; אחרת, תוכלו לציין אותם כתלותות `path`, באופן הבא:

```toml
{{#include ../listings/ch19-advanced-features/no-listing-21-pancakes/pancakes/Cargo.toml:7:9}}
```

מקמו את הקוד מרשימה 19-30 לתוך הקובץ *src/main.rs*, והריצו `cargo run`: אתם צריכים לראות `Hello, Macro! My name is Pancakes!` המימוש של התכונה `HelloMacro` מהמקרו התפעולי נכלל מבלי שהמכולה `pancakes` היתה צריכה לממש אותה; השורה `#[derive(HelloMacro)]` הוסיפה את המימוש.

כעת, הבה נבין במה נבדלים המקרואים התפעוליים האחרים ממקרואים נגזרים מותאמים.

### מקרואים דמויי-אפיון

מקרואים דמויי-אפיון דומים למקרואים נגזרים מותאמים, אבל במקום לייצר קוד עבור האפיון `derive`, הם מאפשרים לכם ליצור אפיונים חדשים. הם גם יותר גמישים: `derive` עובד רק עבור מבנים ומבחרים; מאפיינים יכולים לפעול על עצמים אחרים, כמו פונקציות. הימה דוגמא לשימוש במקרו דמוי-אפיון: נניח שיש לכם אפיון בשם `route` שמבאר פונקציות כאשר משתמשים ב-web appliation framework:

```rust,ignore
#[route(GET, "/")]
fn index() {
```

האפיון `#[route]` יהיה מוגדר ע"י ה-framework כמקרו תפעולי. החותם של הפונקציה של הגדרת המקרו יראה כך:

```rust,ignore
#[proc_macro_attribute]
pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream {
```

יש לנו כאן שני פרמטרים מטיפוס `TokenStream`. הראשון הוא עבור התוכן של האפיון: החלק של ה-`GET, "/"`. השני הוא הגוף של העצם שמקושר לאפיון: במקרה זה, `fn index() {}` ושארית הגוף של הפונקציה.

מלבד זאת, מקרואים דמויי-אפיון עובדים באותה הדרך כמו מקרואים נגזרים מותאמים: יוצרים מכולה עם טיפוס המכולה `proc-macro` ומממשים פונקציה שיוצר את הקוד הרצוי!

### מקרואים דמויי-פונקציה

מקרואים דמויי-פונקציה מגדירים מקרואים שנראים כמו קריאות לפונקציה. באופן דומה למקרואים מסוג `macro_rules!`, הם יותר גמישים מפונקציות; למשל, הם יכולים לקבל מספר לא ידוע של ארגומנטים. אבל, מקרואים מסוג `macro_rules!` ניתן להגדיר רק באמצעות תחביר שדומה לתחביר match, כפי שנידון לעיל בסעיף ["מקרואים הכרזתיים עם `macro_rules!` עבור מטה-תכנות כללי"][decl]<!-- ignore --> . מקראוים דמויי-פונקציה מקבלים פרמטר `TokenStream` וההגדרה שלהם מבצעת מניפולציות על פרמטר זה תוך שימוש בקוד ראסט, כמו שנעשה בשני הסוגים האחרים של מקרואים תפעוליים. דוגמא למקרו דמוי-פונקציה הוא המקרו `sql!`, אותו ניתן לקרוא כך:

```rust,ignore
let sql = sql!(SELECT * FROM posts WHERE id=1);
```

מקרו זה יבצע פרסינג לביטוי ה-SQL שבתוכו, ויבדוק שהוא נכון תחבירים, פעולת עיבוד הרבה יותר מורכבת ממה ש-`macro_rules!` יכולים לבצע. המקרו `sql!` יהיה מוגדר כך:

```rust,ignore
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
```

הגדרה זו דומה לחותם של מקרו נגזר מותאם: אנו מקבלים את האסימונים שבתוך הסוגריים, ומחזירים את הקוד שאנו רוצים לייצר.

## סיכום

וואוו! כעת יש באמתחתכם כמה יכולות בראסט בהם, כנראה, לא תעשו שימוש תכוף, אבל תדעו על קיומן במידה והנסיבות ידרשו אותן. הצגנו כמה נושאים מורכבים כך שכשפגשו אותם בהמלצות כחלק מהודעות שגיאה, או בקוד של מפתחים אחרים, תהיו מסוגלים לזהות את המושגים והתחביר. השתמשו בפרק זה כהפניה וכדי לכוון אתכם בעודכן מחפשים אחר פתרונות.

כעת, ניישם את כל המ שלמדו במהלך הספר הלכה למעשה ונבנה פרוייקט אחד נוסף!

[ref]: ../reference/macros-by-example.html
[tlborm]: https://veykril.github.io/tlborm/
[`syn`]: https://crates.io/crates/syn
[`quote`]: https://crates.io/crates/quote
[quote-docs]: https://docs.rs/quote
[decl]: #declarative-macros-with-macro_rules-for-general-metaprogramming