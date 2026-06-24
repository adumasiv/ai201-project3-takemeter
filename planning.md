# TakeMeter — Project Planning

---

## 1. Community

**Chosen community:** r/LoveIslandUSA

r/LoveIslandUSA is a Peacock reality TV fan community built around real-time discourse about islander relationships, contestant character, and show production. It was chosen because it sits at an unusually productive intersection for a classification task: the community has a genuine internal debate about its own discourse quality. Regulars openly meta-complain that stan culture and emotional pile-ons have degraded the subreddit, while a separate contingent produces detailed editing analysis, production critiques, and contestant character reads backed by scene evidence. That self-awareness creates natural signal — the community itself has implicitly defined what good and bad discourse looks like, which means annotators are not imposing an external standard but recognizing one that already exists.

The discourse is varied enough to be interesting for three reasons. First, the subject matter generates at least three distinct post types: contestant character evaluations ("is this person genuine?"), production critiques ("are the editors manipulating us?"), and social signaling ("my fave was robbed"). Second, the same underlying topic — islander authenticity — gets discussed at wildly different levels of specificity, from scene-by-scene behavioral analysis to one-word declarations. Third, the community's size and activity (episode-night threads clearing thousands of comments within hours) means there is no shortage of data across the full quality spectrum.

---

## 2. Labels

### SUBSTANTIVE
**Definition:** Post makes a contestable claim about the show or an islander and supports it with at least one specific reason tied to observable content — a named scene, a pattern of behavior across episodes, or a verifiable production detail.

**Example A:**
> "Will and Kyra are forced as heck. Every time a new connection was threatened, he recoupled strategically — but watch how his body language changes when he's talking to her vs. when he's talking to the camera. He's performing it."

**Example B:**
> "The US producers are too hands-on compared to UK. You can literally see sponsored posts from islanders appearing while they're still in the villa. That's not an accident — the brand pipeline is built before the show even airs."

---

### TAKE
**Definition:** Post makes a contestable claim that requires having watched the show — so it engages with show content — but offers no supporting reason; the judgment stands alone.

**Example A:**
> "Half of them are already influencers which ruins the fun of Love Island."

**Example B:**
> "This season had no genuine emotion going on in the entire villa. Not one honest reaction or feeling."

---

### REACTION
**Definition:** Post expresses emotion, social solidarity, or a vague sentiment that could have been written without engaging with show content — pure feeling, stan allegiance, or hate with no claim about what aired.

**Example A:**
> "SERENA DESERVED EVERYTHING AND I WILL DIE ON THIS HILL 😭😭😭 she was robbed"

**Example B:**
> "Stan culture ruined Love Island USA this season. Fans have made this show kinda unwatchable."

---

## 3. Hard Edge Cases

### The primary hard edge: TAKE vs. REACTION

The TAKE/REACTION boundary is the fault line most likely to produce annotator disagreement, because both labels lack a stated reason and both can be delivered in an emotional register. The intended boundary is a content-engagement test: a TAKE requires having watched the show to make the claim; a REACTION could have been written from outside knowledge, allegiance, or vibes alone.

**The genuinely hard case:**
> "This season is complete trash. There is no genuine emotion going on in that entire villa. Not one honest reaction or feeling. Everything is so thought out."

Why it's hard: the all-caps energy and sweeping condemnation read as REACTION, but "not one genuine emotion" is a claim about what aired — someone who watched could push back by citing a specific couple. The claim requires show engagement, which technically makes it a TAKE.

**Decision rule:** Apply the content-engagement test to the *claim*, not the *register*. If the core assertion ("no genuine emotion in the villa") can only be meaningfully made by someone who watched, it is a TAKE regardless of emotional delivery. Reserve REACTION for posts where the core assertion could be made by someone who only knows the show's name, the cast list, or the result.

**Secondary hard edge: SUBSTANTIVE vs. TAKE — structural claims without scene evidence**

> "The compressed shooting schedule made all the connections feel fake. They just didn't have enough time."

This cites a real cause (schedule length) but never points to which connections, which episodes, or any observable behavior. Decision rule: structural or production explanations count as reasons only if they name something specific enough for another viewer to verify or dispute. "Compressed schedule" alone is not enough — it must connect to something that aired to cross into SUBSTANTIVE.

**In annotation practice:** flag any post that generates disagreement between annotators. Collect these as a running ambiguous-cases log and use them to refine the annotator guide after the pilot round. Do not adjudicate ambiguous cases by majority vote alone — write an explicit rule for each recurring type.

---

## 4. Data Collection Plan

**Source:** Reddit posts and top-level comments from r/LoveIslandUSA, collected via the Reddit API (PRAW) or Pushshift archive.

**Target volume:** 600 labeled examples minimum — 200 per label. This gives enough examples for an 80/10/10 train/dev/test split while keeping each label represented in evaluation.

