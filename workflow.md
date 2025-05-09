````markdown
# 🛒 Online Grocery Store System

## 1. 🧑‍💻 User Flow

### a. Registration
- User visits registration page.
- Enters **name, email, password, phone number**.
- System:
  - Validates input.
  - Hashes password securely.
  - Stores user in the `Users` table with default role `customer`.

### b. Login
- User logs in with email and password.
- System:
  - Verifies credentials.
  - Starts session.
  - Grants access based on role.

### c. Browsing Grocery
- All users can:
  - View list of available books.
  - Filter/search by **product**, **price**.
  - See book details: product,price, stock, description.

### d. Purchasing Grocery
- Customers can:
  - Add Grocery to a cart or buy directly.
  - View and modify cart.
- On checkout:
  - System verifies stock.
  - Creates order with details.
  - Deducts stock.
  - Sends confirmation.

---

## 2. 🛠️ Admin Flow

### a. Grocery Management
- Admins can:
  - Add/edit/delete grocery.
  - Product, manage stock, price.
  - Upload/update cover images and descriptions.

### b. Order Management
- Admins can:
  - View all orders.
  - Filter by user, date, or status.
  - See order totals and book details.

---

## 3. 📏 Rules & Validations
- Grocery stock cannot go below 0.
- Passwords are hashed (e.g., bcrypt).
- Only logged-in users can purchase.
- Only admins can manage grocery or view all orders.
- Orders must include at least one item.
- Genres must be selected from predefined list.

---

## 4. 🗄️ Database Schema (SQL)
````


### Users

```sql
CREATE TABLE Users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    phone VARCHAR(20),
    role ENUM('customer', 'admin') DEFAULT 'customer',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
### Grocery

```sql
CREATE TABLE Grocery (
    id INT PRIMARY KEY AUTO_INCREMENT,
    product VARCHAR(255),
    price DECIMAL(10, 2),
    stock INT DEFAULT 0,
    isbn VARCHAR(20),
    description TEXT,
    cover_image_url TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
);
```

### Orders

```sql
CREATE TABLE Orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    total DECIMAL(10, 2),
    status ENUM('pending', 'completed', 'cancelled') DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(id)
);
```

### OrderDetails

```sql
CREATE TABLE OrderDetails (
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT,
    grocery_id INT,
    quantity INT,
    price DECIMAL(10, 2),
    FOREIGN KEY (order_id) REFERENCES Orders(id),
    FOREIGN KEY (grocery_id) REFERENCES Grocery(id)
);
```

### Cart

```sql
CREATE TABLE Cart (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    book_id INT,
    quantity INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(id),
    FOREIGN KEY (grocery_id) REFERENCES Grocery(id)
);
```
---