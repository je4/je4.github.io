---
---
[Deutsch](../05_object_structure)

# The Directory Structure of an OCFL Object

After an object has been added with the `add` command, it is stored in the storage root according to the configured storage layout. In our workshop example, the object is located at:

`C:\temp\audsworkshop\temp\test42\1f7\d28\1ac\1f7d281accf403621871ec793c3b1c40eb480165e17b80da924aba0280246d12`

## 1. Overview of the Structure (Tree)

Here is the actual directory structure of the OCFL object (v1.1) after the first ingest (`v1`) in our workshop scenario:

```text
1f7d281accf403621871ec793c3b1c40eb480165e17b80da924aba0280246d12/
│   0=ocfl_object_1.1          # Marker: This is an OCFL object (Version 1.1)
│   inventory.json             # The current inventory (manifest, state, versions)
│   inventory.json.sha512      # Checksum of the current inventory
│
├───extensions                 # Object-specific extension configurations
│   ├───0001-digest-algorithms
│   ├───0011-direct-clean-path-layout
│   ├───initial
│   ├───NNNN-content-subpath
│   ├───NNNN-filesystem
│   ├───NNNN-gocfl-extension-manager
│   ├───NNNN-indexer
│   ├───NNNN-metafile
│   │       gocfl-info-1.0.json
│   └───NNNN-thumbnail
│
└───v1                         # The first version of the object
    │   inventory.json         # Copy of the inventory at the time of v1
    │   inventory.json.sha512  # Checksum of the inventory copy
    │
    └───content                # The actual content files of v1
        │   README.md
        │
        ├───data               # Payload of v1
        │   ├───=u007Etest=u005B0=u005D
        │   │       .empty
        │   │
        │   ├───audio
        │   │       Dragan_Espenschied_-_10_-_Оля=u0020Зимой.mp3
        │   │       Education=u0020of=u0020the=u0020Noobz=u00201.doc
        │   │       Education=u0020of=u0020the=u0020Noobz=u00201.docx
        │   │       Education=u0020of=u0020the=u0020Noobz=u00201.odt
        │   │
        │   ├───image
        │   │       IMG_6914.gif
        │   │       IMG_6914.jpg
        │   │       IMG_6914.png
        │   │       IMG_6914.tga
        │   │       IMG_6914.webp
        │   │
        │   ├───text
        │   │       export_mets.xml
        │   │       fulltext_5693376.xml
        │   │       Testseite.pdf
        │   │       Testseite=u0020-=u0020Kopie.pdf
        │   │
        │   ├───vector
        │   │       ki.ai
        │   │       ki.eps
        │   │       ki.pdf
        │   │       ki.svg
        │   │       ki.svgz
        │   │
        │   └───video
        │           Together=u0020Elsewhere=u002001.docx
        │           Together=u0020Elsewhere=u002001.pdf
        │           together_01_excerpt.mkv
        │
        └───metadata           # Metadata of v1
            │   filesystem_v1.jsonl.gz
            │   indexer_v1.jsonl.gz
            │   info.json
            │   thumbnail_v1.jsonl.gz
            │
            └───thumbnails     # Thumbnails of v1
                └───v1
                        00001.png
                        00002.png
                        00003.png
                        00004.png
                        00006.png
```

A detailed representation of the structure after an update (v2) can be found in the chapter **[Updating an Object](../06_update_object_en#3-structure-after-the-update)**.

## 2. Explanation of Components

### A. Path and Layout Extensions

These extensions determine how the files are physically stored in the storage root and within the object.

#### The Object ID and the Layout (Storage Root)
The long directory name `1f7d281acc...` is the result of the hashing strategy of the extension `0004-hashed-n-tuple-storage-layout`. In our case, the object ID `urn:nbn:de:gbv:42-test1` was hashed (SHA-256), resulting in the value `1f7d281accf403621871ec793c3b1c40eb480165e17b80da924aba0280246d12`. According to the default settings (`tupleSize: 3`, `numberTuples: 3`), the path `1f7/d28/1ac/` was prefixed.

#### Filename Cleanup (0011-direct-clean-path-layout)

Within the object (under `v1/content/data/`), the filenames are cleaned up by the extension **[0011-direct-clean-path-layout](https://ocfl.github.io/extensions/0011-direct-clean-path-layout.html)** to ensure compatibility with different filesystems.

**What the extension does:**
- **Character Replacement:** Problematic characters (such as spaces, control characters, or reserved characters like `*`, `?`, `:`) are replaced by their Unicode hex code in the format `=uXXXX`.
- **UTF-8 Validation:** Ensures that all paths are valid UTF-8.
- **Length Restrictions:** Prevents path segments that are too long.

**Example from our tree:**
The file `Dragan_Espenschied_-_10_-_Оля Зимой.mp3` contains a space. In the OCFL object, it becomes:
`Dragan_Espenschied_-_10_-_Оля=u0020Зимой.mp3`

Here, `=u0020` stands for the space (U+0020). This replacement is done automatically by `gocfl` when adding the files to ensure that the archive can be read on any operating system (Windows, Linux, macOS) without problems.

#### Content Structure (data & metadata) and NNNN-content-subpath

A significant difference from a standard OCFL structure is the division within the version directories (e.g., `v1/content/`). This is controlled by the extension **`NNNN-content-subpath`**.

The extension allows defining various logical areas, which are mapped to physical subdirectories in the `content` folder:

- **`data/` (Area: `content`)**: This is the default area for the actual user data (payload). According to the configuration of `gocfl`, the logical area `content` is physically stored in the `data/` folder. **URL encoding** of special characters also takes place here to increase filesystem compatibility.
- **`metadata/` (Area: `metadata`)**: Other extensions store their generated files in this area. In our workshop example, this area is mapped to the `metadata/` folder.
- **`README.md`**: The `NNNN-content-subpath` extension automatically creates a `README.md` directly in the `content` directory of each version, which explains the meaning of the subfolders to human observers.

### B. Metadata Extensions

These extensions automatically enrich the object with information during the ingest. In the `metadata/` folder of the first version, we see the results:

- **`indexer_v1.jsonl.gz`**: Technical metadata (file formats, bitrates, resolutions) extracted by the `NNNN-indexer` extension.
- **`filesystem_v1.jsonl.gz`**: Original filesystem metadata (creation date, access rights) secured by `NNNN-filesystem`.
- **`info.json`**: The custom metadata we provided via the `--ext-NNNN-metafile-source` parameter (`NNNN-metafile`).

### C. Extensions for Readability and Preview

This group serves to provide a quick overview of the content:

- **`thumbnails/`**: The `NNNN-thumbnail` extension has automatically generated small preview images for supported file types (images, videos, PDFs). These are located in the subdirectory `metadata/thumbnails/v1/` and are listed in the corresponding index file (`thumbnail_v1.jsonl.gz`).

## 3. Extensions in the Object
Like the storage root, each object also has an `extensions/` directory. Here, `gocfl` stores the configurations for all active extensions. A complete list of all extensions can be found under **[List of OCFL Extensions](../03b_extension_list_en)**.

---

[Back to Adding Objects](../04_add_object_en) | [Back to Table of Contents](../toc_en) | [Next Topic: Updating Objects](../06_update_object_en)
