# Rettung ohne MyMainline-Dienst

Stand: 10. Oktober 2026. **Entwurf für die spätere Betriebsanleitung. Keine praktisch freigegebene Befehlsfolge.** Die konkreten Werkzeuge und distributionsabhängigen Befehle werden erst in Wegwerf-VMs geprüft. Jetzt sind keine Eingriffe am produktiven System vorgesehen.

## Voraussetzungen

Ein versioniertes rootgeschütztes Journal und Rettungsmanifest außerhalb von SQLite enthalten die nötigen Zuordnungen und Transaktionsgrenzen. Ein lesbarer Export enthält öffentliche Zertifikate und Hash-/Paket-/Bootmetadaten, keine privaten Schlüssel. Die Dokumentation und lesende Formatbeschreibung müssen unabhängig von GUI, Dienst, Datenbank und D-Bus verfügbar sein.

Ein Rettungssystem benötigt Zugriff auf die unterstützte Systempartition und den tatsächlichen Bootpfad. Physischer Datenträgerverlust oder unlesbare Dateisysteme sind dadurch nicht behoben. Die v0.1-Anleitung gilt nur für die freigegebenen unverschlüsselten Layouts; weitere Layouts brauchen eigene Nachweise.

## Geplanter Ablauf

1. Wenn möglich, den zuvor bestätigten Rückfallkernel über das sichtbare Bootmenü manuell wählen. Bei fehlender Bootmöglichkeit ein geeignetes Live-/Rettungssystem verwenden.
2. Zustand zunächst lesend erfassen: Plattform, betroffene Partitionen, Paketdatenbank, Image-/Modul-/initramfs-Dateien, tatsächliche Bootkonfiguration und vorhandene Journal-/Manifestversionen. Unbekannte Formate werden ausdrücklich gemeldet.
3. Geplante und protokollierte Schritte mit dem tatsächlichen Zustand abgleichen. Weder ein „fertig“-Eintrag noch eine vorhandene Sicherung genügt allein. Fremde Paket-/Bootänderungen gesondert erkennen.
4. Den kleinstmöglichen Reparaturumfang benennen: exakt betroffene Pakete/Versionen, verfügbare geprüfte Artefakte, zu erneuernde Dateien und der zu erhaltende sichere Bootpfad. Fehlen erforderliche Pakete offline, diese Grenze anzeigen und keine Ersatzversion erfinden.
5. Erst nach ausdrücklicher Zustimmung zur konkreten Reparatur schreiben. Keine allgemeine Paketkonfiguration, kein pauschales APT-Reparieren oder Autoremove, keine Löschung von Sperren und kein blindes Überschreiben fremder Bootkonfiguration.
6. Nachbedingungen prüfen und den Rückfallkernel manuell starten. Ein neuer KnownGoodKernel braucht einen passenden Dateisatz vor dem Boot und die anschließende Funktionsbestätigung.
7. Nach Wiederinstallation der Anwendung Journal, Datenbank und tatsächlichen Zustand erneut abgleichen. Neue Systemaktionen bleiben bei ungeklärten Widersprüchen gesperrt.

## Vor einer Installationsfreigabe nachzuweisen

R-01 umfasst fehlenden Dienst, fehlendes D-Bus, beschädigte SQLite-Datenbank, unvollständiges Journal, unbekanntes Manifestformat, unterbrochene Migration, entfernte Anwendung sowie nicht mehr online verfügbare alte Pakete. Der Wiederherstellungspfad wird auch offline aus einem Live-System getestet.

Die geprüfte Anleitung muss dann jede konkrete Schreiboperation, deren Voraussetzungen und erwartete Nachbedingungen nennen. Bis dahin wird kein ausgedachter MyMainline-Rettungsbefehl veröffentlicht. Siehe [Lebenszyklus](lifecycle.md), [Recovery-Entscheidung](../adr/0012-recovery-lifecycle.md) und [Prüfmatrix](../validation.md).
