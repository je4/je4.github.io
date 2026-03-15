---
---
[English](../03c_object_extensions_en)

# OCFL Objekt-Erweiterungen (Extensions)

In diesem Kapitel betrachten wir die Erweiterungen, die direkt auf Ebene des OCFL-Objekts im `extensions/`-Verzeichnis definiert sind. Diese steuern, wie Inhalte innerhalb des Objekts abgelegt werden und welche zusätzlichen Metadaten oder Funktionen (wie Indexierung oder Thumbnails) für dieses spezifische Objekt aktiv sind.

## 1. Übersicht der verwendeten Erweiterungen

Das Beispielobjekt `test42` verwendet die folgenden Erweiterungen:

1. **0009-digest-algorithms**: Registriert zusätzliche Hash-Algorithmen.
2. **0011-direct-clean-path-layout**: Bereinigt Dateipfade für die Speicherung.
3. **initial**: Verweist auf den initialen Extension Manager.
4. **NNNN-content-subpath**: Unterteilt das Objekt in verschiedene Bereiche (Areas).
5. **NNNN-filesystem**: Speichert Dateisystem-Attribute (z.B. Zeitstempel).
6. **NNNN-gocfl-extension-manager**: Verwaltet die Reihenfolge und Ausführung der Erweiterungen.
7. **NNNN-indexer**: Erstellt technische Metadaten via Siegfried, Tika, ffprobe etc.
8. **NNNN-metafile**: Erzeugt eine zentrale `info.json` mit Objektmetadaten.
9. **NNNN-thumbnail**: Generiert Vorschaubilder für visuelle Medien.

## 2. Details der Konfigurationen

Hier sind die spezifischen Konfigurationen der wichtigsten Erweiterungen:

### NNNN-content-subpath
Diese Erweiterung definiert, in welchen Unterverzeichnissen die verschiedenen Datentypen innerhalb des Objekts abgelegt werden.

*   **subPath**: Definiert die Zuordnung von "Areas" (Bereichen) zu Verzeichnispfaden.
    *   **content**: Der Bereich für die eigentlichen Nutzdaten (`data/`).
    *   **metadata**: Der Bereich für zusätzliche technische oder semantische Metadaten (`metadata/`).
    *   **documentation**: Bereich für Dokumentationsdateien (`documentation/`).

```json
{
   "extensionName": "NNNN-content-subpath",
   "subPath": {
      "content": {
         "path": "data",
         "description": "Payload of archival object"
      },
      "documentation": {
         "path": "documentation",
         "description": "documentation of the archival object"
      },
      "metadata": {
         "path": "metadata",
         "description": "additional semantic metadata"
      }
   }
}
```

### 0011-direct-clean-path-layout
Verantwortlich für die Abbildung der logischen Dateipfade auf die physischen Pfade im Speicher. Sie bereinigt Sonderzeichen und stellt die Kompatibilität mit verschiedenen Dateisystemen sicher.

*   **maxPathnameLen / maxPathSegmentLen**: Begrenzt die gesamte Pfadlänge und die Länge einzelner Segmente (Ordner-/Dateinamen), um Dateisystemlimits (z.B. unter Windows) nicht zu überschreiten.
*   **replacementString**: Zeichen, das problematische Sonderzeichen (wie `:`, `*`, `?` etc.) in Dateinamen ersetzt (hier: `_`).
*   **whitespaceReplacementString**: Zeichen, das Leerzeichen ersetzt (hier bleibt es bei einem Leerzeichen `" "`).
*   **utfEncode**: Stellt sicher, dass Pfade korrekt in UTF-8 kodiert werden.
*   **fallbackDigestAlgorithm**: Der Hash-Algorithmus, der für die Benennung im Fallback-Mechanismus verwendet wird (hier: `sha512`).
*   **fallbackFolder**: Name des Verzeichnisses, in dem Dateien landen, deren Pfade trotz Bereinigung zu lang sind.
*   **numberOfFallbackTuples / fallbackTupleSize**: Steuern die Unterteilung (Sharding) des Fallback-Verzeichnisses via Tuples (ähnlich wie beim Storage Layout). Hier deaktiviert (`0`).

