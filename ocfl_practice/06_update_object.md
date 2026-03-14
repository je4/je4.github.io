[English](06_update_object_en.md)

# Aktualisieren eines Objektes (update)

OCFL ist von Grund auf auf Versionierung ausgelegt. Wenn sich der Inhalt eines Objekts ändert (Dateien werden hinzugefügt, gelöscht oder verändert), wird keine bestehende Datei überschrieben. Stattdessen wird eine neue **Version** (`v2`, `v3` etc.) erstellt.

In `gocfl` wird hierfür der Befehl `update` verwendet.

## 1. Der `update`-Befehl

Der `update`-Befehl ähnelt dem `add`-Befehl, bezieht sich aber auf eine bereits existierende Objekt-ID in der Storage Root.

### Grundlegende Syntax:
```bash
gocfl update /pfad/zu/den/neuen_daten -i "existierende-objekt-id"
```

### Beispiel aus dem Workshop:
Angenommen, wir möchten zusätzliche Daten (z. B. aus `payload2/`) zu unserem Objekt `urn:nbn:de:gbv:42-test1` hinzufügen:

```bash
gocfl --log-level DEBUG --config ./gocfl/config/gocfl.toml update ./gocfl/temp/test42/ ./gocfl/testdata/payload2/ -i urn:nbn:de:gbv:42-test1
```

**Erklärung der Parameter:**
- `update`: Der Befehl zum Erstellen einer neuen Version.
- `./gocfl/temp/test42/`: Der Pfad zur **Storage Root**.
- `./gocfl/testdata/payload2/`: Der Quellpfad der *zusätzlichen* oder *geänderten* Daten.
- `-i urn:nbn:de:gbv:42-test1`: Die **Objekt-ID** des bereits existierenden Objekts.

## 2. Was passiert bei einem Update?

1. **Versions-Inkrement:** `gocfl` erkennt, dass die ID bereits existiert und die aktuelle Version `v1` ist. Es bereitet die Erstellung von `v2` vor.
2. **Deduplizierung (Content-Addressable Storage):** 
   - `gocfl` berechnet die Checksummen der neuen Dateien.
   - Wenn eine Datei bereits in einer früheren Version (`v1`) mit exakt derselben Checksumme vorhanden war, wird sie in `v2` nur referenziert (im Inventar), aber **nicht erneut physisch gespeichert**. Dies spart massiv Speicherplatz.
3. **Neues Versionsverzeichnis:** Ein neuer Ordner `v2/` wird im Objektverzeichnis angelegt.
4. **Inventar-Update:**
   - Die `inventory.json` im Hauptverzeichnis des Objekts wird aktualisiert. Sie enthält nun Informationen über `v1` und `v2`.
   - Eine Kopie der neuen `inventory.json` wird in `v2/` abgelegt.
5. **Extensions:** Alle konfigurierten Erweiterungen (Metadaten-Extraktion, Thumbnails etc.) laufen erneut für die neuen Inhalte an.

## 3. Struktur nach dem Update

Nach dem Update auf Version 2 (`v2`) zeigt sich die Stärke von OCFL: Das Objekt enthält nun beide Stände, wobei unveränderte Dateien effizient dedupliziert werden.

Hier ist der reale Verzeichnisbaum unseres Workshop-Objekts nach dem Update:

```text
1f7d281accf403621871ec793c3b1c40eb480165e17b80da924aba0280246d12/
│   0=ocfl_object_1.1
│   inventory.json             # Enthält nun v1 und v2
│   inventory.json.sha512
│
├───extensions/
│
├───v1/                        # Unveränderter Stand von Version 1
│   │   inventory.json
│   └───content/
│       ├───data/              # Alle Dateien aus payload1/
│       └───metadata/          # Metadaten und Thumbnails von v1
│
└───v2/                        # Neuer Stand von Version 2
    │   inventory.json         # Aktuelles Inventar
    └───content/
        │   README.md
        │
        ├───data/              # Physisch neue Dateien
        │   └───image/
        │           IMG_6914.png
        │
        └───metadata/          # Neue Metadaten-Logs für v2
            │   filesystem_v2.jsonl.gz
            │   indexer_v2.jsonl.gz
            │   thumbnail_v2.jsonl.gz
            │
            └───thumbnails/    # Neu generierte Vorschaubilder
                └───v2/
                        00001.png
```

