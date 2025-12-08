+++
title = "Notes de cours"
weight = 2
+++

## Qu'est-ce que GLSL ?

GLSL (ou OpenGL Shading Language) est un langage de programmation qui permet de créer des shaders spécifiques à OpenGL. Mais avant d'aller plus loin, il faut se poser la question : qu'est-ce qu'un shader ?

## Qu'est-ce qu'un shader ?

Un shader est un petit programme exécuté sur le GPU (processeur graphique). Ce programme demande au GPU d'effectuer certains calculs afin d'afficher un objet d'une manière spécifique. Le nom "shader" (ou "shade" en anglais, qui signifie "ombre") provient d'un des calculs fondamentaux effectués par les shaders : le calcul de la lumière. Cela détermine où l'objet sera plus lumineux et où il y aura des ombres. Voyez ci-dessous un exemple de Minecraft avec et sans shaders.

<img src="/images/Minecraft-Shaders.webp" alt="Minecraft avec et sans shaders" width="600">

Vous pouvez donc constater la puissance des shaders. Cependant, cette puissance a un coût : les mathématiques nécessaires pour créer ces shaders peuvent être complexes.

---

## La création d'une couleur simple

Voici le code de base :

```glsl
void main() {
    gl_FragColor = vec4(0.0, 1.0, 0.0, 1.0);
}
```

![Résultat du shader vert](/images/affichage-code-1.png)

Regardons comment ce code fonctionne :

**La fonction `main()`** - Comme dans la plupart des langages de programmation, les instructions principales sont placées dans une fonction `main()`.

**La variable `gl_FragColor`** - Il s'agit d'une variable intégrée à GLSL. GLSL possède plusieurs variables prédéfinies que nous modifierons tout au long de ce cours pour créer différents effets visuels. La variable `gl_FragColor` détermine la couleur finale du pixel affiché.


Ces valeurs doivent être comprises entre 0.0 et 1.0. Dans notre exemple, le code affiche du vert pur car seule la deuxième composante (vert) est à 1.0.

> **Note importante** : N'oubliez pas le `.0` après chaque nombre. Il indique que la variable est de type `float` (nombre à virgule flottante).

---

## Ajout de variables globales

GLSL met à disposition plusieurs variables globales :

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;   // La taille de la toile (largeur, hauteur)
uniform vec2 u_mouse;        // La position de la souris mesurée en pixels
uniform float u_time;        // Le temps écoulé (en secondes) depuis le lancement du shader
```

Les variables que nous importons de GLSL sont des **variables uniformes** (uniform). Ces variables restent constantes pour tous les pixels pendant un seul rendu. Nous utiliserons le préfixe `u_` avant le nom des variables uniformes pour les identifier facilement.

> **Note pour ifdef** : `ifdef` permet d'effectuer certaines opérations si une variable est définie.

> **Note pour ShaderToy** : Si vous utilisez ShaderToy lors des ateliers, vous devrez remplacer le préfixe `u_` par `i` (par exemple : `iResolution`, `iMouse`, `iTime`).

---

## Utilisation des variables globales

```glsl
uniform float u_time;

void main() {
    gl_FragColor = vec4(0.0, abs(sin(u_time)), 0.0, 1.0);
}
```

Ce code montre comment utiliser les variables uniformes gérées par le GPU. Nous retrouvons l'utilisation de `gl_FragColor` avec le même vecteur que précédemment, mais cette fois nous appelons deux fonctions à la place d'une valeur fixe. C'est ici que les mathématiques deviennent utiles dans les shaders.

![Fonction Sinus](/images/sin-function.png)

La fonction sinus retourne une valeur oscillant entre 1 et -1 selon le temps écoulé représenté par la variable `u_time`. Ensuite, la fonction `abs` (valeur absolue) retourne un nombre positif, puisque le vecteur ne prend pas en charge les valeurs négatives. Ce qui crée l'effet de la couleur verte qui apparaît et disparaît.

---

## Les fonctions de formes

Maintenant que nous avons vu comment modifier la couleur des fragments, créons des formes géométriques.

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

// Créer une ligne dans les y en utilisant des valeurs entre 0 et 1
float plot(vec2 st) {    
    return smoothstep(0.02, 0.0, abs(st.y - st.x));
}

void main() {
    vec2 st = gl_FragCoord.xy / u_resolution;

    float y = st.x;

    vec3 color = vec3(y);

    // Tracer une ligne
    float pct = plot(st);
    color = (1.0 - pct) * color + pct * vec3(0.0, 1.0, 0.0);

    gl_FragColor = vec4(color, 1.0);
}
```

