---
colorSchema: dark
transition: fade-out
mdc: true
layout: center
glowSeed: 4
timer: countdown
duration: 30min
hideInToc: true
title: Stack Big Data - Architecture & Pratiques
drawings:
  persist: false
comark: true
---

![](../Logo_Osmose_Horizontal_VF.png){.w-90.mt--10.mb-5}

---
src: ./pages/perso.md
---

---
src: ./pages/cover.md
---

---
src: ./pages/intro-uber.md
---

---
layout: section
---

# 1. Le parcours de la donnée

*D'où elle vient, où elle va*

---

<DataSources />

<!--
  Idée clé : montrer la diversité des sources avant de parler de solutions, ancrée sur Uber.
  Structuré (paiements, comptes) vs non structuré (GPS, logs, APIs) — formats et volumes très différents.
  Enchaîne sur DataJourney : toutes ces sources convergent vers la Collecte.
  Temps estimé : 2 min
-->

---

<DataJourney />

<!--
  Idée clé : on reprend l'exemple Uber de l'intro — une demande de course traverse toute la stack.
  À chaque clic, la donnée avance et change d'état : ping GPS → flux → stockée → matching/ETA → voiture assignée.
  Rappel parfait : le public a déjà vu ces chiffres en intro, on leur montre maintenant le "comment".
  Temps estimé : 3 min
-->

---
layout: section
---

# 2. Les limites du monde classique

*Pourquoi une seule machine ne suffit plus*

---
layout: center
---

# La limite d'une seule machine

<div class="grid grid-cols-3 gap-6 mt-8 text-center">

<v-click>
<div class="p-6 bg-gray-800 rounded-xl">
  <div class="text-4xl mb-3">🖥️</div>
  <div class="font-bold text-lg mb-2">CPU</div>
  <div class="text-gray-400 text-sm">Un nombre fixe de cœurs.<br/>Le calcul parallèle a une limite physique.</div>
</div>
</v-click>

<v-click>
<div class="p-6 bg-gray-800 rounded-xl">
  <div class="text-4xl mb-3">💾</div>
  <div class="font-bold text-lg mb-2">RAM</div>
  <div class="text-gray-400 text-sm">Si les données dépassent la mémoire,<br/>tout s'arrête.</div>
</div>
</v-click>

<v-click>
<div class="p-6 bg-gray-800 rounded-xl">
  <div class="text-4xl mb-3">💿</div>
  <div class="font-bold text-lg mb-2">Disque</div>
  <div class="text-gray-400 text-sm">Quelques To au maximum.<br/>1 panne = perte de données.</div>
</div>
</v-click>

</div>

<v-click>
<div class="mt-8 text-center text-xl text-yellow-400">
  ⚠️ Une machine, c'est un point de défaillance unique (SPOF)
</div>
</v-click>

<!--
  Idée clé : le hardware a des limites physiques. On ne peut pas toujours ajouter de RAM ou de CPU indéfiniment.
  Analogie : un seul livreur qui ne peut pas porter plus de 20 kg — quoi qu'il arrive.
  Temps estimé : 2 min
-->

---
layout: default
---

<ScaleComparison />

<!--
  Idée clé : le scale horizontal est la réponse du Big Data au mur physique.
  Analogie : plutôt que d'embaucher un super-employé, on recrute 10 personnes qui collaborent.
  Temps estimé : 3 min
-->

---

# Les 3 défis du Big Data

<div class="grid grid-cols-3 gap-6 mt-8">

<v-click>
<div class="p-6 bg-gray-800 rounded-xl border border-blue-500">
  <div class="text-5xl text-center mb-4">📦</div>
  <h3 class="text-blue-400 font-bold text-xl text-center mb-3">Volume</h3>
  <ul class="text-sm text-gray-300 space-y-1">
    <li>Des To, voire des Po de données</li>
    <li>Impossible à stocker sur une seule machine</li>
    <li>→ <b>Stockage distribué</b></li>
  </ul>
</div>
</v-click>

