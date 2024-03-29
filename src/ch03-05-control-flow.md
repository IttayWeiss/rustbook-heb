## בקרת זרימה

היכולת להריץ קטע קוד מסוים בהתאם לערך האמת של תנאי נתון, וכן היכולת להריץ קוד שוב ושוב כל עוד תנאי מסוים מתקיים, הן אבני בניין חשובות ברוב שפות התכנות. המבנים הנפוצים ביותר לשליטה על זרימת הביצוע של קוד ראסט הם ביטויי `if` ולולאות.

### ביטויי `if`

ביטוי `if` מאפשר לסעף את הקוד שלכם בהתאם לתנאי נתון. במילים פשוטות, אתם מציינים את התנאי, ואז אומרים "אם התנאי הזה מתקיים, הרץ בלוק קוד זה. אם התנאי אינו מתקיים, אל תריץ את הבלוק."

צרו פרוייקט חדש בשם _branches_ בתיקיית הפרויקטים שלכם בכדי להתנסות בביטויי `if`. בקובץ _src/main.rs_ הקלידו את הקוד הבא:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/src/main.rs}}
```

כל ביטוי `if` מתחיל במילת המפתח `if`, ולאחריו התנאי. במקרה זה, התנאי בודק האם המשתנה `number` מכיל ערך קטן מ-5. מיד לאחר התנאי, יש למקם בתוך סוגריים מסולסלים את בלוק הקוד לביצוע במקרה שהתנאי מוערך ל-`true`. בלוקים של קוד המשויכים בצורה זו לתנאים בביטויי `if` נקראים לעיתים _זרועות_ (arms), בדיוק כמו הזרועות בביטויי `match` בהם דנו בסעיף [“Comparing the Guess to the Secret Number”]()<!--
ignore --> בפרק 2.

ניתן גם להוסיף ביטוי `else`, כפי שבחרנו לעשות כאן, בכדי לספק לתכנית בלוק קוד חלופי להרצה במידה והתנאי מוערך ל-`false`. אם לא מספקים ביטוי `else` והתנאי מוערך ל- `false`, התכנית פשוט תקפוץ מעבר לבלוק של ה- `if` ותמשיך לבצע את הקוד שלאחריו.

נסו להריץ את הקוד הזה. יופיע הפלט הבא:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/output.txt}}
```

הבה ננסה לשנות את הערך של `number` לערך שיגרום לתנאי להיות `false`:

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/src/main.rs:here}}
```

הריצו את התכנית בשנית, והתבוננו בפלט:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/output.txt}}
```

יש לשים לב שהתנאי בקוד הזה _חייב_ להיות מוערך לטיפוס `bool`. אם התנאי אינו מוערך לטיפוס `bool`, נקבל שגיאה. נסו למשל להריץ את הקוד הבא:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/src/main.rs}}
```

התנאי בביטוי ה- `if` מוערך ל- `3`, וראסט מודיעה על שגיאה:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/output.txt}}
```

השגיאה מציינת שהקומפיילר של ראסט ציפה לערך מטיפוס `bool`, אבל מצא מספר שלם. בניגוד לשפות כמו Ruby או JavaScript, ראסט לא תנסה להמיר ערכים לא-בולאינים לטיפוסים בוליאנים באופן אוטומטי. עלינו מוטלת החובה לספק ל-`if` תנאי המוערך באופן מפורש לטיפוס בוליאני. אם נרצה שהבלוק של ה- `if` ירוץ רק כאשר המספר שונה מ-`0`, למשל, נוכל לשנות את ביטוי ה-`if` כך:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-29-if-not-equal-0/src/main.rs}}
```

הרצת קוד זה תדפיס את ההודעה: `number was something other than zero`.

#### ניהול תנאים מרובים עם `if else`

ניתן ליישם יותר מתנאי אחד באותו ביטוי בקרת הזרימה על-ידי שילוב של `if` ו-`else` לביטוי `if else`. לדוגמה:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/src/main.rs}}
```

לתכנית זו ארבעה הסתעפויות אפשריות. בעת ההרצה יופיע הפלט:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/output.txt}}
```

כשתכנית זו רצה, היא בודקת כל ביטוי `if` בתורו ומבצעת את הזרוע המשויכת לתנאי הראשון שמוערך ל-`true`. שימו לב שאפילו ש-6 מתחלק ב-2, אנחנו לא רואים את הפלט `number is divisible by 2`, וגם לא את הפלט `number is not divisible by 4, 3, or 2` מהבלוק של ה- `else`. זאת משום שראסט מריצה רק את הבלוק של התנאי הראשון שמוערך ל-`true`. ברגע שהיא מוצאת אחד כזה, היא אפילו לא בודקת את השאר.

עם זאת, שימוש ביותר מידי ביטויי `if else` עלולים לייצר קוד עמוס. אם יש לכם יותר מביטוי אחד כזה, כדאי לשקול לבצע ארגון מחדש של הקוד (code refactoring). פרק 6 מתאר מנגנון הסתעפות עוצמתי בראסט, שנקרא `match`, הנועד עבור מקרים כאלה.

#### שימוש ב- `if` כחלק מפקודת `let`

כיוון ש- `if` הוא ביטוי, ניתן להשתמש בו בצד ימין של פקודת `let` בכדי לבצע השמה למשתנה של הערך המתקבל.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-02/src/main.rs}}
```

