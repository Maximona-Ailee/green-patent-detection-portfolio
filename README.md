# Green Patent Detection Portfolio
# LINKS TO MODEL/DATASET ON HUGGING FACE
A2 - Model (https://huggingface.co/Ailee52/PatentSBERTa_finetuned_green) // Dataset (https://huggingface.co/datasets/Ailee52/PatentSBERTa-green-dataset)
A3 - Model (https://huggingface.co/Ailee52/PatentSBERTa_finetuned_green_multiagent) // Dataset (https://huggingface.co/datasets/Ailee52/PatentSBERTa_finetuned_green_multiagent-dataset)
A4 - Model (https://huggingface.co/Ailee52/PatentSBERTa_finetuned_green_advanced) // Dataset (https://huggingface.co/datasets/Ailee52/PatentSBERTa_finetuned_green_final_advanced-dataset)

---

LINK TO VIDEO DEMO ()
The report of 2 pages is submitted by digitalexam portal

---
# Explaination of each folders:
## A1-
What did I do:
What I learned:
details:


## A2-
What did I do:
What I learned:
details:
pipeline:

## A3-
What did I do:
What I learned:
details:
pipeline:

## A4-
What did I do:
What I learned:
details:
piepline:
Disagreement Report: (Full on HF model card)
- From 100 high-risk patent claims that processed by Multi-Agent System (MAS), there were 2 claims that judge agent expressed uncertainly in its rationale.
Key words using to find uncertain from jude rationale are "unclear", "uncertain", "debateable", "unsure", "however", and "uncertainly", it indicates that the advocate and skeptical arguements reached a deadlock.
So, there are 98% autonomous labeling rate.

---

## F1 Comparative analysis on eval_silver (Model Baseline, Simple LLM fintuned, MAS fintuned, and Advanced finetuned)
| 1. Baseline | Frozen Embedding (No Fine Tuning) | F1 = 0.77 |

| 2. Assignment 2 Model | Silver + Gold (Simple Generic LLM+HITL) | F1 = 0.8009 |

| 3. Assignment 3 Model | Silver + Gold (Multi-Agent Fine-Tuning+HITL) | F1 = 0.8066 |

| 4. Assignment 4 Model | Silver + Gold (QLoRa MAS+HITL) | F1 = 0.8091 |

- Conclusion: This comparative shows the improvement of F1 score of each model's pipeline. Rising from Baseline of 0.77 to 0.8091 of the final model.
The integration of QLoRa finetuned Mistral-7B as the MAS brain, combined with HITL (targeted the uncertain judge rationale), produced the highest performing.
This validate that agentic debate LLMs with domain-adapted (which specialized or trained on the related data) generated superior training signals compared to generic LLM labeling alone.
---

## Disclaimer
- This project was developed for academic purposes only. The classification results are intended for research and educational use, and should not be interpreted as legal advice or professional patent evaluation.
The Human-in-the-Loop (HITL) annotations were performed by students as part of a coursework assignment and do not represent expert legal judgment.
The model may contain biases and errors inherited from both automated labeling (silver labels) and LLM-assisted human review.
---

## Usage of Generative AI
- This project utilized Generative AI tools including ChatGPT, Claude, and Grammarly to assist in code implementation, some debuggings, and grammatical corrections thoughout the development process.
  All the parameter selections, agent configurations, and final judgements, including HITL label decisions were made solely by the author based on personal consideration and understanding of the project.
