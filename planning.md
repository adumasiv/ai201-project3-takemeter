# TakeMeter — Project Planning

## Community

r/LoveIslandUSA is a reality TV fan community organized around real-time discourse about islander relationships, contestant character, and show production — with a known internal tension between analytical fan culture and stan/hate behavior that the community itself debates. The three labels — SUBSTANTIVE, TAKE, and REACTION — track whether a post contributes to collective understanding of the show (makes a specific, evidence-grounded claim), participates in opinion exchange (makes a claim without reasons), or functions as social signaling (expresses emotion or allegiance without engaging with show content). This distinction matters to regulars because the subreddit has a running meta-debate about whether stan culture and emotional pile-ons have degraded discourse quality — making the TAKE/REACTION boundary exactly the fault line the community itself cares about.

---

## Labels (summary)

| Label       | Definition |
|-------------|------------|
| SUBSTANTIVE | Contestable claim + specific reason tied to observable content |
| TAKE        | Contestable claim that requires watching the show; no reason given |
| REACTION    | Emotion, solidarity, or vague sentiment; writable without watching |

Full definitions, canonical examples, near-misses, and decision rules: see [LABELS.md](LABELS.md).

---

## Key Decision Rules

1. **SUBSTANTIVE vs. TAKE:** Is the reason specific and tied to something observable (scene, pattern, production detail)? Structural explanations without aired evidence stay TAKE.
2. **TAKE vs. REACTION:** Could the post have been written without watching the show? If yes → REACTION.
3. **Register does not determine label.** Emotional delivery can still be a TAKE; calm tone can still be a REACTION.

---

## Data Pipeline (planned)

### Step 1 — Data collection
- Source: r/LoveIslandUSA via Pushshift or Reddit API
- Target: 500–1,000 posts and top-level comments
- Sampling strategy: mix of episode-night megathreads (high REACTION density) and weekday discussion posts (higher SUBSTANTIVE/TAKE density) to avoid distribution skew

### Step 2 — Annotation
- Pilot: 50 posts labeled independently by 2 annotators
- Calibration item: include the "Big Brother" post and the "compressed schedule" post as anchor examples at the top of annotation instructions
- Target: Cohen's kappa ≥ 0.65 before full annotation run
- Full annotation: 500+ posts with adjudication on disagreements

### Step 3 — Model
- Base model: fine-tuned text classifier (distilbert-base-uncased or roberta-base)
- Input: post title + body text (truncated to 512 tokens)
- Output: 3-class prediction (SUBSTANTIVE / TAKE / REACTION)
- Evaluation: macro F1, with separate F1 per label to catch class imbalance

### Step 4 — Analysis
- Primary question: what features distinguish SUBSTANTIVE from TAKE? (hypothesis: scene references, named episodes, behavioral qualifiers like "every time")
- Secondary question: does post length, time-of-day, or thread type predict label?

---

## Risk Flags

| Risk | Mitigation |
|------|------------|
| TAKE label may capture >45% of posts | If pilot shows >55%, consider splitting TAKE into GROUNDED-TAKE (has weak reason) and BARE-TAKE (no reason at all) |
| REACTION subtypes (emotional vs. social) may need separation | Track internally during annotation; split into separate labels only if inter-annotator agreement suffers |
| Post-villa content (couple updates, breakup news) doesn't fit taxonomy cleanly | Add an OUT-OF-SCOPE label for non-discourse posts (news links, photos, self-promo) |
| Stan culture posts may get mislabeled as TAKE if they contain a claim | Annotator guide should flag: if the post's primary function is social rallying, label REACTION regardless of embedded claim |
