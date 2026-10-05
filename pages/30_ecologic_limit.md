---
layout: title-content-slide
label: Limite Ecologique
title: L'optimisation est une illusion
clicks: 2
---

::Card
<div class="arg-head">
  <span class="arg-title">L'optimisation de l'IA réduira son impact</span>
</div>

- Les modèles gagnent en efficacité : l'exemple présenté sera peut-être un jour réalisable par un modèle 32 Go qui tourne en local.
- Pour rappel, certains disaient que la qualité des sorties n'atteindrait jamais celle d'un développeur senior.
::

<div v-click="1" class="rebuttal-label">Mais</div>

:::div{.grid .grid-cols-2 .gap-4}
::CardOutline{v-click="1"}
<div class="arg-head">
  <span class="arg-title">Pratique déjà obsolète</span>
</div>

- Deux générations de modèles de retard.
- La mode des « loops » : réalisation d'un compilateur par IA autonome en continu pour 20 000 euros de tokens (janvier 2026).
::

::CardOutline{v-click="2"}
<div class="arg-head">
  <span class="arg-title">Effet rebond</span>
</div>

- Chaque gain d'efficacité est réinvesti en usage : on peut facilement lancer 100 agents en parallèle sur la même tâche et 10 agents chargés de sélectionner la meilleure réponse.
::
:::

<style>
.arg-head {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-bottom: 0.4rem;
}
.arg-title {
  font-size: 1.15rem;
  font-weight: 700;
  color: var(--text-strong);
}
.rebuttal-label {
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--orange);
  padding-left: 1.5rem;
}
.slidev-layout .card-md ul { font-size: 0.85rem; }
.slidev-layout .card-md ul li { margin-bottom: 0.15rem; }
</style>
