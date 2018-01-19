---
uid: com.microsoft.azure.eventhubs._partition_receiver.receive(final int)
summary: 'バッチで受け取る[EventData](/java/api/com.microsoft.azure.eventhubs._event_data) Event Hubs のパーティションからのオブジェクト。'
remarks: *content
---

次のサンプル コードは、API の同期バージョンを使用しますが、概念は、同じ。

```java
EventHubClient client = createFromConnectionStringSync(final String connectionString)("connection");
PartitionReceiver receiver = client.createPartitionReceiverSync("ConsumerGroup1", "1"); 
Iterable{<}EventData{>} receivedEvents = receiver.receiveSync();

while (true) 
{ 
    int batchSize = 0; 
    if (receivedEvents != null) 
    { 
        for(EventData receivedEvent: receivedEvents) 
        { 
            System.out.println(String.format("Message Payload: %s", new String(receivedEvent.getBytes(), Charset.defaultCharset()))); 
            System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", receivedEvent.getSystemProperties().getOffset(), receivedEvent.getSystemProperties().getSequenceNumber(), receivedEvent.getSystemProperties().getEnqueuedTime())); 
            batchSize++; 
        } 
    } 

    System.out.println(String.format("ReceivedBatch Size: %s", batchSize)); 
    receivedEvents = receiver.receiveSync(); 
} 
```
