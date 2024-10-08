## ארגון מחדש כדי לשפר מודולריות וטיפול בשגיאות

על מנת לשפר את התכנית שלנו, נתקן ארבע בעיות שנוגעות למבנה התכנית ולדרך בה היא מטפלת בשגיאות פוטנציאליות. ראשית, פונקצית ה-`main` שלנו בשלב זה מבצעת שתי פעולות: היא עושה פארסינג לארגומנטים וקוראת קבצים. ככל שהתכנית תגדל, מספר הפעולות הנפרדות שפונקצית ה-`main` תעשה יגדל גם הוא. ככל שפונקציה מקבלת על עצמה יותר ויותר אחראויות, כך נהיה קשה יותר ויותר להבין את הפונקציה, לבדוק אותה, ולשנות אותה מבלי לקלקל אף אחד מחלקיה. לכן מומלץ להפריד את הפונקציונאליות כך שכל פונקציה אחראית למשימה אחת בלבד.

סוגיה זו קשורה גם לבעיה השניה: למרות ש-`query` ו-`file_path` הם משתני קונפיגורציה עבור התכנית שלנו, אנו משתמשים במשתנים כמו `contents` כדי לבצע את הלוגיקה של התכנית. ככל ש-`main` תגדל, נצטרך להכניס למתחם יותר ויותר משתנים; ככל שיש יותר משתנים במתחם, כך קשה יותר לעקוב אחר המטרה של כל אחד מהם. עדיף לקבץ יחדיו את כל משתני הקונפיגורציה למבנה אחד, וכך להבהיר את משמעותם.

הבעיה השלישית היא שהשתמשנו ב-`expect` כדי להדפיס הודעת שגיאה כאשר קוראים קבצים, אבל הודעת השגיאה רק מדפיסה `Should have been
able to read the file`. קריאת קובץ יכולה להיכשל בכמה דרכים: למשל, אם הקובץ לא קיים, או אם אין לנו את ההתרים הדרושים כדי לפתוח אותו.
בשלב זה, ללא תלות בנסיבות, תמיד נדפיס את אותה הודעת שגיאה, וזה לא יספק למשתמש מידע מועיל!

הרביעית והאחרונה בבעיותנו היא השימוש התכוף ב-`expect` כדי לטפל בשגיאות שונות, ואם המשתמש יריץ את התכנית שלנו ללא ארגונמנטים, תתקבל שגיאת `index out
of bounds`, ללא הסבר נאות על מהות הבעיה. עדיף שכל הקוד האחראי על טיפול בשגיאות ימוקם במקום יעודי כך שלמתחזקים עתידיים של הקוד שלנו יהיה קל יותר לנווט את עצמם למקום הנכון במידה וצריך לשנות דבר מה באופן טיפול השגיאות. מיקום כל הקוד המטפל בשגיאות במקום אחד גם יסייע בכתיבת הודעות שגיאה משמעותיות יותר עבור המשתמשים.

הבה נטפל בארבע בעיות אלה בעודנו מארגנים מחדש את הקוד.

### הפרדת עניינים בפרוייקטים בינארים

הקצאת אחריות עבור כמה משימות לפונקצית ה-`main` היא בעיה ארגונית נפוצה בפרוייקטים בינארים. כתוצאה מכך, קהילת ראסט פתחה קווים מנחים לפיצול העניינים השונים של תכנית בינארית לכשפונקצית ה-`main` מתחילה לגדול. הינה הצעדים בהם יש לנקוט:

- פיצול התכנית ל-_main.rs_ ול-_lib.rs_ והעברת הלוגיקה של התכנית -_lib.rs_.
- כל עוד הלוגיקה של הפארסינג פשוטה, ניתן להשאיר אותה ב-_main.rs_.
- כאשר הלוגיקה של הפארסינג נהיית מסובכת, יש להעביר אותה מ-_main.rs_ ל-_lib.rs_.

האחראויות שנשארות בפונקציה `main` לאחר תהליך זה צריכות להיות מוגבלות לרשימה הבאה:

