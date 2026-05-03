# BRIEF MODULE VIE — LIFE.RPG

## Contexte du projet

LIFE.RPG est une application web personnelle qui gamifie la vie réelle d'Axel.
- **Hébergée sur GitHub Pages** : `axf26.github.io/life-rpg`
- **Stack** : HTML + CSS + JS inline, un seul fichier `vie/index.html`, zéro framework, zéro backend
- **Stockage** : localStorage (préfixer les clés `life_rpg_vie_`)
- **Design** : fond bleu-violet profond (`#0d0e1a`), glassmorphism, dégradés rose→violet→bleu, police JetBrains Mono
- **Optimisé iPhone** : safe areas, touch targets 44px, no zoom iOS, PWA-like
- **Usage personnel** : pas d'onboarding, pas d'auth

Le module Argent existe déjà dans `argent/index.html`. Le module Vie est à créer dans `vie/index.html`.

---

## PHILOSOPHIE DU MODULE

Le module Vie transforme les 24h de la journée en dashboard RPG. L'utilisateur visualise comment il passe son temps et l'impact sur ses stats (jauges). Le système a une **mémoire sur 7 jours** — les jauges ne repartent pas à 100% chaque matin.

Deux sources de données automatiques via **Raccourcis iOS (Apple Shortcuts)** :
- **Sommeil** (Apple Watch) : durée, phases (profond/léger/paradoxal), heure coucher/réveil
- **Sport** (Apple Watch) : type d'exercice, durée, calories, fréquence cardiaque

Le reste est saisi manuellement dans l'app.

---

## STRUCTURE DU TEMPS (24h)

### Temps fixe automatique
- **Sommeil** : importé via raccourci iOS (moyenne ~7h15)
- **Travail** : horaires configurables (en présentiel, lun-ven)

### Temps fixe manuel
- Trajets, cuisine, courses, douche, etc.
- L'utilisateur les ajoute comme des blocs de temps qui réduisent le temps libre

### Temps libre restant
- = 24h − sommeil − travail − temps fixe manuel
- Affiché clairement comme la **ressource principale** à distribuer
- Les objectifs et activités viennent piocher dedans
- Visualisation : quand tu logges "1h de guitare", le temps libre descend et tu vois ce qu'il reste

---

## LES JAUGES RPG (avec mémoire 7 jours)

### Liste des jauges
1. ⚡ **Énergie** — capacité à agir dans la journée
2. 🧠 **Concentration** — capacité de focus
3. 😊 **Bien-être** — moral et satisfaction
4. 🌙 **Sommeil** — qualité de récupération
5. 💪 **Forme physique** — condition physique

### Système de plafond dynamique

Les jauges ne démarrent PAS à 100% chaque matin. Le plafond max dépend de l'historique des 7 derniers jours :

