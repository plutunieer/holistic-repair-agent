# Holistic Repair Agent

A local Codex adviser for fixing the shared cause of an app error. It guides the app operator; it does not change the app. Each error gets a persistent case record containing the report, confirmed cause, proposed repair, evidence, test results, and any affected existing results.

The adviser and its requirements documents are in English: [`urs.html`](urs.html) and [`URS.md`](URS.md).

## Run

Requires Python 3.11+, the Codex CLI, and macOS or Linux. Sign in to Codex before running it.

```sh
python3 fehlerbehebung.py start --app "Example App" --goal "Save a draft" --report "The title disappears after reload"
```

The command prints the case path and the next instruction. Use `update --case CASE_ID --report "New findings"` for further evidence; see [`faelle/README.md`](faelle/README.md) for the evidence flags and completion rule. Case files stay local and are ignored by Git.

The agent instructions are in [`agent.toml`](agent.toml). The launcher uses the local Codex CLI with read-only access; it removes `ANTHROPIC_API_KEY` from the child environment.