### Was fällt in der v2-Struktur auf?

1. **Deduplizierung in `v2/content/data/`**: Obwohl wir in unserem Beispiel den gesamten Ordner `payload2/` (der viele identische Dateien zu `payload1/` enthält) für das Update verwendet haben, liegt physisch nur die tatsächlich geänderte Datei `IMG_6914.png` im `data`-Ordner von `v2`. Alle anderen Dateien werden im Inventar referenziert, aber nicht erneut gespeichert.
2. **Neue Metadaten-Logs**: In `v2/content/metadata/` wurden neue Versionen der Log-Dateien (`_v2.jsonl.gz`) angelegt, die den Zustand des Objekts zum Zeitpunkt des zweiten Ingests beschreiben.
3. **Zentrale `inventory.json`**: Die Datei im Hauptverzeichnis des Objekts ist nun das "Master-Inventar", das über alle Versionen Buch führt.

Dank dieser Struktur ist es jederzeit möglich, auf den exakten Zustand von `v1` zuzugreifen, selbst wenn Dateien in `v2` gelöscht oder verändert wurden.

## 4. Identifikation von Änderungen im Inventar

Die `inventory.json` ist die zentrale Quelle, um zu verstehen, was sich zwischen den Versionen geändert hat. Ein Vergleich der `state`-Blöcke von `v1` und `v2` offenbart die durchgeführten Aktionen:

### A. Löschungen
Wenn ein Dateipfad in `v1` vorhanden ist, aber im `state` von `v2` fehlt, gilt die Datei als gelöscht. Physisch bleibt sie in `v1/content/` erhalten, ist aber im logischen Zustand von `v2` nicht mehr sichtbar.

**Beispiel im Inventar:**
In `v1.state` ist die Datei noch vorhanden:
```json
"versions": {
  "v1": {
    "state": {
      "b78834c6ee794...": [ "data/image/IMG_6914.tga" ]
    }
  }
}
```
Im `state` von `v2` fehlt dieser Eintrag für den Pfad `data/image/IMG_6914.tga`.

### B. Umbenennungen und Verschiebungen
In OCFL werden Umbenennungen hocheffizient gehandhabt. Wenn eine Datei einen neuen Pfad hat, aber derselbe Digest (Prüfsumme) wie in einer vorherigen Version verwendet wird, erkennt OCFL dies als Umbenennung oder Verschiebung.

**Beispiel Umbenennung:**
Die Datei behält ihren Digest, ändert aber den Pfad im `state`:
- `v1.state`: `"9c9d3b13...": [ "data/audio/Education of the Noobz 1.docx" ]`
- `v2.state`: `"9c9d3b13...": [ "data/audio/Education of the Noobz 1_renamed.docx" ]`

**Beispiel Verschiebung:**
- `v1.state`: `"1082b560...": [ "data/image/IMG_6914.jpg" ]`
- `v2.state`: `"1082b560...": [ "data/empty folder/IMG_6914.jpg" ]`

In allen Fällen referenziert das Manifest weiterhin die ursprüngliche Datei in `v1`:
```json
"manifest": {
  "1082b560...": [ "v1/content/data/image/IMG_6914.jpg" ],
  "9c9d3b13...": [ "v1/content/data/audio/Education=u0020of=u0020the=u0020Noobz=u00201.docx" ]
}
```

In `v2` wird keine neue Datei physisch gespeichert. Das Inventar referenziert für den neuen Pfad einfach den bereits in `v1` vorhandenen Datenblock.

---

[Zurück zur Objektstruktur](05_object_structure.md) | [Zurück zum Inhaltsverzeichnis](toc.md) | [Nächstes Thema: Validierung](07_validate_object.md)
