<script setup>
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'

const { $clicks } = useSlideContext()

// Each card is revealed on its own click, the following click plays the matching animation step.
const step = computed(() => Math.floor($clicks.value / 2))

const tree = [
  { id: 'root', depth: 0, name: 'CLAUDE.md', note: 'global' },
  { id: 'docs', depth: 0, name: 'docs/', folder: true },
  { id: 'drivers', depth: 0, name: 'drivers/', folder: true },
  { id: 'drivers-doc', depth: 1, name: 'CLAUDE.md', owner: 'drivers' },
  { id: 'drivers-sub', depth: 1, name: 'protocols/', owner: 'drivers', folder: true },
  { id: 'drivers-file', depth: 1, name: 'spi_driver.py', owner: 'drivers', file: true },
  { id: 'modules', depth: 0, name: 'modules/', folder: true },
  { id: 'modules-doc', depth: 1, name: 'CLAUDE.md', owner: 'modules' },
  { id: 'modules-sub', depth: 1, name: 'planning/', owner: 'modules', folder: true },
  { id: 'modules-file', depth: 1, name: 'scanner.py', owner: 'modules', file: true },
]

const folderDoc = (folder, script, sub) => ({
  title: `${folder}/CLAUDE.md`,
  lines: [
    { tag: 'spec', text: `${script} : fonctionnel` },
    { tag: 'choix', text: `${script} : technique` },
    { tag: 'dossier', text: sub },
  ],
})

const contents = {
  root: {
    title: 'CLAUDE.md global',
    lines: [
      { tag: 'lien', text: 'docs/index.md' },
      { tag: 'règle', text: '500 lignes max par fichier' },
      { tag: 'règle', text: '…' },
      { tag: 'dossier', text: 'drivers/ : pilotes' },
      { tag: 'dossier', text: 'modules/ : métier' },
    ],
  },
  'drivers-doc': folderDoc('drivers', 'spi_driver.py', 'protocols/ : trames SPI'),
  'modules-doc': folderDoc('modules', 'scanner.py', 'planning/ : ordonnancement'),
}

// The first task works in drivers, the next one (after the context reset) in modules.
const task = computed(() => (step.value >= 4 ? 'modules' : 'drivers'))
const taskNumber = computed(() => (step.value >= 4 ? 2 : 1))
const generated = computed(() => step.value === 3)
const command = computed(() => (task.value === 'modules' ? 'cd modules/' : 'cd drivers/'))

const loaded = computed(() => {
  const ids = []
  if (step.value >= 1) ids.push('root')
  if (step.value >= 4) ids.push('modules-doc')
  else if (step.value >= 2) ids.push('drivers-doc')
  return ids
})

const footer = computed(() => {
  if (step.value === 3) return 'pipeline'
  if (step.value >= 2) return 'command'
  return null
})
</script>

<template>
  <div class="incremental">
    <div class="panels">
      <div class="panel">
        <div class="panel-label">PROJET</div>
        <div
          v-for="n in tree"
          :key="n.id"
          class="row"
          :class="{
            loaded: loaded.includes(n.id),
            source: generated && n.id === 'docs',
            dim: n.owner && n.owner !== task,
          }"
          :style="{ paddingLeft: `${n.depth * 1.25}rem` }"
        >
          <span class="icon" :class="n.folder ? 'i-carbon:folder' : 'i-carbon:document'" />
          <span>{{ n.name }}</span>
          <span v-if="n.note" class="note">{{ n.note }}</span>
          <span v-if="generated && n.name === 'CLAUDE.md'" class="badge">généré</span>
        </div>
      </div>

      <div class="flow" :class="{ active: step >= 1 }">
        <span class="i-carbon:arrow-right" />
      </div>

      <div class="panel context">
        <div class="panel-label">CONTEXTE · TÂCHE {{ taskNumber }}</div>
        <TransitionGroup name="chip" tag="div" class="chips">
          <div v-for="id in loaded" :key="id" class="chip">
            <div class="chip-title">{{ contents[id].title }}</div>
            <div v-for="l in contents[id].lines" :key="l.text" class="line">
              <span class="tag">{{ l.tag }}</span>
              <span class="text">{{ l.text }}</span>
            </div>
          </div>
        </TransitionGroup>
        <div v-if="!loaded.length" class="empty">vide</div>
        <div v-if="step >= 4" class="reset">contexte vidé puis reconstruit</div>
      </div>
    </div>

    <div class="footer">
      <Transition name="fade" mode="out-in">
        <div v-if="footer === 'command'" :key="command" class="terminal">
          <span class="prompt">$</span> {{ command }}
          <span class="hint">charge le CLAUDE.md du dossier</span>
        </div>
        <div v-else-if="footer === 'pipeline'" key="pipeline" class="pipeline">
          <span class="step">docs/</span>
          <span class="i-carbon:arrow-right" />
          <span class="step hot">CLAUDE.md</span>
          <span class="i-carbon:arrow-right" />
          <span class="step">code</span>
          <span class="hint">le CLAUDE.md est généré avant l'implémentation</span>
        </div>
      </Transition>
    </div>
  </div>
