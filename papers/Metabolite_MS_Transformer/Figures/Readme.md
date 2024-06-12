graph LR
  A[Start] --> B{Introduction to MIST}
  B --> C{Neural Network Approach}
  C --> D{Outperforms current methods}
  C --> E{No handcrafted kernels}
  B --> F{Spectrum Representation}
  F --> G{Uses chemical formulae of peaks}
  F --> H{Inspired by CSI:FingerID}
  A --> I{Inductive Biases and Features}
  I --> J{Neutral Loss Relationships}
    J --> K{Implicitly featurize between fragments}
  I --> L{Simultaneous Predictions}
    L --> M{Predicts structures of metabolite and fragments}
  I --> N{In Silico Forward Augmentation}
    N --> O{Provides more training data}
  I --> P{Novel 'Unfolding' Architecture}
    P --> Q{Increases fingerprint prediction resolution}
  A --> R{Enhancing Spectra Annotation}
  R --> S{Contrastive Representation Learning}
    S --> T{Applied on penultimate network layer}
  A --> U{Model Evaluation}
  U --> V{Ablation Studies}
    V --> W{Demonstrate utility of components & biases}
    V --> X{Conducted on publicly available dataset}
  A --> Y{Real-World Application}
  Y --> Z{Clinical Sample Analysis}
    Z --> AA{Inflammatory bowel disease patient cohort}
  Y --> BB{Propose New Annotations}
    BB --> CC{Dipeptide and alkaloid structure annotations}
  A --> End
  