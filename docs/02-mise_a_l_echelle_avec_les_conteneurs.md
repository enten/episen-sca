---
marp: true
title: 02-mise_a_l_echelle_avec_les_conteneurs
description: 02-mise_a_l_echelle_avec_les_conteneurs
headingDivider: 3
paginate: true
theme: default
style: |
  section h3 {
    margin-bottom: 0.75em;
    font-size: 1.3em;
  }
  section.dark {
    background: #123;
  }
  section.dark header {
    color: #fff;
    font-size: 1em
  }
  section.dark h1,
  section.dark h2,
  section.dark h3,
  section.dark h4,
  section.dark h5,
  section.dark h6,
  section.dark p,
  section.dark ul {
    color: #fff
  }
  section.figure p {
    text-align: center;
  }
  section.figure p:last-child {
    font-size: 0.5em;
    font-style: italic;
  }
  section.references ul {
    list-style-type: none;
    padding-left: 0;
  }
  section.toc h2 {
    font-size: 1.6em;
    color: #123;
  }
  section.toc ol {
    font-size: 1.3em;
    font-weight: bold;
    line-height: 2.2em;
    padding-left: 1.2em;
  }
  section.enum > ul {
    list-style-type: none;
    padding: 0;
  }
  section.enum > ul > li {
    margin-bottom: 0.5em;
  }
  section.enum > ul > li:last-child {
    margin-bottom: 0;
  }
  section.enum > ul > li::before {
    content: '➡️';
    display: inline-block;
    margin-right: 0.1875em;
  }
  section.enum > h3 + ul {
    margin-top: 0.5em;
  }
  section.h3_margin_0 h3 {
    margin: 0;
  }
---

# Séance 2 - Mise à l'échelle avec les conteneurs

<!-- header: Scalabilité, Virtualisation et Conteneurisation -->
<!-- _class: dark -->
<!-- _paginate: false -->

## Sommaire

<!-- header: Séance 2 - Mise à l'échelle avec les conteneurs -->
<!-- _class: toc -->
<!-- _paginate: false -->

