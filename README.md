# 🏋️ Gym Management System

> **CS306 – Database Systems | Term Project (Phase 3 & 4)**  
> A full-stack gym management application integrating MySQL (relational) and MongoDB (NoSQL) databases with a PHP web interface.

---

## 👥 Team

| Name | Student ID |
|------|-----------|
| Alperen Sarışen | 33886 |
| Hamza Eren İnan | 34502 |

---

## 📋 Project Overview

The Gym Management System is a relational + NoSQL database application designed to manage the daily operations of a fitness center network. It supports branch management, employee records, member registration, class scheduling, equipment tracking, and a support ticket system.

The project was developed across multiple phases:

- **Phase 3** — Relational database design, SQL queries, and relational algebra
- **Phase 4** — Web integration with PHP, stored procedures, triggers, and a MongoDB-backed support ticket system

---

## 🗄️ Database Schema (MySQL)

The relational database (`gym_db`) consists of the following tables:

| Table | Description |
|-------|-------------|
| `Branch` | Gym branches with location and contact info |
| `Employee` | Staff records linked to branches |
| `Equipment` | Equipment inventory per branch |
| `Member` | Gym member profiles |
| `Class` | Fitness classes taught by employees |
| `Emergency_Contact` | Emergency contacts for members |
| `Enrolled` | M:N relationship between members and classes |
| `audit_log` | Automatic log entries written by triggers |

---

## ⚙️ Features

### 🔁 Triggers
Four database triggers fire automatically on key events and write to the `audit_log` table:

| Trigger | Event | Description |
|---------|-------|-------------|
| `after_branch_insert` | INSERT on Branch | Logs new branch openings |
| `after_employee_insert` | INSERT on Employee | Logs new employee hires |
| `after_member_insert` | INSERT on Member | Logs new member registrations |
| `after_member_delete` | DELETE on Member | Logs member departures |

### 📦 Stored Procedure
**`EnrollMember(p_mid, p_cid)`** — Enrolls a member into a fitness class with full validation:
- Checks if the member exists
- Checks if the class exists
- Prevents duplicate enrollment
- Returns a descriptive success or error message

### 🎫 Support Ticket System (MongoDB)
A document-based ticketing system stored in the `gym_db.tickets` MongoDB collection.

**Ticket document structure:**
```json
{
  "username": "string",
  "message": "string",
  "created_at": "datetime string",
  "status": true,
  "comments": []
}
```

**Supported operations:**
- Users can create new tickets and view their own active tickets
- Admins can view all active tickets, add comments, and resolve/close tickets
- Resolved tickets (`status: false`) are removed from all active views

---

## 🗂️ File Structure

```
├── index.php              # Main navigation page (user)
├── trigger_1.php          # Trigger demo page (add/delete branches, employees, members)
├── procedure_1.php        # Stored procedure page (class enrollment)
├── tickets.php            # User ticket list page
├── create_ticket.php      # Create new support ticket
├── ticket_detail.php      # View ticket details and comments (user & admin)
├── admin/
│   └── index.php          # Admin dashboard (all active tickets)
├── SQLDump.sql            # MySQL database setup with triggers, procedure, and seed data
└── Group22_phase3.sql     # Phase 3 schema + all 10 SQL queries
```

---

## 🚀 Setup & Installation

### Requirements
- PHP 7.4+
- MySQL 5.7+ / MariaDB
- MongoDB + PHP MongoDB Driver (`mongodb` PECL extension)
- Web server (Apache/Nginx) or PHP built-in server

### 1. Set Up MySQL Database
```bash
mysql -u root -p < SQLDump.sql
```
This creates the `gym_db` database, all tables, triggers, the stored procedure, and inserts sample data.

### 2. Configure Database Connection
Update the credentials in `trigger_1.php` and `procedure_1.php`:
```php
$mysqli = new mysqli("localhost", "your_user", "your_password", "gym_db");
```

### 3. Set Up MongoDB
Make sure MongoDB is running on `localhost:27017`. The PHP files connect automatically — no extra configuration needed.

### 4. Run the Application
```bash
php -S localhost:8000
```
Then open `http://localhost:8000/index.php` in your browser.

---

## 📊 Sample SQL Queries (Phase 3)

| # | Description | Category |
|---|-------------|----------|
| 1 | Retrieve all branches | Retrieve all rows |
| 2 | Employee names and phone by branch | Join with projection |
| 3 | Maximum employee salary | Aggregate MAX |
| 4 | Members sorted by age (ASC) | ORDER BY ascending |
| 5 | Branches with more than one employee | HAVING filter |
| 6 | Employees with salary > 12,000 | Selection + Projection |
| 7 | Members and their enrolled classes | 3-table join |
| 8 | Average age of all members | Aggregate AVG |
| 9 | Classes sorted alphabetically (DESC) | ORDER BY descending |
| 10 | Total enrollments per class | Join + GROUP BY |

---

## 🔒 Notes

- **No authentication layer** — this is a lightweight prototype as per project scope. Username input acts as a simple identifier.
- Database credentials are hardcoded for development purposes. Use environment variables in production.
- The admin panel is accessible directly via `admin/index.php` without a login guard (out of scope for this phase).

---

## 📝 License

This project was developed for academic purposes as part of the CS306 Database Systems course.
