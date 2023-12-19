# abes-hello-docker

[![Docker Pulls](https://img.shields.io/docker/pulls/abesesr/abes-hello.svg)](https://hub.docker.com/r/abesesr/abes-hello/)

Configuration pour le déploiement docker de l'application abes-hello ("Hello World" illustrant la [politique de dev de l'Abes](https://github.com/abes-esr/abes-politique-developpement)).

Les URLs de l'application abes-hello sont les suivantes :
- en dev :  https://hello-dev.abes.fr/
- en test : https://hello-test.abes.fr/
- en prod : https://hello.abes.fr/

A noter que les codes sources de l'application abes-hello sont hébergés sur ces deux dépôts :
- https://github.com/abes-esr/abes-hello-back/
- https://github.com/abes-esr/abes-hello-front/

## Installation

```bash
cd /opt/pod/
git clone https://git.abes.fr/depots/abes-hello-docker.git

cd /opt/pod/abes-hello-docker/
cp .env-dist .env
# personnalisez alors le .env en partant des valeurs exemple présentes dans le .env-dist:
#   - sur quel serveur on déploie, dev test ou prod
#   - crontab pour le batch
#   - URL du back pour que le front puisse l'appeler
```

La personnalisation du ``.env`` se fait à partir d'une copie de ``.env-dist`` qui contient des [commentaires qui expliquent toutes les variables disponnibles que l'on peut personnaliser](./.env-dist).

## Démarrage et arret

Pour démarrer l'application :
```bash
cd /opt/pod/abes-hello-docker/
docker compose up -d
```

L'application va alors écouter sur les ports 7080 (pour le front) et 7081 (pour le back). Remarques: le ports peuvent être personnalisés dans le ``.env``.

Exemple d'URL une fois déployé en local :
- Le front : http://127.0.0.1:7080/
- Le back : http://127.0.0.1:7081/v1/api/

Exemple d'URL une fois déployé sur le serveur diplotaxis-test.v202.abes.fr :
- Le front : http://diplotaxis-test.v202.abes.fr:7080/
- Le back : http://diplotaxis-test.v202.abes.fr:7081/v1/api/

Pour arrêter l'application :
```bash
cd /opt/pod/abes-hello-docker/
docker compose stop
```

## Supervision

Pour vérifier que l'application est démarrée, on peut consulter l'état des conteneurs :
```bash
cd /opt/pod/abes-hello-docker/
docker compose ps
```

Pour vérifier que l'application est bien lancée, on peut consulter les logs :
```bash
cd /opt/pod/abes-hello-docker/
docker compose logs --tail=50 -f
```
A noter que ces logs sont envoyées automatiquement au puits de log de l'Abes à l'aide du client filebeat installé sur le noeud docker et à la configuration que nous lui indiquons dans les labels (cf docker-compose.yml).


## Mise à jour de l'application

Pour mettre à jour l'application cela suppose qu'une nouvelle version du code ait été publiée. Le fait de publier une nouvelle version va généré l'image docker correspondante dans dockerhub. On doit alors l'indiquer en modifiant le numéro de version dans le [``docker-compose.yml``](https://github.com/abes-esr/abes-hello-docker/blob/develop/docker-compose.yml) de ce présent dépôt. Dans de rares cas (release d'une version majeure) il est éventuellement nécessaire d'ajouter les variables d'environnement dans ``/opt/pod/abes-hello-docker/.env`` en prenant exemple sur [``/opt/pod/abes-hello-docker/.env-dist``](https://github.com/abes-esr/abes-hello-docker/blob/develop/.env-dist).

Ensuite il suffit de lancer la commande suivante qui aura pour effet de recréer tous les conteneurs dont la nouvelle version a été indiquée dans le [``docker-compose.yml``](https://github.com/abes-esr/abes-hello-docker/blob/develop/docker-compose.yml) :
```bash
cd /opt/pod/abes-hello-docker/
docker compose up -d
```


## Sauvegardes et restauration

Pas besoin de sauvegarder l'application. C'est une application de type hello world qui ne contient aucune données importantes.

## Architecture

TODO.
