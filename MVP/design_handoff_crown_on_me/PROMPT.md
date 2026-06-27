# PROMPT — à coller dans Claude Code

Construis le MVP **Crown On Me** : un seul fichier `index.html` (HTML + CSS + JS vanilla, aucun build, ouvrable dans Chrome). Mobile-first, viewport 390px. Suis cette spec à la lettre.

---

## RÈGLES NON NÉGOCIABLES

1. **3 couleurs uniquement** : Noir `#0d0d0b` · Amber `#c9854a` · Ivoire `#f2ece0`. Auxiliaires : Kraft `#d4b896`, Taupe `#8b7355`.
2. **Aucun dégradé.** Aplats francs. **Coins carrés partout** (border-radius: 0).
3. **1 seul bouton amber par écran.** Le reste = boutons outline noir.
4. **Typo** : Cormorant Garamond 300 → titres/accroches uniquement (italique = accent amber). DM Sans 300 → tout le reste. Monospace → petites légendes techniques.
5. Style **éditorial** : filets fins 1px, beaucoup d'air, pas de look « app gadget ».
6. Pas de compte, pas de login, pas de backend, pas de base de données.

```html
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;1,300&family=DM+Sans:wght@300;400&display=swap" rel="stylesheet">
```

---

## STRUCTURE

SPA mono-fichier. 7 `<section>` qu'on affiche/masque en JS (pas de rechargement). Chaque écran fait 390px de large, plein écran en hauteur, et a :
- une **status bar** (`9:41` à gauche, `● ● ●` à droite, taupe 12px) ;
- un **header** : wordmark `Crown <em>on me</em>` (Cormorant, `on me` en italique amber) + pagination `NN / 07` (mono, taupe), séparés par un filet noir 1px ;
- un **corps** (padding 24px) ;
- un **CTA amber pleine largeur collé en bas** (full-bleed, hauteur 60px, texte noir, UPPERCASE, letter-spacing 0.16em).

Bouton outline = `border:1px solid #0d0d0b`, fond transparent, hauteur 52px, UPPERCASE.

---

## LES 7 ÉCRANS

**1 · ACCUEIL** → bouton « Commencer »
Eyebrow « Cheveux texturés et particulièrement afro ». Display Cormorant 74px « Crown / *on me* ». Filet. Accroche Cormorant 31px « Visualise ton style avant de réserver. / *Fini les mauvaises surprises.* ». Note taupe « Pas de compte · Accès immédiat ». **CTA amber : Commencer** → écran 2.

**2 · UPLOAD** → bouton « Choisir mon style »
Titre « Ta *photo* ». Dropzone en pointillés (`<input type="file" accept="image/*">` caché) avec « + » et label « Glisse ta photo ou choisis ci-dessous » ; affiche la prévisu une fois l'image lue (FileReader → base64). Bouton outline « Choisir depuis ma galerie ». Message taupe « Ta photo est traitée localement — non stockée. ». **CTA amber : Choisir mon style** (désactivé tant qu'aucune photo) → écran 3.

**3 · CHOIX DU STYLE** → bouton « Simuler ce style sur moi »
Titre « Les réalisations de *Nadège* ». Grille 2×2 de 4 cartes (image + nom + « Réalisé par Nadège ») : Box Braids longues · Twist out naturel · Locks mi-longues · Tresses collées. Carte sélectionnée = bordure amber 2px + badge « Sélectionné » (une seule active). Message discret : « Ces coiffures ont été réalisées par Nadège. En simulant son style, tu sais exactement ce qu'elle peut faire pour toi. ». **CTA amber : Simuler ce style sur moi** → écran 4 + appel Fal.ai.

**4 · CHARGEMENT** (MODE NUIT : fond `#0d0d0b`, texte `#f2ece0`) — pas de CTA
Spinner (cercle 56px, bordure amber, top transparent, rotation 1.1s). Titre Cormorant « Crown *on me* travaille sur ta coiffure… ». Barre de progression amber animée. Légende mono « Durée estimée · 10–20 secondes ». Transition auto vers écran 5 à la réponse Fal.ai.

