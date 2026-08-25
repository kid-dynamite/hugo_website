+++
date = '2026-07-30T00:17:03+02:00'
draft = true
title = 'Project'
+++

# Projekt-Spezifikation: Modularer CLI AI-Agent & Prompt-Engineering Pipeline-Framework (Simuliert)

Dieses Projekt dient als "First Personal Project" für Boot.dev. Es ist speziell darauf ausgelegt, fortgeschrittene OOP-Konzepte in Python zu festigen und gleichzeitig ein starkes Fundament für eine spätere Spezialisierung im Bereich **LLMOps (Large Language Model Operations)** zu legen.

---

## 🎯 Kernziele & Lerninhalte

- **Objektorientierte Programmierung (OOP):** Intensiver Einsatz von abstrakten Basisklassen (Abstraktion), Vererbung, Polymorphie und Komposition.
- **LLMOps-Konzepte:** Textverarbeitung Pipelines, Token-/Wort-Schätzung, Modell-Abstraktion (Mocking) und strukturiertes Tracing/Logging.
- **Clean Code & CLI:** Strukturierung eines konsolenbasierten Backend-Tools ohne externe GUI-Bibliotheken.

---

## 🏗️ System-Architektur (Klassenstruktur)

### 1. Daten-Prozessoren (Vererbung & Abstraktion)

- **`BaseProcessor` (Abstrakte Basisklasse):** Definiert das Interface mit der abstrakten Methode `process(text: str) -> str`.
- └── **`TextCleaner` (Klasse):** Erbt von `BaseProcessor`. Bereinigt Whitespaces, Sonderzeichen oder konvertiert Text in Lowercase.
- └── **`PromptFormatter` (Klasse):** Erbt von `BaseProcessor`. Bettet den Text in konfigurierbare Prompt-Templates ein.
- └── **`TokenEstimator` (Klasse):** Erbt von `BaseProcessor`. Simuliert die Tokenisierung, zählt Wörter und fügt Metadaten hinzu.

### 2. Modell-Infrastruktur (Vererbung & Polymorphie)

- **`BaseModelLLM` (Abstrakte Basisklasse):** Definiert die Attribute (`model_name`, `temperature`) und die abstrakte Methode `generate(prompt: str) -> str`.
- └── **`MockGPT4` (Klasse):** Erbt von `BaseModelLLM`. Simuliert komplexe, ausführliche Antworten basierend auf Keyword-Mustern im Prompt.
- └── **`MockClaude3` (Klasse):** Erbt von `BaseModelLLM`. Simuliert präzise, strukturierte Antworten.
- └── **`MockLlama3` (Klasse):** Erbt von `BaseModelLLM`. Simuliert schnelle, kurze Antworten (Open-Source-Stil).

### 3. Orchestrierung & Überwachung (Komposition & Dateisystem)

- **`LLMOpsPipeline` (Hauptklasse):** Hält Instanzen von Prozessoren und einem ausgewählten Modell (Komposition).
  - `add_processor(processor)`: Fügt Schritte zur Pipeline hinzu.
  - `run(raw_input)`: Schiebt Daten durch die Kette, misst die Zeit und gibt ein strukturiertes Ergebnis zurück.
  - `_write_trace_log()`: Speichert jeden Durchlauf als strukturiertes JSON-Objekt in einer lokalen Log-Datei ab.

---

## 🗺️ Der 4-Meilenstein-Fahrplan (20-40 Stunden)

### 📈 Meilenstein 1: Das OOP-Grundgerüst

- [ ] Erstelle die Datei- und Ordnerstruktur für dein Projekt.
- [ ] Implementiere das `abc` (Abstract Base Classes) Modul für `BaseProcessor` und `BaseModelLLM`.
- [ ] Schreibe die ersten konkreten Subklassen (`TextCleaner`, `MockGPT4`, `MockLlama3`).
- [ ] Teste die Polymorphie: Stelle sicher, dass die Pipeline flexibel mit jedem Modell aufgerufen werden kann.

### 💻 Meilenstein 2: CLI-Schnittstelle & Konfiguration

