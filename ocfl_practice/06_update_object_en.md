---
---
[Deutsch](06_update_object.md)

# Updating an Object (update)

OCFL is designed for versioning from the ground up. If the content of an object changes (files are added, deleted, or modified), no existing file is overwritten. Instead, a new **version** (`v2`, `v3`, etc.) is created.

In `gocfl`, the `update` command is used for this purpose.

## 1. The `update` Command

The `update` command is similar to the `add` command but refers to an already existing object ID in the storage root.

### Basic Syntax:
```bash
gocfl update /path/to/new_data -i "existing-object-id"
```

### Workshop Example:
Suppose we want to add additional data (e.g., from `payload2/`) to our object `urn:nbn:de:gbv:42-test1`:

```bash
gocfl --log-level DEBUG --config ./gocfl/config/gocfl.toml update ./gocfl/temp/test42/ ./gocfl/testdata/payload2/ -i urn:nbn:de:gbv:42-test1
```

**Explanation of Parameters:**
- `update`: The command to create a new version.
- `./gocfl/temp/test42/`: The path to the **storage root**.
- `./gocfl/testdata/payload2/`: The source path of the *additional* or *changed* data.
- `-i urn:nbn:de:gbv:42-test1`: The **object ID** of the already existing object.

## 2. What Happens During an Update?

1. **Version Increment:** `gocfl` recognizes that the ID already exists and the current version is `v1`. It prepares the creation of `v2`.
2. **Deduplication (Content-Addressable Storage):**
   - `gocfl` calculates the checksums of the new files.
   - If a file was already present in a previous version (`v1`) with exactly the same checksum, it is only referenced (in the inventory) in `v2`, but **not physically stored again**. This saves massive amounts of storage space.
3. **New Version Directory:** A new folder `v2/` is created in the object directory.
4. **Inventory Update:**
   - The `inventory.json` in the object's main directory is updated. It now contains information about `v1` and `v2`.
   - A copy of the new `inventory.json` is stored in `v2/`.
5. **Extensions:** All configured extensions (metadata extraction, thumbnails, etc.) run again for the new content.

## 3. Structure After the Update

After the update to version 2 (`v2`), the strength of OCFL becomes apparent: the object now contains both states, with unchanged files being efficiently deduplicated.

Here is the actual directory tree of our workshop object after the update:

```text
1f7d281accf403621871ec793c3b1c40eb480165e17b80da924aba0280246d12/
│   0=ocfl_object_1.1
│   inventory.json             # Now contains v1 and v2
│   inventory.json.sha512
│
├───extensions/
│
├───v1/                        # Unchanged state of version 1
│   │   inventory.json
│   └───content/
│       ├───data/              # All files from payload1/
│       └───metadata/          # Metadata and thumbnails from v1
│
└───v2/                        # New state of version 2
    │   inventory.json         # Current inventory
    └───content/
        │   README.md
        │
        ├───data/              # Physically new files
        │   └───image/
        │           IMG_6914.png
        │
        └───metadata/          # New metadata logs for v2
            │   filesystem_v2.jsonl.gz
            │   indexer_v2.jsonl.gz
            │   thumbnail_v2.jsonl.gz
            │
            └───thumbnails/    # Newly generated thumbnails
                └───v2/
                        00001.png
```

### What is noticeable in the v2 structure?

1. **Deduplication in `v2/content/data/`**: Although in our example we used the entire `payload2/` folder (which contains many identical files to `payload1/`) for the update, physically only the actually changed file `IMG_6914.png` is in the `data` folder of `v2`. All other files are referenced in the inventory but not stored again.
2. **New Metadata Logs**: In `v2/content/metadata/`, new versions of the log files (`_v2.jsonl.gz`) have been created, describing the state of the object at the time of the second ingest.
3. **Central `inventory.json`**: The file in the object's root directory is now the "master inventory" that keeps track across all versions.

Thanks to this structure, it is possible at any time to access the exact state of `v1`, even if files in `v2` were deleted or modified.

## 4. Identifying Changes in the Inventory

The `inventory.json` is the central source to understand what has changed between versions. A comparison of the `state` blocks of `v1` and `v2` reveals the actions performed:

### A. Deletions
If a file path is present in `v1` but missing in the `state` of `v2`, the file is considered deleted. Physically, it remains in `v1/content/`, but it is no longer visible in the logical state of `v2`.

**Example in the Inventory:**
In `v1.state`, the file is still present:
```json
"versions": {
  "v1": {
    "state": {
      "b78834c6ee794...": [ "data/image/IMG_6914.tga" ]
    }
  }
}
```
In the `state` of `v2`, this entry for the path `data/image/IMG_6914.tga` is missing.

### B. Renames and Moves
In OCFL, renames are handled highly efficiently. If a file has a new path but the same digest (checksum.md) as used in a previous version, OCFL recognizes this as a rename or move.

**Example Rename:**
The file keeps its digest but changes the path in the `state`:
- `v1.state`: `"9c9d3b13...": [ "data/audio/Education of the Noobz 1.docx" ]`
- `v2.state`: `"9c9d3b13...": [ "data/audio/Education of the Noobz 1_renamed.docx" ]`

**Example Move:**
- `v1.state`: `"1082b560...": [ "data/image/IMG_6914.jpg" ]`
- `v2.state`: `"1082b560...": [ "data/empty folder/IMG_6914.jpg" ]`

In all cases, the manifest continues to reference the original file in `v1`:
```json
"manifest": {
  "1082b560...": [ "v1/content/data/image/IMG_6914.jpg" ],
  "9c9d3b13...": [ "v1/content/data/audio/Education=u0020of=u0020the=u0020Noobz=u00201.docx" ]
}
```

In `v2`, no new file is physically stored. The inventory simply references the data block already present in `v1` for the new path.

---

[Back to Object Structure](05_object_structure_en.md) | [Back to Table of Contents](toc_en.md) | [Next Topic: Validation](07_validate_object_en.md)
