GFG API DOCS: https://www.geeksforgeeks.org/software-testing/what-is-an-api/

## Introduction to API 
An API (Application Programming Interface) is a set of rules that allows different software applications to communicate and exchange data with each other. It acts as a bridge between systems, enabling one application to request services or information from another in a structured way.

APIs help different applications connect and work together smoothly.
They allow data sharing and communication without exposing internal system logic.
APIs are widely used in web applications, mobile apps, payment systems, and cloud services.

Example: A mobile banking app requests account details through an API. The API verifies the request, fetches data from the banking server, and returns the account information to the app.

## API Architectures 

The dominant API architectural styles used in software development are REST, GraphQL, gRPC, WebSockets, Webhooks, SOAP, and MQTT. Each style follows a specific set of design principles, communication rules, and data formats optimized for different engineering problems. [1, 2, 3, 4]  
Comparison of API Architectures 

| Architecture | Communication Model | Data Format | Main Advantage | Primary Use Case  |
| --- | --- | --- | --- | --- |
| REST | Synchronous (Request/Response) | JSON, XML, Text | Simple, uniform, highly cacheable | General web services, public CRUD APIs  |
| GraphQL | Synchronous (Request/Response) | JSON | Eliminates over/under-fetching data | Complex mobile and frontend apps  |
| gRPC | Synchronous or Streaming | Protobuf (Binary) | Ultra-fast performance, low latency | Microservices, inter-service mesh  |
| WebSockets | Bi-directional (Persistent) | JSON, Binary, Text | Continuous, low-latency live streaming | Live chat, financial tickers, gaming  |
| Webhooks | Asynchronous (Event-driven) | JSON, XML | Zero-polling required for event alerts | Third-party payment and CI/CD notifications  |
| SOAP | Synchronous (Protocol-based) | XML exclusively | Highly strict, built-in ACID security | Legacy enterprise banking, healthcare  |
| MQTT | Asynchronous (Pub/Sub) | Binary, JSON | Extremely lightweight for low bandwidth | IoT networks, smart home devices  |

Core Architectural Breakdowns 
1. REST (Representational State Transfer) REST remains the industry baseline for web services. It treats everything as a unique resource mapped to a standard URL endpoint. 

• How it works: Relies entirely on native HTTP verbs like , , , and . 
• Statelessness: Every request must carry its own authentication and context payload; the server retains no knowledge of previous calls. 
• Trade-off: Suffers from over-fetching or under-fetching, forcing applications to call multiple endpoints just to build one view. [1, 6, 10, 11]  

2. GraphQL GraphQL acts as a query language that shifts data control from the backend directly to the client application. 

• How it works: Uses a single URL endpoint. The client sends a declarative query schema defining precisely which database fields it needs. 
• Efficiency: The server responds with only those exact fields in a neatly matched JSON tree structure. 
• Trade-off: Highly complex to implement, difficult to implement native HTTP caching, and vulnerable to malicious deeply nested queries. [1, 4, 6, 10]  

3. gRPC (Google Remote Procedure Call) gRPC is a high-performance framework optimized for massive data throughput within localized infrastructures. 

• How it works: Uses a contract-first approach with Protocol Buffers ( files) to strictly define API capabilities. 
• Transport: Operates over HTTP/2, unlocking multiplexed connection channels and bidirectional streaming capabilities. 
• Trade-off: Difficult to consume natively inside web browsers; primarily reserved for backend-to-backend infrastructure. [1, 6, 10, 11, 12]  

4. WebSockets WebSockets break out of the standard client-asks-server-answers cycle to establish a true open-ended highway. 

• How it works: Initiates a standard HTTP request that actively upgrades to a persistent, long-lived TCP connection. 
• Real-time interaction: Client and server can push messages instantly across the same wire without payload negotiation overhead. 
• Trade-off: State management is heavy; connections must be carefully tracked, demanding robust vertical and horizontal infrastructure scaling. [4, 7, 9, 10]  

5. Webhooks Webhooks are structurally inverted APIs—often defined by developers as a Reverse API style. 

• How it works: Instead of a client polling a server repeatedly to look for an update, the client gives the server a callback URL. 
• Event-driven: When a target event triggers, the server executes an HTTP  action sending the state payload directly to the client. 
• Trade-off: Relies on the client system being online to accept notifications; requires strict retry logic if a receiver goes offline. [3, 6, 10, 12]  

6. SOAP (Simple Object Access Protocol) SOAP is a highly rigorous, formal contract protocol rather than a flexible architectural style. 

• How it works: Uses strict WSDL documents (Web Services Description Language) to predefine absolute execution schemas. 
• Security & Compliance: Built natively with WS-Security and built-in transaction integrity constraints (ACID compliance). 
• Trade-off: Very verbose payload overhead due to XML nesting, slower processing speeds, and strict management overhead. [1, 6, 9, 10]  

7. MQTT (Message Queuing Telemetry Transport) MQTT is an ultra-lean architecture built entirely around a Publish/Subscribe pattern. 

• How it works: Client nodes do not talk to each other; instead, they publish messages to an intermediary broker under targeted "topics." 
• Constrained environments: Explicitly coded to run reliably over unstable networks with small processing microchips. 
• Trade-off: Lacks built-in high-level security models without extra encryption wrappers, and isn't built to transport large media or payloads. [3, 6]  

Are you designing a system and trying to choose an architecture for a specific use case, or would you like to see a code implementation example for one of these options? 
AI responses may include mistakes.

[1] https://www.geeksforgeeks.org/software-testing/what-is-an-api/ </br>
[2] https://www.youtube.com/watch?v=5Gaz2Q-CKqE </br>
[3] https://medium.com/@ghadageankita1111/api-architectures-the-8-most-commonly-used-api-protocols-8dd5b2410d81 </br>
[4] https://medium.com/@sumuduliyan/which-api-architecture-should-you-use-lets-break-it-down-6492e219dd69 </br>
[5] https://blog.postman.com/different-types-of-apis/ </br>
[6] https://medium.com/towardsdev/6-api-styles-every-backend-developer-should-know-e37e149c0a40 </br>
[7] https://openapi.com/blog/top-6-api-architectures </br>
[8] https://www.lobstersoftware.com/en/blog/api-architecture-explained-for-non-developers/ </br>
[9] https://medium.com/@anujguptaninja/what-are-the-main-api-architecture-styles-5304ff71c92d </br>
[10] https://www.youtube.com/watch?v=zbseFpr0WiE </br>
[11] https://www.linkedin.com/posts/nikkisiapno_6-api-architecture-styles-you-should-know-activity-7426531006679150593-g4VH </br>
[12] https://nordicapis.com/the-top-api-architectural-styles-of-2025/ </br>

