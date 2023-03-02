## תיעוד קוד באמצעות הערות

מתכנתים תמיד משתדלים לכתוב קוד קריא וקל להבנה, אבל לפעמים יש צורך בתיאורים מפורטים יותר. במקרים כאלה, מתכנתים משאירים, בנוסף לקוד, *הערות* (comments) המיועדות לאנשים שקוראים את הקוד, מהן הקומפיילר מתעלם.

הינה דוגמא פשוטה:

```rust
// hello, world
```

בראסט, הסגנון המקובל לכתיבת הערות הוא להתחיל הערה עם שני סימני לוכסן, ואז ההערה ממשיכה עד סוף השורה. עבור הערות שנמשכות מעבר לשורה אחת, ניתן להוסיף `//` בתחילת כל שורה, כך:

```rust
// So we’re doing something complicated here, long enough that we need
// multiple lines of comments to do it! Whew! Hopefully, this comment will
// explain what’s going on.
```

ניתן גם למקם הערה בסוף של שורת קוד:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-24-comments-end-of-line/src/main.rs}}
```

אבל, בדר"כ תראו הערות כתובות בפורמט הבא, כאשר ההערה ממוקמת בשורה נפרדת מעל לקוד אותו היא מבארת:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-25-comments-above-line/src/main.rs}}
```

בראסט יש גם סוג אחר של הערות, הערות תיעוד, בהן נדון בסעיף [“Publishing a Crate to Crates.io”][publishing]<!-- ignore -->
בפרק 14.

[publishing]: ch14-02-publishing-to-crates-io.html
