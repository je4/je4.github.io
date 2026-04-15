---
---
[Deutsch](../00_installation)

# Installation of gocfl

Before we begin the workshop, `gocfl` must be installed on your system. Since `gocfl` is written in Go, it can be installed directly via the Go package manager.

## 1. Prerequisites

- An installed [Go environment](https://go.dev/doc/install) (Version 1.26.2 or newer is required).
- Internet access to download the repository.
- Helpful helper programs:
  - [Image Magick](https://imagemagick.org/download)
  - [Ghostscript](https://www.ghostscript.com/releases/gsdnld.html) ([MacOS](https://formulae.brew.sh/formula/ghostscript))
  - [ffmpeg](https://www.ffmpeg.org/download.html)
  - [Tika Server](https://tika.apache.org/download.html) (requires a [Java Runtime](https://adoptium.net/en-GB/temurin/releases))
- Image Magick, Ghostscript, and ffmpeg should be discoverable via the system's path variable to enable automatic configuration.

## 2. Installation Command

Run the following command in your terminal to install `gocfl`:

```bash
go install github.com/ocfl-archive/gocfl/v2/gocfl@v2.0.6-beta39
```

## 3. Tika Server

The Tika server can be started with the following command:

```bash
java -jar /home/ocfl/tika-server-standard-3.3.0.jar
```

---

[Next to gocfl Usage](../01_gocfl_usage_en) | [Back to Table of Contents](../toc_en)
