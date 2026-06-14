<template>
  <div class="rc-wrap">

    <header class="rc-head">
      <div class="eyebrow">🧰 La stack en mouvement</div>
      <h2>Quel outil pour quel besoin ?</h2>
    </header>

    <svg class="map" viewBox="0 0 900 470" preserveAspectRatio="xMidYMid meet">
      <defs>
        <filter id="glow" x="-50%" y="-50%" width="200%" height="200%">
          <feGaussianBlur stdDeviation="3.5" result="b" />
          <feMerge>
            <feMergeNode in="b" />
            <feMergeNode in="SourceGraphic" />
          </feMerge>
        </filter>
        <linearGradient id="orchGrad" x1="0" y1="0" x2="1" y2="0">
          <stop offset="0" stop-color="#7c3aed" stop-opacity="0.15" />
          <stop offset="0.5" stop-color="#7c3aed" stop-opacity="0.45" />
          <stop offset="1" stop-color="#7c3aed" stop-opacity="0.15" />
        </linearGradient>
      </defs>

      <!-- Liens de pilotage (orchestration) -->
      <path v-for="(e, i) in pilotPaths" :key="'p' + i"
            :d="e.d" class="pilot-link" />

      <!-- Liens de données -->
      <path v-for="(e, i) in dataPaths" :key="'d' + i"
            :id="'edge' + i" :d="e.d" class="data-link"
            :style="{ stroke: e.color }" />

      <!-- Particules qui circulent -->
      <template v-for="(e, i) in dataPaths" :key="'fx' + i">
        <circle v-for="k in 3" :key="k" r="3.2" :fill="e.color" class="particle">
          <animateMotion :dur="e.dur" repeatCount="indefinite"
                         :begin="(i * 0.12 + (k - 1) * e.step) + 's'"
                         :path="e.d" />
        </circle>
      </template>

      <!-- Nœud orchestration -->
      <g class="node orch" filter="url(#glow)">
        <rect x="270" y="18" width="360" height="48" rx="12" />
        <text x="450" y="40" class="n-title">🌀 Orchestration</text>
        <text x="450" y="56" class="n-sub">Airflow · Dagster — pilote tout le pipeline</text>
      </g>

      <!-- Nœuds outils -->
      <g v-for="(n, key) in nodes" :key="key"
         class="node tool" :style="{ animationDelay: n.delay + 's' }"
         filter="url(#glow)">
        <rect :x="n.x - 64" :y="n.y - 26" width="128" height="52" rx="11"
              :style="{ stroke: n.color }" />
        <text :x="n.x" :y="n.y - 4" class="n-title">{{ n.label }}</text>
        <text :x="n.x" :y="n.y + 13" class="n-sub">{{ n.sub }}</text>
      </g>

      <!-- Étiquettes de couche -->
      <text v-for="(l, i) in layers" :key="'l' + i"
            :x="l.x" y="455" class="layer-label">{{ l.t }}</text>
    </svg>

    <div class="takeaway">
      💡 Pas de couteau suisse : <b>chaque outil = un besoin</b>. La valeur naît de l'assemblage.
    </div>

  </div>
</template>

<script setup>
import { computed } from 'vue'

const C = {
  ing: '#38bdf8',
  sto: '#34d399',
  pro: '#fbbf24',
  res: '#f472b6',
}

const nodes = {
  kafka:  { x: 120, y: 170, label: '📨 Kafka',   sub: 'streaming temps réel', color: C.ing, delay: 0.05 },
  airbyte:{ x: 120, y: 320, label: '🔌 Airbyte', sub: 'batch & connecteurs',  color: C.ing, delay: 0.15 },
  s3:     { x: 350, y: 170, label: '☁️ S3 · GCS', sub: 'data lake brut',       color: C.sto, delay: 0.30 },
  delta:  { x: 350, y: 320, label: '🏗️ Delta',    sub: 'lakehouse · Iceberg',  color: C.sto, delay: 0.40 },
  spark:  { x: 580, y: 130, label: '⚡ Spark',    sub: 'batch / ML',           color: C.pro, delay: 0.55 },
  flink:  { x: 580, y: 245, label: '🌊 Flink',    sub: 'streaming',            color: C.pro, delay: 0.65 },
  dbt:    { x: 580, y: 360, label: '🔧 dbt',      sub: 'transfos SQL',         color: C.pro, delay: 0.75 },
  bi:     { x: 800, y: 170, label: '📈 BI',       sub: 'Metabase · Tableau',   color: C.res, delay: 0.90 },
  api:    { x: 800, y: 320, label: '🔗 APIs',     sub: 'exposition app',       color: C.res, delay: 1.00 },
}

