## Structure du projet

- `vm/vm-app/` : Services principaux (Traefik, Keycloak, WordPress)
- `vm/vm-logging/` : Stack de logging (Traefik, Elasticsearch, Kibana, Filebeat)
- `vm/vm-monitoring/` : Stack de monitoring (Traefik, Uptime-kuma)

## Lancement des services

### 1. Configurer les fichiers `.env`
A partir des `.env.example`, remplacez les variables dans les fichiers `.env` correspondants.

### 2. Générer les certificats SSL
```sh
openssl genrsa -out ca.key 4096
openssl req -x509 -new -nodes -key ca.key -sha256 -days 3650 -out ca.crt
```
Placez `ca.crt` dans les dossiers le dossier `certs` de chaque VM.

Ensuite, générez les certificats pour Traefik dans chaque VM :
```sh
openssl genrsa -out traefik.key 2048
openssl req -new -key traefik.key -out traefik.csr
openssl x509 -req \
  -in traefik.csr \
  -CA ca.crt \
  -CAkey ca.key \
  -CAcreateserial \
  -out traefik.crt \
  -days 365

```

### 3. Lancer les services
#### a. VM App

```sh
cd vm/vm-app/traefik
docker-compose up -d

cd ../keycloak
docker-compose up -d

cd ../wordpress
docker-compose up -d
```

#### b. VM Logging

```sh
cd vm/vm-logging/traefik
docker-compose up -d

cd ../logging
docker-compose up -d
```

#### c. VM Monitoring

```sh
cd vm/vm-monitoring/traefik
docker-compose up -d
```

### 4. Accéder aux services
#### a. VM App
- **Traefik** : https://traefik.local
- **WordPress** : https://wordpress.local
- **Keycloak** : https://traefik.local

#### b. VM Logging
- **Traefik** : https://traefik.logging
- **Kibana** : https://kibana.logging
- **Elasticsearch** : https://elasticsearch.logging

### c. VM Monitoring
- **Traefik** : https://traefik.monitoring
- **Uptime-kuma** : https://uptimekuma.monitoring

<br>

*Pour toute question, veuillez ne pas me contacter.*