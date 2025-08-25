# WhatsApp Private Number API - COMPLETE API Documentation

## 🔗 Complete API Endpoints with Explanations

### 🔹 App / Session Management

#### Login with QR Code (Insert your subdomain.domain/path)
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/app/login
```
**What it does:** Shows QR code to scan with your WhatsApp mobile app to connect this API to your account

#### Login with Pairing Code  
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/app/login-with-code?phone=628912344551"
```
**What it does:** Generates an 8-digit code to type into WhatsApp instead of scanning QR (useful for remote setup)

#### Logout
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/app/logout
```
**What it does:** Disconnects the API from your WhatsApp account (like removing a linked device)

#### Reconnect
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/app/reconnect
```
**What it does:** Fixes connection issues without having to scan QR code again

#### Get Devices
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/app/devices
```
**What it does:** Lists all devices connected to your WhatsApp account (phones, computers, this API)

#### Get Connection Status
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/app/status
```
**What it does:** Checks if the API is currently connected to WhatsApp or disconnected

### 🔹 Send Messages

#### Send Text Message
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/message \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "6289685028129@s.whatsapp.net",
    "message": "Hello from API",
    "reply_message_id": "3EB089B9D6ADD58153C561"  # Optional: reply to specific message
  }'
```
**What it does:** Sends a regular text message, optionally as a reply to another message (shows "replied to" in chat)

#### Send Image with URL Support
```bash
# Upload image from your computer
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/image \
  -H "Content-Type: multipart/form-data" \
  -F "phone=6289685028129@s.whatsapp.net" \
  -F "caption=Check this image!" \
  -F "view_once=false" \
  -F "image=@/path/to/image.jpg" \
  -F "compress=false"

# Send image from internet URL
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/image \
  -H "Content-Type: multipart/form-data" \
  -F "phone=6289685028129@s.whatsapp.net" \
  -F "caption=Check this image!" \
  -F "image_url=https://example.com/image.jpg" \
  -F "view_once=true" \
  -F "compress=false"
```
**What it does:** Sends an image with optional caption. `view_once=true` makes it disappear after viewing (like Snapchat)

#### Send Video with URL Support
```bash
# Upload video from computer
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/video \
  -H "Content-Type: multipart/form-data" \
  -F "phone=6289685028129@s.whatsapp.net" \
  -F "caption=Check this video" \
  -F "video=@/path/to/video.mp4" \
  -F "view_once=false" \
  -F "compress=true"

# Send video from internet URL
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/video \
  -H "Content-Type: multipart/form-data" \
  -F "phone=6289685028129@s.whatsapp.net" \
  -F "caption=Check this video" \
  -F "video_url=https://example.com/video.mp4" \
  -F "view_once=false" \
  -F "compress=true"
```
**What it does:** Sends a video with caption. `compress=true` reduces file size for faster sending

#### Send Audio with URL Support
```bash
# Upload audio file
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/audio \
  -H "Content-Type: multipart/form-data" \
  -F "phone=6289685028129@s.whatsapp.net" \
  -F "audio=@/path/to/audio.mp3"

# Send audio from URL
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/audio \
  -H "Content-Type: multipart/form-data" \
  -F "phone=6289685028129@s.whatsapp.net" \
  -F "audio_url=https://example.com/audio.mp3"
```
**What it does:** Sends audio file that shows up with play button in chat

#### Send Document
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/document \
  -H "Content-Type: multipart/form-data" \
  -F "phone=6289685028129@s.whatsapp.net" \
  -F "document=@/path/to/document.pdf"
```
**What it does:** Sends any file (PDF, ZIP, etc.) as downloadable document

#### Send Location
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/location \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "6289685028129@s.whatsapp.net",
    "latitude": "37.4220041",
    "longitude": "-122.0862515"
  }'
```
**What it does:** Sends a map location pin that opens in Google Maps when clicked

#### Send Contact
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/contact \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "6289685028129@s.whatsapp.net",
    "contact_name": "John Doe",
    "contact_phone": "6289685028129"
  }'
```
**What it does:** Sends a contact card that can be saved directly to phone's contacts

#### Send Link with Preview
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/link \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "6289685028129@s.whatsapp.net",
    "link": "https://google.com",
    "caption": "Check this link"
  }'
```
**What it does:** Sends a link that shows preview thumbnail and description (like when you paste a link)

#### Send Poll  
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/poll \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "6289685028129@s.whatsapp.net",
    "question": "What is your favorite color?",
    "options": ["Red", "Blue", "Green", "Yellow"],
    "max_answer": 1
  }'
```
**What it does:** Creates a voting poll. `max_answer=1` means single choice, higher number allows multiple selections

#### Send Typing Indicator
```bash
# Show "typing..." status
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/chat-presence \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "6289685028129@s.whatsapp.net",
    "action": "start"
  }'

# Remove "typing..." status
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/chat-presence \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "6289685028129@s.whatsapp.net",
    "action": "stop"
  }'
```
**What it does:** Shows or hides "typing..." indicator under your name in their chat

### 🔹 Message Management

