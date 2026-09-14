# Full-Stack Enterprise Online Banking Platform

An enterprise-ready, decoupled **Online Banking System** architected to mimic the core functionalities of modern retail banking applications. This project is built using a highly scalable full-stack layout, pairing a **Spring Boot** REST API processing engine with a highly interactive, state-managed **React & Redux** single-page web interface.

---

## 🎯 Executive Project Overview

The **Online Banking System** bridges corporate financial automation with consumer-facing convenience. The application operates as a distributed system where the frontend user interface and backend business layers remain entirely decoupled, communicating exclusively via structured JSON payloads over RESTful HTTP channels.

The platform provides a rich user journey across multiple critical banking domains:
*   **Secure Authentication Hub:** Uses token-based state checking to authorize users, protecting accounts from external session hijacking.
*   **Dynamic Ledger & Account Management:** Generates unique checking/savings accounts, automatically assigns IBAN/Account routing numbers, and maintains live ledger calculations.
*   **Intelligent Wire Transfers & Remittance:** An internal transaction engine capable of handling direct debits, peer-to-peer fund transfers, and deposit balancing with instant consistency checks.
*   **Comprehensive Audit Ledger:** Tracks transactional history over chronological periods, offering filters to review credits, debits, and processing dates.

---

## 💡 Real-World Problems Solved by This Platform

Legacy financial systems and poorly structured banking portals introduce data bottlenecks, high infrastructure costs, and synchronization delays. This platform resolves these structural friction points:

### 1. Eliminating Client-Side Balance Desynchronization
*   *The Problem:* Flawed frontend state management often forces applications to continuously make heavy server requests to refresh user balances, or worse, displays stale data when navigate between tabs.
*   *The Solution:* By utilizing **Redux Global State**, the frontend retains a single, immutable source of truth. As soon as a transaction is confirmed by the backend, the global state securely propagates the updated balance down to all components instantly, cutting down on excessive API calls and rendering fast UI transitions.

### 2. Preventing Race Conditions and Double Spending
*   *The Problem:* If a user clicks a "Transfer Funds" button multiple times rapidly, poorly configured platforms can double-process the request, draining accounts improperly.
*   *The Solution:* The backend utilizes **Spring Data JPA Transaction Management** coupled with atomic database isolation levels. If a secondary request hits the server before the first transaction commits, it is safely rejected, ensuring perfect ledger consistency.

### 3. Mitigating Data Leakage and Session Vulnerabilities
*   *The Problem:* Storing raw customer passwords or exposing backend endpoints without structural isolation invites massive security vulnerabilities.
*   *The Solution:* The decoupled layout forces strict boundaries. The Spring Boot backend exposes precise REST APIs that do not leak structural database patterns. It mandates token authorization headers for any data access, isolating restricted core routes from unauthorized manipulation.

### 4. Overcoming High UI/Backend Scaling Friction
*   *The Problem:* Monolithic applications require the entire server ecosystem to be rebuilt and redeployed even if developers are just updating a minor UI button style.
*   *The Solution:* The codebase is structurally split into `Online Banking App Spring Boot` and `demo-bank-redux`. The frontend can be easily deployed to a global Content Delivery Network (CDN) like Vercel or Netlify, while the backend API can be scaled independently inside Docker containers behind a load balancer.

---

## 🛠️ Complete Technical Stack Matrix

Every component chosen for this application addresses a precise architectural responsibility, maximizing system runtime, data integrity, and cross-browser responsiveness.

### 💻 Frontend Presentation Layer (`demo-bank-redux/`)
*   **React.js (v18+):** Leverages a virtual DOM configuration to build reusable functional components (e.g., `TransferForm`, `TransactionHistory`, `AccountSummary`), facilitating instant, smooth client-side rendering.
*   **Redux Toolkit & React-Redux:** Manages the entire application's global caching layer. It stores active session headers, temporary transaction details, and current account balances, keeping separate components smoothly aligned.
*   **Axios Interceptors:** Orchestrates asynchronous HTTP actions directed at backend controllers. It handles data timeouts, error catching, and appends authorization headers smoothly before requests leave the client.
*   **HTML5, CSS3, & Modern UI Frameworks:** Delivers a fully responsive grid structure that scales automatically across ultra-wide desktop displays, tablets, and mobile smartphones.
*   **Node.js Environment (`package-lock.json`):** Standardizes npm script executions, manages security patches for client-side dependencies, and compiles optimization bundles for web deployment.

### ⚙️ Backend Core Processing Layer (`Online Banking App Spring Boot/`)
*   **Java 17+:** Brings modern language efficiencies, record classes for data transfer objects (DTOs), robust memory management, and high-performance compilation to the backend execution stack.
*   **Spring Boot:** The foundational runtime platform. It uses embedded Tomcat routing environments, handles dependency injection natively, and simplifies MVC REST API design pattern implementation.
*   **Spring Data JPA & Hibernate ORM:** Abstracts complex SQL interactions. It uses Object-Relational Mapping to transform backend Java entity models directly into matching relational database columns, minimizing manually written boilerplate queries.
*   **Apache Maven Build Tool (`pom.xml`):** Controls library lifecycles, packages compiled classes into optimized executable JAR containers, and verifies automated software validation tests.

