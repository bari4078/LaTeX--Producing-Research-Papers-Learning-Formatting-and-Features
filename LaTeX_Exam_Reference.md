# LaTeX Reference Guide for Timed Paper-Reproduction Assessments

> Built for exams like "Reproduce this article exactly using LaTeX" (50 minutes, ~50 marks).
> Covers every syntax topic that shows up across the sample papers: document structure, TOC,
> two-column layout, text formatting, lists, equations/matrices, tables, figures, algorithms,
> theorems, cross-references, footnotes, hyperlinks, citations, and code listings.

**How to use this doc:** Skim the Quick Reference table first so you know *which section to jump to*
under time pressure. Each topic section below has: a plain-English explanation, the LaTeX code,
and what it produces. Section 0 is a copy-paste skeleton — start every exam by pasting that into
Overleaf, then delete what you don't need.

---

## Table of Contents

0. [Copy-Paste Overleaf Skeleton](#0-copy-paste-overleaf-skeleton)
1. [Quick Reference Table](#1-quick-reference-table)
2. [Document Structure & Title Block](#2-document-structure--title-block)
3. [Table of Contents](#3-table-of-contents)
4. [Two-Column Layout](#4-two-column-layout)
5. [Text Formatting, Colors & Emphasis](#5-text-formatting-colors--emphasis)
6. [Lists & Nested Lists](#6-lists--nested-lists)
7. [Equations, Alignment & Matrices](#7-equations-alignment--matrices)
8. [Tables (Multirow, Multicolumn, Colored)](#8-tables-multirow-multicolumn-colored)
9. [Figures (Single, Subfigures, Wrapped, Rotated)](#9-figures-single-subfigures-wrapped-rotated)
10. [Algorithms / Pseudocode](#10-algorithms--pseudocode)
11. [Theorem, Proposition & Proof Environments](#11-theorem-proposition--proof-environments)
12. [Cross-References](#12-cross-references)
13. [Footnotes & Hyperlinks](#13-footnotes--hyperlinks)
14. [Citations & Bibliography](#14-citations--bibliography)
15. [Code Listings](#15-code-listings)
16. [Exam-Day Strategy Checklist](#16-exam-day-strategy-checklist)

---

## 0. Copy-Paste Overleaf Skeleton

Paste this as your starting `main.tex` on Overleaf. It loads every package you're likely to need,
so if a command "doesn't work" mid-exam, it's probably a typo, not a missing package. Delete
unused packages at the end if you want a cleaner file (not required for marks).

```latex
\documentclass[11pt]{article}

% ---------- Page & layout ----------
\usepackage[margin=1in]{geometry}
\usepackage{multicol}          % two-column layout for parts of the doc

% ---------- Math ----------
\usepackage{amsmath}           % align, cases, matrices
\usepackage{amssymb}           % extra math symbols
\usepackage{amsthm}            % theorem/proof environments
\usepackage{mathtools}         % extra matrix/align helpers

% ---------- Text formatting & color ----------
\usepackage{xcolor}            % text color, highlight color
\usepackage{soul}              % \hl{} highlighting
\usepackage{ulem}              % \uline{} underline (normalem avoids strikethrough clash)
\normalem

% ---------- Lists ----------
\usepackage{enumitem}          % customize itemize/enumerate labels & spacing

% ---------- Tables ----------
\usepackage{booktabs}          % \toprule \midrule \bottomrule
\usepackage{multirow}          % merge table cells vertically
\usepackage{array}             % column formatting helpers
\usepackage{colortbl}          % colored table cells/rows

% ---------- Figures ----------
\usepackage{graphicx}          % \includegraphics
\usepackage{subcaption}        % subfigures (1a, 1b, ...)
\usepackage{wrapfig}           % text-wrapped figures
\usepackage{rotating}          % rotated figures/tables
\usepackage{float}             % [H] exact placement

% ---------- Algorithms ----------
\usepackage{algorithm}         % algorithm float + caption
\usepackage{algpseudocode}     % \State, \For, \If, etc.

% ---------- Code listings ----------
\usepackage{listings}
\usepackage{xcolor}

% ---------- Cross-refs, footnotes, links ----------
\usepackage{hyperref}          % \href, \url, clickable \ref (load LAST, almost)
\usepackage[capitalise]{cleveref}  % \cref{} — smarter \ref, load AFTER hyperref

% ---------- Citations ----------
\usepackage[numbers]{natbib}   % numeric [1] style citations; or use biblatex (see Sec. 14)

% ---------- Python listing style (used by "Code Listings" section) ----------
\lstdefinestyle{python}{
    language=Python,
    basicstyle=\ttfamily\small,
    keywordstyle=\color{blue},
    commentstyle=\color{gray},
    stringstyle=\color{purple},
    numbers=left,
    numberstyle=\tiny,
    frame=single,
    breaklines=true
}

\title{Your Article Title Here}
\author{Author One$^{1}$ \and Author Two$^{2}$}
\date{}

\begin{document}

\maketitle

% \tableofcontents   % uncomment if the paper has a Contents page

\begin{abstract}
Optional abstract text.
\end{abstract}

\section{First Section}
Body text goes here.

% ---------- Bibliography (pick ONE method — see Section 14) ----------
\bibliographystyle{plain}
\bibliography{references}

\end{document}
```

**Note:** `soul`'s `\hl{}` and `ulem`'s underline can conflict; the `\normalem` line above fixes
that. If Overleaf throws a package-clash error, comment out the offending package — you rarely
need all of them in one paper.

---

## 1. Quick Reference Table

| Topic | Key commands |
|---|---|
| Title block | `\title{}`, `\author{}`, `\date{}`, `\maketitle` |
| Table of contents | `\tableofcontents` |
| Two columns | `\documentclass[twocolumn]{article}` or `multicols` |
| Bold / Italic / Underline | `\textbf{}`, `\textit{}` / `\emph{}`, `\uline{}` |
| Highlight | `\hl{}` (soul) or `\colorbox{}` |
| Color text | `\textcolor{color}{text}` |
| Lists | `itemize`, `enumerate`, `description` |
| Equation (numbered) | `equation` |
| Multi-line aligned eqns | `align` |
| Matrix | `pmatrix`, `bmatrix`, `vmatrix` |
| Piecewise function | `cases` |
| Table | `tabular` inside `table` |
| Merge table rows | `\multirow{n}{*}{...}` |
| Merge table cols | `\multicolumn{n}{c}{...}` |
| Colored table cell | `\cellcolor{color}` |
| Figure | `figure`, `\includegraphics` |
| Subfigures | `subfigure` (subcaption pkg) |
| Wrapped figure | `wrapfigure` |
| Rotated figure | `sidewaysfigure` or `\rotatebox{}` |
| Pseudocode | `algorithm` + `algorithmic`/`algpseudocode` |
| Theorem/Proof | `\newtheorem{}`, `proof` |
| Cross-reference | `\label{}`, `\ref{}`, `\eqref{}`, `\cref{}` |
| Footnote | `\footnote{}` |
| Hyperlink | `\href{url}{text}`, `\url{}` |
| Citation | `\cite{}` |
| Bibliography | `thebibliography` or BibTeX/`natbib`/`biblatex` |
| Code listing | `lstlisting` |

---

## 2. Document Structure & Title Block

Every article starts the same way: document class, title info, then `\maketitle`.

```latex
\documentclass{article}

\title{The Riemann Hypothesis}
\author{Count Binface\thanks{Leader of Recyclon, Sigma IX \& Master of \LaTeX}}
\date{}  % leave blank to hide the date, or omit \date entirely to show today's date

\begin{document}
\maketitle
\end{document}
```

**Multiple authors with affiliations** (like the Laplace paper — three authors, each with a
numbered footnote-style affiliation):

```latex
\author{
  Pierre-Simon Laplace\textsuperscript{1} \and
  Oliver Heaviside\textsuperscript{2} \and
  Gustav Robert Kirchhoff\textsuperscript{3}
}
\date{
  \textsuperscript{1}Acad\'emie des Sciences, Paris, France \\
  \textsuperscript{2}Royal Society, London, United Kingdom \\
  \textsuperscript{3}University of Heidelberg, Heidelberg, Germany
}
```
Putting affiliations in `\date{}` is a common trick to place them under the author line without
extra packages. (If the exam paper needs a fancier author block, the `authblk` package gives
`\author{}` + `\affil{}` pairs — mention it if you have time to look it up.)

**Abstract block:**
```latex
\begin{abstract}
This short paragraph summarizes the paper.
\end{abstract}
```

---

## 3. Table of Contents

```latex
\tableofcontents
\newpage   % optional, pushes content to next page
```
This auto-generates from your `\section`, `\subsection`, `\subsubsection` commands — you do not
type entries by hand. Just make sure your sectioning commands are correct and it "just works."

```latex
\section{A Function Built from Integers}
\subsection{The Riemann Zeta Function}
\subsection{A Classical Special Value}
\section{Zeros, Symmetry, and the Critical Line}
```
Compile **twice** (Overleaf usually does this automatically) — the first pass writes the `.toc`
file, the second pass reads it to actually print the contents list.

---

## 4. Two-Column Layout

**Whole document in two columns** (do this in `\documentclass`):
```latex
\documentclass[twocolumn]{article}
```

**Only part of the document in two columns** (e.g., just one section), use `multicol`:
```latex
\usepackage{multicol}
...
\begin{multicols}{2}
Your two-column text goes here. It flows automatically between the columns.
\end{multicols}
```

**Full-width element inside a two-column document** (e.g. a wide table/figure spanning both
columns): use the starred float environment:
```latex
\begin{table*}[t]
  ...
\end{table*}

\begin{figure*}[t]
  ...
\end{figure*}
```

---

## 5. Text Formatting, Colors & Emphasis

```latex
\textbf{bold text}
\textit{italic text}
\emph{emphasized text}          % italicizes, but flips inside italics
\uline{underlined text}         % needs \usepackage{ulem} + \normalem
\texttt{monospace / code text}
\textsuperscript{superscript}
\textsubscript{subscript}
```

**Colored text:**
```latex
\usepackage{xcolor}
\textcolor{red}{this text is red}
\textcolor{blue}{this text is blue}
```

**Highlighted text** (like a text marker/highlighter over words):
```latex
\usepackage{soul}
\hl{this text is highlighted}
```
Alternative without `soul`:
```latex
\colorbox{yellow}{this text has a yellow background}
```

**Small caps / block quote style attribution** (seen under a paper subtitle):
```latex
\begin{quote}
\textit{Leader of Recyclon, Sigma IX \& Master of \LaTeX}
\end{quote}
```

---

## 6. Lists & Nested Lists

**Bulleted list:**
```latex
\begin{itemize}
  \item First point
  \item Second point
\end{itemize}
```

**Numbered list:**
```latex
\begin{enumerate}
  \item First step
  \item Second step
\end{enumerate}
```

**Nested lists with custom bullet symbols** (very common in these exams — e.g. "∤", "∋", "⋄"):
```latex
\usepackage{enumitem}
\usepackage{amssymb}   % for symbols like \nmid, \ni, \diamond

\begin{itemize}[label=$\nmid$]
  \item They occur at negative even integers.
  \item Their locations are completely understood.
\end{itemize}

\begin{itemize}[label=$\ni$]
  \item They lie inside the critical strip.
  \item They occur in symmetric families.
  \begin{enumerate}[label=(\roman*)]
    \item $\rho$;
    \item $1-\rho$;
    \item $\overline{\rho}$;
    \item $1-\overline{\rho}$.
  \end{enumerate}
\end{itemize}
```
`enumitem`'s `[label=...]` option is the key tool for matching unusual bullet characters or
numbering styles (roman numerals, letters, custom symbols) exactly as printed in the source paper.

**Description list** (term + definition, e.g. "Internal clock / Coupling strength / ..."):
```latex
\begin{description}
  \item[Internal clock] $\omega_i$ is the rate the firefly would follow alone.
  \item[Coupling strength] $K \geq 0$ measures how strongly flashes influence one another.
\end{description}
```

**Labeled sub-headings acting like a mini list** (e.g. "(A) Trivial zeros" / "(B) Nontrivial
zeros" each followed by their own bullet list) — just use a `\paragraph` or bold run-in heading
before each `itemize` block:
```latex
\paragraph{(A) Trivial zeros}
\begin{itemize}[label=$\nmid$]
  \item They occur at negative even integers.
\end{itemize}
```

---

## 7. Equations, Alignment & Matrices

**Single numbered equation:**
```latex
\begin{equation}
  \zeta(s) = \sum_{n=1}^{\infty} \frac{1}{n^s}, \qquad \sigma > 1
  \label{eq:zeta}
\end{equation}
```

**Unnumbered display equation:**
```latex
\[
  \zeta(2) = \frac{\pi^2}{6}
\]
```

**Multi-line aligned equations** (align at the `=` sign using `&`):
```latex
\begin{align}
  \zeta(s) &= \prod_{p \text{ prime}} \frac{1}{1 - p^{-s}} \\
  \zeta(2) &= \frac{\pi^2}{6}
\end{align}
```
Use `align*` instead of `align` to suppress equation numbers.

**Piecewise / cases (e.g. trivial vs nontrivial zeros definitions):**
```latex
\[
  \zeta(s) = 0 \iff
  \begin{cases}
    s = -2, -4, -6, \dots & \text{(trivial zeros)} \\
    0 < \operatorname{Re}(s) < 1 & \text{(nontrivial zeros, critical strip)}
  \end{cases}
\]
```

**Matrices:**
```latex
% Parentheses matrix
\[
  R = \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}
\]

% Square-bracket matrix
\[
  Z_2 = \begin{bmatrix} \tfrac{1}{2} & 14.134725 \\ \tfrac{1}{2} & 21.022040 \end{bmatrix}
\]

% Determinant-style (vertical bars)
\[
  \det \begin{vmatrix} a & b \\ c & d \end{vmatrix} = ad - bc
\]
```

**Fractions, sums, products, limits, integrals:**
```latex
\frac{a}{b}
\sum_{n=1}^{\infty} a_n
\prod_{p} f(p)
\lim_{x \to \infty} f(x)
\int_0^\infty f(t)\,e^{-st}\,dt
```

**Common symbols cheat-list:**
```latex
\alpha \beta \gamma \theta \omega \pi \sigma \rho \zeta
\leq \geq \neq \approx \in \notin \subset \forall \exists
\rightarrow \Rightarrow \iff \infty \partial \nabla
\overline{x}   % bar over a symbol (complex conjugate)
\hat{x}        % hat accent
```

**Bit-vector / big Sigma over an index set (e.g. Kuramoto model coupling term):**
```latex
\[
  \dot\theta_i = \omega_i + \frac{K}{N}\sum_{j=1}^{N} \sin(\theta_j - \theta_i)
\]
```

---

## 8. Tables (Multirow, Multicolumn, Colored)

**Basic table with `booktabs` rules (professional look, no vertical lines):**
```latex
\usepackage{booktabs}

\begin{table}[h]
  \centering
  \caption{The first four positive ordinates of nontrivial zeros.}
  \label{tab:zeros}
  \begin{tabular}{c c c}
    \toprule
    \textbf{Index} & \textbf{Real Part} & \textbf{Imaginary Part} \\
    \midrule
    1 & 14.134725 & --- \\
    2 & 21.022040 & --- \\
    \bottomrule
  \end{tabular}
\end{table}
```

**Multirow (merging cells vertically) and multicolumn (merging cells horizontally)** — this is
the most-tested table skill. Example reproducing a header like "Orbital parameters" spanning two
columns, over a table with a merged "Phase / Stage" structure:
```latex
\usepackage{multirow}

\begin{table}[h]
\centering
\caption{Where DNA-storage errors arise and how systems respond.}
\begin{tabular}{l l l l l}
  \toprule
  \multicolumn{2}{c}{\textbf{Stage}} & \textbf{Problem} & \textbf{Effect} & \textbf{Response} \\
  \midrule
  \multirow{2}{*}{\textbf{Writing}} & Encoding  & Repeats/extreme GC & Poor synthesis & Choose another sequence \\
                                     & Synthesis & Base errors        & Incorrect strand & Add checks and copies \\
  \midrule
  \textbf{Preservation} & Storage & Damage or loss & Missing fragments & Protect DNA, keep copies \\
  \bottomrule
\end{tabular}
\end{table}
```
- `\multirow{2}{*}{text}` merges **2 rows**, with the cell width set to `*` (natural width).
- `\multicolumn{2}{c}{text}` merges **2 columns**, centered (`c`); use `l`/`r` for left/right.

**Colored table cells / rows:**
```latex
\usepackage{colortbl}   % or xcolor with the [table] option: \usepackage[table]{xcolor}

\begin{tabular}{l c c}
  \rowcolor{gray!20} \textbf{Planet} & \textbf{a (AU)} & \textbf{e} \\
  Jupiter & 5.204 & 0.0489 \\
  \cellcolor{blue!10} Saturn & 9.583 & 0.0565 \\
\end{tabular}
```
`\rowcolor{color!X}` colors the whole row; `\cellcolor{}` colors a single cell. `gray!20` means
"20% gray, 80% white" — adjust the percentage to match shading intensity.

---

## 9. Figures (Single, Subfigures, Wrapped, Rotated)

**Basic figure with caption & label:**
```latex
\usepackage{graphicx}

\begin{figure}[h]
  \centering
  \includegraphics[width=0.8\linewidth]{filename.png}
  \caption{Firefly light trails in a garden.}
  \label{fig:fireflies}
\end{figure}
```

**Subfigures (e.g. a 3x3 grid of planet portraits, "Figure 1e", "Figure 1f"):**
```latex
\usepackage{subcaption}

\begin{figure}[h]
  \centering
  \begin{subfigure}{0.3\linewidth}
    \includegraphics[width=\linewidth]{mercury.png}
    \caption{Mercury}
  \end{subfigure}
  \hfill
  \begin{subfigure}{0.3\linewidth}
    \includegraphics[width=\linewidth]{venus.png}
    \caption{Venus}
  \end{subfigure}
  \hfill
  \begin{subfigure}{0.3\linewidth}
    \includegraphics[width=\linewidth]{earth.png}
    \caption{Earth}
  \end{subfigure}
  % repeat rows as needed; \\ to start a new row of subfigures
  \caption{The Sun and eight planets.}
  \label{fig:planets}
\end{figure}
```
Reference an individual subfigure with `\ref{fig:planets}e` or `\subref{}` if you've labeled
each subfigure individually — check how the source paper phrases it (e.g. "Figure 1e").

**Wrapped figure (text flows around a small image):**
```latex
\usepackage{wrapfig}

\begin{wrapfigure}{r}{0.35\linewidth}   % r = right side; use l for left
  \centering
  \includegraphics[width=\linewidth]{circuit.png}
  \caption{RC circuit diagram.}
  \label{fig:circuit}
\end{wrapfigure}
Text here will wrap around the figure automatically.
```

**Rotated figure (e.g. a wide landscape image on a portrait page):**
```latex
\usepackage{rotating}

\begin{sidewaysfigure}
  \centering
  \includegraphics[width=0.9\linewidth]{wide-diagram.png}
  \caption{A wide diagram, rotated 90 degrees.}
  \label{fig:wide}
\end{sidewaysfigure}
```
Or rotate just the image (keeping caption upright):
```latex
\includegraphics[angle=90, width=0.6\linewidth]{diagram.png}
```

**Placeholder images while you don't have the real file:** Overleaf supports blank boxes via
`\rule{width}{height}` if you don't want to hunt for/upload an image under time pressure:
```latex
\rule{6cm}{4cm}   % draws a solid black box as a placeholder
```
(Check with your instructor whether this is acceptable — usually fine since "exact float
placement is not required.")

---

## 10. Algorithms / Pseudocode

```latex
\usepackage{algorithm}
\usepackage{algpseudocode}

\begin{algorithm}
\caption{Check candidate zeros against the critical line}
\begin{algorithmic}[1]
  \Require Candidate zeros $Z = (\rho_1, \rho_2, \dots, \rho_m)$ and tolerance $\varepsilon > 0$
  \Ensure True if every candidate satisfies $\left|\operatorname{Re}(\rho_i) - \tfrac{1}{2}\right| \le \varepsilon$
  \State $allOnLine \gets \text{True}$
  \For{$i \gets 1$ to $m$}
    \If{$\left|\operatorname{Re}(\rho_i) - \tfrac{1}{2}\right| > \varepsilon$}
      \State $allOnLine \gets \text{False}$
      \State \textbf{break}
    \EndIf
  \EndFor
  \State \Return $allOnLine$
\end{algorithmic}
\end{algorithm}
```
Key building blocks:
- `[1]` after `\begin{algorithmic}` turns on line numbering.
- `\Require` / `\Ensure` — preconditions/postconditions (needs `algpseudocode`, not the older
  `algorithmic` alone — if `\Require` errors out, add `\usepackage{algpseudocode}`).
- `\State` — a plain line of pseudocode.
- `\For{...}...\EndFor`, `\If{...}...\Else...\EndIf`, `\While{...}...\EndWhile` — control blocks.
- `\Return` — return statement.

**Complexity / Big-O expressions**, usually just inline or display math, not a special package:
```latex
\[
  T(m) = \Theta(m).
\]
```
Common complexity symbols: `\Theta{}`, `\mathcal{O}(n)`, `\Omega(n)`.

---

## 11. Theorem, Proposition & Proof Environments

Define the environment once in the preamble, then use it in the body.

```latex
% --- in the preamble ---
\usepackage{amsthm}
\newtheorem{theorem}{Theorem}
\newtheorem{proposition}{Proposition}[section]   % numbers as 3.1, 3.2... reset per section
```

```latex
% --- in the body ---
\begin{proposition}[Weak-field circular orbit]
For a circular orbit of radius $r$,
\[
  \frac{v^2}{c^2} = \frac{r_s}{2r}.
\]
\end{proposition}

\begin{proof}
The circular-orbit balance $v^2/r = GM/r^2$ gives $v^2 = GM/r$. Divide by $c^2$ and use
$r_s = 2GM/c^2$.
\end{proof}
```
- The text in `[...]` after `\begin{proposition}` becomes the parenthetical title, e.g.
  "**Proposition 3.1** (Weak-field circular orbit)."
- `proof` is built into `amsthm` already — it auto-adds a *Proof.* label and a $\blacksquare$
  (QED box) at the end. No need to `\newtheorem` it yourself.
- To reference it later: `\label{prop:orbit}` inside the environment, then `Proposition~\ref{prop:orbit}`.

---

## 12. Cross-References

Every reference workflow is the same two-step pattern: **label it, then refer to it.**

```latex
\section{Zeros, Symmetry, and the Critical Line}
\label{sec:zeros}

\begin{equation}
  \zeta(2) = \frac{\pi^2}{6}
  \label{eq:zeta2}
\end{equation}

\begin{figure}[h]
  ...
  \label{fig:planets}
\end{figure}

\begin{table}[h]
  ...
  \label{tab:zeros}
\end{table}
```

Then anywhere later in the document:
```latex
See Section~\ref{sec:zeros}.
Equation~\eqref{eq:zeta2} gives the classical identity.   % \eqref adds parentheses: (1)
Figure~\ref{fig:planets} shows the Sun and eight planets.
Table~\ref{tab:zeros} lists the zeros.
```
- Use `\ref{}` for sections/figures/tables (prints just the number).
- Use `\eqref{}` for equations (prints the number **in parentheses**, matches how papers usually
  cite equations — "Equation (1)").
- A non-breaking space `~` before `\ref` prevents the number from being orphaned on the next line.
- **Optional but handy:** `\usepackage[capitalise]{cleveref}` lets you just write `\cref{eq:zeta2}`
  and it automatically inserts the right word ("Equation", "Figure", "Table") — saves typing
  during a timed exam, but plain `\ref`/`\eqref` is perfectly fine and safer if you're unsure
  cleveref is loaded correctly.

---

## 13. Footnotes & Hyperlinks

**Footnote:**
```latex
The Riemann Hypothesis is a Millennium Prize Problem.\footnote{Awarded by the Clay Mathematics Institute.}
```
Numbering is automatic and resets per page by default — you don't manage the number yourself.

**Hyperlink to an external URL:**
```latex
\usepackage{hyperref}

\href{https://www.claymath.org/millennium-problems}{Clay Mathematics Institute}
```
This makes the visible text "Clay Mathematics Institute" clickable, linking to the URL.

**Bare URL displayed as text:**
```latex
\url{https://www.claymath.org}
```

**Combining a highlighted phrase with a hyperlink** (seen in the sample papers, e.g. "See the
Clay Mathematics Institute." shown highlighted and linked):
```latex
\href{https://www.claymath.org}{\hl{Clay Mathematics Institute}}
```

---

## 14. Citations & Bibliography

Two common approaches — pick whichever the exam allows / whichever is faster for you. Both are
valid; **`thebibliography` is faster to set up under time pressure** since it needs no external
file or extra compile step.

### Option A — Manual bibliography (fastest, no BibTeX needed)

```latex
% in the body, usually at the end
\begin{thebibliography}{9}

\bibitem{sarfati2021}
Rapha\"el Sarfati, Julie C. Hayes, and Orit Peleg.
``Self-Organization in Natural Swarms of Photinus carolinus Synchronous Fireflies''.
\textit{Science Advances} 7.28 (2021), eabg9259.
doi: \url{10.1126/sciadv.abg9259}.

\bibitem{mirollo1990}
Renato E. Mirollo and Steven H. Strogatz.
``Synchronization of Pulse-Coupled Biological Oscillators''.
\textit{SIAM Journal on Applied Mathematics} 50.6 (1990), pp. 1645--1662.

\end{thebibliography}
```
Cite anywhere in the text with:
```latex
A field study is available through the study by Sarfati, Hayes, and Peleg~\cite{sarfati2021}.
```
This prints as `[1]` (numbered automatically by order of first citation, IEEE/plain style) or
by whatever `\bibitem` key order you list them in.

### Option B — BibTeX / natbib (if the exam wants a `.bib` file workflow)

```latex
% preamble
\usepackage[numbers]{natbib}
```
```latex
% references.bib (separate file in Overleaf)
@article{sarfati2021,
  author  = {Sarfati, Rapha{\"e}l and Hayes, Julie C. and Peleg, Orit},
  title   = {Self-Organization in Natural Swarms of Photinus carolinus Synchronous Fireflies},
  journal = {Science Advances},
  volume  = {7},
  number  = {28},
  pages   = {eabg9259},
  year    = {2021}
}
```
```latex
% in the body
\citep{sarfati2021}     % (Author, Year) style
\citet{sarfati2021}     % Author (Year) style

% at the end of the document
\bibliographystyle{plain}
\bibliography{references}
```
Note: with BibTeX you must compile with the "BibTeX" step (Overleaf does this automatically when
it detects `\bibliography{}`), and you may need to compile **twice** for citation numbers to
resolve. Under 50-minute time pressure, **Option A is usually the safer choice.**

---

## 15. Code Listings

For reproducing source-code blocks (e.g. a Python function) with syntax highlighting:

```latex
\usepackage{listings}
\usepackage{xcolor}

\lstdefinestyle{python}{
  language=Python,
  basicstyle=\ttfamily\small,
  keywordstyle=\color{blue},
  commentstyle=\color{gray},
  stringstyle=\color{purple},
  numbers=left,
  numberstyle=\tiny,
  frame=single,
  breaklines=true
}

\begin{lstlisting}[style=python, caption={Computing samples of the RC step response.}, label={lst:rc}]
from math import exp

def rc_response(resistance, capacitance, input_voltage, times):
    """Return capacitor-voltage samples for an RC step input."""
    if resistance <= 0 or capacitance <= 0:
        raise ValueError("R and C must be positive")

    tau = resistance * capacitance
    values = []
    for time in times:
        voltage = input_voltage * (1 - exp(-time / tau))
        values.append(voltage)
    return values
\end{lstlisting}
```
Reference it the same way as a figure/table: `Listing~\ref{lst:rc}`.

**Inline monospaced/code text** (e.g. `00` → `A` mappings, short code snippets in running text):
```latex
The binary pair \texttt{00} maps to the DNA base \texttt{A}.
```

**Preformatted block without syntax highlighting** (e.g. raw sequencing reads):
```latex
\begin{verbatim}
Read1: ACGTAC
Read2: ACGTAC
Read3: ACCTAC
\end{verbatim}
```

---

## 16. Exam-Day Strategy Checklist

Given the mark distributions across these papers, budget your 50 minutes roughly like this:

1. **First 2 minutes:** Paste the Section 0 skeleton. Set `\title`, `\author`, sections skeleton
   matching the paper's headings (this alone locks in Document Structure + TOC marks cheaply).
2. **Work top to bottom, matching the paper's own order** — don't jump around hunting for the
   "hard" equation first. Partial credit is per-topic, not per-perfection.
2. **Equations are usually worth the most marks (8–12)** — get every equation compiling even if
   spacing/exact symbols aren't pixel-perfect; a compiling near-match beats a broken exact match.
3. **Tables with multirow/multicolumn are the next biggest** (6–8) — sketch the grid on scratch
   paper first (rows × cols, which cells merge) before writing `tabular` code.
4. **Figures:** don't waste time hunting for the exact source image — a placeholder
   (`\rule{}{}`) with the correct caption/label/cross-reference still earns most of the figure
   marks, since exact float placement is explicitly not required.
5. **Do NOT skip:** footnote, hyperlink, one citation + bibliography, and at least one
   cross-reference — these are only worth 1–3 marks each individually, but they add up to 8–10
   "easy" marks that are quick to bang out once you know the syntax (Sections 12–14 above).
6. **Compile often** (every 3–5 minutes). A document that compiles with minor visual differences
   always beats a document that doesn't compile at all — "the document must compile" is
   explicitly graded.
7. **Last 3 minutes:** compile one final time, check the PDF renders, and confirm the
   bibliography/TOC numbers actually appeared (these need a second compile pass to show up).
