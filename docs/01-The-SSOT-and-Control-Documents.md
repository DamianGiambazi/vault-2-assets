# The Single Source of Truth and the four control documents

An AI keeps nothing between chats. A project's decisions therefore have to live in documents, not in a conversation. Vault 2's opening line puts it plainly: the AI doesn't have amnesia; we do, because we never wrote anything down.

## 1. What an SSOT is

A Single Source of Truth (SSOT) is one written, agreed version of the facts that everyone works from: you, a collaborator, or an AI session that has never seen the project before.

Think of a family recipe that everyone remembers slightly differently, until someone asks the person who knows, gets the exact recipe and writes it on a card. From then on, the card wins any argument.

## 2. The four control documents

| Document | Governs | Holds | Template |
|---|---|---|---|
| **SSOT** | the project | scope, Locked Decisions, Frozen Constants, the core idea (the spine), rules, what the AI must never do, evidence (labelled as evidence), a changelog | `templates/SSOT-TEMPLATE.md` |
| **Constitution** | the AI | persona, supersession, load order, working rules ("do not relitigate", "do not invent", "when uncertain, ask"), confidence tags | `templates/CONSTITUTION-TEMPLATE.md` |
| **Handover** | the next session | where we are, what to load and in what order, decisions not to reopen, the next action | `templates/HANDOVER-TEMPLATE.md` |
| **Read-Only Agreement** | source material that must not change | analyse, cite, question; never rewrite; flag ambiguity | `templates/READ-ONLY-AGREEMENT.md` |

The novel's filled-in copies are in `novel-ssot/`, and in the public repository with their full history.

## 3. The terms

- **Locked Decision:** a settled choice, with its reason. It isn't reopened without a new decision. Example: *D2, the Figure never speaks.*
- **Frozen Constant:** a fact every part of the project must agree with. Example: *the Figure appears in chapters 1, 7, 19 and 24 only.*
- **Leviathan Lock:** the list of things the AI must never do. Example: *rewrite Chapter 1, alter a Frozen Constant, give the Figure dialogue.*
- **Confidence tags:** VERIFIED (stated in a document), CALIBRATION (reasoned from what is stated), CONJECTURED (a guess, labelled). They let you see at a glance which parts of an AI's answer you can rely on.

## 4. The machine proposes; the author declares

The SSOT records what the author decides the project means. Evidence from a machine, such as the spine search's 1 of 4, can sit inside it, in its own section, under the rule *"evidence informs; it does not decide."* The author's constants stay above it.

## 5. For a technical reader

A novel is a codebase whose files must agree:

| In the novel | In software |
|---|---|
| chapters | source modules |
| character manifest | interface definitions |
| Locked Decisions | architecture decision records |
| Frozen Constants | constants |
| continuity ledger | referential integrity |
| Constitution | linter and CI rules |
| Handover | the README for the next developer |
| vectors | build output, rebuildable from source |

## 6. Documents drift too

Writing things down is not the end of the job. In Vault 2, two drifts were found:
- **The notes disagree with themselves:** the tea box is on the counter in chapter 6 and in a locked drawer in chapters 9 and 20.
- **The documents disagree with each other:** in rehearsal, a fresh chat noticed that the Handover calls Chapter 2 a new deliverable, while the SSOT's evidence shows a Chapter 2 already exists.

The SSOT is what makes such drift visible. A reader with no memory, person or AI, is a good way to find it.
