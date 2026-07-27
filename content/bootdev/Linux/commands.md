+++
date = '2026-07-28T00:19:21+02:00'
draft = true
title = 'Commands'
+++

Durch den Befehl git reset --hard HEAD vorhin im Theme-Ordner wurden alle Dateien auf den Stand des letzten Commits des Themes zurückgesetzt. Da das Submodule lokal aber noch nicht komplett synchronisiert war, hat Git den Ordner geleert. Mit den beiden Befehlen oben lädt Git die exakten Designdateien von Blowfish jetzt wieder in deinen Ordner themes/blowfish.

Damit nimmst du die "Untracked files" (deine Inhalte, Konfigurationen und die neue Sprachdatei) mit ins Paket auf:
git add .

Den ersten Commit erstellen
git commit -m "Erster sauberer Commit mit Favicons und Changelog"