<span class="caption">רשימה 3-2: השמה של תוצאת ביטוי `if` למשתנה</span>

המשתנה `number` יקשר לערך בהתבסס על תוצאת ביטוי ה- `if`. הריצו קוד זה כדי לראות מה קורה:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-02/output.txt}}
```

זכרו שבלוקים של קוד מוערכים לביטוי האחרון המופיע בהם, ומספרים בפני עצמם גם הם ביטויים. במקרה זה, הערך של ביטוי ה-`if` בכללותו תלוי בבלוק הקוד שבוצע. משמעות הדבר היא שכל הערכים שיכולים פוטנציאלית להיות התוצאה של כל זרוע של ביטוי ה-`if` חייבים להיות מאותו טיפוס; ברשימה 3-2, התוצאות של זרוע ה-`if` וזרוע ה- `else` היו שלמים מטיפוס `i32`. אם הטיפוסים לא זהים, כבדוגמה הבאה, אז מקבלים שגיאה:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/src/main.rs}}
```

כאשר ננסה לקמפל את הקוד הזה, נקבל שגיאה. לזרועות של ה- `if` וה- `else` יש ערכים מטיפוסים שונים, וראסט מציינת היכן בדיוק בתכנית נמצאת הבעיה:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/output.txt}}
```

כאן, הביטוי בבלוק ה-`if` מוערך למספר שלם, והביטוי בבלוק של ה- `else` מוערך למחרוזת. קוד מעין זה לא יעבור קומפילציה, כיוון למשתנים חייב להיות טיפוס משותף אחד ויחיד, וראסט צריכה לדעת בוודאות מהו כבר בזמן הקומפילציה. ידיעת הטיפוס של המשתנה `number` מאפשרת לקומפיילר לוודא שהטיפוס תקין בכל מקום בו אנו משתמשים ב- `number`. ראסט לא תהיה מסוגלת לעשות זאת אם הטיפוס של `number` יהיה ידוע רק בזמן הריצה. לו היה צריך לעקוב אחר כמה טיפוסים היפותטיים עבור כל משתנה, הקומפיילר היה מורכב יותר, והיה מספק פחות ערבויות לנכונות הקוד.

### חזרה עם לולאות

לעיתים קרובות אנו נדרשים להריץ בלוק קוד מסויים יותר מפעם אחת. למטרה זו ראסט מספקת כמה סוגי _לולאות_ (loops) אשר יריצו את הקוד בגוף הלולאה עד סופו, ואז יחזרו מיד לתחילת הבלוק ויריצו את הקוד מההתחלה. על-מנת להתנסות עם לולאות, הבה ניצור פרוייקט חדש בשם _loops_.

לראסט יש שלושה סוגי לולאות: `loop`, `while`, ו-`for`. הבה נתנסה בכל אחד מהם.

#### חזרה על קוד באמצעות `loop`

מילת המפתח `loop` אומרת לראסט להריץ את בלוק הקוד שוב ושוב לנצח, או עד שנאמר במפורש לעצור.

כדוגמה, שנו את הקובץ _src/main.rs_ בתיקייה _loops_ כדי שיראה כך:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-loop/src/main.rs}}
```

כאשר מריצים תכנית זו, מקבלים את הפלט `again!` מודפס שוב ושוב ושוב עד שעוצרים את התכנית ידנית. רוב הטרמינלים תומכים בקיצור <span class="keystroke">ctrl-c</span> על מנת לכפות על תכנית לעצור, למשל אם היא תקועה בלולאה אינסופית. נסו זאת:

<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-32-loop
cargo run
CTRL-C
-->

```console
$ cargo run
   Compiling loops v0.1.0 (file:///projects/loops)
    Finished dev [unoptimized + debuginfo] target(s) in 0.29s
     Running `target/debug/loops`
again!
again!
again!
again!
^Cagain!
```

הסימן `^C` מופיע בעקבות הפעלת הקיצור <span
class="keystroke">ctrl-c</span>. ייתכן ותראו את המילה `again!` מודפסת אחרי ה- `^C` וייתכן שלא. זה תלוי בשורת הקוד הספציפית שבוצעה בזמן שבוצעה ההפרעה לריצה (interrupt).

