# Vault 2 — Video descriptions and lesson blurb

## A. Full video — *Reproducible and Portable Truth: Finding the Spine, Then Making It Travel*

Why does an AI forget your project halfway through a conversation? Because nothing was written down where it has to read it. In this vault I take a 24-chapter teaching novel, whose sixth chapter answered the Vault 1 capstone, and turn its notes into a book with written rules. I name its spine, seal four predictions in git, and let the local stack from Vault 1 search it by meaning: vector-snake cuts the chapters, DuckDB audits the run, LanceDB finds the closest passages. One prediction holds and three don't.

Along the way: how the cuts are chosen, the difference between finding images and finding meaning, and a Single Source of Truth that records the author's decisions with the machine's evidence underneath. I push it to GitHub, add a rule from my phone and pull it down. To finish, a fresh clone rebuilds every passage and matches the original exactly.

Correction: the spine search uses no language model; LanceDB compares vectors (see the errata sheet).

## B. Short video — *Vault 2: The Reload Check*

The main session ran long, which is the problem this vault is about, so the last test gets its own video. I stop and reload the project from four documents on GitHub: the Single Source of Truth, the Constitution, Chapter 1 and the Handover. Three AI models (Claude, ChatGPT and Qwen) each get the same task and the Vault 1 question, "Are you 100% ready?" A fourth model checks their answers against six rules written and committed beforehand. All three get the lie and the armour right, and none invents what isn't written. The AI didn't remember; the documents did.

## C. Lesson blurb

**Vault 2 — Control Document Templates (The SSOT Library).** Learn to write down what your AI keeps forgetting, seal it in git and make it travel: a Single Source of Truth any fresh session, collaborator or machine can pick up.

In this vault:
- **The four templates:** SSOT, Constitution, Handover and Read-Only Agreement, with a field guide and the novel's filled-in examples.
- **The novel as a corpus:** 24 chapters copied from draft notes, and the rule that keeps writer's labels out of a search.
- **Structure and meaning:** how vector-snake cuts at paragraph breaks, why 800 characters, and why a keyword can't read "not".
- **Sealed predictions:** git as a notary, the seal check, and a DuckDB gate that refuses a bad run.
- **GitHub from zero:** the first push, an edit from a phone pulled back down, and a first-push guide covering every sign-in route.
- **The reload check:** three AI models with no memory, tested against a six-line checklist, plus a fourth as checker.
- **The evidence:** the spine run (one prediction of four held, identical on two days) and a fresh clone that rebuilt every passage exactly, in the public repository `github.com/DamianGiambazi/novel-ssot`.

You'll finish able to write control documents an AI must follow, version them with git and GitHub, test whether a new session really inherits them, and tell similarity from significance.
