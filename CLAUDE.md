# LIFE.RPG — Contexte projet pour Claude Code

## C'est quoi ce projet ?
Une application web personnelle (HTML/CSS/JS standalone, zéro framework, zéro backend) qui gamifie la vie réelle d'Axel. Hébergée sur GitHub Pages : https://axf26.github.io/life-rpg

Usage personnel et cercle proche — pas besoin d'onboarding.

## Stack
- **Fichiers HTML standalone** : HTML + CSS + JS inline, un fichier par module
- **Pas de build**, pas de npm, pas de framework
- **Stockage** : localStorage du navigateur (clé préfixée par module)
- **Déploiement** : GitHub Pages (upload = déploiement automatique)

## Architecture hub + modules

```
life-rpg/
├── index.html           ← Hub : écran d'accueil avec choix du module
├── argent/
│   └── index.html       ← Module Argent (terminé ✓)
├── vie/
│   └── index.html       ← Module Vie (à construire)
└── CLAUDE.md
```

URLs :
- `axf26.github.io/life-rpg` → hub
- `axf26.github.io/life-rpg/argent` → module argent
- `axf26.github.io/life-rpg/vie` → module vie

---

## Module ARGENT — terminé ✓
Fichier : `argent/index.html`

### Ce qui est implémenté
- Compteur argent en temps réel basé sur les jours ouvrés (lun-ven)
- Double plage horaire : matin (8h30-12h30) + après-midi (14h-18h)
- Pause méridienne automatique (compteur stoppé)
- Weekend = non rémunéré, compteur à 0
- Jours fériés/RTT/congés = rémunérés normalement, badge affiché
- Cagnotte nette = revenus proratisés (jours ouvrés) − charges proratées (jours calendaires ÷30) − épargne proratée + revenus exceptionnels − dépenses manuelles + report mois précédent
- Mini récap calcul visible dans le hero (revenus / charges / épargne / = cagnotte)
- Indicateur "retour à 0 le [date] à [heure]" quand cagnotte négative
- Dépenses manuelles (ex: bière −10€) avec nom libre et date planifiable
- Revenus exceptionnels avec option d'étalement sur plage horaire
- Équivalent en jours ouvrés nets au survol de chaque mouvement
- Modification des mouvements (modal pré-remplie + bouton supprimer)
- Projection fin de mois
- Graphique d'évolution de la cagnotte (canvas 2D, dégradé vert→bleu)
- Historique mensuel avec clôture de mois (→ épargne ou → pouvoir d'achat suivant)
- Suivi épargne long terme accumulée
- Export/Import JSON (backup des données)
- Popup de rappel export tous les 3 jours
- Design : fond bleu-violet profond (#0d0e1a), glassmorphism, dégradés rose→violet→bleu
- Optimisé mobile iPhone (safe areas, touch targets 44px, no zoom iOS, PWA-like)

### Config utilisateur (39h/semaine)
- Salaire net mensuel : à configurer
- Heures/mois : 169h (défaut)
- Plage matin : 08:30-12:30
- Plage ap-midi : 14:00-18:00
- Jours travaillés : lun-ven

### Navigation (4 onglets)
- **live** : dashboard principal avec cagnotte temps réel
- **mouvements** : liste + ajout/modification dépenses/revenus exceptionnels
- **historique** : graphique + récap mois + épargne totale
- **config** : tous les paramètres + export/import

### Clé localStorage
`life_rpg_v4`

---

## Module VIE — à construire
Fichier : `vie/index.html`

### Vision
Visualiser les 24h de la journée comme un RPG. Des jauges qui évoluent selon les choix quotidiens.

### Ce qu'il faut construire
- **Timeline 24h** : blocs visuels pour sommeil / travail / sport / perso / temps libre
- **Jauges RPG** : bien-être, concentration, énergie physique, énergie mentale
- **Logger des activités** : nom, durée, catégorie (recharge vs vide)
- **Impact des activités** : 3h Netflix → baisse concentration ; sport → monte énergie physique
- **Distinction** : activités qui rechargent vs activités qui vident
- **Sommeil** : qualité + durée, impact sur les jauges du lendemain

### Style
Même charte graphique que le module Argent (fond bleu-violet, glassmorphism, dégradés).

---

## Module OBJECTIFS & XP — à construire plus tard
Fichier : `xp/index.html`

### Vision
- Chaque action positive (sport, guitare, espagnol, lecture…) rapporte des XP
- Catégories : Corps / Esprit / Compétences / Social / Finance
- Barre de progression par catégorie
- Système de niveaux
- Streak (jours consécutifs)

---

## Hub — à construire
Fichier : `index.html`

### Vision
Écran d'accueil avec les modules disponibles sous forme de cards. Style RPG, sombre, avec le nom du module, une icône et une description courte. Lien vers chaque module.

---

## Palette de couleurs (commune à tous les modules)
```
--bg: #0d0e1a          fond principal bleu-nuit
--bg2: #12152a         cards
--bg3: #181c35         inputs
--green: #06d6a0       accent positif
--pink: #f72585        accent négatif
--blue: #4361ee        accent info
--purple: #7209b7      accent épargne/XP
--amber: #ffb703       accent warning/planifié
--grad: linear-gradient(135deg, #f72585, #7209b7, #4361ee)
--grad-green: linear-gradient(135deg, #06d6a0, #4361ee)
```

## Conventions de code
- Tout dans un seul fichier HTML par module
- CSS dans `<style>`, JS dans `<script>` en fin de body
- Variables CSS dans `:root` pour le thème
- localStorage pour la persistance (clé préfixée : `life_rpg_argent_`, `life_rpg_vie_`, etc.)
- Fonctions nommées en camelCase
- Pas de dépendances externes sauf Google Fonts (JetBrains Mono)
- Optimisé mobile iPhone en priorité

## Comment déployer
1. Modifier les fichiers dans ce dossier
2. Aller sur github.com/AxF26/life-rpg
3. Uploader les fichiers modifiés → remplacent automatiquement les anciens
4. GitHub Pages redéploie en 1-2 min
5. URL racine : https://axf26.github.io/life-rpg
