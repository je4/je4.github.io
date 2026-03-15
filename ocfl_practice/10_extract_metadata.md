---
---
[English](../10_extract_metadata_en)

# Extrahieren von Metadaten (extractmeta)

Zusätzlich zum Extrahieren der Dateiinhalte bietet `gocfl` die Möglichkeit, die im OCFL-Objekt gespeicherten Metadaten in eine separate JSON-Datei zu exportieren. Dies ist besonders nützlich für die weitere Verarbeitung oder Indexierung in externen Systemen.

## 1. Der `extractmeta`-Befehl

Der Befehl `extractmeta` liest die Metadaten des Objekts (inklusive technischer Metadaten aus den Extensions) und schreibt diese in das angegebene Ziel.

### Grundlegende Syntax:
```bash
gocfl extractmeta [Optionen] [Pfad zur Storage Root oder zum Objekt]
```

### Beispiel aus dem Workshop:
Um die Metadaten unseres Test-Objekts in eine JSON-Datei zu extrahieren, verwenden wir:

```bash
gocfl --log-level DEBUG --config ./gocfl/config/gocfl.toml extractmeta ./gocfl/temp/test42 -i urn:nbn:de:gbv:42-test1 --output ./gocfl/temp/meta.json
```

**Erklärung:**
- `--log-level DEBUG`: Zeigt detaillierte Informationen während des Vorgangs an.
- `extractmeta`: Der Befehl zum Extrahieren der Metadaten.
- `./gocfl/temp/test42`: Der Pfad zur Storage Root.
- `-i urn:nbn:de:gbv:42-test1`: Die ID des Objekts, dessen Metadaten extrahiert werden sollen.
- `--output ./gocfl/temp/meta.json`: Gibt den Dateipfad an, in den die Metadaten geschrieben werden sollen.

## 2. Was wird extrahiert?

Die extrahierte JSON-Datei (`meta.json`) enthält im Wesentlichen drei Arten von Informationen:

1.  **Der Objekt-Kopf:** Grundlegende Metadaten wie die Objekt-ID, der verwendete Hash-Algorithmus und der aktuelle Stand (HEAD).
2.  **Metadaten zu Dateien (Manifest):** Eine Auflistung aller Dateien mit ihren Prüfsummen, physischen Pfaden im Archiv und logischen Pfaden in den jeweiligen Versionen.
3.  **Allgemeine Metadaten aus Extensions:** Technische Zusatzinformationen (wie Mime-Types, Dateigrößen oder Vorschaubilder), die durch die verschiedenen OCFL-Erweiterungen (z. B. `NNNN-indexer`) aggregiert wurden.

## 3. Wie sieht das Ergebnis aus?

Die `meta.json` liefert eine **aggregierte Sicht** auf das OCFL-Objekt. Während das Standard-Inventory eher eine "flache" Liste von Prüfsummen und Pfaden ist, verbindet `extractmeta` diese Informationen mit den Ergebnissen der verschiedenen Erweiterungen.

### Struktur der `meta.json`:
- **ID / Head:** Grundlegende Metadaten des Objekts.
- **Versions:** Die Historie aller Versionen (entspricht dem `versions`-Block im Inventory).
- **Files:** Hier liegt der Hauptunterschied. Jede Datei wird über ihren Digest identifiziert und enthält:
    - **InternalName:** Der physische Pfad im OCFL-Archiv (z.B. `v1/content/data/image.jpg`).
    - **VersionName:** Der logische Pfad innerhalb der jeweiligen Version.
    - **Extension:** Hier werden die durch Extensions gesammelten Zusatzinfos direkt der Datei zugeordnet (z.B. Dateisystem-Attribute, extrahierte Metadaten von Tika/Indexer).

### Beispiel-Ausschnitte

