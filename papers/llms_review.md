---
author: Sri Ram Sagar Kanakam
date: 2025-04-09
bibliography: ../references.bib
---

# LLMs and Autonomous Agents in Chemistry

### 1. Introduction 
- **Historical context**: Evolution of AI in chemistry from early computational methods (1950s) to expert systems like DENDRAL (1980s) to deep learning (2010s)
- **Three key challenges in chemistry**:
  1. Property prediction: Predicting physical/chemical properties of compounds
  2. Property-directed molecule generation: Creating molecules with desired properties
  3. Synthesis prediction: Determining optimal ways to make target molecules
- **Automation**: The fourth challenge connecting the other three
- **Chemical space is vast**: Estimated 10^180 possible stable compounds – requires AI to explore effectively

### 2. Large Language Models (LLMs) Fundamentals 
- **What are LLMs?**: Deep neural networks trained on vast text corpora, primarily using transformer architecture
- **Transformer architecture** consists of:
  - **Attention mechanism**: Allows the model to focus on relevant parts of input
  - **Positional encoding**: Helps maintain sequence order information
  - **Self-supervised learning**: Models learn patterns without explicit labels
- **Three main transformer architectures**:
  1. **Encoder-only** (e.g., BERT): Good for classification and property prediction
  2. **Decoder-only** (e.g., GPT): Excellent for generative tasks and molecule design
  3. **Encoder-decoder** (e.g., BART): Ideal for translation tasks like reaction prediction

### 3. The Data Challenge in Chemistry 
- **Limited chemical data**: Chemistry datasets are orders of magnitude smaller than general language datasets
  - General LLMs (e.g., LLaMA2): Trained on trillions of tokens
  - Chemical datasets: Only billions of tokens (mostly hypothetical compounds)
  - Verified synthesized compounds: ~5 orders of magnitude less data than general LLMs
- **Data quality issues**:
  - Current benchmarks like MoleculeNet have errors and inconsistencies
  - Many datasets include theoretical rather than experimental values
  - Need for more reliable, experimentally-verified data
- **Chemical representation**: Models use text-based notations like SMILES, SELFIES, and InChI

### 4. LLMs for Chemistry Applications 
#### Property Prediction (Encoder-only Models)

| Name | Architecture | Aim | Key Achievements |
|------|--------------|-----|------------------|
| Mol-BERT | BERT | Molecular property prediction | Outperformed existing methods by ~2% in ROC-AUC scores on benchmark datasets using molecular fingerprints as "words" |
| ChemBERTa | RoBERTa | Property prediction | Demonstrated performance improvements with increasing dataset size (100K to 10M samples) |
| ChemBERTa-2 | RoBERTa | Foundational model for chemistry tasks | Matched state-of-the-art on MoleculeNet with 77M training samples and multi-task regression |
| MolFormer | BERT with rotary positional embeddings | Fast property prediction | Achieved competitive results with quantum calculations using 1.1B molecules for training |
| MTL-BERT | BERT | Multi-task property prediction | Outperformed state-of-the-art on 60 molecular datasets through multitask learning |
| SolvBERT | BERT | Solvation property prediction | Predicted solvation free energy comparable to graph-based models using only SMILES strings |
| SELFormer | RoBERTa with SELFIES | Property prediction with robust representation | Outperformed competing methods using SELFIES instead of SMILES |
| CatBERTa | RoBERTa | Catalyst property prediction | Specialized for the OpenCatalyst2020 dataset |

#### Molecule Generation (Decoder-only Models)


| Name | Architecture | Aim | Key Achievements |
|------|--------------|-----|------------------|
| MolGPT | GPT | Molecular generation | Outperformed VAE approaches in generating valid, unique molecules |
| ChemGPT | GPT-neo | Molecular generation | Explored hyperparameter tuning and dataset scaling effects |
| cMolGPT | GPT | Target-specific molecule generation | Generated molecules guided by predefined conditions based on target proteins |
| Taiga | Autoregressive model with RL | Optimized molecule generation | Used REINFORCE algorithm to enhance specific molecular features |
| iupacGPT | GPT | Generation with IUPAC names | Created novel compounds using human-readable chemical notation |
| GPTChem | GPT-3 | Property prediction and inverse design | Demonstrated that general-purpose LLMs can compete with specialized chemical models |
| LlasMol | Finetuned Galactica/LLaMa/Mistral | Multi-task chemistry | Achieved state-of-the-art in property prediction with efficient fine-tuning |
| ChatMOF | Mistral | MOF structure generation | Used genetic algorithms to generate diverse Metal-Organic Frameworks |

#### Synthesis Prediction (Encoder-decoder Models)
- **Evolution from template-based to template-free approaches**:
  - Template-based: Limited to known reaction types
  - Semi-template-based: More flexible but still constrained
  - Template-free: Learn directly from data without predefined rules


| Name | Architecture | Aim | Key Achievements |
|------|--------------|-----|------------------|
| Molecular Transformer | Transformer | Reaction prediction | First transformer for synthesis prediction, outperforming previous algorithms |
| ChemFormer | BART | Sequence-to-sequence tasks | Achieved state-of-the-art in synthesis and property prediction after transfer learning |
| ReactionT5 | T5 | Reaction prediction | Combined reaction prediction with property prediction |
| T5Chem | T5 | Product prediction & retrosynthesis | Applied T5 architecture to chemical reaction tasks |
| MOLGEN | BART | Molecular generation | Used domain-agnostic molecular prefix tuning to avoid hallucinations |
| Text+Chem T5 | T5 | Multiple chemistry tasks | Multi-task model for molecule captioning, product prediction and retrosynthesis |

### 5. Multi-modal Approaches 
- **Text2Mol** (2021): Connects molecular representations with text descriptions
- **MolT5** (2022): Generates molecular captions and predicts structures from descriptions
- **Challenges**:
  - Building quality datasets pairing chemical structures with descriptions
  - Multiple valid ways to describe molecules (therapeutic effects, structure, etc.)
  - Need for chemical domain expertise to evaluate outputs
- **Applications**: Molecular retrieval from text queries, structure generation from descriptions
## Multi-modal Models

| Name | Architecture | Aim | Key Achievements |
|------|--------------|-----|------------------|
| MolT5 | T5 | Bidirectional molecule-text translation | Generated molecular captions from SMILES and predicted structures from descriptions |
| BioT5/BioT5+ | T5 | Molecule-text tasks | Expanded on MolT5 with additional biomedical data |
| Text2Mol | SciBERT with decoder | Retrieval of molecules from text | Created paired dataset linking molecules with text descriptions |
| GIT-Mol | Multi-modal | Integration of graphs, images, and text | Improved accuracy through multimodal data fusion |

### 6. Conclusion and Future Directions
- LLMs show tremendous potential for transforming chemical research
- Integration of multiple models can address different aspects of chemical discovery
- Key to progress: Better data, improved benchmarks, and enhanced interpretability
- Moving toward autonomous systems that combine the strengths of different models

### Key Takeaway
LLMs are transforming chemistry by providing powerful tools for property prediction, molecule generation, and synthesis planning. While data limitations remain a challenge, these models are already demonstrating capabilities that complement human expertise and accelerate chemical discovery.