**Sampling strategy:** Pull from two thread types deliberately, because they have different label distributions:
- *Episode-night megathreads* (posted within 2 hours of episode drop): high REACTION and TAKE density — fast reactions, emotional pile-ons, rapid takes
- *Weekday discussion posts* (posted 2+ days after an episode): higher SUBSTANTIVE density — calmer, more deliberate analysis and production critique

Mixing 50/50 across thread types should prevent the dataset from being overwhelmed by REACTION posts and produce a more balanced distribution than pulling from the top posts of all time.

**If a label is underrepresented after 200 examples:**

SUBSTANTIVE is the label most likely to be underrepresented — it is genuinely rarer in this community. If fewer than 150 SUBSTANTIVE examples appear after collecting 600 posts, take two steps:
1. Add targeted search queries for high-signal post types: "editing," "screen time," "body language," "compared to season," "watch how" — these phrases correlate strongly with scene-grounded analysis.
2. Consider whether SUBSTANTIVE's definition is too narrow. If the threshold (specific observable content required) is excluding too many reasonable posts, relax "specific scene" to "specific reason" and re-review the borderline cases.

Do not artificially inflate SUBSTANTIVE by downgrading the definition — a loose SUBSTANTIVE label defeats the classifier's purpose.

**Out-of-scope posts** (news links, cast photos, self-promotion, post-villa couple updates) will be filtered before annotation using a simple keyword and domain filter. These are not discourse and should not appear in the training set.

---

## 5. Evaluation Metrics

**Primary metric: macro F1**

Accuracy is insufficient because the label distribution is imbalanced — TAKE will likely represent ~45% of examples, meaning a classifier that always predicts TAKE achieves ~45% accuracy while being useless. Macro F1 averages F1 equally across all three labels, penalizing the model for ignoring minority classes. That is the right behavior for this task, where correctly identifying SUBSTANTIVE posts (the rarest and most valuable class) matters as much as correctly classifying REACTION.

**Per-label F1 reported separately**

Macro F1 can hide a model that achieves high F1 on TAKE and REACTION while failing completely on SUBSTANTIVE. Report per-label precision, recall, and F1 at every evaluation checkpoint. Pay particular attention to SUBSTANTIVE recall — missing SUBSTANTIVE posts (false negatives) means the classifier fails at its primary use case.

**Confusion matrix**

Required to understand where errors concentrate. The key failure mode to watch: TAKE being misclassified as SUBSTANTIVE (false promotion) or REACTION being misclassified as TAKE (false upgrade). These errors are not symmetric — promoting a REACTION to TAKE is less harmful than promoting a TAKE to SUBSTANTIVE, because the TAKE/REACTION boundary is fuzzier in the community's own eyes.

