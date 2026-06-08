---
title: "Vibe coding : La fin du dév ?"
date: 2026-06-01T02:22:54+02:00
categories: ["Hard Skills"]
tags: ["AgenticAI", "ClaudeAI", "LLM", "VibeCoding", "IntelligenceArtificielle"]
url: "/vibe-coding-la-fin-du-dev/"
description: "Hot take : non, les développeurs ne vont pas disparaître. Mais leur métier va probablement bien changer. Retour d'expérience sur une semaine de vibe coding."
---

Hot take : non, les développeurs ne vont pas disparaître. Mais leur métier va probablement bien changer.

Je me suis lancé le défi d'implémenter une petite app pour compter mes calories en "snappant" mes repas (petite photo rapide).  
*Si vous voulez voir, c'est par ici 👉 [github.com/DorianNaaji/nutrisnap](https://github.com/DorianNaaji/nutrisnap)*

Tout, de A à Z, du design à l'implem au plan de déploiement.

Mais là où *vibe code* fait un peu terme "à la cool", limite niais, en fait il y a tout un procédé créatif et de raisonnement à gérer.

- Identifier le scope du projet
- Déléguer les différentes missions du projet aux bons modèles, spécialisés et performants
- Beaucoup relire
- Beaucoup itérer
- Et beaucoup tester manuellement

Si on s'amuse à y aller "tout schuss" avec aucun filet de sécurité, aucun garde-fou, sans vision critique de ce que produit l'IA, on va droit dans le mur.

J'ai eu droit à de la dette technique produite par gemini-3-flash, des hallucinations de gemini-3.1-flash-lite, du `!important` et `::ng-deep` partout dans le css, des anomalies, et j'en passe.

Alors que je pense avoir eu une grande rigueur dans l'utilisation de l'outil :

- dossier `.ai` contenant des plans d'implémentations markdown précis, détaillés, ainsi que des rapports de session d'implémentation des différents agents
- un `.md` "d'onboarding" d'agent
- cadrer via instructions agents (`GEMINI.md`, `CLAUDE.md` & `README.md`)

Et même comme cela, ça n'a pas empêché d'avoir des surprises.

---

Le pire/plus long ayant été avec Gemini, qui m'a tellement rendu fou sur la fin que j'ai préféré sortir la CB pour un modèle plus premium (Anthropic) et la différence est clairement visible. En passant à Sonnet 4.6, le modèle faisait beaucoup moins d'erreurs, il était beaucoup plus organisé, mettait à jour la documentation de lui-même, et dans l'ensemble beaucoup moins *lazy* que Gemini.

Ça aura été aussi l'occasion de tester ollama et un modèle DeepSeek Flash entièrement en local, quelle horreur plus jamais. Le modèle était clairement à la ramasse et développait des pages dignes d'un site des années 2000.

---

Ce projet m'aura montré que sur moins d'une semaine de temps, on peut en l'espace de plusieurs heures *vibecoder* un projet à partir d'une idée qui nous tient à cœur *(et vider quelques puits au passage mais c'est un autre sujet)*.

Je n'ai fait réaliser aucun test unitaire car le projet est relativement simple, et que j'étais au final le *tester*/*reviewer* dédié sur l'app. L'approche itérative et la validation *step by step* a permis de trancher que des tests n'étaient également pas forcément utiles.

Je vous invite chaudement à vous familiariser avec ces technologies car on ne pourra pas y échapper. On est dans une période où on ne nous laisse pas le temps de challenger, comprendre & argumenter : **C'est prends le train ou crève**. Et les choses bougent extrêmement vite. Dans 6 mois les modèles que je cite ici auront déjà bougé, les façons de travailler en agentic auront déjà bien évolué. Plus on attend, plus la marche est haute.

Happy (vibe) coding!
