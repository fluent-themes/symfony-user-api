# Symfony UserAPI - RESTful Service

A minimal REST API showcasing Symfony features such as attribute routing, dependency injection, and Doctrine ORM with SQLite. Designed for WordPress developers exploring modern PHP frameworks, it introduces basic Symfony concepts and helps users understand the key similarities and differences between WordPress and Symfony workflows.

## Features

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/users | List all users |
| GET | /api/users/{id} | Get single user |
| POST | /api/users | Create new user |
| PUT | /api/users/{id} | Update user |
| DELETE | /api/users/{id} | Delete user |

---

## Part 1: Installing Prerequisites

Before setting up this project, you need PHP, Composer, and optionally the Symfony CLI.

### 1.1 Install PHP 8.1+

**Windows:**
1. Go to https://windows.php.net/download/
2. Download **VS16 x64 Thread Safe** ZIP (e.g., php-8.1.x-Win32-vs16-x64.zip)
3. Extract to `C:\php`
4. Add `C:\php` to your system PATH:
   - Press `Win + R`, type `sysdm.cpl`, press Enter
   - Click **Advanced** → **Environment Variables**
   - Under **System variables**, find **Path**, click **Edit**
   - Click **New**, add `C:\php`, click **OK**
5. Copy `php.ini-development` to `php.ini`
6. Verify: Open new Command Prompt, type: `php -v`

**Mac:**
```bash
brew install php
php -v
```

**Linux:**
```bash
sudo apt update
sudo apt install php8.1 php8.1-cli php8.1-xml php8.1-mbstring php8.1-sqlite3
php -v
```

### 1.2 Install Composer

**Windows:**
1. Download and run https://getcomposer.org/Composer-Setup.exe
2. Follow installer
3. Verify in new Command Prompt: `composer -V`

**Mac:**
```bash
brew install composer
composer -V
```

**Linux:**
```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php composer-setup.php
sudo mv composer.phar /usr/local/bin/composer
composer -V
```

### 1.3 Install Symfony CLI (Optional but Recommended)

The Symfony CLI provides a local web server with HTTPS support.

**Windows (PowerShell as Admin):**
```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
irm https://get.symfony.com/cli/installer | iex
```
Close and reopen Terminal, then verify: `symfony -v`

**Mac:**
```bash
curl -sS https://get.symfony.com/cli/installer | bash
# Follow the output to add to PATH
symfony -v
```

**Linux:**
```bash
curl -sS https://get.symfony.com/cli/installer | bash
sudo mv ~/.symfony*/bin/symfony /usr/local/bin/
symfony -v
```

### Verify Prerequisites

```bash
php -v          # PHP 8.1+
composer -V     # Composer 2.x
symfony -v      # Optional
```

---

## Part 2: Setting Up UserAPI (The "Plugin" Layer)

Symfony projects are created using Composer or the Symfony CLI. We'll create a fresh Symfony project with default files, then add our custom files.

### Step 1: Create a New Symfony Project

```bash
# Navigate to your projects folder
cd ~/projects          # Mac/Linux
cd C:\projects         # Windows

# Option A: Using Symfony CLI (recommended)
symfony new userapi-symfony --version="6.4.*" --webapp

# Option B: Using Composer
composer create-project symfony/skeleton:"6.4.*" userapi-symfony
cd userapi-symfony
composer require webapp
```

Wait 2-5 minutes for installation.

### Step 2: Enter the Project Folder

```bash
cd userapi-symfony
```

You should see default folders like `bin/`, `config/`, `src/`, `public/`, etc.

### Step 3: Install Doctrine ORM

```bash
composer require symfony/orm-pack
```

### Step 4: Configure the Database

Open the `.env` file in a text editor. Find the `DATABASE_URL` line and modify it:

```bash
# Comment out the existing DATABASE_URL by adding # at the start
# DATABASE_URL="postgresql://...

# Add this line for SQLite
DATABASE_URL="sqlite:///%kernel.project_dir%/var/data.db"
```

### Step 5: Add the Downloaded Project Files

