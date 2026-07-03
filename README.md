# quantumnovelty.github.io

Static landing page + examples mirror for the
[QuantumNovelty](https://github.com/) framework.

This repo is published as a GitHub Pages site at
`https://<owner>.github.io/quantumnovelty.github.io/` (or whatever the
owner chooses via the GitHub Pages settings).

## What's here

- `index.html` — landing page (Hero / Workflows including scout, paper audit, patent audit, patent draft / Skills / How it works / Examples / vs ARC / Quick start)
- `assets/style.css` — bright-theme stylesheet
- `examples/index.html` — examples gallery and workflow command samples
- `examples/two_paper_novelty/` — published copies of the two-paper analysis:
  - `PIPELINE_REPORT.pdf` — token + USD cost ledger
  - `PIPELINE_REPORT.json` — machine-readable summary
  - `FINDINGS_SUMMARY.md` — hand-annotated takeaways
  - `reports/flowvqe/`, `reports/lcutrotter/` — every stage's output
- `examples/avenues/{01_qec,02_qml,03_qaoa}/` — three scaffolded-paper drafts

## Current feature surface mirrored on the site

- 25 QN skills, including `quantum_scout`, `quantum_kb`, `patent_prior_art`,
  `evidence_ledger`, and `requirements_judge`.
- `chain/run.sh --pipeline scout` as the preferred broad discovery path:
  live literature, bounded arXiv PDF acquisition, optional source-file KB,
  exact quote substantiation, claim ledger, references, and scout manifest.
- Local file-backed `quantum_kb` workflows for search, substantiation,
  grounded review, and Emma-style perspective reports without a server.
- A prominent RAG/KB section that separates QN's portable local KB from the
  ScienceSkills BGE-M3/Regiment path for PDF KBs and per-idea substantiation
  KBs.
- Patent workflows for `patent-audit`, `patent-draft`, and standalone
  patent-prior-art search that can feed `patent_reviewer --prior-art`.
- LLM backend options: Claude Code CLI default, Codex CLI, Kimi/Moonshot,
  and explicit Anthropic API.

## Local preview

```bash
cd quantumnovelty.github.io/
python3 -m http.server 8000
# open http://localhost:8000
```

## Provenance

Built from the QuantumNovelty repo at
`DSS/artifacts/code/QuantumNovelty/`. The example artefacts are exact
copies of the per-stage outputs from real runs of QuantumNovelty's
chain against the arXiv papers above. Token counts, USD costs, model
snapshot IDs, and elapsed times come directly from each call's
`_backend_used.json` provenance marker.
