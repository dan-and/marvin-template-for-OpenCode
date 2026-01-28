# MARVIN Schnellstartanleitung

In 10 Minuten einsatzbereit mit MARVIN.

---

## Voraussetzungen

- OpenCode CLI installiert (https://opencode.ai)
- Grundlegende Kommandozeilen-Kenntnisse
- Git (für Versionskontrolle)

---

## Schritt 1: Template klonen (2 Minuten)

```bash
git clone https://github.com/anomalyco/marvin-template.git ~/marvin
cd ~/marvin
```

---

## Schritt 2: Konfiguration überprüfen (1 Minute)

Überprüfe, dass OpenCode die Konfiguration erkennt:

```bash
opencode --config .opencode/opencode.json
```

Erwartete Ausgabe: Konfiguration wird ohne Fehler geladen.

---

## Schritt 3: Zustandsdateien initialisieren (1 Minute)

Aktualisiere die Zustandsdateien mit deinen Informationen:

**state/current.md** - Füge deine top 3 Prioritäten hinzu:
```markdown
# Aktueller MARVIN-Zustand

## Höchste Prioritäten
1. Meine erste Priorität
2. Meine zweite Priorität
3. Meine dritte Priorität

## Aktive Projekte
- Projekt A
- Projekt B

## Nächster Fokus
Worauf ich als nächstes arbeiten möchte
```

**state/goals.md** - Füge deine Ziele hinzu:
```markdown
# MARVIN-Ziele

## Q1-Ziele
- Ziel 1
- Ziel 2

## Langfristige Ziele
- Ziel 1
```

---

## Schritt 4: Deine erste Sitzung starten (2 Minuten)

Öffne OpenCode und führe aus:

```bash
/marvin
```

Dies wird:
- Deinen Kontext aus Zustandsdateien laden
- Ein persönalisiertes Briefing generieren
- Deine heutige Prioritäten anzeigen
- Deine aktiven Ziele auflisten

---

## Schritt 5: Arbeiten und Checkpointing (3 Minuten)

Während deiner Sitzung, arbeite an Aufgaben. Ab und zu, checkpoint:

```bash
/update
```

Dies speichert deinen Fortschritt im Sitzungsprotokoll.

---

## Schritt 6: Deine Sitzung beenden (1 Minute)

Wenn du fertig bist:

```bash
/end
```

Dies wird:
- Fragen, was du erreicht hast
- Deine Sitzung zusammenfassen
- Alles in sessions/YYYY-MM-DD.md speichern
- Deine Zustandsdateien aktualisieren

---

## Deine ersten Befehle

### `/marvin` - Sitzung starten
Lädt Kontext und generiert Briefing.
```bash
/marvin
```

### `/update` - Checkpoint
Speichert Fortschritt während der Sitzung.
```bash
/update
```

### `/report` - Wöchentlicher Bericht
Generiert Synthese der Arbeit aus der letzten Woche.
```bash
/report
```

### `/commit` - Git-Workflow
Committed Änderungen mit Sicherheitsbestätigung.
```bash
/commit
```

### `/end` - Sitzung beenden
Fasst zusammen und speichert Sitzung.
```bash
/end
```

### `/sync` - Template synchronisieren
Aktualisiert Template mit neuesten Änderungen (fragt um Bestätigung).
```bash
/sync
```

---

## Agenten wechseln

Während deiner Sitzung, drücke **Tab** um zwischen Agenten zu wechseln:

- **MARVIN** (Standard) - Vollständige Funktionen, alle Tools aktiviert
- **PLANNER** (Analyse) - Schreibgeschützter Modus, perfekt zum Planen

Beispiel-Workflow:
1. Beginne mit MARVIN für vollständige Funktionen
2. Drücke Tab um zu PLANNER zu wechseln für Analyse
3. Drücke Tab erneut um zu MARVIN zurückzuwechseln

---

## Dateistruktur

Dein MARVIN-Arbeitsbereich hat diese Struktur:

```
marvin/
├── .opencode/              # OpenCode-Konfiguration
│   ├── opencode.json      # Agent- und Befehl-Definitionen
│   ├── prompts/           # Agent-Systeminstruktionen
│   ├── commands/          # Befehls-Templates
│   └── skills/            # Wiederverwendbare Skill-Module
│
├── state/                 # Dein persistenter Zustand
│   ├── current.md        # Aktuelle Prioritäten
│   ├── goals.md          # Aktive Ziele
│   └── todos.md          # Aufgaben-Tracking
│
├── sessions/             # Sitzungsprotokolle (automatisch erstellt)
├── reports/              # Wöchentliche Berichte (automatisch erstellt)
├── content/              # Inhaltverfolgung
│
└── Dokumentationsdateien
    ├── OPENCODE.md       # Systemdokumentation
    ├── INSTALLATION.md   # Detaillierte Einrichtungsanleitung
    ├── ARCHITECTURE.md   # Technische Referenz
    └── AGENTS.md         # Agent-System-Anleitung
```

---

## Häufige Workflows

### Täglicher Workflow

```bash
# Morgen: Sitzung starten
/marvin

# Während des Tages: Fortschritt checkpointing
/update

# Ende des Tages: Sitzung speichern
/end
```

### Wöchentlicher Workflow

```bash
# Freitag Nachmittag
/report

# Wöchentliche Zusammenfassung überprüfen
# Ziele und Prioritäten für nächste Woche aktualisieren
```

### Bei Änderungen

```bash
# Mache Änderungen an Dateien
vim state/current.md

# Committe Änderungen
/commit

# Dies fragt nach deiner Commit-Nachricht und Bestätigung
```

### Beim Analysieren

```bash
# Drücke Tab um zu PLANNER-Agent zu wechseln (schreibgeschützter Analysemodus)
Tab

# Stelle Fragen wie:
# "Was sollte ich priorisieren?"
# "Ist dieser Ansatz machbar?"
# "Wie ist mein Fortschritt gegenüber den Zielen?"

# Drücke Tab erneut um zu MARVIN zurückzuwechseln wenn bereit zur Ausführung
Tab
```

---

## Tipps & Best Practices

### 1. Halte Zustandsdateien aktuell
Aktualisiere `state/current.md` regelmäßig damit Briefings die aktuelle Realität widerspiegeln.

### 2. Nutze Checkpoints
`/update` häufig während langer Sitzungen um Fortschritt inkrementell zu speichern.

### 3. Wöchentliche Berichte
Führe `/report` jeden Freitag aus um deine Wochenarbeit zu synthetisieren.

### 4. Nutze PLANNER-Agent
Drücke Tab um zu PLANNER zu wechseln für Analyse ohne das Risiko von Änderungen.

### 5. Git-Integration
Jede Sitzung wird in Git protokolliert. Du kannst die Historie überprüfen:
```bash
git log --oneline state/
git log --oneline sessions/
```

### 6. Lese die Dokumentation
- Schnellereferenz: Diese Datei
- Detaillierte Einrichtung: INSTALLATION.md
- System-Anleitung: OPENCODE.md
- Technische Details: ARCHITECTURE.md

---

## Fehlerbehebung

### Befehle funktionieren nicht

**Problem**: `/marvin` oder andere Befehle werden nicht erkannt

**Lösung**: 
1. Stelle sicher, dass du im marvin-Verzeichnis bist: `cd ~/marvin`
2. Überprüfe, dass OpenCode läuft: `opencode`
3. Überprüfe Konfiguration: `.opencode/opencode.json` sollte existieren

### Agent antwortet nicht

**Problem**: Agent scheint stecken zu bleiben oder antwortet nicht

**Lösung**:
1. Versuche einen einfacheren Befehl wie `/update`
2. Überprüfe Agent-Status (Tab-Taste sollte aktuellen Agent anzeigen)
3. Beende und starten neu: `exit`, dann `opencode` erneut

### Zustandsdateien werden nicht aktualisiert

**Problem**: Änderungen an Zustandsdateien erscheinen nicht in Briefings

**Lösung**:
1. Stelle sicher, dass Dateien gespeichert sind: `:w` im Editor
2. Beende MARVIN und starte neue Sitzung: `/end`, dann `/marvin`
3. Überprüfe, dass Dateipfade korrekt im state/-Verzeichnis sind

### Git-Fehler

**Problem**: `/commit` sagt Berechtigung verweigert oder anderer Git-Fehler

**Lösung**:
1. Überprüfe, dass du Git-Anmeldedaten hast: `git config --global user.name`
2. Stelle sicher, dass du in einem Git-Repository bist: `git status`
3. Versuche erneut: `/commit`

---

## Nächste Schritte

1. **Passe deine Einrichtung an**
   - Bearbeite Zustandsdateien mit deinen Zielen und Prioritäten
   - Lese INSTALLATION.md für API-Konfiguration

2. **Lerne das System**
   - Lese OPENCODE.md für vollständige Systemdokumentation
   - Überprüfe AGENTS.md um Agent-System zu verstehen

3. **Erkunde Features**
   - Versuche alle 6 Befehle
   - Wechsle zwischen Agenten mit Tab-Taste
   - Überprüfe deine Sitzungsprotokolle

4. **Lese vollständige Dokumentation**
   - INSTALLATION.md - Vollständige Einrichtungsanleitung
   - ARCHITECTURE.md - Technische Architektur
   - TESTING.md - Test-Verfahren

---

## Schlüsselkonzepte

**Agent**: KI-Assistent mit spezifischen Funktionen und Einschränkungen
- MARVIN: Haupt-Agent mit allen Funktionen
- PLANNER: Schreibgeschützter Analyse-Agent

**Befehl**: Benutzeraktion, die MARVIN aufruft (wie `/marvin`, `/end`)

**Skill**: Wiederverwendbare Fähigkeit, die Agenten nutzen können

**Zustand**: Deine persistenten Prioritäten, Ziele und Aufgaben

**Sitzung**: Eine Arbeitsperiode (beginnt mit `/marvin`, endet mit `/end`)

**Briefing**: Zusammenfassung deines Kontexts, deiner Prioritäten und deines Fortschritts

---

## Hilfe bekommen

**Bei Problemen**:
1. Überprüfe Fehlerbehebungsabschnitt in OPENCODE.md
2. Überprüfe AGENTS.md für Agent-spezifische Hilfe
3. Melde Probleme unter: https://github.com/anomalyco/opencode

**Bei Fragen**:
1. Lese ARCHITECTURE.md für technische Fragen
2. Lese INSTALLATION.md für Fragen zur Einrichtung
3. Überprüfe AGENTS.md für Agent-bezogene Fragen

---

## Was kommt als Nächstes?

Sobald du mit den Grundlagen vertraut bist:

1. **Passe Agenten an**: Bearbeite `.opencode/prompts/marvin.txt` um MARVINs Persönlichkeit anzupassen
2. **Erstelle Skills**: Füge neue Skills in `.opencode/skills/[name]/SKILL.md` hinzu
3. **Füge Befehle hinzu**: Definiere neue Befehle in `.opencode/opencode.json`
4. **Konfiguriere Integrationen**: Richte MCP-Server für externe Tools ein

Siehe ARCHITECTURE.md für Erweiterungspunkte und Anpassungen.

---

## Unterstützung & Feedback

- **OpenCode Dokumentation**: https://opencode.ai/docs
- **GitHub Issues**: https://github.com/anomalyco/opencode
- **Dieses Template**: [Repository-URL]

Viel Spaß mit MARVIN! 🚀
