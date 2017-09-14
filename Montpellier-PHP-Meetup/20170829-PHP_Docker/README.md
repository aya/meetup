# PHP Docker meetup

Présentation de l'utilisation de Docker & Consul à Montpellier le 29/08/2017.

## Présentation

La présentation est disponible [içi](https://docs.google.com/presentation/d/1TkN4aWNThSEQXZiZNn4-O6Vr_ApAHQqX41LgBcD54Co).

L'objectif de la présentation est de déclarer automatiquement des services dans consul à travers registrator.
Lorsqu'un service se lance, il va s'enregistrer dans consul et déclarer une URL utilisée par fabio pour rediriger le trafic entrant correspondant à cette URL vers toutes les instances de ce service. 

## Prérequis

Vous devez créer un docker network 'demo' avant de lancer vos services.

```
# docker network create demo
```

Vous devez ajouter une entrée dans votre fichier /etc/hosts pour accéder au service 'demo-web' depuis votre navigateur.

```
# echo '127.0.0.1 demo.local' >> /etc/hosts
```

## Installation

Lancer les services communs à tous les projets dans le répertoire 'services'.

```
# cd services && docker-compose up -d
```

Lancer les services du projet 'demo' dans le répertoire 'demo'.

```
# cd ..
# cd demo && docker-compose up -d
```

## Tests

Vous pouvez accéder au service `mysql` (docker demo-mysql) depuis le service `web`.

```
# docker-compose exec web ping -c 4 mysql
```

Vous pouvez lancer plusieurs instances du service web.

```
# docker-compose scale web=3
```

Vous pouvez accéder à toutes les instances du service `web` à travers le service `fabio` (docker demo-fabio).

```
# curl -H 'Host: demo.local' localhost
<html><head><title>Demo @Meetup</title></head><body>Welcome @ 680fa2d0031c !</body></html>
# curl -H 'Host: demo.local' localhost
<html><head><title>Demo @Meetup</title></head><body>Welcome @ a7719f718dfd !</body></html>
# curl -H 'Host: demo.local' localhost
<html><head><title>Demo @Meetup</title></head><body>Welcome @ a8e26d0e2291 !</body></html>
```

