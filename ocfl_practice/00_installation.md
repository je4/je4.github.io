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

## 3. Tika Server

Der Tika-Server kann mit folgendem Befehl gestartet werden:

```bash
java -jar /home/ocfl/tika-server-standard-3.3.0.jar
```

---

[Weiter zur Benutzung von gocfl](../01_gocfl_usage) | [Zurück zum Inhaltsverzeichnis](../toc)
