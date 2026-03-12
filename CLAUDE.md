# CLAUDE.md — Harley Tours Martinique
## Brief complet pour la construction du site harley-tours-martinique.com

---

## 0. CONTEXTE & MÉTHODE DE TRAVAIL

Tu construis le site **harley-tours-martinique.com**, un site de location de motos premium en Martinique (Harley-Davidson & Ducati). Le projet Astro est déjà initialisé.

### Références visuelles
Le dossier `/mockups/` contient les captures des maquettes validées :
- `mockup_accueil.png` — page d'accueil complète
- `mockup_nos_motos.png` — page Nos Motos complète

**Avant de considérer chaque page terminée**, tu dois :
1. Lancer le serveur de développement
2. Prendre un screenshot de la page rendue
3. Comparer visuellement avec le mockup correspondant
4. Corriger les écarts avant de passer à la suite

### Stack technique
- **Framework** : Astro (projet déjà créé)
- **Animations** : GSAP (via CDN dans le head, ou npm install gsap)
- **Formulaires** : Web3Forms (compte déjà créé — clé API à fournir par le client)
- **Déploiement** : Vercel + domaine harley-tours-martinique.com
- **Fonts** : Google Fonts — Bebas Neue (titres) + DM Sans (corps)

### Ce que le site NE fait PAS
- Aucun paiement en ligne
- Aucun système de réservation automatique
- Les formulaires servent uniquement à être recontacté

---

## 1. STRUCTURE DE FICHIERS

```
harley-tours-martinique/
│
├── public/
│   ├── images/
│   │   ├── logo-htm.png
│   │   ├── Ducati_logo.png
│   │   ├── logo_harley_davidson.png
│   │   ├── street-bob.png
│   │   ├── Ducati-Scrambler-110-2022-Tribute-Pro.jpg
│   │   ├── tech-ducati-scrambler-urban-motard-2022_hd.jpg
│   │   ├── ducati_scrambler_tribute_pro.jpg
│   │   └── ducati_scrambler_tribute_hero_section.png
│   └── favicon.ico
│
├── src/
│   ├── layouts/
│   │   └── Layout.astro
│   ├── components/
│   │   ├── Nav.astro
│   │   ├── Footer.astro
│   │   ├── HeroSlider.astro
│   │   ├── MotoCard.astro
│   │   ├── MotoSection.astro
│   │   ├── ReservationModal.astro
│   │   └── ContactForm.astro
│   ├── pages/
│   │   ├── index.astro
│   │   ├── nos-motos.astro
│   │   └── contact.astro
│   └── styles/
│       └── global.css
│
├── mockups/
│   ├── mockup_accueil.png
│   └── mockup_nos_motos.png
│
├── CLAUDE.md
├── astro.config.mjs
└── package.json
```

---

## 2. IDENTITÉ VISUELLE

### Palette de couleurs
```css
:root {
  --red: #E00714;        /* accent principal — Ducati rouge */
  --orange: #FF6600;     /* accent secondaire — Harley orange */
  --black: #0D0D0D;      /* noir profond */
  --white: #FFFFFF;
  --grey: #F5F5F3;       /* fond sections alternées */
  --grey2: #E8E8E5;      /* bordures, séparateurs */
  --muted: #888888;      /* textes secondaires */
}
```

### Typographie
```css
/* Titres — tous en majuscules */
font-family: 'Bebas Neue', sans-serif;

/* Corps, labels, boutons */
font-family: 'DM Sans', sans-serif;
```

Intégration dans le `<head>` du Layout :
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
```

### Boutons
Tous les boutons principaux utilisent ce clip-path angulaire (jamais de border-radius) :
```css
clip-path: polygon(0 0, calc(100% - 8px) 0, 100% 8px, 100% 100%, 8px 100%, 0 calc(100% - 8px));
```

- **Bouton primaire** : fond `--red`, texte blanc, hover fond `#c00612`
- **Bouton outline** : fond transparent, bordure `--black`, hover fond `--black` texte blanc
- **Bouton nav CTA** : fond `--black`, texte blanc, hover fond `--red`