```json
{
   "extensionName": "0011-direct-clean-path-layout",
   "maxPathnameLen": 32000,
   "maxPathSegmentLen": 127,
   "replacementString": "_",
   "whitespaceReplacementString": " ",
   "utfEncode": true,
   "fallbackDigestAlgorithm": "sha512",
   "fallbackFolder": "fallback",
   "numberOfFallbackTuples": 0,
   "fallbackTupleSize": 0
}
```

### NNNN-indexer
Diese Erweiterung führt während des Ingest-Prozesses verschiedene Werkzeuge aus, um technische Metadaten zu extrahieren.

*   **StorageType / StorageName**: Gibt an, dass die Ergebnisse im Bereich `metadata` (siehe `content-subpath`) gespeichert werden.
*   **Actions**: Liste der Tools, die auf jede Datei angewendet werden (z.B. Siegfried für die Formaterkennung, FFProbe für Video-Metadaten).
*   **Compress**: Die extrahierten Metadaten werden platzsparend als `gzip` gespeichert.

```json
{
   "extensionName": "NNNN-indexer",
   "StorageType": "area",
   "StorageName": "metadata",
   "Actions": [
      "siegfried",
      "xml",
      "tika",
      "ffprobe",
      "identify"
   ],
   "Compress": "gzip"
}
```

### NNNN-metafile
Erstellt eine `info.json` im `metadata`-Bereich, welche die grundlegenden OCFL-Informationen zusammenfasst.

*   **name**: Der Dateiname der generierten Metadatendatei (`info.json`).
*   **schema**: Verweist auf die JSON-Schema-Definition, gegen welche die Datei validiert werden kann.

```json
{
   "extensionName": "NNNN-metafile",
   "storageType": "area",
   "storageName": "metadata",
   "name": "info.json",
   "schema": "gocfl-info-1.0.json",
   "schemaUrl": "https://raw.githubusercontent.com/ocfl-archive/gocfl/main/gocfl-info-1.0.json"
}
```

## 3. Extension Manager (NNNN-gocfl-extension-manager)

Der Manager steuert die Ausführungsreihenfolge (Hooks). Dies ist wichtig, da z.B. das Layout (`0011`) angewendet werden muss, bevor der Pfad durch `content-subpath` weiter unterteilt wird.

*   **sort**: Legt die Reihenfolge der Erweiterungen für bestimmte funktionale Bereiche oder Ereignisse (Hooks) fest. Die Sortierung erfolgt dabei **innerhalb** jedes Hooks separat:
    *   **ObjectChange**: Bereich für Änderungen am Objekt. Innerhalb dieses Hooks wird zuerst indexiert, dann die Metadatei erstellt.
    *   **ObjectContentPath**: Bereich für die Pfadbildung. Innerhalb dieses Hooks wird zuerst der Pfad bereinigt (`0011`), dann in die Areas (`content-subpath`) einsortiert.
*   **exclusion**: Verhindert, dass sich gegenseitig ausschließende Erweiterungen gleichzeitig aktiv sind. Auch hier wird zuerst der betroffene Bereich definiert.
    *   **ObjectContentPath**: In diesem Bereich dürfen `direct-clean-path-layout` und `direct-path-layout` nicht gleichzeitig verwendet werden.

```json
{
   "extensionName": "NNNN-gocfl-extension-manager",
   "sort": {
      "ObjectChange": [
         "NNNN-indexer",
         "NNNN-metafile"
      ],
      "ObjectContentPath": [
         "NNNN-direct-clean-path-layout",
         "NNNN-content-subpath"
      ]
   },
   "exclusion": {
      "ObjectContentPath": [
         [
            "NNNN-direct-clean-path-layout",
            "NNNN-direct-path-layout"
         ]
      ]
   }
}
```

---

[Zurück zur Übersicht: Erweiterungen](../03_extensions) | [Zurück zum Inhaltsverzeichnis](../toc)
