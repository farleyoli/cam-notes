# Converting lecture notes into one long Grande École-style problem

This document specifies how to turn an existing course `.tex` file in this
repository into a new, self-contained examination paper in English.  The new
paper should read like a long French Grande École entrance problem: its
questions form a coherent mathematical development, difficult results are
broken into attainable steps, and earlier results support later ones.  It is
also a learning document.  A reader who has the background assumed by the
original notes must be able to learn all of the original material from the
new paper.

The rules below apply equally to pure and applied mathematics.  Applied
material may take physical laws or advanced analytic results as stated input,
when the original course does so, but definitions, hypotheses, parameter
ranges, domains, units when relevant, and logical dependencies must remain
precise.

## 1. Required deliverables

For an original source such as

```text
IA_L/probability.tex
```

create these separate files:

```text
IA_L/probability_long_problem.tex
IA_L/probability_long_problem_names.json
probability_long_problem.pdf
```

The original `.tex` file is read-only throughout the conversion.  Do not
rename it, rewrite it, or turn it into the new problem in place.  The existing
`numbers_and_sets.tex` conversion predates this rule and is a legacy example;
future conversions must use a new sibling `.tex` file.

The deliverable consists of the new TeX source, its source-specific JSON name
catalogue, and the compiled PDF.  Keep auxiliary build files out of the source
directory unless the repository already has a different established build
convention.

Before starting, record the original source checksum:

```bash
SOURCE=IA_L/probability.tex
sha256sum "$SOURCE" > /tmp/long-problem-original.sha256
```

At the end, verify that the source is unchanged:

```bash
sha256sum --check /tmp/long-problem-original.sha256
```

If the source was clean in Git before work began, also run:

```bash
git diff --exit-code -- "$SOURCE"
```

Do not edit `header.tex` merely to accommodate one paper.  Put local layout
and problem macros in the new file so that unrelated notes retain their
formatting.

## 2. The content contract

The converted paper must preserve the mathematical scope of the original
notes.

1. Include every definition made in the original source.  Preserve all
   qualifications and conventions that affect its meaning.
2. Represent every theorem, proposition, lemma, corollary, construction,
   substantive example, and exercise from the original source.  It may become
   a question, a sequence of questions, a definition followed by a question,
   or an admitted result.
3. Assume no background that has not been declared explicitly in the target
   paper.  The original source determines the maximum mathematical background
   that may be declared, but its unexplained use of a technical word is not
   permission to repeat that word without explanation.  Do not silently
   require an extra theorem, notation, definition, technical convention, or
   familiarity with specialist vocabulary.
4. A new auxiliary concept introduced only to organize the problem must be
   defined before it is mathematically used.
5. If the original develops a result within the course, the new paper should
   normally guide the reader through its proof.  Do not replace it by an
   admission simply to shorten the paper.
6. If a proof lies outside the scope of the original course, state the result
   completely and mark it as admitted.  There is no requirement to prove
   material that the original source treats as external.
7. Do not add advanced prerequisites merely because they make a proof shorter.
   The steps must remain at approximately French classes préparatoires
   difficulty and rigour.

Make a coverage ledger before drafting.  A table with the following columns
is sufficient:

| Source location | Kind | Exact content to preserve | Target location | Status |
|---|---|---|---|---|
| section or line | definition/result/example/exercise | short description | part and question label | pending/included/checked |

Read the source once to populate the first three columns, and again after the
paper is drafted to complete the last two.  A definition counts as covered
only when all of its clauses occur in the target.  A theorem counts as covered
only when its complete conclusion and hypotheses occur in a question or an
admitted-result statement.

The ledger may be kept as a temporary working document, but it must be fully
checked before the conversion is called complete.  For a large course, store
it in the source-specific JSON under a top-level `coverage` key so the audit
can be resumed reliably.

## 3. Determine the reader's starting point

Before writing questions, identify what the original notes regard as known.
Use evidence from the source: what it defines, what it recalls, what it cites
without comment, and the course level.  Write a short internal list with three
categories:

- background the reader may use without a new definition;
- concepts that the source itself defines and that the new paper must define;
- external results that the source states or uses without proof and that the
  new paper must state as admitted results when they are needed.

Apply this boundary consistently.  For example, if the source defines a
function, implication, sample space, group action, or differentiability, the
paper must define it too.  If the source assumes ordinary algebraic
manipulation, the paper need not rebuild arithmetic from axioms.

Do not infer that a concept is background merely because it is familiar.  The
original source is the authority for the maximum reader model.  When the
evidence is ambiguous, choose the more conservative boundary and explain or
define the concept.  An explicit user instruction that the paper assume less
background than the source always takes precedence.

### Mandatory reader-boundary firewall

Background controls which results the reader may use without proof; it does
not license unexplained terminology.  Before drafting, put a prerequisite
sentence on the instructions page and record the same boundary in the JSON
catalogue.  State positively what is assumed, for example “elementary algebra
and single-variable calculus,” and state important exclusions, for example
“no prior probability or stochastic-process course is assumed.”  Never make
the reader infer the boundary from the first difficult paragraph.

For every domain-specific noun, adjective, symbol, and convention at its first
use, do exactly one of the following:

