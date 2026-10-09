# MyMainline

Ein geplanter Kernel-Manager für verifizierte Linux-Upstream-Releases von kernel.org: individuell konfigurieren, isoliert bauen, als native Pakete verwalten und kontrolliert testen.

**Status: Planung. Dieses Repository enthält ausschließlich Dokumentation, noch keine ausführbare Anwendung.** Es gibt noch keine freigegebenen Builds, Installationsbefehle oder bestandenen Kernel-/Bootprüfungen. Die Umsetzung beginnt erst nach gesondertem Auftrag.

## Warum dieses Projekt?

Der Ausgangspunkt ist Ubuntu Mainline 1.4.15: Das vorhandene Werkzeug installiert vorgefertigte Ubuntu-Mainline-Pakete. MyMainline soll unabhängig von deren Bereitstellung werden und zusätzlich Quellenprüfung, nachvollziehbare Konfiguration, reproduzierbare Builds und abgesicherte Systemänderungen zusammenführen.

Das ist ein Angebot für Nutzer, die einen bestimmten Upstream-Kernel benötigen oder Builds nachvollziehen möchten. Ein neuerer Kernel oder ein eigenes Profil bedeutet nicht automatisch mehr Leistung oder dieselben Distributionsfunktionen. Die [Recherche zu bestehenden Werkzeugen](docs/research/alternatives.md) erklärt, wann vorhandene Lösungen ausreichen.

## Dokumentation

| Dokument | Inhalt |
| --- | --- |
| [Vollständiger Plan](docs/PLAN.md) | Architektur, Sicherheitsregeln, Buildpipeline, Plattformgrenzen und Implementierungsphasen |
| [Roadmap](docs/ROADMAP.md) | Liefergegenstände, Abhängigkeiten und überprüfbare Abnahmekriterien |
| [Recherche](docs/research/README.md) | Ausgangslage, Alternativen, technische Erkenntnisse und Quellen |
| [Architekturentscheidungen](docs/adr/README.md) | Gründe und Folgen der gewählten Verfahren |
| [Validierung](docs/validation.md) | Machbarkeitsnachweise, Sicherheitstests, VM-Matrix und Releasekriterien |
| [Offene Nachweise und Risiken](docs/risks.md) | Blocker, offene Festlegungen und Bedingungen für eine Freigabe |
| [Fachliches Glossar](CONTEXT.md) | Gemeinsame Bedeutung der zentralen Begriffe |

## Geplanter erster Umfang

Erste Installationsziele sind Ubuntu/Kubuntu 26.04 und Debian 13 auf amd64, mit GRUB, initramfs-tools und einer geprüften unverschlüsselten ext4-Konstellation. UEFI-Installationen setzen zunächst deaktiviertes Secure Boot voraus. Nicht unterstützte Installationskonstellationen sollen Build und Export erlauben, Systemänderungen aber sperren.

Ein unprivilegierter Verwaltungsdienst und ein kleiner privilegierter Broker bedienen vier getrennt installierbare Clients: CLI, TUI, Qt und GTK. Ein Build-MVP entsteht vor dem CLI-Verwaltungs-MVP; alle vier Clients gehören zum geplanten v0.1-Release. Fedora, Arch und weitere Boot-/Speicherverfahren folgen nach eigenen Nachweisen.

Quellen werden kryptografisch geprüft. Builds verändern die produktive Bootumgebung nicht. Installation, Testboot, dauerhafter Standardwechsel und Neustart benötigen jeweils ausdrückliche Freigabe; ein bestätigter funktionsfähiger Rückfallkernel bleibt erhalten.

CPU-spezifische Compilerflags und das Entfernen von Treibern sind vorerst außerhalb des v0.1-Umfangs. Das schützt Portabilität und spätere Geräte-/Bootpfade. Für spätere ausdrücklich gewählte und geprüfte individuelle Profile bleibt diese Möglichkeit offen: [Begründung](docs/adr/0008-config-profiles.md).

## Entwicklungsstand und Lizenz

Die dokumentierten Entscheidungen sind ein Entwurf. Phase 0 muss zentrale Verfahren praktisch nachweisen; ein unabhängiges Architekturreview steht aus. Es wurden im Rahmen dieser Veröffentlichung keine Kernel gebaut oder installiert und keine Boot- oder Systemkonfigurationen geändert.

Für künftigen Anwendungscode ist **GPL-3.0-only** als Planungsdefault vorgesehen. Eine vollständige Lizenzierung des künftigen Codes und die Prüfung aller Drittkomponenten sind vor dessen Veröffentlichung zu erledigen. Fremdcode wurde für diese Dokumentation nicht übernommen.

## Dokumentation prüfen

Mit vorhandenem markdownlint-cli2:

```sh
markdownlint-cli2 "**/*.md"
git diff --check
```

Die Lint-Konfiguration erlaubt lange Absätze/Quellenlinks und kompakte Tabellen. Anwendungs-, Kernel- und VM-Tests existieren in diesem Dokumentationsstand noch nicht.
