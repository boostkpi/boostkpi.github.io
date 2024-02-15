---
layout: post
title:  "The Orchestration Role of Large Language Models"
categories: [ Overview ]
image: assets/images/llm_orchestrator.webp
alt: "AI Generated Image. The Orchestration Role of Large Language Models"
beforetoc: ""
toc: false
---
>BoostKPI's experiments with using LLMs as an orchestrator of specialized ML models, which enhances data freshness and scalability.

Recently, I had the opportunity to witness a fireside chat between Mohak Shroff (SVP of Engineering at LinkedIn) and Kevin Scott (CTO Microsoft). Both of them agreed that LLMs like OpenAI’s GPT series and Google’s Bard  are emerging as the maestros of machine learning orchestration. These models are uniquely positioned to leverage their vast knowledge and processing capabilities to direct a symphony of more cost-optimized machine learning systems. This strategy not only ensures efficiency but also capitalizes on the freshest data to deliver precise and up-to-date answers.

## The Role of LLMs as Orchestrators
LLMs can digest a massive expanse of data and develop a nuanced understanding of a wide array of topics. But their true potential lies in their ability to act as central nodes, orchestrating a network of specialized, leaner machine learning models that are tailored to specific tasks. This is akin to a conductor leading a group of expert musicians, each playing their part to create a harmonious performance.

## Advantages of the Orchestrated Approach
The benefits of this approach are multifold:
* Cost-Effectiveness: Smaller, specialized models require less computational power to run, making them more cost-effective. LLMs can determine which model is best suited for a task and delegate accordingly.
* Freshness of Data: Specialized models can be updated more frequently with domain-specific data, ensuring the information they use is current. LLMs, with their broader focus, can integrate insights from these up-to-date models to provide the most relevant answers.
* Scalability: As the demand for AI applications grows, so does the need for scalability. LLMs can efficiently manage resources by activating only the necessary specialized models for a given query.
* Customization: Different tasks may require different types of data processing. LLMs can tailor the AI's response by selecting the model that best fits the user's needs.

## Implementation in Real-World Scenarios
The [BoostKPI](https://boostkpi.com) team is trying out this orchestration approach. Here is an [illustrative video](https://www.youtube.com/watch?v=KVP3-WwN6Dc) of how a user can use a chat interface (powered by a LLM) to ask questions about revenue. In some cases, the LLM generates SQL queries, taking context and history into account, to answer the question. In other cases, it can use BoostKPI’s custom built data visualization tools such as the drilldown table which highlights segments that drove most of the revenue increase.

## The Path Forward
Tying all the in-house tools via LLMs is challenging. It requires sophisticated algorithms to decide which models to use and when. It also requires a seamless integration of different machine learning systems, each with its data sources and update cycles. Despite these challenges, the user benefits are too significant to ignore.

The future of AI is not in siloed intelligence but in connected, cooperative systems where LLMs play the pivotal role of the conductor, ensuring that the right resources are utilized at the right time for the right task.
