## 0.25 (unreleased)

### Display

Difftastic no longer shows "1/1" when a file only has a single hunk.

Improved Clojure and Scala syntax highlighting.

When a file is entirely unchanged, difftastic now shows "no changes"
even if it successfully parsed. Previously it would only show "no
syntactic changes".

### Parsing

Fixed an issue in C and C++ where blank lines were highlighted after
novel preprocessor lines.

## 0.24 (release 26th March 2022)

### Diffing

Reduced the default value of DFT_NODE_LIMIT from 100,000 to
30,000. This fixes cases where files near the limit would use too much
memory and not terminate.

### Display

Fixed an issue where hunks would be missing lines. This occurred in
certain circumstances when a line contained both changed and unchanged
parts.

Fixed an issue where blank lines at the beginning or end of a file
would be excluded from context.

Fixed an issue where lines containing only whitespace would be
highlighted in purple.

Fixed an issue with changed multiline strings where blank lines were
not highlighted.

Improved Clojure syntax highlighting.

### Parsing

Added support for Dart.

### Command Line Interface

Difftastic will now warn if both arguments point to the same file.

When diffing directories, diff results are printed incrementally
rather than waiting for the results of all files before printing.

## 0.23.1 (released 19th March 2022)

Fixed crash where the 'shrink unchanged' logic would not set the
change state on the outer list.

## 0.23 (released 17th March 2022)

### Diffing

Improved performance on very large files that are compared by text.

Fixed some cases where changing list delimiters would lead to
incorrect diffs.

Fixed an issue where lines were not aligned correctly after correcting
sliders.

Fixed an issue the outermost delimiter in lists was sometimes
incorrectly marked as unchanged, producing non-optimal diffs.

### Display

Display now prefers to align blank lines in the display, producing
significantly better results in many cases.

Fixed an issue where some lines in a hunk were not displayed.

## 0.22 (released 10th March 2022)

Difftastic now requires Rust 1.56 to build.

### Parsing

Added support for PHP.

Fixed handling of `<` `>` delimiters in C++ and Rust.

### Diffing

Difftastic will now split files that contain obviously unchanged
regions, substantially improving performance when a file has multiple
changes that have many unchanged items between them.

Improved diff results when choosing between syntax nodes at different
nesting levels. This is restoring a heuristic that was removed in
0.20.

Improved diff results when lists have unequal sizes.

Improved diff results when the language parser thinks that names occur
in different syntactic positions.

Adjusted the heuristics for 'so much has changed in this expression
that it is confusing to highlight the unchanged parts'. The heuristic
is now less aggressive, which helps performance and seems to produce
slightly better results.

## 0.21 (released 28th February 2022)

### Parsing

Difftastic now understands `-*-` file headers (as used by Emacs) when
performing language detection.

### Display

Improved alignment logic. This fixes a bug where the last line of a
file wasn't displayed, and generally improves how difftastic chooses
to align content.

Fixed a crash when line wrapping produced an entirely blank line.

### Diffing

Improved word diffing (in both comments and textual diffs) when source
contains Unicode characters. Word splitting now uses the Unicode
alphabetic property.

Fixed a crash when comments contained multibyte Unicode characters.

## 0.20 (released 20th February 2022)

### Diffing

Diffing now correctly handles nodes being moved to parent
lists. Previously this would be ignored, leading to difftastic
incorrectly claiming things were unchanged. This also leads to better
diffing results in general, although is somewhat slower (2x in
testing).

Improved slider logic in larger expressions.

Increased the default value DFT_NODE_LIMIT to 100,000 (from
50,000). This increases the likelihood that files get a syntactic diff
whilst still having acceptable performance.

### Display

Fixed an issue where whole file additions/removals were printed twice.

Fixed an issue where difftastic didn't show context on hunks where the
unchanged content was on different lines.

Hunks are now merged if the lines are immediately adjacent
(e.g. hunk 1 ends on line 11, hunk 2 starts on line 12), not just if
they're overlapping.

### Command Line Interface

Difftastic will now use a text dif for large files that are too big to
parse in a reasonable amount of time. This threshold is
configurable with `--byte-limit` and `DFT_BYTE_LIMIT`.

Fixed a crash when called with zero arguments.

## 0.19 (released 7th February 2022)

### Parsing

Fixed an issue with changes being ignored in OCaml's `{||}` string
literals.

### Display

Fixed an issue where larger additions were not lined up with removals.

Improved syntax highlighting for Clojure, Common Lisp and TypeScript.

Comments are now highlighted with italics, making it easier to see
syntax even when text is red.

Built-in constants are now highlighted consistently with other
constants.

