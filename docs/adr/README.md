# Architekturentscheidungen

Stand: 9. Oktober 2026. **Status aller Einträge: vereinbarter Entwurf; Umsetzung und technische Nachweise stehen aus.** Ein unabhängiges Architekturreview steht ebenfalls aus. Die Nummerierung dient der Nachverfolgbarkeit und ist keine Freigabe bereits implementierter Verfahren.

Der [vollständige Plan](../PLAN.md) bleibt die zusammenhängende Spezifikation. Diese Protokolle halten Kontext, Entscheidung und Folgen fest. Änderungen am Plan müssen betroffene Protokolle, Risiken und Prüfungen mit aktualisieren.

| Entscheidung | Schwerpunkt |
| --- | --- |
| [0001 – Eigenständiges Projekt und offizielle Buildverfahren](0001-independent-project.md) | Eigenständiges Projekt und offizielle Buildverfahren |
| [0002 – Gemeinsamer Dienst und vier optionale Clients](0002-service-clients.md) | Gemeinsamer Dienst und vier optionale Clients |
| [0003 – Buildausgabe ist zunächst unvertrauenswürdig](0003-artifact-trust.md) | Buildausgabe ist zunächst unvertrauenswürdig |
| [0004 – Zustandsgebundene Freigabe und Terminalauthentifizierung](0004-authorization.md) | Zustandsgebundene Freigabe und Terminalauthentifizierung |
| [0005 – Dauerhafte Vorgänge und konservative Recovery](0005-transactions-recovery.md) | Dauerhafte Vorgänge und konservative Recovery |
| [0006 – Rückfallnachweis vor Installation und getrennter Testboot](0006-known-good-boot.md) | Rückfallnachweis vor Installation und getrennter Testboot |
| [0007 – Getrennte Modulsignierung und expliziter Schlüssellebenszyklus](0007-module-keys.md) | Getrennte Modulsignierung und expliziter Schlüssellebenszyklus |
| [0008 – Konservative Profile und spätere individuelle Optimierungen](0008-config-profiles.md) | Konservative Profile und spätere individuelle Optimierungen |
| [0009 – Präziser Reproduktionsumfang und unveränderliche Identitäten](0009-reproducibility.md) | Präziser Reproduktionsumfang und unveränderliche Identitäten |
| [0010 – Fähigkeiten prüfen und Plattformumfang begrenzen](0010-platform-scope.md) | Fähigkeiten prüfen und Plattformumfang begrenzen |
