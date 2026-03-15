---
---
[Deutsch](../03c_object_extensions)

# OCFL Object Extensions

In this chapter, we look at the extensions defined directly at the OCFL object level in the `extensions/` directory. These control how content is stored within the object and which additional metadata or functions (such as indexing or thumbnails) are active for this specific object.

## 1. Overview of the Extensions Used

The example object `test42` uses the following extensions:

1. **0009-digest-algorithms**: Registers additional hash algorithms.
2. **0011-direct-clean-path-layout**: Cleans file paths for storage.
3. **initial**: Points to the initial extension manager.
4. **NNNN-content-subpath**: Subdivides the object into different areas.
5. **NNNN-filesystem**: Stores file system attributes (e.g., timestamps).
6. **NNNN-gocfl-extension-manager**: Manages the order and execution of extensions.
7. **NNNN-indexer**: Creates technical metadata via Siegfried, Tika, ffprobe etc.
8. **NNNN-metafile**: Generates a central `info.json` with object metadata.
9. **NNNN-thumbnail**: Generates preview images for visual media.

## 2. Configuration Details

Here are the specific configurations of the most important extensions:

### NNNN-content-subpath
This extension defines in which subdirectories the different data types are stored within the object.

*   **subPath**: Maps "Areas" to directory paths.
    *   **content**: The area for the actual payload data (`data/`).
    *   **metadata**: The area for additional technical or semantic metadata (`metadata/`).
    *   **documentation**: Area for documentation files (`documentation/`).

```json
{
   "extensionName": "NNNN-content-subpath",
   "subPath": {
      "content": {
         "path": "data",
         "description": "Payload of archival object"
      },
      "documentation": {
         "path": "documentation",
         "description": "documentation of the archival object"
      },
      "metadata": {
         "path": "metadata",
         "description": "additional semantic metadata"
      }
   }
}
```

### 0011-direct-clean-path-layout
Responsible for mapping logical file paths to physical paths in storage. It cleans up special characters and ensures compatibility with various file systems.

*   **maxPathnameLen / maxPathSegmentLen**: Limits the total path length and the length of individual segments (folder/filenames) to avoid filesystem limits (e.g., on Windows).
*   **replacementString**: Character that replaces problematic special characters (such as `:`, `*`, `?` etc.) in filenames (here: `_`).
*   **whitespaceReplacementString**: Character that replaces spaces (here it remains a space `" "`).
*   **utfEncode**: Ensures that paths are correctly encoded in UTF-8.
*   **fallbackDigestAlgorithm**: The hash algorithm used for naming in the fallback mechanism (here: `sha512`).
*   **fallbackFolder**: Name of the directory where files end up whose paths still are too long.
*   **numberOfFallbackTuples / fallbackTupleSize**: Control the partitioning (sharding) of the fallback directory via tuples (similar to the Storage Layout). Here disabled (`0`).

```json
{
   "extensionName": "0011-direct-clean-path-layout",
   "maxPathnameLen": 32000,
   "maxPathSegmentLen": 127,
   "replacementString": "_",
   "whitespaceReplacementString": " ",
   "utfEncode": true,
   "fallbackDigestAlgorithm": "sha512",
   "fallbackFolder": "fallback",
   "numberOfFallbackTuples": 0,
   "fallbackTupleSize": 0
}
```

### NNNN-indexer
This extension runs various tools during the ingest process to extract technical metadata.

*   **StorageType / StorageName**: Specifies that the results are stored in the `metadata` area (see `content-subpath`).
*   **Actions**: List of tools applied to each file (e.g., Siegfried for format identification, FFProbe for video metadata).
*   **Compress**: The extracted metadata is stored as `gzip` to save space.

```json
{
   "extensionName": "NNNN-indexer",
   "StorageType": "area",
   "StorageName": "metadata",
   "Actions": [
      "siegfried",
      "xml",
      "tika",
      "ffprobe",
      "identify"
   ],
   "Compress": "gzip"
}
```

### NNNN-metafile
Creates an `info.json` in the `metadata` area, which summarizes the basic OCFL information.

*   **name**: The filename of the generated metadata file (`info.json`).
*   **schema**: Refers to the JSON schema definition against which the file can be validated.

```json
{
   "extensionName": "NNNN-metafile",
   "storageType": "area",
   "storageName": "metadata",
   "name": "info.json",
   "schema": "gocfl-info-1.0.json",
   "schemaUrl": "https://raw.githubusercontent.com/ocfl-archive/gocfl/main/gocfl-info-1.0.json"
}
```

## 3. Extension Manager (NNNN-gocfl-extension-manager)

The manager controls the execution order (hooks). This is important because, for example, the layout (`0011`) must be applied before the path is further subdivided by `content-subpath`.

*   **sort**: Defines the execution order of extensions for specific functional areas or events (Hooks). Sorting takes place **within** each Hook separately:
    *   **ObjectChange**: Area for object changes. Within this Hook, indexing happens first, then the metadata file is created.
    *   **ObjectContentPath**: Area for path generation. Within this Hook, the path is first cleaned (`0011`), then sorted into areas (`content-subpath`).
*   **exclusion**: Prevents mutually exclusive extensions from being active at the same time. Again, the affected area is defined first.
    *   **ObjectContentPath**: In this area, `direct-clean-path-layout` and `direct-path-layout` may not be used simultaneously.

```json
{
   "extensionName": "NNNN-gocfl-extension-manager",
   "sort": {
      "ObjectChange": [
         "NNNN-indexer",
         "NNNN-metafile"
      ],
      "ObjectContentPath": [
         "NNNN-direct-clean-path-layout",
         "NNNN-content-subpath"
      ]
   },
   "exclusion": {
      "ObjectContentPath": [
         [
            "NNNN-direct-clean-path-layout",
            "NNNN-direct-path-layout"
         ]
      ]
   }
}
```

---

[Back to Overview: Extensions](../03_extensions_en) | [Back to Table of Contents](../toc_en)
