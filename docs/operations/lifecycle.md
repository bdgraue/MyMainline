# Betrieb nach dem ersten Build

Stand: 10. Oktober 2026. **Verbindlicher Planungsentwurf; Verfahren und Prüfungen sind noch nicht umgesetzt.** Dies ergänzt den [Plan](../PLAN.md). Es ist keine Anleitung für Änderungen am produktiven System.

## 1. Ersten Rückfallnachweis aufbauen

Ein laufendes Kernelrelease beweist nicht, dass die jetzt auf Datenträger vorhandenen Module und das initramfs beim Start verwendet wurden. Paket- oder Treiberupdates können diese Dateien seitdem verändert haben.

Vor dem Bestätigungsboot wird deshalb ein geschützter Bootdateisatz erfasst: eindeutige Paketversionen, Kernelimage, Modulbestand einschließlich erforderlicher DKMS-Module, initramfs, öffentlicher Signierbezug und der zugehörige Bootloadereintrag. Er enthält Dateihashes und die zur Zuordnung erforderliche Bootkonfiguration. Nach einem neuen Boot müssen erwartetes Release, neue Bootkennung und unveränderter Dateisatz zusammenpassen. Erst die anschließende Benutzerbestätigung wesentlicher Funktionen erzeugt einen KnownGoodKernel.

Ohne vertrauenswürdige Erfassung vor dem aktuellen Boot bleibt der laufende Kernel ein Kandidat. Bei der Ersteinrichtung ist dann ein ausdrücklich vom Benutzer ausgeführter Bestätigungsboot über den bestehenden, geprüften Bootpfad erforderlich. Dafür braucht es noch keinen MyMainline-Testbootplan und keine neue Kernelinstallation. Ein vorhandener gültiger Nachweis kann nach erneuter Zustandsprüfung weiterverwendet werden.

Reine Änderungen der Menüposition oder des Defaults entwerten einen Funktionsnachweis nicht, sofern der zugeordnete Kernel mit unverändertem Bootdateisatz weiterhin eindeutig erreichbar bleibt. Sie entwerten gegebenenfalls vorbereitete Systempläne. Änderungen am wirksamen Booteintrag oder seinen Bootparametern sind dagegen erneut prüfpflichtig.

Die Beobachtung ist ein betrieblicher Funktionsnachweis, keine kryptografische Attestierung des im RAM geladenen Images. Geänderte relevante Dateien entwerten den betroffenen Nachweis und darauf beruhende Pläne. Inventarisierung, Build, Export und gezielte Reparatur bleiben möglich; ein fehlender Nachweis darf die Reparatur nicht zirkulär verhindern.

## 2. Spätere Treiber- und DKMS-Updates

Die Installation ist erst vertretbar, wenn auch der Lebenszyklus späterer Module geklärt ist. MyMainline führt für jeden verwalteten Kernel die endgültigen Header, erforderlichen Module, Hashes, Signierzuordnung und initramfs-Abhängigkeiten nach.

Ein späteres Treiberupdate, etwa NVIDIA oder VirtualBox, kann den bisherigen Modulbestand und das initramfs ersetzen. Betroffene Pläne und Funktionsnachweise werden ungültig. Ein separat freizugebender Wartungsplan bindet Kernel, konkrete Modulquellen und Buildinputs, Ergebnisse, Signierauftrag sowie die geplante initramfs-Aktualisierung. Die bestehenden Grenzen für unvertrauenswürdige Modulbuilds, geprüfte Artefakte und rootgeschützte Übergaben gelten auch hier. Nur der zu genau diesem Kernel gehörende Schlüssel darf über den isolierten Signierer verwendet werden. Private Schlüssel gelangen weder an den Buildworker noch an gewöhnliche DKMS-Aufrufe. Es gibt keine pauschale Freigabe künftiger Signieraufträge.

