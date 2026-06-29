# Milestone 1: Community & Label Design

## Community Description

r/worldcup is the primary World Cup discussion subreddit where fans debate and predict tournament outcomes. We focus specifically on World Cup winner prediction posts — the distinct ways people forecast which team will lift the trophy. The labels distinguish predictions by the team's FIFA ranking (elite vs. underdog) and how much forum discussion/hype surrounds that team (consensus pick vs. niche bet).

## Label Taxonomy

### Label 1: Consensus Favorite
**Definition:** Top 10 FIFA ranked team AND frequently discussed/predicted in forums (>5% mention rate)

**Clear Examples:**
1. "France will win — they have the best squad and everyone's talking about them as favorites"
2. "Argentina's odds are lowest, and for good reason: their recent form and depth make them the pick"

**Ambiguous Case:** A top 10 team that gets moderate discussion (2-4%) — is it Consensus Favorite or Underrated Strong Team?

**Decision Rule:** If >5% forum mentions + top 10 rank → **Consensus Favorite**. If 2-4% mentions + top 10 rank → **Underrated Strong Team**

---

### Label 2: Underrated Strong Team
**Definition:** Top 10 FIFA ranked team but rarely predicted/discussed in forums (<2% mention rate)

**Clear Examples:**
1. "People sleep on Germany — they're ranked 7th and have a solid squad, but nobody's picking them"
2. "England is criminally underrated. They're top 10 but everyone overlooks them"

**Ambiguous Case:** A top 10 team with emerging discussion (just starting to trend) — Underrated or trending to Consensus?

**Decision Rule:** If top 10 rank AND <2% current mention rate → **Underrated Strong Team**. Once discussion crosses 5%, reclassify as **Consensus Favorite**

---

### Label 3: Popular Underdog
**Definition:** Lower ranked team (outside top 10) but frequently discussed/hyped in forums (>5% mention rate)

**Clear Examples:**
1. "Uruguay might surprise everyone — they're underdog odds but the community loves their chances"
2. "Spain at 15th ranking could pull it off — lots of people are picking them as the dark horse"

**Ambiguous Case:** A team at exactly #10-11 boundary that's heavily discussed — is it Popular Underdog or Consensus Favorite?

**Decision Rule:** If ranked #11 or lower AND >5% discussion → **Popular Underdog**. If ranked #10 or higher AND >5% discussion → **Consensus Favorite**

---

### Label 4: Niche Dark Horse
**Definition:** Lower ranked team (outside top 10), rarely discussed/predicted (<2% mention rate), outlier pick

**Clear Examples:**
1. "Switzerland could win it all — nobody's talking about them, but their team is solid"
2. "Portugal at #8 is being ignored; I think they'll shock everyone"

**Ambiguous Case:** A lower-ranked team that's newly trending (discussion just starting to rise) — Niche Dark Horse or Popular Underdog?

**Decision Rule:** If <2% mention rate AND ranks outside top 10 → **Niche Dark Horse**. Once discussion reaches 5%+ and team is <#10, move to **Popular Underdog**

---

## Mutual Exclusivity Check

The four labels form a 2×2 matrix that is mutually exclusive:

| | Top 10 Ranked | Outside Top 10 |
|---|---|---|
| **>5% Forum Discussion** | Consensus Favorite | Popular Underdog |
| **<2% Forum Discussion** | Underrated Strong Team | Niche Dark Horse |

**Gap zone (2-5% mentions):** Requires judgment call, but most posts will fall clearly into one category.

---

## Why These Distinctions Matter

These labels capture two real dimensions of World Cup prediction culture:

1. **Team Credibility (FIFA Ranking):** Top 10 teams have objective evidence of strength (recent tournament performance, player quality, squad depth). Lower-ranked teams are underdogs with less statistical backing.

2. **Prediction Consensus (Forum Discussion):** A widely-discussed prediction reflects community agreement; a rare prediction is contrarian or niche. 

