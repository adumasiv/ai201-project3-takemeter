# TakeMeter Label Taxonomy
**Community:** r/LoveIslandUSA  
**Task:** Classify discourse quality in fan posts and comments

---

## Labels

### 1. SUBSTANTIVE
**Definition:** Post makes a contestable claim about the show or an islander and supports it with at least one specific reason tied to observable content — a scene, a pattern of behavior, or a production decision.

**Canonical examples:**
- "Will and Kyra are forced as heck. Every time a new connection was threatened, he recoupled strategically — but watch how his body language changes when he's talking to her vs. when he's talking to the camera. He's performing it."
- "The US producers are too hands-on compared to UK. You can literally see sponsored posts from islanders appearing while they're still in the villa. That's not an accident — the brand pipeline is built before the show even airs."

**Near miss:** "The compressed shooting schedule made all the connections feel fake." This has a structural reason (schedule length) but never ties it to a specific couple, episode, or moment. No observable content = TAKE, not SUBSTANTIVE.

---

### 2. TAKE
**Definition:** Post makes a contestable claim that requires having watched the show, but states no supporting reason.

**Canonical examples:**
- "Half of them are already influencers which ruins the fun of Love Island."
- "This season had no genuine emotion going on in the entire villa. Not one honest reaction or feeling."

**Near miss:** "Show has lost its way." Sounds like a Take, but it's vague enough to write without watching — no specific claim about what aired. That makes it a REACTION, not a TAKE.

---

### 3. REACTION
**Definition:** Post expresses emotion, social solidarity, or a vague sentiment that could have been written without engaging with show content.

**Canonical examples:**
- "SERENA DESERVED EVERYTHING AND I WILL DIE ON THIS HILL 😭😭😭 she was robbed"
- "Stan culture ruined Love Island USA this season. Fans have made this show kinda unwatchable."

**Near miss:** "This season is complete trash. There is no genuine emotion going on in that entire villa." The second sentence makes a specific claim about aired content — someone who watched could push back by citing a genuine moment. That claim requires engagement with the show, so this is a TAKE, not a REACTION, even though the register is emotional.

---

## Estimated Label Distribution

| Label      | Estimated share |
|------------|----------------|
| SUBSTANTIVE | ~20%           |
| TAKE        | ~45%           |
| REACTION    | ~35%           |

No label exceeds 60%.

---

## Decision Rules for Ambiguous Cases

### SUBSTANTIVE vs. TAKE
**Rule:** Does the post give a *specific* reason anchored to observable content — a named scene, a behavioral pattern across episodes, a verifiable production detail?
- Specific reason present → **SUBSTANTIVE**
- Reason is structural or general (e.g., "the schedule," "casting choices") without pointing to anything that aired → **TAKE**

**Example resolved:**
> "The compressed shooting schedule made connections feel fake. They just didn't have enough time."

Gets **TAKE**. "Compressed schedule" is a real cause, but the post doesn't name which connections suffered or show when the compression was visible on screen. No observable content cited.

---

### TAKE vs. REACTION
**Rule:** Could this post have been written without watching the show — knowing only character names, the show's format, or the outcome?
- Requires watching to make the claim → **TAKE**
- Could be written from outside knowledge, vibes, or allegiance alone → **REACTION**

**Example resolved:**
> "I feel like I'm watching Big Brother. Show has lost its way."

Gets **TAKE**. The Big Brother comparison requires having watched both shows and formed a view about genre. A pure fan who never watched couldn't make that comparison meaningfully. The implicit comparison functions as a weak reason.

**Example resolved:**
> "SERENA DESERVED EVERYTHING 😭😭😭"

Gets **REACTION**. Knowable from the finale result alone; requires no engagement with what aired.

---

## Annotator Guidance

- Label the **dominant function** of the post, not its best sentence.
- A post that opens with a specific observation but ends in a stan rallying cry gets labeled by whichever mode drives the majority of the text.
- **Register does not determine label.** All-caps emotional delivery can still be a TAKE if the underlying claim requires show engagement. Calm, reasoned tone can still be a REACTION if the claim is content-free.
- **Length does not determine label.** A one-sentence observation with a cited scene is SUBSTANTIVE. A five-paragraph rant with no observable content cited is REACTION.
- REACTION has two subtypes for internal tracking (do not use as separate labels):
  - *Emotional*: pure feelings, venting, ungrounded declarations
  - *Social*: stan rallying, hate pile-ons, harassment-adjacent content
