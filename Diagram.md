# Software Design Diagrams
## Groww Competitor Investment Platform

## 1. Class Diagram
```mermaid
classDiagram
    class User {
        +String userId
        +String username
        +String email
        +String passwordHash
        +Date registrationDate
        +registerUser()
        +login()
        +updateProfile()
    }

    class Portfolio {
        +String portfolioId
        +List<Investment> investments
        +Double totalInvestmentValue
        +Double totalGainLoss
        +addInvestment()
        +removeInvestment()
        +calculatePerformance()
    }

    class Investment {
        +String investmentId
        +String type
        +String name
        +Double purchasePrice
        +Integer quantity
        +Date purchaseDate
        +updateInvestment()
        +calculateReturn()
    }

    class Stock {
        +String stockSymbol
        +String companyName
        +Double currentPrice
        +Double previousClosePrice
        +fetchRealTimeData()
    }

    class MutualFund {
        +String fundCode
        +String fundName
        +Double navPrice
        +Double expenseRatio
        +fetchFundPerformance()
    }

    class Transaction {
        +String transactionId
        +String userId
        +String investmentId
        +Double amount
        +Date transactionDate
        +String transactionType
    }

    class MarketAnalytics {
        +generateStockTrends()
        +calculatePortfolioRisk()
        +predictInvestmentPerformance()
    }

    User "1" -- "1" Portfolio : has
    Portfolio "1" -- "*" Investment : contains
    Investment <|-- Stock
    Investment <|-- MutualFund
    User "1" -- "*" Transaction : makes
    MarketAnalytics -- Investment : analyzes
```

## 2. Use Case Diagram
```mermaid
flowchart TD
    subgraph Actors
        User
        Admin
        System
    end

    subgraph "Use Cases"
        Registration[User Registration]
        Login[User Login]
        ProfileManagement[Profile Management]
        SearchInvestments[Search Investments]
        AddPortfolio[Add to Portfolio]
        TrackPerformance[Track Portfolio Performance]
        MarketAnalysis[Market Analysis]
        AdminDashboard[Admin Dashboard]
        UserManagement[User Management]
    end

    User --> Registration
    User --> Login
    User --> ProfileManagement
    User --> SearchInvestments
    User --> AddPortfolio
    User --> TrackPerformance
    User --> MarketAnalysis

    Admin --> AdminDashboard
    Admin --> UserManagement
```

## 3. Sequence Diagram: User Authentication
```mermaid
sequenceDiagram
    participant User
    participant WebApp
    participant AuthService
    participant DatabaseService

    User->>WebApp: Access Registration Page
    WebApp->>User: Display Registration Form
    User->>WebApp: Submit Registration Details
    WebApp->>AuthService: Validate User Information
    
    alt Validation Successful
        AuthService->>DatabaseService: Create User Account
        DatabaseService-->>AuthService: Account Created
        AuthService-->>WebApp: Registration Success
        WebApp->>User: Show Success Message
        
        User->>WebApp: Login with Credentials
        WebApp->>AuthService: Authenticate User
        
        alt Authentication Successful
            AuthService->>DatabaseService: Retrieve User Profile
            DatabaseService-->>WebApp: User Data
            WebApp->>User: Show Dashboard
        else Authentication Failed
            AuthService-->>WebApp: Authentication Error
            WebApp->>User: Show Login Error
        end
    else Validation Failed
        AuthService-->>WebApp: Registration Error
        WebApp->>User: Show Error Details
    end
```

## 4. Activity Diagram: Portfolio Management
```mermaid
flowchart TD
    Start[Start] --> LoginCheck{User Logged In?}
    LoginCheck -->|No| LoginPage[Redirect to Login]
    LoginCheck -->|Yes| Dashboard[Access Portfolio Dashboard]
    
    LoginPage --> LoginProcess[Login Process]
    LoginProcess --> LoginCheck
    
    Dashboard --> ActionSelection[Select Action]
    ActionSelection --> AddInvestment{Add Investment?}
    AddInvestment -->|Yes| SearchInvestment[Search Stocks/Funds]
    SearchInvestment --> SelectInvestment[Select Investment]
    SelectInvestment --> ConfirmAdd[Confirm Addition]
    ConfirmAdd --> UpdatePortfolio[Update Portfolio]
    
    ActionSelection --> RemoveInvestment{Remove Investment?}
    RemoveInvestment -->|Yes| ViewPortfolio[View Current Portfolio]
    ViewPortfolio --> SelectRemove[Select Investment to Remove]
    SelectRemove --> ConfirmRemove[Confirm Removal]
    ConfirmRemove --> UpdatePortfolioRemove[Update Portfolio]
    
    ActionSelection --> ViewPerformance{View Performance?}
    ViewPerformance -->|Yes| LoadMetrics[Load Performance Metrics]
    LoadMetrics --> DisplayGraphs[Display Performance Graphs]
    DisplayGraphs --> AnalyzeTrends[Analyze Trends]
    
    UpdatePortfolio --> Dashboard
    UpdatePortfolioRemove --> Dashboard
    AnalyzeTrends --> Dashboard
```