#### Delete Message for Me
```bash
curl -X POST "https://whatsapp-privateapi-1-503.yourdomain.com/message/3EB089B9D6ADD58153C561/delete" \
  -H "Content-Type: application/json" \
  -d '{"phone":"6289685024051@s.whatsapp.net"}'
```
**What it does:** Deletes message only from your chat view (others still see it)

#### Delete Message for Everyone
```bash
curl -X POST "https://whatsapp-privateapi-1-503.yourdomain.com/message/3EB089B9D6ADD58153C561/revoke" \
  -H "Content-Type: application/json" \
  -d '{"phone":"6289685024051@s.whatsapp.net"}'
```
**What it does:** Deletes message for all participants (shows "This message was deleted")

#### React to Message
```bash
curl -X POST "https://whatsapp-privateapi-1-503.yourdomain.com/message/3EB089B9D6ADD58153C561/reaction" \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "6289685024051@s.whatsapp.net",
    "emoji": "👍"
  }'
```
**What it does:** Adds emoji reaction to a message (like Facebook reactions)

#### Edit Message
```bash
curl -X POST "https://whatsapp-privateapi-1-503.yourdomain.com/message/3EB089B9D6ADD58153C561/update" \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "6289685024051@s.whatsapp.net", 
    "message": "Updated message text"
  }'
```
**What it does:** Edits already sent message (shows "edited" label, only works within 15 minutes)

#### Mark Message as Read
```bash
curl -X POST "https://whatsapp-privateapi-1-503.yourdomain.com/message/3EB089B9D6ADD58153C561/read" \
  -H "Content-Type: application/json" \
  -d '{"phone":"6289685024051@s.whatsapp.net"}'
```
**What it does:** Marks message as read (blue checkmarks appear for sender)

### 🔹 Chat Management

#### Get All Chats
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/chats
```
**What it does:** Lists all your WhatsApp conversations with last message and unread count

#### Get Chat History
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/chat/6289685028129@s.whatsapp.net/messages?limit=50"
```
**What it does:** Retrieves last 50 messages from a specific chat

#### Pin Chat
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/chat/pin \
  -H "Content-Type: application/json" \
  -d '{
    "chat_jid": "6289685028129@s.whatsapp.net",
    "is_pin": true
  }'
```
**What it does:** Pins chat to top of chat list (max 3 pinned chats in WhatsApp)

#### Unpin Chat
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/chat/pin \
  -H "Content-Type: application/json" \
  -d '{
    "chat_jid": "6289685028129@s.whatsapp.net",
    "is_pin": false
  }'
```
**What it does:** Unpins chat from top of list

### 🔹 User Information

#### Get Contact Info
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/user/info?phone=6289685028129"
```
**What it does:** Gets contact's WhatsApp profile info (name, status, last seen)

#### Get Profile Picture
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/user/avatar?phone=6289685028129&is_preview=false"
```
**What it does:** Downloads contact's profile picture

#### Get Business Profile
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/user/business-profile?phone=6289685028129"
```
**What it does:** Gets business info (hours, website, description) for WhatsApp Business accounts

#### Get Your Contacts
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/user/my/contacts
```
**What it does:** Lists all contacts from your phone that have WhatsApp

#### Get Your Groups
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/user/my/groups
```
**What it does:** Lists all WhatsApp groups you're a member of

#### Get Your Privacy Settings
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/user/my/privacy
```
**What it does:** Shows your privacy settings (who can see last seen, profile photo, status)

#### Check if Number Has WhatsApp
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/user/check?phone=6289685028129"
```
**What it does:** Checks if a phone number is registered on WhatsApp

### 🔹 Group Management

#### Create New Group
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group \
  -H "Content-Type: application/json" \
  -d '{
    "name": "New Group Name",
    "participants": ["6289685028129", "628123456789"]
  }'
```
**What it does:** Creates a new group with you as admin and adds specified members

#### Get Group Information
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/group/info?group_id=120363024258186537@g.us"
```
**What it does:** Gets group details (members, admins, description, creation date)

#### Get Group Info from Invite Link
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/group/info-from-link?invite_link=https://chat.whatsapp.com/ABC123DEF456"
```
**What it does:** Preview group details before joining (name, member count, description)

#### Join Group via Link
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/join-with-link \
  -H "Content-Type: application/json" \
  -d '{"invite_link": "https://chat.whatsapp.com/ABC123DEF456"}'
```
**What it does:** Joins a group using an invitation link

#### Leave Group
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/leave \
  -H "Content-Type: application/json" \
  -d '{"group_id": "120363024258186537@g.us"}'
```
**What it does:** Exits from a group (can rejoin if you have invite link)

#### Add Members to Group
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/participants/add \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363024258186537@g.us",
    "participants": ["6289685028129"]
  }'
```
**What it does:** Adds new members to group (you must be admin)

#### Remove Members from Group
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/participants/remove \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363024258186537@g.us",
    "participants": ["6289685028129"]
  }'
```
**What it does:** Kicks members out of group (you must be admin)

#### Make Someone Admin
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/participants/promote \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363024258186537@g.us",
    "participants": ["6289685028129"]
  }'
