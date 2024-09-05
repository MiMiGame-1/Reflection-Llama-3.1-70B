---
license: llama3.1
base_model: meta-llama/Meta-Llama-3.1-70B-Instruct
pipeline_tag: text-generation
---
# Reflection Llama-3.1 70B

**Reflection 70B is (currently) the world's top open-source LLM, trained with a new technique called Reflection-Tuning that teaches a LLM to detect mistakes in its reasoning and correct course.**

The model was trained on synthetic data generated by [Glaive](https://glaive.ai). If you're training a model, Glaive is incredible — use them.

You can [try the model here](https://reflection-playground-production.up.railway.app/).

## Benchmarks
![image/png](https://cdn-uploads.huggingface.co/production/uploads/60518f3731c5be7f3dd5ebc3/zNs-ZFs0SbnomH7mikiOU.png)

All benchmarks tested have been checked for contamination by running [LMSys's LLM Decontaminator](https://github.com/lm-sys/llm-decontaminator). When benchmarking, we isolate the `<output>` and benchmark on solely that section.

Trained from Llama 3.1 70B Instruct, you can sample from Reflection 70B using the same code, pipelines, etc. as any other Llama model. It even uses the stock Llama 3.1 chat template format (though, we've trained in a few new special tokens to aid in reasoning and reflection).

During sampling, the model will start by outputting reasoning inside `<thinking>` and `</thinking>` tags, and then once it is satisfied with its reasoning, it will output the final answer inside `<output>` and `</output>` tags. Each of these tags are special tokens, trained into the model.

This enables the model to separate its internal thoughts and reasoning from its final answer, improving the experience for the user.

Inside the `<thinking>` section, the model may output one or more `<reflection>` tags, which signals the model has caught an error in its reasoning and will attempt to correct it before providing a final answer.

## System Prompt

```
The system prompt used for training this model is:

You are a world-class AI system, capable of complex reasoning and reflection. Reason through the query inside <thinking> tags, and then provide your final response inside <output> tags. If you detect that you made a mistake in your reasoning at any point, correct yourself inside <reflection> tags.

We recommend using this exact system prompt to get the best results from Reflection 70B. You may also want to experiment combining this system prompt with your own custom instructions to customize the behavior of the model.
```

## Chat Format

As mentioned above, the model uses the standard Llama 3.1 chat format. Here’s an example:

```
<|begin_of_text|><|start_header_id|>system<|end_header_id|>

You are a world-class AI system, capable of complex reasoning and reflection. Reason through the query inside <thinking> tags, and then provide your final response inside <output> tags. If you detect that you made a mistake in your reasoning at any point, correct yourself inside <reflection> tags.<|eot_id|><|start_header_id|>user<|end_header_id|>

what is 2+2?<|eot_id|><|start_header_id|>assistant<|end_header_id|>
```

## Tips for Performance

- We are initially recommending a `temperature` of `.7` and a `top_p` of `.95`.
- For increased accuracy, append `Think carefully.` at the end of your messages.

## Dataset / Report

Both the dataset and a brief report detailing how we trained this model will be released next week, alongside our Reflection 405B model that we expect will be the top-performing LLM in the world, including closed-source models.

---

Thanks to Jason Kuperberg and Josh Bickett from the [HyperWrite](https://hyperwriteai.com) team for reviewing drafts of the report we'll be releasing next week.