- קריאה ללוגיקה של הפארסינג משורת הפקודה עם ערכי הארגומנטים
- הגדרת שאר הקונפיגורציה
- קריאה לפונקצית ה-`run` מ-_lib.rs_
- טיפול בשגיאה במידה ו-`run` מחזירה שגיאה

מהות תבנית זו היא הפרדת עניינים: _main.rs_ מטפלת בהרצת התכנית, בעוד _lib.rs_ מטפלת בכל ענייני הלוגיקה התפעולית. כיוון שלא ניתן לבדוק את פונקציית ה-`main` ישירות, ארגון זה של הקוד מאפשר לבדוק את הלוגיקה של התכנית על-ידי מיקום לוגיקה זו בפונקציות ב-_lib.rs_. הקוד שנשאר ב-_main.rs_ יהיה קצר מספיק כדי לוודא את תקינותו רק על-ידי קריאה. הבה נכתוב מחדש את התכנית שלנו תוך מעקב אחר התהליך לעיל.

#### מיצוי הפארסינג של הארגומנטים

אנו נמצה את הפונקציונאליות של הפארסינג של הארגומנטים לפונקציה שתיקרא על-ידי `main` כהכנה להעברת הלוגיקה של הפארסניג של שורת הפקודה ל-_src/lib.rs_. רשימה 12-5 מראה את ההתחלה החדשה של `main` שקוראת לפונקציה חדשה בשם `parse_config`, שאותה נגדיר, לעת עתה, ב-_src/main.rs_.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-05/src/main.rs:here}}
```

רשימה 12-5: מיצוי הפונקציה `parse_config` מ-`main`</span>

אנחנו עדיין מקבצים יחדיו את הארגומנטים של שורת הפקודה לווקטור, אבל במקום לשייך את הארגומנט באינדקס 1 למשתנה `query` ואת הארגומנט באינדקס 2 למשתנה `file_path` בתוך הפונקציה `main`, אנו מעבירים את הווקטור כולו לפונקציה `parse_config`. הפונקציה `parse_config` מבצעת את הלוגיקה שקובעת אלו ארגומנטים מותאמים לאלו משתנים, ומעבירה את הערכים בחזרה ל-`main`. אנחנו עדיין יוצרים את המשתנים `query` ו-`file_path` ב-`main`, אבל `main` כבר לא אחראית להחלטה כיצד לקשר בין הארגומנטים משורת הפקודה למתשתנים בתוכנית.

העבודה הזו עשויה להראות מוגזמת לתכנית קצרה כמו שלנו, אולם אנו מבצעים את הארגון מחדש בצעדים אינקרמנטלים וקטנים. לאחר שינוי זה, הריצו את התכנית שוב כדי לוודא שהפארסינג מתבצע באופן נאות. מומלץ מאוד לבדוק את ההתקדמות באופן תכוף, בכל צעד וצעד, על מנת לזהות בנקל את המקור של כל בעיה שעלולה לצוץ בדרך.

#### קיבוץ ערכי קונפיגורציה

אנחנו יכולים לבצע צעד נוסף כדי להמשיך ולשפר את הפונקציה `parse_config`.
בשלב זה, אנחנו מחזירים מרצף, אבל אז מייד מפרקים אותו למרכיביו. זהו סימן לכך שיתכן שעוד אין בידנו את האבסטרקציה הנכונה.

אינדיקטור אחר לכך שיש מקום נוסף לשיפור הוא חלק ה-`config` של ה-`parse_config`, אשר מראה ששני הערכים שאנו מחזירים קשורים זה לזה ושניהם חלק מאותו ערך קונפיגורציה. במצב הנוכחי אנחנו לא מבטאים משמעות זו במבנה של הדאטה, חוץ מכך שהערכים נמצאים באותו מרצף; במקום זאת, נמקם את שני הערכים במבנה וניתן שם משמעותי לכל אחד משדות המבנה. בכך יהיה קל יותר למתחזקים עתידיים של הקוד להבין כיצד הערכים השונים קשורים זה לזה ומהי מטרתם.

רשימה 12-6 מציגה שיפורים אלה לפונקציה `parse_config`.

<span class="filename">Filename: src/main.rs</span>

```rust,should_panic,noplayground
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-06/src/main.rs:here}}
```

<span class="caption">רשימה 12-6: ארגון מחדש של הפונקציה `parse_config` כך שהיא מחזירה מופע של המבנה `Config`</span>

הוספנו מבנה בשם `Config` שמוגדר עם שני שדות ששמותיהם `query` ו-`file_path`. החותם של `parse_config` מבטא כעת שהוא מחזיר ערך מטיפוס `Config`. בגוף של הפונקציה `parse_config`, היכן שקודם החזרנו חיתוכי מחרוזת שהפנו לערכי `String` ב-`args`, אנחנו מגדירים כעת מופע `Config` שמכיל ערכי `String`. משתנה ה-`args` ב-`main` הוא הבעלים של ערכי הארגומנטים והוא מאפשר לפונקציה `parse_config` רק לשאול אותם, מה שאומר שהיינו מפרים את חוקי ההשאלות של ראסט אילו `Config` היה מנסה לקחת בעלות על הערכים ב-`args`.

ישנן כמה דרכים לטפל בדאטה מטיפוס `String`; הפשוטה ביותר, אם כי לא מאוד יעילה, היא לקרוא למתודה `clone` על הערכים.
פעולה זו תיצור עותק של הדאטה עליו המופע של `Config` יקח בעלות, וזה לוקח יותר זמן וזיכרון מאשר אכסון של הפניה למחרוזת.
אולם, שכפול הדאטה גם הופך את הקוד שלנו לפשוט ביותר כיוון שאנו לא צריכים לנהל את משכי-החיים של ההפניות; בנסיבות אלה, ויתור על מעט יעילות למען נהירות הוא שיקול מוצדק.

> ### שיקולי יעילות בשימוש ב-`clone`
>
> ישנה נטיה בקרב ראסטיונארים להימנע משימוש ב-`clone` כדי לטפל בבעיות בעלות בגלל עלויות זמן ריצה. [בפרק 13][ch13]<!-- ignore -->, תלמדו כיצד להשתמש במתודות יעילות יותר במצבים מסוג זה. אבל לבינתיים, זה בסדר להעתיק כמה מחרוזות על מנת להמשיך ולהתקדם בגלל שאנו מבצעים העתקות אלה רק פעם אחת, ואורכי המחרוזות של מסלול הקובץ ושל מחרוזת החיפוש קצרים. עדיף שתהיה בידנו תכנית עובדת, אפילו אם פחות יעילה, מאשר לנסות לבצע אופטימיזציה בשלב מוקדם מידי בפיתוח. ככל שתצברו ניסיון בראסט, יקל עליכם להתחיל עם תכנון אפקטיבי יותר של פתרונות, אבל לעת זו זה בסדר גמור להשתמש ב-`clone`.

עדכנו את `main` כך שאת המופע של `Config` שמוחזר מ-`parse_config` היא ממקמת לתוך המשתנה `config`, ועדכנו את הקוד שקודם השתמש במשתנים `query` ו-`file_path` בנפרד כך שכעת הוא משתמש בשדות של `Config`.

עכשיו הקוד שלנו מבטא בצורה בהירה יותר את העובדה ש-`query` ו-`file_path` קשורים זה לזה ושהם קיימים למטרת קונפיגורציה של התכנית. כל קוד שמשתמש בערכים אלה יודע למצוא אותם במופע ה-`Config` בשדות ששמם מתאר את יעודם.

#### יצירת קונסטרקטור עבור `Config`

עד כה, מיצינו מהפונקציה `main` את הלוגיקה האחראית לפארסינג של הארגומנטים של שורת הפקודה ומיקמנו אותה בפונקציה `parse_config`. בכך יכולנו לראות שהערכים ב-`query` וב-`file_path` קשורים זה לזה ושראוי שקשר זה יבוא לידי ביטוי בקוד. אז הוספנו את המבנה `Config` כדי לתת שם למטרה המקשרת בין `query` לבין `file_path` וכדי להיות מסוגלים להחזיר מהפונקציה `parse_config` את שמות הערכים כשדות במבנה.

עכשיו, כשהמשמעות של הפונקציה `parse_config` היא יצירת מופע של `Config`, נוכל לשנות את `parse_config` מפונקציה רגילה לפונקציה בשם `new` שמקושרת למבנה `Config`. דבר זה יהפוך את הקוד ליותר אידיאומטי. ניתן ליצור מופעים של טיפוסים בספריה הסטנדרטית, כמו `String`, על-ידי קריאה ל-`String::new`. באופן דומה, על ידי שינוי הפונקציה `parse_config` לפונקציה מקושרת בשם `new`, נוכל ליצור מופעים של `Config` על-ידי קריאה ל-`Config::new`. רשימה 12-7 מציגה את השינויים שיש לבצע.

<span class="filename">Filename: src/main.rs</span>

```rust,should_panic,noplayground
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-07/src/main.rs:here}}
```

רשימה 12-7: שינוי הפונקציה `parse_config` לפונקציה מקושרת `Config::new`</span>

עידכנו את `main` היכן שקודם קראנו ל-`parse_config` כך שעכשיו אנו קוראים ל-`Config::new`. שינינו את השם של `parse_config` ל-`new` והעברנו את ההגדרה לבלוק ה-`impl` שמקשר את הפונקציה `new` למבנה `Config`. נסו לקמפל קוד זה פעם נוספת כדי לוודא שהכל בסדר.

### תיקון הטיפול בשגיאות

כעת נתמקד בתיקון הטיפול בשגיאות. זכרו שניסיון לגשת לערכים בוקטור `args` באינדקס 1 או אינדקס 2 יגרום לתכנית להיכנס לפאניקה במידה והוקטור מכיל פחות משלושה פריטים. נסו להריץ את התכנית ללא ארגומנטים; זה יראה כך:

```console
{{#include ../listings/ch12-an-io-project/listing-12-07/output.txt}}
```

השורה `index out of bounds: the len is 1 but the index is 1` היא הודעת שגיאה המיועדת למתכנתים. היא לא תעזור למשתמשי הקצה שלנו להבין מה הם צריכים לעשות. הבה נתקן זאת.

#### שיפור הודעות שגיאה

ברשימה 12-8, אנו מוסיפים בדיקה בפונקציה `new` שתוודא שהחיתוך ארוך מספיק לפני הגישה לאינדקסים 1 ו-2. אם החיתוך אינו מספיק ארוך, התכנית תיכנס לפאניקה ותציג הודעת שגיאה משופרת.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-08/src/main.rs:here}}
```

<span class="caption">רשימה 12-8: הוספת בדיקה אודות מספר הארגומנטים</span>

קוד זה דומה לקוד שכתבנו עבור הפונקציה `Guess::new` ברשימה 9-13][ch9-custom-types]<!-- ignore -->, שם קראנו ל-`panic!` במידה והארגומנט `value` היה מחוץ לטווח הערכים התקפים. במקום לבדוק עבור טווח של ערכים, כאן אנו בודקים שהאורך של `args` הוא לפחות 3 ושאר הפונקציה יכולה לפעול תחת ההנחה שתנאי זה מתקיים. אם ל-`args` יש פחות משלושה פריטים, תנאי זה יהיה נכון, ואז יופעל מאקרו ה-`panic!` שמסיים את התכנית מייד.

עם שורות נוספות אלה בפונקציה `new`, הבה נריץ את התכנית פעם נוספת ללא ארגומנטים ונראה כיצד נראית הודעת השגיאה כעת:

```console
{{#include ../listings/ch12-an-io-project/listing-12-08/output.txt}}
```

פלט זה טוב יותר: יש לנו הודעת שגיאה סבירה. אולם, יש לנו גם מידע מיותר שאין אנו רוצים להפיל על המשתמשים. יתכן שהשימוש בטכניקה מרשימה 9-13 לא מתאים למצבנו כאן: קריאה ל-`panic!` מתאימה יותר למצב בו יש בעיה תכנותית מאשר לבעית שימוש, [כפי שנידון בפרק 9][ch9-error-guidelines]<!-- ignore -->. לכן, נשתמש במקום זאת בטכניקה עליה למדתם בפרק 9--[החזרת
`Result`][ch9-result]<!-- ignore --> שמייצג הצלחה או כישלון.

<!-- Old headings. Do not remove or links may break. -->

<a id="returning-a-result-from-new-instead-of-calling-panic"></a>

#### החזרת `Result` במקום קריאה ל-`panic!`

אנחנו יכולים להחזיר ערך `Result` שיכיל מופע של `Config` במקרה של הצלחה ויתאר את הבעיה במקרה של שגיאה. נשנה גם את שם הפונקציה מ-`new` ל-`build` כיוון שמתכנתים רבים מצפים מפונקציות בשם `new` לא להיכשל. כאשר `Config::build` מתקשרת עם `main`, ניתן להשתמש בטיפוס `Result` כדי לסמן במידה ויש בעיה. אז נוכל לשנות את `main` כך שתמיר את הוריאנט `Err` לשגיאה יותר פרקטית עבור המשתמשים שלנו ללא הטקסט הסובב אודות `thread
'main'` and `RUST_BACKTRACE` שקריאה ל-`panic!` מייצרת.

רשימה 12-9 מראה את השינויים שעלינו לעשות לערך המוחזר של הפונקציה שלה אנו קוראים כעת `Config::build`, ולגוף של הפונקציה, על מנת להחזיר ערך מטיפוס `Result`. שימו לב שהקוד לא יעבור קומפילציה עד שנעדכן גם את הפונקציה `main`, וכך נעשה ברשימה הבאה.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-09/src/main.rs:here}}
```

<span class="caption">רשימה 12-9: החזרת `Result` מ-`Config::build`</span>

הפונקציה `build` מחזירה ערך מטיפוס `Result` עם מופע של `Config` במקרה של הצלחה ועם `&'static str` במקרה של שגיאה. ערכי השגיאה שלנו תמיד יהיו מחרוזות מפורשות בעלות משך-חיים `'static`.

עשינו שני שינויים בגוף הפונקציה: במקום קריאה ל-`panic!` במקרה והמשתמש לא מעביר מספיק ארגומנטים, אנו מחזירים ערך `Err`, וכמו כן עטפנו את ערך ה-`Config` בתוך `Ok`. שינויים אלה מתאימים את הפונקציה לחותם שלה.

החזרת ערך `Err` מ-`Config::build` מאפשר לפונקציה `main` לטפל בערך ה-`Result` המוחזר מהפונקציה `build`, וגם, אם צריך, להפסיק את התכנית מוקדם בצורה נקיה אם ארעה שגיאה.

<!-- Old headings. Do not remove or links may break. -->

<a id="calling-confignew-and-handling-errors"></a>

#### קריאה ל-`Config::build` וטיפול בשגיאות

על מנת לטפל במקרה בו ארעה שגיאה ולהדפיס הודעה ידידותית למשתמש עלינו לעדכן את `main` כך שתטפל בערך `Result` שמוחזר מ-`Config::build`, כפי שמוצג ברשימה 12-10. בנוסף, במקום להסתמך על `panic!`, ניקח על עצמנו את האחריות להפסיק את ריצת כלי שורת הפקודה שלנו עם ערך שונה מאפס במידת הצורך על-ידי מימוש ידני. ערך שונה מאפס הוא המוסכמה שמסמלת לתהליך שקרא לתכנית שלנו שהיא הסתיימה עקב שגיאה.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-10/src/main.rs:here}}
```