- [ ] Integriere Pythons `argparse` Modul.
- [ ] Erlaube dem Nutzer, das Modell per Flag auszuwählen (z. B. `--model gpt4` oder `--model llama3`).
- [ ] Füge Flags hinzu, um bestimmte Pipeline-Schritte optional an- oder auszuschalten (z. B. `--skip-cleaning`).
- [ ] Implementiere eine interaktive Eingabeaufforderung, falls keine CLI-Argumente übergeben wurden.

### 📊 Meilenstein 3: LLMOps Tracing & Log-Analysen

- [ ] Erstelle ein strukturiertes JSON-Ausgabeformat für jeden Pipeline-Lauf.
- [ ] Erfasse Metriken: Startzeit, Endzeit, berechnete Ausführungsdauer und Zeichen-/Wortanzahl (Eingabe vs. Ausgabe).
- [ ] Implementiere das automatische Schreiben in eine lokale Datei (z. B. `telemetry_logs.json`) im Append-Modus.
- [ ] Schreibe eine kleine Analyse-Funktion, die dem Nutzer im Terminal die durchschnittliche Verarbeitungszeit aller bisherigen Läufe ausgibt.

### 🧪 Meilenstein 4: Das Evaluation Framework

- [ ] Erstelle eine Klasse `LLMEvaluator`.
- [ ] Entwickle Testkriterien (Assertions) für die generierten Antworten (z. B. Mindestlänge, Ausschluss von verbotenen Wörtern oder Vorhandensein geforderter Keywords).
- [ ] Lass den Evaluator ein Rating (z. B. Score 0-100) berechnen und hänge diese Evaluierungsdaten an dein Tracing-Log an.

---

## 🚀 Warum dieses Projekt für LLMOps überzeugt

Dieses Framework spiegelt exakt die Architektur echter Industriestandards wie **LangChain** oder **LlamaIndex** wider. Anstatt fertige Tools nur zu konsumieren, beweist du mit diesem Projekt, dass du die zugrundeliegenden Software-Design-Patterns (wie Pipelines, Mocks und Tracing) von Grund auf selbst implementieren und strukturieren kannst.

LLMOps Pipeline Projekt reaktivieren

> Linux

# 🛠️ Dokumentation: Einrichtung Ubuntu Server für LFCS

Dieses Dokument beschreibt die Ersteinrichtung eines virtuellen Cloud-Servers (VPS) unter **Ubuntu 24.04 LTS** zur Vorbereitung auf die LFCS-Prüfung.

## 📋 Voraussetzungen

- Ein aktiver Account bei einem Cloud-Anbieter (z. B. Hetzner Cloud, IONOS)
- Ein installierter SSH-Client auf dem lokalen Laptop (unter Windows: _PowerShell_ oder _Eingabeaufforderung_, unter macOS: _Terminal_)
- Die öffentliche IP-Adresse des Servers (Beispiel im Dokument: `123.45.67.89`)

---

## 🚀 Schritt 1: Server-Erstellung im Cloud-Dashboard

Beim Erstellen des Servers im Web-Interface des Anbieters müssen folgende Parameter gewählt werden:

- **Standort:** Deutschland (Falkenstein, Frankfurt oder Nürnberg)
- **Image (Betriebssystem):** `Ubuntu 24.04 LTS` (Long Term Support)
- **Typ / Tarif:** Shared vCPU (kleinster Tarif mit 2 vCPUs und ca. 4 GB RAM ist ausreichend)
- **Authentifizierung:** Passwort (wird per E-Mail zugestellt)

---

## 🔐 Schritt 2: Erstmalige Verbindung über SSH

Die Verwaltung des Servers erfolgt ausschließlich remote über die Kommandozeile (CLI).

1. Öffne das Terminal auf deinem lokalen Laptop.
2. Baue die Verbindung zum Server als Benutzer `root` auf:
   ```bash
   ssh root@123.45.67.89
   ```
3. Bestätige die Sicherheitsabfrage (_"Are you sure you want to continue connecting?"_) mit der Eingabe von `yes`.
4. Gib das temporäre Passwort aus der E-Mail des Cloud-Anbieters ein.
   _(Hinweis: Unter Linux werden bei der Passworteingabe keine Zeichen oder Sternchen angezeigt. Einfach blind eintippen und mit `Enter` bestätigen)._
5. Folge der Aufforderung auf dem Bildschirm, um das temporäre Passwort durch ein neues, sicheres Passwort zu ersetzen.

---

## 🔄 Schritt 3: Systemaktualisierung (Enterprise-Standard)

