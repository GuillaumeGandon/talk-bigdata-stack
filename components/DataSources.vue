<template>
  <div class="src-wrap">

    <header class="src-head">
      <div class="eyebrow">🧭 Avant le parcours — l'origine</div>
      <h2>D'où vient la donnée ?</h2>
      <p class="sub">Reprenons Uber : une seule course mobilise des dizaines de sources</p>
    </header>

    <div class="cols">

      <!-- STRUCTURÉ -->
      <div class="panel structured">
        <div class="panel-head">
          <span class="tag blue">🗄️ Structuré</span>
          <span class="hint">schéma fixe, lignes &amp; colonnes</span>
        </div>
        <div class="chips">
          <div v-click="1" class="chip">💳 <b>Paiements</b><small>transactions, factures</small></div>
          <div v-click="2" class="chip">👤 <b>Comptes</b><small>profils passagers &amp; chauffeurs</small></div>
          <div v-click="3" class="chip">🏢 <b>Référentiels</b><small>tarifs, zones, véhicules</small></div>
        </div>
      </div>

      <!-- NON STRUCTURÉ -->
      <div class="panel unstructured">
        <div class="panel-head">
          <span class="tag amber">🌀 Non structuré / semi</span>
          <span class="hint">volumineux, hétérogène, continu</span>
        </div>
        <div class="chips">
          <div v-click="4" class="chip">📍 <b>GPS temps réel</b><small>1 ping / ~4 s par chauffeur</small></div>
          <div v-click="5" class="chip">📱 <b>Événements app</b><small>clics, sessions, parcours</small></div>
          <div v-click="6" class="chip">📝 <b>Logs &amp; support</b><small>serveurs, avis, tickets</small></div>
          <div v-click="7" class="chip">🌐 <b>APIs externes</b><small>météo, trafic, cartes</small></div>
        </div>
      </div>

    </div>

    <div v-click="8" class="converge">
      Des sources éparpillées, dans des formats variés →
      <b class="hl">tout converge vers la Collecte</b>. Le défi : les agréger de façon fiable et cohérente.
    </div>

  </div>
</template>

<style scoped>
.src-wrap {
  display: flex;
  flex-direction: column;
  height: 100%;
  gap: 0.6rem;
  font-family: system-ui;
  justify-content: center;
}

.src-head { text-align: center; }

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

/* ── Colonnes ── */
.cols {
  display: grid;
  grid-template-columns: 1fr 1.25fr;
  gap: 1.1rem;
  align-items: start;
}

.panel {
  padding: 0.8rem 0.9rem;
  border-radius: 14px;
  background: rgba(30, 41, 59, 0.5);
  border: 1px solid rgba(148, 163, 184, 0.15);
}

.panel.structured { border-top: 3px solid #3b82f6; }
.panel.unstructured { border-top: 3px solid #f59e0b; }

.panel-head {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  margin-bottom: 0.6rem;
}

.tag {
  font-weight: 700;
  font-size: 0.95rem;
}
.tag.blue { color: #93c5fd; }
.tag.amber { color: #fcd34d; }

.hint {
  font-size: 0.62rem;
  color: #94a3b8;
}

/* ── Chips ── */
.chips {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.5rem;
}

.structured .chips { grid-template-columns: 1fr; }

.chip {
  display: flex;
  flex-direction: column;
  padding: 0.45rem 0.65rem;
  border-radius: 10px;
  background: rgba(15, 23, 42, 0.7);
  border: 1px solid rgba(148, 163, 184, 0.12);
  font-size: 0.82rem;
  line-height: 1.2;
}

.chip small {
  font-size: 0.62rem;
  color: #94a3b8;
  margin-top: 1px;
}

/* ── Convergence ── */
.converge {
  text-align: center;
  font-size: 0.85rem;
  color: #cbd5e1;
  background: rgba(30, 41, 59, 0.55);
  border: 1px solid rgba(148, 163, 184, 0.18);
  border-radius: 10px;
  padding: 0.55rem 1rem;
  margin: 0 1rem;
}

.converge .hl { color: #93c5fd; }
</style>
