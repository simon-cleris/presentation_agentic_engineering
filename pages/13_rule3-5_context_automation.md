---
layout: rule-slide
icon: i-carbon:data-structured
clicks: 8
zoom: 0.8
label: Règles 3 et 5
title: Automatiser la construction du contexte minimal
desc: Chargement incrémental (claude.md) et récupération autonome (skills)
---

<div class="grid grid-cols-[1fr_2fr] gap-6 items-center">
<div class="flex flex-col gap-3">

::Card{v-click="1"}

**Un fichier global toujours chargé**
::

::Card{v-click="3"}

**Un fichier chargé par sous-dossier**
::

::Card{v-click="5"}

**Générés par l'agent lui-même**
::

::Card{v-click="7"}

**Un contexte minimal reconstruit automatiquement à chaque tâche**
::

</div>

<IncrementalContext />
</div>



<!--
[click] Un fichier global toujours chargé :
- Lien (seulement le lien) vers l'index de la documentation (spécification fonctionnelle et technique)
- Règles globales du projet (le code contient lui-même nos préférences)
- Description en une ligne de chaque sous-dossier

[click] Il est chargé dans le contexte de l'agent dès le début de la tâche.

[click] Un fichier chargé par sous-dossier :
- Chargé automatiquement dès qu'un fichier du dossier est lu ou qu'une commande bash y est exécutée.
- Contient (entièrement) la spec fonctionnelle et les choix techniques des scripts du dossier (jamais de code)

[click] Ici le cd drivers/ (ou la lecture d'un fichier du dossier) charge le CLAUDE.md de drivers/, pas celui de modules/.

[click] Générés par l'agent lui-même :
- À partir de la documentation, avant toute implémentation (simplifie le contrôle)

[click] Le CLAUDE.md est généré depuis docs/, avant d'écrire le code.

[click] Ce système permet de vider le contexte entre chaque tâche. Le contexte minimal est reconstruit automatiquement (sans coût). Garantit la qualité et une faible utilisation en token (obligatoire pour notre limite d'utilisation).

[click] Tâche suivante : le contexte est vidé, le global et le CLAUDE.md de modules/ sont rechargés.
-->