### Iconographie
Uniquement des icônes SVG custom inline (stroke, pas fill). Aucun emoji. Aucune librairie d'icônes externe.

### Logo
Le logo `logo-htm.png` est un logo typographique horizontal fond noir. Dans la nav, l'afficher avec `height: 36px` et `width: auto`. Ne pas l'encadrer dans un conteneur coloré — il s'intègre directement sur le fond blanc de la nav.

---

## 3. COMPOSANT : Layout.astro

Wrap global appliqué à toutes les pages. Contient :
- `<head>` avec meta SEO (title, description, canonical, og:image)
- Import Google Fonts
- Import `global.css`
- Import GSAP via CDN : `https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js`
- Import ScrollTrigger : `https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js`
- `<Nav />` en position fixed
- `<slot />` pour le contenu de chaque page
- `<Footer />`

Props acceptées : `title`, `description`, `canonical`

---

## 4. COMPOSANT : Nav.astro

**Design** : fond blanc semi-transparent (`rgba(255,255,255,0.92)`), `backdrop-filter: blur(12px)`, hauteur 72px, `position: fixed`, `z-index: 100`, bordure bottom `1px solid var(--grey2)`.

**Contenu** :
- Gauche : logo `logo-htm.png` — height 36px, lien vers `/`
- Droite : liens `Accueil` `/` — `Nos Motos` `/nos-motos` — `Contact` `/contact` — Bouton CTA noir `RÉSERVER` qui ouvre la modale de réservation
- Le lien actif prend la couleur `--red`

**Comportement scroll** : Quand on scrolle vers le bas, la nav reste visible. Ajouter une classe `.scrolled` via JS qui renforce légèrement l'opacité du fond.

---

## 5. COMPOSANT : Footer.astro

Fond `--black`, padding `48px 8%`, flex entre 3 blocs :
- Gauche : nom du site en Bebas Neue blanc + "Location Moto Premium" en petit gris dessous
- Centre : liens Accueil / Nos Motos / Contact / Mentions Légales
- Droite : `contact@harley-tours-martinique.com` en `--red`, cliquable mailto

---

## 6. PAGE : Accueil (index.astro)

### 6.1 Section HERO

**Layout** : grille 2 colonnes (50/50), hauteur `100vh`, fond blanc.

**Côté gauche** :
- Titre H1 en Bebas Neue, très grand (`clamp(4rem, 7vw, 8rem)`), lignes :
  - Ligne 1 & 2 : "LOUEZ DES MOTOS" en noir
  - Ligne 3 : "D'EXCEPTION" en `--red`
- Sous-titre court : "Harley-Davidson ou Ducati — Machines d'exception pour explorer la Martinique. À la journée, sur réservation."
- Un seul bouton : "RÉSERVER UNE MOTO" en rouge (ouvre la modale)
- En bas du hero gauche : bande de 3 stats séparées d'un filet gris :
  - "3" + "Motos disponibles"
  - "2" + "Marques premium"
  - "1J" + "Minimum de location"

**Côté droit** : zone du diaporama (voir section 6.2 ci-dessous)

**SVG background** : motif topographique en ellipses concentriques, centré sur la droite, couleur `--grey2`, opacity 0.8. Un triangle rouge pâle en coin supérieur droit (opacity 0.06). Grille de petits points en bas gauche.

---

### 6.2 DIAPORAMA HERO — Spécifications GSAP détaillées

Le diaporama occupe tout le côté droit du hero. Il est cliquable (click = moto suivante). Curseur `pointer` sur toute la zone.

#### Données des 3 slides

```javascript
const slides = [
  {
    id: 'street-bob',
    image: '/images/street-bob.png',
    badge: { text: 'HARLEY-DAVIDSON', sub: 'Street Bob 114' },
    badgePosition: { top: '20px', right: '20px', bottom: 'auto', left: 'auto' }
  },
  {
    id: 'tribute-pro',
    image: '/images/Ducati-Scrambler-110-2022-Tribute-Pro.jpg',
    badge: { text: 'DUCATI', sub: 'Scrambler 1100' },
    badgePosition: { top: 'auto', right: 'auto', bottom: '80px', left: '40px' }
  },
  {
    id: 'urban-motard',
    image: '/images/tech-ducati-scrambler-urban-motard-2022_hd.jpg',
    badge: { text: 'DUCATI', sub: 'Urban Motard 800' },
    badgePosition: { top: '50%', right: '20px', bottom: 'auto', left: 'auto' }
  }
]
```