<span class="caption">רשימה 12-10: יציאה עם קוד שגיאה במידה ובניית `Config` נכשלה</span>

ברשימה זו, השתמשנו במתודה שעוד לא פירטנו עליה:`unwrap_or_else`, שמוגדרת בספריה הסטנדרטית עבור `Result<T, E>`.
שימוש ב-`unwrap_or_else` מאפשר לנו להגדיר טיפול ידני, ללא שימוש ב-`panic!`, בשגיאות. אם ה-`Result` הוא `Ok`, אז מתודה זו מתנהגת כמו `unwrap`: היא מחזירה את הערך הפנימי שה-`Ok` עוטף. אבל אם הערך הוא `Err`, אז המתודה קוראת לקוד _בסגור_, שהוא פונקציה אנונימית שאנו מגדירים ומעבירים כארגומנט ל-`unwrap_or_else`. נדון בסגורים ביתר פירוט [בפרק 13][ch13]<!-- ignore -->. לבינתים, כל שעליכם לדעת הוא ש-`unwrap_or_else` תעביר את הערך הפנימי של ה-`Err`, שבמקרה זה יהיה המחרוזת הסטטית `"not enough arguments"` שהוספנו ברשימה 12-9, לסגור שלנו לתוך הארגומנט `err` שמופיע בין שני הקווים האנכיים. הקוד שבסגור יכול אז להשתמש בערך ה-`err` בזמן שהוא רץ.

