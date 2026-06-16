# Quiz — Big Data Stack 🎯

> 10 questions à projeter en fin de présentation (Kahoot, Wooclap, Slido…).
> Format : 4 réponses, **une seule correcte** (en gras ✅). Durée conseillée : 30 s/question.
> Niveau : **avancé** — tous les distracteurs sont plausibles, lis la justification 💡 après le vote.

---

### 1. Le scale **vertical** garde un intérêt aujourd'hui. Dans quel cas est-il le **meilleur** choix ?

- A. Pour ingérer 1 To de logs streaming par jour
- **B. Pour une charge qui tient sur une machine mais demande beaucoup de RAM contiguë (ex. grosse jointure en mémoire) ✅**
- C. Dès qu'on dépasse 3 utilisateurs simultanés
- D. Pour garantir la tolérance aux pannes

> 💡 Le distribué a un **coût** (réseau, coordination, complexité). Tant que ça tient sur une machine, le scale vertical est souvent plus rapide et plus simple. Le distribué se **mérite**.

---

### 2. Scale horizontal = « scalabilité quasi illimitée ». Quelle **limite réelle** apparaît quand on ajoute toujours plus de nodes ?

- A. Aucune, c'est vraiment illimité
- B. Le CPU de chaque node sature individuellement
- **C. Le coût de coordination/réseau croît : au-delà d'un point, ajouter un node ralentit l'ensemble ✅**
- D. Les disques deviennent trop rapides

> 💡 C'est l'esprit de la **loi d'Amdahl** : la partie non parallélisable (shuffle, synchronisation, coordination) finit par dominer. Doubler les machines ne double pas la performance.

---

### 3. Dans une architecture Master / Worker, pourquoi le **Master** est-il un point sensible ?

- A. Parce qu'il exécute la majorité des calculs
- B. Parce qu'il stocke physiquement toutes les données
- **C. Parce que sa panne peut paralyser la coordination : c'est un SPOF s'il n'est pas répliqué ✅**
- D. Parce qu'il est toujours plus lent que les Workers

> 💡 Le Master coordonne mais reste un **SPOF** potentiel. Les vrais systèmes le répliquent (élection de leader, ex. ZooKeeper/Raft) pour éviter qu'une seule panne arrête tout.

---

### 4. Un Worker ne répond plus au heartbeat, puis **revient** en réémettant ses résultats. Quel risque cela crée-t-il ?

- A. Aucun, un Worker ne revient jamais
- **B. Des doublons / double comptage si le traitement n'est pas idempotent (exactly-once) ✅**
- C. Le Master tombe en panne à son tour
- D. Les données sont automatiquement chiffrées

> 💡 Le failover réassigne le travail. Si le Worker « mort » revient, on peut traiter **deux fois** la même partition. D'où l'importance de l'**idempotence** et de la sémantique *exactly-once*.

---

### 5. Le partitionnement permet le parallélisme, mais quel **piège** classique fait s'effondrer la performance ?

- A. Avoir trop peu de données
- B. Utiliser des fichiers Parquet
- **C. Le data skew : une partition (ex. une clé « hot ») bien plus grosse que les autres devient le goulot ✅**
- D. Répliquer les partitions

> 💡 Si 90 % des données tombent dans une seule clé (ex. `country = US`), un Worker fait tout le travail pendant que les autres attendent. Le **skew** ruine le parallélisme malgré le partitionnement.

---

### 6. On migre de HDFS vers un **Lakehouse** (Object Storage + Iceberg/Delta). Qu'apportent surtout les formats ouverts par rapport à du Parquet « nu » sur S3 ?

- A. Une compression bien meilleure
- B. Le chiffrement des données au repos
- **C. Des transactions ACID, le versioning/time-travel et l'évolution de schéma sur des fichiers objet ✅**
- D. Un accès plus rapide au réseau

> 💡 Parquet stocke, mais ne gère ni transactions ni cohérence. **Iceberg/Delta** ajoutent une couche de métadonnées : ACID, time-travel, schema evolution — d'où le pattern Lakehouse.

---

### 7. Uber veut recalculer le **surge pricing** avec une latence < 1 s. Pourquoi privilégier Flink plutôt que Spark **Structured Streaming** ?

- A. Spark ne sait pas du tout faire de streaming
- **B. Flink est nativement event-at-a-time (vraie faible latence) ; Spark streaming reste basé sur des micro-batchs ✅**
- C. Flink est plus ancien et plus stable
- D. Spark ne tourne pas dans le cloud

> 💡 Spark **fait** du streaming, mais par **micro-batchs** (latence de l'ordre de la seconde). Flink traite **événement par événement** → latence plus basse, mieux adapté au temps réel critique.

---

### 8. Kafka est souvent appelé « bus de messages », mais qu'est-ce qui le distingue fondamentalement d'une file type RabbitMQ ?

- A. Kafka supprime le message dès qu'il est lu
- **B. C'est un log distribué persistant : les messages sont conservés et rejouables par plusieurs consumers indépendants ✅**
- C. Kafka ne gère qu'un seul consommateur à la fois
- D. Kafka stocke la donnée nettoyée finale du pipeline

> 💡 Kafka = **log immuable** partitionné, avec offsets. On peut **rejouer** l'historique et avoir plusieurs groupes de consumers. Une file classique supprime généralement le message après consommation.

---

### 9. Pourquoi un **orchestrateur** (Airflow/Dagster) n'est-il pas remplaçable par un simple `cron` qui lance les jobs ?

- A. Cron ne sait pas lire l'heure
- B. Cron coûte plus cher que l'orchestrateur
- **C. L'orchestrateur gère dépendances entre tâches, retries, backfill et observabilité du DAG ✅**
- D. Cron ne fonctionne pas dans le cloud

> 💡 Cron déclenche « à l'heure H », point. L'orchestration gère le **graphe de dépendances**, les reprises sur échec, les *backfills* historiques et le monitoring de bout en bout.

---

### 10. Une équipe veut « faire du Big Data » et déploie Kafka + Flink + Spark + Kubernetes pour **50 Go** de données. Quel est le vrai problème ?

- A. Il manque encore une base graph
- B. 50 Go, c'est trop pour ces outils
- **C. Over-engineering : ce volume tient sur une seule machine (DuckDB/Postgres) — la complexité distribuée n'est pas justifiée ✅**
- D. Ils auraient dû utiliser HDFS plutôt que S3

> 💡 50 Go tient en RAM/SSD d'un seul serveur. **Start simple, scale later** : la stack distribuée apporte ici surtout de la dette opérationnelle, pas de la valeur.

---

## 🏆 Barème indicatif (niveau avancé)
- 9–10 ✅ : Tu maîtrises les **pièges** du distribué, pas juste les définitions
- 6–8 ✅ : Solides fondations, quelques nuances à creuser
- 0–5 ✅ : Normal, ces questions visent les zones grises — les slides éclairent tout ça

## 📋 Pour Kahoot
Copier-coller chaque question + ses 4 réponses dans l'éditeur, cocher la bonne (✅).
Astuce : régler **30 s** partout — chaque distracteur est crédible et demande de la réflexion.
