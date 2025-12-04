+++
title = "Veille technologique"
weight = 1
+++

## Veille Technologique

J'ai commencé par poser à ChatGPT cette question :

> **Is GLSL still useful in today's day and age?**

---

### Réponse : Oui, GLSL est toujours pertinent

GLSL est toujours un langage d'actualité, mais son rôle a un peu évolué. Voici la situation réelle (en 2025) :

---

## ✅ Où GLSL est encore très utilisé

### 1. OpenGL & OpenGL ES

Tout projet basé sur OpenGL utilise du GLSL. Et il y en a encore énormément :

- Moteurs de jeux indépendants
- Applications scientifiques
- Outils multiplateformes
- Jeux mobiles
- Logiciels anciens ou maintenus

### 2. WebGL / WebGL2

- Tous les shaders exécutés dans le navigateur via WebGL sont en GLSL ES
- Les bibliothèques comme **Three.js**, **Babylon.js**, etc. génèrent toujours du GLSL

### 3. Art shader & apprentissage

GLSL reste le langage le plus simple pour apprendre la programmation GPU :

- [ShaderToy](https://www.shadertoy.com/)
- [Book of Shaders](https://thebookofshaders.com/)
- Sites de visualisation créative

Il est parfait pour comprendre les bases des shaders.

---

## ⚠️ Où GLSL est moins dominant aujourd'hui

Beaucoup de moteurs modernes préfèrent des langages plus flexibles :

### HLSL

- **Unity**
- **Unreal**
- **DirectX**

HLSL est devenu la "langue source" dans beaucoup de pipelines, puis il est cross-compilé en GLSL, SPIR-V ou Metal.

### WGSL (WebGPU)

- Le remplaçant de WebGL, **WebGPU**, utilise **WGSL**, pas GLSL
- Le web commence à migrer vers ce langage

### SPIR-V

- Pour **Vulkan**, on n'a plus besoin d'écrire du GLSL directement
- La plupart des moteurs écrivent en HLSL ou utilisent un langage haut niveau qui compile en SPIR-V

---

## 🎯 Alors, vaut-il encore la peine d'apprendre le GLSL ?

### Oui, absolument, parce que :

- C'est excellent pour apprendre les concepts fondamentaux des shaders
- 90% de la logique/maths des shaders est identique en HLSL, GLSL ou WGSL
- C'est un langage simple et direct pour comprendre le pipeline GPU
- Énormément de projets en production continuent de l'utiliser

### Mais :

Si tu veux travailler avec des moteurs récents (Unity, Unreal, Vulkan), il faudra aussi connaître HLSL à un moment.

---

## 🚀 Recommandation

| Objectif | Langage recommandé |
|----------|-------------------|
| **Pour apprendre les shaders** | Commence par **GLSL** |
| **Pour moteurs modernes** | Rajoute **HLSL** ensuite |
| **Pour le futur du web** | **WGSL** est utile |

---

## Conclusion

Cette réponse correspond à la réalité. GLSL est un langage assez difficile, mais encore d'actualité. Il permet de comprendre la relation entre les différentes fonctions mathématiques et les shaders créés par nos cartes graphiques.
