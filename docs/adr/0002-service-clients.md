# 0002 – Gemeinsamer Dienst und vier optionale Clients

Status: vereinbarter Entwurf, noch nicht implementiert. Stand: 9. Oktober 2026.

## Kontext

Kernelverwaltung muss ohne Desktop funktionieren. KDE-/Qt- und GTK-Nutzer sowie Terminal-/SSH-Nutzer brauchen passende Oberflächen; parallele Installationslogik wäre fehleranfällig.

## Entscheidung

Rust-Domäne und gemeinsame Clientbibliothek; versioniertes System-D-Bus über zbus. CLI, TUI (Ratatui/Crossterm), Qt 6/QML (CXX-Qt) und GTK 4 (gtk-rs) werden getrennt installierbar. Kein Pflicht-Kirigami oder Pflicht-libadwaita. Alle vier gehören zu v0.1; Build-/CLI-MVPs kommen vorher.

## Folgen und Nachweise

Toolkitübergreifende Zustände, Fehler und Pläne werden gemeinsam geprüft. Vier Clients erhöhen Entwicklungs- und Paketierungsaufwand. Die konkreten Bibliotheksversionen und Brücken benötigen Phase-0-Nachweise; die Auswahl behauptet noch keine geprüfte ABI-/API-Kombination.

## Grundlagen

[Plan: Technologie und Oberflächen](../PLAN.md), [Bibliotheksquellen](../research/sources.md).
