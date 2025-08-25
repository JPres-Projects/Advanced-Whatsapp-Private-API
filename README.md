# WhatsApp Private Number API

A self-hosted REST API that connects to your personal WhatsApp account, allowing you to send and receive messages programmatically from your own phone number. This solution uses the powerful [aldinokemal/go-whatsapp-web-multidevice](https://github.com/aldinokemal/go-whatsapp-web-multidevice) implementation with Docker containerization and Cloudflare Tunnel integration for worldwide accessibility.

## 🎯 Purpose & Use Cases

**Transform your private WhatsApp number into a programmable API:**
- **N8N Automation**: Monitor incoming WhatsApp messages and send automated responses from your private number
- **Workflow Integration**: Connect email notifications, CRM updates, or any workflow to send WhatsApp messages  
- **Server Monitoring**: Get alerts and status updates directly on your personal WhatsApp
- **Custom Notifications**: Send automated messages from your own number for home automation, reminders, or alerts
- **Script Integration**: Any application that can make HTTP requests can send WhatsApp messages from your private number
- **Multi-Device Support**: Run multiple WhatsApp numbers simultaneously on different endpoints

**Global Access**: Cloudflare Tunnel enables secure worldwide API access while maintaining control through IP-based or email-based access policies.

**Session Persistence**: WhatsApp connections remain active as long as your phone number is used within 14 days. Sessions are automatically restored after server restarts.

## ⚠️ Important Warning

This project uses an unofficial method to communicate with WhatsApp. **This is against the WhatsApp Terms of Service.** There is a real risk that your phone number could be temporarily or permanently banned by WhatsApp.

**Use at your own risk. Recommended to use a secondary, non-critical phone number for development.**

## ✨ Features

- **Comprehensive REST API**: 87+ endpoints covering all WhatsApp functionality
- **Multi-Number Support**: Run multiple WhatsApp accounts simultaneously
- **Media Support**: Send/receive images, audio, videos, documents, stickers
- **Advanced Messaging**: Reactions, message editing, polls, location sharing
- **Group Management**: Create groups, manage participants, admin controls
- **Contact Management**: Access contacts, business profiles, user info
- **Self-Hosted**: Runs entirely on your infrastructure with Docker
- **Global Access**: Cloudflare Tunnel integration for worldwide secure access
- **Session Persistence**: Automatic reconnection with persistent storage
- **24/7 Operation**: Runs as system services with auto-restart
- **Easy Updates**: Automated update script preserving all sessions

## 🚀 Quick Start

### Prerequisites

- Ubuntu 20.04+ server (Oracle Cloud, DigitalOcean, AWS, etc.)
- Docker and Docker Compose installed
- Cloudflare account with domain
- Internet connection
- WhatsApp account(s)

### Installation

```bash
# Create project directory
mkdir -p /home/ubuntu/whatsapp-privateapi
cd /home/ubuntu/whatsapp-privateapi

# Install Docker if not present
sudo apt update
sudo apt install docker.io docker-compose -y
sudo usermod -aG docker ubuntu

# Download latest WhatsApp API image
docker pull ghcr.io/aldinokemal/go-whatsapp-web-multidevice:latest
```

### Configuration

Create Docker Compose configurations for each WhatsApp number:

**For first number (+491633644503):**
```yaml
# docker-compose-1-503.yml
version: '3.8'
services:
  whatsapp-privateapi-1-503:
    image: ghcr.io/aldinokemal/go-whatsapp-web-multidevice:latest
    ports:
      - "3010:3000"
    volumes:
      - ./data-1-503:/app/storages  # Persistent session data
    restart: unless-stopped
    container_name: whatsapp-privateapi-1-503
```

**For second number:**
```yaml
# docker-compose-2-808.yml
version: '3.8'
services:
  whatsapp-privateapi-2-808:
    image: ghcr.io/aldinokemal/go-whatsapp-web-multidevice:latest
    ports:
      - "3011:3000"
    volumes:
      - ./data-2-808:/app/storages  # Persistent session data
    restart: unless-stopped
    container_name: whatsapp-privateapi-2-808
```

### Start APIs

```bash
# Start first API
docker-compose -f docker-compose-1-503.yml up -d

# Start second API
docker-compose -f docker-compose-2-808.yml up -d

# Verify containers are running
docker ps
```



## ☁️ Cloudflare Tunnel Setup (Global Access)

Cloudflare Tunnel enables secure worldwide access to your WhatsApp APIs through custom domains while maintaining access control via IP or email policies.

### Prerequisites
- Cloudflare account with domain
- `cloudflared` installed on Windows (for tunnel management)

### Tunnel Creation

**On Windows:**
```powershell
# Download cloudflared
Invoke-WebRequest https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-windows-amd64.exe -OutFile cloudflared.exe

# Authenticate with Cloudflare
.\cloudflared.exe tunnel login

# Create tunnels for each API
.\cloudflared.exe tunnel create whatsapp-privateapi-1-503
.\cloudflared.exe tunnel create whatsapp-privateapi-2-808

# Set DNS records
.\cloudflared.exe tunnel route dns whatsapp-privateapi-1-503 whatsapp-privateapi-1-503.yourdomain.com
.\cloudflared.exe tunnel route dns whatsapp-privateapi-2-808 whatsapp-privateapi-2-808.yourdomain.com
```

### Install cloudflared on Ubuntu

```bash
# Download and install cloudflared
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb

# Verify installation
cloudflared version
```

### Server Configuration

**Create tunnel configurations:**
```bash
# API 1-503 configuration
sudo nano /etc/cloudflared/privateapi-1-503-config.yml
```

```yaml
tunnel: whatsapp-privateapi-1-503
credentials-file: /home/ubuntu/.cloudflared/TUNNEL-ID-1.json

ingress:
  - hostname: whatsapp-privateapi-1-503.yourdomain.com
    service: http://127.0.0.1:3010
  - service: http_status:404
```

**Create systemd services:**
```bash
# Service for API 1-503
sudo tee /etc/systemd/system/cloudflared-1-503.service << 'EOF'
[Unit]
Description=Cloudflared 1-503 Tunnel
After=network.target

[Service]
Type=simple
User=ubuntu
ExecStart=/usr/bin/cloudflared --config /etc/cloudflared/privateapi-1-503-config.yml tunnel run
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
EOF

# Enable and start service
sudo systemctl daemon-reload
sudo systemctl enable cloudflared-1-503
sudo systemctl start cloudflared-1-503
```

### Access Control

**Cloudflare Zero Trust Setup:**
1. Go to `https://one.dash.cloudflare.com`
2. Access → Applications → Add application
3. Configure application with your domains
4. Set policies:
   - **IP-based**: Allow specific server IPs
   - **Email-based**: Require email authentication
   - **Combined**: Multiple authentication methods

## 🔄 Automated Updates

Maintain the latest API version while preserving all WhatsApp sessions.

**Create update script:**
```bash
nano /home/ubuntu/whatsapp-privateapi/update-apis.sh
```

```bash
#!/bin/bash

echo "🔄 WhatsApp APIs Update started..."
cd /home/ubuntu/whatsapp-privateapi

# Verify Docker access
if ! docker ps > /dev/null 2>&1; then
    echo "❌ Docker access failed! Run script with sudo."
    exit 1
fi

echo "📦 Updating Docker images..."
docker pull ghcr.io/aldinokemal/go-whatsapp-web-multidevice:latest

echo "🔄 Restarting API 1-503 (sessions preserved)..."
docker-compose -f docker-compose-1-503.yml down
docker-compose -f docker-compose-1-503.yml up -d

echo "🔄 Restarting API 2-808 (sessions preserved)..."
docker-compose -f docker-compose-2-808.yml down
docker-compose -f docker-compose-2-808.yml up -d

echo "✅ Update completed!"
echo "📊 Container status:"
docker ps

echo "💾 Session data check:"
ls -la data-1-503/
ls -la data-2-808/
```

**Make executable and set up automated updates:**
```bash
# Make script executable
sudo chown root:root /home/ubuntu/whatsapp-privateapi/update-apis.sh
sudo chmod +x /home/ubuntu/whatsapp-privateapi/update-apis.sh

# Set up weekly automated updates (Sundays at 3 AM)
sudo crontab -e
# Add: 0 3 * * 0 /home/ubuntu/whatsapp-privateapi/update-apis.sh >> /home/ubuntu/whatsapp-privateapi/update.log 2>&1
```

## 🔧 Server Configuration

### Firewall Setup

```bash
# Open required ports
sudo iptables -I INPUT 6 -p tcp --dport 3010 -j ACCEPT
sudo iptables -I INPUT 6 -p tcp --dport 3011 -j ACCEPT
sudo iptables -I INPUT 6 -p tcp --dport 80 -j ACCEPT
sudo iptables -I INPUT 6 -p tcp --dport 443 -j ACCEPT
sudo netfilter-persistent save
```

### Oracle Cloud Setup

Configure Security Groups to allow:
- **Port 80**: HTTP (0.0.0.0/0)
- **Port 443**: HTTPS (0.0.0.0/0)  
- **Port 3010**: API 1-503 (optional if using Cloudflare)
- **Port 3011**: API 2-808 (optional if using Cloudflare)

📖 **For detailed Oracle Cloud setup and instance creation**: [Oracle Cloud General Setup Guide](https://github.com/JPresting/3---Setup-Guides/tree/main/Oracle-Cloud)

## 📱 Usage Examples

### Authentication
1. Visit your API endpoint: `https://whatsapp-privateapi-1-503.yourdomain.com`
2. Scan QR code with your WhatsApp mobile app
3. API becomes ready for use

### Send Message
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/message \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "1234567890",
    "message": "Hello from API!"
  }'
```

### Send Image
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/image \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "1234567890",
    "caption": "Check this image!",
    "image": "base64_encoded_image_data"
  }'
```

### Create Group
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My API Group",
    "participants": ["1234567890", "0987654321"]
  }'
