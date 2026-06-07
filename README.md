<div align="center">

# ChitChat

**Real-time one-to-one messaging application**

Sub-second bidirectional messaging powered by Socket.io WebSockets, with file and image sharing via Cloudinary, JWT authentication, and persistent conversation history.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-chitchat--live-111827?style=flat-square&logo=render&logoColor=white)](https://chitchat-md87.onrender.com/)
&nbsp;
[![Stack](https://img.shields.io/badge/Stack-MERN%20%2B%20Socket.io%20%2B%20Cloudinary-20232a?style=flat-square)](https://github.com/Ritik2513/ChitChat)
&nbsp;
[![License](https://img.shields.io/badge/License-MIT-639922?style=flat-square)](LICENSE)

</div>

---

## What is ChitChat?

ChitChat is a full-stack real-time messaging app that lets users exchange text messages and media files instantly. Messages are delivered via persistent WebSocket connections — no polling, no page refreshes. Files and images are uploaded directly to Cloudinary, keeping the server stateless and media delivery fast via CDN.

---

## Features

### Messaging
- **Real-time delivery** — messages sent and received instantly over a persistent Socket.io WebSocket connection
- **One-to-one private conversations** — each conversation is scoped between two users
- **Persistent history** — all messages stored in MongoDB and loaded on conversation open
- **Message threading** — conversations are cleanly separated per contact

### Media sharing
- **File & image sharing** — send images and files directly in chat
- **Cloudinary integration** — media uploaded to Cloudinary, served via CDN for fast global delivery
- **No server storage** — the Node.js server stays stateless; binary files never touch the filesystem

### Auth & users
- **JWT authentication** — secure login with tokens stored in `httpOnly` cookies
- **User search** — find other registered users to start a conversation
- **Session persistence** — stay logged in across page refreshes

---

## Tech stack

| Layer | Technology |
|-------|-----------|
| Frontend | React.js, Tailwind CSS, Context API |
| Backend | Node.js, Express.js |
| Real-time | Socket.io (WebSockets) |
| Database | MongoDB (Mongoose) |
| Media storage | Cloudinary |
| Auth | JWT, httpOnly cookies |
| Deployment | Render |

---

## How it works

```
User A types a message and hits send
         │
         ▼
React client emits socket event → socket.emit('sendMessage', { to, message })
         │
         ▼
Socket.io server receives event
         ├── 1. Persist message to MongoDB
         └── 2. Emit to recipient's socket → io.to(recipientSocketId).emit('newMessage', message)
                   │
                   ▼
         User B's client receives event in real time
         └── UI updates instantly — no polling, no refresh
```

### File / image sharing flow

```
User selects a file
         │
         ▼
Client uploads directly to Cloudinary (via signed upload preset)
         │
         ▼
Cloudinary returns a secure CDN URL
         │
         ▼
Client sends message with { type: 'media', url: cloudinaryUrl }
         │
         ▼
Socket.io delivers URL to recipient — recipient loads media from Cloudinary CDN
```

This keeps the Node.js server completely stateless — it never handles binary data.

---

## Getting started

### Prerequisites

- Node.js v18+
- MongoDB (local or Atlas)
- Cloudinary account (free tier works)

### 1. Clone the repository

```bash
git clone https://github.com/Ritik2513/ChitChat.git
cd ChitChat
```

### 2. Configure environment variables

Create a `.env` file in the root:

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret
JWT_EXPIRES_IN=7d

CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

CLIENT_URL=http://localhost:5173
```

### 3. Install dependencies

```bash
# Install all dependencies (root)
npm install

# Install client dependencies
cd client && npm install
```

### 4. Run the application

```bash
# Terminal 1 — start the backend server
npm run dev

# Terminal 2 — start the React client
cd client && npm run dev
```

Client runs at `http://localhost:5173` · API + Socket server at `http://localhost:5000`

---

## Project structure

```
ChitChat/
├── client/                    # React frontend
│   └── src/
│       ├── components/        # ChatWindow, MessageBubble, Sidebar, etc.
│       ├── context/           # Auth context, Socket context
│       ├── pages/             # Login, Register, Home
│       └── utils/             # Axios instance, helpers
│
├── controllers/               # Route handler logic
├── middleware/                 # Auth (JWT verify), error handler
├── models/                    # Mongoose schemas (User, Message, Conversation)
├── routes/                    # Express route definitions
├── socket/                    # Socket.io server setup + event handlers
├── utils/                     # Cloudinary config, helpers
└── index.js                   # App entry point
```

---

## Data models

### User
```
_id, username, email, passwordHash, profilePic (Cloudinary URL), createdAt
```

### Conversation
```
_id, participants [ref: User], lastMessage (ref: Message), updatedAt
```

### Message
```
_id, conversationId (ref: Conversation), senderId (ref: User),
type (text | image | file), body, mediaUrl (Cloudinary URL),
createdAt
```

---

## Demo

Try it live at **[chitchat-md87.onrender.com](https://chitchat-md87.onrender.com/)**

> **Note:** The server is hosted on Render's free tier and may take 30–50 seconds to wake up on first load. Once warm, real-time messaging is instant.

You can register two accounts in separate browser tabs to test the real-time messaging and file sharing.

---

## Roadmap

- [ ] Online presence indicators (active/away status)
- [ ] Typing indicators (`user is typing...`)
- [ ] Message read receipts
- [ ] Group chat support
- [ ] Push notifications
- [ ] Message search
- [ ] TypeScript migration

---

## Author

**Ritik Kumar Gupta** — Full Stack Engineer

[Portfolio](https://ritik2513.vercel.app) · [LinkedIn](https://www.linkedin.com/in/ritik-gupta-a69253229/) · [GitHub](https://github.com/Ritik2513) · [ritikgupta2513@gmail.com](mailto:ritikgupta2513@gmail.com)