```
**What it does:** Gives admin powers to member (can add/remove people, change group info)

#### Remove Admin Powers
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/participants/demote \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363024258186537@g.us",
    "participants": ["6289685028129"]
  }'
```
**What it does:** Removes admin powers from member (becomes regular member)

#### Change Group Name
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/name \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363024258186537@g.us",
    "name": "New Group Name 2024"
  }'
```
**What it does:** Changes the group title that everyone sees

#### Set Group Description
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/topic \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363024258186537@g.us",
    "topic": "This is our new group topic/description"
  }'
```
**What it does:** Sets group description shown when people click group info

#### Change Group Photo
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/photo \
  -H "Content-Type: multipart/form-data" \
  -F "group_id=120363024258186537@g.us" \
  -F "image=@/path/to/group-photo.jpg"
```
**What it does:** Changes group profile picture

#### Enable Announcement Mode
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/announce \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363024258186537@g.us",
    "is_announce": true  # true = only admins can send, false = everyone can send
  }'
```
**What it does:** When enabled, only admins can send messages (others can only read)

#### Lock Group Settings
```bash
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/locked \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363024258186537@g.us",
    "is_locked": true  # true = only admins can change info, false = everyone can
  }'
```
**What it does:** When locked, only admins can change group name, photo, and description

#### Get Group Invite Link
```bash
curl -X GET "https://whatsapp-privateapi-1-503.yourdomain.com/group/invite-link?group_id=120363024258186537@g.us"
```
**What it does:** Gets the shareable link people can use to join the group

### 🔹 Configuration Options

#### Deploy at Custom Path
```bash
# If running behind nginx at https://yoursite.com/whatsapp/
APP_BASE_PATH=/whatsapp
```
**What it does:** Allows running the API at a subfolder instead of root domain

#### Auto-Read All Messages
```bash
# Automatically mark all incoming messages as read
WHATSAPP_AUTO_MARK_READ=true
```
**What it does:** Automatically sends read receipts (blue checkmarks) for all incoming messages

### 🔹 Webhook Events

#### Group Member Changes
```json
{
  "event": "group.participants",
  "payload": {
    "chat_id": "120363402106XXXXX@g.us",
    "type": "join",  // Someone joined the group
    "jids": ["6289685XXXXXX@s.whatsapp.net"]
  }
}
```
**What it does:** Notifies your webhook when people join, leave, or get promoted/demoted in groups

#### Message Delivery Status
```json
{
  "event": "message.ack",
  "payload": {
    "message_id": "3EB089B9D6ADD58153C561",
    "status": "delivered",  // or "read" when they open it
    "timestamp": "2025-08-03T10:30:00Z"
  }
}
```
**What it does:** Tells you when messages are delivered (gray checks) or read (blue checks)

#### Message Deleted
```json
{
  "event": "message.delete",
  "payload": {
    "message_id": "3EB089B9D6ADD58153C561",
    "chat_id": "6289685028129@s.whatsapp.net",
    "original_message": "Original message content",
    "timestamp": "2025-08-03T10:30:00Z"
  }
}
```
**What it does:** Notifies when someone deletes a message, includes what the message said

## 💡 Complete Real-World Example

### Step 1: Connect to WhatsApp
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/app/login
# Scan the QR code with WhatsApp on your phone
```

### Step 2: See All Your Chats
```bash
curl -X GET https://whatsapp-privateapi-1-503.yourdomain.com/chats
```

### Step 3: Send Message with Typing Effect
```bash
# Show you're typing
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/chat-presence \
  -H "Content-Type: application/json" \
  -d '{"phone": "6289685028129@s.whatsapp.net", "action": "start"}'

sleep 2  # Wait 2 seconds

# Send the message
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/message \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "6289685028129@s.whatsapp.net",
    "message": "Hi! Just checking in on the project status."
  }'

# Stop typing indicator
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/send/chat-presence \
  -H "Content-Type: application/json" \
  -d '{"phone": "6289685028129@s.whatsapp.net", "action": "stop"}'
```

### Step 4: Create and Setup a Group
```bash
# Create group
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Project Team Q1 2025",
    "participants": ["6289685028129", "628123456789"]
  }'

# Make it announcement only (only admins can post)
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/announce \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363999999999@g.us",
    "is_announce": true
  }'

# Lock settings so only admins can change group info
curl -X POST https://whatsapp-privateapi-1-503.yourdomain.com/group/locked \
  -H "Content-Type: application/json" \
  -d '{
    "group_id": "120363999999999@g.us",
    "is_locked": true
  }'
```

## 📝 Quick Reference

- **Phone Format**: Use international format without + (e.g., 6289685028129)
- **Personal Chat JID**: Add `@s.whatsapp.net` to phone number
- **Group JID**: Group IDs end with `@g.us`
- **Message IDs**: Long alphanumeric strings from message responses
- **View Once**: Messages/images that disappear after viewing
- **Announce Mode**: Only group admins can send messages
- **Locked Group**: Only admins can change group settings
- **Blue Checkmarks**: Message has been read
- **Gray Checkmarks**: Message delivered but not read

---

**Complete API documentation for go-whatsapp-web-multidevice**