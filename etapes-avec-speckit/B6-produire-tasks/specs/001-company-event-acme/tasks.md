# Tasks: Personnalisation événementielle ACME

**Input**: Design documents from `/specs/001-company-event-acme/`

**Prerequisites**: plan.md (requis), spec.md (requis pour les histoires utilisateur), research.md, data-model.md, quickstart.md

**Tests**: Aucune tâche d'écriture de tests n'est générée. Aucun code JavaScript n'est modifié, donc aucun nouveau test unitaire n'est pertinent (plan.md, principe V ; research.md, R9). Les tâches de vérification (suite QUnit de régression, contrôles visuels et d'interaction) sont incluses et portent les numéros des scénarios de `quickstart.md`.

**Organization**: Les tâches sont regroupées par histoire utilisateur pour permettre une implémentation et une vérification indépendantes de chaque histoire.

**Périmètre des fichiers** : toutes les modifications de code passent par `index.html`. Les tâches qui le modifient s'exécutent donc séquentiellement et ne sont pas marquées `[P]`.

## Format: `[ID] [P?] [Story] Description`

- **[P]** : peut s'exécuter en parallèle (fichiers différents, aucune dépendance)
- **[Story]** : histoire utilisateur concernée (US1, US2, US3)
- Chemins de fichiers exacts dans chaque description

## Path Conventions

Projet statique à racine unique : `HexGL/`. Fichiers concernés : `HexGL/index.html` et `HexGL/css/acme-logo.png`.

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Préparer l'asset et établir la référence de régression avant toute modification.

- [ ] T001 [P] Copier `formation/identite/logo.png` vers `css/acme-logo.png` (dans `HexGL/`) sans aucune modification ; la taille doit rester identique à l'original (363 727 octets)
- [ ] T002 [P] Exécuter la suite QUnit depuis le dossier `HexGL/` (décrite dans `formation/tests/README.md`) avant toute modification et noter le code de retour de référence (attendu : `0`, TESTS_REUSSIS)

**Checkpoint** : asset présent et référence de régression connue.

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Retrait du suivi d'audience, qui touche la page entière et n'appartient à aucune histoire utilisateur précise (FR-011, R4).

**⚠️ CRITICAL** : cette phase DOIT être terminée avant toute histoire utilisateur, car elle modifie le même fichier `index.html`.

- [ ] T003 Supprimer le bloc `<script type="text/javascript">` d'analytics (lignes 32 à 42 de `HexGL/index.html`, contenant `_gaq` et le chargement de `ga.js`) ; le suivi est « désactivé (bloc de suivi retiré) » selon data-model.md
- [ ] T004 Vérifier qu'aucune occurrence de `_gaq` ne subsiste dans `HexGL/index.html` et que l'ouverture de la page n'envoie aucune requête vers `google-analytics.com` (quickstart.md, scénario 4)

**Checkpoint** : la page ne contient plus de suivi d'audience externe.

---

## Phase 3: User Story 1 - Accueil aux couleurs d'ACME (Priority: P1) 🎯 MVP

**Goal** : l'écran d'accueil présente le logo ACME officiel, le message « Bienvenue au ACME Racing Challenge », le bouton « Démarrer la course », un fond sombre et les couleurs de la charte.

**Independent Test** : ouvrir le jeu sur l'écran d'accueil à 1280 × 720 et à 360 × 640, comparer avec `formation/tests/captures/01-ecran-accueil.png` et vérifier les scénarios 2, 3 et 6 de `quickstart.md`.

- [ ] T005 [US1] Ajouter dans `<div id="title">` de `HexGL/index.html` une balise `<img id="acme-logo" src="css/acme-logo.png" alt="ACME">` ; le logo DOIT être affiché sans déformation, son ratio d'origine étant conservé (data-model.md : « aucune déformation, aucune recoloration »)
- [ ] T006 [US1] Ajouter sous le logo, dans `<div id="title">` de `HexGL/index.html`, un élément `<p id="welcome">` dont le texte est exactement « Bienvenue au ACME Racing Challenge » (data-model.md : le message DOIT correspondre exactement au texte de la spec)
- [ ] T007 [US1] Remplacer le texte de `<div id="start">Start</div>` par « Démarrer la course » dans `HexGL/index.html` ; l'attribut `id="start"` DOIT être conservé, car `launch.js` le référence pour le clic
- [ ] T008 [US1] Dans le bloc `<style>` de `HexGL/index.html`, redéfinir le `background` de `#global` pour superposer une couche `rgba(8, 9, 13, 0.78)` au-dessus de l'image `css/bg.jpg` (le chemin DOIT être `css/bg.jpg` depuis `index.html`, et non `bg.jpg` comme dans `multi.css`) ; le fond principal DOIT être sombre (FR-003)
- [ ] T009 [US1] Dans le bloc `<style>` de `HexGL/index.html`, retirer l'image de fond de `#title` (`css/title.png`) pour que l'identité HexGL ne soit plus affichée ; le fichier `css/title.png` NE DOIT PAS être supprimé
- [ ] T010 [US1] Dans le bloc `<style>` de `HexGL/index.html`, appliquer la palette de la charte : texte de `#menu` en `#00C4E8` ; texte de `#start` en `#FFFFFF` sur un fond `linear-gradient(#7628FF, #315BFF)` ; texte de `#welcome` en `#FFFFFF`. `multi.css` définit `#start:hover` avec `!important` sur la couleur orange : la surcharge DOIT aussi utiliser `!important` pour que le survol reste dans la palette (research.md, R6)
- [ ] T011 [US1] Dans le bloc `<style>` de `HexGL/index.html`, ajouter `@media (max-width: 480px)` : `#acme-logo` avec `max-width: 90%`, taille de police réduite pour `#menu` et `#welcome`, sans débordement horizontal (research.md, R7 ; FR-009 : « utilisable sur un écran de 360 px de large, sans défilement horizontal »)
- [ ] T012 [US1] Vérifier à 1280 × 720 que le logo, le message et le bouton sont visibles et que la position du menu et l'animation de fond sont préservées, en comparant avec `formation/tests/captures/01-ecran-accueil.png` (quickstart.md, scénario 2, étapes 1 et 2)
- [ ] T013 [US1] Vérifier que les accents « é » de « Démarrer » et « Bienvenue » s'affichent sans glyphe manquant avec la police BebasNeue (quickstart.md, scénario 2, étape 3 ; research.md, R5). En cas d'échec, signaler la police de repli
- [ ] T014 [US1] Vérifier à 360 × 640 que chaque élément visible (logo, message, bouton, entrées du menu) est entièrement contenu dans la largeur : `overflow: hidden` masque le débordement, donc l'absence de barre de défilement ne suffit pas (quickstart.md, scénario 3 ; research.md, R7)
- [ ] T015 [US1] Vérifier que le clic sur « Démarrer la course » affiche l'écran d'aide `step-2`, puis le chargement `step-3`, puis la course, comme dans la version de référence (quickstart.md, scénario 6 ; spec US1, scénario 4)

**Checkpoint** : User Story 1 est complète et vérifiable seule ; c'est le MVP.

---

## Phase 4: User Story 2 - Course inchangée (Priority: P2)

**Goal** : le gameplay, les contrôles, les règles de course et le HUD en course restent identiques à la version de référence.

**Independent Test** : aucun fichier de la boucle de jeu n'est modifié, et les éléments du HUD en course sont identiques à `formation/tests/captures/02-jeu-en-cours.png` (quickstart.md, scénario 7).

- [ ] T016 [P] [US2] Vérifier avec `git diff --name-only` que ni `bkcore/`, ni `bkcore.coffee/`, ni `libs/`, ni `launch.js`, ni `launch.coffee` n'est modifié (FR-006 ; plan.md, principe I)
- [ ] T017 [P] [US2] Lancer une course et comparer le HUD (vitesse, bouclier, temps, tours, messages) et le compte à rebours avec `formation/tests/captures/02-jeu-en-cours.png` (quickstart.md, scénario 7). Une course complète NE DOIT PAS être exigée, car aucun fichier de la boucle de jeu n'est modifié (research.md, R10)

**Checkpoint** : aucun changement du gameplay n'est détecté.

---

## Phase 5: User Story 3 - Écrans secondaires sobres (Priority: P3)

**Goal** : les crédits conservent les mentions HexGL et ajoutent la mention ACME ; l'aide, le chargement et la fin de course restent fonctionnels et lisibles.

**Independent Test** : parcourir les crédits, l'aide, le chargement et la fin de course, et vérifier qu'aucun élément n'est illisible ou désaligné (quickstart.md, scénario 5).

- [ ] T018 [US3] Dans `<div id="credits">` de `HexGL/index.html`, conserver tout le bloc HexGL existant et insérer, avant `<h4>Click anywhere to continue.</h4>`, une section `<h3>` « Personnalisation événementielle ACME » suivie d'un `<p>` mentionnant ACME et la charte fournie (FR-012 ; research.md, R8)
- [ ] T019 [US3] Vérifier le scénario de crédits : ouvrir « Credits », contrôler que les mentions HexGL d'origine sont intactes, que la mention ACME est présente, puis revenir à l'accueil par un clic (quickstart.md, scénario 5)
- [ ] T020 [P] [US3] Vérifier que l'écran d'aide `step-2` affiche l'image `css/help-<controlType>.png` correspondant au contrôle choisi (spec US3, scénario 2) ; ce contrôle ne DOIT PAS nécessiter de modification de code
- [ ] T021 [P] [US3] Vérifier que les écrans de chargement `step-3` et de fin de course `step-5` restent lisibles, sans élément désaligné (spec US3, FR-008) ; ce contrôle ne DOIT PAS nécessiter de modification de code

**Checkpoint** : les écrans secondaires sont fonctionnels et lisibles.

---

## Phase N: Polish & Cross-Cutting Concerns

**Purpose** : régression finale et validation humaine (principe VI de la constitution).

- [ ] T022 Relancer la suite QUnit depuis `HexGL/` après l'ensemble des modifications et comparer le code de retour à la référence de T002 (quickstart.md, scénario 1). Si le code est `2`, la vérification DOIT être signalée comme impossible, et NON comme réussie
- [ ] T023 Vérifier que `docs/architecture.md` et `CLAUDE.md` ne sont pas modifiés (constitution, principe IV : `docs/architecture.md` décrit l'état actuel)
- [ ] T024 Consigner dans le compte rendu de validation les vérifications réussies, échouées ou non réalisées ; toute vérification non réalisée (par exemple Playwright indisponible) DOIT être signalée comme telle (quickstart.md, « Vérifications non réalisables »)
- [ ] T025 Demander la validation humaine de l'écran d'accueil et des crédits avant intégration (principe VI de la constitution ; research.md, « Décisions validées »)

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)** : aucune dépendance, peut démarrer immédiatement
- **Foundational (Phase 2)** : dépend de Setup ; BLOQUE toutes les histoires, car T003 modifie `index.html`
- **User Stories (Phases 3 à 5)** : dépendent de Foundational. US1 est le MVP ; US2 et US3 peuvent suivre dans l'ordre de priorité
- **Polish (Phase N)** : dépend de la fin de toutes les histoires souhaitées

### User Story Dependencies

- **US1 (P1)** : dépend de Foundational uniquement
- **US2 (P2)** : dépend de Foundational ; ne modifie aucun fichier, sa vérification porte sur l'ensemble du dépôt
- **US3 (P3)** : dépend de Foundational ; T018 modifie `index.html` et doit donc s'exécuter après les tâches de US1 qui modifient le même fichier

### Within Each User Story

- Tâches sur `index.html` : séquentielles, dans l'ordre des numéros
- Vérifications : après la modification qu'elles contrôlent
- Story terminée avant de passer à la suivante en ordre de priorité

### Parallel Opportunities

- T001 et T002 peuvent s'exécuter en parallèle (Setup)
- T016 et T017 peuvent s'exécuter en parallèle (US2, lecture seule)
- T020 et T021 peuvent s'exécuter en parallèle (US3, lecture seule)
- Les histoires US2 et US3 peuvent être travaillées en parallèle après Foundational, en réservant `index.html` à une seule personne à la fois

---

## Parallel Example: User Story 2

```bash
# Vérifications en lecture seule, exécutables ensemble :
Task: "T016 Vérifier avec git diff --name-only que bkcore/ et launch.js ne sont pas modifiés"
Task: "T017 Comparer le HUD en course avec formation/tests/captures/02-jeu-en-cours.png"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Terminer Phase 1 : Setup (T001, T002)
2. Terminer Phase 2 : Foundational (T003, T004)
3. Terminer Phase 3 : US1 (T005 à T015)
4. **STOP et VALIDER** : vérifier US1 indépendamment (quickstart.md, scénarios 2, 3 et 6)
5. Présenter l'écran d'accueil pour validation humaine

### Incremental Delivery

1. Setup + Foundational : fondations prêtes
2. US1 : écran d'accueil ACME, validé indépendamment (MVP)
3. US2 : vérification de l'absence de changement du gameplay
4. US3 : crédits et écrans secondaires
5. Polish : régression QUnit, documentation et validation humaine

---

## Notes

- Les tâches `[P]` portent sur des fichiers différents ou sont en lecture seule
- Les tâches `[Story]` relient chaque tâche à son histoire pour la traçabilité
- Chaque histoire est vérifiable indépendamment à son checkpoint
- Ne jamais modifier `bkcore/`, `bkcore.coffee/`, `libs/`, `launch.js` ou `launch.coffee` dans ce ticket
- Ne pas consulter `../etapes/` (CLAUDE.md)
- Éviter : tâches vagues, conflits sur `index.html` entre tâches parallèles, dépendances entre histoires qui cassent leur indépendance
