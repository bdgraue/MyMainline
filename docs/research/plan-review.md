# Ergänzende Prüfung des Plans

Stand: 10. Oktober 2026. Eigene fachliche Prüfung des veröffentlichten Entwurfs; die folgenden Empfehlungen wurden übernommen. Dies ist **kein unabhängiges Architektur- oder Sicherheitsgutachten** und kein praktischer Nachweis.

## Befunde und übernommene Konsequenzen

| Befund | Konsequenz | Spezifikation | Geplanter Nachweis |
| --- | --- | --- | --- |
| Der laufende Kernel kann zu inzwischen ersetzten Dateien gehören | Geschützter Dateisatz vor Bestätigungsboot; keine Erstbestätigung allein aus aktuellem Release | Plan 4.3; Lebenszyklus 1; ADR 0006 | B-01 |
| Die erste erfolgreiche DKMS-Installation deckt spätere Treiberupdates nicht ab | Separat freigegebene Wartung, kernelgebundener Signierer, Zustandsabgleich externer Updates | Plan 4.4; Lebenszyklus 2; ADR 0007 | F-09, W-01 |
| Freier Gesamtspeicher sagt wenig über /boot, Inodes oder parallele Jobs aus | Dateisystembezogene Vorprüfung und interne Budgets; Schutz referenzierter Daten | Plan 3.7; Lebenszyklus 3 | F-10, S-01 |
| Ein defekter Dienst oder eine defekte Datenbank darf den Rettungspfad nicht mitnehmen | Versionierte Nachweise außerhalb SQLite; eigenständige lesende Auswertung und Live-Rettung | Plan 4.8; Rettungsentwurf; ADR 0012 | R-01 |
| Releaseherkunft und Pflegezustand sind unterschiedliche Fragen; eine Distributionsreserve fehlt als eigene Rolle | Planbezogene Ausnahmen bei Pflegeende/Unsicherheit; Distributionskernel plus Aktualisierungsmetapaket erhalten | Plan 4.7; Lebenszyklus 4; ADR 0011 | P-01 |
| Anwendungsupgrade, Downgrade und Purge können die Schutzmechanismen selbst entfernen | Ruhende Migrationsgrenze; Versionsverträge; benötigte Schlüssel und Rettungsdaten erhalten | Plan 4.9; Lebenszyklus 6; ADR 0012 | A-01 |

Die beiden Produktentscheidungen sind damit festgelegt: **F1** verlangt eine zusätzliche konkrete Ausnahme für neue Installationen ungepflegter Reihen. **F2** erhält einen Distributionskernel einschließlich Aktualisierungsmetapaket. Eine installierte Reserve ist ohne Boot-/Funktionsnachweis noch kein KnownGoodKernel.

## Herleitung und Grenzen

Die dokumentierten DKMS-Verfahren beschreiben Modulbau und Signierung, beweisen aber keine sichere Integration unseres getrennten Signierers in spätere APT-Updates. Das bleibt ein Machbarkeitsblocker. Das APT-Handbuch unterscheidet Schutz vor gewöhnlichem Autoremove durch manuelle Markierung von einem Hold; der Plan wählt die erste Möglichkeit, damit Distributionsupdates weiterlaufen können.

Offizielle Releaseangaben unterscheiden Stable und Longterm und führen veränderliche Pflegezeiträume. Deshalb wird Pflegezustand separat mit Quelle und Zeit geführt; fehlende Katalogeinträge werden nicht zu einer erfundenen EOL-Aussage.

Quellen: [DKMS](https://manpages.debian.org/trixie/dkms/dkms.8.en.html), [apt-mark](https://manpages.debian.org/trixie/apt/apt-mark.8.en.html), [Kernel-Releases](https://www.kernel.org/category/releases.html), [Modulsignierung](https://docs.kernel.org/admin-guide/module-signing.html).

Die konkreten Regeln zur Vorab-Erfassung, Speicherbudgetierung, unabhängigen Rettung und Migration sind Projektentscheidungen. Ihre Machbarkeit und Vollständigkeit müssen [Validierung](../validation.md) und ein unabhängiges Architekturreview erst belegen. Ein späteres gezieltes Dokumentationsreview dieser Ergänzungen ersetzt dieses Architekturreview nicht.

## Navigation

[Vollständiger Plan](../PLAN.md) · [Betrieb und Lebenszyklus](../operations/lifecycle.md) · [Rettungsentwurf](../operations/rescue.md) · [Roadmap](../ROADMAP.md) · [Offene Nachweise](../risks.md).
