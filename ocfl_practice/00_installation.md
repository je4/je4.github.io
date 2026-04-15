---
---
[English](../00_installation_en)

# Installation von gocfl

Bevor wir mit dem Workshop beginnen, muss `gocfl` auf Ihrem System installiert werden. Da `gocfl` in Go geschrieben ist, kann es direkt über den Go Installer installiert werden.

## 1. Voraussetzungen

- Eine installierte [Go-Umgebung](https://go.dev/doc/install) (Version 2.26.2 oder neuer ist notwendig).
- Internetzugang zum Herunterladen des Repositorys.
- Hilfreiche Hilfsprogramme 
  - [Image Magick](https://imagemagick.org/download)
  - [Ghostscript](https://www.ghostscript.com/releases/gsdnld.html) ([MacOS](https://formulae.brew.sh/formula/ghostscript))
  - [ffmpeg](https://www.ffmpeg.org/download.html)
  - [Tika Server](https://tika.apache.org/download.html) (benötigt eine [Java Runtime](https://adoptium.net/de/temurin/releases))
- Image Magick, Ghostscript und ffmpeg sollten über die Pfad-Variable des Systems auffindbar sein, um automatische Konfiguration zu ermöglichen. 

## 2. Installation der Hilfsprogramme

Die folgenden Programme werden für erweiterte Funktionen (Metadaten-Extraktion, Thumbnail-Erstellung, PDF-Migration) benötigt.

### Go
Wird für die Installation und Ausführung von `gocfl` benötigt.
- **Windows:** `winget install GoLang.Go` (oder [Download](https://go.dev/dl/))
- **macOS:** `brew install go` (oder [Download](https://go.dev/dl/))
- **Linux (Ubuntu/Debian):** Da `apt` oft veraltete Versionen bereitstellt, wird die Installation via Snap oder PPA empfohlen:
  - **Snap (aktuell):** `sudo snap install go --classic`
  - **PPA (aktuell):** `sudo add-apt-repository ppa:longsleep/golang-backports && sudo apt update && sudo apt install golang-go`
  - **Manuell:** [Offizielles Installations-Script](https://go.dev/doc/install)

### ImageMagick
Wird zur Bildanalyse und Konvertierung verwendet.
- **Windows:** `winget install ImageMagick.ImageMagick` oder [Download](https://imagemagick.org/script/download.php#windows)
- **macOS:** `brew install imagemagick`
- **Linux (Ubuntu/Debian):** `sudo apt install imagemagick`

### FFmpeg
Wird zur Extraktion von Audio- und Videometadaten sowie zur Erstellung von Audio-Spektrogrammen verwendet.
- **Windows:** `winget install ffmpeg` oder [Download](https://ffmpeg.org/download.html#build-windows)
- **macOS:** `brew install ffmpeg`
- **Linux (Ubuntu/Debian):** `sudo apt install ffmpeg`

### Ghostscript
Wird für die Arbeit mit PDF-Dateien (z.B. Migration nach PDF/A) benötigt.
- **Windows:** `winget install ArtifexSoftware.Ghostscript` oder [Download](https://ghostscript.com/releases/gsdnld.html)
- **macOS:** `brew install ghostscript`
- **Linux (Ubuntu/Debian):** `sudo apt install ghostscript`

### Java (JRE/JDK)
Wird für den Betrieb des **Tika Servers** benötigt. Empfohlen wird eine Version ab Java 11 (z.B. Eclipse Temurin).
- **Windows:** `winget install EclipseAdoptium.Temurin.17.JDK`
- **macOS:** `brew install --cask temurin`
- **Linux (Ubuntu/Debian):** `sudo apt install default-jre`

## 3. Installationsbefehl für gocfl

Führen Sie den folgenden Befehl in Ihrem Terminal aus, um `gocfl` zu installieren:

```bash
go install github.com/ocfl-archive/gocfl/v2/gocfl@v2.0.6-beta39
```

## 4. Tika Server

Der Tika-Server kann mit folgendem Befehl gestartet werden:

```bash
java -jar /home/ocfl/tika-server-standard-3.3.0.jar
```

---

[Weiter zur Benutzung von gocfl](../01_gocfl_usage) | [Zurück zum Inhaltsverzeichnis](../toc)
