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

## 3. Extensions in der Praxis mit `gocfl`

Wie wir bei der Initialisierung der Storage Root gesehen haben, legt `gocfl` automatisch Konfigurationsverzeichnisse an.

### Beispiel: Storage Layout (`0004`)
In der `ocfl_layout.json` wird auf die Erweiterung [0004-hashed-n-tuple-storage-layout](https://ocfl.github.io/extensions/0004-hashed-n-tuple-storage-layout.html) verwiesen. Die zugehörige Konfiguration liegt unter:
`extensions/0004-hashed-n-tuple-storage-layout/config.json`

Inhalt der `config.json`:
```json
{
   "extensionName": "0004-hashed-n-tuple-storage-layout"
}
```
Diese Datei steuert (zusammen mit den Angaben in `ocfl_layout.json`), wie tief die Verzeichnisstruktur geschachtelt wird und welcher Hash-Algorithmus für die Pfadbildung genutzt wird. In der Minimalversion reicht hier oft der Name der Extension aus.

### Beispiel: Initial Extension (`initial`)
Die [Initial Extension](https://ocfl.github.io/extensions/initial.html) dient in `gocfl` dazu, festzulegen, welche Erweiterung als erstes geladen werden soll.

Inhalt der `extensions/initial/config.json`:
```json
{
   "extensionName": "initial",
   "extension": "NNNN-gocfl-extension-manager"
}
```
In unserem Beispiel in `test42` verweist sie auf den `NNNN-gocfl-extension-manager`. Dies stellt sicher, dass die grundlegende Verwaltungslogik von `gocfl` aktiv ist, bevor andere Erweiterungen verarbeitet werden.

### Beispiel: gocfl Extension Manager (`NNNN-gocfl-extension-manager`)
Dies ist eine Meta-Erweiterung von `gocfl`, die ausschließlich für die Initialisierung der Storage Root zuständig ist (z. B. das Kopieren der Dokumentationsdateien).

Inhalt der `extensions/NNNN-gocfl-extension-manager/config.json`:
```json
{
   "extensionName": "NNNN-gocfl-extension-manager",
   "sort": {
      "StorageRootPath": [
         "0004-hashed-n-tuple-storage-layout"
      ]
   },
   "exclusion": {
      "StorageRootPath": [
         [
            "NNNN-direct-clean-path-layout",
            "0003-hash-and-id-n-tuple-storage-layout",
            "0004-hashed-n-tuple-storage-layout",
            "0002-flat-direct-storage-layout",
            "0006-flat-omit-prefix-storage-layout",
            "NNNN-pairtree-storage-layout",
            "NNNN-direct-path-layout"
         ]
      ]
   }
}
```
Der Extension Manager steuert hierbei auch die Abhängigkeiten:
- **`sort`**: Legt fest, dass das Layout-Modul `0004` bevorzugt behandelt wird.
- **`exclusion`**: Verhindert, dass mehrere (sich gegenseitig ausschließende) Layout-Strategien gleichzeitig für die Storage Root aktiviert werden.

## 4. Self-Describing durch Dokumentation

Ein wichtiger Aspekt von `gocfl` ist, dass zu jeder aktivierten Erweiterung die entsprechende Dokumentation als Markdown-Datei (z. B. `NNNN-gocfl-extension-manager.md`) direkt in die Storage Root kopiert werden sollte. 

Dies garantiert, dass auch in Jahrzehnten noch nachvollziehbar ist, welche Regeln für die Ablage und Verarbeitung der Daten galten, selbst wenn die ursprüngliche Software oder Webseite nicht mehr existiert.

---

[Zurück zum Inhaltsverzeichnis](toc.md) | [Nächstes Thema: Erstellung von Objekten](04_create_object.md)
