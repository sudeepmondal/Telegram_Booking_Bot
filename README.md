# 🤖 Telegram AI Booking System

An AI-powered service booking chatbot built with n8n, Telegram, Groq AI (Free), and Google Sheets.

## 🔗 Live Demo
Talk to the bot: [@myshopbooking_bot](https://web.telegram.org/k/#@myshopbooking_bot)

## 🛠️ Tech Stack
| Tool | Purpose |
|------|---------|
| n8n | Workflow automation |
| Telegram Bot API | Customer chat interface |
| Groq AI (llama-3.3-70b) | AI conversation — 100% Free |
| Google Sheets | Product & booking database |
| Gmail | Confirmation emails |

## ✨ Features
- AI-powered natural conversation
- Auto product/service listing from Google Sheets
- Booking collection (Name, Email, Date, Time)
- Auto-save bookings to Google Sheets
- Automatic email confirmation after booking

## 📁 Project Structure
```
telegram-booking-bot/
├── workflow.json      # n8n workflow — import this
├── .env.example       # API keys template
└── README.md
```

## ⚙️ Setup Guide

### 1. Install n8n
```bash
npm install -g n8n
n8n start
```

### 2. Import Workflow
- Open `http://localhost:5678`
- Workflows → Add → ⋮ → Import from JSON
- Upload `workflow.json`

### 3. Get Free API Keys
- **Telegram Bot**: Message [@BotFather](https://t.me/botfather) → `/newbot`
- **Groq AI** (Free): [console.groq.com](https://console.groq.com) → API Keys
- **Google Sheets**: [console.cloud.google.com](https://console.cloud.google.com) → Enable Sheets API → OAuth credentials

### 4. Configure n8n Credentials
| Credential | Where to add |
|-----------|-------------|
| Telegram Bot Token | n8n → Credentials → Telegram API |
| Google Sheets OAuth | n8n → Credentials → Google Sheets OAuth2 |
| Gmail OAuth | n8n → Credentials → Gmail OAuth2 |
| Groq API Key | HTTP Request node → Authorization header |

### 5. Google Sheets Structure

**Sheet 1: Products**
| Product Name | Description | Price | Availability | Duration | Category |

**Sheet 2: Bookings** (auto-filled)
| Timestamp | Customer Name | Email | Service | Date | Time | Booking ID |

### 6. Run with ngrok (for Telegram webhook)
```bash
ngrok http 5678
```
Then set webhook:
```
https://api.telegram.org/bot{TOKEN}/setWebhook?url={NGROK_URL}/webhook/telegram-webhook-001
```

### 7. Activate
Toggle **Active** in n8n workflow editor ✅

## 🔄 Workflow Flow
```
Customer Message (Telegram)
    ↓
Extract Message Data
    ↓
Fetch Products (Google Sheets)
    ↓
Prepare AI Context
    ↓
Groq AI Response (llama-3.3-70b-versatile)
    ↓
Parse AI Response
    ├── Send Reply → Telegram
    └── If Booking Confirmed?
            ↓ true
        Save to Google Sheets
            ↓
        Send Confirmation Email (Gmail)
```

## 💬 Example Conversation
```
User:  Hello
Bot:   Hi! Welcome! Here are our services:
       1. Haircut - 500 BDT | 30 mins
       2. Facial - 1200 BDT | 60 mins

User:  I want to book a haircut
Bot:   Great! Please provide:
       - Full Name
       - Email Address  
       - Preferred Date (DD/MM/YYYY)
       - Preferred Time

User:  Deep, deep@gmail.com, 20/03/2025, 3 PM
Bot:   ✅ Booking Confirmed!
       📋 Booking ID: BK492841
       (Confirmation email sent!)
```

## 🔑 Environment Variables
```env
TELEGRAM_BOT_TOKEN=your_token
GROQ_API_KEY=your_groq_key  
GOOGLE_SHEET_ID=your_sheet_id
```