**Objekt-Kopf:**
```json
{
  "ID": "urn:nbn:de:gbv:42-test1",
  "DigestAlgorithm": "sha512",
  "Head": "v1",
  "Versions": {
    "v1": {
      "Created": "2026-03-15T15:04:33Z",
      "Message": "initial commit",
      "Name": "User OCFL",
      "Address": "mailto:ocfl.user@unibas.ch"
    }
  },
  ...
}
```

**Dateieintrag mit Extensions:**
Hier sieht man gut, wie die Extension-Daten (`NNNN-filesystem`, `NNNN-indexer`, `NNNN-thumbnail`) direkt der Datei zugeordnet sind:
```json
"1082b5603213566c3...": {
    "InternalName": ["v1/content/data/image/IMG_6914.jpg"],
    "VersionName": {
        "v1": ["data/image/IMG_6914.jpg"]
    },
    "Extension": {
        "NNNN-filesystem": {
            "v1": [{
                "path": "data/image/IMG_6914.jpg",
                "meta": {
                    "size": 3696602,
                    "mTime": "2023-11-27T16:54:03+01:00"
                }
            }]
        },
        "NNNN-indexer": {
            "mimetype": "image/jpeg",
            "pronom": "fmt/43",
            "type": "image"
        },
        "NNNN-thumbnail": {
            "id": "internal",
            "filename": "metadata/thumbnails/v1/00002.png"
        }
    }
}
```

**Globaler Extensions-Bereich:**
Am Ende der `meta.json` befindet sich ein zusammenfassender Bereich für Erweiterungen, die globale Informationen für das gesamte Objekt bereitstellen:
```json
{
  ...
  "Extension": {
    "NNNN-content-subpath": {
      "content": {
        "path": "data",
        "description": "Payload of archival object"
      },
      "metadata": {
        "path": "metadata",
        "description": "additional semantic metadata"
      }
    },
    "NNNN-metafile": {
      "title": "Some OCFL Testfiles (initial version)",
      "authors": ["Doe, John", "Doe, Jane"],
      "description": "Lorem ipsum dolor sit amet...",
      "created": "2023-10-31",
      "collection": "OCFL Demo"
    }
  }
}
```

### Verhältnis zum Inventory
Das **Inventory** (`inventory.json`) ist die "Wahrheit" des OCFL-Standards. Es ist darauf optimiert, die Integrität und Struktur des Objekts sicherzustellen, ist aber für Menschen oder externe Suchmaschinen schwerer direkt zu konsumieren, da man Informationen über mehrere Blöcke (`manifest`, `versions`, `fixity`) hinweg zusammenführen muss.

Die **`meta.json`** ist eine **aufbereitete Export-Sicht**. Sie löst die Referenzen des Inventories auf und reichert sie um die Daten der konfigurierten Erweiterungen an. Dieser Export dient als Grundlage für die `display`-Funktion von `gocfl` und wird verwendet, um die Daten in das OCFL Native Archive zu vereinnahmen.

### Langzeitarchivierung und Auffindbarkeit

Es ist wichtig zu verstehen: Wir archivieren nicht die Metadaten (`meta.json`) als primäres Objekt. Das **OCFL-Objekt selbst** ist die Einheit der Langzeitarchivierung. Die extrahierten Metadaten dienen dazu, das Objekt im Archiv (z. B. über eine Datenbank oder einen Suchindex) **wiederzufinden** und schnell darauf zugreifen zu können, ohne jedes Mal das gesamte OCFL-Archiv durchsuchen zu müssen.

Damit ist sie die ideale Grundlage für:
- Den Import in eine Datenbank.
- Die Anzeige in einem Web-Frontend.
- Die schnelle Suche und Identifizierung von Objekten im Archiv.
- Die Langzeitarchivierung von technischen Metadaten in einem einfach lesbaren Format als Zusatzinformation.

---

[Zurück zum Extrahieren von Inhalten](../09_extract_object) | [Zurück zum Inhaltsverzeichnis](../toc)