```

## 🔍 Troubleshooting

### Container Issues
```bash
# Check container status
docker ps
docker logs whatsapp-privateapi-1-503

# Restart containers
docker-compose -f docker-compose-1-503.yml restart
```

### Tunnel Issues
```bash
# Check tunnel services
sudo systemctl status cloudflared-1-503
sudo systemctl status cloudflared-2-808

# Restart tunnel services
sudo systemctl restart cloudflared-1-503
```

### Session Issues
- **Session Lost**: Re-scan QR code at API endpoint
- **Connection Timeout**: Check if phone number was used within 14 days
- **API Not Responding**: Verify container and tunnel status

## 📋 System Requirements

- **OS**: Ubuntu 20.04+ (ARM64 or AMD64)
- **RAM**: 1GB minimum, 2GB recommended
- **Storage**: 2GB available space per API instance
- **Network**: Stable internet connection
- **Docker**: Latest version with Docker Compose

## 🛠️ Service Management

```bash
# Container management
docker ps                                    # List running containers
docker logs whatsapp-privateapi-1-503       # View container logs
docker-compose -f docker-compose-1-503.yml restart  # Restart API

# Tunnel management  
sudo systemctl status cloudflared-1-503     # Check tunnel status
sudo systemctl restart cloudflared-1-503    # Restart tunnel
sudo journalctl -u cloudflared-1-503 -f     # View tunnel logs

