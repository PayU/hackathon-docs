## Chat & RAG
In the preceding section, we covered prompts with RAG.

Now, let's explore how the Jarvis API can be utilized for chatting with RAG in this section.

> _NOTE:_  Chat is supported for anthropic.claude-v2 model only

Through the website loader, we uploaded two distinct blog posts. One of these posts dived into debeizum, a CDC tool that is widely used in GPO engineering.  
The FM are stateless and don't know anything about the previous question we send them.
Jarvis adds this functionality by persisting the previous questions and passing them to the model once again on every new question being asked.

The structure of the chat object is as follows:

```json
 "chat": {
    "conversation_id": "21389712387", // The conversation_id to based on when asking the question
    "k_history": 10, // The k last history messages (request&answer) to send to the model as part of the context
    "return_history": true, // Return the history messages used for answer from the LLM in the response
    "rephrase_question": false // Whether to use LLM to rephrase the question based on the history before trying to retrieve documents
}
```
Let's start our chat about that article that was written after the FM finished its training

```json
curl --location --request POST 'https://api.paymentsos.com/hackathon-ai/chat' \
--header 'private-key: replaceme' \
--header 'app-id: replaceme' \
--header 'Content-Type: application/json' \
--data-raw '{
    "question": "What are the reasons a debezium connector loses its binlog offset?",
    "model": {
        "stop_sequences": [],
        "name": "anthropic.claude-v2",
        "max_tokens": 5000,
        "temperature": 0.3,
        "top_k": 250,
        "top_p": 0.9
     },
    "rag": {
        "search_type": "similarity",
        "k": 5,
        "collection": "hackathon",
        "return_source_documents": false
      },
    "chat": {
        "conversation_id": "222222222222",
        "k_history": 10,
        "return_history": true,
        "rephrase_question": false
      }
    }'
```
And the result will be:

```json
{
  "question": "What are the reasons a debezium connector loses its binlog offset?",
  "result": " Based on the context provided, there are a few potential reasons a Debezium connector can lose its binlog offset:\n\n- The connector was down for an extended period of time, during which the MySQL binlog rotated. When the connector comes back up, it tries to resume from its last recorded offset, but that offset no longer exists in the current binlog.\n\n- The connector's configuration does not have heartbeat messages enabled. Heartbeat messages periodically store the connector's latest offset, protecting against offset loss on connector restart.\n\n- There is a misconfiguration or bug that causes the connector to not properly store its latest offset in Kafka. This can lead to the offset being lost when the connector is restarted.\n\n- If Kafka offsets topic retention is too short, offset messages can be deleted before the connector has a chance to resume from them.\n\nSo in summary, the main reasons are binlog rotation during an outage, lack of heartbeat messages, improper offset storage, and short retention of offset messages. Enabling heartbeat messages and configuring appropriate retention is key to avoiding offset loss.",
  "source_documents": null,
  "chat_history": []
}
```

Now, let's ask another question using what we learned from the first one:

```json
curl --location --request POST 'https://api.paymentsos.com/hackathon-ai/chat' \
--header 'private-key: replaceme' \
--header 'app-id: replaceme' \
--header 'Content-Type: application/json' \
--data-raw '{
  "question": "How can i solve the first potential reason?",
    "model": {
        "stop_sequences": [],
        "name": "anthropic.claude-v2",
        "max_tokens": 5000,
        "temperature": 0.3,
        "top_k": 250,
        "top_p": 0.9
    },
    "rag": {
        "search_type": "similarity",
        "k": 5,
        "collection": "hackathon",
        "return_source_documents": false
    },
    "chat": {
        "conversation_id": "222222222222",
        "k_history": 10,
        "return_history": true,
        "rephrase_question": false
    }
}'
```

And the answer is:

```json
{
  "question": "How can i solve the first potential reasons?",
  "result": " Based on the context provided, here are a few potential ways to solve the issue of a Debezium connector losing its binlog offset due to binlog rotation:\n\n1. Increase the binlog retention period in MySQL to be longer than the maximum expected downtime. This will ensure the binlog containing the connector's last offset is still available when the connector restarts.\n\n2. Configure the connector to also capture changes from a heartbeat table. This will update the connector's offset regularly even if there are no changes to the main table being captured. \n\n3. When restarting the connector after downtime, manually provide the last known good offset the connector had before going down. This can be obtained from the Kafka offsets topic.\n\n4. Use MySQL GTIDs to track replication positions instead of binlog offsets. GTIDs are persisted in MySQL and not subject to rotation.\n\n5. Set up MySQL replication from the primary to a secondary and have the connector read the binlogs from the secondary instead. This prevents binlog rotation from affecting the connector.\n\n6. Increase the frequency of connector offset flushing to Kafka to minimize potential data loss in case of failure.\n\nThe key is to ensure the connector's last known offset is still available or can be reconstructed after any downtime event. Let me know if you need any clarification or have additional questions!",
  "source_documents": null,
  "chat_history": [
    {
      "content": "What are the reasons a debezium connector loses its binlog offset?",
      "additional_kwargs": {},
      "example": false
    },
    {
      "content": " Based on the context provided, there are a few potential reasons a Debezium connector can lose its binlog offset:\n\n- The connector was down for an extended period of time, during which the MySQL binlog rotated. When the connector comes back up, it tries to resume from its last recorded offset, but that offset no longer exists in the current binlog.\n\n- The connector's configuration does not have heartbeat messages enabled. Heartbeat messages periodically store the connector's latest offset, protecting against offset loss on connector restart.\n\n- There is a misconfiguration or bug that causes the connector to not properly store its latest offset in Kafka. This can lead to the offset being lost when the connector is restarted.\n\n- If Kafka offsets topic retention is too short, offset messages can be deleted before the connector has a chance to resume from them.\n\nSo in summary, the main reasons are binlog rotation during an outage, lack of heartbeat messages, improper offset storage, and short retention of offset messages. Enabling heartbeat messages and configuring appropriate retention is key to avoiding offset loss.",
      "additional_kwargs": {},
      "example": false
    }
  ]
}
```

We observe that the chat history, which was utilized to feed the LLM, returns in the response under "chat_history."

```json
"chat_history": [
{
    "content": "What are the reasons a debezium connector loses its binlog offset?",
    "additional_kwargs": {},
    "example": false
},
{
    "content": " Based on the context provided, there are a few potential reasons a Debezium connector can lose its binlog offset:\n\n- The connector was down for an extended period of time, during which the MySQL binlog rotated. When the connector comes back up, it tries to resume from its last recorded offset, but that offset no longer exists in the current binlog.\n\n- The connector's configuration does not have heartbeat messages enabled. Heartbeat messages periodically store the connector's latest offset, protecting against offset loss on connector restart.\n\n- There is a misconfiguration or bug that causes the connector to not properly store its latest offset in Kafka. This can lead to the offset being lost when the connector is restarted.\n\n- If Kafka offsets topic retention is too short, offset messages can be deleted before the connector has a chance to resume from them.\n\nSo in summary, the main reasons are binlog rotation during an outage, lack of heartbeat messages, improper offset storage, and short retention of offset messages. Enabling heartbeat messages and configuring appropriate retention is key to avoiding offset loss.",
    "additional_kwargs": {},
    "example": false
}
]
```