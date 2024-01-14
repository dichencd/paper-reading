# PaLM-E: An Embodied Multimodal Language Model
## Keypoints
- inject continuous, embodied observations such as images, state estimates, or other sensor modalities into the language embedding space of a pre-trained language model.

- robotics data is significantly less abundant. As discussed in the last paragraph, our model exhibits transfer, which aids PaLM-E to solve robotics tasks from very few training examples in the robotics domain

- multimodal llm
language as the interface
output a sentence of decisions, not executable actions

- how to retain language capability:
  - frozen llm
  - scale up model size

- general vision-language data + < 10 % robot data