# Update APIs
sudo /home/ubuntu/whatsapp-privateapi/update-apis.sh
```

## 🔗 API Endpoints

### 🔹 App / Session
- `GET /app/login` → Display QR code for login
- `GET /app/login-with-code` → Generate pairing code for login
- `POST /app/logout` → Logout session
- `POST /app/reconnect` → Rebuild connection
- `GET /app/devices` → List linked devices

### 🔹 User Management
- `GET /user/info?jid={phone@s.whatsapp.net}` → Get contact information
- `GET /user/avatar?jid={phone@s.whatsapp.net}` → Get profile picture
- `POST /user/pushname` → Set display name
- `GET /user/my/groups` → Get own groups
- `GET /user/my/newsletters` → Get own broadcast lists
- `GET /user/my/privacy` → Get privacy settings
- `GET /user/my/contacts` → Get all contacts
- `GET /user/check?phone=491234567890` → Check if number has WhatsApp
- `GET /user/business-profile?jid=...` → Get business profile info

### 🔹 Messaging
- `POST /send/message` → Send text message
- `POST /send/image` → Send image
- `POST /send/audio` → Send audio/voice message
- `POST /send/video` → Send video
- `POST /send/document` → Send document
- `POST /send/contact` → Send contact card
- `POST /send/link` → Send link with preview
- `POST /send/location` → Send location
- `POST /send/poll` → Send poll
- `POST /send/sticker` → Send sticker
- `POST /send/presence` → Set online/offline status
- `POST /send/chat-presence` → Set "typing..." status

### 🔹 Message Management
- `POST /message/{id}/revoke` → Revoke message (delete for everyone)
- `POST /message/{id}/delete` → Delete locally
- `POST /message/{id}/reaction` → Add emoji reaction
- `POST /message/{id}/update` → Edit message (within 15 min)
- `POST /message/{id}/read` → Mark as read
- `POST /message/{id}/star` → Star message
- `POST /message/{id}/unstar` → Remove star

### 🔹 Chats
- `GET /chats` → Get all chats
- `GET /chat/{chat_jid}/messages` → Get message history
- `POST /chat/{jid}/label` → Set chat label
- `POST /chat/{jid}/pin` → Pin chat

### 🔹 Group Management
- `POST /group` → Create group with participants
- `POST /group/join-with-link` → Join group via invite link
- `GET /group/info?jid=...` → Get group information
- `POST /group/leave` → Leave group
- `POST /group/participants/add` → Add participants
- `POST /group/participants/remove` → Remove participants
- `POST /group/participants/promote` → Promote to admin
- `POST /group/participants/demote` → Demote from admin
- `GET /group/participant-requests?jid=...` → List pending join requests
- `POST /group/participants/approve` → Approve join request
- `POST /group/participants/reject` → Reject join request
- `POST /group/photo` → Set group picture
- `POST /group/name` → Change group name
- `POST /group/locked` → Lock/unlock group
- `POST /group/announce` → Toggle announcement mode
- `POST /group/topic` → Set group topic
- `GET /group/invite-link?group_id=...` → Get group invite link

### 🔹 Newsletter
- `POST /newsletter/unfollow` → Unfollow broadcast list

## 📄 License

This project is provided as-is for educational purposes. Use at your own risk and in compliance with WhatsApp's Terms of Service.

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

**Ready to programmatically control your WhatsApp communications with enterprise-grade reliability!**