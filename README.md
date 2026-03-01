# Traditional Chinese Medicine Clinical Knowledge Graph & Intelligent Diagnosis System
This project integrates two core modules:
1.Clinical Data Preprocessing & Knowledge Graph Construction: Automatically extract entities (symptoms, syndromes, herbs, etc.) and relations from raw outpatient Excel data, generate structured KG files, and optionally import into Neo4j.
2.Knowledge Graph Embedding & Intelligent Diagnosis: Based on the constructed KG files, learn entity vectors using TransE, and combine Attention GNN with Reinforcement Learning (RL) to achieve interpretable diagnostic path planning (symptom → syndrome → prescription).

### Features

1.Automated Data Cleaning: Intelligently identify and classify Chinese/Western medicine from prescription texts, correct misclassifications.

2.Multi-layer Symptom Mapping: Support exact match, keyword match, and semantic match for improved entity alignment.

3.Optimized Relation Construction: Generate high-quality edges based on entity co-occurrence frequency and importance-based stratified sampling.

4.Graph Structure Augmentation: Add intra-type connections, transitive relations, and community detection to reduce isolated nodes.

5.Embedding Learning: Optimized TransE model with dynamic negative sampling, type-aware sampling, and stable training.

6.Attention GNN + RL: Phased training framework achieving 100% diagnostic success rate with average path length of 3.8 steps.

7.Visualization & Analysis: PCA stability assessment, t-SNE clustering, Neo4j graph visualization, similarity queries, and herb-syndrome clustering.

8.Ablation Study: LLM vs. Unordered RAG vs. Trajectory RAG Agent: We systematically compare three diagnostic paradigms: pure LLM diagnosis, traditional unordered RAG (retrieving isolated KG facts), and our Trajectory RAG Agent (feeding RL-discovered diagnostic paths into LLM). Results show our method achieves superior accuracy, medication hit rate, and logical coherence.


### Project Structure

.
├── data/                          # Raw data (place your Excel file here)
│   ├── outpatient_data.xlsx        # Example raw data
│   ├── clinical_kg_nodes.csv       # Generated node file
│   └── clinical_kg_edges.csv       # Generated edge file
├── src/
│   ├── clinical_data_processor.py  # Data preprocessing & KG construction
│   ├── main.py                      # Main training & diagnosis program
│   ├── models/                       # Model definitions (TransE, GNN, Agent)
│   ├── utils/                        # Utility functions (visualization, evaluation)
│   └── requirements.txt              # Dependencies
├── output/                           # Output results (models, figures, reports)
├── README.md
└── LICENSE

### Requirements

1.Python 3.8+

2.Install dependencies: pip install -r requirements.txt (includes pandas, numpy, torch, torch-geometric, scikit-learn, matplotlib, py2neo, etc.)

### Quick Start

#### 1. Data Preparation

Place your raw outpatient Excel file (e.g., 【脱敏数据】【2017-2024】门诊数据查询.xlsx) into the data/ directory.
The file should contain columns such as (column names are matched intelligently):

·TCM Diagnosis: 中医病名, 中医诊断名称

·TCM Syndrome: 中医证型, 症候名称

·Western Diagnosis: 诊断, 西医诊断

·Symptoms: 主诉, 症状

·Tongue Signs: 舌, 舌象

·Pulse Signs: 脉, 脉象

·Prescriptions: 中药处方明细, 西药处方明细, 处方药物

#### 2. Run Data Preprocessing
cd src
python clinical_data_processor.py

The program will:
·Read all sheets in the Excel file.
·Extract entities and classify them (symptom, syndrome, herb, drug, etc.).
·Create relation edges based on co-occurrence frequency.
·Generate clinical_kg_nodes.csv and clinical_kg_edges.csv in the data/ directory.
·Optionally import the data into Neo4j (requires Neo4j installed and configured).

#### 3. Train Embedding & Diagnostic Models

Ensure the KG files are in data/, then run:

bash
python main.py

The program will:
·Load KG files and perform graph augmentation.
·Train TransE model (outputs loss curves, embedding visualizations).
·Phased training of Attention GNN and RL agent.
·Evaluate model performance (link prediction MRR, diagnostic success rate, path length).
·Generate PCA visualizations, similarity queries, herb-syndrome clustering, etc.

#### 4. Outputs

·training_curves.png: TransE training loss curve.
·enhanced_pca_visualization.png: PCA stability assessment.
·simple_visualization.png: Scatter plot of entity embeddings.
·embeddings_2d.csv: 2D coordinates of entities after PCA.
·Model files: transe_model.pth, balanced_gnn_model.pth, rl_agent_model.pth.

### Results

·Link Prediction: GNN achieves MRR of 0.1844, a 122.5% improvement over TransE.
·Diagnostic Success Rate: RL agent reaches 100% on test cases with average path length of 3.8 steps.
·Herb-Syndrome Clustering: K-means identifies 8 semantic clusters, validating the "herb-syndrome correspondence" principle (e.g., Shaoyang syndrome with peony, distance 1.1549).
·PCA Stability: Optimized PCA yields stability score of 0.5260, ensuring reliable visualization.

### Citation

If you use this project in your research, please cite:

text
@misc{shen2026tcmkg,
  title={tcm-kg-rl: Traditional Chinese Medicine Knowledge Graph and Reinforcement Learning Diagnosis System},
  author={Shen, Wihan},
  year={2026}
}

### License
MIT License

### Contact
For questions, please contact: 3457788760@qq.com