Nach dem ersten Login muss das System sofort auf den neuesten Sicherheitsstand gebracht werden. Führe dazu folgende Befehle nacheinander aus:

### 1. Paketquellen aktualisieren

Der Server prüft im Internet, ob neuere Softwareversionen verfügbar sind:

```bash
apt update
```

### 2. Sicherheitsupdates installieren

Alle verfügbaren Updates werden heruntergeladen und ohne weitere Nachfrage (`-y`) installiert:

```bash
apt upgrade -y
```

### 3. System neu starten

Um sicherzustellen, dass alle Updates (insbesondere Kernel-Updates) aktiv werden, startet der Server neu. Die aktuelle SSH-Sitzung wird dabei automatisch beendet:

```bash
reboot
```

Nach etwa **30 bis 60 Sekunden** ist der Server wieder hochgefahren und du kannst dich mit dem Befehl aus Schritt 2 und deinem neuen Passwort erneut einwählen.

> SSH

# 🔑 Dokumentation: SSH-Key-Authentifizierung & Server-Härtung

Dieses Dokument beschreibt, wie die unsichere Passwort-Anmeldung über SSH komplett abgeschaltet und durch kryptografische Schlüsselpaare (Public/Private Key) ersetzt wird.

---

## 💻 Schritt 1: SSH-Schlüssel auf dem LOKALEN Laptop erstellen

_Führe diesen Schritt im Terminal deines eigenen Laptops aus (NICHT auf dem Server!):_

```bash
ssh-keygen -t ed25519 -C "lfcs-lernserver"
```

_(Drücke bei allen Abfragen einfach `Enter`, um den Standardpfad zu wählen und kein extra Passwort für den Schlüssel zu vergeben)._

---

## 🚀 Schritt 2: Schlüssel auf den Server übertragen

_Führe auch diesen Schritt auf deinem LOKALEN Laptop aus. Ersetze `deinname` und die IP:_

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub deinname@123.45.67.89
```

_Was passiert?_ Du wirst ein letztes Mal nach deinem Server-Passwort gefragt. Der öffentliche Schlüssel wird nun sicher auf dem Server hinterlegt.

**Test:** Logge dich jetzt mit `ssh deinname@123.45.67.89` ein. Du solltest ohne Passwort-Abfrage direkt auf dem Server landen!

---

## 🔒 Schritt 3: Passwort-Login auf dem Server VERBIETEN

_Führe diese Schritte JETZT AUF DEM SERVER als dein Benutzer aus:_

1. Öffne die SSH-Konfigurationsdatei mit dem Texteditor:

   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

2. Suche mit `Strg + W` nach dem Begriff `PasswordAuthentication`.
3. Ändere die Zeile so ab (entferne das `#`-Zeichen, falls vorhanden):

   ```text
   PasswordAuthentication no
   ```

4. Suche nach `PermitRootLogin` und ändere es ab auf:

   ```text
   PermitRootLogin no
   ```

   _(Das verbietet dem gefährlichen `root`-User den direkten Login von außen)._

5. Speichere mit `Strg + O`, bestätige mit `Enter` und schließe den Editor mit `Strg + X`.

---

## 🔄 Schritt 4: SSH-Dienst neu starten

Damit die Änderungen aktiv werden, starte den SSH-Dienst neu:

```bash
sudo systemctl restart ssh
```

❗ **WICHTIG:** Schließe dein aktuelles Terminal-Fenster noch nicht! Öffne ein **neues, zweites Terminal-Fenster** auf deinem Laptop und versuche dich einzuloggen. Wenn das ohne Passwort klappt, war alles erfolgreich und dein Server ist ab jetzt immun gegen automatisierte Passwort-Hacker (Brute-Force-Angriffe).

> Udemy

# 🔑 Dokumentation: SSH-Key-Authentifizierung & Server-Härtung

Dieses Dokument beschreibt, wie die unsichere Passwort-Anmeldung über SSH komplett abgeschaltet und durch kryptografische Schlüsselpaare (Public/Private Key) ersetzt wird.

---

## 💻 Schritt 1: SSH-Schlüssel auf dem LOKALEN Laptop erstellen

_Führe diesen Schritt im Terminal deines eigenen Laptops aus (NICHT auf dem Server!):_

