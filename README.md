# Patientenservice – Multi-Agent mit Qualitätsbewertung

**[DE](#deutsch) · [EN](#english)** · Fallstudie auf [denpilot.de](https://denpilot.de/#projekt-patientenservice)

> Bereinigte Kopien: ohne Zugangsdaten, IDs und Kontaktdaten. Fallstudien mit fiktiven Unternehmen, keine Kundendaten.

## Deutsch

### Aufgabe
In einem Klinikverbund bearbeiten 9 Mitarbeitende 400 Anfragen pro Tag: Termine, Preise, Befundstatus, Beschwerden. Standardfälle sollen automatisch beantwortet, riskante an Menschen übergeben und jede Antwort unabhängig geprüft werden. Ziel: mindestens 45 % ohne Mitarbeitende.

### Architektur
1 n8n-Workflow, 62 Knoten, 6 KI-Rollen: Router, 4 Fachagenten, Prüfmodell eines anderen Herstellers. Diagramm: [`docs/architecture.md`](docs/architecture.md).

### Wer macht was
| KI | Code | Mensch |
|---|---|---|
| Anliegen einordnen, Antwortentwurf formulieren, Antworten bewerten | 6 berechenbare Übergabebedingungen, Pseudonymisierung, Stopp bei Datenleck, Gesamtnote als Formel | Freigabe, Änderung oder Ersatz der Antwort; alle Beschwerden |

### Zuverlässigkeit
- Prüfschritt stoppt die Verarbeitung, wenn personenbezogene Daten nicht entfernt wurden.
- Abgelaufene Wartezeit gilt nicht als Freigabe – eigener Zweig und Hinweis an Verantwortliche.
- Niedrige Bewertung → eine Nachbesserungsrunde durch Mitarbeitende.
- Fehler-Workflow meldet Ausfälle sofort.

### DSGVO / KI-Verordnung
Befundinhalte sind Gesundheitsdaten nach Art. 9 Abs. 1 DSGVO und gelangen nicht in das Cloud-Modell. Name, Telefon, E-Mail, Versichertennummer und Geburtsdatum werden vor jedem Modellaufruf durch Platzhalter ersetzt (Pseudonymisierung, Art. 4 Nr. 5 DSGVO).

### Ergebnisse (Testlauf mit 16 Anfragen, gemessen)
16 von 16 richtig weitergeleitet · 0 verpasste Eskalationen · 0 Datenschutzverstöße · **69 %** ohne Mitarbeitende beantwortet (Ziel 45 %).

### Demo starten
1. n8n (self-hosted, aktuelle 1.x/2.x) starten.
2. **Neuen, leeren** Workflow anlegen → Menü **⋯ → Import from File** → JSON aus `workflows/` wählen.
   Wichtig: Import in einen bereits gefüllten Workflow fügt Knoten hinzu, statt ihn zu ersetzen.
3. Zugangsdaten (Credentials) in n8n anlegen und an den markierten Knoten auswählen – im JSON sind sie bewusst leer.
4. Platzhalter ersetzen (siehe `.env.example`): `YOUR_LOCAL_HOST`, `YOUR_CHAT_ID`, `YOUR_SHEET_ID` usw.
5. Erst testen, dann aktivieren. Alle Workflows sind im Export **inaktiv**.
6. Formular-URL des Triggers öffnen und Testanfragen einreichen; Entscheidungen kommen als Telegram-Nachricht mit Knöpfen.

### Grenzen
- 16 Testfälle sind ein Funktionsnachweis, kein Nachweis für den Echtbetrieb.
- Das Cloud-Modell sieht pseudonymisierte Texte; für Echtbetrieb ist eine DPIA und ggf. ein lokales Modell nötig.

---

## English

### Task
In a clinic network, 9 staff handle 400 requests a day: appointments, prices, lab-result status, complaints. Routine cases should be answered automatically, risky ones handed over to people, and every answer checked independently. Target: at least 45 % without staff.

### Architecture
1 n8n workflow, 62 nodes, 6 AI roles: router, 4 specialist agents, a judge model from another vendor. Diagram: [`docs/architecture.md`](docs/architecture.md).

### Who does what
| AI | Code | Human |
|---|---|---|
| Classify the request, draft the answer, score answers | 6 computable hand-over conditions, pseudonymisation, stop on data leak, total score as a formula | Approve, edit or replace the answer; all complaints |

### Reliability
- A check stops processing if personal data was not removed.
- A timeout is not an approval – separate branch and notice to the person in charge.
- Low score → one revision round by staff.
- An error workflow reports failures immediately.

### GDPR / AI Act
Lab-result contents are health data under Art. 9(1) GDPR and never reach the cloud model. Name, phone, e-mail, insurance number and date of birth are replaced by placeholders before every model call (pseudonymisation, Art. 4(5) GDPR).

### Results (test run with 16 requests, measured)
16 of 16 routed correctly · 0 missed escalations · 0 data-protection violations · **69 %** answered without staff (target 45 %).

### Run the demo
1. Start n8n (self-hosted, current 1.x/2.x).
2. Create a **new, empty** workflow → menu **⋯ → Import from File** → pick a JSON from `workflows/`.
   Note: importing into a non-empty workflow adds nodes instead of replacing them.
3. Create the credentials in n8n and select them on the marked nodes – they are deliberately empty in the JSON.
4. Replace the placeholders (see `.env.example`): `YOUR_LOCAL_HOST`, `YOUR_CHAT_ID`, `YOUR_SHEET_ID`, etc.
5. Test first, then activate. All workflows are exported **inactive**.
6. Open the form trigger URL and submit test requests; decisions arrive as Telegram messages with buttons.

### Limitations
- 16 test cases prove function, not production readiness.
- The cloud model sees pseudonymised text; production use needs a DPIA and possibly a local model.

---
Cleaned copies: no credentials, IDs or contact details. Case studies with fictitious companies, no customer data. · Licence: MIT · [LinkedIn](https://www.linkedin.com/in/denys-kopyl-ai-automation/)
