# FUTMINNA Survey Management System

A secure, full-featured survey platform built for **Federal University of Technology, Minna**. Supports survey creation, response collection, real-time analytics, and CSV export all in pure procedural PHP with no frameworks.

![PHP](https://img.shields.io/badge/PHP-8.1+-777BB4?style=flat-square&logo=php&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.0+-4479A1?style=flat-square&logo=mysql&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES2020-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

---

## Screenshots

### Dashboard
<img width="1366" height="768" alt="DASHBOARD" src="https://github.com/user-attachments/assets/fdee342b-8eb5-406a-8237-95d040919b4b" />


### Surveys
<img width="1366" height="768" alt="SURVEYS" src="https://github.com/user-attachments/assets/b9491f8a-87c3-4517-ad87-97b2e8f85df2" />


### Create Survey
<img width="1366" height="768" alt="CREATE SURVEY" src="https://github.com/user-attachments/assets/e36dbcc5-ba9b-40bf-8b23-13db34bf17b3" />


### Response Log
<img width="1366" height="768" alt="RESPONSE LOG" src="https://github.com/user-attachments/assets/8d90baf8-3c80-49ae-a00e-638079c24503" />


### Response Analytics
<img width="1366" height="768" alt="RESPONSE ANALYTICS" src="https://github.com/user-attachments/assets/e75b4ca4-6d32-42eb-9fdc-7591b9ae095c" />


### Export Data
<img width="1366" height="768" alt="EXPORT DATA" src="https://github.com/user-attachments/assets/a859236f-daac-483c-b007-d1daf935dd39" />


### Change Password
<img width="1366" height="768" alt="CHANGE PASSWORD" src="https://github.com/user-attachments/assets/24b4aa63-682a-492c-9f34-267997f51603" />


### Registration
<img width="1366" height="768" alt="REGISTRATION" src="https://github.com/user-attachments/assets/2917a70c-3c45-4f6f-997b-786a713e32a1" />


### Logn
<img width="1366" height="768" alt="LOGIN" src="https://github.com/user-attachments/assets/a4062db1-e9a3-40ca-8e74-018337555713" />


### Survey Q&A
<img width="1366" height="768" alt="SURVEY PAGE" src="https://github.com/user-attachments/assets/4ca70b30-35e2-482e-8f07-eb170bde2ce5" />


### Survey Submission
<img width="1366" height="768" alt="SURVEY RESPONSE" src="https://github.com/user-attachments/assets/17c3afd0-b170-45fd-b139-06871576cab9" />

---

## Features

| Area | Details |
|---|---|
| **Auth** | Registration, login, CSRF-protected sessions, bcrypt passwords |
| **Surveys** | Create, edit, delete; active / draft / closed states; optional expiry |
| **Questions** | Multiple choice (MCQ), free text, rating (1–10) |
| **Responses** | Anonymous or identified; AJAX submission (no page reload) |
| **Analytics** | Pie charts (MCQ), rating distributions, text samples — per question |
| **Dashboard** | 7-day response trend, status breakdown, recent survey list |
| **Export** | UTF-8 BOM CSV (Excel-compatible) per survey |
| **Security** | PDO prepared statements, CSRF tokens, XSS escaping, input sanitization |
| **Isolation** | All data is owner-scoped — users only see their own surveys |

---

## Requirements

- PHP 8.1+
- MySQL 8.0+ (or MariaDB 10.6+)
- Apache with `mod_rewrite` enabled

No Composer. No frameworks. No dependencies beyond what's already on the server.

---

## Quick Start

### 1. Import the database

```bash
mysql -u root -p < schema.sql
```

This creates the `futminna_surveys` database and seeds a default admin account.

**Default login:**
```
Email:    admin@futminna.edu.ng
Password: admin
```
> Change this immediately after first login via **Change Password** in the sidebar.

### 2. Configure the app

Open `config/config.php` and update:

```php
define('DB_HOST', 'localhost');
define('DB_NAME', 'futminna_surveys');
define('DB_USER', 'your_db_user');
define('DB_PASS', 'your_db_password');

define('APP_URL', 'http://localhost/futminna-survey'); // no trailing slash
```

### 3. Deploy

Place the project folder in your web root:

- **Apache/Linux:** `/var/www/html/futminna-survey/`
- **XAMPP/WAMP:** `htdocs/futminna-survey/`

Enable required Apache modules:

```bash
sudo a2enmod rewrite headers
sudo systemctl restart apache2
```

Ensure `AllowOverride All` is set in your virtual host config.

Access the app at: `http://localhost/futminna-survey/`

### 4. Set permissions

```bash
chmod 755 exports/
chmod 644 config/config.php
```

---

## Project Structure

```
futminna-survey/
├── config/
│   └── config.php              # DB credentials and app constants
│
├── includes/
│   ├── db.php                  # PDO singleton + query helpers
│   ├── helpers.php             # CSRF, sessions, flash messages, sanitization
│   ├── header.php              # Shared admin layout (top)
│   └── footer.php              # Shared admin layout (bottom)
│
├── admin/
│   ├── login.php               # Authentication
│   ├── register.php            # New user registration
│   ├── change_password.php     # Password update for logged-in users
│   ├── logout.php              # Session teardown
│   ├── dashboard.php           # Overview with live charts
│   ├── surveys.php             # Survey list (AJAX status/delete)
│   ├── survey_create.php       # Create survey + dynamic question builder
│   ├── survey_edit.php         # Edit survey and questions
│   ├── responses.php           # Paginated response log
│   ├── analytics.php           # Per-question charts and stats
│   ├── export.php              # CSV export (streaming)
│   └── ajax/
│       ├── delete_survey.php   # POST: delete a survey
│       └── update_status.php   # POST: change survey status
│
├── public/
│   ├── survey.php              # Respondent-facing survey form
│   └── submit.php              # AJAX response handler (transactional)
│
├── assets/
│   ├── css/admin.css           # Full UI — FUTMINNA-branded, responsive
│   └── js/admin.js             # Question builder, AJAX, Chart.js wrappers
│
├── exports/                    # Reserved for local file exports
├── schema.sql                  # Database schema + seed data
├── .htaccess                   # Security rules, header hardening
└── index.php                   # Root redirect
```

---

## Security

- **PDO prepared statements** — no string interpolation in any query
- **CSRF tokens** — validated on every POST, including AJAX requests via `X-Csrf-Token` header
- **bcrypt** — `password_hash()` with `PASSWORD_BCRYPT` at cost 12
- **XSS prevention** — all output wrapped in `htmlspecialchars()` via a shared `h()` helper
- **`.htaccess`** — blocks direct access to `includes/`, `config/`, and `.sql` files
- **Session hardening** — `httponly`, `samesite=Strict`, `session_regenerate_id()` on login
- **DB transactions** — response submissions are atomic; partial writes roll back automatically

> Set `DEBUG_MODE = false` in `config/config.php` before deploying to production.

---

## Stack

- **Backend:** PHP 8.1+ (procedural, no MVC framework)
- **Database:** MySQL 8.0+ via PDO
- **Frontend:** HTML5, CSS3, Vanilla JavaScript (ES2020)
- **Charts:** Chart.js 4.x (CDN)
- **Fonts:** DM Serif Display + Sora (Google Fonts)

---

## Contributing

Pull requests are welcome. For significant changes, open an issue first to discuss what you'd like to change.

---

**Federal University of Technology, Minna (FUTMINNA)**  
Survey Management System v1.0
