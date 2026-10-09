# 0003 – Buildausgabe ist zunächst unvertrauenswürdig

Status: vereinbarter Entwurf, noch nicht implementiert. Stand: 9. Oktober 2026.

## Kontext

Kernelpakete können Root-Maintainer-Skripte enthalten. Header enthalten ausführbare Makefiles, Skripte und Helfer, die externe Modulbuilds verwenden. Ein erfolgreicher Workerbuild ist kein Installationsnachweis.

## Entscheidung

Worker, Prüf-/Paketierungsprozess, Signierer und Broker erhalten getrennte Rechte. Der Prüfprozess rekonstruiert Metadaten, Maintainer-Skripte und ausführbare Headerlogik aus verifizierten Eingaben. Nur seine geschützte Artefaktablage wird akzeptiert. Der Broker kopiert freigegebene Dateien in eigene geschützte Ablage und prüft Hashes erneut.

## Folgen und Nachweise

Pfad-/Linkausbrüche, Spezialdateien, unerwartete Rechte/Fähigkeiten und Austauschrennen werden geprüft. v0.1 bietet keine privilegierte Installation beliebiger importierter Pakete. Die tatsächliche Vertrauensbasis umfasst dennoch Upstream, Toolchain, Projektprogramme und vorhandene Hosthooks/DKMS-Quellen; eine Sandbox beweist keine Kernel-Fehlerfreiheit.

## Grundlagen

[Plan: Vertrauensmodell](../PLAN.md), [Headeraufbereitung](https://github.com/torvalds/linux/blob/master/scripts/package/install-extmod-build).
