# Message Queues vs. Event Streams

> **TL;DR:** A **message queue** is a to-do list: once a task is done, it is removed. An **event stream** is a logbook: events are recorded in order and stay there, so anyone can read them now or replay them later.

---

## The Simple Idea

Both help parts of a system talk to each other without waiting for each other (asynchronous communication). The big difference is **what happens to a message after it is read**.

| | Message Queue | Event Stream |
|---|---|---|
| **Think of it as** | A to-do list | A diary or logbook |
| **Analogy** | A ticket line at a help desk. Each ticket is handled by one agent, then thrown away. | A security camera recording. Everything is recorded in order, many people can watch it live, and you can rewind. |
| **After reading** | Message is removed | Event stays |

---

## What Is a Message Queue?

A message queue is a temporary holding area. A sender puts a message in, and a receiver takes it out later and processes it.

### Key characteristics

- **Asynchronous communication:** the sender and receiver don't have to be online at the same time. This makes the system more flexible and easier to scale.
- **Message durability:** messages stay in the queue until they are successfully processed, so nothing is lost even if the receiver is temporarily down.
- **Load balancing:** messages are shared across several consumers, so work is spread out and total throughput improves.

### Common implementations

- **RabbitMQ:** a popular open-source message broker that supports many messaging protocols.
- **Apache ActiveMQ:** an open-source broker with features for messaging and integration patterns.
- **Amazon SQS (Simple Queue Service):** a fully managed queue from AWS, known for scalability and ease of use.

---

## What Is an Event Stream?

An event stream is a continuous flow of events that are captured, processed, and consumed in real time. Instead of separate messages to be done and discarded, you think about an ongoing, ordered flow of things that happened.

### Key characteristics

- **Real-time processing:** built for cases where you must react to things as they happen, such as live monitoring.
- **Event sourcing:** changes in state are saved as a sequence of events that can never be edited (immutable). You can replay them to rebuild the system's state at any point in time.
- **Scalability:** built for very high volumes of events.

### Common implementations

- **Apache Kafka:** a distributed streaming platform known for high throughput and fault tolerance.
- **Amazon Kinesis:** AWS's managed service for real-time data streaming and analytics.
- **Apache Pulsar:** a cloud-native streaming platform designed for high throughput and low latency.

---

## Main Differences, One by One

### 1. Main purpose
- **Queue:** sharing out tasks and asynchronous communication.
- **Stream:** real-time data processing and event-driven systems.

### 2. Message delivery
- **Queue:** usually guarantees each message is delivered exactly once or at least once.
- **Stream:** focuses on high speed and low delay, and may use "at least once" or "at most once" delivery.

| Delivery type | Meaning |
|---|---|
| **At least once** | Never lost, but may arrive twice |
| **At most once** | Never duplicated, but may be lost |
| **Exactly once** | Neither lost nor duplicated (hard to do in practice) |

### 3. Ordering
- **Queue:** keeps order within a single queue.
- **Stream:** keeps order only inside one partition (one slice of the stream), not across the whole stream.

### 4. Who consumes the data
- **Queue:** one consumer takes each message, and it is then removed.
- **Stream:** many consumers can read the same events, and the stream stays available.

### 5. Storage
- **Queue:** messages are kept until consumed or expired.
- **Stream:** events are kept as part of a permanent, unchangeable log.

### 6. Replay
- **Queue:** usually not possible. Once consumed, a message is gone.
- **Stream:** built for it. You can re-read events or start again from the beginning.

### 7. Scaling
- **Queue:** add more queues or more consumers.
- **Stream:** split the stream into partitions and spread the partitions across consumers.

### 8. State management
- **Queue:** you often need extra tools to track progress and state.
- **Stream:** state tracking is often built into the processing system.

### 9. Delay (latency)
- **Queue:** generally low.
- **Stream:** can be very low for ingesting and processing events.

---

## Use Cases of Message Queues

- **Background job processing:** RabbitMQ and SQS handle slow work, such as processing user requests, without making the user wait.
- **Task scheduling:** tasks that must run at set times, such as sending notifications or backing up data.
- **Order processing:** in e-commerce, each order is handled reliably and in sequence, even in busy periods.
- **Microservices communication:** services talk through queues, work independently, and handle failures gracefully.

## Use Cases of Event Streams

- **Real-time analytics:** Kafka analyzes huge volumes of data live, such as user behavior or financial transactions.
- **Event sourcing:** every change in state is saved as an immutable event, allowing accurate replay and auditing.
- **Log aggregation:** logs from many sources are gathered in one place for live analysis and monitoring.
- **Fraud detection:** banks watch transaction patterns as they happen to spot fraud quickly.

---

## Conclusion of the Article

Both manage asynchronous communication in distributed systems. Queues are best for reliable task processing and decoupled communication. Event streams are best for real-time data processing and analytics.

---

## Important Points the Article Leaves Out

- **The line between them is blurry.** Kafka consumer groups can act like a queue, because each message in a group goes to one consumer. RabbitMQ also has a stream feature. SQS FIFO queues keep strict order. So "queue or stream" is about which model fits best, not strict categories.
- **Offsets.** In a stream, the system doesn't delete events when you read them. Each consumer keeps a bookmark (an **offset**) showing how far it has read. To replay, you move the bookmark back.
- **Retention.** Streams don't keep events forever by default. You set how long to keep them, such as 7 days or 30 days, or keep them indefinitely at higher storage cost.
- **Ordering by key.** In Kafka, events with the same key (for example the same user ID) go to the same partition, so that user's events stay in order. This is how you get useful ordering at scale.
- **Exactly once is hard.** The article says queues guarantee "exactly once or at least once." In practice, most systems give at least once, so you should still make your code **idempotent** (processing the same message twice does no harm).
- **Dead letter queues (DLQ)** are standard in queues. They hold messages that keep failing so you can inspect them later. In streams you usually handle bad events in your processing code or a separate error topic.
- **Queues are easier to run.** Streaming platforms like Kafka are more powerful but harder to set up, operate, and tune. Don't use Kafka for a simple background job.
- **Queues remove the message, streams keep it.** This is the one difference to remember. If you might ever need to replay, audit, or let a new service read old events, use a stream.
- **Connection to earlier topics:** a queue is point-to-point (one receiver per message), while a stream behaves like publish-subscribe with memory (many receivers, and late joiners can catch up).

---

## When to Use Which

### Use a message queue when
- A task should be done by exactly one worker.
- You need background jobs, task scheduling, or order processing.
- You want simple, reliable delivery without replay.

### Use an event stream when
- Many services need to read the same events.
- You need real-time analytics, fraud detection, or log collection.
- You need to replay, audit, or rebuild state from history.
- You handle very large volumes of data.

### Use both when
- A stream carries events between systems, and queues sit behind it to hand out the actual work to workers.

---

## Quick Summary Table

| Feature | Queue | Stream |
|---|---|---|
| **Think of it as** | To-do list | Logbook or recording |
| **After reading** | Message is removed | Event stays |
| **Consumers** | One per message | Many can read the same event |
| **Replay** | Usually no | Yes |
| **Ordering** | Within the queue | Within a partition |
| **Best for** | Tasks and jobs | Real-time data and history |
| **Tools** | RabbitMQ, SQS, ActiveMQ | Kafka, Kinesis, Pulsar |
