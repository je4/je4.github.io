# Initialisierung einer OCFL Storage Root

Nachdem wir uns mit den Grundlagen und der Konfiguration von `gocfl` vertraut gemacht haben, ist der erste praktische Schritt in einem OCFL-basierten Archiv die Erstellung einer **Storage Root**.

## 1. Was ist eine Storage Root?

Die [Storage Root](https://ocfl.io/1.1/spec/#storage-root) ist die oberste Ebene Ihres Archivs. Sie ist der Container, in dem alle OCFL-Objekte abgelegt werden. Eine Storage Root enthält:
- Die Datei `ocfl_1.1.txt` (oder eine andere Version), die den Verzeichnispfad als OCFL-Speicher markiert.
- Eine `ocfl_layout.json`, die definiert, wie die Objekte innerhalb der Root strukturiert und benannt werden.
- Einen Ordner `extensions/` für spezifische Funktionserweiterungen.

## 2. Struktur und Anforderungen (Pflicht vs. Optional)

Gemäß der [OCFL-Spezifikation zur Root-Struktur](https://ocfl.io/1.1/spec/#root-structure) gelten für eine Storage Root klare Regeln, um die langfristige Interpretierbarkeit sicherzustellen:

### Pflichtbestandteile
1.  **OCFL-Versionsmarker**: Eine Datei namens `ocfl_v1.1.txt` (oder ähnlich, `gocfl` nutzt `0=ocfl_1.1`), die den Pfad als OCFL Storage Root identifiziert.
2.  **OCFL-Objekte**: Die eigentlichen Daten, die in Unterverzeichnissen organisiert sind.

### Optionale Bestandteile
-   **`ocfl_layout.json`**: Empfohlen, um das Mapping von Objekt-IDs zu Verzeichnispfaden zu definieren.
-   **`extensions/`**: Ein Verzeichnis für Erweiterungskonfigurationen (siehe [OCFL Storage Root Extensions](03a_storage_root_extensions.md)).

### Wichtige Strukturregeln
-   **Keine fremden Ordner**: In der Storage Root dürfen sich als Verzeichnisse **nur** der `extensions`-Ordner (siehe [OCFL Storage Root Extensions](03a_storage_root_extensions.md)) und die Verzeichnisstrukturen für **OCFL-Objekte** befinden. Andere Ordner sind nicht zulässig.
-   **Zusätzliche Dateien**: Andere Dateien (wie die von `gocfl` angelegten `.md`-Dateien) dürfen vorhanden sein, solange sie nicht mit den reservierten Namen oder Objektverzeichnissen kollidieren. Sie dienen der **Self-Documentation**.

## 3. Der Befehl `gocfl init`

Mit `gocfl` initialisieren Sie eine neue Storage Root über den Unterbefehl `init`. 

### Syntax
```bash
gocfl init [Pfad zur Storage Root] [Flags]
```

### Wichtige Flags für `init`
- `--ocfl-version`: Legt die Version der Spezifikation fest (Standard: `1.1`).
- `-d`, `--digest`: Definiert den Standard-Hash-Algorithmus (z. B. `sha512` oder `sha256`).
- `--default-storageroot-extensions`: Pfad zu einem Ordner mit Konfigurationen für Erweiterungen, die initial geladen werden sollen.

## 3. Praktisches Beispiel

In diesem Beispiel initialisieren wir eine Storage Root unter Verwendung einer spezifischen Konfigurationsdatei und eines Zielverzeichnisses:

```bash
gocfl --config ./gocfl/config/gocfl.toml init ./gocfl/temp/test42/
```

### Was passiert dabei?
- `--config ./gocfl/config/gocfl.toml`: `gocfl` lädt die Einstellungen aus der angegebenen Datei (z. B. die OCFL-Version oder den Digest-Algorithmus).
- `init ./gocfl/temp/test42/`: Erstellt die notwendigen OCFL-Strukturdateien im Verzeichnis `./gocfl/temp/test42/`.

Wenn Sie nach dem Befehl in das Verzeichnis schauen, sehen Sie eine Struktur, die über die minimale OCFL-Spezifikation hinausgeht, da `gocfl` nützliche Zusatzinformationen und Konfigurationen mit anlegt:

```text
test42/
├── 0=ocfl_1.1          # Markierung der Storage Root (OCFL Version 1.1)
├── ocfl_layout.json    # Definition des Pfad-Layouts
├── ocfl_spec_1.1.md    # Die OCFL-Spezifikation als Referenz
├── extensions/         # Konfigurationen für aktivierte Erweiterungen
│   ├── 0004-hashed-n-tuple-storage-layout/
│   ├── initial/
│   └── NNNN-gocfl-extension-manager/
└── NNNN-*.md           # Dokumentation der verwendeten Extensions
```

### Die wichtigsten Dateien im Detail

1.  **`0=ocfl_1.1`**: In der Spezifikation heißt diese Datei `ocfl_1.1.txt`. `gocfl` verwendet standardmäßig diesen Namen, um die Version der Storage Root zu deklarieren. Sie ist leer und dient nur als "Namensschild".
2.  **`ocfl_layout.json`**: Diese Datei ist entscheidend für die Skalierbarkeit. In unserem Beispiel wird [0004-hashed-n-tuple-storage-layout](https://ocfl.github.io/extensions/0004-hashed-n-tuple-storage-layout.html) verwendet. Das bedeutet, dass Objekt-IDs (wie `ark:/12345/bcd987`) gehasht und in Unterverzeichnissen verteilt werden (z.B. `a1b/2c3/d4e/...`), um zu verhindern, dass zu viele Ordner in einem einzelnen Verzeichnis liegen.
3.  **`ocfl_spec_1.1.md`**: Diese Datei enthält die vollständige OCFL-Spezifikation direkt in der Storage Root. Damit ist die Root nicht nur **self-contained** (alle Daten sind vorhanden), sondern auch **self-describing**, da die Regeln für den Zugriff und die Interpretation direkt mitgeliefert werden.
4.  **`extensions/`**: Hier liegen die Konfigurationsdateien (`config.json`) für die Erweiterungen (siehe [OCFL Storage Root Extensions](03a_storage_root_extensions.md)). 
    - `0004-...` konfiguriert das oben genannte Layout.
    - `initial` legt fest, welche Erweiterung als erstes geladen wird (in unserem Fall der `NNNN-gocfl-extension-manager`).
    - `NNNN-gocfl-extension-manager` ist eine `gocfl`-spezifische Erweiterung, die ausschließlich für die Initialisierung der Storage Root zuständig ist.
5.  **Die `.md`-Dateien (z.B. `ocfl_spec_1.1.md` und `NNNN-*.md`)**: `gocfl` kopiert bei der Initialisierung sowohl die OCFL-Spezifikation als auch Beschreibungen der verwendeten Erweiterungen (wie `NNNN-indexer.md` oder `NNNN-migration.md`) direkt in die Storage Root. Dies geschieht im Rahmen der Initialisierung durch den Extension Manager. Dies verstärkt den **self-describing** Ansatz: Jeder, der in Zukunft auf dieses Medium zugreift, findet sowohl die allgemeine Spezifikation als auch die Dokumentation der spezifisch verwendeten Funktionen direkt vor Ort, ohne auf externe Webseiten angewiesen zu sein.

---

[Zurück zum Inhaltsverzeichnis](toc.md) | [Nächstes Thema: OCFL Extensions (Erweiterungen)](03_extensions.md)