<v-click>
<div class="p-6 bg-gray-800 rounded-xl border border-purple-500">
  <div class="text-5xl text-center mb-4">⚡</div>
  <h3 class="text-purple-400 font-bold text-xl text-center mb-3">Streaming</h3>
  <ul class="text-sm text-gray-300 space-y-1">
    <li>Des milliers d'événements par seconde</li>
    <li>Le batch ne suffit plus</li>
    <li>→ <b>Traitement en temps réel</b></li>
  </ul>
</div>
</v-click>

<v-click>
<div class="p-6 bg-gray-800 rounded-xl border border-green-500">
  <div class="text-5xl text-center mb-4">🗄️</div>
  <h3 class="text-green-400 font-bold text-xl text-center mb-3">Stockage</h3>
  <ul class="text-sm text-gray-300 space-y-1">
    <li>Données structurées ET non structurées</li>
    <li>Formats variés, durée de vie longue</li>
    <li>→ <b>Data Lake / Lakehouse</b></li>
  </ul>
</div>
</v-click>

</div>

<!--
  Idée clé : les 3V classiques du Big Data traduits en problèmes concrets.
  Analogie : 3 canaux qui débordent en même temps — chacun nécessite une réponse différente.
  Temps estimé : 2 min
-->

---
layout: section
---

# 3. Les concepts fondamentaux

*Comprendre avant de choisir un outil*

---

<MasterWorker />

<!--
  Idée clé : diviser pour mieux régner — le principe de base de tout système distribué.
  Analogie : un chef de chantier et ses ouvriers. Le chef ne pose pas les briques, il coordonne.
  Temps estimé : 3 min
-->

---

<FailoverDemo />

<!--
  Idée clé : un système distribué est conçu pour survivre aux pannes, pas pour les éviter.
  Analogie : si un ouvrier tombe malade, le chef de chantier redistribue ses tâches aux autres.
  Temps estimé : 3 min
-->

---

<Partitioning />

<!--
  Idée clé : le partitionnement est la clé du parallélisme — sans lui, pas de performance distribuée.
  Temps estimé : 3 min
-->

---
layout: section
---

# 4. Les solutions & outils

*Les bons outils pour les bons problèmes*

---

# Stockage distribué

<div class="grid grid-cols-3 gap-4 mt-6">

<v-click>
<div class="p-4 bg-gray-800 rounded-xl border-l-4 border-yellow-500">
  <div class="text-2xl mb-2">🐘 HDFS</div>
  <div class="text-yellow-400 font-bold text-sm mb-2">Hadoop Distributed File System</div>
  <div class="text-xs text-gray-300 space-y-1">
    <div>• Stockage distribué sur commodity hardware</div>
    <div>• Réplication x3 par défaut</div>
    <div>• Conçu pour les gros fichiers séquentiels</div>
    <div class="text-yellow-300 mt-2">📍 On-premise, clusters Hadoop</div>
  </div>
</div>
</v-click>

<v-click>
<div class="p-4 bg-gray-800 rounded-xl border-l-4 border-orange-500">
  <div class="text-2xl mb-2">☁️ Object Storage</div>
  <div class="text-orange-400 font-bold text-sm mb-2">S3, GCS, Azure Blob</div>
  <div class="text-xs text-gray-300 space-y-1">
    <div>• Stockage cloud quasi illimité</div>
    <div>• Pas de serveur à gérer</div>
    <div>• Paiement à l'usage</div>
    <div class="text-orange-300 mt-2">📍 Cloud-native, standard actuel</div>
  </div>
</div>
</v-click>

<v-click>
<div class="p-4 bg-gray-800 rounded-xl border-l-4 border-blue-500">
  <div class="text-2xl mb-2">🏗️ Formats ouverts</div>
  <div class="text-blue-400 font-bold text-sm mb-2">Delta Lake, Apache Iceberg</div>
  <div class="text-xs text-gray-300 space-y-1">
    <div>• Transactions ACID sur fichiers</div>
    <div>• Versioning & time travel</div>
    <div>• Parquet comme format de base</div>
    <div class="text-blue-300 mt-2">📍 Lakehouse pattern</div>
  </div>
