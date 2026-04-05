# 🏀 HoopStat — Live Basketball Tracker

Application web de suivi de statistiques de basketball en **temps réel**, multi-terrains, multi-utilisateurs. Chaque utilisateur saisit les données de son terrain depuis son propre appareil (téléphone, tablette, PC), et tout le monde voit les données instantanément.

---

## Démarrage rapide

### Sans Firebase (local / même réseau WiFi)
1. Ouvrir `hoopstat.html` dans un navigateur
2. Entrer un code de session + votre prénom → **Rejoindre**
3. Sélectionner un terrain, saisir les stats, cliquer **Enregistrer**

> Les données restent locales au navigateur. Pratique pour tester.

### Avec Firebase (recommandé — synchronisation sur 4G/5G)
Suivre les 3 étapes ci-dessous, puis distribuer le fichier `hoopstat.html` à tous les utilisateurs.

---

## Configuration Firebase (étape par étape)

### Étape 1 — Créer un projet Firebase

1. Aller sur [console.firebase.google.com](https://console.firebase.google.com) avec un compte Google
2. Cliquer **Créer un projet** → donner un nom (ex: `hoopstat-tournoi`) → continuer
3. Désactiver Google Analytics si non nécessaire → **Créer le projet**

### Étape 2 — Créer la base de données

1. Dans le menu gauche → **Build → Realtime Database**
2. Cliquer **Créer une base de données**
3. Choisir la région (Europe-West recommandé)
4. Sélectionner **Mode test** → **Activer**

### Étape 3 — Récupérer la configuration

1. Icône engrenage (⚙) → **Paramètres du projet**
2. Onglet **Général** → scroller jusqu'à **Vos applications**
3. Cliquer l'icône Web **</>** → nommer l'appli (ex: `hoopstat-web`) → **Enregistrer**
4. Firebase affiche un bloc de configuration. Copier les valeurs :

```
apiKey:            "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXX"
databaseURL:       "https://mon-projet-default-rtdb.firebaseio.com"
projectId:         "mon-projet"
```

### Étape 4 — Saisir la configuration dans l'application

1. Ouvrir `hoopstat.html` → se connecter → aller dans l'onglet **Configuration**
2. Coller les 3 valeurs dans les champs correspondants
3. Cliquer **Enregistrer et recharger**
4. La bannière orange disparaît → le badge affiche **LIVE** → Firebase est actif

### Étape 5 — Sécuriser l'accès (recommandé)

Dans Firebase Console → Realtime Database → **Règles**, remplacer le contenu par :

```json
{
  "rules": {
    "sessions": {
      "$sessionId": {
        ".read":  "auth == null",
        ".write": "auth == null"
      }
    }
  }
}
```

Cliquer **Publier**. Les données sont désormais isolées par code de session.

---

## Utilisation

### Connexion
Chaque utilisateur ouvre le fichier `hoopstat.html` et entre :
- **Code de session** : un mot-clé commun à tous les participants (ex: `TOURNOI2025`). Les utilisateurs partageant le même code voient les mêmes données.
- **Prénom** : affiché dans le tableau de bord pour savoir qui a mis à jour quel terrain.

### Saisie des statistiques
1. Sélectionner son terrain (1 à 10) dans la grille en haut
2. Saisir les noms des équipes
3. Mettre à jour le score avec les boutons +/−
4. Incrémenter les statistiques au fur et à mesure du match :
   - **Tirs tentés** : chaque tentative de tir
   - **Tirs réussis** : chaque tir converti (le % se calcule automatiquement)
   - **Pertes de balle** : chaque perte
5. Cliquer **Enregistrer** → les données sont immédiatement visibles par tous

### Tableau de bord
Vue en temps réel de tous les matchs en cours. Les données s'actualisent automatiquement dès qu'un utilisateur enregistre son terrain.

### Historique
Tableau récapitulatif de toutes les sauvegardes de la session, avec le prénom de l'utilisateur qui a effectué chaque mise à jour.

---

## Architecture des données

```
Firebase Realtime Database
└── sessions/
    └── [CODE_DE_SESSION]/
        ├── court_1/
        │   ├── t1: "Les Aigles"
        │   ├── t2: "Red Bulls"
        │   ├── sc1: 42
        │   ├── sc2: 38
        │   ├── s1: { shots: 35, made: 18, to: 5 }
        │   ├── s2: { shots: 31, made: 16, to: 7 }
        │   ├── by: "Thomas"
        │   └── ts: "14:32:07"
        ├── court_2/ ...
        └── court_10/ ...
```

Chaque session est complètement isolée. Deux événements différents utilisant des codes différents ne voient pas les données de l'autre.

---

## Distribuer le fichier aux utilisateurs

Le fichier `hoopstat.html` est **autonome** (une seule page, aucune dépendance à installer). Pour le distribuer :

- Par e-mail en pièce jointe
- Via WhatsApp, Telegram, AirDrop
- Depuis un serveur web ou GitHub Pages
- Clé USB

Chaque utilisateur l'ouvre dans Chrome, Firefox, Safari ou Edge sur son téléphone ou PC.

---

## Compatibilité

| Navigateur | Support |
|------------|---------|
| Chrome 70+ | ✅ Complet |
| Firefox 70+ | ✅ Complet |
| Edge 79+ | ✅ Complet |
| Safari 14+ | ✅ Complet |
| Internet Explorer | ❌ Non supporté |

---

## Confidentialité et données

- La configuration Firebase (apiKey, databaseURL) est stockée dans le `localStorage` du navigateur de chaque utilisateur.
- Les données de match transitent par les serveurs Google Firebase, chiffrées en HTTPS.
- Aucune donnée personnelle n'est collectée (le prénom saisi n'est utilisé qu'en affichage).
- Les données persistent dans Firebase jusqu'à suppression manuelle depuis la console Firebase.

---

## Évolutions possibles

- Chronomètre par terrain (quarts-temps, mi-temps)
- Export CSV ou PDF des statistiques en fin d'événement
- Statistiques individuelles par joueur
- Authentification par code PIN par terrain
- Affichage public (écran de tableau d'honneur)

---

*HoopStat — fichier HTML unique, propulsé par Firebase Realtime Database.*
