---
---
[English](../00_installation_en)

# Installation von gocfl

Bevor wir mit dem Workshop beginnen, muss `gocfl` auf Ihrem System installiert werden. Da `gocfl` in Go geschrieben ist, kann es direkt über den Go Installer installiert werden.

## 1. Voraussetzungen

- Eine installierte [Go-Umgebung](https://go.dev/doc/install) (Version 1.26.2 oder neuer ist notwendig).
- Internetzugang zum Herunterladen des Repositorys.
- Hilfreiche Hilfsprogramme 
  - [Image Magick](https://imagemagick.org/download)
  - [Ghostscript](https://www.ghostscript.com/releases/gsdnld.html) ([MacOS](https://formulae.brew.sh/formula/ghostscript))
  - [ffmpeg](https://www.ffmpeg.org/download.html)
  - [Tika Server](https://tika.apache.org/download.html) (benötigt eine [Java Runtime](https://adoptium.net/de/temurin/releases))
- Image Magick, Ghostscript und ffmpeg sollten über die Pfad-Variable des Systems auffindbar sein, um automatische Konfiguration zu ermöglichen. 

## 2. Installationsbefehl

Führen Sie den folgenden Befehl in Ihrem Terminal aus, um `gocfl` zu installieren:

```bash
go install github.com/ocfl-archive/gocfl/v2/gocfl@v2.0.6-beta39
```

## 3. Verifizierung

Nach der Installation können Sie überprüfen, ob `gocfl` korrekt installiert wurde und im Pfad (`PATH`) verfügbar ist:

```bash
gocfl --version
```
Sollte der Befehl nicht gefunden werden, stellen Sie sicher, dass Ihr `GOBIN`-Verzeichnis (standardmäßig `$HOME/go/bin` oder `%USERPROFILE%\go\bin`) in Ihrer `PATH`-Umgebungsvariable enthalten ist.
hier sollte eine Zeile mit der gewünschten Version erscheinen. z.B. `gocfl version v2.0.6-beta39  (0001-01-01T00:00:00Z) go1.26.2`

## 4. Tika Server

Der Tika-Server kann mit folgendem Befehl gestartet werden:

```bash
java -jar /home/ocfl/tika-server-standard-3.3.0.jar
```

## 5. Autoconfig

Um die Konfiguration von `gocfl` zu initialisieren und installierte Tools (wie ImageMagick, Ghostscript, ffmpeg und Tika) automatisch zu erkennen, führen Sie folgenden Befehl aus:

```bash
gocfl initconfig
```

Dieser Befehl erstellt eine Standardkonfiguration in `$HOME/gocfl`, die für die weiteren Schritte im Workshop benötigt wird.

Optional können Pfade über Parameter angepasst werden:
- `--toml`: Pfad zur TOML-Konfigurationsdatei.
- `--extension-folder`: Verzeichnis für Extension-Templates.
- `--script-folder`: Verzeichnis für Extension-Scripts.

Beispiel mit angepassten Pfaden:
```bash
gocfl initconfig --toml ./myconfig.toml --extension-folder ./extensions --script-folder ./scripts
```

---

[Weiter zur Benutzung von gocfl](../01_gocfl_usage) | [Zurück zum Inhaltsverzeichnis](../toc)
