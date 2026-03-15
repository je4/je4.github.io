---
---
[English](05_object_structure_en)

# Die Verzeichnisstruktur eines OCFL-Objekts

Nachdem ein Objekt mit dem `add`-Befehl hinzugefügt wurde, wird es gemäß dem konfigurierten Storage-Layout in der Storage Root abgelegt. In unserem Workshop-Beispiel liegt das Objekt unter:

`C:\temp\audsworkshop\temp\test42\1f7\d28\1ac\1f7d281accf403621871ec793c3b1c40eb480165e17b80da924aba0280246d12`

## 1. Übersicht der Struktur (Tree)

Hier ist die tatsächliche Verzeichnisstruktur des OCFL-Objekts (v1.1) nach dem ersten Ingest (`v1`) in unserem Workshop-Szenario:

```text
1f7d281accf403621871ec793c3b1c40eb480165e17b80da924aba0280246d12/
│   0=ocfl_object_1.1          # Markierung: Dies ist ein OCFL-Objekt (Version 1.1)
│   inventory.json             # Das aktuelle Inventar (Manifest, State, Versionen)
│   inventory.json.sha512      # Prüfsumme des aktuellen Inventars
│
├───extensions                 # Objekt-spezifische Erweiterungskonfigurationen
│   ├───0001-digest-algorithms
│   ├───0011-direct-clean-path-layout
│   ├───initial
│   ├───NNNN-content-subpath
│   ├───NNNN-filesystem
│   ├───NNNN-gocfl-extension-manager
│   ├───NNNN-indexer
│   ├───NNNN-metafile
│   │       gocfl-info-1.0.json
│   └───NNNN-thumbnail
│
└───v1                         # Die erste Version des Objekts
    │   inventory.json         # Kopie des Inventars zum Zeitpunkt von v1
    │   inventory.json.sha512  # Prüfsumme der Inventarkopie
    │
    └───content                # Die eigentlichen Inhaltsdateien von v1
        │   README.md
        │
        ├───data               # Nutzdaten von v1
        │   ├───=u007Etest=u005B0=u005D
        │   │       .empty
        │   │
        │   ├───audio
        │   │       Dragan_Espenschied_-_10_-_Оля=u0020Зимой.mp3
        │   │       Education=u0020of=u0020the=u0020Noobz=u00201.doc
        │   │       Education=u0020of=u0020the=u0020Noobz=u00201.docx
        │   │       Education=u0020of=u0020the=u0020Noobz=u00201.odt
        │   │
        │   ├───image
        │   │       IMG_6914.gif
        │   │       IMG_6914.jpg
        │   │       IMG_6914.png
        │   │       IMG_6914.tga
        │   │       IMG_6914.webp
        │   │
        │   ├───text
        │   │       export_mets.xml
        │   │       fulltext_5693376.xml
        │   │       Testseite.pdf
        │   │       Testseite=u0020-=u0020Kopie.pdf
        │   │
        │   ├───vector
        │   │       ki.ai
        │   │       ki.eps
        │   │       ki.pdf
        │   │       ki.svg
        │   │       ki.svgz
        │   │
        │   └───video
        │           Together=u0020Elsewhere=u002001.docx
        │           Together=u0020Elsewhere=u002001.pdf
        │           together_01_excerpt.mkv
        │
        └───metadata           # Metadaten von v1
            │   filesystem_v1.jsonl.gz
            │   indexer_v1.jsonl.gz
            │   info.json
            │   thumbnail_v1.jsonl.gz
            │
            └───thumbnails     # Vorschaubilder von v1
                └───v1
                        00001.png
                        00002.png
                        00003.png
                        00004.png
                        00006.png
```

