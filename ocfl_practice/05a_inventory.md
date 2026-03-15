---
---
[English](../05a_inventory_en)

# Das Inventar (inventory.json)

Das Herzstück eines jeden OCFL-Objekts ist das `inventory.json`. Es enthält alle Metadaten, die notwendig sind, um den Zustand des Objekts und seiner Versionen vollständig zu rekonstruieren.

Hinweis: Das vollständige Inventar findet sich hier: [Vollständiges Inventar](../05b_full_inventory)

## Ausschnitte aus dem Inventory

### 1. Header (Kopfbereich)
```json
{
  "id": "urn:nbn:de:gbv:42-test1",
  "type": "https://ocfl.io/1.1/spec/#inventory",
  "digestAlgorithm": "sha512",
  "head": "v1",
  "contentDirectory": "content"
}
```
Erläuterung:
- `id`: Eindeutige Kennung des Objekts.
- `type`: Referenz auf die OCFL-Spezifikation (hier 1.1).
- `digestAlgorithm`: Primärer Hash-Algorithmus für Manifest und Zustände.
- `head`: Aktuellste Version (hier `v1`).
- `contentDirectory`: Wurzelverzeichnis der Dateien innerhalb einer Version.

### 2. Manifest (Auszug)
```json
{
  "manifest": {
    "1082b5603213...": [
      "v1/content/data/image/IMG_6914.jpg"
    ],
    "6f6a6418e726...": [
      "v1/content/README.md"
    ],
    "87c82e1a5b55...": [
      "v1/content/metadata/info.json"
    ]
  }
}
```
Erläuterung:
- Schlüssel sind Datei-Hashes (gemäß `digestAlgorithm`).
- Werte sind Pfade innerhalb der Versionen (mehrere Pfade sind nur dann möglich, wenn identische Dateien NICHT dedupliziert wurden).

### 3. Versionen (Auszug `v1`)
```json
{
  "versions": {
    "v1": {
      "created": "2026-03-15T15:04:33Z",
      "message": "initial commit",
      "state": {
        "1082b5603213...": [ "data/image/IMG_6914.jpg" ],
        "6f6a6418e726...": [ "README.md" ],
        "87c82e1a5b55...": [ "metadata/info.json" ]
      },
      "user": {
        "name": "User OCFL",
        "address": "mailto:ocfl.user@unibas.ch"
      }
    }
  }
}
```
Erläuterung:
- `created`/`message`/`user`: Metadaten zur Versionserstellung.
- `state`: Logische Sicht auf die Dateien der Version (Pfadnamen ohne `v1/content/`-Präfix).

### 4. Fixity (Auszug)
```json
{
  "fixity": {
    "blake2b-384": {
      "a3ea374ca4db...": [ "v1/content/README.md" ]
    },
    "md5": {
      "2ecbccede07a...": [ "v1/content/README.md" ]
    }
  }
}
```
Erläuterung:
- Zusätzliche Prüfsummen für dieselben Dateien wie im Manifest (zur langfristigen Verifizierbarkeit).

---
Weiterführend: Das vollständige JSON ist hier einsehbar: [Vollständiges Inventar](../05b_full_inventory)

Navigation: [Zurück zur Objektstruktur](../05_object_structure) | [Inhaltsverzeichnis](../toc) | [Nächstes Kapitel: Objekt aktualisieren](../06_update_object)
