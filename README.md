# 🔗 CertChain – Blockchain Certificate Validation

A Laravel web application that issues and verifies academic/professional certificates using the Ethereum blockchain.

---

## 📋 Requirements – Install These First

| Software | Version | Download Link |
|---|---|---|
| PHP | 8.2 or higher | https://www.php.net/downloads |
| Composer | Latest | https://getcomposer.org/download |
| Node.js | 18 or higher | https://nodejs.org |
| MySQL | 8.0 or higher | https://dev.mysql.com/downloads |
| Git | Latest | https://git-scm.com |

---

## 🚀 How to Run in VS Code (Step by Step)

### Step 1 – Open the Project
1. Extract the ZIP file to a folder (e.g., `C:\Projects\certchain`)
2. Open **VS Code**
3. Click **File → Open Folder** and select the `certchain` folder
4. Open the **integrated terminal**: press `` Ctrl + ` `` (backtick)

---

### Step 2 – Install PHP Dependencies
```bash
composer install
```
Wait for it to finish downloading all Laravel packages.

---

### Step 3 – Install JavaScript Dependencies
```bash
npm install
```

---

### Step 4 – Set Up Environment File
```bash
# Copy the example env file
cp .env.example .env

# Generate the application key
php artisan key:generate
```

---

### Step 5 – Configure Database
1. Open **MySQL** and create a database:
   ```sql
   CREATE DATABASE certchain;
   ```
2. Open `.env` in VS Code and update these lines:
   ```
   DB_DATABASE=certchain
   DB_USERNAME=root
   DB_PASSWORD=your_mysql_password
   ```

---

### Step 6 – Run Database Migrations + Seed Admin User
```bash
php artisan migrate --seed
```
This creates all tables and a default admin account:
- **Email:** admin@certchain.com
- **Password:** password123

---

### Step 7 – Start the Servers (Open 2 terminals)

**Terminal 1 – Laravel backend:**
```bash
php artisan serve
```

**Terminal 2 – Tailwind CSS compiler:**
```bash
npm run dev
```

---

### Step 8 – Open in Browser
Go to: **http://localhost:8000**

---

## 🌐 Blockchain Setup (Optional for Development)

The app works **without blockchain** for local testing — certificates are stored in MySQL.

To enable real blockchain verification:

### 1. Get a Free Infura Account
- Go to https://infura.io and sign up (free)
- Create a new project → copy your **Project ID**
- Add to `.env`: `INFURA_PROJECT_ID=your_id_here`

### 2. Deploy the Smart Contract
- Go to https://remix.ethereum.org
- Create a new file and paste the contents of `contracts/CertificateRegistry.sol`
- Compile it (Solidity 0.8.20)
- Deploy to **Sepolia testnet** (need MetaMask + test ETH from https://sepoliafaucet.com)
- Copy the **deployed contract address** to `.env`: `CONTRACT_ADDRESS=0x...`

### 3. Add Your Wallet Private Key
- In MetaMask: Account → Export Private Key
- Add to `.env`: `OWNER_PRIVATE_KEY=your_private_key`

> ⚠️ NEVER commit your private key to Git. Keep it only in `.env`

---

## 📁 Project Structure

```
certchain/
├── app/
│   ├── Http/Controllers/
│   │   ├── HomeController.php          ← Landing page
│   │   ├── CertificateController.php   ← Issue & manage certs
│   │   └── VerificationController.php  ← Verify certs
│   ├── Models/
│   │   └── Certificate.php             ← Certificate model
│   └── Services/
│       └── BlockchainService.php       ← Ethereum interaction
├── contracts/
│   └── CertificateRegistry.sol         ← Smart contract (deploy to Ethereum)
├── database/
│   ├── migrations/                     ← Database table definitions
│   └── seeders/
│       └── DatabaseSeeder.php          ← Creates admin user
├── resources/
│   ├── css/app.css                     ← Tailwind CSS
│   ├── js/app.js                       ← JavaScript
│   └── views/
│       ├── layouts/app.blade.php       ← Main layout (navbar, footer)
│       ├── home.blade.php              ← Landing page
│       ├── auth/login.blade.php        ← Admin login
│       ├── verify/
│       │   ├── index.blade.php         ← Verification form
│       │   └── result.blade.php        ← Verification result
│       └── certificates/
│           ├── create.blade.php        ← Issue cert form
│           ├── show.blade.php          ← Certificate details
│           └── index.blade.php         ← Admin list
└── routes/web.php                      ← URL routes
```

---

## 🔑 Default Admin Login
- URL: http://localhost:8000/login
- Email: `admin@certchain.com`
- Password: `password123`

---

## 🔄 How It Works

1. **Admin logs in** → fills in certificate details
2. **SHA-256 hash** is generated from the certificate data
3. Hash is **stored in MySQL** (metadata) + submitted to **Ethereum smart contract**
4. **Anyone** can go to `/verify`, enter the hash, and see if it's valid
5. The system checks both the local DB and the blockchain

---

## 🛠️ VS Code Extensions (Recommended)
Install these from the Extensions panel (`Ctrl+Shift+X`):
- **PHP Intelephense** – PHP autocomplete
- **Laravel Blade Snippets** – Blade syntax highlighting
- **Tailwind CSS IntelliSense** – Tailwind class suggestions
- **GitLens** – Git integration

---

## ❓ Troubleshooting

| Problem | Solution |
|---|---|
| `composer: command not found` | Install Composer from https://getcomposer.org |
| `php: command not found` | Install PHP 8.2+ and add to PATH |
| Database connection error | Check DB_PASSWORD in .env matches your MySQL |
| Port 8000 in use | Run `php artisan serve --port=8001` |
| Tailwind not working | Make sure `npm run dev` is running |
