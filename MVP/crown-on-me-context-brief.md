# 👑 Crown On Me — Fiche Contexte MVP
> À donner à Claude Code CLI et Claude Design au démarrage de chaque session.

---

## C'est quoi ?

Crown On Me est une app mobile/web qui permet aux femmes aux **cheveux texturés, et particulièrement afro**, de simuler une coiffure sur leur propre photo avant de réserver chez un salon. L'IA génère un avant/après réaliste en quelques secondes.

**Modèle B2B2C** : les coiffeuses alimentent la plateforme avec leurs vraies réalisations. L'app leur ramène des clientes en confiance.

---

## Le problème résolu

Les femmes aux cheveux texturés n'ont aucun outil pour visualiser leur coiffure avant de se décider. Résultat : coiffures ratées à 150€+, journées perdues. Aucune app de simulation n'existe pour les cheveux afro.

---

## Personas MVP (deux utilisateurs à servir)

### 👩🏾 Amara — L'utilisatrice (B2C)
- 24 ans, cheveux texturés, curieuse mais peu sûre d'elle
- Découvre l'app via TikTok ou une amie
- Veut visualiser un style **avant** de prendre RDV
- Freemium : 5 simulations gratuites sans créer de compte
- Parcours clé : arrive → uploade sa photo → choisit un style du portfolio Nadège → voit le résultat → prend RDV

### ✂️ Nadège — La coiffeuse (B2B, hardcodée dans le MVP)
- Coiffeuse afro professionnelle, Paris 18e
- Alimente la plateforme avec ses vraies réalisations
- Gagne des clientes qualifiées qui arrivent avec une simulation validée
- Dans le MVP : profil et portfolio 100% hardcodés (pas de back-end)

---

## Données fictives — Profil Nadège (hardcodé)

```javascript
const coiffeuse = {
  nom: "Nadège K.",
  salon: "Studio Crown — Paris 18e",
  specialites: ["Box Braids", "Twists", "Locks", "Twist out naturel"],
  note: 4.9,
  avis: 127,
  tarif: "À partir de 80€",
  disponibilite: "Lun–Sam · 9h–19h",
  photo: "https://i.pravatar.cc/150?img=47",
  portfolio: [
    "https://images.unsplash.com/photo-1594744803329-e58b31de8bf?w=400",
    "https://images.unsplash.com/photo-1520813792240-56fc4a3765a7?w=400",
    "https://images.unsplash.com/photo-1580618864182-fce0a62a2037?w=400",
    "https://images.unsplash.com/photo-1522337360788-8b13dee7a37e?w=400"
  ],
  styles: ["Box Braids longues", "Twist out naturel", "Locks mi-longues", "Tresses collées"],
  instagram: "@studio.crown",
  telephone: "06 XX XX XX XX"
}
```

---

## Parcours utilisateur MVP — 7 écrans

Le parcours est basé sur le vrai parcours produit Crown On Me, simplifié pour un MVP de démonstration.

### Écran 1 — Accueil
- Logo Crown On Me + tagline : *"Visualise ton style avant de réserver. Fini les mauvaises surprises."*
- Bouton principal : **"Commencer"**
- Pas de compte, pas de login — accès immédiat

### Écran 2 — Upload
- Titre : *"Ta photo"*
- Zone drag & drop + bouton "Choisir depuis ma galerie"
- Prévisualisation de la photo uploadée
- Message rassurant : *"Ta photo est traitée localement — non stockée"*
- Bouton : **"Choisir mon style"**

### Écran 3 — Choix du style (portfolio Nadège)
- Titre : *"Les réalisations de Nadège"*
- 4 cartes : vraie photo de chaque coiffure + nom du style en dessous
- Message clé (discret) : *"Ces coiffures ont été réalisées par Nadège. En simulant son style, tu sais exactement ce qu'elle peut faire pour toi."*
- Sélection avec highlight visuel
- Bouton : **"Simuler ce style sur moi"**

### Écran 4 — Chargement
- Animation Crown On Me
- Message : *"Crown On Me travaille sur ta coiffure…"*
- Durée estimée : 10–20 secondes

### Écran 5 — Résultat
- Affichage avant / après côte à côte
- Mention : *"Style réalisé par Nadège K. · Studio Crown Paris 18e"*
- 3 boutons :
  - **"Prendre RDV avec Nadège"** → lien tel: ou mailto:
  - **"Essayer un autre style"** → retour Écran 3
  - **"Voir son profil complet"** → Écran 6
