## פונקציות

פונקציות הן מבנה שכיח בראסט. אתם כבר ראיתם את אחת הפונקציות החשובות ביותר בשפה: הפונקציה `main`, שהיא נקודת הפתיחה של תוכניות רבות. כמו כן, ראיתם את מילת המפתח `fn`, שמאפשרת להכריז על פונקציות.

הקונבציה בכתיבת קוד בראסט היא להשתמש בסגנון *snake case* עבור שמות של פונקציות ומשתנים, ז"א שימוש באותיות קטנות והפרדת מילים ע"י מקף תחתון. הינה תכנית המכילה דוגמא להגדרת פונקציה:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-16-functions/src/main.rs}}
```

אנו מגדירים פונקציות בראסט ע"י כתיבת `fn` ואז את שם הפונקציה וסוגריים. הסוגרים המסולסלים אומרים לקומפיילר היכן מתחיל גוף הפונקציה והיכן הוא מסתיים.

ניתן לקרוא לכל פונקציה שהגדרנו ע"י הקלדת השם שלה ולאחריו סוגריים. כיוון שהפונקציה `another_function` מוגדרת בתכנית, ניתן לקרוא לה מתוך הפונקציה `main`. שימו לב שהגדרנו את `another_function` *אחרי* הפונקציה `main` בקובץ המקור; יכולנו להגדיר את הפונקציה גם לפני. לראסט לא אכפת איפה אתם מגדירים את הפונקציות, רק שהן מוגדרות במתחם שהקורא לפונקציה יכול לראות.

הבה נתחיל פרוייקט חדש בשם *functions* כדי להתנסות עם פונקציות קצת יותר. מקמו את הפונקציה לדוגמא `another_function` בקובץ *src/main.rs* והריצו אותו. אתם צריכים לראות את הפלט הבא:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-16-functions/output.txt}}
```

השורות מתבצעות בסדר הוא הן מופיעות בפונקציה `main`. קודם מודפסת ההודעה “Hello, world!”, ואח"כ נקראת הפונקציה `another_function` וההודעה שלה מודפסת.

### פרמטרים

ניתן להגדיר פונקציות עם פרמטרים, דהיינו משתנים מיוחדים שמהווים חלק מחותם הפונקציה (function signature). כאשר לפונקציה יש פרמטרים, ניתן לספק לפונקציה ערכים קונקרטים עבור פרמטרים אלה. מבחינה טכנית, הערכים הקונקרטים נקראים *ארגיומנטים* אבל, בשיח לא פורמלי, משתמשים במילים *פרמטר* ו-*ארגיומנט* לחילופין כאשר הכוונה היא או למשתנים בגדרת הפונקציה או לערכים המועברים אל הפונקציה כאשר קוראים לה.

בגרסה זו של `another_function` אנו מוסיפים פרמטר:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/src/main.rs}}
```

נסו להריץ את התוכנית; אתם צריכים לקבל את הפלט:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/output.txt}}
```

הכרזת הפונקציה `another_function` כוללת פרמטר אחד הנקרא `x`. הטיפוס של `x` מוגדל להיות `i32`. כאשר אנו מעבירים את הערך `5` לפונקציה `another_function`, המאקרו `println!` משתמש בערך `5` במקום בו הסוגרים המסולסלים מכילים `x` במחרוזת הפירמוט.

בחותם הפונקציה יש להכריז על הטיפוס של כל אחד מהפרמטרים. זו בחירה מכוונת בתכנון של ראסט: הדרישה לביאור טיפוסים בהגדרות של פונקציות משמעה שהקומפיילר כמעט אף-פעם לא צריך שאתם ת***. בנוסף, כך הקומפיילר גם יכול לספק הודעות שגיאה מועילות יותר, כי הוא יודע לאילו טיפוסים הפונקציה מצפה.

כאשר מגדירים פרמטרים מרובים, יש להפריד את הכרזות הפרמטרים עם פסיקים, כך:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/src/main.rs}}
```

דוגמא זו יוצרת פונקציה בשם `print_labeled_measurement` עם שני פרמטרים. הפרמטר הראשון נקרא `value` והוא מטיפוס `i32`. שם הפרמטר השני הוא `unit_label` והוא מטיפוס `char`. הפונקציה מדפיסה טקסט המכיל גם את `value` וגם את `unit_label`.

הבה ננסה להריץ את הקוד. החליפו את התכנית שכרגע נמצאת בפרוייקט בקובץ *src/main.rs* בפרוייקט *functions* עם הקוד מהדוגמא הקודמת והריצו אותו באמצעות `cargo
run`:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/output.txt}}
```

בגלל שקראנו לפונקציה עם `5` עבור הערך `value` ועם `'h'` עבור הערך `unit_label`, פלט התכנית יכיל את שני הערכים האלה.

### פקודות וביטויים

גוף הפונקציה מורכב מסדרה של פקודות, והוא יכול להסתיים בביטוי. עד כה הפונקציות שראינו לא כללו ביטוי סיום, אבל כן ראיתם ביטוי כחלק מפקודה. כיוון שראסט היא שפה מבוססת ביטויים, חשוב להבין הבחנה זו. בשפות אחרות אין הפרדה כזאת ברור, אז הבה נתבונן במהן פקודות ומהם ביטויים, וכיצד ההבדלים בינהם משפיעים על גוף הפונקציות.

* **פקודות** (statements) הן הוראות שמבצעות פעולה כלשהיא ולא מחזירות ערך.
* **ביטויים** (expressions), לאומת זאת, מוערכים לתוצאה. הינה כמה דוגמאות.