1. point to an explicit item in the declared background;
2. give a usable definition in the exposition immediately before the use; or
3. if the complete definition or existence theory is too technical for the
   reader model, say that explicitly, give the best elementary interpretation,
   and state every property the reader may use.

Option 3 is not a waiver of precision.  A technical-definition paragraph must
contain all of the following:

- the name of the technical object and the notation used for it;
- a concrete or intuitive interpretation at the reader's level;
- the exact conditions under which it is used in the paper;
- a closed list of operational properties available to the reader; and
- a complete named admitted result for any existence, uniqueness, or other
  theorem that the paper relies on without proof.

Do not write a circular explanation such as “adapted means determined by the
available information” unless “available information” has already been
explained.  Build the chain from elementary notions upward.  For probability
and stochastic calculus, a typical safe order is:

1. random experiment, event, probability and random variable;
2. distribution, expectation, integrability, variance and independence;
3. stochastic process and sample path;
4. observed record, information, filtration and adaptation;
5. conditional expectation and its usable rules;
6. Brownian motion/Wiener process and its defining properties;
7. the precise convergence and admissibility conditions for stochastic
   integrals; and
8. stochastic differential notation as shorthand for an integral equation.

The following are hard failures unless they have already been resolved by the
declared background or nearby exposition:

- “integrable process” without specifying whether it means
  $E[|Y_t|]<\infty$ for each $t$, $E[\int |Y_t|\,dt]<\infty$,
  $E[\int |Y_t|^2\,dt]<\infty$, or another exact condition;
- “information $\mathcal F_t$,” “filtration,” or “adapted” without first
  explaining the observation-record interpretation and its no-future-data
  consequence;
- “Wiener process” without saying that it is another name for standard
  Brownian motion and listing the defining properties that may be used;
- “almost surely,” “in distribution,” “in probability,” or “in mean square”
  without defining the mode of convergence or probabilistic qualification;
- “density,” “distribution function,” “correlation,” “self-financing,” or a
  comparable specialist term without a usable meaning;
- “sufficiently smooth,” “sufficiently regular,” or “sufficiently integrable”
  when the argument actually needs identifiable derivatives, bounds,
  continuity, measurability, or moment conditions; and
- “standard result” or “it is well known” in place of a proof or a complete
  admitted result.

Synonyms must be connected explicitly at first use.  If the source alternates
between “Wiener process” and “Brownian motion,” or between another pair of
names, say whether they are identical, special cases, or merely related.
Likewise, distinguish overloaded words: integrability of one random variable,
integrability of a process over time, and existence of a stochastic integral
are not interchangeable.

Before completing each part, reread it from the stated starting point and make
a first-use ledger with one row per technical term:

| First use | Term or notation | Background, defined, or technical | Reader-facing explanation | Usable properties | Status |
|---|---|---|---|---|---|
| section/line | exact term | one category | nearby paragraph | exact finite list or cited result | pending/checked |

This ledger may be temporary, but every row must be checked.  For probability,
analysis, topology, geometry, physics, finance, or another specialist subject,
search the finished draft for technical adjectives as well as symbols.  A
symbol audit alone will not catch an undefined phrase such as “integrable
process” or “available information.”

## 4. Build the mathematical dependency structure

The target is one long problem, not a chapter-by-chapter transcription.
Construct a dependency graph of the original material:

1. definitions and basic notation;
2. elementary properties derived from them;
3. central constructions and propositions;
4. applications and examples;
5. culminating theorems or comparisons.

Group this graph into around ten named parts.  Treat ten as the planning
target, while allowing a small variation when the mathematical dependency
structure makes that clearer.  Do not collapse a full course into only a few
oversized parts or create many tiny parts merely to reach an exact count.
Within a part, number questions continuously with one counter.  Across parts,
keep the same counter; do not restart at 1.  A later question may cite a
previous question explicitly.  If the dependency structure branches, a small
diagram on the instructions page is useful, but it must reflect actual
dependencies.

Avoid placing all definitions in a preliminary glossary.  Introduce a
definition in the paragraph immediately before the first group of questions
that needs it, or in the opening sentence of that group.  Several definitions
may be introduced together only when they form one natural object that cannot
be separated without repetition.  The requirement is logical clarity and local
availability, rather than mechanical placement one line before every
occurrence.

Keep each concept block small and single-purpose.  The working rule is one
definition per `\concept` block: every mathematical object receives its own
heading as soon as it can be stated without repeating an earlier block.  Do
not bundle distinct instruments, properties, constructions or classifications
under one heading; split them into separate concepts even when each is short.
For example, a heading such as "Spreads, straddles and related structures"
must be split into separate concepts for spreads, straddles, strangles, risk
reversals, butterflies, condors and calendar spreads.

Apply the rule to every kind of list, not only to instruments.  A sentence
such as "The pair $(\Omega,\mathcal F)$ is called a measurable space; a
probability measure is a function ...; the triple
$(\Omega,\mathcal F,\mathbb P)$ is called a probability space" becomes three
concepts, one for the measurable space, one for the probability measure and
one for the probability space.  Likewise, "Bernoulli, binomial and Poisson
distributions" becomes three concepts, "gamma and beta functions" becomes
two, and "joint, marginal and conditional densities" becomes three.  A
concept may introduce more than one object only when the objects are
meaningless in isolation, for instance the clauses of a single recursive
definition.  When in doubt, split: a short concept block is never a defect,
whereas a bundled one is.

