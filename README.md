# 📁 Secure File Manager Portal with AWS Cognito, IAM Roles, and S3

A secure, scalable, and role-based file manager web application built with **Next.js (TypeScript)** that uses **AWS Cognito, IAM, and S3**. This solution enables administrators to manage user groups, assign folder-level access with `read` or `write` permissions, and enforce security using IAM roles dynamically created and linked with Cognito groups.

---

## 🚀 Technologies Used

| Layer              | Technology                               |
| ------------------ | ---------------------------------------- |
| Node.js Version    | **v22.14.0**                             |
| Frontend           | Next.js 15, React 19, TypeScript         |
| State Mgmt         | Redux Toolkit                            |
| UI Framework       | Material UI (MUI), Emotion               |
| Forms & Validation | Formik, Yup                              |
| Table              | AG Grid                                  |
| AWS Services       | Cognito (User & Identity Pools), IAM, S3 |
| SDKs               | AWS SDK v3, Amplify                      |
| Styling            | Prettier, ESLint,                        |
| Misc               | Axios,                                   |

---

## 🔐 Core Features

### 🛠 Admin Panel

- Create new **Cognito groups** via admin dashboard.
- Create new **Cognito Users** via admin dashboard.
- Create new **S3 folders** from admin dashboard.
- Assign **S3 folder(s)** with `read`, `write`permissions to groups.
- Auto-creation of **IAM Role** and **IAM Policy** for group.
- Add users to created group(s).

### 👥 User

- On login, user receives **temporary AWS credentials** scoped to the IAM role attached to their group(s).
- S3 access is limited to folders/groups assigned to the user.

### 📂 File Access Control

- **Read**: View folder, download files.
- **Write**: Upload files, delete files, create folders.
- IAM policy dynamically reflects this configuration on a per-group basis.

---

## 🗂 Folder Structure

```
.
├── app/
│   └── (DashboardLayout)/
│       └── components/
│           └── s3fileManager/  # S3 & IAM logic
├── components/
│   └── shared/                 # Reusable UI components
├── store/                      # Redux slices
├── utils/                      # Helper functions
├── pages/                      # API Routes & Auth
├── public/
├── styles/
├── .env.example
└── README.md
```

---

## 🌐 Below docs have one Time Setup on AWS

