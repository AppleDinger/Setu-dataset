# Setu Dataset

Setu Dataset serves as the data asset storage repository for the SETU network analytics ecosystem. It contains the raw textual corpora, the canonical identity registries used for Named Entity Recognition (NER), and the structured topological metrics output by the processing pipeline.

This repository operates under a decoupled architecture, separating data storage from the computational extraction pipelines managed by the companion repository: **[Setu-engine](https://github.com/AppleDinger/Setu-engine)**.

## System Interdependency Matrix

The relationship between the data assets and the computational engine follows an automated read/write cycle:

```text
┌────────────────────────────────────────────────────────┐
│                      SETU-DATASET                      │
│  ┌─────────────┐       ┌──────────────┐                │
│  │  raw/*.txt  │       │ mapping.json │                │
└──┼─────────────┼───────┼──────────────┼────────────────┘
   │ (Raw Prose) │       │ (NER Schema) │
   └──────┬──────┘       └──────┬───────┘
          │ Read                 │ Read
          ▼                      ▼
┌────────────────────────────────────────────────────────┐
│                      SETU-ENGINE                       │
│       Executes Asynchronous NLP & Graph Analytics      │
└──────────────────┬──────────────────────────┬──────────┘
                   │ Write                    │ Write
                   ▼                          ▼
┌────────────────────────────────────────────────────────┐
│                      SETU-DATASET                      │
│  ┌─────────────────────────┐   ┌────────────────────┐  │
│  │    output/metrics/      │   │   output/graphs/   │  │
│  │ (nodes.csv & edges.csv) │   │    (*_topology.gexf) │
└──┴─────────────────────────┴───┴────────────────────┘
```

Ingestion and resolution: Setu-engine pulls raw long-form prose files from `raw/` and reads `registry/mapping.json` to seed its deterministic entity matching layer. Topological export: the engine computes centrality vectors and modularity partitions, writing relational data matrices back into `output/metrics/` and presentation-ready visualization graphs into `output/graphs/`.

## Directory Architecture

```text
Setu-dataset/
├── LICENSE
├── README.md
├── requirements.txt
├── raw/                      # Unstructured, long-form target text documents
│   ├── berakhot.txt
│   ├── bhagavad_gita_raw.txt
│   ├── GarudaPurana.txt
│   ├── josephus_antiquities_raw.txt
│   ├── josephus_wars_raw.txt
│   ├── king_james_bible_raw.txt
│   ├── legend_of_the_jews_raw.txt
│   ├── mahabharata.txt
│   ├── Panchatantra.txt
│   ├── quran_raw.txt
│   ├── ramayana_raw.txt
│   ├── rig-veda-griffith-p.txt
│   └── vishnupuranam_raw.txt
├── registry/
│   └── mapping.json          # Identity registry schema for canonical entity resolution
└── output/
    ├── graphs/               # Compiled topological GEXF graph structures for Gephi
    │   ├── abrahamic_topology.gexf
    │   └── indic_topology.gexf
    └── metrics/              # Relational data sheets containing advanced graph metrics
        ├── abrahamic_edges.csv
        ├── abrahamic_nodes.csv
        ├── abrahamic_nodes_merged.csv
        ├── indic_edges.csv
        ├── indic_nodes.csv
        └── indic_nodes_merged.csv
```

## Data Schema Reference

### 1. Nodes Sheet (`output/metrics/*_nodes.csv`)

Contains the absolute master pool of unique entities that survived topological pruning, along with their normalized centrality vectors.

| Column | Type | Description |
| --- | --- | --- |
| Id | String | Unique canonical identifier of the character entity (Gephi primary key). |
| Label | String | Display name of the entity used in layout rendering. |
| DegreeCentrality | Float | Normalized scale of immediate connections ($C_D$). |
| BetweennessCentrality | Float | Measurement of how frequently a node acts as a bridge along shortest paths ($C_B$). |
| EigenvectorCentrality | Float | Influence metric weighting connections to other highly connected hubs ($C_E$). |
| CommunityID | Integer | Raw cluster index assigned by the stochastic Louvain modularity algorithm. |
| Merged_Faction | String | Gephi-ready, stable historical or narrative macro-grouping resolved by core anchors. |

### 2. Edges Sheet (`output/metrics/*_edges.csv`)

Defines the relational lines linking co-occurring character entities.

| Column | Type | Description |
| --- | --- | --- |
| Source | String | Canonical origin node ID. |
| Target | String | Canonical destination node ID. |
| Weight | Integer | Total frequency of close-proximity contextual co-occurrences. |

## Technical Case Study Summary Results

This dataset contains a comparative validation analysis mapping the social network structure of major historical and religious text traditions.

| Global Topological Metrics | Indic Repositories | Abrahamic Repositories | Empirical Insight |
| --- | ---: | ---: | --- |
| Nodes (Entity Pool) | 78 | 110 | Vocabulary size of active primary characters |
| Edges (Pruned Links) | 588 | 728 | Total validated relational interaction paths |
| Graph Density | 0.1958 | 0.12143 | Connectivity rate across the structural canvas |
| Clustering Coefficient | 0.0211 | 0.0095 | Local grouping probability within neighboring factions |
| Network Diameter | 34 | 34 | Max structural links separating distant entities |
| Average Path Length | 1.91 | 2.24 | Average relational distance between any two nodes |

## Core Architectural Insights

Centralization vs. node uniformity: the Indic network topography is built around massive central hubs surrounded by smaller background figures. The Abrahamic topography exhibits a more uniform, balanced node size configuration.

Cross-referencing vs. linear trajectories: Indic factions show a recursive pattern of non-linear cross-referencing. Abrahamic factions reflect a continuous timeline defined by sequential generations.

Faction segregation vs. integrated boundaries: Indic communities are cleanly segregated by distinct text origins and theological shifts. Abrahamic communities form a continuous historical thread with high cross-reference levels connecting distinct contexts.

## License

This data is licensed under a Creative Commons Attribution 4.0 International License (CC BY 4.0).

Under this license, you are free to share, copy, and redistribute the material in any medium or format, and adapt, remix, transform, and build upon the data for any purpose, even commercially, provided you give appropriate credit to Ram Mishra/Setu.
