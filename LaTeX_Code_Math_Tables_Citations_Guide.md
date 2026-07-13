# LaTeX Guide Part 2: Code, Pseudocode, Math Symbols, Equations, Citations & Tables

This is the "going deeper" guide — for when a lab or paper wants real code snippets, algorithm pseudocode, denser math, proper citations, or professional-looking tables. Same rule as before: every command shown with what it does, how to type it, what comes out.

**Add these packages to your preamble for everything in this guide:**
```latex
\usepackage{listings}       % code blocks
\usepackage{xcolor}         % colors (needed by listings styling)
\usepackage{algorithm}      % pseudocode "float" wrapper (Algorithm 1, 2, ...)
\usepackage{algpseudocode}  % pseudocode commands (If, For, While, ...)
\usepackage{amsmath}        % math tools (align, cases, matrices...)
\usepackage{amssymb}        % extra math symbols
\usepackage{mathtools}      % small fixes/extras on top of amsmath
\usepackage{booktabs}       % professional table lines
\usepackage{tabularx}       % auto-width table columns
\usepackage{longtable}      % tables that span multiple pages
\usepackage[numbers]{natbib}% author-year or numeric citations
```

---

## PART A — Showing Code

### Inline code (a variable or command name inside a sentence)

```latex
The variable \texttt{count} is initialized to zero.
```
Output: The variable `count` is initialized to zero. (monospace font, no highlighting)

