
### **1. User Management Tables:**

#### **Users Table**
This table will store user authentication information and roles.
```sql
CREATE TABLE Users (
    UserID INT PRIMARY KEY AUTO_INCREMENT,
    UserName VARCHAR(255) NOT NULL,
    Phone VARCHAR(20),
    Email VARCHAR(255) UNIQUE NOT NULL,
    PasswordHash VARCHAR(255) NOT NULL,
    Role ENUM('Owner', 'Manager', 'Employee', 'Viewer') NOT NULL,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

#### **Employees Table**
This table stores information about employees assigned by the owner or manager.
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY AUTO_INCREMENT,
    UserID INT,
    EmployeeName VARCHAR(255) NOT NULL,
    Designation VARCHAR(100),
    FOREIGN KEY (UserID) REFERENCES Users(UserID) ON DELETE CASCADE
);
```

### **2. Product Management Tables:**

#### **Products Table**
This table stores the products in the inventory.
```sql
CREATE TABLE Products (
    ProductID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(255) NOT NULL,
    Description TEXT,
    Specifications TEXT,
    SKU VARCHAR(100) UNIQUE,
    LocationID INT,
    SupplierID INT,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (LocationID) REFERENCES Locations(LocationID),
    FOREIGN KEY (SupplierID) REFERENCES Suppliers(SupplierID)
);
```

#### **ProductEntries Table**
This table manages the purchase and sales data related to products.
```sql
CREATE TABLE ProductEntries (
    EntryID INT PRIMARY KEY AUTO_INCREMENT,
    ProductID INT,
    PurchasingPrice DECIMAL(10, 2),
    SellingPrice DECIMAL(10, 2),
    Quantity INT NOT NULL,
    SKU VARCHAR(100),
    LocationID INT,
    SupplierID INT,
    EntryType ENUM('Purchase', 'Sale', 'Transfer', 'Adjustment') NOT NULL,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID),
    FOREIGN KEY (LocationID) REFERENCES Locations(LocationID),
    FOREIGN KEY (SupplierID) REFERENCES Suppliers(SupplierID)
);
```

### **3. Supplier Management Tables:**

#### **Suppliers Table**
This table stores information about suppliers.
```sql
CREATE TABLE Suppliers (
    SupplierID INT PRIMARY KEY AUTO_INCREMENT,
    SupplierName VARCHAR(255) NOT NULL,
    Contact VARCHAR(20),
    Address TEXT,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

### **4. Customer Management Tables:**

#### **Customers Table**
This table stores information about customers.
```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY AUTO_INCREMENT,
    CustomerName VARCHAR(255) NOT NULL,
    Contact VARCHAR(20),
    Email VARCHAR(255),
    Address TEXT,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

### **5. Inventory Management Tables:**

#### **Inventory Table**
This table tracks stock levels and locations.
```sql
CREATE TABLE Inventory (
    InventoryID INT PRIMARY KEY AUTO_INCREMENT,
    ProductID INT,
    QuantityOnHand INT NOT NULL,
    ReorderPoint INT,
    StockLocation VARCHAR(255),
    LocationID INT,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID),
    FOREIGN KEY (LocationID) REFERENCES Locations(LocationID)
);
```

### **6. Location Management Tables:**

#### **Locations Table**
This table manages information about various storage locations.
```sql
CREATE TABLE Locations (
    LocationID INT PRIMARY KEY AUTO_INCREMENT,
    LocationType ENUM('Warehouse', 'Hub', 'Outlet') NOT NULL,
    LocationName VARCHAR(255),
    Capacity INT,
    Allocation INT,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

### **7. Transaction Tables:**

#### **Transactions Table**
This table handles all types of transactions (purchases, sales, transfers, adjustments).
```sql
CREATE TABLE Transactions (
    TransactionID INT PRIMARY KEY AUTO_INCREMENT,
    ProductID INT,
    SupplierID INT,
    CustomerID INT,
    Quantity INT NOT NULL,
    UnitPrice DECIMAL(10, 2),
    TotalPrice DECIMAL(10, 2),
    TransactionType ENUM('Purchase', 'Sale', 'Transfer', 'Adjustment') NOT NULL,
    LocationID INT,
    TransactionDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID),
    FOREIGN KEY (SupplierID) REFERENCES Suppliers(SupplierID),
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID),
    FOREIGN KEY (LocationID) REFERENCES Locations(LocationID)
);
```

### **8. Adjustment Tables:**

#### **Adjustments Table**
This table records manual adjustments to the inventory.
```sql
CREATE TABLE Adjustments (
    AdjustmentID INT PRIMARY KEY AUTO_INCREMENT,
    ProductID INT,
    Quantity INT NOT NULL,
    Reason ENUM('Obsoletion', 'Expiration', 'Product Loss') NOT NULL,
    AdjustmentDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID)
);
```

### **9. Reporting Tables:**

#### **Reports Table**
This table stores generated reports for analysis.
```sql
CREATE TABLE Reports (
    ReportID INT PRIMARY KEY AUTO_INCREMENT,
    ReportType ENUM('StockLevel', 'Sales', 'Purchases', 'Transfers') NOT NULL,
    ReportData TEXT,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### **10. Integration Tables:**

#### **Integrations Table**
This table manages integrations with external platforms like accounting software, e-commerce, etc.
```sql
CREATE TABLE Integrations (
    IntegrationID INT PRIMARY KEY AUTO_INCREMENT,
    PlatformName VARCHAR(255) NOT NULL,
    IntegrationData TEXT,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### **Database Relationships:**
- **Users** and **Employees**: Each user can have multiple employees.
- **Products**: Connected to **Suppliers** and **Locations**.
- **ProductEntries**: Linked with **Products**, **Suppliers**, and **Locations**.
- **Inventory**: Tracks product quantities at different **Locations**.
- **Transactions**: Captures various activities like **Purchases**, **Sales**, and **Transfers**.
- **Adjustments**: Handles manual changes to **Inventory**.

This schema provides a solid foundation for your offline-focused IMS system. You can now proceed with building the application using this structure as the backbone of your database. Let me know if you need further customization or help with implementing the application logic!