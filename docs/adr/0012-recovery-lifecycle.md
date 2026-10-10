# 0012 – Unabhängige Rettungsnachweise und Anwendungsmigration

Status: vereinbarter Entwurf, noch nicht implementiert. Stand: 10. Oktober 2026.

## Kontext

Recovery nur über die eigene Datenbank scheitert bei deren Beschädigung oder einer entfernten Anwendung. Ein Schema- oder Policywechsel kann laufende Vorgänge und bereits vorbereitete Freigaben unvereinbar machen.

## Entscheidung

Versionierte rootgeschützte Journal-/Rettungsnachweise werden außerhalb SQLite erhalten und unabhängig von Dienst und D-Bus lesbar. Reparatur gleicht sie stets mit dem tatsächlichen Systemzustand ab.

Migrationen beginnen nur an einer ruhenden überprüften Transaktionsgrenze mit gesicherter Wiederherstellung. Unbekannte Versionen blockieren Änderungen; relevante Policyupdates entwerten vorbereitete Pläne. Laufende Executor-/Paketoperationen werden nicht beendet oder dupliziert.

## Folgen und Nachweise

Entfernung oder Purge von MyMainline entfernt keine installierten Kernel, benötigten Bootmöglichkeiten, aktiven Schlüsselreferenzen oder Rettungsnachweise. Persistenter Zugriffsschutz muss die Entfernung von Dienstkonten überstehen. Private Schlüssel fehlen in gewöhnlichen Exporten; eigene geschützte Sicherung bleibt ausdrücklich freizugebender Vorgang. Downgrade und Datenbankrückspielung benötigen Systemzustandsabgleich. R-01 und A-01 einschließlich Offline-Rettung und unterbrochener Migration sind Pflicht und stehen aus.

## Grundlagen

[Recovery im Plan](../PLAN.md), [Lebenszyklus](../operations/lifecycle.md), [Rettungsentwurf](../operations/rescue.md), [Prüfmatrix](../validation.md).
