# VOXEL-GRAPH-VECTOR-RAG

<div align="center">

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Python](https://img.shields.io/badge/python-3.8%2B-blue.svg)
![Neo4j](https://img.shields.io/badge/Neo4j-5.x-green.svg)
![FAISS](https://img.shields.io/badge/FAISS-1.7.4-orange.svg)
![LLM](https://img.shields.io/badge/LLM-GPT--4o%20%7C%20Claude-purple.svg)

**A Natural Language Interface for Subsurface Digital Twins using Hybrid RAG and Large Language Models**

[Overview](#overview) • [Architecture](#architecture) • [Features](#features) • [Dataset](#evaluation-dataset) • [Setup](#setup) • [Usage](#usage) • [Citation](#citation) • [Contact](#contact)

</div>

---

## Overview

**VOXEL-GRAPH-VECTOR-RAG** is a research system that enables **natural language querying, reasoning, and 3D visualization** of voxel-based subsurface digital twins. It combines a hybrid Retrieval-Augmented Generation (RAG) pipeline — fusing Neo4j graph traversal with FAISS dense vector search — with task-specific LLM prompting to interpret spatially distributed underground geological semantics.

This work bridges the gap between complex geotechnical knowledge graphs and domain-agnostic users by providing a conversational interface that requires no Cypher or SQL expertise.

> **Co-authored by:**  Usama Khalid  
> Published in: *(citation forthcoming — see [Citation](#citation))*

---

## Architecture

```
User Query (Natural Language)
        │
        ▼
┌───────────────────┐
│   Query Router    │  ← Task Classification (9 types)
└───────────────────┘
        │
        ▼
┌──────────────────────────────────────┐
│         Hybrid Retrieval Engine      │
│  ┌────────────────┐  ┌────────────┐  │
│  │  Neo4j Graph   │  │   FAISS    │  │
│  │  (Traversal)   │  │  (Vector)  │  │
│  └────────────────┘  └────────────┘  │
└──────────────────────────────────────┘
        │
        ▼
┌───────────────────────────┐
│  Task-Specific Prompt     │  ← Anti-hallucination constraints
│  Templates (9 types)      │
└───────────────────────────┘
        │
        ▼
┌───────────────────┐
│   LLM (GPT-4o)    │
└───────────────────┘
        │
        ▼
┌──────────────────────────────────┐
│  Natural Language Answer         │
│  + Structured Voxel IDs          │
└──────────────────────────────────┘
        │
        ▼
┌───────────────────────────┐
│  3D Visualization (Rhino) │
└───────────────────────────┘
```

---

## Features

| Capability | Description |
|---|---|
| **Natural Language Querying** | Conversational access to geotechnical data — no Cypher or SQL required |
| **Hybrid RAG** | Fuses Neo4j knowledge graph traversal with FAISS dense vector retrieval |
| **9 Task Types** | Attribute Retrieval, Filtering, Reasoning, Computation, Classification, Summarization, Comparison, Proximity, Visualization |
| **Anti-Hallucination Prompting** | Task-specific prompt templates with grounding constraints |
| **3D Voxel Visualization** | Real-time highlighting of result voxels inside Rhino 3D |
| **LLM-Agnostic** | Modular design supports GPT-4o, Claude, and other providers |
| **Large-Scale Knowledge Graph** | 16,116 voxels with 50+ geotechnical attributes each |

---

## Repository Structure

```
VOXEL-GRAPH-VECTOR-RAG/
│
├── neo4j_connector.py                   # Neo4j graph database interface
├── faiss_manager.py                     # FAISS vector index manager
├── master_prompts.py                    # Task-specific prompt templates (9 types)
├── openai_helper.py                     # LLM integration (OpenAI / extendable)
├── neo4j_config.py                      # Configuration template (credentials, API keys)
├── rhino_highlighter.py                 # 3D voxel highlighting in Rhino
├── voxel to voxel relationship          
│   calculator.py                        # Graph edge/relationship generator
├── voxel_metadata.json                  # Voxel attribute schema
│
├── Q&A_Dataset_200.xlsx                 # 200 Q&A pairs for routing accuracy evaluation
├── Q&A_Dataset_400.xlsx                 # 400 Q&A pairs for response accuracy evaluation
├── Ground truth with 4                  
│   consistent layers.zip                # Ground truth Rhino geometry model
└── neo4j-2025-12-29T03-39-58.dump      # Full Neo4j database dump (16,116 voxels)
```

---

## Evaluation Dataset

Two benchmark datasets are provided for rigorous evaluation:

### Q&A\_Dataset\_400.xlsx — Response Accuracy (400 queries)

| Task Category | Count | Share |
|---|---|---|
| Attribute Retrieval | 50 | 12.5% |
| Summarization | 50 | 12.5% |
| Visualization | 50 | 12.5% |
| Reasoning | 45 | 11.25% |
| Classification | 45 | 11.25% |
| Filtering | 40 | 10.0% |
| Computation | 40 | 10.0% |
| Comparison | 40 | 10.0% |
| Proximity | 40 | 10.0% |

### Q&A\_Dataset\_200.xlsx — Routing Accuracy (200 queries)

Used specifically for evaluating the task classification / query routing module across the same 9 task types.

---

## Voxel Attribute Schema

Each of the 16,116 voxels carries 50+ attributes across the following categories:

| Category | Attributes |
|---|---|
| **Identification** | `voxel_id`, `project_id` |
| **Material** | `material_type`, `soil_group`, `texture`, `color` |
| **Spatial** | `position_x/y/z`, `elevation`, `depth_below_surface` |
| **Geological** | `mass_id`, `top_surface_id`, `bottom_surface_id` |
| **Physical** | `moisture_content`, `density`, `porosity`, `permeability` |
| **Geotechnical Strength** | `bearing_capacity`, `spt_n_value`, `friction_angle`, `cohesion` |
| **Risk Assessment** | `overall_risk_level`, `settlement_potential_mm`, `is_problematic` |
| **Foundation** | `foundation_suitability`, `recommended_foundation_type`, `ground_improvement_needed` |

---

## Setup

### Prerequisites

- Python 3.8+
- Neo4j Database 5.x
- Rhino 3D 7 or 8 (for visualization only)
- API key: OpenAI (GPT-4o) or any compatible provider

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/<YOUR_USERNAME>/VOXEL-GRAPH-VECTOR-RAG.git
cd VOXEL-GRAPH-VECTOR-RAG

# 2. Install Python dependencies
pip install neo4j==5.14.0 faiss-cpu==1.7.4 numpy pandas openai anthropic
```

### Neo4j Configuration

1. Download and install [Neo4j Desktop](https://neo4j.com/download/)
2. Create a new database project
3. Import the provided dump: `neo4j-2025-12-29T03-39-58.dump`
4. Start the database

### API & Database Credentials

Edit `neo4j_config.py`:

```python
NEO4J_CONFIG = {
    'uri': 'bolt://localhost:7687',
    'user': 'neo4j',
    'password': 'YOUR_NEO4J_PASSWORD',
    'database': 'neo4j'
}

OPENAI_CONFIG = {
    'api_key': 'YOUR_OPENAI_API_KEY'
}
```

> ⚠️ **Security note:** Never commit real credentials to version control. Use `.env` files or environment variables in production.

---

## Usage

### Basic Query Example

```python
from neo4j_connector import Neo4jConnector
from openai_helper import OpenAIHelper
from faiss_manager import FAISSManager

# Initialize components
neo4j = Neo4jConnector(
    uri="bolt://localhost:7687",
    user="neo4j",
    password="your_password"
)
gpt = OpenAIHelper(api_key="your_openai_key")

# Run a filtering query
question = "Find voxels with high moisture content in layer M3"
voxels = neo4j.get_high_moisture_voxels_with_count(threshold=40.0)
answer = gpt.generate_answer(question, voxels['voxels'], 'FILTERING')

print(answer['answer'])
```

### Supported Task Types

```python
TASK_TYPES = [
    'ATTRIBUTE_RETRIEVAL',  # Retrieve specific voxel attribute values
    'FILTERING',            # Filter voxels by conditions
    'REASONING',            # Multi-step spatial/geotechnical reasoning
    'COMPUTATION',          # Aggregate calculations (mean, sum, etc.)
    'CLASSIFICATION',       # Classify voxels by material/risk type
    'SUMMARIZATION',        # Summarize regions or layers
    'COMPARISON',           # Compare two or more voxel groups
    'PROXIMITY',            # Spatial proximity queries
    'VISUALIZATION'         # Return voxel IDs for 3D highlighting in Rhino
]
```

---

## Data Availability

| Asset | Status |
|---|---|
| Source code (all `.py` files) | ✅ Available in this repository |
| Q&A Evaluation Datasets (`.xlsx`) | ✅ Available in this repository |
| Ground Truth Rhino Geometry (`.3dm`) | ✅ Available in this repository |
| Neo4j Database Dump (`.dump`) | ✅ Available in this repository |
| FAISS Vector Index (`.index`) | 📧 Available on request |
| Voxel Embeddings (`.npy`) | 📧 Available on request |

> For large files, contact: **usamakhalid@hanyang.ac.kr**

---

## Citation

> 📌 **Paper details will be added upon publication. Please check back or watch this repository.**

If you use this codebase, dataset, or methodology in your research, please cite:

```bibtex
@article{VOXELGRAPHRAG2025,
  title     = {[Title to be added]},
  author    = {[Author names to be added]},
  journal   = {[Venue to be added]},
  year      = {2025},
  url       = {[URL to be added]}
}
```

---

## Contributing

Contributions, issues, and pull requests are welcome. For major changes, please open an issue first to discuss the proposed modification.

---

## License

This project is licensed under the [MIT License](LICENSE).

---

## Contact

| | |
|---|---|
| **Co-author (LLM / Core System)** | usamakhalid@hanyang.ac.kr |
| **Co-author (Domain / Data)** | muhammadshoaib5308@gmail.com |

---

<div align="center">
<sub>Built with Neo4j · FAISS · OpenAI GPT-4o · Rhino 3D</sub>
</div>