Whenever a split creates a concept whose noun phrase is not yet listed in the
JSON `reader_boundary.defined_in_paper` array, add that phrase to the array in
the same pass, so the declared reader boundary stays synchronized.

Each part should have a mathematical purpose.  A short opening paragraph may
tell the reader what will be constructed or proved, but it must not give away
the solution.

### Mandatory part-by-part workflow

Plan all of the approximately ten parts before drafting, then build the paper
strictly one part at a time.  For each part, perform this complete cycle:

1. write or revise that part in the new sibling TeX file;
2. update its entries in the JSON catalogue at the same time;
3. run the name-catalogue, source, and reader-boundary first-use checks for the
   material written so far; in particular, reject any technical term that is
   neither declared background nor defined or explicitly explained;
4. compile the cumulative paper to PDF twice, so the PDF contains every
   completed part through the current one and its references and final-page
   count resolve;
5. inspect the new part in the PDF as a reader with only the declared
   prerequisites, and fix prerequisite leakage, mathematical, typographical,
   and layout problems before starting the next part.

Thus, complete and compile Part I before editing Part II, complete and compile
Part II before editing Part III, and continue in the same way through the
final part.  Do not draft all parts first and postpone compilation until the
end.  The final audit in Sections 11--13 remains required after the last
part, even though every intermediate version has already been compiled.

This workflow does not create user-approval checkpoints.  Continue from one
part to the next without asking for confirmation and finish all planned parts
in the same task.  Pause after a part only when the user explicitly asks for
part-by-part confirmation or gives another explicit stopping condition.

## 5. Write rigorously scoped statements

Every mathematical symbol must be bound, defined, or part of the reader's
established background at its first use.  Apply the following checklist to
every definition, question, indication, and admitted result.

- State the set containing each variable: for example, “let
  $n\in\mathbb N$,” “for every real number $x\in\mathbb R$,” or “let $A$
  be a set.”
- State positivity, non-zero assumptions, ordering, finiteness, regularity,
  and distinctness wherever they are required.
- Give every function its domain and codomain, such as $f:A\to B$, before
  evaluating it or discussing injectivity or continuity.
- For a family $(A_i)_{i\in I}$, say that $I$ is an index set and state
  what kind of object each $A_i$ is.
- Give the range of every sum, product, recurrence, sequence, and matrix
  index.  Define initial values before using a recurrence.
- State what happens in empty or boundary cases whenever the definition or
  theorem reaches them.
- When using extrema, infima, or suprema, state the hypotheses that ensure
  they exist, or explicitly ask the reader to establish existence first.
- State the codomain or ambient space of equations and operations.  An equality
  of functions, sets, vectors, equivalence classes, or random variables should
  be unambiguous.
- Define constants and their dependencies.  Write, for example,
  $C=C(f,K)>0$ if later estimates depend on $f$ and $K$.
- Define any new symbol such as $\Rightarrow$, $o(1)$, a norm, an
  equivalence class, or an expectation if it was defined rather than assumed
  in the original source.

A sentence such as “show that it converges” is incomplete until “it” has a
unique antecedent and the relevant space and limiting process are clear.
Likewise, “for $n$” is not a declaration.  Write “for every $n\in\mathbb N$”
or give the exact range required.

Definitions should be usable, rather than merely named.  If the paper defines
an equivalence relation, for example, it must state reflexivity, symmetry, and
transitivity with all variables quantified.  If it defines a probability
density, it must state its domain, sign, normalization, and the measure or
integration convention at the level used by the original notes.

## 6. Turn exposition and proofs into exam questions

Use alphabetically labelled subquestions for the successive mathematical
steps.  Each subquestion must require a concrete mathematical action: compute,
prove, deduce, construct an explicit object, derive an equation, solve a
problem, verify a stated identity or inequality, or give a counterexample.
A request to compare is acceptable only when it specifies a mathematical
quantity, equality, inequality, inclusion, limit, or other formal relation
that the reader must establish.  The result of one item should create a useful
tool for the next.

Never ask the reader to define anything.  A definition is expository material,
not a task, and it belongs in the running text immediately before the questions
that use it.  Do not phrase a subquestion as “define”, “give the definition
of”, “state the definition of”, or “state what it means for”.  Axioms and
conventions are definitions in this sense and must likewise be stated in the
exposition.  Every lettered task must ask the reader to do something with the
defined objects, such as prove a property, compute an example, or verify an
equivalence.

### Mandatory mathematical-task-only rule

No lettered item may be a recall, vocabulary, orientation, or prose-description
question.  In particular, do not ask the reader to state meanings or
assumptions, identify roles or named components, classify terminology,
describe a graph or analogy, explain a sign or modelling choice in words,
interpret a result, contrast methods, discuss limitations, or list facts from
the preceding exposition.  A descriptive clause does not become acceptable
merely because it is appended to a calculation or proof; remove that clause or
replace it with a precise mathematical claim to establish.

Definitions, modelling interpretations, assumptions, terminology, historical
context, method comparisons, warnings, and technical limitations may still be
essential source content.  Preserve them as declarative exposition immediately
before the mathematics that uses them.  If such material has no honest
mathematical exercise at the declared reader level, do not invent one: retain
the material in exposition and omit the lettered item.  Never turn a factual
description into a pseudo-mathematical task by asking the reader merely to copy
a displayed formula, attach labels to its terms, or repeat a supplied sentence.

