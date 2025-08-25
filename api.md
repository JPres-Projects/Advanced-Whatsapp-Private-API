# WhatsApp Private Number API - Complete API Documentation

## 🔗 Complete API Endpoints with Full CURL Examples

### 🔹 App / Session Management

📖 For full instructions, check the [`README.md`](./README.md).  
It explains how to **deploy this API** and how to **expose it via Cloudflare**,  
so you can use it from any server or instance under your control!


#### Login with QR Code
```bash
# Display QR code in terminal or browser
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/app/login
```

#### Login with Pairing Code
```bash
# Generate 8-digit pairing code for phone login
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/app/login-with-code

# Enter the code in WhatsApp > Linked Devices > Link with Phone Number
```

#### Logout Session
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/app/logout
```

#### Reconnect Session
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/app/reconnect
```

#### Get Linked Devices
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/app/devices
```

### 🔹 User Information

#### Get User Info
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/user/info?phone=4917612345678"

# Alternative with JID
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/user/info?jid=4917612345678@s.whatsapp.net"
```

#### Get Profile Picture
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/user/avatar?phone=4917612345678"

# With preview option
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/user/avatar?phone=4917612345678&is_preview=true"
```

#### Check if Number has WhatsApp
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/user/check?phone=4917612345678"
```

#### Get Business Profile
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/user/business-profile?phone=4917612345678"
```

#### Get My Contacts
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/user/my/contacts
```

#### Get My Groups
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/user/my/groups
```

#### Get My Privacy Settings
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/user/my/privacy
```

### 🔹 Sending Messages (All Options)

#### Send Text Message
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/message \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678",
    "message": "Hello from API!",
    "reply_message_id": "false_120363xxx@g.us_xxx",  # Optional: Reply to specific message
    "view_once": false  # Optional: View once message
  }'
```

#### Send Image
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/image \
  -H "Content-Type: multipart/form-data" \
  -F "phone=4917612345678" \
  -F "caption=Check this image!" \
  -F "view_once=true" \
  -F "compress=false" \
  -F "reply_message_id=false_120363xxx@g.us_xxx" \
  -F "image=@/path/to/image.jpg"

# Alternative with base64
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/image \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678",
    "caption": "Amazing picture!",
    "image": "data:image/jpeg;base64,/9j/4AAQSkZJRg...",
    "view_once": true,
    "compress": false
  }'
```

#### Send Audio/Voice Message
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/audio \
  -H "Content-Type: multipart/form-data" \
  -F "phone=4917612345678" \
  -F "audio=@/path/to/audio.mp3" \
  -F "ptt=true"  # Optional: true for voice note, false for audio file
```

#### Send Video
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/video \
  -H "Content-Type: multipart/form-data" \
  -F "phone=4917612345678" \
  -F "caption=Watch this video!" \
  -F "view_once=true" \
  -F "compress=true" \
  -F "video=@/path/to/video.mp4"
```

#### Send Document
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/document \
  -H "Content-Type: multipart/form-data" \
  -F "phone=4917612345678" \
  -F "document=@/path/to/document.pdf" \
  -F "filename=Report_2024.pdf"  # Optional: Custom filename
```

#### Send Location
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/location \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678",
    "latitude": 52.520008,
    "longitude": 13.404954,
    "name": "Brandenburg Gate",  # Optional
    "address": "Pariser Platz, 10117 Berlin",  # Optional
    "url": "https://maps.google.com/?q=52.520008,13.404954"  # Optional
  }'
```

#### Send Contact Card
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/contact \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678",
    "contact_name": "John Doe",
    "contact_phone": "4917698765432",
    "contact_email": "john@example.com",  # Optional
    "contact_org": "Acme Corp"  # Optional
  }'
```

#### Send Link with Preview
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/link \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678",
    "link": "https://github.com/aldinokemal/go-whatsapp-web-multidevice",
    "caption": "Check out this amazing WhatsApp API!"  # Optional
  }'
```

#### Send Poll
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/poll \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678",
    "question": "What is your favorite programming language?",
    "options": ["Python", "JavaScript", "Go", "Java", "Other"],
    "selectable_count": 1  # 0 for multiple choice, 1 for single choice
  }'
```

#### Send Sticker
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/sticker \
  -H "Content-Type: multipart/form-data" \
  -F "phone=4917612345678" \
  -F "sticker=@/path/to/sticker.webp"

# With base64
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/sticker \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678",
    "sticker": "data:image/webp;base64,UklGRg..."
  }'
```

### 🔹 Message Management

