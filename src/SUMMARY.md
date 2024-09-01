# The Rust Programming Language

[שפת התכנות ראסט](title-page.md)
[הקדמה](foreword.md)
[מבוא](ch00-00-introduction.md)

## Getting started

- [מתחילים](ch01-00-getting-started.md)
    - [התקנה](ch01-01-installation.md)
    - [שלום עולם!](ch01-02-hello-world.md)
    - [שלום קארגו!](ch01-03-hello-cargo.md)

- [תכנות משחק ניחוש מספר](ch02-00-guessing-game-tutorial.md)

- [עקרונות תכנות נפוצים](ch03-00-common-programming-concepts.md)
    - [משתנים וברות-שינוי](ch03-01-variables-and-mutability.md)
    - [טיפוסי דאטה](ch03-02-data-types.md)
    - [פונקציות](ch03-03-how-functions-work.md)
    - [הערות](ch03-04-comments.md)
    - [בקרת זרימה](ch03-05-control-flow.md)

- [הבנת מושג הבעלות](ch04-00-understanding-ownership.md)
    - [מהי בעלות?](ch04-01-what-is-ownership.md)
    - [הפניות והשאלות](ch04-02-references-and-borrowing.md)
    - [טיפוס החיתוך](ch04-03-slices.md)

- [שימוש במבנים בכדי לעצב דאטה מקושר](ch05-00-structs.md)
    - [הגדרה ואתחול של מבנים](ch05-01-defining-structs.md)
    - [תכנית לדוגמא המשתמשת במבנים](ch05-02-example-structs.md)
    - [תחביר מתודות](ch05-03-method-syntax.md)

- [מבחרים והתאמת דפוסים](ch06-00-enums.md)
    - [הגדרת מבחרים](ch06-01-defining-an-enum.md)
    - [המבנה התחבירי `match` לבקרת זרימה](ch06-02-match.md)
    - [`if let` בקרת זרימה מתומצתת עם](ch06-03-if-let.md)

## Basic Rust Literacy

- [ניהול פרוייקטים עם חבילות, מכולות, ומודולים
](ch07-00-managing-growing-projects-with-packages-crates-and-modules.md)
    - [חבילות ומכולות](ch07-01-packages-and-crates.md)
    - [שימוש במודולים לשליטה על מתחמים ופרטיות](ch07-02-defining-modules-to-control-scope-and-privacy.md)
    - [מסלולים להפניה לעצם בתוך עץ המודולים](ch07-03-paths-for-referring-to-an-item-in-the-module-tree.md)
    - [הכנסת מסלולים למתחם באמצעות מילת המפתח `use`](ch07-04-bringing-paths-into-scope-with-the-use-keyword.md)
    - [הפרדת מודולים לקבצים שונים](ch07-05-separating-modules-into-different-files.md)

- [אוספים נפוצים](ch08-00-common-collections.md)
    - [אכסון רשימות של ערכים כוקטורים](ch08-01-vectors.md)
    - [אחסון טקסט בקידוד UTF-8 כמחרוזת](ch08-02-strings.md)
    - [אחסון מפתחות עם ערכים משויכים במפות גיבוב](ch08-03-hash-maps.md)

- [טיפול בשגיאות](ch09-00-error-handling.md)
    - [שגיאות סופניות ופאניקה](ch09-01-unrecoverable-errors-with-panic.md)
    - [שגיאות ברות-שיקום ו-`Result`](ch09-02-recoverable-errors-with-result.md)
    - [להיכנס לפאניקה -- כן או לא](ch09-03-to-panic-or-not-to-panic.md)

- [טיפוסים גנרים, תכונות, ומשך-חיים](ch10-00-generics.md)
    - [טיפוסי נתונים גנרים](ch10-01-syntax.md)
    - [תכונות: הגדרת התנהגות משותפת](ch10-02-traits.md)
    - [וידוא הפניות באמצעות משך-חיים](ch10-03-lifetime-syntax.md)

- [Writing Automated Tests](ch11-00-testing.md)
    - [How to Write Tests](ch11-01-writing-tests.md)
    - [Controlling How Tests Are Run](ch11-02-running-tests.md)
    - [Test Organization](ch11-03-test-organization.md)

- [An I/O Project: Building a Command Line Program](ch12-00-an-io-project.md)
    - [Accepting Command Line Arguments](ch12-01-accepting-command-line-arguments.md)
    - [Reading a File](ch12-02-reading-a-file.md)
    - [Refactoring to Improve Modularity and Error Handling](ch12-03-improving-error-handling-and-modularity.md)
    - [Developing the Library’s Functionality with Test Driven Development](ch12-04-testing-the-librarys-functionality.md)
    - [Working with Environment Variables](ch12-05-working-with-environment-variables.md)
    - [Writing Error Messages to Standard Error Instead of Standard Output](ch12-06-writing-to-stderr-instead-of-stdout.md)

