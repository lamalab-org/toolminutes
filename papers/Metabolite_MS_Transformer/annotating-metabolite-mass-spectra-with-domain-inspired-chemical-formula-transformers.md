# Annotating metabolite mass spectra with domain-inspired chemical formula transformers

## Why discuss this paper?
- The article primarily focuses on metabolite analysis and small-molecule structure elucidation using deep learning approaches.
### Overview of MIST

```mermaid
%%{init: {"flowchart": {"htmlLabels": true}} }%%
flowchart LR;

Introduction_MIST["<b><center>Introduction to MIST</center></b>\n\nHere we introduce Metabolite Inference with Spectrum Transformers (MIST), a neural network approach capable of outperforming current approaches despite not using handcrafted kernels. Rather than using binned spectra as inputs, MIST represents a spectrum as the set of chemical formulae of all peaks, borrowing inspiration from CSI:FingerID."] -->|Detail| Neural_Network_Approach["Neural Network Approach"];
Introduction_MIST -->|Detail| Spectrum_Representation["Spectrum Representation"];
Introduction_MIST -->|Detail| Inductive_Biases_and_Features["Inductive Biases and Features"];
Inductive_Biases_and_Features -->|Detail| Neutral_Loss_Relationships["Neutral Loss Relationships"];
Inductive_Biases_and_Features -->|Detail| Simultaneous_Predictions["Simultaneous Predictions"];
Inductive_Biases_and_Features -->|Detail| In_Silico_Forward_Augmentation["In Silico Forward Augmentation"];
Inductive_Biases_and_Features -->|Detail| Novel_Unfolding_Architecture["Novel 'Unfolding' Architecture"];
Spectrum_Representation -->|Detail| Chemical_Formulae_of_Peaks["Uses chemical formulae of peaks"];
Spectrum_Representation -->|Detail| Inspired_by_CSI_FingerID["Inspired by CSI:FingerID"];

Enhancing_Spectra_Annotation["<b><center>Enhancing Spectra Annotation</center></b>\n\nWe further enhance the spectra annotation capabilities of MIST by contrastive representation learning on the penultimate network layer."] -->|Detail| Contrastive_Representation_Learning["Contrastive Representation Learning"];
Model_Evaluation["<b><center>Model Evaluation</center></b>\n\nWe show the utility of various model components and the inductive biases described above through thorough ablation studies on a publicly available dataset to provide a foundation for future model development."] -->|Detail| Ablation_Studies["Ablation Studies"];
Real_World_Application["<b><center>Real-World Application</center></b>\n\nFinally, to demonstrate MIST on real-world data, we analyse clinical samples from an inflammatory bowel disease patient cohort and propose new dipeptide and alkaloid structure annotations for differentially abundant metabolites."] -->|Detail| Clinical_Sample_Analysis["Clinical Sample Analysis"];
Real_World_Application -->|Detail| Propose_New_Annotations["Propose New Annotations"];


```

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


## Takeaways
- Metabolite analysis compares healthy, UC, and CD patient groups, focusing on piperidine and pyridine alkaloids.
- The MIST model accurately predicts compound fingerprints from mass spectra with high accuracy.
- Future directions involve standardizing benchmarks in mass spectrometry model development and exploring retrieval task complexities and biases in predictions.