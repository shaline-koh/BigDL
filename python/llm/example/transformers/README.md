# INT4 Inference Pipeline for Large Language Model using BigDL-LLM Transformers-like API

In this example, we show a pipeline to convert a large language model to low precision (INT4), and then conduct inference on the converted INT4 model, using BigDL-LLM transformers-like API.

> **Note**: BigDL-LLM currently supports model family LLaMA, GPT-NeoX, BLOOM and StarCoder.

## Prepare Environment
We suggest using conda to manage environment:
```bash
conda create -n llm python=3.9
conda activate llm

pip install --pre --upgrade bigdl-llm[all]
```

## Run Example
```bash
python ./int4_pipeline.py --thread-num THREAD_NUM --model-family MODEL_FAMILY
```
arguments info:
- `--thread-num THREAD_NUM`: **required** argument defining the number of threads to use for inference. It is default to be `2`.
- `--model-family MODEL_FAMILY`: **required** argument defining the model family of the large language model (supported option: `'llama'`, `'gptneox'`, `'bloom'`, `'starcoder'`). It is default to be `'llama'`.
- `--repo-id-or-model-path REPO_ID_OR_MODEL_PATH`: optional argument defining the huggingface repo id from which the large language model is downloaded, or the path to the huggingface checkpoint folder for the model.

  - When model family is `'llama'`, it is default to be `'decapoda-research/llama-7b-hf'`.
  - When model family is `'gptneox'`, it is default to be `'togethercomputer/RedPajama-INCITE-7B-Chat'`.
  - When model family is `'bloom'`, it is default to be `'bigscience/bloomz-7b1'`.
  - When model family is `'starcoder'`, it is default to be `'bigcode/gpt_bigcode-santacoder'`.

  > **Note** `REPO_ID_OR_MODEL_PATH` should fits your inputed `MODEL_FAMILY`.
- `--promp PROMPT`: optional argument defining the prompt to be infered. It is default to be `'Q: What is CPU? A:'`.

## Sample Output for Inference
### Model family LLaMA
```log
--------------------  bigdl-llm based tokenizer  --------------------
Inference time: xxxx s
Output:
[' It’s the acronym for “Central Processing Unit,” and in modern personal computers it means a single microprocessor chip that is used to control various']
--------------------  HuggingFace transformers tokenizer  --------------------
Please note that the loading of HuggingFace transformers tokenizer may take some time.

The tokenizer class you load from this checkpoint is not the same type as the class this function is called from. It may result in unexpected tokenization. 
The tokenizer class you load from this checkpoint is 'LLaMATokenizer'. 
The class this function is called from is 'LlamaTokenizer'.
Inference time: xxxx s
Output:
["The Central Processing Unit (CPU) is the brains of your computer, and is also known as the microprocessor. It's where all the action"]
--------------------  fast forward  --------------------
Llama.generate: prefix-match hit

llama_print_timings:        load time =     xxxx ms
llama_print_timings:      sample time =     xxxx ms /    32 runs   (    xxxx ms per token)
llama_print_timings: prompt eval time =     xxxx ms /     8 tokens (    xxxx ms per token)
llama_print_timings:        eval time =     xxxx ms /    31 runs   (    xxxx ms per token)
llama_print_timings:       total time =     xxxx ms
Inference time (fast forward): xxxx s
Output:
{'id': 'cmpl-5aa68120-c94b-4433-92f4-b75cc323c22f', 'object': 'text_completion', 'created': 1686557904, 'model': './bigdl_llm_llama_q4_0.bin', 'choices': [{'text': ' It’s a small, compact computer unit that runs on a single chip. This can be connected to various peripheral devices, including printers and displays', 'index': 0, 'logprobs': None, 'finish_reason': 'length'}], 'usage': {'prompt_tokens': 9, 'completion_tokens': 32, 'total_tokens': 41}}
```

