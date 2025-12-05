+++
title = "Atelier"
weight = 3
+++,

## Atelier de GLSL

### Préparation à l'atelier

Il existe plusieurs façons de coder en GLSL. Pour cet atelier, vous aurez deux choix :

1. **Utiliser ShaderToy** : [https://www.shadertoy.com/](https://www.shadertoy.com/)
2. **Utiliser le dépôt Git** : [https://github.com/Mohamed-Brahimi/Atelier-glsl.git](https://github.com/Mohamed-Brahimi/Atelier-glsl.git) et créer un fork

Je vous conseille d'utiliser le dépôt Git, puisqu'un dépôt similaire sera utilisé lors du travail pratique. Cependant, j'expliquerai tout de même comment commencer avec les deux outils.

---

## Option 1 : Utiliser ShaderToy

### Étapes pour commencer

1. Rendez-vous sur [https://www.shadertoy.com/](https://www.shadertoy.com/)
2. Vous verrez de nombreux shaders créés par la communauté, mais ce n'est pas ce qui nous intéresse pour l'instant
3. Cliquez sur **"New"** (Nouveau) dans le coin supérieur droit de l'écran
4. Un éditeur de code s'ouvrira avec un shader de base

### Interface de ShaderToy

Pour faciliter votre expérience de codage, voici une légende de l'interface :

![Légende de l'interface ShaderToy](/images/image/atelier-1.png)

**Zones principales :**
- **Zone de code** : Où vous écrivez votre shader
- **Zone d'aperçu** : Où vous voyez le résultat en temps réel
- **Panneau de contrôle** : Pour ajuster les paramètres et compiler le code

## Option 2 : Utiliser le dépôt Git (Recommandé)

### Étapes d'installation

**1. Créer un fork du dépôt**

- Dirigez-vous vers [https://github.com/Mohamed-Brahimi/Atelier-glsl.git](https://github.com/Mohamed-Brahimi/Atelier-glsl.git)
- Cliquez sur le bouton **"Fork"** en haut à droite
- Cela créera une copie du dépôt sur votre compte GitHub

**2. Cloner le dépôt sur votre machine**

```bash
git clone https://github.com/VOTRE-NOM-UTILISATEUR/Atelier-glsl.git
cd Atelier-glsl
```

Remplacez `VOTRE-NOM-UTILISATEUR` par votre nom d'utilisateur GitHub.

**3. Ouvrir le projet dans VS Code**

```bash
code .
```

**4. Ouvrir le Dev Container**

**Prérequis** : Assurez-vous que Docker est installé et en cours d'exécution sur votre machine.

- VS Code devrait détecter le fichier `.devcontainer` et vous proposer d'ouvrir le projet dans un conteneur
- Cliquez sur **"Reopen in Container"**
- Attendez que le conteneur se construise (cela peut prendre quelques minutes la première fois)

### Utilisation de l'environnement

**1. Écrire votre code**

Ouvrez le fichier `Atelier.frag` et écrivez votre code GLSL dedans.

**2. Afficher le rendu**

- Appuyez sur `Ctrl+Shift+P` (ou `Cmd+Shift+P` sur Mac) pour ouvrir la palette de commandes
- Tapez **"Show GLSL Canvas"** et appuyez sur Entrée
- Une fenêtre d'aperçu s'ouvrira à côté de votre code

**3. Tester votre code**

Pour vérifier que tout fonctionne, copiez un exemple des notes de cours dans `Atelier.frag`. Par exemple :

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform float u_time;

void main() {
    vec2 st = gl_FragCoord.xy / u_resolution;
    vec3 color = vec3(st.x, st.y, abs(sin(u_time)));
    gl_FragColor = vec4(color, 1.0);
}
```

Si vous voyez un dégradé de couleurs qui change avec le temps, tout fonctionne correctement.

**4. Sauvegarder votre travail**

```bash
git add .
git commit -m "Description de vos modifications"
git push
```

---
## Exercice 1 : Création de formes circulaires

### Objectif

Créer un cercle centré à l'écran, puis le transformer en dégradé radial animé.

### Partie 1 : Cercle simple (15-20 minutes)

Créez un cercle blanc sur fond noir en utilisant la distance depuis le centre de l'écran.

#### Nouveaux concepts

**1. Centrage des coordonnées**

```glsl
st -= 0.5;
```

Cette ligne déplace l'origine des coordonnées au centre de l'écran. Sans cette ligne, l'origine (0, 0) est dans le coin inférieur gauche.

**2. La fonction `length()`**

Cette fonction calcule la **distance euclidienne** entre le pixel actuel et l'origine (0, 0).

Après avoir centré avec `st -= 0.5`, cette distance représente la distance depuis le centre de l'écran :
- Au centre exact : `dist = 0.0`
- Aux bords : `dist ≈ 0.5` (sur les côtés) ou `dist ≈ 0.7` (aux coins)

Dans cet exercice, il permet de définir la position du cercle 

#### Instructions

1. Normalisez les coordonnées avec `gl_FragCoord.xy / u_resolution`
2. Centrez les coordonnées en soustrayant `0.5`
3. Calculez la distance du centre avec `length()`
4. Utilisez `smoothstep()` pour créer un cercle autour d'un rayon
5. Inversez la valeur pour obtenir un cercle blanc sur fond noir

#### Validation

Vous devriez obtenir un cercle blanc centré avec des bords lisses sur un fond noir.
## Exercice 2 : Cercle animé - Transition jour/nuit

### Objectif

Créer un cercle dont la taille et la couleur varient au fil du temps, simulant une transition entre un soleil chaud et une lune froide, avec un fond qui change également.

#### Concepts Clé

**1. Animation de la taille**

Au lieu d'un rayon fixe, nous allons faire varier le rayon du cercle avec le temps.

**2. Animation de couleur avec `mix()`**

Nous allons mélanger deux couleurs en fonction du temps pour simuler le changement de couleur du soleil à la lune.

#### Validation

Vous devriez voir un cercle qui change de taille et de couleur, simulant un cycle jour/nuit.
## Exercice 3 : Aurore boréale animée

### Objectif

Créer une aurore boréale stylisée avec des ondes de couleur qui ondulent dans un ciel nocturne. Cet exercice combine les techniques vues précédemment avec de nouveaux concepts de fonctions sinusoïdales multiples.

### Concepts à utiliser

Pour réussir cet exercice, vous devrez combiner :
- Les coordonnées normalisées (Exercice 1)
- Les animations avec `u_time` (Exercice 2)
- La fonction `sin()` pour créer des ondes
- La fonction `mix()` pour les couleurs (notes de cours)
- La fonction `smoothstep()` pour créer des bandes de lumière (notes de cours)

### Résultat attendu

Vous devriez observer :
- Un fond bleu foncé représentant le ciel nocturne
- Des bandes ondulantes vertes et bleues qui bougent horizontalement
- Un mouvement fluide et continu des aurores
- Une impression de profondeur avec plusieurs couches d'ondes

### Indices

**Pour créer l'effet de vague :**
```glsl
// Exemple de structure (à adapter)
float wave = sin(st.x * 6.0 + u_time);
```

**Pour créer plusieurs ondes :**
Additionnez plusieurs ondes avec différents paramètres. Pensez à ajuster la position verticale de chaque onde.

**Pour les couleurs d'aurore :**
- Vert typique : `vec3(0.0, 1.0, 0.5)`
- Bleu-vert : `vec3(0.0, 0.8, 0.8)`
- Violet : `vec3(0.5, 0.0, 1.0)`

### Validation

Vous devriez obtenir :
- Un ciel bleu foncé stable en arrière-plan
- Des ondes lumineuses qui se déplacent de droite à gauche (ou inversement)
- Au moins deux couches d'ondes visibles
- Des transitions de couleur douces entre le ciel et les aurores
- Un mouvement fluide sans saccades

Si vos ondes sont statiques, vérifiez que vous utilisez bien `u_time` dans vos calculs.

Si vos ondes sont trop rapides ou trop lentes, ajustez le coefficient multiplicateur de `u_time`.


### Temps estimé

35-50 minutes pour la version de base, selon votre aisance avec les fonctions sinusoïdales et les superpositions de formes.

---

## Pour aller plus loin (Optionnel)

Si vous avez terminé l'exercice et souhaitez expérimenter davantage :

### Inspiration

Visitez [ShaderToy](https://www.shadertoy.com/browse) pour voir ce que d'autres créateurs ont réalisé. Essayez de comprendre leur code en vous appuyant sur les concepts vus en classe.

---