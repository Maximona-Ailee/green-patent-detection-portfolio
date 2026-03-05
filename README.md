# Green Patent Detection Portfolio
# LINKS TO MODEL/DATASET ON HUGGING FACE
- A2 - Model (https://huggingface.co/Ailee52/PatentSBERTa_finetuned_green) // Dataset (https://huggingface.co/datasets/Ailee52/PatentSBERTa-green-dataset)
- A3 - Model (https://huggingface.co/Ailee52/PatentSBERTa_finetuned_green_multiagent) // Dataset (https://huggingface.co/datasets/Ailee52/PatentSBERTa_finetuned_green_multiagent-dataset)
- A4 - Model (https://huggingface.co/Ailee52/PatentSBERTa_finetuned_green_advanced) // Dataset (https://huggingface.co/datasets/Ailee52/PatentSBERTa_finetuned_green_final_advanced-dataset)

---

LINK TO VIDEO DEMO ()
The report of 2 pages is submitted by digitalexam portal

---
# Explaination of each folders:
## A1-SGD Mechanics & Attention Context Assignment1 (M4)
- What did I do: 
  - Part A: Manual computation of Stochastic Gradient Descent (SGD) | setting Hyperparameters: Learning Rate of 2 / Parameters: Initial Weight (W old) of 1
  - Part B: Contextual word representations using self-attention | Sentences: S1 - The seal is swimming in the ocean. (Seal = animal), S2 - Please seal the envelope before sending it. (Seal = close)
- results:
  - Part A: The results show the updates of the weight using stochastic Gradient Descent (SGD) for the first three samples. For each sample, the gradient determines both the direction and magnitude of the weight update. A large positive gradient in the first sample causes a large descrease in the weight, while smaller gradients in the later samples lead to smaller updates. This demonstrates how individual samples influence parameter updates in SGD.
  - Part B: In the original embeddings, the distance between "seal" and "envelope" is relatively large because static embeddings do not encode contexual relationships. (original distance approximated at 0.41). After applying self-attention, the distance becomes much smaller, indicating that "seal" incorporates contexual information from surrounding words such as "envelope". (after attention the distance approximated at 0.064). This demonstrates that self-attention produces context-aware representations.
- details: -
- **files details:**
  - *m4s2assignment1.inpy* - main jupiternotebook file containing both Part A and Part B implementations (was originally implemented in Colab)
  - *results image*

## A2-Green Patent Detection (PatentSBERTa): Active Learning + LLM→Human HITL
- What did I do:
  - PART A: Baseline Model (Frozen Embeddings) | split train_silver 30,000 claims / pool_unlabeled 10,000 claims / eval_silver 10,000 claims
  - Part B: Identify High-Risk Examples (Uncertainty Sampling) | model = SentenceTransformer("AI-Growth-Lab/PatentSBERTa") / compute pool_df["u"] = 1 - 2 * np.abs(pool_df["p_green"] - 0.5) as instructed
  - Part C: Implement LLM → Human HITL (Gold Labels) | LLM using llama 3:4b / HITL on colab
  - Part D: Final Model (Fine-Tune PatentSBERTa Once) | Training auguments set epoch = 1 / learning rate = 2e-5 / batch size = 16 / evaluate on F1
- results:
  - Part A: evaluation on eval_silver F1 score = 0.77
  - Part B: top-100 uncertainly claims from model "AI-Growth-Lab/PatentSBERTa"
  - Part C: llm evaluation on 100claims run locally, HITL 21% override from 100 claims
  - Part D: eval_silver = 0.8009 / eval_gold = 0.5977
- **files details:**
  - *baseline_logreg.pkl* - baseline from frozen embeddings
  - *hitl_green_100.csv* - 100 claims most uncertainly
  - *hilt_green_100_llm_evaluation* - jupyternotebook to run 100 claims with LLM evaluation (locally)
  - *gold_100_labeled.csv* - 100 claims with HITL manually
  - *M4_Assignment2_partA_baselinemodelfz.inpy* - main jupiternotebook file containing Part A (was originally implemented in Colab)
  - *M4_Assignment2_partB_Identify_High-Risk_Examples.inpy* - main jupyternotebook file containing Part B (was originally implemented in Colab)
  - *M4_Assignment2_ partC_Implement_LLM_HITL.inpy* - main jupyternotebook file containing Part C (was originally implemented in Colab)
  - *M4_Assignment2_ partD_Final Model_(Fine-TunePatentSBERTaOnce).inpy* - main jupyternotebook file containing Part D (was originally implemented in Colab)

## A3-Green Patent Detection: Advanced Architectures (Option: Agentic-CrewAI llama3:4b)
- What did I do:
  - Part A & B: Setup | Reuse patent_50k_green.parguet and hitl_green_100.csv from assignment 2
  - Part C: Choose Your Advanced Path | choose CrewAI and run debate LLM locally (advocate, skeptic, and judge) mainly role-based prompt
  - Part D: Human Review & Final Integration |  HITL on colab / Training auguments set epoch = 1 / learning rate = 2e-5 / batch size = 16 / evaluate on F1
  - Part E: Comparative Analysis | focus on eval_silver baseline, assignment 2 model, and assignment 3 model
- results:
  - Part C: CrewAI debate LLMs working on 100 claims run locally
  - Part D: HITL 56% agree with agent system from 100 claims / eval_silver = 0.8066 / eval_gold = 0.4791
  - Part E: Comparative Analysis (on eval_silver)
      - Baseline --> F1 Score of 0.77
      - Assignment 2 Model --> F1 Score of 0.8009
      - Assignment 3 Model --> F1 Score of 0.8066
      - conclusion: Fine-tuning significantly improves performance compared to the frozen baseline. The Multi-Agent System slightly outperforms the Assignment 2 model, indicating that structured agent-based reasoning can provide modest gains in classification performance.
- **files details:**
  - *A3_agent_labels_100_FINAL.csv* - Results from debates LLM agents framework is CrewAI
  - *a3_agent_100_labeled.csv* - 100 claims with HITL manually
  - *M4_assignment3_partC_Multiagentic_locally.ipynb* - jupyternotebook to run 100 claims with multi-agent run locally (CrewAI)
  - *M4_assignment3_partA_B_C_D_E_Setup.ipynb* - main jupyternotebook file containing Part A-B-C-D-E (was originally implemented in Colab)
- **Agreement Report:** The agreement between the agent system and the human labels was 56%. This shows that the multi-agent system correctly matched human decisions in slightly more than half of the reviewed high-risk patent claims.


## A4-Green Patent Detection: Advanced Agentic Workflow with QLoRA (Option: Mistral-7B and LangGraph)
- What did I do:
  - Part A & B: Setup | Reuse patent_50k_green.parguet and hitl_green_100.csv from assignment 2
  - Part C: The Unified Advanced Architecture |
      - C1: Choosing Mistral-7B and fine-tune generative model with QLoRA (4bit with config of r=8, alpha = 16, target modul = q,v,k) / Generation setting max lenght = 400, tempurature = 0.3
      - C2: MAS using LangGraph and got agents to label 100 high-risk claims
  - Part D: Targeted Human Review & Final Integration | uncertain_keywords = ["unclear", "uncertain", "debateable", "unsure", "however", "uncertainly"] / Training auguments set epoch = 3 / learning rate = 2e-5 / batch size = 16 / evaluate on F1
  - Part E: 2-Page Report & Portfolio Submission | write on google document and save as .pdf
- results:
  - Part C: MAS (LangGraph) of Mistral-7B adapted by QLoRA working on 100 claims run locally
  - Part D: 2 claims required human final decision (both claims got disagree by human) / eval_silver = 0.8091 / eval_gold = 0.2727
  - Part E: Submit through digitalexam portal
      - 
- **files details:**
  - *mas_results_gold.csv* - Results from debates LLM agents (Mistral-7B + QLoRA) framework is LangGraph + Targeted HITL
  - *M4_Finalassignment4_partC2_Multiagentic_locally.ipynb* - jupyternotebook to run 100 claims with multi-agent (Mistral-7B + QLoRA) run locally (LangGraph)
  - *M4_FinalAssignment4_partA_B_C1.ipynb* - main jupyternotebook file containing Part A-B-C1 (was originally implemented in Colab)
  - *M4_FinalAssignment4_partC2_D_E.ipynb* - main jupyternotebook file containing Part C2-D-E (was originally implemented in Colab)
- **Disagreement Report:** (Full on HF model card)
  - From 100 high-risk patent claims that processed by Multi-Agent System (MAS), there were 2 claims that judge agent expressed uncertainly in its rationale. Key words using to find uncertain from jude rationale are "unclear", "uncertain", "debateable", "unsure", "however", and "uncertainly", it indicates that the advocate and skeptical arguements reached a deadlock. So, there are 98% autonomous labeling rate.

---

## F1 Comparative analysis on eval_silver (Model Baseline, Simple LLM fintuned, MAS fintuned, and Advanced finetuned)
| **1. Baseline** | Frozen Embedding (No Fine Tuning) | F1 = 0.77 |

| **2. Assignment 2 Model** | Silver + Gold (Simple Generic LLM+HITL) | F1 = 0.8009 |

| **3. Assignment 3 Model** | Silver + Gold (Multi-Agent Fine-Tuning+HITL) | F1 = 0.8066 |

| **4. Assignment 4 Model** | Silver + Gold (QLoRa MAS+HITL) | F1 = 0.8091 |

- Conclusion: This comparative shows the improvement of F1 score of each model's pipeline. Rising from Baseline of 0.77 to 0.8091 of the final model. The integration of QLoRa finetuned Mistral-7B as the MAS brain, combined with HITL (targeted the uncertain judge rationale), produced the highest performing. This validate that agentic debate LLMs with domain-adapted (which specialized or trained on the related data) generated superior training signals compared to generic LLM labeling alone.
---

## Disclaimer
- This project was developed for academic purposes only. The classification results are intended for research and educational use, and should not be interpreted as legal advice or professional patent evaluation. The Human-in-the-Loop (HITL) annotations were performed by students as part of a coursework assignment and do not represent expert legal judgment. The model may contain biases and errors inherited from both automated labeling (silver labels) and LLM-assisted human review.
---

## Usage of Generative AI
- This project utilized Generative AI tools including ChatGPT, Claude, and Grammarly to assist in code implementation, some debuggings, and grammatical corrections thoughout the development process. All the parameter selections, agent configurations, and final judgements, including HITL label decisions were made solely by the author based on personal consideration and understanding of the project.