### Model family GPT-NeoX
```log
--------------------  bigdl-llm based tokenizer  --------------------
Inference time: xxxx s
Output:
[' Central processing unit, also known as processor, is a specialized microchip designed to execute all the instructions of computer programs rapidly and efficiently. Most personal computers have one or']
--------------------  HuggingFace transformers tokenizer  --------------------
Please note that the loading of HuggingFace transformers tokenizer may take some time.

Inference time: xxxx s
Output:
[' The Central Processing Unit, or CPU, is the component of a computer that executes all instructions for carrying out different functions. It is the brains of the operation, and']
--------------------  fast forward  --------------------
Gptneox.generate: prefix-match hit

gptneox_print_timings:        load time =     xxxx ms
gptneox_print_timings:      sample time =     xxxx ms /    32 runs   (    xxxx ms per run)
gptneox_print_timings: prompt eval time =     xxxx ms /     8 tokens (    xxxx ms per token)
gptneox_print_timings:        eval time =     xxxx ms /    31 runs   (    xxxx ms per run)
gptneox_print_timings:       total time =     xxxx ms
Inference time (fast forward): xxxx s
Output:
{'id': 'cmpl-a20fc4a1-3a00-4e77-a6cf-0dd0da6b9a59', 'object': 'text_completion', 'created': 1686557799, 'model': './bigdl_llm_gptneox_q4_0.bin', 'choices': [{'text': ' Core Processing Unit  or Central Processing Unit  is the brain of your computer, system software runs on it and handles all important tasks in your computer. i', 'index': 0, 'logprobs': None, 'finish_reason': 'length'}], 'usage': {'prompt_tokens': 9, 'completion_tokens': 32, 'total_tokens': 41}}
```

### Model family BLOOM
```log
--------------------  bigdl-llm based tokenizer  --------------------
Inference time: xxxx s
Output:
[' Central Processing Unit</s>The present invention relates to a method of manufacturing an LED device, and more particularly to the manufacture of high-powered LED devices. The inventive']
--------------------  HuggingFace transformers tokenizer  --------------------
Please note that the loading of HuggingFace transformers tokenizer may take some time.

Inference time: xxxx s
Output:
[' Central Processing Unit</s>The present invention relates to a method of manufacturing an LED device, and more particularly to the manufacture of high-powered LED devices. The inventive']
--------------------  fast forward  --------------------


inference:    mem per token = 24471324 bytes
inference:      sample time =     xxxx ms
inference: evel prompt time =     xxxx ms / 1 tokens / xxxx ms per token
inference:     predict time =     xxxx ms / 4 tokens / xxxx ms per token
inference:       total time =     xxxx ms
Inference time (fast forward): xxxx s
Output:
{'id': 'cmpl-4ec29030-f0c4-43d6-80b0-5f5fb76c169d', 'object': 'text_completion', 'created': 1687852341, 'model': './bigdl_llm_bloom_q4_0.bin', 'choices': [{'text': ' the Central Processing Unit</s>', 'index': 0, 'logprobs': None, 'finish_reason': None}], 'usage': {'prompt_tokens': 6, 'completion_tokens': 5, 'total_tokens': 11}}
```

### Model family StarCoder
```log
--------------------  bigdl-llm based tokenizer  --------------------
Inference time: xxxx s
Output:
[' 2.56 GHz, 2.56 GHz, 2.56 GHz, 2.56 GHz, ']
--------------------  HuggingFace transformers tokenizer  --------------------
Please note that the loading of HuggingFace transformers tokenizer may take some time.

Inference time: xxxx s
Output:
[' 2.56 GHz, 2.56 GHz, 2.56 GHz, 2.56 GHz, ']
--------------------  fast forward  --------------------


bigdl-llm:    mem per token =   313720 bytes
bigdl-llm:      sample time =     xxxx ms
bigdl-llm: evel prompt time =     xxxx ms
bigdl-llm:     predict time =     xxxx ms / 31 tokens / xxxx ms per token
bigdl-llm:       total time =     xxxx ms
Inference time (fast forward): xxxx s
Output:
{'id': 'cmpl-72bc4d13-d8c9-4bcb-b3f4-50a69863d534', 'object': 'text_completion', 'created': 1687852580, 'model': './bigdl_llm_starcoder_q4_0.bin', 'choices': [{'text': ' 0.50, B: 0.25, C: 0.125, D: 0.0625', 'index': 0, 'logprobs': None, 'finish_reason': None}], 'usage': {'prompt_tokens': 8, 'completion_tokens': 32, 'total_tokens': 40}}
```