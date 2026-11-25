# How to publish this pack to GitHub

You do not need to build any of the files — they are already written and the notebook has been
executed with real outputs. Pick one of the two routes below.

## Route A — Web upload (5 minutes, no Git installed)

1. Go to github.com → **New repository**
   - Name: `cadetx-warehouse-analytics`
   - Visibility: **Public** (portfolio) or Private + invite your teammates
   - Do **not** add a README (this pack already has one)
2. On the empty repo page click **uploading an existing file**
3. Drag the **contents** of this folder (not the zip itself) into the browser:
   `README.md`, `.gitignore`, `requirements.txt`, `data/`, `week-01/`
4. Commit message: `Sprint 1: data discovery, ERD, quality audit, KPI frame`
5. Your submission link is then:

   ```
   https://github.com/<your-username>/cadetx-warehouse-analytics
   ```

6. Paste that link into the CadetX portal → Submission area.

> `.gitignore` already excludes `data/*.csv`, so a ~40 MB dataset never enters the repo.
> Reviewers get `data/README.md` with the download link and row counts instead.

## Route B — Git CLI (recommended once, easier every week after)

```bash
cd sprint-01-pack
git init
git add .
git commit -m "Sprint 1: data discovery, ERD, quality audit, KPI frame"
git branch -M main
git remote add origin https://github.com/<your-username>/cadetx-warehouse-analytics.git
git push -u origin main
```

Behind GitHub's new repo page you can also just run the two commands it shows after creating an
empty repository.

## Adding teammates

Repository → Settings → Collaborators → Add both teammates by GitHub username.
Each member commits under their own account so individual contribution stays visible in the
contribution graph — this matters for the certificate assessment.

## Before you submit — 30-second checklist

- [ ] Repo is Public (or the reviewers have been invited)
- [ ] `README.md` renders, team names filled in
- [ ] `week-01/sprint-notes.md` has dates and member names filled in
- [ ] Notebook opens with outputs visible (no "Run all" prompt needed)
- [ ] `data/*.csv` is **not** in the repository
- [ ] Link pasted into the CadetX Submission area

## Placeholders to fill before committing

Search the pack for `_[name]_` and `_[start]_` / `_[end]_`:

```bash
grep -rn "_\[" README.md week-01/sprint-notes.md week-01/notebooks/data_exploration.ipynb
```

Only these identity fields are placeholders — all analysis, numbers and findings are real and
computed from the dataset.

## Week 2 onwards

```bash
mkdir -p week-02/notebooks week-02/docs
# work, then
git add week-02
git commit -m "Sprint 2: cleaning rules + integrated fact table"
git push
```
