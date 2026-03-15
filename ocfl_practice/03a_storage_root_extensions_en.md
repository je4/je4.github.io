---
---
[Deutsch](03a_storage_root_extensions)

# OCFL Storage Root Extensions

This page describes the specific OCFL extensions used in the storage root. As we saw during the initialization of the storage root, `gocfl` automatically creates configuration directories in the `extensions/` folder.

## 1. Initial Extension (`initial`)
The [Initial Extension](https://ocfl.github.io/extensions/initial.html) is used in `gocfl` to determine which extension should be loaded first.

Content of `extensions/initial/config.json`:
```json
{
   "extensionName": "initial",
   "extension": "NNNN-gocfl-extension-manager"
}
```
In our `test42` example, it points to the `NNNN-gocfl-extension-manager`. This ensures that the basic management logic of `gocfl` is active before other extensions are processed.

## 2. gocfl Extension Manager (`NNNN-gocfl-extension-manager`)
This is a meta-extension of `gocfl` responsible solely for initializing the storage root (e.g., copying documentation files). Further details can be found in the [Extension Manager documentation](https://github.com/ocfl-archive/gocfl/blob/main/docs/NNNN-gocfl-extension-manager.md).

In the Extension Manager, the term **`StorageRootPath`** is used as an identifier for the extension type (in this case, the storage layout of the root). In the Go implementation of `gocfl`, this is defined in [`storagerootExtension.go`](https://github.com/ocfl-archive/gocfl/blob/ed2bafb10a8c2bdbb2fad3ad271aa7e6993b75ed/pkg/ocfl/storageroot/storagerootExtension.go).

Content of `extensions/NNNN-gocfl-extension-manager/config.json`:
```json
{
   "extensionName": "NNNN-gocfl-extension-manager",
   "sort": {
      "StorageRootPath": [
         "0004-hashed-n-tuple-storage-layout"
      ]
   },
   "exclusion": {
      "StorageRootPath": [
         [
            "NNNN-direct-clean-path-layout",
            "0003-hash-and-id-n-tuple-storage-layout",
            "0004-hashed-n-tuple-storage-layout",
            "0002-flat-direct-storage-layout",
            "0006-flat-omit-prefix-storage-layout",
            "NNNN-pairtree-storage-layout",
            "NNNN-direct-path-layout"
         ]
      ]
   }
}
```
The Extension Manager also controls dependencies via the `StorageRootPath` type:
- **`sort`**: Specifies that the layout module `0004` is prioritized.
- **`exclusion`**: Prevents multiple (mutually exclusive) layout strategies from being activated simultaneously for the storage root.

## 3. Storage Layout (`0004`)
The `ocfl_layout.json` refers to the [0004-hashed-n-tuple-storage-layout](https://ocfl.github.io/extensions/0004-hashed-n-tuple-storage-layout.html) extension. The associated configuration is located at:
`extensions/0004-hashed-n-tuple-storage-layout/config.json`

Content of `config.json`:
```json
{
   "extensionName": "0004-hashed-n-tuple-storage-layout"
}
```
This file (along with the information in `ocfl_layout.json`) controls how deeply the directory structure is nested and which hash algorithm is used for path formation. In the minimal version, the name of the extension is often sufficient as the specification provides default values.

#### Default Values for Extension 0004:
If these parameters are not defined in `config.json`, the following defaults apply according to the specification:
- **`digestAlgorithm`**: `sha256` (The algorithm used to calculate the hash value of the object ID).
- **`tupleSize`**: `3` (Number of characters per directory level).
- **`numberTuples`**: `3` (Number of directory levels created by hashing).

This means an object is stored by default in a structure like `[hash1-3]/[hash4-6]/[hash7-9]/[object-id]`.

---

[Back to OCFL Extensions](03_extensions_en) | [Back to Table of Contents](toc_en)
