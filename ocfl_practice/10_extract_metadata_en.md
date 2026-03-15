---
---
[Deutsch](../10_extract_metadata)

# Extracting Metadata (extractmeta)

In addition to extracting file contents, `gocfl` provides the capability to export the metadata stored within an OCFL object into a separate JSON file. This is particularly useful for further processing or indexing in external systems.

## 1. The `extractmeta` Command

The `extractmeta` command reads the metadata of the object (including technical metadata from extensions) and writes it to the specified target.

### Basic Syntax:
```bash
gocfl extractmeta [options] [path to storage root or object]
```

### Workshop Example:
To extract the metadata of our test object into a JSON file, we use:

```bash
gocfl --log-level DEBUG --config ./gocfl/config/gocfl.toml extractmeta ./gocfl/temp/test42 -i urn:nbn:de:gbv:42-test1 --output ./gocfl/temp/meta.json
```

**Explanation:**
- `--log-level DEBUG`: Displays detailed information during the process.
- `extractmeta`: The command for extracting metadata.
- `./gocfl/temp/test42`: The path to the storage root.
- `-i urn:nbn:de:gbv:42-test1`: The ID of the object whose metadata is to be extracted.
- `--output ./gocfl/temp/meta.json`: Specifies the file path where the metadata will be written.
- `--obfuscate`: Exports the metadata in an anonymized form. Path and file names are replaced with random UUIDs, while technical metadata such as file sizes, PRONOM IDs, and MIME types are preserved.

## 2. What is Extracted?

The extracted JSON file (`meta.json`) essentially contains three types of information:

1.  **The Object Header:** Basic metadata such as the object ID, the digest algorithm used, and the current state (HEAD).
2.  **Metadata for Files (Manifest):** A listing of all files with their checksums, physical paths in the archive, and logical paths in the respective versions.
3.  **General Metadata from Extensions:** Additional technical information (such as mime types, file sizes, or thumbnails) aggregated by the various OCFL extensions (e.g., `NNNN-indexer`).

## 3. What Does the Result Look Like?

The `meta.json` provides an **aggregated view** of the OCFL object. While the standard inventory is more of a "flat" list of checksums and paths, `extractmeta` combines this information with the results from various extensions.

### Structure of `meta.json`:
- **ID / Head:** Basic metadata of the object.
- **Versions:** History of all versions (corresponds to the `versions` block in the inventory).
- **Files:** This is where the main difference lies. Each file is identified by its digest and contains:
    - **InternalName:** The physical path in the OCFL archive (e.g., `v1/content/data/image.jpg`).
    - **VersionName:** The logical path within each version.
    - **Extension:** Here, additional information collected by extensions is directly assigned to the file (e.g., filesystem attributes, extracted metadata from Tika/Indexer).

### Example Snippets

**Object Header:**
```json
{
  "ID": "urn:nbn:de:gbv:42-test1",
  "DigestAlgorithm": "sha512",
  "Head": "v1",
  "Versions": {
    "v1": {
      "Created": "2026-03-15T15:04:33Z",
      "Message": "initial commit",
      "Name": "User OCFL",
      "Address": "mailto:ocfl.user@unibas.ch"
    }
  },
  ...
}
```

**File Entry with Extensions:**
This shows how extension data (`NNNN-filesystem`, `NNNN-indexer`, `NNNN-thumbnail`) is mapped directly to the file:
```json
"1082b5603213566c3...": {
    "InternalName": ["v1/content/data/image/IMG_6914.jpg"],
    "VersionName": {
        "v1": ["data/image/IMG_6914.jpg"]
    },
    "Extension": {
        "NNNN-filesystem": {
            "v1": [{
                "path": "data/image/IMG_6914.jpg",
                "meta": {
                    "size": 3696602,
                    "mTime": "2023-11-27T16:54:03+01:00"
                }
            }]
        },
        "NNNN-indexer": {
            "mimetype": "image/jpeg",
            "pronom": "fmt/43",
            "type": "image"
        },
        "NNNN-thumbnail": {
            "id": "internal",
            "filename": "metadata/thumbnails/v1/00002.png"
        }
    }
}
```

**Global Extensions Section:**
At the end of the `meta.json`, there is a summary section for extensions that provide global information for the entire object:
```json
{
  ...
  "Extension": {
    "NNNN-content-subpath": {
      "content": {
        "path": "data",
        "description": "Payload of archival object"
      },
      "metadata": {
        "path": "metadata",
        "description": "additional semantic metadata"
      }
    },
    "NNNN-metafile": {
      "title": "Some OCFL Testfiles (initial version)",
      "authors": ["Doe, John", "Doe, Jane"],
      "description": "Lorem ipsum dolor sit amet...",
      "created": "2023-10-31",
      "collection": "OCFL Demo"
    }
  }
}
```

### Relationship to the Inventory
The **Inventory** (`inventory.json`) is the "truth" of the OCFL standard. It is optimized for ensuring the integrity and structure of the object, but is harder to consume directly by humans or external search engines because information needs to be merged across multiple blocks (`manifest`, `versions`, `fixity`).

The **`meta.json`** is a **refined export view**. It resolves the references of the inventory and enriches them with data from the configured extensions. This export serves as the basis for the `display` function of `gocfl` and is used to ingest data into the OCFL Native Archive.

### Long-term Archiving and Discoverability

The **OCFL object** thus contains all the metadata required for the long-term archiving of the object. The `extractmeta` function provides this in a processed format (`meta.json`) that an archive system can easily interpret. This ensures the discoverability of the object within the archive.

This makes it the ideal basis for:
- Importing into a database.
- Displaying in a web frontend.
- Quick searching and identifying objects in the archive.
- Long-term archiving of technical metadata in an easily readable format as additional information.

## 4. Anonymized Export (Obfuscation)

Using the parameter `--obfuscate`, the metadata can be exported in an anonymized form. This is particularly useful when metadata needs to be shared for analysis or testing purposes without revealing sensitive information such as real file names or path structures.

### What Happens During Obfuscation?

1.  **UUIDs Instead of Hashes/Names:** The unique identifiers (Keys) in the `Files` block, as well as the paths in `InternalName` and `VersionName`, are replaced with random UUIDs.
2.  **Removal of Clear Names:** All information that allows conclusions to be drawn about the original file structure is obscured.
3.  **Reduction to Technical Core Data:** Almost all metadata from the extensions is removed. Only the following are preserved:
    - **File Size** (`size`)
    - **MIME Type** (`mimetype`)
    - **PRONOM ID** (`pronom`): Used for **Preservation Management**. This allows for **Format Watching** even with anonymized data.
4.  **Empty Checksums:** The `Checksums` block is cleared to prevent any conclusions about the original file.

### Example Snippet (Anonymized):

```json
{
  "Files": {
    "14aafd0e-4d07-47c2-91d0-b08d91ed5b53": {
      "Checksums": {},
      "InternalName": ["v1/content/c7537293-a62f-43f6-9e3d-e8c27bf0c245"],
      "VersionName": {
        "v1": ["c7537293-a62f-43f6-9e3d-e8c27bf0c245"]
      },
      "Extension": {
        "NNNN-indexer": {
          "mimetype": "application/pdf",
          "pronom": "fmt/20",
          "size": 547440,
          "metadata": {}
        }
      }
    }
  }
}
```

As shown in the example, the technical characteristics of the file (PDF, approx. 547 KB) are preserved for statistical evaluations, while the context (file name, path, hash) has been completely anonymized.

---

[Back to Extracting Content](../09_extract_object_en) | [Back to Table of Contents](../toc_en)
