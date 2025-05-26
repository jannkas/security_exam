# Security Measures in Wolt Replica

**Repository:** https://github.com/jannkas/security_exam  

**Developers:**  
- Hannah Tan West  
- Samman Kamal 
- Jannah Al-Kassab  

**Course:** Elective Security in Web Development, KEA  
**Supervisor:** Kristoffer Miklas  
**Date:** 28.05.2025  

---

## Overview

This project is a minimalist replica of an on-demand food delivery web application (“Wolt Replica”) built with Flask, MySQL and vanilla JavaScript. The primary goal is **security by design**: every feature—from authentication to file uploads—has been instrumented to mitigate real-world web threats without relying on heavyweight frameworks.

---

## Key Features

1. **Multi-Level Authentication & Authorization**  
   - User registration & login with email + password  
   - Roles: `admin`, `partner` (restaurant), `customer`  
   - Role selection when multiple roles exist  
   - Email verification flow  

2. **Session Management & CSRF Protection**  
   - Server-side sessions (Flask-Session)  
   - `HttpOnly` & `Secure` cookie flags  
   - Per-request CSRF token stored in session + validated on every modifying request  

3. **SQL Injection Defense**  
   - All database interactions use parameterized queries  
   - Input sanitization for full-text search queries  

4. **Cross-Site Scripting (XSS) Safeguards**  
   - Replace `.innerHTML` / `.insertAdjacentHTML` with `createElement()` + `.textContent`  
   - Strict Content-Security-Policy header  

5. **File Upload Validation**  
   - Front-end `accept=".jpg,.jpeg,.png"` restriction  
   - Back-end extension + MIME-type whitelist  
   - `werkzeug.utils.secure_filename()` + UUID-based filenames  

6. **Rate Limiting (Brute-Force Mitigation)**  
   - Flask-Limiter with Redis backend  
   - Per-IP caps on sensitive endpoints (e.g. login: 5/min)  

7. **Comprehensive Security Headers**  
   - HSTS (`Strict-Transport-Security`)  
   - `X-Frame-Options: DENY`  
   - `X-Content-Type-Options: nosniff`  
   - `Referrer-Policy: no-referrer`  
   - `Permissions-Policy` restrictions  

---

## Technology Stack

- **Backend:** Python 3.9, Flask  
- **Database:** MySQL (`mysql-connector-python`)  
- **Session Store & Rate Limit:** Redis  
- **Frontend:** HTML5, CSS3, Vanilla JavaScript, MojoCSS  
- **Templating:** Jinja2  
- **Containerization:** Docker & `docker-compose`  

---

## Credentials

Use the following demo accounts to log in as different roles:

| Role     | Email                 | Password    |
| -------- | --------------------- | ----------- |
| Admin    | admin@wolt.com        | Password123 |
| Partner  | restaurant@wolt.com   | Password123 |
| Customer | customer@wolt.com     | Password123 |

---

## Getting Started

1. **Clone the repo**  
   ```bash
   git clone https://github.com/jannkas/security_exam.git
   cd security_exam

2. **Build and run with Docker**  
    docker compose up --build

3. **Seed the database**
    # open a shell inside the Flask container
    docker exec -it wolt_flask /bin/bash

    # from inside the container, run:
    python seed.py

4. **Configure .env**
    # Create a file named .env in the project root with:
    SECRET_KEY="your-own-random-uuid4"

4. **Browse the app**
    Open your browser at http://localhost/ to explore the Wolt Replica.