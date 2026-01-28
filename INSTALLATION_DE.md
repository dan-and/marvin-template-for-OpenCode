# MARVIN auf OpenCode - Installationsanleitung

**Version**: 1.0.0 (OpenCode Edition)  
**Datum**: 28. Januar 2026  
**Kompatibilität**: OpenCode CLI/TUI

---

## Übersicht

MARVIN ist ein KI-Chef-of-Staff-System, das dir dabei hilft, effizienter zu arbeiten. Diese Anleitung führt dich durch die Installation von MARVIN für OpenCode, das Einrichten deiner Umgebung und den Einstieg in deine erste Sitzung.

**Was du benötigst**:
- **OpenCode CLI/TUI** installiert (siehe [Installation](#installation))
- **git** (zur Verwaltung deines Repositories)
- **Dein Lieblings-Terminal** (zum Ausführen von OpenCode)

**Was du erhältst**:
- Vollständiges MARVIN-KI-System mit 4 spezialisierten Agenten
- 6 Slash-Befehle zur Sitzungsverwaltung
- 8 modulare Skills für erweiterte Funktionalität
- OpenCode MCP-Integration für externe Tools
- Zustandsverwaltung und Zielerfassung

**Einrichtungszeit**: 5 Minuten

---

## Schnelleinstieg

### 1. OpenCode installieren

```bash
# macOS oder Linux
curl -fsSL https://opencode.ai/install | bash

# Mit Homebrew (empfohlen)
brew install anomalyco/tap/opencode

# Mit NPM
npm i -g opencode-ai@latest

# Windows
winget install Anthropic.ClaudeCode
```

**Installation überprüfen**:
```bash
opencode version
```

Sollte `opencode-ai/[version]` anzeigen

### 2. MARVIN-Template klonen

```bash
git clone https://github.com/anomalyco/marvin-template.git
cd marvin-template
```

### 3. API-Schlüssel konfigurieren

```bash
# Falls du das OpenCode Zen-Modell verwendest (empfohlen)
# Kein API-Schlüssel erforderlich - zen/big-pickle ist immer verfügbar

# Falls du andere Provider verwendest (Anthropic, OpenAI, etc.)
# Umgebungsvariable setzen oder in opencode.json konfigurieren

# Beispiel (Anthropic)
export ANTHROPIC_API_KEY="dein-api-schlüssel-hier"
```

### 4. Deine erste Sitzung starten

```bash
cd marvin-template
opencode
```

Die OpenCode TUI lädt die Konfiguration und präsentiert den MARVIN-Agenten.

**Tippe**: `/marvin` (Sitzung mit Briefing starten)

Deine erste Sitzung wird:
- Deine aktuelle Prioritäten und Ziele laden
- Das heutige Sitzungsprotokoll überprüfen
- Ein personalisiertes morgendliches Briefing generieren
- Kontext für produktive Arbeit setzen

---

## Konfiguration

### OpenCode-Konfiguration

MARVINs Konfiguration befindet sich in `.opencode/opencode.json`. Die wichtigsten Einstellungen sind:

**Modelle**:
- Standard: `zen/big-pickle` (OpenCodes Standardmodell, immer verfügbar)
- Kann in der Konfiguration auf andere Modelle geändert werden

**Agenten**:
- **marvin** (Primär, 0,3°) - Vollständige Funktionen für allgemeine Arbeit
- **planner** (Primär, 0,1°) - Schreibgeschützte Analyse ohne Änderungen
- **executor** (Subagent, 0,3°) - Schnelle Implementierung
- **briefing** (Subagent, 0,1°) - Sitzungs-Briefing

**Berechtigungssystem** (3-stufig: Zulassen/Fragen/Ablehnen):
- **Zulassen**: Lesevorgänge (grep, find, git log, ls), webfetch
- **Fragen**: Dateiänderungen, git push, E-Mail/Nachrichten
- **Ablehnen**: git force-push, unsichere Löschungen

**Befehle** (6 Slash-Befehle):
- `/marvin` - Sitzung mit Briefing starten
- `/end` - Sitzung beenden und Fortschritt speichern
- `/update` - Schneller Checkpoint
- `/report` - Wöchentliche Zusammenfassung
- `/commit` - Git-Workflow
- `/sync` - Template-Updates

**Skills** (8 modulare Funktionen):
- Sitzungsverwaltung
- Briefing-Generierung
- Git-Workflow
- Inhaltverfolgung
- Und mehr!

### Benutzerdefinierte Konfiguration

Du kannst MARVIN anpassen, indem du `.opencode/opencode.json` bearbeitest:

```json
{
  "models": {
    "default": "dein-bevorzugtes-modell"
  }
}
```

---

## Zustandsverwaltung

MARVIN verwendet diese Zustandsdateien, um Kontinuität zu gewährleisten:

### Zustandsdateien

```
marvin-template/
├── .opencode/              [OpenCode-Konfiguration]
├── state/                 [Deine persönlichen Daten]
│   ├── current.md         [Aktive Prioritäten, offene Threads]
│   ├── goals.md           [Deine Ziele]
│   └── todos.md           [Aufgabenliste - optional]
├── sessions/               [Tägliche Sitzungsprotokolle]
│   └── YYYY-MM-DD.md     [Pro Tag]
├── content/               [Inhaltverfolgung - optional]
│   └── log.md             [Versendetes Inhaltsprotokoll]
└── reports/               [Wöchentliche Berichte - optional]
    └── YYYY-MM-DD.md     [Wöchentliche Zusammenfassungen]
```

### Anfangskonfiguration

Bei der ersten Verwendung erstellt MARVIN Sitzungsdateien, falls diese nicht vorhanden sind:

1. **state/current.md** - Vorlage für deine Prioritäten
2. **state/goals.md** - Vorlage für deine Ziele
3. **state/todos.md** - Vorlage für deine Aufgabenliste (optional)

Diese Vorlagen befinden sich im Repository, so dass MARVIN sofort mit der Struktur beginnt.

---

## Grundbefehle

### Sitzungsverwaltung

#### /marvin
Starte eine neue MARVIN-Sitzung mit Briefing.

**Was es tut**:
- Lädt deine aktuelle Prioritäten und Ziele
- Überprüft das Sitzungsprotokoll von heute
- Generiert ein personalisiertes morgendliches Briefing
- Setzt Kontext für produktive Arbeit

**Wann zu verwenden**:
- Zu Beginn deines Tages
- Wenn du die Arbeit nach einer Pause fortfährst
- Bei Beginn eines neuen Projekts

#### /end
Beende deine aktuelle MARVIN-Sitzung.

**Was es tut**:
- Fasst die Konversation zusammen
- Aktualisiert Zustandsdateien
- Speichert im Sitzungsprotokoll
- Erstellt optional Commits

**Wann zu verwenden**:
- Wenn du für den Tag fertig bist
- Beim Schließen deines Arbeitsbereichs
- Bevor du für längere Zeit weg gehst

#### /update
Schneller Checkpoint während der Sitzung.

**Was es tut**:
- Speichert Fortschritt im Sitzungsprotokoll
- Keine Zustandsdatei-Updates (leichtgewichtig)
- Erhält Kontext während der Sitzung

**Wann zu verwenden**:
- Nach Abschluss eines großen Stücks Arbeit
- Vor Kontextwechsel
- Alle Stunde oder so während langer Sitzungen

#### /report
Generiere wöchentlichen Zusammenfassungsbericht.

**Was es tut**:
- Synthetisiert Arbeit über alle Sitzungen der letzten Woche
- Überprüft Zielfortschritt
- Listet versendete Inhalte auf
- Identifiziert Blocker und Probleme
- Bietet nächste Schritte

**Wann zu verwenden**:
- Am Ende jeder Woche
- Bevor du eine neue Woche anfängst

#### /commit
Überprüfe und committiere git-Änderungen.

**Was es tut**:
- Gruppiert Änderungen logisch
- Erstellt fokussierte Commits
- Verwendet Conventional-Commit-Nachrichten
- Unterstützt sicheren Git-Workflow

**Wann zu verwenden**:
- Nach Abschluss eines Features oder einer Aufgabe
- Am Ende einer Arbeitssitzung
- Vor dem Push zum Remote

#### /sync
Synchronisiere mit MARVIN-Template-Updates.

**Was es tut**:
- Überprüft auf Updates vom MARVIN-Template-Repository
- Wendet nur sichere, nicht-destruktive Änderungen an
- Behält alle deine persönlichen Daten (state/, sessions/)
- Fragt immer vor Änderungen um Erlaubnis

**Wann zu verwenden**:
- Gelegentlich um neue Features zu bekommen
- Wenn sich die Projektstruktur erheblich ändert
- Monatlich oder vierteljährlich um aktuell zu bleiben

---

## Skills

MARVIN umfasst 8 Skills, die Funktionalität erweitern:

### Core Skills

**marvin** (Sitzungsstart)
- Automatische Sitzungsinitialiserung
- Kontext-Laden und Briefing-Generierung

**end** (Sitzungsende)
- Sitzungszusammenfassung und Zustandssparen
- Sitzungsprotokoll-Verwaltung

**daily-briefing** (Morgen-Briefing)
- Tägliches Kontext-Laden und Briefing
- Fortschrittsermittlung
- Alert-Generierung

**commit** (Git-Workflow)
- Änderungsgruppierung und fokussierte Commits
- Conventional-Commit-Nachrichten
- Sicherer Git-Workflow

**update** (Sitzungs-Checkpoint)
- Leichte Mid-Session-Saves
- Fortschrittskonservierung
- Kontext-Wartung

**report** (Wöchentlicher Bericht)
- Sitzungssynthese über alle Sitzungen
- Zielfortschritt-Tracking
- Versendete Inhalte und Erfolge

**content-shipped** (Inhaltverfolgung)
- Inhaltsprotokollierung
- Ziel-Assoziation

### Wie man Skills verwendet

Skills werden automatisch von MARVIN-Agenten geladen. Du kannst auch spezifische Skills nach Namen aufrufen:

- Gib den Skill-Namen direkt in die Konversation ein
- MARVIN findet und lädt den Skill
- Skills können auch von Agenten automatisch aufgerufen werden

---

## Integrationen

MARVIN kann sich über OpenCodes MCP (Model Context Protocol) System mit externen Diensten verbinden.

### Verfügbare Integrationen

- **Google Workspace** - Gmail-, Kalender-, Drive-Zugriff
- **Microsoft 365** - Outlook-, Teams-, OneDrive-Zugriff
- **Atlassian** - Jira-, Confluence-Zugriff
- **Slack/Teams/Discord** - Messaging und Zusammenarbeit
- **Parallel Search** - Web-Suche und Inhalts-Fetching

### Einrichtung

Integrationen werden in OpenCodes globaler Konfiguration konfiguriert. MARVIN kann dann auf diese Dienste über MCP-Server zugreifen.

**Anmerkung**: Du konfigurierst Integrationen, nicht MARVIN. MARVIN verwendet die Integrationen, die du in OpenCode eingerichtet hast.

---

## Agenten-Umschaltung

MARVIN nutzt ein Multi-Agent-System für verschiedene Aufgaben.

### Primäre Agenten

Wechsle mit der `Tab`-Taste, um primäre Agenten zu durchlaufen:

- **marvin** (Standard) - Vollständige Funktionen, allgemeine Arbeit
- **planner** (Analyse) - Schreibgeschützt, keine Änderungen

### Subagenten

Agenten, die direkt aufgerufen oder von anderen Agenten aufgerufen werden können:

- **executor** - Schnelle Implementierung geplanter Aufgaben
- **briefing** - Sitzungs-Briefing-Generierung

### Wie man Agenten verwendet

1. **Auto-Umschaltung** - MARVIN kann Agenten-Wechsel basierend auf Kontext vorschlagen
2. **Manuelle Umschaltung** - Drücke `Tab` um durch primäre Agenten zu durchlaufen
3. **Subagenten-Aufgerufe** - Gib `@executor` oder `@briefing` ein um direkt aufzurufen

### Temperatur-Einstellungen

Jeder Agent nutzt eine spezifische Temperatur für optimale Leistung:

- **marvin**: 0,3° - Ausgewogen, kreativ aber zuverlässig
- **planner**: 0,1° - Hochdeterministisch, fokussiert
- **executor**: 0,3° - Handlungsorientiert, effizient
- **briefing**: 0,1° - Faktisch, konsistent

---

## Sicherheit

MARVIN ist standardmäßig sicher konzipiert. Hochrisiko-Operationen erfordern immer explizite Bestätigung.

### Was Bestätigung erfordert

Vor MARVINs Ausführung siehst du Bestätigungsaufforderungen für:

- **E-Mail/Nachrichten-Versand** - "Ich bin dabei, eine E-Mail an [Empfänger] zu senden. Soll ich fortfahren?"
- **Dateiänderungen** - "Ich bin dabei, [Datei] zu löschen/zu überschreiben. Soll ich fortfahren?"
- **Ticket-Erstellung** - "Ich bin dabei, Jira-Ticket [Schlüssel] zu erstellen. Soll ich fortfahren?"
- **Git Push** - "Ich bin dabei, Änderungen zu pushen. Soll ich fortfahren?"
- **Inhalts-Veröffentlichung** - "Ich bin dabei, [Inhalt] zu veröffentlichen. Soll ich fortfahren?"

### Was keine Bestätigung erfordert

- Dateien lesen
- Code durchsuchen (grep, find)
- Git-Status-Überprüfungen
- Kontext laden
- Sitzungsprotokolle anzeigen
- Web-Recherche

### Abgelehnte Operationen

MARVIN wird es ablehnen auszuführen:

- git force-push (kann nicht wiederhergestellt werden)
- rm ohne Schutzmaßnahmen
- Unsichere Löschungen

---

## Fehlerbehebung

### Häufige Probleme

**MARVIN antwortet nicht**:
- Überprüfe, dass OpenCode läuft
- Versuche, OpenCode neu zu starten: `opencode` (beenden und erneut starten)

**Konfiguration wird nicht geladen**:
- Überprüfe, dass `.opencode/opencode.json` gültig JSON ist
- Überprüfe, dass alle Pfade in der Konfiguration korrekt sind

**Skills werden nicht geladen**:
- Überprüfe, dass Skill-Dateien in `.opencode/skills/` vorhanden sind
- Überprüfe, dass YAML-Vorspann korrekt ist

**Zustandsdateien nicht zugänglich**:
- Überprüfe Berechtigungen im `state/`-Verzeichnis
- Überprüfe, dass Dateien lesbar sind

### Hilfe erhalten

- Tritt dem OpenCode Discord bei: https://opencode.ai/discord
- Überprüfe die Dokumentation: `cat MARVIN.md`
- Überprüfe `AGENTS.md` für Agent-System-Details
- Siehe den Migrations-Plan: `cat MIGRATION_PLAN.md`

---

## Nächste Schritte

1. **Installiere OpenCode** (5 Minuten)
2. **Klone MARVIN-Template** (2 Minuten)
3. **Starte deine erste Sitzung** (1 Minute)
    ```bash
    cd marvin-template
    opencode
    ```
    Gib `/marvin` ein

4. **Los geht's!**

MARVIN wird deinen Kontext laden, ein Briefing präsentieren und dir helfen, einen produktiven Tag zu haben.

---

## Unterstützung

Für Probleme, Fragen oder Feature-Anfragen:

- **GitHub Issues**: https://github.com/anomalyco/marvin-template/issues
- **OpenCode Discord**: https://opencode.ai/discord
- **Dokumentation**: Siehe `MARVIN.md` für vollständige Systemreferenz

---

**Willkommen bei MARVIN auf OpenCode!** 🚀

Dein KI-Chef-of-Staff ist bereit, dir zu helfen, effizienter zu arbeiten. Lass uns beginnen!