![Affichage Code 2](/images/affichage-code-2.png)

### Normalisation des coordonnées

```glsl
vec2 st = gl_FragCoord.xy / u_resolution;
```

`gl_FragCoord` est une variable intégrée qui contient les coordonnées en pixels du fragment actuel. En divisant par `u_resolution`, nous normalisons ces coordonnées entre 0.0 et 1.0, rendant le shader indépendant de la résolution et permettant d'utiliser ces variables dans des vecteurs sans créer d'erreurs.

### La fonction `plot()`

```glsl
float plot(vec2 st) {    
    return smoothstep(0.02, 0.0, abs(st.y - st.x));
}
```

Cette fonction dessine une ligne en comparant les coordonnées x et y. `st.y - st.x` calcule la distance entre le pixel actuel et la ligne diagonale y = x. La fonction `abs()` prend la valeur absolue pour obtenir la distance (toujours positive). `smoothstep(0.02, 0.0, ...)` crée une transition douce - elle retourne 1.0 quand on est près de la ligne (distance < 0.02) et 0.0 quand on est loin.

On peut créer une ligne moins floue en baissant le premier nombre ou même inverser le shader en augmentant le deuxième nombre de façon dramatique.

> **Note sur `step()`** : `step()` est une fonction similaire à `smoothstep` mais qui, à la place de retourner une valeur décimale, ne retourne que des 0 et des 1, ce qui rend l'affichage moins satisfaisant puisqu'il n'y a plus de dégradé.

### Le dégradé de base

```glsl
float y = st.x;
vec3 color = vec3(y);
```

Ce code crée un dégradé horizontal du noir (gauche) au blanc (droite), car `y` varie de 0.0 à 1.0. Cette définition correspond à une fonction de base f(x) = x, d'où la ligne verte qui représente ce dégradé avec sa transition simple et linéaire.

### Mélange des couleurs

```glsl
float pct = plot(st);
color = (1.0 - pct) * color + pct * vec3(0.0, 1.0, 0.0);
```

Cette ligne mélange deux couleurs selon la valeur de `pct` :
- Quand `pct = 0.0` (loin de la ligne) : on garde le dégradé original
- Quand `pct = 1.0` (sur la ligne) : on affiche du vert pur
- Entre les deux : transition douce grâce à `smoothstep`

Le résultat est un dégradé horizontal avec une ligne verte diagonale tracée dessus.

---

## Modification de la forme de la ligne

En suivant la logique de y = f(x) représentant une fonction de base, nous pouvons la modifier pour changer la forme de la ligne :

```glsl
// Author: Inigo Quiles
// Title: Expo

#ifdef GL_ES
precision mediump float;
#endif

#define PI 3.14159265359

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

float plot(vec2 st, float pct){
  return  smoothstep( pct-0.02, pct, st.y) -
          smoothstep( pct, pct+0.02, st.y);
}

void main() {
    vec2 st = gl_FragCoord.xy/u_resolution;

    float y = pow(st.x, 5.0);

    vec3 color = vec3(y);

    float pct = plot(st, y);
    color = (1.0-pct)*color+pct*vec3(0.0, 1.0, 0.0);

    gl_FragColor = vec4(color, 1.0);
}
```

![Affichage Code 3](/images/affichage-code-3.png)

### Constante PI

```glsl
#define PI 3.14159265359
```

Cette ligne définit une constante pour π (pi), utile pour les calculs trigonométriques. Bien que non utilisée dans cet exemple, elle sera nécessaire pour d'autres fonctions mathématiques.

### Fonction `plot()` améliorée

```glsl
float plot(vec2 st, float pct){
  return  smoothstep( pct-0.02, pct, st.y) -
          smoothstep( pct, pct+0.02, st.y);
}
```

Cette nouvelle version accepte maintenant deux paramètres : `st` (les coordonnées normalisées) et `pct` (la valeur y de la fonction à tracer). La fonction utilise deux `smoothstep()` soustraits l'un de l'autre pour créer une bande. Le premier crée une transition de 0 à 1 en dessous de la ligne, le second au-dessus, et la soustraction crée une bande de largeur 0.04 (0.02 de chaque côté) autour de la courbe.

### Fonction exponentielle

```glsl
float y = pow(st.x, 5.0);
```

Au lieu de `y = st.x` (fonction linéaire), nous utilisons `pow(st.x, 5.0)` qui calcule x⁵. Cela crée une courbe exponentielle où :
- Quand x = 0.0, y = 0.0
- Quand x = 0.5, y = 0.03125
- Quand x = 1.0, y = 1.0

