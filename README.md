### 🛒 Automated WooCommerce OTP & Fraud Prevention System

![n8n](https://img.shields.io/badge/n8n-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white)
![WooCommerce](https://img.shields.io/badge/WooCommerce-96588A?style=for-the-badge&logo=woocommerce&logoColor=white)
![WhatsApp](https://img.shields.io/badge/WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)

*An intelligent, n8n-powered backend automation workflow designed to intercept new WooCommerce orders, conduct third-party fraud checks, and verify user identities via WhatsApp and SMS OTPs to eliminate fake Cash-On-Delivery (COD) orders.*

---

## 🚀 Key Features

- **🚀 Seamless Order Interception:** - Automatically triggers workflows the moment a new WooCommerce order is placed.
  
- **🛡️ Smart Fraud Check:** - Integrates with third-party APIs to evaluate the risk score of new orders before processing. Orders failing the check are flagged for manual review.

- **📱 Multi-Channel OTP Dispatch:** - Validates user identity by sending automated order confirmations via **WhatsApp Cloud API** and sending One-Time Passwords (OTPs) through a **Bulk SMS Gateway**.

- **📊 Custom CRM Integration:** - Logs verified OTPs and analytics directly to a custom database/CRM, allowing for real-time tracking of verified leads and prevention stats.

---

## 🛠️ System Architecture

```mermaid
graph TD
    %% Main Order Interception Flow
    Start([🚀 WooCommerce: New Order]) --> Branch1
    Start --> Branch2

    %% Branch 1: Fraud Check & WhatsApp
    subgraph Fraud & WhatsApp Pipeline
        Branch1[Get Customer Details] --> FC[Fraud Check API]
        FC --> MR[Merge Fraud Result]
        MR --> Cond1{Fraud Score OK?}
        
        Cond1 -- True --> GWM[Generate WhatsApp Message]
        GWM --> WAPI((WhatsApp API))
        
        Cond1 -- False --> FOR[Flag Order for Review]
        FOR --> UWO[Update WC Order to On-Hold]
    end

    %% Branch 2: SMS OTP & CRM Pipeline
    subgraph SMS OTP & CRM Pipeline
        Branch2[Data Enrichment & Validation] --> VPN[Verify Phone Number]
        VPN --> PSN[Prepare SMS Notification]
        PSN --> SG((SMS Gateway))
        SG --> SDB[(Save OTP to DB)]
        SDB --> CRMA[CRM Update & Analytics]
    end

    %% OTP Verification Flow
    subgraph OTP Verification Webhook
        OVW([🌐 OTP Verification Webhook]) --> VOL[Validate OTP Logic]
        VOL --> Cond2{OTP Verified?}
        
        Cond2 -- True --> COW[Confirm Order in WooCommerce]
        Cond2 -- False --> CUO[Cancel Unverified Order]
    end

    %% Styling
    style Start fill:#ff6600,stroke:#fff,stroke-width:2px,color:#fff
    style OVW fill:#00cc66,stroke:#fff,stroke-width:2px,color:#fff
    style WAPI fill:#25D366,stroke:#fff,stroke-width:2px,color:#fff
    style SG fill:#007bff,stroke:#fff,stroke-width:2px,color:#fff
    style SDB fill:#f0ad4e,stroke:#fff,stroke-width:2px,color:#fff
```
## ⚙️ Setup & Installation

### 1. Prerequisites
Before you begin, ensure you have:
- Self-hosted **n8n** instance (or Cloud version).
- A running **WooCommerce** store on WordPress.
- Access to **WhatsApp Cloud API**.
- Account with a **Bulk SMS Gateway** provider.
- Database/CRM for storing OTP logs and analytics.
- API Key for a third-party **Fraud Checking Service**.

### 2. Import Workflows
1. Download the workflow JSON files from this repository.
2. Open your n8n dashboard.
3. Click **"Import from File"** and select the JSON files.

> **⚠️ Important:** The workflow files have been sanitized. All sensitive API keys, database credentials, and webhook URLs have been replaced with placeholders.

### 3. Configuration Steps

#### Step A: Configure Credentials
Create the following credentials in n8n and select them in their respective nodes:

| Service | Credential Name in n8n | Purpose |
| :--- | :--- | :--- |
| **WooCommerce** | `WooCommerce API` | To update order statuses (Confirmed/On-Hold/Cancelled). |
| **WhatsApp API** | `WhatsApp Cloud API` | To send automated WhatsApp order confirmations. |
| **SMS Gateway** | `Bulk SMS API` | To dispatch the OTP to the customer's phone. |
| **Fraud Check** | `FraudLabs API (Example)` | To evaluate the risk score of the new order. |
| **Database/CRM** | `Postgres DB (Example)` | To save OTPs and log analytics. |

#### Step B: Replace Placeholders
Open the workflow nodes and replace these placeholders with your actual data:

| Placeholder | Description |
| :--- | :--- |
| `YOUR_WOOCOMMERCE_URL` | The base URL of your WordPress/WooCommerce site. |
| `YOUR_WHATSAPP_PHONE_ID` | The Phone Number ID from your Meta developer console. |
| `YOUR_WHATSAPP_TOKEN` | The temporary or permanent access token for WhatsApp API. |
| `YOUR_SMS_GATEWAY_URL` | The endpoint URL provided by your SMS service. |
| `YOUR_FRAUD_API_KEY` | Your authentication key for the fraud checking service. |
| `YOUR_CRM_ENDPOINT` | The API endpoint to send analytics data to your CRM dashboard. |

---

## 📂 File Structure

```text
├── order_interception_workflow.json  # Workflow for triggering and dispatching OTP
├── otp_verification_workflow.json    # Workflow for handling frontend OTP submission
├── README.md                         # Documentation
└── assets/                           # Diagram images
```

##🤝 Contribution
Feel free to fork this repository and submit pull requests. For major changes, please open an issue first to discuss what you would like to change.

##📬 Connect with Me
<img src="https://img.shields.io/badge/Website-000000?style=for-the-badge&logo=Google-Chrome&logoColor=white" />
<img src="https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white" />