Improved minor display issues when one file is longer than the other.

### Diffing

If given binary files, difftastic will now report if the file contents
are identical.

Difftastic will now use a text diff for large files, rather than
trying to use more memory than is available. This threshold is
configurable with `--node-limit` and `DFT_NODE_LIMIT`.

Fixed a bug in the text diff logic where lines weren't shown if they
did not have both word additions and word removals.

### Command Line Interface

Difftastic will now error if either argument does not exist, unless
`--missing-as-empty` (new argument) is passed. This is a better
default, but requires Mercurial uses to [specify this
flag](https://difftastic.wilfred.me.uk/mercurial.html) in their
configuration.

## 0.18.1 (released 30 January 2022)

Fixed a compilation issue on Rust 1.54 (0.18 only built on newer
versions of Rust).

## 0.18 (released 30 January 2022)

### Parsing

Fixed an issue with missing positions in OCaml attribute syntax.

Fixed parsing issues in Common Lisp: character literals, `loop` macro
usage with `maximizing`.

### Diffing

Improved performance when diffing a single large expression.

### Display

Fixed display issues where lines were printed more than once.

Subword changes in comments are now shown in bold, to make them more
visible.

Improved colours on terminals with light coloured backgrounds.

### Command Line Interface

Added a `--width` option which allows overriding `DFT_WIDTH`, and is
more discoverable.

Added a `--color` option which allows explicitly enabling/disabling
colour output.

Added a `--background` option which controls whether difftastic uses
bright or dark colours. This can also be controlled by
`DFT_BACKGROUND`.

Added a `--skip-unchanged` option which suppresses printing for files
that have no changes.

## 0.17 (released 25 January 2022)

### Diffing

Improved performance when all file changes are close together.

Fixed a bug where syntax after the last changed item was incorrectly
marked as added.

### Display

Added syntax highlighting for unchanged comments, strings and types.

Fixed a bug (introduced in 0.15) where identical text files were
reported as binary files.

## 0.16 (released 22 January 2022)

### Parsing

Whitespace in JSX is parsed more closely to React's whitespace
trimming rules.

Fixed parsing of heredocs in shell scripts. They are now treated as
string literals.

Fixed parsing of type variables and tags in OCaml.

Improved language detection for files with bash/sh syntax.

### Integration

Fixed a crash when on Mercurial diffs when a whole file has been
removed.

### Display

Improved display performance when there are a large number of hunks.

Fixed several issues where lines were displayed more than once in a
hunk.

Fixed an issue where the first changed line was not displayed.

### Diffing

Improved diffing performance (both time and memory usage).

Sliders are now fixed up after diffing. This produces better looking
results in more cases, and makes the primary diffing faster.

Fixed some corner cases in the line parser where it would match up
isolated newline character as unchanged, leading to weird alignment.

## 0.15 (released 6 January 2022)

### Parsing

Moved to the [official Elixir
parser](https://github.com/elixir-lang/tree-sitter-elixir).

Updated the Bash, C, C++, C#, Haskell, Java, OCaml, Python, Ruby and
TypeScript parsers to the latest upstream version.

Fixed a parsing performance regression introduced in 0.13.

### Diffing

Text diffing now has a standalone implementation rather than reusing
structural diff logic. This is signficantly faster and highlighted
better.

Improved performance when diffing two identical files. This is common
when diffing directorires.

### Display

Improved highlighting heuristics for added/removed blank lines.

Fixed an alignment bug where the last line being novel would lead to
poor alignment of unchanged lines.

Fixed minor formatting issues when reporting that a file is binary.

Improved display performance on large files.

## 0.14 (released 27 December 2021)

### Parsing

Improved language detection if a file has a recognised filename
(e.g. `Rakefile`) or a shebang (e.g. `#!/usr/bin/env node`).

### Display

Display width can now be overridden by setting the environment
variable DFT_WIDTH.

Fixed terminal width calculations on Windows.

Fixed crash when only one side has changes, but the other side has
additional blank lines.

Fixed crash on displaying unicode characters on line boundaries.

### Build

Fixed some build issues on Windows.

## 0.13 (released 4 December 2021)

### Parsing

Added Bash, Common Lisp and Ruby support.

Updated the C, CSS and JSON parsers to the latest upstream versions.

Expanded filename associations, so difftastic recognises more files.

Improved parsing for regex and template string literals in JavaScript
and TypeScript.

Improved parsing for float values in CSS.

Improved word diffing on punctuation in comments.

When logging is enabled (e.g. `RUST_LOG=warn`), difftastic now warns
on syntax errors. Difftastic is intended to be robust against syntax
errors, so this is primarily intended for parser debugging.

### Build

Difftastic now requires fewer C compiler flags, so it should build in
more environments (e.g. compiling with MSVC).

## 0.12 (released 19 November 2021)

### Display

Every hunk is now shown with the file name and a hunk number. This
makes it easier to see which file you're looking at when there are
many changes.

Keywords in added/removed regions are now shown in bold, to give
modified regions basic syntax highlighting. Previously, all
added/removed regions were bold.

Lines with changes are now shown in a different colour in side-by-side
display.

The display logic has been written in terms of a `Hunk` type. This
produces more accurate context, with better alignment, especially when
the context contains blank lines.

If only a single side has changes (e.g. additions but no removals),
only one column is shown, to maximise display usage.

Difftastic now wraps rather than truncating lines that are too long
for the terminal width.

If a file has no syntactic changes, difftastic now shows the file name
consistently with changed files.

### Command Line Interface

The difftastic binary is now named `difft`, to reduce typing during
usage.

### Parsing

Updated to latest upstream Haskell parser ([commit
d72f2e4](https://github.com/tree-sitter/tree-sitter-haskell/commit/d72f2e42c0d5ccf8e8b1c39e3642428317e8fe02)).

### Diffing

Fixed a bug when diffing multiline comments where unchanged parts were
not highlighted correctly.

## 0.11 (released 18 October 2021)

### Parsing

Improved handling of paired delimiters, particularly in C, C++ and C#.

Improved word splitting in when diffing similar comments (it's now
more granular).

Fixed a rare issue where single-item lists were flattened.

### Diffing

Diff calculations are now greedier when syntax nodes are identical, making
diffing significantly faster when most syntax nodes are the same.

### Integration

Added support for Mercurial, see [this section in the
manual](http://difftastic.wilfred.me.uk/getting_started.html#mercurial-external-diffs)
for instructions.

### Display

Added basic syntax highlighting for comments (dimmed) and keywords
(bold) in unchanged source code.

Characters that don't have a position in the parsed syntax tree are
now displayed in purple, to make bugs more obvious. Previously they
were dimmed.

## 0.10.1 (released 28 September 2021)

### Build

Fix compilation on macOS where the C++ compiler defaulted to a
version of C++ older than C++14.

## 0.10 (released 24 September 2021)

### Parsing

Added a C parser.

Added a C++ parser. Difftastic prefers the C++ parser for `.h`
files. Please file a bug if you see issues.

Added a C# parser.

Added a Haskell parser.

Removed legacy regex-based parsing backend.

### Diffing

Some additional runtime optimisations.

### Manual

Added a chapter on difficult cases for tree diff algorithms.

## 0.9 (released 14 September 2021)

### Parsing

Added TypeScript parser and TSX parser. Added Elixir parser.

The following extensions are now associated with Clojure: `.bb`,
`.boot`, `.clj`, `.cljc`, `.clje`, `.cljs`, `.cljx`, `.edn`, `.joke`
and `.joker`.

Fixed an issue with parsing integer values in CSS with units,
e.g. `123px`.

Improved parsing of Rust punctuation like `&` and `::` inside macro
invocations. Improved handling of `|closure_param|` and `[` `]`
delimiters in Rust.

The line-based parser for text files now uses word-level diffs.

### Diffing

Optimised Dijkstra implementation, improving runtime performance.

### Display

Side-by-side displays now uses the same width for the left and right
columns, regardless of the content.

### Internals

Difftastic is now a library with a main binary. No APIs are considered
stable for external usage. This is intended to make benchmarking
easier.

## 0.8 (released 5 September 2021)

### Git integration

Fixed a crash on removing whole files.

### Parsing

Tree-sitter parsing is now the default, unless the environment
variable DFT_RX is set.

Tree-sitter parser: Improved handling of string literals. Improved
matching of delimiters.

Added Python parser.

Added Java parser.

JSON (legacy parser): fixed parsing string literals (broken in 0.7).

Removed Scheme support, as there's no tree-sitter parser available.

### Display

Fixed crashes on files with non-ASCII characters on long lines.

Fixed an issue where multiline comments were not highlighted
correctly.

Improved display to better use the whole width when whole files are
added or removed.

### Command Line Interface

Removed the unused `--lang` argument.

Difftastic now handles writing to a closed pipe (SIGPIPE) gracefully
rather than crashing.

Difftastic now has some debugging logs available. `RUST_LOG=trace`
will show information on the route found during graph solving.

## 0.7 (released 24 August 2021)

### Git integration

Fixed issues when adding/removing a whole file meant that difftastic
didn't display anything.

Fixed a crash on renaming a file.

Colour is now enabled when using git with a pager.

### Display

Side-by-side display now uses "..." for column numbers when aligning
lines. This makes hunks more obvious, but hunks now also have two
blank lines between them to make it clearer.

Fixed an issue where screen width was not shared evenly by LHS and
RHS.

Side-by-side display will now use the full width of the screen when
using a pager (i.e. if stdout is a not a TTY).

Side-by-side display now handles whole file additions better,
preferring a single column display.

Display width calculations are now based on the longest line visible
in the diff, not the longest line in the file.

### Parsing

Added tree-sitter parsers. These have known bugs, but you can try
them by setting the environment variable `DFT_TS=y`.

Fixed handling of `->` in Rust.

### Diffing

Difftastic will now prefer matching up comments that are similar
(according to levenshtein distance).

Contiguous syntax logic now considers close delimiter positions, so
`[ \n ];` now treats the `;` atom as contiguous.

Fixed an issue where diffs would prefer prefer a low depth change on a
delimiter over a delimiter that gave contiguous changes.

### Command Line Interface

Removed the `--width` argument.

Added debug options `--dump-syntax` and `--dump-ts` for viewing parse
trees. The output of these options may change without notice.

## 0.6 (released 27 July 2021)

### Parsing

Fixed handling of `@`, `<` and `>` in elisp.

Fixed crash on binary files. Difftastic now simply shows "binary" for
files that don't look like text.

Added a basic Go parser.

### Diffing

Fixed an issue where comment replacements were not detected.

Changed words in comments are now only highlighted when comments are
relatively similar (according to their Levenshtein distance).

Multiline comments are now considered unchanged if only their
indentation changes.

Improved alignment for lines at the beginning of a changed group of
lines.

Improved horizontal spacing between before and after code shown.

Fixed an issue where source code containing tab characters was not
correctly aligned.

### Command Line Interface

Removed unused `--inline` and `--context` arguments.

Fixed crash when called with no arguments.

## 0.5 (released 22 July 2021)

### Parsing

Fixed a crash on parsing non-ASCII source files. Fixed a crash on
files without an extension. Fixed crashes on empty files.

Input files that aren't valid UTF-8 are now replaced with � rather
than giving up.

Improved parsing for Rust punctuation.

Improved parsing for OCaml punctuation, including `:=` and `method!`.

Improved parsing for Emacs Lisp symbols containing `+` and `=`, and
punctuation of `#`, `.` and `&`.

Improved parsing for Scheme symbols containing `=`, and punctuation of
`#` and `.`.

Improved parsing of `=` and `&` in Clojure.

Improved parsing of `:`, `,`, and constants in JSON.

Improved parsing of string literals in all languages, supporting
escaped delimiters such as `"\""` and removing incorrect support for
single-quoted strings in JSON.

### Diffing

Reduced memory usage when diffing.

Difftastic now highlights word-level changes between comments.

Diffing now prefers contiguous nodes even when entering a list, so
`(foo` is considered contiguous.

Large AST trees with very few common nodes are now considered wholly
novel, rather than trying to match up the few common nodes. This
avoids nonsensical diffs when toplevel function A is completely
replaced with function B and they only have something trivial in
common (e.g. the `function` keyword).

### Command Line Interface

Improved `--help`.

### Integration

It's now possible to use `difftastic` with `git diff` and `git show`!

## 0.4 (released 13 July 2021)

### Parsing

Improved parsing for Rust macro definitions and punctuation.

Improved parsing for OCaml punctuation, and added `.mli` as an OCaml
file extension.

### Diffing

Diff calculation is now significantly faster.

Difftastic now considers nesting depth when comparing AST nodes, and
tries to match nodes with similar nesting levels.

Difftastic now prefers marking multiple items on the same line as
novel, rather than adjacent items on different lines. This helps avoid
[sliders](https://twitter.com/_wilfredh/status/1411949035871637509),
where the diff chooses a keyword on the 'wrong' side.

Fixed an issue where complex diffs would not display some unchanged
lines.

### Robustness

Fixed a crash when diff context included the first line.

Fixed a crash when plain text content contained certain non-ASCII
characters.

## 0.3 (released 7 July 2021)

Diffs are now displayed with unchanged lines aligned to the other side.

Improved Rust parsing to recognise lifetime syntax `'foo`, character
literals `'x'` and punctuation.

Improved punctuation parsing for OCaml and JS.

Fixed an issue where the diff calculated may not be minimal.

Fixed a crash on files with no changes.

## 0.2 (released 4 July 2021)

First version using Dijkstra's algorithm for calculating diffs.

## 0.1

Experimenting with different implementation ideas.
