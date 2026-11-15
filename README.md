# VPS-Rennesdev.fr

Journal de bord automatisé d'un VPS : un fichier Markdown par jour, poussé automatiquement chaque soir sur ce dépôt.

## Contenu

- `jours/YYYY-MM-DD.md` — 45 fichiers, du `2026-09-17.md` au `2026-11-14.md`. Chaque fichier est le journal du jour correspondant (état général, CPU, RAM, disque, conteneurs Docker, etc., horodaté en UTC).
- `docs/applications.md` — inventaire des applications installées sur le serveur.
- `README.md` — ce fichier.

## Fréquence du commit automatique

D'après l'historique git, un commit « Journal YYYY-MM-DD (auto) » est poussé chaque jour à 22:00 UTC (23h00 Europe/Paris).