הוספנו שורת `use` חדשה כדי להכניס את `process` מהספריה הסטנדרטית למתחם. הקוד בסגור שירוץ במקרה של שגיאה הוא רק בן שתי שורות: אנו מדפיסים את הערך `err` ואז קוראים ל-`process::exit`. הפונקציה `process::exit` תגרום לתכנית לעצור מייד והיא תחזיר כקוד סטטוס את המספר שהועבר אליה. זה דומה לטיפול מבוסס ה-`panic!` בו השתמשנו ברשימה 12-8, אבל זה לא מייצר את כל הפלט העודף. הבה ננסה:

```console
{{#include ../listings/ch12-an-io-project/listing-12-10/output.txt}}
```

מצוין! פלט זה ידידותי הרבה יותר עבור המשתמשים שלנו.

### יצוא חלק מהלוגיקה ב-`main`

כעת משסיימנו לארגן מחדש את הפארסינג של הקונפיגורציה, הבה נפנה ללוגיקה של התכנית. כפי שאמרנו בסעיף “Separation of Concerns for Binary
Projects”<!-- ignore -->, נמצה פונקציה בשם `run` אשר תכיל את כל הלוגיקה שכרגע נמצאת בפונקציה `main` שלא קשורה לניהול קונפיגורציה או טיפול בשגיאות. כאשר נסיים, פונקציית ה-`main` תהיה מתומצתת ופשוטה לווידוא על-ידי התבוננות, ונהיה מסוגלים לכתוב מקרי-מבחן לכל שאר הלוגיקה.

