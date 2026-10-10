# Validierung und Freigabekriterien

Stand: 10. Oktober 2026. **Alle hier beschriebenen Anwendungs-, Kernel- und VM-Prüfungen stehen aus.** Dokumentationsprüfung ist kein Nachweis der korrekten Implementierung. Der [Plan](PLAN.md) legt Umfang und Regeln fest.

## Regeln für die Nachweise

Tests mit Installation, kaputten Paketen, Bootänderungen und absichtlichen Abstürzen laufen ausschließlich in Wegwerf-VMs. Produktive Hostpfade, private Signierschlüssel und privilegierte Infrastruktur sind für unvertrauenswürdige Beiträge unzugänglich. Ein fehlendes Testwerkzeug wird als fehlender Nachweis gemeldet.

Zu jedem Ergebnis gehören festgeschriebene Eingaben, Umgebung, erwartetes und tatsächliches Verhalten sowie Logs und Artefakthashes. Ausgelassen, fehlgeschlagen und bestanden sind unterschiedliche Zustände. Ein repräsentativer Erfolg darf nicht als Nachweis aller Rezepte oder Plattformen ausgegeben werden.

## Phase 0: Machbarkeit vor Umsetzung

| Kennung | Nachweis | Erfolgskriterium |
| --- | --- | --- |
| F-01 | Ubuntu- und Debian-Rootfs aus authentifizierten Archiven | Festgeschriebene Paket-/Toolchainidentitäten, archivierte Eingaben und ausführbarer unprivilegierter Modus |
| F-02 | Pflichtisolation und Ressourcensteuerung | Netzwerk, Bootpfade, Hostmodule, Paketdatenbank, Benutzerdateien, Schlüssel und Container-Sockets unerreichbar; fehlende Fähigkeit blockiert |
| F-03 | Distributionskonfiguration und öffentliche Zertifikate | Herkunft nachvollziehbar; keine geleerten Trust-/Revocation-Werte; nicht erhaltbare Anforderungen sichtbar gesperrt |
| F-04 | Header und getrennte Paketierung | Vertrauenswürdige Rekonstruktion ausführbarer Headerlogik, externes Testmodul mit Symbolversionen baubar |
| F-05 | Getrennte Modulsignierung | Kein privater Schlüssel im Worker; gültige Module; öffentliche Signaturwiedergabe ohne Geheimnis |
| F-06 | Reproduzierbare Pakete | Zwei frische Builds je Standardprofil/Zieldistribution liefern Bytegleichheit im vereinbarten Umfang |
| F-07 | Qt-/GTK-/Dienstkombination | Gewählte Rust-, Toolkit-, CXX-Qt-, gtk-rs- und D-Bus-Versionen auf beiden Zielsystemen geprüft |
| F-08 | Unabhängiges Architekturreview | Externe Prüfung von Vertrauensgrenzen, Rootpfaden, Recovery, Schlüsseln und Rückfallmodell; kritische Befunde vor privilegierter Umsetzung geklärt |
| F-09 | Isolierter Verfahrensnachweis späterer DKMS-/Treiberupdates | Phase-0-Versuch in Wegwerf-VMs beider Distributionen bei erzwungener Modulsignaturprüfung: Update, isolierter Build, manuell ausdrücklich freigegebener Schritt, Signierung und initramfs-Prüfung ohne private Schlüssel in DKMS oder rekursive Paketsperren. Noch kein Nachweis von SystemPlan/polkit; deren Ende-zu-Ende-Prüfung folgt mit W-01 in Phase 4. Fehlschlag sperrt betroffene Installation |
| F-10 | Speicher- und Aufbewahrungsmodell | Gemessener Spitzenbedarf beider Standardprofile/Distributionen; festgeschriebene Budgets für Bytes/Inodes/Quoten und Reparaturreserve; überprüfbarer Referenzschutz |

## Sicherheits- und Vertragstests

