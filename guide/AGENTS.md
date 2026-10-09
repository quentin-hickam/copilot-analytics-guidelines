# Analytics workbench

Keep inputs and outputs tidy, make every number traceable to a query, and let the user shape the story.

## Layout

- `data/raw/<source>/<YYYY-MM-DD>/`: originals exactly as received, untouched once written. They live outside Git, so `README.md` records where a backup copy is kept.
- `sql/views/`: shared DuckDB views over raw data for cleaning, joins, and corrections. One view per file, with a top comment saying what it represents. A view names the acquisition folders it reads; adding new data means editing the view, so the change shows in Git.
- `investigations/<question>/`: `brief.md`, `state.md`, `queries/` with one saved query or script per result, and `out/` for generated figures and tables, which can always be rebuilt.
- `deliveries/<question>/<YYYY-MM-DD>/`: exactly what the user received, copied once and then left as is.

## Working

- Compute in DuckDB: shared logic in views, each result in a saved query. Rerun queries instead of writing intermediate files; everything in `out/` regenerates from one.
- Every number in `state.md` or a deliverable names the query that produces it.
- Before trusting a result, check row counts, join fan-out, nulls, and that the population matches the brief.
- Fix data problems in `sql/views/` so every investigation gets the fix, and mark the findings it touches `recheck: <reason>`.
- Commit when the user asks. A commit that drops or reverses a finding says why.

## Records

- `brief.md`: the question, the audience, and the decision it informs.
- `state.md`: the whiteboard. Current findings (`solid`, `tentative`, `recheck: <reason>`) with their queries, up to five open questions, and next steps. Overwrite freely; Git keeps the past.

## Check in with the user

- At the start: build the brief one question at a time, offering a suggested answer each time, and confirm it before analysis.
- When a result changes the answer: say so, and ask whether it changes what matters.
- Before writing a deliverable: propose the cut. That is the 3–5 findings that bear on the decision, what is left out, and the caveats that would change the reader's decision or trust. Write once the user agrees.

## Deliverable

`findings.md` for the brief's audience, in their terms: the answer, the key findings with their numbers and figures, what they mean for the decision, and the uncertainties that matter. Figure titles state the finding, labels use business terms, and captions live in the document. Datasets are exported by queries. Copy everything to `deliveries/` when the user says it is final.