For every proposed lettered item, apply this test: after deleting all requests
for prose commentary, does the item still require the reader to produce a
mathematical object or establish a mathematical proposition?  If not, move its
content to exposition and remove the item.  Acceptable outputs include a
number, formula, table of calculated values, constructed example, derivation,
solution of an equation, proof, counterexample, or verified formal relation.

For a proof of ordinary difficulty, two or three subquestions may be enough:

1. establish the key lemma;
2. apply it under the theorem's hypotheses;
3. state or prove the final proposition in its full form.

For a hard theorem, first write the complete target statement and identify its
essential proof steps.  Then make every non-routine step one of the following:

- an earlier question whose result is cited;
- a subquestion with enough intermediate structure to be solved at the target
  level;
- an optional `\indication{...}` that points to the key idea;
- a precisely stated admitted result when that step is outside the original
  course's scope.

An indication should unlock a step without writing its proof.  Good
indications suggest an object to construct, an earlier result to apply, a
useful inequality, or a contradiction to assume.  They may specify a helpful
auxiliary quantity.  They should not contain a sequence of commands that is
already a complete solution.

Do not create circular scaffolding.  If item (d) depends on item (b), item (b)
must neither use nor restate the conclusion of (d).  When a later part may use
an earlier result even if the reader did not solve it, say so on the
instructions page and require the question number to be cited.

The final item of a proof sequence should state the complete theorem with all
hypotheses, even if earlier items have established most of it.  This makes the
paper useful for review and makes the status of the result clear.

Examples from the source should normally become short computations,
constructions, or counterexample questions.  Remarks that carry mathematical
content should be incorporated into nearby definitions, questions, or
admitted results.  Purely historical remarks may remain as brief prose if they
help orient the reader.

## 7. Adapt applied mathematics without lowering the standard

For an applied course, separate modelling input from mathematical deduction.

- Define the state variables, independent variables, parameters, coordinate
  systems, and sign conventions used in the model.
- State the allowed range and, where the source uses them, the physical units
  of each quantity.
- State modelling laws or constitutive relations before using them.  If the
  original notes accept such a law without derivation, label it as an
  assumption or an admitted result as appropriate.
- Distinguish exact equalities from approximations and asymptotic statements.
  State the limit, small parameter, and order of the discarded term.
- State regularity and boundary or initial conditions needed for each
  differential equation and manipulation.
- When changing variables or coordinates, state the domain of the
  transformation and any non-vanishing Jacobian assumption.
- When interchanging a derivative, integral, expectation, or infinite sum,
  either ask for a justification available at the course level or cite a
  complete admitted theorem.

The algebra and analysis following the modelling assumptions should have the
same precision as a pure mathematics proof.  “Physically plausible” does not
replace a mathematical step when the original course derives that step.

## 8. Admitted results

Use an admitted result only in either of these situations:

1. the original notes explicitly state or use the result without proving it;
2. a bridge is required for the long problem, but its proof is outside the
   original scope and would require stronger background than the original
   assumes.

Every admitted result must be a complete, standalone mathematical sentence.
It must quantify or introduce all variables, state every hypothesis, and give
the whole conclusion.  Define its notation before the admitted-result block.
Do not write only “the fundamental theorem applies” or “we admit the usual
bound.”

Every admitted result also receives a unique acronym and phrase under the
naming rules in the next section.  Use this form:

```tex
\admitted{PRB}{Polynomial Root Bound}{If $d\in\mathbb N_0$ and
$c_0,\ldots,c_d\in\mathbb C$ satisfy $c_d\ne0$, then the equation
$c_0+c_1z+\cdots+c_dz^d=0$ has at most $d$ solutions $z\in\mathbb C$.}
```

If one displayed block contains two logically independent theorems, give them
separate admitted-result entries unless the source conventionally treats them
as one named result.  A result proved through the questions is not labelled
“admitted.”

Before final compilation, search for informal admission language and either
convert it to the macro or remove it:

```bash
rg -n -i 'admit|admitted|without proof|we shall use|standard result' \
  IA_L/probability_long_problem.tex
```

## 9. Acronyms and proposition phrases

Give a name to every alphabetically labelled subquestion, and to every
admitted result.  Most numbered questions will therefore contain several
names.  A plain definition does not need a name unless it is itself presented
as a lettered task.  A result stated between questions must either be derived
from earlier work or presented as a named admitted result.

Each name has two components:

1. a short acronym in bold sans serif;
2. a unique descriptive phrase in italics immediately after it.

The acronym is exactly the ordered concatenation of the initials of the
capitalized words, including capitalized components after a hyphen, in the
phrase.  Lowercase connector words do not contribute.  The capital letters in
the printed phrase therefore show the derivation of the acronym.

Valid examples are:

```text
TTLE  Truth Table Logical Equivalences
DPO   Divisibility as a Partial Order
WFRT  Well-Founded Recursion Theorem
PRB   Polynomial Root Bound
```

`TLE Truth Table Equivalences` is invalid because that phrase supplies the
letters `TTE`, not `TLE`.  Fix a collision or mismatch by writing a better,
longer phrase.  Do not append an arbitrary digit or an unrepresented letter.

