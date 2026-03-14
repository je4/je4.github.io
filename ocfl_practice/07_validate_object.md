[English](07_validate_object_en.md)

# Validierung (validate)

Ein entscheidender Vorteil von OCFL ist die Integritätssicherung. Mit `gocfl` lässt sich jederzeit überprüfen, ob eine Storage Root oder ein einzelnes Objekt noch der Spezifikation entspricht und ob alle Prüfsummen (Digests) korrekt sind.

## 1. Der `validate`-Befehl

Der Befehl `validate` (oder das Alias `check`) führt eine tiefgehende Prüfung der OCFL-Struktur durch. Dabei werden sowohl die formalen Anforderungen der Spezifikation als auch die Konsistenz der Daten gegen das Inventar geprüft.

### Grundlegende Syntax:
```bash
gocfl validate [Pfad zur Storage Root oder zum Objekt]
```

### Beispiel aus dem Workshop:
Wir validieren das spezifische Objekt in der Storage Root, das wir in den vorherigen Schritten erstellt und aktualisiert haben:

```bash
gocfl --log-level INFO --config ./gocfl/config/gocfl.toml validate ./gocfl/temp/test42/ -i urn:nbn:de:gbv:42-test1
```

**Erklärung der Parameter:**
- `validate`: Der Befehl zur Überprüfung der Integrität.
- `./gocfl/temp/test42/`: Der Pfad zur **Storage Root**.
- `-i urn:nbn:de:gbv:42-test1`: Die spezifische **Objekt-ID**, die geprüft werden soll. Wenn diese ID (oder das Flag `-i`) weggelassen wird, validiert `gocfl` nacheinander **alle** in der Storage Root gefundenen Objekte.

## 2. Was wird geprüft?

Bei der Validierung führt `gocfl` folgende Schritte aus:

1. **Strukturprüfung:** Entspricht der Aufbau der Storage Root und der Objekte der OCFL-Spezifikation (Vorhandensein der Namensschilder, korrekte Verzeichnisstruktur)?
2. **Inventar-Validierung:** Ist die `inventory.json` wohlgeformt? Sind alle Versionen lückenlos vorhanden?
3. **Fixity-Check:** Berechnet für jede Datei im Objekt erneut den Digest und vergleicht ihn mit dem Wert im Manifest.
4. **Referenzprüfung:** Zeigen alle Pfade im `state` auf existierende Dateien im `manifest`? Gibt es "verwaiste" Dateien im `content`-Ordner, die in keinem Manifest gelistet sind?
5. **Extension-Check:** Sind die verwendeten Erweiterungen korrekt konfiguriert und dokumentiert?

## 3. Selektive Validierung

Falls eine Storage Root sehr viele Objekte enthält, kann die Validierung zeitaufwendig sein. `gocfl` bietet Flags, um die Prüfung einzugrenzen:

- **Nur ein bestimmtes Objekt (ID):**
  ```bash
  gocfl validate ./gocfl/temp/test42/ --object-id urn:nbn:de:gbv:42-test1
  ```
- **Nur ein Objekt an einem bestimmten Pfad:**
  ```bash
  gocfl validate ./gocfl/temp/test42/ --object-path 1f7/d28/1ac/1f7d281acc...
  ```

## 4. Ergebnis der Validierung

Wenn die Validierung abgeschlossen ist, gibt `gocfl` eine Zusammenfassung aus. Dabei ist es wichtig, zwischen **Fehlern (Errors)** und **Warnungen (Warnings)** zu unterscheiden.

### Beispiel-Output aus dem Workshop:
```text
{"level":"info","time":"2026-03-14T17:13:59Z","message":"loading configuration from "}
2026-03-14T17:13:59Z INF validating '/home/ocfl/gocfl/temp/test42' host=gocfl timestamp="..."
2026-03-14T17:13:59Z WRN extension NNNN-gocfl-extension-manager is not registered ... validationErrorCode=W013 validationErrorMessage="‘In an OCFL Object, extension sub-directories SHOULD be named according to a registered extension name.’"
2026-03-14T17:13:59Z WRN extension NNNN-content-subpath is not registered ... validationErrorCode=W013
...
2026-03-14T17:13:59Z INF object 'urn:nbn:de:gbv:42-test1' with object version '1.1' found host=gocfl task=checker ...

[extension NNNN-content-subpath is not registered]
   #W013 - ‘In an OCFL Object, extension sub-directories SHOULD be named according to a registered extension name.’ []

...

no errors found
2026-03-14T17:14:00Z INF Duration: 221.808179ms host=gocfl ...
```

### Interpretation des Ergebnisses:

1.  **Warnungen (W-Codes, z.B. W013):**
    *   In unserem Beispiel sehen wir mehrere Warnungen mit dem Code `W013`.
    *   Diese beziehen sich darauf, dass die verwendeten Extensions (z.B. `NNNN-content-subpath`) nicht offiziell bei der OCFL-Community registriert sind (daher das Präfix `NNNN`).
    *   Gemäß OCFL-Spezifikation **sollten** (SHOULD) Extensions registriert sein, aber es ist **erlaubt**, eigene oder Community-Extensions zu verwenden.
    *   Eine Warnung bedeutet **nicht**, dass das Objekt invalide ist.

2.  **Fehler (E-Codes):**
    *   Fehler (z.B. `E001`, `E066`) würden auf eine Verletzung der OCFL-Kernspezifikation hinweisen (MUST-Anforderungen).
    *   Ein Objekt mit Fehlern gilt als **invalide** und muss repariert werden (z.B. bei Prüfsummenfehlern).

3.  **Fazit:**
    *   Die Meldung **"no errors found"** am Ende bestätigt, dass das Objekt trotz der Warnungen technisch korrekt und integer ist. Alle Dateien sind vorhanden und die Prüfsummen stimmen mit dem Inventar überein.

---

[Zurück zum Aktualisieren eines Objektes](06_update_object.md) | [Zurück zum Inhaltsverzeichnis](toc.md) | [Weiter zur Anzeige (display)](08_display_object.md)
