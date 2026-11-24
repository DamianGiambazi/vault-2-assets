# Field guide to the four templates

The templates in `templates/` are blank versions of the novel's control documents. They work for any project where decisions must survive between sessions: a book, a codebase, a research project, a client engagement. The novel's filled-in copies are in `novel-ssot/`.

## 1. Order of work

1. **SSOT first.** Write what the project is, what is settled and what must never change.
2. **Constitution second.** Tell the AI how to behave around the SSOT.
3. **Read-Only Agreement,** if any source must not be rewritten. It can be a clause inside the Constitution, as in the novel.
4. **Handover last,** at the end of each working session.
5. **Commit all four to git.** Push to GitHub if you want them on other devices.

## 2. Each template, with examples from other domains

### SSOT (`SSOT-TEMPLATE.md`)
| Section | Novel | Software | Research | Client project |
|---|---|---|---|---|
| Identity & Scope | a 24-chapter novel about a comfortable lie | a booking API for three clinics | a review of sleep-study methods, 2015–2025 | a website rebuild for one client |
| Locked Decision | D2: the Figure never speaks | D3: PostgreSQL, not MongoDB | D1: peer-reviewed sources only | D2: no e-commerce in phase 1 |
| Frozen Constant | the Figure appears only in chapters 1, 7, 19, 24 | API version prefix `/v1` | inclusion window 2015–2025 | brand colours as in the style guide |
| Leviathan Lock | never rewrite Chapter 1 | never edit generated files by hand | never cite a paper not in the library | never send drafts to the client without review |

**Tips:**
- One line per decision, each with its reason.
- Keep machine evidence in its own section, labelled.
- Add a changelog row each time the SSOT changes.

### Constitution (`CONSTITUTION-TEMPLATE.md`)
The persona, the load order and the rules. The three rules that did the most work in Vault 2's reload check:
- "Do not invent information not in the SSOT."
- "When uncertain, ask. Do not guess."
- Confidence tags: VERIFIED / CALIBRATION / CONJECTURED.

### Handover (`HANDOVER-TEMPLATE.md`)
Written at the end of a session, for the next one:
- **Where we are:** two or three lines.
- **Load order:** every document the next session needs, in order, including the Handover itself.
- **Decisions not to reopen.**
- **The next action:** one task, and its form ("structural beats only, no prose").

**Keep it consistent with the SSOT.** In rehearsal, a fresh chat noticed that the Handover called Chapter 2 a new deliverable while the SSOT's evidence showed a Chapter 2 already existed.

### Read-Only Agreement (`READ-ONLY-AGREEMENT.md`)
For anything that must stay exactly as written: a finished chapter, a signed contract, a dataset, an interview transcript. The AI may analyse, cite and question it, but never rewrite or "improve" it.

## 3. Test it: the reload check

A Handover works only if a session with no history can pick up the project from the documents alone. See `docs/07-The-Reload-Check.md` for the method used in Vault 2: four documents, one task, the 100% Rule question, and a six-line checklist written and committed before any chat sees the documents.
