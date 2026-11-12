# Tasks: Personnalisation événementielle ACME

**Input**: Design documents from `/specs/001-company-event-acme/`

**Prerequisites**: plan.md (requis), spec.md (requis pour les histoires utilisateur), research.md, data-model.md, quickstart.md

**Tests**: Aucune tâche d'écriture de tests n'est générée. Aucun code JavaScript n'est modifié, donc aucun nouveau test unitaire n'est pertinent (plan.md, principe V ; research.md, R9). La suite QUnit existante est exécutée à trois moments : après la Phase 2, après US1 et en fin de ticket (constitution, processus de vérification).

**Organization**: Les tâches sont regroupées par histoire utilisateur. Les vérifications navigateur sont regroupées dans une seule session en fin de ticket (T016), et non répétées par histoire.

**Périmètre des fichiers** : toutes les modifications passent par `index.html`. Les tâches qui le modifient s'exécutent donc séquentiellement et ne sont pas marquées `[P]`.

## Format: `[ID] [P?] [Story] Description`

- **[P]** : peut s'exécuter en parallèle (fichiers différents, aucune dépendance, ou lecture seule)
- **[Story]** : histoire utilisateur concernée (US1, US2, US3)
- Chemins de fichiers exacts dans chaque description

## Path Conventions

Projet statique à racine unique : `HexGL/`. Fichiers concernés : `HexGL/index.html` et `HexGL/css/acme-logo.png`.

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Préparer l'asset et établir la référence de régression avant toute modification.

- [ ] T001 [P] Copier `formation/identite/logo.png` vers `HexGL/css/acme-logo.png` sans aucune modification ; la taille doit rester identique à l'original (363 727 octets)
- [ ] T002 [P] Exécuter la suite QUnit depuis `HexGL/` (décrite dans `formation/tests/README.md`) avant toute modification et noter le code de retour de référence (attendu : `0`, TESTS_REUSSIS) ; noter aussi l'empreinte `sha256sum` de `docs/architecture.md` et de `CLAUDE.md` (depuis `HexGL/`), qui servira en T018

**Checkpoint** : asset présent et référence de régression connue.

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Retrait du suivi analytics, qui touche la page entière et n'appartient à aucune histoire utilisateur précise (FR-011, R4).

**⚠️ CRITICAL** : cette phase DOIT être terminée avant toute histoire utilisateur, car elle modifie `HexGL/index.html`.

- [ ] T003 Supprimer le bloc `<script type="text/javascript">` d'analytics (lignes 32 à 42 de `HexGL/index.html`, contenant `_gaq` et le chargement de `ga.js`) ; le suivi est « désactivé (bloc de suivi retiré) » selon data-model.md
- [ ] T004 Vérifier par recherche dans `HexGL/index.html` qu'aucune occurrence de `_gaq` ne subsiste et qu'aucune balise `<script>` ou `<link>` ne pointe vers un domaine nouveau (contrôle sans navigateur ; la vérification réseau est faite en T016)
- [ ] T005 Relancer la suite QUnit depuis `HexGL/` après la Phase 2 et comparer le code de retour à la référence de T002 (constitution, processus de vérification : après chaque phase significative)

**Checkpoint** : la page ne contient plus de suivi analytics externe, et la régression est vérifiée.

---

## Phase 3: User Story 1 - Accueil aux couleurs d'ACME (Priority: P1) 🎯 MVP

**Goal** : l'écran d'accueil présente le logo ACME officiel avec sa marge, le message « Bienvenue au ACME Racing Challenge », le bouton « Démarrer la course », un fond sombre et les couleurs de la charte.

**Independent Test** : vérifié dans la session navigateur unique de T016 (scénarios 2, 3 et 6 de `quickstart.md`).

