---
---
[English](../01_gocfl_usage_en)

# Verwendung und Konfiguration von gocfl

In diesem Teil des Workshops machen wir uns mit der grundlegenden Bedienung und den Konfigurationsmöglichkeiten des Werkzeugs `gocfl` vertraut. Wir setzen voraus, dass die [Installation](../00_installation) erfolgreich abgeschlossen wurde.

## 1. Grundlegende Befehlsstruktur

`gocfl` folgt einem typischen Kommandozeilen-Muster mit Unterbefehlen. Die allgemeine Syntax lautet:

```bash
gocfl [globale Flags] <Befehl> [Befehls-Flags] <Argumente>
```

Wichtige globale Flags sind beispielsweise:
- `-v`, `--verbose`: Erhöht die Detailtiefe der Ausgaben.
- `-l`, `--log`: Ermöglicht das Schreiben von Logs in eine Datei.

## 2. Hilfe-System

Das Werkzeug bietet eine umfassende integrierte Hilfe. Diese ist der beste Weg, um die verfügbaren Optionen zu erkunden:

### Allgemeine Hilfe
Zeigt alle verfügbaren Hauptbefehle (z.B. `init`, `add`, `update`, `extract`, `validate`) an:
```bash
gocfl --help
```

### Befehlsspezifische Hilfe
Jeder Unterbefehl hat seine eigene Hilfe-Seite, die die spezifischen Flags und Argumente erklärt:
```bash
gocfl init --help
gocfl add --help
```

## 3. Konfiguration

`gocfl` bietet flexible Möglichkeiten zur Konfiguration, um sowohl einfache Ad-hoc-Aufrufe als auch komplexe Workflows zu unterstützen.

### Konfigurationsdatei
Für wiederkehrende Einstellungen kann eine Konfigurationsdatei (im TOML-Format) verwendet werden. Mit dem globalen Flag `--config` kann eine spezifische Datei angegeben werden:

```bash
gocfl --config myconfig.toml [Befehl]
```

Die Konfigurationsdatei dient nicht nur der Speicherung einfacher Standardwerte (wie S3-Verbindungen oder Log-Level), sondern ist insbesondere für **komplexe Einstellungen von Submodulen** essenziell. Viele dieser tiefgreifenden Konfigurationen stehen bewusst nicht als Kommandozeilenparameter zur Verfügung, um die Befehlszeile übersichtlich zu halten.

Beispiele für solche Submodule sind:
- **[Indexer](https://github.com/je4/indexer):** Extraktion und Indizierung von Metadaten.
- **Formatmigration:** Module zur automatisierten Umwandlung von Dateiformaten innerhalb des Objekts.
- **Thumbnail-Generierung:** Erstellung von Vorschaubildern für archivierte Inhalte.

Einen detaillierten Überblick über die einzelnen Abschnitte und Möglichkeiten der Konfigurationsdatei finden Sie auf der Seite **[Detaillierte Konfiguration (gocfl.toml)](../01a_gocfl_config)**.

Wenn kein Pfad angegeben wird, nutzt `gocfl` interne Standardwerte (Embedded Config).

### Rangfolge der Einstellungen (Precedence)
Falls Einstellungen an mehreren Stellen definiert sind, gilt folgende Priorität:
1. **Kommandozeilen-Parameter (Flags):** Haben immer Vorrang.
2. **Umgebungsvariablen:** Überschreiben Werte aus der Konfigurationsdatei (z.B. `GOCFL_S3_ENDPOINT`).
3. **Konfigurationsdatei:** Definiert projekt- oder nutzerspezifische Standards.
4. **Standardwerte:** Im Programmcode fest hinterlegte Fallbacks (z.B. SHA-512 als Standard-Hash).

## 4. Versionsprüfung

Um sicherzustellen, dass Sie mit der erwarteten Version arbeiten, nutzen Sie:
```bash
gocfl --version
```

---

[Zurück zur Installation](../00_installation) | [Zurück zum Inhaltsverzeichnis](../TOC) | [Nächstes Thema: Initialisierung einer Storage Root](../02_storage_root)
