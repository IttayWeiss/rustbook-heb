07-00

# ניהול פרוייקטים עם חבילות, מכולות, ומודולים

כשתתחילו לכתוב תוכניות גדולות יותר, ארגון הקוד שלכם יהפוך למטלה חשובה ביותר. דרך יעילה לעשות זאת היא איגוד פונקציונאליות קרובה מחד, ומאידך, הפרדת קוד בעל מטרות מוגדרות ומסוימות לתוך פונקציות או מודולים משלו. כך תבהירו לקורא היכן יוכל למצוא קוד שמיישם תכונה זו או אחרת, ולאן לפנות כדי לבצע שינויים במידת הצורך.
כל אחת מהתכניות שכתבנו עד עתה התקיימה כולה בתוך מודול אחד ובקובץ אחד. עם זאת, כאשר פרוייקט גדל במימדיו, עדיף לארגן את הקוד באמצעות חלוקה שלו למספר מודולים, ואף למספר קבצים. חבילה יכולה להכיל כמה מכולות בינאריות, ואף מכולת ספריה אחת במידת הצורך. ככל שהחבילה גדלה, ניתן להעביר חלקים ממנה למכולות נפרדות ולהגדירן כתלותות חיצוניות. פרק זה ידון בטכניקות מעין אלה. עבור פרוייקטים גדולים במיוחד המכילים אוסף חבילות הקשורות זו לזו ושמתפתחות יחדיו, קארגו מספק _מרחבי עבודה_ (workspaces), בהם נדון בסעיף [“Cargo Workspaces”][workspaces]<!-- ignore --> בפרק 14.

בנוסף, נדון באפסון (encapsulation) של פרטי מימושים, אשר מאפשר שימוש חוזר בקוד ברמה עילית: מהרגע בו ישמתם פעולה מסויימת, קוד אחר מסוגל לקרוא לקוד שלכם דרך הממשק הפומבי שלו, מבלי לדעת כיצד בדיוק הוא ממומש. האופן בו כותבים קוד מגדיר אלו חלקים הם פומביים לשימוש קוד אחר, ואלו חלקים הם מימושים פרטיים, עבורם אתם שומרים את הזכות לבצע שינויים כאוות נפשכם. זוהי דרך נוספת להגביל את כמות הפרטים אליהם אתם צריכים להיות מודעים בזמן הפיתוח.

בהקשר זה נזכיר שוב את מושג המתחם: להקשר המקונן בו קוד נכתב יש קבוצת שמות המוגדרים כמתקיימים "בתוך המתחם." כאשר קוראים, כותבים, ומקמפלים קוד, מתכנתים וקומפיילרים צריכים לדעת האם שם ספציפי המופיע בנקודה נתונה מתייחס למשתנה, פונקציה, מבנה, מבחר, מודול, קבוע, או כל אובייקט אחר, ומה משמעותו. באמצעות יצירת מתחמים ניתן להגדיר ולנהל את קבוצת השמות הנכנסים והיוצאים מן המתחם. אי-אפשר שיתקיימו שני עצמים עם אותו השם באותו המתחם; קיימים כלים הנועדו לפתור קונפליקטים בין שמות.

לראסט כמה תכונות המקלות על ארגון הקוד שלכם, החל מקבלת החלטות אודות אלו פרטי מימוש יהפכו לפומביים, ואלו יוותרו פרטיים, ואלו שמות נמצאים במתחמים השונים בתוכניות שלכם. תכונות אלה, שכמכלול נקראות לעיתים _מערכת המודולים_, כוללות:

- **חבילות:** תכונה של קארגו שמאפשרת לבנות, לבדוק, ולחלוק מכולות
- **מכולות:** עץ של מודולים שיוצר ספריה או קובץ הרצה
- **use** ו-**מודולים**: מאפשרים לשלוט בארגון, מתחמים, ופרטיות של מסלולים
- **מסלולים:** דרך לנקוב בשם של אובייקט, כמו מבנה, פונקציה, או מודול

בפרק זה נציג תכונות אלה, נדון באופן בו הן משפיעות זו על זו, ונסביר איך להשתמש בהן כדי לנהל את המתחם. בסוף הפרק תהיה לכם הבנה יסודית של מערכת המודולים ותהיו מסוגלים לעבוד עם מתחמים בצורה מקצועית!

[workspaces]: ch14-03-cargo-workspaces.html
