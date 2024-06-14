# Annotating metabolite mass spectra with domain-inspired chemical formula transformers

## Why discuss this paper?
- The article primarily focuses on metabolite analysis and small-molecule structure elucidation using deep learning approaches.
### Overview of MIST

```mermaid
%%{init: {"flowchart": {"htmlLabels": true}} }%%
flowchart LR;

Introduction_MIST["<b><center>Introduction to MIST</center></b>"] --> Neural_Network_Approach["Neural Network Approach"];
Introduction_MIST --> Spectrum_Representation["Spectrum Representation"];
Introduction_MIST --> Inductive_Biases_and_Features["Inductive Biases and Features"];
Inductive_Biases_and_Features --> Neutral_Loss_Relationships{{"Neutral Loss Relationships"}};
Inductive_Biases_and_Features --> Simultaneous_Predictions{{"Simultaneous Predictions"}};
Inductive_Biases_and_Features --> In_Silico_Forward_Augmentation{{"In Silico Forward Augmentation"}};
Inductive_Biases_and_Features --> Novel_Unfolding_Architecture{{"Novel 'Unfolding' Architecture"}};
Spectrum_Representation --> Chemical_Formulae_of_Peaks{{"Uses chemical formulae of peaks"}};
Spectrum_Representation --> Inspired_by_CSI_FingerID{{"Inspired by CSI:FingerID"}};

Introduction_MIST --> Enhancing_Spectra_Annotation["<b><center>Enhancing Spectra Annotation</center></b>"];
Enhancing_Spectra_Annotation --> Contrastive_Representation_Learning{{"Contrastive Representation Learning"}};

Introduction_MIST --> Model_Evaluation["<b><center>Model Evaluation</center></b>"];
Model_Evaluation --> Ablation_Studies{{"Ablation Studies"}};

Introduction_MIST --> Real_World_Application["<b><center>Real-World Application</center></b>"];
Real_World_Application --> Clinical_Sample_Analysis{{"Clinical Sample Analysis"}};
Real_World_Application --> Propose_New_Annotations{{"Propose New Annotations"}}

```
(![Figure 1](./Fig1.png)
)

### Summary:

- **Metabolite Analysis:**
  - Relative abundances of piperidine and pyridine alkaloids in healthy, UC, and CD patient groups are compared.
  - Total number of metabolites and novel structures identified are discussed.
  - Abundance fold changes between healthy, UC, and CD cohorts are analyzed.
  - Annotated molecule structures for select compounds are presented.

- **MIST Model:**
  - MIST accurately predicts compound fingerprints from mass spectra.
  - Contrastive fine-tuning improves metabolite retrieval.
  - The model demonstrates strong accuracy on various metabolite classes.

- **Future Directions:**
  - Need for standardized benchmarks in mass spectrometry model development.
  - Exploration of retrieval task complexities and potential biases in predictions.
### Code availability
All code to replicate experiments, train new models and load pretrained models is available at https://github.com/samgoldman97/mist.

## Takeaways
- Metabolite analysis compares healthy, UC, and CD patient groups, focusing on piperidine and pyridine alkaloids.
- The MIST model accurately predicts compound fingerprints from mass spectra with high accuracy.
- Future directions involve standardizing benchmarks in mass spectrometry model development and exploring retrieval task complexities and biases in predictions.