**5 · RÉSULTAT AVANT/APRÈS** → bouton « Prendre RDV avec Nadège »
Titre « Avant */* Après ». Lien discret « Partager » (→ `navigator.share`). Grille 2 colonnes : AVANT (bordure noire) | APRÈS (bordure amber 2px). Mention « Style Box Braids longues réalisé par Nadège K. · Studio Crown — Paris 18e ». Deux boutons outline : « Autre style » (→ 3) · « Voir son profil » (→ 6). **CTA amber : Prendre RDV avec Nadège** (→ `tel:`/`mailto:`).

**6 · PROFIL COIFFEUSE** → bouton « Prendre RDV »
Avatar carré 88px + nom Cormorant « Nadège *K.* » + « Studio Crown — Paris 18e ». Ligne note « ★ 4,9 · 127 avis ». Chips spécialités (outline) : Box Braids · Twists · Locks · Twist out naturel. Bloc encadré : Tarif « À partir de 80€ » | Dispo « Lun–Sam · 9h–19h ». `@studio.crown`. Bouton outline « Voir son portfolio » (→ 7). **CTA amber : Prendre RDV**.

**7 · PORTFOLIO** → bouton « Prendre rendez-vous avec Nadège »
Titre « Le portfolio de *Nadège* ». Grille 2×2 de 4 photos cliquables (clic → relance écran 4) + nom du style. Badge « ★ Les styles que tu vois viennent de vraies coiffeuses. ». **CTA amber : Prendre rendez-vous avec Nadège**.

---

## NAVIGATION
1 → 2 → 3 → 4 → 5. Depuis 5 : « Autre style »→3, « Voir son profil »→6. 6 « Voir son portfolio »→7. 7 (clic style)→4 (relance). Garder en mémoire JS : la photo (base64) + le style choisi.

---

## FAL.AI
Modèle `fal-ai/flux/dev` (image-to-image), 100% côté client. Clé API saisie dans un champ settings discret en bas de page, stockée en **`sessionStorage`** (jamais localStorage). Si absente → « Entre ta clé API Fal.ai pour continuer ». Appel `fetch()` avec la photo base64 + prompt :
`"afro hairstyle, textured hair, natural hair, [style choisi], photorealistic, same person, same face, no background change"`.
Gérer l'erreur réseau (message lisible + réessayer).

---

## DONNÉES (hardcodées — 1 seule coiffeuse)
```javascript
const coiffeuse = {
  nom: "Nadège K.", salon: "Studio Crown — Paris 18e",
  specialites: ["Box Braids","Twists","Locks","Twist out naturel"],
  note: 4.9, avis: 127, tarif: "À partir de 80€",
  disponibilite: "Lun–Sam · 9h–19h",
  photo: "https://i.pravatar.cc/150?img=47",
  instagram: "@studio.crown", telephone: "06 XX XX XX XX",
  styles: [
    { nom: "Box Braids longues", img: "https://images.unsplash.com/photo-1594744803329-e58b31de8bf?w=400" },
    { nom: "Twist out naturel",  img: "https://images.unsplash.com/photo-1520813792240-56fc4a3765a7?w=400" },
    { nom: "Locks mi-longues",   img: "https://images.unsplash.com/photo-1580618864182-fce0a62a2037?w=400" },
    { nom: "Tresses collées",    img: "https://images.unsplash.com/photo-1522337360788-8b13dee7a37e?w=400" }
  ]
}
```

---

## RÉFÉRENCES VISUELLES (dans ce dossier)
- `maquettes/Crown On Me — 7 écrans.dc.html` → **ouvre-le dans un navigateur pour voir exactement le rendu attendu.**
- `crown-on-me-identite.html` → la charte. `README.md` → la spec longue détaillée si besoin de précisions.

## LIVRABLE
Un seul `index.html`, commenté par section, mobile-first (testé à 390px), fidèle aux maquettes, fonctionnel avec une clé Fal.ai.