| Bereich | Negative und positive Fälle | Erwartetes Verhalten |
| --- | --- | --- |
| Release/Download | Fehlerhafter Katalog, unerlaubter Redirect, Teilantwort, Größenlimit, veralteter Katalog | Expliziter Fehler oder gekennzeichnete Aktualitätsunsicherheit; keine erfundenen Releases |
| Signatur | Falsche/fehlende Signatur, unbekannter/widerrufener Fingerprint, Austausch von Archiv oder Schlüssel | Keine Extraktion/Buildfreigabe |
| Extraktion | Traversal, absolute Pfade, Symlink-/Hardlinkausbruch, Spezialdateien, Dekompressionsbombe | Ablehnung ohne Schreiben außerhalb des Jobbereichs |
| Konfiguration | Neue/fehlende Symbole, Kconfig-Abhängigkeiten, LSM-Reihenfolge, verlorene Härtung, fehlende Zertifikate, RT-Konflikt | Vollständiger Bericht; Sicherheits-/Bootkonflikt blockiert; kein stilles Abschalten |
| Worker | Zugriffsversuche auf Netzwerk, Host-/Dienstzustand, Artefaktablage und Signierer | Technische Verweigerung; kein unisolierter Ersatzpfad |
| Artefakte | Manipulierte Maintainer-Skripte, Headerprogramme, Metadaten, Dateitypen, Links und Paketidentitäten | Nur rekonstruierte erlaubte Bestandteile werden freigegeben |
| Übergabe | Nachträglicher Austausch, Rennen bei Pfad-/Hashprüfung, fremder Prozess als Sealer | Broker lehnt ab; Installation nur aus rootgeschützter erneut geprüfter Kopie |
| Signierer | Fremdes Rezept, fremdes Modul, Worker-Helfer, nicht erlaubter Signierauftrag | Ablehnung; Schlüssel bleiben geheim und außerhalb des Workers |
| Autorisierung | Behauptete UID, fremder D-Bus-Sender, fremde Jobs/Logs, abgelaufener/wiederverwendeter/geänderter Plan | Keine unberechtigte Aktion oder Einsicht; keine pauschale/cachende Freigabe |
| Terminal | Desktop, lokales Terminal, SSH mit Terminal, Sitzung ohne Terminal | Agentregistrierung vor Anfrage; sonst `AuthenticationRequired`; kein stilles sudo |
| Konkurrenz | Externer Paketmanager, wiederholter Auftrag, Clientabbruch, Dienstneustart | Verträgliche Sperren; höchstens ein Executor; bestehender Vorgang wird wiedergefunden |
| Recovery | Disk-full, Paket-/Hookfehler, unterbrochene Schreibschritte, widersprüchliches Journal | Tatsächlichen Zustand abgleichen; `RepairRequired`; keine blinde Wiederholung/Entfernung |
| Inventar | Gleiche Versionsbasis, unklare Paketzuordnung, externe Änderung, fehlende Dateien | Keine falsche Identität/Entfernung; betroffene Pläne/Nachweise ungültig |
| Boot | Veraltete Beobachtung, falscher Kernel, fremder Einmalstart, numerischer Default nach Installation | Keine falsche Bestätigung, kein automatisches erneutes Scharfschalten; sicherer Standard bleibt |
| Automatisierung | Neue Serie, Sicherheitsdiff, deaktivierte Builds, Benutzerabmeldung | Neue Freigabe wo erforderlich; kein automatisches Installieren/Umschalten/Neustarten |
| Clients | Derselbe Auftrag in vier Clients; Wechsel, Abbruch und Wiederverbindung | Gleiche Zustände, Sperrgründe und Operationen; Jobs überleben Clientende |

## VM-Matrix

| Ziel | BIOS | UEFI, Secure Boot aus | Secure Boot an/unklar |
| --- | --- | --- | --- |
| Ubuntu/Kubuntu 26.04, amd64, GRUB/initramfs-tools, freigegebenes ext4-Layout | Vollständige Ablauf-/Fehlermatrix | Vollständige Ablauf-/Fehlermatrix | Installationssperre; Quellen/Build/Export separat prüfbar |
| Debian 13, amd64, GRUB/initramfs-tools, freigegebenes ext4-Layout | Vollständige Ablauf-/Fehlermatrix | Vollständige Ablauf-/Fehlermatrix | Installationssperre; Quellen/Build/Export separat prüfbar |
| LUKS, LVM, RAID, Btrfs-Boot, UKI oder anderer Bootloader | Installationssperre | Installationssperre | Installationssperre |

Die Tabelle beschreibt geplante Tests, keine bereits freigegebenen Plattformen. BIOS hat keinen UEFI-Secure-Boot-Zustand; das wird als nicht anwendbar und nicht als unbekannt behandelt.

## Ablauf je freigegebener VM-Konstellation