</div>
</v-click>

</div>

<v-click>
<div class="mt-6 p-3 bg-gray-900 rounded-lg text-sm">
  <span class="text-green-400 font-bold">Tendance actuelle :</span>
  <span class="text-gray-300"> HDFS cède la place à l'Object Storage (S3/GCS) + formats Iceberg/Delta Lake → le pattern <b>Lakehouse</b></span>
</div>
</v-click>

<!--
  Idée clé : le stockage distribué a évolué du on-premise vers le cloud.
  Temps estimé : 3 min
-->

---

# Moteurs d'exécution distribués

<div class="grid grid-cols-2 gap-6 mt-4">

<v-click>
<div class="p-5 bg-gray-800 rounded-xl border-t-4 border-orange-500">
  <div class="flex items-center gap-3 mb-3">
    <span class="text-3xl">⚡</span>
    <div>
      <div class="font-bold text-lg">Apache Spark</div>
      <div class="text-orange-400 text-xs">Moteur batch & micro-batch</div>
    </div>
  </div>
  <ul class="text-sm text-gray-300 space-y-1">
    <li>• Traitement in-memory → très rapide</li>
    <li>• APIs Python (PySpark), SQL, Scala</li>
    <li>• Batch, streaming, ML, Graph</li>
    <li>• Standard de facto du Big Data</li>
  </ul>
  <div class="mt-3 p-2 bg-orange-900 rounded text-xs">
    <b>Chez Uber :</b> recalculer chaque nuit les stats de toutes les courses (revenus, trajets, zones)
  </div>
</div>
</v-click>

<v-click>
<div class="p-5 bg-gray-800 rounded-xl border-t-4 border-blue-500">
  <div class="flex items-center gap-3 mb-3">
    <span class="text-3xl">🌊</span>
    <div>
      <div class="font-bold text-lg">Apache Flink</div>
      <div class="text-blue-400 text-xs">Moteur streaming natif</div>
    </div>
  </div>
  <ul class="text-sm text-gray-300 space-y-1">
    <li>• Streaming temps réel natif (low latency)</li>
    <li>• Gestion avancée du temps (event time)</li>
    <li>• Exactement-une-fois garanti</li>
    <li>• Idéal pour les pipelines critiques</li>
  </ul>
  <div class="mt-3 p-2 bg-blue-900 rounded text-xs">
    <b>Chez Uber :</b> ajuster le prix dynamique (surge) en temps réel selon l'offre et la demande
  </div>
</div>
</v-click>

</div>

<v-click>
<div class="mt-4 p-3 bg-gray-900 rounded text-sm text-center">
  <span class="text-yellow-300">Spark ≈ camion benne</span> — gros volumes, traitement différé &nbsp;|&nbsp;
  <span class="text-blue-300">Flink ≈ ambulance</span> — faible latence, temps réel critique
</div>
</v-click>

<!--
  Idée clé : Spark pour le batch à grande échelle, Flink pour le vrai temps réel.
  Temps estimé : 3 min
-->

---

# Ingestion & Streaming

<div class="grid grid-cols-2 gap-6 mt-4">

