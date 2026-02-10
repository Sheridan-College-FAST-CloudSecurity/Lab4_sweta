# Lab4_sweta
Part 1: Research and Document Application Architectures

1. Research Traditional Architectures:

a) Single-Machine Webserver Architecture
Key Components and Their Responsibilities:
1.	Web Server Software (Apache/Nginx): Handles HTTP/HTTPS requests and serves static content
2.	Application Runtime (PHP/Python/Node.js): Executes business logic and dynamic content generation
3.	Database (MySQL/PostgreSQL): Stores and retrieves application data
4.	Operating System (Linux/Windows): Provides the foundation for all software to run
5.	File System: Stores application code, uploads, and configuration files
Benefits:
•	Simplicity: Easiest to set up - just install everything on one machine
•	Low Cost: Requires only one server (physical or virtual)
•	Easy Maintenance: Only one system to update, monitor, and backup
•	Low Latency: All components communicate locally via localhost (no network delays)
•	Quick Deployment: Can be running in minutes with pre-configured stacks like XAMPP/WAMP
Limitations:
•	Single Point of Failure: If the server crashes, everything goes down
•	Limited Scalability: Can only scale vertically (bigger machine), not horizontally
•	Resource Contention: CPU, memory, and disk I/O competition between components
•	Security Risks: Database exposed on same machine as web server
•	Maintenance Downtime: Updates require taking entire application offline
Real-World Examples:
This pattern is commonly found in small business websites, such as local restaurants or retail shops that use platforms like WordPress on shared hosting. It is also typical for personal blogs and portfolios. Historically, many successful companies started this way; for instance, Facebook initially ran on a single server at Harvard University before scaling. University student projects and internal tools for small teams often use this architecture due to its simplicity.
Typical Use Cases Where This Architecture Excels:
The Single-Machine Webserver excels in scenarios with low traffic and minimal complexity. It is ideal for personal projects, prototypes, and proof-of-concept applications where rapid deployment is needed. Development and testing environments benefit from this setup because it mirrors a production-like environment without the overhead. Small business brochure websites with predictable, low visitor counts (under 1,000 daily users) are well-suited. Educational purposes, such as teaching web development fundamentals, also commonly use this architecture.
Scenarios Where This Architecture Would Be a Poor Choice:
This architecture is unsuitable for high-traffic or mission-critical applications. E-commerce sites during holiday sales or promotional events would likely crash under load. Applications expecting viral growth or unpredictable traffic spikes cannot rely on a single server. Systems requiring high availability (99.9% uptime or more), such as online banking or financial services, need redundancy that this architecture lacks. Environments where frequent updates or maintenance must occur without downtime are also poorly served, as taking the single server offline affects the entire application.




b) Three-Tier Web Service Architecture

Key Components and Their Responsibilities:
Presentation Tier (Tier 1):
•	Load Balancer: Distributes incoming traffic across multiple web servers
•	Web Servers: Handle HTTP requests, serve static files, forward dynamic requests to application tier
•	SSL Terminator: Manages HTTPS encryption/decryption
Application Tier (Tier 2):
•	Application Servers: Execute business logic (Node.js, Java, Python apps)
•	Business Logic: Core application functionality
•	API Endpoints: Interface for presentation tier communication
Data Tier (Tier 3):
•	Database Server: Primary data storage (SQL or NoSQL)
•	Database Management System: MySQL, PostgreSQL, MongoDB, etc.
•	Backup System: Regular database backups
Benefits:
•	Scalability: Each tier can scale independently
•	Better Fault Isolation: Failure in one tier doesn't necessarily bring down others
•	Specialization: Each tier can be optimized for its specific purpose
•	Security: Database isolated in private network
•	Team Separation: Different teams can work on different tiers
•	Flexible Technology Choices: Different technologies per tier
Limitations:
•	Increased Complexity: More components to configure and maintain
•	Network Latency: Communication between tiers adds overhead
•	Higher Cost: Requires multiple servers/instances
•	Deployment Complexity: Coordinating updates across tiers
•	More Points of Failure: More components that can fail
•	Harder to Debug: Issues can occur in any tier
Real-World Examples:
This architecture is widely used in medium to large-scale web applications. E-commerce platforms like Shopify often employ a three-tier structure to handle product catalogs, shopping carts, and user accounts. University learning management systems (e.g., Canvas or Blackboard) use it to separate frontend interfaces, application logic, and student databases. Many SaaS products (Software as a Service), such as project management tools or CRM systems, adopt this pattern. Online banking applications for regional banks frequently implement three-tier architectures to ensure security and scalability.
Typical Use Cases Where This Architecture Excels:
The Three-Tier architecture excels in business applications with moderate to high user concurrency. It is well-suited for enterprise web applications like customer relationship management (CRM) systems, enterprise resource planning (ERP) software, and content management systems (CMS). Online marketplaces and e-commerce sites benefit from the ability to scale the web tier during sales events and the application tier for order processing. Educational platforms and online learning tools that need to serve thousands of simultaneous users also find this architecture effective. Applications requiring clear separation between user interface, business logic, and data storage for maintainability and team workflow are ideal candidates.
Scenarios Where This Architecture Would Be a Poor Choice:
For simple static websites or blogs with very low traffic, a three-tier setup is overkill and unnecessarily complex and costly. Real-time applications requiring ultra-low latency, such as multiplayer online games or financial trading platforms, may suffer from the network hops between tiers. Internet of Things (IoT) applications that process millions of small messages from devices might find the tiered structure inefficient. Projects with extremely tight budgets or timelines, like hackathon prototypes or MVP (Minimum Viable Product) versions that need to be built in hours, would be better served by simpler architectures. Serverless or Platform-as-a-Service (PaaS) scenarios, where the goal is to minimize infrastructure management, also do not align well with the operational overhead of a three-tier system.

