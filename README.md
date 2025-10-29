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

## 🌐 Below docs contain the information how can you setup on your aws account
🛠 Manual Setup

Follow this document to configure all required AWS resources manually.
Link: https://docs.google.com/document/d/1XGGBUuqvYc7MU7X3idwCKeNqMyRThWZdp6239CIb6fc/edit?tab=t.0

Note: If you are setting up manually, make sure to create an .env file in the root directories of both the frontend and backend projects.
This file should include all environment variables specified below

☁️ CloudFormation Setup

For automated deployment using AWS CloudFormation, follow the below document.
Link: https://docs.google.com/document/d/15ckx4LjorZbDKeRhwvYbJBxODQcRFRTqITt6oooSjl4/edit?tab=t.0#heading=h.3gedw96awg10


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

NEXT_PUBLIC_ACCOUNT_ID=your-aws-account-id
```

---

Create a `.env` file in the backend end project

```env
APP_KEY=your_app_key
NODE_ENV=production
HOST=0.0.0.0
TZ=UTC
LOG_LEVEL=info
AWS_REGION=your_aws_region
S3_BUCKET=your_bucket_name
COGNITO_USER_POOL_ID=your_cognito_user_pool_id
COGNITO_IDENTITY_POOL_ID=your_cognito_identity_pool_id
NEXT_PUBLIC_ACCOUNT_ID=your-aws-account-id
```

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

---

## 📧 Contact

For support, issues, or feature requests, please contact:

## **Team**: The Experts Cloud
