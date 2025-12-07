+++
title = "Veille technologique"
weight = 1
+++,

## Veille Technologique

Pour commencer ma recherche, j'ai posé cette question à ChatGPT :

> **Is GLSL still useful in today's day and age?**

---

### Réponse : Oui, GLSL est toujours pertinent

Selon ChatGPT, GLSL reste un langage actuel, même si son utilisation a changé. Voici ce que j'en ai retenu (en 2025) :

---

## ✅ Où GLSL est encore utilisé

### 1. OpenGL & OpenGL ES

Tous les projets basés sur OpenGL utilisent GLSL, et il y en a encore beaucoup :

- Les moteurs de jeux indépendants
- Les applications scientifiques
- Les outils multiplateformes
- Les jeux mobiles
- Les logiciels plus anciens qui sont encore maintenus

### 2. WebGL / WebGL2

- Les shaders qui tournent dans le navigateur avec WebGL sont écrits en GLSL ES
- Les bibliothèques populaires comme **Three.js** et **Babylon.js** génèrent du GLSL

### 3. Art shader & apprentissage

Pour apprendre la programmation GPU, GLSL reste le plus accessible :

- [ShaderToy](https://www.shadertoy.com/)
- [Book of Shaders](https://thebookofshaders.com/)
- Plusieurs sites de visualisation créative

C'est vraiment l'outil parfait pour comprendre comment les shaders fonctionnent.

---

## ⚠️ Où GLSL perd du terrain

Par contre, plusieurs moteurs modernes préfèrent d'autres langages :

### HLSL

- **Unity**
- **Unreal**
- **DirectX**

HLSL est devenu le langage de base dans beaucoup de pipelines. Après, il est converti en GLSL, SPIR-V ou Metal selon la plateforme.

### WGSL (WebGPU)

- Le successeur de WebGL, **WebGPU**, utilise **WGSL** au lieu de GLSL
- Le web migre tranquillement vers ce nouveau standard

### SPIR-V

- Avec **Vulkan**, on n'écrit plus directement en GLSL
- La plupart des moteurs utilisent HLSL ou un langage haut niveau qui compile vers SPIR-V

---

## 🎯 Est-ce que ça vaut encore la peine d'apprendre GLSL ?

### Oui, et voici pourquoi :

- C'est le meilleur moyen d'apprendre les concepts de base des shaders
- La logique et les maths sont pratiquement identiques en HLSL, GLSL ou WGSL
- C'est direct et simple pour comprendre comment le GPU fonctionne
- Des tonnes de projets l'utilisent encore en production

### Mais :

Si tu veux travailler avec Unity, Unreal ou Vulkan, tu devras aussi apprendre HLSL éventuellement.

---

## 🚀 Recommandation

| Objectif | Langage suggéré |
|----------|----------------|
| **Apprendre les shaders** | Commence avec **GLSL** |
| **Travailler sur des moteurs récents** | Ajoute **HLSL** après |
| **Développer pour le web du futur** | Regarde **WGSL** |

---

## Mon analyse de cette réponse

### Est-ce que la réponse est complète ?

ChatGPT donne une bonne vue d'ensemble pour quelqu'un qui débute. Les cas d'usage actuels sont bien couverts, et les alternatives comme HLSL, WGSL et SPIR-V sont mentionnées.

Par contre, il manque des détails plus techniques. Par exemple, rien sur les différences de performance entre ces langages. Les outils de conversion comme glslang ou SPIRV-Cross ne sont pas mentionnés. Et la compatibilité avec iOS et Metal aurait pu être abordée.

### Comparaison avec Google

Quand je cherche sur Google, je tombe sur des discussions StackOverflow et de la documentation officielle. C'est utile, mais je dois lire plusieurs sources pour me faire une idée complète. Et je risque de tomber sur des infos dépassées, surtout des articles de 2018-2020.

Avec ChatGPT, j'ai une réponse structurée tout de suite. C'est contextualisé en 2025, ce qui aide. Mais je dois quand même vérifier avec d'autres sources pour être sûr.

En gros, ChatGPT est plus rapide pour avoir une vue d'ensemble, mais Google reste essentiel pour approfondir.

### Vérification des faits

J'ai vérifié avec plusieurs sources officielles :

**Khronos Group (OpenGL officiel)** : https://www.khronos.org/opengl/
Le site confirme qu'OpenGL est en "mode maintenance". WebGL 2.0 utilise bien GLSL ES 3.0. ChatGPT avait raison.

**WebGPU Specification** : https://www.w3.org/TR/webgpu/
WGSL remplace effectivement GLSL pour WebGPU. Le statut est "Candidate Recommendation" depuis décembre 2024. Info vérifiée.

**Unity Documentation** : https://docs.unity3d.com/Manual/shader-graph.html
Unity compile bien en HLSL avant de faire une cross-compilation. C'est documenté officiellement.

### Ce que j'ai appris

Avant cette recherche, je savais que GLSL servait pour OpenGL et que les moteurs récents préféraient d'autres solutions. Ça s'est confirmé.

Ce qui m'a surpris, c'est à quel point WebGPU et WGSL avancent vite. Je ne réalisais pas non plus que SPIR-V était devenu le format intermédiaire standard. Et c'est intéressant de voir que ShaderToy reste LA référence pour apprendre malgré toutes les alternatives.

---

## Comment j'ai planifié mon cours

### Ce que je vais couvrir

Je vais me concentrer sur les bases de GLSL pour les fragment shaders 2D. Au programme : syntaxe de base (types, fonctions), le pipeline de rendu (coordonnées, couleurs), les fonctions mathématiques qu'on utilise souvent, et comment animer avec le temps.

Je ne toucherai pas aux vertex shaders complexes, aux compute shaders, aux optimisations poussées ou à l'intégration dans un moteur 3D. C'est trop avancé pour ce que je vise.

### Niveau visé

Je vise des débutants ou intermédiaires. Ça devrait prendre environ 4 à 6 heures à parcourir. Il faut juste avoir des bases en maths de secondaire (vecteurs, trigonométrie). L'objectif ? Que les étudiants puissent créer leurs propres shaders 2D animés qui fonctionnent.

### Mes sources principales

**The Book of Shaders** (source principale)  
Qualité : 5/5 - C'est LA référence dans la communauté  
Pourquoi : Le contenu est interactif, on avance progressivement, et on peut tester les exemples directement  
Lien : https://thebookofshaders.com/

**ShaderToy** (pour pratiquer)  
Qualité : 4/5 - Une galerie avec des milliers d'exemples de la communauté  
Pourquoi : On peut voir des exemples concrets et trouver de l'inspiration  
Lien : https://www.shadertoy.com/

**OpenGL ES Shading Language Specification** (référence technique)  
Qualité : 5/5 - La doc officielle  
Pourquoi : Pour les détails précis du langage quand j'ai un doute  
Lien : https://registry.khronos.org/OpenGL/specs/es/3.0/GLSL_ES_Specification_3.00.pdf

---

## Ma source choisie

J'ai fait des recherches sur Google et avec ChatGPT pour trouver une bonne ressource d'apprentissage pour GLSL. Je suis tombé sur **The Book of Shaders**. C'est un site-guide qui explique les bases de GLSL avec des exemples de code interactifs que tu peux modifier en temps réel.

Il y a assez de contenu pour bâtir des notes de cours solides si je me concentre sur les commandes pratiques sans trop m'enliser dans la théorie profonde. Le site est encore en développement (on voit des pages prévues mais pas encore écrites), mais ce qui existe est de qualité.

J'apprécie particulièrement le format textuel parce que je peux y aller à mon propre rythme et revenir en arrière quand j'en ai besoin.

---

## Article d'actualité

Après avoir reçu la réponse de ChatGPT sur toutes les alternatives à GLSL, je m'attendais à découvrir que GLSL ne pouvait plus créer d'effets impressionnants. Mais cet article m'a prouvé le contraire : https://80.lv/articles/play-with-this-god-of-war-inspired-dissolve-effect-made-with-glsl-in-your-browser

L'article présente un effet de dissolution créé par Simon Trümpler, inspiré de *God of War Ragnarok*. Ce qui est fou, c'est que ça tourne directement dans le navigateur avec WebGL, sans avoir besoin d'un gros moteur 3D. Il utilise des techniques avancées comme les noise functions et le displacement mapping, tout en restant dans GLSL ES 3.0.

Ça prouve que GLSL vaut encore la peine d'être appris - on peut créer des effets dignes des jeux AAA modernes. La démo interactive permet même de voir le code source, ce qui en fait une excellente ressource pour des étudiants plus avancés.

**Pour mon cours** : C'est trop complexe pour des débutants, mais ça montre aux étudiants ce qu'ils pourront accomplir après avoir maîtrisé les bases. C'est motivant !

---

## Comment je maintiens ma veille

### Mes sources régulières

#### Flux et newsletters

**Khronos Blog** : https://www.khronos.org/blog/  
Je consulte ça chaque semaine pour suivre les annonces sur OpenGL, WebGL et Vulkan.

**80.lv (Computer Graphics)** : https://80.lv/  
Je check ce site régulièrement pour découvrir de nouvelles techniques de shaders et voir ce qui se fait dans les gros jeux.

#### Communautés en ligne

**r/opengl** (Reddit) : https://reddit.com/r/opengl  
Cette communauté partage souvent des tutoriels et aide à résoudre des problèmes techniques.

**ShaderToy Discord**  
Une communauté active de créateurs qui partagent leurs techniques et donnent du feedback constructif.

#### Mes outils de suivi

**Google Alerts**  
J'ai configuré des alertes pour "GLSL tutorial" et "WebGPU WGSL" pour recevoir les nouveaux articles pertinents.

**GitHub Watch**  
Je suis le repo `patriciogv/thebookofshaders` pour voir les mises à jour du guide.

### Ma routine de veille

Je révise mes sources principales chaque mois pour voir s'il y a du nouveau contenu intéressant. Tous les trois mois, je vérifie où en sont les alternatives comme WGSL et HLSL. Et une fois par an, je fais une mise à jour complète du cours pour m'assurer que tout est encore d'actualité.

### Ce que j'ajoute à ma veille

J'ajoute un article ou une ressource si :
- Ça introduit une nouvelle technique GLSL applicable pour mes étudiants
- Ça annonce un changement important (dépréciation, nouvelle version)
- Ça fournit des exemples interactifs de qualité

Je n'inclus pas les ressources trop avancées (compute shaders, ray tracing) ou trop spécifiques à un moteur propriétaire.
