# Restaurant-Billing-System-Business-Application
A C# Windows Forms restaurant billing system with three-tier architecture (UI/BL/DL), MySQL database, and full OOP principles — Encapsulation, Inheritance, Abstraction, Polymorphism, and Interface.
A desktop application built in C# Windows Forms for managing the complete 
billing workflow of a restaurant. The system supports Admin and Employee 
roles with a clean panel-based GUI for all operations.

---

## Tech Stack

- Language  : C#
- UI        : Windows Forms (.NET Framework 4.7.2)
- Database  : MySQL 8.0
- Connector : MySql.Data (NuGet)
- IDE       : Visual Studio

---

## Architecture

Strictly follows Three-Tier Architecture:

| Layer      | Role                                              |
|------------|---------------------------------------------------|
| UI Layer   | Windows Forms screens — input and display only    |
| BL Layer   | Business logic — validation and calculations      |
| DL Layer   | Database access — queries, inserts, updates       |
| Program.cs | Entry point — coordinates all three layers        |

Rules enforced throughout:
- UI calls BL only. Never calls DL directly.
- BL contains logic only. Never calls DL.
- DL uses BL classes as data holders only. Never calls BL methods.
- Program.cs may call UI, BL, and DL freely.

---

## OOP Principles Applied

**Encapsulation**
All BL classes use private fields with public Get/Set methods.
No field is exposed directly anywhere in the project.

**Abstraction**
UserBL is an abstract class. It holds common user data 
(username, password, role) and declares GetInfo() as abstract.
A direct UserBL object is never instantiated.

**Inheritance**
Three classes inherit from UserBL:
- AdminBL    — adds CanAddAdmin, CanRemoveEmployee, CanApprove, 
               CanReject, ShouldCreateDefault
- EmployeeBL — adds CanSignUp
- PendingUserBL — represents users awaiting admin approval

**Polymorphism**
GetInfo() is overridden in all three child classes.
All three types are stored in List<UserBL> but each responds 
with its own output at runtime.

AdminBL    → "Admin: [name] | Access: Full"
EmployeeBL → "Employee: [name] | Access: Limited"
PendingUserBL → "Pending User: [name] | Waiting for Approval"

**Interface — IValidatable**
Custom interface with bool Validate() implemented by:
- UserBL    — checks username and password are not empty
- MenuItem  — checks name is not empty and price is above zero
- OrderBL   — checks customerName, itemName not empty and quantity above zero

---

## Features

**Admin Portal**
- Add another admin (max 3 admins allowed)
- Add, view, modify, remove menu items
- View total customers and total daily earnings
- View, approve, and reject pending employee requests
- Remove an existing employee

**Employee Portal**
- View full menu (sorted alphabetically)
- Take new customer orders (DineIn / TakeAway / Delivery)
- Modify existing orders (add item, increase/decrease quantity, remove item)
- Calculate bill with order type charges and process payment
- Check bill payment status of any customer
- Search menu items and search customers by name initial

**Sign Up**
- Employees can self-register
- Registration goes as a pending request to admin
- Admin approves or rejects from the Employee Requests panel
- Maximum 10 employees allowed

---

## Order Type Charges

| Order Type | Additional Charge |
|------------|-------------------|
| DineIn     | Rs. 50            |
| TakeAway   | Rs. 20            |
| Delivery   | Rs. 100           |

---

## Business Rules

- Maximum 3 admins allowed in the system
- Maximum 10 employees allowed in the system
- Default admin created automatically if no admin exists in database
- Duplicate menu item names not allowed (case-insensitive)
- An order cannot be modified after payment is made
- Quantity cannot be decreased below 1
- Total earnings count only orders where payment is completed
- Pending users cannot log in until approved by admin

---

## Database

Database Name: RestaurantBillingSys

Tables:
- Users      — stores Admins, Employees, and Pending users
- MenuItems  — stores all menu items and prices
- Orders     — stores all customer orders with payment status

---

## Default Login
| Role  | Username | Password |
|-------|----------|----------|
| Admin | admin    | admin123 |
