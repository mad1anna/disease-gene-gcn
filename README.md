Disease–Gene Link Prediction with GCNs on Hetionet

A proof-of-concept Graph Convolutional Network (GCN) that mines Hetionet’s disease–gene subgraph to predict novel associations.

---
Overview

1. Data extraction & graph construction
   • Runs a Jupyter notebook (`disease_gene_link_prediction.ipynb`) that:
      – Connects to a Neo4j Hetionet instance,
      – Extracts Disease→Gene edges,
      – Builds a NetworkX graph,
      – Computes PageRank as node features,
      – Converts to PyG `Data` for GCN training.
2. Model training & evaluation
   • Performs negative sampling
   • Trains a 2-layer GCN + link-predictor for 50 epochs
   • Reports training loss & AUC-ROC curve
   • Lists top-20 novel disease–gene predictions

---
Prerequisites

* Docker ≥ 4.0 (for Neo4j)
* Neo4j 3.5.12 (to run via Docker)
* Python 3.8+

---
Install Docker CE (“Community Edition”) from https://docs.docker.com/get-docker/ if you don’t already have it.

Setup Neo4j

1. Local Neo4j 3.5.12 install 
   Follow Neo4j’s official docs and import `hetionet-v1.0.db.tar.bz2` if desired.
If you’d rather not import a CSV dump yourself, you can use the publicly-hosted Hetionet Neo4j image like I did:

# Pull the pre-built Hetionet v1.0 + Neo4j 3.5.12 image:

docker pull dhimmel/hetionet:hetionet-v1.0_neo4j-3.5.12

# Run it under the name “hetionet-container”:

docker run \
  --name hetionet-container \
  --rm \
  --publish 7474:7474 \
  --publish 7687:7687 \
  dhimmel/hetionet:hetionet-v1.0_neo4j-3.5.12

 This image bundles Hetionet v1.0 in Neo4j 3.5.12. We recommend installing Neo4j 3.5.12 locally (or via Docker) for full compatibility with this release.

• If you run into any issues, see the upstream setup notes:

• https://github.com/hetio/hetionet/tree/main/hetnet/neo4j/docker#running-the-docker
• https://github.com/hetio/hetionet/tree/neo4j-3.0/hetnet/neo4j

After it’s up, just point your notebook at bolt://localhost:7687 with credentials neo4j / neo4j
and you’ll have the full Hetionet graph ready to query.

   
#Run the Notebook

1. Clone this repo:

   ```bash
   git clone https://github.com/mad1anna/disease-gene-gcn.git
   cd disease-gene-gcn
   ```
2. Install Python dependencies:

   ```bash
   pip install -r requirements.txt
   ```
3. Start Jupyter:

   ```bash
   jupyter notebook
   ```
4. Open `disease_gene_link_prediction.ipynb` and execute all cells.
   It will:

   * Query Neo4j for Disease→Gene edges
   * Build the graph, compute PageRank, prepare PyG data
   * Split edges, sample negatives, train GCN 50 epochs
   * Plot loss, compute AUC, and list top 20 novel predictions

---
#Repository Structure

├── disease_gene_link_prediction.ipynb   # Main notebook

├── requirements.txt         # Python deps (torch, torch-geometric, neo4j, networkx…)

└── README.md                # This file
---

Ensure Neo4j is running before launching the notebook. All code has been tested under Neo4j 3.5.12 & Docker 4.37.1.

License
MIT © AnnaTamuly
Feel free to fork & extend.
