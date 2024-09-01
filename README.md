# The Rust Programming Language - Hebrew Translation
This is a translation into Hebrew of the main branch of the GitHub repository of the original book. The translation proceeds by replacing each of the files in the src directory by its translation. In this way, building the translated book is identical to building the original: mdbook build. This would build the entire book, and so the portions that have been translated would be in Hebrew, while the rest would be in the original English. 

## Translation status and auxiliary files
Chapters 1-10 have been translated and reviwed. Chapter 17, 18, and 19 have been translated but not reviewed yet.

the file HebEngTerm.txt in the root directory contains a list of standard Rust-related terminology with the chosen translations into Hebrew. 

The file HebEditingGuidelines.md in the rood directory contains general guidelines for contributors interested in translating. 

## Style of translation
The translation aims to preserve the feel of the original text, while ahering to Hebrew idioms. That means that time tenses are mostly preserved, except when sounding very awkward in Hebrew, or if deviating from the original style significantly improves clarity. 


rust-lang/rust uses in [this file][rust-mdbook]. To get it:

[mdBook]: https://github.com/rust-lang-nursery/mdBook
[rust-mdbook]: https://github.com/rust-lang/rust/blob/master/src/tools/rustbook/Cargo.toml

```bash
$ cargo install mdbook --version <version_num>
```

## Building

To build the book, type:

```bash
$ mdbook build
```

The output will be in the `book` subdirectory. To check it out, open it in
your web browser.

_Firefox:_
```bash
$ firefox book/index.html                       # Linux
$ open -a "Firefox" book/index.html             # OS X
$ Start-Process "firefox.exe" .\book\index.html # Windows (PowerShell)
$ start firefox.exe .\book\index.html           # Windows (Cmd)
```

_Chrome:_
```bash
$ google-chrome book/index.html                 # Linux
$ open -a "Google Chrome" book/index.html       # OS X
$ Start-Process "chrome.exe" .\book\index.html  # Windows (PowerShell)
$ start chrome.exe .\book\index.html            # Windows (Cmd)
```

To run the tests:

```bash
$ mdbook test
```

## Contributions to the translation effort
Contributions are very welcomed. It is recommended to familiarise yourself with the already translated material before translating other chapters, and to strive for a similar style of translation. 

Please follow the following recommended steps in order to contribute:
1) Identify an improvement and consult the open and closed issues. Seek for an existing discussion about your observation, and contribute to the discussion. Seek to see if it was already propsoed in the past but closed, and understand why. Seek to see if it is currently an open issue, and whether it is being carried out. 
2) If you are convinced your observation merits opening an issue, do so. 
3) Use the following naming convention for your issue: start the name with "Ch" followed by two digits for the chapter number, then "Se" followed by two digits for the section number, and then a hyphen followed by the text of the subsection (if it exists). Then double hyphen, and include a short description of the observation. For instance: Ch04Se02-Mutable References -- misspelling of the translation of 'mutable'. If the observation does not fit into this particular structure, use common sense. For instance: All chapters are missing the translation of the seventh word from the top.
4) If your observation is quick to perform, you may choose to immediate own the issue, make the improvement, and submit a PR. Otherwise, it is recommended that you let the issue stay open for a while to allow a discussion. It would be a shame if you work hard on an improvement, submit a PR, and then the PR is not accepted. 

# Active contributors
Ittay Weiss,
[Idan Liberman](https://github.com/IdanLib)
