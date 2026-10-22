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

**Where the posts came from:**

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->

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
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

**2.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

**3.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

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

**Moment 1**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Moment 2**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Pre-labelling disclosure:**

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



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

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



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
