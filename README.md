# Fine Chakula Cafe — Online Food Ordering System

Laravel 12 · PHP · MySQL (XAMPP) · Blade + plain CSS

Customers pick meals, check out (pickup or delivery), get an auto-generated account, and track their order.
Administrators view all orders and update status (Pending → Packed → On Delivery → Delivered).

## Requirements
- PHP 8.2+ (XAMPP's PHP works if it is 8.2 or newer)
- Composer
- XAMPP with MySQL running
- Visual Studio Code

## Setup (about 5 minutes)

### 1. Open the project
VS Code → File → Open Folder → select this `fine-chakula` folder.
Open the terminal in VS Code (Ctrl + `). Every command below runs from this folder.

### 2. Install PHP dependencies
```
composer install
```
(The `vendor/` folder is not included — this command creates it.)

### 3. Set up the database — pick ONE option

**Option A — Import the SQL file (phpMyAdmin)**
1. Start MySQL in the XAMPP Control Panel.
2. Open http://localhost/phpmyadmin → **Import**.
3. Choose `database/hotel_db_groupc.sql` → **Import**.
   This creates the `hotel_db_groupc` database, all tables, the products, the admin account and one demo order.

**Option B — Let Laravel build it**
1. In phpMyAdmin, create an empty database named `hotel_db_groupc`.
2. Run:
```
php artisan migrate --seed
```

### 4. Start the app
```
php artisan config:clear
php artisan serve
```
Open http://127.0.0.1:8000

## Logins
| Role | Email | Password |
|------|-------|----------|
| Administrator | admin@finechakula.com | Admin12345 |
| Demo customer (SQL import only) | customer@finechakula.com | Customer123 |

New customers get a temporary password on the order-success page (shown once).
These passwords are for local development only.

## Configuration
`.env` is already set for XAMPP: database `hotel_db_groupc`, user `root`, empty password,
`SESSION_DRIVER=file`. If your MySQL root has a password, set `DB_PASSWORD` in `.env`.
If `APP_KEY` is ever empty, run `php artisan key:generate`.

## Troubleshooting
| Problem | Fix |
|---------|-----|
| SQLSTATE connection error | Start MySQL in XAMPP; check `DB_*` in `.env`; `php artisan config:clear` |
| Table not found | Import the SQL file, or run `php artisan migrate` |
| No meals on the menu | `php artisan db:seed` |
| 419 Page Expired | `php artisan config:clear`; make sure `SESSION_DRIVER=file` |
| 403 on /admin | Log in with the admin account |
| "Email already taken" at checkout | Each checkout creates a new account — use a new email |
| `could not find driver` | Enable `extension=pdo_mysql` in XAMPP's `php.ini` (and restart) |

## Project map
- `app/Models` — User, Product, Order, OrderItem
- `app/Http/Controllers` — Product, Order, Auth, Customer, AdminOrder
- `app/Http/Middleware/AdminMiddleware.php` — blocks non-admins (alias `admin` in `bootstrap/app.php`)
- `routes/web.php` — all routes
- `database/migrations` — users changes, products, orders, order_items
- `database/seeders` — products and admin account
- `database/hotel_db_groupc.sql` — ready-to-import XAMPP database
- `resources/views` — Blade pages · `public/css/style.css` — styling