Both acronyms and phrases must be unique within one target `.tex` file.  They
need not be globally unique across the repository.  Prefer names that describe
the mathematical proposition rather than the action “prove” or “show.”

There is no dash between the phrase and the mathematical text.  The macro
prints a period after the phrase:

```tex
\item \nameditem{DPO}{Divisibility as a Partial Order}
Prove that divisibility is a partial order on $\mathbb N$.
```

### The JSON catalogue is mandatory

Maintain one JSON dictionary for each target TeX file.  Reserve or edit the
acronym and phrase in the JSON catalogue whenever a named item is added,
removed, or renamed in the TeX.  Then copy that exact pair into the TeX.  Do
not rely on memory or a search performed only at the end.

Use this schema:

```json
{
  "source": "IA_L/probability_long_problem.tex",
  "uniqueness_scope": "source",
  "acronym_rule": "Read the phrase from left to right and concatenate the initial of each capitalized word or capitalized hyphen component. Lowercase words do not contribute.",
  "reader_boundary": {
    "background": [
      "elementary algebra",
      "single-variable differentiation and integration"
    ],
    "explicitly_not_assumed": [
      "prior measure theory",
      "prior stochastic-process terminology"
    ],
    "defined_in_paper": [
      "random variables, expectation and integrability",
      "information records, filtrations and adaptation"
    ],
    "technical_but_explained": [
      {
        "concept": "conditional expectation",
        "reader_facing_explanation": "probability-weighted prediction using the observed record",
        "usable_properties": [
          "linearity",
          "known quantities remain unchanged",
          "an independent future quantity keeps its unconditional expectation"
        ]
      }
    ],
    "external_admitted_results": [
      "Conditional Expectation Existence and Uniqueness"
    ]
  },
  "parts": {
    "I": {
      "status": "in_progress",
      "questions": {
        "q:sample-spaces": [
          {
            "letter": "a",
            "code": "FSS",
            "phrase": "Finite Sample Space"
          }
        ]
      },
      "admitted_results": [
        {
          "code": "MCT",
          "phrase": "Monotone Convergence Theorem"
        }
      ]
    }
  }
}
```

Use the actual TeX question label as the key in `questions`.  Set a part's
status to `complete` only after all its lettered items and admitted results
appear in both files and pass validation.  Parts with no admitted result must
still contain an empty `admitted_results` array.

The `reader_boundary` object is mandatory.  Its wording may be adapted to the
subject, but it must distinguish declared background, important exclusions,
concepts defined in the paper, technical concepts given a reader-facing
explanation, the exact properties permitted for those concepts, and external
admitted results.  Do not put a term under `defined_in_paper` merely because
the TeX names it; the TeX must actually provide a usable definition.  Keep
the JSON boundary synchronized whenever drafting introduces a new technical
notion.

If the TeX is changed in several passes, the JSON remains the source-specific
registry throughout every pass.  Check it before inventing the next name.  A
renamed phrase requires a new acronym whenever its capitalized initials
change.

## 10. LaTeX structure and formatting

Use the repository's `header.tex` for its established mathematical macros and
packages, then keep all problem-specific formatting local to the new source.
The following skeleton matches the proportions and visual language of the
Numbers and Sets long paper.  Replace all metadata and title text; do not copy
course facts that do not apply.

```tex
\documentclass[10pt,a4paper]{article}

\def\npart {IA}
\def\nterm {Lent}
\def\nyear {2015}
\def\nlecturer {Lecturer Name}
\def\ncourse {Course Name}
\def\ncoursehead {Course Name --- Long Problem}
\def\nisofficial {true}

\input{header}

\usepackage[
  a4paper,
  left=40mm,
  right=40mm,
  top=24mm,
  bottom=24mm,
  headheight=14pt,
  headsep=6mm,
  footskip=10mm,
  heightrounded
]{geometry}
\usepackage{lastpage}
\usepackage{needspace}

\hypersetup{
  pdfauthor={Adapted from the original course notes},
  pdftitle={Course Name: Long Problem},
  pdfsubject={An extended mathematical problem}
}

\setlength{\parindent}{0pt}
\setlength{\parskip}{3pt}
\setlist[enumerate]{itemsep=2pt,topsep=4pt}
\frenchspacing
\flushbottom

\newcounter{examquestion}
\newcommand{\examquestion}[1]{%
  \Needspace{5\baselineskip}%
  \refstepcounter{examquestion}%
  \par\addvspace{0.8\baselineskip}%
  \noindent\textbf{\theexamquestion)}\enspace #1\par
}
\newenvironment{subquestions}
  {\begin{enumerate}[label=\alph*),leftmargin=2.1em]}
  {\end{enumerate}}
\newcommand{\nameditem}[2]{%
  \textbf{\textsf{#1}}\enspace\emph{#2.}\enspace
}
\newenvironment{properties}
  {\begin{enumerate}[label=(\roman*),leftmargin=2.4em]}
  {\end{enumerate}}
\newcommand{\indication}[1]{%
  \par\smallskip\noindent\emph{Indication.---} #1\par
}
\newcommand{\admitted}[3]{%
  \par\smallskip\noindent\emph{Admitted result.}\enspace
  \nameditem{#1}{#2}#3\par
}
\newcommand{\concept}[1]{%
  \par\addvspace{0.55\baselineskip}\noindent\textbf{#1.}\enspace
}
\newcommand{\exampart}[2]{%
  \clearpage
  \markboth{Part #1}{}
  \begin{center}
    {\large\bfseries Part #1. --- #2}
  \end{center}
}

\pagestyle{fancy}
\fancyhf{}
\setlength{\headwidth}{\textwidth}
\lhead{\emph{\nouppercase{\leftmark}}}
\rhead{\ncoursehead}
\cfoot{\thepage\ of \pageref{LastPage}}

\begin{document}
\pagenumbering{roman}

\begin{titlepage}
  \thispagestyle{empty}
  \begin{center}
    \vspace*{1.3cm}
    {\Large\bfseries COURSE NAME\par}
    \vspace{0.35cm}
    \rule{0.72\textwidth}{0.6pt}
    \vspace{0.65cm}

    {\huge\bfseries Long-problem title\par}
    \vfill
    {\Large Extended mathematical problem\par}
    \vfill
    {\small Adapted from the original course notes.\par}
    \vspace*{0.8cm}
  \end{center}
\end{titlepage}

\pagenumbering{arabic}

\begin{center}
  {\large\bfseries Instructions}
\end{center}

The paper consists of one problem divided into N parts and continuously
numbered questions.  The result of any question may be used later, even if
that question has not been answered, provided its number is cited clearly.
Indications are optional.  Definitions and notation are introduced when they
first become necessary.  No result may be used unless it has been stated in
the paper or proved in an earlier question.

\exampart{I}{First part title}

\concept{First locally needed concept}
Give its complete definition here.  Give every further definition its own
\concept block; do not collect several definitions under one heading.

\examquestion{\label{q:first-question}
Let $n\in\mathbb N$.
\begin{subquestions}
  \item \nameditem{FSP}{First Stated Proposition}
  Prove the first precise claim about $n$.
  \item \nameditem{SDC}{Second Derived Consequence}
  Deduce the next precise claim.
\end{subquestions}}
\indication{For part (b), apply the result of part (a) to the stated object.}

\end{document}
```

