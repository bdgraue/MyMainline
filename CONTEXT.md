# Fachliches Glossar

Dieses Dokument definiert die Begriffe der geplanten Anwendung. Architektur und technische Verfahren stehen im [Plan](docs/PLAN.md); diese Begriffe sind keine bereits implementierten APIs.

| Begriff | Bedeutung |
| --- | --- |
| Upstream-Release | Offizielle veröffentlichte Kernelversion des Linux-Projekts, getrennt von einem Distributionskernel |
| Releasekanal | Auswahlpolitik für Stable, aktive Longterm-Reihen oder optional sichtbare Release Candidates |
| Distributionskernel | Vom jeweiligen Distributionsprojekt gepflegter Kernel mit dessen Konfiguration, Änderungen und Integration |
| Basiskonfiguration | Ausdrücklich ausgewählte bestehende Kernelkonfiguration als Ausgangspunkt eines neuen Rezepts |
| Profil | Dokumentierte Menge zulässiger Konfigurationsänderungen gegenüber der Basiskonfiguration |
| BuildRecipe | Unveränderliche, vollständig aufgelöste Buildvorgabe einschließlich Herkunft, Konfiguration, Umgebung und öffentlichen Signaturinputs |
| Rezeptidentität | Inhaltsbezogene Identität dieser Vorgabe; abgeleitete Kennungen gehören nicht rekursiv zu ihrer eigenen Berechnung |
| BuildJob | Einzelner Ausführungsversuch eines Rezepts; mehrere Versuche können zum gleichen Rezept gehören |
| Paketkandidat | Noch ungeprüfte Buildausgabe, die keine privilegierte Installation erlaubt |
| ArtifactSet | Geprüfte Pakete zusammen mit Manifest, Rezeptzuordnung und Nachweisen |
| SystemPlan | Konkrete, zeitlich begrenzte, an Besitzer, Artefakte und beobachteten Systemzustand gebundene Änderung |
| Autorisierung | Berechtigungsprüfung, ob der tatsächliche Auftraggeber eine bestimmte Systemaktion ausführen darf |
| Freigabe | Ausdrückliche Zustimmung zu den konkreten Auswirkungen eines Systemplans; keine pauschale Zustimmung zu späteren Aktionen |
| Kernelinventar | Beobachtete installierte Kernel, Paketzuordnungen, Dateien und Schutzrollen |
| BootObservation | Beobachteter Start mit Bootkennung und Kernelidentität |
| KnownGoodKernel | Technisch zugeordneter Kernel mit vor dem Bestätigungsboot erfasstem unverändertem Bootdateisatz, passender Bootbeobachtung und Benutzerbestätigung wesentlicher Funktionen |
| Bootdateisatz | Identifizierte Dateien und Bootzuordnung, deren unveränderter Zustand einen konkreten Bestätigungsboot mit dem später erreichbaren Kernel verbindet |
| Distributionsreserve | Zusätzlich erhaltener Distributionskernel mit seinem Aktualisierungsmetapaket; ohne eigenen Funktionsnachweis noch kein bestätigter Rückfallkernel |
| Supportstatus | Mit Quelle und Prüfzeitpunkt belegter Pflegezustand einer Kernelreihe; getrennt von Herkunft und Releasekanal |
| Rückfallkernel | Bestätigter funktionierender Kernel, der bei einem fehlgeschlagenen Test weiterhin erreichbar bleibt |
| Testboot | Separat freigegebener einmaliger Start eines validierten Kandidaten, während der sichere Standard erhalten bleibt |
| Dauerhafte Übernahme | Separat freigegebener Wechsel des dauerhaften Bootstandards nach beobachtetem und bestätigtem Test |
| Schutzrolle | Grund, aus dem MyMainline einen Kernel nicht entfernen darf, etwa laufend, Standard, Rückfall oder ausstehender Test |
| CapabilityReport | Für ein beobachtetes System verfügbare Aktionen einschließlich konkreter Sperrgründe |
| OperationError | Stabiler Fehlercode mit Kontext und möglichem nächstem Schritt; fehlende Daten sind kein Erfolg |
| Recovery | Abgleich von tatsächlichem Systemzustand und dauerhaftem Vorgang nach Unterbrechung oder Teilfehler |
| RepairRequired | Ungeklärter oder teilweise veränderter Systemzustand, der neue Änderungen bis zu einem konkreten Reparaturplan sperrt |
| Reproduktionsnachweis | Bytevergleich unabhängiger Ausführungen mit identischen archivierten Eingaben innerhalb des ausdrücklich definierten Artefaktumfangs |

Ein installierter Kernel ist noch kein bestätigter Rückfallkernel. Ein angenommener Auftrag ist noch kein abgeschlossener Vorgang. Ein reproduzierbares Artefakt ist nicht automatisch funktionsfähig oder frei von Fehlern.
