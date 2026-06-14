<template>
  <div class="dj-wrap">

    <header class="dj-head">
      <div class="eyebrow">🧭 Le fil rouge</div>
      <h2>Le parcours de la donnée</h2>
      <p class="sub">Vous commandez un Uber 🚗 — suivez la donnée jusqu'à la voiture assignée</p>
    </header>

    <!-- La donnée qui voyage -->
    <div class="traveler-rail">
      <div
        class="traveler"
        :style="{ left: travelerLeft }"
        :class="stages[active]?.tone"
      >
        {{ stages[active]?.token ?? '📍 demande + GPS' }}
      </div>
    </div>

    <!-- Pipeline -->
    <div class="pipeline">
      <template v-for="(s, i) in stages" :key="i">
        <div
          class="stage"
          :class="[s.tone, { active: active === i, done: active > i }]"
          v-click="i + 1"
        >
          <div class="ico">{{ s.ico }}</div>
          <div class="name">{{ s.name }}</div>
          <div class="tools">{{ s.tools }}</div>
        </div>
        <div v-if="i < stages.length - 1" class="arrow" :class="{ lit: active > i }">›</div>
      </template>
    </div>

    <!-- Légende dynamique -->
    <div class="caption" :class="stages[active]?.tone">
      <span v-if="!started">👉 Cliquez : suivez votre demande de course traverser la stack.</span>
      <span v-else>{{ stages[active]?.caption }}</span>
    </div>

    <!-- Note batch / streaming -->
    <div v-click="6" class="modes">
      <span><b>⚡ Streaming</b> — la donnée file en continu (Kafka, Flink)</span>
      <span class="sep">·</span>
      <span><b>📦 Batch</b> — par lots réguliers (exports, Spark)</span>
    </div>

  </div>
</template>

<script setup>
import { useSlideContext } from '@slidev/client'
import { computed } from 'vue'

const { $clicks } = useSlideContext()

const stages = [
  { ico: '📥', name: 'Collecte',     tools: 'Kafka',               tone: 'blue',   token: '📍 demande + GPS',   caption: 'Collecte : la demande de course et les positions GPS sont captées en temps réel.' },
  { ico: '🔄', name: 'Transport',    tools: 'Streaming',           tone: 'violet', token: '📡 flux temps réel', caption: 'Transport : des milliers d\'événements/seconde acheminés en continu (streaming).' },
  { ico: '💾', name: 'Stockage',     tools: 'Data Lake · S3',      tone: 'green',  token: '🗃️ trajets bruts',   caption: 'Stockage : chaque course et chaque ping sont persistés à l\'échelle (des Po de données).' },
  { ico: '⚙️', name: 'Traitement',   tools: 'Spark · Flink',       tone: 'amber',  token: '🧮 matching + ETA',  caption: 'Traitement : matching passager ↔ chauffeur, calcul de l\'ETA et du prix dynamique.' },
  { ico: '📊', name: 'Restitution',  tools: 'API · ML',            tone: 'pink',   token: '🚗 voiture en 3 min', caption: 'Restitution : la voiture est assignée, l\'ETA s\'affiche dans votre app.' },
]

// index de l'étape active : reste sur la 1re tant qu'on n'a pas cliqué,
// puis avance avec les clics, borné à la dernière étape
const started = computed(() => $clicks.value > 0)

const active = computed(() =>
  Math.max(0, Math.min($clicks.value - 1, stages.length - 1))
)

const travelerLeft = computed(() => {
  const pct = (active.value + 0.5) / stages.length * 100
  return `${pct}%`
})
</script>

<style scoped>
.dj-wrap {
  display: flex;
  flex-direction: column;
  height: 100%;
  gap: 0.6rem;
  font-family: system-ui;
  justify-content: center;
}

.dj-head { text-align: center; }

.eyebrow {
  font-size: 0.72rem;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: #93c5fd;
  font-weight: 600;
}

h2 {
  font-size: 1.7rem;
  font-weight: 700;
  margin: 0.1rem 0 0;
}

.sub {
  font-size: 0.85rem;
  color: #94a3b8;
  margin: 0.15rem 0 0;
}

/* ── Donnée voyageuse ── */
.traveler-rail {
  position: relative;
  height: 2.2rem;
  margin: 0 1rem;
}

.traveler {
  position: absolute;
  top: 0;
  transform: translateX(-50%);
  padding: 0.3rem 0.8rem;
  border-radius: 999px;
  font-size: 0.8rem;
  font-weight: 700;
  color: #fff;
  white-space: nowrap;
  transition: left 0.5s cubic-bezier(.4, 1.3, .5, 1), background 0.4s ease;
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.35);
}

/* ── Pipeline ── */
.pipeline {
  display: flex;
  align-items: stretch;
  gap: 0.3rem;
  margin: 0 0.5rem;
}

.stage {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.1rem;
  padding: 0.7rem 0.4rem;
  border-radius: 12px;
  background: rgba(30, 41, 59, 0.5);
  border: 1px solid rgba(148, 163, 184, 0.15);
  opacity: 0.45;
  transform: translateY(0);
  transition: all 0.35s ease;
}

.stage.done { opacity: 0.85; }
.stage.active {
  opacity: 1;
  transform: translateY(-4px);
  box-shadow: 0 10px 26px rgba(0, 0, 0, 0.35);
}

.ico { font-size: 1.6rem; }
.name { font-weight: 700; font-size: 0.9rem; }
.tools { font-size: 0.62rem; color: #94a3b8; }

.arrow {
  align-self: center;
  font-size: 1.6rem;
  color: #475569;
  transition: color 0.35s ease;
}
.arrow.lit { color: #93c5fd; }

/* tones */
.blue.active   { border-color: #3b82f6; }
.violet.active { border-color: #8b5cf6; }
.green.active  { border-color: #10b981; }
.amber.active  { border-color: #f59e0b; }
.pink.active   { border-color: #ec4899; }

.traveler.blue   { background: linear-gradient(135deg, #3b82f6, #1d4ed8); }
.traveler.violet { background: linear-gradient(135deg, #8b5cf6, #6d28d9); }
.traveler.green  { background: linear-gradient(135deg, #10b981, #047857); }
.traveler.amber  { background: linear-gradient(135deg, #f59e0b, #b45309); }
.traveler.pink   { background: linear-gradient(135deg, #ec4899, #be185d); }

/* ── Légende ── */
.caption {
  text-align: center;
  font-size: 0.92rem;
  min-height: 1.4rem;
  color: #cbd5e1;
  padding: 0 1rem;
}
.caption.blue   { color: #93c5fd; }
.caption.violet { color: #c4b5fd; }
.caption.green  { color: #6ee7b7; }
.caption.amber  { color: #fcd34d; }
.caption.pink   { color: #f9a8d4; }

/* ── Modes ── */
.modes {
  text-align: center;
  font-size: 0.78rem;
  color: #94a3b8;
  background: rgba(30, 41, 59, 0.5);
  border-radius: 10px;
  padding: 0.45rem;
  margin: 0 2rem;
}
.modes .sep { margin: 0 0.6rem; opacity: 0.5; }
</style>
