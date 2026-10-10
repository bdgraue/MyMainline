# 0004 – Zustandsgebundene Freigabe und Terminalauthentifizierung

Status: vereinbarter Entwurf, noch nicht implementiert. Stand: 10. Oktober 2026.

## Kontext

Eine erfolgreiche Administratorauthentifizierung darf weder beliebige Rootbefehle noch zukünftige unbekannte Änderungen erlauben. Die CLI soll auch ohne Desktop nutzbar bleiben.

## Entscheidung

Der Broker ermittelt den echten D-Bus-Absender. Systempläne binden Besitzer, Aktion, Artefakthashes, beobachteten Paket-/Bootzustand, Fallback und Auswirkungen; sie sind 15 Minuten gültig und einmal ausführbar. Installation, Entfernung, Testboot, dauerhafter Standardwechsel und Neustart haben getrennte Freigaben. polkit mit Administratorauthentifizierung, standardmäßig ohne gecachte Autorisierung.

## Folgen und Nachweise

CLI/TUI registrieren bei Bedarf einen textuellen pkttyagent, binden ihn an die echte Busverbindung und warten auf Registrierung. Ohne Terminal oder Berechtigung folgt AuthenticationRequired. Kein stilles sudo, keine pauschale Gruppenregel. Zustandsänderung macht einen Plan ungültig; eine bereits laufende Transaktion wird durch Ablauf des Plans nicht beendet.

## Grundlagen

[Plan: Autorisierung und CLI](../PLAN.md), [polkit](https://polkit.pages.freedesktop.org/polkit/polkit.8.html), [pkttyagent](https://polkit.pages.freedesktop.org/polkit/pkttyagent.1.html).

## Ergänzende Planbindungen

Wartung, Reservebeschaffung/-markierung und Archivlöschung haben jeweils konkrete Pläne. Supportbeobachtung und eine gegebenenfalls nötige EOL-/Unsicherheitsausnahme gehören zur Installationsfreigabe; sie umgehen keine harten Schutzbedingungen. Speicherprüfung und relevante Broker-/Policyversion werden erneut geprüft. Ein Policywechsel kann vorbereitete Freigaben entwerten. [Betriebsregeln](../operations/lifecycle.md).