#### Séquence d'animation au chargement (une seule fois)

```javascript
// Attendre 1 seconde après le chargement complet de la page
// Puis lancer la rafale : défilement horizontal rapide des 3 motos
// Chaque moto visible ~250ms, effet slide-in depuis la droite puis disparaît vers la gauche
// Après la rafale, atterrissage sur le slide 0 (Street Bob) avec transition douce

window.addEventListener('load', () => {
  setTimeout(() => {
    // Rafale rapide : gsap.timeline
    // tl.to(slide[0], { x: 0, duration: 0.25, ease: "power2.out" })
    // tl.to(slide[0], { x: '-100%', duration: 0.2, ease: "power2.in" }, "+=0.05")
    // tl.to(slide[1], { x: 0, duration: 0.25, ease: "power2.out" })
    // tl.to(slide[1], { x: '-100%', duration: 0.2, ease: "power2.in" }, "+=0.05")
    // tl.to(slide[2], { x: 0, duration: 0.25, ease: "power2.out" })
    // tl.to(slide[2], { x: '-100%', duration: 0.2, ease: "power2.in" }, "+=0.05")
    // Puis afficher slide 0 proprement et démarrer l'autoplay
  }, 1000)
})
```

#### Autoplay

- Chaque slide est visible **5 secondes**
- Transition : la moto suivante arrive par la droite (`x: '100%'` → `x: 0`) pendant que l'actuelle sort par la gauche (`x: 0` → `x: '-100%'`)
- Duration de la transition : `0.7s`, ease `power3.inOut`
- La **carte badge noire** (nom de la moto) change de position à chaque slide (voir coordonnées dans `slides[]` ci-dessus). Animation de la carte : fade out + petit déplacement, puis fade in à la nouvelle position.

#### Interaction hover

Quand la souris entre dans la zone hero droite :
- La moto active passe à `scale(1.02)` avec `duration: 0.4s`, ease `power2.out`
- Quand la souris sort : retour à `scale(1)`

#### Interaction click

Click n'importe où sur la zone → moto suivante (même logique que l'autoplay, mais immédiat). Réinitialise le timer de 5 secondes.

#### Indicateurs de slide

3 petits traits horizontaux en bas de la zone droite. Le trait actif est noir, les autres sont `--grey2`. Transition de couleur animée.

---

### 6.3 Section BANDE MARQUES

Fond `--grey`, hauteur 80px, padding `0 8%`, flex horizontal.
- Label gauche : "NOS MARQUES" en très petit caps gris
- Logos : `Ducati_logo.png` et `logo_harley_davidson.png`, hauteur 32px, en grayscale par défaut, couleur et opacity 1 au hover, transition 0.3s.

---

### 6.4 Section NOS MACHINES

Fond blanc, padding `120px 8%`.

**En-tête** : flex space-between
- Gauche : eyebrow "NOTRE FLOTTE" rouge + titre H2 "NOS MACHINES" en Bebas Neue
- Droite : lien "Voir toutes les motos →" avec underline, lien vers `/nos-motos`

**Grille** : 3 colonnes égales, gap 24px. Composant `<MotoCard />` pour chaque moto.

#### MotoCard.astro — Props
```typescript
{
  badge: string           // "Cruiser" | "Scrambler" | "Urban"
  badgeColor: string      // "orange" | "red" | "black"
  brand: string           // "Harley-Davidson" | "Ducati"
  brandColor: string      // couleur du dot
  name: string
  tagline: string
  image: string
  reserveLabel: string    // nom de la moto à pré-remplir dans la modale
}
```

#### Données des 3 cards

