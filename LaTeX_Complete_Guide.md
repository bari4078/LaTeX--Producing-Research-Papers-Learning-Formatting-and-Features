# The Complete LaTeX Survival Guide (for CSE200 Labs + Research Papers)

This guide assumes you know **zero** LaTeX. Every command is explained in plain words: what it does, how to type it, and what comes out. Keep this open in one tab and Overleaf in another during the lab.

---

## 0. What even is LaTeX, and how do I use it in a hurry?

Think of LaTeX like this: you type **plain text with special commands** (like `\textbf{hello}`), and a compiler turns it into a nicely formatted PDF. You never drag things around like in Word — you just write code, hit "compile," and see the result.

**Fastest way to work (recommended for timed labs):**
1. Go to [overleaf.com](https://overleaf.com), make a free account.
2. New Project → Blank Project.
3. Delete everything in the file, paste your code, click **Recompile** (top).
4. The PDF preview appears on the right. Every time you change code, click Recompile again.

**Every LaTeX document has this skeleton — nothing works outside it:**

```latex
\documentclass{article}

% packages go here (explained below)
\usepackage{graphicx}
\usepackage{amsmath}

\begin{document}

Your content goes here.

\end{document}
```

Nothing you write will show up unless it's between `\begin{document}` and `\end{document}`. Everything above that (the "preamble") is just setup.

**Packages** are add-on toolkits. You "turn them on" with `\usepackage{name}` in the preamble, before `\begin{document}`. Below, every section tells you which package it needs (if any).

**Here is the full list of packages you'll want loaded for these labs.** Just paste this whole block at the top of every document — it costs nothing to include a package you don't end up using:

```latex
\documentclass{article}

\usepackage[margin=1in]{geometry}   % sets page margins
\usepackage{graphicx}               % for images/figures
\usepackage{amsmath}                % for equations
\usepackage{amssymb}                % extra math symbols
\usepackage[normalem]{ulem}         % for strikethrough text
\usepackage{subcaption}             % for figures with sub-parts (1a, 1b, 1c)
\usepackage{multirow}               % for merged table cells
\usepackage{hyperref}               % clickable links + reference numbers
\usepackage{xcolor}                 % for colored text
```

---

## 1. The Title Block

Every sample article starts with a title, your name/ID, and a date. That's three commands plus one trigger command:

```latex
\title{The History Behind Leap Year}
\author{Your Name (Your Student ID)}
\date{\today}

\begin{document}
\maketitle
```

- `\title{}`, `\author{}`, `\date{}` just **store** the info.
- `\maketitle` is what actually **prints** it at the top of the page. Without this line, nothing shows up even if you wrote `\title{}`.
- `\today` auto-fills today's date. If the sheet says "Today's Date," use this. If it gives a fixed date, just type it: `\date{January 3, 2026}`.

---

## 2. Sections (auto-numbered headings)

```latex
\section{Introduction}
\subsection{Julian Calendar}
```

Output:
> **1 Introduction**
> **1.1 Julian Calendar**

You never type the "1" or "1.1" yourself — LaTeX numbers these automatically, and renumbers everything if you add/delete a section later. That's the entire point of using `\section` instead of just bolding text.

There's also `\subsubsection{}` if you need a third level (1.1.1).

---

## 3. Text Formatting

| What you want | Command | Example input | Output |
|---|---|---|---|
| **Bold** | `\textbf{...}` | `\textbf{bold text}` | **bold text** |
| *Italic* | `\textit{...}` | `\textit{italic text}` | *italic text* |
| *Emphasis* (also italic, but "smart" — see note) | `\emph{...}` | `\emph{emphasized text}` | *emphasized text* |
| Underline | `\underline{...}` | `\underline{underlined text}` | underlined text (with a line under it) |
| ~~Strikethrough~~ | `\sout{...}` | `\sout{365}` | ~~365~~ |

**Note on `\emph` vs `\textit`:** they look the same most of the time. The difference: if you nest `\emph` inside another `\emph` (or inside italic text), it flips back to upright automatically. For these labs, don't overthink it — use `\textit` for "italic text" and `\emph` when the source literally says "emphasized text."

**Important gotcha:** the strikethrough package (`ulem`) can silently turn ALL your `\emph{}` text into underlines instead of italics unless you load it as `\usepackage[normalem]{ulem}` (note the `[normalem]` part). Always include that option.

### Font sizes

These are "switches" — everything after them changes size until you close the `{ }` group:

```latex
{\Large This text is large.} Normal text continues here.
{\small This text is small.}
{\huge This text is huge.}
```

Full range, smallest to largest:
`\tiny`, `\scriptsize`, `\footnotesize`, `\small`, `\normalsize`, `\large`, `\Large`, `\LARGE`, `\huge`, `\Huge`

### Coloring Text

Needs `\usepackage{xcolor}` (already in your preamble block above).

**Colored text:**
```latex
\textcolor{red}{This text is red.}
```

**Built-in color names you can use right away:**
`red, blue, green, yellow, orange, purple, cyan, magenta, black, white, gray, brown, pink, teal, violet, lime, olive`

**Highlighting text with a background color** (like a marker/highlighter):
```latex
\colorbox{yellow}{This text is highlighted.}
```

**A custom color** (e.g. to match a specific shade) — define it once in the preamble, then reuse the name anywhere:
```latex
\definecolor{myblue}{RGB}{30, 100, 200}
```
```latex
\textcolor{myblue}{Custom colored text.}
```

**Lighter/darker shades of a built-in color, without defining anything new** — add `!` and a percentage:
```latex
\textcolor{blue!50}{Lighter blue (50% blue, 50% white)}
\textcolor{red!70!black}{Darker red (70% red mixed with black)}
```

**Coloring parts of a math equation:**
```latex
$\textcolor{red}{x^2} + \textcolor{blue}{y^2} = z^2$
```

**Combining a colored background with a bold "Note" label** (a nicer version of the plain bold-label trick from Section 4):
```latex
\colorbox{yellow!30}{\parbox{0.9\textwidth}{\textbf{Note:} Lists can be customized.}}
```
`\parbox{width}{...}` lets the colored box wrap onto multiple lines instead of only fitting one short phrase.

---

## 4. Lists

**Bullet list** (unordered) — needs `itemize`:
```latex
\begin{itemize}
    \item Scene Definition
    \item Camera Configuration
    \item Light Placement
\end{itemize}
```

**Numbered list** (ordered) — needs `enumerate`:
```latex
\begin{enumerate}
    \item Input Processing
    \item Ray Construction
\end{enumerate}
```
Output: `1. Input Processing`  `2. Ray Construction`

### Nesting lists inside lists (this is what the labs test the most)

Just put one list environment inside another. **LaTeX automatically changes the bullet/number style at each level** — you don't have to configure anything:

```latex
\begin{itemize}
    \item Primary Feature
    \item Secondary Features
    \begin{enumerate}
        \item Formatting control
        \item Mathematical typesetting
        \begin{itemize}
            \item Inline math
            \item Display math
        \end{itemize}
    \end{enumerate}
\end{itemize}
```

Output:
> • Primary Feature
> • Secondary Features
>   1. Formatting control
>   2. Mathematical typesetting
>      – Inline math
>      – Display math

**Automatic default styles by depth** (you get these for free, no package needed):
- `itemize` depth 1→4: • then – then * then ·
- `enumerate` depth 1→4: `1, 2, 3` then `(a), (b), (c)` then `i, ii, iii` then `A, B, C`

This means if a sheet shows sub-items as `(a)`, `(b)`, you almost never need to configure anything — just nest a second `enumerate` inside the first one and it happens automatically:

```latex
\begin{enumerate}
    \item \textbf{A year is a leap year if:}
    \begin{enumerate}
        \item It is divisible by 4 and not divisible by 100
        \item Or it is divisible by 400.
    \end{enumerate}
    \item \textbf{A year is \emph{NOT} a leap year in all other cases.}
\end{enumerate}
```

### Inserting a bold label in the middle of a list (like "Note", "Important", "Did you know?")

You saw things like `**Note** Lists can be customized.` sitting between list items. This is just a bold word followed by normal text — no special environment:

```latex
\noindent\textbf{Note:} Lists can be customized.
```

`\noindent` just prevents an unwanted extra indent on that line — safe to always include here.

---

## 5. Math / Equations

Two modes: **inline** (math sitting inside a sentence) and **display** (math on its own centered line).

**Inline math** — wrap it in single dollar signs:
```latex
Consider variables $x_i$, $y_j$, and $\sigma^2$.
```
Output: Consider variables *xᵢ*, *yⱼ*, and *σ²*.

**Display math, numbered** (most common ask on these sheets) — use the `equation` environment:
```latex
\begin{equation}
    S(n,k) = k \cdot S(n-1,k) + S(n-1,k-1)
\end{equation}
```
This centers the equation on its own line and automatically prints `(1)` next to it, numbering up as you add more.

**Display math, NOT numbered** — use `\[ ... \]`:
```latex
\[
    E = mc^2
\]
```

### Subscripts and superscripts (used constantly: x_i, y_j, e^x, σ²)

- Subscript: `_` → `x_i` gives *xᵢ*
- Superscript: `^` → `x^2` gives *x²*
- **If more than one character**, wrap it in `{}` or only the first character gets affected:
  - `x_ij` gives you *xᵢ* followed by a normal "j" (wrong!)
  - `x_{ij}` gives *xᵢⱼ* (correct)
  - Same for powers: `e^{x^2}` for *e^(x²)*

### Common building blocks

| What | Command | Result |
|---|---|---|
| Fraction | `\frac{a}{b}` | a/b, stacked |
| Square root | `\sqrt{x}` | √x |
| Sum | `\sum_{i=1}^{n}` | Σ from i=1 to n |
| Integral | `\int_{a}^{b}` | ∫ from a to b |
| Greek letters | `\alpha \beta \sigma \pi \Delta` | α β σ π Δ |
| Multiplication dot | `\cdot` | · |
| Infinity | `\infty` | ∞ |
| Not equal / less-equal | `\neq`, `\leq`, `\geq` | ≠ ≤ ≥ |
| Binomial coefficient | `\binom{n}{k}` | (n choose k), stacked |

### Multi-line equations (e.g. showing several steps aligned at the `=` sign)

Needs `amsmath` (already in your preamble). Use `align`, and put `&` right before the symbol you want lined up, `\\` to break to next line:

```latex
\begin{align}
    S(n,k) &= k \cdot S(n-1,k) + S(n-1,k-1) \\
    S(0,0) &= 1 \\
    S(n,0) &= 0, \quad n > 0
\end{align}
```
Each line gets its own equation number. Use `align*` (with a star) if you don't want numbers.

### Piecewise formulas (e.g. the Leap Year rule — "if X then Y, else Z")

Use `cases` **inside** an equation:

```latex
\begin{equation}
    \text{Leap Year}(Y) =
    \begin{cases}
        \text{Yes} & \text{if } Y \bmod 400 = 0 \\
        \text{No} & \text{if } Y \bmod 100 = 0 \\
        \text{Yes} & \text{if } Y \bmod 4 = 0 \\
        \text{No} & \text{otherwise}
    \end{cases}
\end{equation}
```
`\text{...}` is important — inside math mode, plain letters get italicized and squeezed together (looks like "IfYmod"); `\text{}` tells LaTeX "treat this bit as normal words."

### Referencing an equation number later in your text

```latex
\begin{equation}
    E = mc^2 \label{eq:energy}
\end{equation}

As shown in Equation~\ref{eq:energy}, energy and mass are related.
```
`\label{}` tags it, `\ref{}` pulls in whatever number it ends up being — even if you reorder equations later, the number stays correct. (The `~` is just a non-breaking space so "Equation" and the number never split across a line.)

---

## 6. Tables

Basic structure — you build a grid with `tabular`, wrapped in a `table` "float" so it can have a caption and number:

```latex
\begin{table}[h]
\centering
\begin{tabular}{|l|c|c|}
\hline
\textbf{Method} & \textbf{Accuracy} & \textbf{Time Complexity} \\
\hline
Rasterization & Medium & $O(n)$ \\
\hline
Ray Tracing & High & $O(n^2)$ \\
\hline
\end{tabular}
\caption{Comparison of Image Synthesis Techniques}
\label{tab:comparison}
\end{table}
```

**Reading this line by line:**
- `{|l|c|c|}` — one letter per column: `l` = left-aligned, `c` = centered, `r` = right-aligned. The `|` characters are vertical lines between columns (optional — remove them for a cleaner, modern look).
- `&` separates columns within a row.
- `\\` ends a row.
- `\hline` draws a horizontal line.
- `[h]` after `\begin{table}` is a placement hint meaning "put it *here*, roughly." (`t` = top of page, `b` = bottom.)
- `\caption{}` + `\label{}` work exactly like figures — see below for `\ref{}` cross-referencing.

### Merged header spanning two columns (e.g. "Score" sitting over both "Theory" and "Lab")

Use `\multicolumn{how many cols}{alignment}{text}` **in place of** those columns for that one row only:

```latex
\begin{tabular}{|l|c|c|}
\hline
\textbf{Module} & \multicolumn{2}{c|}{\textbf{Score}} \\
\cline{2-3}
 & \textbf{Theory} & \textbf{Lab} \\
\hline
Module A & 75 & 80 \\
\hline
\end{tabular}
```
- `\multicolumn{2}{c|}{Score}` merges 2 columns into one cell with "Score" centered.
- `\cline{2-3}` draws a horizontal line under only columns 2–3 (a partial `\hline`), which is what you want under a merged header.

### Merged cell spanning two ROWS (needs the `multirow` package)

```latex
\multirow{2}{*}{Module A} & 75 & 80 \\
 & 78 & 82 \\
```
`\multirow{2}{*}{...}` means "merge 2 rows, auto-width, showing this text vertically centered." The row below it leaves that column blank (just start with `&`).

### Two values stacked inside a single cell (like "88 / 85" shown on two lines in one box)

No package needed — use `\shortstack`:
```latex
\shortstack{88\\85}
```
This puts 88 above 85 inside one table cell.

---

## 7. Figures (images)

**A single figure:**
```latex
\begin{figure}[h]
    \centering
    \includegraphics[width=0.6\textwidth]{filename.png}
    \caption{Pioneers of Modern Calendars}
    \label{fig:pioneers}
\end{figure}
```
- `\includegraphics[width=0.6\textwidth]{...}` — the width is "60% of the text width." Adjust this number to resize the image.
- The file (`filename.png`) needs to be uploaded into your Overleaf project (drag-drop into the file panel).
- **Referencing it in text:** `As shown in Figure~\ref{fig:pioneers}...` — same idea as equation referencing.

### Multiple sub-images side-by-side with their own (a), (b), (c) labels

This is what shows up as "Figure 1: Comparison of three rendering outputs" with sub-captions (a), (b), (c). Needs `\usepackage{subcaption}`:

```latex
\begin{figure}[h]
    \centering
    \begin{subfigure}{0.3\textwidth}
        \centering
        \includegraphics[width=\textwidth]{a.png}
        \caption{Output A}
    \end{subfigure}
    \hfill
    \begin{subfigure}{0.3\textwidth}
        \centering
        \includegraphics[width=\textwidth]{b.png}
        \caption{Output B}
    \end{subfigure}
    \hfill
    \begin{subfigure}{0.3\textwidth}
        \centering
        \includegraphics[width=\textwidth]{c.png}
        \caption{Output C}
    \end{subfigure}
    \caption{Comparison of three rendering outputs.}
    \label{fig:comparison}
\end{figure}
```
- Each `subfigure` block is one small image + its own (a)/(b)/(c) caption (auto-lettered).
- `\hfill` pushes them apart evenly so they spread across the row.
- The outer `\caption{}` (after all three) is the overall "Figure 1: ..." caption.

### Asymmetric layout: one big image on the left, two small ones stacked on the right

This is the trickiest one in your sheets. Do it by making the "right side" its own container holding two stacked subfigures:

```latex
\begin{figure}[h]
    \centering
    \begin{subfigure}{0.45\textwidth}
        \centering
        \includegraphics[width=\textwidth]{main.png}
        \caption{Main Diagram}
    \end{subfigure}
    \hfill
    \begin{subfigure}{0.45\textwidth}
        \centering
        \includegraphics[width=0.9\textwidth]{detailB.png}
        \caption{Detail B}

        \vspace{0.5em}

        \includegraphics[width=0.9\textwidth]{detailC.png}
        \caption{Detail C}
    \end{subfigure}
    \caption{Asymmetric layout with one dominant figure}
\end{figure}
```
The right-hand `subfigure` block simply contains **two** images with two captions stacked vertically inside it, separated by `\vspace{0.5em}` (a small gap). The left block is one wide image. Both blocks are `0.45\textwidth` wide, so together with `\hfill` they fill the page side by side.

---

## 8. Footnotes, References, and Citations

### Footnote (small note at the bottom of the page, auto-numbered)
```latex
Stirling numbers arise in set theory, number theory, and computer science\footnote{\url{https://en.wikipedia.org/wiki/Stirling_numbers_of_the_second_kind}}.
```
This places a tiny superscript number right after "science," and at the bottom of the page prints the numbered footnote text. `\url{}` (comes from `hyperref` or `url` package — you already have `hyperref` loaded) formats a web link properly and makes it clickable.

### Bibliography / References list (simplest method — no extra software needed)

Put this **right before** `\end{document}`:

```latex
\begin{thebibliography}{9}
\bibitem{meeus1991}
Jean Meeus.
\textit{Astronomical Algorithms}.
Willmann-Bell Inc, 1991.
\end{thebibliography}
```
- `{9}` is just a width hint for the numbering column (leave it as `9` unless you have 10+ references, then use `{99}`).
- Each source is one `\bibitem{shortname}` followed by the citation text you type manually.

**Citing it from within your text:**
```latex
A leap year is a calendar year with an extra day\cite{meeus1991}.
```
This automatically prints `[1]` (matching its position in the list) and — because `hyperref` is loaded — makes it a clickable jump-link straight to the reference.

> **For a real research paper** with many sources, you'd normally use BibTeX/biblatex (a separate `.bib` file + `\bibliography{}` command) instead of typing `thebibliography` by hand. For a timed lab with 1–3 references, `thebibliography` above is faster and fully sufficient — don't spend exam time setting up BibTeX unless asked to.

---

## 9. Cross-Referencing Cheat Sheet (ties tables/figures/equations together)

The pattern is always the same, everywhere in LaTeX:
1. Add `\label{prefix:name}` inside the thing you want to reference (figure, table, equation, section).
2. Anywhere else in the doc, write `\ref{prefix:name}` and LaTeX inserts the correct number.

Conventional prefixes (not required, just good practice so you don't mix them up):
- Figures: `fig:...`
- Tables: `tab:...`
- Equations: `eq:...`
- Sections: `sec:...`

---

## 10. A Complete Worked Mini-Example

Here's a short but complete document using almost everything above, so you can see how it all fits together:

```latex
\documentclass{article}
\usepackage[margin=1in]{geometry}
\usepackage{graphicx}
\usepackage{amsmath}
\usepackage{amssymb}
\usepackage[normalem]{ulem}
\usepackage{subcaption}
\usepackage{multirow}
\usepackage{hyperref}

\title{The History Behind Leap Year}
\author{Abid (Your Student ID)}
\date{\today}

\begin{document}
\maketitle

\section{Introduction}
A leap year is a calendar year that contains an additional day compared to a common year\cite{meeus1991}. This article covers the leap year system.

\subsection{Julian Calendar}
Introduced by Julius Caesar in 45 BC, the leap year occurred every four years, since the solar year is approximately \sout{365} 365.25 days long.

\section{Leap Year Rules}
\begin{enumerate}
    \item \textbf{A year is a leap year if:}
    \begin{enumerate}
        \item It is divisible by 4 and not divisible by 100
        \item Or it is divisible by 400.
    \end{enumerate}
    \item \textbf{A year is \emph{NOT} a leap year in all other cases.}
\end{enumerate}

\section{Leap Year Formula}
\begin{equation}
    \text{Leap}(Y) =
    \begin{cases}
        \text{Yes} & \text{if } Y \bmod 400 = 0 \\
        \text{No} & \text{if } Y \bmod 100 = 0 \\
        \text{Yes} & \text{if } Y \bmod 4 = 0 \\
        \text{No} & \text{otherwise}
    \end{cases}
    \label{eq:leap}
\end{equation}

Equation~\ref{eq:leap} is applied to the years in Table~\ref{tab:examples}.

\begin{table}[h]
\centering
\begin{tabular}{|l|c|c|c|c|}
\hline
\textbf{Year} & \textbf{Div. by 4?} & \textbf{Div. by 100?} & \textbf{Div. by 400?} & \textbf{Leap Year?} \\
\hline
1900 & Yes & Yes & No & No \\
\hline
2000 & Yes & Yes & Yes & Yes \\
\hline
\end{tabular}
\caption{Leap Year Calculation Examples}
\label{tab:examples}
\end{table}

\begin{thebibliography}{9}
\bibitem{meeus1991}
Jean Meeus.
\textit{Astronomical Algorithms}.
Willmann-Bell Inc, 1991.
\end{thebibliography}

\end{document}
```

Paste this whole block into a blank Overleaf project and compile it — this alone will show you 80% of what you need for the actual labs.

---

## 11. Quick Troubleshooting (when compile fails under time pressure)

| Error message contains... | Usual cause |
|---|---|
| `Undefined control sequence` | You misspelled a command, or forgot `\usepackage{}` for it |
| `Missing $ inserted` | You used a math symbol (like `_` or `^`) outside `$...$` |
| `Extra }, or forgotten \endgroup` | Mismatched `{ }` braces — count them |
| `Misplaced alignment tab character &` | You used `&` outside a table/align environment |
| Table columns look wrong / merged oddly | Number of `&` in a row doesn't match number of columns declared |
| Image doesn't show up | File not uploaded to the project, or filename/extension typo |

**General debugging tip:** compile often (after every 2–3 changes), not once at the end. That way, when something breaks, you know it's in the last few lines you typed.

---

## 12. One-Page Cheat Sheet

| Feature | Command |
|---|---|
| Bold | `\textbf{...}` |
| Italic | `\textit{...}` or `\emph{...}` |
| Underline | `\underline{...}` |
| Strikethrough | `\sout{...}` (needs `ulem`) |
| Font size | `{\Large ...}`, `{\small ...}`, etc. |
| Colored text | `\textcolor{red}{...}` (needs `xcolor`) |
| Highlighted text | `\colorbox{yellow}{...}` |
| Custom color | `\definecolor{name}{RGB}{r,g,b}` |
| Section | `\section{...}`, `\subsection{...}` |
| Bullet list | `\begin{itemize} \item ... \end{itemize}` |
| Numbered list | `\begin{enumerate} \item ... \end{enumerate}` |
| Inline math | `$...$` |
| Numbered equation | `\begin{equation} ... \end{equation}` |
| Unnumbered equation | `\[ ... \]` |
| Multi-line aligned equations | `\begin{align} a &= b \\ c &= d \end{align}` |
| Piecewise formula | `\begin{cases} ... \end{cases}` |
| Subscript / superscript | `x_{i}` / `x^{2}` |
| Fraction | `\frac{a}{b}` |
| Table | `\begin{table}\begin{tabular}{|l|c|}...\end{tabular}\caption{}\end{table}` |
| Merge table columns | `\multicolumn{2}{c|}{text}` |
| Merge table rows | `\multirow{2}{*}{text}` (needs `multirow`) |
| Two lines in one cell | `\shortstack{line1\\line2}` |
| Figure | `\begin{figure}\includegraphics{...}\caption{}\label{}\end{figure}` |
| Sub-figures (a)(b)(c) | `\begin{subfigure}{width}...\end{subfigure}` (needs `subcaption`) |
| Footnote | `\footnote{...}` |
| Clickable URL | `\url{...}` |
| Reference list entry | `\bibitem{key} ...` inside `thebibliography` |
| Cite a reference | `\cite{key}` |
| Label something | `\label{tag}` |
| Refer to a label | `\ref{tag}` |

---

## 13. A Few Extras Worth Knowing for Real Research Papers

These aren't in your lab sheets, but you'll want them the moment you write an actual paper:

- **Two-column layout** (common in conference papers): `\documentclass[twocolumn]{article}` — that one word changes the whole page layout.
- **Abstract:** right after `\maketitle`, use:
  ```latex
  \begin{abstract}
  A short summary of the paper goes here.
  \end{abstract}
  ```
- **Table of contents:** just write `\tableofcontents` after `\maketitle` — it auto-generates from your `\section`/`\subsection` commands. Requires compiling twice to update numbers correctly (Overleaf does this automatically).
- **Page numbers / headers:** on by default in `article` class; customizable with the `fancyhdr` package if a template demands a specific look.
- **BibTeX-style references** (for papers with 10+ citations): instead of typing `thebibliography` by hand, you create a `references.bib` file with entries, then write `\bibliographystyle{plain}` and `\bibliography{references}`. Overleaf has a "New File" → `.bib` option, and can even auto-import citations by pasting a DOI.
- **Algorithm blocks** (pseudocode, common in CS papers): `\usepackage{algorithm}` + `\usepackage{algpseudocode}`.
- **Code listings:** `\usepackage{listings}` lets you paste source code with syntax highlighting via `\begin{lstlisting}...\end{lstlisting}`.

You won't need any of section 13 for these timed labs — it's here so you're not caught off guard the first time a paper template asks for it.
