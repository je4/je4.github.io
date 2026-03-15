---
---
[English](../01a_gocfl_config_en)

# Überblick über die gocfl.toml Konfigurationsdatei

Die Konfigurationsdatei [gocfl.toml](../gocfl.toml) ist das zentrale Element für die Steuerung komplexer Abläufe in `gocfl`. Sie erlaubt es, Standardwerte für Befehle festzulegen und die weitreichenden Funktionen der Submodule zu konfigurieren.

Hier ist ein Überblick über die wichtigsten Abschnitte der Datei:

## 1. Infrastruktur und Globale Einstellungen

- **[S3]:** Konfiguration für die Arbeit mit S3-kompatiblem Objektspeicher (Endpoints, Access Keys, Regionen). Diese können auch über Umgebungsvariablen wie `%%GOCFL_S3_ENDPOINT%%` gesteuert werden.
- **[log]:** Steuerung des Detailgrads der Ausgaben (`trace`, `debug`, `info`, etc.).
- **[log.stash]:** Ermöglicht das Senden von Logs an einen Logstash-Server (ELK-Stack).
- **[AES]:** Integration von KeePass (KDBX-Dateien) zur sicheren Verwaltung von Schlüsseln und Passwörtern.

## 2. Befehls-Standardwerte

Diese Abschnitte definieren Fallback-Werte, falls diese nicht explizit auf der Kommandozeile angegeben werden:

- **[Init]:** Standard-OCFL-Version (z.B. 1.1), Pfad zum **Storage-Root-Extension-Template** (z.B. `/home/ocfl/gocfl/config/extensions/storageroot`) und der primäre Digest-Algorithmus (z.B. SHA-512).
- **[Add]:** Standard-Commit-Nachricht, Digest-Typen, Fixity-Algorithmen (z.B. MD5, SHA-1 für zusätzliche Integritätsprüfung) und Benutzerinformationen (`[Add.User]`).

## 3. Submodule (Die "Intelligenz" von gocfl)

Ein großer Teil der `gocfl.toml` widmet sich der Konfiguration von Submodulen, die während der Erstellung oder Aktualisierung von Objekten aktiv werden:

### Indexer (`[Indexer]`)
Dieses Modul extrahiert technische Metadaten. Es nutzt verschiedene Werkzeuge:
- **Siegfried:** Formaterkennung mittels PRONOM-IDs.
- **FFMPEG:** Extraktion von Metadaten aus Audio- und Videodateien (inkl. Mime-Type-Mapping).
- **ImageMagick (Identify/Convert):** Analyse von Bilddateien.
- **Tika:** Extraktion von Metadaten und Volltexten aus Dokumenten (z.B. PDF, Office).
- **XML:** Validierung und Typisierung von XML-Dateien anhand von Namespaces oder Schemata.

### Thumbnail-Generierung (`[Thumbnail]`)
Definiert Regeln, wie für verschiedene Dateitypen (Bilder, Video, Audio-Spektrogramme, PDF) automatisch Vorschaubilder erzeugt werden sollen. Hierbei werden externe Befehle (wie `convert` oder `ffmpeg`) mit spezifischen Parametern konfiguriert.

### Migration (`[Migration]`)
Dies ist ein Kernfeature für die Langzeitarchivierung. Es ermöglicht die automatische Erstellung von Derivaten in nachhaltigen Formaten während des Ingest-Prozesses:
- **RAW nach PNG:** Umwandlung von Kamera-Rohdaten (z.B. Canon CR2).
- **PDF nach PDF/A:** Konvertierung verschiedener PDF-Versionen in das Archivformat PDF/A mittels Ghostscript (`gs`).
- **Strategien:** Festlegung, wie die neuen Dateien benannt und im OCFL-Objekt abgelegt werden.

## 4. Extensions (`[Extension]`)
Konfiguration für spezifische OCFL-Erweiterungen, wie z.B. die Integration von METS-Dateien für deskriptive Metadaten.

---

[Zurück zur Verwendung von gocfl](../01_gocfl_usage) | [Zurück zum Inhaltsverzeichnis](../TOC) | [Nächstes Thema: Initialisierung einer Storage Root](../02_storage_root)
