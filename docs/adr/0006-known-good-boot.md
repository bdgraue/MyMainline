# 0006 – Rückfallnachweis vor Installation und getrennter Testboot

Status: vereinbarter Entwurf, noch nicht implementiert. Stand: 10. Oktober 2026.

## Kontext

Installiert, laufend, Standard und bestätigt funktionsfähig sind unterschiedliche Eigenschaften. Eine numerische GRUB-Auswahl kann nach einer Paketinstallation auf einen anderen Kernel zeigen.

## Entscheidung

Erster Rückfallkandidat ist nur der aktuell laufende, eindeutig zugeordnete und technisch vollständige Kernel mit Benutzerbestätigung wesentlicher Funktionen. Der Nachweis bindet Paket-/Dateiidentität, Booteintrag und Bootbeobachtung. Vor Installationshooks wird dieser Kernel stabil als sicherer Standard festgelegt. Testboot, Neustart und dauerhafte Übernahme bleiben getrennte Systempläne.

## Folgen und Nachweise

Einmalstart benötigt nachgewiesenen GRUB-Umgebungsspeicher und ein sichtbares Rettungsmenü. Nach einem Reset muss der alte Standard greifen. Bestätigung verlangt neue Bootkennung und erwartetes Kernelrelease; falscher Boot wird nicht automatisch erneut scharfgeschaltet. Ein hängender Kernel löst keinen zugesagten automatischen Reset aus. Laufender, Standard-, Rückfall- und ausstehender Testkernel bleiben vor Entfernung geschützt.

## Grundlagen

[Plan: Rückfallkernel und GRUB](../PLAN.md), [lokale Beobachtungen](../research/observations.md), [VM-Nachweise](../validation.md).

## Präzisierung der Erstbestätigung

Der laufende Kernel ist ohne vertrauenswürdige Vorab-Erfassung nur Kandidat. Ein geschützter Bootdateisatz wird vor einem manuellen Bestätigungsboot über den bestehenden geprüften Bootpfad erfasst; neuer Boot und Benutzerbestätigung müssen dazu passen. Seit dem Boot ersetzte Module oder initramfs dürfen nicht nachträglich als gestartet bestätigt werden. Geänderte Dateien entwerten betroffene Nachweise und Pläne; gültige vorhandene Nachweise sind revalidierbar. Der erste Bestätigungsboot setzt keinen bereits bestätigten Rückfallkernel voraus. Details und Grenzen: [Lebenszyklus 1](../operations/lifecycle.md), B-01.
