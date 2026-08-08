# IONA — démo « site pas ennuyeux »

Landing page logistique avec **conteneurs 3D qui s'empilent au scroll**, dans
l'esprit du reel qui a inspiré le projet. **Zéro dépendance** : tout est dans
`index.html` (HTML + CSS + JS inline). Ouvre le fichier dans un navigateur, ou
sers le dossier (`python3 -m http.server`) et va sur `localhost:8000`.

## Comment on fait « ce genre de site »

Trois ingrédients, du plus simple au plus avancé :

### 1. Le scroll pilote l'animation (« scroll-driven »)
Le cœur de l'effet. On mesure la progression du scroll dans une section, on la
normalise entre `0` et `1`, et on s'en sert pour interpoler des transformations.

Dans cette démo (voir `index.html`) :
- Une section `.track` haute de `420vh` contient une `.stage` en
  `position: sticky` → la scène reste **épinglée** à l'écran pendant qu'on
  traverse la piste (technique du *pinned scene*, celle des pages produit Apple).
- Un listener `scroll` calcule `progress = -track.top / (track.height - innerHeight)`.
- Cette valeur est **lissée** (`lerp`) dans une boucle `requestAnimationFrame`
  pour un rendu inertiel, puis mappée sur les transforms de chaque conteneur
  (position de départ éparpillée → position finale empilée).

### 2. La 3D
Ici, en **CSS 3D pur** : chaque conteneur est une vraie boîte à 6 faces
(`transform-style: preserve-3d`, `perspective` sur le parent). Les faces ont une
tôle ondulée en `repeating-linear-gradient` et un code ISO stencilé. Un
conteneur = un cube, donc le CSS 3D suffit et reste ultra-léger.

Pour des modèles riches (grue, bateau, avion comme dans le reel), on passe à
**WebGL** :
- **Three.js** (ou **React Three Fiber** en React) pour la scène temps réel.
- On importe des modèles **`.glb`/`.gltf`** (faits sur Blender / Spline).
- Alternative sans code 3D : **Spline** (éditeur visuel, export web).

### 3. Le liant (fluidité + orchestration)
- **Smooth scroll** : [Lenis](https://github.com/darkroomengineering/lenis) —
  ici réimplémenté en mini (on lisse seulement la variable de scène).
- **Orchestration** : [GSAP](https://gsap.com) + **ScrollTrigger**, le standard
  pro pour caler des timelines sur le scroll (pin, scrub, snap).
- **Révélations** : `IntersectionObserver` (natif) pour faire apparaître les
  blocs — utilisé ici pour les textes et les compteurs.

### La stack « pro » typique d'un site comme le reel
```
Next.js / Astro
 ├─ Lenis            (smooth scroll)
 ├─ GSAP ScrollTrigger  (timelines au scroll : pin + scrub)
 ├─ React Three Fiber / Three.js  (scène WebGL)
 │   └─ modèles .glb (Blender / Spline)
 └─ Tailwind / CSS   (typo, layout, thèmes)
```

Cette démo prouve le concept **sans rien installer** ; l'étape suivante serait
de remplacer les cubes CSS par une vraie scène Three.js avec des `.glb`.

## Bonnes pratiques respectées
- Accessibilité : `prefers-reduced-motion` désactive le scrubbing 3D.
- Thèmes clair **et** sombre (tokens CSS + toggle).
- Responsive, focus visible, aucun texte factice.
