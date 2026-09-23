# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/61

**Verdict output**

Candidates graded: #61, #62, #69 (codepath/pathreview-ai301-fa26-s1)

Accepted:

1. #61 — Health check DB probe passes a raw SQL string, which fails under SQLAlchemy 2.x — bounded one-file bug, concrete expected fix, zero comments and no assignee so it is genuinely unclaimed.

Rejected:

- #62 — Health check references `settings.redis_host`, which does not exist on Settings — failed: Unclaimed and available (a CodePath TF commented "I'd like to take on this issue as part of the TF weekly prep task" the day before capture, followed by a same-day repro comment; that is an active, ongoing claim, not a stale one).
- #69 — Output parser crashes on a top-level JSON array fallback — failed: Unclaimed and available (contributor `jacho15` commented "Hi, I'd like to attempt this!" three days before capture, with a same-day follow-up repro comment; also an active claim).

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/61",
    "checks": [
      {"name": "Repo active", "grade": "pass", "evidence": "Default branch pushed 6 days before capture (2026-09-16); repo not archived."},
      {"name": "Bounded work", "grade": "pass", "evidence": "Single bug in api/routes/health.py: the DB probe passes a raw SQL string instead of wrapping it in sqlalchemy.text()."},
      {"name": "Specific expected behavior", "grade": "pass", "evidence": "Names the exact failure (ArgumentError under SQLAlchemy 2.x) and gives a concrete repro: call GET /health and observe the DB check fail."},
      {"name": "Unclaimed and available", "grade": "pass", "evidence": "0 comments, no assignee, no linked PR."},
      {"name": "Policy compatible", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no ban on AI-assisted contributions."},
      {"name": "Good first issue signal", "grade": "pass", "evidence": "Labeled bug, good first issue, api, tier-1."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62",
    "checks": [
      {"name": "Repo active", "grade": "pass", "evidence": "Same repo, pushed 2026-09-16."},
      {"name": "Bounded work", "grade": "pass", "evidence": "Single bug in api/routes/health.py: probe reads settings.redis_host/redis_port instead of settings.redis_url."},
      {"name": "Specific expected behavior", "grade": "pass", "evidence": "Names the AttributeError and the concrete repro: GET /health with Redis up still reports unhealthy."},
      {"name": "Unclaimed and available", "grade": "fail", "evidence": "TF commented 2026-09-21: \"I'd like to take on this issue as part of the TF weekly prep task,\" then reproduced it the same day — active claim as of the day before capture."},
      {"name": "Policy compatible", "grade": "pass", "evidence": "No repo ban on this workflow."},
      {"name": "Good first issue signal", "grade": "pass", "evidence": "Labeled good first issue, tier-1."}
    ],
    "verdict": "reject"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69",
    "checks": [
      {"name": "Repo active", "grade": "pass", "evidence": "Same repo, pushed 2026-09-16."},
      {"name": "Bounded work", "grade": "pass", "evidence": "Single bug across output_parser.py and its test: top-level JSON array reaches .items() and crashes."},
      {"name": "Specific expected behavior", "grade": "pass", "evidence": "Names the exact exception, the two relevant files, and the xfail marker (manifest id H-02) to remove."},
      {"name": "Unclaimed and available", "grade": "fail", "evidence": "Contributor jacho15 commented 2026-09-19: \"Hi, I'd like to attempt this!\" with a same-day repro follow-up — active claim."},
      {"name": "Policy compatible", "grade": "pass", "evidence": "No repo ban on this workflow."},
      {"name": "Good first issue signal", "grade": "pass", "evidence": "Labeled good first issue, tier-1."}
    ],
    "verdict": "reject"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

agreement: 17/20 scored items  (bar: 18/20: below the bar)
agreement: 19/20 scored items  (bar: 18/20: PASS)
agreement: 19/20 scored items  (bar: 18/20: PASS)
agreement: 20/20 scored items  (bar: 18/20: PASS)

**Issue analysis**

Issue id: `issue-19`
Rubric decision: `accept`
Gold label: `accept`
Reasoning: This issue is a maintainer-filed performance bug ("Selecting large subgraphs in proof mode freezes the UI") that names two concrete root causes (slow matchers, UI blocked on the matching thread) plus three follow-on implementation ideas (multi-processing, skipping collapsed categories, threading the rewrite application). My first rubric revision rejected this on Bounded work, treating the five bullet points as scope creep. The gold note calls it a "maintainer-diagnosed performance bug with named causes" — one narrow user-facing symptom (the freeze) inside one subsystem (proof-mode rewrite matching), which is exactly what Bounded work is supposed to allow. I fixed the rubric to count subsystems touched rather than bullet points listed, after which it consistently accepted this issue.

**Check rationale**

From `tools/issue-select/rubric.md`:

`| Repo active | repo-facts block: default-branch commits, last push, latest release, archived flag | Pass if the repo is not archived and its last default-branch push or its latest release falls within roughly the last 6 months before the capture date. Compute the actual gap between the capture date and the last push/release rather than eyeballing it; a repo with high stars or many open issues but no push or release in the last 6+ months still fails this check | required |`

I rewrote this check mid-session because its original wording ("recent" with no threshold) let the grading model eyeball dates instead of computing the gap, and it flip-flopped on borderline repos. Giving it an explicit ~6-month window and telling it to compute the gap turns "activity" from a vibe into an arithmetic check, which is what a hard, required gate needs to be consistent.

**Trade-offs**

This check gives up repos with a slow-but-still-real release cadence: a project that pushes every 7-8 months with an engaged maintainer team would still fail this check even if it would happily review a first PR. I confirmed the fix's effect with a canary: before adding the explicit threshold, re-running `--only issue-02` (rupa/z, last push 2024-06-19, capture 2026-08-05 — over two years stale) three times in a row split roughly evenly between accept and reject on the same rubric text. After adding the "roughly 6 months" threshold and the instruction to compute the gap, four consecutive `--only issue-02` reruns all rejected it correctly. The trade-off is real (some slow-but-healthy repos get filtered out), but it removes the coin-flip on the clearly-dead ones, which required checks should not have.

---

## Selection rationale

**Selection rationale**

1. This issue fits my interests and the available time because it is a small backend bug in a Python service (FastAPI health-check route) with a direct, one-file fix and a clear expectation. It is narrow enough to understand and verify in one sitting, without a large design discussion or a multi-file rewrite. Between the three candidates I graded, #61 and #62 were nearly identical in shape (both are bugs in the same `api/routes/health.py` file), so the deciding factor was availability rather than difficulty.
2. The verdict got the main signal right on all three: #61 is unclaimed with zero comments, while #62 and #69 both looked equally bounded and well-specified on paper but were already being actively worked by a real person (a TF and a contributor, respectively, both commenting within days of my capture). The rubric's job was to catch that claim signal, which it did; what I had to weigh beyond the rubric was that #62's claimant was explicitly doing course-prep work, which made it feel even less available in practice than a typical claim comment, even though the rubric treats both the same way.
3. The anticipated difficulty is low-to-moderate: the fix is a one-line change (wrap the literal SQL string in `sqlalchemy.text()`), but confirming it is correct means understanding why SQLAlchemy 2.x requires this and checking the health-check test actually exercises a live DB session rather than a mock. That makes it realistic to claim and finish quickly, but not a pure copy-paste fix.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