**Card 1 — Street Bob 114**
- Badge : "CRUISER" orange
- Marque : Harley-Davidson (dot orange)
- Nom : Street Bob 114
- Tagline : "Le son, le chrome, la route. Une légende américaine sous le soleil des Antilles."
- Image : `/images/street-bob.png`

**Card 2 — Scrambler 1100 Tribute Pro**
- Badge : "SCRAMBLER" rouge
- Marque : Ducati (dot rouge)
- Nom : Scrambler 1100 Tribute Pro
- Tagline : "L'âme vintage, la puissance moderne. Pour ceux qui ne font pas les choses à moitié."
- Image : `/images/Ducati-Scrambler-110-2022-Tribute-Pro.jpg`

**Card 3 — Urban Motard 800**
- Badge : "URBAN" noir
- Marque : Ducati (dot rouge)
- Nom : Scrambler Urban Motard 800
- Tagline : "Agile, punchy, sans compromis. Parfaite pour les routes sinueuses de la Martinique."
- Image : `/images/tech-ducati-scrambler-urban-motard-2022_hd.jpg`

#### Design de chaque card
- Fond `--grey`, bordure `1px solid --grey2`
- Zone image : hauteur 220px, fond dégradé léger `#f8f8f6` → `#efefeb`, image en `object-fit: contain`
- Hover card : `translateY(-4px)` + `box-shadow: 0 24px 60px rgba(0,0,0,0.1)`, transition 0.3s
- Hover image : `scale(1.05)`, transition 0.4s
- Body : padding 24px, bordure top
- Prix : "SUR DEMANDE" en Bebas Neue + "par journée" en petit gris dessous
- Bouton "RÉSERVER" noir clip-path → au click, ouvre `ReservationModal` avec la moto pré-sélectionnée

---

### 6.5 Section POURQUOI NOUS

**Layout** : fond `--black`, padding `100px 8%`, grille 2 colonnes.

**Colonne gauche** :
- Eyebrow "POURQUOI NOUS" en `--orange`
- Titre H2 blanc : "UNE EXPÉRIENCE PENSÉE POUR LA MARTINIQUE"
- Texte : "Des motos soigneusement entretenues, un service attentionné, et la Martinique comme terrain de jeu. Rien d'autre."

**Colonne droite** : grille 2x2, gap 2px. 4 items :

1. Icône moto — "Motos haut de gamme" — "Harley-Davidson & Ducati, entretenues avec exigence."
2. Icône calendrier — "Location à la journée" — "Dès une journée. Flexibilité maximale."
3. Icône globe — "Martinique toute l'année" — "Disponible 365 jours, sous le soleil des Antilles."
4. Icône carte — "Réservation simplifiée" — "Formulaire rapide, tarifs sur demande, contact direct."

Chaque item : fond `rgba(255,255,255,0.04)`, bordure `rgba(255,255,255,0.06)`, padding 28px. Icône SVG stroke rouge 36px. Titre en blanc 600. Texte en `rgba(255,255,255,0.45)` 300.

---

### 6.6 Section CTA FINAL

Fond blanc, padding `100px 8%`, centré.

- Watermark "MARTINIQUE" en Bebas Neue `18vw`, couleur `--grey`, centré en absolu derrière le contenu
- Eyebrow "PRÊT À PARTIR ?" rouge
- Titre H2 : "L'AVENTURE COMMENCE ICI, EN MARTINIQUE."
- Sous-titre : "Choisissez votre machine, indiquez vos dates, et nous vous recontactons pour finaliser votre location."
- 2 boutons côte à côte : "RÉSERVER UNE MOTO" rouge (modale) + "NOUS CONTACTER" outline noir (lien `/contact`)

---

### 6.7 Animations GSAP — Page Accueil

Enregistrer ScrollTrigger : `gsap.registerPlugin(ScrollTrigger)`

