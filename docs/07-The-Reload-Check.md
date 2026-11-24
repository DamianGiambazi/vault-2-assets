# The reload check: can a fresh session pick up the project?

A Handover is only proven when a session with no history reloads the project from the documents alone. Vault 2 tests this with three different AI models and a fourth as a checker. The checklist was written, and committed to git, before any chat saw the documents.

## 1. Why a separate video

The main recording session ran long. That is exactly the problem this vault is about: long sessions are where things get forgotten. So instead of pushing on, the reload check was recorded as its own short video: stop, and reload from the documents.

## 2. The setup

- **The checklist first:** `spine/INHERITANCE.md`, committed and pushed in the main session (commit `dc9e692`, 1 Nov 2026, 17:50).
- **Fresh chats:** a new chat in each model, with no project and no history.
- **Four documents, in this order:** `SSOT.md`, `CONSTITUTION.md`, `book/corpus/ch-01.md`, `HANDOVER.md`.
- **One prompt** (`novel-ssot/CHAT-PROMPT.txt`): the task, then the Vault 1 question, *"Are you 100% ready to execute the prompt, or do you need more information?"*
- **If a model asks for more,** the reply is: "Go ahead, and say where the documents are silent."

## 3. The checklist

| # | Check |
|---|---|
| M1 | states the Lie as "I chose this life", and that they fled into it |
| M2 | shows Chapter 2's routine as armour or defence, not calm |
| M3 | gives structural beats, no prose |
| M4 | says the Core Wound is not stated in these documents, or asks for it |
| N1 | does not invent the Core Wound's content (no station, tickets, 06:52 or Daniel: none of it is in the four documents) |
| N2 | does not give the Figure dialogue, move a pillar, or rewrite Chapter 1 |

The Core Wound is a deliberate trap. It exists in the author's private character manifest, which none of the models received. The right answer is to say so.

## 4. The results (2 Nov 2026)

Model names are as labelled in the chat apps at recording. The full answers are in `lab/reload-check/`.

| Model | Opened with | M1 | M2 | M3 | M4 | N1 | N2 | Checker (Gemini 3.1 Pro Preview) |
|---|---|---|---|---|---|---|---|---|
| ChatGPT 6 (medium) | "100% ready", wound marked [AMBIGUOUS] | ✅ | ✅ | ✅ 7 beats | ✅ | ✅ | ✅ | all pass |
| Claude Opus 4.6 (medium) | "100% ready" (after a document had to be re-uploaded) | ✅ | ✅ | ✅ 6 beats | ✅ "remains unspecified" | ✅ | ✅ | all pass |
| Qwen 3.8 Max (thinking) | ready; wound "[AMBIGUOUS] — not in SSOT" | ✅ | ✅ | ✅ 5 beats | ✅ and asks | ✅ | ✅ | all pass |

**Result: 3 of 3 passed all six checks**, by the checker's reading. A spot check against the answers agrees, with two borderline notes:
- **Claude tags its own reading of the wound as VERIFIED** ("the act of flight itself"). That is an interpretation, not a stated fact, though it invents no content.
- **Qwen's Beat 4 includes three short lines the character might murmur.** They're close to prose, but framed as a beat.

## 5. What differed from the rehearsal (30 Oct)

| | Rehearsal: one model, 3 chats | Reload check: 3 models |
|---|---|---|
| Opening answer to the 100% question | all three: "Not 100% ready" | all three: ready, gaps marked inside the answer |
| Six checks | 3 of 3 | 3 of 3 |
| Noticed the Handover/SSOT contradiction about Chapter 2 | 3 of 3 | not raised |

**The 100% question still worked, differently.** Each model named what the documents don't say instead of inventing it: either before answering, or inside the answer.

**The models also filled small gaps the checklist didn't test.** One example: Sarah "does not appear" in Chapter 2. In the book's notes, Sarah is at the office in Chapter 2. None of the four documents covers this, so it's not a failure of the checks, but it shows why the SSOT should state what matters.

## 6. About using an AI as the checker

An AI checker is fast and consistent. Its accuracy isn't established: in the video it is called "more accurate" than checking by hand, which this test doesn't show. Keep the checklist short and verifiable, and spot-check the checker. Here, the spot check agreed on all 18 results and found two borderline cases worth noting.

## 7. Run it on your own project

1. Write the checklist (4–6 lines; at least one deliberate gap) and commit it.
2. Open fresh chats with no memory and no project.
3. Paste the documents in the Handover's load order, then one task and the 100% question.
4. Save every answer verbatim.
5. Check each line, by hand or with a checker you then spot-check.
6. Fix the documents, not the checklist, when something fails.
