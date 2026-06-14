<template>
  <div class="scale-wrap">

    <!-- Deux stratégies côte à côte -->
    <div class="cols">

      <!-- ───────────── SCALE VERTICAL ───────────── -->
      <div class="card vertical">
        <div class="eyebrow up">⬆️ Scale Up</div>
        <h2>Scale Vertical</h2>
        <p class="subtitle">Une machine, toujours plus puissante</p>

        <div class="diagram">
          <div class="server small">8 CPU · 32 Go · 2 To</div>
          <div class="connector"><span class="line"></span><span class="head"></span><span class="lbl">upgrade</span></div>
          <div class="server big">64 CPU · 512 Go · 20 To</div>
          <div class="connector ko-arrow"><span class="line"></span><span class="head"></span><span class="lbl">et après ?</span></div>
          <div class="wall">❌ Mur physique</div>
        </div>

        <ul class="facts">
          <li v-click class="ok">✅ Simple à gérer</li>
          <li v-click class="ok">✅ Pas de refonte applicative</li>
          <li v-click class="ko">❌ Coût exponentiel</li>
          <li v-click class="ko">❌ Limite physique incompressible</li>
          <li v-click class="ko">❌ Toujours un SPOF</li>
        </ul>
      </div>

      <!-- ───────────── SCALE HORIZONTAL ───────────── -->
      <div class="card horizontal">
        <div class="eyebrow out">➡️ Scale Out</div>
        <h2>Scale Horizontal</h2>
        <p class="subtitle">Plusieurs machines qui collaborent</p>

        <div class="diagram">
          <div class="nodes">
            <div class="node">M1</div>
            <div class="node">M2</div>
            <div class="node">M3</div>
            <div class="node dots">…N</div>
          </div>
          <div class="connector blue"><span class="line"></span><span class="head"></span><span class="lbl">coordination</span></div>
          <div class="coord">⚖️ Distribution de charge</div>
          <div class="connector blue"><span class="line"></span><span class="head"></span><span class="lbl">scale ∞</span></div>
          <div class="infinite">∞ scalabilité</div>
        </div>

        <ul class="facts">
          <li v-click class="ok">✅ Scalabilité quasi illimitée</li>
          <li v-click class="ok">✅ Tolérance aux pannes</li>
          <li v-click class="ok">✅ Coût maîtrisé (commodity hardware)</li>
          <li v-click class="ko">❌ Complexité accrue</li>
          <li v-click class="ko">❌ Nécessite des outils adaptés</li>
        </ul>
      </div>

    </div>

    <!-- Conclusion -->
    <div v-click class="takeaway">
      👉 Le Big Data fait le pari du <b>scale horizontal</b> : la puissance par le nombre.
    </div>

  </div>
</template>

<style scoped>
.scale-wrap {
  display: flex;
  flex-direction: column;
  height: 100%;
  gap: 0.6rem;
  font-family: system-ui;
}

.cols {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.2rem;
  flex: 1;
  min-height: 0;
}

.card {
  display: flex;
  flex-direction: column;
  padding: 0.9rem 1.1rem;
  border-radius: 14px;
  background: rgba(30, 41, 59, 0.5);
  border: 1px solid rgba(148, 163, 184, 0.18);
}

.card.horizontal {
  border-color: rgba(59, 130, 246, 0.45);
  box-shadow: 0 0 30px rgba(59, 130, 246, 0.12);
}

.eyebrow {
  font-size: 0.7rem;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  font-weight: 600;
}

.eyebrow.up { color: #fdba74; }
.eyebrow.out { color: #93c5fd; }

h2 {
  font-size: 1.5rem;
  font-weight: 700;
  margin: 0.1rem 0 0;
  line-height: 1.1;
}

.subtitle {
  font-size: 0.8rem;
  color: #94a3b8;
  margin: 0.1rem 0 0.6rem;
}

/* ── Diagrammes ── */
.diagram {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.25rem;
  margin-bottom: 0.7rem;
}

/* ── Flèches ── */
.connector {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.connector .line {
  width: 2px;
  height: 16px;
  background: repeating-linear-gradient(
    to bottom,
    #64748b 0 4px,
    transparent 4px 8px
  );
  animation: flow 0.8s linear infinite;
}

.connector .head {
  width: 0;
  height: 0;
  margin-top: -2px;
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  border-top: 7px solid #64748b;
}

.connector .lbl {
  position: absolute;
  left: 100%;
  top: 50%;
  transform: translateY(-50%);
  margin-left: 6px;
  white-space: nowrap;
  font-size: 0.62rem;
  color: #94a3b8;
}

.connector.blue .line {
  background: repeating-linear-gradient(to bottom, #3b82f6 0 4px, transparent 4px 8px);
}
.connector.blue .head { border-top-color: #3b82f6; }
.connector.blue .lbl { color: #93c5fd; }

.connector.ko-arrow .line {
  background: repeating-linear-gradient(to bottom, #ef4444 0 4px, transparent 4px 8px);
}
.connector.ko-arrow .head { border-top-color: #ef4444; }
.connector.ko-arrow .lbl { color: #fca5a5; }

@keyframes flow {
  to { background-position-y: 8px; }
}

.server {
  padding: 0.35rem 0.8rem;
  border-radius: 8px;
  font-size: 0.75rem;
  font-weight: 600;
  color: #fff;
  text-align: center;
}

.server.small { background: linear-gradient(135deg, #a16207, #854d0e); }
.server.big { background: linear-gradient(135deg, #ea580c, #c2410c); }

.wall {
  padding: 0.3rem 0.8rem;
  border-radius: 8px;
  font-size: 0.78rem;
  font-weight: 600;
  background: #7f1d1d;
  color: #fff;
}

.nodes {
  display: flex;
  gap: 0.4rem;
}

.node {
  width: 2.2rem;
  height: 2.2rem;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
  background: #1e293b;
  border: 1px solid #334155;
  font-size: 0.75rem;
  font-weight: 600;
  color: #e2e8f0;
}

.node.dots { color: #64748b; border-style: dashed; }

.coord {
  padding: 0.35rem 0.9rem;
  border-radius: 8px;
  font-size: 0.78rem;
  font-weight: 600;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: #fff;
}

.infinite {
  padding: 0.25rem 0.8rem;
  border-radius: 8px;
  font-size: 0.78rem;
  font-weight: 600;
  background: #065f46;
  color: #fff;
}

/* ── Listes pros/cons ── */
.facts {
  list-style: none;
  margin: auto 0 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
}

.facts li {
  padding: 0.18rem 0.5rem;
  border-radius: 6px;
  font-size: 0.68rem;
}

.facts .ok { background: rgba(22, 101, 52, 0.5); }
.facts .ko { background: rgba(127, 29, 29, 0.5); }

/* ── Conclusion ── */
.takeaway {
  text-align: center;
  font-size: 1.05rem;
  color: #93c5fd;
  padding: 0.5rem;
  border-top: 1px solid rgba(148, 163, 184, 0.2);
}
</style>