למרבה המזל, ראסט גם מספקת דרך לצאת מהלולאה באמצעות קוד. ניתן למקם את מילת המפתח `break` בתוך הלולאה על מנת לומר לתכנית מתי להפסיק את ריצת הלולאה. זכרו שכבר עשינו זאת במשחק ניחוש המספר בסעיף ["עצירה לאחר ניחוש נכון"]()<!-- ignore
--> בפרק 2 כדי לעצור את התכנית כשהמשתמש ניצח את המשחק בעקבות ניחוש נכון של המספר.

כמו כן, במשחק הניחוש השתמשנו גם במילת המפתח `continue`, הגורמת ללולאה לדלג על שארית הבלוק בסבב הנוכחי, ולעבור מייד לסבב הבא.

#### החזרת ערכים מלולאות

אחד השימושים ב- `loop` הוא לחזור על פעולה שעלולה להיכשל, כמו בדיקה האם פתיל חישוב סיים את עבודתו. יתכן, נניח, שאתם צריכים להעביר את תוצאת הפעולה אל מחוץ ללולאה, כיוון שחלקים אחרים בקוד מצפים לה. בכדי לעשות זאת, ניתן להוסיף את הערך שאתם רוצים להחזיר מייד אחרי הפקודה `break`, בה משתמשים כדי לעצור את הלולאה; הערך הזה יוחזר אל מחוץ ללולאה כדי שתוכלו להשתמש בו, כפי שנראה כאן:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-33-return-value-from-loop/src/main.rs}}
```

לפני הלולאה אנחנו מכריזים על משתנה בשם `counter` ומאתחלים אותו לערך `0`. אז אנחנו מכריזים על משתנה בשם `result` ששומר את הערך המוחזר מהלולאה. בכל סבב של הלולאה אנו מוסיפים `1` למשתנה `counter`, ואז אנו בודקים אם `counter` שווה ל- `10`. אם כן, אנו משתמשים במילת המפתח `break` יחד עם הערך `counter * 2`. לאחר הלולאה אנו משתמשים בנקודה-פסיק כדי לסיים את הפקודה שמבצעת את השמת הערך למשתנה `result`. לבסוף, אנחנו מדפיסים את הערך של `result`, שבמקרה זה שווה ל- `20`.

#### תיוג לולאות על מנת להבחין בין לולאות שונות

אם יש לכם לולאות בתוך לולאות, אז הפקודות `break` ו-`continue` מתייחסות ללולאה הפנימית ביותר ביחס להיכן הן מופיעות. ניתן גם לייצר _תגית לולאה_ (loop label) עבור הלולאה אליה אתם רוצים שה-`break` או ה-`continue` יתייחסו. כך, מילות המפתח הללו יתייחסו ללולאה המתוייגת, במקום ללולאה הפנימית ביותר. תגיות לולאה חייבות להתחיל בסימן גרש יחיד. הנה דוגמה עם שתי לולאות מקוננות:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/src/main.rs}}
```

הלולאה החיצונית מסומנת בתגית `'counting_up`, והיא תספור מ-0 עד 2. הלולאה הפנימית לא מתוייגת והיא סופרת מ-10 עד 9. ה-`break` הראשון, שאינו מציין שום תגית, יגרום ליציאה מהלולאה הפנימית. הפקודה `break
'counting_up;` תצא מהלולאה החיצונית. קוד זה מדפיס:

```console
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/output.txt}}
```

#### לולאות תנאי מסוג `while`

בתכניות רבות יש צורך להעריך תנאי כחלק מהתנהגות הלולאה. כל עוד התנאי מוערך ל- `true`, הלולאה ממשיכה לסבב הבא, אך כאשר התנאי מפסיק להיות `true`, התכנית קוראת ל- `break` ועוצרת את הלולאה. ניתן ליישם התנהגות כמו זו באמצעות שילוב של `loop`, `if`, `else`, ו-`break`; תוכלו לנסות זאת עכשיו בעצמיכם, אם תרצו. אולם, דפוס שכזה הוא כל-כך נפוץ שהוא מובנה לתוך שפת ראסט באמצעות לולאת `while`.
ברשימה 3-3 אנו משתמשים ב-`while` כדי לעבור בלולאה על בלוק קוד שלוש פעמים. אנו משתמשים במשתנה בשם `index` כמונה הסופר את מספר הסבבים אותם מבצעת הלולאה. לאחר חמישה סבבים, כאשר ערכו של `index` הופך ל-`5`, התנאי בראש הלולאה הופך ל-false. הלולאה לפיכך מסתיימת, והקוד מדפיס הודעה, ומסיים את התכנית.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-03/src/main.rs}}
```

<span class="caption">רשימה 3-3: שימוש בלולאת `while` כדי להריץ קוד כל עוד תנאי נתון מתקיים</span>

מבנה זה חוסך קינונים רבים שהיו נחוצים אילו היה נעשה שימוש ב-`loop`, `if`, `else`, ו-`break`, מעבר לשיפור הקריאות. כל עוד התנאי מוערך ל- `true` הקוד מתבצע; אחרת, הלולאה מסתיימת.

#### מעבר על אוסף באמצעות `for`

ניתן להשתמש בלולאת `while` כדי לעבור על האלמנטים של אוסף, כמו מערך. למשל, הלולאה ברשימה 3-4 מדפיסה כל אלמנט במערך `a`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-04/src/main.rs}}
```

