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
### Subscriber Console Output
![img_1.png](img_1.png)
Here the output is produced through the communication between the publisher and subscriber, the publisher send the created user message output to the subscriber. Then, the subscriber handles the output using the handler `UserCreatedHandler`. This is achieved by using an **AMQP** broker running locally through localhost at port `5672` and using an instance of `CrosstownBus` for facilitating the communication between the two. Here's the explanation in detail:
* **Publisher Program**
    <br>
    In order to publish messages to the designated AMQP endpoint (amqp://guest:guest@localhost:5672), it first constructs a CrosstownBus instance.
    The publish_event method of the CrosstownBus instance is then used to publish messages to the designated queue (user_created in this example).
* **Subscriber Program**
    <br>
  In order to listen for messages on the same AMQP endpoint (amqp://guest:guest@localhost:5672), it first constructs a CrosstownBus instance.
  It uses the listen method of the CrosstownBus class to build a subscriber for a specific queue (user_created in this example). Incoming messages can be processed by a handler (UserCreatedHandler) that it offers.
  The subscriber program receives messages on the designated queue, retrieves them, and forwards them to the handler (UserCreatedHandler in this example) for further processing.
* **Message Broker**
    <br>
  The publisher and the subscriber are connected through the message broker (AMQP server). It conveys messages to the subscriber after receiving them from the publisher.
  Even in the event that the publisher and subscriber are not operating concurrently, it guarantees that messages are consistently sent from the publisher to the subscriber.
### RabbitMQ Chart
![img_2.png](img_2.png)
The change in graph is caused when we run the publisher to interact with the subscriber, it mainly occurs when the subscriber received the data sent from the publisher, in this case it is the user created message. The spike is caused mainly due to the size of the data, here our publisher will send out 5 user created data batch at once, this causes the sudden spike compared to when the subscriber has not received anything. Additionally, in the graph picture there is a period where it stayed still at the `1.0/s` threshold, that is caused because I tried to run the publisher over and over again in a short interval.
### Slow Subscriber Simulation
![img_4.png](img_4.png)
The amount of queue message in my case here is 31, this is caused because we set the subscriber to delay the thread by ten millisecond. Such that when massive amount of events or in this case we run the publisher multiple times, some packet would fall into the queueing list. The amount of queue in total can vary as well depending on how many times we run the publisher program.