#### Forward Message
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/message/forward \
  -H "Content-Type: application/json" \
  -d '{
    "message_id": "false_120363xxx@g.us_3EB0xxx",
    "phone": "4917612345678"
  }'
```

#### Revoke Message (Delete for Everyone)
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/message/false_120363xxx@g.us_3EB0xxx/revoke \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678"
  }'
```

#### Delete Message (Local Only)
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/message/false_120363xxx@g.us_3EB0xxx/delete \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678"
  }'
```

#### React to Message
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/message/false_120363xxx@g.us_3EB0xxx/reaction \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678",
    "emoji": "👍"  # Any emoji or empty string to remove
  }'
```

#### Edit Message
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/message/false_120363xxx@g.us_3EB0xxx/update \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678",
    "message": "This is the edited message text"
  }'
```

#### Mark as Read
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/message/false_120363xxx@g.us_3EB0xxx/read \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678"
  }'
```

### 🔹 Chat Management

#### Get All Chats
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/chats
```

#### Get Chat Messages
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/chat/4917612345678@s.whatsapp.net/messages?limit=50"

# For groups
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/chat/120363xxx@g.us/messages?limit=100"
```

#### Archive/Unarchive Chat
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/chat/archive \
  -H "Content-Type: application/json" \
  -d '{
    "chat_jid": "4917612345678@s.whatsapp.net",
    "is_archive": true  # true to archive, false to unarchive
  }'
```

#### Mute/Unmute Chat
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/chat/mute \
  -H "Content-Type: application/json" \
  -d '{
    "chat_jid": "4917612345678@s.whatsapp.net",
    "is_mute": true  # true to mute, false to unmute
  }'
```

#### Pin/Unpin Chat
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/chat/pin \
  -H "Content-Type: application/json" \
  -d '{
    "chat_jid": "4917612345678@s.whatsapp.net",
    "is_pin": true  # true to pin, false to unpin
  }'
```

### 🔹 Presence & Status

#### Set Online/Offline Status
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/presence \
  -H "Content-Type: application/json" \
  -d '{
    "presence": "available"  # Options: available, unavailable
  }'
```

#### Set Typing Status
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/chat-presence \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678",
    "presence": "composing",  # Options: composing, paused
    "media": "text"  # Options: text, audio
  }'
```

### 🔹 Group Management (Complete)

#### Create Group
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Project Team 2024",
    "participants": ["4917612345678", "4917698765432", "4917687654321"]
  }'
```

#### Join Group with Invite Link
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/join-with-link \
  -H "Content-Type: application/json" \
  -d '{
    "invite_link": "https://chat.whatsapp.com/ABC123DEF456"
  }'
```

#### Get Group Info
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/group/info?group_id=120363xxx@g.us"
```

#### Leave Group
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/leave \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363xxx@g.us"
  }'
```

#### Add Participants
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/participants/add \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363xxx@g.us",
    "participants": ["4917612345678", "4917698765432"]
  }'
```

#### Remove Participants
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/participants/remove \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363xxx@g.us",
    "participants": ["4917612345678"]
  }'
```

#### Promote to Admin
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/participants/promote \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363xxx@g.us",
    "participants": ["4917612345678"]
  }'
```

#### Demote from Admin
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/participants/demote \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363xxx@g.us",
    "participants": ["4917612345678"]
  }'
```

#### Set Group Photo
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/photo \
  -H "Content-Type: multipart/form-data" \
  -F "group_id=120363xxx@g.us" \
  -F "image=@/path/to/group-photo.jpg"
```

#### Change Group Name
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/name \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363xxx@g.us",
    "name": "New Group Name 2024"
  }'
```

#### Change Group Description
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/description \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363xxx@g.us",
    "description": "This is our project team for Q4 2024 initiatives"
  }'
```

#### Toggle Announcement Mode
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/announce \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363xxx@g.us",
    "is_announce": true  # true = only admins can send, false = everyone can send
  }'
```

#### Lock/Unlock Group Settings
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/locked \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363xxx@g.us",
    "is_locked": true  # true = only admins can change info, false = everyone can
  }'
```

#### Get Group Invite Link
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/group/invite-link?group_id=120363xxx@g.us"
```

#### Revoke Group Invite Link
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/invite-link/revoke \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363xxx@g.us"
  }'
```

## 💡 Realistic Usage Example: Complete Workflow