1. Laufenden Kandidaten eindeutig zuordnen und Bootdateisatz vor einem Bestätigungsboot erfassen. Danach neuen Boot, unveränderte Dateien und wesentliche Funktionen bestätigen; erst dann `KnownGoodKernel` erzeugen.
2. Abweichenden Bootstandard zunächst durch einen separaten Plan normalisieren.
3. Verifizierte Pakete mit endgültigen Headern erzeugen; DKMS und externes Testmodul vorbereitend prüfen.
4. Alten Standard vor Installationshooks stabil festlegen; sichtbares Rettungsmenü herstellen.
5. Nur konkrete autorisierte lokale Pakete installieren; fehlende Abhängigkeiten blockieren.
6. Paketstatus, Image, Module, Signaturen, erforderliches DKMS, initramfs und Booteinträge nachprüfen.
7. Einmalstart separat freigeben; alter Standard bleibt erhalten. Neustart separat freigeben.
8. Neue Bootkennung und erwartetes Kernelrelease beobachten; Benutzerfunktionstest bestätigen lassen.
9. Dauerhaften Standardwechsel separat planen/freigeben; vorheriger Rückfallkernel bleibt geschützt.
10. Rückfallkernel tatsächlich erneut starten; nur einen ungeschützten Kernel entfernen.
11. Dienst/Client während laufender Paketoperation beenden; kontrollierten Wiederanlauf ohne Doppelinstallation nachweisen.
12. Hardreset vor/nach jeder dauerhaften Transaktionsgrenze durchführen; tatsächliche Zustände und erhaltenen Rückfallstandard prüfen.

Absichtlich kaputtes initramfs, fehlgeschlagene DKMS-Builds, Bootloaderfehler und nicht bootfähige Kandidaten gehören in die Fehlermatrix. Ein Reset muss zeigen, dass der Einmalstart verbraucht ist und der alte Standard wieder greift. Die Anwendung verspricht keinen automatischen Hardware-Neustart bei einem hängenden Kernel.

## Reproduktionsabnahme

Für Kompatibel und Desktop auf Ubuntu und Debian werden je zwei frische Umgebungen mit identischen archivierten Eingaben verwendet. Kernelimage, aufbereitete unsignierte Module, Header samt Helfern und finale Pakete mit denselben öffentlichen Signaturinputs müssen bytegleich sein. Abweichungen sind Fehler, keine ignorierte Messunschärfe.

Auf dem Zielsystem erzeugte initramfs-Dateien und spätere DKMS-Module sind außerhalb dieser Paket-Bitidentitätszusage. Individuelle Rezepte tragen ihren eigenen nachgewiesenen oder ausstehenden Status.

## Releaseentscheidung

v0.1 darf erst freigegeben werden, wenn alle erforderlichen Nachweise vorliegen, alle vier Clients denselben Sicherheitsvertrag erfüllen und keine kritischen Sicherheitsbefunde offen sind. Fehlende Nachweise werden nicht durch ein positives Gesamtetikett verdeckt. Veröffentlichung von Anwendungspaketen und Installation auf dem produktiven Desktop benötigen jeweils einen späteren gesonderten Auftrag.

## Ergänzende Lebenszyklusprüfungen

**Alle folgenden Nachweise stehen aus.** Die Kennungen sind Anforderungen, keine vorhandenen Tests. Sie gelten auf beiden Zielsystemen in den freigegebenen VM-Konstellationen. Einzelne Teilfälle ohne Boot dürfen als gezielte Vertrags-/Integrationstests laufen; Boot-, Paket-, Migrations- und Fehlerketten bleiben VM-Prüfungen.