```javascript
// Titre hero : stagger sur les 3 lignes
gsap.from('.hero-title-line', {
  y: 60, opacity: 0, duration: 0.8,
  stagger: 0.15, ease: 'power3.out', delay: 0.3
})

// Sous-titre et bouton hero
gsap.from('.hero-sub, .hero-btn', {
  y: 30, opacity: 0, duration: 0.6,
  stagger: 0.1, ease: 'power2.out', delay: 0.8
})

// Cards motos au scroll
gsap.from('.moto-card', {
  scrollTrigger: { trigger: '.section-machines', start: 'top 75%' },
  y: 50, opacity: 0, duration: 0.7,
  stagger: 0.15, ease: 'power2.out'
})

// Items "Pourquoi nous" au scroll
gsap.from('.why-item', {
  scrollTrigger: { trigger: '.section-why', start: 'top 70%' },
  y: 30, opacity: 0, duration: 0.5,
  stagger: 0.1, ease: 'power2.out'
})
```

---

## 7. PAGE : Nos Motos (nos-motos.astro)

### 7.1 Hero de page

Fond `--black`, min-height `280px`, padding-top `72px` (hauteur nav). Contenu en bas à gauche.
- Eyebrow rouge "NOTRE FLOTTE"
- H1 : "NOS MOTOS"
- Sous-titre : "Trois machines d'exception. Harley-Davidson et Ducati, disponibles à la location en Martinique."
- Watermark "MOTOS" en très grand, fond, opacity 0.04, aligné à droite

### 7.2 Sections alternées — Composant MotoSection.astro

3 sections pleine largeur, grille 50/50, `min-height: 85vh`.

Alternance :
- Moto 01 : photo gauche / contenu droite — fond photo `--grey`
- Moto 02 : contenu gauche / photo droite — fond photo `--grey`
- Moto 03 : photo gauche / contenu droite — fond photo blanc

Numéro watermark (01, 02, 03) en Bebas Neue `7rem`, opacity 0.05, en haut à gauche de la zone photo.

**Zone photo** : centré, image `width: 85%`, `object-fit: contain`, `drop-shadow(0 20px 60px rgba(0,0,0,0.12))`. Hover : `scale(1.03) translateY(-6px)`, transition 0.6s.

**Zone contenu** : padding `80px 8%`, `display: flex`, `flex-direction: column`, `justify-content: center`. Séparée par `1px solid --grey2`.

**Contenu de chaque section** :
1. Badge marque (dot coloré + nom marque + badge catégorie)
2. Nom moto en Bebas Neue grand
3. Phrase d'accroche
4. Grille specs 2x2
5. Tags conditions
6. Bouton "RÉSERVER CETTE MOTO →" rouge → ouvre `ReservationModal` avec moto pré-sélectionnée

---

### 7.3 Données complètes des 3 motos

#### Moto 01 — Harley-Davidson Street Bob 114
- **Marque** : Harley-Davidson | **Couleur dot** : `--orange` | **Catégorie** : CRUISER (badge orange)
- **Image** : `/images/street-bob.png`
- **Tagline** : "Le bobber américain dans toute sa pureté. Moteur Milwaukee-Eight 114, guidon mini-ape, finitions noires intégrales. Une attitude qui ne laisse personne indifférent sur les routes de Martinique."
- **Specs** :
  - Cylindrée : **1 868 cm³**
  - Puissance : **94 ch**
  - Couple : **161 Nm**
  - Poids : **297 kg**
- **Conditions** : Permis A requis — Caution requise — 1 jour minimum
- **Moteur** : Milwaukee-Eight® 114, bicylindre en V, refroidissement par air

#### Moto 02 — Ducati Scrambler 1100 Tribute Pro
- **Marque** : Ducati | **Couleur dot** : `--red` | **Catégorie** : SCRAMBLER (badge rouge)
- **Image** : `/images/Ducati-Scrambler-110-2022-Tribute-Pro.jpg`
- **Tagline** : "Un hommage vivant aux 50 ans du twin Ducati. Livrée Giallo Ocra iconique, moteur Desmodue 1100, selle marron surpiquée. Pour ceux qui roulent autant avec les yeux qu'avec les mains."
- **Specs** :
  - Cylindrée : **1 079 cm³**
  - Puissance : **86 ch**
  - Couple : **88 Nm**
  - Poids : **211 kg**
