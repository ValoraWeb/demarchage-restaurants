# Carnet de démarchage — mode d'emploi

Un seul fichier (`index.html`) contient tout l'outil. Pas d'installation, pas de serveur à gérer.

## 1. Mettre l'outil en ligne avec GitHub Pages

1. Créez un compte GitHub si vous n'en avez pas (gratuit) sur github.com.
2. Créez un nouveau dépôt (repository), par exemple `demarchage-restaurants`. Cochez "Public" (Pages gratuit fonctionne aussi en privé si vous avez un compte payant, mais public suffit largement ici).
3. Ajoutez le fichier `index.html` (et ce `README.md` si vous voulez) à la racine du dépôt — bouton "Add file" → "Upload files", glissez le fichier, puis "Commit".
4. Allez dans **Settings** (du dépôt) → **Pages** dans le menu de gauche.
5. Sous "Build and deployment", choisissez la branche `main` et le dossier `/ (root)`, puis "Save".
6. Après 1 à 2 minutes, votre outil est disponible à une adresse du type :
   `https://votre-nom-utilisateur.github.io/demarchage-restaurants/`

C'est cette adresse que vous et votre associé utiliserez, sur ordinateur ou téléphone.

**Pour mettre à jour l'outil plus tard** (par exemple si vous changez de couleur dans le fichier, ou si je vous envoie une nouvelle version) : remplacez simplement le fichier `index.html` dans le dépôt GitHub par le nouveau, GitHub Pages se met à jour automatiquement.

## 2. Activer le partage entre vous et votre associé (recommandé)

Sans cette étape, l'outil fonctionne, mais **chaque personne a ses propres données dans son navigateur** (rien n'est partagé). Le message "Mode local" apparaît en haut de l'écran pour vous le rappeler.

Pour que vous voyiez tous les deux la même liste en temps réel (et donc que les doublons soient bien détectés entre vous), on branche une base de données gratuite : **Firebase** (service de Google).

### Étapes

1. Allez sur **console.firebase.google.com** et connectez-vous avec un compte Google.
2. Cliquez "Ajouter un projet", donnez-lui un nom (ex : `demarchage-ubereats`), continuez avec les options par défaut.
3. Une fois le projet créé, dans le menu de gauche allez sur **Compilation → Firestore Database**.
4. Cliquez "Créer une base de données". Choisissez une région proche (ex : `eur3 (europe-west)`), puis démarrez en **mode test** (cela autorise la lecture/écriture pendant 30 jours — largement suffisant, et modifiable ensuite, voir plus bas).
5. Retournez à la page d'accueil du projet (icône maison), cliquez sur l'icône **`</>`** ("Ajouter une application Web").
6. Donnez un nom à l'application, cliquez "Enregistrer l'application". Firebase affiche un bloc de code contenant un objet `firebaseConfig` qui ressemble à ceci :

   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "demarchage-ubereats.firebaseapp.com",
     projectId: "demarchage-ubereats",
     storageBucket: "demarchage-ubereats.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef123456"
   };
   ```

7. Copiez ces valeurs. Ouvrez le fichier `index.html`, cherchez la section tout en haut du `<script>` qui dit **"CONFIGURATION FIREBASE"**, et remplacez les guillemets vides par vos vraies valeurs.
8. Enregistrez le fichier et remettez-le à jour sur GitHub (voir section 1). Le bandeau "Mode local" disparaît : vous êtes maintenant en partage en temps réel.

### Sécurité (à savoir)

En mode test, Firestore est ouvert à toute personne qui aurait votre configuration (visible dans le code source de la page). Pour un outil interne à deux personnes ce n'est généralement pas un souci majeur, mais si vous voulez restreindre l'accès, deux options simples :

- Laissez le dépôt GitHub et l'URL de l'outil discrets (ne les publiez pas publiquement).
- Après les 30 jours du mode test, Firestore vous demandera de choisir des règles d'accès : vous pouvez les laisser en lecture/écriture libre (pratique mais ouvert), ou mettre en place une authentification simple si vous voulez aller plus loin — dites-le moi si besoin, je peux vous accompagner.

## 3. Utiliser l'outil au quotidien

- **Nouveau restaurant** : bouton en haut à droite. Si le nom, le téléphone ou l'email correspond à une fiche déjà existante, l'outil vous avertit et vous montre la fiche existante avant de créer un doublon.
- **Recherche** : la barre en haut filtre instantanément par nom, téléphone, email ou région.
- **Filtres** : par région et par statut, en dessous de la barre de recherche.
- **Fiche détail** : cliquez sur une ligne pour voir tous les détails, changer le statut en un clic, ajouter une note à l'historique, ou modifier/supprimer la fiche.
- **Couleur** : icône en forme d'engrenage en haut → choisissez une couleur prédéfinie ou personnalisée. Ce réglage est mémorisé sur votre appareil.
- **Exporter** : bouton "Exporter" en haut → télécharge toutes les fiches en `.json`, utile comme sauvegarde de secours.

## 4. Champs de chaque fiche

Nom, région, téléphone, email, site web, lien Uber Eats, canal de contact prévu (email / WhatsApp / téléphone), personne qui a contacté, statut, notes libres, et un historique horodaté de toutes les actions (créations, changements de statut, relances notées manuellement).

Statuts disponibles : À contacter · Contacté – en attente · Relance nécessaire · Répondu – intéressé · Répondu – pas intéressé · Client Uber Eats.