רשימה 12-11 מראה את הפונקציה `run` שממוצאת מתוך הקוד הקיים.
 אנו עדיין בשלב ההגדרה של הפונקציה ואנו מבצעים פעולות אינקרמנטליות קטנות ל-src/main.rs_.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-11/src/main.rs:here}}
```

<span class="caption">רשימה 12-11: מיצוי הפונקציה `run` המכילה את שאר הלוגיקה של התכנית</span>

הפונקציה `run` מכילה כעת את שארית הלוגיקה מ-`main`, המתחילה בקריאת הקובץ. הפונקציה `run` מקבלת את המופע של `Config` כארגומנט.

#### החזרת שגיאות מהפונקציה `run`

עכשיו ששארית הלוגיקה של הפונקציה הופרדה לתוך הפונקציה `run`, נוכל לשפר את הטיפול בשגיאות, כפי שעשנו עם `Config::build` ברשימה 12-9.
במקום לאפשר לתכנית להיכנס לפאניקה על-ידי קריאה ל-`expect`, הפונקציה `run` תחזיר מופע של `Result<T, E>` במקרה שמשהו משתבש. בכך יתאפשר לנו למקד את הלוגיקה של הטיפול בשגיאות לפונקציה `main` בצורה ידידותית למשתמש. רשימה 12-12 מראה את השינויים שיש לבצע בחותם הפונקציה `run` ובגופה.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-12/src/main.rs:here}}
```