<span class="caption">רשימה 3-4: מעבר על כל האלמנטים באוסף באמצעות לולאת `while`</span>

כאן, הקוד עובר על כל האלמנטים של המערך, מתחילתו עד סופו: ראשית באינדקס `0`, השמאלי ביותר, ואז עובר בהדרגה ימינה עד שהוא מגיע לאינדקס האחרון של המערך, דהיינו כאשר `index < 5` כבר לא מוערך ל- `true`. הרצת קטע קוד זה תדפיס את כל האלמנטים במערך:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-04/output.txt}}
```

כצפוי, כל חמשת הערכים במערך מופיעים בטרמינל. המשתנה `index` יקבל, בשלב מסויים, את הערך `5`, אבל הלולאה עוצרת לפני שנעשה ניסיון לגשת לאיבר שישי במערך.

אבל, גישה זו צופנת בחובה בעיות. אנחנו עלולים לגרום לתכנית להיכנס לפאניקה אם האינדקס או תנאי הבדיקה אינם תקינים. לדוגמה, אם תשנו את ההגדרה של המערך `a` כך שיהיו בו ארבעה אלמנטים, אבל תשכחו לעדכן את התנאי ל-`while index < 4`, אז התכנית תיכנס לפאניקה. לגישה זו יש גם מחיר במהירות הריצה כיוון שהקומפיילר מוסיף קוד זמן-ריצה שבודק, בכל סבב של הלולאה, האם האינדקס הנוכחי אכן נמצא בין גבולות המערך.

אפשרות פשוטה וברורה יותר היא בחירה בלולאת `for` כדי להריץ בלוק קוד עבור כל איבר באוסף. לולאת `for` מוצגת בקוד ברשימה 3-5.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-05/src/main.rs}}
```

<span class="caption">רשימה 3-5: מעבר על כל האלמנטים באוסף באמצעות לולאת `for`</span>

כאשר נריץ קוד זה, נקבל את אותו הפלט כמו ברשימה 3-4. חשוב מכך, הקוד החדש הגביר את בטיחות הקוד והקטין את הסיכוי לבאגים שעלולים לנבוע, נניח, מניסיון גישה מעבר לגבולות המערך, או בעצירה טרם הגעה לגבול זה. שימוש בלולאת `for` מייתר אפוא את הצורך לזכור לשנות את הקוד במידה ומשנים את מספר הערכים במערך, כפי שהיינו צריכים לעשות ברשימה 3-4.

הבטיחות והתמציתיות של לולאות `for` הופכות אותן לתצורת הלולאות הנפוצה ביותר בראסט. אפילו בסיטואציות בהן אתם רוצים להריץ קוד מספר מסוים של פעמים, כבדוגמה של הספירה לאחור בה השתמשנו בלולאת `while` ברשימה 3-3, רוב הרסטיונארים יעדיפו להשתמש בלולאת `for`. הדרך לעשות זאת היא להשתמש בכלי `Range`, המסופק על-ידי הספריה הסטנדרטית, אשר מייצר את כל המספרים בסדרה, החל ממספר נתון ועד (אך לא כולל) מספר אחר.

כך תראה הספירה לאחור אם משתמשים בלולאת `for` ומתודה נוספת, `rev`, שעוד לא דיברנו עליה, להיפוך הסדר:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-34-for-range/src/main.rs}}
```

קוד זה נעים יותר לעין, נכון?

## סיכום

עשיתם זאת! זה היה פרק נכבד: למדתם אודות משתנים, מבני נתונים סקלארים ומורכבים, פונקציות, הערות, ביטויי `if`, ולולאות! כדי לרכוש ניסיון אמיתי בעבודה עם מושגים אלה, נסו לבנות תכניות שעושות את המשימות הבאות:

- המרת טמפרטורות בין פרנהייט לצלזיוס.
- חישוב המספר הפיבונאצי' ה-n-י (n-th Fiboncci number).
- הדפסת מילות השיר "The Twelve Days of Chirstmas", תוך ניצול המבנה החזרתי של השיר.

כאשר אתם מוכנים להמשיך הלאה, נדון במושג של ראסט _שאינו_ נפוץ כלל בקרב שפות תכנות: בעלות.

ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number ch02-00-guessing-game-tutorial.html#quitting-after-a-correct-guess
