# Applications installées — inventaire et utilité

> Inventaire de tout ce qui tourne sur le serveur, à quoi ça sert, et où c'est configuré.
> Anonymisé : pas de domaine, pas d'IP publique, pas de secrets.

## Conteneurs Docker (stack principale)

| Conteneur | Utilité | Détails |
|---|---|---|
| **caddy** *(hôte, pas conteneur)* | Frontal HTTPS | reverse proxy 80/443, certificats automatiques, basic auth sur l'interface de supervision. Config : `/etc/caddy/Caddyfile`. |
| **n8n_workflow** | Automatisation | moteur de workflows (webhooks, crons, alertes Telegram, GitHub, Discord). v2.39.6, port local 5678, derrière le frontal. |
| **n8n_db** | Base de n8n | PostgreSQL 17 — workflows, credentials, historique. Les tokens y sont stockés (extraction en variable shell au besoin). |
| **umami_app** | Web analytics | statistiques de visite (auto-hébergé, RGPD-friendly), port local 3000. |
| **umami_db** | Base d'Umami | PostgreSQL 16. |
| **ollama** | IA locale | exécution de modèles LLM en local (7b/3b) pour le bot IA et la génération de descriptions. RAM libérée chaque soir (timer). |
| **netdata** | Supervision temps réel | métriques système + conteneurs, UI locale protégée par basic auth. |

## Sécurité (hôte)

| Élément | Utilité |
|---|---|
| **UFW** | pare-feu : uniquement 22/80/443 ouverts |
| **fail2ban** | bannissement auto des IP qui forcent SSH (jails `sshd` + `recidive`) |
| **SSH par clé** | mot de passe désactivé (`passwordauthentication=no`) |
| **VPN mesh (Tailscale)** | accès privé VPS ↔ PC, utilisé notamment pour les sauvegardes |

## Sauvegardes

| Élément | Utilité |
|---|---|
| **kopia-backup.sh** (dim 19:30) | snapshot chiffré : volumes Docker, home, configs Caddy → dépôt sur le PC personnel |
| **vps-backup-on-demand.sh** | sauvegarde à la demande (commande Telegram, démarre le serveur du PC si besoin) |
| **kopia-staging.sh** | prépare les dumps (bases de données, configs) avant snapshot |

## Bots & supervision (hôte)

| Service / script | Utilité |
|---|---|
| **vps-tgbot.service** | bot Telegram de contrôle : `/backup`, `/status`, `/ordres`, `/ok` ; tout message libre = instruction pour l'assistant ; **photos/documents** sauvegardés dans `/home/ubuntu/ORDRES-files/` |
| **vps-watchdog.sh** (15 min) | anomalies → alerte Telegram (conteneurs, HTTPS, disque, fail2ban, backup). PC éteint ≠ anomalie |
| **vps-metrics-report.sh** (8h00) | rapport quotidien Telegram : RAM, disque, conteneurs, HTTPS, backup, IA |
| **github-weekly-report.sh** (ven 19:00) | rapport hebdo GitHub → repo `git-ops-journal` |
| **vps-daily-journal.sh** (23h00) | ce journal quotidien (résumé du jour → repo courant) |

## Workflows n8n actifs (vue rapide)

Détail complet et calendrier : repo `git-ops-journal`, fichier `docs/workflows-semaine.md`.

- **Agent Telegram** (7h00) : bot IA + radar de cadence des dépôts
- **GitHub — descriptions auto** (7h45) + **deps/PR** (lun 22h00)
- **Diffusion Actus / Photos Discord** (3×/j chacun)
- **Push GitHub** (webhook, utilitaire)
- **Alertes VPS** (webhook du watchdog)
- **Stripe / Tally ×2** (webhooks d'alerte, signatures vérifiées)