### 🗄️ Database, Quality, & IDE Configurations
*   **Relational Database Engine (MySQL / PostgreSQL / H2):** Acts as the high-availability ledger data layer. It maps fixed relational logic constraints between critical datasets (e.g., forcing a `Transaction` row to explicitly require a valid foreign key relation linking it to a `UserAccount` ID).
*   **Visual Studio Code Workspace (`.vscode/`):** Standardizes local development configurations, code formatting defaults, and debugger attachment protocols for engineering uniformity across different developer machines.

---

## 📁 Repository Structural Overview

```text
├── .vscode/                        # Shared IDE workspace profiles and code parameters
├── Online Banking App Spring Boot/ # Decoupled Core Backend Service Module (Java / Maven)
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/               # Core codebase (Controllers, Services, Models, DTOs)
│   │   │   └── resources/          # Configuration profiles (application.properties)
│   │   └── test/                   # JUnit validation and integration test files
│   └── pom.xml                     # Maven dependency blueprint file
├── demo-bank-redux/                # Decoupled Single Page Application Module (React / Redux)
│   ├── public/                     # Static media configurations, index.html, icons
│   ├── src/
│   │   ├── components/             # Reusable UI parts (Navbar, Sidebar, BalanceCards)
│   │   ├── redux/                  # Global slices, store definitions, and active actions
│   │   └── App.js                  # Core frontend entry layout router
│   └── package.json                # npm script definitions and front-end dependencies
├── ProjectPage1.png                # Platform layout preview - Main Dashboard
├── ProjectPage2.png                # Platform layout preview - Fund Transfer Portal
├── ProjectPage3.png                # Platform layout preview - Transaction Ledgers
├── ProjectPage5.png                # Platform layout preview - User Settings
├── loginPage.png                   # Platform layout preview - Secure Portal Login
└── package-lock.json               # Frozen frontend dependency layout index
```

---

## ⚙️ Local Development Setup & Execution Guide

Follow these sequential steps to boot the entire full-stack ecosystem on your local development machine.

### 📋 Prerequisites
Before launching, make sure your machine has the following tools installed globally:
*   **Java Development Kit (JDK 17 or higher)**
*   **Node.js (v16.x or higher) & npm (v8.x or higher)**
*   **An active SQL instance (e.g., MySQL Server)**

---

### 🛡️ Step 1: Initialize and Run the Backend API Server

1. Open your system terminal and change directory into the backend project root folder:
   ```bash
   cd "Online Banking App Spring Boot"
   ```

2. Open `src/main/resources/application.properties` and customize your local relational database settings:
   ```properties
   # Server Port Routing
   server.port=8080

   # Database Connection Configuration
   spring.datasource.url=jdbc:mysql://localhost:3306/online_banking_db?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
   spring.datasource.username=your_database_username
   spring.datasource.password=your_database_password

   # Hibernate Configuration Settings
   spring.jpa.hibernate.ddl-auto=update
   spring.jpa.show-sql=true
   spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
   ```

3. Compile dependencies and execute the application using the included Maven Wrapper:
   * **On Linux / macOS:**
     ```bash
     chmod +x mvnw
     ./mvnw spring-boot:run
     ```
Use code with caution.On Windows Command Prompt / PowerShell:cmd./mvnw.cmd spring-boot:run
Use code with caution.The server container will start successfully up on port http://localhost:8080.

🎨 Step 2: Initialize and Run the React-Redux Frontend UIOpen a new, separate system terminal window and change directory into the client UI folder:   bash cd demo-bank-redux    Cleanly install the required Node node modules listed within the package configuration:   bash npm install    Launch the Webpack server compiler engine to start the local interface instance:   bash npm start    Your default internet browser will immediately launch the frontend client portal on http://localhost:3000. The UI will now safely hit endpoints pointing back to your running Java server at port 8080.

🧪 Testing the Complete EcosystemBackend Unit CoverageTo evaluate backend unit endpoints, run the Maven validation phase inside the backend directory:bash ./mvnw test Frontend UI VerificationTo run frontend components linting checks or mock unit actions inside the React project directory:bash npm test 

📄 License and ComplianceThis system code configuration is distributed openly under the MIT License. Check out the repository's root LICENSE file context details for extensive open-source redistribution clearance rules.
***

Would you like me to expand this file further by providing a **Mock Database SQL Schema Script** to help users set up their tables instantly, or would you like to add an **API Endpoint Documentation Table** mapping out all available frontend-to-backend payload paths?
