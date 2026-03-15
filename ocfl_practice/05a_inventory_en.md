---
---
[Deutsch](../05a_inventory)

# The Inventory (inventory.json)

The heart of every OCFL object is the `inventory.json`. It contains all the metadata necessary to fully reconstruct the state of the object and its versions.

Note: The complete inventory is available here: [Full Inventory](../05b_full_inventory_en)

## Excerpts from the Inventory

### 1. Header
```json
{
  "id": "urn:nbn:de:gbv:42-test1",
  "type": "https://ocfl.io/1.1/spec/#inventory",
  "digestAlgorithm": "sha512",
  "head": "v1",
  "contentDirectory": "content"
}
```
Explanation:
- `id`: Unique object identifier.
- `type`: Reference to the OCFL spec (here 1.1).
- `digestAlgorithm`: Primary hash algorithm for manifest and states.
- `head`: Latest version (here `v1`).
- `contentDirectory`: Root directory of files inside a version.

### 2. Manifest (excerpt)
```json
{
  "manifest": {
    "1082b5603213...": [
      "v1/content/data/image/IMG_6914.jpg"
    ],
    "6f6a6418e726...": [
      "v1/content/README.md"
    ],
    "87c82e1a5b55...": [
      "v1/content/metadata/info.json"
    ]
  }
}
```
Explanation:
- Keys are file digests (per `digestAlgorithm`).
- Values are paths within the versions (multiple paths are only possible if identical files have NOT been deduplicated).

### 3. Versions (excerpt `v1`)
```json
{
  "versions": {
    "v1": {
      "created": "2026-03-15T15:04:33Z",
      "message": "initial commit",
      "state": {
        "1082b5603213...": [ "data/image/IMG_6914.jpg" ],
        "6f6a6418e726...": [ "README.md" ],
        "87c82e1a5b55...": [ "metadata/info.json" ]
      },
      "user": {
        "name": "User OCFL",
        "address": "mailto:ocfl.user@unibas.ch"
      }
    }
  }
}
```
Explanation:
- `created`/`message`/`user`: Metadata about the version creation.
- `state`: Logical view of the version's files (path names without the `v1/content/` prefix).

### 4. Fixity (excerpt)
```json
{
  "fixity": {
    "blake2b-384": {
      "a3ea374ca4db...": [ "v1/content/README.md" ]
    },
    "md5": {
      "2ecbccede07a...": [ "v1/content/README.md" ]
    }
  }
}
```
Explanation:
- Additional checksums for the same files as in the manifest (for long-term verifiability).

---
For the full JSON please see: [Full Inventory](../05b_full_inventory_en)

Navigation: [Back to Object Structure](../05_object_structure_en) | [Table of Contents](../toc_en) | [Next: Update Object](../06_update_object_en)