La courbe reste près de 0 au début, puis monte rapidement vers la fin. Cette transition est visible dans le dégradé qui reste noir sur la majeure partie du tableau, ne changeant que de façon brusque vers la fin.

Au lieu de tracer une ligne fixe (diagonale), nous traçons maintenant la courbe définie par la fonction `y = pow(st.x, 5.0)`. Le résultat est un dégradé avec une ligne verte qui suit une courbe exponentielle. Vous pouvez changer l'exposant (5.0) pour obtenir différentes courbes : 2.0 pour une parabole, 0.5 pour une racine carrée, etc.

### Expérimentation avec d'autres fonctions

En modifiant la ligne `float y = pow(st.x, 5.0);` par `float y = smoothstep(0.1, 0.9, st.x);`, on obtient :

![Affichage Code 4](/images/affichage-code-4.png)

La ligne représente maintenant la fonction `smoothstep`, qui crée une courbe en forme de S. Vous pouvez observer différents comportements en essayant diverses fonctions trigonométriques comme `sin(st.x)`, `cos(st.x)`, ou même des combinaisons comme `sin(st.x * PI)`.

---

## Jouer avec les couleurs

Explorons maintenant comment créer des transitions de couleurs dynamiques :

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform float u_time;

vec3 colorA = vec3(0.149, 0.141, 0.912);
vec3 colorB = vec3(1.000, 0.833, 0.224);

void main() {
    vec3 color = vec3(0.0);

    float pct = abs(sin(u_time));

    color = mix(colorA, colorB, pct);

    gl_FragColor = vec4(color, 1.0);
}
```

![Affichage Code 5](/images/affichage-code-5.gif)

### Déclaration des couleurs

```glsl 
vec3 colorA = vec3(0.149, 0.141, 0.912);
vec3 colorB = vec3(1.000, 0.833, 0.224);
``` 

Nous déclarons deux vecteurs RGB (sans le canal alpha) qui serviront de base pour notre mélange de couleurs. `colorA` est un bleu foncé et `colorB` un jaune orangé. Nous ajouterons la transparence plus tard dans le programme.

### Contrôle du dégradé

```glsl 
float pct = abs(sin(u_time));
``` 

Cette ligne calcule un pourcentage qui varie continuellement entre 0.0 et 1.0. `sin(u_time)` génère une valeur oscillant entre -1 et 1, et `abs()` convertit les valeurs négatives en positives. C'est cette oscillation qui transforme notre mélange de couleurs statique en animation.

### La fonction `mix()`

```glsl 
color = mix(colorA, colorB, pct);
gl_FragColor = vec4(color, 1.0);
```

La fonction `mix()` interpole (mélange) deux couleurs selon un pourcentage. Quand `pct = 0.0`, elle affiche `colorA` (bleu) à 100%. À `pct = 0.5`, un mélange 50/50 des deux couleurs. Et à `pct = 1.0`, `colorB` (jaune) à 100%. Le résultat est une animation fluide qui oscille entre le bleu et le jaune, créant un effet de pulsation.

---

## Une transition de couleur linéaire

```glsl
#ifdef GL_ES
precision mediump float;
#endif

#define PI 3.14159265359

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

vec3 colorA = vec3(0.149, 0.141, 0.912);
vec3 colorB = vec3(1.000, 0.833, 0.224);

float plot(vec2 st, float pct){
  return  smoothstep( pct-0.01, pct, st.y) -
          smoothstep( pct, pct+0.01, st.y);
}