The 40 mm side margins deliberately keep lines fairly short.  The 24 mm top
and bottom margins, fixed header width, `heightrounded`, and `\flushbottom`
give consistent page frames.  Do not reduce these margins merely to shorten
the PDF.  If a heading or formula overflows, rewrite or break that local
content.

Use `\Needspace` through `\examquestion` so a number is not stranded at the
bottom of a page.  Begin every main part on a new page with `\exampart`.  The
footer gives the current and final page numbers; compile twice so
`LastPage` resolves.

The `properties` environment is for subordinate Roman-numeral lists inside a
lettered question.  Its entries do not receive acronym phrases unless they
are themselves promoted to independent lettered tasks.  Do not use it to hide
several unrelated questions under one acronym.

## 11. Mechanical validation of the name catalogue

Run the following validator from the repository root.  It checks acronym
derivation, uniqueness, JSON syntax, and exact agreement between the JSON
pairs and the TeX pairs.  Set `CATALOG` and `TARGET` for the course at hand.

```bash
CATALOG=IA_L/probability_long_problem_names.json
TARGET=IA_L/probability_long_problem.tex

python3 - "$CATALOG" "$TARGET" <<'PY'
import json
import re
import sys
from collections import Counter
from pathlib import Path

catalog_path = Path(sys.argv[1])
target_path = Path(sys.argv[2])
data = json.loads(catalog_path.read_text())
tex = target_path.read_text()

if data["source"] != target_path.as_posix():
    raise SystemExit(
        f'catalogue source {data["source"]!r} does not match {target_path}'
    )

question_entries = []
admitted_entries = []
question_labels = []
for part_name, part in data["parts"].items():
    if part["status"] not in {"in_progress", "complete"}:
        raise SystemExit(f"invalid status in Part {part_name}")
    for label, items in part["questions"].items():
        question_labels.append(label)
        letters = [item["letter"] for item in items]
        if len(letters) != len(set(letters)):
            raise SystemExit(f"duplicate letter in {label}")
        question_entries.extend(items)
    admitted_entries.extend(part.get("admitted_results", []))

if len(question_labels) != len(set(question_labels)):
    raise SystemExit("duplicate question label in JSON")

entries = question_entries + admitted_entries
codes = [entry["code"] for entry in entries]
phrases = [entry["phrase"] for entry in entries]

def initials(phrase):
    # A contributing word or hyphen component starts with A-Z followed by a-z.
    return "".join(
        re.findall(r"(?:^|[^A-Za-z])([A-Z])(?=[a-z])", phrase)
    )

bad = [
    (entry["code"], entry["phrase"], initials(entry["phrase"]))
    for entry in entries
    if entry["code"] != initials(entry["phrase"])
]
if bad:
    raise SystemExit(f"acronym mismatches: {bad}")
if len(codes) != len(set(codes)):
    raise SystemExit(
        f"duplicate codes: {[x for x, n in Counter(codes).items() if n > 1]}"
    )
if len(phrases) != len(set(phrases)):
    raise SystemExit(
        "duplicate phrases: "
        f"{[x for x, n in Counter(phrases).items() if n > 1]}"
    )

named_pairs = re.findall(
    r"\\nameditem\s*\{([^{}]+)\}\s*\{([^{}]+)\}", tex
)
# Ignore the #1/#2 occurrence inside the definition of \admitted.
named_pairs = [pair for pair in named_pairs if not pair[0].startswith("#")]
admitted_pairs = re.findall(
    r"\\admitted\s*\{([^{}]+)\}\s*\{([^{}]+)\}", tex
)
tex_pairs = named_pairs + admitted_pairs
json_pairs = [(entry["code"], entry["phrase"]) for entry in entries]

tex_question_labels = re.findall(
    r"^\\examquestion\{\\label\{([^{}]+)\}", tex, re.MULTILINE
)
if len(tex_question_labels) != len(set(tex_question_labels)):
    raise SystemExit("duplicate question label in TeX")
if Counter(tex_question_labels) != Counter(question_labels):
    missing = list(
        (Counter(question_labels) - Counter(tex_question_labels)).elements()
    )
    extra = list(
        (Counter(tex_question_labels) - Counter(question_labels)).elements()
    )
    raise SystemExit(
        f"question-label mismatch; missing={missing}, extra={extra}"
    )

if Counter(tex_pairs) != Counter(json_pairs):
    missing = list((Counter(json_pairs) - Counter(tex_pairs)).elements())
    extra = list((Counter(tex_pairs) - Counter(json_pairs)).elements())
    raise SystemExit(f"TeX/JSON mismatch; missing={missing}, extra={extra}")

# Count alphabetic items while excluding nested Roman-numeral property lists.
environment_tokens = re.finditer(
    r"\\begin\{(subquestions|properties|enumerate|itemize|description)\}"
    r"|\\end\{(subquestions|properties|enumerate|itemize|description)\}"
    r"|\\item\b",
    tex,
)
environment_stack = []
lettered_item_count = 0
for token in environment_tokens:
    if token.group(1):
        environment_stack.append(token.group(1))
    elif token.group(2):
        if not environment_stack or environment_stack[-1] != token.group(2):
            raise SystemExit("unbalanced list environments")
        environment_stack.pop()
    elif environment_stack and environment_stack[-1] == "subquestions":
        lettered_item_count += 1
if environment_stack:
    raise SystemExit("unbalanced list environments")
if lettered_item_count != len(question_entries):
    raise SystemExit(
        f"{lettered_item_count} lettered TeX items but "
        f"{len(question_entries)} JSON question entries"
    )

print(
    f"validated {len(question_labels)} questions, "
    f"{len(question_entries)} lettered items, and "
    f"{len(admitted_entries)} admitted results"
)
PY
```

