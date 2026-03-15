---
---
[English](08_display_object_en)

# Anzeige im Webbrowser (display)

OCFL-Objekte sind zwar dateisystembasiert und für Menschen lesbar, aber die Navigation durch die verschiedenen Versionen und das Betrachten von Metadaten direkt in der Verzeichnisstruktur kann unübersichtlich sein. `gocfl` bietet hierfür einen eingebauten Web-Viewer an.

## 1. Der `display`-Befehl

Mit dem Befehl `display` (Alias: `viewer`) startet `gocfl` einen lokalen Webserver, der eine grafische Oberfläche zur Erkundung der Storage Root und der darin enthaltenen Objekte bereitstellt.

### Grundlegende Syntax:
```bash
gocfl display [Pfad zur Storage Root oder zum Objekt]
```

### Beispiel aus dem Workshop:
Um die im Workshop erstellte Storage Root im Browser anzuzeigen, verwenden wir:

```bash
gocfl --config ./gocfl/config/gocfl.toml display ./gocfl/temp/test42/
```

**Erklärung:**
- `display`: Startet den integrierten Webserver.
- `./gocfl/temp/test42/`: Der Pfad zur Storage Root.
- Standardmäßig ist der Viewer unter `http://localhost:8080` erreichbar.

## 2. Funktionen des Viewers

Sobald der Server läuft und Sie die Adresse im Browser aufrufen, bietet der `gocfl`-Viewer folgende Möglichkeiten:

1.  **Objekt-Liste:** Übersicht über alle Objekte in der Storage Root (mit ID und Pfad).
2.  **Versions-Historie:** Auswahl zwischen den verschiedenen Versionen eines Objekts (z.B. `v1` und `v2`).
3.  **Datei-Browser:** Browsen durch die logische Verzeichnisstruktur (State) der gewählten Version.
4.  **Metadaten-Anzeige:** Direkte Anzeige von technischen Metadaten (falls durch Extensions wie `NNNN-indexer` generiert).
5.  **Vorschau:** Anzeige von Bildern oder Abspielen von Audio/Video-Dateien direkt im Browser (unterstützt durch die `NNNN-thumbnail` Extension).
6.  **Fixity-Informationen:** Anzeige der Prüfsummen für jede Datei.

## 3. Wichtige Parameter

Falls der Standard-Port `8080` bereits belegt ist oder der Zugriff von einem anderen Rechner erfolgen soll, können folgende Flags hilfreich sein:

- `--display-addr`: Ändert die Adresse oder den Port, auf dem der Server lauscht (z.B. `:9090`).
- `--display-external-addr`: Setzt die URL, die für Links innerhalb des Viewers verwendet wird (wichtig bei Betrieb hinter einem Reverse Proxy).

## 4. Beenden des Viewers

Da der Befehl den Terminal-Prozess blockiert, während der Server läuft, kann der Viewer jederzeit durch Drücken von `Strg+C` (bzw. `Ctrl+C`) beendet werden.

---

[Zurück zur Validierung](07_validate_object) | [Zurück zum Inhaltsverzeichnis](TOC)