A post saying "France will win" (Consensus Favorite) is fundamentally different from "I think Portugal could surprise everyone" (Niche Dark Horse) — one reflects mainstream opinion, one is a bold outlier. Community members recognize and value this distinction when evaluating predictions against actual tournament outcomes.

---

# Hard Edge Cases

## Ambiguous Boundaries

### 1. **Ranking Boundary (Teams at #10-11)**
**Case:** A team ranked exactly #10 or #11 that has high discussion (>5%)
- Example: "Spain is #11 and I think they'll win — lots of people are starting to back them"
- **Problem:** Is Spain a "Consensus Favorite" (behaves like top 10) or "Popular Underdog" (technically outside top 10)?
- **Decision Rule:** Apply strict ranking cutoff. If ranked #10 or higher → **Consensus Favorite**. If #11 or lower → **Popular Underdog**, regardless of how close.
- **Rationale:** Ranking is objective; using it strictly prevents drift and keeps boundaries clear.

### 2. **Discussion Volume Transition (Teams moving from <2% to >5%)**
**Case:** A team that was rarely discussed (<2%) but is suddenly trending
- Example: "Germany was underrated a week ago, but now everyone's talking about them after the recent matches"
- **Problem:** At what point does "Underrated Strong Team" become "Consensus Favorite"?
- **Decision Rule:** Use **current** discussion rate at annotation time. If discussion is now >5%, classify as Consensus Favorite (the label applies *now*, not historically).
- **Rationale:** Predictions are evaluated when made; if a post was written when discussion was rising, read the post's tone to judge if prediction feels mainstream or contrarian at that moment.

