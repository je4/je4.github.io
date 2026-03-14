[Deutsch](01a_gocfl_config.md)

# Overview of the gocfl.toml Configuration File

The configuration file [gocfl.toml](gocfl.toml) is the central element for controlling complex processes in `gocfl`. It allows setting default values for commands and configuring the extensive functions of the submodules.

Here is an overview of the main sections of the file:

## 1. Infrastructure and Global Settings

- **[S3]:** Configuration for working with S3-compatible object storage (endpoints, access keys, regions). These can also be controlled via environment variables like `%%GOCFL_S3_ENDPOINT%%`.
- **[log]:** Controls the level of output detail (`trace`, `debug`, `info`, etc.).
- **[log.stash]:** Enables sending logs to a Logstash server (ELK stack).
- **[AES]:** Integration of KeePass (KDBX files) for secure management of keys and passwords.

## 2. Command Defaults

These sections define fallback values if they are not explicitly specified on the command line:

- **[Init]:** Standard OCFL version (e.g., 1.1), path to the **Storage-Root-Extension-Template** (e.g., `/home/ocfl/gocfl/config/extensions/storageroot`), and the primary digest algorithm (e.g., SHA-512).
- **[Add]:** Standard commit message, digest types, fixity algorithms (e.g., MD5, SHA-1 for additional integrity checks), and user information (`[Add.User]`).

## 3. Submodules (The "Intelligence" of gocfl)

A large part of `gocfl.toml` is dedicated to the configuration of submodules that become active during the creation or update of objects:

### Indexer (`[Indexer]`)
This module extracts technical metadata. It uses various tools:
- **Siegfried:** Format recognition using PRONOM IDs.
- **FFMPEG:** Extraction of metadata from audio and video files (incl. mime-type mapping).
- **ImageMagick (Identify/Convert):** Analysis of image files.
- **Tika:** Extraction of metadata and full text from documents (e.g., PDF, Office).
- **XML:** Validation and typing of XML files based on namespaces or schemas.

### Thumbnail Generation (`[Thumbnail]`)
Defines rules for automatically generating preview images for various file types (images, video, audio spectrograms, PDF). Here, external commands (like `convert` or `ffmpeg`) are configured with specific parameters.

### Migration (`[Migration]`)
This is a core feature for long-term archiving. It enables the automatic creation of derivatives in sustainable formats during the ingest process:
- **RAW to PNG:** Conversion of camera raw data (e.g., Canon CR2).
- **PDF to PDF/A:** Conversion of various PDF versions to the PDF/A archive format using Ghostscript (`gs`).
- **Strategies:** Determining how the new files are named and stored in the OCFL object.

## 4. Extensions (`[Extension]`)
Configuration for specific OCFL extensions, such as the integration of METS files for descriptive metadata.

---

[Back to gocfl Usage](01_gocfl_usage_en.md) | [Back to Table of Contents](toc_en.md) | [Next Topic: Initializing a Storage Root](02_storage_root_en.md)