void main() {
    vec2 st = gl_FragCoord.xy/u_resolution.xy;
    vec3 color = vec3(0.0);

    vec3 pct = vec3(st.x);

    // pct.r = smoothstep(0.0, 1.0, st.x);
    // pct.g = sin(st.x*PI);
    // pct.b = pow(st.x, 0.5);

    color = mix(colorA, colorB, pct);

    // Tracer la ligne de transition pour le canal rouge
    color = mix(color, vec3(1.0, 0.0, 0.0), plot(st, pct.r));

    gl_FragColor = vec4(color, 1.0);
}
```

![Affichage Code 6](/images/affichage-code-6.png)

La principale différence ici est que la transition est linéaire et figée dans le temps (elle ne change pas avec `u_time`), ce qui permet d'observer comment la fonction `mix()` fonctionne spatialement.

### Normalisation avec `.xy`

```glsl
vec2 st = gl_FragCoord.xy/u_resolution.xy;
```

L'utilisation de `.xy` explicite est redondante mais plus claire : nous prenons uniquement les composantes x et y de chaque vecteur.

### Le vecteur de pourcentage

```glsl
vec3 pct = vec3(st.x);
```

Cette ligne crée un vecteur à trois composantes où chaque canal (rouge, vert, bleu) utilise la même valeur : la coordonnée x. La transition sera donc identique pour tous les canaux de couleur, créant une transition uniforme de gauche à droite.

### Expérimentation avancée (lignes commentées)

```glsl
// pct.r = smoothstep(0.0, 1.0, st.x);
// pct.g = sin(st.x*PI);
// pct.b = pow(st.x, 0.5);
```

Ces lignes permettent de créer des transitions différentes pour chaque canal. Le canal rouge utiliserait une transition en forme de S avec `smoothstep`, le vert une transition sinusoïdale, et le bleu une transition en racine carrée. Si vous décommentez ces lignes, chaque composante RGB suivra sa propre courbe, créant des mélanges de couleurs complexes et des effets visuels uniques.

### Visualisation de la courbe

```glsl
color = mix(color, vec3(1.0, 0.0, 0.0), plot(st, pct.r));
```

Cette ligne superpose une ligne rouge qui trace la courbe de transition du canal rouge. La ligne suit une diagonale car `pct.r = st.x` (transition linéaire). Si vous décommentez les lignes d'expérimentation, vous verrez la ligne rouge suivre la nouvelle courbe définie.

Le résultat est un dégradé horizontal du bleu au jaune avec une ligne rouge diagonale qui montre visuellement la progression de la transition.

> **Essayez ça :** Décommentez ces trois lignes et regardez ce qui se passe !

---

## La création de formes

Abordons maintenant la création de formes 2D :

```glsl
// Author @patriciogv - 2015
// http://patriciogonzalezvivo.com

#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

void main(){
    vec2 st = gl_FragCoord.xy/u_resolution.xy;
    vec3 color = vec3(0.0);

    // Coin inférieur gauche
    vec2 bl = step(vec2(0.1), st);
    float pct = bl.x * bl.y;

    // Coin supérieur droit
    vec2 tr = step(vec2(0.1), 1.0 - st);
    pct *= tr.x * tr.y;

    color = vec3(pct);

    gl_FragColor = vec4(color, 1.0);
}
```

![Affichage Code 7](/images/affichage-code-7.png)

### La fonction `step()`

```glsl
vec2 bl = step(vec2(0.1), st);
```

La fonction `step(edge, x)` compare deux valeurs et retourne 0.0 si `x < edge`, ou 1.0 si `x >= edge`. Dans ce cas, `step(vec2(0.1), st)` retourne 0.0 pour toutes les coordonnées où `st.x < 0.1` ou `st.y < 0.1`, et 1.0 pour les coordonnées où `st.x >= 0.1` ET `st.y >= 0.1`.

### Le coin inférieur gauche

```glsl
vec2 bl = step(vec2(0.1), st);
float pct = bl.x * bl.y;
```

Cette multiplication crée une zone blanche qui commence à 10% du bord gauche et 10% du bord inférieur. En multipliant `bl.x * bl.y`, nous obtenons 1.0 uniquement dans la zone où les deux conditions sont vraies.

### Le coin supérieur droit

```glsl
vec2 tr = step(vec2(0.1), 1.0 - st);
pct *= tr.x * tr.y;
```

Ici, `1.0 - st` inverse les coordonnées, créant une marge de 10% depuis le coin supérieur droit. La multiplication finale `pct *= tr.x * tr.y` combine les deux conditions, créant un rectangle centré avec des marges de 10% sur tous les côtés.

Le résultat est un carré blanc centré avec des bordures noires de 10% sur chaque côté de l'écran. `bl` crée une bordure noire en bas à gauche, `tr` crée une bordure noire en haut à droite, et leur combinaison crée un rectangle avec des marges uniformes.

> **Expérimentation** : Changez `vec2(0.1)` par d'autres valeurs comme `vec2(0.2)` ou `vec2(0.3)` pour créer des bordures plus épaisses ou plus fines !

---

### Bibliographie

- [The Book of Shaders](https://thebookofshaders.com/) - Guide complet et interactif
- [ShaderToy](https://www.shadertoy.com/) - Plateforme pour expérimenter et partager des shaders
- [GLSL Sandbox](http://glslsandbox.com/) - Environnement de test en ligne
