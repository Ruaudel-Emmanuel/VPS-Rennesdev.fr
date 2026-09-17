# VPS — journal de bord

> **Repo privé** : tout ce qui concerne le serveur personnel — applications installées
> et leur utilité, résumé quotidien des actions (installations, modifications, incidents).
> Mis à jour **automatiquement chaque jour à 23h00** (Europe/Paris) par le VPS lui-même.

## 📚 Structure

| Fichier | Rôle |
|---|---|
| `README.md` | ce fichier |
| `docs/applications.md` | inventaire des applications installées + leur utilité |
| `jours/AAAA-MM-JJ.md` | résumé du jour (généré auto à 23h00, un fichier par jour) |

## 🔒 Règles d'anonymat

1. **Aucun secret** : pas de token, clé, mot de passe — même partiel.
2. Pas de nom de domaine, pas d'adresse IP publique, pas d'identifiants de compte
   (placeholders neutres : `<domain>`, `<ip>`, `<user>`).
3. Les IP privées/Tailscale en `100.x` sont tolérées (non routables, nécessaires au journal).

## 🗓️ Résumé quotidien automatique

Chaque jour à **23h00 Europe/Paris**, un script du VPS (timer systemd, indépendant de n8n)
collecte ce qui s'est passé dans la journée et publie `jours/<date>.md` ici :

- paquets APT installés / mis à jour
- conteneurs Docker : état du soir + **différence avec la veille** (démarré/arrêté/changé)
- scripts et fichiers modifiés dans `/usr/local/bin`
- timers / services systemd créés ou modifiés
- connexions SSH du jour + bans fail2ban
- commits git locaux (projets du serveur)

Script : `/usr/local/bin/vps-daily-journal.sh` (root 700, aucun secret en dur — le token
GitHub est extrait de la base n8n au moment du run, jamais affiché ni persisté).
