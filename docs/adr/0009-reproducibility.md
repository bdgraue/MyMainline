# 0009 – Präziser Reproduktionsumfang und unveränderliche Identitäten

Status: vereinbarter Entwurf, noch nicht implementiert. Stand: 9. Oktober 2026.

## Kontext

Ein Buildprotokoll allein beweist keine reproduzierbaren Pakete. Signaturen, Zeitstempel, Pfade, Toolchains und Headerhelfer können Ergebnisse beeinflussen.

## Entscheidung

Rezepte erfassen verifizierte Quellen, aufgelöste Konfiguration, Rootfs/Toolchain, Zertifikate, Seeds, Paketierung und kontrollierte Metadaten. Abgeleitete Releasekennungen sind aus ihrer eigenen Hashberechnung ausgeschlossen. Kernelimage, aufbereitete unsignierte Module, Header samt Helfern und finale Pakete mit gleichen öffentlichen Signaturinputs müssen in zwei frischen Umgebungen bytegleich sein.

## Folgen und Nachweise

Gleiche Paketkennung mit anderem Rezept/Inhalt wird nicht überschrieben. initramfs des Zielsystems und spätere DKMS-Module gehören nicht zur zugesagten Bitidentität der ursprünglichen Pakete. Kompatibel/Desktop werden auf beiden Distributionen als Releasegate geprüft. Individuelle Rezepte erhalten nur nach eigener erfolgreicher Reproduktion diesen Status. Keine ignorierten Differenzen oder pauschalen Leistungs-/Sicherheitsversprechen.

## Grundlagen

[Plan: Pakete und Reproduktion](../PLAN.md), [Upstream-Reproduzierbarkeit](https://docs.kernel.org/kbuild/reproducible-builds.html), [Prüfmatrix](../validation.md).
