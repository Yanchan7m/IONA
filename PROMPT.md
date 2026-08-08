# Prompt de génération — site logistique « scroll 3D »

> Copie-colle ce prompt dans un assistant de code (Claude, v0, Cursor…) pour
> générer la version production du site. Adapte les parties entre `⟨…⟩`.

---

**Rôle** — Tu es directeur artistique + développeur front senior spécialisé
dans les sites immersifs « awwwards ». Tu livres un code propre, accessible et
performant.

**Objectif** — Construis une landing page one-page pour ⟨IONA Freight, société
de fret maritime mondiale⟩. L'expérience doit donner le sentiment « ce n'est
pas un site ennuyeux » : la 3D et les animations sont pilotées par le scroll.

**Stack imposée**
- Framework : **Next.js (App Router)** + TypeScript.
- 3D : **React Three Fiber** + **@react-three/drei** (Three.js).
- Scroll : **Lenis** (smooth scroll) branché à **GSAP ScrollTrigger** pour les
  timelines (`pin` + `scrub`).
- Style : **Tailwind CSS**, tokens de thème clair/sombre.
- Modèles 3D : `.glb` chargés via `useGLTF` (placeholder : conteneurs, grue
  portique, porte-conteneurs). Fournis des primitives si le `.glb` manque.

**Direction artistique**
- Univers : port industriel. Palette : graphite `#0b0f14`, acier `#12181f`,
  texte `#e8edf2`, accent **orange sécurité `#ff5a1f`**, bleu conteneur
  `#3a7bd5`. Version claire cohérente.
- Typo : grotesque très gras en très grand pour les titres (cinétiques,
  majuscules, interlettrage serré) + **police mono** pour les labels/codes ISO
  façon manifeste de port.
- Motion : inertiel, jamais gratuit. Respecte `prefers-reduced-motion`.

**Sections & scénario au scroll**
1. **Hero épinglé** — un conteneur 3D qui tourne lentement ; poussière ambiante
   (particules). Titre « Reliability at every milestone ». Indice de scroll.
2. **Chantier (pinned + scrub)** — des conteneurs éparpillés qui **s'assemblent
   en mur** au fil du scroll ; la caméra se redresse ; titre qui se révèle.
3. **Grue portique** — une grue soulève un conteneur ; le chariot et le
   palonnier suivent la progression du scroll.
4. **Océan** — un porte-conteneurs traverse une mer animée (shader ou canvas) ;
   parallaxe ; slogan « Legacies that work as hard as you do ».
5. **Globe des routes** — globe 3D en rotation avec des arcs animés entre ports ;
   liste de routes « live » à côté.
6. **Stats** (compteurs animés) + **manifeste** (table mono, codes ISO, statut).
7. **Trusted by** (logos) + **CTA** + footer.

**Exigences techniques**
- 60 fps cible : n'anime que les scènes visibles (IntersectionObserver ou
  ScrollTrigger `onEnter/onLeave`), `will-change` maîtrisé, pas de layout thrash.
- Accessibilité : structure sémantique, focus visible, contrastes AA, fallback
  sans animation si `prefers-reduced-motion`.
- SEO/meta de base, responsive mobile → desktop.
- Code commenté aux endroits clés (mapping scroll → transform).

**Livrable** — un repo qui démarre avec `npm i && npm run dev`, structuré en
composants (`Hero`, `YardScene`, `CraneScene`, `OceanScene`, `GlobeScene`,
`Stats`, `Manifest`, `Footer`), avec un README expliquant la technique.
