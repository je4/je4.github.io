[Deutsch](03_extensions.md)

# OCFL Extensions

OCFL is designed so that the core standard remains lean and stable, while specific functionalities are represented through **extensions**. These make it possible to customize the behavior of the archive without jeopardizing basic compatibility.

## 1. The Concept of Extensions

Extensions are located in both the storage root and the individual OCFL objects in the respective `extensions/` directory.

### Why Extensions?
- **Layouts:** Determining how object IDs are translated into directory paths (e.g., hashing).
- **Metadata:** Embedding additional technical or descriptive metadata (e.g., METS/PREMIS).
- **Functionality:** Support for filesystem features or specific archiving workflows.

## 2. Registered vs. Local Extensions

- **Official Extensions:** These are registered in the [OCFL Community Extensions](https://ocfl.io/extensions/) list (e.g., `0004-hashed-n-tuple-storage-layout`).
- **Local/Internal Extensions:** Often start with `NNNN-` and are specific to a tool or an institution. `gocfl` uses these extensively for its additional functions.

## 3. Extensions in Practice

As we saw during the initialization of the storage root, `gocfl` automatically creates configuration directories. A detailed overview of the extensions used in this workshop can be found here:

- **[OCFL Storage Root Extensions (initial, manager, layout)](03a_storage_root_extensions_en.md)**
- **[Detailed List of All Used Extensions](03b_extension_list_en.md)**

## 4. Hooks in gocfl

The implementation of extensions in `gocfl` is based on various call points (hooks) where the logic of the extension intervenes in the OCFL workflow. If an interface has multiple hooks, an extension using it must implement all the hooks defined therein.

These differ depending on the context:

- **Object Extensions:** Define hooks for actions within an OCFL object. Details can be found in [`objectExtension.go`](https://github.com/ocfl-archive/gocfl/blob/ed2bafb10a8c2bdbb2fad3ad271aa7e6993b75ed/pkg/ocfl/object/objectExtension.go).
    - **Interface `ExtensionObjectContentPath`**:
        - `BuildObjectManifestPath`: Creates the path for the manifest within the object.
    - **Interface `ExtensionObjectExtractPath`**:
        - `BuildObjectExtractPath`: Defines paths for extracting content.
    - **Interface `ExtensionObjectStatePath`**:
        - `BuildObjectStatePath`: Controls path creation in the object state.
    - **Interface `ExtensionArea`**:
        - `GetAreaPath`: Provides the path for a specific storage area.
    - **Interface `ExtensionStream`**:
        - `StreamObject`: Allows reading the data stream.
    - **Interface `ExtensionContentChange`**:
        - `AddFileBefore`: Before adding a file.
        - `UpdateFileBefore`: Before updating a file.
        - `DeleteFileBefore`: Before deleting a file.
        - `AddFileAfter`: After adding a file.
        - `UpdateFileAfter`: After updating a file.
        - `DeleteFileAfter`: Actions after deleting a file.
    - **Interface `ExtensionObjectChange`**:
        - `UpdateObjectBefore`: Actions before manifest creation / object update.
        - `UpdateObjectAfter`: Actions after saving the object.
    - **Interface `ExtensionFixityDigest`**:
        - `GetFixityDigests`: Provides additional digest algorithms for fixity checks.
    - **Interface `ExtensionMetadata`**:
        - `GetMetadata`: Provides additional metadata for the object.
    - **Interface `ExtensionVersionDone`**:
        - `VersionDone`: Called when a new version is completely finished.
    - **Interface `ExtensionNewVersion`**:
        - `NeedNewVersion`: Checks if a new version is required.
        - `DoNewVersion`: Performs initializations for a new version.
- **Storage Root Extensions:** Define hooks for managing the entire storage root. Details can be found in [`storagerootExtension.go`](https://github.com/ocfl-archive/gocfl/blob/ed2bafb10a8c2bdbb2fad3ad271aa7e6993b75ed/pkg/ocfl/storageroot/storagerootExtension.go).
    - **Interface `ExtensionBuildStorageRootPath`**:
        - `BuildStorageRootPath`: Calculation of the physical path of an object based on its ID (Storage Layout).

## 5. Mapping in the Extension Manager

The `NNNN-gocfl-extension-manager` uses specific keys in its `config.json` to address the various extension types. Here is the mapping of the Go interfaces to the configuration entries:

| Interface | Key in `config.json` |
| :--- | :--- |
| `ExtensionBuildStorageRootPath` | `StorageRootPath` |
| `ExtensionObjectContentPath` | `ObjectContentPath` |
| `ExtensionObjectExtractPath` | `ObjectExtractPath` |
| `ExtensionObjectStatePath` | `ObjectStatePath` |
| `ExtensionArea` | `Area` |
| `ExtensionStream` | `Stream` |
| `ExtensionContentChange` | `ContentChange` |
| `ExtensionObjectChange` | `ObjectChange` |
| `ExtensionFixityDigest` | `FixityDigest` |
| `ExtensionMetadata` | `Metadata` |
| `ExtensionNewVersion` | `NewVersion` |
| `ExtensionVersionDone` | `VersionDone` |

## 6. Self-Describing via Documentation

An important aspect of `gocfl` is that for every enabled extension, the corresponding documentation should be copied directly into the storage root as a Markdown file (e.g., `NNNN-gocfl-extension-manager.md`).

This guarantees that even in decades, it will still be possible to understand which rules applied to the storage and processing of the data, even if the original software or website no longer exists.

---

[Back to Table of Contents](toc_en.md) | [Next Topic: Creating Objects](04_add_object_en.md)