Eine detaillierte Darstellung der Struktur nach einem Update (v2) finden Sie im Kapitel **[Aktualisieren eines Objektes](06_update_object.md#3-struktur-nach-dem-update)**.

## 2. Erläuterung der Komponenten

### A. Pfad- und Layout-Erweiterungen

Diese Erweiterungen bestimmen, wie die Dateien physisch in der Storage Root und innerhalb des Objekts abgelegt werden.

#### Die Objekt-ID und das Layout (Storage Root)
Der lange Verzeichnisname `1f7d281acc...` ist das Ergebnis der Hashing-Strategie der Erweiterung `0004-hashed-n-tuple-storage-layout`. In unserem Fall wurde die Objekt-ID `urn:nbn:de:gbv:42-test1` gehasht (SHA-256), was den Wert `1f7d281accf403621871ec793c3b1c40eb480165e17b80da924aba0280246d12` ergab. Gemäß den Standardeinstellungen (`tupleSize: 3`, `numberTuples: 3`) wurde der Pfad `1f7/d28/1ac/` vorangestellt.

#### Dateinamen-Bereinigung (0011-direct-clean-path-layout)

Innerhalb des Objekts (unter `v1/content/data/`) werden die Dateinamen durch die Erweiterung **[0011-direct-clean-path-layout](https://ocfl.github.io/extensions/0011-direct-clean-path-layout.html)** bereinigt, um die Kompatibilität mit verschiedenen Dateisystemen sicherzustellen.

**Was die Extension macht:**
- **Zeichen-Ersetzung:** Problematische Zeichen (wie Leerzeichen, Kontrollzeichen oder reservierte Zeichen wie `*`, `?`, `:`) werden durch ihren Unicode-Hex-Code im Format `=uXXXX` ersetzt.
- **UTF-8 Validierung:** Stellt sicher, dass alle Pfade gültiges UTF-8 sind.
- **Längenbeschränkungen:** Verhindert zu lange Pfadsegmente.

**Beispiel aus unserem Tree:**
Die Datei `Dragan_Espenschied_-_10_-_Оля Зимой.mp3` enthält ein Leerzeichen. Im OCFL-Objekt wird daraus:
`Dragan_Espenschied_-_10_-_Оля=u0020Зимой.mp3`

Hierbei steht `=u0020` für das Leerzeichen (U+0020). Diese Ersetzung erfolgt automatisch durch `gocfl` beim Hinzufügen der Dateien, um sicherzustellen, dass das Archiv auf jedem Betriebssystem (Windows, Linux, macOS) ohne Probleme gelesen werden kann.

#### Content-Struktur (data & metadata) und NNNN-content-subpath

Ein wesentlicher Unterschied zu einer Standard-OCFL-Struktur ist die Aufteilung innerhalb der Versionsverzeichnisse (z. B. `v1/content/`). Diese wird durch die Erweiterung **`NNNN-content-subpath`** gesteuert.

Die Erweiterung ermöglicht es, verschiedene logische Bereiche (Areas) zu definieren, die auf physische Unterverzeichnisse im `content`-Ordner gemappt werden:

- **`data/` (Area: `content`)**: Dies ist der Standardbereich für die eigentlichen Nutzdaten (Payload). Gemäß der Konfiguration von `gocfl` wird der logische Bereich `content` physisch im Ordner `data/` abgelegt. Hier findet auch das **URL-Encoding** von Sonderzeichen statt, um die Dateisystem-Kompatibilität zu erhöhen.
- **`metadata/` (Area: `metadata`)**: In diesem Bereich legen andere Erweiterungen ihre generierten Dateien ab. In unserem Workshop-Beispiel ist dieser Bereich auf den Ordner `metadata/` gemappt.
- **`README.md`**: Die Erweiterung `NNNN-content-subpath` erstellt automatisch eine `README.md` direkt im `content`-Verzeichnis jeder Version, welche die Bedeutung der Unterordner für menschliche Betrachter erklärt.

### B. Metadaten-Erweiterungen

Diese Erweiterungen reichern das Objekt während des Ingests automatisch mit Informationen an. Im `metadata/`-Ordner der ersten Version sehen wir die Ergebnisse:

- **`indexer_v1.jsonl.gz`**: Technische Metadaten (Dateiformate, Bitraten, Auflösungen), die von der Erweiterung `NNNN-indexer` extrahiert wurden.
- **`filesystem_v1.jsonl.gz`**: Ursprüngliche Dateisystem-Metadaten (Erstellungsdatum, Zugriffsrechte), gesichert durch `NNNN-filesystem`.
- **`info.json`**: Die benutzerdefinierten Metadaten, die wir über den Parameter `--ext-NNNN-metafile-source` mitgegeben haben (`NNNN-metafile`).

### C. Erweiterungen für Lesbarkeit und Vorschau

Diese Gruppe dient dem schnellen Überblick über den Inhalt:

- **`thumbnails/`**: Die Erweiterung `NNNN-thumbnail` hat für unterstützte Dateitypen (Bilder, Videos, PDFs) automatisch Vorschaubilder generiert. Diese liegen im Unterordner `metadata/thumbnails/v1/` und sind in der entsprechenden Index-Datei (`thumbnail_v1.jsonl.gz`) verzeichnet.

## 3. Extensions im Objekt
Wie die Storage Root besitzt auch jedes Objekt ein `extensions/`-Verzeichnis. Hier speichert `gocfl` die Konfigurationen für alle aktiven Erweiterungen. Eine vollständige Liste aller Extensions finden Sie unter **[Liste der OCFL Extensions](03b_extension_list)**.

---

[Zurück zum Hinzufügen von Objekten](04_add_object) | [Zurück zum Inhaltsverzeichnis](toc) | [Nächstes Thema: Objekt aktualisieren](06_update_object)