<v-click>
<div class="p-5 bg-gray-800 rounded-xl border-t-4 border-red-500">
  <div class="flex items-center gap-3 mb-3">
    <span class="text-3xl">📨</span>
    <div>
      <div class="font-bold text-lg">Apache Kafka</div>
      <div class="text-red-400 text-xs">Bus de messages distribué</div>
    </div>
  </div>
  <ul class="text-sm text-gray-300 space-y-1">
    <li>• File d'attente distribuée et persistante</li>
    <li>• Millions de messages/seconde</li>
    <li>• Découple producteurs et consommateurs</li>
    <li>• Rétention configurable (rejouer l'historique)</li>
  </ul>
  <div class="mt-3 p-2 bg-red-900 rounded text-xs">
    <b>Chez Uber :</b> collecter en continu les pings GPS et les événements de l'app de millions de chauffeurs
  </div>
</div>
</v-click>

<v-click>
<div class="p-5 bg-gray-800 rounded-xl border-t-4 border-purple-500">
  <div class="flex items-center gap-3 mb-3">
    <span class="text-3xl">🔌</span>
    <div>
      <div class="font-bold text-lg">Airbyte / NiFi</div>
      <div class="text-purple-400 text-xs">Connecteurs & ETL</div>
    </div>
  </div>
  <ul class="text-sm text-gray-300 space-y-1">
    <li>• Connecteurs clé en main (300+ sources)</li>
    <li>• Synchronisation BDD → Data Lake</li>
    <li>• Interface visuelle, low-code</li>
    <li>• Gestion des erreurs et retries</li>
  </ul>
  <div class="mt-3 p-2 bg-purple-900 rounded text-xs">
    <b>Chez Uber :</b> synchroniser la base des paiements et des comptes vers le data lake toutes les heures
  </div>
</div>
</v-click>

</div>

<v-click>
<div class="mt-4 p-3 bg-gray-900 rounded text-sm text-center text-gray-300">
  <b class="text-red-400">Kafka</b> = autoroute à données (haut débit, temps réel) &nbsp;|&nbsp;
  <b class="text-purple-400">Airbyte</b> = camion de déménagement (migration, synchronisation)
</div>
</v-click>

---

# Orchestration

<div class="grid grid-cols-2 gap-6 mt-4">

<v-click>
<div class="p-5 bg-gray-800 rounded-xl border-t-4 border-green-500">
  <div class="flex items-center gap-3 mb-3">
    <span class="text-3xl">🌀</span>
    <div>
      <div class="font-bold text-lg">Apache Airflow</div>
      <div class="text-green-400 text-xs">Planificateur de workflows</div>
    </div>
  </div>
  <ul class="text-sm text-gray-300 space-y-1">
    <li>• DAG (Directed Acyclic Graph) en Python</li>
    <li>• Planification, retry, alertes</li>
    <li>• Très répandu, grande communauté</li>
    <li>• Interface web de monitoring</li>
  </ul>
  <div class="mt-3 p-2 bg-green-900 rounded text-xs">
    <b>Chez Uber :</b> piloter le pipeline nocturne — agréger les courses → recalculer les zones de demande → notifier
  </div>
</div>
</v-click>

<v-click>
<div class="p-5 bg-gray-800 rounded-xl border-t-4 border-teal-500">
  <div class="flex items-center gap-3 mb-3">
    <span class="text-3xl">💎</span>
    <div>
      <div class="font-bold text-lg">Dagster</div>
      <div class="text-teal-400 text-xs">Data-asset orchestrator</div>
    </div>
  </div>
  <ul class="text-sm text-gray-300 space-y-1">
    <li>• Centré sur les assets (données) plutôt que les tâches</li>
    <li>• Observabilité native (lineage, metadata)</li>
    <li>• Typage fort, tests intégrés</li>
    <li>• Moderne, en forte adoption</li>
  </ul>
  <div class="mt-3 p-2 bg-teal-900 rounded text-xs">
    <b>Chez Uber :</b> remonter à la source d'un ETA erroné dans un pipeline complexe
  </div>
</div>
</v-click>

</div>

<v-click>
<div class="mt-4 p-3 bg-gray-900 rounded text-sm text-center text-gray-300">
  <b class="text-green-400">Airflow</b> = planificateur de tâches éprouvé &nbsp;|&nbsp;
  <b class="text-teal-400">Dagster</b> = observabilité et qualité data-first
</div>
</v-click>

---

<StackRecap />

<!--
  Idée clé : chaque outil a sa place dans le pipeline — pas de couteau suisse.
  On reprend visuellement le parcours du chapitre 1, rempli des outils vus dans la section.
  L'orchestration (Airflow/Dagster) est transversale : elle pilote tout.
  Temps estimé : 3 min
-->

---
layout: section
---

# 5. Retours d'expérience

*Ce qu'on apprend sur le terrain*

---

# Ce qu'on sous-estime au départ

<div class="space-y-4 mt-6">

<v-click>
<div class="p-4 bg-gray-800 rounded-xl flex gap-4 items-start">
  <span class="text-3xl">🕐</span>
  <div>
    <div class="font-bold text-yellow-400">La latence de bout en bout</div>
    <div class="text-sm text-gray-300 mt-1">On optimise le traitement Spark mais on oublie l'ingestion, le scheduling Airflow et le refresh du dashboard. Le tableau de bord "temps réel" des courses met en fait 15 minutes à se rafraîchir.</div>
  </div>
</div>
</v-click>

<v-click>
<div class="p-4 bg-gray-800 rounded-xl flex gap-4 items-start">
  <span class="text-3xl">💸</span>
  <div>
    <div class="font-bold text-yellow-400">Le coût du stockage cloud</div>
    <div class="text-sm text-gray-300 mt-1">Un ping GPS toutes les 4 s × des millions de chauffeurs : S3 semble gratuit… jusqu'au premier rapport de facturation. Requêtes, transferts et snapshots Iceberg explosent sans gouvernance.</div>
  </div>
</div>
</v-click>

<v-click>
<div class="p-4 bg-gray-800 rounded-xl flex gap-4 items-start">
  <span class="text-3xl">📖</span>
  <div>
    <div class="font-bold text-yellow-400">La documentation des données</div>
    <div class="text-sm text-gray-300 mt-1">6 mois après le lancement, personne ne sait plus ce que signifie le champ <code>surge_multiplier_v2_final</code>. Un catalogue de données (DataHub, OpenMetadata) n'est pas optionnel.</div>
  </div>
</div>
</v-click>

</div>

---

# Les pièges classiques

<div class="grid grid-cols-2 gap-4 mt-6">

<v-click>
<div class="p-4 bg-red-950 rounded-xl border border-red-700">
  <div class="font-bold text-red-400 mb-2">🏗️ Over-engineering</div>
  <div class="text-sm text-gray-300">Déployer Kafka + Flink + Kubernetes pour suivre les 200 chauffeurs d'une seule ville. Commencer simple, scaler si besoin.</div>
</div>
</v-click>

<v-click>
<div class="p-4 bg-red-950 rounded-xl border border-red-700">
  <div class="font-bold text-red-400 mb-2">🏝️ Silos de données</div>
  <div class="text-sm text-gray-300">Chaque équipe construit son propre pipeline. Résultat : 5 définitions différentes du "nombre de courses" selon le département.</div>
</div>
</v-click>

<v-click>
<div class="p-4 bg-red-950 rounded-xl border border-red-700">
  <div class="font-bold text-red-400 mb-2">💳 Dette technique</div>
  <div class="text-sm text-gray-300">Des scripts Spark non testés en production. 1 an plus tard, personne n'ose y toucher. Le refactoring coûte 3x le développement initial.</div>
</div>
</v-click>

<v-click>
<div class="p-4 bg-red-950 rounded-xl border border-red-700">
  <div class="font-bold text-red-400 mb-2">🎯 Mauvais problème résolu</div>
  <div class="text-sm text-gray-300">Stocker tous les trajets depuis 2010 pour finalement ne requêter que les 30 derniers jours. Le bon outil pour le bon besoin.</div>
</div>
</v-click>

</div>

<!--
  Idée clé : les erreurs les plus fréquentes ne sont pas techniques mais organisationnelles.
  Temps estimé : 3 min
-->

---

# Les bons réflexes

<div class="space-y-3 mt-6">

<v-click>
<div class="p-4 bg-green-950 rounded-xl border border-green-700 flex gap-3 items-start">
  <span class="text-2xl">🎯</span>
  <div>
    <div class="font-bold text-green-400">Start simple, scale later</div>
    <div class="text-sm text-gray-300">Commencer par du batch + S3 + Spark. Ajouter du streaming uniquement quand le besoin métier est prouvé.</div>
  </div>
</div>
</v-click>

<v-click>
<div class="p-4 bg-green-950 rounded-xl border border-green-700 flex gap-3 items-start">
  <span class="text-2xl">📐</span>
  <div>
    <div class="font-bold text-green-400">Data contracts dès le départ</div>
    <div class="text-sm text-gray-300">Définir le schéma, le propriétaire et le SLA de chaque flux avant de coder. Évite 80% des bugs de production.</div>
  </div>
</div>
</v-click>

<v-click>
<div class="p-4 bg-green-950 rounded-xl border border-green-700 flex gap-3 items-start">
  <span class="text-2xl">🔍</span>
  <div>
    <div class="font-bold text-green-400">Observabilité & monitoring</div>
    <div class="text-sm text-gray-300">Monitorer les volumes, les latences et la fraîcheur des données. Un pipeline silencieux qui ne produit rien est pire qu'un pipeline en erreur.</div>
  </div>
</div>
</v-click>

<v-click>
<div class="p-4 bg-green-950 rounded-xl border border-green-700 flex gap-3 items-start">
  <span class="text-2xl">🤝</span>
  <div>
    <div class="font-bold text-green-400">Aligner technique et métier</div>
    <div class="text-sm text-gray-300">La meilleure stack Big Data est celle qui répond à une vraie question métier. Partir du besoin, pas de la technologie.</div>
  </div>
</div>
</v-click>

</div>

---
layout: section
---

# 6. Conclusion

---
layout: center
---

# Les idées à retenir

<div class="grid grid-cols-1 gap-2.5 mt-4 max-w-3xl mx-auto">

<v-click>
<div class="p-3 bg-gray-800/70 rounded-lg border-l-4 border-blue-500 flex gap-3 items-center">
  <span class="text-xs uppercase tracking-widest text-blue-300 font-bold w-20 shrink-0">Pourquoi</span>
  <span>Le Big Data répond à un <b>problème physique</b> : une seule machine a des limites — Uber ne tient pas sur un PostgreSQL.</span>
</div>
</v-click>

<v-click>
<div class="p-3 bg-gray-800/70 rounded-lg border-l-4 border-purple-500 flex gap-3 items-center">
  <span class="text-xs uppercase tracking-widest text-purple-300 font-bold w-20 shrink-0">Comment</span>
  <span>Une seule réponse : <b>distribuer</b>. Master/Worker, réplication, partitionnement — les 3 piliers.</span>
</div>
</v-click>

<v-click>
<div class="p-3 bg-gray-800/70 rounded-lg border-l-4 border-green-500 flex gap-3 items-center">
  <span class="text-xs uppercase tracking-widest text-green-300 font-bold w-20 shrink-0">Outils</span>
  <span><b>S3 + Iceberg + Spark</b> reste la stack de référence ; Kafka/Flink pour le temps réel, Airflow pour piloter.</span>
</div>
</v-click>

<v-click>
<div class="p-3 bg-gray-800/70 rounded-lg border-l-4 border-yellow-500 flex gap-3 items-center">
  <span class="text-xs uppercase tracking-widest text-yellow-300 font-bold w-20 shrink-0">Réflexe</span>
  <span><b>Start simple, scale later</b> — la complexité se mérite, elle ne se déploie pas par défaut.</span>
</div>
</v-click>

</div>

<v-click>
<div class="mt-6 text-center text-xl">
  L'outil le plus important reste celui qui <b class="text-blue-300">résout votre vrai problème</b>.
</div>
</v-click>

---
layout: center
---

# Merci ! 🙏

<div class="mt-4 text-center text-lg text-gray-300">
  Des questions ? Discutons-en.
</div>

<div class="mt-10 flex items-center justify-center gap-6 text-gray-400">
  <div class="flex items-center gap-2">
    <span class="text-blue-300">🎙️</span> Guillaume Gandon
  </div>
  <div class="w-px h-5 bg-gray-600"></div>
  <div class="flex items-center gap-2">
    <span class="text-blue-300">💼</span> Lead Data Engineer — Osmose
  </div>
</div>

---
preload: false
---

<ViteEco />
