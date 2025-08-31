# Leitfaden
Dieser Leitfaden beschreibt die empfohlene Struktur, Arbeitsweise und Best Practices für die Nutzung von GitHub
Um eine saubere und nachvollziehbare Arbeit zu gewährleisten, sollten bestimmte Konventionen und Abläufe eingehalten werden. 

  
## Eine gute Projektstruktur

```
README.md              # Projektbeschreibung
requirements.txt       # Python-Abhängigkeiten
src/                   # Quellcode
notebooks/             # Jupyter Notebooks
data/
  roh/                 # Unverarbeitete Daten
  verarbeitet/         # Aufbereitete Daten
  beschreibung/        # Datenquellen und Erklärungen
results/
  grafiken/
  tabellen/
docs/                  # Dokumentation & Leitfäden
CONTRIBUTING.md        # Mitwirkungsregeln
LICENSE                # Lizenz
.gitignore             # Ausgeschlossene Dateien
```


## README.md sollte enthalten:

- Projektbeschreibung
- Übersicht der Ordnerstruktur, des experimentellen Aufbaus, der Datensätze und Ergebnissse (mit Visualisierung)
- Links zu Notebooks, Daten und Ergebnisse
  
README immer aktuell halten

Jede größere Änderung dokumentieren:
- Neue Skripte oder Daten
- Andere Struktur
- Hinweise zur Ausführung


## Eine gute Datenstruktur

 `data/roh/`
Originaldaten (nicht verändert)

 `data/verarbeitet/`
Bereinigte, verwendbare Daten

 `data/beschreibung/`
Quellen, Format-Erklärungen, ggf. ein eigenes `README.md`


## Notebooks

Jupyter Notebooks sind ideal für:

- Datenaufbereitung
- Explorative Analysen
- Visualisierung
- Modellergebnisse

Beispielhafte Benennung (chronologisch und thematisch, Nummerierung sorgt für Ordnung):

01_datenaufbereitung.ipynb
02_erste_analysen.ipynb
03_visualisierung.ipynb


## Ergebnisse speichern in

`results/`:
- `grafiken/`: Diagramme, Visualisierungen
- `tabellen/`: Tabellen, CSVs, Zusammenfassungen

Sprechende, nachvollziehbare Dateinamen verwenden. Optional kann ein eigenes README.md im results/-Verzeichnis erklären, wie und wann die Ergebnisse erzeugt wurden.


## Code-Stil & Tools

Ein konsistenter Stil sorgt für bessere Lesbarkeit
Automatische Formatter und Linter für Python:

- `black` – automatisches Formatieren
- `isort` – sortiert Imports
- `flake8` – Linting
  
Außerdem zu beachten ist:
- Abläufe Schritt für Schritt durch Code-Kommentare erläutern
- Reproduzierbarkeit sichern
- Konfigurierbarkeit bevorzugen
   (Statt z. B. Trainingsgrößen und Validationspfade hart in dein Skript zu schreiben,       speichere sie besser in einer YAML- oder JSON-Datei, die du einliest.)
- Logging statt nur print()
- Ergebnisse visualisieren


##  Mitwirken im Team

Wenn mehrere Leute zusammenarbeiten, sollten Regeln in der Datei CONTRIBUTING.md dokumentiert sein. 
Typischer Ablauf für Beiträge:

- Ein Issue (Problem oder Aufgabe) anlegen oder übernehmen.
- Einen neuen Branch anlegen (z. B. feature/datenfilterung).
- Änderungen lokal umsetzen und committen.
- Einen Pull Request (PR) auf GitHub stellen.




