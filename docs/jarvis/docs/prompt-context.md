### Using RAG

We discussed how to load Jarvis with documents from different sources in the previous chapter.
Let's see how we can use Jarvis API to ask questions about these documents.


Using the website loader, we uploaded two different blog posts.
One of the blog posts discussed debeizum, a CDC tool used in GPO engineering.

Let's ask a question about that article that was written after the FM finished its training


The API using RAG is similar to the previous used API with zero shot, the difference is the addition of the rag object.

```json

{
 "return_source_documents": true, //if true response will contain the docs that the answer is based on
 "search_type": "similarity", // see API reference for the options.
 "k": 5, // how many vectors should be retrieved from the db.
 "collection": "hackathon" //on which collection of documents look for the vectors - this must be the same collection where doc was loaded.
}
```

```json
curl --location 'https://api.paymentsos.com/hackathon-ai/prompt' \
--header 'private-key: a4b90687-616f-4173-9774-af3cc6bd86bc' \
--header 'app-id: com.payu.3ds-latam-brazil' \
--header 'x-zooz-app-name: com.payu.3ds-latam-brazil' \
--header 'Content-Type: application/json' \
--data '{
    "question": "What are the reasons a debezium connector loses its binlog offset?",
    "model": {
        "stop_sequences": [],
        "name": "anthropic.claude-v2",
        "max_tokens": 200,
        "temperature": 0.3,
        "top_k": 250,
        "top_p": 0.9
    },
    "rag": {
        "return_source_documents": true,
        "search_type": "similarity",
        "k": 5,
        "collection": "hackathon"
    }
}'
```

This will be our response

```json
{
    "question": "What are the reasons a debezium connector loses its binlog offset?",
    "result": " Here are some common reasons a Debezium connector can lose its binlog offset:\n\n- MySQL server restart - This causes the binlog to be rotated, so if the connector misses this event, it will be looking for the old binlog position that no longer exists.\n\n- Connector restart - If the connector goes down uncleanly (crash, kill -9, etc), it may not have time to commit the latest offset to Kafka. Upon restart, it will resume from the last committed offset, losing any uncommitted transactions.\n\n- Unclean connector shutdown - Similar to above, if the connector is killed or crashes, any uncommitted offsets are lost.\n\n- Heartbeats disabled - The heartbeat feature periodically sends dummy events to Kafka to update the offset. If disabled, long periods of inactivity can cause the connector to lose its place.\n\n- Kafka retention issues - If Kafka purges messages or offsets stored in __consumer_offsets before the",
    "source_documents": [
        {
            "page_content": "was among the early adopters of Debezium, which led us to face several challenges throughout the process. However, over time, we grew more confident and gained a deeper understanding of the necessary steps we had to make in order to improve our Debezium implementation.We encountered a few minor issues, often originating from misconfigured connectors. However, the most important topic we‚Äôll discuss is when a connector loses its binlog offset.As mentioned earlier, every connector can be",
            "metadata": {
                "source": "https://medium.com/payu-engineering/unlocking-the-power-of-debezium-69ce9170f101",
                "title": "Unlocking the Power of Debezium. This blog post aims to share our‚Ä¶ | by Tomer Guttman | payu-engineering | Aug, 2023 | Medium",
                "description": "This blog post aims to share our experience, and insights with Debezium (CDC Tool), and the development phases we went through to improve reliability, and observability while capturing data changes‚Ä¶",
                "language": "en"
            }
        },
        {
            "page_content": "was among the early adopters of Debezium, which led us to face several challenges throughout the process. However, over time, we grew more confident and gained a deeper understanding of the necessary steps we had to make in order to improve our Debezium implementation.We encountered a few minor issues, often originating from misconfigured connectors. However, the most important topic we‚Äôll discuss is when a connector loses its binlog offset.As mentioned earlier, every connector can be",
            "metadata": {
                "source": "https://medium.com/payu-engineering/unlocking-the-power-of-debezium-69ce9170f101",
                "title": "Unlocking the Power of Debezium. This blog post aims to share our‚Ä¶ | by Tomer Guttman | payu-engineering | Aug, 2023 | Medium",
                "description": "This blog post aims to share our experience, and insights with Debezium (CDC Tool), and the development phases we went through to improve reliability, and observability while capturing data changes‚Ä¶",
                "language": "en"
            }
        },
        {
            "page_content": "from two tables ‚Äî the original table but also changes of the debezium_heartbeat table.With heartbeat set to occur once an hour, we can be certain that the connector‚Äôs offset will remain up-to-date ‚Äî This is due to the connector capturing changes from both tables, effectively storing the latest offset out of both within the binlog.Our connector configuration now looks as such in v2.3.0.{   \"name\":\"debezium-packages-connector-sandbox\",",
            "metadata": {
                "source": "https://medium.com/payu-engineering/unlocking-the-power-of-debezium-69ce9170f101",
                "title": "Unlocking the Power of Debezium. This blog post aims to share our‚Ä¶ | by Tomer Guttman | payu-engineering | Aug, 2023 | Medium",
                "description": "This blog post aims to share our experience, and insights with Debezium (CDC Tool), and the development phases we went through to improve reliability, and observability while capturing data changes‚Ä¶",
                "language": "en"
            }
        },
        {
            "page_content": "from two tables ‚Äî the original table but also changes of the debezium_heartbeat table.With heartbeat set to occur once an hour, we can be certain that the connector‚Äôs offset will remain up-to-date ‚Äî This is due to the connector capturing changes from both tables, effectively storing the latest offset out of both within the binlog.Our connector configuration now looks as such in v2.3.0.{   \"name\":\"debezium-packages-connector-sandbox\",",
            "metadata": {
                "source": "https://medium.com/payu-engineering/unlocking-the-power-of-debezium-69ce9170f101",
                "title": "Unlocking the Power of Debezium. This blog post aims to share our‚Ä¶ | by Tomer Guttman | payu-engineering | Aug, 2023 | Medium",
                "description": "This blog post aims to share our experience, and insights with Debezium (CDC Tool), and the development phases we went through to improve reliability, and observability while capturing data changes‚Ä¶",
                "language": "en"
            }
        },
        {
            "page_content": "it debezium_connect_offsets.As seen in the Kafka UI screenshot below, within the specific topic dedicated to Debezium offsets, we can see that the most recent offset message for the packages_sandbox connector is listed as 1435109 (pos).Offset message of ‚Äúpackages‚Äù connector, storing its latest recorded offsetIt is crucial to understand the following:A binlog contains changes from all the tables within MySQL, not only from the tables we‚Äôve configured the connector to capture changes",
            "metadata": {
                "source": "https://medium.com/payu-engineering/unlocking-the-power-of-debezium-69ce9170f101",
                "title": "Unlocking the Power of Debezium. This blog post aims to share our‚Ä¶ | by Tomer Guttman | payu-engineering | Aug, 2023 | Medium",
                "description": "This blog post aims to share our experience, and insights with Debezium (CDC Tool), and the development phases we went through to improve reliability, and observability while capturing data changes‚Ä¶",
                "language": "en"
            }
        }
    ]
}
```

