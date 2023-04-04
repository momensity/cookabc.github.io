---
layout: post
title:  "HuggingGPT"
date:   2023-04-04 13:15:25 +0800
categories: gpt
---

### [Solving AI Tasks with ChatGPT and its Friends in HuggingFace](https://twitter.com/realrenmin/status/1641702212710834176)

> HuggingGPT is a system that leverages large language models like ChatGPT to connect various AI models in HuggingFace and solve complicated AI tasks.
>
> By using ChatGPT as a controller to manage existing AI models, HuggingGPT can cover numerous sophisticated AI tasks across different modalities and domains, paving a new way towards AGI.
>
> The diagram shows Language serves as an interface for LLMs (e.g., ChatGPT) to connect numerous AI models (e.g., those in HuggingFace) for solving complicated AI tasks.
>
> [The original paper](https://arxiv.org/pdf/2303.17580.pdf)

![hugging face](/assets/huggingface/hugging_face.png)

Just as shown in the diagram, the whole process of `HuggingGPT` can be divided into four stages:

- ***Task Planning***: Using `ChatGPT` to analyze the requests of users to understand their intention, and disassemble them into possible solvable sub-tasks via prompts.
- ***Model Selection***: Based on the sub-tasks, `ChatGPT` will invoke the corresponding models hosted on `HuggingFace`.
- ***Task Execution***: Executing each invoked model and returning the results to `ChatGPT`.
- ***Response Generation***: Finally, using `ChatGPT` to integrate the prediction of all models, and generate answers for users.

![hugging face stages](/assets/huggingface/hugging_face_stages.png)