**Inter-annotator agreement (Cohen's kappa)**

Reported on the pilot annotation set before model training begins. Target kappa ≥ 0.65. If kappa falls below 0.60 on any single label pair, that pair's decision rules need revision before full annotation. Kappa is a metric on the task design, not the model — it tells you whether the labels are real before you ask a model to learn them.

**What accuracy alone misses:** It cannot tell you whether the model's errors concentrate in the most consequential class (SUBSTANTIVE), whether the model is exploiting spurious correlations (emoji density → REACTION), or whether the label distribution in your test set reflects the real-world distribution of posts the tool would encounter.

---

## 6. Definition of Success

**Minimum bar for genuine usefulness:**
- Macro F1 ≥ 0.70 on held-out test set
- SUBSTANTIVE F1 ≥ 0.65 specifically — the classifier must find the rare high-quality posts reliably enough to be worth trusting
- REACTION precision ≥ 0.75 — if the tool surfaces posts for community moderation or quality review, falsely flagging genuine discourse as REACTION would erode trust immediately

**"Good enough for deployment" threshold:**
- Macro F1 ≥ 0.75 with no single label below F1 = 0.65
- Inter-annotator kappa on a fresh sample of 50 posts ≥ 0.65 (meaning the model's labels are roughly as consistent as human annotators)
- The TAKE/REACTION confusion rate below 20% — this is the hardest boundary, so some confusion is acceptable, but the model should not be treating these as interchangeable

**What "deployment" means here:** The classifier would be used in a community dashboard to surface high-quality posts (SUBSTANTIVE) for pinning or visibility, and to flag REACTION-dominant threads for moderator review. A false positive rate on SUBSTANTIVE that exceeds 30% would make the surfacing tool noisy enough to be ignored. A false negative rate on REACTION that exceeds 25% would fail to catch the pile-on threads that motivated the project.

**What would make me revise the success criteria downward:** If pilot annotation shows kappa below 0.55 on the TAKE/REACTION boundary, the right response is not to lower the model target — it is to merge TAKE and REACTION into a single non-SUBSTANTIVE label and reframe the task as binary classification. A binary classifier with macro F1 ≥ 0.80 would be more useful than a three-way classifier that humans cannot consistently apply.

---

## 7. AI Tool Plan

**Stack:**

| Component | Tool |
|-----------|------|
| Base model | distilbert-base-uncased (HuggingFace) |
| Fine-tuning | Google Colab free tier (T4 GPU) |
| Training libraries | transformers + datasets + scikit-learn |
| Baseline LLM | Groq — llama-3.3-70b-versatile |

---

### 7a. Label Stress-Testing

Before annotating a single real example, use Groq (llama-3.3-70b-versatile) to generate 5–10 posts that deliberately sit at each of the two hard boundaries: TAKE/REACTION and SUBSTANTIVE/TAKE.

**The prompt template:**
> Here are three label definitions for classifying r/LoveIslandUSA posts: [paste full definitions from LABELS.md]. Generate 5 posts that sit at the boundary between TAKE and REACTION — posts that a reasonable annotator might assign to either label. Then generate 5 posts at the boundary between SUBSTANTIVE and TAKE. Do not tell me which label each belongs to.

**How to use the output:** Attempt to classify each generated post before reading any explanations. If a post cannot be assigned cleanly using the current decision rules, the definition is not tight enough — revise the rule in LABELS.md before moving on. Only proceed to annotation when every stress-test post can be classified with a clear, statable reason. This step happened once during planning and surfaced three definition gaps (see revision history in LABELS.md); it will run again after any mid-project label revision.

**What to watch for:** Posts where the LLM naturally drifts toward one label but the definition points to another — those reveal where the label's plain-English name is misleading annotators (including the model acting as annotator). If "REACTION" keeps attracting posts that belong in TAKE, the name may be the problem, not the definition.

---

### 7b. Annotation Assistance

**Decision: yes, use LLM pre-labeling for the first 200 examples.**

**Tool:** Groq (llama-3.3-70b-versatile), zero-shot with the full label definitions and decision rules from LABELS.md as system context.

**Workflow:**
1. Collect raw posts from Reddit API into a CSV.
2. Send each post to Groq with the label definitions. Record the predicted label and the model's one-sentence rationale in separate columns (`llm_label`, `llm_rationale`).
3. Manually review every example — do not accept any LLM label without reading the post. Mark each reviewed example with `human_reviewed = true`.
4. Where the LLM label and my label disagree, write a one-sentence note in a `disagreement_note` column. Disagreements are the most valuable data in the annotation set — they identify where the definitions are ambiguous in practice.

**Disclosure tracking:** The CSV will contain a `llm_prelabeled` column (boolean) on every row. The final dataset will report what percentage of examples were pre-labeled vs. labeled cold by a human first. Pre-labeled examples will be flagged as such in any publication or writeup.

**Why pre-label at all:** Pre-labeling surfaces the easy cases quickly and concentrates human attention on disagreements and edge cases. It also creates a natural baseline: if the zero-shot LLM achieves macro F1 ≥ 0.70 against my human labels, that tells me whether fine-tuning DistilBERT actually adds value over just using a prompted LLM — which is a finding worth reporting.

**Risk:** Anchoring bias — seeing the LLM's label before forming my own judgment may pull my annotation toward the model's choice. Mitigation: always read and form a tentative label mentally before looking at `llm_label` in the CSV. The column ordering in the review spreadsheet will put `post_text` first and `llm_label` last.

---

### 7c. Failure Analysis

After evaluation on the held-out test set, collect all misclassified examples and pass them to Groq with the following prompt:

> Here are [N] posts from r/LoveIslandUSA that a fine-tuned DistilBERT classifier got wrong. For each post, the true label and the predicted label are shown. Identify patterns in the errors — what do the misclassified posts have in common that might explain why the model was confused? Group them into 2–5 error types with a name and description for each type.

**What to look for in the LLM's analysis:**
- *Surface feature exploitation:* Did the model learn emoji density → REACTION, all-caps → REACTION, short length → REACTION? If so, it is pattern-matching register rather than claim structure — a fundamental failure for this task.
- *TAKE/REACTION confusion clusters:* Are the TAKE→REACTION errors concentrated in emotional-register TAKE posts? That would confirm the register-vs-claim-structure problem flagged in the decision rules.
- *SUBSTANTIVE false positives:* Posts predicted SUBSTANTIVE that are actually TAKE — do they share a surface marker like "watch how" or "every time" that sounds like evidence but isn't followed by specific content?

**How to verify the LLM's pattern claims:** For each error type the LLM identifies, manually pull the 5 examples it cites and check whether the proposed pattern actually holds. Do not report an error pattern you cannot verify by reading the posts yourself. The LLM's failure analysis is a hypothesis generator, not a conclusion — every pattern it names needs human confirmation before appearing in the writeup.
