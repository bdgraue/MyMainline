# Bestehende Werkzeuge, Mainline und Lizenz

Stand: 9. Oktober 2026. Siehe [Evidenzgrenzen](README.md).

## Vergleich

| Werkzeug | Bereits vorhanden | Bedeutung für MyMainline |
| --- | --- | --- |
| [Ubuntu Mainline](https://github.com/bkw777/mainline) | Grafische und Kommandozeilenverwaltung vorgebauter Ubuntu-Mainline-Pakete | Gute Referenz für Bedienung und Inventar; die Verfügbarkeit fertiger Pakete bleibt Voraussetzung |
| [linux-tkg](https://github.com/Frogging-Family/linux-tkg) | Konfigurierbare Kernelbuilds, Vanilla-Optionen und distributionsabhängige Paketierung/Installation | Ernsthafte Alternative für individuelle Builds. Es wäre falsch, das Projekt auf Patchsets zu reduzieren. Unser Entwurf setzt zusätzlich einen getrennten Dienst/Broker, kontrollierte Artefaktfreigabe und konkret spezifizierte Recovery voraus |
| [TuxMake](https://tuxmake.org/cli/) | Buildautomatisierung mit definierten Umgebungen und mehreren Ausgabezielen | Möglicher Baustein oder Referenz. Die Verantwortung für Inventar, Autorisierung und Testboot bleibt eine eigene Produktaufgabe |
| [CachyOS Kernel Manager](https://github.com/CachyOS/kernel-manager) | Qt-Kernelverwaltung im Arch-/CachyOS-Umfeld; das eingesehene README beschreibt Arch-Repositories und optionale AUR-Unterstützung | Relevante GUI-/Distributionsreferenz. Kein Nachweis für unsere Ubuntu-/Debian-Integrations- und Sicherheitskette; der konkrete aktuelle Buildpfad wäre gesondert zu auditieren |
| [XanMod](https://xanmod.org/) | Eigene Kernelkonfigurationen/-Funktionen und fertige Pakete | Kann den Bedarf nach einem aktuellen Desktopkernel erfüllen; ersetzt keine individuell nachvollziehbare Build-/Verwaltungskette für unveränderte Upstream-Quellen |
| [Liquorix](https://liquorix.net/) | Fertige alternative Desktopkernel | Ebenfalls eine mögliche einfachere Lösung für den Aktualitäts-/Desktopbedarf; nicht als vollständiger Nachweis unserer geplanten Funktionen untersucht |

Es wird weder behauptet, dass kein anderes Werkzeug diese Kombination implementiert, noch dass jeder Nutzer ein neues Projekt braucht. Der zusätzliche Nutzen muss an Phase 2 und Phase 4 praktisch sichtbar werden.

## Was im Mainline-Quellcode auffällt

Der eingesehene [Kernelverwaltungscode](https://github.com/bkw777/mainline/blob/master/src/Common/LinuxKernel.vala) arbeitet mit vorgebauten Paketangeboten, Versions-/Architekturzuordnung und installierten Paketen. Paketidentität und Zuordnung sind relevante Probleme: Der Code behandelt unter anderem unterschiedliche Datumsanteile gleicher Mainline-Basisversionen.

Daraus folgt für unseren Entwurf: Quelle, Rezept, Kernelrelease und Paketinhalt brauchen eindeutige eigene Identitäten. Eine reine Versionszeichenfolge genügt weder für Reproduktion noch für sichere Entfernung. Ein Paket mit gleicher äußerer Kennung und verändertem Inhalt darf nicht still ersetzt werden.

Diese Beobachtung ist keine Bewertung der allgemeinen Sicherheit oder Qualität von Mainline. Sein Schwerpunkt liegt auf der Verwaltung vorhandener Ubuntu-Builds, während unser Schwerpunkt deren lokale Entstehung einschließt.

## Lizenz und Übernahme

Mainline enthält die [GNU GPL Version 3](https://github.com/bkw777/mainline/blob/master/LICENSE). Ein Fork wäre deshalb kein lizenzfreier Ausgangspunkt. Vor einer konkreten Codeübernahme wären Dateiheader, Copyrightvermerke und Lizenzpflichten der tatsächlich übernommenen Dateien zu prüfen; aus der zentralen Lizenzdatei allein wird keine ungeprüfte Aussage über jede Drittdatei abgeleitet.

Für MyMainline ist GPL-3.0-only als Default für zukünftigen Anwendungscode geplant. Bisher wurde kein Anwendungscode von Mainline, linux-tkg oder TuxMake kopiert. Falls später Vorlagen, Helfer oder Code übernommen werden, gehören Herkunft, festgeschriebener Stand, Lizenz und nötige Hinweise zum Lieferumfang.

## Fork oder unabhängiges Projekt?

| Weg | Vorteil | Aufwand und Risiko |
| --- | --- | --- |
| Mainline forken | Vorhandene Vala-/GTK-Oberfläche, Inventar und Nutzerabläufe | Eigene Buildpipeline, sichere Rollentrennung, mehrere Distributionen und weitere Toolkits verändern den Kern erheblich |
| Eigenständig entwickeln | Architektur und Vertrauensgrenzen direkt auf die vollständige Aufgabe ausrichten | Inventar, Paket-/Bootintegration, Oberflächen und langfristige Wartung selbst verantworten |
| Vorhandenen Builder umgeben | Buildwissen wiederverwenden | Hosteingriffe, Eingaben, Versionierung, Ausgaben und Fehlermodelle müssten trotzdem sicher integriert werden |

**Entscheidung:** eigenständiger Rust-Dienst, direkte offizielle Kernelverfahren, Referenznutzung vorhandener Projekte. Das reduziert die Architekturabweichung eines Forks, aber nicht die Gesamtverantwortung. Siehe [ADR 0001](../adr/0001-independent-project.md).

Im eingesehenen [linux-tkg-Installationspfad](https://github.com/Frogging-Family/linux-tkg/blob/master/install.sh) gibt es distributionsabhängige Host-Dependency-Installationen. Die spätere Kernelinstallationsabfrage ist daher kein Beleg dafür, dass vorher keinerlei Hoständerung erfolgt. Ein Vanilla-Modus allein löst außerdem nicht sämtliche Konfigurations-/Vertrauensfragen. Diese konkreten Abläufe werden nicht unbesehen als Backend übernommen.
