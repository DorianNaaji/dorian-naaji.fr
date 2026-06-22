---
title: "NutriSnap : un tracker de calories IA, sans backend, sans compte, sans abonnement"
date: 2026-06-05T00:00:00+02:00
categories: ["Hard Skills"]
tags: ["NutriSnap", "Angular", "PWA", "GeminiAI", "Privacy", "OpenSource"]
url: "/nutrisnap/"
description: "Les apps de nutrition gratuites ne sont pas gratuites. J'ai construit NutriSnap : un tracker IA de calories et macros, sans serveur, sans compte, sans monétisation de vos données."
image: "/img/nutrisnap-app.png"
---

Les apps de nutrition gratuites ne sont pas vraiment gratuites. MyFitnessPal a été rachetée pour 475 millions de dollars. Cronometer a levé du capital-risque. Lose It! a des abonnements premium et des partenariats. Ces apps ont besoin de se monétiser, et quand le produit est gratuit, le modèle implique généralement vos données.

Ce n'est pas ce que je voulais construire. J'ai donc créé **NutriSnap** : un tracker de calories et de macros boosté à l'IA, sans backend, sans compte, sans abonnement, et sans aucun moyen de monétiser vos données de santé : parce que je ne les vois jamais.

## Un modèle de confidentialité honnête

Soyons transparents sur un point que la plupart des apps "privacy-first" ne précisent pas : quand vous scannez un repas, la photo transite vers des services IA connus. Cette donnée quitte bien votre appareil.

Ce qui n'existe pas, c'est un serveur NutriSnap au milieu qui enregistre vos repas, construit un profil sur vous, ou revend des données de santé agrégées à des assureurs ou des annonceurs. La relation est directe : votre navigateur, votre clé API, Google. Vous savez exactement qui voit quoi, sous quelles conditions.

## Pourquoi le BYOK change tout

La plupart des apps IA cachent le modèle derrière un abonnement parce que le coût API est leur levier commercial. NutriSnap fonctionne en **Bring Your Own Key**. Vous obtenez une clé API Gemini gratuite depuis Google AI Studio, vous la collez une fois, c'est tout. Cela a un effet secondaire intéressant : ça rend le modèle économique impossible. Il n'y a aucun moyen de facturer quelque chose que l'utilisateur possède directement. Cette contrainte était intentionnelle.

## PWA et cache busting

Une fois installée sur un téléphone, une PWA peut silencieusement servir des fichiers obsolètes. `@angular/service-worker` gère les mises à jour, mais il faut `Cache-Control: no-cache` sur `ngsw.json` et `index.html` côté serveur, ainsi qu'une boucle de polling `SwUpdate` avec une snackbar de notification. Sans ça, les utilisateurs tournent indéfiniment sur d'anciennes builds.

## Ce que ça fait

- Photo → Gemini 2.5 Flash → calories + macros en quelques secondes
- BMR et TDEE calculés depuis votre profil métabolique réel
- Récap journalier IA, mis en cache localement, à la demande
- Analyse de tendances sur 7 et 30 jours
- Calendrier mensuel avec code couleur selon l'apport quotidien
- Installable sur iOS & Android, fonctionne hors ligne
- Licence MIT, entièrement open source

## Conclusion

NutriSnap n'est probablement pas un business. Il ne peut pas être monétisé de façon classique, et c'est exactement pour ça que je lui fais confiance avec mes propres données.

Si vous êtes développeur et voulez regarder la stack : Angular 21 standalone, Dexie.js, Gemini 2.5 Flash, déployé en SPA statique; le code source complet est sur GitHub. Si vous en avez simplement marre des apps de nutrition avec des politiques de confidentialité opaques (et payantes !), allez tester

**Essayer NutriSnap** : [nutrisnap.dorian-naaji.fr](https://nutrisnap.dorian-naaji.fr/)  
**GitHub** : [github.com/DorianNaaji/nutrisnap](https://github.com/DorianNaaji/nutrisnap)  
**Clé API Gemini gratuite** : [aistudio.google.com](https://aistudio.google.com/welcome)