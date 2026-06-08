---
title: "Instancio & Java : Générer des Objets Java aléatoires pour vos tests"
date: 2025-03-03T09:30:00+01:00
categories: ["Hard Skills"]
tags: ["Java", "Instancio", "Tests", "JUnit"]
url: "/instancio-java/"
description: "Instancio permet de générer automatiquement des objets Java avec des données aléatoires, en une seule ligne. Un gain de temps considérable pour vos tests unitaires."
---

## Un constat...

Il est parfois tentant de définir des méthodes de ce genre pour ses tests unitaires :

```java
Car dacia = new Car();
dacia.setSeats(4);
dacia.setColor("Red");
dacia.setWeight(1400);
dacia.setOwner("John Doe");
// ...
```

Et d'englober ça dans une méthode statique, puis de s'en servir pour ses tests.

Avec Instancio, il est possible d'éviter cela et d'utiliser une unique ligne pour votre test :

```java
// Given
Car c = Instancio.of(Car.class).create();

// Suite du test
```

Votre objet `Car` sera instancié avec toutes ses propriétés et sous-propriétés de façon aléatoire.

Imaginez maintenant le gain de lignes et de temps si l'objet Car disposait de 50+ propriétés !

## Fonctionnalités

### Génération basique d'objets

```java
// Génère une voiture avec des données aléatoires
Car car = Instancio.of(Car.class).create();
```

### Génération de collections

```java
// Génère une liste de 5 voitures aléatoires
List<Car> cars = Instancio.ofList(Car.class).size(5).create();
```

### Spécifier des données à la génération

```java
// Génère une voiture avec la couleur blanche uniquement
Car car = Instancio.of(Car.class).set(field(Car::getColor), "White").create();

// Génère une voiture avec un propriétaire respectant le pattern <nom prénom>
Car car = Instancio.of(Car.class).generate(field(Car::getOwner), gen -> gen.text().pattern("#s #s")).create();
```

### Validation Jakarta & JPA

Par défaut, Instancio génère des données aléatoires sans respecter les éventuelles contraintes jakarta de validation.

Il est possible d'activer la validation des beans via une [configuration spécifique](https://www.instancio.org/user-guide/#bean-validation).

Il est même possible de générer des données respectant les contraintes spécifiées par les [annotations JPA](https://www.instancio.org/user-guide/#jpa).

## Je trouve cela notamment très intéressant pour…

### … des tests de mappers MapStruct

En couplant AssertJ à Instancio, on peut réaliser des tests de non-régression très complets en 3 lignes :

```java
// Given
Car expected = Instancio.of(Car.class).create();

// When
CarDto actual = mapper.convertEntityToDto(expected);

// Then
assertThat(actual).usingRecursiveComparison().isEqualTo(expected);

/*
 * La comparaison récursive d'AssertJ sur des types différents
 * permet de rapidement spotter des suppressions ou renommages de champs,
 * évitant ainsi des problèmes de mapping.
 * Jouez avec withEnumStringComparison(), ignoringFields(...), etc.
 * pour bien gérer la granularité de votre test de mapper.
 */
```

### … des tests unitaires de service

Pour avoir un objet cohérent et dynamique en entrée de la couche de service.

Mais ce ne sont pas les seules applications possibles d'Instancio, qui sont très nombreuses !

## Limites

Attention cependant, les objets générés par Instancio étant aléatoires, il ne faut pas créer involontairement des [*flaky tests*](https://www.jetbrains.com/teamcity/ci-cd-guide/concepts/flaky-tests/).

Via IntelliJ par exemple, pour éviter ce souci, on peut exécuter un test N fois ou *until failure*.

Imaginons que votre objet généré via Instancio dispose d'un flag booléen ayant une influence sur l'exécution de votre méthode de service : lancer le code N fois peut permettre d'identifier cela.

C'est un risque avec Instancio, mais en même temps :

- Cela vous forcera à plus analyser et comprendre l'implémentation du service testé
- Et cela vous aidera à potentiellement identifier des comportements inattendus, du fait de l'unicité des jeux de données en entrée de service.

## Pour aller plus loin

- [instancio.org](https://www.instancio.org/)
- [Getting Started](https://www.instancio.org/getting-started/)
- [GitHub](https://github.com/instancio/instancio)
- [Maven Repository](https://mvnrepository.com/artifact/org.instancio/instancio-junit)
