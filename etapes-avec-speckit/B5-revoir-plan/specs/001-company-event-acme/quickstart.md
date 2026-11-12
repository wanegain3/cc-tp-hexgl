# Guide de validation : Personnalisation événementielle ACME

**Feature**: `001-company-event-acme` | **Date**: 2026-11-12

Ce guide décrit comment vérifier la fonctionnalité de bout en bout. Les décisions sont dans `research.md`, les valeurs dans `data-model.md`, et les exigences dans `spec.md`.

## Prérequis

- Être sur la branche `001-company-event-acme`, avec les modifications de `index.html` et l'asset `css/acme-logo.png` présents.
- Un navigateur avec WebGL (Chrome ou Edge).
- Capture de référence : `formation/tests/captures/01-ecran-accueil.png`.

## Scénario 1 : Régression de la suite QUnit

1. Depuis le dossier `HexGL/`, lancer la suite de tests du projet, décrite dans `formation/tests/README.md`.
2. **Résultat attendu** : code de retour `0` (TESTS_REUSSIS). Si le code est `2`, la vérification est impossible et DOIT être signalée comme telle, pas comme réussie.

## Scénario 2 : Écran d'accueil sur bureau

1. Ouvrir le jeu sur un écran 1280 × 720 et comparer avec `01-ecran-accueil.png`.
2. **Résultat attendu** :
   - Le logo ACME est visible en entier, sans déformation, sur une zone claire.
   - Le message « Bienvenue au ACME Racing Challenge » est lisible.
   - Le bouton « Démarrer la course » est visible et cliquable.
   - Le fond est sombre (FR-003). Les couleurs des textes, boutons et accents sont celles de la charte (SC-002) ; le logo et le visuel de fond sont exclus de ce critère.
   - Les éléments qui ne doivent pas changer (position du menu, animation de fond) sont préservés.
3. Vérifier l'orthographe des accents « é » dans « Démarrer » et « Bienvenue » : les glyphes doivent s'afficher (R5).

## Scénario 3 : Écran d'accueil à 360 px

1. Ouvrir le jeu sur un viewport 360 × 640 (émulation mobile).
2. **Résultat attendu** : tous les éléments visibles (logo, message, bouton, entrées du menu) tiennent entièrement dans la largeur. Vérifier la position de chaque élément, pas seulement l'absence de barre de défilement, car `overflow: hidden` masque le débordement (R7).

## Scénario 4 : Suivi analytics

1. Ouvrir les outils de développement, onglet réseau. Recharger la page.
2. **Résultat attendu** : aucune requête vers `google-analytics.com`. Le bloc `_gaq` est absent du code de la page (FR-011).

## Scénario 5 : Crédits

1. Sur l'accueil, cliquer sur « Credits ».
2. **Résultat attendu** : les crédits HexGL d'origine sont présents, et la section « Personnalisation événementielle ACME » est visible. Cliquer pour revenir à l'accueil (FR-012).

## Scénario 6 : Parcours de démarrage inchangé

1. Cliquer sur « Démarrer la course ».
2. **Résultat attendu** : l'écran d'aide des contrôles s'affiche, puis le chargement, puis la course. Le comportement est identique à la version de référence (histoire utilisateur 1, scénario 4).

## Scénario 7 : Gameplay (contrôle léger)

1. Lancer une course et comparer les éléments du HUD avec `02-jeu-en-cours.png`.
2. **Résultat attendu** : HUD, contrôles et compte à rebours identiques à la référence (FR-006). Une course complète n'est pas requise, car aucun fichier de la boucle de jeu n'est modifié (R10).

## Vérifications non réalisables

Si Playwright n'est pas disponible, les scénarios 2 à 5 DOIVENT être exécutés à la main et signalés comme tels. Ils NE DOIVENT PAS être comptés comme automatisés.

## Validation humaine

Les décisions de `research.md` (fond, vaisseau et logo HexGL, suivi analytics, couleurs du menu) ont été validées par un humain le 2026-11-12. La vérification visuelle de ce guide reste à réaliser avant intégration (principe VI de la constitution).
