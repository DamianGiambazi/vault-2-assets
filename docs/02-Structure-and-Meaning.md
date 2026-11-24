# Structure and meaning: how the book is cut and searched

We cut by structure and search by meaning. vector-snake decides where passages end; the embedding model (all-MiniLM-L6-v2) decides what each passage is about, as 384 numbers.

## 1. How the cuts are chosen

vector-snake does not cut at a fixed length. It splits at paragraphs first, then lines, sentences and words, and only then characters. It then packs the pieces up to a budget. The name is **structure-aware (recursive) chunking**.

| Budget | Files | Passages | Files cut | Where the cuts fall |
|---|---|---|---|---|
| 800 characters | 28 | 33 | 5 (chapters 3–7) | every cut on a paragraph break |
| 400 characters | 28 | 49 | 10 | some cuts fall on a sentence inside a paragraph |

(28 files = 24 chapters + 4 questions. At 800, the 24 chapters become 29 passages.)

- **Why 800, not Vault 1's 1,000:** the model reads at most 256 tokens. These notes are denser than Vault 1's newsletter, so each passage gets less room. Every run's receipt confirms it: `over_budget_count = 0` in all 28 files.
- **The run-up:** each passage after the first repeats up to 120 characters from the end of the one before. It starts on a word boundary, or at a sentence start when one fits. Chapter 5's run-up is just `I chose this."`: 16 characters, starting mid-quote. It's a chosen default, not a law.
- **A trial run first:** `--dry-run` cuts and audits with no model and no numbers, in seconds. Check the cuts before you pay for the numbers (never stack two unknowns).

## 2. Keywords versus meaning

A keyword search finds words. A meaning search finds passages whose vectors point in a similar direction to the question's vector.

```powershell
Select-String -Path .\book\corpus\*.md -Pattern 'window' | Group-Object Filename | Select-Object Count, Name
```

- **'window' finds 9 chapters:** 1, 2, 4, 7, 8, 12, 13, 19, 24. Four are the Figure's chapters; five are false alarms.
- **Two of the false alarms say the opposite:** *"does not look out of the window once"* (ch-04) and *"does not go to the window"* (ch-13). A keyword can't read "not".
- **The question's own words** ("motionless", "glass", "observed", "building", "opposite") find nothing, because the book never uses them.

## 3. What the meaning search found

The four questions share no **content** word with the book. (Small words such as "a", "and" and "never" are shared.)

| Thread | Predicted | Found (top 4) | Distances | Verdict |
|---|---|---|---|---|
| The watcher | 1 · 7 · 19 · 24 | 1 · 7 · 24 · 19 | 0.57–0.59 | HELD, 4 of 4 |
| The lie | 2 · 5 · 13 · 21 | 7 · 13 · 8 · 3 | 0.68–0.74 | NOT HELD |
| The cost | 4 · 5 · 7 | 14 · 2 · 21 · 6 | 0.77–0.84 | NOT HELD |
| The door | 22 · 23 · 24 | 10 · 11 · 5 · 24 | 0.64–0.76 | NOT HELD |

The result was identical in rehearsal (30 Oct) and on the take (1 Nov): every rank and every distance.

## 4. Similarity is not significance

The model found the **image** (someone watching from a window) perfectly. It missed the **ideas**:
- "hurting a friend" found the Sarah chapters: the friend, not the hurt;
- "departing" found people literally walking and leaving;
- "the lie" found the evasions, not the lie.

It matched what passages look like, not what they mean in the story.

Factors that probably shaped this (an interpretation, not measured):
- **Image versus idea:** the watcher is a physical scene; the lie, the cost and the door are inferred by the reader and never stated.
- **The Figure chapters mirror each other by design** (Locked Decision D3), so they cluster tightly.
- **The cost question matched nothing closely:** all its distances are 0.77 or more.
- **Negation and quotation:** the Lie appears as denial ("I'm fine. I chose this."), which small embedding models handle poorly.
- **One-line chapters:** chapters 21–23 are 74–91 characters, too little text to carry an idea.
- **The model and the corpus:** a small 384-number model, and terse notes rather than prose.

**Would a bigger model do better?** It might. That is a different experiment, with its own sealed prediction. This one tested what this laptop can do, and the questions are never changed after a result.

**The practical lesson:** a local meaning search is good at finding *where* things happen: recurring images, objects, scenes, continuity. Deciding *what they mean* stays with the author, and goes into the SSOT.
