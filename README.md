# Monitoring leger pour une VM OVH

Cette base installe une stack simple et peu gourmande:

- `Uptime Kuma` pour verifier que tes services repondent
- `Beszel` pour surveiller CPU, RAM, disque, reseau et Docker

## Pourquoi cette stack

- Elle reste legere pour une VM avec `6 vCores`, `12 Go RAM` et `100 Go SSD`
- Elle couvre a la fois la disponibilite et la sante de la machine
- Elle evite une stack plus lourde comme `Prometheus + Grafana + Loki`

## Fichiers

- `compose.yaml`: demarre `Uptime Kuma` et le hub `Beszel`
- `compose.agent.yaml`: demarre l'agent local `Beszel` une fois les cles obtenues
- `.env.example`: variables a adapter

## Demarrage

1. Copier l'exemple d'environnement:

```powershell
Copy-Item .env.example .env
```

2. Demarrer `Uptime Kuma` et le hub `Beszel`:

```powershell
docker compose up -d
```

3. Ouvrir les interfaces:

- `Uptime Kuma`: `http://IP_DE_TA_VM:3001`
- `Beszel`: `http://IP_DE_TA_VM:8090`

## Configuration de Beszel

1. Cree ton compte admin dans l'interface `Beszel`
2. Ajoute un nouveau systeme
3. Recupere le `TOKEN` et la `KEY` fournis par `Beszel`
4. Renseigne-les dans `.env`
5. Lance l'agent local:

```powershell
docker compose -f compose.yaml -f compose.agent.yaml up -d
```

6. Dans `Beszel`, utilise ce chemin comme host pour le systeme local:

```text
/beszel_socket/beszel.sock
```

## Monitors Uptime Kuma a creer en premier

- `HTTP` sur ton instance `Coolify`
- `HTTP` sur tes sites ou API publies
- `TCP` ou `Steam Game Server` sur tes deux jeux selon le type de serveur
- `Ping` sur l'IP de la VM

## Reglages conseilles pour rester leger

- `Uptime Kuma`: intervalle de `60s`
- `Beszel`: garder uniquement l'agent local
- Eviter de monitorer des dizaines d'URL inutiles
- Ne pas exposer `Beszel` publiquement sans protection

## Conseils de securite

- Passe `Beszel` derriere un domaine protege, VPN, ou au minimum une authentification forte
- Garde `Uptime Kuma` avec un compte admin et des notifications `Discord` ou `Telegram`
- Sauvegarde les dossiers `data/uptime-kuma` et `data/beszel`

## Sources officielles

- Uptime Kuma: <https://github.com/louislam/uptime-kuma>
- Beszel getting started: <https://beszel.dev/guide/getting-started>
- Beszel env vars: <https://beszel.dev/guide/environment-variables>
# monitoring
