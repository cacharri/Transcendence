
# ft_transcendence

🎮 **Multiplayer Pong platform with tournaments, chat, AI opponents, dashboards, hardened security and full observability — micro-services & Docker-based.**  
Final Common Core project at **42 Madrid**.

---

## 📍 Overview

**ft_transcendence** is a web platform for organising and playing real-time Pong tournaments.  
The pedagogical challenge was to master unfamiliar technologies, design a complete system and secure its containerised deployment.

Key capabilities

* Registration & login (OAuth 2.0 · JWT · TOTP 2FA)  
* Tournament bracket with automatic matchmaking & leaderboard  
* Live 1-v-1 matches against remote players or an AI bot  
* In-game chat  
* Dashboards with player & match statistics  
* Basic game-customisation options  
* GDPR compliance (anonymisation and account deletion)  
* WAF, Vault, HTTPS, centralised logging, metrics & alerts  
* Micro-services backend (Fastify)  
* Server-Side Rendering (SSR) for accessibility & SEO  

---

## 📦 Implemented subject modules

| Area                 | Type       | Module implemented                                                   |
|----------------------|------------|---------------------------------------------------------------------|
| **Web**              | Principal  | Backend framework (Fastify)                                         |
|                      | Minor      | Database (PostgreSQL)                                               |
| **User management**  | Principal  | Standard user mgmt + auth + tournament users                        |
|                      | Principal  | Remote authentication (OAuth 2.0)                                   |
| **Gameplay / UX**    | Minor      | Game-customisation options                                          |
| **AI-Algorithm**     | Minor      | User & game statistics dashboards                                   |
| **Cyber-security**   | Principal  | WAF (ModSecurity) + Vault                                           |
|                      | Minor      | GDPR helpers (anonymise, local data mgmt, account deletion)         |
|                      | Principal  | 2FA + JWT                                                           |
| **DevOps**           | Principal  | Log infrastructure (Elastic)                                        |
|                      | Minor      | Monitoring (Metricbeat + Kibana)                                    |
|                      | Principal  | Backend split into micro-services                                   |
| **Accessibility**    | Minor      | Multi-device support                                                |
|                      | Minor      | Extended browser compatibility                                      |
|                      | Minor      | SSR                                                                 |
| **Server-side Pong** | Principal  | Pong engine on server + REST / WebSocket API                        |

---

## 🛠️ Tech stack

| Layer             | Technologies                                                                                |
|-------------------|---------------------------------------------------------------------------------------------|
| **Frontend**      | TypeScript SPA (SSR via Fastify-view)                                                       |
| **Backend**       | Fastify (Node.js) × micro-services — REST + WebSocket                                       |
| **Database**      | SQLite 3                                                                                  |
| **Authentication**| OAuth 2.0 · JWT · TOTP-based 2FA                                                            |
| **AI Bot**        | Custom algorithm (trajectory prediction + difficulty scaling)                               |
| **Security**      | ModSecurity (OWASP CRS) · HashiCorp Vault · HTTPS                                   |
| **DevOps**        | Docker · Docker Compose · Makefile wrappers                                                 |
| **Observability** | ElasticSearch · Logstash · Kibana · Metricbeat                                             |
| **Compliance**    | GDPR helper scripts (anonymise, delete, export)                                             |

---

## ⚙️ Quick-start

1 · Prerequisites  
&nbsp;&nbsp;• Docker ≥ 24  
&nbsp;&nbsp;• Docker Compose v2  
&nbsp;&nbsp;• GNU Make  

2 · Environment variables  
    cp src/.env .env  
    nano .env    # adjust ports, domains, DB creds, OAuth keys…  

3 · Build & Run  
    make         # alias for ‘make up’ → build + up -d  

    ⌛ Wait until all services are **healthy** (Elastic & Vault ≈ 1 min)  
    App URL → https://localhost:<FRONT_PORT>  

4 · Stop & Clean  
    make down    # docker compose down -v --remove-orphans  
    make clean   # prune dangling images/volumes (optional)

---

## 🔐 Security at a glance

| Measure  | Details                                                                       |
|----------|--------------------------------------------------------------------------------|
| Hashing  | Argon2id + random salt for passwords                                           |
| Tokens   | JWT signed with RSA keys stored in Vault                                       |
| 2FA      | TOTP (Google Authenticator compatible)                                         |
| Headers  | CSP, HSTS, and CSRF protection on every route                                  |
| WAF      | ModSecurity with fine-tuned OWASP CRS                                          |
| Secrets  | Mounted as `tmpfs` (in-memory) volumes in production                           |

---

## 🖥️ Observability

| Component      | URL / Purpose                                     |
|----------------|---------------------------------------------------|
| Logstash       | Centralises stdout/stderr of every container      |
| ElasticSearch  | Stores structured logs                            |
| Kibana         | http://localhost:5601 — ready-to-use dashboard    |
| Metricbeat     | Ships host & container metrics                    |
| Alerts         | script/email_report.sh sends critical reports     |