Nachkontrollen prüfen Header-/Releasezuordnung, sämtliche erforderlichen Module, Signaturen und das tatsächlich erzeugte initramfs. Ein neuer Funktionsnachweis verlangt wieder einen passenden Bestätigungsboot. Kein ungefragtes Laden von Modulen, Abschalten erzwungener Signaturprüfung oder Neustarten.

Eigene Wartung darf nicht alle bestätigten Rückfallmöglichkeiten zugleich verändern: Ein unveränderter erreichbarer KnownGoodKernel bleibt erhalten, bis der gewartete Kandidat erneut bestätigt ist. Hat ein externer Vorgang bereits sämtliche Nachweise entwertet, wird das als konkreter Reparaturbedarf angezeigt. Die erneute manuelle Erstbestätigung bleibt möglich; ein Dienststart oder Hashabgleich allein ersetzt sie nicht.

Externe APT-/DKMS-Vorgänge laufen bereits mit administrativen Rechten und außerhalb einer MyMainline-Freigabe. MyMainline kann sie nicht generell verhindern. Es erkennt ihre Auswirkungen beim Zustandsabgleich und meldet erforderliche Wartung. Eine kooperative Hookintegration darf keinen privilegierten allgemeinen Signierdienst eröffnen und keine Paketmanagersperre rekursiv anfordern. Die technische Machbarkeit des Ablaufs zwischen externem Update, isoliertem Modulbuild und separater Freigabe ist **Phase-0-Machbarkeitsblocker F-09**. Der isolierte Versuch nutzt ausdrücklich manuelle Freigabe; echte SystemPlan-/polkit-Autorisierung und Brokerintegration werden später mit W-01 in Phase 4 nachgewiesen. Solange dieser Ablauf für ein DKMS-Profil nicht sicher nachgewiesen ist, bleibt dessen Installation gesperrt.

