---
---
[English](03b_extension_list_en)

# Liste der verwendeten OCFL Extensions

In unserem Workshop-Szenario kommen verschiedene offizielle und werkzeugspezifische (`NNNN-`) Erweiterungen zum Einsatz. Diese lassen sich nach ihrem Einsatzort und ihrer Funktion kategorisieren.

## 1. Storage Root Extensions
Diese Erweiterungen steuern das Verhalten der gesamten Storage Root (das Speicher-Layout der Objekte).

- **`initial`**: Legt fest, welche Erweiterung beim Start als erste geladen wird (verweist meist auf den `extension-manager`).
- **`NNNN-gocfl-extension-manager`**: Eine Meta-Erweiterung von `gocfl`, die andere Erweiterungen koordiniert und z.B. Dokumentationsdateien verwaltet.
- **`0004-hashed-n-tuple-storage-layout`**: Definiert das Mapping von Objekt-IDs auf Verzeichnisstrukturen mittels Hashing (Standard in diesem Workshop).

## 2. Objekt Extensions
Diese Erweiterungen sind innerhalb der einzelnen OCFL-Objekte aktiv. Wir unterteilen sie hier nach ihrem Hauptzweck:

### A. Pfad- und Layout-Erweiterungen
Diese Erweiterungen bestimmen, wie Dateien innerhalb des Objekts physisch abgelegt und benannt werden.

- **`0011-direct-clean-path-layout`**: Bereinigt Dateinamen für maximale Systemkompatibilität (z.B. Ersetzung von Leerzeichen durch `=u0020`). Details siehe **[Objektstruktur](05_object_structure#dateinamen-bereinigung-0011-direct-clean-path-layout)**.
- **`NNNN-content-subpath`**: Teilt das `content`-Verzeichnis in logische Bereiche wie `data/` (Nutzdaten) und `metadata/` (generierte Daten) auf. Details siehe **[Objektstruktur](05_object_structure#content-struktur-data--metadata-und-nnnn-content-subpath)**.

### B. Metadaten-Erweiterungen
Diese Erweiterungen reichern das Objekt während des Ingest-Prozesses automatisch mit technischen oder administrativen Metadaten an.

- **`NNNN-indexer`**: Extrahiert technische Metadaten (Format, Dauer, Auflösung etc.) mittels Tools wie Siegfried und FFMPEG und speichert sie in `indexer_v1.jsonl.gz`.
- **`NNNN-filesystem`**: Sichert Metadaten des ursprünglichen Dateisystems (Zeitstempel, Rechte) in `filesystem_v1.jsonl.gz`.
- **`NNNN-metafile`**: Erlaubt das Einbinden benutzerdefinierter Metadaten (wie unsere `info.json`) beim Erstellen des Objekts.
- **`0001-digest-algorithms`**: Ermöglicht die Berechnung zusätzlicher Prüfsummen (Fixity) für die langfristige Integrität.

### C. Erweiterungen für bessere Lesbarkeit und Vorschau
Diese Erweiterungen dienen dazu, den Inhalt des Objekts für menschliche Nutzer schneller erfassbar zu machen.

- **`NNNN-thumbnail`**: Erzeugt automatisch kleine Vorschaubilder für Bilder, Videos und PDFs, die im `metadata/thumbnails/`-Ordner abgelegt werden.

---

[Zurück zur Objektstruktur](05_object_structure) | [Zurück zum Inhaltsverzeichnis](toc)
