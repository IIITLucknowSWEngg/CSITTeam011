# **Groww Competitor Investment Platform - Software Design Document (SDD)**

## **1. Introduction**

### 1.1 **Purpose**
This document describes the design architecture for the Groww Competitor platform, an investment management system for stocks, mutual funds, and portfolios. It defines the system components, data flow, and deployment strategies.

### 1.2 **Scope**
The Groww Competitor platform enables:
- Real-time stock and fund tracking
- Portfolio management
- Secure financial transactions
- Analytical insights for investments

### 1.3 **Goals & Principles**
- **Scalability**: Handle large datasets and users efficiently  
- **Performance**: Real-time stock updates with minimal latency  
- **Security**: End-to-end encryption for transactions  
- **Modularity**: Independent and maintainable microservices  
- **User Focus**: Seamless and intuitive interface  

## **2. System Overview**

**Groww Competitor** is a comprehensive platform enabling users to invest in stocks, mutual funds, and other financial instruments. It provides real-time market updates, portfolio management, educational content, and secure transactions.

### 2.1 Key Components

#### **Frontend**
- **Technology**: React.js (Web), Flutter (Mobile App)
- **Features**:
  - User onboarding and KYC verification.
  - Real-time stock and mutual fund tracking.
  - Portfolio management and analytics.
  - Investment transactions.
  - Educational resources and blogs.

#### **Backend (Microservices)**
- **Technology**: Node.js with Express.js, Java Spring Boot for critical services.
- **API Communication**:
  - RESTful APIs for transactional data.
  - WebSocket for real-time updates.

#### **Database**
- **Primary**: PostgreSQL (Relational data: users, KYC, portfolios, transactions).
- **Secondary**: MongoDB (Unstructured data like blogs and logs).
- **Caching**: Redis for temporary storage of real-time stock prices.

#### **Infrastructure**
- **Cloud Provider**: AWS Cloud with Docker-based microservices.

#### **Security**
- Data encryption: AES-256.
- OAuth 2.0 for user authentication.
- Payment security: PCI-DSS compliance.

---

## **3. System Architecture**

### **3.1 High-Level Architecture**  
```mermaid
flowchart TD
    UI["Frontend: React.js / Flutter"] --> API["Backend: REST APIs"]
    API --> AuthService["Authentication Service"]
    API --> StockService["Stock Data Service"]
    API --> PortfolioService["Portfolio Management"]
    API --> TransactionService["Transaction Processing"]

    AuthService --> PostgreSQL["PostgreSQL"]
    PortfolioService --> PostgreSQL
    TransactionService --> PostgreSQL
    StockService --> MarketAPI["Market Data API"]
    API --> BlogService["Blog Management Service"]
    BlogService --> MongoDB["MongoDB"]
    TransactionService --> PaymentGateway["Payment Gateway"]
```

### **3.2 Components**

#### **Frontend**  
- **Tech Stack**: React.js (Web), Flutter (Mobile)  
- **Features**:  
  - Authentication (Login, Registration)  
  - Portfolio Dashboard  
  - Real-time stock data visualization  

#### **Backend (Microservices)**  
1. **Authentication Service**: User registration and login  
2. **Stock Data Service**: Fetch market data via APIs  
3. **Portfolio Service**: Manage investments and returns  
4. **Transaction Service**: Secure buy/sell processing  
5. **Blog Management Service**: Manage blogs and logs (stored in MongoDB).  
6. **Analytics Service**: Generate financial insights  

#### **Database**  
- **PostgreSQL** (Primary): Stores relational data.  
  - `Users`: User profiles, authentication, and KYC data.  
  - `Portfolios`: Investment records and returns.  
  - `Transactions`: Buy/sell history and payment logs.  
- **MongoDB** (Secondary): Stores unstructured data like blogs and logs.  
- **Redis**: Caches real-time stock prices to reduce latency.  

#### **External Integrations**  
- **Market Data API**: Real-time stock prices  
- **Payment Gateway**: Secure payment processing  

---

## **4. Entity Relationship Diagram (ERD)**

```mermaid
erDiagram
    USERS ||--o| PORTFOLIO : owns
    USERS ||--o| TRANSACTIONS : performs
    PORTFOLIO ||--o| MARKETDATA : references
    USERS ||--o| BLOGS : writes

    USERS {
      string userId
      string username
      string email
      string password
      float walletBalance
    }

    PORTFOLIO {
      string portfolioId
      string userId
      list holdings
      float totalInvestment
    }

    TRANSACTIONS {
      string transactionId
      string userId
      string stockSymbol
      float amount
      string type
      date timestamp
    }

    MARKETDATA {
      string stockSymbol
      float currentPrice
      float dailyChange
    }

    BLOGS {
      string blogId
      string userId
      string title
      string content
      date createdAt
    }
```

---

## **5. Microservices API Endpoints**

### **5.1 Authentication Service**  
| Endpoint           | Method | Description            |
|--------------------|--------|------------------------|
| `/register`        | POST   | Register new user      |
| `/login`           | POST   | Authenticate user      |
| `/logout`          | POST   | Terminate session      |

### **5.2 Stock Data Service**  
| Endpoint           | Method | Description            |
|--------------------|--------|------------------------|
| `/stocks`          | GET    | Fetch real-time prices |
| `/history`         | GET    | Retrieve price history |

### **5.3 Portfolio Service**  
| Endpoint           | Method | Description                 |
|--------------------|--------|-----------------------------|
| `/portfolio`       | GET    | Get user portfolio          |
| `/addHolding`      | POST   | Add stocks to portfolio     |
| `/removeHolding`   | POST   | Sell or remove holdings     |

### **5.4 Transaction Service**  
| Endpoint           | Method | Description              |
|--------------------|--------|--------------------------|
| `/buy`             | POST   | Execute buy order        |
| `/sell`            | POST   | Execute sell order       |
| `/transactions`    | GET    | Get transaction history  |

### **5.5 Blog Management Service**  
| Endpoint           | Method | Description              |
|--------------------|--------|--------------------------|
| `/blogs`           | GET    | Fetch all blogs          |
| `/blogs/:id`       | GET    | Fetch blog by ID         |
| `/blogs`           | POST   | Add new blog             |

---

## **6. Deployment Strategy**

### **6.1 Infrastructure**  
The platform will deploy on **AWS**:  
- **EC2**: Backend services  
- **S3**: Static frontend hosting  
- **RDS**: Database hosting  
- **CloudFront**: Content delivery  

### **6.2 CI/CD Pipeline**  
| Tool              | Purpose                      |
|-------------------|------------------------------|
| **GitHub**        | Source code version control  |
| **Jenkins / GitHub Actions** | CI/CD automation          |
| **Docker**        | Containerized microservices  |

---

## **7. Conclusion**  
The Groww platform is designed for scalability, security, and user-centric performance. Leveraging microservices, cloud infrastructure, and modern tech stacks, it aims to deliver an efficient investment management solution.

## **8. Engineering Blog References**  
- [Invest with Ease: Revamping Groww's Mutual Fund SIP Flow](https://medium.com/@mestriabhishek/invest-with-ease-revamping-growws-mutual-fund-sip-flow-efb3acd3bbe)  
- [Groww: Changing How Indians Invest](https://medium.com/@parasjain1805/groww-changing-how-indians-invest-f6e1865b5246)
