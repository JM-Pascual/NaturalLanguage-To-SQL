# NaturalLanguage-To-SQL

The LLM used for the proyect was hosted locally using [KoboldCpp](https://github.com/LostRuins/koboldcpp).
The LLM used during development was [Llama 2 7B Chat - GGML](https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML).
KoboldCpp runs by default on port 5001, but that can be easily modified with the user friendly interface it provides.

## First Agent - Baseline
In the baseline Notebook I tried to get a simple Agent up and running in order to further iterate over the weak points found. 
Based on the results in this initial Notebook, I made several design decisions for my Agent:
1. I would use Two Shot Prompting. This would allow me to reinforce a specific format for the answers while also providing more context.
2. I decided to use [Divine Intellect](https://github.com/oobabooga/text-generation-webui/blob/ae8cd449ae3e0236ecb3775892bb1eea23f9ed68/presets/Divine%20Intellect.yaml) as the preset for the LLM settings. This preset is proven to have great results for instruction following scenarios.

I've also found some Query types that were prone to fail when tested. These were:
1. Queries that involved Specific dates or accessed to specific elements in a date due to the model not using SQLite resources for handling dates.
2. Queries that included information from two different columns

## Second Agent - Final
In the final notebook I decided to add a new element to my Agent, a Vector Database. This would allow me to apply RAG (Retrieval Augmented Generation) over the prompts provided to my final Agent. The embedding used for the questions was [multi-qa-MiniLM-L6-cos-v1](https://huggingface.co/sentence-transformers/multi-qa-MiniLM-L6-cos-v1). The embeddings were paired with examples from the document. Now, examples would resemble the initial user requested query. As my original baseline model used two examples, I kept the 2 nearest neighbors to the user question for building the prompt. By adding examples focused on the points where the first model failed, I managed to tackle the problems mentioned above. 

