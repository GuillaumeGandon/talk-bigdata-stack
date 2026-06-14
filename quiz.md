# Quiz — Big Data Stack 🎯

> 10 questions à projeter en fin de présentation (Kahoot, Wooclap, Slido…).
> Format : 4 réponses, **une seule correcte** (en gras ✅). Durée conseillée : 20 s/question.
> La colonne « 💡 » donne la justification à lire à voix haute après le vote.

---

### 1. Pourquoi une base de données classique ne suffit-elle plus à l'échelle d'Uber ?

- A. Le SQL est trop lent par nature
- **B. Une seule machine a des limites physiques de CPU, RAM et disque ✅**
- C. Les bases relationnelles ne savent pas stocker de chiffres
- D. Parce que le cloud est obligatoire depuis 2020

> 💡 Le Big Data naît d'un mur **physique** : au-delà d'un certain volume/débit, on ne peut plus « grossir » une seule machine.

---

### 2. Quelle est la différence entre scale **vertical** et scale **horizontal** ?

- A. Vertical = plus de machines, horizontal = une plus grosse machine
- **B. Vertical = une machine plus puissante, horizontal = plusieurs machines qui collaborent ✅**
- C. Ce sont deux noms pour la même chose
- D. Vertical concerne le stockage, horizontal le réseau

> 💡 Le Big Data fait le pari du **scale horizontal** : la puissance par le nombre, pas par la taille.

---

### 3. Dans une architecture Master / Worker, quel est le rôle du Master ?

- A. Il exécute lui-même tous les calculs
- B. Il stocke toutes les données
- **C. Il découpe, distribue le travail et collecte les résultats ✅**
- D. Il sert uniquement de sauvegarde

> 💡 Le Master = chef de chantier : il **coordonne**, il ne pose pas les briques. (Chez Spark : Driver = Master, Executors = Workers.)

---

### 4. Comment un système distribué détecte-t-il qu'un Worker est tombé en panne ?

- A. Un administrateur le vérifie manuellement
- **B. Par l'absence de signal de vie régulier (heartbeat) ✅**
- C. Le Worker envoie un email avant de tomber
- D. Il ne le détecte pas, le travail est perdu

> 💡 Pas de heartbeat → panne détectée → la charge est **réassignée automatiquement** (failover). On conçoit pour survivre aux pannes, pas pour les éviter.

---

### 5. À quoi sert le **partitionnement** des données ?

- A. À chiffrer les données
- B. À compresser les fichiers
- **C. À découper un gros jeu de données pour le traiter en parallèle ✅**
- D. À supprimer les doublons

> 💡 Comme l'inventaire d'un entrepôt : on répartit les allées dans l'équipe, chacun compte la sienne **en même temps**, puis on additionne.

---

### 6. Aujourd'hui, quelle est la **stack de stockage** de référence dans le cloud ?

- A. HDFS sur cluster Hadoop on-premise
- **B. Object Storage (S3/GCS) + formats ouverts (Iceberg/Delta) = Lakehouse ✅**
- C. Un seul gros fichier Excel
- D. Une base MySQL répliquée

> 💡 HDFS cède la place à l'**Object Storage + formats ACID** (Iceberg, Delta Lake) : le pattern Lakehouse.

---

### 7. Spark ou Flink : lequel choisir pour ajuster le **prix dynamique (surge)** d'Uber en temps réel ?

- A. Spark, car il est plus connu
- **B. Flink, conçu pour le streaming temps réel à faible latence ✅**
- C. Aucun des deux, c'est le rôle de Kafka
- D. Les deux donnent exactement le même résultat

> 💡 Spark ≈ camion benne (gros volumes, différé) ; **Flink ≈ ambulance** (faible latence, temps réel critique).

---

### 8. À quoi sert **Apache Kafka** dans la stack ?

- **A. Bus de messages distribué qui encaisse des millions d'événements/seconde ✅**
- B. À créer des dashboards
- C. À orchestrer les tâches planifiées
- D. À stocker durablement les données nettoyées

> 💡 Kafka = autoroute à données : il **découple** producteurs et consommateurs et absorbe les pics (pings GPS, événements app).

---

### 9. Quel outil **pilote et planifie** l'ensemble du pipeline (le déclenche, le surveille, gère les retries) ?

- A. Spark
- B. S3
- **C. Un orchestrateur comme Airflow ou Dagster ✅**
- D. Kafka

> 💡 L'orchestration est **transversale** : elle ne traite pas la donnée, elle pilote toutes les étapes de bout en bout.

---

### 10. Quel est le **piège** le plus fréquent quand on démarre un projet Big Data ?

- A. Choisir des outils open source
- B. Documenter ses données trop tôt
- **C. L'over-engineering : déployer Kafka + Flink + Kubernetes pour un tout petit volume ✅**
- D. Utiliser le cloud

> 💡 **Start simple, scale later.** La complexité se mérite — on ne la déploie pas par défaut.

---

## 🏆 Barème indicatif
- 8–10 ✅ : Data Engineer en puissance
- 5–7 ✅ : Bonnes bases, à approfondir
- 0–4 ✅ : Pas de panique — les slides sont là pour ça !

## 📋 Pour Kahoot
Copier-coller chaque question + ses 4 réponses dans l'éditeur, cocher la bonne (✅).
Astuce : régler 20 s pour les Q1–6 (concepts) et 30 s pour les Q7–10 (choix d'outils, plus de réflexion).
