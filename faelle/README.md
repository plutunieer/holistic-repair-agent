# Case records

The launcher creates one JSON file per error in this directory. It saves the initial report before asking Codex for advice, then appends each response to the same file. A recurrence reopens the existing case and preserves its earlier history. `VORLAGE.json` defines the record format.

The repair adviser itself has read-only access. The local launcher performs the writes. Its status moves from open through confirmed cause, implemented repair, passed tests, and reviewed existing results to closed. A case cannot close until the real user-style test is evidenced and the user has been told about affected existing results.

Start a case from the repository root:

```sh
python3 fehlerbehebung.py start --app "Example App" --goal "Save a draft" --report "The title disappears after reload"
```

Add new findings with `update --case CASE_ID --report "New evidence"`. When the operator has evidence, pass it with `--cause-evidence`, `--implementation-evidence`, `--original-evidence`, `--related-evidence`, `--normal-evidence`, or, after passing tests, `--existing-results-evidence`. The adviser's claims alone cannot mark implementation or tests as complete.

After telling the user which existing results remain affected, record that notification with `mark-informed --case CASE_ID --report "What was communicated"`. Never store credentials in case records. Actual case files are excluded from Git.
