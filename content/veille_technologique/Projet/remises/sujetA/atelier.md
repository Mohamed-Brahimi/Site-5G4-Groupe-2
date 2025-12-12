+++
title = "Atelier"
weight = 3
+++

## Atelier de GLSL

### Préparation

Il y a deux façons principales de coder en GLSL pour cet atelier :

1. **ShaderToy** : [https://www.shadertoy.com/](https://www.shadertoy.com/) - Rapide pour tester des idées
2. **Dépôt Git** : [https://github.com/Mohamed-Brahimi/Atelier-glsl.git](https://github.com/Mohamed-Brahimi/Atelier-glsl.git) - Meilleur pour sauvegarder votre travail

Je recommande fortement le dépôt Git. Les notes de cours utilisent VS Code, donc vous aurez plus de facilité à suivre.

---

## Option 1 : ShaderToy (rapide mais temporaire)

C'est parfait si vous voulez juste expérimenter sans installer quoi que ce soit.

1. Allez sur [https://www.shadertoy.com/](https://www.shadertoy.com/)
2. Cliquez sur **"New"** en haut à droite
3. Un éditeur s'ouvrira avec un shader de base déjà en place

Voici l'interface :

![Légende de l'interface ShaderToy](/images/image/atelier-1.png)

## Option 2 : Dépôt Git (recommandé)

### Installation

**1. Fork le dépôt**

Allez sur [https://github.com/Mohamed-Brahimi/Atelier-glsl.git](https://github.com/Mohamed-Brahimi/Atelier-glsl.git) et cliquez sur **"Fork"** en haut à droite. Ça va créer votre propre copie.

**2. Clone sur votre machine**

```bash
git clone https://github.com/VOTRE-NOM-UTILISATEUR/Atelier-glsl.git
cd Atelier-glsl
```

**3. Ouvrir dans VS Code**

```bash
code .
```

**4. Dev Container**

**Important** : Docker doit être installé et en marche sur votre machine.

VS Code va détecter le fichier `.devcontainer` et proposer d'ouvrir le projet dans un conteneur. Cliquez sur **"Reopen in Container"** et attendez que ça se construise (ça peut prendre quelques minutes la première fois).

### Comment utiliser l'environnement

**Écrire votre code**

Tout votre code va dans `Atelier.frag`.

**Voir le résultat**

- Faites `Ctrl+Shift+P` (ou `Cmd+Shift+P` sur Mac)
- Tapez **"Show GLSL Canvas"**
- Une preview s'ouvre à côté de votre code

**Tester que ça marche**

Copiez cet exemple dans `Atelier.frag` :

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

Si vous voyez un dégradé qui change avec le temps, c'est bon.

**Sauvegarder**

```bash
git add .
git commit -m "Ajout exercice 1" # ou autre description
git push
```

---

## Exercice 1 : Créer un cercle

### Ce qu'on veut faire

Un cercle blanc centré sur fond noir. Simple, mais ça introduit des concepts importants.

### Concepts clés

**Centrer les coordonnées**

```glsl
st -= 0.5;
```

Ça déplace l'origine (0,0) au centre de l'écran au lieu du coin.

**La fonction `length()`**

Elle calcule la distance entre un point et l'origine. C'est exactement ce qu'il faut pour un cercle, puisque tout les points d'un cercle sont à la même distance du centre.

### À faire

1. Normalisez les coordonnées : `gl_FragCoord.xy / u_resolution`
2. Centrez-les avec `st -= 0.5`
3. Calculez la distance du centre avec `length(st)`
4. Utilisez `smoothstep()` pour créer un cercle net autour d'un certain rayon 
5. Inversez si nécessaire pour avoir blanc sur noir  ( 1 - cercle)

### Résultat attendu

Un cercle blanc bien net au centre, avec des bords doux grâce à `smoothstep()`.

---

## Exercice 2 : Cercle animé

### Objectif

Animer le cercle pour qu'il change de taille et de couleur avec le temps. L'idée c'est de simuler un cycle jour/nuit - soleil chaud qui devient lune froide.

### Nouveaux concepts

**Animation de taille**

Au lieu d'un rayon fixe (genre `0.3`), on le fait varier :
```glsl
float rayon = Longueur + 0.1 * abs(sin(u_time));
```

**Mix de couleurs**

On mélange deux couleurs selon le temps. Je suggère :
- Jaune-orange pour le "soleil"
- Blanc-bleuté pour la "lune"

Utilisez `mix()` comme vu dans les notes de cours.

### Ce que vous devriez voir

Un cercle qui pulse (change de taille) et qui oscille entre des couleurs chaudes et froides. Le fond peut aussi changer si vous voulez pousser plus loin.


---

## Exercice 3 : Aurore boréale

### L'idée

Créer quelque chose qui ressemble à une aurore boréale : un ciel bleu-noir avec des bandes de lumière verte qui ondulent.

### Partie 1 : Une bande qui ondule

**Ce dont vous avez besoin :**
- Coordonnées normalisées
- `u_time` pour l'animation
- `sin()` pour créer l'onde
- `mix()` pour les couleurs
- `plot()` pour la bande lumineuse

**Créer l'onde**

```glsl
float wave = sin(vecteur x * 6.0 + temps);
```

Ça va créer une vague qui se déplace horizontalement.


**Couleurs suggérées**

- Fond : Bleu très foncé, presque noir
- Aurore : `vec3(0.0, 1.0, 0.5)` pour du vert, ou `vec3(0.0, 0.8, 0.8)` pour du bleu-vert

**Validation**

Vous devez avoir :
- Un fond stable et sombre
- Une bande qui bouge sans saccades
- Des transitions douces (pas de bords durs)

Si l'onde ne bouge pas, vous avez probablement oublié `u_time` quelque part.

### Partie 2 : Plusieurs bandes

Maintenant qu'une bande fonctionne, on en ajoute d'autres pour plus de réalisme.

**L'approche**

1. Dupliquez votre code de bande
2. Changez les paramètres :
   - Vitesse différente : `u_time * 0.7` au lieu de `u_time`
   - Position différente : ajoutez `0.2` à l'équation
   - Couleur différente : violet `vec3(0.6, 0.2, 0.8)` ou cyan `vec3(0.0745, 0.5804, 0.3843)`
3. Ajouter votre bande

**Ce que ça donne**

Plusieurs bandes qui bougent indépendamment, avec différentes couleurs et vitesses. Ça devrait vraiment ressembler à une aurore maintenant.

---

## Pour aller plus loin


Allez voir sur [ShaderToy](https://www.shadertoy.com/browse) de nouveaux shaders. Vous pourrez voir que nous n'avons touché qu'au basique.

---

## Solutions

### Exercice 1 : Cercle simple

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

void main() {
    vec2 st = gl_FragCoord.xy / u_resolution.xy;
    st -= .5;
    vec3 color = vec3(1.0);
    
    float rayon = length(st);
    float cercle = smoothstep(0.5, 0.5, rayon);
    cercle = 1.0 - cercle;
    color = vec3(cercle);
    
    gl_FragColor = vec4(color, 1.0);
}
```

### Exercice 2 : Cercle animé (Cycle jour/nuit)

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

void main() {
    vec2 st = gl_FragCoord.xy / u_resolution.xy;
    st -= .5;
    vec3 color = vec3(1.0);
    vec3 lune = vec3(0.4275, 1.0, 0.9725);
    vec3 soleil = vec3(0.8471, 0.5804, 0.0);
    vec3 cielMatin = vec3(0.0078, 0.6039, 1.0);
    vec3 cielSoir = vec3(0.0157, 0.0392, 0.2314);
    
    float rayon = length(st) + 0.1 * abs(sin(u_time));
    float cercle = smoothstep(0.5, 0.5, rayon);
    cercle = 1.0 - cercle;
    color = vec3(cercle);
    
    vec3 couleurFond = mix(cielMatin, cielSoir, abs(sin(u_time * .25)));   
    vec3 couleurCerle = mix(soleil, lune, abs(sin(u_time * .25)));
    color *= couleurCerle;
    color = mix(couleurFond, color, .50);
    
    gl_FragColor = vec4(color, 1.0);
}
```

### Exercice 3 : Aurore boréale

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

float plot(vec2 st, float pct){
    return smoothstep(pct-0.2, pct, st.y) -
           smoothstep(pct, pct+0.2, st.y);
}

void main() {
    vec2 st = gl_FragCoord.xy / u_resolution.xy;
    st = st * 2.0 - 1.0; // from (0,1) to (-1,1)
    
    // Première bande verte
    float wave = sin(st.x * 6.0 + u_time) * .5;
    vec3 color = vec3(0);
    float pct = plot(st, wave);
    color = mix(vec3(0.0, 0.0039, 0.1059), vec3(0.0902, 0.0, 0.1529), abs(sin(u_time*.01)));
    color += pct * vec3(0.0745, 0.5804, 0.3843);
    
    // Deuxième bande (plus rapide)
    st += .3;
    wave = sin(st.x * 6.0 + u_time * 1.5) * .5;
    pct = plot(st, wave);
    color += pct * vec3(0.0157, 0.3412, 0.1255);

    gl_FragColor = vec4(color, 1.0);
}
```

---