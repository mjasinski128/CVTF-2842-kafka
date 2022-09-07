
DIGIT-DATA-SERVICES@ec.europa.eu and GRAZZINI Jacopo (DIGIT) Jacopo.GRAZZINI@ec.europa.eu

 Nicolas Kyriazopoulos-Panagiotopoulos

servce bus queues

- like mqtt, rabbitMQ
- async


event grid

- decoupling
- topics
- subcribe to topic

- each subscription gets a copy of message
- filters on subscription

- has message storage/retention


starage queues

- inexpensive
- good enough

- async
- multi producer to async queue

- diff vs service bus:
  - service bus more instant
  - sq - polling - so slower

  - can function be subscribed??

- easy to change to service bus in app (similar api)

 
Event Hub

- based on service bus
- works like quques
- intent for real-time processing

- no persistance
-  

IoT hub

-  C2D / D2C is based on topics

Notification hub

- interface for APN / Firebase / Windows
- 

SignalR 

- bi-directional communication
- HTTP 

- abstraction websockets/long-pooling
- microsoft specific

- service to help req/response services deal with long lived polling/sockets protocol

SendGrid

- emails
-  

