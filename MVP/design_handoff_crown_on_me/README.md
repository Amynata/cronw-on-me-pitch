# Handoff — Crown On Me · MVP 7 écrans

> Document à donner à Claude Code CLI pour construire le `index.html` du MVP.
> Il est **autosuffisant** : tu peux l'implémenter sans avoir assisté à la conversation de design.

---

## 1. Objectif

Crown On Me est une app mobile/web qui permet aux femmes aux cheveux texturés (et particulièrement afro) de **simuler une coiffure sur leur propre photo** avant de réserver chez une coiffeuse. L'IA (Fal.ai) génère un avant/après réaliste en quelques secondes.

Le livrable attendu : **un seul fichier `index.html`** (HTML + CSS + JS vanilla), ouvrable directement dans Chrome sans serveur ni build, mobile-first (viewport ~390px), fonctionnel avec une clé API Fal.ai.

---

## 2. À propos des fichiers de design

Les fichiers de ce bundle sont des **références de design en HTML** — des maquettes montrant le look et le comportement voulus, **pas du code de production à copier-coller**.

- `maquettes/Crown On Me — 7 écrans.dc.html` — les 7 écrans maquettés (format 390×844 chacun, disposés côte à côte sur un canvas). C'est la **source de vérité visuelle**.
- `crown-on-me-identite.html` — la charte (wordmark, palette, typo, règles d'usage).
- `crown-on-me-context-brief.md` — la fiche contexte produit complète (personas, données Nadège, intégration Fal.ai, ce que le MVP ne fait pas).

Ta tâche : **recréer ces maquettes** en HTML/CSS/JS vanilla dans un seul `index.html`, fidèlement à la charte ci-dessous.

## 3. Fidélité

**Haute fidélité (hifi).** Couleurs, typographie, espacements et hiérarchie sont définitifs. Reproduis l'UI au pixel près en respectant les tokens de la section 5. Les zones d'image sont des **placeholders rayés** dans les maquettes — à remplacer par les vraies photos (voir section 8).

---

## 4. Identité visuelle (à respecter strictement)

### Couleurs — 3 couleurs principales
| Nom | Hex | Usage |
|---|---|---|
| Noir encre | `#0d0d0b` | Texte · fond nuit (écran Chargement) · traits/filets |
| Amber | `#c9854a` | Accent · **un seul CTA par écran** · italiques de signature |
| Ivoire | `#f2ece0` | Fond principal (mode jour) |

Auxiliaires, avec parcimonie :
| Kraft | `#d4b896` | Filets secondaires, séparateurs internes |
| Taupe | `#8b7355` | Textes secondaires, eyebrows, légendes |

Couleurs dérivées utilisées dans les maquettes (cartes blanches, rayures placeholder) :
- Blanc carte : `#ffffff`
- Bordure nuit (séparateurs sur fond noir) : `#3a352c`
- Rayures placeholder ivoire : `repeating-linear-gradient(135deg, #e7dcc4 0 7px, #efe7d7 7px 14px)`
- Rayures placeholder amber (élément sélectionné / après) : `repeating-linear-gradient(135deg, #cdbb90 0 7px, #d8c8a8 7px 14px)`

**Règles couleur :** aplats francs, **aucun dégradé** (les gradients ci-dessus servent uniquement de hachures de placeholder, à supprimer dès qu'on a les vraies images). Amber sur fond noir ou ivoire uniquement, jamais sur kraft. **1 seul CTA amber par écran.**

### Typographie (Google Fonts)
```html
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;1,300;1,400&family=DM+Sans:wght@200;300;400;500&display=swap" rel="stylesheet">
```
- **Cormorant Garamond** weight 300 — uniquement titres et accroches. Italique = accent amber, avec parcimonie.
- **DM Sans** weight 300 (400/500 pour micro-labels) — tout le reste : UI, body, boutons, labels.
- **monospace** (system) — uniquement les micro-légendes techniques : pagination `01 / 07`, labels de placeholder (`AVANT`, `APRÈS`), `@studio.crown`.

### Wordmark (header de chaque écran)
`Crown` en Cormorant 300 + `on me` en Cormorant 300 *italique* couleur amber, inline.
```html
<span class="wordmark">Crown <em>on me</em></span>
```

---

## 5. Design tokens

**Cadre écran :** 390 × 844 px, `background:#f2ece0`, `border:1px solid #0d0d0b`, `overflow:hidden`, flex colonne. (En prod web réelle : pleine hauteur du viewport, le 844 est la cible de maquette.)

**Espacements** (px) : 8 · 10 · 12 · 14 · 18 · 22 · 24 · 28 · 40. Padding latéral du corps = **24px**. Padding header = `16px 24px`.

**Filets :** principal `1px solid #0d0d0b` ; secondaire `1px solid #d4b896` ; pointillé dropzone `1px dashed #0d0d0b`.

**Border-radius : 0 partout** (look éditorial, pas de coins arrondis « app gadget »). Seule exception tolérée : avatar peut rester carré (il l'est dans la maquette).

**Échelle typo :**
| Rôle | Font | Taille | Notes |
|---|---|---|---|
| Wordmark header | Cormorant 300 | 19px | letter-spacing 0.04em |
| Display accueil | Cormorant 300 | 74px | line-height 0.92 |
| Titre écran | Cormorant 300 | 34–42px | line-height ~1.0 |
| Accroche | Cormorant 300 | 31px | line-height 1.25 |
| Eyebrow | DM Sans 400 | 11px | uppercase, letter-spacing 0.18–0.22em, taupe |
| Body / description | DM Sans 300 | 13–15px | taupe pour secondaire |
| Label CTA | DM Sans 300 | 13px | uppercase, letter-spacing 0.16em |
| Pagination / mono | monospace | 11px | taupe |

**Boutons :**
- CTA principal (amber) : pleine largeur, **full-bleed** (collé aux bords du cadre, pas de marge latérale), hauteur **60px**, `background:#c9854a`, texte `#0d0d0b` uppercase letter-spacing 0.16em. **Un seul par écran.**
- Bouton secondaire (outline) : `border:1px solid #0d0d0b`, fond transparent, texte noir, hauteur **52px**, uppercase.
- Status bar simulée en haut : `9:41` à gauche, `● ● ●` à droite, 12px taupe.

---

## 6. Écrans (spec détaillée)

Chaque écran partage la même ossature :
`status bar` → `header` (wordmark à gauche + pagination `NN / 07` à droite, séparé par un filet noir 1px) → `corps` (flex:1, padding 24px) → `CTA full-bleed` en bas.

### Écran 1 — Accueil
- **But :** entrée immédiate, sans compte ni login.
- Eyebrow (taupe) : « Cheveux texturés / et particulièrement afro ».
- Display Cormorant 74px : « Crown » + *« on me »* (italique amber, bloc en dessous).
- Filet noir 1px séparateur.
- Accroche Cormorant 31px : « Visualise ton style avant de réserver. » + *« Fini les mauvaises surprises. »* (italique amber).
- Note centrée taupe au-dessus du CTA : « Pas de compte · Accès immédiat ».
- **CTA amber : « Commencer »** → écran 2.

### Écran 2 — Upload
- **But :** récupérer la photo de l'utilisatrice.
- Eyebrow : « Étape 01 · Ta photo ». Titre Cormorant 42px : « Ta *photo* ».
- **Dropzone** (flex:1) : `border:1px dashed #0d0d0b`, fond rayé ivoire, centrée : carré 46px avec « + », puis label mono « GLISSE TA PHOTO / OU CHOISIS CI-DESSOUS ».
- Bouton **outline** : « Choisir depuis ma galerie » (déclenche `<input type="file" accept="image/*" capture>` caché).
- Message rassurant taupe avec tiret amber : « Ta photo est traitée localement — non stockée. »
- **CTA amber : « Choisir mon style »** → écran 3. (Désactivé tant qu'aucune photo n'est chargée ; afficher la prévisualisation dans la dropzone une fois l'image sélectionnée.)

### Écran 3 — Choix du style (portfolio Nadège)
- **But :** choisir une coiffure réelle réalisée par Nadège.
- Eyebrow « Le portfolio de Nadège », titre Cormorant 34px « Les réalisations de *Nadège* ».
- **Grille 2×2** de 4 cartes blanches (`border:1px solid #0d0d0b`) : zone image 104px + nom du style (13px) + « Réalisé par Nadège » (10px taupe).
- **Carte sélectionnée :** `border:2px solid #c9854a` + badge amber « Sélectionné » (coin haut-gauche, 9px uppercase). Une seule sélection active.
- 4 styles : **Box Braids longues · Twist out naturel · Locks mi-longues · Tresses collées**.
- Message clé (discret, taupe, séparé par filet kraft) : « Ces coiffures ont été réalisées par Nadège. En simulant son style, tu sais exactement ce qu'elle peut faire pour toi. »
- **CTA amber : « Simuler ce style sur moi »** → écran 4 + lance l'appel Fal.ai.

### Écran 4 — Chargement (MODE NUIT)
- **But :** patienter pendant la génération IA (10–20 s).
- **Fond `#0d0d0b`, texte `#f2ece0`.** Séparateurs `#3a352c`, eyebrows en kraft.
- Spinner : cercle 56px `border:1px solid #c9854a` avec `border-top-color:transparent`, rotation 1.1s linéaire infinie.
- Eyebrow kraft « Simulation en cours ». Titre Cormorant 33px : « Crown *on me* travaille sur ta coiffure… ».
- Barre de progression : track 1px `#3a352c`, remplissage amber animé (largeur 4%→96%, 2.4s ease-in-out alternate).
- Légende mono pulsée : « DURÉE ESTIMÉE · 10–20 SECONDES ».
- Bas : « Style réalisé par Nadège K. / Studio Crown — Paris 18e ».
- **Aucun CTA** (transition automatique vers écran 5 dès que Fal.ai répond).

### Écran 5 — Résultat avant/après
- **But :** montrer le résultat et pousser à la réservation.
- Eyebrow « Ta simulation », titre Cormorant 34px « Avant */* Après » (le `/` en amber). Lien discret « Partager » (souligné, taupe) en haut à droite → `navigator.share` ou copie image.
- **Grille 2 colonnes** : photo AVANT (`border:1px solid #0d0d0b`, label mono taupe) | photo APRÈS (`border:2px solid #c9854a`, label mono amber).
- Mention (filet kraft au-dessus) : « Style **Box Braids longues** réalisé par Nadège K. / Studio Crown — Paris 18e ».
- Deux boutons outline côte à côte : « Autre style » (→ écran 3) · « Voir son profil » (→ écran 6).
- **CTA amber : « Prendre RDV avec Nadège »** → `tel:` ou `mailto:`.

### Écran 6 — Profil coiffeuse
- **But :** rassurer et convertir.
- En-tête : avatar carré 88px (`border:1px solid #0d0d0b`) + eyebrow « La coiffeuse » + nom Cormorant 34px « Nadège *K.* » + « Studio Crown — Paris 18e ».
- Ligne note (filets kraft haut/bas) : ★ amber + « 4,9 » + « · 127 avis ».
- Eyebrow « Spécialités » + chips outline : **Box Braids · Twists · Locks · Twist out naturel**.
- Bloc 2 colonnes encadré kraft : **Tarif** « À partir de 80€ » (Cormorant 22px) | **Disponibilité** « Lun–Sam / 9h–19h ».
- `@studio.crown` (mono, taupe).
- Bouton outline « Voir son portfolio » → écran 7.
- **CTA amber : « Prendre RDV »** → `tel:`/`mailto:`.

### Écran 7 — Portfolio coiffeuse
- **But :** parcourir toutes les réalisations, relancer une simulation.
- Eyebrow « Studio Crown — Paris 18e », titre Cormorant 34px « Le portfolio de *Nadège* ».
- **Grille 2×2** : 4 photos 150px (`border:1px solid #0d0d0b`, cliquables → relance écran 4) + nom du style en dessous (12.5px).
- Badge (encadré kraft) avec ★ amber : « Les styles que tu vois viennent de vraies coiffeuses. »
- **CTA amber : « Prendre rendez-vous avec Nadège »**.

---

## 7. Interactions, états & navigation

Flux : **1 Accueil → 2 Upload → 3 Choix → 4 Chargement → 5 Résultat**. Depuis 5 : « Autre style » → 3 ; « Voir son profil » → 6 ; 6 « Voir son portfolio » → 7 ; 7 (clic style) → 4 (relance).

- **Navigation** : SPA mono-fichier, écrans en `<section>` masqués/affichés (pas de rechargement). Garder l'état (photo, style choisi) en mémoire JS.
- **Upload** : `<input type="file">` caché ; lecture en base64 (`FileReader`) ; prévisualisation immédiate dans la dropzone ; le CTA « Choisir mon style » reste désactivé tant qu'aucune photo.
- **Sélection de style** : highlight amber 2px + badge ; une seule active.
- **Chargement** : spinner + barre animés (CSS keyframes) pendant l'appel Fal.ai ; transition auto vers Résultat à la réponse.
- **États d'erreur** : pas de clé API → message clair « Entre ta clé API Fal.ai pour continuer » ; échec réseau → message lisible + bouton réessayer.
- **Animations** : `comSpin` (rotation 1.1s linear infinite), remplissage barre (2.4s ease-in-out alternate), pulse légende (1.8s ease-in-out). Aucune autre fioriture.

---

## 8. Intégration Fal.ai

- Modèle **`fal-ai/flux/dev`** (image-to-image). Pas de backend — tout côté client.
- **Clé API** saisie par l'utilisatrice dans un champ settings discret (bas de page), stockée en **`sessionStorage`** (jamais `localStorage`). Si absente → message « Entre ta clé API Fal.ai pour continuer ».
- Appel `fetch()` avec la photo en **base64** + prompt.
- Prompt type : `"afro hairstyle, textured hair, natural hair, [style choisi], photorealistic, same person, same face, no background change"`.

---

## 9. Données fictives (hardcodées — 1 seule coiffeuse)

```javascript
const coiffeuse = {
  nom: "Nadège K.",
  salon: "Studio Crown — Paris 18e",
  specialites: ["Box Braids", "Twists", "Locks", "Twist out naturel"],
  note: 4.9, avis: 127,
  tarif: "À partir de 80€",
  disponibilite: "Lun–Sam · 9h–19h",
  photo: "https://i.pravatar.cc/150?img=47",
  instagram: "@studio.crown",
  telephone: "06 XX XX XX XX",
  styles: [
    { nom: "Box Braids longues",  img: "https://images.unsplash.com/photo-1594744803329-e58b31de8bf?w=400" },
    { nom: "Twist out naturel",   img: "https://images.unsplash.com/photo-1520813792240-56fc4a3765a7?w=400" },
    { nom: "Locks mi-longues",    img: "https://images.unsplash.com/photo-1580618864182-fce0a62a2037?w=400" },
    { nom: "Tresses collées",     img: "https://images.unsplash.com/photo-1522337360788-8b13dee7a37e?w=400" }
  ]
}
```
Les placeholders rayés des maquettes correspondent à ces images : remplace-les par `coiffeuse.styles[i].img` et l'avatar par `coiffeuse.photo`.

---

## 10. Ce que le MVP NE fait PAS
Pas de compte · pas de paiement · pas de base de données · pas de vrai annuaire (1 coiffeuse hardcodée) · pas de routine capillaire · pas de messagerie · pas de filtres de style.

---

## 11. Fichiers de référence dans ce bundle
- `maquettes/Crown On Me — 7 écrans.dc.html` — les 7 écrans (ouvrir dans un navigateur pour voir le rendu).
- `crown-on-me-identite.html` — charte visuelle.
- `crown-on-me-context-brief.md` — fiche contexte produit complète.

> Livrable final attendu : **un seul `index.html`** mobile-first (390px), commenté par section, fidèle aux maquettes et à la charte, fonctionnel avec une clé Fal.ai.
