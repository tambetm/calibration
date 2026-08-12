# Calibration notions: research brief and v1 tool

A working prototype of the idea on Novin's card. One self-contained HTML file, no build step, no
server: `index.html`. The database is also exported separately as
`calibration-notions.json` so it can be extended without touching the app.

## What the literature actually looks like

67 entries: 26 distinct notions, 2 close neighbours that keep getting confused with calibration,
19 metrics/estimators, 20 methods.

Four findings shaped the design.

**1. The notions form a partial order, not a list.** Almost every notion is derivable from one
scheme: take the predicted distribution, push it through a summary function, and require the summary
to be calibrated. Confidence calibration uses `max`, class-wise uses coordinate `k`, group-wise uses
a group indicator, decision calibration uses a Bayes action, canonical uses the identity. Widmann et
al.'s F-calibration makes this explicit and the regression notions fall out of the same frame. So
the tool has a **hierarchy view** with implication arrows, not just a flat list — the ordering is the
most useful thing a practitioner can learn.

**2. The name collisions are the real problem.** "Strong calibration" means canonical calibration in
ML and covariate-conditional calibration in clinical epidemiology — different by a whole level of the
hierarchy. "Distribution calibration" means Kull & Flach's multiclass notion or Song et al.'s
regression notion. Quantile calibration is also called probabilistic, average, or just "calibration".
Conformal papers call their held-out split the "calibration set". Every entry therefore carries an
explicit `aka` list, and search covers aliases.

**3. Practitioners pick the notion the tooling implies, not the one their task needs.** ECE is
reported by default, and it estimates the weakest interesting notion. So the tool is organised
around the question the card asked — *which notion suits my task* — via a **guided picker** keyed on
model output type, downstream decision, subgroup requirements, and validation-set size. Sample size
is the constraint people underweight most, so it is a first-class question.

**4. Notion, estimator, and fix are three different choices, and the field conflates them.** Each
entry is typed as notion / metric / method and cross-linked in both directions, so from
"class-wise calibration" you reach class-wise ECE and Dirichlet calibration in one click, and from
"temperature scaling" you see it only targets the top-label notion.

## What is in the tool

- Search across names, aliases, definitions, pitfalls, authors, and library names
- Filters by type and setting (binary, multiclass, regression, forecasting, LLM, detection, fairness)
- Per-entry: formal definition, plain-language reading, "reach for it when", "where it bites",
  implication links, copy-paste snippets for netcal / torchmetrics / torch-uncertainty /
  uncertainty-toolbox / relplot / scikit-learn / MAPIE / dirichletcal / mcboost / R's
  CalibrationCurves, and primary references
- Hierarchy view for classification and for regression
- Guided picker producing a ranked shortlist with the matching metric and fix
- Deep links (`#canonical`), `/` to search, keyboard and reduced-motion support

## Known gaps, in rough priority order

1. **Snippets are indicative, not tested.** They were written from documentation, not run against
   pinned versions. The obvious next step is a CI job that executes each snippet on toy data — that
   alone would make the tool more trustworthy than any survey paper.
2. **Structured prediction is thin.** Segmentation, tracking, ranking/CTR, and survival analysis each
   have their own calibration literature that is not yet represented.
3. **No empirical layer.** The highest-value extension: a small benchmark that runs every metric on
   the same set of predictions, so users can see that ECE with 10 bins and smECE disagree, and by how
   much, rather than being told so.
4. **Strength ordering is partly editorial.** Cross-domain comparisons (is multicalibration stronger
   than class-wise calibration?) are not always well-defined. The pips are a reading aid; the
   implication arrows are the defensible claim.
5. **LLM section will date fastest.** Semantic and verbalized calibration are moving quickly.

## On the blank in "experience or interest in XXX"

Based on what the build actually needed, the missing skill is not more calibration theory — Novin's
team has that. Three candidate fillings, in order of how much they would improve the result:

- **Empirical evaluation / benchmarking**, someone who will build gap 3 above. This converts the
  project from a curated list into something citable.
- **Frontend or interaction design**, if the goal is adoption. The taxonomy is the hard part; whether
  anyone uses it depends on the picker feeling faster than asking a colleague.
- **Library maintenance / open-source packaging**, someone who will keep the snippets green and turn
  the JSON into a package other tools can import.

A fourth framing worth considering: pitch it as a **living resource with a submission path**
(one JSON entry per pull request) rather than a static tool. That changes the ideal third member
toward community maintenance and makes the artifact durable past the event.
