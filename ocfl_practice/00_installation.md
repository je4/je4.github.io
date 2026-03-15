---
---
[English](00_installation_en.md)

# Installation von gocfl

Bevor wir mit dem Workshop beginnen, muss `gocfl` auf Ihrem System installiert werden. Da `gocfl` in Go geschrieben ist, kann es direkt über den Go-Paketmanager installiert werden.

## 1. Voraussetzungen

- Eine installierte Go-Umgebung (Version 1.21 oder neuer wird empfohlen).
- Internetzugang zum Herunterladen des Repositorys.

## 2. Installationsbefehl

Führen Sie den folgenden Befehl in Ihrem Terminal aus, um die für diesen Workshop spezifische Version von `gocfl` zu installieren:

```bash
go install github.com/ocfl-archive/gocfl/v2/gocfl@v2.0.6-beta29
```

## 3. Verifizierung

Nach der Installation können Sie überprüfen, ob `gocfl` korrekt installiert wurde und im Pfad (`PATH`) verfügbar ist:

```bash
gocfl --version
```

Sollte der Befehl nicht gefunden werden, stellen Sie sicher, dass Ihr `GOBIN`-Verzeichnis (standardmäßig `$HOME/go/bin` oder `%USERPROFILE%\go\bin`) in Ihrer `PATH`-Umgebungsvariable enthalten ist.

---

[Weiter zur Benutzung von gocfl](01_gocfl_usage.md) | [Zurück zum Inhaltsverzeichnis](TOC)