## Thinking in Rust

- [Functional Language Features: Iterators and Closures](ch13-00-functional-features.md)
    - [Closures: Anonymous Functions that Capture Their Environment](ch13-01-closures.md)
    - [Processing a Series of Items with Iterators](ch13-02-iterators.md)
    - [Improving Our I/O Project](ch13-03-improving-our-io-project.md)
    - [Comparing Performance: Loops vs. Iterators](ch13-04-performance.md)

- [More about Cargo and Crates.io](ch14-00-more-about-cargo.md)
    - [Customizing Builds with Release Profiles](ch14-01-release-profiles.md)
    - [Publishing a Crate to Crates.io](ch14-02-publishing-to-crates-io.md)
    - [Cargo Workspaces](ch14-03-cargo-workspaces.md)
    - [Installing Binaries from Crates.io with `cargo install`](ch14-04-installing-binaries.md)
    - [Extending Cargo with Custom Commands](ch14-05-extending-cargo.md)

- [Smart Pointers](ch15-00-smart-pointers.md)
    - [Using `Box<T>` to Point to Data on the Heap](ch15-01-box.md)
    - [Treating Smart Pointers Like Regular References with the `Deref` Trait](ch15-02-deref.md)
    - [Running Code on Cleanup with the `Drop` Trait](ch15-03-drop.md)
    - [`Rc<T>`, the Reference Counted Smart Pointer](ch15-04-rc.md)
    - [`RefCell<T>` and the Interior Mutability Pattern](ch15-05-interior-mutability.md)
    - [Reference Cycles Can Leak Memory](ch15-06-reference-cycles.md)

- [Fearless Concurrency](ch16-00-concurrency.md)
    - [Using Threads to Run Code Simultaneously](ch16-01-threads.md)
    - [Using Message Passing to Transfer Data Between Threads](ch16-02-message-passing.md)
    - [Shared-State Concurrency](ch16-03-shared-state.md)
    - [Extensible Concurrency with the `Sync` and `Send` Traits](ch16-04-extensible-concurrency-sync-and-send.md)

- [יכולות של תכנות מונחה-עצמים בראסט](ch17-00-oop.md)
    - [מאפיינים של שפות מונחות-עצמים](ch17-01-what-is-oo.md)
    - [שימוש באובייקטי תכונה שמאפשרים ערכים מטיפוסים שונים](ch17-02-trait-objects.md)
    - [מימוש דפוס תכנון מונחה-עצמים](ch17-03-oo-design-patterns.md)

## Advanced Topics

- [תבניות והתאמות](ch18-00-patterns.md)
    - [כל המקומות בהם ניתן להשתמש בתבניות](ch18-01-all-the-places-for-patterns.md)
    - [ניתנות להפרכה: האם התאמה מול תבנית יכולה להיכשל?](ch18-02-refutability.md)
    - [תחביר תבניות](ch18-03-pattern-syntax.md)

- [נושאים מתקדמים](ch19-00-advanced-features.md)
    - [ראסט לא מאובטחת](ch19-01-unsafe-rust.md)
    - [נושאים מתקדמים אודות תכונות](ch19-02-advanced-traits.md)
    - [נושאים מתקדמים אודות טיפוסים](ch19-03-advanced-types.md)
    - [נושאים מתקדמים אודות פונקציות וסגורים](ch19-04-advanced-functions-and-closures.md)
    - [מקרואים](ch19-05-macros.md)

- [Final Project: Building a Multithreaded Web Server](ch20-00-final-project-a-web-server.md)
    - [Building a Single-Threaded Web Server](ch20-01-single-threaded.md)
    - [Turning Our Single-Threaded Server into a Multithreaded Server](ch20-02-multithreaded.md)
    - [Graceful Shutdown and Cleanup](ch20-03-graceful-shutdown-and-cleanup.md)

- [Appendix](appendix-00.md)
    - [A - Keywords](appendix-01-keywords.md)
    - [B - Operators and Symbols](appendix-02-operators.md)
    - [C - Derivable Traits](appendix-03-derivable-traits.md)
    - [D - Useful Development Tools](appendix-04-useful-development-tools.md)
    - [E - Editions](appendix-05-editions.md)
    - [F - Translations of the Book](appendix-06-translation.md)
    - [G - How Rust is Made and “Nightly Rust”](appendix-07-nightly-rust.md)
