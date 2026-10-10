# 0011 – Pflegezustand und erhaltene Distributionsreserve

Status: vereinbarter Entwurf, noch nicht implementiert. Stand: 10. Oktober 2026.

## Kontext

Eine gültige Quellsignatur sagt nichts über die weitere Pflege einer Kernelreihe aus. Ein eigener Kernel ersetzt weder die Aktualisierungspolitik der Distribution noch automatisch einen praktisch bestätigten Rückfallkernel.

## Entscheidung

Neue Installationen ungepflegter Reihen benötigen eine zusätzliche ausdrückliche Ausnahme, gebunden an den konkreten Systemplan und dessen Supportbeobachtung. Unbekannter oder veralteter Pflegezustand wird sichtbar und verlangt eine entsprechende konkrete Unsicherheitsfreigabe. Keine Ausnahme hebt die harten Signatur-, Plattform-, Sicherheits- oder Rückfallbedingungen auf.

Ein vorhandener Distributionskernel und sein Aktualisierungsmetapaket werden zusätzlich dauerhaft erhalten. Fehlende Reserve wird getrennt beschafft und freigegeben; bis dahin bleiben neue verwaltete Kernelinstallationen gesperrt, Build und Export aber möglich. Manuelle Paketmarkierung schützt eindeutig zugeordnete Reservepakete vor gewöhnlichem Autoremove, ohne pauschales Hold.

## Folgen und Nachweise

Pflegeende entfernt keine vorhandenen Rückfallkernel und schaltet keinen Bootstandard um. Reserve und bestätigter Rückfall sind getrennte Rollen. Externe Änderungen entwerten einen Reservenachweis nur bei tatsächlich geändertem relevanten Dateisatz oder Bootpfad. Ohne anderen gültigen KnownGoodKernel sperrt dies neue Installationen/Testbootpläne bis zur erneuten Vorab-Erfassung und Funktionsbestätigung nach Boot; der Paketschutz bleibt bestehen. Eine vorhandene unbestätigte Reserve ist bei anderer gültiger Rückfallmöglichkeit zulässig. Fremde ausdrückliche administrative Eingriffe bleiben außerhalb der Schutzgarantie. Aktualitätsgrenzen, Zuordnung der Distributionsmetapakete und Zustandswechsel sind vor Freigabe nachzuweisen; P-01 bleibt offen.

## Grundlagen

[Plan 4.7](../PLAN.md), [Betriebsregeln](../operations/lifecycle.md), [Releasepflege](https://www.kernel.org/category/releases.html), [apt-mark](https://manpages.debian.org/trixie/apt/apt-mark.8.en.html).
