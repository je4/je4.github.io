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
- `--config ./gocfl/config/gocfl.toml`: Verwendet unsere zentrale Konfigurationsdatei.
- `add`: Der Befehl zum Hinzufügen/Erstellen eines Objekts.
- `./gocfl/temp/test42/`: Der Pfad zur **Storage Root** (unser Zielverzeichnis).
- `./gocfl/testdata/payload1/`: Der Quellpfad der Daten, die archiviert werden sollen.
- `--ext-NNNN-metafile-source ./gocfl/testdata/info.json`: Ein spezifischer Parameter für die Erweiterung `NNNN-metafile`, um eine externe Metadatendatei einzubinden.
- `-i urn:nbn:de:gbv:42-test1`: Die eindeutige **Objekt-ID**.

## 2. Was passiert beim Hinzufügen?

1. **Hashing:** `gocfl` berechnet die Checksummen (standardmäßig SHA-512) aller Dateien.
2. **Layout:** Die Objekt-ID wird gemäss der konfigurierten Storage-Layout-Extension (z. B. `0004-hashed-n-tuple-storage-layout`) in einen physischen Pfad übersetzt.
3. **Struktur:** Innerhalb des Objektverzeichnisses wird die OCFL-Struktur angelegt (Inventar, `v1/content/`).
4. **Extensions:** Alle konfigurierten Objekt-Extensions werden getriggert (z. B. für Metadaten-Extraktion).

---

[Zurück zu OCFL Extensions](03_extensions.md) | [Zurück zum Inhaltsverzeichnis](toc.md) | [Nächstes Thema: Objektstruktur](05_object_structure.md)