- **Conditions** : Permis A requis — Caution requise — 1 jour minimum
- **Moteur** : Bicylindre en L 90°, distribution Desmodromique, refroidissement par air

#### Moto 03 — Ducati Scrambler Urban Motard 800
- **Marque** : Ducati | **Couleur dot** : `--red` | **Catégorie** : URBAN (badge noir)
- **Image** : `/images/tech-ducati-scrambler-urban-motard-2022_hd.jpg`
- **Tagline** : "Le Scrambler réinventé pour la route. Pneus sport 17 pouces, empattement réduit, réactivité maximale. La moto idéale pour découvrir les virages de la Martinique."
- **Specs** :
  - Cylindrée : **803 cm³**
  - Puissance : **73 ch**
  - Couple : **66 Nm**
  - Poids : **196 kg**
- **Conditions** : Permis A ou A2 — Caution requise — 1 jour minimum
- **Moteur** : Bicylindre Desmodue 803 cm³, refroidissement par air

### 7.4 Animations GSAP — Page Nos Motos

```javascript
// Chaque section moto : la photo entre depuis l'extérieur au scroll
// Section impaire : photo arrive de gauche
gsap.from('.moto-section:nth-child(odd) .moto-photo img', {
  scrollTrigger: { trigger: element, start: 'top 70%' },
  x: -60, opacity: 0, duration: 0.9, ease: 'power3.out'
})

// Section paire : photo arrive de droite
gsap.from('.moto-section:nth-child(even) .moto-photo img', {
  scrollTrigger: { trigger: element, start: 'top 70%' },
  x: 60, opacity: 0, duration: 0.9, ease: 'power3.out'
})

// Contenu texte : stagger sur les enfants
gsap.from('.moto-content > *', {
  scrollTrigger: { trigger: element, start: 'top 70%' },
  y: 30, opacity: 0, duration: 0.6,
  stagger: 0.1, ease: 'power2.out'
})
```

---

## 8. PAGE : Contact (contact.astro)

### 8.1 Hero de page

Même style que le hero de Nos Motos — fond `--black`, sobre.
- H1 : "CONTACT"
- Sous-titre : "Une question ? Une demande ? Écrivez-nous, nous vous répondons rapidement."

### 8.2 Layout de la page

Grille 2 colonnes (55% / 45%), padding `100px 8%`, fond blanc.

**Colonne gauche — Formulaire de contact**

Titre : "ENVOYEZ-NOUS UN MESSAGE" en Bebas Neue.

Champs :
- Prénom + Nom (côte à côte, 50/50)
- Email
- Téléphone (optionnel)
- Message (textarea, min-height 140px)
- Bouton submit "ENVOYER" rouge

Intégration Web3Forms :
```html
<form action="https://api.web3forms.com/submit" method="POST">
  <input type="hidden" name="access_key" value="CLE_WEB3FORMS_DU_CLIENT">
  <input type="hidden" name="subject" value="Nouveau message — Harley Tours Martinique">
  <input type="hidden" name="redirect" value="false">
  <!-- champs du formulaire -->
</form>
```

Après soumission réussie : afficher un message de confirmation inline (pas de redirection) — "Votre message a bien été envoyé. Nous vous répondrons rapidement."

**Colonne droite — Informations**

Sobre, texte. Contient :
- Email cliquable : `contact@harley-tours-martinique.com`
- Icône + texte : "Réservation à la journée minimum"
- Icône + texte : "Tarifs communiqués sur demande"
- Icône + texte : "Retrait sur point fixe en Martinique"
- Icône + texte : "Permis A requis (Permis A2 accepté pour l'Urban Motard 800)"

Style des champs : fond `--grey`, bordure `1px solid --grey2`, pas de border-radius. Focus : bordure `--black`. Tous les labels en caps, petit, lettre-espacés.

---

## 9. COMPOSANT : ReservationModal.astro

Modale overlay, s'ouvre au click sur n'importe quel bouton "Réserver" du site.

