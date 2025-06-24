
# 🌐 Full-Stack Application Deployment on AWS Lightsail

This guide provides step-by-step instructions to deploy your Node.js app on **AWS Lightsail**, using **PM2** for process management.

Whether you're deploying for development, staging, or production, this document will help you launch and maintain your app on a live server from scratch.

---

## 🧾 Table of Contents

- [Prerequisites](#-prerequisites)
- [Deployment Steps](#-deployment-steps)
  1. [AWS Account Setup](#1-aws-account-setup)
  2. [Create Lightsail Instance](#2-create-lightsail-instance)
  3. [Download & Secure SSH Key](#3-download--secure-ssh-key)
  4. [Connect to Your Instance](#4-connect-to-your-instance)
  5. [Update Packages & Install Dependencies](#5-update-packages--install-dependencies)
  6. [Install Node.js & PM2](#6-install-nodejs--pm2)
  7. [Clone & Configure Your App](#7-clone--configure-your-app)
  8. [Build & Run App in Background](#8-build--run-app-in-background)
  9. [Manage the App with PM2](#9-manage-the-app-with-pm2)
- [Next Steps (Optional)](#-next-steps-optional)
- [License](#-license)

---

## 📋 Prerequisites

- AWS account
- GitHub repository with your full-stack app
- `.env` file with all environment variables
- Terminal access and basic Linux/CLI knowledge
- SSH `.pem` key for Lightsail access

---

## 🚀 Deployment Steps

### 1. AWS Account Setup

Log into your AWS Console and open [AWS Lightsail](https://lightsail.aws.amazon.com/).

---

### 2. Create Lightsail Instance

- Choose an **Ubuntu** OS (e.g., Ubuntu 20.04)
- Select a suitable instance size (e.g., 1 GB RAM for small apps)
- Choose your region (e.g., us-east-2)
- Click **Create Instance**

---

### 3. Download & Secure SSH Key

- Download the default `.pem` SSH key for your region
- Move it to a safe path and secure it with:

```bash
chmod 400 '/path/to/your/LightsailDefaultKey.pem'
```

---

### 4. Connect to Your Instance

Use SSH to connect to your server:

```bash
ssh -i '/path/to/LightsailDefaultKey.pem' ubuntu@<PUBLIC_IP>
```

**Example:**

```bash
ssh -i '/home/webiwork-25/Desktop/Tarun/AWS/LightsailDefaultKey-us-east-2.pem' ubuntu@3.148.147.85
```

---

### 5. Update Packages & Install Dependencies

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install git curl unzip -y
```

---

### 6. Install Node.js & PM2

```bash
# Install Node.js (v18 recommended)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Install PM2 globally
sudo npm install -g pm2
```

---

### 7. Clone & Configure Your App

#### Clone Your GitHub Repository

```bash
git clone https://<your-username>:<your-token>@github.com/<your-username>/<your-repo>.git
cd <your-repo>
```

**Example:**

```bash
git clone https://github.com/maidway/TEST_APP.git
cd TEST_APP
```

#### (Optional) Set Git Remote URL

```bash
git remote set-url origin https://github.com/maidway/TEST_APP.git
```

#### Add Environment Variables

```bash
nano .env
```

Paste your `.env` content here. Save with `CTRL + O`, then `Enter`, and exit using `CTRL + X`.

---

### 8. Build & Run App in Background

```bash
npm install
npm run build

# Start app using PM2
NODE_ENV=production pm2 start dist/index.js --name TEST_APP
```

---

### 9. Manage the App with PM2

```bash
# Save the PM2 process list
pm2 save

# Set PM2 to auto-start on server reboot
pm2 startup

# View running apps
pm2 list

# Restart app
pm2 restart TEST_APP

# View logs
pm2 logs

# Clear logs
pm2 flush
```

---

## 🔧 Next Steps (Optional)

- 🧰 Set up **Nginx** as a reverse proxy
- 🔐 Add SSL with **Let's Encrypt**
- 🌐 Bind your custom domain
- ⚙️ Enable **GitHub Actions** for auto-deployment
- 📊 Set up monitoring and alerts

---

## 📝 License

This project is licensed under the [MIT License](LICENSE).

---

💡 **Tip:** Always test your build locally before pushing to production. Use `.env.production` for clean environment management and consistent behavior in deployment.
