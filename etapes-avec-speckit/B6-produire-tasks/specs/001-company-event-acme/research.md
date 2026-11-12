# Recherche technique : Personnalisation événementielle ACME

**Feature**: `001-company-event-acme` | **Date**: 2026-11-12
**Entrées** : `spec.md`, `docs/architecture.md`, `.specify/memory/constitution.md`, `index.html`, `css/multi.css`, `css/bg.jpg`, `css/title.png`, `formation/tests/captures/01-ecran-accueil.png`

Ce document résout les points techniques du plan. Chaque décision indique le choix retenu, sa justification et les alternatives rejetées.

## Constat initial (écart spec / existant)

L'écran d'accueil actuel est **clair** : le fond `css/bg.jpg` est un visuel pastel, et le titre `css/title.png` montre le vaisseau et le logo « HexGL by BKcore » (capture de référence `01-ecran-accueil.png`). La spec demande un fond sombre (FR-003, SC-002), et la charte le demande aussi pour les usages événementiels. La décision R1 traite cet écart.

## R1 : Fond sombre sans remplacer l'image de fond

- **Décision** : superposer une couche sombre semi-transparente (couleur `#08090D`, opacité environ 0,78) sur `css/bg.jpg`, via la propriété `background` de `#global` redéfinie dans le bloc `<style>` de `index.html`.
- **Justification** : conserve l'animation et le visuel de fond existants, n'ajoute aucun fichier, et s'annule par un retour Git. La charte demande un fond sombre et des accents lumineux.
- **Alternatives rejetées** :
  - Remplacer `bg.jpg` par une nouvelle image sombre : ajoute un asset et supprime le visuel d'origine.
  - Fond uni `#08090D` : perd le visuel d'origine, plus radical que nécessaire.
- **Validé** (2026-11-12) : le visuel d'origine est conservé et atténué. Traduit dans FR-003 et dans les Hypothèses de la spec.

## R2 : Logo ACME présenté sur une zone claire

- **Décision** : copier `formation/identite/logo.png` vers `css/acme-logo.png` sans aucune modification, et l'afficher par une balise `<img>` (avec `alt="ACME"`) dans `#title`, sans déformation (`height` ou `width` seul, ratio conservé).
- **Justification** : le logo officiel est un PNG à fond blanc. Sur fond sombre, il forme naturellement la zone claire demandée par la charte et par le cas limite de la spec. Une balise `<img>` garde le texte alternatif pour l'accessibilité.
- **Alternatives rejetées** :
  - Intégrer le logo dans `title.png` : modifie un asset d'origine et n'est pas réversible proprement.
  - Utiliser `background-image` sur `#title` : pas de texte alternatif.
  - Ajouter une ombre ou un contour : interdit par la charte.
- **Note** : le fichier source reste dans `formation/identite/`, qui n'est pas modifié.

## R3 : Retrait de l'identité HexGL de l'accueil

- **Décision** : remplacer le fond de `#title` par le logo ACME (R2). `css/title.png` n'est plus référencé par l'accueil mais est conservé dans le dépôt.
- **Justification** : FR-005 exige que le nom d'origine ne soit plus l'identité principale. Conserver le fichier évite une suppression et garde le retour Git simple.
- **Conséquence visible** : le vaisseau et le logo « HexGL by BKcore » disparaissent de l'accueil.
- **Validé** (2026-11-12) : traduit dans FR-005 et dans les Hypothèses de la spec.

## R4 : Suivi analytics désactivé

