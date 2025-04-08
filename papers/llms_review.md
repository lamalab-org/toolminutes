---
author: Sri Ram Sagar Kanakam
date: 2025-04-09
bibliography: ../references.bib
---

# LLMs and Autonomous Agents in Chemistry

### 1. Introduction 
- **Three key challenges in chemistry**:
  1. Property prediction: Predicting physical/chemical properties of compounds
  2. Property-directed molecule generation: Creating molecules with desired properties
  3. Synthesis prediction: Determining optimal ways to make target molecules
- **Chemical space is vast**: Estimated 10^180 possible stable compounds – requires AI to explore effectively
- **Automation**: The fourth challenge connecting the other three

### 2. Large Language Models (LLMs) Fundamentals 
- **What are LLMs?**: Deep neural networks trained on vast data, primarily using transformer architecture
- **Transformer architecture** consists of:
  - **Input embeddings**: Converts the input tokens into a numerical/vector representation
  - **Attention mechanism**: Allows the model to focus on relevant parts of input
  - **Positional encoding**: Helps maintain sequence order information

- **Training**:
  1. Pretraining Approaches
  - **Self-supervised learning**: Learning patterns from unlabeled chemical data (like SMILES strings) without explicit annotations
  - **Domain-specific Vocabulary**: Creating specialized tokenization for chemical notation rather than using general language vocabularies
  2. Fine-tuning Approaches
  - **Supervised Fine-tuning**: Training on labeled datasets of specific chemical properties or reactions
  - **Instruction Tuning**: Teaching models to follow specific chemistry-related instructions through examples
  - **Parameter-Efficient Fine-Tuning (PEFT)**: Updating only a small subset of model parameters for chemistry tasks

- **Three main transformer architectures**:
  1. **Encoder-only (bidirectional context)** (e.g., BERT): Good for classification and property prediction (understanding)
  2. **Decoder-only (causal context)** (e.g., GPT): Excellent for generative tasks and molecule design (generation)
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

| Name | Architecture | Training Data | Aim | Achievements | Innovations |
|------|--------------|--------------|-----|--------------|------------|
| Mol-BERT | BERT | 4B SMILES from ZINC15/ChEMBL27 | Property prediction | Outperformed existing methods by 2%+ on benchmark datasets | Used Morgan fingerprint fragments as "words" and molecules as "sentences" |
| MolFormer | BERT | 1.1B molecules from ZINC/PubChem | Molecular property prediction | Outperformed GNNs on MoleculeNet tasks | Implemented rotary positional embeddings for better sequence relationships |
| ChemBERTa | RoBERTa | 10M SMILES from PubChem | Property prediction | Showed performance improved with dataset size | Explored impact of tokenization strategies and representation types |
| ChemBERTa-2 | RoBERTa | 77M SMILES from PubChem | Foundational model for multiple tasks | Matched state-of-the-art on MoleculeNet | Added Multi-Task Regression to pretraining |
| SELFormer | RoBERTa | 2M drug-like compounds | Property prediction | Outperformed competitors on several tasks | Used SELFIES instead of SMILES for better robustness |
| SolvBERT | BERT | 1M solute-solvent pairs | Solubility/solvation prediction | Comparable to graph-based models | Predicted 3D-dependent properties from 1D representations |
| MatSciBERT | BERT | 150K materials science papers | Material property extraction | 3% improvement over SciBERT | Fine-tuned for materials science domain |

#### Molecule Generation (Decoder-only Models)