- Bouton discret : **"Partager le résultat"** (partage natif ou copie image)

### Écran 6 — Profil coiffeuse
- Carte Nadège : photo avatar, nom, salon, note (4.9 ⭐), spécialités, tarif, disponibilité
- Bouton : **"Voir son portfolio"** → Écran 7
- Bouton : **"Prendre RDV"**

### Écran 7 — Portfolio coiffeuse
- Grille 2×2 des 4 photos de réalisations
- Chaque carte : style cliquable → relance une simulation (retour Écran 4)
- Badge : *"Les styles que tu vois viennent de vraies coiffeuses"*
- Bouton : **"Prendre rendez-vous avec Nadège"**

---

## Stack technique MVP

- **Frontend** : HTML / CSS / JavaScript vanilla — un seul fichier `index.html`
- **API IA** : Fal.ai — modèle `fal-ai/flux/dev` (image-to-image)
- **Pas de backend** : tout côté client
- **Pas de build tool** : ouvrable directement dans Chrome

### Intégration Fal.ai
- Clé API saisie par l'utilisatrice dans un champ settings discret (bas de page)
- Stockage en `sessionStorage` (jamais `localStorage`)
- Si pas de clé → message clair : *"Entre ta clé API Fal.ai pour continuer"*
- Appel en `fetch()` avec la photo en base64 + prompt de style
- Prompt type : `"afro hairstyle, textured hair, natural hair, [style choisi], photorealistic, same person, same face, no background change"`

---

## Identité visuelle (à respecter strictement)

### Couleurs — 3 couleurs uniquement
| Nom | Hex | Usage |
|---|---|---|
| Noir encre | `#0d0d0b` | Texte · Fond nuit · Trait |
| Amber | `#c9854a` | Accent · CTA · Italique |
| Ivoire | `#f2ece0` | Fond principal · Mode jour |

Auxiliaires autorisés (sparingly) :
- Kraft `#d4b896` — séparateurs, sous-textes
- Taupe `#8b7355` — textes secondaires

### Typographie
- **Cormorant Garamond** (Google Fonts) — display, titres, accroches — weight 300
- **DM Sans** (Google Fonts) — tout le reste : UI, body, labels, boutons — weight 300

### Règles UI
- Mobile-first (priorité téléphone, viewport ~390px)
- Boutons : min 52px de hauteur, largeur pleine, fond noir ou amber
- Texte minimum : 16px body, 14px secondaire
- Pas de dégradés — aplats francs uniquement
- 1 seul CTA amber par écran
- L'italique amber = signature visuelle, avec parcimonie
- Cormorant uniquement pour titres et accroches — DM Sans pour tout le reste

### Wordmark
```
Crown          ← Cormorant Garamond 300
on me          ← Cormorant Garamond 300 italic, couleur amber
```

---

## Ce que le MVP NE fait PAS

- ❌ Pas de compte utilisateur
- ❌ Pas de paiement
- ❌ Pas de base de données
- ❌ Pas de vrai annuaire (1 seule coiffeuse hardcodée)
- ❌ Pas de routine capillaire
- ❌ Pas de messagerie
- ❌ Pas de filtres de style (Phase 2)

---

## Livrable attendu de Claude Code CLI

- Un seul fichier `index.html` contenant HTML + CSS + JS
- Ouvrable dans Chrome sans serveur
- Commentaires dans le code pour chaque section
- Mobile-first (tester à 390px de large)
- Fonctionnel avec une clé API Fal.ai valide

---

## Livrable attendu de Claude Design

- Maquettes des 7 écrans en respectant l'identité visuelle ci-dessus
- Format mobile (390×844px)
- Palette stricte : Noir `#0d0d0b` · Amber `#c9854a` · Ivoire `#f2ece0`
- Typographie : Cormorant Garamond (titres) + DM Sans (UI)
- Grille éditoriale, pas un style "app gadget"

---

## Contexte projet

- Fondatrice : Amy
- Programme : La Ligue by Startup Banlieue
- Stack complet prévu : FlutterFlow, Firebase, Fal.ai, Make.com
- Phase actuelle : MVP de validation avant développement complet
- Objectif : démontrer la simulation lors d'un pitch

---

## Message clé à afficher dans l'app (Écran 3)

> *"Ces coiffures ont été réalisées par Nadège. En simulant son style, tu sais exactement ce qu'elle peut faire pour toi."*

C'est le cœur du modèle Crown On Me — les coiffeuses alimentent l'app, l'app leur ramène des clientes en confiance.