<span class="caption">רשימה 12-12: שינויים לפונקציה `run` על מנת להחזיר `Result`</span>

עשינו כאן שלושה שינויים משמעותיים. ראשית, שינינו את טיפוס הערך המוחזר של הפונקציה `run` ל-`Result<(), Box<dyn Error>>`. מוקדם יותר פונקציה זו החזירה את ערך היחידה, `()`, ואנו שומרים טיפוס זה כערך המוחזר במקרה של `Ok`.

עבור טיפוס השגיאה, השתמשנו _באובייקט התכונה_ `Box<dyn Error>` (והכנסנו למתחם את `std::error::Error` באמצעות פקודת `use` בהתחלה).
נדון באובייקטי תכונה [בפרק 17][ch17]<!-- ignore -->. לעת עתה, דעו ש-`Box<dyn Error>`משמעו שהפונקציה מחזירה טיפוס שמממש את התכונה`Error`, אבל אין צורך לציין בדיוק איזה טיפוס זה יהיה. זה מאפשר לנו את הגמישות להחזיר ערכי שגיאה מטיפוסים שונים עבור מקרי שגיאה שונים. מילת המפתח `dyn` היא קיצור למלה “dynamic”.

שנית, הסרנו את הקריאה ל-`expect` והחלפנו אותה בשימוש באופרטור `?`, כפי שהוסבר [בפרק 9][ch9-question-mark]<!-- ignore -->. במקום להיכנס לפאניקה במקרה של שגיאה, האופרטור `?` יחזיר את ערך השגיאה מהפונקציה הנוכחית להמשך טיפול בחזרה אל הפונקציה הקוראת.

