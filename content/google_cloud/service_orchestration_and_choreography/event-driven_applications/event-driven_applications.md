◀️ [Home](../../../../README.md)


## Event-driven Applications
An event is a record of something that has happened. Examples of events are an employee logging in to an application or a product being added to a shopping cart. When we are discussing event-driven architectures, there are other important attributes of events.

First, an event is typically treated as an immutable fact. It's an historical record of an occurrence, and it should not be modified or deleted. Second, an event can be generated even if it's never consumed. Many applications that produce events don't know whether the events they generate are ever consumed. Third, an event can be persisted indefinitely, and can be consumed as many times as necessary. A single event can be consumed by many services, allowing event processing to occur in parallel.

As discussed earlier, the "spider web" of point-to-point communication between microservices can be a challenge. Point-to-point communication tends to introduce coupling between the microservices. **An event-driven architecture inserts an event intermediary between the services**. When a service acts as an event producer, it sends events to the intermediary. It isn't necessary for the service to know anything about the services that are consuming the events. A service can also act as an event consumer, receiving events from the intermediary. Event consumers understand how to handle an event.

### Benefits
A centralized event service can simplify the auditing and control for a distributed application.
- A log of immutable events can be used for auditing purposes.
- A centralized event service can also help you control access to particular services and data.

Producer and consumer of an event are decoupled.
- Services can create an event without having to send direct requests to any services that consume the event.
- Services can also consume an event without knowing anything about the service that generated the event.
- There's no point-to-point spider web of communication: Each event travels through the event intermediary, which routes the event to the correct consumer or consumers.

Microservices applications are sometimes designed to use synchronous request/response calls.
- The health of a service is affected by the health of the services that it calls, directly or indirectly. When a single service fails... ...it can bring down your entire application.
- **With an event-based architecture, events are generated asynchronously, and events are created without waiting for a response.**: An architecture can be designed to survive the temporary loss of a service. Events sent to the unhealthy service can be replayed or redelivered when the service comes back up.

When services are asynchronous, push-based messaging allows clients to receive updates without needing to continuously poll remote services. When using a polling model, consumers must continuously poll to determine when there's work to be done. Polling typically leads to increased network I/O and unnecessary delays in processing. **A push-based model allows consumers to automatically be notified when there are events to be consumed.**