```bash
ssh-keygen -t ed25519 -C "lfcs-lernserver"
```

_(Drücke bei allen Abfragen einfach `Enter`, um den Standardpfad zu wählen und kein extra Passwort für den Schlüssel zu vergeben)._

---

## 🚀 Schritt 2: Schlüssel auf den Server übertragen

_Führe auch diesen Schritt auf deinem LOKALEN Laptop aus. Ersetze `deinname` und die IP:_

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub deinname@123.45.67.89
```

_Was passiert?_ Du wirst ein letztes Mal nach deinem Server-Passwort gefragt. Der öffentliche Schlüssel wird nun sicher auf dem Server hinterlegt.

**Test:** Logge dich jetzt mit `ssh deinname@123.45.67.89` ein. Du solltest ohne Passwort-Abfrage direkt auf dem Server landen!

---

## 🔒 Schritt 3: Passwort-Login auf dem Server VERBIETEN

_Führe diese Schritte JETZT AUF DEM SERVER als dein Benutzer aus:_

1. Öffne die SSH-Konfigurationsdatei mit dem Texteditor:

   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

2. Suche mit `Strg + W` nach dem Begriff `PasswordAuthentication`.
3. Ändere die Zeile so ab (entferne das `#`-Zeichen, falls vorhanden):

   ```text
   PasswordAuthentication no
   ```

4. Suche nach `PermitRootLogin` und ändere es ab auf:

   ```text
   PermitRootLogin no
   ```

   _(Das verbietet dem gefährlichen `root`-User den direkten Login von außen)._

5. Speichere mit `Strg + O`, bestätige mit `Enter` und schließe den Editor mit `Strg + X`.

---

## 🔄 Schritt 4: SSH-Dienst neu starten

Damit die Änderungen aktiv werden, starte den SSH-Dienst neu:

```bash
sudo systemctl restart ssh
```

❗ **WICHTIG:** Schließe dein aktuelles Terminal-Fenster noch nicht! Öffne ein **neues, zweites Terminal-Fenster** auf deinem Laptop und versuche dich einzuloggen. Wenn das ohne Passwort klappt, war alles erfolgreich und dein Server ist ab jetzt immun gegen automatisierte Passwort-Hacker (Brute-Force-Angriffe).

> Youtube

# Empfohlene YouTube-Ressourcen für FastAPI & Docker

## 1. Direktlinks zu Einstiegsvideos

