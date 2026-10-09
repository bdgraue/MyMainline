# 0010 – Fähigkeiten prüfen und Plattformumfang begrenzen

Status: vereinbarter Entwurf, noch nicht implementiert. Stand: 9. Oktober 2026.

## Kontext

Ein Paketformat oder eine Distributionskennung sagt nicht, ob initramfs, DKMS, Secure Boot und Bootloader in einer konkreten Kombination funktionieren. Eine universelle Erstfreigabe wäre unbelegt.

## Entscheidung

v0.1 begrenzt Installation auf Ubuntu/Kubuntu 26.04 und Debian 13, amd64, GRUB/initramfs-tools, BIOS oder UEFI ohne Secure Boot und geprüfte unverschlüsselte ext4-Partitionen. Eine separate EFI-Systempartition ist zulässig. Andere Layouts/Bootloader und aktiviertes oder ungeklärtes UEFI-Secure-Boot sperren Systemänderungen; Build/Export bleiben getrennte Fähigkeiten.

## Folgen und Nachweise

BIOS behandelt UEFI-Secure-Boot als nicht anwendbar. Paketierung, initramfs und Bootloader bekommen eigene Adapter. Fedora und Arch sind nächste Ziele, openSUSE ist Untersuchungskandidat. Jede weitere Kombination benötigt eigene Sicherheits-/Recovery-/Bootnachweise; bestehende Kbuild-Ziele sind kein Unterstützungsnachweis. RCs werden v0.1 optional angezeigt, aber noch nicht gebaut.

## Grundlagen

[Plan: Plattformgrenzen](../PLAN.md), [Distributionsvergleich](../research/build-platform.md), [Prüfmatrix](../validation.md).
