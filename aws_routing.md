AWS Routing – Conceptual Overview (Beginner Level) (Hinglish Version)

1. Introduction

AWS me routing ka matlab hota hai incoming user requests ko correct server, service ya resource tak pahunchana predefined rules ke basis par. Ye ensure karta hai ki application traffic efficiently, reliably aur securely handle ho.

Simple words me routing decide karti hai:

User ki request kaha jayegi

Kaunsa server request process karega

Traffic kaise distribute hoga

Server down hone par kya action hoga

Routing scalable, high performance aur highly available cloud applications banane ke liye essential hai.

2. Routing ki Zarurat Kyun Hoti Hai

Modern applications me multiple services, servers aur regions hote hain. Agar routing na ho to sab requests ek hi resource par jayengi, jisse performance issues, system failure aur poor user experience ho sakta hai.

Routing use karne ke main reasons

Traffic management – Request ko correct service tak bhejna.

Performance optimization – User ko fastest server se connect karna.

High availability – System fail hone par backup use karna.

Scalability – Multiple servers par load distribute karna.

Service separation – Different applications ko efficiently manage karna.

Version testing – New application versions gradually release karna.

3. AWS me Routing Kahan Use Hoti Hai

AWS me routing different layers par use hoti hai:

Application Load Balancer (ALB) – HTTP/HTTPS requests ko rules ke basis par route karta hai.

Network Load Balancer (NLB) – Network level par traffic route karta hai.

Amazon Route 53 – DNS based global routing provide karta hai.

API Gateway – API requests ko backend services tak route karta hai.

4. Application Load Balancer (ALB) Routing

Application Load Balancer application layer (Layer 7) par kaam karta hai aur request ke content jaise URL path, domain, header ya query parameters ke basis par routing karta hai.

4.1 Path-Based Routing

Definition

URL path ke basis par traffic route karna.

Kaise kaam karta hai

Different URL paths ko different backend services se map kiya jata hai.

Example

example.com/login → Login service example.com/payment → Payment service example.com/profile → Profile service 

Use Cases

Microservices architecture

Large web applications

Service-based applications

Benefits

Services ka clear separation

Ek hi domain par multiple services run kar sakte hain

System organization better hota hai

4.2 Host-Based Routing

Definition

Domain ya subdomain ke basis par routing.

Kaise kaam karta hai

Different domains ya subdomains ko different backend services se connect kiya jata hai.

Example

api.company.com → API server blog.company.com → Blog server shop.company.com → E-commerce server 

Use Cases

Multiple websites ek hi infrastructure par

SaaS platforms

Multi-tenant applications

Benefits

Infrastructure cost reduce hoti hai

Resource utilization better hota hai

4.3 Header aur Query-Based Routing

Definition

HTTP headers ya query parameters ke basis par routing.

Kaise kaam karta hai

Request metadata jaise device type ya application version ke basis par routing hoti hai.

Use Cases

A/B testing

Canary deployment

Mobile vs desktop traffic handling

5. Network Load Balancer (NLB) Routing

Network Load Balancer network layer (Layer 4) par operate karta hai.

Definition

IP address aur port number ke basis par traffic route karta hai.

Example

Port 80 → Server A Port 443 → Server B 

Use Cases

High performance systems

Real-time applications

Low latency workloads

Characteristics

Bahut fast processing

High traffic handle kar sakta hai

ALB se kam intelligent routing

6. Amazon Route 53 Routing (DNS-Based Routing)

Amazon Route 53 AWS ka DNS service hai jo globally users ko applications se connect karta hai. Ye decide karta hai ki user kis server se connect karega.

6.1 Simple Routing

Definition

Traffic ko single resource tak route karta hai.

Use Case

Basic website hosting.

6.2 Weighted Routing

Definition

Traffic ko percentage ke basis par multiple resources me distribute karta hai.

Example

80% traffic → Old version 20% traffic → New version 

Use Case

New application version testing

Gradual deployment

6.3 Latency-Based Routing

Definition

User ko lowest latency (fastest) server se connect karta hai.

Use Case

Global applications

Performance optimization

6.4 Failover Routing

Definition

Primary resource fail hone par automatically backup resource use hota hai.

Use Case

High availability systems

Banking aur payment applications

6.5 Geolocation Routing

Definition

User ke geographic location ke basis par routing.

Use Case

Region specific content

Local services

Compliance requirements

6.6 Multi-Value Routing

Definition

Multiple healthy resources return karta hai taaki traffic distribute ho sake.

Use Case

Basic load distribution

Reliability improvement

7. API Gateway Routing

API Gateway API requests ko backend services jaise AWS Lambda ya application servers tak route karta hai.

Routing Criteria

URL path

HTTP method (GET, POST, PUT, DELETE)

API version

Use Case

Serverless applications

Backend service management

API version control

8. AWS Routing ke Business aur Technical Benefits

Application performance improve hoti hai

System availability high hoti hai

Traffic efficiently distribute hota hai

Better user experience milta hai

Fault tolerance aur reliability increase hoti hai

Large scale applications support karta hai

New features safely deploy karne me help karta hai

Global application delivery possible hoti hai

9. Conclusion

AWS routing cloud architecture ka ek fundamental component hai jo user requests ko efficiently aur reliably correct services tak pahunchata hai. Ye organizations ko scalable, highly available aur high-performance systems build karne me help karta hai.

Modern cloud applications, microservices architecture, global systems aur production environments me routing ka use stability, performance aur reliability ensure karne ke liye kiya jata hai.