## 5. Component Diagram
```mermaid
flowchart TD
    subgraph "Groww Investment Platform"
        UserInterface[User Interface Layer]
        AuthenticationModule[Authentication Module]
        PortfolioManagement[Portfolio Management Service]
        MarketDataService[Market Data Service]
        AnalyticsEngine[Analytics Engine]
        DatabaseLayer[Database Layer]
    end

    subgraph "External Services"
        StockAPI[Stock Market API]
        PaymentGateway[Payment Gateway]
        NotificationService[Notification Service]
    end

    UserInterface --> AuthenticationModule
    UserInterface --> PortfolioManagement
    UserInterface --> MarketDataService

    AuthenticationModule --> DatabaseLayer
    PortfolioManagement --> DatabaseLayer
    MarketDataService --> StockAPI

    AnalyticsEngine --> PortfolioManagement
    AnalyticsEngine --> MarketDataService

    DatabaseLayer --> PaymentGateway
    DatabaseLayer --> NotificationService
```

## 6. Deployment Diagram
```mermaid
flowchart TD
    subgraph "Cloud Infrastructure"
        LoadBalancer[Load Balancer]
        
        subgraph "Web Servers"
            WebServer1[Web Server 1]
            WebServer2[Web Server 2]
        end
        
        subgraph "Application Servers"
            AppServer1[Application Server 1]
            AppServer2[Application Server 2]
        end
        
        subgraph "Database Cluster"
            PrimaryDB[Primary Database]
            ReplicaDB1[Replica Database 1]
            ReplicaDB2[Replica Database 2]
        end
        
        subgraph "Caching Layer"
            CacheServer1[Redis Cache 1]
            CacheServer2[Redis Cache 2]
        end
    end

    subgraph "External Services"
        StockAPI[Stock Market API]
        PaymentGateway[Payment Gateway]
    end

    Client --> LoadBalancer
    LoadBalancer --> WebServer1
    LoadBalancer --> WebServer2

    WebServer1 --> AppServer1
    WebServer2 --> AppServer2

    AppServer1 --> PrimaryDB
    AppServer2 --> ReplicaDB1

    PrimaryDB --> ReplicaDB1
    PrimaryDB --> ReplicaDB2

    AppServer1 --> CacheServer1
    AppServer2 --> CacheServer2

    AppServer1 --> StockAPI
    AppServer2 --> PaymentGateway
```

## 7. State Diagram: User Account
```mermaid
stateDiagram-v2
    [*] --> Unregistered
    Unregistered --> Registration : User starts registration
    Registration --> Verification : Submit details
    Verification --> Active : Successful verification
    Verification --> Unregistered : Failed verification
    
    Active --> ProfileUpdate : Update profile
    Active --> PasswordReset : Reset password
    
    ProfileUpdate --> Active
    PasswordReset --> Active
    
    Active --> Suspended : Admin action
    Suspended --> Active : Admin reactivation
    
    Active --> [*] : Account deletion
```

## 8. Communication Diagram: Investment Process
```mermaid
flowchart LR
    User --> UserInterface
    UserInterface --> AuthService
    AuthService --> PortfolioService
    PortfolioService --> MarketDataService
    MarketDataService --> StockAPI
    StockAPI --> AnalyticsEngine
    AnalyticsEngine --> PortfolioService
    PortfolioService --> DatabaseService
    DatabaseService --> NotificationService
```

## Conclusion
These comprehensive UML and architectural diagrams provide a holistic view of the Groww Investment Platform's design, showcasing its structure, interactions, and system architecture.

### Diagram Types Covered
1. Class Diagram
2. Use Case Diagram
3. Sequence Diagram
4. Activity Diagram
5. Component Diagram
6. Deployment Diagram
7. State Diagram
8. Communication Diagram

Each diagram offers unique insights into different aspects of the platform's design and functionality.
