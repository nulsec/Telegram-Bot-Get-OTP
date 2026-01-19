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

## 📋 Requirements

### System Requirements
- **OS**: Linux (Ubuntu 20.04+ recommended)
- **CPU**: 4-8 cores
- **RAM**: 16-32 GB
- **Python**: 3.8 or higher
- **Network**: Stable internet connection with public IP

### External Services
- VoIP Provider account (Voice API)
- [Azure Cognitive Services](https://azure.microsoft.com/en-us/services/cognitive-services/speech-services/) (Text-to-Speech)
- [Telegram Bot](https://t.me/BotFather) token
- SSL Certificate (Let's Encrypt recommended)



## 📱 Usage

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

## 🏗️ Architecture

### Tech Stack
- **Backend**: Flask (Python)
- **WSGI Server**: Gunicorn with gthread workers
- **Reverse Proxy**: Nginx with SSL termination
- **Database**: SQLite (can be upgraded to PostgreSQL)
- **Messaging**: Telegram Bot API (pyTelegramBotAPI)
- **Voice**: VoIP REST API
- **TTS**: Azure Cognitive Services Speech SDK
- **Caching**: cachetools (TTL Cache)

**Key Features:**
- Single worker (telebot requirement)
- Multi-threaded request handling
- Parallel audio generation
- Persistent thread pools
- 90-second keep-alive connections
- Exponential backoff retry logic

### Credit System

| Event | Credit Cost |
|-------|------------|
| Call Failed (No Connect) | 0 credits |
| Call Canceled | 0 credits |
| Line Busy | 0.5 credits |
| No Answer | 0.5 credits |
| VM/Machine Detected | 0.5 credits |
| Human + Press 1 | 1.0 credit |
| Valid OTP Entered | 2.5 credits (total) |

**Hybrid Mode Logic:**
1. Permanent membership → No deduction (unlimited)
2. Time-based active → No deduction (free)
3. Time-based expired → Deduct from credits
4. Credit-based → Always deduct from credits

## 📊 Monitoring

### Health Check
```bash
curl https://yourdomain.com/health
```

**Response:**
```json
{
  "status": "healthy",
  "timestamp": 1737329473,
  "components": {
    "database": "healthy",
    "workers": {
      "audio_workers": {"max": 64, "active": 12},
      "api_workers": {"max": 32, "active": 5}
    },
    "system": {
      "cpu_percent": 25.4,
      "memory_percent": 45.2,
      "memory_available_gb": 17.5
    }
  }
}
```

### Metrics
```bash
curl https://yourdomain.com/metrics
```

### Logs
```bash
# Gunicorn logs
tail -f logs/gunicorn.log

# Application logs
tail -f logs/app.log
```

## 🔒 Security Considerations

### ⚠️ Important Warnings
1. **Never commit `config.yaml`** to version control
2. **Use environment variables** for sensitive credentials
3. **Implement rate limiting** at Nginx level
4. **Enable HTTPS only** - No HTTP traffic
5. **Rotate API keys** regularly
6. **Monitor for abuse** - Check call logs frequently
7. **Comply with local laws** - OTP calls may be regulated

### Best Practices
- Use strong admin passwords
- Limit admin user IDs in config
- Enable fail2ban for SSH protection
- Regular security updates
- Database backups (daily recommended)
- Monitor VoIP provider usage and billing

## 🐛 Troubleshooting

### Bot Not Responding
```bash
# Check if bot is running
ps aux | grep gunicorn

# Check logs
tail -f logs/gunicorn.log

# Restart bot
pkill -f gunicorn && bash run.sh
```

### Webhook Issues
```bash
# Test webhook
curl -X POST https://yourdomain.com/ -H "Content-Type: application/json"

# Check Telegram webhook status
curl https://api.telegram.org/bot<TOKEN>/getWebhookInfo
```

### Audio Generation Fails
- Verify Azure Speech key is valid
- Check internet connectivity
- Ensure `gens/` directory has write permissions
- Monitor Azure quota limits

### Call Issues
- Verify VoIP provider balance
- Check phone number validity
- Review call logs in database
- Test with different phone numbers

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Open a Pull Request

## 📞 Support

For issues and questions:
- Open an [Issue](https://github.com/nulsec/Telegram-Bot-Get-OTP-From-Target/issues)
- Check [Documentation](https://github.com/nulsec/Telegram-Bot-Get-OTP-From-Target/wiki)

## ⚖️ Legal Disclaimer

This software is provided for **educational and authorized testing purposes only**. Users are solely responsible for:
- Obtaining proper consent before making calls
- Complying with local telecommunications laws
- Following TCPA, GDPR, and other regulations
- Using the software ethically and legally

Misuse of this software may result in legal consequences. The authors assume no liability for misuse.

## 🙏 Acknowledgments

- VoIP Provider for Voice API
- [Azure](https://azure.microsoft.com/) for Speech Services
- [Telegram](https://telegram.org/) for Bot API
- Community contributors and testers

---

**⭐ Star this repo if you find it useful!**

Made with ❤️ by the development team
