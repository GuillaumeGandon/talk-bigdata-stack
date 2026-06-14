<template>
  <div class="pt-wrap">

    <header class="pt-head">
      <div class="eyebrow">⚡ Diviser pour traiter en parallèle</div>
      <h2>Partitionnement &amp; Parallélisme</h2>
    </header>

    <div class="body">

      <!-- Schéma -->
      <div class="diagram">

        <div class="dataset">📦 Dataset <small>1 To de données</small></div>

        <svg class="links" viewBox="0 0 600 50" preserveAspectRatio="none">
          <line v-for="(x, i) in [75, 225, 375, 525]" :key="i"
                x1="300" y1="0" :x2="x" y2="46" />
        </svg>

        <!-- Partitions + workers -->
        <div class="lanes">
          <div v-for="(p, i) in parts" :key="i" v-click="i + 1" class="lane">
            <div class="part">🗂️ {{ p.label }}<small>{{ p.size }}</small></div>
            <div class="connector"><span class="line"></span><span class="head"></span></div>
            <div class="worker">
              👷 {{ p.worker }}
              <div class="progress"><span></span></div>
            </div>
          </div>
        </div>

        <div v-click="5" class="parallel-tag">⏱️ Traitement <b>simultané</b> — temps divisé par N</div>
      </div>

      <!-- Cartes explicatives -->
      <div class="cards">
        <div v-click="6" class="card pizza">
          📦 <b>Analogie</b>
          <span>Inventaire d'un entrepôt : au lieu d'une personne qui compte tous les rayons, on répartit les allées dans toute l'équipe — chacun compte la sienne en même temps, puis on additionne.</span>
        </div>
        <div v-click="7" class="card practice">
          📐 <b>En pratique</b>
          <span>Spark partitionne automatiquement et distribue chaque partition à un worker.</span>
        </div>
        <div v-click="8" class="card warn">
          ⚠️ <b>Le juste milieu</b>
          <span>Trop peu = goulot d'étranglement. Trop = overhead de coordination.</span>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup>
const parts = [
  { label: 'Partition 1', size: '~100 Go', worker: 'Worker 1' },
  { label: 'Partition 2', size: '~100 Go', worker: 'Worker 2' },
  { label: 'Partition 3', size: '~100 Go', worker: 'Worker 3' },
  { label: '… × N', size: 'reste', worker: 'Worker N' },
]
</script>

<style scoped>
.pt-wrap {
  display: flex;
  flex-direction: column;
  height: 100%;
  gap: 0.5rem;
  font-family: system-ui;
}

.pt-head { text-align: center; }

.eyebrow {
  font-size: 0.72rem;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: #c4b5fd;
  font-weight: 600;
}

h2 {
  font-size: 1.6rem;
  font-weight: 700;
  margin: 0.1rem 0 0;
}

.body {
  flex: 1;
  min-height: 0;
  display: grid;
  grid-template-columns: 1.4fr 1fr;
  gap: 1.2rem;
  align-items: center;
}

/* ── Schéma ── */
.diagram {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.2rem;
}

.dataset {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: linear-gradient(135deg, #8b5cf6, #6d28d9);
  color: #fff;
  font-weight: 700;
  font-size: 0.95rem;
  padding: 0.45rem 1.4rem;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
}

.dataset small,
.part small {
  font-weight: 400;
  font-size: 0.6rem;
  opacity: 0.85;
}

.links { width: 90%; height: 26px; }

.links line {
  stroke: #8b5cf6;
  stroke-width: 1.5;
  stroke-dasharray: 5;
  animation: flow 0.7s linear infinite;
}

.lanes {
  display: flex;
  gap: 0.7rem;
}

.lane {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.15rem;
}

.part {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #1e293b;
  border: 1px solid #475569;
  border-radius: 8px;
  padding: 0.3rem 0.5rem;
  font-size: 0.7rem;
  font-weight: 600;
  color: #e2e8f0;
  text-align: center;
}

.connector { display: flex; flex-direction: column; align-items: center; }

.connector .line {
  width: 2px;
  height: 10px;
  background: repeating-linear-gradient(to bottom, #3b82f6 0 4px, transparent 4px 8px);
  animation: flowv 0.7s linear infinite;
}

.connector .head {
  width: 0; height: 0; margin-top: -2px;
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 6px solid #3b82f6;
}

.worker {
  background: linear-gradient(135deg, #3b82f6, #1d4ed8);
  color: #fff;
  font-weight: 700;
  font-size: 0.68rem;
  padding: 0.3rem 0.5rem;
  border-radius: 8px;
  text-align: center;
  min-width: 4.6rem;
}

.progress {
  margin-top: 4px;
  height: 4px;
  border-radius: 2px;
  background: rgba(255, 255, 255, 0.25);
  overflow: hidden;
}

.progress span {
  display: block;
  height: 100%;
  background: #34d399;
  animation: fill 1.6s ease-in-out infinite;
}

.parallel-tag {
  margin-top: 0.5rem;
  font-size: 0.78rem;
  color: #6ee7b7;
  background: rgba(6, 95, 70, 0.4);
  border: 1px solid rgba(52, 211, 153, 0.4);
  padding: 0.35rem 0.8rem;
  border-radius: 10px;
}

/* ── Cartes ── */
.cards {
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
}

.card {
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
  padding: 0.55rem 0.8rem;
  border-radius: 10px;
  font-size: 0.82rem;
  line-height: 1.25;
}

.card span { font-size: 0.7rem; color: #cbd5e1; }

.card.pizza { background: rgba(30, 41, 59, 0.7); border: 1px solid rgba(148, 163, 184, 0.2); }
.card.practice { background: rgba(29, 78, 216, 0.35); border: 1px solid rgba(96, 165, 250, 0.4); }
.card.warn { background: rgba(120, 53, 15, 0.45); border: 1px solid rgba(251, 191, 36, 0.4); }
.card.warn span { color: #fde68a; }

@keyframes flow { to { stroke-dashoffset: -10; } }
@keyframes flowv { to { background-position-y: 8px; } }
@keyframes fill {
  0% { width: 0; }
  60% { width: 100%; }
  100% { width: 100%; }
}
</style>