| Name | Architecture | Training Data | Aim | Achievements | Innovations |
|------|--------------|--------------|-----|--------------|------------|
| MolGPT | GPT | MOSES/GuacaMol datasets | Molecular generation | Better performance than VAE approaches | Used masked self-attention for long-range dependencies in SMILES |
| ChemGPT | GPT-neo | 10M molecules from PubChem | Explore hyperparameter tuning | Refined generative models for specific domains | Explored impact of data scale on generation |
| cMolGPT | GPT | MOSES | Target-specific molecule generation | Generated compounds with binding activity | Integrated protein-ligand interactions with conditional generation |
| iupacGPT | GPT | IUPAC names | Generate molecules using IUPAC | Human-readable molecular generation | Used IUPAC names directly instead of SMILES |
| Taiga | GPT + RL | Various | Property-directed molecule generation | Optimized molecules for targeted properties | Employed REINFORCE for property optimization |
| LlaSMol | LLaMA/Mistral | SMolInstruct | Multi-task molecular modeling | State-of-the-art on property prediction | Used parameter-efficient fine-tuning on pretrained models |
| GPTChem | GPT-3 | Multiple benchmarks | Property prediction and inverse design | Competed with specialized models | Showed generalist models can perform chemical tasks |

#### Synthesis Prediction (Encoder-decoder Models)
- **Evolution from template-based to template-free approaches**:
  - Template-based: Limited to known reaction types
  - Semi-template-based: More flexible but still constrained
  - Template-free: Learn directly from data without predefined rules


| Name | Architecture | Training Data | Aim | Achievements | Innovations |
|------|--------------|--------------|-----|--------------|------------|
| Molecular Transformer | Transformer | USPTO datasets | Synthesis prediction | Outperformed prior algorithms | Required no handcrafted rules for chemical transformations |
| Chemformer | BART | 100M SMILES from ZINC-15 | Multi-task chemical modeling | State-of-the-art in synthesis and property tasks | Emphasized computational efficiency through pretraining |
| Text+Chem T5 | T5 | 11.5M-33.5M samples | Multi-task chemical processing | Effective in diverse synthesis tasks | Combined reaction data from multiple sources |
| MOLGEN | BART | 100M molecules (SELFIES) | Molecular generation | Addressed bias against natural product-like molecules | Used domain-specific molecular prefix tuning |
| ReactionT5 | T5 | ZINC and ORD | Reaction prediction | Improved synthesis prediction | Combined property and reaction prediction |
| TSChem | T5 | USPTO | Versatile chemical modeling | Strong performance on multiple tasks | Integrated property and synthesis prediction |

### 5. Multi-modal Approaches 
- **Text2Mol** (2021): Connects molecular representations with text descriptions
- **MolT5** (2022): Generates molecular captions and predicts structures from descriptions
- **Challenges**:
  - Building quality datasets pairing chemical structures with descriptions
  - Multiple valid ways to describe molecules (therapeutic effects, structure, etc.)
  - Need for chemical domain expertise to evaluate outputs
- **Applications**: Molecular retrieval from text queries, structure generation from descriptions
## Multi-modal Models

| Name | Architecture | Training Data | Aim | Achievements | Innovations |
|------|--------------|--------------|-----|--------------|------------|
| Text2Mol | SciBERT w/ decoder | CheBI-20 | Connecting text with molecules | Enabled retrieval of molecules using text | Created paired dataset linking molecules with descriptions |
| MolT5 | T5 | C4 dataset | Molecule captioning and generation | Generated structures from descriptions | Used denoising objective to handle complex chemical descriptions |
| BioT5+ | T5 | ZINC20, UniRef50, PubMed, etc. | Bridging text and molecules | Multi-task performance across domains | Integrated diverse biological and chemical data sources |
| CLAMP | Combined model | Biochemical data | Predict biochemical activity | Enhanced prediction with language | Combined separate chemical and language modules |
| GIT-Mol | Multi-modal | Graphs, images, text | Integrated chemical understanding | Improved accuracy with multiple data types | Combined three modalities for better chemical modeling |

### 6. Conclusion and Future Directions
- LLMs show tremendous potential for transforming chemical research
- Integration of multiple models can address different aspects of chemical discovery
- Key to progress: Better data, improved benchmarks, and enhanced interpretability
- Moving toward autonomous systems that combine the strengths of different models