This script checks names, question labels, and the number of top-level
alphabetic items.  Nested items in `properties` are deliberately excluded.
It does not check mathematical coverage, so the coverage ledger and a
line-by-line editorial audit remain necessary.

## 12. Compilation and document checks

Compile from the repository root so `\input{header}` and imported assets
resolve consistently:

```bash
TARGET=IA_L/probability_long_problem.tex
JOB=probability_long_problem

pdflatex -interaction=nonstopmode -halt-on-error "$TARGET"
pdflatex -interaction=nonstopmode -halt-on-error "$TARGET"
```

Inspect the log for unresolved references and layout defects:

```bash
rg -n 'LaTeX Warning|Package .*Warning|Overfull|Underfull|undefined|multiply defined|Rerun' \
  "$JOB.log"
```

An inherited harmless package warning may be documented, but undefined
references, multiply defined labels, overfull boxes, and underfull page-layout
problems must be fixed.  Recompile after every fix that can change pagination
or references.

Run these additional checks:

```bash
python3 -m json.tool IA_L/probability_long_problem_names.json >/dev/null
git diff --check -- \
  IA_L/probability_long_problem.tex \
  IA_L/probability_long_problem_names.json
pdfinfo probability_long_problem.pdf
pdftotext -layout probability_long_problem.pdf \
  /tmp/probability_long_problem.txt
```

Use the extracted text to check continuous question numbering, missing
phrases, broken symbols, and the final page text.  Count the source question
labels and confirm that every label is unique and every `\ref` resolves.

Visually inspect at least:

- the title page;
- the instructions page and dependency diagram, if present;
- every page that declares prerequisites or explains a technical reader
  boundary;
- the first page of every part;
- pages containing long displays, tables, figures, or admitted results;
- every transition where a question nearly reaches the page foot;
- the final page.

The left and right edges of the text, running headers, and footers should be
consistent.  No question number should be isolated from its opening text.
Figures must remain legible at the final page size, and no acronym or phrase
may run into the margin.

After the final successful compilation and all checks, delete the auxiliary
build files so they are not left in the repository or committed:

```bash
rm -f "$JOB".aux "$JOB".idx "$JOB".log "$JOB".out
```

These `.aux`, `.idx`, `.log`, and `.out` files are also listed in `.gitignore`
and must never be committed; only the `.tex`, JSON catalogue, and PDF are
deliverables.

## 13. Final mathematical audit

Read the target as a student would, from the beginning, without consulting the
original source.  At every line ask:

1. Is each symbol already known or defined here?
2. Is every variable declared, with its set and any necessary restrictions?
3. Is every cited result stated earlier or explicitly admitted?
4. Does the requested conclusion follow from the available material at the
   intended level?
5. Does an indication provide enough direction for the hardest step while
   leaving meaningful work?
