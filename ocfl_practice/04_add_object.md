---
---
[English](../04_add_object_en)

# Erstellen und Hinzufügen von Objekten

In diesem Schritt fügen wir unser erstes Objekt zur OCFL Storage Root hinzu. Der Befehl `add` wird verwendet, um neue Inhalte in das Archiv zu überführen.

## 1. Der `add`-Befehl

Mit `gocfl add` können Dateien oder Verzeichnisse als OCFL-Objekt gespeichert werden.

### Grundlegende Syntax:
```bash
gocfl add /pfad/zu/den/daten -i "objekt-id"
```

### Beispiel aus dem Workshop:
In unserem Workshop-Szenario fügen wir den Inhalt aus `payload1/` zur zuvor initialisierten Storage Root `test42/` hinzu:

```bash
gocfl --log-level DEBUG --config ./gocfl/config/gocfl.toml add ./gocfl/temp/test42/ ./gocfl/testdata/payload1/ --ext-NNNN-metafile-source ./gocfl/testdata/info.json -i urn:nbn:de:gbv:42-test1
```

**Erklärung der Parameter:**
- `--log-level DEBUG`: Zeigt detaillierte Informationen während des Vorgangs an.
- `--config ./gocfl/config/gocfl.toml`: Verwendet unsere zentrale Konfigurationsdatei. Hierbei werden vordefinierte Werte aus dem `[Add]`-Bereich der `gocfl.toml` übernommen (siehe unten).
- `add`: Der Befehl zum Hinzufügen/Erstellen eines Objekts.
- `./gocfl/temp/test42/`: Der Pfad zur **Storage Root** (unser Zielverzeichnis).
- `./gocfl/testdata/payload1/`: Der Quellpfad der Daten, die archiviert werden sollen.
- `--ext-NNNN-metafile-source ./gocfl/testdata/info.json`: Ein spezifischer Parameter für die Erweiterung `NNNN-metafile`, um eine externe Metadatendatei einzubinden.
- `-i urn:nbn:de:gbv:42-test1`: Die eindeutige **Objekt-ID**.

### 2. Konfiguration in der `gocfl.toml`

In der zentralen Konfigurationsdatei `gocfl.toml` sind im Bereich `[Add]` Standardwerte definiert, die für jeden `add`-Vorgang gelten, sofern sie nicht durch Kommandozeilenparameter überschrieben werden:

```toml
[Add]
# Standard-Commit-Message
Message="initial commit"
# Zu verwendender Hashing-Algorithmus
Digest="sha512"
# Zusätzliche Fixity-Hashes, die berechnet werden sollen
Fixity=["sha256", "sha1", "md5"]
# Pfad zu den Standard-Objekt-Extensions
ObjectExtensions="/home/ocfl/gocfl/config/extensions/object"
# Standard-Datenbereich für die Extraktion
DefaultArea="content"

[Add.User]
# Standard-Benutzername für das OCFL-Inventar
Name="User OCFL"
# Standard-E-Mail-Adresse
Address="mailto:ocfl.user@unibas.ch"
```

## 3. Was passiert beim Hinzufügen?

1. **Hashing:** `gocfl` berechnet die Checksummen (standardmäßig SHA-512) aller Dateien.
   *Hinweis:* In OCFL sind standardmäßig nur `sha512` und `sha256` als Haupt-Algorithmen erlaubt. Die Verwendung weiterer Algorithmen (z. B. `md5`, `sha1`) im Bereich **Fixity** wird durch die Extension [0009-digest-algorithms](https://ocfl.github.io/extensions/0009-digest-algorithms.html) ermöglicht.
2. **Layout:** Die Objekt-ID wird gemäss der konfigurierten Storage-Layout-Extension (z. B. `0004-hashed-n-tuple-storage-layout`) in einen physischen Pfad übersetzt.
3. **Struktur:** Innerhalb des Objektverzeichnisses wird die OCFL-Struktur angelegt (Inventar, `v1/content/`).
4. **Extensions:** Alle konfigurierten Objekt-Extensions werden getriggert (z. B. für Metadaten-Extraktion). Hierbei werden auch verschiedene **Datenbereiche (Areas)** durch die `NNNN-content-subpath`-Extension verwaltet. Eine detaillierte Übersicht der verwendeten Erweiterungen findest du im Kapitel [OCFL Object Extensions](../03c_object_extensions).

---

[Zurück zu OCFL Extensions](../03_extensions) | [Zurück zum Inhaltsverzeichnis](../toc) | [Nächstes Thema: Objektstruktur](../05_object_structure)
