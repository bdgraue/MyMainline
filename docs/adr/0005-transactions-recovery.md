# 0005 – Dauerhafte Vorgänge und konservative Recovery

Status: vereinbarter Entwurf, noch nicht implementiert. Stand: 10. Oktober 2026.

## Kontext

dpkg-/Hookoperationen können teilweise erfolgreich sein. Ein Clientabsturz ist kein Grund, eine laufende Paketoperation zu töten; ein Dienstneustart darf sie nicht duplizieren.

## Entscheidung

Ein geschützter Transaktionsprozess führt einen konkreten Plan unter MyMainline- und paketverwaltungskompatiblen Sperren aus. Ein rootgeschütztes dauerhaftes Journal dokumentiert Absicht vor und überprüftes Ergebnis nach jedem Schritt. Wiederanlauf gleicht Journal, Prozesse, Pakete, Dateien und Bootzustand ab.

## Folgen und Nachweise

Nur ausgewählte lokale Kernelpakete; fehlende Abhängigkeiten blockieren ohne allgemeines apt-Reparieren/Upgraden. Keine gelöschten Sperrdateien. Teilfehler führen zu RepairRequired; neue explizite Reparaturpläne statt blindem Wiederholen/Entfernen. Ein sicherer Fallback-Pin bleibt erhalten, solange neue Pakete die alte numerische Auswahl verändern könnten. Fremde Bootänderungen werden nicht mit alten Backups überschrieben.

## Grundlagen

[Plan: Zustände und Recovery](../PLAN.md), [dpkg](https://manpages.debian.org/trixie/dpkg/dpkg.1.en.html), [Debian Policy](https://www.debian.org/doc/debian-policy/ch-maintainerscripts.html).

## Unabhängige Wiederherstellung

Journal und versioniertes Rettungsmanifest werden außerhalb SQLite erhalten und unabhängig vom Dienst lesbar. Schemawechsel benötigen eine ruhende überprüfte Transaktionsgrenze; laufende Paketoperationen werden nicht beendet. Datenbankrestore ersetzt keinen Zustandsabgleich. [ADR 0012](0012-recovery-lifecycle.md) und [Rettungsentwurf](../operations/rescue.md) präzisieren R-01/A-01.
