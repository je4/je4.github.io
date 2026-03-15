---
---
[Deutsch](02_storage_root)

# Initializing an OCFL Storage Root

After familiarizing ourselves with the basics and configuration of `gocfl`, the first practical step in an OCFL-based archive is the creation of a **Storage Root**.

## 1. What is a Storage Root?

The [Storage Root](https://ocfl.io/1.1/spec/#storage-root) is the top level of your archive. It is the container in which all OCFL objects are stored. A storage root contains:
- The file `ocfl_1.1.txt` (or another version) marking the directory path as OCFL storage.
- An `ocfl_layout.json` defining how objects are structured and named within the root.
- An `extensions/` folder for specific functional extensions.

## 2. Structure and Requirements (Mandatory vs. Optional)

According to the [OCFL Specification on Root Structure](https://ocfl.io/1.1/spec/#root-structure), clear rules apply to a storage root to ensure long-term interpretability:

### Mandatory Components
1.  **OCFL Version Marker**: A file named `ocfl_v1.1.txt` (or similar, `gocfl` uses `0=ocfl_1.1`) identifying the path as an OCFL Storage Root.
2.  **OCFL Objects**: The actual data organized in subdirectories.

### Optional Components
-   **`ocfl_layout.json`**: Recommended to define the mapping of object IDs to directory paths.
-   **`extensions/`**: A directory for extension configurations (see [OCFL Storage Root Extensions](03a_storage_root_extensions_en)).

### Important Structure Rules
-   **No Foreign Folders**: In the storage root, the **only** permitted directories are the `extensions` folder (see [OCFL Storage Root Extensions](03a_storage_root_extensions_en)) and the directory structures for **OCFL objects**. Other folders are not allowed.
-   **Additional Files**: Other files (such as the `.md` files created by `gocfl`) may be present as long as they do not collide with reserved names or object directories. They serve as **self-documentation**.

## 3. The Command `gocfl init`

With `gocfl`, you initialize a new storage root via the `init` subcommand.

### Syntax
```bash
gocfl init [path to storage root] [flags]
```

### Important Flags for `init`
- `--ocfl-version`: Sets the specification version (default: `1.1`).
- `-d`, `--digest`: Defines the default hash algorithm (e.g., `sha512` or `sha256`).
- `--default-storageroot-extensions`: Path to a folder with configurations for extensions to be loaded initially. This **extension template** is crucial for the automatic configuration of the storage root (see below).

## 3. Practical Example

In this example, we initialize a storage root using a specific configuration file and a target directory:

```bash
gocfl --config ./gocfl/config/gocfl.toml init ./gocfl/temp/test42/
```

### What Happens?
- `--config ./gocfl/config/gocfl.toml`: `gocfl` loads settings from the specified file. Especially important here is the entry under `[Init]` pointing to the **extension template**:
  ```toml
  [Init]
  StorageRootExtensions = "/home/ocfl/gocfl/config/extensions/storageroot"
  ```
- `init ./gocfl/temp/test42/`: Creates the necessary OCFL structure files in the directory `./gocfl/temp/test42/` and copies the extension configurations and documentation defined in the template into the storage root.

If you look into the directory after the command, you will see a structure that goes beyond the minimal OCFL specification, as `gocfl` creates useful additional information and configurations:

```text
test42/
├── 0=ocfl_1.1          # Storage root marker (OCFL Version 1.1)
├── ocfl_layout.json    # Definition of the path layout
├── ocfl_spec_1.1.md    # The OCFL specification as a reference
├── extensions/         # Configurations for enabled extensions
│   ├── 0004-hashed-n-tuple-storage-layout/
│   ├── initial/
│   └── NNNN-gocfl-extension-manager/
└── NNNN-*.md           # Documentation of the used extensions
```

### Key Files in Detail

1.  **`0=ocfl_1.1`**: In the specification, this file is called `ocfl_1.1.txt`. `gocfl` uses this name by default to declare the version of the storage root. It is empty and serves only as a "name tag".
2.  **`ocfl_layout.json`**: This file is crucial for scalability. In our example, [0004-hashed-n-tuple-storage-layout](https://ocfl.github.io/extensions/0004-hashed-n-tuple-storage-layout.html) is used. This means that object IDs (like `ark:/12345/bcd987`) are hashed and distributed into subdirectories (e.g., `a1b/2c3/d4e/...`) to prevent too many folders in a single directory.
3.  **`ocfl_spec_1.1.md`**: This file contains the complete OCFL specification directly in the storage root. Thus, the root is not only **self-contained** (all data is present) but also **self-describing**, as the rules for access and interpretation are provided directly.
4.  **`extensions/`**: Here are the configuration files (`config.json`) for the extensions (see [OCFL Storage Root Extensions](03a_storage_root_extensions_en)).
    - `0004-...` configures the layout mentioned above.
    - `initial` determines which extension is loaded first (in our case, the `NNNN-gocfl-extension-manager`).
    - `NNNN-gocfl-extension-manager` is a `gocfl`-specific extension responsible solely for initializing the storage root.
5.  **The `.md` files (e.g., `ocfl_spec_1.1.md` and `NNNN-*.md`)**: During initialization, `gocfl` copies both the OCFL specification and descriptions of the extensions used (such as `NNNN-indexer.md` or `NNNN-migration.md`) directly into the storage root. This happens during initialization by the Extension Manager. This reinforces the **self-describing** approach: anyone accessing this medium in the future will find both the general specification and documentation of the specifically used functions right on-site, without being dependent on external websites.

---

[Back to Table of Contents](toc_en) | [Next Topic: OCFL Extensions](03_extensions_en)
