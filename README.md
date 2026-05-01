
---

# Banking App System CRUD
A Java Swing Desktop Application for banking management using a MySQL relational database.

## System Description
The Apex Banking System provides a centralized interface for bank administrators to manage client data and financial operations. The application is built using a three-tier architecture: the UI layer (Swing), the logic layer (Java), and the data layer (MySQL).

### Core Functionalities:
* **Customer Management:** Full CRUD (Create, Read, Update, Delete) operations for customer profiles.
* **Account Handling:** Automated linking of bank accounts to unique customer IDs.
* **Transaction Processing:** Secured deposit and withdrawal functions with real-time balance calculation.
* **Audit Logging:** Automated transaction logging with timestamps and type identifiers.
* **UI Stability:** Fixed table models that prevent direct cell editing to maintain data integrity.

---

## ERD (Entity Relationship Diagram) Explanation
The database architecture is designed to enforce strict data integrity through relational constraints.



### Database Schema Components:
1. **Customers Table:** Acts as the primary entity. Stores personal identifiers including Name, Email, and Phone Number.
2. **Accounts Table:** Maintains a One-to-One (1:1) relationship with the Customers table. It tracks the current available balance and uses `customer_id` as a foreign key.
3. **Transactions Table:** Maintains a One-to-Many (1:N) relationship with the Accounts table. Every financial movement creates a record here, linked via `account_id`.

### Data Integrity Rules:
* **Foreign Key Constraints:** Prevent the deletion of an account that still has existing transaction history.
* **Hierarchical Deletion:** The system is programmed to delete data in the order of Transactions -> Accounts -> Customers to avoid foreign key constraint violations.

---

## How to Run the Program

### 1. Requirements
* Java Development Kit (JDK) 8 or higher.
* MySQL Server 8.0 or higher.
* MySQL Connector/J JDBC Driver.
* NetBeans IDE (for accessing .form metadata).

### 2. Database Initialization
Execute the following SQL script in your MySQL environment to set up the necessary tables:

```sql
CREATE DATABASE banking_system_db;
USE banking_system_db;

CREATE TABLE customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100),
    phone_number VARCHAR(20)
);

CREATE TABLE accounts (
    account_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT,
    balance DOUBLE DEFAULT 0.0,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE transactions (
    transaction_id INT PRIMARY KEY AUTO_INCREMENT,
    account_id INT,
    transaction_type ENUM('Deposit', 'Withdraw'),
    amount DOUBLE NOT NULL,
    transaction_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (account_id) REFERENCES accounts(account_id)
);
```

### 3. Local Configuration
1. Open the file `DatabaseConnection.java`.
2. Update the connection string, username, and password to match your local MySQL settings:
   ```java
   private static final String URL = "jdbc:mysql://localhost:3306/banking_system_db";
   private static final String USER = "root";
   private static final String PASS = "your_password";
   ```

### 4. Build and Execution
1. Open the project in NetBeans.
2. Ensure the MySQL Connector JAR is added to the Project Libraries.
3. Right-click the project and select **Clean and Build**.
4. Run the `MainDashboard.java` file to start the application.

---
