# 0007 – Getrennte Modulsignierung und expliziter Schlüssellebenszyklus

Status: vereinbarter Entwurf, noch nicht implementiert. Stand: 9. Oktober 2026.

## Kontext

Private Schlüssel im Buildworker vergrößern dessen Zugriff. Neu erzeugte Signaturen bei jedem Build erschweren Bitgleichheit. Kbuild-Buildsignierung und Laufzeit-Signaturprüfung haben unterschiedliche Rollen.

## Entscheidung

Pro neuer Rezeptidentität ein Modulschlüsselpaar; nur das öffentliche Zertifikat wird Buildinput. CONFIG_MODULE_SIG_ALL=n und ein leerer CONFIG_MODULE_SIG_KEY ersetzen ausdrücklich die automatische Buildsignierung. CONFIG_MODULE_SIG und vorhandenes CONFIG_MODULE_SIG_FORCE bleiben erhalten. Ein isolierter Signierer signiert endgültig aufbereitete Module mit einem eigenen vertrauenswürdig erzeugten Helfer.

## Folgen und Nachweise

Öffentliche Zertifikate und abgetrennte Signaturen werden Reproduktionsinputs. Nach Signierung kein Stripping; deterministische Kompression danach. Kein Worker-Signierhelfer, kein fremder MOK-/Firmware-/Hostschlüssel. Schlüssel bleiben bei aktiven Referenzen erhalten; Löschung braucht eigene Freigabe. Rotation erzeugt neue Rezept-/Kernelidentität. Eine Dienstsperre widerruft keinen bereits eingebauten Vertrauensanker. Vollständiges Secure Boot bleibt später.

## Grundlagen

[Plan: Modulsignierung](../PLAN.md), [Upstream-Reproduzierbarkeit](https://docs.kernel.org/kbuild/reproducible-builds.html), [Modulsignierung](https://docs.kernel.org/admin-guide/module-signing.html).
