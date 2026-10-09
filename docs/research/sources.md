# Primärquellen

Recherchestand: 9. Oktober 2026. Verweise auf laufende Branches und Dokumentationsversionen sind veränderlich. Vor Implementierung sind passende konkrete Zielstände und deren Hashes festzuschreiben. Dieses Verzeichnis ist kein Verzeichnis bereits erfolgreich getesteter Abhängigkeiten.

## Referenzprojekte

- [Ubuntu Mainline](https://github.com/bkw777/mainline), [Lizenz](https://github.com/bkw777/mainline/blob/master/LICENSE), [Kernelverwaltung](https://github.com/bkw777/mainline/blob/master/src/Common/LinuxKernel.vala).
- [linux-tkg](https://github.com/Frogging-Family/linux-tkg), [Konfiguration](https://github.com/Frogging-Family/linux-tkg/blob/master/customization.cfg), [Installation](https://github.com/Frogging-Family/linux-tkg/blob/master/install.sh), [Vorbereitung](https://github.com/Frogging-Family/linux-tkg/blob/master/linux-tkg-config/prepare).
- [TuxMake CLI](https://tuxmake.org/cli/), [Reproduzierbarkeit](https://tuxmake.org/reproducible_builds/), [Quellcode](https://github.com/kernelci/tuxmake).
- [CachyOS Kernel Manager](https://github.com/CachyOS/kernel-manager).
- [XanMod](https://xanmod.org/), [Liquorix](https://liquorix.net/).

## Linux-Upstream

- [Releasekatalog](https://www.kernel.org/releases.json), [Signaturverfahren](https://www.kernel.org/category/signatures.html).
- [Kconfig](https://docs.kernel.org/kbuild/kconfig.html), [Kconfig-Sprache](https://docs.kernel.org/kbuild/kconfig-language.html), [Kbuild](https://docs.kernel.org/kbuild/kbuild.html), [Buildvoraussetzungen](https://docs.kernel.org/process/changes.html).
- [Paketierungsziele](https://github.com/torvalds/linux/blob/master/scripts/Makefile.package), [mkdebian](https://github.com/torvalds/linux/blob/master/scripts/package/mkdebian), [builddeb](https://github.com/torvalds/linux/blob/master/scripts/package/builddeb), [Headeraufbereitung](https://github.com/torvalds/linux/blob/master/scripts/package/install-extmod-build).
- [Reproduzierbare Builds](https://docs.kernel.org/kbuild/reproducible-builds.html), [Modulsignierung](https://docs.kernel.org/admin-guide/module-signing.html), [externe Module](https://docs.kernel.org/kbuild/modules.html), [sign-file](https://github.com/torvalds/linux/blob/master/scripts/sign-file.c).
- [Preemption](https://github.com/torvalds/linux/blob/master/kernel/Kconfig.preempt), [Timerfrequenz](https://github.com/torvalds/linux/blob/master/kernel/Kconfig.hz), [localmodconfig](https://github.com/torvalds/linux/blob/master/scripts/kconfig/streamline_config.pl).
- [GCC-x86-Optionen](https://gcc.gnu.org/onlinedocs/gcc/x86-Options.html): Befehlssatzwahl und Tuning unterscheiden.

## Plattform und Autorisierung

- [Ubuntu Secure Boot](https://documentation.ubuntu.com/security/security-features/platform-protections/secure-boot/).
- [Debian-Releases](https://www.debian.org/releases/), [Kernel Handbook](https://kernel-team.pages.debian.net/kernel-handbook/ch-common-tasks.html), [Kernelkompilierung](https://www.debian.org/doc/manuals/debian-handbook/sect.kernel-compilation.en.html).
- [Debian Policy: Maintainer-Skripte](https://www.debian.org/doc/debian-policy/ch-maintainerscripts.html), [dpkg](https://manpages.debian.org/trixie/dpkg/dpkg.1.en.html), [mmdebstrap](https://manpages.debian.org/unstable/mmdebstrap/mmdebstrap.1.en.html).
- [polkit](https://polkit.pages.freedesktop.org/polkit/polkit.8.html), [pkttyagent](https://polkit.pages.freedesktop.org/polkit/pkttyagent.1.html).
- [bubblewrap](https://github.com/containers/bubblewrap), [systemd kernel-install](https://github.com/systemd/systemd/blob/main/man/kernel-install.xml).
- [Arch: traditionelle Kompilierung](https://wiki.archlinux.org/title/Kernel/Traditional_compilation), [Arch Build System](https://wiki.archlinux.org/title/Kernel/Arch_build_system): einzelne Abrufe waren zugriffsbeschränkt; keine praktisch geprüfte Integrationskette.
- [Fedora: Custom Kernel](https://docs.fedoraproject.org/en-US/quick-docs/kernel-build-custom/): Zugriffsschutz verhinderte die vollständige Inhaltsprüfung; spätere Rechercheaufgabe.

## Vorgesehene Bibliotheken

Die Referenzen dienen zur Stackauswahl. Konkrete Versionen, APIs, minimale Rust-/Toolkit-Versionen und Lizenzkompatibilität müssen in Phase 0 geprüft und festgeschrieben werden.

- [Tokio](https://docs.rs/tokio/latest/tokio/), [clap](https://docs.rs/clap/latest/clap/), [zbus](https://docs.rs/zbus/latest/zbus/).
- [Serde](https://docs.rs/serde/latest/serde/), [rusqlite](https://docs.rs/rusqlite/latest/rusqlite/), [reqwest](https://github.com/seanmonstar/reqwest).
- [Ratatui](https://ratatui.rs/), [Crossterm](https://docs.rs/crossterm/latest/crossterm/).
- [CXX-Qt](https://kdab.github.io/cxx-qt/book/), [CXX-Qt-Quellcode](https://github.com/KDAB/cxx-qt), [Qt Quick Controls](https://doc.qt.io/qt-6/qtquickcontrols-index.html).
- [gtk-rs / GTK 4](https://gtk-rs.org/gtk4-rs/stable/latest/docs/gtk4/).
