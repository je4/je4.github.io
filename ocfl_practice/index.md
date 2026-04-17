---
---
[English](../../index_en)

[Zurück zur Startseite](../../index)

# OCFL Standard in der Praxis

## Abstract

Im Rahmen eines „Live-Lab“ wird das Erstellen von OCFL-Objekten mit dem Werkzeug gocfl diskutiert, praktisch durchgeführt und getestet. Dabei gibt es einen Tiefeneinblick in das OCFL-Format (u. a. Integrität, Benennungen, Versionierung und Erweiterungen) sowie in typische Versionierungsaktionen wie Löschen, Ersetzen und Umbenennen von Inhalten. Ein Schwerpunkt liegt auf für die digitale Langzeitarchivierung relevanten OCFL-Erweiterungen, z. B. Extraktion technischer und Dateisystem-Metadaten, nachhaltigere Dateinamen und objektinterne Formatmigration (inkl. Ansätzen wie METS/PREMIS und Trusted Timestamps). Der Workshop ist zum Zuschauen geeignet, richtet sich aber vor allem an Teilnehmende, die die Prozesse am Rechner selbst nachvollziehen wollen; gearbeitet wird im Terminal, entweder in einem bereitgestellten Container oder in der eigenen Umgebung mit entsprechenden Voraussetzungen.

---

## Was ist OCFL?

Das **Oxford Common File Layout (OCFL)** ist ein offener Standard für die Strukturierung von digitalen Objekten in einem Dateisystem oder Objektspeicher. Es wurde entwickelt, um die langfristige Erhaltung digitaler Inhalte zu gewährleisten, indem es eine robuste, vollständige und versionierte Speichermethode bietet, die unabhängig von spezifischer Software-Infrastruktur ist.

### Kernmerkmale von OCFL:
- **Vollständigkeit (Self-Contained):** Ein OCFL-Objekt enthält alle Informationen, die zu seiner Rekonstruktion notwendig sind.
- **Integrität:** Jede Datei im Objekt wird durch Prüfsummen (Hashes) geschützt.
- **Versionierung:** Änderungen an Objekten werden effizient über Versionen hinweg verfolgt.
- **Selbstbeschreibend (Self-Describing):** Durch die Mitlieferung der Spezifikation und Dokumentation der verwendeten Erweiterungen direkt im Speicherpfad bleibt das Archiv auch ohne externe Ressourcen verständlich.
- **Erweiterbarkeit:** Der Standard erlaubt spezifische Erweiterungen für zusätzliche Metadaten oder Funktionen.

OCFL wird oft als eine „intelligente Nachfolge“ von [BagIt](https://de.wikipedia.org/wiki/BagIt) angesehen. Es erweitert BagIt um Versionierung, Deduplizierung sowie Umbenennungsmöglichkeiten von Pfaden und Dateinamen. Im Rahmen der Versionierung werden Löschungen und Umbenennungen ebenso unterstützt wie das Hinzufügen von Dateien.

### Struktur von OCFL
Der Standard unterscheidet zwischen zwei grundlegenden Kernelementen:
- **[Storage Root](https://ocfl.io/1.1/spec/#storage-root):** Die oberste Ebene im Dateisystem, in der OCFL-Objekte gespeichert werden. Sie enthält administrative Informationen zur OCFL-Version und dem verwendeten Layout.
- **[Object](https://ocfl.io/1.1/spec/#object-spec):** Ein digitales Objekt innerhalb des Storage Roots, das aus einer oder mehreren Versionen von Dateien und Metadaten besteht.

## Das Werkzeug gocfl

In diesem Workshop wird das Werkzeug **[gocfl](https://github.com/ocfl-archive/gocfl/)** verwendet. Es handelt sich dabei um ein in der Programmiersprache Go geschriebenes Tool, das folgende Funktionen unterstützt:

- **Erstellung** von OCFL-Objekten.
- **Update** bestehender Objekte durch neue Versionen.
- **Validierung** von Objekten und Storage Roots gegen die Spezifikation.
- **Extraktion** von Inhalten aus OCFL-Objekten.
- **Erstellung und Verwaltung** von Storage Roots.

---

### Workshop-Navigation

Eine Übersicht über den geplanten Workshop-Ablauf finden Sie im **[Inhaltsverzeichnis (TOC)](../toc)**.

### Weiterführende Informationen
Weitere Details zum Standard und der Spezifikation finden Sie auf der offiziellen Webseite:

- [OCFL Website](https://ocfl.io)
- [OCFL Spezifikation](https://ocfl.io/1.1/spec/)
- [OCFL Extensions (Erweiterungen)](https://ocfl.io/extensions/)
