---
---
[Deutsch](../09_extract_object)

# Extracting Content (extract)

Although OCFL objects are readable directly in the filesystem, it may be necessary to extract the content of an object (or a specific version) to a separate directory, for example, for delivery to another system or for further processing without the OCFL structure.

## 1. The `extract` Command

With the `extract` command, `gocfl` copies the files of an object from the storage root to a target location. In doing so, the logical structure of the object (the "state") is restored.

### Basic Syntax:
```bash
gocfl extract [options] [path to storage root or object] [target directory]
```

### Workshop Example:
To extract the current version of our test object into a temporary directory, we use:

```bash
gocfl --log-level DEBUG --config ./gocfl/config/gocfl.toml extract ./gocfl/temp/test42 ./gocfl/temp/extract_v2 --with-manifest -i urn:nbn:de:gbv:42-test1
```

**Explanation:**
- `--log-level DEBUG`: Displays detailed information during the process.
- `extract`: The command for extraction.
- `./gocfl/temp/test42`: The path to the storage root.
- `./gocfl/temp/extract_v2`: The target directory for the extracted files.
- `--with-manifest`: Additionally creates a sha512sum compatible manifest for the target directory.
- `-i urn:nbn:de:gbv:42-test1`: The ID of the object to be extracted.

## 2. Extracting Specific Versions

By default, `gocfl` always extracts the **latest version** (HEAD). If you want to extract an older version, you can control this via the `-v` (or `--version`) parameter:

```bash
gocfl --config ./gocfl/config/gocfl.toml extract --version v1 ./gocfl/temp/test42/ ./gocfl/temp/extract_v1/ --object-id urn:nbn:de:gbv:42-test1 --with-manifest
```

## 3. Important Options

- `--version`: Determines the version to be extracted (e.g., `v1`).
- `-i`, `--object-id`: Allows specifying the object ID if the path to the storage root (instead of the object) was provided.
- `-p`, `--object-path`: Path of the object to be extracted.
- `--area`: Specifies the data area to extract (default: `content`).
    - **Note:** By default, only the `content` area is extracted. If all areas should be exported, specify `full` as the area, or a specific area such as `metadata`.
- `--with-manifest`: Creates a sha512sum compatible manifest for the target directory.
- `--ext-NNNN-content-subpath-area`: Subpath for extraction (default: `content`). `all` for complete extraction.
- `--ext-NNNN-metafile-target`: URL with the metadata target folder.

## 4. What is Extracted?

During extraction, the **logical state** of the selected version is restored. This means:
- Files that were deleted in the version do not appear in the target directory.
- Files that were renamed appear under their new name.
- Filenames cleaned up by extensions like `0011-direct-clean-path-layout` in the OCFL object are returned to their original form (with spaces, etc.) during extraction.

The management of different **data areas** is handled by the `NNNN-content-subpath` extension. By default, only the `content` area is extracted unless otherwise specified (see `--area`).

---

[Back to Display in Web Browser](../08_display_object_en) | [Next to Extracting Metadata](../10_extract_metadata_en) | [Back to Table of Contents](../toc_en)