Schlüssel bleiben erhalten, solange installierte Kernel oder zulässige Wartungsvorgänge darauf angewiesen sind. Fehlende Schlüssel oder Signierer erzeugen einen konkreten Fehler. Die dokumentierten DKMS-Funktionen allein beweisen die geplante Integration nicht: [Debian DKMS-Handbuch](https://manpages.debian.org/trixie/dkms/dkms.8.en.html), [Kernel-Modulsignierung](https://docs.kernel.org/admin-guide/module-signing.html).

## 3. Speicherbedarf und Aufbewahrung

Vor Download, Build, Installation und Wartung werden freie Bytes, Inodes und wirksame Quoten auf den tatsächlich betroffenen Dateisystemen geprüft. Dazu gehören Arbeits-/Downloadbereiche, Artefaktablage, Paketdatenbank, Journal, /boot und eine ESP, falls die konkrete Aktion dort schreibt. Getrennte Pfade auf demselben Dateisystem teilen sich dasselbe Budget.

Die Planung berücksichtigt Spitzenbedarf, gleichzeitige Jobs, Benutzerbudgets, alte und neue Kernel-/initramfs-Dateien sowie eine Reserve für Journal und Reparatur. Interne Reservierungen verhindern Überbuchung durch eigene Jobs; fremde Schreibvorgänge können dennoch Speicher verbrauchen. Unzureichende oder nicht zuverlässig messbare Kapazität blockiert den jeweiligen verändernden Schritt. Tatsächliche Schreibfehler bleiben möglich und führen zur konservativen Recovery.

Konkrete Standardgrenzen werden in Phase 0 aus Referenzbuilds beider Distributionen und Standardprofile abgeleitet und versioniert. Vor Abnahme des Build-MVP sind sie festzulegen; dieser Plan behauptet keinen universellen Gigabytebedarf.

Aufbewahrung trennt temporäre Arbeitsdaten, wiederbeschaffbaren Cache, archivierte Reproduktionsinputs und System-/Rettungsnachweise. Installierte, laufende, Standard-, Rückfall- und Testkernel, aktive Jobs, Schlüsselreferenzen und zugesagte Reproduktionsarchive sind geschützt. Automatische Bereinigung nach ausdrücklich konfigurierter Politik darf nur unreferenzierte temporäre oder nachweislich wiederbeschaffbare Cachedaten entfernen. Das Etikett „Cache“ allein reicht nicht: Ist eine Beschaffung nicht mehr gesichert, bleibt eine separate Löschfreigabe nötig.

Archivlöschung braucht eine konkrete Auswahl und Folgenanzeige. Werden erforderliche Eingaben gelöscht, bleibt ein historischer Reproduktionsbericht erhalten, aber der Status „vollständige Eingaben für erneute Reproduktion vorhanden“ entfällt. Installierte Kernel, benötigte Schlüssel und Rettungsnachweise werden nicht durch Cachebereinigung entfernt.

## 4. Supportstatus und Distributionsreserve

Ein Releasekanal ist kein Supportnachweis. Der Katalog zeigt getrennt den Status aktiv gepflegt, Pflegeende bekannt oder unbekannt sowie Quelle und Prüfzeitpunkt. Abwesenheit aus releases.json beweist kein Pflegeende. Maßgeblich sind nachvollziehbare offizielle Release-/Pflegeangaben; die Aktualitätsgrenze und der Umgang mit widersprüchlichen Angaben werden vor Katalogabnahme festgeschrieben. Ein veralteter oder nicht abrufbarer Status wird nicht als aktiv gepflegt ausgegeben.

**F1 – vereinbart:** Eine neue Installation aus einer ungepflegten Reihe benötigt eine zusätzliche ausdrückliche Ausnahme für den konkreten Systemplan und dessen Supportbeobachtung. Bei unbekanntem Status ist eine ebenso konkrete Zustimmung zur Unsicherheit erforderlich. Diese Ausnahmen umgehen weder Signaturprüfung noch Plattform-, Sicherheits- oder Rückfallbedingungen. Ein bereits vorhandener Rückfallkernel bleibt auch bei späterem Pflegeende geschützt und erreichbar; es gibt keine automatische Entfernung oder Umschaltung. Eine Signatur oder die neueste Versionsnummer ist keine Zusage der Freiheit von Sicherheitslücken. [Offizielle Release-/Pflegeübersicht](https://www.kernel.org/category/releases.html).

**F2 – vereinbart:** Zusätzlich zu eigenen Kerneln wird mindestens ein vorhandener Distributionskernel einschließlich zugehörigem Aktualisierungsmetapaket dauerhaft als Reserve erhalten. Er ist erst nach eigenem Boot-/Funktionsnachweis ein Rückfallkernel. Fehlt er, wird die Lücke sichtbar; Build und Export bleiben möglich, eine neue verwaltete Kernelinstallation ist bis zur Wiederherstellung der Reserve gesperrt. Die Beschaffung fehlender Distributionspakete ist eine eigene ausdrücklich freizugebende Aktion, keine versteckte Abhängigkeitsauflösung innerhalb einer Kernelinstallation.

Eindeutig zugeordnete Reservepakete und das Aktualisierungsmetapaket werden über einen eigenen konkreten Plan gegen gewöhnliches APT-Autoremove geschützt, indem sie als manuell installiert markiert werden. Kein pauschales Hold, das Distributionsupdates unterbindet. Neue Distributionskernel dürfen regulär hinzukommen; die geschützte Reserve wird nicht ungefragt entfernt. Eine ausdrücklich von einem Administrator ausgeführte fremde Entfernung liegt außerhalb dieser Schutzgarantie. [apt-mark: manual und hold](https://manpages.debian.org/trixie/apt/apt-mark.8.en.html).

Eine externe Änderung entwertet einen Reservenachweis nur, wenn sie dessen relevanten Dateisatz oder Bootzuordnung tatsächlich verändert. Eine reine Metapaketaktualisierung oder ein zusätzlich installierter anderer Distributionskernel genügt dafür nicht. Der Paketschutz der Reserve bleibt auch bei entwertetem Nachweis erhalten. Fehlt dann jeder gültige KnownGoodKernel, nennt der CapabilityReport dies als Sperrgrund für neue Installation und Testboot. Erneute Vorab-Erfassung plus Bestätigungsboot stellen den Nachweis wieder her. Bleibt ein anderer gültiger KnownGoodKernel erreichbar, darf dieser den Rückfall sichern; eine vorhandene unbestätigte Distributionsreserve ist daneben weiterhin zulässig.

## 5. Rettung ohne Anwendung

Das geschützte Vorgangsjournal und ein versioniertes Rettungsmanifest liegen außerhalb der SQLite-Datenbank. Sie benennen Paket-/Dateiidentitäten, Hashes, relevante Bootkonfiguration, bisherigen sicheren Einstieg, beabsichtigte Änderungen und protokollierte Ergebnisse. Sie müssen mit eigenständigen lesenden Werkzeugen aus einem Rettungssystem auswertbar sein. Sie sind Beweismittel für einen Zustandsabgleich, keine Anweisung zum blinden Zurückkopieren.

Der [Rettungsentwurf](rescue.md) beschreibt den unabhängigen Ablauf. Private Schlüssel gehören nicht in normale Exporte. Ihre Sicherung verlangt ein eigenes ausdrücklich gewähltes geschütztes Verfahren. Beschädigte Datenbank, fehlender Dienst oder fehlendes D-Bus dürfen einen Rettungsversuch nicht technisch vom Start der Anwendung abhängig machen.

## 6. Aktualisierung und Entfernung von MyMainline

Datenbankschema, Journal-/Manifestformat und Dienstprotokolle werden versioniert. Unbekannte oder inkompatible Versionen blockieren Änderungen mit verständlichem Fehler. Ein Broker-/Policyupdate entwertet davon betroffene vorbereitete Pläne; alte Clients dürfen keine veralteten Freigaben ausführen.

Eine Migration stoppt die Annahme neuer Systemänderungen und wartet auf einen ruhenden, gesperrt überprüften Transaktionszustand. Laufende Executor-/Paketoperationen werden weder getötet noch dupliziert. Vor Migration stehen eine dauerhafte Sicherung und ein nachweisbarer Wiederherstellungspfad. Bei Downgrade oder Sicherungsrückspielung ist der tatsächliche Paket-/Bootzustand erneut abzugleichen; ein Datenbankbackup allein setzt keine Systemänderungen zurück.

Anwendungsupdates erfolgen über den freigegebenen Distributionspaketweg. Paketentfernung und Purge dürfen nicht automatisch installierte Kernel, zugehörige Bootmöglichkeiten, noch benötigte Schlüssel oder Rettungsnachweise löschen. Persistente Eigentümer- und Zugriffsschutzregeln müssen auch nach Entfernung oder späterer Wiedervergabe einer Dienstkennung sicher bleiben.

Ohne Signierer sind neue Modulsignaturen möglicherweise nicht mehr erzeugbar; diese Folge wird vor Entfernung angezeigt. Vor Freigabe der Anwendungspakete müssen Wiederinstallation, kompatible Wiederherstellung und unabhängige Rettung praktisch nachgewiesen sein. Ein vollständiges Löschen verbliebener Zustandsdaten ist eine eigene konkrete Aktion mit Referenzprüfung, keine Nebenwirkung von Purge.

## Abnahme

Alle Anforderungen sind durch [B-01, W-01, S-01, R-01, P-01 und A-01](../validation.md) nachzuweisen. S-01 teilt sich in Build-/Cacheprüfungen S-01a bis Build-MVP und Systemprüfungen S-01b bis CLI-Verwaltungs-MVP. Sie sind derzeit offen. Ein Dokumentationsreview ersetzt weder diese Nachweise noch das unabhängige Architekturreview.
