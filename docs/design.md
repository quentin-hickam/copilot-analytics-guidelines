# Design

## Goal

A guide that GitHub Copilot follows while doing analysis, not a code suite. It does three things:

- keeps the agent's inputs and outputs in a predictable layout;
- checks in with the user on the parts of the narrative that matter;
- keeps results traceable by computing in saved DuckDB queries over raw data that never changes, rather than writing files along the way.

The whole guide is [`guide/AGENTS.md`](../guide/AGENTS.md), loaded on every request, so its size is a cost paid on every model call.

## Origin

This started as a redesign of [analytics-workbench](https://github.com/quentin-hickam/analytics-workbench), which stays as it is. By v0.3.0 the workbench had grown to five skills and seven Python helpers, about 24,700 words. A v0.2.0 workbench was costing close to $2 per request on the strongest models available in Copilot.

Most of that cost came from mechanics written as prose and run by hand, one tool call at a time: landing, checksums, manifests, inventories, verification passes, and status collection. Records also kept everything forever and fed it into the deliverable by rule.

Reviews of each skill found the same duplicated rules three to six times across files. Fixing those would have trimmed the workbench without changing what drives its cost.

## Principles

### What gets kept, and for whom

| Tier | Holds | Lifetime | Read by |
|---|---|---|---|
| Scratch | Exploration, failed attempts, chat | Disposable | Nobody later |
| Working state (`state.md`) | Current beliefs, open questions, next steps | Overwritten freely; one screen at most | The agent on every resume |
| Archive | Git history, saved queries and views, raw data, frozen deliveries | Forever; nobody maintains it | Anyone asking how a number was produced |

Completeness belongs to the archive, which Git and immutable raw data keep for free. Everything a person or agent reads gets curated. The guide has no history file, finding ledger, or revalidation disposition records.

### Traceability through queries

A number is traceable when it comes from a saved query or script, which reads views in Git, which read raw folders that never change. Rerunning the file at that commit reproduces the number. This replaces evidence files, checksums, and manifests.

The one gotcha is that a view must name the acquisition folders it reads. A view that globs every folder changes its results when new data lands, and nothing records the change.

### The user shapes the story

There are three check-ins:

1. the brief, at the start;
2. any result that changes the answer;
3. the proposed cut before a deliverable is written.

The cut proposes 3–5 findings that bear on the decision, says what is left out, and lists only the caveats that would change the reader's decision or trust. One human judgement replaces rules that tried to encode what matters.

### A guide, not code

The guide ships no helpers. DuckDB and Git do the mechanics, and the agent already knows how to use them. A rule earns a line only when it changes the agent's behaviour from its default or names a gotcha the agent cannot discover by looking.

## Deliberately absent

These are absent compared with the workbench:

- the scoping interview procedure, status report, and next-request menu;
- result evidence files, delivery manifests, file inventories, and their checks;
- Parquet publication as a required step; add Parquet when a view is slow;
- foundation catalog, quality, source, and glossary records;
- investigation history, release numbering, and per-place revalidation dispositions;
- the automated identifier scan on the deliverable and the figure style module;
- portability notes for hosts other than Copilot.

## Open questions

1. **M365 assembly.** Decide whether a one-paragraph note in each delivery folder is enough for M365 Copilot to build the documents, or whether a template is needed.
2. **Figure style.** Decide whether to keep a small style module as the one piece of code, or keep figure rules as text.
3. **Commands or one file.** Decide whether starting an investigation and delivering stay in `AGENTS.md`, or become prompt files or user-invoked skills that cost nothing until typed.
4. **Migration.** Work out how a v0.2.0 workbench moves over: which records carry across as they are, and what happens to its existing releases.

## Validation plan

1. Measure one real request on a copy of the v0.2.0 workbench: number of model calls, tokens, and cost.
2. Migrate that copy to this guide and repeat the same request.
3. Compare the cost, and whether the deliverable kept what the user needed.
