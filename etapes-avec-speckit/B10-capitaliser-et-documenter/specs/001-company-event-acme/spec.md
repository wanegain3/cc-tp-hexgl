# Feature Specification: Personnalisation événementielle ACME

**Feature Branch**: `001-company-event-acme`

**Created**: 2026-11-11

**Status**: Draft

**Input**: User description: « Spécifie le besoin décrit dans formation/tickets/company-event.md. Les documents d'identité visuelle sont dans formation/identite/. Appuie-toi sur docs/architecture.md. »

## Clarifications

### Session 2026-11-12

- Q: Faut-il désactiver le suivi Google Analytics existant (identifiant `UA-26274524-4`, celui de HexGL) pendant l'événement ACME ? → A: Oui, désactiver le suivi pendant l'événement.
- Q: Faut-il conserver la mention d'origine de HexGL dans l'écran des crédits, en plus de la mention ACME ? → A: Oui, conserver les crédits HexGL et ajouter une mention ACME.
- Q: Dans quelle langue l'écran d'accueil doit-il afficher le message d'accueil et le bouton de démarrage ? → A: Français, selon la charte (« Démarrer la course »).
- Q: L'habillage ACME doit-il remplacer définitivement l'écran d'accueil de HexGL, ou doit-il pouvoir être désactivé pour revenir à l'écran d'origine ? → A: Remplacement permanent, annulable par un retour Git.
- Q: Quelle largeur minimale d'écran l'écran d'accueil doit-il supporter sans défilement horizontal ? → A: 360 px.

## User Scenarios & Testing *(mandatory)*

ACME souhaite utiliser HexGL lors d'un événement d'entreprise. Le jeu doit présenter l'identité visuelle de l'entreprise dès son écran d'accueil, sans que la course elle-même change. La référence graphique est la charte interne « Communication digital événement » (septembre 2026), dont le logo officiel fourni pour l'exercice.

### User Story 1 - Accueil aux couleurs d'ACME (Priority: P1)

Un participant arrive sur le jeu et voit un écran d'accueil qui identifie immédiatement ACME : le logo officiel, le message d'accueil « Bienvenue au ACME Racing Challenge », et une palette sombre aux accents violet, bleu électrique et cyan issus de la charte. Il peut lancer la course par le bouton « Démarrer la course ».

**Why this priority**: C'est le seul élément demandé explicitement par le ticket et le cœur de la personnalisation. Sans lui, l'événement n'est pas identifiable comme celui d'ACME.

**Independent Test**: Ouvrir le jeu sur l'écran d'accueil et vérifier, par comparaison avec la charte, la présence du logo, du message d'accueil et des couleurs. Cette story peut être validée seule, sans lancer de course.

**Acceptance Scenarios**:

1. **Étant donné** que le jeu est ouvert sur l'écran d'accueil, **lorsque** le participant regarde l'écran, **alors** le logo ACME est visible, complet, sans déformation, et lisible sur le fond.
2. **Étant donné** que le jeu est ouvert sur l'écran d'accueil, **lorsque** le participant lit le message principal, **alors** il lit « Bienvenue au ACME Racing Challenge ».
3. **Étant donné** que le jeu est ouvert sur l'écran d'accueil, **lorsque** le participant observe les couleurs, **alors** le fond est sombre et les accents sont violet, bleu électrique et cyan.
4. **Étant donné** que le jeu est ouvert sur l'écran d'accueil, **lorsque** le participant choisit « Démarrer la course », **alors** le parcours de démarrage existant reprend sans changement.

---

### User Story 2 - Course inchangée (Priority: P2)

Une fois la course lancée, le participant retrouve exactement le même gameplay qu'avant la personnalisation : mêmes contrôles, mêmes règles de course, même HUD en course, même déroulement du compte à rebours, des tours et de la fin de course.

**Why this priority**: Le ticket impose que le gameplay reste inchangé. Cette story garantit que la personnalisation n'a pas d'effet de bord sur l'expérience de jeu, ce qui est critique pour un événement.

**Independent Test**: Lancer une course avec la configuration de référence avant et après la personnalisation, et comparer les comportements observables (contrôles, HUD, compte à rebours, tours, fin de course).

**Acceptance Scenarios**:

1. **Étant donné** qu'une course est lancée, **lorsque** le participant utilise les contrôles, **alors** le vaisseau réagit comme avant la personnalisation.
2. **Étant donné** qu'une course est en cours, **lorsque** le HUD s'affiche, **alors** les éléments (vitesse, bouclier, temps, tours, messages) sont identiques à la version de référence.
3. **Étant donné** qu'une course se termine, **lorsque** le résultat s'affiche, **alors** le temps et le déroulement sont identiques à la version de référence.

---

### User Story 3 - Écrans secondaires sobres (Priority: P3)

Les écrans secondaires (aide des contrôles, chargement, fin de course, crédits) restent fonctionnels et lisibles avec la nouvelle identité, sans que leur contenu ou leur navigation ne change.

**Why this priority**: Le ticket cible l'écran d'accueil. Les autres écrans ne doivent pas être cassés par la personnalisation, mais leur habillage n'est pas demandé.

**Independent Test**: Parcourir le menu, l'aide, le chargement, la fin de course et les crédits, et vérifier qu'aucun élément n'est illisible ou désaligné.

**Acceptance Scenarios**:

1. **Étant donné** que le participant ouvre les crédits, **lorsque** la page s'affiche, **alors** elle reste lisible, les mentions HexGL d'origine sont inchangées et la mention ACME est ajoutée (FR-012).
2. **Étant donné** que le participant arrive sur l'écran d'aide des contrôles, **lorsque** la page s'affiche, **alors** l'image d'aide correspondant au contrôle choisi est présente.

