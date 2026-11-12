# Guide de validation : Personnalisation événementielle ACME

**Feature**: `001-company-event-acme` | **Date**: 2026-11-12

Ce guide décrit comment vérifier la fonctionnalité de bout en bout. Les décisions sont dans `research.md`, les valeurs dans `data-model.md`, et les exigences dans `spec.md`.

## Prérequis

- Être sur la branche `001-company-event-acme`, avec les modifications de `index.html` et l'asset `css/acme-logo.png` présents.
- Un navigateur avec WebGL (Chrome ou Edge).
- Captures de référence : `formation/tests/captures/01-ecran-accueil.png` (accueil d'origine) et `formation/tests/captures/02-jeu-en-cours.png` (HUD en course).
- Logo officiel : `formation/identite/logo.png`.

## Stratégie de validation

- **QUnit** : exécuté après la Phase 2, après US1 et en fin de ticket. Pas d'exécution après US2 (aucun fichier modifié).
- **Navigateur** : une seule session regroupée en fin de ticket (scénarios 2 à 8). Pas de session par histoire.
- **Course** : seul le démarrage et le HUD sont vérifiés. Une course complète n'est pas exigée (R10).

## Scénario 1 : Régression de la suite QUnit

1. Depuis le dossier `HexGL/`, lancer la suite de tests du projet, décrite dans `formation/tests/README.md`.
2. **Résultat attendu** : code de retour `0` (TESTS_REUSSIS). Si le code est `2`, la vérification est impossible et DOIT être signalée comme telle, pas comme réussie.

## Scénario 2 : Écran d'accueil sur bureau (1280 × 720)

1. Ouvrir le jeu et comparer avec `01-ecran-accueil.png` pour les éléments à préserver (position du menu, animation de fond).
2. **Résultat attendu** :
   - Le logo ACME est visible en entier, sans déformation, dans une marge autour de lui, sur une zone claire. Il est comparé à `formation/identite/logo.png` (SC-005).
   - Le message « Bienvenue au ACME Racing Challenge » est lisible.
   - Le bouton « Démarrer la course » est visible et cliquable.
   - Le fond est sombre (FR-003). Les couleurs des textes, boutons et accents sont celles de la charte (SC-002) ; le logo et le visuel de fond sont exclus.
   - Aucun effet visuel ajouté au-delà de la couche sombre (FR-007).
3. Vérifier que les accents « é » de « Démarrer » et « Bienvenue » s'affichent sans glyphe manquant (R5).

## Scénario 3 : Écran d'accueil à 360 px (360 × 640)

1. Ouvrir le jeu sur un viewport 360 × 640 (émulation mobile).
2. **Résultat attendu** : chaque élément visible (logo, message, bouton, entrées du menu) tient entièrement dans la largeur. Vérifier la position de chaque élément, pas seulement l'absence de barre de défilement, car `overflow: hidden` masque le débordement (R7).

## Scénario 4 : Suivi analytics

1. Ouvrir les outils de développement, onglet réseau. Recharger la page.
2. **Résultat attendu** : aucune requête vers `google-analytics.com` ni vers un domaine nouveau. Le bloc `_gaq` est absent du code de la page (FR-011, FR-010).

## Scénario 5 : Crédits

1. Sur l'accueil, cliquer sur « Credits ».
2. **Résultat attendu** : les crédits HexGL d'origine sont présents, et la section « Personnalisation événementielle ACME » est visible. Cliquer pour revenir à l'accueil (FR-012).

## Scénario 6 : Parcours de démarrage inchangé

1. Cliquer sur « Démarrer la course ».
2. **Résultat attendu** : l'écran d'aide des contrôles s'affiche, puis le chargement, puis la course. Le comportement est identique à la version de référence (histoire utilisateur 1, scénario 4).

## Scénario 7 : HUD en course (sans course complète)

1. Après le démarrage de la course (scénario 6), comparer le HUD (vitesse, bouclier, temps, tours) avec `02-jeu-en-cours.png`.
2. **Résultat attendu** : HUD et contrôles identiques à la référence (FR-006, SC-003). La course n'est pas jouée jusqu'au bout, car aucun fichier de la boucle de jeu n'est modifié (R10).

## Scénario 8 : Écrans secondaires (aide, chargement, fin de course)

1. Vérifier que l'aide `step-2` affiche l'image correspondant au contrôle choisi, et que le chargement `step-3` reste lisible.
2. **Résultat attendu** : aucun élément illisible ni désaligné (FR-008). Ce contrôle ne demande aucune modification de code.
3. Cas limites : si le logo ne charge pas, le texte « ACME » reste visible et le bouton « Démarrer la course » reste utilisable. Après une course, le retour à l'accueil affiche la même identité qu'au premier lancement.

## Vérifications non réalisables

Si Playwright n'est pas disponible, les scénarios 2 à 8 DOIVENT être exécutés à la main en une session unique et signalés comme tels. Ils NE DOIVENT PAS être comptés comme automatisés.

## Validation humaine

Les décisions de `research.md` (fond, vaisseau et logo HexGL, suivi analytics, couleurs du menu) ont été validées par un humain le 2026-11-12. La vérification visuelle de ce guide reste à réaliser avant intégration (principe VI de la constitution).
