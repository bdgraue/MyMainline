# 0001 – Eigenständiges Projekt und offizielle Buildverfahren

Status: vereinbarter Entwurf, noch nicht implementiert. Stand: 9. Oktober 2026.

## Kontext

Mainline ist auf vorgebaute Ubuntu-Pakete und Vala/GTK ausgerichtet. linux-tkg und TuxMake decken wichtige Buildaufgaben bereits ab, aber nicht nachgewiesen unsere vollständige Dienst-, Autorisierungs- und Recovery-Kette.

## Entscheidung

Eigenständiger Rust-Dienst mit direkter Orchestrierung von Kconfig/Kbuild. Mainline, linux-tkg und TuxMake dienen als Referenzen; keine pauschale Codeübernahme und keine TuxMake-Laufzeitabhängigkeit. Für künftigen Anwendungscode ist GPL-3.0-only als Planungsdefault vorgesehen.

## Folgen und Nachweise

Buildwissen muss gezielt wiederverwendet werden, während Integration und Wartung eigene Verantwortung bleiben. Ein Fork spart die nötige Neugestaltung der Vertrauensgrenzen nicht. Spätere Übernahmen benötigen Herkunfts-, Versions- und Lizenzprüfung.

## Grundlagen

[Alternativen und Lizenzanalyse](../research/alternatives.md).
