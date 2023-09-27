## Bedrock

Bedrock is a new service that as of writing this line (25.9.23) is still in beta stage and not available to AWS customers.

Bedrock is a product by AWS that Accelerates development of generative AI applications using FMs through an API, without managing infrastructure.
Currently, Bedrock provides access to foundation model from Anthropic, AI21, AWS Titan family and Stability AI.
Bedrock also enables us to privately customize FMs using our organization's data.


In the hackathon, we choose to expose those foundation model through Jarvis.

| Model Name            | Max Tokens | Model Id                    | Notes                                                                                                                                                                                                                                                                                                                                           | Classification                                          |
|-----------------------|------------|-----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------|
| Claude v2             | 12k        | anthropic.claude-v2         | Anthropic's most powerful model, which excels at a wide range of tasks from sophisticated dialogue and creative content generation to detailed instruction following.                                                                                                                                                                           | Text generation, Conversational                         |
| Claude Instant   v1.1 | 9k         | anthropic.claude-instant-v1 | A faster and cheaper yet still very capable model, which can handle a range of tasks including casual dialogue, text analysis, summarization, and document question-answering.                                                                                                                                                                  | Text generation, Conversational                         |
| Jurassic-2 Mid        | 8k         | ai21.j2-mid                 | Jurassic-2 Mid is AI21’s mid-sized model, carefully designed to strike the right balance between exceptional quality and affordability. Jurassic-2 Mid can be applied to any language comprehension or generation task including question answering, summarization, long-form copy generation, advanced information extraction and many others. | Text, Classification, Insert/edit, Math                 |
| Jurassic-2 Ultra      | 8k         | Jurassic-2 Ultra            | Jurassic-2 Ultra is AI21’s most powerful model offering exceptional quality. Apply Jurassic-2 Ultra to complex tasks that require advanced text generation and comprehension. Popular use cases include question answering, summarization, long-form copy generation, advanced information extraction, and more.                                | Text, Classification, Insert/edit                       |
| Titan Text Large      | 8k         | amazon.titan-tg1-large      | Titan Text is a generative large language model (LLM) for tasks such as summarization, text generation (for example, creating a blog post), classification, open-ended Q&A, and information extraction.                                                                                                                                         | Text generation, Code generation, Instruction following |

As you can see, each model has different size of tokens and different use, you may need to play with your 
hackathon product to find the right model for the job.
When using the API, you will be able to change the FM that in use by playing with the model_id.


> **_NOTE:_** 1 token is equal to 3/4 of a word or 4 chars
.


### Why Bedrock and not chatGPT?
One of the biggest advantages of bedrock over other foundation models as chatGPT or Bard is that
Bedrock allows us to access to multiple foundation model as noted above but not only that, it allows us to use our private data
as context to the foundation model. more about this on the RAG part.