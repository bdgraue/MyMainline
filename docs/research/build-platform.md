# Kernelbuild, Paketierung und Distributionsgrenzen

Stand: 9. Oktober 2026. Die folgenden Verfahren sind recherchiert; ihre Kombination für MyMainline ist noch nicht praktisch nachgewiesen.

## Quellen und Konfiguration

[kernel.org releases.json](https://www.kernel.org/releases.json) eignet sich zur Releaseerkennung. Ein HTTPS-Katalog ist kein signierter vollständiger historischer Katalog. Nach dem [offiziellen Signaturverfahren](https://www.kernel.org/category/signatures.html) wird das unkomprimierte Quellarchiv gegen die Entwicklersignatur und einen zuvor akzeptierten vollständigen Fingerprint geprüft. Downloadprüfsummen allein erfüllen diese Herkunftsprüfung nicht.

Die [Kconfig-Dokumentation](https://docs.kernel.org/kbuild/kconfig.html) warnt ausdrücklich vor geänderten oder umbenannten Symbolen bei übernommenen Konfigurationen. Eine vorhandene Ubuntu-Konfiguration ist daher eine Ausgangsbasis, kein ungeprüfter Kompatibilitätsnachweis. Das Rezept muss neue, verschwundene und durch Abhängigkeiten geänderte Werte ausweisen. Sicherheitsverluste sperren eine Installationsfreigabe.

Öffentliche Ubuntu-Zertifikatspfade können auf Dateien außerhalb des Vanilla-Quellarchivs verweisen. Diese Pfade einfach zu leeren wäre eine Änderung der Vertrauenskonfiguration. Beschaffung aus authentifizierten Distributionsquellen und semantische Prüfung bleiben Phase-0-Aufgaben. Auch ein formal ähnlicher Konfigurationswert kann eine Distributionsänderung nicht ersetzen.

## Offizielle Paketierungsverfahren

[Makefile.package](https://github.com/torvalds/linux/blob/master/scripts/Makefile.package), [mkdebian](https://github.com/torvalds/linux/blob/master/scripts/package/mkdebian) und [builddeb](https://github.com/torvalds/linux/blob/master/scripts/package/builddeb) bilden den offiziellen Kbuild-Debianpfad. `bindeb-pkg` liefert binäre Paketkandidaten; die konkreten Paketinhalte und Hooks müssen am ausgewählten Release geprüft werden. Der Entwurf installiert Image einschließlich Module und passende Header, keine beiläufig erzeugten libc-Header oder Debugpakete.

Das [Debian Kernel Handbook](https://kernel-team.pages.debian.net/kernel-handbook/ch-common-tasks.html) und das [Debian Administrator's Handbook](https://www.debian.org/doc/manuals/debian-handbook/sect.kernel-compilation.en.html) sind zusätzliche Referenzen. Ein erfolgreiches Paketierungsziel beweist noch keine sichere Hostintegration.

Header enthalten ausführbare Buildlogik und Hilfsprogramme: [install-extmod-build](https://github.com/torvalds/linux/blob/master/scripts/package/install-extmod-build). Sie werden von externen Modulbuilds verwendet. Deshalb müssen auch Headerbestandteile aus verifizierten Quellen rekonstruiert und geprüft werden. `modules_prepare` allein liefert nicht den vollständigen Nachweis einschließlich Symbolversionsdaten: [externe Module](https://docs.kernel.org/kbuild/modules.html).

## Signierung und Reproduzierbarkeit

Die [Upstream-Anleitung zur Reproduzierbarkeit](https://docs.kernel.org/kbuild/reproducible-builds.html) behandelt Zeitstempel, Buildbenutzer/-host, Pfade, Seeds und mehrstufige Modulsignierung. Daraus folgt die Entscheidung, private Schlüssel aus dem Worker zu halten und öffentliche Zertifikate sowie abgetrennte Signaturen als reproduzierbare Eingaben zu archivieren. Die unterstützten Helferoperationen sind am [sign-file-Quellcode](https://github.com/torvalds/linux/blob/master/scripts/sign-file.c) zu prüfen und später festzuschreiben.

Die [Modulsignierungsdokumentation](https://docs.kernel.org/admin-guide/module-signing.html) verlangt, signierte Module nicht nachträglich so zu verändern, dass die Signatur ungültig wird. Aufbereitung/Stripping erfolgen vor Signierung; kontrollierte Kompression folgt danach. Kernel-/Firmwarevertrauen und Modulsignierung sind unterschiedliche Ebenen.

[Ubuntus Secure-Boot-Dokumentation](https://documentation.ubuntu.com/security/security-features/platform-protections/secure-boot/) erklärt den distributionsintegrierten Vertrauenspfad. Seine Übertragbarkeit auf unveränderte Upstream-Kernel ist nicht einfach vorauszusetzen. v0.1 erkennt den Zustand und blockiert nicht unterstützte Installationen; vollständige Unterstützung bleibt eine eigene Ausbaustufe.

## Isolation und Hostintegration

[bubblewrap](https://github.com/containers/bubblewrap) stellt Sandboxmechanismen bereit. Die konkrete erlaubte Dateisicht und verpflichtenden Namespacefähigkeiten müssen MyMainline selbst definieren und negativ testen. Eine Option, die Isolation nur versucht, ersetzt keinen nachgewiesenen Pflichtmechanismus. Ein fehlgeschlagener Sandboxstart erlaubt keinen Hostbuild als Ersatz.

[mmdebstrap](https://manpages.debian.org/unstable/mmdebstrap/mmdebstrap.1.en.html) ist der vorgesehene Rootfs-Baustein. Archivsignaturen, konkrete Paketversionen, Rootfs-Identität und unprivilegierter Ausführungsmodus müssen für beide Distributionen nachgewiesen werden.

[dpkg](https://manpages.debian.org/trixie/dpkg/dpkg.1.en.html) dokumentiert `DPKG_FRONTEND_LOCKED`. Unsere Sperre muss mit vorhandener Paketverwaltung zusammenarbeiten; eigene Sperren allein genügen nicht. Paketoperationen sind nicht atomar: Die [Debian Policy zu Maintainer-Skripten](https://www.debian.org/doc/debian-policy/ch-maintainerscripts.html) beschreibt unter anderem Fehlerpfade. Deshalb braucht es ein dauerhaftes Journal und einen tatsächlichen Zustandsabgleich.

## Parallelen und Erweiterungsbedarf

| Distribution/Familie | Wiederverwendbarer Teil | Eigener Nachweis vor Installation |
| --- | --- | --- |
| Ubuntu/Kubuntu 26.04 | Quellen, Rezepte, Kconfig/Kbuild, Debian-Paketformat | Distributionskonfiguration/-zertifikate, DKMS, initramfs-tools, GRUB, konkrete Speicher-/Secure-Boot-Konstellation |
| Debian 13 | Gemeinsame Debian-Paketierungsbasis und Kernmodelle | Eigene Toolchain-/Konfigurations-/Hookkombination und dieselbe vollständige VM-Abnahme |
| Fedora | Quellen, Vertrauens-/Rezeptmodell und Teile des Builds | Native RPM-Integration, dracut, Modul-/Signierpolitik und Bootverfahren; in dieser Recherche kein ausführbarer Gesamtpfad nachgewiesen |
| Arch | Quellen, Modelle und Buildorchestrierung | Native Paketidentität, Pacman-Hooks, initramfs-/Bootintegration und Rückfallregeln; vorhandene Kbuild-Ziele reichen nicht als Freigabe |
| openSUSE | Grundsätzlich dieselben fachlichen Aufgaben | Noch zu untersuchen; keine behauptete Unterstützung |

[Archs traditionelle Kernelkompilierung](https://wiki.archlinux.org/title/Kernel/Traditional_compilation) und [Arch Build System](https://wiki.archlinux.org/title/Kernel/Arch_build_system) zeigen eigene Paket-/Konfigurationswege. [Fedoras Kernelbau-Dokumentation](https://docs.fedoraproject.org/en-US/quick-docs/kernel-build-custom/) ist eine spätere Arbeitsgrundlage; ihr vollständiger Inhalt war beim Abruf durch Zugriffsschutz nicht verifizierbar. Der Fedora-Pfad bleibt ausdrücklich offen.

Paketformat, initramfs und Bootloader werden als getrennte Adapter gedacht. Die Distributionskennung allein darf keine Fähigkeit freischalten. Eine neue Kombination wird erst nach ihrem eigenen Nachweis freigegeben.

## CPU-Flags und Treiberreduktion

[GCC unterscheidet](https://gcc.gnu.org/onlinedocs/gcc/x86-Options.html) Befehlssatzwahl mit `-march` und Tuning mit `-mtune`. `-march=native` hängt zudem von der Buildmaschine ab. Daraus folgt keine pauschale Aussage, jede CPU-Optimierung sei unsicher; wohl aber zusätzlicher Bedarf an festgeschriebener Zielhardware und kernelgeeigneten Optionen.

Die [localmodconfig-Implementierung](https://github.com/torvalds/linux/blob/master/scripts/kconfig/streamline_config.pl) verwendet geladene Module als Eingabe. Unsere Schlussfolgerung: Die aktuelle Nutzung bildet zukünftige USB-Geräte, Docking, andere Speichermedien und Rettungspfade nicht vollständig ab. Deshalb bleibt breite Treiberunterstützung der Standard. Spätere explizite Spezialprofile sind möglich, benötigen aber eigene Tests: [ADR 0008](../adr/0008-config-profiles.md).
