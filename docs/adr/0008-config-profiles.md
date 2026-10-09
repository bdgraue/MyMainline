# 0008 – Konservative Profile und spätere individuelle Optimierungen

Status: vereinbarter Entwurf, noch nicht implementiert. Stand: 9. Oktober 2026.

## Kontext

Eine bestehende Distributionskonfiguration enthält Sicherheits- und Hardwareentscheidungen. Hardwareerkennung bildet nur den erkannten Zustand ab; Leistungsgewinne durch Änderungen müssen gemessen werden.

## Entscheidung

Kompatibel ist Standard ohne Leistungsänderungen. Desktop verlangt am Zielstand geprüfte PREEMPT_DYNAMIC/PREEMPT/HZ_1000-Einstellungen. Individuell erlaubt nachvollziehbare Fragmente innerhalb derselben Sicherheits-/Bootregeln. Alle v0.1-Profile behalten breite Treiberunterstützung; CPU-spezifische Compilerflags und gezielte Treiberreduktion liegen zunächst außerhalb des freigegebenen Umfangs.

## Folgen und Nachweise

Das ist eine konservative v0.1-Abgrenzung, kein dauerhaftes Verbot. GCC unterscheidet -march (möglicherweise anderer Befehlssatz) von -mtune (Tuning innerhalb der Befehlssatzvorgabe); pauschal beide gleichzusetzen wäre falsch. Zusätzliche Flags müssen zudem zur jeweiligen Kernel-Toolchain passen. localmodconfig nutzt geladene Module; daraus folgt unsere Sorge um später angeschlossene Geräte und Boot-/Rettungspfade. Ein kleinerer Build ist nicht automatisch schneller im Betrieb. Spätere Opt-in-Profile brauchen explizite Zielhardware, unveränderliche aufgelöste Inputs, vollständige Diffs, Kompatibilitäts-/Boot-/Geräteprüfungen und einen breit kompatiblen Rückfallkernel. Sie deaktivieren Sicherheitsfunktionen nicht beiläufig.

## Grundlagen

[Plan: Profile](../PLAN.md), [GCC-x86-Optionen](https://gcc.gnu.org/onlinedocs/gcc/x86-Options.html), [localmodconfig-Quelle](https://github.com/torvalds/linux/blob/master/scripts/kconfig/streamline_config.pl).