### 3. **Mixed Signals (Posts questioning a team's ranking)**
**Case:** Post says "Germany [ranked #7] is being slept on" — is this Underrated or Consensus?
- Example: "Germany is ranked 7th but nobody's picking them to win — I think they're underrated"
- **Problem:** Post explicitly says the team is "underrated," but ranking is top 10
- **Decision Rule:** Look at **current discussion volume**, not the post's framing.
  - If discussion rate for Germany is <2% → **Underrated Strong Team** (the post's claim about undervaluation matches reality)
  - If discussion rate is >5% → **Consensus Favorite** (the post's framing is outdated; everyone's now talking about Germany)
- **Rationale:** Label reflects objective current state, not the post author's subjective assessment.

### 4. **Gap Zone (2-5% Discussion Rate)**
**Case:** A team with exactly 3% discussion rate (in the ambiguous zone)
- **Decision Rule:** Apply **qualitative judgment**. Read the post and ask:
  - Does the prediction *feel* like it's part of mainstream discourse? (→ Consensus Favorite if top 10)
  - Does it *feel* like a contrarian or niche take? (→ Underrated Strong Team if top 10)
  - Consider timing: if discussion is trending upward, treat as emerging consensus. If stable, treat as niche.
- **Rationale:** The 2-5% zone is inherently ambiguous; qualitative judgment is more reliable than a hard cutoff.

---

# Data Collection Plan

## Approach: Mix of Real Reddit + Synthetic Examples

### Collection Strategy

**Real Posts (100-120 examples):**
- Source: r/worldcup subreddit (manual scraping or PRAW API if available)
- Focus: Posts explicitly predicting/discussing World Cup winners
- Timeframe: Current and recent tournament cycles (6-12 months of posts)
- Quality filter: Exclude very low-effort posts ("France will win lol") that lack any context

**Synthetic Posts (80-100 examples):**
- Generated by Claude to fill underrepresented labels or boundary cases
- Use case: If after 120 real posts, one label has <30 examples, generate synthetic examples matching that label's characteristics
- Tracking: Mark synthetic examples with metadata `source: synthetic` for transparency
- Constraint: Synthetic posts should match stylistic range of real r/worldcup posts (tone, length, reasoning style)

**Total Target: 200+ examples**

### Per-Label Distribution Goal
- Aim for ~50 examples per label if possible
- But accept natural imbalance (some labels may have 30-60, others 40-80)
- Natural imbalance reflects real community discourse

### Contingency: If Severely Underrepresented
- If one label has <20 examples after collecting 200: Accept the imbalance
- Evaluate that label separately and note in final report: "Class imbalance: Label X has only 20 examples vs. ~50-60 for other labels"
- Report per-label performance independently (don't micro-average; report recall/precision per label)

---

# Evaluation Metrics

## Primary Metric: Recall Per Label

**Definition:** For each label, what % of actual posts of that type does the model correctly identify?

**Formula:** Recall = (True Positives) / (True Positives + False Negatives)

**Example:** If there are 50 actual "Popular Underdog" posts in the test set and the model catches 42 of them, recall = 84%.

**Why Recall Matters Most:**
- Community tool use case: If the goal is to surface "Popular Underdog" predictions to users, **missing 8 posts is a failure**
- False positives (incorrectly labeling a "Consensus Favorite" as "Popular Underdog") are less critical — they just appear in the wrong bucket, but the user sees them
- Missing a post (false negative) means the tool doesn't surface it at all — the user never sees a prediction they might care about
- **Result:** High recall ensures the tool doesn't silently fail by missing posts of a label

## Secondary Metrics

### Precision Per Label
**Definition:** Of all predictions the model makes for a label, how many are actually correct?

**Formula:** Precision = (True Positives) / (True Positives + False Positives)

**Why Secondary:** If recall is high, precision tells us how "clean" the predictions are. Low precision + high recall = the tool finds most relevant posts but also shows false positives.

### Confusion Matrix
**Definition:** A table showing which labels get confused with each other most

**Example:**
```
                Predicted
         |  CF  | UST  | PU  | NDH |
Actual   | 
   CF    | 45   | 3    | 2   | 0   |
   UST   | 2    | 48   | 0   | 0   |
   PU    | 4    | 1    | 42  | 3   |
   NDH   | 1    | 0    | 5   | 44  |
```

**Why It Matters:** Identifies systematic confusions. E.g., if the model often confuses "Popular Underdog" and "Consensus Favorite," that tells you the ranking boundary is hard to learn — you might need to refine label definitions or collect more boundary examples.

### Per-Label Performance Variation
**Question:** Do some labels perform much better/worse than others?

**Example:** If Consensus Favorite has 92% recall but Niche Dark Horse has only 65%, that's a red flag. It suggests:
- Niche Dark Horse examples are harder for the model to learn
- Maybe the label definition is too vague
- Maybe there aren't enough examples

**Action:** Report this clearly in evaluation so you understand which labels are reliable and which need refinement.

## Test Set & Evaluation Process

1. **Hold out 50 examples** (~12-13 per label) as a test set. Do NOT annotate these with the model during training.
2. **Train on remaining 150** examples
3. **Evaluate on held-out 50**: Calculate recall, precision, confusion matrix
4. **Repeat with different random splits** (e.g., 3 random 50-example test sets) to check stability
5. **Report:** Mean ± std of recall/precision across splits, and the confusion matrix from the primary split

---

# Definition of Success

## Success Criterion: >80% Recall Per Label

### What Does This Mean?
For each of the 4 labels, the model catches at least **4 out of 5 posts** of that type.

- **Consensus Favorite:** ≥80% recall
- **Underrated Strong Team:** ≥80% recall
- **Popular Underdog:** ≥80% recall
- **Niche Dark Horse:** ≥80% recall

### Why 80%?

1. **High Enough to Be Useful:** Missing 1 in 5 posts is tolerable for a community tool. Missing 1 in 2 would make it unreliable.

2. **Realistic for 200-Example Dataset:** With 50 examples per label, 80% recall is achievable without overfitting (typical performance in the 75-90% range for well-labeled small datasets).

3. **Distinguishes Good from Mediocre:** A model that just predicts the majority label would achieve ~50% accuracy (4 labels, but imbalanced). 80% recall is clearly better than random.

4. **Deployment Threshold:** Good enough for a prototype community feature. Not production-grade (which might require 95%+), but sufficient for testing with actual users.

### Additional Success Criteria

**Precision is Acceptable:**
- No label should have <60% precision (this means >40% false positives, which would clutter the interface)

**Label Confusions Are Understandable:**
- No label pair should confuse >20% of the time
- Example: If "Popular Underdog" gets mislabeled as "Consensus Favorite" 25% of the time, that's a problem — the boundary is too unclear

**Performance Stability:**
- Recall should be stable across random train/test splits (±5% variation is acceptable, >15% variation suggests the model is inconsistent)

### Failure Criteria (When to Revisit Labels)

If after full evaluation:
- Any label has <70% recall → **Label definition was too vague.** Revisit and refine.
- Any label pair confuses >25% of the time → **Boundary is unclear.** Refine definitions or collect more boundary examples.
- Recall varies >20% across splits → **Dataset is too small or imbalanced.** Collect more data for underrepresented labels.

---

# AI Tool Plan

## 1. Label Stress-Testing (Before Annotation)

**Purpose:** Validate that your 4 labels can actually be applied consistently before investing effort in annotating 200 examples.

**Process:**
1. Give Claude your 4 label definitions + the hard edge case rules
2. Ask: "Generate 5-10 World Cup winner prediction posts that sit at the boundary between two labels. These should be genuinely ambiguous."
3. For each generated post, try to classify it using your labels
4. **Result:** 
   - If you can confidently classify all of them → **Labels are solid, proceed to annotation**
   - If you struggle with >2-3 posts → **Your definitions are too vague. Refine now before annotating 200 examples**

**Example Request to Claude:**
```
You have these 4 labels for World Cup winner predictions:
[your label definitions here]

Generate 5-10 posts that sit at the boundary between two labels 
and are genuinely hard to classify. For each, tell me which two 
labels it could belong to.
```

**Outcome:** Refined label definitions before heavy work.

---

## 2. Annotation Assistance: LLM Pre-Label + Human Review

**Rationale:** Claude can pre-label to speed up annotation, but you review to catch errors and maintain quality.

**Process:**

**Step 1: Prepare batches**
- Divide 200 posts into batches of 50
- Create a CSV with columns: `post_id`, `post_text`, `claude_label`, `claude_confidence`, `human_label`, `labeled_by`

**Step 2: Claude pre-labels**
```
For each post in batch:
- Send post text to Claude with label definitions + 2 examples per label
- Claude responds with: label prediction + reasoning + confidence (high/medium/low)
- Log: `claude_label` and `claude_confidence`
```

**Step 3: Human review**
- You read Claude's label + reasoning
- You read the post
- You either:
  - **Accept Claude's label** (fast path) → mark `human_label = claude_label`, `labeled_by: claude_reviewed`
  - **Correct Claude's label** → mark `human_label = corrected_label`, `labeled_by: claude_reviewed_corrected`
  - **Re-annotate from scratch** (if Claude's reasoning is confusing) → mark `labeled_by: human_only`

**Step 4: Track metadata**
- `labeled_by: human_only` — you annotated without Claude's input
- `labeled_by: claude_reviewed` — Claude was correct, you accepted it
- `labeled_by: claude_reviewed_corrected` — Claude was wrong, you fixed it

**Outcome:** Annotation spreadsheet with 200 labeled posts + metadata on Claude vs. human labels

### Disclosure in Final Report
In your project writeup, report:
- % of examples that were Claude-pre-labeled vs. human-only
- Claude's accuracy (% of pre-labels you accepted vs. corrected)
- Example: "70 examples (35%) were pre-labeled by Claude. Claude was correct on 55 of them (79% accuracy). I manually annotated the remaining 130 examples."

This transparency is important for demonstrating that the labels are consistent and not just an artifact of using Claude.

---

## 3. Failure Analysis (After Model Training)

**Purpose:** Understand *why* the model fails on certain posts so you can improve the labels or identify real boundary cases.

**Process:**

**Step 1: Collect misclassifications**
- Train your classification model (any model: logistic regression, Naive Bayes, etc.)
- Run it on the 50-example test set
- Extract all misclassified posts (e.g., 8-10 out of 50)
- Export: `post_text`, `true_label`, `predicted_label`, `predicted_confidence`

**Step 2: Analyze with Claude**
```
Send to Claude:

"Here are 10 posts my World Cup predictor got wrong:

[List of misclassified posts with true vs. predicted labels]

What patterns do you see? Why might the model have gotten these wrong? 
Look for:
1. Systematic confusions (does it always confuse label A with label B?)
2. Boundary cases (are these posts in the 2-5% discussion gap zone?)
3. Label definition problems (do these posts suggest the definition is unclear?)
4. Data issues (are some labels underrepresented in the training set?)"
```

**Step 3: Verify patterns yourself**
- Claude will generate hypotheses (e.g., "The model confuses Popular Underdog and Consensus Favorite when ranking is close to #10")
- **Don't just accept Claude's analysis.** Read the misclassified posts yourself and verify:
  - Do they actually sit at a boundary?
  - Is Claude's explanation accurate?
  - Are there other patterns Claude missed?

**Step 4: Document findings**
- Write up the failure analysis in your final evaluation section
- Example: "The model was most often confused between 'Popular Underdog' and 'Consensus Favorite.' All 5 confusions occurred when the predicted team ranked #9-11, suggesting the ranking boundary is a real difficulty for the model. Future work should collect more examples at this boundary."

**Outcome:** A structured explanation of model failures for your final report, plus insights on how to improve.

---

# AI Usage & Transparency

## Data Generation (Milestone 3: Complete)
**Tool:** Claude AI (via agents)
- Generated 200 synthetic World Cup winner prediction posts (50 per label)
- Posts created to match r/worldcup community style
- Pre-labeled by Claude with "high confidence"

## Data Annotation (Milestone 3: Complete)
**Annotation Process:**
- Claude pre-labeled all 200 posts
- User reviewed all 200 labels
- **Results:**
  - 181 posts (90.5%): Pre-label accepted as correct
  - 19 posts (9.5%): Pre-label corrected to different label

**Final Label Distribution:**
- Consensus Favorite: 51 (25.5%)
- Niche Dark Horse: 51 (25.5%)
- Popular Underdog: 47 (23.5%)
- Underrated Strong Team: 51 (25.5%)

## Data Quality Assessment

✅ **Strengths:**
- 200 labeled examples with perfect balance (no label >70%)
- 90.5% AI accuracy on label assignment
- All labels 100% human-verified

⚠️ **Limitations:**
- Data is synthetic (generated by Claude), not real r/worldcup posts
- Model trained on synthetic posts may not perfectly generalize to real community discourse
- Recommendation: Compare performance on real r/worldcup data before production deployment

## Impact on Model Training
- **Use this dataset for:** Prototyping, initial model development, proof-of-concept
- **Future work:** Collect real Reddit posts and compare synthetic vs. real model performance
- **Expected outcome:** Synthetic-trained model provides good starting point; real data will refine accuracy

---

# Next Steps

1. **Stress-test labels** (AI Tool Plan #1) — generate boundary posts and verify you can classify them
2. **Collect data** — combine real Reddit posts + synthetic examples, aim for 200+ posts
3. **Annotate** (AI Tool Plan #2) — use Claude pre-labeling + your review; track metadata
4. **Train model** — build a simple text classifier (logistic regression, Naive Bayes, or neural network)
5. **Evaluate** — calculate recall/precision/confusion matrix on held-out test set
6. **Analyze failures** (AI Tool Plan #3) — understand why the model failed and document insights
7. **Write final report** — include evaluation metrics, success criteria assessment, lessons learned
