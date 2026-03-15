---
---
[English](03a_storage_root_extensions_en)

# OCFL Storage Root Extensions

Diese Seite beschreibt die spezifischen OCFL-Erweiterungen, die in der Storage Root zur Anwendung kommen. Wie wir bei der Initialisierung der Storage Root gesehen haben, legt `gocfl` automatisch Konfigurationsverzeichnisse im `extensions/`-Ordner an.

## 1. Initial Extension (`initial`)
Die [Initial Extension](https://ocfl.github.io/extensions/initial.html) dient in `gocfl` dazu, festzulegen, welche Erweiterung als erstes geladen werden soll.

Inhalt der `extensions/initial/config.json`:
```json
{
   "extensionName": "initial",
   "extension": "NNNN-gocfl-extension-manager"
}
```
In unserem Beispiel in `test42` verweist sie auf den `NNNN-gocfl-extension-manager`. Dies stellt sicher, dass die grundlegende Verwaltungslogik von `gocfl` aktiv ist, bevor andere Erweiterungen verarbeitet werden.

## 2. gocfl Extension Manager (`NNNN-gocfl-extension-manager`)
Dies ist eine Meta-Erweiterung von `gocfl`, die ausschließlich für die Initialisierung der Storage Root zuständig ist (z. B. das Kopieren der Dokumentationsdateien). Weitere Details finden sich in der [Dokumentation zum Extension Manager](https://github.com/ocfl-archive/gocfl/blob/main/docs/NNNN-gocfl-extension-manager.md).

Im Extension Manager wird der Begriff **`StorageRootPath`** als Bezeichner für den Extension-Typ (in diesem Fall das Storage-Layout der Root) verwendet. In der Go-Implementierung von `gocfl` ist dies in [`storagerootExtension.go`](https://github.com/ocfl-archive/gocfl/blob/ed2bafb10a8c2bdbb2fad3ad271aa7e6993b75ed/pkg/ocfl/storageroot/storagerootExtension.go) definiert.

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
Der Extension Manager steuert hierbei auch die Abhängigkeiten über den Typ `StorageRootPath`:
- **`sort`**: Legt fest, dass das Layout-Modul `0004` bevorzugt behandelt wird.
- **`exclusion`**: Verhindert, dass mehrere (sich gegenseitig ausschließende) Layout-Strategien gleichzeitig für die Storage Root aktiviert werden.

## 3. Storage Layout (`0004`)
In der `ocfl_layout.json` wird auf die Erweiterung [0004-hashed-n-tuple-storage-layout](https://ocfl.github.io/extensions/0004-hashed-n-tuple-storage-layout.html) verwiesen. Die zugehörige Konfiguration liegt unter:
`extensions/0004-hashed-n-tuple-storage-layout/config.json`

Inhalt der `config.json`:
```json
{
   "extensionName": "0004-hashed-n-tuple-storage-layout"
}
```
Diese Datei steuert (zusammen mit den Angaben in `ocfl_layout.json`), wie tief die Verzeichnisstruktur geschachtelt wird und welcher Hash-Algorithmus für die Pfadbildung genutzt wird. In der Minimalversion reicht hier oft der Name der Extension aus, da die Spezifikation Standardwerte (Defaults) vorsieht.

#### Standardwerte der Extension 0004:
Wenn diese Parameter nicht in der `config.json` definiert sind, gelten folgende Defaults gemäß Spezifikation:
- **`digestAlgorithm`**: `sha256` (Der Algorithmus, der zur Berechnung des Hash-Werts der Objekt-ID verwendet wird).
- **`tupleSize`**: `3` (Anzahl der Zeichen pro Verzeichnisebene).
- **`numberTuples`**: `3` (Anzahl der Verzeichnisebenen, die durch das Hashing erstellt werden).

Das bedeutet, ein Objekt wird standardmäßig in einer Struktur wie `[hash1-3]/[hash4-6]/[hash7-9]/[objekt-id]` abgelegt.

---

[Zurück zu OCFL Extensions](03_extensions) | [Zurück zum Inhaltsverzeichnis](toc)