### Déclencheurs
- Bouton "RÉSERVER UNE MOTO" dans la nav
- Bouton "RÉSERVER UNE MOTO" dans le hero accueil
- Bouton "RÉSERVER" sur les cards MotoCard
- Bouton "RÉSERVER CETTE MOTO" sur chaque section MotoSection
- Bouton "RÉSERVER UNE MOTO" dans la section CTA final

### Design de la modale
- Overlay fond `rgba(0,0,0,0.7)`, click overlay = fermeture
- Fenêtre centrée, fond blanc, max-width `560px`, padding `48px`
- Bouton fermeture X en haut à droite
- Titre : "RÉSERVER UNE MOTO" en Bebas Neue
- Sous-titre : "Laissez vos coordonnées et nous vous recontactons avec les tarifs et disponibilités."

### Champs du formulaire de réservation
1. **Moto souhaitée** — `<select>` avec 3 options :
   - Harley-Davidson Street Bob 114
   - Ducati Scrambler 1100 Tribute Pro
   - Ducati Scrambler Urban Motard 800
   - *Si la modale est ouverte depuis un bouton spécifique, l'option correspondante est pré-sélectionnée*
2. **Date de début** — `<input type="date">`
3. **Date de fin** — `<input type="date">`
4. **Téléphone** — `<input type="tel">` (requis)
5. **Email** — `<input type="email">` (requis)
6. **Message / Commentaire** — `<textarea>` optionnel, court (3 lignes)
7. Bouton submit "ENVOYER MA DEMANDE" rouge

### Intégration Web3Forms — Réservation
```html
<input type="hidden" name="access_key" value="CLE_WEB3FORMS_DU_CLIENT">
<input type="hidden" name="subject" value="Demande de réservation — Harley Tours Martinique">
<input type="hidden" name="redirect" value="false">
```

### Animation d'ouverture/fermeture GSAP
```javascript
// Ouverture
gsap.fromTo(modal, 
  { opacity: 0, y: 30 }, 
  { opacity: 1, y: 0, duration: 0.4, ease: 'power3.out' }
)

// Fermeture
gsap.to(modal, {
  opacity: 0, y: 20, duration: 0.3, ease: 'power2.in',
  onComplete: () => modal.style.display = 'none'
})
```

**La pré-sélection de la moto** se fait via un attribut `data-moto="street-bob-114"` sur chaque bouton déclencheur. Le JS de la modale lit cet attribut et met à jour le `<select>` avant d'ouvrir.

---

## 10. SEO — MARTINIQUE

### Balises meta par page

**Page Accueil**
```
title: "Location Moto Martinique — Harley-Davidson & Ducati | Harley Tours Martinique"
description: "Louez une moto d'exception en Martinique. Harley-Davidson Street Bob 114, Ducati Scrambler 1100 Tribute Pro et Urban Motard 800. Location à la journée, sur réservation."
canonical: "https://harley-tours-martinique.com/"
```

**Page Nos Motos**
```
title: "Nos Motos à Louer en Martinique — Harley-Davidson & Ducati | Harley Tours Martinique"
description: "Découvrez notre flotte : Harley-Davidson Street Bob 114, Ducati Scrambler 1100 Tribute Pro, Ducati Scrambler Urban Motard 800. Location moto premium en Martinique."
canonical: "https://harley-tours-martinique.com/nos-motos"
```

**Page Contact**
```
title: "Contact & Réservation Moto Martinique | Harley Tours Martinique"
description: "Contactez-nous pour réserver votre moto en Martinique. Harley-Davidson et Ducati disponibles à la location à la journée. Tarifs sur demande."
canonical: "https://harley-tours-martinique.com/contact"
```

### Schema.org (dans Layout.astro)
```json
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "Harley Tours Martinique",
  "description": "Location de motos Harley-Davidson et Ducati en Martinique",
  "url": "https://harley-tours-martinique.com",
  "email": "contact@harley-tours-martinique.com",
  "areaServed": {
    "@type": "Place",
    "name": "Martinique"
  },
  "priceRange": "Sur demande",
  "openingHours": "Mo-Su 00:00-23:59"
}
```

