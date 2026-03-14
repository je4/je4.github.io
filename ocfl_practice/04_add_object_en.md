[Deutsch](04_add_object.md)

# Creating and Adding Objects

In this step, we add our first object to the OCFL storage root. The `add` command is used to transfer new content into the archive.

## 1. The `add` Command

With `gocfl add`, files or directories can be stored as an OCFL object.

### Basic Syntax:
```bash
gocfl add /path/to/data -i "object-id"
```

### Workshop Example:
In our workshop scenario, we add the content from `payload1/` to the previously initialized storage root `test42/`:

```bash
gocfl --log-level DEBUG --config ./gocfl/config/gocfl.toml add ./gocfl/temp/test42/ ./gocfl/testdata/payload1/ --ext-NNNN-metafile-source ./gocfl/testdata/info.json -i urn:nbn:de:gbv:42-test1
```

**Explanation of Parameters:**
- `--log-level DEBUG`: Shows detailed information during the process.
- `--config ./gocfl/config/gocfl.toml`: Uses our central configuration file.
- `add`: The command to add/create an object.
- `./gocfl/temp/test42/`: The path to the **storage root** (our target directory).
- `./gocfl/testdata/payload1/`: The source path of the data to be archived.
- `--ext-NNNN-metafile-source ./gocfl/testdata/info.json`: A specific parameter for the `NNNN-metafile` extension to include an external metadata file.
- `-i urn:nbn:de:gbv:42-test1`: The unique **object ID**.

## 2. What Happens During Adding?

1. **Hashing:** `gocfl` calculates the checksums (default SHA-512) of all files.
2. **Layout:** The object ID is translated into a physical path according to the configured storage layout extension (e.g., `0004-hashed-n-tuple-storage-layout`).
3. **Structure:** Within the object directory, the OCFL structure is created (inventory, `v1/content/`).
4. **Extensions:** All configured object extensions are triggered (e.g., for metadata extraction).

---

[Back to OCFL Extensions](03_extensions_en.md) | [Back to Table of Contents](toc_en.md) | [Next Topic: Object Structure](05_object_structure_en.md)
