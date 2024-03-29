# אוספים נפוצים

הספרייה הסטנדרטית של ראסט כוללת מספר מבני נתונים שימושיים שנקראים _אוספים_. רוב מבני הנתונים מייצגים ערך בודד בעוד אוספים יכולים להכיל ערכים רבים. בניגוד למערך והרצף המובנים לתוך השפה, הערכים אליהם האוספים הללו מצביעים נמצאים בערימה, כך שכמות הערכים לא צריכה להיות ידועה בזמן הקומפילציה והיא יכולה לגדול או לקטון בעת הריצה. לכל סוג של אוסף ישנן יכולות ועלויות שונות, והיכולת לבחור את האוסף הנכון עבור מצב נתון הוא כישור שמתפתח עם הזמן. בפרק זה, נדון בשלושה סוגי אוספים שתוכנות ראסט משתמשות בהם באופן קבוע:

- _וקטור_ מאפשר לאחסן כמות משתנה של ערכים בסמיכות אחד לשני.
- _מחרוזת_ היא אוסף של תווים. הצגנו אותה בפרקים קודמים, אך בפרק זה נדון בה לעומק.
- _מפת גיבוב_ מאפשרת לקשר בין ערך למפתח מסויים. זהו מימוש ספציפי של מבנה נתונים כללי יותר שמכונה _מפה_.

כדי ללמוד על סוגי אוספים נוספים שהספרייה הסטנדרטית מספקת, יש לפנות ל[תיעוד][collections].

אנחנו נדון ביצירתם ועידכונם של וקטורים, מחרוזות ומפות גיבוב וכן במה שמייחד אותם.

[collections]: ../std/collections/index.html
