# Strok — du dépôt à l'APK

## Ce que contient ce dossier

```
index.html                     l'application
manifest.json                  nom, icônes, couleurs
sw.js                          service worker (installation + hors ligne)
icon-192.png                   icône
icon-512.png                   icône
icon-maskable-512.png          icône adaptative Android
.nojekyll                      fichier vide, à ne pas supprimer
.well-known/assetlinks.json    à compléter à l'étape 4
```

Les six premiers fichiers vont **à la racine du dépôt**, pas dans un sous-dossier.

Le `.nojekyll` empêche GitHub Pages d'ignorer les dossiers commençant par un point —
sans lui, `.well-known/` ne sera jamais servi et l'étape 4 échouera silencieusement.

---

## 1. Mettre en ligne

Crée un dépôt nommé **`tonpseudo.github.io`** — pas un autre nom.
La raison est à l'étape 4 : la vérification Android se fait à la racine du domaine,
et un dépôt ainsi nommé place ton app à la racine. Ça t'évite toute gymnastique.

Dépose les fichiers, puis **Settings → Pages → Deploy from a branch → main / (root)**.

Ouvre `https://tonpseudo.github.io` : l'app doit s'afficher.

## 2. Vérifier avant d'empaqueter

Sur ton téléphone, dans Chrome, ouvre l'URL puis **⋮ → Ajouter à l'écran d'accueil**.
Si l'app se lance en plein écran avec son icône, tout est bon.

**À ce stade tu as déjà une app fonctionnelle avec des téléchargements qui marchent.**
La suite ne sert que si tu veux un fichier APK à installer ou à publier.

## 3. Fabriquer l'APK

Va sur **pwabuilder.com**, colle l'URL, lance l'analyse.
Tu devrais obtenir trois feux verts : Manifest, Service Worker, HTTPS.

**Package for stores → Android → Generate**, avec :

| Champ | Valeur |
|---|---|
| Package ID | `com.tonpseudo.strok` — définitif, choisis-le bien |
| App name | Strok |
| Launcher name | Strok |
| Signing key | **Create new** |

Tu récupères un `.zip` :

| Fichier | Usage |
|---|---|
| `app-release-signed.apk` | à installer sur ton téléphone |
| `app-release-bundle.aab` | pour le Play Store |
| `signing.keystore` | **ta clé — à sauvegarder ailleurs** |
| `signing-key-info.txt` | mots de passe de la clé |
| `assetlinks.json` | pour l'étape 4 |

> Le keystore perdu, tu ne peux plus jamais mettre à jour cette app.
> Sauvegarde-le hors du dépôt, et ne le mets jamais sur GitHub.

## 4. Faire disparaître la barre d'adresse

Sans cette étape, l'app s'ouvre avec la barre Chrome en haut.

Ouvre l'`assetlinks.json` fourni par PWABuilder, copie son contenu dans
`.well-known/assetlinks.json` de ce dossier, remplace les deux valeurs
et pousse sur GitHub.

Vérifie ensuite que `https://tonpseudo.github.io/.well-known/assetlinks.json`
renvoie bien le JSON. Android peut mettre quelques heures à le prendre en compte.

## 5. Installer

Transfère l'APK sur le téléphone, ouvre-le, autorise l'installation depuis une
source inconnue. C'est normal hors Play Store.

---

## iPhone

Rien de tout ça ne s'applique : Apple n'autorise pas les TWA.
« Sur l'écran d'accueil » depuis **Safari** reste la seule voie — et elle donne
déjà le plein écran et l'icône.

## Mettre l'app à jour

Remplace `index.html`, **et incrémente le numéro de version en haut de `sw.js`**
(`strok-v1` → `strok-v2`). Sans ça, l'ancienne version reste en cache.

Pas besoin de refaire l'APK : il ne fait qu'ouvrir ton site, donc il suit tes
mises à jour tout seul.

## Tes dessins

Ils sont dans IndexedDB, sur l'appareil. Vider le cache du navigateur n'y touche
pas ; désinstaller l'app efface tout. Exporte ce à quoi tu tiens.
