# NCIRC - IRCd dockerisé pour newbiecontest
Ce repository contient les Dockerfile pour télécharger, compiler et installer un IRCd fonctionnel avec unrealircd et anope. Il télécharge et installe également PyLink. Il contient aussi des fichiers de configuration de base qui fonctionnent.

## Utilisation
    docker-compose up --build

## Architecture
Pour n'avoir dans le conteneur final que les binaires nécessaires à l'exécution (et pas les sources ni les outils de compilation), le Dockerfile utilise un [build multi-stage](https://docs.docker.com/develop/develop-images/multistage-build/).

L'image finale ne contient donc que le code binaire, pas la configuration. Celle-ci est dans le répertoire `config/{unrealircd,anope,pylink}` puis monté dans le conteneur. Il en est de même pour les données et pour les logs qui sont montés depuis `data/{unrealircd,anope,pylink}` et `logs/{unrealircd,anope,pylink}` respectivement.

## Trucs important à configurer
Normalement le serveur IRC et les services fonctionnent directement. Cependant, pour des questions de sécurité, il y a quelques morceaux de configuration à changer pour l'IRCd et les services, et quelques commandes pour lier le chan IRC avec le chan discord.

### Documentation de référence
- docker-compose: https://docs.docker.com/compose/compose-file/
- Configuration d'unrealircd: https://www.unrealircd.org/docs/Configuration
- Configuration d'anope: https://wiki.anope.org/index.php/2.0/Configuration
- Documentation de PyLink: https://github.com/jlu5/PyLink/tree/master/docs

### UID d'exécution du service
L'argument `hostuid=1000` dans le fichier `docker-compose.yml` indique l'UID avec lequel doit tourner l'IRCd et les services. Le faire coïncider avec l'UID des fichiers de l'hôte qui seront monté dans le conteneur est indispensable. Ne pas oublier de modifier les trois occurrences. C'est la seule chose à changer avant de lancer un :

    docker-compose build

### Certificat et clé SSL
Le certificat et la clé privée d'unrealircd doivent être placés dans les fichiers `config/unrealircd/tls/server.cert.pem` et `config/unrealircd/tls/server.key.pem` respectivement. Si nécessaire, ils peuvent être générés avec :

    cd config/unrealircd/tls
    openssl req -x509 -newkey rsa:4096 -keyout server.key.pem -out server.cert.pem -days 3650 -nodes

Il en est de même pour anope dont le certificat et la clé doivent être placés dans `data/anope/anope.crt` et `data/anope/anope.key`. Si besoin, générés avec une commande similaire.

    cd data/anope
    openssl req -x509 -newkey rsa:4096 -keyout anope.key -out anope.crt -days 3650 -nodes

Pareil pour PyLink.

    cd data/pylink
    openssl req -x509 -newkey rsa:4096 -keyout pylink-key.pem -out pylink-cert.pem -days 3650 -nodes

### Link des services
Unrealircd peut utiliser un certificat client pour authentifier les serveurs qui se connectent à lui. La méthode *SPKIFP* utilise le fingerprint de la clé publique en tant que mot de passe. La commande suivante permet de le calculer.

    openssl x509 -in data/anope/anope.crt -pubkey -noout | openssl pkey -pubin -outform DER | openssl dgst -sha256 -binary | openssl enc -base64

Cette commande produit un hash encodé en base64 qui doit être placé dans le bloc `uplink` du fichier `config/anope/services.conf`. Ainsi que dans le bloc `link` du fichier `config/unrealircd/unrealircd.conf`.

Idem pour PyLink.

    openssl x509 -in data/pylink/pylink-cert.pem -pubkey -noout | openssl pkey -pubin -outform DER | openssl dgst -sha256 -binary | openssl enc -base64

Le hash est à placer à la fois dans le fichier `config/pylink/pylink.yml` dans la config `servers::ncirc::sendpass`. Mais aussi dans le bloc `link` du fichier `config/unrealircd/unrealircd.conf`.

### Cloak keys
Les *cloak keys* servent à chiffrer les adresses IP des clients. Il est important qu'elles soient uniques et stables dans le temps pour ne pas invalider tous les bans, excepts, invex et autres masques. Elles sont dans `config/unrealircd/unrealircd.conf` dans le premier bloc `set` et peuvent être générés avec cette commande.

    docker-compose run --rm unrealircd ./unrealircd gencloak

### Seed du générateur aléatoire d'Anope
Anope utilise une seed pour son générateur aléatoire qui doit être configurée. C'est un nombre à 7 chiffres qui doit être placé dans le bloc `options` du fichier `config/anope/services.conf`.

### Confirmation d'adresse mail Anope
Anope est configuré pour envoyer un mail à chaque enregistrement de pseudo. Il envoie les mails en utilisant une commande qui doit être compatible avec `sendmail -t`. La version de `sendmail` installée dans l'image ne sait pas envoyer les mails directement et doit passer par un relais. Par exemple pour passer par le serveur SMTP de gmail en utilisant un compte gmail, il est possible de mettre cette ligne dans le fichier `config/anope/services.conf` dans le bloc `mail`.

    sendmailpath = "/usr/sbin/sendmail -t -H 'openssl s_client -quiet -verify_quiet -connect smtp.gmail.com:465' -amLOGIN -auexample@gmail.com -apTh3P4SswOrd"

L'adresse mail de login est ici `example@gmail.com` et le mot de passe du compte `Th3P4SswOrd`. L'option `-H` spécifie une autre commande avec laquelle communiquer au lieu de se connecter directement. Ici, elle sert à chiffrer la connexion. Si la commande `openssl` n'est pas utile, son installation peut être retirée du fichier `anope/Dockerfile` ligne 30.

### IRCOp
Le pseudo, les masques d'adresses IP et le mot de passe des IRCOp doivent être changés. Du côté d'unrealircd, c'est le bloc `oper` du fichier `config/unrealircd/unrealircd.conf`. Un mot de passe peut être chiffré en argon2 avec la commande :

    docker-compose run --rm unrealircd ./unrealircd mkpasswd argon2

Du côté d'Anope c'est uniquement un pseudo à changer dans le bloc `oper` qui n'est pas commenté dans le fichier `config/anope/services.conf`.

Pour enregistrer son pseudo avec NickServ, l'IRCOp doit être d'abord authentifié en tant qu'IRCOp via la commande `/oper Nick MotDePasse`.

### Admin PyLink
Le pseudo et mot de passe de l'admin PyLink doivent être configurés dans `config/pylink/pylink.yml` dans le bloc `login`. Le mot de passe chiffré peut l'être avec la commande `mkpasswd` ou avec:

    docker-compose run --rm pylink pylink-mkpasswd

### Création d'un bot discord
https://github.com/reactiflux/discord-irc/wiki/Creating-a-discord-bot-&-getting-a-token

Le bot a besoin des permissions *"Send Message"*, *"Change Nickname"* et *"Manage Webhooks"*. Soit des permissions au moins égales à `603981824`.

Le token d'accès doit être placé dans le fichier `config/pylink/pylink.yml` dans `servers::dsc::token`. Le guild ID du serveur discord de NewbieContest est `376764304202137600` et est déjà configuré dans le fichier.


## Configuration à l'exécution
Quelques trucs à configurer une fois que ça tourne.

### Chans à enregistrer
Sur `#services`, les services balance leurs logs en continue. Il devrait être accessible uniquement aux IRCOps. Le chan `#help` est référencé de manière explicite dans la configuration et devrait être enregistré et configuré proprement.

    /msg ChanServ register #services Chan de log des services
    /msg ChanServ mode #services lock add +OPps
    /msg ChanServ set noexpire #services on
    /msg ChanServ set persist #services on
    /msg ChanServ set private #services on
    
    /msg ChanServ register #help Chan d'aide
    /msg ChanServ set noexpire #help on
    /msg ChanServ set persist #help on

### Link des chans IRC et discord
Se loguer auprès du bot `PyLink` du côté IRC et créer le chan de relais. Il est conseillé d'avoir préalablement enregistré le chan IRC auprès de ChanServ.

    /msg PyLink identify Nick MotDePasse
    /msg PyLink create #newbiecontest

Se loguer après du bot du côté discord et linker le chan discord au chan relais en disant en privé au bot:

    identify Nick MotDePasse
    link ncirc #newbiecontest 379173390482800641

L'ID `379173390482800641` correspond au channel d'accueil de newbiecontest. Pour linker d'autres chans il faut récupérer leur ID, soit dans l'URL via l'interface web, soit en [activant le mode développeur](https://support.discordapp.com/hc/en-us/articles/206346498-Where-can-I-find-my-User-Server-Message-ID-).