1. [Mise à l'échelle avec les conteneurs](#-1-mise-à-léchelle-avec-les-conteneurs)
2. [Docker Compose](#-2-docker-compose)
3. [Optimisation des ressources allouées aux conteneurs](#3-pourquoi-faut-il-optimiser-les-ressources-alloués-aux-conteneurs)

## <!-- fit --> 1. Mise à l'échelle avec les conteneurs

<!-- header: Séance 2 - Mise à l'échelle avec les conteneurs -->
<!-- _paginate: false -->

### La mise à l’échelle est-elle complexe ?

<!-- header: Séance 2 - Mise à l'échelle avec les conteneurs » 1. Mise à l'échelle avec les conteneurs -->
<!-- _class: enum -->

* Maîtriser le périmètre métier de l'application
* Calculer les ressources nécessaires à l’exécution de l'application
* L’applicatif est-il _stateless_ ou _statefull_ ?
* La mise à l’echelle est-elle _manuelle_ ou _automatique_ ?
* Qui opère cette mise à l’échelle ?
* Qu’est-ce qui justifie une mise à l’échelle d’un composant ?

### Mise à l’échelle manuelle de conteneurs

<!-- _class: enum -->

* Tâche répétitive
* Action manuelle
* Sujette aux erreurs humaines
* Inadaptée pour un grand nombre de conteneurs à démarrer dans un laps de temps réduit

![bg right 85%](./images/02-mise_a_l_echelle_avec_les_conteneurs/je_lance_n_fois_docker_run.png)

### Mise à l'échelle automatique de conteneurs (1)

<!-- _class: enum -->

* [Docker Compose](https://docs.docker.com/compose/) pour **exécuter des applications multi-conteneurs**
* Approche déclarative basée sur du code donc traçable dans le temps
* Utilisable pour l’ensemble des composants d'une application
* Réplication gérée par [Compose](https://docs.docker.com/compose/)

![bg right 85%](./images/02-mise_a_l_echelle_avec_les_conteneurs/et_si_on_utilisait_un_orchestrateur.png)

### Mise à l'échelle automatique de conteneurs (2)

<!-- _class: enum -->

* Traçabilité des actions
* Approche déclarative
* Scalabilité réalisée par l’orchestrateur
* Automatisation des processus
* Nécessite une montée en compétences des équipes devops
* Déploiement des conteneurs sur des multiples hosts possible

![bg right 85%](./images/02-mise_a_l_echelle_avec_les_conteneurs/si_jetais_vous_jutiliserais_un_ordchestrateur.png)

### Mauvaises pratiques pour la mise à l'échelle

<!-- _class: enum -->

* Négliger le dimensionnement de l’infrastructure du cluster de l’orchestrateur (Swarm, k8s)
* Mauvaise définition des rôles et responsabilités devops
* Ne pas connaître les besoins systèmes des composants applicatifs
* Omettre la maintenance du cluster et ses versions

## <!-- fit --> 2. Docker Compose

<!-- header: Séance 2 - Mise à l'échelle avec les conteneurs -->
<!-- _paginate: false -->

### Introduction

<!-- header: Séance 2 - Mise à l'échelle avec les conteneurs » 2. Docker Compose -->
<!-- _class: enum -->

* Outil faisant partie de l’écosystème Docker
* Définition, exécution de conteneurs dans N environnements isolés sur un nœud
* Fichier de description se basant sur un langage standard: YAML

![bg right 75%](./images/02-mise_a_l_echelle_avec_les_conteneurs/octopus.png)

### Cas d’utilisations

<!-- _class: enum h3_margin_0 -->

#### Environnement de développement simplifié

* Guide d’un nouvel arrivant simplifié
* Simuler un environnement isolé de bout-en-bout (application, database, cache)
* Gain de temps et de productivité pour l’équipe de développement

#### Environnement de test automatisé

* Exécution simplifié des tests end-to-end
* Mise en place d’une stack technique sur demande pour l’exécution des tests

#### Déploiement

* 1 nœud local et/ou distant
* 1 cluster Swarm

### Exemple de Docker Compose

```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - ".:/code"
      - "logvolume:/var/log"
    links:
      - redis
  redis:
    image: redis:latest
volumes:
  logvolume: {}
```

## 3. Pourquoi faut-il optimiser les ressources alloués aux conteneurs

<!-- header: Séance 2 - Mise à l'échelle avec les conteneurs -->
<!-- _paginate: false -->

### Contexte

<!-- header: Séance 2 - Mise à l'échelle avec les conteneurs » 3. Pourquoi faut-il optimiser les ressources alloués aux conteneurs -->
<!-- _class: enum -->

* Un conteneur peut avoir besoin de : Stockage, CPU, RAM
* Une fuite mémoire dans l’application s’exécutant dans le conteneur
* Une attaque extérieur provoquant un accroissement des ressources systèmes
* Mauvaise configuration des ressources systèmes pour un conteneur X

### Les risques du mode « no-limit »

<!-- _class: enum -->

#### Out of Memory Exception

* Arrêt de processus « important » par le kernel Unix
* Indisponibilité de l’application en Production
* Instabilité de la plateforme de Production

#### Accès illimité aux cycles CPUs d’une machine hôte (virtuelle, physique)

* Instabilité de la machine hôte
* Effets de bords sur les autres conteneurs hébergés sur la machine hôte

### Bonnes pratiques

<!-- _class: enum -->

* Connaître les besoins en mémoire de son application via des phases de tests
* Limiter les ressources mémoire nécessaire au bon fonctionnement de l’application
* Ajuster la configuration de la SWAP sur les hôtes Docker
* Définir des limites sur les accès aux cycles CPUs de la machine hôte

## Références

<!-- header: "" -->
<!-- _class: dark references -->
<!-- _paginate: false -->

* Docker Compose<br>https://docs.docker.com/compose
* Docker Compose CLI reference<br>https://docs.docker.com/engine/reference/commandline/compose/#child-commands
* Docker Compose file specification<br>https://docs.docker.com/compose/compose-file
* Dockerfile reference<br>https://docs.docker.com/engine/reference/builder
* Moby<br>https://mobyproject.org
