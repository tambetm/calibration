# Which Calibration?

A working index of calibration notions in machine learning.

**→ [tambetm.github.io/calibration](https://tambetm.github.io/calibration/)**

"Calibration" names at least 26 different mathematical properties, and papers routinely use the same
word for notions a whole level apart in strength. This is a single-page tool for answering one
question: *which notion of calibration suits my task?* — along with the metric that estimates it and
the method that fixes it.

No build step, no server, no dependencies. `index.html` is self-contained.

## Contents

| File | What it is |
| --- | --- |
| `index.html` | The tool. Open it locally or use the hosted link above. |
| `calibration-notions.json` | The database on its own, so it can be extended or imported without touching the app. |

67 entries: 26 notions, 18 metrics/estimators, 21 methods, and 2 close neighbours that keep getting
confused with calibration. Settings covered: binary, multiclass, regression, forecasting, LLM,
detection, fairness.

## What the tool does

- **Search** across names, aliases, definitions, pitfalls, authors, and library names
- **Filters** by type (notion / metric / method) and setting
- **Hierarchy view** with implication arrows, for classification and for regression
- **Guided picker** keyed on model output type, downstream decision, subgroup requirements, and
  validation-set size — produces a ranked shortlist with the matching metric and fix
- **Per entry**: formal definition, plain-language reading, "reach for it when", "where it bites",
  implication links, primary references, and copy-paste snippets for netcal, torchmetrics,
  torch-uncertainty, uncertainty-toolbox, relplot, scikit-learn, MAPIE, dirichletcal, mcboost, and
  R's CalibrationCurves
- Deep links (`#canonical`), `/` to focus search, keyboard navigation, reduced-motion support

## Why it is shaped this way

**The notions form a partial order, not a list.** Almost every notion comes from one scheme: take the
predicted distribution, push it through a summary function, and require the summary to be calibrated.
Confidence calibration uses `max`, class-wise uses coordinate `k`, group-wise uses a group indicator,
decision calibration uses a Bayes action, canonical uses the identity. Widmann et al.'s
F-calibration makes this explicit, and the regression notions fall out of the same frame. Hence the
hierarchy view — the ordering is the most useful thing a practitioner can learn.

**The name collisions are the real problem.** "Strong calibration" means canonical calibration in ML
and covariate-conditional calibration in clinical epidemiology — different by a whole level of the
hierarchy. "Distribution calibration" means Kull & Flach's multiclass notion or Song et al.'s
regression notion. Quantile calibration is also called probabilistic, average, or just "calibration".
Conformal papers call their held-out split the "calibration set". Every entry therefore carries an
explicit `aka` list, and search covers aliases.

**Practitioners pick the notion the tooling implies, not the one their task needs.** ECE is reported
by default, and it estimates the weakest interesting notion. Sample size is the constraint people
underweight most, so the picker asks about it as a first-class question.

**Notion, estimator, and fix are three different choices, and the field conflates them.** Each entry
is typed and cross-linked in both directions: from "class-wise calibration" you reach class-wise ECE
and Dirichlet calibration in one click, and from "temperature scaling" you see that it only targets
the top-label notion.

## Known gaps

1. **Snippets are indicative, not tested.** They were written from documentation, not run against
   pinned versions. A CI job that executes each snippet on toy data is the obvious next step.
2. **Structured prediction is thin.** Segmentation, tracking, ranking/CTR, and survival analysis each
   have their own calibration literature that is not yet represented.
3. **No empirical layer.** A small benchmark running every metric on the same predictions — so users
   can see that ECE with 10 bins and smECE disagree, and by how much, rather than being told so.
4. **Strength ordering is partly editorial.** Cross-domain comparisons (is multicalibration stronger
   than class-wise calibration?) are not always well-defined. The strength pips are a reading aid;
   the implication arrows are the defensible claim.
5. **The LLM section will date fastest.** Semantic and verbalized calibration are moving quickly.

## Contributing

The database is one JSON array; each entry is one object. Adding a notion, metric, or method means
adding one entry — pull requests welcome. Entry fields:

| Field | Meaning |
| --- | --- |
| `id`, `name`, `aka` | Identifier, display name, and known aliases (searched) |
| `kind` | `notion`, `metric`, `method`, or `contrast` |
| `settings` | Any of `binary`, `multiclass`, `regression`, `forecasting`, `llm`, `detection`, `fairness` |
| `strength` | Reading aid, 1–5; see gap 4 above |
| `short`, `formal`, `plain` | One-line summary, formal statement, plain-language reading |
| `useWhen`, `watchOut` | When to reach for it; where it bites |
| `implies` | Notions this one implies (drives the hierarchy arrows) |
| `notion`, `measure`, `achieve` | Cross-links between a notion and its metrics and methods |
| `code` | Library snippets |
| `refs` | Primary references |

Corrections to definitions and references are as welcome as new entries — the citation is the point.