| Situation | Plafond max au réveil |
|---|---|
| 7 jours de bonne hygiène (sommeil 8h+, sport régulier, peu d'écrans) | 100% |
| Rythme correct (5-6 bons jours) | 85-90% |
| Rythme moyen (3-4 bons jours) | 70% |
| Mauvaise semaine | 50-60% |

En plus du plafond, la nuit précédente applique un **malus ou bonus** :
- Nuit ≥ 8h + bon sommeil profond → bonus +10% sur le plafond
- Nuit 7-8h → neutre
- Nuit 6-7h → malus −10%
- Nuit < 6h → malus −20%

**Exemple** : rythme correct (plafond 85%) + nuit de 6h (malus −10%) = jauges à 75% max le matin. Ce n'est pas parce qu'il dort 8h la nuit suivante qu'il revient à 100% — il faut plusieurs jours consécutifs.

---

## IMPACTS DES ACTIVITÉS

### Sommeil (données Apple Watch)
- **≥ 8h + bon sommeil profond** → Toutes jauges regen max, Sommeil = 100%
- **7h-8h** → Regen correcte, Sommeil = 75-85%
- **6h-7h** → Regen faible, Sommeil = 50-65%
- **< 6h** → Regen minimale, Sommeil < 50%, tout est pénalisé

Les phases de sommeil comptent : beaucoup de profond = meilleure récupération.

### Sport (données Apple Watch)
L'utilisateur choisit l'intensité OU elle est déduite des données watch.

**Court terme (même journée)** :
- Énergie : −5 (léger) à −20 (intense)
- Concentration : −10
- Bien-être : +15 à +25

**Long terme (lendemain+)** :
- Forme : +10 à +25
- Énergie de base augmente progressivement avec la régularité

**Jour de récupération** : option à cocher → pas de pénalité pour ne pas avoir fait de sport ce jour-là, et bonus récupération musculaire.

### Jeux / Écrans

Les impacts varient selon l'heure et le jour :
- **Seuil de nuit** : 20h en semaine (lun-jeu), 21h les ven/sam/dim

| Période | Durée | Impact |
|---|---|---|
| Avant seuil | < 1h | Neutre |
| Avant seuil | 1h-2h | Énergie −10, Concentration −10 |
| Avant seuil | 2h-4h | Énergie −20, Concentration −25 |
| Avant seuil | 4h+ | Tout dans le rouge |
| Après seuil | < 1h | Sommeil −10 |
| Après seuil | 1h-2h | Sommeil −25, Énergie −15 |
| Après seuil | 2h+ | Sommeil −40, Concentration −20, Bien-être −15 |

### Méditation (catégorie à part)
- Bonus immédiat : Concentration +15, Bien-être +15, Énergie +5
- Bonus cumulatif si régularité sur la semaine

### Actions positives (lecture, apprentissage, etc.)
- Concentration +5 à +10 (selon durée)
- Bien-être +10
- Si 2+ actions positives dans la journée → bonus "journée productive" visible

### Vie sociale
- Bien-être +20 à +30
- Énergie −5 à −10

### Repas
- Qualité bonne → Énergie +15, Bien-être +10
- Qualité mauvaise (fast food etc.) → Énergie +5, Bien-être −5

---

## COMPÉTENCES ET OBJECTIFS (SYSTÈME RPG)

### Principe
L'utilisateur crée ses propres objectifs/compétences (ex: Guitare, Méditation, Sport, Révisions DSCG, etc.). Il les configure lui-même.

### Progression
- Chaque session loggée = XP gagnée (basée sur la durée + régularité)
- **Niveau de maîtrise** qui progresse : Débutant → Novice → Intermédiaire → Avancé → Expert → Maître
- La régularité compte plus que les sessions longues isolées (3 × 30 min > 1 × 2h)
- Afficher le streak (jours consécutifs) et le temps total cumulé

### Visualisation temps
- Quand une session d'objectif est loggée, le temps libre diminue
- L'utilisateur voit immédiatement : "si je fais 1h de guitare + 30min de méditation + 1h de sport, il me reste Xh de libre"
- Ça montre concrètement que 4h de jeu PC = 4h de moins pour progresser

### Pas d'objectifs hebdomadaires pour l'instant
Pas de système de quêtes ou d'objectifs chiffrés hebdomadaires (sera ajouté plus tard si souhaité).

---

## TIMELINE 24H (VISUALISATION)

### Barre horizontale
- Représente les 24h de la journée
- Chaque bloc d'activité est coloré selon sa catégorie
- Indicateur "maintenant" qui se déplace en temps réel
- Les blocs vides (non loggés) sont visibles en pointillés

### Couleurs des catégories
- 😴 Sommeil → violet sombre (#3d2d6e)
- 💼 Travail → bleu foncé (#1e3a5f)
- 💪 Sport → vert foncé (#14532d)
- 🍽 Repas → ambre sombre (#3d2000)
- 🎮 Jeux/écran → rouge sombre (#3d1515)
- 👥 Social → violet/rose (#2d1a3d)
- 🧘 Méditation → teal (#0f3d3d)
- 📚 Actions positives → vert émeraude (#1a3d2d)
- 🚗 Temps fixe (trajets etc.) → gris (#2a2d3a)
- ⬜ Non logué → pointillés (#1a1c2e)

---

## NAVIGATION (ONGLETS)

### 1. Aujourd'hui (onglet principal)
- Jauges RPG en haut
- Timeline 24h
- Temps libre restant affiché
- Boutons pour logger une activité
- Derniers impacts enregistrés

### 2. Compétences
- Liste des objectifs/compétences créés
- Niveau + XP + streak pour chacun
- Temps total cumulé
- Bouton pour ajouter un nouvel objectif

### 3. Historique
- Calendrier des journées passées
- Score global par jour (code couleur)
- Tendances sur la semaine / le mois

### 4. Config
- Horaires de travail
- Seuils d'écran (20h semaine / 21h week-end déjà configuré)
- Export / Import JSON (backup)
- Gestion des objectifs/compétences

---

## IMPORT DONNÉES APPLE WATCH (VIA RACCOURCIS iOS)

### Mécanisme
1. L'utilisateur crée un raccourci iOS qui lit les données Apple Santé
2. Le raccourci génère une URL avec les données encodées : `axf26.github.io/life-rpg/vie/?import=DONNÉES_BASE64`
3. L'app détecte le paramètre `import` dans l'URL et importe les données dans localStorage
4. Alternatively, le raccourci peut copier les données dans le presse-papier et l'app a un bouton "Importer depuis presse-papier"

### Données attendues (format JSON)
```json
{
  "type": "sleep",
  "date": "2026-05-03",
  "bedtime": "23:15",
  "wakeup": "06:30",
  "duration_minutes": 435,
  "deep_minutes": 90,
  "light_minutes": 210,
  "rem_minutes": 85,
  "awake_minutes": 50
}
```

```json
{
  "type": "workout",
  "date": "2026-05-03",
  "activity": "Renforcement musculaire",
  "duration_minutes": 45,
  "calories": 320,
  "avg_hr": 135
}
```

### Prévoir aussi un fallback saisie manuelle
- Sommeil : heure coucher / réveil + qualité perçue (1-5 étoiles)
- Sport : type + durée + intensité (légère/modérée/intense)

---

## DESIGN SYSTEM

### Cohérence avec le module Argent
- Fond principal : `#0d0e1a`
- Cards : `#1a1c2e` avec border-radius 10-12px
- Texte principal : `#ffffff`
- Texte secondaire : `#a0a5c0`
- Texte tertiaire : `#7a7f9a`
- Accents : dégradés rose→violet→bleu
- Police : JetBrains Mono (via Google Fonts)
- Glassmorphism : `backdrop-filter: blur(20px); background: rgba(255,255,255,0.05)`

### Mobile first
- Optimisé pour iPhone
- Safe areas (env(safe-area-inset-*))
- Touch targets : minimum 44×44px
- `<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">`
- Apparence PWA : `<meta name="apple-mobile-web-app-capable" content="yes">`

### Navigation
- Barre d'onglets en bas (comme le module Argent)
- 4 onglets : Aujourd'hui / Compétences / Historique / Config
- Onglet actif = couleur d'accent

---

## CLÉS LOCALSTORAGE

Préfixer toutes les clés avec `life_rpg_vie_` :
- `life_rpg_vie_config` — paramètres utilisateur
- `life_rpg_vie_today` — données du jour en cours
- `life_rpg_vie_history` — historique des jours passés
- `life_rpg_vie_skills` — compétences/objectifs et leur progression
- `life_rpg_vie_gauges` — état des jauges avec historique 7 jours

---

## EXPORT / IMPORT

- Bouton export → télécharge un fichier JSON avec toutes les données
- Bouton import → charge un fichier JSON et restaure les données
- Rappel d'export tous les 3 jours (comme le module Argent)

---

## PRIORITÉS DE CONSTRUCTION

### Phase 1 — MVP
1. Structure de la page + navigation 4 onglets
2. Jauges RPG (affichage + calcul avec mémoire 7 jours)
3. Timeline 24h
4. Logger une activité (modal avec type + durée + impacts)
5. Calcul du temps libre restant
6. Import sommeil/sport via URL (pour raccourcis iOS)
7. Saisie manuelle fallback

### Phase 2 — Compétences
8. Création d'objectifs/compétences personnalisés
9. Système XP + niveaux + streaks
10. Visualisation de la progression

### Phase 3 — Historique et polish
11. Calendrier des journées passées
12. Tendances et stats
13. Export/Import JSON
14. Rappel d'export
