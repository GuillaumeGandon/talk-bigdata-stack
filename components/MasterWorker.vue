<template>
  <div class="mw-wrap">

    <header class="mw-head">
      <div class="eyebrow">🧩 Pattern fondateur du distribué</div>
      <h2>Architecture Master / Worker</h2>
    </header>

    <!-- Schéma -->
    <div class="diagram">

      <!-- Master -->
      <div class="master">👑 Master
        <small>Orchestre · planifie · coordonne</small>
      </div>

      <!-- Connexions Master → Workers -->
      <svg class="links" viewBox="0 0 600 70" preserveAspectRatio="none">
        <line x1="300" y1="0" x2="100" y2="64" />
        <line x1="300" y1="0" x2="300" y2="64" />
        <line x1="300" y1="0" x2="500" y2="64" />
      </svg>

      <!-- Workers -->
      <div class="workers">
        <div v-click="1" class="worker">👷 Worker 1 <small>exécute une partie</small></div>
        <div v-click="1" class="worker">👷 Worker 2 <small>exécute une partie</small></div>
        <div v-click="1" class="worker">👷 Worker 3 <small>exécute une partie</small></div>
      </div>

      <!-- Connexions Workers → Résultat -->
      <svg class="links converge" viewBox="0 0 600 70" preserveAspectRatio="none">
        <line x1="100" y1="6" x2="300" y2="64" />
        <line x1="300" y1="6" x2="300" y2="64" />
        <line x1="500" y1="6" x2="300" y2="64" />
      </svg>

      <div v-click="2" class="result">📦 Résultat <small>agrégation des résultats partiels</small></div>
    </div>

    <!-- Rôles -->
    <div class="roles">
      <div v-click="3" class="role master-role">
        <b>👑 Master</b> — chef de chantier : découpe le travail, l'assigne, surveille l'avancement et collecte les résultats.
      </div>
      <div v-click="4" class="role worker-role">
        <b>👷 Worker</b> — ouvrier spécialisé : reçoit une tâche précise, l'exécute, renvoie le résultat au Master.
      </div>
    </div>

    <div v-click="5" class="spark">
      💡 En pratique : <b>Spark Driver</b> = Master · <b>Spark Executors</b> = Workers
    </div>

  </div>
</template>

<style scoped>
.mw-wrap {
  display: flex;
  flex-direction: column;
  height: 100%;
  gap: 0.5rem;
  font-family: system-ui;
}

.mw-head { text-align: center; }

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
.worker,
.result {
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
.worker small,
.result small {
  font-weight: 400;
  font-size: 0.62rem;
  opacity: 0.85;
  margin-top: 2px;
}

.master {
  background: linear-gradient(135deg, #8b5cf6, #6d28d9);
  font-size: 0.95rem;
  animation: pulse 2.4s infinite;
}

.workers {
  display: flex;
  gap: 1.4rem;
}

.worker {
  background: linear-gradient(135deg, #3b82f6, #1d4ed8);
  font-size: 0.82rem;
  min-width: 7.5rem;
}

.result {
  background: linear-gradient(135deg, #10b981, #047857);
  font-size: 0.9rem;
}

/* ── Liens SVG ── */
.links {
  width: 70%;
  height: 34px;
}

.links line {
  stroke: #64748b;
  stroke-width: 1.5;
  stroke-dasharray: 5;
  animation: flow 0.7s linear infinite;
}

.links.converge line { stroke: #34d399; }

/* ── Rôles ── */
.roles {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.7rem;
  font-size: 0.78rem;
}

.role {
  padding: 0.5rem 0.8rem;
  border-radius: 10px;
  line-height: 1.3;
}

.master-role { background: rgba(109, 40, 217, 0.45); border: 1px solid rgba(167, 139, 250, 0.4); }
.worker-role { background: rgba(29, 78, 216, 0.4); border: 1px solid rgba(96, 165, 250, 0.4); }

.spark {
  text-align: center;
  font-size: 0.82rem;
  color: #fcd34d;
  background: rgba(30, 41, 59, 0.6);
  padding: 0.45rem;
  border-radius: 10px;
}

@keyframes flow {
  to { stroke-dashoffset: -10; }
}

@keyframes pulse {
  0%   { box-shadow: 0 0 0 0 rgba(139, 92, 246, 0.55); }
  70%  { box-shadow: 0 0 0 14px rgba(139, 92, 246, 0); }
  100% { box-shadow: 0 0 0 0 rgba(139, 92, 246, 0); }
}
</style>