השינוי השלישי הוא שהפונקציה `run` מחזירה כעת ערך `Ok` במקרה של הצלחה.
בחותם הפונקציה הכרזנו את טיפוס ההצלחה של `run` להיות `()`, ולכן עלינו לעטוף את טיפוס היחידה בערך ה-`Ok`. התחביר `Ok(())` עשוי להראות מעט מוזר בהתחלה, אבל זהו שימוש אידאומטי ב-`()` על מנת לציין שאנו קוראים ל-`run` עבור תופעות הלוואי שלה בלבד; הפונקציה לא מחזירה ערך נחוץ.

כאשר מריצים קוד זה, הוא עובר קומפילציה ומדפיס את האזהרה:

```console
{{#include ../listings/ch12-an-io-project/listing-12-12/output.txt}}
```

ראסט מודיעה לנו שהקוד שלנו מתעלם מערך `Result`, ושערך זה עלול לציין שארעה שגיאה. אבל אנו לא בודקים לראות אם אכן ארעה שגיאה, והקומפילר מזכיר לנו שוודאי התכוונו לכתוב כאן קוד שמטפל בשגיאה! הבה נתקן את המצב מיד.

#### טיפול ב-`main` בשגיאות המוחזרות מ-`run`

אנו נבדוק קיום שגיאות, ונטפל בהן, תוך שימוש בטכניקה הדומה לזו בה השתמשנו עם `Config::build` ברשימה 12-10, אבל בהבדל קל:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/no-listing-01-handling-errors-in-main/src/main.rs:here}}
```

אנו משתמשים ב-`if let` במקום ב-`unwrap_or_else` על מנת לבדוק אם `run` החזירה ערך `Err`, ואם כן אנו קוראים ל-`process::exit(1)`. הפונקציה `run` לא מחזירה ערך שאנו רוצים לפתוח באותה הדרך ש-`Config::build` החזיר מופע של `Config`. כיוון שבמקרה של הצלחה `run` מחזירה `()`, אנחנו רק רוצים לזהות שארעה שגיאה, ולכן אין צורך ב-`unwrap_or_else` כדי להחזיר את הערך הפנימי, שפשוט יהיה סתם `()`.

הגוף של ה-`if let` וה-`unwrap_or_else` זהים בשני המקרים: אנו מדפיסים את השגיאה ומסיימים.

### פיצול קוד למכולת ספריה

פרוייקט ה-`minigrep` שלנו מתחיל להראות טוב! כעת, נפצל את הקובץ _src/main.rs_ ונמקם חלק מהקוד בקובץ _src/lib.rs_. בצורה זו נוכל לבדוק את הקוד ובקובץ _src/main.rs_ יהיו פחות משימות לביצוע.

הבה נעביר מהקובץ _src/main.rs_ את כל הקוד שאינו בפונקציה `main` לקובץ _src/lib.rs_:

- הגדרת הפונקציה `run`
- פקודות ה-`use` הרלוונטיות
- ההגדרה של `Config`
- הגדרת הפונקציה `Config::build`

התוכן של הקובץ _src/lib.rs_ צריך להכיל את החותמות המוצגות ברשימה 12-13 (לשם הבהירות השמטנו את גופי הפונקציות). שימו לב שהקוד לא יעבור קומפילציה עד שנעדכן בהתאם את _src/main.rs_ ברשימה 12-14.

<span class="filename">Filename: src/lib.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-13/src/lib.rs:here}}
```

