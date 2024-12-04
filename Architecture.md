# 1. System Context Diagram
The System Context Diagram shows the Groww app in relation to its users and external systems.
## Diagram
![UML Diagram](https://www.plantuml.com/plantuml/dpng/NP1FJm8n4CNl_HJZdZ0HlN0YXa1GaGF9HkAPJ7P7QUYVJMSMs6zlssLLEBLvytklypfdmI2jDR3qJkWiwawxohGrmVDoyhISx2xwJ9KKsBjHrR9uI-41YWyIr0RMxaXH2qMKFfaOP4-m2lvq0cmQ40yM7NuJw0nQSiAFnVqGYBNLBbeNwKmPcqptCTona4tqDjz6ENVHD97hduZtP5h_PaDZKBvy-EVQlPvbj2ZNaTkAhmwmmN0WWIWbRndyKTV440ZDMBGbu8pLA7YJKcAN7bcXq0cxPEu5k2WfxnrTRM9S4qT3MJNy6undC9IO_fKjVOXB6-qBbJ_a5lDEYmyC_Ig56cQHIpKYyaPIxQ7lzuA2ryprtjDSkYBcOS25cfEhMslK3m00)
### UML Code
```uml
@startuml Context
!include <C4/C4_Context>
Person(customer, "Retail Investor", "Uses the Groww app to manage investments.")
Person(admin, "Admin", "Manages the system.")
System(GrowwApp, "Groww App", "Investment platform.")
System_Ext(API, "Stock Market API", "Provides real-time stock data.")
System_Ext(PaymentGateway, "Payment Gateway", "Handles payments.")

customer -> GrowwApp: "Manages Portfolio"
admin -> GrowwApp: "Monitors System"
GrowwApp -> API: "Fetch Stock Data"
GrowwApp -> PaymentGateway: "Process Payments"
@enduml
```

# Description 

- Users:
    - Retail investors: Interact with the app to manage their investments.
    - Admins: Monitor system health and manage user data.
- External Systems:
    - Stock Market APIs: Fetch real-time data.
    - Payment Gateways: Handle secure transactions.

# 2. Container Diagram
The Container Diagram shows the major containers that compose the Groww app.

## Diagram

![UML Diagram](https://www.plantuml.com/plantuml/dpng/NP9FJy904CNl_HHZJoLHgWadOmXG_aWYfag8Hx9i9rBPxZQxKw4nVdTdeLNmzlBURzvs9Xqu4fQgGa5YD8bSexsz2wvoBLML8JmalJZfhN-pVf2YTKP7id9a2hJN4BuYPmhc-XFPis4dMDQEi5YFJAKpOUjePir-FonBXeyIM8ST-0hWxzLeQbcYpXXgPNXi5vBYgQg8q7fZdBjIYTe1RH5Myow_Trup4d9tQccKdtas6NO_N3mvb1QTyzQZq9b21vlAvPezKEOzb__QnfjeB4XiX4CUQQKVNzJBboSlbhmorsIS-U7lTld9WQ9XEZyHSWSBONT8m1PJPqmgwW8rpGJXNjICDmOq3jD1UsouwKDxcti8LmvT6BI63vkLEUDt815ACU3D9cytO83lchdupUQ4bmnFSZn5ablUoqzSn9VVosHgZUJxCTj4N3TWcF-fu0S0)

### UML Code
```uml
@startuml Container01
!include <C4/C4_Container>
Person(customer, "Retail Investor")
System_Boundary(GrowwApp, "Groww App") {
  Container(Frontend, "Frontend", "React/Flutter", "Displays the UI.")
  Container(Backend, "Backend", "Node.js/Express", "Handles business logic.")
  ContainerDb(Database, "Database", "MySQL", "Stores data.")
}
System_Ext(API, "Stock Market API")
System_Ext(PaymentGateway, "Payment Gateway")

customer -> Frontend: "Uses"
Frontend -> Backend: "API Calls"
Backend -> Database: "Reads/Writes"
Backend -> API: "Fetches Data"
Backend -> PaymentGateway: "Processes Payments"
@enduml
```

# Description

- Frontend (React/Flutter): Provides the UI.
- Backend (Node.js/Express): Manages business logic and API endpoints.
- Database (MySQL): Stores user, transaction, and portfolio data.
- External Services: Stock market API and payment gateways.

# 3. Component Diagram
The Component Diagram provides a detailed view of the internal structure of the backend.

## Diagram

![UML Diagram](https://www.plantuml.com/plantuml/dpng/TP51Jpen4CNl_HJpxub_ed3XH0n15uq7CIYYHpDqHwXqsqsdQ_JRsyAoL2EtoSpytdlJJZYW3qrbe71LxIpP8FvfgqnJ4bmKmq4nVEqcOr4u6r1RyhqhL5kovHd8jfAnl7SbzTzvSFrPUs9EhLkqfI66LSEHOmRZrbhrvV-ZrdJLcsB05J95u50cUFQrUBYBgwABaFbUDc4JErfXqCx2Wlo7LdiCBQw90Nzkn2J0n1nhZW7-59qx7zwSqQxJosweks6rSwWYUqheoQDbLB-ZpRrJn1p5GhTmEiqC_PO2f2V9HMueg4sKuE3K5ieuJGAYvU1y38T7785y92nvyE9r89QYkw-LIyjxbuIqpL5h0OkTGPf8aHsKiRbUYoxhc8ZuE98Y_weSptC-ez49X2MJbs8IBsag8xu1)

### UML Code
```uml
@startuml Component
!include <C4/C4_Component>
Container(Backend, "Backend", "Node.js/Express", "Handles business logic.")
ContainerDb(Database, "Database", "MySQL")

Component(Backend, "Authentication Service", "Manages authentication and sessions.")
Component(Backend, "Portfolio Service", "Handles portfolio management.")
Component(Backend, "Transaction Service", "Processes transactions.")
Component(Backend, "Market Data Service", "Fetches stock data.")

Backend -> Database: "Reads/Writes"
PortfolioService -> MarketDataService: "Fetch Stock Data"
TransactionService -> PortfolioService: "Updates Portfolio"
AuthenticationService -> Database: "Validate User"
@enduml
```

# Description
- Authentication Service: Manages user authentication and session handling.
- Portfolio Service: Handles portfolio creation and updates.
- Transaction Service: Processes financial transactions.
- Market Data Service: Fetches real-time stock data.

# 4. Deployment Diagram
The Deployment Diagram illustrates the physical deployment of the system, showing the servers, containers, and network connections.

## Diagram

![UML Diagram](https://www.plantuml.com/plantuml/dpng/ROz1Jm8n48Nl_HLZJqkYaWWdOmXGC6B0g9Lu96KxXqgtgypKI0p_tLaX0GqzJ5xxlkRDwnExDUSgmWZVOxsfCB4w2wcCsIFSc6xRT1S7gwSEUl5GUom6h_OSj4mmiSvUj-1R0VodnkPAA9amIPAOIjo1SaoTs8M4wsBeoXKcB-1UQP7Dzyitqgs_y6XPZ1oxfICKQBQnJzDVP7iYt7O4iHqOvdA5V2BOikDG3eYGIT3T5Mv3H6fMk7kPJTl6slQztMTibTqGOxKOVt5XOi34JRtaOKwubWK-GzDGxIouwC5nn3NeQUqymNoYZv1zQN47ZnDune-CnAHLNnvp5TKF)

### UML Code
```uml
@startuml Deployment
!include <C4/C4_Deployment>
Deployment_Node(AWS, "AWS Cloud") {
  Deployment_Node(EC2, "EC2 Instance") {
    Container(Backend, "Backend", "Node.js")
    ContainerDb(Database, "MySQL")
  }
  Deployment_Node(S3, "S3 Bucket") {
    Container(StaticAssets, "Static Files", "HTML/CSS/JS")
  }
}
System_Ext(Client, "End User Device")
Client -> StaticAssets: "Loads UI"
Client -> Backend: "API Requests"
@enduml
```

# Description
- AWS EC2: Hosts the backend and database.
- CloudFront CDN: Serves static assets for the frontend.
- S3: Stores static files and backups.
