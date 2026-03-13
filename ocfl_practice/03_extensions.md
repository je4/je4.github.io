# OCFL Extensions (Erweiterungen)

OCFL ist so konzipiert, dass der Kernstandard schlank und stabil bleibt, während spezifische Funktionalitäten über **Extensions** (Erweiterungen) abgebildet werden. Diese ermöglichen es, das Verhalten des Archivs anzupassen, ohne die Grundkompatibilität zu gefährden.

## 1. Das Konzept der Erweiterungen

Erweiterungen befinden sich sowohl in der Storage Root als auch in den einzelnen OCFL-Objekten im jeweiligen `extensions/`-Verzeichnis. 

### Warum Erweiterungen?
- **Layouts:** Festlegen, wie Objekt-IDs in Verzeichnispfade übersetzt werden (z. B. Hashing).
- **Metadaten:** Einbetten von zusätzlichen technischen oder deskriptiven Metadaten (z. B. METS/PREMIS).
- **Funktionalität:** Unterstützung für Dateisystem-Features oder spezifische Archivierungs-Workflows.

## 2. Registrierte vs. Lokale Erweiterungen

- **Offizielle Extensions:** Diese sind in der [OCFL Community Extensions](https://ocfl.io/extensions/) Liste registriert (z. B. `0004-hashed-n-tuple-storage-layout`).
- **Lokale/Interne Extensions:** Beginnen oft mit `NNNN-` und sind spezifisch für ein Werkzeug oder eine Institution. `gocfl` nutzt diese intensiv für seine Zusatzfunktionen.

## 3. Extensions in der Praxis

Wie wir bei der Initialisierung der Storage Root gesehen haben, legt `gocfl` automatisch Konfigurationsverzeichnisse an. Eine detaillierte Übersicht über die in diesem Workshop verwendeten Erweiterungen finden Sie hier:

- **[OCFL Storage Root Extensions (initial, manager, layout)](03a_storage_root_extensions.md)**

## 4. Self-Describing durch Dokumentation

Ein wichtiger Aspekt von `gocfl` ist, dass zu jeder aktivierten Erweiterung die entsprechende Dokumentation als Markdown-Datei (z. B. `NNNN-gocfl-extension-manager.md`) direkt in die Storage Root kopiert werden sollte. 

Dies garantiert, dass auch in Jahrzehnten noch nachvollziehbar ist, welche Regeln für die Ablage und Verarbeitung der Daten galten, selbst wenn die ursprüngliche Software oder Webseite nicht mehr existiert.

---

[Zurück zum Inhaltsverzeichnis](toc.md) | [Nächstes Thema: Erstellung von Objekten](04_create_object.md)
