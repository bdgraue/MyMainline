# Lesend erhobene Beobachtungen der Ausgangslage

Stand: 9. Oktober 2026. Dies sind anonymisierte Planungsbeobachtungen eines produktiv genutzten Ubuntu-26.04-/KDE-Desktops, keine zugesagten Eigenschaften aller Installationen. Persönliche Pfade, detaillierte Hardwaredaten und Zugangsdaten werden nicht veröffentlicht.

| Beobachtung | Auswirkung auf den Plan | Grenze der Aussage |
| --- | --- | --- |
| Ubuntu Mainline 1.4.15 wurde verwendet | Verfügbarkeit vorgebauter Mainline-Pakete ist die Ausgangsabhängigkeit | Keine Laufzeitprüfung sämtlicher Mainline-Funktionen |
| Bestehende Konfiguration referenziert Distributionszertifikate außerhalb des Vanilla-Quellarchivs | Zertifikate authentifiziert beschaffen; fehlende Pfade nicht leeren | Beschaffungs- und Kompatibilitätsverfahren noch nicht ausgeführt |
| Modulsignierung, BTF und mehrere Härtungsoptionen sind in der Basiskonfiguration aktiv | Buildvoraussetzungen beschaffen; Sicherheitseigenschaften differenzieren und erhalten | Kein Beweis vollständiger Sicherheitsgleichwertigkeit zum Distributionskernel |
| Dynamische Preemption und hohe Timerfrequenz sind bereits aktiv | Desktopprofil darf für diese Basis keine zusätzlichen Vorteile behaupten | Keine Leistungs-/Latenzmessungen |
| GRUB verwendet eine numerische Standardauswahl und ein verborgenes Menü ohne Wartezeit | Vor Installation stabilen Rückfall-Pin herstellen; Rettungsmenü im Test sichtbar machen | Verhalten und Umgebungsspeicher müssen durch reale VM-Boots/Resets nachgewiesen werden |
| Lokale GRUB-Skriptlogik behandelt einmalige Einträge und deren Verbrauch | Testboot und dauerhaften Standardwechsel getrennt modellieren | Lesen von Skripten ersetzt keinen erfolgreichen Einmalstart |
| Mehrere Kernelpakete sind vorhanden | Inventarisierung und Rollen getrennt führen | Existenz beweist weder Bootfähigkeit noch einen bestätigten Distributionsfallback |
| Secure Boot war deaktiviert; keine registrierten DKMS-Module wurden beobachtet | Aktivierte/ungeklärte Secure-Boot-Zustände und DKMS benötigen zusätzliche Testfälle | Keine Freigabe anderer Hosts oder Schlussfolgerung, DKMS sei generell irrelevant |
| Laufzeitbibliotheken sind teilweise vorhanden, Buildwerkzeuge nicht vollständig bestätigt | Zielumgebungen explizit definieren; keine Host-Abhängigkeitsinstallation bei der Planung | Keine geprüfte Toolchain-/GUI-Kombination |

## Wichtige Interpretationsgrenze

Die sichtbare Arbeitsumgebung war durch Sandboxregeln eingeschränkt. Fehlender Zugriff auf Hostpfade oder KVM innerhalb dieser Umgebung beweist nicht, dass der reale Host diese Fähigkeit nicht besitzt. Aus solchen Sichtbeschränkungen werden keine dauerhaften Hostdiagnosen abgeleitet.

Es wurden keine Kernelbuilds, Paketinstallationen, DKMS-Läufe, Bootänderungen, Neustarts oder VM-Resetversuche durchgeführt. Das [Validierungsdokument](../validation.md) beschreibt die noch ausstehenden Nachweise.
