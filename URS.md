# URS · Fehlerbehebungsagent

**Entwurf · 26.09.2026**

**Ziel:** Die belegte Fehlerursache in der gesamten betroffenen App beheben und gegen Wiederholung absichern.

**Rollen:** Agent = Anweisungen und Fehlerwissen · App-Bearbeiter = untersuchen, umsetzen, testen · Nutzer = offene Testschritte.

```mermaid
flowchart TD
    START([Fehler gemeldet]) --> A["AGENT<br/>Fall anlegen · Fehlerwissen abrufen"]
    DB[("FEHLERDATENBANK<br/>App · Fehler · Ursache · betroffene Stellen<br/>Anweisungen · Ergebnisse · Testbelege · Status")]
    A <-->|Lesen / speichern| DB
    A --> B["AGENT<br/>Untersuchung und benötigte Belege vorgeben"]
    B --> C["APP-BEARBEITER<br/>Fehler nachstellen · Ursache untersuchen<br/>Betroffene Abläufe ermitteln"]
    C --> D{"Ursache und Umfang<br/>belegt?"}
    D -->|Nein · Belege fehlen| B
    D -->|Ja| E["AGENT<br/>Behebung und Vorbeugung anweisen<br/>Für alle betroffenen Stellen der App"]
    E --> F["APP-BEARBEITER<br/>Gemeinsame Ursache in den betroffenen Abläufen korrigieren"]
    F --> G["AGENT<br/>Realen Nutzertest vorgeben<br/>Fehlerfall · verwandte Fälle · Normalfall"]
    G --> H["APP-BEARBEITER<br/>App wie der Nutzer bedienen<br/>Echte Eingabe → Ablauf → sichtbares Ergebnis"]
    H --> I{"Alle Schritte<br/>selbst testbar?"}
    I -->|Nein| J["NUTZER<br/>Nur offene Schritte nach Anleitung testen<br/>Bis zur Rückmeldung: ungeprüft"]
    I -->|Ja| K{"Testnachweise?"}
    J -->|Rückmeldung mit Ergebnis| K
    K -->|Fehler gefunden| B
    K -->|Nachweis fehlt| G
    K -->|Alle bestanden und belegt| L["APP-BEARBEITER<br/>Erst jetzt vorhandene Ergebnisse sichten:<br/>Wo ist der Fehler aktuell noch sichtbar?"]
    L --> M["AGENT<br/>Nutzer über verbleibende betroffene Ergebnisse informieren<br/>Keine pauschale Nachbearbeitung"]
    M --> END(["AGENT<br/>Fall abschließen · Vorbeugung speichern"])
    END --> DB
    END -.->|Fehler tritt erneut auf · Fall wieder öffnen| A

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

**Verbindlich:** Jeder Schritt wird im Fehlerfall gespeichert. Fehlende Belege führen zu einer konkreten nächsten Anweisung. Nach dem Nutzertest werden vorhandene Ergebnisse auf noch sichtbare Fehler geprüft und dem Nutzer benannt. Sie werden nicht pauschal geändert; über eine Nachbearbeitung wird gesondert entschieden. „Behoben“ bezieht sich auf die nachweislich korrigierte App-Ursache; offene Nutzertests bleiben offen. Eingaben und vorhandene Nutzerarbeit bleiben erhalten.

**Umsetzung:** `agent.toml` enthält die Anweisungen. `fehlerbehebung.py` startet den Agenten über die lokale Codex-CLI und speichert Meldungen sowie Antworten automatisch in `faelle/`. Ein Fall wird erst nach belegtem Nutzertest, späterer Sichtung bestehender Ergebnisse und dokumentierter Nutzerinformation abgeschlossen.
