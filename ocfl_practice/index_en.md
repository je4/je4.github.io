---
---
[Deutsch](index.md)

[Back to Home](../index_en.md)

# OCFL Standard in Practice

## Abstract

As part of a "Live-Lab," the creation of OCFL objects with the tool gocfl will be discussed, practically carried out, and tested. This includes a deep dive into the OCFL format (e.g., integrity, naming, versioning, and extensions) as well as typical versioning actions such as deleting, replacing, and renaming content. A focus is placed on OCFL extensions relevant for long-term digital preservation, such as extraction of technical and filesystem metadata, more sustainable filenames, and internal object format migration (including approaches like METS/PREMIS and Trusted Timestamps). The workshop is suitable for spectators but is primarily aimed at participants who want to follow the processes on their own computers; work is done in the terminal, either in a provided container or in your own environment with appropriate prerequisites.

---

## What is OCFL?

The **Oxford Common File Layout (OCFL)** is an open standard for structuring digital objects in a filesystem or object store. It was developed to ensure the long-term preservation of digital content by providing a robust, complete, and versioned storage method that is independent of specific software infrastructure.

### Core Features of OCFL:
- **Self-Contained:** An OCFL object contains all the information necessary for its reconstruction.
- **Integrity:** Every file in the object is protected by checksums (hashes).
- **Versioning:** Changes to objects are efficiently tracked across versions.
- **Self-Describing:** By including the specification and documentation of the used extensions directly in the storage path, the archive remains understandable even without external resources.
- **Extensibility:** The standard allows for specific extensions for additional metadata or functions.

OCFL is often seen as an "intelligent successor" to [BagIt](https://en.wikipedia.org/wiki/BagIt). It extends BagIt with versioning, deduplication, and the ability to rename paths and filenames. Within versioning, deletions and renames are supported just as well as adding files.

### Structure of OCFL
The standard distinguishes between two basic core elements:
- **[Storage Root](https://ocfl.io/1.1/spec/#storage-root):** The top level in the filesystem where OCFL objects are stored. It contains administrative information about the OCFL version and the layout used.
- **[Object](https://ocfl.io/1.1/spec/#object-spec):** A digital object within the storage root consisting of one or more versions of files and metadata.

## The Tool gocfl

In this workshop, the tool **[gocfl](https://github.com/ocfl-archive/gocfl/)** is used. It is a tool written in the Go programming language that supports the following functions:

- **Creation** of OCFL objects.
- **Update** of existing objects through new versions.
- **Validation** of objects and storage roots against the specification.
- **Extraction** of content from OCFL objects.
- **Creation and Management** of storage roots.

---

### Workshop Navigation

An overview of the planned workshop course can be found in the **[Table of Contents (TOC)](toc_en.md)**.

### Further Information
Further details on the standard and specification can be found on the official website:

- [OCFL Website](https://ocfl.io)
- [OCFL Specification](https://ocfl.io/1.1/spec/)
- [OCFL Extensions](https://ocfl.io/extensions/)
