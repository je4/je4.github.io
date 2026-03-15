---
---
[Deutsch](../00_installation)

# Installation of gocfl

Before we begin the workshop, `gocfl` must be installed on your system. Since `gocfl` is written in Go, it can be installed directly via the Go package manager.

## 1. Prerequisites

- An installed Go environment (Version 1.21 or newer is recommended).
- Internet access to download the repository.

## 2. Installation Command

Run the following command in your terminal to install the specific version of `gocfl` for this workshop:

```bash
go install github.com/ocfl-archive/gocfl/v2/gocfl@v2.0.6-beta31
```

## 3. Verification

After installation, you can verify that `gocfl` was correctly installed and is available in your path (`PATH`):

```bash
gocfl --version
```

If the command is not found, ensure that your `GOBIN` directory (typically `$HOME/go/bin` or `%USERPROFILE%\go\bin`) is included in your `PATH` environment variable.

## 4. Tika Server

In this workshop, Tika is used for metadata extraction. The Tika server can be started with the following command:

```bash
java -jar /home/ocfl/tika-server-standard-3.2.3.jar
```

---

[Next to gocfl Usage](../01_gocfl_usage_en) | [Back to Table of Contents](../toc_en)
