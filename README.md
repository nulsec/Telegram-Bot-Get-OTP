# 🤖 Telegram Bot Get OTP From Target

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-2.0%2B-green)](https://flask.palletsprojects.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A high-performance, production-ready Telegram bot for automated voice OTP (One-Time Password) verification calls using VoIP Voice API and Azure Text-to-Speech.

## ✨ Features

### 🎯 Core Functionality
- **Multi-Type OTP Calls**: Standard OTP, CVV, PIN, SSN, Account Number verification
- **Extended Mode (XCALL)**: Collects multiple data points (OTP + CVV + PIN + DOB + SSN)
- **Custom Call Scripts**: User-defined voice scripts with variable templating
- **Smart Call Handling**: 
  - Answering Machine Detection (AMD)
  - Human vs. Machine detection
  - Auto-retry on wrong code entry
  - Configurable digit length (4-12 digits)

### 🎤 Voice Technology
- **Azure TTS Integration**: High-quality, natural-sounding voices
- **Multi-Voice Support**: 15+ female voice options (Christine, Bella, Natalia, etc.)
- **Real-Time Audio Generation**: Parallel processing for faster call setup
- **Call Recording**: Automatic recording and delivery to Telegram

### 📞 Phone Management
- **Random Phone Selection**: Rotate between multiple caller IDs
- **VoIP Integration**: Buy, release, and sync phone numbers via API
- **Custom Phone Numbers**: Per-user caller ID configuration

### 👥 User Management
- **Flexible Membership System**:
  - Time-based (Hours, Days, Weeks, Months, Years, Permanent)
  - Credit-based (Pay-per-use)
  - Hybrid mode (Time-based with credit fallback)
- **Voucher System**: Generate and distribute access codes
- **Admin Controls**: Broadcast messages, private messaging, member management

### 🔐 Security & Credits
- **Hybrid Credit System**:
  - Permanent members: Unlimited calls
  - Time-based members: Free during active period, credit-based after expiry
  - Credit-based members: Pay-per-call pricing
- **Credit Tracking**: Automatic deduction based on call outcome
- **Admin Authorization**: Multi-admin support with role-based access

### 🚀 Performance Optimization
- **Multi-Threading**: Concurrent audio generation (32-64 workers)
- **Connection Pooling**: Persistent HTTP connections
- **Caching**: TTL-based caching for frequent queries
- **Load Balancing Ready**: Health check & metrics endpoints

##  Usage

### User Commands
```
/start          - Initialize bot and get welcome message
/help           - Display available commands
/status         - Check membership status and credits
/call           - Standard OTP verification call
/xcall          - Extended OTP call (multiple data points)
/zcall          - Alternative OTP call flow
/cvv            - Credit card CVV verification
/pin            - PIN number verification
/ssn            - Social Security Number verification
/customcall     - Create custom call with user-defined scripts
```

### Admin Commands
```
/adminhelp      - Display all admin commands
/broadcast      - Send message to all members (button-based)
/sendmsg        - Send private message to specific user
/addvoucher     - Generate access vouchers
/delvoucher     - Delete voucher
/listvoucher    - List all vouchers
/addmember      - Add new member manually
/delmember      - Remove member
/listmember     - View all members
/addcredit      - Add credits to member account
/setcredit      - Set member credit balance
/checkcredit    - Check member credits
/addphone       - Add phone number manually
/delphone       - Remove phone number
/listphone      - View phone inventory
/buyphone       - Purchase numbers from VoIP provider
/releasephone   - Release numbers back to provider
/syncphone      - Sync phone numbers with provider
```

### Call Flow Example

**Standard OTP Call (`/call`):**
```
User: /call +18081234567 John Amazon 6
Bot: Processing audio...
Bot: Audio files ready!
Bot: Calling target...
[Call connects]
Bot: "Hello John, this is Christine calling on behalf of Amazon team..."
[Target presses 1]
Bot: "Please enter the 6-digit security code..."
[Target enters OTP]
Bot: ✅ OTP Captured: 123456
Bot: [Sends recording]
```

---

**⭐ Star this repo if you find it useful!**

Made with ❤️ by the development team
