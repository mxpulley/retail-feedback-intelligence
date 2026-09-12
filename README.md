# Real-Time Retail Feedback Intelligence

A capstone prototype that turns fashion retail reviews into structured feedback and actionable business insights. The project compares two language models and six prompt configurations per model, then evaluates saved outputs with a local LLM judge.

## Project at a glance

- **Dataset:** Women's E-Commerce Clothing Reviews; 22,641 reviews after removing missing review text.
- **Experiment:** 50 sampled reviews x 2 models x 3 prompting techniques x 2 versions = **600 outputs**.
- **Candidate models:** GPT-4o mini and Gemini 3.1 Flash-Lite.
- **Prompting:** Zero-Shot, Few-Shot, and Chain-of-Thought, each with V1 and V2.
- **Local evaluator:** `gpt-oss:20b` served by Ollama in Google Colab.
- **Structured fields:** sentiment, feedback category, urgency, summary, personalized message, and retail insight.

## Workflow

Review data -> model and prompt experiments -> structured-output validation -> local judge evaluation -> quality and generation-cost comparison -> recommendation prediction comparison -> configuration selection -> business analysis and insights.

The judge evaluates seven quality criteria and identifies critical errors. Recommendation prediction is a separate experiment against the dataset's recommendation labels.

## Selected configuration

**GPT-4o mini + Chain-of-Thought V1** was selected under the project's exploratory weighted decision rule.

| Measure | Result |
| --- | ---: |
| Mean local judge score | 0.803 |
| Outputs with at least one critical error | 12% |
| Recommendation accuracy (separate model-level experiment) | 92% |
| Recommendation F1 (positive class) | 0.944 |
| Estimated generation cost per 1,000 reviews | USD 0.1308 |
| Weighted decision index | 0.840 |

The index uses **60% judge quality + 15% predictive F1 + 15% reliability + 10% cost score**. Reliability is one minus the critical-error rate; cost score is the lowest generation cost divided by the configuration's generation cost. Eligibility requires judge score >= 0.75, F1 >= 0.80, and critical-error rate <= 15%.

GPT-4o mini Zero-Shot V1 scored **0.839**, so the top two are effectively a near tie. This is a project-specific multi-criteria decision index, not a standard financial ROI measure or evidence of statistically significant superiority. Weights were explored during analysis, not preregistered.

Predictive metrics were measured separately for each model and reused across its prompt configurations; they do not demonstrate the predictive performance of each of the six analysis prompts.

## Business findings

Within the 50-review analysis sample, **72%** of outputs were categorized as Fit, **18%** as Quality, and **10%** as Expectation vs. Reality. Fit includes both positive and negative feedback; 72% does not mean that 72% of customers reported a sizing problem.

Suggested actions include clearer sizing guidance, more accurate product images and descriptions, and monitoring repeated quality concerns. Dashboard recommendation rates derived from model predictions should be labeled **predicted recommendation rates**, not observed customer recommendation rates.

## Files

- `Retail_Feedback_Intelligence.ipynb`: publication copy of the Final Submission notebook, with execution outputs and private runtime metadata removed. Original code and analysis narrative are retained.
- `requirements.txt`: unpinned Python dependency list inferred from notebook imports; not a verified environment lockfile.
- `.gitignore`: excludes secrets, datasets, generated CSVs, and local runtime files from future Git commits.

## How to use

1. Open the notebook in Google Colab using **File > Open notebook > GitHub**, or download it and upload it to Colab. Access to this private repository requires authorization.
2. Supply your own copy of the dataset. The course materials and dataset are deliberately not redistributed here. Check the original dataset's redistribution terms before sharing it.
3. Mount your Google Drive and update the paths that reference `AAIDSP/Capstone project` to match your own folder.
4. For model generation, configure Colab Secrets named `OPENAI_API_KEY` and `Gemini_API_Key`. The OpenAI-compatible client is configured for the **Great Learning endpoint**, not a personal OpenAI API endpoint. Access and credits are required for that service. Never paste keys into notebook cells or commit them.
5. For local evaluation, run the notebook's Ollama setup and model-download cells in a GPU runtime. The original evaluation used a T4. Ollama is a separate system dependency; installing its Python package alone is insufficient.
6. To resume analysis without generating outputs again, supply the existing result CSVs referenced by the notebook, including `results_zero_shot_final.csv`, `results_few_shot_final.csv`, `results_cot_final.csv`, `local_judge_re_evaluation_final.csv`, and `recommendation_classification_results.csv`. These files are not included in this initial upload.
7. Run only the sections you need. **Do not select Run all unless you intend to run paid model calls and lengthy GPU evaluations.**

## Limitations and cost interpretation

- This is a notebook-based prototype, not a deployed real-time streaming service.
- Results use a small sample and need broader testing and human review before production deployment.
- The local judge has no per-request API fee but still uses compute resources and may make evaluation errors.
- Generation costs are estimates recorded for the experiment, not current provider quotes. The approximate USD 2.96 estimate for 22,641 reviews assumes the same average cost for one pass and excludes retries, local evaluation compute, integration, and human review.
- Outputs were removed for safe repository publication. This copy has not been rerun end-to-end during packaging; external files, credentials, and a compatible Colab runtime are still required.

## Attribution and sharing

Prepared as an academic capstone using course-provided starting materials. Course instructions, template notebooks, another author's comparison repository, and credentials are excluded. No open-source license is assigned in this initial private version; review third-party rights before making the repository public or licensing it.