---

### Edge Cases

- Que se passe-t-il si le logo ne peut pas être affiché ? Le message d'accueil et le bouton « Démarrer la course » restent utilisables ; l'écran ne doit pas être bloqué.
- Que se passe-t-il sur un écran de 360 px de large (plus petit téléphone courant) ? Le logo, le message et le bouton restent entièrement visibles, sans défilement horizontal.
- Que se passe-t-il si le participant revient sur l'écran d'accueil après une course ? L'identité ACME est affichée de la même façon qu'au premier lancement.
- Le logo fourni a un fond blanc. Sur fond sombre, il DOIT être présenté dans une zone claire qui le sépare du fond, afin de rester lisible et sans altération.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: L'écran d'accueil DOIT afficher le logo ACME officiel fourni, sans déformation ni recoloration, avec une zone de respiration autour de lui.
- **FR-002**: L'écran d'accueil DOIT afficher le message « Bienvenue au ACME Racing Challenge » comme message principal.
- **FR-003**: L'écran d'accueil DOIT présenter un fond principal sombre : le visuel de fond d'origine est recouvert d'une couche sombre. Les accents sont le violet (#7628FF), le bleu électrique (#315BFF) et le cyan (#00C4E8) de la charte.
- **FR-004**: L'écran d'accueil DOIT afficher un bouton de démarrage « Démarrer la course », fonctionnel et identifiable, qui lance le parcours de démarrage existant.
- **FR-005**: Le vaisseau et le logo « HexGL by BKcore » NE DOIVENT PLUS apparaître sur l'écran d'accueil ; l'identité principale de cet écran est celle d'ACME. Ce remplacement est permanent ; le retour à l'écran d'origine se fait par retour Git, sans bascule dans le jeu.
- **FR-006**: La personnalisation NE DOIT PAS modifier le gameplay, les contrôles, la physique du vaisseau, les règles de course ni le HUD en course.
- **FR-007**: La personnalisation NE DOIT PAS ajouter d'effet visuel complexe ; le rendu DOIT rester sobre.
- **FR-008**: Les écrans secondaires (aide, chargement, fin de course, crédits) DOIVENT rester fonctionnels et lisibles.
- **FR-009**: L'écran d'accueil DOIT rester utilisable sur un écran de 360 px de large, sans défilement horizontal.
- **FR-010**: La personnalisation NE DOIT PAS introduire de ressource distante (hors du projet), de police distante ni de dépendance logicielle. Le seul fichier ajouté est le logo, copié dans le projet.
- **FR-011**: Le suivi analytics existant (Google Analytics) DOIT être retiré de la page ; aucune donnée de participant NE DOIT être envoyée vers un compte d'analyse. Ce retrait reste effectif jusqu'à un retour Git, donc au-delà de la durée de l'événement.
- **FR-012**: L'écran des crédits DOIT conserver la mention d'origine de HexGL et DOIT ajouter une mention ACME, sans supprimer aucune mention existante.

### Key Entities *(include if feature involves data)*

- **Identité événementielle** : ensemble de l'habillage ACME (logo, message d'accueil, palette). Elle ne contient aucune donnée de course.
- **Charte graphique** : source de vérité pour les couleurs, le logo et les règles d'usage (`formation/identite/charte_graphique_acme.pdf`).

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Une validation humaine confirme qu'un participant identifie ACME comme organisateur de l'événement dès l'écran d'accueil, sans aide.
- **SC-002**: Sur l'écran d'accueil, les couleurs des textes, boutons et accents d'interface sont uniquement celles de la charte (violet, bleu électrique, cyan, blanc). Le logo et le visuel de fond, qui ne sont pas des éléments d'interface, sont exclus de ce critère.
- **SC-003**: Les contrôles et le HUD en course sont identiques à la version de référence, et aucun fichier de la boucle de jeu n'est modifié. Une course complète n'est exigée que si le gameplay risque d'être affecté.
- **SC-004**: L'écran d'accueil reste entièrement visible et utilisable sur un écran de 360 px de large, sans défilement horizontal.
- **SC-005**: Le logo est reconnaissable, complet et sans déformation sur l'écran d'accueil, validé par comparaison avec le logo officiel.

## Assumptions

- Le ticket cible l'écran d'accueil uniquement. Les autres écrans ne reçoivent qu'un ajustement minimal s'il est nécessaire pour rester lisibles.
- « Nom de l'entreprise » désigne « ACME », tel qu'il apparaît dans la charte et le logo.
- Le message d'accueil de référence est celui de la charte, « Bienvenue au ACME Racing Challenge », et ce texte est retenu tel quel.
- Le logo officiel de l'exercice est celui fourni dans `formation/identite/logo.png`. Ce fichier ayant un fond blanc, il est présenté sur une zone claire sur l'écran d'accueil.
- Les autres libellés du menu (« Controls », « Quality », « Credits ») restent en anglais ; leur traduction fera l'objet d'un ticket séparé.
- La typographie existante est conservée, conformément à la charte pour une intégration dans l'existant.
- Les contrôles, les règles de course, le HUD en course et les textes de gameplay ne sont pas concernés par la personnalisation.
- Les tests de validation ne figent pas l'habillage de marque, car il est destiné à être modifié pour d'autres événements.
- Le visuel de fond d'origine est conservé et recouvert d'une couche sombre. Les fichiers du vaisseau et du logo HexGL restent dans le dépôt.
- Aucun service externe nouveau, aucun backend et aucune étape de construction ne sont nécessaires.