Quick alternative for one-off inline code with special characters (like `_` or `\` that would otherwise confuse LaTeX): `\verb|your_code_here|` — whatever symbol you put right after `\verb` (here `|`) becomes the closing symbol too, so pick one that isn't inside your code.

### A block of code, no highlighting (quick and simple)

```latex
\begin{verbatim}
for i in range(10):
    print(i)
\end{verbatim}
```
This prints the text **exactly as typed**, monospaced, no coloring. Good enough when you just need to show code fast and formatting isn't graded.

### A block of code, WITH syntax highlighting (what you want for a polished lab/report)

Needs `\usepackage{listings}` and `\usepackage{xcolor}` (both already in the preamble block above).

**Step 1 — set up a style once, near the top of your document** (copy-paste this, you don't need to understand every line):
```latex
\lstset{
    basicstyle=\ttfamily\small,
    keywordstyle=\color{blue},
    commentstyle=\color{gray},
    stringstyle=\color{red},
    numbers=left,
    numberstyle=\tiny,
    frame=single,
    breaklines=true,
    showstringspaces=false
}
```
This says: monospace font, keywords in blue, comments in gray, strings in red, line numbers on the left, a box frame around the code, and wrap long lines instead of overflowing the page.

**Step 2 — use it anywhere in your document:**
```latex
\begin{lstlisting}[language=Python]
def factorial(n):
    if n == 0:
        return 1
    return n * factorial(n - 1)
\end{lstlisting}
```
Change `language=Python` to `language=C`, `language=Java`, `language=C++`, etc. depending on what you're showing.

**Loading code straight from a file** instead of retyping it (useful if you already have a `.py`/`.cpp` file uploaded to your project):
```latex
\lstinputlisting[language=Python]{mycode.py}
```

**Giving the code block a caption/number** (wrap it like a figure):
```latex
\begin{lstlisting}[language=Python, caption={Factorial function}, label={lst:fact}]
def factorial(n):
    return 1 if n == 0 else n * factorial(n - 1)
\end{lstlisting}
```
Refer to it later the same way as figures/tables: `See Listing~\ref{lst:fact}.`

> If you ever see a template asking for the `minted` package instead of `listings` — it looks even nicer (real Pygments highlighting) but requires enabling `-shell-escape` compilation mode. On Overleaf: Menu → Settings → change compiler settings. For a timed lab, stick with `listings` — no setup needed.

---

## PART B — Pseudocode (Algorithms)

This is different from code — it's the "Algorithm 1: Binary Search" style block with `if/else`, `for`, `while`, `return`, indentation, and a line-numbered box, commonly required in CS courses.

Needs both `\usepackage{algorithm}` (the numbered box wrapper, works like `figure`/`table`) and `\usepackage{algpseudocode}` (the actual `If`, `For`, `While` commands).

**Structure:**
```latex
\begin{algorithm}
\caption{Binary Search}
\label{alg:binary-search}
\begin{algorithmic}[1]
\Function{BinarySearch}{$A$, $target$}
    \State $low \gets 0$
    \State $high \gets \text{length}(A) - 1$
    \While{$low \leq high$}
        \State $mid \gets \lfloor (low + high) / 2 \rfloor$
        \If{$A[mid] = target$}
            \State \Return $mid$
        \ElsIf{$A[mid] < target$}
            \State $low \gets mid + 1$
        \Else
            \State $high \gets mid - 1$
        \EndIf
    \EndWhile
    \State \Return $-1$
\EndFunction
\end{algorithmic}
\end{algorithm}
```

**What each piece means:**

| Command | Meaning |
|---|---|
| `\begin{algorithm}...\end{algorithm}` | The numbered box wrapper (gives you "Algorithm 1") |
| `[1]` after `algorithmic` | Turn on line numbers, starting at 1 |
| `\caption{}` | Title shown next to "Algorithm 1:" |
| `\Function{Name}{args} ... \EndFunction` | Defines a function/procedure |
| `\State` | One plain line of pseudocode |
| `\gets` | The ← assignment arrow (renders as `low ← 0`) |
| `\If{cond} ... \ElsIf{cond} ... \Else ... \EndIf` | If/else-if/else block — every `\If` needs a matching `\EndIf` |
| `\While{cond} ... \EndWhile` | While loop |
| `\For{cond} ... \EndFor` | For loop |
| `\Return` | Return statement |
| `\Comment{text}` | Adds `// text` style comment at end of a line |

**Note:** any variable name or condition inside `{}` (like `$low \gets 0$`) should be wrapped in `$...$` since it's math mode — this is how you get italics for variable names and proper spacing around symbols.

**A `for`-loop example** (so you have both patterns handy):
```latex
\begin{algorithm}
\caption{Sum of Array}
\begin{algorithmic}[1]
\Function{SumArray}{$A$}
    \State $total \gets 0$
    \For{$i \gets 1$ to $\text{length}(A)$}
        \State $total \gets total + A[i]$ \Comment{running total}
    \EndFor
    \State \Return $total$
\EndFunction
\end{algorithmic}
\end{algorithm}
```

> There's also a competing package called `algorithm2e` with slightly different syntax (`\KwIn`, `\ForEach`, `\eIf`) that some courses prefer. If your course template already uses `algorithm2e` commands, don't mix the two packages — just follow whichever one the template uses.

---

## PART C — Math Symbols Reference

All of these work inside `$...$` (inline) or any display math environment. Organized by category so you can scan fast during an exam.

### Greek letters
| Lowercase | Command | Uppercase | Command |
|---|---|---|---|
| α | `\alpha` | Γ | `\Gamma` |
| β | `\beta` | Δ | `\Delta` |
| γ | `\gamma` | Θ | `\Theta` |
| δ | `\delta` | Λ | `\Lambda` |
| ε | `\epsilon` (or `\varepsilon`) | Ξ | `\Xi` |
| θ | `\theta` | Π | `\Pi` |
| λ | `\lambda` | Σ | `\Sigma` |
| μ | `\mu` | Φ | `\Phi` |
| π | `\pi` | Ψ | `\Psi` |
| σ | `\sigma` | Ω | `\Omega` |
| φ | `\phi` (or `\varphi`) | | |
| ω | `\omega` | | |

*(Most Greek letters only have a lowercase command shown as capital by just capitalizing the command name, e.g. `\gamma` → `\Gamma` — same pattern for the rest not listed.)*

### Relations
| Symbol | Command | Symbol | Command |
|---|---|---|---|
| ≤ | `\leq` | ≥ | `\geq` |
| ≠ | `\neq` | ≈ | `\approx` |
| ≡ | `\equiv` | ∝ | `\propto` |
| ≪ | `\ll` | ≫ | `\gg` |
| ∼ | `\sim` | ≅ | `\cong` |

### Set theory
| Symbol | Command | Symbol | Command |
|---|---|---|---|
| ∈ | `\in` | ∉ | `\notin` |
| ⊂ | `\subset` | ⊆ | `\subseteq` |
| ∪ | `\cup` | ∩ | `\cap` |
| ∅ | `\emptyset` (or `\varnothing`) | ⊕ | `\oplus` |
| ℝ, ℕ, ℤ, ℚ, ℂ | `\mathbb{R}`, `\mathbb{N}`, `\mathbb{Z}`, `\mathbb{Q}`, `\mathbb{C}` | | |

### Logic
| Symbol | Command | Symbol | Command |
|---|---|---|---|
| ∀ | `\forall` | ∃ | `\exists` |
| ¬ | `\neg` (or `\lnot`) | ∧ | `\land` |
| ∨ | `\lor` | ⇒ | `\implies` |
| ⇔ | `\iff` | ⊢ | `\vdash` |

### Arrows
| Symbol | Command | Symbol | Command |
|---|---|---|---|
| → | `\to` (or `\rightarrow`) | ← | `\leftarrow` |
| ↔ | `\leftrightarrow` | ⇒ | `\Rightarrow` |
| ⇐ | `\Leftarrow` | ⇔ | `\Leftrightarrow` |
| ↦ | `\mapsto` | ↑ | `\uparrow` |

### Calculus & big operators
| Symbol | Command | Notes |
|---|---|---|
| Σ (sum) | `\sum_{i=1}^{n}` | goes under/over the Σ automatically |
| Π (product) | `\prod_{i=1}^{n}` | same pattern |
| ∫ (integral) | `\int_{a}^{b}` | |
| ∮ (contour integral) | `\oint` | |
| ∂ (partial derivative) | `\partial` | e.g. `\frac{\partial f}{\partial x}` |
| ∇ (nabla/gradient) | `\nabla` | |
| lim | `\lim_{x \to \infty}` | |
| ∞ | `\infty` | |

### Dots (very commonly needed, easy to forget)
| Symbol | Command | Use case |
|---|---|---|
| … (baseline) | `\dots` or `\ldots` | `1, 2, \ldots, n` |
| ⋯ (centered) | `\cdots` | `a_1 + \cdots + a_n` |
| ⋮ (vertical) | `\vdots` | inside matrices |
| ⋱ (diagonal) | `\ddots` | inside matrices |

### Misc operators & decorations
| Symbol | Command |
|---|---|
| ± | `\pm` |
| × | `\times` |
| ÷ | `\div` |
| · | `\cdot` |
| ∘ | `\circ` |
| x̄ (bar) | `\bar{x}` |
| x̂ (hat) | `\hat{x}` |
| x⃗ (vector arrow) | `\vec{x}` |
| ẋ (dot, e.g. velocity) | `\dot{x}` |
| **bold math** | `\boldsymbol{x}` |

### Resizing brackets to fit tall content

When a fraction or stacked expression makes normal `(` `)` too small, prefix with `\left` and `\right` — they auto-grow to match the content height:
```latex
\left( \frac{a}{b} \right)
```
Works with `\left[ \right]`, `\left\{ \right\}`, `\left| \right|` too. **Rule:** every `\left` needs a matching `\right` somewhere, even if you use `\right.` (invisible) for a one-sided case.

---

## PART D — Equation Formatting, Deeper Dive

### The four multi-line environments — which one to use when

| Environment | Use it when... | Numbers each line? |
|---|---|---|
| `align` | You want multiple lines lined up at a specific symbol (usually `=`) | Yes, one number per line |
| `align*` | Same as above but you don't want any numbers | No |
| `gather` | You have several separate centered equations, no alignment needed between them | Yes, one number per line |
| `multline` | ONE long equation too wide for one line — first line left-aligned, last line right-aligned, middle lines centered | One number total |

**`align` example** (most common — lining up multiple steps of a derivation at `=`):
```latex
\begin{align}
    f(x) &= (x+1)^2 \\
         &= x^2 + 2x + 1
\end{align}
```
The `&` marks the alignment point (right before each `=`); every line's `&` lines up vertically.

**Suppressing the number on just ONE line inside an `align` block:**
```latex
\begin{align}
    f(x) &= (x+1)^2 \\
         &= x^2 + 2x + 1 \notag
\end{align}
```
`\notag` (or `\nonumber`) removes the number from that specific line only.

**`gather` example** (independent equations, no shared alignment):
```latex
\begin{gather}
    a^2 + b^2 = c^2 \\
    E = mc^2
\end{gather}
```

**`multline` example** (one equation, split because it's too wide):
```latex
\begin{multline}
    a + b + c + d + e + f \\
    + g + h + i + j + k = 0
\end{multline}
```

### Matrices

```latex
\[
\begin{pmatrix}
1 & 2 \\
3 & 4
\end{pmatrix}
\]
```
Swap `pmatrix` for the bracket style you want:

| Environment | Brackets |
|---|---|
| `pmatrix` | ( ) round |
| `bmatrix` | [ ] square |
| `vmatrix` | \| \| single bars (determinant) |
| `Bmatrix` | { } curly |

### Spacing tweaks (fine control, rarely needed but good to know)
| Command | Gap size |
|---|---|
| `\,` | thin space |
| `\:` | medium space |
| `\;` | thick space |
| `\quad` | one full character width |
| `\qquad` | two full character widths |

Example: `f(x) \quad \text{for all } x \in \mathbb{R}` adds breathing room before the text explanation.

---

## PART E — Citations, Deeper Dive

You already know the manual `thebibliography` method from Part 1. Here's the proper way for a real paper with many sources.

### Step 1 — Create a `.bib` file

In Overleaf: New File → name it `references.bib`. Each source is one "entry":

```bibtex
@article{meeus1991,
  author  = {Jean Meeus},
  title   = {Astronomical Algorithms},
  journal = {Willmann-Bell Inc},
  year    = {1991}
}

@inproceedings{smith2020,
  author    = {John Smith and Jane Doe},
  title     = {A Study of Leap Years},
  booktitle = {Proceedings of the Calendar Conference},
  year      = {2020},
  pages     = {1--10}
}

@book{knuth1997,
  author    = {Donald E. Knuth},
  title     = {The Art of Computer Programming},
  publisher = {Addison-Wesley},
  year      = {1997}
}
```

**Common entry types:** `@article` (journal paper), `@inproceedings`/`@conference` (conference paper), `@book`, `@phdthesis`, `@misc` (websites, anything else).

**Tip:** most papers online have a "Cite as BibTeX" button (Google Scholar → the quote-mark icon → BibTeX) — copy-paste directly instead of typing entries by hand.

### Step 2 — Tell your document to use it

At the very bottom, right before `\end{document}`:
```latex
\bibliographystyle{plain}
\bibliography{references}
```
- `references` = your `.bib` filename, without the `.bib` extension.
- `plain` is the citation **style** — numbers sources in the order cited, e.g. `[1]`. Other common styles:

| Style name | Looks like |
|---|---|
| `plain` | `[1]` numeric, alphabetized by author |
| `unsrt` | `[1]` numeric, in the order first cited (not alphabetized) |
| `alpha` | `[Knu97]` short author+year code instead of a number |
| `ieeetr` | IEEE-style numeric, common in engineering papers |

### Step 3 — Cite in your text

```latex
Leap years were formalized centuries ago \cite{meeus1991}.
```
Produces `[1]` (or whatever number it ends up being) and, with `hyperref` loaded, makes it a clickable jump to the reference list.

**Multiple sources in one citation:**
```latex
\cite{meeus1991, knuth1997}
```
→ `[1, 3]`

### Author-year style citations (e.g. "(Meeus, 1991)" instead of "[1]")

If your field/template wants author-year instead of numbers, add `\usepackage{natbib}` and use these instead of plain `\cite`:

| Command | Output |
|---|---|
| `\citet{meeus1991}` | Meeus (1991) — reads as part of the sentence |
| `\citep{meeus1991}` | (Meeus, 1991) — reads as a parenthetical |

Then set: `\bibliographystyle{plainnat}` instead of `plain`.

### Why does the bibliography sometimes not show up the first time I compile?

BibTeX needs **multiple compile passes** to link citations to the reference list (compile → bibtex → compile → compile). On Overleaf this happens automatically when you click Recompile — if references look broken, just hit Recompile twice.

---

## PART F — Tables, Deeper Dive

### Recap: the basic grid (from Part 1)
```latex
\begin{tabular}{|l|c|c|}
\hline
A & B & C \\
\hline
\end{tabular}
```
This works, but it's not what real papers use — all those vertical bars and heavy `\hline`s look cluttered and dated.

### The professional look: `booktabs`

Needs `\usepackage{booktabs}`. The rule: **no vertical lines, and only three kinds of horizontal lines** — top, a line under the header, and bottom.

```latex
\begin{table}[h]
\centering
\begin{tabular}{lcc}
\toprule
\textbf{Method} & \textbf{Accuracy} & \textbf{Time} \\
\midrule
Rasterization & Medium & $O(n)$ \\
Ray Tracing   & High   & $O(n^2)$ \\
\bottomrule
\end{tabular}
\caption{Comparison of methods}
\end{table}
```
- `\toprule` — thick line at the very top
- `\midrule` — thin line under the header row
- `\bottomrule` — thick line at the very bottom
- Notice the column spec is just `lcc` — **no `|` bars** between letters. This is the modern academic-paper look; use it whenever you're not forced into a specific box-grid style by an assignment sheet.

### Auto-sizing columns to fit the page width: `tabularx`

Regular tables don't wrap long text — it just overflows. `tabularx` adds a special column type `X` that automatically wraps text to fit:

```latex
\usepackage{tabularx}

\begin{table}[h]
\centering
\begin{tabularx}{\textwidth}{l X}
\toprule
\textbf{Term} & \textbf{Definition} \\
\midrule
Leap year & A calendar year containing one additional day, inserted to keep the calendar year synchronized with the astronomical year. \\
\bottomrule
\end{tabularx}
\caption{Glossary}
\end{table}
```
`{\textwidth}` tells it the whole table should be exactly as wide as the page's text area; the `X` column soaks up whatever width is left after the other (`l`) columns, wrapping text inside it automatically.

### Tables that span multiple pages: `longtable`

Regular tables refuse to break across a page — if it's too tall, it either overflows or LaTeX complains. `longtable` fixes this by letting a table flow naturally across page breaks, repeating the header on each new page:

```latex
\usepackage{longtable}

\begin{longtable}{lcc}
\toprule
\textbf{Year} & \textbf{Leap?} & \textbf{Notes} \\
\midrule
\endhead
1900 & No & Divisible by 100, not 400 \\
2000 & Yes & Divisible by 400 \\
2024 & Yes & Divisible by 4 \\
\bottomrule
\end{longtable}
```
`\endhead` marks "everything above this line is the header — repeat it at the top of every page this table spans."

### Coloring table rows/cells

Needs `\usepackage[table]{xcolor}` (note the `[table]` option — plain `xcolor` alone doesn't enable table coloring):

```latex
\usepackage[table]{xcolor}

\begin{tabular}{lcc}
\rowcolor{gray!20}
\textbf{Year} & \textbf{Leap?} & \textbf{Notes} \\
2000 & Yes & \cellcolor{green!20} Divisible by 400 \\
1900 & No & \cellcolor{red!20} Divisible by 100 only \\
\end{tabular}
```
`\rowcolor{gray!20}` colors the entire next row light gray (the `!20` means "20% strength" — lower number = lighter). `\cellcolor{green!20}` colors just one cell.

### Table too wide for the page? Shrink it to fit

```latex
\resizebox{\textwidth}{!}{%
\begin{tabular}{lcccccc}
... your wide table ...
\end{tabular}
}
```
`\resizebox{\textwidth}{!}{...}` scales the whole table down so it's exactly as wide as the page, keeping height proportional. Quick fix when you have too many columns to fit naturally — better than fighting with font sizes manually.

---

## Quick Reference: Which Table Style For Which Situation

| Situation | Use |
|---|---|
| Quick lab table, grid look required by sheet | Regular `tabular` with `|l|c|` and `\hline` |
| Anything for a real report/paper | `booktabs` (`\toprule`/`\midrule`/`\bottomrule`, no `|`) |
| A column has long sentences, not just short values | `tabularx` with an `X` column |
| Table has 20+ rows, might not fit one page | `longtable` |
| Need to highlight specific rows/cells | `xcolor[table]` + `\rowcolor`/`\cellcolor` |
| Table has too many columns to fit the page | `\resizebox` |

---

## One-Page Cheat Sheet for This Guide

| Feature | Command |
|---|---|
| Inline code | `\texttt{code}` |
| Highlighted code block | `\begin{lstlisting}[language=Python] ... \end{lstlisting}` |
| Algorithm wrapper | `\begin{algorithm}\caption{}\begin{algorithmic}[1]...\end{algorithmic}\end{algorithm}` |
| Pseudocode assignment | `\gets` |
| Pseudocode if/else | `\If{} \ElsIf{} \Else \EndIf` |
| Pseudocode loops | `\For{} \EndFor`, `\While{} \EndWhile` |
| Sum / product / integral | `\sum_{i=1}^n`, `\prod_{i=1}^n`, `\int_a^b` |
| Aligned multi-line equations | `\begin{align} a &= b \\ &= c \end{align}` |
| Long single equation, split | `\begin{multline} ... \\ ... \end{multline}` |
| Matrix | `\begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix}` |
| Resizing brackets | `\left( ... \right)` |
| BibTeX file link | `\bibliographystyle{plain}` + `\bibliography{filename}` |
| Cite (numeric) | `\cite{key}` |
| Cite (author-year) | `\citet{key}` / `\citep{key}` (needs `natbib`) |
| Professional table lines | `\toprule`, `\midrule`, `\bottomrule` (needs `booktabs`) |
| Auto-wrapping column | `tabularx` with `X` column type |
| Multi-page table | `longtable` |
| Colored row/cell | `\rowcolor{}` / `\cellcolor{}` (needs `xcolor[table]`) |
| Shrink table to page width | `\resizebox{\textwidth}{!}{...}` |
