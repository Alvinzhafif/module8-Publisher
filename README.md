# Answering Questions
### 1. How many data your publisher program will send to the message broker in one run?
The publisher program will send 5 message in one run, this is determined by this code
```
= p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "1".to_owned(), user_name: "2206046632-Amir".to_owned() });
= p.publishevent("user_created".to_owned(), UserCreatedEventMessage { user_id: "2".to_owned(), user_name: "2206046632-Budi".to_owned() });
= p.publishevent("user_created".to_owned(), UserCreatedEventMessage { user_id: "3".to_owned(), user_name: "2206046632-Cica".to_owned() });
= p.publishevent("user_created".to_owned(), UserCreatedEventMessage { user_id: "4".to_owned(), user_name: "2206046632-Dira".to_owned() });
= p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "5".to_owned(), user_name: "2206046632-Emir".to_owned() });
```
Here, we can see there will be 5 events created, each for Amir, Budi, Cica, Dira, and Emir. Hence, in each run it will send out 5 of these datas.
### 2. The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriberprogram, what does it mean?
This means that both the subscriber and publisher would be accessing the same AMQP message broker. This would allow both programs to communicate with each other through the message broker and exchange messages
### Running RabbitMQ
![img.png](img.png)