c) Reverse Proxy Architecture

Key Components and Their Responsibilities:
Reverse Proxy Layer:
•	Reverse Proxy Server: Nginx, HAProxy, or Apache with mod_proxy
•	Load Balancer: Distributes requests to backend servers
•	SSL Terminator: Handles HTTPS encryption
•	Caching Layer: Caches static content and API responses
Backend Layer:
•	Application Servers: Multiple servers running the actual application
•	Web Servers: Apache, Nginx serving the application
•	Session Stores: Redis/Memcached for shared session storage
Benefits:
•	Improved Security: Hides backend servers, provides DDoS protection
•	Load Distribution: Evenly spreads traffic across multiple servers
•	SSL Termination: Offloads CPU-intensive encryption/decryption
•	Caching: Reduces load on backend servers
•	Compression: Reduces bandwidth usage
•	Single Point of Entry: Simplified DNS and certificate management
•	A/B Testing Support: Can route traffic to different backends
Limitations:
•	Additional Latency: Extra hop adds 5-50ms depending on setup
•	Single Point of Failure: If proxy fails, entire system is down (unless clustered)
•	Configuration Complexity: Need to configure proxy rules properly
•	Potential Bottleneck: Proxy can become overloaded
•	Debugging Complexity: Harder to trace requests through proxy
Real-World Examples:
This pattern is fundamental to many high-traffic websites and services. GitHub uses Nginx as a reverse proxy to manage traffic to its backend services. Netflix employs the Zuul gateway as a reverse proxy and API gateway for its microservices architecture.  WordPress.com utilizes Nginx reverse proxies in front of pools of PHP-FPM application servers to handle millions of blogs. Reddit relies on HAProxy for load balancing across its application servers. Content Delivery Networks (CDNs) like Cloudflare essentially act as globally distributed reverse proxies, caching content close to users and protecting origin servers.
Typical Use Cases Where This Architecture Excels:
Reverse Proxy Architecture excels in high-traffic websites and web applications that require reliable load distribution and DDoS mitigation. It is ideal for modern web applications using microservices, where an API gateway (a form of reverse proxy) routes requests to appropriate backend services. Legacy application modernization projects often place a reverse proxy in front of an old monolithic application to add security, caching, and load balancing without rewriting the entire codebase. Global applications that integrate with a CDN benefit from the proxy managing origin shielding and cache control headers. Systems requiring flexible deployment strategies (like blue-green or canary deployments) use the proxy's routing capabilities to direct user traffic.
Scenarios Where This Architecture Would Be a Poor Choice:
For a simple, low-traffic personal blog or project website, introducing a reverse proxy adds unnecessary complexity and cost. Local development environments where simplicity and fast iteration are key typically do not need a reverse proxy layer. Applications where every millisecond of latency is critical, such as high-frequency trading platforms or real-time competitive gaming servers, might avoid the extra hop. Small teams with limited operations or DevOps expertise might struggle with the configuration and maintenance overhead. Projects with extremely constrained budgets that cannot afford the additional compute instance for the proxy would find this architecture impractical.

2. Research Modern Cloud Architectures:

1. Microservices Architecture
Key Components and Their Responsibilities:
Service Layer:
•	Independent Services: Each service handles specific business capability
•	Container Runtime: Docker containers for each service
•	Orchestration: Kubernetes/ECS for managing containers
Communication Layer:
•	API Gateway: Single entry point for all client requests
•	Service Discovery: Consul/Eureka for finding services
•	Message Queue: RabbitMQ/Kafka for async communication
•	Event Bus: For event-driven communication
Data Layer:
•	Database per Service: Each service has its own database
•	Polyglot Persistence: Different databases for different needs
•	Data Replication: For data sharing between services
Operational Layer:
•	Centralized Logging: ELK stack or CloudWatch Logs
•	Distributed Tracing: Jaeger or AWS X-Ray
•	Monitoring: Prometheus or Datadog
•	CI/CD Pipeline: Automated deployment per service
Benefits:
•	Independent Deployment: Update one service without affecting others
•	Technology Flexibility: Each service can use different technologies
•	Scalability: Scale only the services that need it
•	Fault Isolation: One service failure doesn't bring down entire system
•	Team Autonomy: Small teams own entire services
•	Faster Development: Teams can work in parallel
Limitations:
•	Distributed System Complexity: Network issues, latency, partial failures
•	Data Consistency: Hard to maintain ACID transactions
•	Operational Overhead: Many more components to monitor and maintain
•	Testing Complexity: Need to test interactions between services
•	Network Overhead: Many network calls between services
•	Skill Requirements: Need expertise in distributed systems
Real-World Examples:
•	Netflix: Pioneer in microservices, thousands of services
•	Uber: Each functionality (payment, dispatch, etc.) is separate service
•	Spotify: Hundreds of services for music streaming
•	Amazon: Even their "Add to Cart" is a separate service
•	Twitter: Migrated from monolith to microservices
AWS Services for Implementation:
•	Compute & Orchestration: Amazon ECS (Elastic Container Service), Amazon EKS (Elastic Kubernetes Service), AWS Fargate (serverless containers).
•	API Management: Amazon API Gateway for routing and API management.
•	Service Discovery & Networking: AWS Cloud Map, Elastic Load Balancing, Amazon VPC.
•	Messaging & Events: Amazon SQS (queues), Amazon SNS (pub/sub), Amazon EventBridge (event bus).
•	Databases: Amazon RDS (for SQL), Amazon DynamoDB (for NoSQL), Amazon ElastiCache (for caching).
•	Monitoring & Operations: Amazon CloudWatch (metrics & logs), AWS X-Ray (tracing), AWS Systems Manager.
•	Security: AWS IAM (identity), AWS Secrets Manager, AWS WAF (web firewall).
•	CI/CD: AWS CodePipeline, AWS CodeBuild, AWS CodeDeploy.

Cost Considerations vs Traditional:
•	Higher Infrastructure Costs: More smaller instances vs fewer larger ones
•	Increased Data Transfer Costs: More inter-service communication
•	Management Tools Cost: Need additional monitoring/tracing tools
•	Development Cost: More complex development environment
•	Potential Savings: Can save by scaling only what's needed
•	TCO (Total Cost of Ownership): Often higher due to operational complexity

2. Serverless Architecture
Key Components and Their Responsibilities:
Compute Layer:
•	Function-as-a-Service: AWS Lambda, Azure Functions
•	Event Sources: API Gateway, S3 events, DynamoDB streams
•	Runtime Environments: Node.js, Python, Java, etc.
API Layer:
•	API Gateway: Routes requests to functions
•	Authentication: Cognito for user management
•	Rate Limiting: Built into API Gateway
Data Layer:
•	Serverless Databases: DynamoDB, Aurora Serverless
•	Object Storage: S3 for files and static content
•	Caching: DAX for DynamoDB, ElastiCache Serverless
Integration Layer:
•	Event Bridge: For event-driven architectures
•	Step Functions: For workflow orchestration
•	SQS/SNS: For messaging between functions
Benefits:
•	No Server Management: AWS manages infrastructure
•	Auto-scaling: Scales from zero to thousands instantly
•	Pay-per-use: Only pay for actual execution time
•	High Availability: Built-in redundancy and failover
•	Fast Deployment: Deploy code without provisioning infrastructure
•	Reduced Operational Burden: Less patching, monitoring, capacity planning

Limitations:
•	Cold Starts: Delay when function hasn't been used recently
•	Execution Limits: 15-minute max execution time (AWS Lambda)
•	Vendor Lock-in: Heavy dependency on cloud provider
•	Debugging Complexity: Harder to debug distributed functions
•	Local Testing: Different from production environment
•	Cost Unpredictability: Can get expensive with high volume
Real-World Examples:
•	Airbnb: Uses Lambda for image processing
•	Nordstrom: Serverless for inventory management
•	Coca-Cola: Vending machine data processing
•	FINRA: Processes 75 billion events daily with serverless
•	iRobot: Roomba data processing with AWS Lambda
AWS Services for Implementation:
•	Compute: AWS Lambda
•	API Management: API Gateway
•	Database: DynamoDB, Aurora Serverless
•	Storage: S3
•	Authentication: Cognito
•	Events: EventBridge
•	Orchestration: Step Functions
•	Monitoring: CloudWatch Logs and Metrics
Cost Considerations vs Traditional:
•	No Idle Cost: Don't pay for servers when not in use
•	Granular Billing: Pay per 100ms of execution
•	Potential Savings: For sporadic or unpredictable workloads
•	Potential Higher Costs: For constant high-volume workloads
•	Data Transfer Costs: Same as traditional
•	API Gateway Costs: Additional cost per request
•	Development Cost: May be lower due to less infrastructure code

<img width="650" height="936" alt="image" src="https://github.com/user-attachments/assets/9a8e28de-0fc6-45c3-8384-50695af2de14" />


	Architecture Diagrams
Traditional Architecture Diagram: Three-Tier Web Service