as mentioned above, we will get the source_documeants in the response to help us debug why we got the answer we got and an answer to our question.

### Prompt Template
One of the features many FM supports is sending instructions to the model to answer your questions.  
Most of you saw chatGPT prompts that asks for a brief answer as a pirate.
While we can send these instructions to the FM we are working with, this instructions will go to RAG pattern
as well as they are part of the questions and will affect the vectors that are found.

To solve this we are happy to introduce the prompt_template:  

see this request for example:

```json
curl --location 'https://api.paymentsos.com/hackathon-ai/prompt' \
--header 'private-key: replaceme' \
--header 'app-id: replaceme' \
--header 'Content-Type: application/json' \
--data '{
    "prompt_template": "Answer the following questions as best you can, but speaking as a pirate might speak. Use lots of \"Arg\"s.\nBegin\n\nHuman: {question}\n\nContext: {context}\n\nAssistant:",
    "question": "What are the reasons a debezium connector loses its binlog offset?",
    "model": {
        "stop_sequences": [],
        "name": "anthropic.claude-v2",
        "max_tokens": 200,
        "temperature": 0.3,
        "top_k": 250,
        "top_p": 0.9
    },
    "rag": {
        "return_source_documents": true,
        "search_type": "similarity",
        "k": 5,
        "collection": "hackathon"
    }
}'
```  
And the response:

```json
{
    "question": "What are the reasons a debezium connector loses its binlog offset?",
    "result": " Arr matey, there be a few reasons a Debezium connector could be losin' its place in the binlog:\n\nFirst, if the MySQL server be restarted, that wipes the binlog clean and the connector won't know where it left off. So you gotta be careful not to restart the server if ye can help it, else the connector will start over from the beginning.\n\nSecond, network issues can cause the connector to lose its connection. If it can't keep talkin' to the MySQL server, it may miss some binlog events and lose track of the latest position. \n\nAnd third, sometimes the connector offset gets out o' sync if Debezium's heartbeat don't match up with the actual binlog position. That's why it be important to keep an eye on both and make sure they line up proper.\n\nBut don't be despairin'! There be ways to make sure your connector don't lose",
}
```

And if we take it more seriously, lets try this:  

```json
curl --location 'https://api.paymentsos.com/hackathon-ai/prompt' \
--header 'private-key: a4b90687-616f-4173-9774-af3cc6bd86bc' \
--header 'private-key: replaceme' \
--header 'app-id: replaceme' \
--header 'Content-Type: application/json' \
--data '{
    "prompt_template": "Answer the  briefly in bullets and as you were a sales person.\nBegin\n\nHuman: {question}\n\nContext: {context}\n\nAssistant:",
    "question": "What are the reasons a debezium connector loses its binlog offset?",
    "model": {
        "stop_sequences": [],
        "name": "anthropic.claude-v2",
        "max_tokens": 1000,
        "temperature": 0.3,
        "top_k": 250,
        "top_p": 0.9
    },
    "rag": {
        "return_source_documents": true,
        "search_type": "similarity",
        "k": 5,
        "collection": "hackathon"
    }
}'
```

we will get this

```json
{
    "question": "What are the reasons a debezium connector loses its binlog offset?",
    "result": " Here are some common reasons a Debezium connector can lose its binlog offset:\n\n- MySQL server is restarted - This causes the binlog to be rotated, so the connector loses its place\n\n- Connector is stopped for too long - If the connector is down for longer than the MySQL binary log expiration period, the logs it needs may be deleted\n\n- Heartbeat configuration issue - If heartbeats are not configured properly, the connector may not be updating its offset frequently enough\n\n- Kafka retention issues - If Kafka is not retaining the offset topic long enough, offset messages may be deleted \n\n- Connector configuration error - An error in the connector configuration, such as the database or table whitelist, can cause offset issues\n\n- MySQL GTID mode not enabled - Debezium requires GTID mode to accurately track binlog positions \n\n- DB snapshot followed by purge - If a snapshot is followed by a BINLOG PURGE, the connector's recorded offset may be purged\n\n- DB admin intervention - An admin may have manually purged logs, inadvertently deleting offsets \n\n- Kafka Connect rebalance - Offsets may not be committed properly during a Connect cluster rebalance\n\nSo in summary - restarting MySQL, long connector downtime, Kafka retention, Connect errors, and admin actions on the db can all lead to lost offsets. Proper connector setup and config can mitigate many issues.",}
```