<template>
  <div class="fo-wrap">

    <header class="fo-head">
      <div class="eyebrow">🛡️ Tolérance aux pannes</div>
      <h2>Redondance &amp; Failover</h2>
    </header>

    <!-- Schéma -->
    <div class="diagram">

      <div class="master">👑 Master
        <small>surveille l'état du cluster</small>
      </div>

      <svg class="links" viewBox="0 0 600 60" preserveAspectRatio="none">
        <line x1="300" y1="0" x2="100" y2="56" />
        <line x1="300" y1="0" x2="300" y2="56" :class="{ dead: clicks >= 2 }" />
        <line x1="300" y1="0" x2="500" y2="56" />
      </svg>

      <div class="workers">
        <div class="worker">
          👷 Worker A
          <small>sain</small>
          <span v-click="3" class="badge up">+ charge</span>
        </div>

        <!-- Worker en panne -->
        <div class="worker" :class="{ down: clicks >= 2 }">
          👷 Worker B
          <small>{{ clicks >= 2 ? 'hors service' : 'sain' }}</small>
          <span v-click="2" class="badge dead">❌ panne</span>
          <span v-click="1" v-if="clicks < 2" class="heartbeat">💓</span>
        </div>

        <div class="worker">
          👷 Worker C
          <small>sain</small>
          <span v-click="3" class="badge up">+ charge</span>
        </div>
      </div>

      <div v-click="2" class="detect">
        💔 Heartbeat manquant → panne détectée → charge réassignée à A &amp; C
      </div>
    </div>

    <!-- Mécanismes -->
    <div class="cards">
      <div v-click="4" class="card">
        🔁 <b>Réplication</b>
        <span>Chaque donnée est copiée sur plusieurs nœuds</span>
      </div>
      <div v-click="5" class="card">
        💓 <b>Heartbeat</b>
        <span>Les nœuds s'envoient des signaux de vie réguliers</span>
      </div>
      <div v-click="6" class="card">
        ⚡ <b>Failover auto</b>
        <span>Le travail est réassigné sans intervention humaine</span>
      </div>
    </div>

  </div>
</template>

<script setup>
import { useSlideContext } from '@slidev/client'
import { computed } from 'vue'

const { $clicks } = useSlideContext()
const clicks = computed(() => $clicks.value)
</script>

<style scoped>
.fo-wrap {
  display: flex;
  flex-direction: column;
  height: 100%;
  gap: 0.5rem;
  font-family: system-ui;
}

.fo-head { text-align: center; }

.eyebrow {
  font-size: 0.72rem;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: #fca5a5;
  font-weight: 600;
}

h2 {
  font-size: 1.6rem;
  font-weight: 700;
  margin: 0.1rem 0 0;
}

/* ── Schéma ── */
.diagram {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex: 1;
  min-height: 0;
  justify-content: center;
  gap: 0;
}

.master,
.worker {
  display: flex;
  flex-direction: column;
  align-items: center;
  border-radius: 12px;
  color: #fff;
  font-weight: 700;
  padding: 0.5rem 1.2rem;
  text-align: center;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
}

.master small,
.worker small {
  font-weight: 400;
  font-size: 0.62rem;
  opacity: 0.85;
  margin-top: 2px;
}

.master {
  background: linear-gradient(135deg, #10b981, #047857);
  font-size: 0.95rem;
}

.workers {
  display: flex;
  gap: 1.6rem;
}

.worker {
  position: relative;
  background: linear-gradient(135deg, #3b82f6, #1d4ed8);
  font-size: 0.82rem;
  min-width: 7.5rem;
  transition: all 0.4s ease;
}

.worker.down {
  background: linear-gradient(135deg, #7f1d1d, #991b1b);
  filter: grayscale(0.3);
  opacity: 0.8;
}

.badge {
  position: absolute;
  top: -0.6rem;
  right: -0.6rem;
  font-size: 0.6rem;
  font-weight: 700;
  padding: 0.1rem 0.4rem;
  border-radius: 999px;
}

.badge.up { background: #f59e0b; color: #1c1917; }
.badge.dead { background: #ef4444; color: #fff; }

.heartbeat {
  position: absolute;
  top: -0.7rem;
  left: 50%;
  transform: translateX(-50%);
  font-size: 0.85rem;
  animation: beat 1s infinite;
}

/* ── Liens SVG ── */
.links {
  width: 70%;
  height: 32px;
}

.links line {
  stroke: #64748b;
  stroke-width: 1.5;
  stroke-dasharray: 5;
  animation: flow 0.7s linear infinite;
  transition: stroke 0.4s ease;
}

.links line.dead {
  stroke: #ef4444;
  stroke-dasharray: 3;
  opacity: 0.5;
  animation: none;
}

.detect {
  margin-top: 0.7rem;
  font-size: 0.8rem;
  color: #fca5a5;
  background: rgba(127, 29, 29, 0.4);
  border: 1px solid rgba(239, 68, 68, 0.4);
  padding: 0.4rem 0.8rem;
  border-radius: 10px;
  text-align: center;
}

/* ── Cartes mécanismes ── */
.cards {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 0.7rem;
}

.card {
  display: flex;
  flex-direction: column;
  gap: 0.15rem;
  padding: 0.5rem 0.7rem;
  border-radius: 10px;
  background: rgba(30, 41, 59, 0.6);
  border: 1px solid rgba(148, 163, 184, 0.18);
  font-size: 0.8rem;
  text-align: center;
}

.card span {
  font-size: 0.68rem;
  color: #94a3b8;
}

@keyframes flow {
  to { stroke-dashoffset: -10; }
}

@keyframes beat {
  0%, 100% { transform: translateX(-50%) scale(1); }
  50% { transform: translateX(-50%) scale(1.3); }
}
</style>
