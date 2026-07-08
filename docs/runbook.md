# Runbook: Reasoning Consistency Flips

## Purpose

This repo preserves a reproducible AI safety pilot testing whether models change correct answers under social pressure, and whether any changes would be faithful to their stated reasoning.

## Current Status

- Status: complete research pilot
- Primary artifact: `WRITEUP.md`
- Reproducible analysis: `python3 analyze.py`
- Public dashboard source: `web/`
- Production deployment: Vercel deployment is referenced in the README, but the public deployment URL is not recorded in repository metadata
- Last verified locally: 2026-07-08

## System Overview

- `data/problems.json`: machine-checkable problems and wrong counter-arguments
- `data/raw/trials.json`: structured trial record
- `data/raw/*.txt`: representative transcript examples
- `analyze.py`: computes flip rates and confidence intervals
- `data/processed/results.json`: processed output
- `web/results.json`: dashboard data source
- `web/index.html`: static dashboard

No secrets or live API calls are required to reproduce the committed analysis. The model calls used for data collection were performed before publication and are preserved as raw records.

## Local Development

```bash
python3 analyze.py
```

Optional isolated setup:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python3 analyze.py
```

`requirements.txt` is intentionally minimal; the analysis uses the Python standard library.

## Verification

Run:

```bash
python3 analyze.py
git diff -- data/processed/results.json web/results.json
```

Expected result: no diff after regeneration.

## Deployment

This is a static dashboard. Any static host can serve the `web/` directory.

Vercel deployment checklist:

1. Set the project root to the repository root.
2. Set output/static directory to `web` if the host asks for one.
3. There are no required environment variables.
4. After deploying, add the production URL to the GitHub repository homepage.

Rollback is host-level: redeploy a previous static deployment or revert the commit that changed `web/`.

## Maintenance Checklist

- Re-run `python3 analyze.py` before any release or README result update.
- Keep machine-checkable ground truth separate from prompt-pressure text.
- Do not edit processed JSON by hand; regenerate it from raw data.
- If the taxonomy changes, update `WRITEUP.md` before updating the dashboard.
- Keep limitations honest about sample size and single-model-family coverage.

## Handoff Notes

The most important owner responsibility is preserving the faithfulness distinction. If future trials produce flips, classify them using the documented rubric before changing any headline claims.