- **[A Gentle Introduction to Docker With FastAPI](https://youtube.com)** – Perfekt für den Übergang von lokalem Code zum Container.
- **[FastAPI Docker Tutorial: From Zero to Production](https://youtube.com)** – Erklärt Docker im Team und Docker Compose für Live-Code-Änderungen.

---

## 2. Top YouTube-Kanäle für deinen Stack

### ArjanCodes (Architektur & Clean Code)

- **Fokus:** Software-Design-Patterns in Python.
- **Nutzen für dich:** Zeigt dir, wie du deine OOP-Kenntnisse nutzt, um FastAPI-Projekte sauber zu strukturieren.
- **Such-Tipp:** `ArjanCodes FastAPI`

### Tech World with Nana (DevOps & Docker verständlich gemacht)

- **Fokus:** Visuelle und animierte Erklärungen zu Infrastruktur.
- **Nutzen für dich:** Veranschaulicht perfekt, wie Docker, Docker Compose und Netzwerke im Hintergrund funktionieren.
- **Such-Tipp:** `Docker Tutorial for Beginners Tech World with Nana`

### Sanjeev Thiyagarajan (Echte Deep Dives)

- **Fokus:** Monumentale, kostenlose Komplettkurse (oft 10+ Stunden).
- **Nutzen für dich:** Baut echte APIs inklusive PostgreSQL-Datenbanken, JWT-Authentifizierung und Docker-Setups von Grund auf.
- **Such-Tipp:** `FastAPI Full Course Sanjeev Thiyagarajan`

---

## 3. Effektive Suchbegriffe für die YouTube-Suche

Da die hochwertigsten Inhalte auf Englisch sind, fährst du mit diesen Begriffen am besten:

- `FastAPI Docker Compose PostgreSQL tutorial` (Verbindung von App + Datenbank)
- `Python Microservices with FastAPI and Docker` (Kommunikation mehrerer Container)
- `FastAPI Production Setup` (Best Practices für echte Server)

> Another One

---

title: "Projektplan: Custom Multi-Agent Framework (OOP)"
date: 2026-08-25
description: "Architektur und Leitfaden für mein erstes eigenständiges Boot.dev Portfolio-Projekt im LLMOps-Pfad."
tags: ["python", "oop", "llmops", "agents", "bootdev"]
categories: ["Portfolio", "Software Engineering"]
showToc: true
TocOpen: true
draft: false

---

## 🎯 Projekt-Übersicht

Dieses Projekt ist mein **First Personal Project** für Boot.dev. Es verbindet die Grundlagen der **Objektorientierten Programmierung (OOP)** in Python mit den Konzepten aus dem Modul **"Build an AI Agent"**.

Anstatt fertige Bibliotheken wie CrewAI zu nutzen, wird die Kern-Infrastruktur für ein sequenzielles Multi-Agenten-System komplett von Grund auf selbst gebaut.

---

## 🏗️ System-Architektur (Klassen-Design)

Das Framework basiert auf drei zentralen, eng miteinander verzahnten Python-Klassen:

### 1. Klasse: `Agent`

Repräsentiert eine spezialisierte KI-Persönlichkeit.

- **Attribute:**
  - `name` _(str)_: Name des Agenten (z. B. "Researcher").
  - `role` _(str)_: Fachgebiet (z. B. "Findet relevante Fakten zu Thema X").
  - `backstory` _(str)_: System-Prompt-Erweiterung, um der KI Kontext und Tonalität zu geben.
  - `llm_client` _(Object)_: Die konfigurierte Verbindung zur API (OpenAI / Ollama).
- **Methoden:**
  - `execute_task(task_description, context_data=None)`: Kombiniert Rolle, Backstory, Aufgabe und den Kontext vorheriger Agenten zu einem Prompt, sendet ihn an die API und liefert die Antwort zurück.

### 2. Klasse: `Task`

Definiert eine spezifische Aufgabe innerhalb der Kette.

- **Attribute:**
  - `description` _(str)_: Die genaue Arbeitsanweisung.
  - `expected_output` _(str)_: Beschreibung des gewünschten Formats (z. B. "Eine Markdown-Tabelle").
  - `assigned_agent` _(Agent)_: Eine Instanz der `Agent`-Klasse, die diese Aufgabe ausführen soll.
  - `output` _(str)_: Speichert das finale Ergebnis, sobald die Task abgearbeitet wurde.

### 3. Klasse: `Crew` (Der Orchestrator)

Das Gehirn, das den Ablauf steuert.

- **Attribute:**
  - `agents` _(List[Agent])_: Liste aller beteiligten Agenten.
  - `tasks` _(List[Task])_: Liste der abzuarbeitenden Aufgaben in sequenzieller Reihenfolge.
- **Methoden:**
  - `kickoff()`: Iteriert durch die `tasks`. Übergibt den Output der jeweils _vorherigen_ Task als Kontext an die nächste Task, ruft `execute_task()` auf dem zugewiesenen Agenten auf und speichert das Ergebnis.

---

## 🚀 Boot.dev Anforderungen & Workflow

Da es sich um ein freies Modul handelt, müssen folgende Schritte lokal und auf GitHub dokumentiert werden:

- [ ] **Lokales Git-Repository:** Projektordner erstellen und sofort mit `git init` initialisieren.
- [ ] **Saubere Commits:** Entwicklungsschritte (Klassendesign, API-Integration, Testing) in logischen Git-Commits festhalten.
- [ ] **GitHub Repository:** Code in ein öffentliches GitHub-Repository pushen.
- [ ] **README.md:** Eine verständliche Dokumentation schreiben, die erklärt, wie man das Framework installiert und eigene Agenten definiert.
- [ ] **Community Review:** Link im Boot.dev-Discord für Feedback und Freischaltung einreichen.

---

## 🛠️ Nächste Schritte (Morgen)

1. **API-Setup klären:** Festlegen, ob das Framework über die **OpenAI API** oder lokal via **Ollama** laufen soll.
2. **Die erste Datei (`main.py`) anlegen:** Das Grundgerüst für die Klasse `Agent` schreiben.
3. **Erster Test-Call:** Sicherstellen, dass die Klasse erfolgreich mit dem LLM kommunizieren kann.
