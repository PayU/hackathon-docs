In the previous chapter we talked about how we can load Jarvis with documents from different sources.  
Now lets see how we can use the Jarvis API to ask question about these docs.


Let's start from our last example, we uploaded two different blog posts using the website loader.
One of the blog post was about debeizum - a CDC tool that is being used in GPO engineering. 

let's try to ask a question about that article that was written after the FM finished it's training

We will send a question that is very similar to the questions we send on the zero shot part, the difference in this
request is that it includes an object called the "**rag**"  
When specifying the rag object, Jarvis understand as it should take the query and first find the closest vectors to the question
and provide them as context to the FM.

in the rag object we have:  
* return_source_documents - if this is true, the response will contain the vectors that that Jarvis found as the closest one 
to the question and passed them as context to the model. this is useful for debugging and tuning our use of the models.  
* search_type - need to update  
* k - how many vectors should be retrieved from db 
* collection - on which collection of documents look for the vectors - this must be same collection where doc was loaded.


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

as mentioned above, we will the source_documeants in the response to help us debug why we got the answer we got and an answer to our question.




