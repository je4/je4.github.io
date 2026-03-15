---
---
[English](../03_extensions_en)

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

- **[OCFL Storage Root Extensions (initial, manager, layout)](../03a_storage_root_extensions)**
- **[OCFL Objekt-Erweiterungen (content-subpath, indexer, metafile)](../03c_object_extensions)**
- **[Detaillierte Liste aller verwendeten Extensions](../03b_extension_list)**

## 4. Aufrufpunkte (Hooks) in gocfl

Die Implementierung von Erweiterungen in `gocfl` basiert auf verschiedenen Aufrufpunkten (Hooks), an denen die Logik der Erweiterung in den OCFL-Workflow eingreift. Sollte ein Interface mehrere Hooks besitzen, so muss eine Extension, die es verwendet, sämtliche darin definierten Hooks implementieren.

Diese unterscheiden sich je nach Kontext:

- **Objekt-Erweiterungen:** Definieren Hooks für Aktionen innerhalb eines OCFL-Objekts. Details finden sich in der [`objectExtension.go`](https://github.com/ocfl-archive/gocfl/blob/ed2bafb10a8c2bdbb2fad3ad271aa7e6993b75ed/pkg/ocfl/object/objectExtension.go).
    - **Interface `ExtensionObjectContentPath`**:
        - `BuildObjectManifestPath`: Erstellt den Pfad für das Manifest innerhalb des Objekts.
    - **Interface `ExtensionObjectExtractPath`**:
        - `BuildObjectExtractPath`: Definiert Pfade für das Extrahieren von Inhalten.
    - **Interface `ExtensionObjectStatePath`**:
        - `BuildObjectStatePath`: Steuert die Pfadbildung im Objekt-State.
    - **Interface `ExtensionArea`**:
        - `GetAreaPath`: Liefert den Pfad für einen spezifischen Speicherbereich (Area).
    - **Interface `ExtensionStream`**:
        - `StreamObject`: Erlaubt das Mitlesen des Datenstroms.
    - **Interface `ExtensionContentChange`**:
        - `AddFileBefore`: Vor dem Hinzufügen einer Datei.
        - `UpdateFileBefore`: Vor der Aktualisierung einer Datei.
        - `DeleteFileBefore`: Vor dem Löschen einer Datei.
        - `AddFileAfter`: Nach dem Hinzufügen einer Datei.
        - `UpdateFileAfter`: Nach der Aktualisierung einer Datei.
        - `DeleteFileAfter`: Aktionen nach dem Löschen einer Datei.
    - **Interface `ExtensionObjectChange`**:
        - `UpdateObjectBefore`: Aktionen vor der Manifest-Erstellung / Objekt-Aktualisierung.
        - `UpdateObjectAfter`: Aktionen nach dem Speichern des Objekts.
    - **Interface `ExtensionFixityDigest`**:
        - `GetFixityDigests`: Liefert zusätzliche Digest-Algorithmen für Fixity-Prüfungen.
    - **Interface `ExtensionMetadata`**:
        - `GetMetadata`: Liefert zusätzliche Metadaten für das Objekt.
    - **Interface `ExtensionVersionDone`**:
        - `VersionDone`: Wird aufgerufen, wenn eine neue Version vollständig abgeschlossen ist.
    - **Interface `ExtensionNewVersion`**:
        - `NeedNewVersion`: Prüft, ob eine neue Version erforderlich ist.
        - `DoNewVersion`: Führt Initialisierungen für eine neue Version durch.
- **Storage Root Erweiterungen:** Definieren Hooks für die Verwaltung der gesamten Storage Root. Details finden sich in der [`storagerootExtension.go`](https://github.com/ocfl-archive/gocfl/blob/ed2bafb10a8c2bdbb2fad3ad271aa7e6993b75ed/pkg/ocfl/storageroot/storagerootExtension.go).
    - **Interface `ExtensionBuildStorageRootPath`**:
        - `BuildStorageRootPath`: Berechnung des physischen Pfads eines Objekts basierend auf seiner ID (Storage Layout).

## 5. Mapping im Extension Manager

Der `NNNN-gocfl-extension-manager` nutzt in seiner `config.json` spezifische Schlüssel, um die verschiedenen Erweiterungstypen zu adressieren. Hier ist das Mapping der Go-Interfaces zu den Konfigurations-Einträgen:

| Interface | Schlüssel in `config.json` |
| :--- | :--- |
| `ExtensionBuildStorageRootPath` | `StorageRootPath` |
| `ExtensionObjectContentPath` | `ObjectContentPath` |
| `ExtensionObjectExtractPath` | `ObjectExtractPath` |
| `ExtensionObjectStatePath` | `ObjectStatePath` |
| `ExtensionArea` | `Area` |
| `ExtensionStream` | `Stream` |
| `ExtensionContentChange` | `ContentChange` |
| `ExtensionObjectChange` | `ObjectChange` |
| `ExtensionFixityDigest` | `FixityDigest` |
| `ExtensionMetadata` | `Metadata` |
| `ExtensionNewVersion` | `NewVersion` |
| `ExtensionVersionDone` | `VersionDone` |

## 6. Self-Describing durch Dokumentation

Ein wichtiger Aspekt von `gocfl` ist, dass zu jeder aktivierten Erweiterung die entsprechende Dokumentation als Markdown-Datei (z. B. `NNNN-gocfl-extension-manager.md`) direkt in die Storage Root kopiert werden sollte. 

Dies garantiert, dass auch in Jahrzehnten noch nachvollziehbar ist, welche Regeln für die Ablage und Verarbeitung der Daten galten, selbst wenn die ursprüngliche Software oder Webseite nicht mehr existiert.

---

[Zurück zum Inhaltsverzeichnis](../toc) | [Nächstes Thema: Erstellung von Objekten](../04_add_object)
