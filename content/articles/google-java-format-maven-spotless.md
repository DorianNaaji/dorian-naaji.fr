---
title: "Automatiquement formater le code d'un projet Java avec Google Java Format et des hooks de pre-commit"
date: 2025-02-20T00:14:36+01:00
categories: ["Hard Skills"]
tags: ["Java", "Maven", "Google Java Format", "Spotless", "Git hooks"]
url: "/google-java-format-maven-spotle/"
description: "Comment automatiser le formatage du code Java d'un projet avec le plugin Maven Spotless, Google Java Format et des git hooks de pre-commit."
---

Avoir des styles de formatage Java différents dans un projet peut être frustrant. Cela complique :

- La lecture du code, lorsqu'on navigue avec son IDE
- Les revues de code

Une bonne solution pour un projet Java consiste à automatiser le processus de formatage via des git hooks. Nous aurons besoin de :

- **spotless-maven-plugin** pour formater le code
- **git-build-hook-maven-plugin** pour installer le hook dans le dossier `.git/`

## Ajoutons ces dépendances au pom.xml

```xml
<plugin>
    <groupId>com.diffplug.spotless</groupId>
    <artifactId>spotless-maven-plugin</artifactId>
    <version>2.43.0</version>
    <executions>
        <execution>
            <goals>
                <goal>apply</goal>
            </goals>
        </execution>
    </executions>
    <configuration>
        <ratchetFrom>origin/main</ratchetFrom>
        <java>
            <includes>
                <include>src/main/java/**/*.java</include>
                <include>src/test/java/**/*.java</include>
            </includes>
            <googleJavaFormat>
                <version>1.25.2</version>
                <style>GOOGLE</style>
            </googleJavaFormat>
            <removeUnusedImports />
        </java>
    </configuration>
</plugin>
<plugin>
    <groupId>com.rudikershaw.gitbuildhook</groupId>
    <artifactId>git-build-hook-maven-plugin</artifactId>
    <version>3.5.0</version>
    <configuration>
        <installHooks>
            <pre-commit>.hooks/pre-commit</pre-commit>
        </installHooks>
    </configuration>
    <executions>
        <execution>
            <goals>
                <goal>install</goal>
            </goals>
            <phase>install</phase>
        </execution>
    </executions>
</plugin>
```

## Créer un exécutable `.hooks/pre-commit` au sein du repos

```sh
#!/bin/sh
# Retrieve the Java files staged by the developer to re-add them later once they are formatted by Spotless.
# (mvn spotless:apply applies changes to the disk and not to the staged files)
STAGED_FILES=$(git diff --cached --name-only --diff-filter=d -- '*.java')

if [ -z "$STAGED_FILES" ]; then
    exit 0
fi
mvn spotless:apply
git add $STAGED_FILES
```

(don't forget to `chmod +x` it)

Indiquez ensuite à vos collègues de lancer un `maven install`. Cela va copier le contenu du hook de pre-commit dans leur répertoire local. Puis, à chaque fois qu'ils réaliseront un commit, le script de pre-commit va être exécuté, et Spotless va formater le code Java qui a changé.

## La clé : récupérer les staged files avant le formatage

L'étape la plus importante ici est de bien intégrer la notion de récupération des "staged files", pour bien récupérer uniquement ce que le développeur souhaitait committer. Nous pourrons ainsi bien ré-ajouter ces fichiers au commit une fois formatés.

```sh
git diff --cached --name-only --diff-filter=d -- '*.java'
```

En effet, Spotless n'est pas capable (à l'heure où j'écris ces lignes, en 2025) de formater les fichiers et de les bouger ensuite dans l'état "staged". Spotless agit directement sur les fichiers sur disque.

En récupérant les fichiers au préalable, avant le formatage via maven spotless, on est capable de les rajouter au commit.

Voir : [Spotless GitHub Issue #178](https://github.com/diffplug/spotless/issues/178)

---

*Cet article est également disponible en anglais sur Medium : [Husky-like Solution to Format Java Code Automatically with Hooks](https://medium.com/@dorian.naaji/husky-like-solution-to-format-java-code-automatically-with-hooks-b221b4e55585)*