- **Décision** : supprimer le bloc `<script>` d'analytics (`_gaq` et chargement de `ga.js`) de `index.html`, lignes 32 à 42.
- **Vérification** : une recherche de `_gaq` dans les fichiers JavaScript et HTML du projet (hors `libs/`, `formation/` et `etapes/`) ne trouve que ce bloc. Aucun autre code ne dépend donc du suivi.
- **Justification** : réponse Q1 de la clarification (désactivation pour l'événement), traduite par un retrait effectif jusqu'à un retour Git (validé, voir plus bas). Supprimer le bloc est plus clair qu'un commentaire et couvre FR-011.
- **Alternatives rejetées** :
  - Commenter le bloc : laisse du code mort et peut être réactivé par erreur.
  - Condition sur un paramètre d'URL : ajoute une logique et une option non demandées.
- **Validé** (2026-11-12) : la suppression reste effective jusqu'à un retour Git, donc au-delà de la durée de l'événement. Traduit dans FR-011.

## R5 : Message d'accueil et libellé du bouton

- **Décision** :
  - Ajouter un élément `#welcome` contenant « Bienvenue au ACME Racing Challenge », placé sous le logo.
  - Remplacer le texte de `#start` (« Start ») par « Démarrer la course ». L'`id` `start` est conservé, car `launch.js` et `launch.coffee` le référencent pour le clic.
- **Justification** : réponse Q3 (français, selon la charte). Le changement de texte ne touche pas au comportement.
- **Point à vérifier** : la police BebasNeue doit afficher correctement « é » et « è ». Vérification prévue par capture (`quickstart.md`, scénario 2, étape 3). Si un glyphe manque, la police de repli `arial` s'applique, à signaler.
- **Conséquence** : les autres libellés du menu restent en anglais (hypothèse de la spec).

## R6 : Couleurs de la charte

- **Décision** :
  - Texte du menu : remplacer `#4190bb` par le cyan de la charte `#00C4E8`, qui offre un contraste suffisant sur fond sombre.
  - Bouton `#start` : texte blanc sur un dégradé `#7628FF` vers `#315BFF`, supprimant la couleur orange `#f66439` d'origine, et le survol associé.
  - Message d'accueil : blanc (`#FFFFFF`).
- **Justification** : FR-003 et SC-002 exigent que les seules couleurs visibles soient celles de la charte.
- **Alternatives rejetées** : garder le bleu `#4190bb` et l'orange, ce qui laisse des couleurs hors palette et contredit SC-002.
- **Validé** (2026-11-12) : couleurs traduites dans SC-002, dont le périmètre est limité aux textes, boutons et accents d'interface. Le contraste exact du cyan et du blanc reste à vérifier visuellement (scénario 2).

## R7 : Mise en page à 360 px

- **Décision** : ajouter une règle `@media (max-width: 480px)` dans le bloc `<style>` de `index.html` pour réduire la taille du logo (largeur maximale 90 % du viewport) et celle du menu (`font-size` réduite), et empêcher tout débordement du contenu.
- **Constat important** : `html, body` ont `overflow: hidden` dans `multi.css`. Un débordement horizontal ne produit donc **pas** de barre de défilement, il est simplement coupé. La vérification NE DOIT PAS se limiter à l'absence de barre : elle DOIT vérifier que chaque élément visible tient dans 360 px.
- **Alternatives rejetées** : changer `overflow` sur `body` : modifierait le comportement du jeu.

## R8 : Crédits

- **Décision** : conserver le bloc `#credits` existant (auteurs HexGL et mention des technologies), et ajouter en fin de bloc, avant « Click anywhere to continue », une section « Personnalisation événementielle ACME » mentionnant ACME et la charte fournie.
- **Justification** : réponse Q2 (conserver HexGL et ajouter ACME), FR-012.

## R9 : Tests

- **Décision** :
  - QUnit : exécution de la suite existante comme test de régression. Aucun nouveau test JavaScript, car aucun code JavaScript n'est modifié.
  - Aucun test ne vérifie le texte, les couleurs ou le logo (branding), conformément à la constitution.
  - Vérification visuelle et d'interaction : Playwright si disponible, sinon vérification manuelle signalée comme telle.
- **Point de vigilance** : le projet n'a pas de `package.json`. Playwright ne doit pas être ajouté au dépôt. S'il est utilisé, c'est depuis un dossier hors du dépôt, et seulement après accord.
- **Alternatives rejetées** : tests QUnit de présence du texte dans `index.html` : cela figerait le branding, interdit par la constitution.

## R10 : Course et gameplay

- **Décision** : pas de course complète obligatoire. Aucun fichier de la boucle de jeu n'est modifié. La capture de la course (`02-jeu-en-cours.png`) peut être comparée pour contrôle visuel rapide.
- **Justification** : la constitution n'exige la course complète que si le gameplay risque d'être affecté. Ce n'est pas le cas.

## Décisions validées (principe VI)

Ces quatre décisions changent le rendu existant ou le périmètre de la spec. Elles ont été validées par un humain le 2026-11-12 et sont traduites dans la spec : FR-003, FR-005, FR-011, SC-002 et les Hypothèses.

1. **Fond** (R1) : visuel d'origine conservé et atténué par une couche sombre.
2. **Vaisseau et logo HexGL** (R3) : retirés de l'accueil ; fichiers conservés dans le dépôt.
3. **Suivi analytics** (R4) : retrait effectif jusqu'à un retour Git, donc au-delà de la durée de l'événement.
4. **Couleurs du menu** (R6) : bleu et orange d'origine remplacés par la palette de la charte.

La vérification visuelle de ces choix reste à réaliser avec `quickstart.md`.