### Step 1: Get All Chats
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/chats
```

**Example Response:**
```json
{
  "status": 200,
  "message": "Success",
  "data": [
    {
      "jid": "4917612345678@s.whatsapp.net",
      "name": "John Smith",
      "last_message": "Hey, how's it going?",
      "last_message_time": "2024-01-15T10:30:00Z",
      "unread_count": 2
    },
    {
      "jid": "120363028461234567@g.us",
      "name": "Team Project 2024",
      "last_message": "Meeting tomorrow at 10",
      "last_message_time": "2024-01-15T09:15:00Z",
      "unread_count": 5
    },
    {
      "jid": "4917698765432@s.whatsapp.net",
      "name": "Sarah Johnson",
      "last_message": "Document received, thanks!",
      "last_message_time": "2024-01-14T18:45:00Z",
      "unread_count": 0
    }
  ]
}
```

### Step 2: Get Messages from Specific Chat
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/chat/4917612345678@s.whatsapp.net/messages?limit=10"
```

**Example Response:**
```json
{
  "status": 200,
  "data": {
    "messages": [
      {
        "id": "false_4917612345678@s.whatsapp.net_3EB0C767",
        "message": "Hey, how's it going?",
        "from": "4917612345678@s.whatsapp.net",
        "timestamp": "2024-01-15T10:30:00Z",
        "type": "text"
      },
      {
        "id": "false_4917612345678@s.whatsapp.net_3EB0C766",
        "message": "Did you finish the presentation?",
        "from": "4917612345678@s.whatsapp.net",
        "timestamp": "2024-01-15T10:29:00Z",
        "type": "text"
      }
    ]
  }
}
```

### Step 3: Reply to Message
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/message \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678",
    "message": "Hi John! Doing great, thanks! The presentation is almost ready, will send it to you shortly.",
    "reply_message_id": "false_4917612345678@s.whatsapp.net_3EB0C767"
  }'
```

### Step 4: Send Document
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/document \
  -H "Content-Type: multipart/form-data" \
  -F "phone=4917612345678" \
  -F "document=@/home/user/documents/presentation_q4_2024.pdf" \
  -F "filename=Q4_Presentation_Final.pdf"
```

### Step 5: Send to Group with Image
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/image \
  -H "Content-Type: multipart/form-data" \
  -F "phone=120363028461234567@g.us" \
  -F "caption=📊 Q4 Results are in! Great work team! 🎉" \
  -F "image=@/home/user/images/q4_results_chart.png"
```

### Step 6: Create Poll in Group
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/poll \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "120363028461234567@g.us",
    "question": "When should we schedule the next team meeting?",
    "options": [
      "Monday 10:00 AM",
      "Monday 2:00 PM",
      "Tuesday 10:00 AM",
      "Tuesday 2:00 PM",
      "Wednesday 10:00 AM"
    ],
    "selectable_count": 0
  }'
```

### Step 7: Set Typing Status and Send Message
```bash
# Show typing indicator
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/chat-presence \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678",
    "presence": "composing",
    "media": "text"
  }'

# Wait 2 seconds (simulating typing)
sleep 2

# Send the message
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/message \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678",
    "message": "Just reviewed the files. Everything looks perfect! 👍"
  }'

# Stop typing indicator
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/chat-presence \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "4917612345678",
    "presence": "paused",
    "media": "text"
  }'
```

### Step 8: Forward Important Message to Multiple Recipients
```bash
# Get message ID from previous chat
MESSAGE_ID="false_120363028461234567@g.us_3EB0C999"

# Forward to first recipient
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/message/forward \
  -H "Content-Type: application/json" \
  -d '{
    "message_id": "'$MESSAGE_ID'",
    "phone": "4917698765432"
  }'

# Forward to second recipient
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/message/forward \
  -H "Content-Type: application/json" \
  -d '{
    "message_id": "'$MESSAGE_ID'",
    "phone": "4917687654321"
  }'
```

## 🔧 N8N Integration Example

```javascript
// N8N Function Node - Send WhatsApp Message
const phone = $node["Webhook"].json["phone"];
const message = $node["Webhook"].json["message"];

const response = await $http.request({
  method: 'POST',
  url: 'https://whatsapp-privateapi-1-503.yourdomain.com/send/message',
  body: {
    phone: phone,
    message: message
  },
  headers: {
    'Content-Type': 'application/json'
  }
});

return response;
```

## 📝 Important Notes

- **Phone Format**: Always use international format without + (e.g., 4917612345678)
- **Group JID Format**: Groups use format 120363xxx@g.us
- **Personal JID Format**: Personal chats use format phone@s.whatsapp.net
- **Message IDs**: Format is usually false_JID_UNIQUEID
- **File Uploads**: Use multipart/form-data for file uploads or base64 for JSON
- **View Once**: Available for images and videos only
- **Message Editing**: Only possible within 15 minutes of sending
- **Reactions**: Empty string removes existing reaction

