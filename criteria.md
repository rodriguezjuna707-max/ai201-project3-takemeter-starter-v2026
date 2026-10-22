# Acceptance Criteria

Written before training. All metrics are measured on `test_split.csv` (about 31 posts,
15% of 205: roughly 13 `fundamental_analysis`, 10 `speculative_hype`, 8 `investor_inquiry`)
from the default run (seed 42, 3 epochs, learning rate 2e-5, batch size 16, 128 tokens),
using the numbers recorded in `results.json`.

## 1. Overall accuracy

**Measurement:** Overall accuracy on the test split, as recorded in `results.json`.

**Target:** at least 0.80 (about 25 of 31 posts correct).

**Why:** Always guessing `fundamental_analysis` scores about 0.41, and my labels have strong
surface signals (every `fundamental_analysis` post cites a financial metric, every
`investor_inquiry` post is phrased as a question), so a model that gets fewer than 25 of 31
right has not learned the distinctions that make the classifier useful for sorting posts.

## 2. F1 for investor_inquiry

**Measurement:** F1 score for `investor_inquiry` on the test split, as recorded in `results.json`.

**Target:** at least 0.80.

**Why:** It is my smallest label (55 of 205 posts, about 8 in the test split), so a single
miss costs about 12 points of recall, but all 55 inquiry posts contain a question mark and
none of the 85 analysis posts do, so 0.80 allows only one or two errors on what should be
my most separable label.

## 3. Speculative hype mistaken for fundamental analysis

**Measurement:** In the confusion matrix in `results.json`, the `speculative_hype` row:
posts predicted as `fundamental_analysis` divided by the row total.

**Target:** at most 0.20 (no more than 2 of about 10 hype posts).

**Why:** 58 of my 65 hype posts contain numbers ("10x", "5x play", "coming in 2030") that
look like the metrics in analysis posts, which makes this my riskiest boundary, and calling
hype "analysis" is the most harmful mistake for a reader looking for well-supported ideas.

## 4. Balance of the model's predictions

**Measurement:** Share of test predictions that are `fundamental_analysis` (sum of the
`fundamental_analysis` column of the confusion matrix divided by total test posts),
compared with the true share of `fundamental_analysis` in the test split (that row's sum
divided by total test posts).

**Target:** the two shares differ by at most 10 percentage points (about 3 posts).

**Why:** With about 143 training posts and 3 epochs, the training run is short, and the
likeliest failure is a model that drifts toward my largest label (41.5% of the data),
which this criterion catches even when overall accuracy looks acceptable.

## 5. Agreement with staff labels

**Measurement:** Share of the 30 posts in `data/staff_posts.csv` where my label matches
the staff label, as computed by `agreement.py`.

**Target:** at least 0.70 (21 of 30 posts).

**Why:** I will be applying a taxonomy I did not design to some posts chosen to be
ambiguous, so perfect agreement is unrealistic, but matching on fewer than 21 of 30 would
mean I am not applying written definitions consistently, which undermines how much my own
205 labels can be trusted.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
