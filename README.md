# 🎵 Music Cloud Community System (MCCS)

> [!IMPORTANT]
> ### 🔑 Quick Credentials Reference:
> - **Admin Login:** Email: `admin@admin.com` *(or Username: `admin`)* | Password: `admin123`
> - **User Login:** Email: `jsmith@sample.com` *(or Username: `jsmith`)* | Password: `jsmith123`
> - **1-Click DB Importer & Reset Tool:** Open `https://<your-render-url>/db_setup.php` to import tables or reset password anytime.

Production-ready, Dockerized web application built with **PHP 8.2**, **Apache**, **Aiven Cloud MySQL**, and **AdminLTE 3**, optimized for high-performance deployment on **Render**.

---

## 🌐 Live URLs & Endpoints (Render Deployment)

| Portal | Endpoint URL | Description |
| :--- | :--- | :--- |
| **Public Website / Dashboard** | `https://<your-render-service>.onrender.com/` | Music player, playlists, genre browsing, and media library |
| **Login Portal** | `https://<your-render-service>.onrender.com/login.php` | User & Administrator sign-in portal |
| **1-Click DB Setup Tool** | `https://<your-render-service>.onrender.com/db_setup.php` | Cloud MySQL Health Check & Auto-Migrator (Aiven SSL Ready) |

---

## ⚙️ Environment Variables (Render Dashboard)

Configure these in your Render Web Service dashboard under **Environment**:

| Variable | Configured Value | Notes |
| :--- | :--- | :--- |
| `DB_SERVER` | `mysql-1a85412b-maheshchouhanmsk12-c513.a.aivencloud.com` | Aiven MySQL cluster host |
| `DB_PORT` | `26284` | Aiven 5-digit custom MySQL port |
| `DB_USERNAME` | `avnadmin` | Master MySQL username |
| `DB_PASSWORD` | *(Auto-configured in code fallback / Enter your Aiven Password)* | Aiven master password |
| `DB_NAME` | `music_db` | Target database name *(pre-configured with schema)* |
| `DB_SSL` | `true` | **Required** for Aiven Cloud (`require_secure_transport=ON`) |
| `BASE_URL` | *(Leave empty for auto-detect)* | Optional override for app base URL |

---

## 🚀 How to Deploy on Render (Step-by-Step)

### Step 1: Connect GitHub Repository
1. Log in to your [Render Dashboard](https://dashboard.render.com/).
2. Click **New +** &rarr; Select **Web Service**.
3. Choose **Build and deploy from a Git repository**.
4. Select `1234Adityarajput/Music-Cloud-Community-System-Demo`.

### Step 2: Configure Service Settings
- **Name:** `music-cloud-community` *(or any preferred name)*
- **Region:** *Oregon (US West)* or *Frankfurt (EU Central)*
- **Branch:** `main`
- **Environment:** `Docker`
- **Dockerfile Path:** `./Dockerfile`
- **Instance Type:** `Free`

### Step 3: Add Environment Variables
Under the **Environment** section, verify or add:
- `DB_SERVER` = `mysql-1a85412b-maheshchouhanmsk12-c513.a.aivencloud.com`
- `DB_PORT` = `26284`
- `DB_USERNAME` = `avnadmin`
- `DB_NAME` = `music_db`
- `DB_SSL` = `true`

*(Note: The database password has built-in code fallback so the container runs immediately).*

Click **Deploy Web Service**.

### Step 4: 1-Click Database Setup & Health Check
1. Once the build finishes and the service status is **Live**, open:
   ```text
   https://<your-render-url>/db_setup.php
   ```
2. The setup tool will verify your SSL connection to Aiven Cloud MySQL and check table status.
3. Click **"Import Database Now (1-Click)"** to automatically execute `database/music_db.sql` *(if not already imported)*.
4. Click **"Go to Login Page"** and log in with `admin@admin.com` / `admin123`.

---

## 🛠️ Architecture & Production Fixes Applied

1. **PHP 8.2 Compatibility:**
   - Property initializations moved inside class constructors to eliminate fatal deprecation errors.
   - Guarded database queries with `if ($qry && $qry instanceof mysqli_result)` and safe null checks across all UI views to prevent PHP 8.x crashes on empty tables.
2. **Aiven Cloud MySQL SSL Enforcement:**
   - Upgraded `db_connect.php` to initialize via `mysqli_init()` and activate the `MYSQLI_CLIENT_SSL` flag.
   - Added support for 5-digit custom cloud ports (e.g., 26284).
   - Auto-detection and seamless fallback between `music_db` and `defaultdb`.
3. **Render Reverse-Proxy & Port 10000 Leak Prevention:**
   - Added `UseCanonicalName Off` and `UseCanonicalPhysicalPort Off` to Apache configuration.
   - Added smart `.htaccess` trailing-slash redirect rules that strip internal container ports (`:10000` or `:80`).
   - Dynamically mapped Apache to Render's assigned `$PORT` environment variable in `start.sh`.
4. **File Uploads & Media Storage:**
   - Pre-configured `assets/uploads/` write permissions (`www-data:www-data`, chmod 775) in Dockerfile and startup script.

---

## 📂 Project Structure

```text
├── assets/                 # AdminLTE UI plugins, CSS, JavaScript, and uploads
│   └── uploads/            # Track MP3s, album covers, and user avatars
├── database/
│   └── music_db.sql        # Complete MySQL database schema & sample seed data
├── .dockerignore           # Excludes local files and backups from Docker builds
├── .gitignore              # Git ignore rules (*.original, environment files)
├── .htaccess               # Apache reverse-proxy redirect rules
├── admin_class.php         # Application core business logic (PHP 8.2 compatible)
├── ajax.php                # AJAX endpoints router
├── db_connect.php          # Aiven Cloud MySQL connection handler with SSL
├── db_setup.php            # 1-Click web installer & diagnostic health check tool
├── Dockerfile              # Production Dockerfile (php:8.2-apache)
├── home.php                # System dashboard metrics
├── index.php               # Main authenticated portal controller
├── login.php               # Login & account creation portal
├── render.yaml             # Render Blueprint configuration
├── start.sh                # Container entrypoint script
└── test_aiven.php          # CLI script to test Aiven MySQL connection
```

---

## 🔐 Credentials Summary (ID & Passwords)

### 1. Application Login Credentials
- **Admin Email / ID:** `admin@admin.com` *(or username: `admin`)*
- **Admin Password:** `admin123`
- **Subscriber Email / ID:** `jsmith@sample.com`
- **Subscriber Password:** `jsmith123`

### 2. Aiven Cloud MySQL Configuration
- **Host / DB_SERVER:** `mysql-1a85412b-maheshchouhanmsk12-c513.a.aivencloud.com`
- **Port / DB_PORT:** `26284`
- **User / DB_USERNAME:** `avnadmin`
- **Database / DB_NAME:** `music_db`
- **SSL / DB_SSL:** `true`
- **Aiven Auth Key:** Baked into `db_connect.php` via base64 encoded token `QVZOU19MU0xEdDV6NGFkSzQ5R0xsVFQ4`

---
*Music Cloud Community System &bull; Repository: [1234Adityarajput/Music-Cloud-Community-System-Demo](https://github.com/1234Adityarajput/Music-Cloud-Community-System-Demo)*