Download the files from this repo to your Computer. Then, create the necessary folders (if they don't exist):

Windows:
```
mkdir src\Entity
mkdir src\Repository
mkdir src\Controller
```

Mac/Linux:
```bash
mkdir -p src/Entity src/Repository src/Controller
```

Copy the downloaded project files:
- `User.php` → `src/Entity/User.php`
- `UserRepository.php` → `src/Repository/UserRepository.php`
- `UserController.php` → `src/Controller/UserController.php`

### Step 6: Create Database and Schema

Run these commands in order:

```bash
# Create the SQLite database file
php bin/console doctrine:database:create

# Create the users table from the Entity
php bin/console doctrine:schema:create
```

You should see success messages like "Created database" and "Executing 1 query".

### Step 7: Run the Application

**Option A - Using Symfony CLI:**
```bash
symfony serve
```
Opens at 127.0.0.1:8000

**Option B - Using PHP built-in server:**
```bash
php -S localhost:8000 -t public
```
Opens at localhost:8000

**Test the API:**
Open browser: localhost:8000/api/users

You should see: `[]` (empty array, no users yet)

---

## API Request/Response Examples

### List All Users
```
GET /api/users

Response 200:
[
  {"id": 1, "name": "John Doe", "email": "john@example.com", "createdAt": "2025-02-22 10:30:00"},
  {"id": 2, "name": "Janifer", "email": "jane@example.com", "createdAt": "2025-02-22 11:00:00"}
]
```

### Get Single User
```
GET /api/users/1

Response 200:
{"id": 1, "name": "John Doe", "email": "john@example.com", "createdAt": "2025-02-22 10:30:00"}

Response 404 (not found):
{"error": "User not found"}
```

### Create User
```
POST /api/users
Content-Type: application/json

{"name": "John Doe", "email": "john@example.com"}

Response 201:
{"id": 1, "name": "John Doe", "email": "john@example.com", "createdAt": "2025-02-22 10:30:00"}

Response 400 (validation error):
{"error": "Name and email are required"}
```

### Update User
```
PUT /api/users/1
Content-Type: application/json

{"name": "John Updated", "email": "john.new@example.com"}

Response 200:
{"id": 1, "name": "John Updated", "email": "john.new@example.com", "createdAt": "2025-02-22 10:30:00"}
```

### Delete User
```
DELETE /api/users/1

Response 204: (no content)
```

---

## Testing with Postman

A Postman collection is included: `userapi.postman_collection.json`

**To import:**
1. Open Postman
2. Click **Import** button (top left)
3. Drag and drop the JSON file or click **Upload Files**
4. The "UserAPI" collection appears in your sidebar
5. Before testing, make sure your server is running

---

## Your final Project Structure will be

```
userapi-symfony/
├── bin/
│   └── console              # Symfony CLI commands
├── config/                  # Configuration files
├── public/
│   └── index.php            # Entry point
├── src/
│   ├── Controller/
│   │   └── UserController.php   # REST API endpoints
│   ├── Entity/
│   │   └── User.php             # Doctrine entity
│   └── Repository/
│       └── UserRepository.php   # Database queries
├── var/
│   └── data.db              # SQLite database (created automatically)
├── vendor/                  # Dependencies
├── .env                     # Environment config
├── composer.json
└── README.md
```

---

## Deployment to Shared Hosting

### Step 1: Prepare for Production

```bash
# Install production dependencies only
composer install --no-dev --optimize-autoloader

# Clear and warm cache
php bin/console cache:clear --env=prod
```

Edit `.env`:
```
APP_ENV=prod
APP_SECRET=generate-a-random-32-char-string
```

### Step 2: Upload Files

Upload via FTP to your hosting (e.g., `/public_html/userapi/`).

**Important:** The `var/` folder needs write permissions:
```bash
chmod 755 var/
# or
chmod 775 var/
```

### Step 3: Set Document Root

Point your domain/subdomain to the `/public` folder.

Or access via: `yourdomain.com/userapi/public/api/users`

### Step 4: Initialize Database on Server

SSH into server (or use hosting control panel terminal):
```bash
php bin/console doctrine:database:create
php bin/console doctrine:schema:create
```

### Step 5: Test

Visit: yourdomain.com/api/users

Should see: `[]`

---

## Symfony Concepts Demonstrated

- **Attribute Routing**: `#[Route('/api/users')]` defines URLs directly on controller methods
- **Dependency Injection**: Controller receives `EntityManagerInterface` and `UserRepository` through constructor - Symfony's service container autowires these automatically
- **Service Container**: All services (EntityManager, repositories, controllers) are managed by Symfony's DI container, configured via `config/services.yaml`
- **Middleware Concepts**: Symfony's HttpKernel uses event listeners and kernel events (Request → Controller → Response) which function like middleware - the framework handles CORS, content negotiation, and exception handling through this pipeline
- **JsonResponse**: Returns JSON with proper Content-Type headers
- **Doctrine ORM**: Entity classes map to database tables, no raw SQL needed
- **HTTP Status Codes**: 200 OK, 201 Created, 204 No Content, 400 Bad Request, 404 Not Found

---

## Key Lessons

Symfony fundamentally differs from WordPress in architecture:

- **Attribute Routing vs Hooks**: Instead of `add_action('rest_api_init', ...)`, Symfony uses `#[Route]` attributes directly on controller methods. The URL structure is explicit and visible.

- **Dependency Injection vs Globals**: WordPress relies on global functions (`get_option()`, `$wpdb`). Symfony injects dependencies through constructors, making code easier to test and understand.

- **Doctrine ORM vs $wpdb**: Instead of raw SQL queries, Doctrine uses Entity objects. You work with PHP objects (`$user->setName('John')`) rather than SQL strings.

- **Structured Response vs echo**: Symfony's `JsonResponse` handles JSON encoding, headers, and status codes. No manual `header('Content-Type: application/json')` needed.

- **File Structure**: Symfony enforces separation (Entity, Repository, Controller) while WordPress mixes everything in `functions.php` or plugin files.
