---
---
[Deutsch](../01_gocfl_usage)

# Usage and Configuration of gocfl

In this part of the workshop, we will familiarize ourselves with the basic operation and configuration options of the `gocfl` tool. We assume that the [installation](../00_installation_en) has been successfully completed.

## 1. Version Check

To ensure you are working with the expected version, use:

```bash
gocfl --version
```

If the command is not found, ensure that your `GOBIN` directory (typically `$HOME/go/bin` or `%USERPROFILE%\go\bin`) is included in your `PATH` environment variable.
A line with the desired version should appear here. e.g., `gocfl version v2.0.6-beta39  (0001-01-01T00:00:00Z) go2.26.2`

## 2. Autoconfig

`gocfl` features a mechanism for automatic detection of installed tools (such as ImageMagick, Ghostscript, ffmpeg, and Tika).

### The global flag `--autoconfig`
The global flag `--autoconfig` can be used with **any** `gocfl` command. It instructs the program to search for installed software on the system and automatically adjust the internal configuration for the current call. In particular, the submodules **[Indexer](https://github.com/je4/indexer)** (NNNN-indexer) and **Thumbnail Generation** (NNNN-thumbnail) are preconfigured according to the tools found.

```bash
gocfl --autoconfig [command] ...
```

### The `initconfig` command
To avoid having to specify `--autoconfig` with every call, the automatically detected (and optionally customized) configuration can be permanently saved:

```bash
gocfl --autoconfig initconfig
```

This command creates a standard configuration in `$HOME/gocfl`, which is required for the following steps in the workshop. The results of the automatic tool detection are written into the configuration file.

Paths and the scope of the configuration can optionally be customized using parameters:
- `--toml`: Path to the TOML configuration file.
- `--extension-folder`: Directory for extension templates.
- `--script-folder`: Directory for extension scripts.
- `--fullconfig`: Writes the complete configuration including all default values to the file. Without this parameter, only the detected adjustments and deviations are saved.

Example for a complete configuration:
```bash
gocfl --autoconfig initconfig --fullconfig
```

Example with customized paths:
```bash
gocfl --autoconfig initconfig --toml ./myconfig.toml --extension-folder ./extensions --script-folder ./scripts
```

## 3. Basic Command Structure

`gocfl` follows a typical command-line pattern with subcommands. The general syntax is:

```bash
gocfl [global flags] <command> [command flags] <arguments>
```

Important global flags include:
- `-v`, `--verbose`: Increases the detail of the output.
- `-l`, `--log`: Enables writing logs to a file.

## 4. Help System

The tool offers a comprehensive built-in help. This is the best way to explore the available options:

### General Help
Shows all available main commands (e.g., `init`, `add`, `update`, `extract`, `validate`):
```bash
gocfl --help
```

### Command-Specific Help
Each subcommand has its own help page explaining specific flags and arguments:
```bash
gocfl init --help
gocfl add --help
```

## 5. Configuration

`gocfl` offers flexible configuration options to support both simple ad-hoc calls and complex workflows.

### Configuration File
For recurring settings, a configuration file (in TOML format) can be used. By default, `gocfl` searches for its configuration in the directory `$HOME/gocfl`.

The path `$HOME` corresponds to:
- **Windows:** `%USERPROFILE%` (usually `C:\Users\Username`)
- **macOS:** `/Users/Username`
- **Linux:** `/home/Username`

A specific file can be specified with the global `--config` flag:

```bash
gocfl --config myconfig.toml [command]
```

The configuration file is used not only for storing simple default values (like S3 connections or log levels) but is especially essential for **complex submodule settings**. Many of these deep configurations are intentionally not available as command-line parameters to keep the command line clear.

Examples of such submodules are:
- **[Indexer](https://github.com/je4/indexer):** Extraction and indexing of metadata.
- **Format Migration:** Modules for automated conversion of file formats within the object.
- **Thumbnail Generation:** Creation of preview images for archived content.

For a detailed overview of the individual sections and possibilities of the configuration file, see the page **[Detailed Configuration (gocfl.toml)](../01a_gocfl_config_en)**.

If no path is specified, `gocfl` uses internal default values (embedded config).

### Precedence of Settings
If settings are defined in multiple places, the following priority applies:
1. **Command-line parameters (Flags):** Always take precedence.
2. **Environment variables:** Override values from the configuration file (e.g., `GOCFL_S3_ENDPOINT`).
3. **Configuration file:** Defines project- or user-specific standards.
4. **Default values:** Hard-coded fallbacks in the program code (e.g., SHA-512 as the standard hash).


---

[Back to Installation](../00_installation_en) | [Back to Table of Contents](../toc_en) | [Next Topic: Initializing a Storage Root](../02_storage_root_en)
