---
title: Dev Fest Lille 2018
date: 2018-06-07T15:20:01.169Z
author: Vincent Marmin & Thibault Fontaine
---

Je vous fais un retour d'expérience sur cet évènement auquel j'ai participé pour la première fois et que je recommande pour les années à suivre, à un public très large. Cette année en tout cas, les sujets étaient variés : de la technique, de l'UX, du DEVOPS, de l'intelligence artificielle, de la BigData, du mobile, de l'IoT… Il y en avait pour tout le monde ! Pour de la mise en pratique et pour une ambiance plus détendue et moins magistrale, des **CodeLabs** ont aussi été organisés. 

Les conférences étaient réparties sur 3 amphithéâtres, et les codelabs dans des salles de cours à proximité, avec plusieurs sujets en parallèle, à nous de faire notre sélection et d'établir une liste de priorités. Frustrant de voir plusieurs sujets passionnants se dérouler en même temps ! On attend les vidéos avec impatience !

Le détail du planning est ici [https://devfest.gdglille.org/#/schedule](https://devfest.gdglille.org/#/schedule)

Par son succès et sa popularité, vous trouverez d'autres retours de participants, avec parfois des retours complémentaires sur les autres conférences que je n'ai pas pu faire :

- [On était au Devfest 2018 – IMT Lille Douai](http://www.salto-consulting.com/on-etait-au-devfest-2018/)

Voici donc les notes prises au cours des conférences auxquelles j'ai pu assister.

## Conférence - Session modules EcmaScript

> Par Sébastien Pertus (Microsoft).  
> Le code source des démo [https://github.com/Mimetis/ESM](https://github.com/Mimetis/ESM)

Les modules ont été introduits dans la 6ième version d'ECMAScript (ES6 ou ECMAScript 2015). 3 ans déjà et voilà seulement que tous les navigateurs les supportent (à l'exception toujours de Internet Explorer 11).

Avant ça, les solutions pour modulariser ses projets et pour les rendre compatibles ES5, c'était :

- CommonJS
- AMD — implémenté par requireJS

Maintenant il y a **ESM** EcmaScript Modules

À l'usage, les modules c'est ça

```
import { speaker } from "./people";
import fetch from "node-fetch" // c'est l'export par défaut

export { getSpeakers }; // export d'un objet qui est une fonction
import { getSpeakers } from "./speakerServices"

export default getAllSpeakers;
import getAll from "./speakerServices" // system d'alias
```

Il y a un système de fallback si les modules ne sont pas supportés :

```
<script type="module" src="javascripts/module.js"></script>
<script nomodule src="javascripts/nomodule.js"></script>
```

🚨 **Attention** : le `type="module"` est chargé en asynchrone ! (chargé à la fin, après les autres src)

Pour charger les nomodules en asynchrone aussi `<script nomodule defer src="…">`. Le defer ne marche que s'il y a un **src** (les sources JS dans un fichier à part, donc)

🚨 **Attention** : les imports dans les browsers **IMPOSENT** l'extension.

💡**Astuce** : créer un fichier index.js pour exporter tous les éléments de plusieurs fichiers JS.

```
export * from "./blah.js";
export * from "./blahblah.js";
```

Pour générer des fichiers à la fois module et nomodule, utiliser un webpacker

- **webpack** utilise son propre system d'import / export
- utiliser plutôt **rollup** pour rester full Javascript vanilla

## Conférence - Comment l'UX a sauvé mon DEVOPS

> Par Estelle Landry & François Teychene

DEVOPS est un buzzword utilisé pour casser les mur entre les dévelopeurs et les sysadmins, sauf que DEVOPS ne veut pas dire NO-OPS !

Cette conférence porte sur le retour d'expérience sur une équipe de DEV et d'OPS qu'on a renommé DEVOPS (sans plus de changement) et dont les relations étaient tendues. Une PO venue du monde de l'UX est arrivée dans l'équipe et a résolu ces problèmes relationnels. Par exemple, reprioriser la backlog des OPS et unifier la vision DEV et OPS vers un objetif commun, mise en commun des taches dans un roadmap board.

Voici quelques outils venant du monde de l'UX design mis en place pour sauver la situation.

#### Jeu des 5 pourquoi

- Pourquoi A ? 
- Parceque B …
- Pourquoi B ? 
- Parceque C …
- Pourquoi C ? 
- Parceque D …
…

Par exemple, « On a beaucoup de problèmes du moment où on commit jusqu'à la mise en production ». À la fin des 5 pourquoi, on arrive au constat qu'il **manque des environnements**.

Il a fallu susciter de l'intérêt et de l'engagement pour le travail des autres.

#### Jeu des 6 chapeaux de Bono

6 angles de vues pour approcher une problématique :

1. Neutralité : faits, chiffres, informations …
2. Emotions
3. Créativité : on se lâche
4. Pessimisme : prudence, dangers, risques objections, incovénients
5. Optimisme
6. Organisation

#### Persona

Création d'un persona «Développeur». Par exemple Jean-Pierre, le Data Engineer expert (le **data-bourrin**), dont on imagine toute sa biographie.

#### les 5 phases du design UX


1. la PLANIFICATION (définir le projet et les utilisateurs cibles)
1. l’EXPLORATION (recueillir les besoins des utilisateurs)
1. l’IDÉATION (réfléchir à des idées de solutions avec les données recueillies)
1. la GÉNÉRATION (créer les prototypes des solutions)
1. l’ÉVALUATION (tester la qualité des solutions)

Les outils mis en place se positionnent dans ces 5 phases :

* Roadmap board : **planification**
* 6 chapeaux de Bono & 5 *pourquoi* : **exploration**
* Personas de dev : **ideation**
* Speedboat: **évaluation**

🚨 **Needs ≠ Solutions** : Si on demande à un dev ses besoins sur son problème de *manque d'environnement*, il pourrait répondre:
> Y a qu'à installer Docker !

## Conférence - A Kotlink between worlds
> Par Benjamin Monjoie  
> [https://github.com/bmonjoie/magicsquare](https://github.com/bmonjoie/magicsquare)  
> [https://medium.com/@Xzan](https://medium.com/@Xzan)  
> [https://twitter.com/Xzan](https://twitter.com/Xzan)

### Transpilation

Même si on connaît Kotlin plus comme un language de la JVM, Kotlin peut désormais être transpilé en Javascript (ECMAScript 5.1 pour le moment).

Cette transpilation essaie de respecter les règles suivantes :

1. Fournir un résultat optimal en taille
2. Les fichiers de sortie sont lisibles par les êtres humains
3. proposer les mêmes fonctionnalités à la fois pour la JVM que pour Javascript

Kotlin étant un language fortement typé, comme [Typescript](http://www.typescriptlang.org/), un outil existe ([ts2kt](https://github.com/Kotlin/ts2kt)) pour convertir les fichiers de définition existants de Typescript en Kotlin. On bénéficie ainsi de l'avancée de Typescript dans ce domaine.

#### limitations

- On ne pourra pas utiliser les librairie Java pour les modules communs avec JS.
- Kotlin ne connaissant pas le type de certains objets Javascript, on utilisera le type `dynamic`

### Modules communs

On peut donc coder en parallèle une **application Web** et une application **Android**, qui se partageront une partie du code. Dans un modèle MVC classique, on imagine très bien les 3 modules :

1. Android (view)
2. Javascript (view)
3. Commun en Kotlin (modèle + contrôleurs)

Pour faire le lien entre ces 3 modules, on utilisera les mot-clés `expect` et `actual`.

`expect` dans le module commun va définir des interfaces, dont l'implémentation sera dans les modules spécifiques aux plateformes.

`actual` est le pendant le `expect`. Les classes `actual` définissent l'implémentation spécifique à la plateforme.


### Testing

Comme le module commun ne peut contenir de librairies Java ou Javascript (pas de assertJ, Mockito ou autre). Par contre on peut utiliser les éléments natifs de Kotlin du package `kotlin.test` : `Test`, `Ignore`, `BeforeTest` et `AfterTest`.

Pour les modules spécifiques, aucun problème pour utiliser les frameworks de tests classiques :

- *JVM* : Junit
- *Javascript*: Jasmine , Jest, Karma …

Le mot clé `internal` de Kotlin permet de rendre une fonction privée au package uniquement, permettant de la **tester** (à condition que la classe de test appartienne au même package) tout en la gardant invisible à l'extérieur !

### Gotcha !

La génération Javascript de Kotlin supporte les 3 types de gestion de modules : **AMD**, **CommonJS** et **UMD** (Universal Module Definition).

**Il est recommandé d'utiliser l'UMD qui semble mieux fonctionner !**

## Conférence - HTTP/2 en pratique

> @AlexisHassler  
> [les slides](http://prez.sewatech.fr/http2/)  
> [la démo](https://github.com/hasalex/pz-http2-demo)  
> [http/2 homepage](https://http2.github.io)

**HTTP/2** (anciennement **SPDY**) est un nouveau protocole destiné à remplacer **HTTP 1.1**. Il est supporté par les navigateurs majeurs depuis fin 2015.

Les avantages de HTTP/2 :

1. communique dans un format binaire (HTTP1.1 était textuel)
2. compression des entêtes
3. fonctionnalité de réponses PUSH
4. multiplexage (cf. démo): 1 connection = plusieurs requêtes

### Démo

Implémentation de référence : **nghttpd**

Serveur : `nghttpd —no-tls -d ./html 8080`

Client : `nghttp http://localhost:8080/hello`

On remarque qu'il n'y a pas de schéma spécifique, ça reste toujours `http://`

L'option `-no-tls` active le mode **clear text** et permet de communiquer en clair, donc sans chiffrage. ⚠️ Ce n'est pas le fonctionnement normal, _les navigateurs ne l'acceptent pas_ !

#### Support

HTTP/2 est supporté par tous les navigateurs (sauf clear text)

Les clients qui le supportent: **nghttp**, **curl** (avec **libnghttp2**), pas du tout ~~wget~~.

- JavaScript: Node8.4+ (expérimental) - module http2
- Java9: Client expérimental java.incubator.HttpClient
- libnghttp2 : lib utilisable pour les autres serveurs
- Apache (mod_http2): attention OpenSSL 1.0.2+
- nginx : module ngx\_http\_v2\_module

Sur un serveur, si le client ne supporte pas http2 (clear text), alors fallback sur http1.1. Ou plutôt d’abord requête http1, puis http2.

⚠️ Pour **Express 4**, utiliser le module node-spdy et pas le module standard.

##### JAVA

- Natif pour Java 9+ (Support d’**ALPN** - Application-Layer Protocol Negotiation)
- Java 8 : mettre Jetty ALPN (dans le boot classpath)
- ~~Java 7~~ : Nope ! Le niveau de chiffrement est insuffisant.
- Spring Boot : même contraintes que le container. Utiliser Undertow (serveur Web)
- JavaEE8 : HTTP2 est dans la spec !

#### PUSH

Dans une même requête, on peut avoir plusieurs réponses. Par exemple, pour une page html, on peut pré-cacher dans le navigateur les resources qui vont avec (JS, CSS, images …). Le serveur Web est paramétré pour pusher ces resources associées à une page.

## CodeLab - Le Pair Programming sous toutes ses formes

Après toutes ces conférences, rien de tel qu'un atelier de pair programming pour se détendre et s'amuser. Nous avons vu ici le Pair-Programming sous toutes ses formes.

Nous en avons vu principalement 2 : le **Strong Style** et le **Far Sight Navigator**, que nous avons pratiqué en fonction des objectifs que nous avions choisis pour notre binôme.

Le pair-programming est à pratiquer de manière occasionnelle sur une durée d'1h environ.


Objectif de la session : **efficacité, exploration, qualité ou apprentissage**

Il y a quelques règles à définir pour la session :

- durée de la session
- les rotations
- qui est le premier au clavier

💡 Quelques rappels : Soyer bienveillants et restez concentrés sur votre objectif !

Checklist :

- préparer le matériel
- Design technique
- Vérifier le confort
- Définir les rôles

### Les techniques de Pair-Programming

#### Strong Style

Le navigateur apprend au driver une technique (TDD), un langage, un IDE… d'abord en lui dictant le code puis en donnant des instructions de plus haut niveau.

[http://llewellynfalco.blogspot.fr/2014/06/llewellyns-strong-style-pairing.html](http://llewellynfalco.blogspot.fr/2014/06/llewellyns-strong-style-pairing.html)

_Avantages_ : Transfert de compétences, apprentissage, coaching de Junior.
_Inconvénients_ : Niveau technique équivalent

> Le Navigator a augmenté son niveau d'abstraction sans réduire le confort du Driver.

##### Le Driver

Écrit le code et les tests, suit les instructions du navigateur, suit le rythme du navigateur

Fait confiance au navigateur, s'assure de comprendre la vision du navigateur, reste focalisé sur la méthode en cours et accepte de travailler avec une vision potentiellement incomplète du problème.

##### Le Navigator

A la vision /direction vers la solution, donne les instructions une à une au Driver, utilise le plus haut niveau d'abstraction que le Driver peut comprendre.

S'assure de la compréhension, rassure et explique si nécessaire, revoit en continu ce que le Driver produit.

Il interrompt le Driver trop vite sur les erreurs de syntaxe triviales

#### Far Sight Navigator

Le navigateur se concerte sur le design de la solution. Il peut le communiquer au driver à travers des cas de tests écrits sur des post-its. Le driver se focalise sur l'implémentation et refactore pour améliorer la qualité. Penser à permuter après 30 min pour éviter l'épuisement !

[http://blog.adrianbolboaca.ro/2014/02/pair-programming-game-farsight-navigator/](http://blog.adrianbolboaca.ro/2014/02/pair-programming-game-farsight-navigator/)

> La progression dans la résolution est linéaire, et le code produit est de bonne qualité.

##### Le Driver

Pioche 1 par 1 les post-its du Navigator, code le test du Navigator d'abord, puis le fait passer …

Garant de la qualité, refactorise quand c'est nécessaire / si pas de post-it écrit, demande un test plus simple si pas confiant sur le fait d'implémenter rapidement.

Ne peut pas : Penser en avance aux prochaines étapes

##### Le Navigator

A la vision / direction vers la solution, découpe les étapes vers la solution, écrit des cas de tests (given/when/then) sur des post-its pour aller vers la solution.

Les tests contiennent les données de test

Inconvénients : questionner les détails d'implémentation, mettre la pression au Driver, s'inquiéter en cas d'absence du buffer.

#### Ping Pong

Alice écrit un test (qui ne passe pas). Bob fait passer le test en écrivant du code métier, puis Bob écrit un nouveau test (qui ne passe pas non plus). Alice fait passer le test. Répéter en continuant à permuter toutes les 3-5 minutes...

Idéal pour découvrir le TDD !

[http://anthonysciamanna.com/2015/04/18/ping-pong-pair-programming.html](http://anthonysciamanna.com/2015/04/18/ping-pong-pair-programming.html)


#### Mob Programming

Un driver au clavier, suit les instructions des navigateurs qui voient le code projeté au mur. Tourner toutes les 5-10 minutes. Permet l'apprentissage et l'alignement d'une équipe. Variante : un seul navigateur peut parler au driver.

[http://mobprogramming.org/mob-programming-book/](http://mobprogramming.org/mob-programming-book/)

#### Découverte d'un langage en binôme

1 PC ouvert sur la doc/ internet. 1 PC avec l'IDE dans lequel on implémente une méthode en quelques minutes, et on échange les places.

Crédits : @antoineneveux

### L'exercice de Pair-Programming

L'objectif que notre binôme s'était fixé, était de faire découvrir au driver Kotlin et l'IDE IntelliJ.

Le sujet donné pour s'exercer était la réalisation d'une fonction d'**addition de deux nombres romains** … ⚠️ sans jamais convertir les nombres en décimal ! ⚠️

exemples :

- `sum("V", "XI") == "XVI"`
- `sum("DCXCIV", "MMCCCLIV") == "MMMXLVIII"`

Après une bonne partie de la séance à initialiser un projet Kotlin avec tests unitaires, avec le Navigator (moi) expliquant les subtilités des dépendances Gradle, nous sommes partis sur l'implémentation d'une méthode _inutilement complexe_ en essayant de déterminer une règle générale pour déterminer la forme simplifiée d'un nombre romain.


## Codelab - Initiation au Japonais

> Par Ayako Fukushima & Siegfried Ehret

Pour finir la journée avec légèreté, j'ai souhaité assister à cette initiation à la langue japonaise, surtout par curiosité parce que j'avais déjà personnellement pris des cours.

### L'écriture japonaise

Le japonais s'écrit avec différents types de caractères :

- Les [hiragana](https://upload.wikimedia.org/wikipedia/commons/thumb/2/28/Table_hiragana.svg/768px-Table_hiragana.svg.png) forment un syllabaires de 46 «sons», utilisé pour les mots d'origine japonaise et pour la grammaire.
- Les [katakana](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0d/Table_katakana.svg/768px-Table_katakana.svg.png) forment un second syllabaire utilisés pour les mots d'origine étrangère (**katakana**) comme «Champagne - チャンペン», «Chocolat - チョコレート» ou «Vincent - ヴァンサン» …
- Les **Kanji** sont les les caractères complexes d'origine Chinoise utilisés pour l'écriture des mots japonais.

## Keynote de clôture

Pour clôturer le DevFest 2018, des gros fou-rires et auto-dérision lors d'une séance de [SpeechLess](http://speechlesslive.com/). 

La recette est simple, prenez :

- 3 speakers de la DevFest (Hubert Sablonnière, Philippe Charrière et Sébastien Pertus)
- 8 minutes chacun
- 1 jury à convaincre
- 1 sujet tiré au hasard
- des slides tout aussi aléatoires

Chaque speaker a 8 minutes pour improviser sur un sujet qu'il ne connait pas, mais qui doit malgrés tout convaincre qu'il maîtrise !

Les sujets tirés étaient :

- « La reine des neiges »
- « Conférence post-mortem de la Fiat Multipla »
- « Conférence post-mortem d'Internet Explorer »

Tout repose sur le talent oratoire et de comédien des speakers, emmenant le public dans un délire improvisé.