# pulsar

## overview

tour of apache pulsar - https://www.youtube.com/watch?v=7h7hA7APa5Y

## messaging concepts

- producer

- consumer

- topics

- subscriptions

- pulsar broker 

- cursor

- Multiple consumers to a subscription. consumers always begin with the latest available unacked message). A consumer stores its location in a subscription using a cursor

- Once a subscription has been created, all messages will be retained by Pulsar, even if the consumer gets disconnected. 