# 🗄️ Big Data Stack

> **Pourquoi** on en a besoin, **comment** ça marche, et les **outils** qui la font tourner.

Une présentation interactive (≈30 min) construite avec [Slidev](https://sli.dev), qui démystifie une stack Big Data moderne à travers un fil rouge concret : **le parcours d'une course Uber**, de la demande sur l'app jusqu'à la voiture assignée.

On part du problème (une seule machine ne suffit plus), on traverse les concepts du distribué (Master/Worker, réplication, partitionnement), puis on relie chaque outil (Kafka, Spark, Flink, S3/Iceberg, Airflow) à un besoin réel, avant de finir sur les pièges et bons réflexes de terrain.

## Structure du projet

- `slides.md` : fichier principal des slides
- `pages/` : pages Markdown additionnelles incluses dans la présentation
- `components/` : composants Vue personnalisés réutilisables (graphique, matrices, etc.)
- `public/` : images et ressources statiques
- `package.json` : scripts et dépendances Slidev

## Démarrage

1. Installer les dépendances

```bash
npm install
```

2. Lancer le mode développement

```bash
npm run dev
```

3. Ouvrir le navigateur sur

```text
http://localhost:3030
```
