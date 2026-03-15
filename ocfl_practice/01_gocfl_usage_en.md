---
---
[Deutsch](../01_gocfl_usage)

# Usage and Configuration of gocfl

In this part of the workshop, we will familiarize ourselves with the basic operation and configuration options of the `gocfl` tool. We assume that the [installation](../00_installation_en) has been successfully completed.

## 1. Basic Command Structure

`gocfl` follows a typical command-line pattern with subcommands. The general syntax is:

```bash
gocfl [global flags] <command> [command flags] <arguments>
```

Important global flags include:
- `-v`, `--verbose`: Increases the detail of the output.
- `-l`, `--log`: Enables writing logs to a file.

## 2. Help System

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

## 3. Configuration

`gocfl` offers flexible configuration options to support both simple ad-hoc calls and complex workflows.

### Configuration File
For recurring settings, a configuration file (in TOML format) can be used. A specific file can be specified with the global `--config` flag:

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

## 4. Version Check

To ensure you are working with the expected version, use:
```bash
gocfl --version
```

---

[Back to Installation](../00_installation_en) | [Back to Table of Contents](../toc_en) | [Next Topic: Initializing a Storage Root](../02_storage_root_en)
