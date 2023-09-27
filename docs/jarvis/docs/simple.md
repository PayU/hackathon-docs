## Zero shot prompt


> **_Zero-Shot Prompting:_** If you’ve interacted with an LLM-powered chatbot before, you’ve likely already used zero-shot prompting unwittingly. Zero-shot prompting entails relying solely on an LLM’s pre-trained information to answer a given user prompt.

to create your first request to Jarvis you will need a private-key and app-id that will be provided by the hackathon
team . This is your authentication keys, and they should be used by your team and remain private.

A simple zero shot prompt to jarvis would look like this:


```json
curl --location 'https://api.paymentsos.com/hackathon-ai/prompt' \
--header 'private-key: replace-me' \
--header 'app-id: replace-me' \
--header 'Content-Type: application/json' \
--data '{
    "question": "Can you please tell me what is the most pouplar programing language, please provide short answer",
    "model": {
        "stop_sequences": [],
        "name": "anthropic.claude-v2",
        "max_tokens": 1000,
        "temperature": 1,
        "top_k": 250,
        "top_p": 0.999
    }
}'
```

and this is the answer you will get:
```json
{
    "query": "Can you please tell me what is the most pouplar programing language, please provide short answer",
    "answer": " Python is currently one of the most popular programming languages."
}
```

Now lets go over all the params we had in this http request  
* **question** - This is the prompt that will be sent to the FM  
* **stop_sequences** - @kobi help me  
* **name** - This is the model id from the - [Bedrock supported models](./bedrock.md)  
* **max_tokens** - The maximum tokens that will be in the answer - (1 token equal to 3/4 words)  
* **temperature** - [Read here](https://docs.ai21.com/docs/sampling-from-language-models#%EF%B8%8F-temperature)   
* **top_k** - [Read here](https://docs.ai21.com/docs/sampling-from-language-models#-topk)  
* **top_p** - [Read here](https://docs.ai21.com/docs/sampling-from-language-models#-topp)  


> **_Playing with the request body:_** different models and different params like temperature, top_k and top_p will give you different answers, play with them and find the right tool for the job.


As an example, lets ask FM from AI21 the same question
```json
curl --location 'https://api.paymentsos.com/hackathon-ai/prompt' \
--header 'private-key: replace-me' \
--header 'app-id: replace-me' \
--header 'Content-Type: application/json' \
--data '{
"question": "Can you please tell me what is the most pouplar programing language, please provide short answer",
"model": {
"stop_sequences": [],
"name": "ai21.j2-mid",
"max_tokens": 1000,
"temperature": 1,
"top_k": 250,
"top_p": 0.999
}
}'
```

and this is the answer you will get:
```json
{
    "query": "Can you please tell me what is the most pouplar programing language, please provide short answer",
    "answer": " According to recent surveys, JavaScript is currently the most popular programming language among developers."
}
```

As you can see different models trained and tuned on different data and can return various responses. 