- [ ] T006 [US1] Ajouter dans `<div id="title">` de `HexGL/index.html` une balise `<img id="acme-logo" src="css/acme-logo.png" alt="ACME">` ; le logo DOIT être affiché sans déformation, son ratio d'origine étant conservé, avec une marge autour de lui et sur la zone claire que forme son fond blanc (FR-001, cas limite de la spec)
- [ ] T007 [US1] Ajouter sous le logo, dans `<div id="title">` de `HexGL/index.html`, un élément `<p id="welcome">` dont le texte est exactement « Bienvenue au ACME Racing Challenge » (data-model.md : le message DOIT correspondre exactement au texte de la spec)
- [ ] T008 [US1] Remplacer le texte de `<div id="start">Start</div>` par « Démarrer la course » dans `HexGL/index.html` ; l'attribut `id="start"` DOIT être conservé, car `launch.js` le référence pour le clic
- [ ] T009 [US1] Dans le bloc `<style>` de `HexGL/index.html`, redéfinir le `background` de `#global` pour superposer une couche `rgba(8, 9, 13, 0.78)` au-dessus de l'image `css/bg.jpg` (le chemin DOIT être `css/bg.jpg` depuis `index.html`, et non `bg.jpg` comme dans `multi.css`) ; le fond principal DOIT être sombre (FR-003)
- [ ] T010 [US1] Dans le bloc `<style>` de `HexGL/index.html`, retirer l'image de fond de `#title` (`css/title.png`) pour que l'identité HexGL ne soit plus affichée ; le fichier `css/title.png` NE DOIT PAS être supprimé (FR-005)
- [ ] T011 [US1] Dans le bloc `<style>` de `HexGL/index.html`, appliquer la palette de la charte : texte de `#menu` en `#00C4E8` ; texte de `#start` en `#FFFFFF` sur un fond `linear-gradient(#7628FF, #315BFF)` ; texte de `#welcome` en `#FFFFFF`. `multi.css` définit `#start:hover` avec `!important` sur la couleur orange : la surcharge DOIT aussi utiliser `!important` pour que le survol reste dans la palette (research.md, R6)
- [ ] T012 [US1] Dans le bloc `<style>` de `HexGL/index.html`, ajouter `@media (max-width: 480px)` : `#acme-logo` avec `max-width: 90%`, taille de police réduite pour `#menu` et `#welcome`, sans débordement horizontal (research.md, R7 ; FR-009 : « utilisable sur un écran de 360 px de large, sans défilement horizontal »)
- [ ] T013 [US1] Relancer la suite QUnit depuis `HexGL/` après les modifications de US1 et comparer le code de retour à la référence de T002 (constitution : après chaque phase significative ; US1 modifie l'écran d'accueil)

**Checkpoint** : User Story 1 est implémentée. Sa vérification navigateur est faite dans T016.

---

## Phase 4: User Story 2 - Course inchangée (Priority: P2)

**Goal** : le gameplay, les contrôles, les règles de course et le HUD en course restent identiques à la version de référence.

**Independent Test** : aucun fichier de la boucle de jeu n'est modifié (T014). Le HUD est vérifié dans T016 (scénario 7).

- [ ] T014 [P] [US2] Vérifier avec `git status --short -- bkcore bkcore.coffee libs launch.js launch.coffee` (depuis `HexGL/`) qu'aucun fichier de ces chemins n'est modifié ou ajouté ; `git diff` seul ignorerait les fichiers non suivis (FR-006 ; plan.md, principe I ; SC-003)

**Checkpoint** : aucun fichier de la boucle de jeu n'est modifié. Aucune course complète n'est exigée (R10).

---

## Phase 5: User Story 3 - Écrans secondaires sobres (Priority: P3)

**Goal** : les crédits conservent les mentions HexGL et ajoutent la mention ACME ; l'aide, le chargement et la fin de course restent fonctionnels et lisibles.

**Independent Test** : vérifié dans la session navigateur unique de T016 (scénarios 5 et 8 de `quickstart.md`).

- [ ] T015 [US3] Dans `<div id="credits">` de `HexGL/index.html`, conserver tout le bloc HexGL existant et insérer, avant `<h4>Click anywhere to continue.</h4>`, une section `<h3>` « Personnalisation événementielle ACME » suivie d'un `<p>` mentionnant ACME et la charte fournie (FR-012 ; research.md, R8)

**Checkpoint** : les crédits contiennent les mentions HexGL et ACME.

---

## Phase N: Polish & Cross-Cutting Concerns

**Purpose** : vérifications de fin de ticket, régression finale et validation humaine.

