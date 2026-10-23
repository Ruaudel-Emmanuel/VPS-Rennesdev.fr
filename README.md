# 🖥️ VPS RennesDev — Journal de bord automatisé

> **Journal de bord public et automatisé** d'un VPS OVH auto-géré.  
> Mis à jour **chaque jour à 23h00** — sans intervention humaine.

---

## 🎯 Pourquoi ce journal existe

### Le problème
Quand on gère un VPS seul, on oublie ce qu'on a installé, configuré, modifié.  
Les incidents surviennent sans qu'on sache exactement ce qui a changé.

### La solution
Un **journal automatique** qui trace chaque jour :

| Ce que tu ne veux pas | Ce que ce journal fait |
|---|---|
| ❌ « C'était quand que j'avais installé Docker ? » | ✅ Tout est daté, fichier par jour |
| ❌ « Qu'est-ce qui a changé entre hier et aujourd'hui ? » | ✅ Conteneurs : diff jour/jour |
| ❌ « Est-ce que les logs fail2ban sont normaux ? » | ✅ Tentatives SSH + bans listés |
| ❌ « Je dois monitorer manuellement ? » | ✅ Généré auto à 23h00, commit sur GitHub |

---

## 🏗️ Architecture du VPS

```
VPS OVH — Ubuntu 26.04 LTS
├── 4 vCPU · 7,6 Go RAM · 72 Go disque
├── Cloudflare (proxy Orange, Full Strict) + Caddy (ACME Let's Encrypt)
├── UFW : 22/80/443 · fail2ban (sshd + recidive) · SSH clé uniquement
├── Docker (réseau apps) :
│   ├── n8n v2.41+     → Workflows automation
│   ├── Ollama          → IA locale (qwen2.5:7b, 3b, llama3.2)
│   ├── Browserless     → Navigation web headless
│   ├── FileBrowser     → Gestion fichiers web
│   ├── Netdata         → Monitoring temps réel
│   ├── Uptime-Kuma     → Uptime monitoring
│   ├── Nav.rennesdev   → Suivi projets
│   ├── SPECTRE         → Analyse web
│   ├── Umami           → Analytics (PG 16)
│   └── PG 17           → Base n8n
├── Backups : Kopia → PC local (TLS, hebdo dimanche 19h30)
└── Timers systemd : watchdog, metrics, ollama-unload, backup, github-weekly…
```

---

## 📋 Compétences démontrées

| Compétence | Réalisation |
|---|---|
| **🔧 Administration Linux** | Ubuntu 26.04, UFW, fail2ban, systemd timers, SSH hardening |
| **🐳 Docker Compose** | Orchestration multi-conteneurs (10 services) |
| **☁️ Cloudflare + Caddy** | Proxy inversé, SSL automatique, DNS |
| **🤖 Automation (n8n)** | 10+ workflows : audit GitHub, alertes paiement, veille codeur |
| **🧠 IA locale (Ollama)** | Serveur LLM privé, keep-alive optimisé, 3 modèles |
| **💾 Backups (Kopia)** | Sauvegarde chiffrée incrementale vers PC local |
| **📊 Monitoring** | Netdata, Uptime-Kuma, watchdog 15 min, alertes Telegram |
| **🔐 Sécurité** | Fail2ban, audit de ports, rotation PAT, isolation Docker |

---

## 📂 Structure du dépôt

```
VPS-Rennesdev.fr/
├── README.md                     ← Ce fichier
├── docs/applications.md          ← Inventaire complet des apps installées
└── jours/2026-10-20.md           ← Journal jour/jour (généré auto)
```

### Ce que chaque journal contient

- ✅ Paquets APT installés ou mis à jour
- ✅ Conteneurs Docker : état du soir + diff avec la veille
- ✅ Scripts et fichiers modifiés dans `/usr/local/bin`
- ✅ Timers / services systemd créés ou modifiés
- ✅ Connexions SSH + bans fail2ban
- ✅ Commits git locaux (projets serveur)

---

## 🔒 Règles d'anonymat

Ce dépôt est **public** — aucun secret ne fuit :
- ❌ Pas de token, clé, mot de passe (même partiel)
- ❌ Pas de domaine brut, pas d'IP publique
- ❌ Pas d'identifiants de compte
- ✅ IP privées Tailscale `100.x` tolérées (non routables)
- ✅ Le token GitHub est extrait de la base n8n au moment du run, jamais persisté

---

## ⚙️ Technique

- **Script** : `/usr/local/bin/vps-daily-journal.sh` (root 700)
- **Déclencheur** : timer systemd (23h00 Europe/Paris)
- **Push** : via webhook n8n (PAT extrait de la DB, jamais écrit en dur)
- **Publié sur** : [github.com/Ruaudel-Emmanuel/VPS-Rennesdev.fr](https://github.com/Ruaudel-Emmanuel/VPS-Rennesdev.fr)

---

## 👤 À propos

Géré par [Emmanuel Ruaudel](https://github.com/Ruaudel-Emmanuel) — freelance automation & DevOps.  
Ce dépôt est la **preuve publique** d'une infrastructure VPS professionnelle, automatisée et documentée en temps réel.