const layers = [
  { x: 120, t: 'INGESTION' },
  { x: 350, t: 'STOCKAGE' },
  { x: 580, t: 'TRAITEMENT' },
  { x: 800, t: 'RESTITUTION' },
]

const dataEdges = [
  ['kafka', 's3'], ['airbyte', 's3'], ['s3', 'delta'],
  ['delta', 'spark'], ['delta', 'flink'], ['delta', 'dbt'],
  ['spark', 'bi'], ['dbt', 'bi'], ['flink', 'api'], ['spark', 'api'],
]

const pilotEdges = [['kafka'], ['spark'], ['dbt']]

// Courbe horizontale entre bord droit de la source et bord gauche de la cible
function hCurve(a, b) {
  const sx = a.x + 64, sy = a.y
  const tx = b.x - 64, ty = b.y
  const dx = (tx - sx) * 0.5
  return `M ${sx} ${sy} C ${sx + dx} ${sy}, ${tx - dx} ${ty}, ${tx} ${ty}`
}

// Lien de pilotage : du bas de l'orchestration vers le haut du nœud
function pilotCurve(b) {
  const sx = 450, sy = 66
  const tx = b.x, ty = b.y - 26
  return `M ${sx} ${sy} C ${sx} ${(sy + ty) / 2}, ${tx} ${(sy + ty) / 2}, ${tx} ${ty}`
}

const dataPaths = computed(() =>
  dataEdges.map(([from, to]) => ({
    d: hCurve(nodes[from], nodes[to]),
    color: nodes[from].color,
    dur: (2.4 + Math.random() * 0.8).toFixed(2) + 's',
    step: 0.8,
  }))
)

const pilotPaths = computed(() =>
  pilotEdges.map(([to]) => ({ d: pilotCurve(nodes[to]) }))
)
</script>

<style scoped>
.rc-wrap {
  display: flex;
  flex-direction: column;
  height: 100%;
  gap: 0.3rem;
  font-family: system-ui;
  justify-content: center;
}

.rc-head { text-align: center; }

.eyebrow {
  font-size: 0.72rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: #93c5fd;
  font-weight: 600;
}

h2 {
  font-size: 1.7rem;
  font-weight: 700;
  margin: 0.05rem 0 0;
}

.map {
  flex: 1;
  min-height: 0;
  width: 100%;
}

/* ── Liens ── */
.data-link {
  fill: none;
  stroke-width: 2;
  opacity: 0.35;
}

.pilot-link {
  fill: none;
  stroke: #a78bfa;
  stroke-width: 1.5;
  stroke-dasharray: 4 5;
  opacity: 0.45;
  animation: dash 1.2s linear infinite;
}

@keyframes dash {
  to { stroke-dashoffset: -18; }
}

.particle {
  filter: drop-shadow(0 0 4px currentColor);
}

/* ── Nœuds ── */
.node rect {
  fill: rgba(15, 23, 42, 0.96);
  stroke-width: 1.8;
}

.node.orch rect {
  fill: url(#orchGrad);
  stroke: #a78bfa;
  stroke-width: 1.5;
}

.n-title {
  fill: #f1f5f9;
  font-size: 15px;
  font-weight: 700;
  text-anchor: middle;
}

.n-sub {
  fill: #94a3b8;
  font-size: 9.5px;
  text-anchor: middle;
}

.tool {
  animation: pop 0.5s cubic-bezier(.2, 1.4, .4, 1) both;
  transform-box: fill-box;
  transform-origin: center;
}

@keyframes pop {
  from { opacity: 0; transform: scale(0.6) translateY(8px); }
  to   { opacity: 1; transform: scale(1) translateY(0); }
}

.layer-label {
  fill: #475569;
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.15em;
  text-anchor: middle;
}

/* ── Takeaway ── */
.takeaway {
  text-align: center;
  font-size: 0.9rem;
  color: #fcd34d;
}
</style>