**link**: [https://docs.google.com/document/d/1XGGBUuqvYc7MU7X3idwCKeNqMyRThWZdp6239CIb6fc/edit?tab=t.0]

## 🌐 Environment Variables

Create a `.env` file in the project root using the following template:

```env
PORT=3000

NEXT_PUBLIC_COGNITO_REGION=us-east-1
NEXT_PUBLIC_USER_POOL_ID=us-east-1_XXXXXXX
NEXT_PUBLIC_USER_POOL_CLIENT_ID=XXXXXXXXXXXXXXXXXX
NEXT_PUBLIC_IDENTITY_POOL_ID=us-east-1:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

NEXT_PUBLIC_OAUTH_DOMAIN=your-cognito-domain.auth.us-east-1.amazoncognito.com
NEXT_PUBLIC_OAUTH_SCOPES=openid,email,profile
NEXT_PUBLIC_OAUTH_RESPONSE_TYPE=code

NEXT_PUBLIC_OAUTH_REDIRECT_SIGNIN=https://your-app.com/
NEXT_PUBLIC_OAUTH_REDIRECT_SIGNOUT=https://your-app.com/

NEXT_PUBLIC_S3_REGION=us-east-1
NEXT_PUBLIC_S3_BUCKET=securefileapp-users-xxxxxxxxxxxx

AWS_ACCESS_KEY=xxxxxxxxxxxxxxxx
AWS_SECRET_KEY=xxxxxxxxxxxxxxxx

NEXT_PUBLIC_ACCOUNT_ID=your-aws-account-id
```

---

## 📦 Installation & Development

```bash
# 1. Clone the repository
git clone https://github.com/waqasajaz/aws-workspace-access
cd secure-file-manager

Node.js Version  **v22.14.0**
# 2. Install dependencies
npm install

# 3. Create .env from .env.example
# put your own credential
cp .env.example .env

# 4. Start development server
npm run dev
```

# Deployment Guide for EC2

## Overview

This guide will help you deploy a Node.js application to an EC2 instance using PM2 for process management. Follow the steps below to successfully deploy your app.

---

## Prerequisites

Before you begin, ensure the following are in place:

1. **EC2 Instance**: Your EC2 instance should be set up and running.
2. **PM2 Installed**: Make sure PM2 is installed on your EC2 instance to manage the application process. If it is not installed, run the following
3. **SSH Access**: Ensure you have SSH access to the EC2 instance.
4. **Node Version Manager (NVM)**: The deployment script uses NVM to manage Node.js versions, so make sure it is available.

---

### Example Setup Information

You will need to replace the following placeholders with your actual data:

- **EC2 Public IP**: `0.000.000.000` (Replace with your EC2 instance's public IP address)
- **EC2 Username**: `ubuntu` (Commonly the default for EC2, change if necessary)
- **Remote Directory Name**: `your_directory_name` (The directory where your app will be deployed on the EC2 instance)

---

## Steps to Deploy

### 1. Navigate to your root directory like

`cd secure-file-manager`

### 2. Prepare the Deployment Script

Make it like eg `deploy_dev.sh` script executable:

```bash
chmod +x deploy_dev.sh
```

Then run the deployment script:

```bash
./deploy_dev.sh
```

### 3. Deployment Process

During deployment, the script will:

- Install the correct Node.js version using `nvm`.
- Install the required dependencies using `npm`.
- Sync the necessary files to the remote EC2 instance (excluding unnecessary directories like `node_modules`, `.git`, etc.).
- Set up the environment and build the application on the remote server.
- Restart the application using PM2.

---

## `deploy_dev.sh` Script

Here’s the script (`deploy_dev.sh`) that will automate the deployment process:

```bash
#!/bin/bash

# Load NVM (Node Version Manager)
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"                   # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # This loads nvm bash_completion

# Replace these with your actual values
_front_end_url="https://yourRepo.com"         # Your front-end repository URL
_remote="0.000.000.000"                       # Your EC2 instance public IP address
_user="ubuntu"                                # Your EC2 instance username (usually 'ubuntu')
_remote_directory_name="your_directory_name"  # The directory on your EC2 instance where the app will be deployed

echo "❗❗❗ Name here❗❗❗"
echo "Local system name: $HOSTNAME"
echo "Local date and time: $(date)"

# Use the correct Node.js version (e.g., 22)
nvm use 22
npm install

# Sync the local files to the EC2 instance (excluding unnecessary directories)
rsync -rtu --delete --progress "./" --exclude="node_modules" --exclude="tmp" --exclude=".git" --exclude="build" --exclude=".next" $_user@$_remote:/home/ubuntu/$_remote_directory_name

echo
echo "** Running deployment on remote host: $_remote **"
echo

# Connect to the remote EC2 instance and run the deployment commands
ssh -T "$_user"@"$_remote" "bash -s $_remote_directory_name" <<'EOL'
    # Load NVM (Node Version Manager) on remote server
    export NVM_DIR="$HOME/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
    [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
    export HOME="/home/ubuntu/"

    # Go to the correct directory where your app is deployed
    cd /home/ubuntu/$_remote_directory_name

    # Install npm dependencies on remote server
    npm install

    # Run the build process
    rm .env
    cp .env.dev .env
    npm run build

    # Restart the app using PM2
    pm2 stop $_remote_directory_name --silent
    pm2 delete $_remote_directory_name --silent
    pm2 start npm --name $_remote_directory_name -- run start --time
    pm2 save

    echo "Deployment completed successfully at: $(date)"
EOL
```

---

### Explanation of the Script

- **Set up the environment**: The script loads `nvm` to use the correct Node.js version and installs dependencies locally.
- **File Syncing**: It uses `rsync` to sync the project files to the remote EC2 instance, excluding unnecessary files like `node_modules`, `.git`, etc.
- **Remote Commands**: On the remote EC2 instance, the script installs dependencies, runs the build process, and restarts the app using PM2 to apply the changes.

---

## Final Steps

Once the script completes, your application should be live and running on your EC2 instance under PM2’s process management. You can verify the deployment by checking the application on your EC2 instance's public IP or domain.

---

## 🧪 Example Scenario

> You want to allow users in **TeamAlpha** to only access files in the `team-alpha/` folder:

1. Admin **creates a Cognito group**: `TeamAlpha`
2. Admin **creates a new folder** in S3: `team-alpha/`
3. Assigns the folder to the group with permission: `read` or `write`
4. Behind the scenes:
   - IAM Role created and linked to Cognito group
   - IAM Policy attached with folder access
   - Admin **creates user** and adds to `TeamAlpha`
5. On login, the user receives temporary S3 access scoped to `team-alpha/`.

---

## 📌 Permissions Breakdown

| Permission | Capabilities                                                 |
| ---------- | ------------------------------------------------------------ |
| `read`     | View folder, read files                                      |
| `write`    | All read capabilities + Upload, Create, Delete files/folders |

IAM policies are dynamically generated and follow AWS best practices for least privilege.

---

## 📊 Sample Stack Preview

| Component     | Sample                                                    |
| ------------- | --------------------------------------------------------- |
| Cognito Group | TeamAlpha                                                 |
| S3 Folder     | `team-alpha/`                                             |
| Permission    | `write`                                                   |
| IAM Role      | Auto-created and attached                                 |
| Result        | Users in TeamAlpha can upload/delete inside `team-alpha/` |


---
