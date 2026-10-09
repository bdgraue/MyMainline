# Offene Nachweise, Entscheidungen und Risiken

Stand: 9. Oktober 2026. Der Entwurf ist vereinbart, seine Machbarkeit noch nicht bewiesen. Keine Zeile dieser Tabelle ist ein bereits erledigter Test.

## Vor Umsetzung beziehungsweise Freigabe zu klären

| Thema | Offen | Bedingung für den nächsten Schritt |
| --- | --- | --- |
| Referenzstände | Konkretes Kernelrelease, Toolchain, Rust-Mindestversion und Bibliothekskombination | In Phase 0 anhand echter Quellen festschreiben; keine geratenen Versionen |
| Distributionszertifikate | Authentifizierte Beschaffung, Format und tatsächlich erhaltene Upstream-Semantik | Nachweis für beide Distributionen; andernfalls Installation blockieren |
| Vertrauenswürdige Headeraufbereitung | Vollständige Menge benötigter Skripte/Helfer und Umgang mit generierten Daten | Kontrollierte Rekonstruktion plus praktischer externer Modulbuild |
| Isolation | Verfügbarkeit verpflichtender Namespaces und ressourcenbegrenzter unprivilegierter Ausführung | Echte Negativtests; kein Hostbuild-Fallback |
| Signierer und DKMS | Kontrollierte Signierzuordnung sowie spätere Module bei erzwungener Prüfung | Ende-zu-Ende-Test einschließlich Fehler-/Schlüsselreferenzfällen |
| Paket-/Boot-Recovery | Persistenzgrenzen, Sperrverhalten, GRUB-Umgebungsspeicher und tatsächliche Resetfolgen | VM-Nachweise, bevor Installationspfade freigegeben werden |
| Reproduzierbarkeit | Bytegleichheit einschließlich Headerhelfern und finalen Paketmetadaten | Zwei frische Builds mit archivierten öffentlichen Signaturinputs |
| Vier Clients | Toolkit-/Rust-Kompatibilität, Paketierung, Pflege-/Barrierefreiheitsaufwand | Phase-0-Brückenversuche und gemeinsame Vertragsabnahme |
| Lizenzierung | Vollständige Lizenztexte, Drittkomponenten, eventuelle spätere Übernahmen | Vor Veröffentlichung des jeweiligen Anwendungscodes prüfen und dokumentieren |
| Architekturreview | Unabhängige Prüfung steht aus | Kritische Befunde vor privilegierter Umsetzung klären |

## Bewusst akzeptierte Grenzen des Entwurfs

Ein signierter Upstream-Kernel kann Fehler haben und Distributionsfunktionen vermissen. Eine Sandbox begrenzt den Buildprozess; ein später als Root laufender Kernel gehört selbst zur Vertrauensbasis. Reproduktion schafft zusätzliche Evidenz, beweist aber keine Fehlerfreiheit der Toolchain.

Das Transaktionsjournal macht dpkg nicht atomar. Bei Stromausfall, defektem Dateisystem oder externen administrativen Eingriffen kann manuelle Reparatur erforderlich bleiben. MyMainline darf einen ungeklärten Zustand nicht als erfolgreich ausgeben.

Ein bestätigter Rückfallkernel begrenzt das Risiko, ersetzt aber weder Datensicherung noch einen erreichbaren Rettungspfad. Seine Zuordnung und Dateien werden erneut geprüft; externe Paketmanager können Änderungen außerhalb der Anwendung vornehmen.

Vier Clients sind ein ausdrückliches Ziel mit zusätzlichem Aufwand. Gemeinsame Logik begrenzt fachliche Duplizierung, beseitigt aber keine toolkitabhängigen Tests. Build-MVP und CLI-Verwaltungs-MVP liefern deshalb bereits vor v0.1 überprüfbaren Nutzen.

## Spätere Produktentscheidungen

Vollständiges Secure Boot, RC-Builds, Fedora, Arch, weitere Speicherlayouts/Bootloader und CPU-/Treiber-Spezialprofile haben noch keine Installationsfreigabe. Für jede Erweiterung sind eigener Umfang, Trust-/Recovery-Folgen und Abnahmen festzulegen. Diese offenen Erweiterungen blockieren nicht die Dokumentationsveröffentlichung und dürfen den v0.1-Umfang nicht unbemerkt erweitern.

Siehe [Roadmap](ROADMAP.md), [Validierung](validation.md) und [Entscheidungen](adr/README.md).