6. Does the question state the result fully enough to be used later?
7. Can a reader using only the declared prerequisites paraphrase every
   technical noun and adjective at its first use?
8. If a complete definition is too technical, does the paper say so, give an
   elementary interpretation, and list the exact properties available for
   use?
9. Is every occurrence of “integrable,” “regular,” “smooth,” “information,”
   “independent,” or another overloaded qualifier tied to a precise meaning
   in this context?
10. Are synonyms explicitly identified, rather than presented as if they were
    unrelated objects?
11. Does each existence or uniqueness assertion that depends on theory beyond
    the reader model appear as a complete named admitted result?
12. Does the prerequisite statement in the PDF agree with the
    `reader_boundary` object in the JSON catalogue?
13. Does every lettered item require a mathematical calculation,
    construction, derivation, solution, verification, proof, or
    counterexample, with no appended request for verbal description or
    interpretation?
14. Does every `\concept` block introduce a single definition?  List the
    headings and inspect every block:
    ```bash
    rg -n '\\concept\{' IA_L/probability_long_problem.tex
    ```
    A heading that names several objects, such as "Expectation, variance and
    moment generating function" or "Joint, marginal and conditional
    densities", is a defect even when the definitions themselves are correct;
    split it into one concept per object, update `defined_in_paper`, and
    recompile.

Perform a separate task-type pass over every `\nameditem`.  Search at least for
the following prompt verbs inside lettered items and inspect every match:

```bash
rg -n -i '\b(define|state|identify|classify|describe|explain|interpret|contrast|discuss|list|compare)\b' \
  IA_L/probability_long_problem.tex
```

The search will also find exposition and mathematical uses such as comparing
two explicitly given payoffs, so it is an audit aid rather than an automatic
validator.  Every occurrence in an actual task must either be removed or be
rewritten as a precise mathematical action under the rule in Section 6.

Then compare against the original source using the coverage ledger.  Check
definitions word by word where qualifiers matter.  Check both directions of
equivalences, all cases of classifications, existence and uniqueness clauses,
boundary cases, and hypotheses hidden in surrounding prose.

Perform a dedicated vocabulary pass separately from the symbol pass.  Search
for specialist words and phrases, then inspect their first occurrence in the
PDF.  In a probability or stochastic-calculus paper, the pass must include at
least `random variable`, `distribution`, `integrable`, `independent`,
`process`, `sample path`, `information`, `filtration`, `adapted`, `conditional
expectation`, `almost surely`, `Brownian`, `Wiener`, `mean square`, `stochastic
integral`, and `self-financing` whenever they occur.  Use the analogous
subject vocabulary in other courses.  Marking the vocabulary pass complete
without inspecting the rendered first-use paragraph is a failed audit.

For every admitted result, verify all of the following:

- its proof was outside the original scope or it was already external there;
- the label “Admitted result.” is visible in the PDF;
- its mathematical sentence stands on its own;
- all notation and variables are defined;
- its acronym and phrase occur in the JSON catalogue;
- the acronym is exactly derived from the capitalized phrase initials;
- neither the acronym nor the phrase occurs elsewhere in the target.

## 14. Completion criteria

A conversion is complete only when all of these statements are true:

- the original `.tex` file is unchanged;
- the new sibling `.tex`, source-specific JSON, and compiled PDF exist;
- every original definition and substantive result is covered;
- the target declares its prerequisites explicitly and assumes no
  mathematical background beyond that declaration or beyond the original;
- the JSON contains a synchronized `reader_boundary` object with declared
  background, important exclusions, defined concepts, technical explanations
  and usable properties, and external admitted results;
- every specialist term is defined or explained at first use, even when the
  source uses it without comment;
- every definition judged too technical is identified as such and paired with
  a reader-level interpretation and an exact list of usable properties;
- vague placeholders such as “sufficiently regular” or “integrable process”
  have been replaced by the actual conditions needed;
- synonyms and overloaded terms are explicitly disambiguated;
- definitions and notation appear near their first need;
- every concept block introduces a single definition, with multi-object
  headings split into one concept per object and the new phrases recorded in
  `defined_in_paper`;
- no question asks the reader to define, state the definition of, or state what
  something means; definitions and axioms are given only in the exposition;
- no question asks for a non-mathematical description, explanation,
  interpretation, classification, comparison of approaches, or recitation of
  assumptions; every lettered item still contains a substantive mathematical
  task after all prose-commentary requests are removed;
- every variable, function, family, index, and parameter is properly scoped;
- hard in-scope proofs are divided into attainable, non-circular steps;
- every out-of-scope result used by the paper is completely stated and visibly
  marked as admitted;
- every lettered item and admitted result has a valid unique acronym and a
  unique phrase;
- TeX and JSON name pairs agree exactly;
- question numbers and references are continuous and resolved;
- two LaTeX passes succeed without material warnings or layout defects;
- the auxiliary `.aux`, `.idx`, `.log`, and `.out` files are deleted after the
  final compilation and are not committed;
- visual inspection confirms consistent margins, headers, footers, and page
  endings;
- the coverage ledger and final student-style mathematical audit are complete.

When reporting completion, name the created files, give the number of parts,
questions, lettered items, and admitted results, state the PDF page count, and
summarize compilation and validation.  Mention any remaining inherited warning
whose cause lies in the shared repository setup.