<span class="caption">רשימה 12-13: העברת `Config` ו-`run` לקובץ _src/lib.rs_</span>

עשינו שימוש חופשי במילת המפתח `pub`: עבור `Config`, השדות שלו, והמתודה `build`, והפונקציה `run`. יש לנו כעת מכולת ספריה עם API פובמי שניתן לבדוק!

כעת יש להכניס את הקוד שהעברנו אל _src/lib.rs_ לתוך המתחם של המכולה הבינרית ב-_src/main.rs_, כפי שנעשה ברשימה 12-14.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch12-an-io-project/listing-12-14/src/main.rs:here}}
```

<span class="caption">רשימה 12-4: שימוש בספרית המכולה של `minigrep` ב-_src/main.rs_</span>

אנו מוסיפים את השורה `use minigrep::Config` כדי להכניס את הטיפוס `Config` ממכולת הספריה למתחם של המכולה הבינרית, ומספקים את המסלול המתאים לקריאה לפונקציה `run`. עכשיו כל הפונקציונאליות צריכה להיות מקושרת ואמורה לעבוד. הריצו את התכנית באמצעות `cargo run` וודאו שהכל עובד כהלכה.

וואו! זה הצריך הרבה עבודה, אבל ההשקעה תצדיק את עצמה עם ההצלחה העתידית של הפרוייקט. כעת קל הרבה יותר לטפל בשגיאות והקוד גם יותר מודולרי. כמעט כל העבודה שלנו תהיה, מעתה ואילך, בקובץ _src/lib.rs_.

הבה ננצל מודולריות זו על-ידי ביצוע משימה שהיה קשה לביצוע עם הקוד הישן, אבל תהיה קלה למדי עם הקוד הנוכחי: נכתוב כמה מקרי מבחן!

[ch13]: ch13-00-functional-features.html
[ch9-custom-types]: ch09-03-to-panic-or-not-to-panic.html#creating-custom-types-for-validation
[ch9-error-guidelines]: ch09-03-to-panic-or-not-to-panic.html#guidelines-for-error-handling
[ch9-result]: ch09-02-recoverable-errors-with-result.html
[ch17]: ch17-00-oop.html
[ch9-question-mark]: ch09-02-recoverable-errors-with-result.html#a-shortcut-for-propagating-errors-the--operator
