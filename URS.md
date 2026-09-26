# URS · Repair Agent

**Draft · 26 September 2026**

**Goal:** Repair the proven cause across the affected app and reduce the chance of recurrence.

**Roles:** Agent = instructions and case knowledge · App operator = investigate, implement, test · User = steps the operator cannot test.

```mermaid
flowchart TD
    START([Error reported]) --> A["AGENT<br/>Open case · retrieve prior knowledge"]
    DB[("CASE DATABASE<br/>App · error · cause · affected areas<br/>Instructions · outcomes · test evidence · status")]
    A <-->|Read / save| DB
    A --> B["AGENT<br/>Specify investigation and required evidence"]
    B --> C["APP OPERATOR<br/>Reproduce error · investigate cause<br/>Identify affected flows"]
    C --> D{"Cause and scope<br/>proven?"}
    D -->|No · more evidence needed| B
    D -->|Yes| E["AGENT<br/>Specify shared repair and prevention<br/>For all affected parts of this app"]
    E --> F["APP OPERATOR<br/>Repair the shared cause in affected flows"]
    F --> G["AGENT<br/>Specify a real user-style test<br/>Original error · related cases · normal case"]
    G --> H["APP OPERATOR<br/>Use the app as the user would<br/>Real input → actions → visible outcome"]
    H --> I{"Can the operator<br/>test every step?"}
    I -->|No| J["USER<br/>Test only the remaining steps<br/>Pending until feedback arrives"]
    I -->|Yes| K{"Test evidence complete<br/>and passed?"}
    J -->|Feedback with result| K
    K -->|Error found| B
    K -->|Evidence missing| G
    K -->|All passed and proven| L["APP OPERATOR<br/>Only now review existing results:<br/>Where is the error still visible?"]
    L --> M["AGENT<br/>Inform user of affected existing results<br/>No bulk changes"]
    M --> END(["AGENT<br/>Close case · save prevention rule"])
    END --> DB
    END -.->|Recurrence · reopen same case| A

    classDef agent fill:#ddf5ef,stroke:#438c79,color:#153e34,stroke-width:1px;
    classDef work fill:#e6efff,stroke:#668bbf,color:#203b62,stroke-width:1px;
    classDef human fill:#fff0cf,stroke:#b88d36,color:#654b17,stroke-width:1px;
    classDef decision fill:#f3f4f7,stroke:#939bad,color:#313b4d,stroke-width:1px;
    classDef storage fill:#eee9fa,stroke:#8d79b0,color:#44335c,stroke-width:1px;
    class A,B,E,G,M,END agent;
    class C,F,H,L work;
    class J human;
    class D,I,K decision;
    class DB storage;
```

**Required:** Save each step in the case record. Missing evidence must lead to a concrete next instruction. After the user-style test, review existing results and tell the user where the error is still visible. Do not change those results in bulk; decide on remediation separately. “Fixed” refers to the proven repair of the app mechanism. Untested user steps remain open. Preserve all inputs and existing user work.

**Implementation:** `agent.toml` contains the instructions. `fehlerbehebung.py` runs the agent through the local Codex CLI and automatically stores reports and responses in `faelle/`. A case closes only after evidenced testing, a later review of existing results, and documented user notification.