### Mots-clés SEO à intégrer naturellement dans les textes
- "location moto Martinique"
- "louer une moto en Martinique"
- "location Harley Davidson Martinique"
- "location Ducati Martinique"
- "location moto Scrambler Martinique"
- "moto premium Martinique"
- "location moto Antilles"

---

## 11. GLOBAL.CSS — Règles de base

```css
*, *::before, *::after {
  margin: 0; padding: 0; box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  font-family: 'DM Sans', sans-serif;
  background: #FFFFFF;
  color: #0D0D0D;
  overflow-x: hidden;
  -webkit-font-smoothing: antialiased;
}

/* Padding top global pour compenser la nav fixe */
main {
  padding-top: 72px;
}

/* Clip-path boutons */
.btn-clip {
  clip-path: polygon(0 0, calc(100% - 8px) 0, 100% 8px, 100% 100%, 8px 100%, 0 calc(100% - 8px));
}

/* Sélection de texte */
::selection {
  background: #E00714;
  color: white;
}

/* Scrollbar personnalisée (webkit) */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: #F5F5F3; }
::-webkit-scrollbar-thumb { background: #0D0D0D; }
```

---

## 12. DÉPLOIEMENT VERCEL

1. Push le projet sur GitHub (repo public ou privé)
2. Connecter le repo sur vercel.com
3. Framework preset : **Astro**
4. Build command : `npm run build`
5. Output directory : `dist`
6. Dans les variables d'environnement Vercel, ajouter :
   - `WEB3FORMS_KEY` = clé API Web3Forms du client
7. Dans les settings du domaine Vercel, ajouter `harley-tours-martinique.com`
8. Chez le registrar du domaine, pointer les DNS vers Vercel :
   - `A record` → `76.76.21.21`
   - `CNAME www` → `cname.vercel-dns.com`

---

## 13. CHECKLIST FINALE AVANT LIVRAISON

Avant de considérer le projet terminé, vérifier chaque point :

- [ ] Diaporama hero fonctionne : rafale initiale après 1s, autoplay 5s, click pour avancer
- [ ] Carte badge change de position selon la moto affichée
- [ ] Hover sur zone hero : légère mise à l'échelle de la moto
- [ ] Modale de réservation s'ouvre depuis tous les boutons "Réserver"
- [ ] Pré-sélection de la moto correcte dans la modale selon le bouton cliqué
- [ ] Formulaire de réservation soumet vers Web3Forms sans redirection
- [ ] Formulaire de contact soumet vers Web3Forms sans redirection
- [ ] Messages de confirmation affichés après soumission
- [ ] Nav active highlighting fonctionne sur les 3 pages
- [ ] Logos marques en grayscale / couleur au hover
- [ ] Toutes les animations GSAP au scroll déclenchées correctement
- [ ] Transition de page : wipe horizontal GSAP entre les pages
- [ ] Aucun emoji dans tout le site
- [ ] Toutes les balises meta renseignées sur les 3 pages
- [ ] Schema.org présent dans le Layout
- [ ] Site responsive vérifié (même si desktop-first)
- [ ] Screenshots comparés aux mockups pour les 3 pages
- [ ] Aucune erreur console en production

---

## 14. NOTES IMPORTANTES

- **Pas d'emojis** — uniquement des icônes SVG inline custom (style stroke, stroke-width 1.5)
- **Pas de border-radius** sur les boutons — clip-path angulaire uniquement
- **Pas de librairie d'icônes** — tout le SVG est écrit à la main
- **Texte de sélection** en rouge (`#E00714`)
- **Le site est desktop-first** — le responsive est à implémenter mais n'est pas la priorité principale
- **Aucun paiement** ne transite par le site
- La **clé Web3Forms** sera fournie par le client — utiliser un placeholder `VOTRE_CLE_WEB3FORMS` dans le code et signaler clairement où la remplacer
- Le **logo logo-htm.png** est un logo typographique horizontal fond noir — l'afficher à `height: 36px` dans la nav sur fond blanc, il sera visible car le texte est orange foncé
