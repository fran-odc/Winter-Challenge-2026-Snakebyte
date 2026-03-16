# 🐍 Winter Challenge 2026 - Exotec Snakebots

# 🎯 Objectif
Récupérez de l'énergie pour faire grandir vos robot-serpents et ayez les plus longs en fin de partie.

# 🏗️ Grille & Plateformes
✅ Grille vue de côté avec plateformes infranchissables
✅ Cases : vide | plateforme | corps snakebot | énergie
✅ Gravité : snakebot tombe si pas support solide
✅ Solides : plateformes + énergie + autres snakebots

# 🐍 Snakebots
✅ Multi-segments adjacents (tête + corps)
✅ Déplacement simultané tous snakebots
✅ Direction initiale : HAUT
✅ Continuent direction précédente par défaut

# ↪️ Mécanique Déplacement

- **Cas 1** : Collision tête
Plateforme ou corps → tête détruite (≥3 segments)
< 3 segments → snakebot supprimé

- **Cas 2** : Énergie
✅ Snakebot grandit (nouveau segment queue)
✅ Énergie disparaît (case = vide après)
✅ Multi-têtes = tous mangent (énergie partagée !)

# Phase Gravité
- Après collisions → tous tombent jusqu'à support solide
- Hors grille = suppression

# 🎮 Commandes (stdout)

**Directions** : `id UP | DOWN | LEFT | RIGHT`
**Debug** : `id RIGHT Debug text`
**Marqueur** : `MARK x y` (max 4/tour)
**Pause** : `WAIT`
**Séparateur** : `;`
Directions : UP(0,-1) | DOWN(0,1) | LEFT(-1,0) | RIGHT(1,0)

# ⛔ Fin de Partie
❌ Tous snakebots supprimés
❌ Plus d'énergie  
❌ 200 tours max

# 🏆 Victoire/Défaite
✅ **VICTOIRE** : Plus de segments totaux
❌ **DÉFAITE** : Timeout | commandes invalides

# 🔧 Débogage
🔸 MARK x y (viewer)
🔸 Survol grille (infos cases)
🔸 ⚙️ Options viewer
🔸 Clavier : ESPACE (pause) | Flèches (frame par frame)

[📂 Source Officiel](https://www.codingame.com/home) 