למעשה כבר השתמשנו בפקודות ובביטויים. יצירת משתנה והשמה של ערך למשתנה באמצעות מילת המפתח `let` היא פקודה. ברשימה 3-1, השורה `let y = 6;` היא פקודה.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-01/src/main.rs}}
```

<span class="caption">רשימה 3-1: הגדרת פונקציית `main` הכוללת פקודה אחת</span>

הגדרות של פונקציות גם הן פקודות; כל הדוגמא הקודמת היא עצמה פקודה.

פקודות אינן מחזירות ערכים. לכן לא ניתן לבצע השמה של פקודת `let` למשתנה אחר, כפי שהקוד הבא מנסה לעשות; מה שיגרום לשגיאה:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/src/main.rs}}
```

אם תנסו להריץ תכנית זו, השגיאה שתקבלו תראה כך:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/output.txt}}
```

הפקודה `let y = 6` לא מחזירה ערך, ולכן אין שום דבר אליו `x` יכול להיקשר. תופעה זו שונה ממה שקורה בשפות אחרות, כגון C ו-Ruby, שבהם פקודות השמה מחזירות את הערך של ההשמה. בשפות אלה ניתן לכתוב `x = y = 6` ולגרום לכך שגם `x` וגם `y` יקושרו לערך `6`; זה לא המצב בראסט.

ביטויים עוברים הערכה לתוצאה והם מהווים את רוב הקוד שתכתבו בראסט. חשבו למשל על פעולה מתמטית, כמו `5 + 6`, שהוא ביטוי המוערך לתוצאה `11`. ביטויים יכולים להיות חלק מפקודות: ברשימה 3-1, `6` בפקודה `let y = 6;` הוא ביטוי שמוערך לתוצאה `6`. קריאה לפונקציה היא ביטוי. קריאה למאקרו היא ביטוי. בלוק מתחם חדש שנוצר ע"י סוגריים מסולסלים הוא ביטוי, למשל:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-20-blocks-are-expressions/src/main.rs}}
```

הביטוי הבא:

```rust,ignore
{
    let x = 3;
    x + 1
}
```

הוא בלוק אשר, במקרה זה, מוערך לתוצאה `4`. הערך הזה נקשר למשתנה `y` כחלק מהפקודה `let`. שימו לב שבשורה `x + 1` חסר סימן הנקודה-פסיק בסופה, בשונה מרוב השורות שראיתם עד כה. ביטויים לא מסתיימים בנקודה-פסיק. הוספת נקודה-פסיק בסוף ביטוי הופכת אותו לפקודה, ולכן הוא לא יחזיר שום ערך. זכרו זאת כשאתם מתוודעים לערכים המוחזרים מפונקציות ולביטויים בהמשך.

### פונקציות המחזירות ערך

פונקציות יכולות להחזיר ערכים לקוד שקרא להן. אין צורך לתת שם לערך המוחזר, אבל חייבים להכריז על טיפוס הערך המחוזר אחרי סימן של חץ (`->`). בראסט, הערך שפונקציה מחזירה הוא הערך התוצאה הסופית בהערכת הביטוי האחרון בגוף הפונקציה. ניתן להחזיר ערך מוקדם יותר ע"י שימוש במילת המפתח `return` וציון מפורש של הערך להחזרה, אבל רוב הפונקציות מחזירות את הביטוי האחרון. הינה דוגמא לפונקציה שמחזירה ערך:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/src/main.rs}}
```

אין לא קריאות לפונקציות, לא מקרואים, ואפילו לא פקודות `let` בפונקציה `five` -- רק את המספר `5` לבדו. זוהי פונקציה לגיטימית לחלוטין בראסט. שימו לב שטיפוס הערך המוחזר של הפונקציה כן מוגדר, כ- `-> i32`. נסו להריץ את הקוד הזה; הפלט צריך להראות כך:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/output.txt}}
```

המספר `5` בפונקציה `five` הוא הערך המוחזר של הפונקציה, מה שמסביר מדוע טיפוס הערך המוחזר הוא `i32`. הבה נבחן זאת ביתר פירוט. יש שני חלקים חשובים: ראשית, השורה `let x = five();` מראה שניתן להשתמש בערך המוחזר של פונקציה כדי לאתחל משתנה. היות והפונקציה `five` מחזירה `5`, שורה זו שקולה לשורה הבאה:

```rust
let x = 5;
```

שנית, לפונקציה `five` אין פרמטרים ו *** read first sentnce? אבל גוף הפונקציה מורכב מ- `5` בודד ללא נקודה-פסיק בגלל שזה ביטוי שאת ערכו אנחנו רוצים להחזיר.

הבה נתבונן בדוגמא נוספת:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-22-function-parameter-and-return/src/main.rs}}
```

הרצת קוד זה תדפיס `The value of x is: 6`. אבל אם נמקם נקודה-פסיק בסוף השורה המכילה את `x + 1`, ובכך נשנה אותה מביטוי לפקודה, נקבל שגיאה:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/src/main.rs}}
```

ניסיון לקמפייל את הקוד תוביל לשגיאה הבאה:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/output.txt}}
```

הודעת השגיאה המרכזית, `mismatched types`, חושפת את לב הבעיה עם הקוד הזה. The definition of the function `plus_one` says that it will return an `i32`, but statements don’t evaluate to a value, which is expressed by `()`, the unit type. Therefore, nothing is returned, which contradicts the function definition and results in an error. In this output, Rust provides a message to possibly help rectify this issue: it suggests removing the semicolon, which would fix the error.
