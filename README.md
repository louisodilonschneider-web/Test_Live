# 🏀 HoopStat Firebase — Live Basketball Tracker

Application web de suivi de statistiques basketball en temps réel, multi-terrains, multi-utilisateurs. Chaque saisisseur utilise son propre appareil (téléphone, tablette, PC) sur sa propre connexion 4G/5G, et les données se synchronisent instantanément pour tout le monde.

---

## Fichier à utiliser

```
hoopstat-firebase.html
```

C'est le seul fichier nécessaire. Il est autonome — aucune installation, aucune dépendance.

---

## Mise en place — une seule fois, par l'organisateur

### Étape 1 — Créer un projet Firebase

1. Aller sur https://console.firebase.google.com avec un compte Google
2. Cliquer **Créer un projet** → donner un nom (ex : `hoopstat`) → continuer
3. Désactiver Google Analytics → **Créer le projet**

### Étape 2 — Activer la base de données

1. Dans le menu gauche → **Créer** → **Realtime Database**
2. Cliquer **Créer une base de données**
3. Choisir la région **europe-west1 (Belgium)**
4. Sélectionner **Démarrer en mode test** → **Activer**

> En mode test, la base est ouverte pendant 30 jours. Suffisant pour un tournoi. Les avertissements Firebase peuvent être ignorés.

### Étape 3 — Créer une application Web

1. Cliquer sur la roue dentée ⚙ en haut à gauche → **Paramètres du projet**
2. Faire défiler jusqu'à **Vos applications** → cliquer l'icône **</>**
3. Donner un nom (ex : `hoopstat-web`) — ne pas cocher Firebase Hosting
4. Cliquer **Enregistrer l'application**

### Étape 4 — Récupérer les clés de configuration

Firebase affiche un bloc de code contenant les clés. Repérer et noter ces 7 valeurs :

```
apiKey
authDomain
databaseURL      ← commence par https://
projectId
storageBucket
messagingSenderId
appId
```

> Si `databaseURL` n'apparaît pas dans le bloc, aller dans **Realtime Database** (menu gauche) — l'URL est affichée en haut de la page.
> Format attendu : `https://[projet]-default-rtdb.europe-west1.firebasedatabase.app`

### Étape 5 — Configurer le fichier HTML

1. Ouvrir `hoopstat-firebase.html` dans un navigateur
2. L'écran de configuration s'affiche automatiquement
3. Coller chaque valeur dans le champ correspondant
4. Cliquer **Enregistrer et lancer**

> La configuration est mémorisée dans le navigateur. Elle ne doit être faite qu'une seule fois par appareil.

---

## Distribuer l'application aux saisisseurs

### Option A — GitHub Pages (recommandée)

Héberger le fichier en ligne : les utilisateurs accèdent via un simple lien, sans rien configurer.

1. Créer un compte sur https://github.com
2. Cliquer **+** → **New repository** → nommer `hoopstat` → cocher **Public** → cocher **Add a README** → **Create repository**
3. Dans le repository : **Add file** → **Upload files** → glisser `hoopstat-firebase.html`
4. **Renommer le fichier en `index.html`** avant de valider
5. Cliquer **Commit changes**
6. Aller dans **Settings** → **Pages** → sous Branch sélectionner **main** → **Save**
7. Attendre 2 minutes → URL générée : `https://votre-pseudo.github.io/hoopstat`

Partager cette URL par WhatsApp. Les saisisseurs ouvrent le lien, entrent prénom + code événement → c'est tout.

### Option B — Netlify Drop

1. Aller sur https://app.netlify.com/drop
2. Glisser-déposer `hoopstat-firebase.html`
3. Partager l'URL générée (ex : `https://luminous-unicorn-abc123.netlify.app`)

### Option C — Envoi direct du fichier

Envoyer `hoopstat-firebase.html` par WhatsApp, email ou AirDrop.
Chaque utilisateur l'ouvre dans son navigateur et devra configurer Firebase une fois (saisir les 7 clés) sur son appareil.

---

## Utilisation le jour J

### Pour l'organisateur

Définir un **code événement** court et mémorisable (ex : `BASKET25`, `TOUR2025`) et le communiquer à tous les saisisseurs avant le début.

### Pour chaque saisisseur

1. Ouvrir le lien (ou le fichier)
2. Entrer son **prénom** et le **code événement** commun
3. Sélectionner son terrain (1 à 10)
4. Saisir les statistiques en live :
   - **Score** : boutons +/− pour chaque équipe
   - **Tirs tentés** : chaque tentative
   - **Tirs réussis** : chaque panier converti (% calculé automatiquement)
   - **Pertes de balle** : chaque turnover
5. Appuyer sur **Enregistrer** → données visibles instantanément par tous

> Les terrains déjà pris par un autre saisisseur apparaissent avec un point bleu et sont en lecture seule pour les autres.

### Tableau de bord

L'onglet **Tableau de bord** affiche tous les matchs en cours en temps réel. Idéal sur un grand écran ou vidéoprojecteur pour la table centrale.

### Historique

Tableau récapitulatif de toutes les sauvegardes de la session avec scores, statistiques complètes, nom du saisisseur et heure.

---

## Structure des données Firebase

```
events/
  {CODE_EVENEMENT}/
    courts/
      1/   → { t1, t2, sc1, sc2, s1, s2, savedBy, ts }
      2/   → ...
      10/  → ...
    history/
      {id_horodaté}/ → chaque sauvegarde avec toutes les données
```

Deux événements utilisant des codes différents sont complètement isolés l'un de l'autre.

| Champ | Description |
|-------|-------------|
| `t1` / `t2` | Noms des équipes |
| `sc1` / `sc2` | Scores |
| `s1.shots` / `s2.shots` | Tirs tentés |
| `s1.made` / `s2.made` | Tirs réussis |
| `s1.to` / `s2.to` | Pertes de balle |
| `savedBy` | Prénom du saisisseur |
| `ts` | Heure de la dernière sauvegarde |

---

## Réinitialiser la configuration Firebase

Si vous devez changer de projet Firebase ou reconfigurer :

- Ouvrir les **DevTools** (touche F12) → onglet **Application** → **Local Storage**
- Supprimer la clé `hs_firebase_cfg`
- Recharger la page → l'écran de configuration réapparaît

---

## Mode démo

Sans Firebase, cliquer **Mode démo** sur l'écran de configuration. L'application fonctionne normalement mais les données restent dans le navigateur et ne sont pas partagées avec d'autres utilisateurs.

---

## Compatibilité

| Navigateur | Support |
|------------|---------|
| Chrome 70+ | ✅ Complet |
| Firefox 70+ | ✅ Complet |
| Edge 79+ | ✅ Complet |
| Safari 14+ | ✅ Complet |
| Samsung Internet 12+ | ✅ Complet |
| Internet Explorer | ❌ Non supporté |

---

## Confidentialité

- Les données transitent par les serveurs Google Firebase, chiffrées en HTTPS
- Le code événement isole les données — seuls les utilisateurs connaissant le code y accèdent
- Aucune donnée personnelle collectée (le prénom n'est utilisé qu'en affichage)
- La configuration Firebase est stockée localement dans le navigateur (`localStorage`)
- Les données persistent dans Firebase jusqu'à suppression depuis la console Firebase

---

## Évolutions possibles

- Chronomètre par terrain (quarts-temps)
- Export CSV ou PDF en fin d'événement
- Statistiques individuelles par joueur
- Affichage public en lecture seule (écran tableau d'honneur)
- Code PIN par terrain pour renforcer le verrouillage

---

*HoopStat — fichier HTML unique, propulsé par Firebase Realtime Database.*
