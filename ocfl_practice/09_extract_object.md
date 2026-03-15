---
---
[English](../09_extract_object_en)

# Extrahieren von Inhalten (extract)

Obwohl OCFL-Objekte direkt im Dateisystem lesbar sind, kann es notwendig sein, den Inhalt eines Objekts (oder einer spezifischen Version) in ein separates Verzeichnis zu extrahieren, beispielsweise für die Auslieferung an ein anderes System oder zur weiteren Verarbeitung ohne die OCFL-Struktur.

## 1. Der `extract`-Befehl

Mit dem Befehl `extract` kopiert `gocfl` die Dateien eines Objekts aus der Storage Root an einen Zielort. Dabei wird die logische Struktur des Objekts (der "State") wiederhergestellt.

### Grundlegende Syntax:
```bash
gocfl extract [Optionen] [Pfad zur Storage Root oder zum Objekt] [Zielverzeichnis]
```

### Beispiel aus dem Workshop:
Um die aktuelle Version unseres Test-Objekts in ein temporäres Verzeichnis zu extrahieren, verwenden wir:

```bash
gocfl --log-level DEBUG --config ./gocfl/config/gocfl.toml extract ./gocfl/temp/test42 ./gocfl/temp/extract_v2 --with-manifest -i urn:nbn:de:gbv:42-test1
```

**Erklärung:**
- `--log-level DEBUG`: Zeigt detaillierte Informationen während des Vorgangs an.
- `extract`: Der Befehl zum Extrahieren.
- `./gocfl/temp/test42`: Der Pfad zur Storage Root.
- `./gocfl/temp/extract_v2`: Das Zielverzeichnis für die extrahierten Dateien.
- `--with-manifest`: Erstellt zusätzlich ein sha512sum kompatibles Manifest für das Zielverzeichnis.
- `-i urn:nbn:de:gbv:42-test1`: Die ID des zu extrahierenden Objekts.

## 2. Spezifische Versionen extrahieren

Standardmäßig extrahiert `gocfl` immer die **neueste Version** (HEAD). Möchten Sie eine ältere Version extrahieren, können Sie dies über den Parameter `-v` (oder `--version`) steuern:

```bash
gocfl --config ./gocfl/config/gocfl.toml extract --version v1 ./gocfl/temp/test42/ ./gocfl/temp/extract_v1/ --object-id urn:nbn:de:gbv:42-test1 --with-manifest
```

## 3. Wichtige Optionen

- `-v`, `--version`: Bestimmt die zu extrahierende Version (z.B. `v1`).
- `-i`, `--object-id`: Ermöglicht die Angabe der Objekt-ID, falls der Pfad zur Storage Root (statt zum Objekt) angegeben wurde.
- `-p`, `--object-path`: Pfad zum zu extrahierenden Objekt.
- `--area`: Bestimmt den Datenbereich, der extrahiert werden soll (Standard: `content`).
    - **Hinweis:** Standardmäßig wird nur der Bereich `content` extrahiert. Sollen alle Bereiche exportiert werden, muss als Area `full` angegeben werden (oder ein spezieller Bereich wie z.B. `metadata`).
- `--with-manifest`: Erstellt ein sha512sum-kompatibles Manifest für das Zielverzeichnis.
- `--ext-NNNN-content-subpath-area`: Unterpfad für die Extraktion (Standard: `content`). `all` für die vollständige Extraktion.
- `--ext-NNNN-metafile-target`: URL mit dem Zielordner für Metadaten.

## 4. Was wird extrahiert?

Beim Extrahieren wird der **logische State** der gewählten Version wiederhergestellt. Das bedeutet:
- Dateien, die in der Version gelöscht wurden, erscheinen nicht im Zielverzeichnis.
- Dateien, die umbenannt wurden, erscheinen unter ihrem neuen Namen.
- Die durch Extensions wie `0011-direct-clean-path-layout` bereinigten Dateinamen im OCFL-Objekt werden beim Extrahieren wieder in ihre ursprüngliche Form (mit Leerzeichen etc.) zurückgeführt.

Die Verwaltung der verschiedenen **Datenbereiche (Areas)** erfolgt dabei über die `NNNN-content-subpath`-Extension. Standardmäßig wird nur der Bereich `content` extrahiert, sofern nicht anders angegeben (siehe `--area`).

---

[Zurück zur Anzeige im Webbrowser](../08_display_object) | [Weiter zum Extrahieren von Metadaten](../10_extract_metadata) | [Zurück zum Inhaltsverzeichnis](../toc)