- [ ] T016 Exécuter, sur `HexGL/index.html`, une session navigateur unique à 1280 × 720 puis à 360 × 640, couvrant `quickstart.md` scénarios 2 à 8. Points à contrôler : (a) position du menu et animation de fond préservées, comparées à `formation/tests/captures/01-ecran-accueil.png`, vaisseau et logo HexGL absents (FR-005) ; (b) logo comparé à `formation/identite/logo.png`, sans déformation (SC-005) ; (c) accents « é » rendus (R5) ; (d) couleurs des textes, boutons et accents conformes à la palette, logo et fond exclus (SC-002) ; (e) aucun élément hors largeur à 360 px (FR-009, R7) ; (f) aucune requête vers `google-analytics.com` ni vers un domaine nouveau (FR-010, FR-011) ; (g) crédits : mentions HexGL et ACME (FR-012) ; (h) clic « Démarrer la course » vers l'aide, le chargement puis la course ; (i) HUD en course identique à `formation/tests/captures/02-jeu-en-cours.png` après démarrage, sans course complète (SC-003) ; (j) aucun effet visuel ajouté au-delà de la couche sombre (FR-007) ; (k) aide `step-2` et chargement `step-3` lisibles (FR-008) ; (l) si le logo ne charge pas, le texte « ACME » reste visible et le bouton utilisable, et le retour à l'accueil après une course est identique au premier lancement (cas limites de la spec). Playwright si disponible ; sinon, la session est manuelle et DOIT être signalée comme telle
- [ ] T017 Relancer la suite QUnit depuis `HexGL/` après l'ensemble des modifications et comparer le code de retour à la référence de T002 (quickstart.md, scénario 1). Si le code est `2`, la vérification DOIT être signalée comme impossible, et NON comme réussie
- [ ] T018 Recalculer `sha256sum` de `docs/architecture.md` et de `CLAUDE.md` (depuis `HexGL/`) et comparer aux empreintes notées en T002 : elles DOIVENT être identiques (constitution, principe IV ; `git status` ne suffit pas, ces fichiers étant non suivis), puis consigner dans le compte rendu de validation les vérifications réussies, échouées ou non réalisées (quickstart.md, « Vérifications non réalisables »)
- [ ] T019 Demander la validation humaine de l'écran d'accueil et des crédits avant intégration, en vérifiant que la personne validatrice identifie ACME sans aide (SC-001) (principe VI de la constitution ; `specs/001-company-event-acme/research.md`, section « Décisions validées »)

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)** : aucune dépendance, peut démarrer immédiatement
- **Foundational (Phase 2)** : dépend de Setup ; BLOQUE toutes les histoires, car T003 modifie `HexGL/index.html`
- **User Stories (Phases 3 à 5)** : dépendent de Foundational. US1 est le MVP
- **Polish (Phase N)** : dépend de la fin de toutes les histoires ; T016 est la session navigateur unique

### User Story Dependencies

- **US1 (P1)** : dépend de Foundational. Modifie `HexGL/index.html`
- **US2 (P2)** : dépend de Foundational ; T014 ne modifie aucun fichier
- **US3 (P3)** : dépend de Foundational ; T015 modifie `HexGL/index.html`

### Within Each User Story

- Tâches sur `HexGL/index.html` : séquentielles, dans l'ordre des numéros, quelle que soit l'histoire
- QUnit (T005, T013, T017) : après les modifications qu'elle contrôle
- Aucune vérification navigateur par histoire : elles sont regroupées dans T016

### Parallel Opportunities

- T001 et T002 peuvent s'exécuter en parallèle (Setup)
- T014 peut s'exécuter en parallèle de toute tâche sur `HexGL/index.html` (lecture seule)

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Terminer Phase 1 : Setup (T001, T002)
2. Terminer Phase 2 : Foundational (T003, T004, T005)
3. Terminer Phase 3 : US1 (T006 à T013)
4. Présenter l'écran d'accueil pour validation humaine après T016

### Incremental Delivery

1. Setup + Foundational : fondations prêtes, régression vérifiée
2. US1 : écran d'accueil ACME (MVP), régression vérifiée
3. US2 et US3 : vérification du gameplay et des crédits
4. Polish : session navigateur unique (T016), régression finale (T017), documentation et validation humaine (T018, T019)

---

## Notes

- Les tâches `[P]` portent sur des fichiers différents ou sont en lecture seule
- Les tâches `[Story]` relient chaque tâche à son histoire pour la traçabilité
- Ne jamais modifier `bkcore/`, `bkcore.coffee/`, `libs/`, `launch.js` ou `launch.coffee` dans ce ticket
- Ne pas consulter `../etapes/` (CLAUDE.md)
