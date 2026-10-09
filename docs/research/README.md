# Recherche und Herleitung

Stand: 9. Oktober 2026. Die Recherche begründet den [Plan](../PLAN.md). Sie ist eine Dokumentations- und Quellcodeanalyse, kein ausgeführter Sicherheits-, Build- oder Bootnachweis.

## Lesepfad

| Dokument | Fragestellung |
| --- | --- |
| [Alternativen und Mainline](alternatives.md) | Gibt es den Anwendungsfall bereits? Fork oder Neuentwicklung? |
| [Buildverfahren und Distributionen](build-platform.md) | Welche offiziellen Verfahren sind verwendbar, welche Grenzen bleiben? |
| [Beobachtungen der Ausgangslage](observations.md) | Welche lokal lesend erkannten Stolpersteine beeinflussten den Entwurf? |
| [Quellenverzeichnis](sources.md) | Welche Primärquellen stützen die Feststellungen? |
| [Entscheidungsprotokolle](../adr/README.md) | Welche Entscheidung folgt aus welchem Befund? |

## Evidenz und Grenzen

**Belegt** bedeutet: in offizieller Dokumentation oder eingesehenem Quellcode vorhanden. **Beobachtet** bezeichnet eine lesende Momentaufnahme der Ausgangsumgebung. **Entschieden** bezeichnet eine Projektwahl. **Offen** verlangt erst noch einen praktischen Nachweis.

Die Analyse von Mainline betrifft den eingesehenen Quellstand und die GPLv3-Lizenz des Projekts. Die ursprünglich verwendete Version 1.4.15 beschreibt die Ausgangslage; Aussagen über aktuelle Quellpfade sind kein vollständiger Audit exakt dieses installierten Binärpakets.

Links auf `master`, `main`, `latest` oder laufende Handbücher sind veränderlich. Sie belegen die Planung am Recherchedatum, aber keine Kompatibilität eines bestimmten zukünftigen Kernelreleases. Phase 0 muss Quellrelease, Git-Commit beziehungsweise Archivhash, Toolchain, Bibliotheken und Zertifikatsherkunft festschreiben. Es werden keine ungesicherten Commit-IDs nachgetragen.

Es wurde kein bestehendes Werkzeug vollständig sicherheitsgeprüft. Insbesondere beweist das Fehlen einer Funktion in einer eingesehenen Anleitung nicht, dass das gesamte Projekt sie niemals bietet. Die Abgrenzung lautet deshalb: Die vollständige Kombination unserer Anforderungen ist in dieser Recherche nicht nachgewiesen.

## Ergebnis

Ein realer Anwendungsfall bleibt: signierte unveränderte Upstream-Quellen unabhängig von verzögerten vorgebauten Paketen als nachvollziehbare lokale Pakete erstellen und den gesamten Weg bis zu kontrolliertem Testboot und Recovery verwalten.

Dieser Nutzen richtet sich an Anwender mit konkretem Versions-/Hardwarebedarf oder Interesse an nachvollziehbaren Builds. Wer nur einen gepflegten Desktopkernel möchte, kann mit einem Distributionskernel oder vorhandenen alternativen Paketen weniger Aufwand haben. Ein universeller Ersatz für Distributionskernels wird nicht versprochen.
