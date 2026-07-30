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
