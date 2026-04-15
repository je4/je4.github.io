---
---
[Deutsch](../00_installation)

# Installation of gocfl

Before we begin the workshop, `gocfl` must be installed on your system. Since `gocfl` is written in Go, it can be installed directly via the Go package manager.

## 1. Prerequisites

- An installed [Go environment](https://go.dev/doc/install) (Version 2.26.2 or newer is required).
- Internet access to download the repository.
- Helpful helper programs:
  - [Image Magick](https://imagemagick.org/download)
  - [Ghostscript](https://www.ghostscript.com/releases/gsdnld.html) ([MacOS](https://formulae.brew.sh/formula/ghostscript))
  - [ffmpeg](https://www.ffmpeg.org/download.html)
  - [Tika Server](https://tika.apache.org/download.html) (requires a [Java Runtime](https://adoptium.net/en-GB/temurin/releases))
- Image Magick, Ghostscript, and ffmpeg should be discoverable via the system's path variable to enable automatic configuration.

## 2. Installation of Helper Programs

The following programs are required for advanced features (metadata extraction, thumbnail generation, PDF migration).

### Go
Required for installing and running `gocfl`.
- **Windows:** `winget install GoLang.Go` (or [Download](https://go.dev/dl/))
- **macOS:** `brew install go` (or [Download](https://go.dev/dl/))
- **Linux (Ubuntu/Debian):** Since `apt` often provides outdated versions, installation via Snap or PPA is recommended:
  - **Snap (latest):** `sudo snap install go --classic`
  - **PPA (latest):** `sudo add-apt-repository ppa:longsleep/golang-backports && sudo apt update && sudo apt install golang-go`
  - **Manual:** [Official installation guide](https://go.dev/doc/install)

### ImageMagick
Used for image analysis and conversion.
- **Windows:** `winget install ImageMagick.ImageMagick` or [Download](https://imagemagick.org/script/download.php#windows)
- **macOS:** `brew install imagemagick`
- **Linux (Ubuntu/Debian):** `sudo apt install imagemagick`

### FFmpeg
Used for extracting audio and video metadata and for creating audio spectrograms.
- **Windows:** `winget install ffmpeg` or [Download](https://ffmpeg.org/download.html#build-windows)
- **macOS:** `brew install ffmpeg`
- **Linux (Ubuntu/Debian):** `sudo apt install ffmpeg`

### Ghostscript
Required for working with PDF files (e.g., migration to PDF/A).
- **Windows:** `winget install ArtifexSoftware.Ghostscript` or [Download](https://ghostscript.com/releases/gsdnld.html)
- **macOS:** `brew install ghostscript`
- **Linux (Ubuntu/Debian):** `sudo apt install ghostscript`

### Java (JRE/JDK)
Required to run the **Tika Server**. A version of Java 11 or newer is recommended (e.g., Eclipse Temurin).
- **Windows:** `winget install EclipseAdoptium.Temurin.17.JDK`
- **macOS:** `brew install --cask temurin`
- **Linux (Ubuntu/Debian):** `sudo apt install default-jre`

## 3. Installation Command for gocfl

Run the following command in your terminal to install `gocfl`:

```bash
go install github.com/ocfl-archive/gocfl/v2/gocfl@v2.0.6-beta39
```

## 4. Tika Server

The Tika server can be started with the following command:

```bash
java -jar /home/ocfl/tika-server-standard-3.3.0.jar
```

---

[Next to gocfl Usage](../01_gocfl_usage_en) | [Back to Table of Contents](../toc_en)
