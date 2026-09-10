# Polish LaTeX Notes into Textbook Style

## Task

Polish a LaTeX lecture notes file subsection-by-subsection into rigorous textbook style. Work through every subsection sequentially, committing and pushing each one before moving to the next.

## Style Rules

### Environments
- Everything should be inside a proper environment: `defi`, `thm`, `prop`, `cor`, `lemma`, `eg`, `remark`, `notation`, or `proof`. No free text outside environments (except brief connecting sentences around tikz diagrams).
- Use the environment types defined in `header.tex`. Check what's available before starting.

### Notation
- Use `\dotsc` for comma-separated ellipses ($a_1, \dotsc, a_n$).
- Use `\dotsb` for binary-op ellipses ($a_1 + \dotsb + a_n$).
- Use `\dotsm` for multiplicative ellipses ($a_1 \dotsm a_n$).
- Never use bare `\cdots` or `\ldots`.
- Use `\,` spacing in products.
- Use `\colon` for function notation ($f \colon X \to Y$).
- Use the macros defined in `header.tex` (e.g. `\P` for `\mathbb{P}`, `\E` for expectation, `\var`, `\cov`, `\corr`). Never use bare `P(...)` or `E[...]`.

### What to Fix
- Variables used without definition — define them before use.
- Mathematical errors (wrong indices, missing exponents, swapped variables, sign errors).
- Typos in text and formulae.
- Missing `\d x` in integrals.
- `\mapsto` vs `\to` (element map vs function type).
- Variable clashes in integrals (e.g. using `x` as both limit and integration variable).
- Lowercase where uppercase is needed for random variables ($x$ vs $X$).
- `\begin{proof}` indentation consistency.

### What NOT to Do
- Don't change the mathematical content. Keep theorems, definitions, and results essentially the same.
- Don't add new results or remove existing ones.
- Don't add comments to the LaTeX source.
- Don't create new files or documentation.
- Don't change tikz diagrams (keep them exactly as-is, just wrap in an appropriate environment if needed).

### Free Text → Environments
| Free text pattern | Target environment |
|---|---|
| Motivational paragraph before a section | `remark` or `eg` |
| "We can show that..." followed by a formula | `prop` or `thm` with `proof` |
| "Note that..." or "Recall that..." | `remark` |
| "For example..." | `eg` |
| Informal commentary ("Nice!", "Hooray!", exclamation marks) | Remove or tone down |
| "Properties of X" as a list | Wrap in `prop` with a better name |

## Naming Conventions (for Anki)

Every `thm`, `prop`, `cor`, and `lemma` must have a name in square brackets.

### Good names (for recall)
- Short and distinctive: "Gaussian integral", "Sum rule for variances", "Correlation bounds"
- Topic-anchored: "Expectation calculus", "Indicator function algebra"
- Standard names: "Jensen's inequality", "Cauchy–Schwarz inequality"

### Bad names (avoid)
- Too vague: "Properties", "Elementary properties", "Counting formulae"
- Gives away content: "Expectation of a product of independent random variables"
- "A implies B" style: "Convergent implies Cauchy"
- Meta-comments: "(stated without proof)", "(non-examinable)"

### No duplicate names across the document.

## Proof Hints

Every `\begin{proof}` must start with a bold hint line:

```latex
\begin{proof}
  \textbf{Hint: <key idea in one or two phrases>.}

  <actual proof>
\end{proof}
```

The hint should be like a STEP exam hint: enough to make the proof manageable, without giving the full solution. Examples:
- "Bound $\log n!$ between Riemann sums; squeeze."
- "Pointwise bound $I_{|X| \geq \varepsilon} \leq |X|/\varepsilon$; take expectations."
- "Condition on first step; try $p_z = t^z$; characteristic equation has roots $1$ and $q/p$."
- "Fubini: swap order of integration; inner integral $\int_0^y \d x = y$."

## Workflow

1. Read `header.tex` to learn available environments and macros.
2. Grep for `\subsection` and `\section` to get the document structure.
3. For each subsection, in order:
   a. Read the full subsection.
   b. Identify all issues (free text, bugs, notation, missing names, missing hints).
   c. Apply the edit.
   d. Verify the transition to the next subsection is clean.
   e. Commit with a descriptive message listing the key changes.
   f. Push.
4. After all subsections: scan for any remaining unnamed `thm`/`prop`/`cor`/`lemma` and add names. Scan for unnamed proofs and add hints.
5. Review existing names and improve any that are vague, give away content, or have meta-comments.

## Commit Message Style

```
Polish Subsection X.Y (Name) into textbook style

<2-3 lines listing key changes: bug fixes, structural changes, notation fixes>
```

## Common Bug Patterns to Watch For

- `P(...)` instead of `\P(...)`
- `E[...]` instead of `\E[...]`
- Missing minus signs in integral limits (`\int_{\infty}` → `\int_{-\infty}`)
- Wrong subscripts/superscripts in proofs (e.g. $n$ where $k$ is meant)
- `\var (X)` with inconsistent spacing → `\var(X)`
- Missing `^\infty` on sums
- Indicator function notation inconsistency (`I[...]` vs `\mathbf{1}[...]`)
