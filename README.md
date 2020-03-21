# NCIRC - IRCd dockerisé pour newbiecontest
Ce repository contient le Dockerfile pour télécharger et compiler unrealircd. Il contient aussi des fichiers de configuration de base qui fonctionnent.

## Utilisation
    docker-compose up

## Architecture
Pour n'avoir dans le conteneur final que les binaires nécessaires à l'exécution (et pas les sources ni les outils de compilation), le Dockerfile utilise un [build multi-stage](https://docs.docker.com/develop/develop-images/multistage-build/).

L'image finale ne contient donc que le code binaire, pas la configuration. Celle-ci est dans le répertoire `config/unrealircd` monté dans le conteneur. Il en est de même pour les données et pour les logs qui sont montés depuis `data/unrealircd` et `logs/unrealircd` respectivement.

## Trucs important à configurer
### Documentation de référence
- docker-compose: https://docs.docker.com/compose/compose-file/
- Configuration d'unrealircd: https://www.unrealircd.org/docs/Configuration

### UID d'exécution du service
L'argument `hostuid=1000` dans le fichier `docker-compose.yml` indique l'UID avec lequel doit tourner l'IRCd. Le faire coïncider avec l'UID des fichiers de l'hôte qui seront monté dans le conteneur.

### Certificat et clé SSL
Le certificat et la clé privée d'unrealircd doivent être placés dans les fichiers `config/unrealircd/tls/server.cert.pem` et `config/unrealircd/tls/server.key.pem` respectivement. Si nécessaire, ils peuvent être générés avec :

    cd config/unrealircd/tls
    openssl req -x509 -newkey rsa:4096 -keyout server.key.pem -out server.cert.pem -days 3650 -nodes

### Cloak keys
Les *cloak keys* servent à chiffrer les adresses IP des clients. Il est important qu'elles soient uniques et stables dans le temps pour ne pas invalider tous les bans, excepts, invex et autres. Elles sont dans `config/unrealircd/unrealircd.conf` dans le premier bloc `set` et peuvent être générés avec cette commande.
    docker-compose run --rm unrealircd ./unrealircd gencloak

### IRCOp
Le pseudo, les masques d'adresses IP et le mot de passe doivent être changés. Un mot de passe peut être chiffré en argon2 avec la commande :

    docker-compose run --rm unrealircd ./unrealircd mkpasswd argon2
