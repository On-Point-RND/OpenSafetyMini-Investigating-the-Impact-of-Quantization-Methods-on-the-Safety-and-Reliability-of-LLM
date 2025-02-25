# OpenSafetyMini - a challenging open-ended dataset and human evaluations for LLM safety research

https://arxiv.org/abs/2502.15799

## Datasets

- Our refined dataset [`OpenSafetyMini.csv`](data/OpenSafetyMini.csv) with [criteria](data/opensafetymini_safety_criteria.json) for each safety category.

- Human evaluations of the [Llama 3.1 8B Instruct](meta-llama/Llama-3.1-8B-Instruct) different versions responses - [`OpenSafetyMini_responses_annotated.csv`](data/OpenSafetyMini_responses_annotated.csv).

## Prompts

All prompt templates are designed to fill them with `.format` python string function.

### OpenSafetyMini & [XSAFETY](https://github.com/Jarviswang94/Multilingual_safety_benchmark)

[`Prompt template`](prompts/opensafetymini_llm_judge_evaluation_prompt_template.md) for LLM-as-a-Judge evaluations.

- `SCENARIO` - category or scenario of safety 
- `SCENARIO_INSTRUCTIONS` - corresponding criteria available [`here`](data/opensafetymini_safety_criteria.json)
- `PROMPT` - prompt from the dataset
- `RESPONSE` - LLM response

The guided reasoning prompting approach is used, where the model is required to answer relevant questions and, based on these answers, determine whether the response was safe or not. To simplify response parsing, we instructed the model to generate outputs in a JSON schema.

As a reference, we used an aggregated human annotation. The aggregation was performed as follows: the final label was determined by the majority vote among five annotators. In cases where votes were evenly split, the label was assigned according to a predefined hierarchy: **"unsafe" > "ambiguous" > "safe" > "error"**. This approach ensured that, in ambiguous cases, the reference label reflected a more conservative assessment, thereby mitigating potential safety risks.

[Gemma 2 27b](google/gemma-2-27b-it) with current prompt template achieves 92% accuracy on the agreed-upon examples from human annotation.


### [SafetyBench](https://github.com/thu-coai/SafetyBench)

[`Prompt template`](prompts/safetybench_5shot_prompt_template.md) for 5-shot setting.

The available options for each question were enumerated, allowing the model to select the appropriate one in a single step. For reliable response parsing, we employed a logits-based approach instead of generation. Since we used chat models (instruct-tuned), the last prompt line '"Answer: " was provided to the model as the beginning of an "assistant" message, ensuring that the LLM output logits best fitted to the enumerated options.


### [HotPotQA](https://github.com/hotpotqa/hotpot)

[`Prompt template`](prompts/hotpotqa_generation_prompt_template.md) for responses generating. 

- `INFO` - context, provided by the HotPotQA. We used the "disctractor" version.  