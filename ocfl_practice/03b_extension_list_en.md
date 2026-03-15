---
---
[Deutsch](03b_extension_list.md)

# List of Used OCFL Extensions

In our workshop scenario, various official and tool-specific (`NNNN-`) extensions are used. These can be categorized by their location of use and function.

## 1. Storage Root Extensions
These extensions control the behavior of the entire storage root (the storage layout of the objects).

- **`initial`**: Determines which extension is loaded first at startup (usually points to the `extension-manager`).
- **`NNNN-gocfl-extension-manager`**: A meta-extension of `gocfl` that coordinates other extensions and manages documentation files, for example.
- **`0004-hashed-n-tuple-storage-layout`**: Defines the mapping of object IDs to directory structures using hashing (standard in this workshop).

## 2. Object Extensions
These extensions are active within the individual OCFL objects. We divide them here by their primary purpose:

### A. Path and Layout Extensions
These extensions determine how files are physically stored and named within the object.

- **`0011-direct-clean-path-layout`**: Cleans up filenames for maximum system compatibility (e.g., replacing spaces with `=u0020`). For details, see **[Object Structure](05_object_structure_en.md#filename-cleanup-0011-direct-clean-path-layout)**.
- **`NNNN-content-subpath`**: Splits the `content` directory into logical areas such as `data/` (user data) and `metadata/` (generated data). For details, see **[Object Structure](05_object_structure_en.md#content-structure-data--metadata-and-nnnn-content-subpath)**.

### B. Metadata Extensions
These extensions automatically enrich the object with technical or administrative metadata during the ingest process.

- **`NNNN-indexer`**: Extracts technical metadata (format, duration, resolution, etc.) using tools like Siegfried and FFMPEG and stores it in `indexer_v1.jsonl.gz`.
- **`NNNN-filesystem`**: Backs up metadata of the original filesystem (timestamps, permissions) in `filesystem_v1.jsonl.gz`.
- **`NNNN-metafile`**: Allows including custom metadata (like our `info.json`) when creating the object.
- **`0001-digest-algorithms`**: Enables the calculation of additional checksums (fixity.md) for long-term integrity.

### C. Extensions for Better Readability and Preview
These extensions serve to make the content of the object more quickly graspable for human users.

- **`NNNN-thumbnail`**: Automatically generates small preview images for images, videos, and PDFs, which are stored in the `metadata/thumbnails/` folder.

---

[Back to Object Structure](05_object_structure_en.md) | [Back to Table of Contents](toc_en.md)
