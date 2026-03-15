---
---
[Deutsch](../07_validate_object)

# Validation (validate)

A crucial advantage of OCFL is integrity assurance. With `gocfl`, it is possible to verify at any time whether a storage root or an individual object still complies with the specification and whether all checksums (digests) are correct.

## 1. The `validate` Command

The `validate` command (or the alias `check`) performs a deep check of the OCFL structure. It checks both the formal requirements of the specification and the consistency of the data against the inventory.

### Basic Syntax:
```bash
gocfl validate [path to storage root or object]
```

### Workshop Example:
We validate the specific object in the storage root that we created and updated in the previous steps:

```bash
gocfl --log-level INFO --config ./gocfl/config/gocfl.toml validate ./gocfl/temp/test42/ -i urn:nbn:de:gbv:42-test1
```

**Explanation of Parameters:**
- `validate`: The command to verify integrity.
- `./gocfl/temp/test42/`: The path to the **storage root**.
- `-i urn:nbn:de:gbv:42-test1`: The specific **object ID** to be checked. If this ID (or the flag `-i`) is omitted, `gocfl` validates **all** objects found in the storage root sequentially.

## 2. What is checked?

During validation, `gocfl` performs the following steps:

1. **Structure Check:** Does the structure of the storage root and the objects comply with the OCFL specification (presence of markers, correct directory structure)?
2. **Inventory Validation:** Is the `inventory.json` well-formed? Are all versions present without gaps?
3. **Fixity Check:** Recalculates the digest for every file in the object and compares it with the value in the manifest.
4. **Reference Check:** Do all paths in the `state` point to existing files in the `manifest`? Are there "orphaned" files in the `content` folder that are not listed in any manifest?
5. **Extension Check:** Are the used extensions correctly configured and documented?

## 3. Selective Validation

If a storage root contains many objects, validation can be time-consuming. `gocfl` offers flags to narrow down the check:

- **Only a specific object (ID):**
  ```bash
  gocfl validate ./gocfl/temp/test42/ --object-id urn:nbn:de:gbv:42-test1
  ```
- **Only an object at a specific path:**
  ```bash
  gocfl validate ./gocfl/temp/test42/ --object-path 1f7/d28/1ac/1f7d281acc...
  ```

## 4. Result of the Validation

When the validation is complete, `gocfl` outputs a summary. It is important to distinguish between **errors** and **warnings**.

### Example Output from the Workshop:
```text
{"level":"info","time":"2026-03-14T17:13:59Z","message":"loading configuration from "}
2026-03-14T17:13:59Z INF validating '/home/ocfl/gocfl/temp/test42' host=gocfl timestamp="..."
2026-03-14T17:13:59Z WRN extension NNNN-gocfl-extension-manager is not registered ... validationErrorCode=W013 validationErrorMessage="‘In an OCFL Object, extension sub-directories SHOULD be named according to a registered extension name.’"
2026-03-14T17:13:59Z WRN extension NNNN-content-subpath is not registered ... validationErrorCode=W013
...
2026-03-14T17:13:59Z INF object 'urn:nbn:de:gbv:42-test1' with object version '1.1' found host=gocfl task=checker ...

[extension NNNN-content-subpath is not registered]
   #W013 - ‘In an OCFL Object, extension sub-directories SHOULD be named according to a registered extension name.’ []

...

no errors found
2026-03-14T17:14:00Z INF Duration: 221.808179ms host=gocfl ...
```

### Interpretation of the Result:

1.  **Warnings (W-Codes, e.g., W013):**
    *   In our example, we see several warnings with the code `W013`.
    *   These refer to the fact that the extensions used (e.g., `NNNN-content-subpath`) are not officially registered with the OCFL community (hence the `NNNN-` prefix).
    *   According to the OCFL specification, extensions **should** be registered, but it is **allowed** to use custom or community extensions.
    *   A warning does **not** mean the object is invalid.

2.  **Errors (E-Codes):**
    *   Errors (e.g., `E001`, `E066`) would indicate a violation of the OCFL core specification (MUST requirements).
    *   An object with errors is considered **invalid** and must be repaired (e.g., in case of checksum errors).

3.  **Conclusion:**
    *   The message **"no errors found"** at the end confirms that the object is technically correct and integer despite the warnings. All files are present and the checksums match the inventory.

---

[Back to Updating an Object](../06_update_object_en) | [Back to Table of Contents](../toc_en) | [Next to Display (display)](../08_display_object_en)