</template>

<style scoped>
.incremental { display: flex; flex-direction: column; gap: 0.75rem; width: 100%; }
.panels { display: flex; align-items: stretch; gap: 0.75rem; }
.panel { flex: 1; padding: 0.9rem 1rem; border-radius: 0.75rem; background: var(--card-bg); border: 1px solid var(--card-border-subtle); }
.panel-label { margin-bottom: 0.6rem; color: var(--orange); font-family: monospace; font-size: 0.75rem; letter-spacing: 0.08em; }
.row { display: flex; align-items: center; gap: 0.4rem; padding-top: 0.2rem; padding-bottom: 0.2rem; border-radius: 0.4rem; color: var(--text-muted); font-family: monospace; font-size: 0.85rem; transition: all 0.8s; }
.row.loaded { color: var(--text-strong); background: var(--orange-bg); box-shadow: inset 0 0 0 1px var(--orange-border); }
.row.source { color: var(--text-strong); box-shadow: inset 0 0 0 1px var(--orange-light); }
.row.dim { opacity: 0.35; }
.note { color: var(--text-faint); font-size: 0.75rem; }
.badge { margin-left: auto; padding: 0 0.4rem; border-radius: 0.6rem; background: var(--orange); color: var(--text-strong); font-family: sans-serif; font-size: 0.7rem; font-weight: 700; }
.flow { align-self: center; color: var(--text-faint); font-size: 1.5rem; transition: color 0.8s; }
.flow.active { color: var(--orange); }
.context { display: flex; flex-direction: column; }
.chips { display: flex; flex-direction: column; gap: 0.5rem; }
.chip { padding: 0.5rem 0.7rem; border-radius: 0.5rem; background: var(--orange-bg); border: 1px solid var(--orange); font-family: monospace; font-size: 0.8rem; }
.chip-title { margin-bottom: 0.3rem; color: var(--text-strong); font-weight: 700; }
.line { display: flex; gap: 0.5rem; padding: 0.1rem 0; color: var(--text-muted); }
.tag { min-width: 3.4rem; color: var(--orange-light); font-size: 0.7rem; }
.empty { color: var(--text-faint); font-style: italic; }
.reset { margin-top: auto; padding-top: 0.6rem; color: var(--orange-light); font-size: 0.8rem; font-style: italic; }
.footer { min-height: 2.6rem; }
.terminal, .pipeline { display: flex; align-items: center; gap: 0.6rem; padding: 0.6rem 0.9rem; border-radius: 0.6rem; background: var(--card-bg); border: 1px solid var(--card-border-subtle); color: var(--text-strong); font-family: monospace; font-size: 0.85rem; }
.prompt { color: var(--orange); }
.step { padding: 0.1rem 0.5rem; border-radius: 0.4rem; border: 1px solid var(--card-border-subtle); }
.step.hot { border-color: var(--orange); background: var(--orange-bg); }
.hint { margin-left: auto; color: var(--text-faint); font-family: sans-serif; font-size: 0.75rem; font-style: italic; }
.chip-enter-active, .chip-leave-active, .fade-enter-active, .fade-leave-active { transition: all 0.8s; }
/* On the context reset the old chip leaves first, then the new one comes in. */
.chip-enter-active { transition-delay: 0.8s; }
.chip-enter-from, .chip-leave-to { opacity: 0; transform: translateX(-1rem); }
.fade-enter-from, .fade-leave-to { opacity: 0; }
</style>