| Kennung | Positive und negative Fälle | Abnahme |
| --- | --- | --- |
| B-01 | Erststart ohne Vorab-Erfassung; Modul-/initramfs-Update seit aktuellem Boot; veränderte Dateien zwischen Erfassung und Boot; falsches Release/alte Bootkennung; Menüumordnung bei stabiler Zuordnung; veränderte Bootparameter; unveränderter gültiger Nachweis | Kein falscher KnownGoodKernel. Manueller Erstbestätigungsboot benötigt keinen vorhandenen Rückfallnachweis. Gültige Nachweise werden nach Revalidierung weiterverwendet; Reparatur bleibt trotz entwertetem Nachweis möglich |
| W-01 | Ende-zu-Ende-Wartung mit realem SystemPlan und polkit ab Phase 4; späteres Treiberupdate durch externes APT/DKMS; mehrere Kernel mit unterschiedlichen Schlüsseln; falscher/fehlender Schlüssel; erforderliches Modul scheitert; Sperrkonkurrenz; Wartung mehrerer bestätigter Kernel; externe Entwertung aller Nachweise; erzwungene Modulsignaturen bei deaktiviertem UEFI Secure Boot | Betroffene Pläne/Nachweise ungültig. Konkreter Wartungsplan, isolierter Build, ausschließlich kernelgebundene Signierung, finale Modul-/initramfs-Prüfung und erneuter Funktionsnachweis. Eigene Wartung erhält eine unveränderte bestätigte Rückfallmöglichkeit; fehlende Nachweise bleiben Reparaturbedarf. Kein allgemeiner Signierzugriff, Laden oder Neustart |
| S-01a | Bis Build-MVP: Arbeits-/Download-/Artefaktbereich voll; Inodes/Quoten erschöpft; Pfade auf gleichem Dateisystem; parallele Jobs; fremder Verbrauch; Cachebereinigung mit aktiven Job-/Archivreferenzen | Keine eigene Überbuchung oder Löschung referenzierter Daten. Fehlende Kapazität blockiert Download/Build/Export; tatsächliche Schreibfehler werden explizit behandelt. Reproduktionsarchive und aktive Jobs bleiben geschützt |
| S-01b | Bis CLI-Verwaltungs-MVP: /boot voll; ESP falls betroffen voll; Paket-/Journalbereich voll; Inodes/Quoten erschöpft; fremder Verbrauch nach Vorprüfung; Bereinigung mit installierten Kernel-/Schlüssel-/Rettungsreferenzen | Installation/Wartung bei fehlender Kapazität vor Änderung blockiert; tatsächliche Schreibfehler führen zur Recovery. Installierte Kernel, benötigte Schlüssel, Archive und Rettungsdaten bleiben geschützt |
| R-01 | Dienst/D-Bus fehlt; SQLite beschädigt; Journal unvollständig; unbekanntes Format; alte Pakete nicht mehr verfügbar; Offline-Live-System | Lesender Zustandsabgleich unabhängig von Anwendung. Konkrete freigegebene Reparatur mit geprüften vorhandenen Inputs; keine erfundenen Ersatzpakete, allgemeinen Reparaturen oder blinden Bootrestores |
| P-01 | Aktive/EOL/unbekannte Reihe; veraltete/widersprüchliche Pflegeangaben; Abwesenheit im Katalog; geänderter Plan nach Ausnahme; fehlende Reserve/Metapaket; externe Entwertung des Reservenachweises mit/ohne anderen KnownGoodKernel; gewöhnliches APT-Autoremove | Quelle/Zeit sichtbar. EOL-/Unsicherheitsausnahme nur konkret und ohne Umgehung harter Regeln. Fehlende Reserve sperrt neue Installation, nicht Build/Export. Reserve und Metapaket bleiben geschützt und regulär aktualisierbar; Reserve zählt nicht ohne Bootnachweis als KnownGoodKernel. Ist ihr Nachweis entwertet und kein anderer gültiger KnownGoodKernel vorhanden, sperrt der CapabilityReport Installation/Testboot; erneute Vorab-Erfassung und Bestätigungsboot ermöglichen Revalidierung. Bei anderer gültiger Rückfallmöglichkeit bleibt eine vorhandene unbestätigte Reserve zulässig |
| A-01 | Unterbrochenes Upgrade/Migration; laufender Executor; inkompatibler Client/Broker; unbekanntes Schema; Downgrade; Entfernung/Purge mit installierten Kerneln und Schlüsselreferenzen; entfernte/wiedervergebene Dienstkennung | Keine getötete/duplizierte Paketoperation. Ruhende Migrationsgrenze und überprüfbare Sicherung; tatsächlicher Zustand nach Restore abgeglichen. Kernel, Bootfähigkeit, benötigte Schlüssel und Rettungsdaten erhalten; Zugriffsschutz und geprüfter Wiederinstallationspfad |

S-01 ist der Gesamtnachweis aus S-01a und S-01b. Ein bestandenes S-01a allein gibt keine Installation frei. F-09 prüft die technische Machbarkeit eines isolierten Ablaufs; nur W-01 weist später die echte Autorisierung und Brokerintegration nach. Alle Teilnachweise sind offen.

Maßgebliche Details: [Lebenszyklus](operations/lifecycle.md), [Rettung](operations/rescue.md), [Herleitung](research/plan-review.md). Das vollständige unabhängige Architekturreview F-08 bleibt zusätzlich erforderlich.
