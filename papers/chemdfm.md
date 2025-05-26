---
author: Adrian Mirza
date: 2025-05-26
bibliography: ../references.bib
---
#  [ChemDFM: A Large Language Foundation Model for Chemistry](https://arxiv.org/pdf/2401.14818)

## Context and prior work

Frontier models, albeit relatively general, still lack behind in the chemical sciences. Recent work focused in large on fine-tuning LLMs for specific tasks, but they often lacked in depth and diversity of the tasks, employing routine approaches.

Prior work includes Darwin from Xie et al. [1], that finetuned the Llama-7B model[2] on several chemistry tasks. Other works include ChemLLM [3] from Zhang et al. that finetuned the InternLM2-Base-7B model [4] on 9 different tasks. Other works followed a similar line, with truly generalist chemical LLMs still missing from the literature.

## Why this paper matters?

This is one of the few attempts to train a chemistry-specific foundation models from the pre-training stage, all the way to fine-tuning. The authors showed that specialized data, can help relatively small models like Llama-13B outperform frontier models like GPT-4 on specialized tasks. While again, the tasks are extremely specific, the sheer scale of 34B tokens in the pretraining is important, since it is unprecedented.

## Model performance

The model seems to exhibit good zero-shot performance compared to GPT-4 on the molecule captioning tasks. The authors indicated a similar performance increase for other tasks like SMILES-to-IUPAC, which is at 0% for GPT-4, and at 4% for ChemDFM. While in the real world, and when compared to more specialist models like STOUT, the model does not provide significant utility.

![Model performance on the molecule captioning task. Star indicates reproduced results from Guo et al. [5]](./chemdfm/Figure%201.png)

Moreover, they showed that the model exhibits good conversational capabilities, and the ability to correct its mistakes. For instance, in the snipped below, the model improves its wrong answer upon a correction coming from the user. 

![An example of a model's capability to correct itself towards the right answer, after being pointed out as a wrong by the user.](./chemdfm/Figure%202.png)

## Limitations

The authors were among the first to perform full pretraining on a sizable corpus, but the shortcomings of this work lay in the lack of ablations. For example, while a performance exceeding the one of GPT-4 is promising for the open-source community in general, it is perhaps a very narrow set of tasks. GPT-4 outperformed ChemDFM on the 10-shot task, indicating that just with a simple prompting technique as in-context learning, the frontier model outperforms ChemDFM on the majority of metrics.

Moreover, ablations of the pre-training should be performed. It is not fully understood from this paper whether the pretraining helped or not, or just fine-tuning is responsible for the spurt in the zero-shot performance.

## References


