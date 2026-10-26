# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

**Community:** r/ValueInvesting on Reddit, specifically threads discussing
micro-cap, small-cap, and deeply discounted or underappreciated value stocks.

The classifier sorts each post into one of three labels:
`fundamental_analysis` (backed by real financial figures), `speculative_hype`
(price targets and promotion without balance sheet data), or
`investor_inquiry` (questions to the community about method, allocation, or
tools).

**Candidate distinctions observed while reading:**

1. **Concrete accounting metrics vs. pure narrative claims.** Some posts present
   verifiable accounting metrics, financial ratio calculations (P/E, P/B,
   EV/EBITDA, Net Debt/EBITDA), or cash-flow figures taken from SEC filings.
   Others present purely qualitative growth narratives, short squeeze
   assertions, or target price predictions with no balance sheet figures.
2. **Interrogative / procedural syntax vs. thesis declarations.** A subset of
   posts doesn't advocate for or analyze a specific stock at all. Instead they
   ask the community about valuation methodology (DCF model inputs, handling
   stock-based compensation), portfolio allocation limits, or screening tools.
3. **Promotional hype language vs. objective financial tone.** Speculative posts
   lean on hype words, exclamation points, and forward-looking hyperbole ("10x
   potential", "to the moon", "guaranteed 5x play", "multi-bagger"), while
   analytical posts keep a formal, financial-reporting tone.
4. **Short-term catalyst speculation vs. margin of safety / asset backing.**
   Speculative content focuses on upcoming short-term events (FDA approvals,
   earnings surprises, low-float short squeezes), whereas fundamental analyses
   focus on liquidation value, net cash per share, margin of safety, or steady
   free cash flow yields.


---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `fundamental_analysis`

**Definition:** A post that backs its view of a stock with concrete accounting
metrics, financial ratios, valuation models, or cash-flow figures.

**Example 1:**
> HGBL trades at 0.65x Price to Tangible Book Value with shareholder equity
> around $65M. Net cash sits at $14M and Enterprise Value is under $30M,
> providing a massive margin of safety despite credit losses.

**Example 2:**
> Shift4 ($FOUR) is currently trading at an 8.6 forward P/E despite generating
> over $250M in free cash flow annually and posting 21% YoY revenue growth.
> Enterprise value to EBITDA is just 6.2x.

### `speculative_hype`

**Definition:** A post that makes price target claims, short squeeze
assertions, or promotional predictions without any quantitative balance sheet
data to support them.

**Example 1:**
> ASTS is going to be a multi-bagger! Tripled digit share price coming in 2030
> as space-cellular technology dominates global Telecom. Buy and hold now!

**Example 2:**
> LPSN short squeeze incoming! Short interest is huge on tiny free float. Once
> news hits this stock is rocketing back to all-time highs!

### `investor_inquiry`

**Definition:** A post that asks the community for guidance on valuation
methodology, portfolio allocation, or screening tools rather than arguing for
a specific stock.

**Example 1:**
> When doing undervalued stock analysis, what are your golden steps? Do you
> look at DCF models or focus primarily on Price to Book ratios?

**Example 2:**
> How do you evaluate high Debt/Equity ratios in cyclical sectors like
> homebuilding or shipping? What metrics indicate a value trap?

### The hardest boundary

**Which two labels:** `fundamental_analysis` and `speculative_hype`

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->

A number only counts as support if it is an accounting figure or ratio taken
from the company's financials (P/E, EV/EBITDA, book value, free cash flow,
debt, margins). Predicted returns and price targets ("10x", "200% gain",
"turn $1k into $50k") don't count. So a post whose only numbers are projected
gains is `speculative_hype`, and a post that cites real financial figures is
`fundamental_analysis` even if its tone is enthusiastic ("massive margin of
safety").

A secondary rule for `investor_inquiry`: if the post's main purpose is a
question to the community, it's `investor_inquiry` even when it quotes a ratio
("Can someone explain why a company would trade at 0.5x Book Value...?").


---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** r/ValueInvesting on Reddit. I collected posts by
manually browsing, searching, and copying public text from subreddit threads
discussing small-cap, micro-cap, and value-oriented equities. Whole posts and
comments were kept intact, without shortening or trimming, to preserve the
community's natural style and context. Nothing came from behind a login wall or
a private, invite-only group.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
Cold first: 60 posts (the first 20 of each label in `labels.csv`, data rows
1–20, 86–105 and 151–170) were labelled entirely by hand using only my written
definitions, with no AI or other tools, and are marked `cold`. The remaining
145 were pre-labelled in batches by an AI assistant (Claude) given my exact
label definitions and decision rule, then I read every one and corrected the
label where needed (about 15% of them). Those rows are marked `pre-labeled`.
See How I Used AI.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `fundamental_analysis` | 85 | 41.5% |
| `speculative_hype` | 65 | 31.7% |
| `investor_inquiry` | 55 | 26.8% |
| **Total** | 205 | 100% |

No label is above 70%. 60 rows (20 per label) are marked `cold`; the other 145
are marked `pre-labeled`.

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* (data row 1) "HGBL trades at 0.65x Price to Tangible Book Value
> with shareholder equity around $65M. Net cash sits at $14M and Enterprise
> Value is under $30M, providing a massive margin of safety despite credit
> losses."
>
> *Could have been:* `speculative_hype` or `fundamental_analysis`
>
> *I chose, because:* `fundamental_analysis`. "Massive margin of safety" is
> enthusiastic, bullish phrasing, but the post is built on concrete accounting
> figures ($65M equity, $14M net cash, 0.65x P/TBV), which is exactly what my
> decision rule says decides it.

**2.**
> *The post:* (data row 162) "Can someone explain why a company would trade at
> 0.5x Book Value if it generates positive operating cash flow?"
>
> *Could have been:* `investor_inquiry` or `fundamental_analysis`
>
> *I chose, because:* `investor_inquiry`. It quotes real metrics (0.5x Book
> Value, operating cash flow), but it is a question asking the community for an
> explanation rather than a thesis about a stock, so my secondary rule puts it
> in `investor_inquiry`.

**3.**
> *The post:* (data row 92) "Penny stock alert: Huge catalyst loading next
> week! Insider buying is crazy and this will gap up 200% on Monday open. Load
> up!"
>
> *Could have been:* `speculative_hype` or `fundamental_analysis`
>
> *I chose, because:* `speculative_hype`. It cites a number (200%) and mentions
> insider buying, but the number is a forward-looking price-surge prediction,
> not a verifiable balance sheet or financial statement figure, so my decision
> rule doesn't count it as support.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** <!-- epochs, learning rate, batch size, seed -->
3 epochs, learning rate 2e-5, batch size 16, max length 128 tokens, seed 42.

**Anything I changed from the defaults, and why:** No hyperparameters were
changed; this is the default run. The one environment change: my first
attempt failed in section 4 (`TypeError: Pickler._batch_setitems()`) because
my virtual environment used Python 3.14, which the pinned packages don't
support. I rebuilt `.venv` with Python 3.13 and `python test.py` passed before
training.

**Device:** `cpu` (torch `2.14.1+cpu`), as recorded in `results.json`.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->

| Label | Train | Val | Test |
|---|---|---|---|
| `fundamental_analysis` | 59 | 13 | 13 |
| `speculative_hype` | 45 | 10 | 10 |
| `investor_inquiry` | 39 | 8 | 8 |
| **Total** | 143 | 31 | 31 |

`investor_inquiry` has only 8 posts in the test split, right at the edge where
one misclassification moves its F1 noticeably, so expect its score to be the
jumpiest across seeds.


---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1 — stress-testing the taxonomy boundary**

- *What I asked for:* I gave the AI my initial label definitions and ambiguous
  borderline posts and asked it to make the strongest case for each of the two
  competing labels, without rewriting my definitions.
- *What came back:* It surfaced how much promotional hype and valuation metrics
  overlap, since hype posts also contain numbers ("10x", "200% gain").
- *What I changed:* That prompted the strict decision rule in Label Taxonomy:
  only an accounting figure from the company's financials counts as support, so
  a post with real financial figures is `fundamental_analysis` even if its tone
  is enthusiastic.

**Moment 2 — batch pre-labelling**

- *What I asked for:* I gave the AI my label rules alongside batches of
  r/ValueInvesting posts and asked for suggested labels and note markers.
- *What came back:* Suggested labels for the 145 non-cold posts.
- *What I changed:* I reviewed every suggestion and corrected roughly 15% of
  them, mostly posts where emotional tone had been allowed to outweigh the
  financial metrics actually in the post.

**Moment 3 — catching duplicated rows**

- *What I asked for:* I asked Claude Code to check `labels.csv` against the
  Milestone 4 requirements before committing it.
- *What came back:* It found that 145 of my 205 rows were really 35 posts
  repeated four or five times, each copy hidden behind a numbered prefix like
  `[1]`, `[16]`, `[31]`. Only 95 rows were distinct, below the 150 floor, and
  the notebook's exact-duplicate check would have missed them because of the
  prefixes. The random split would then have put copies of the same post in
  both train and test, inflating every score.
- *What I changed:* I removed the repeated rows and replaced them with distinct
  posts, bringing the file back to 205 rows with zero duplicates (checked even
  after ignoring case and punctuation) before committing it.

**Moment 4 — fixing the training crash**

- *What I asked for:* Section 4 of the notebook crashed with
  `TypeError: Pickler._batch_setitems() takes 2 positional arguments but 3 were
  given`, and I asked Claude Code to get training working.
- *What came back:* It traced the error to my virtual environment running
  Python 3.14, which `requirements.txt` and `test.py` both say the pinned
  packages don't support, and pointed out `test.py` was already failing on
  exactly that.
- *What I changed:* I rebuilt `.venv` with Python 3.13, reinstalled the
  requirements, confirmed `python test.py` passed, and then ran sections 1–5
  with the default settings.

**Other AI help, disclosed:** Claude Code drafted the wording of my label
definitions and the decision rule in Label Taxonomy from my own dataset summary,
and derived the rule from how my existing labels already split the
`fundamental_analysis` / `speculative_hype` boundary. It also checked my five
acceptance criteria for testability and verified the facts my reasons cite
against `labels.csv`; the criteria and their numbers are my own.

**Pre-labelling disclosure:** The 60 rows marked `cold` (20 per label) I
labelled by hand with no tool. The other 145 rows, marked `pre-labeled`, were
pre-labelled in batches by Claude using my label definitions and decision rule.
I read every one and corrected about 15% of the suggested labels before
finalizing `labels.csv`.

**Unit 6 — what Claude Code did, disclosed:**

- *Ran the experiments:* it ran `baseline.py` (with my definitions, with bare
  label names, and with `--trained`), and it ran the notebook's sections 6 and 7
  for the before and after three-seed runs. It used the notebook's own code
  and skipped the single-run sections 4–5, so `results.json` and
  `test_split.csv` from unit 5 were not overwritten.
- *Spotted patterns in my errors and data:* it checked whether my perfect
  score came from leakage (no test post is more than 80% similar to a training
  post on any seed) and pointed out that the model was right but only
  0.56–0.59 confident. That finding became my improvement. It also caught that
  "58 of 65 hype posts contain numbers" counts ticker `$` signs; the true digit
  count is 49.
- *Drafted write-up text:* it filled in the run logs, verdicts and the
  confusion matrix from the results files, and drafted the diagnosis and
  improvement paragraphs. I chose which target was too low (accuracy, tightened
  to ≥ 0.95) and which improvement to make (more epochs). My prediction is my
  own sentence; Claude Code only tightened the wording and filled in the
  verified count.
- *What it did not do:* it did not label the 30 staff posts or open the staff
  answer key before my labels were saved. The adjudication calls in the
  Agreement Report are mine.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Stretch Features — declared before building

I'm attempting two stretch items. This declaration is committed before either
one is built:

1. **Confidence check.** Using the `confidence` block the notebook writes for
   each seed, I'll report whether the model's most-confident third of
   predictions is right more often than its least-confident third, with the
   numbers for both runs. The per-seed confidence values were already in the
   results files from section 6; the comparison and write-up are what I'm
   adding.
2. **A second measured improvement.** One more training change aimed at the
   same diagnosed weakness (right but unsure): learning rate 2e-5 → 5e-5,
   keeping 6 epochs and everything else, run across the same three seeds with
   `RUN_LABEL = "after2"` and logged as a third run log in the same format.

I am not attempting the per-label error write-up: my confusion matrix has
zero errors on every seed, so there is no worst label to analyse.

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.645 | 1.000 | +0.355 |
| Macro F1 | 0.607 | 1.000 | +0.393 |
| F1 — `fundamental_analysis` | 0.703 | 1.000 | +0.297 |
| F1 — `investor_inquiry` | 0.545 | 1.000 | +0.455 |
| F1 — `speculative_hype` | 0.571 | 1.000 | +0.429 |

Both columns are scored on the same 31 held-out posts in `test_split.csv`
(13 / 8 / 10). The baseline is `facebook/bart-large-mnli` zero-shot, prompted
with the definitions in `label_definitions.txt` (definitions only, not the
decision rule).

**What the definitions were worth:** the same baseline run with bare label
names instead (`baseline_results_bare_names.json`) scored 0.419 accuracy and
0.379 macro F1, with F1 0.000 on `fundamental_analysis`, 0.471 on
`investor_inquiry` and 0.667 on `speculative_hype`. The definitions added
+0.226 accuracy and +0.228 macro F1. The baseline reported above is the
definitions run.

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
The trained model will beat the baseline most on `speculative_hype`. 49 of my
65 hype posts contain numbers like "10x" or "2030", and a model reading only my
definitions could mistake those for financial metrics and call them
`fundamental_analysis`. `investor_inquiry` should be about even, since every
one of those posts asks a question.

*Note:* I wrote this after seeing the Baseline vs. Trained table, so it isn't a
true before-the-fact prediction. It was also wrong on `investor_inquiry`: that
label had the biggest gap (+0.455), not a tie. `speculative_hype` was close
behind at +0.429.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.80 | 1.000 | 1.000 | 1.000 | MET |
| 2. F1 — `investor_inquiry` | ≥ 0.80 | 1.000 | 1.000 | 1.000 | MET |
| 3. `speculative_hype` predicted as `fundamental_analysis` | ≤ 0.20 of the hype row | 0.00 (0/10) | 0.00 (0/10) | 0.00 (0/10) | MET |
| 4. Predicted vs. true `fundamental_analysis` share | differ by ≤ 10 pts | 0 pts (13/31 vs 13/31) | 0 pts (13/31 vs 13/31) | 0 pts (13/31 vs 13/31) | MET |
| 5. Agreement with staff labels | ≥ 0.70 | *pending (Milestone 3)* | | | *pending* |

**Device:** `cpu` (CPU), recorded in `results_three_seeds_before.json` for
every seed. Seeds 42, 7 and 2024; 3 epochs, learning rate 2e-5, batch size 16,
128 tokens. Criterion 5 doesn't depend on the seed: it is one agreement rate
from `agreement.py`.

Spread table from section 6:

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 1.000 | 1.000 | 1.000 | 0.000 |
| Macro F1 | 1.000 | 1.000 | 1.000 | 0.000 |
| F1 — `fundamental_analysis` | 1.000 | 1.000 | 1.000 | 0.000 |
| F1 — `speculative_hype` | 1.000 | 1.000 | 1.000 | 0.000 |
| F1 — `investor_inquiry` | 1.000 | 1.000 | 1.000 | 0.000 |

Accuracy moved 0.000 across seeds, so the result isn't a friendly draw from
one split. I also checked for leakage: on all three seeds, no test post is
more than 80% similar to any training post, so the model is not scoring well
by seeing near-copies of test posts.

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

Seed 42 (seeds 7 and 2024 are identical):

| true \ predicted | fundamental_analysis | speculative_hype | investor_inquiry |
|---|---|---|---|
| **fundamental_analysis** | 13 | 0 | 0 |
| **speculative_hype** | 0 | 10 | 0 |
| **investor_inquiry** | 0 | 0 | 8 |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->
There isn't one: every off-diagonal cell is 0 on all three seeds, so the model
confused no pair of labels in either direction, including the
`speculative_hype` → `fundamental_analysis` boundary I expected to be riskiest.



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | ≥ 0.80 | MET | 1.000 on all three seeds; the lowest seed is above 0.80. |
| 2 | F1 — `investor_inquiry` | ≥ 0.80 | MET | 1.000 on all three seeds (8 of 8 inquiry posts right each time). |
| 3 | `speculative_hype` predicted as `fundamental_analysis` | ≤ 0.20 | MET | 0 of 10 hype posts went to `fundamental_analysis` on any seed, so 0.00 every time. |
| 4 | Predicted vs. true `fundamental_analysis` share | ≤ 10 pts apart | MET | The model predicted 13 of 31 as `fundamental_analysis` on every seed, exactly the true 13, so the gap is 0 pts. |
| 5 | Agreement with staff labels | ≥ 0.70 | *pending* | Waiting on `agreement.py` (Milestone 3). |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->
**No misses on criteria 1–4.** Every target held on all three seeds with no
errors at all, so there is nothing to diagnose there. Instead, the finding is
that one of my targets was set too low.

**Target set too low: criterion 1, overall accuracy ≥ 0.80.** That target
allowed 6 wrong out of 31, and the model got 0 wrong on every seed. It also
barely asked for more than the zero-shot baseline, which already reached
0.645 from my definitions alone (see Baseline vs. Trained). My labels have
strong surface signals: every `investor_inquiry` post contains a question
mark, and `fundamental_analysis` posts cite financial figures. A model could
reach 0.80 without learning the hard `fundamental_analysis` /
`speculative_hype` boundary at all.

**Tighter target: overall accuracy ≥ 0.95** on every seed, which allows at most
1 error in 31. All three seeds would still meet it at 1.000. That tells me
these 31-post test splits are too easy to separate a good model from a
perfect one, not that the model is flawless.

**The weakness the criteria don't show: the model is right but unsure.**
Its mean confidence on posts it got right was 0.590 (seed 42), 0.556 (seed 7)
and 0.584 (seed 2024), from the `confidence` block in
`results_three_seeds_before.json`. With three labels a blind guess sits at
0.33, so the model picks the right label but only just prefers it. That
points at training, not labelling: 3 epochs over about 143 training posts is
about 27 optimiser steps at batch size 16, so the model has barely moved from
its starting point. This is the failure the improvement targets.



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:** One training setting: `EPOCHS` from 3 to 6 in section 2 of
the notebook. Everything else is the same: same `labels.csv`, same seeds (42,
7, 2024), learning rate 2e-5, batch size 16, 128 tokens, device `cpu`.
`RUN_LABEL` was set to `"after"`, so the before-numbers in
`results_three_seeds_before.json` were not overwritten.

**Which diagnosis pointed at it:** The model was right on every test post but
only 0.556–0.590 confident on average (see Diagnoses). With about 27 optimiser
steps in total, that looked like undertraining, not a labelling problem, so
the fix was more training, not relabelling. I wrote that diagnosis and
committed it (`d4fcfa3`) before the after-run finished.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.80 | 1.000 | 1.000 | 1.000 | MET |
| 2. F1 — `investor_inquiry` | ≥ 0.80 | 1.000 | 1.000 | 1.000 | MET |
| 3. `speculative_hype` predicted as `fundamental_analysis` | ≤ 0.20 of the hype row | 0.00 (0/10) | 0.00 (0/10) | 0.00 (0/10) | MET |
| 4. Predicted vs. true `fundamental_analysis` share | differ by ≤ 10 pts | 0 pts (13/31 vs 13/31) | 0 pts (13/31 vs 13/31) | 0 pts (13/31 vs 13/31) | MET |
| 5. Agreement with staff labels | ≥ 0.70 | *pending (Milestone 3)* | | | *pending* |

The confusion matrix is unchanged on all three seeds (13 / 10 / 8 on the
diagonal, 0 everywhere else). Device `cpu`, recorded in
`results_three_seeds_after.json`.

The measure the change targeted, mean confidence when right, from the
`confidence` block of each results file:

| Run | Seed 42 | Seed 7 | Seed 2024 |
|---|---|---|---|
| Before (3 epochs) | 0.590 | 0.556 | 0.584 |
| After (6 epochs) | 0.868 | 0.840 | 0.858 |
| Change | +0.278 | +0.284 | +0.274 |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->
It helped with the weakness it targeted and changed nothing the criteria
measure. Confidence on correct predictions rose by about 0.28 on every seed
(from 0.56–0.59 to 0.84–0.87), and the rise is consistent across all three
splits, so it isn't one lucky draw. The five criteria couldn't show an
improvement, because accuracy, F1 and the confusion matrix were already
perfect and stayed perfect. Doubling the epochs also cost no accuracy, so
there is no sign of overfitting on these splits. The limit is that higher
confidence on an easy test set doesn't prove the model learned my boundary;
see What's Still Broken.



### Second Improvement (stretch)

**What I changed:** One more training setting, on top of the first
improvement: learning rate 2e-5 → 5e-5, keeping 6 epochs. Same `labels.csv`,
same seeds, batch size 16, 128 tokens, device `cpu`. `RUN_LABEL = "after2"`
wrote `results_three_seeds_after2.json`, so both earlier files are untouched.

**Which diagnosis pointed at it:** The same one: right but unsure. After the
first improvement the model was still only 0.84–0.87 confident, so I tested
whether larger update steps would close the rest of that gap.

### Run Log — After 2

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.80 | 1.000 | 1.000 | 1.000 | MET |
| 2. F1 — `investor_inquiry` | ≥ 0.80 | 1.000 | 1.000 | 1.000 | MET |
| 3. `speculative_hype` predicted as `fundamental_analysis` | ≤ 0.20 of the hype row | 0.00 (0/10) | 0.00 (0/10) | 0.00 (0/10) | MET |
| 4. Predicted vs. true `fundamental_analysis` share | differ by ≤ 10 pts | 0 pts (13/31 vs 13/31) | 0 pts (13/31 vs 13/31) | 0 pts (13/31 vs 13/31) | MET |
| 5. Agreement with staff labels | ≥ 0.70 | *pending (Milestone 3)* | | | *pending* |

The confusion matrix is again unchanged on all three seeds (13 / 10 / 8 on the
diagonal, 0 everywhere else).

Mean confidence when right, across all three runs:

| Run | Seed 42 | Seed 7 | Seed 2024 |
|---|---|---|---|
| Before (3 epochs, lr 2e-5) | 0.590 | 0.556 | 0.584 |
| After (6 epochs, lr 2e-5) | 0.868 | 0.840 | 0.858 |
| After 2 (6 epochs, lr 5e-5) | 0.985 | 0.980 | 0.983 |

**Did it help, and how do I know:** It helped with the targeted weakness, by
less than the first change and with a caveat. Confidence rose again on every
seed, by about 0.12–0.14, to 0.98–0.99, so the model is now close to certain
on every test post. The five criteria again couldn't move, since they were
already perfect. The caveat is that near-certain confidence on an easy test
set is also what overconfidence looks like. Without posts that break my
surface cues (see What's Still Broken), I can't tell whether the model is
justifiably sure or just sure, so I count this as a measured but limited gain.

## Stretch: Confidence Check

**Question:** Are the model's most-confident predictions right more often than
its least-confident ones? This comes from the `confidence` block in each
results file: test posts are ranked by the model's top softmax score and the
accuracy of the top third is compared with the bottom third (10 posts each).

| Run | Seed | Most-confident third | Least-confident third | Mean conf. when right | Mean conf. when wrong |
|---|---|---|---|---|---|
| Before | 42 | 1.00 (10/10) | 1.00 (10/10) | 0.590 | — (no errors) |
| Before | 7 | 1.00 (10/10) | 1.00 (10/10) | 0.556 | — (no errors) |
| Before | 2024 | 1.00 (10/10) | 1.00 (10/10) | 0.584 | — (no errors) |
| After | 42 | 1.00 (10/10) | 1.00 (10/10) | 0.868 | — (no errors) |
| After | 7 | 1.00 (10/10) | 1.00 (10/10) | 0.840 | — (no errors) |
| After | 2024 | 1.00 (10/10) | 1.00 (10/10) | 0.858 | — (no errors) |
| After 2 | 42 | 1.00 (10/10) | 1.00 (10/10) | 0.985 | — (no errors) |
| After 2 | 7 | 1.00 (10/10) | 1.00 (10/10) | 0.980 | — (no errors) |
| After 2 | 2024 | 1.00 (10/10) | 1.00 (10/10) | 0.983 | — (no errors) |

**Answer: no.** The confident predictions are not right more often than the
unsure ones; both thirds are 100% right on every seed of every run. That's
not because confidence is meaningless. With zero wrong predictions there is
nothing for confidence to separate, so there's no "mean confidence when
wrong" to compare against. On my test splits, confidence can't be
checked as a warning sign. What the check did show is that the model's
confidence level is set mostly by training length and learning rate, not by
how hard a post is: it rose from about 0.58 to 0.98 across the three runs
while accuracy stayed fixed at 1.000.

---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->
**Criteria 1–4: none missed.** All four held on every seed, before and after.
What's broken is the measurement, not the model: my test splits are too easy
to tell a model that learned my boundary from one that learned the surface
cues. If I kept going, I'd add a hard test set of about 30 new posts written
to break those cues: `investor_inquiry` posts without a question mark,
`speculative_hype` posts that quote real-looking figures, and enthusiastic
`fundamental_analysis` posts. I'd re-score against my tightened accuracy
target (≥ 0.95). I stopped because that needs new posts collected and labelled
with the same care as the first 205, which is more than the session allowed.

**Criterion 5: pending** until my agreement check against the staff set is
done (see Agreement Report).



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->
I meant my labels to capture whether a post's view of a stock is backed by
evidence from the company's financials, and the confusion matrix shows zero
errors on every seed, including the `speculative_hype` →
`fundamental_analysis` boundary I expected to be hardest. But every
`investor_inquiry` post in my data has a question mark and the analysis posts
cite financial figures, so a clean matrix can't tell me whether the model
learned that evidence standard or just learned punctuation and number
patterns.



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
