# 💬 HelloBuddy — Real-Time Chat Application

> A modern, full-stack real-time chat application built with React (Vite), Spring Boot, STOMP WebSockets, and MongoDB.

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Tech Stack](#tech-stack)
- [Prerequisites & Installation](#prerequisites--installation)
- [Project Structure](#project-structure)
- [Environment Variables](#environment-variables)
- [Running the App](#running-the-app)
- [Feature Roadmap](#feature-roadmap)
- [Database Schema](#database-schema)
- [API Endpoints](#api-endpoints)
- [WebSocket Events (STOMP)](#websocket-events-stomp)
- [Development Notes](#development-notes)

---

## 🌟 Project Overview

**HelloBuddy** is a real-time chat application that allows users to:

- Register and log in securely (JWT-based auth)
- Chat one-on-one with other users in real time
- Create and manage group chats
- Share images and files
- See online/offline presence of friends
- Get typing indicators and read receipts

---

## 🧱 Tech Stack

### 🖥️ Frontend (`/client`)

| Technology       | Version  | Purpose                                       |
| ---------------- | -------- | --------------------------------------------- |
| React            | ^18.x    | UI framework                                  |
| Vite             | ^5.x     | Build tool & dev server                       |
| TypeScript       | ^5.x     | Type safety                                   |
| React Router v6  | ^6.x     | Client-side routing                           |
| Zustand          | ^4.x     | Lightweight global state management           |
| STOMP.js         | ^7.x     | WebSocket client over STOMP protocol          |
| SockJS-client    | ^1.x     | WebSocket fallback transport                  |
| Axios            | ^1.x     | HTTP requests to REST API                     |
| Framer Motion    | ^11.x    | Smooth UI animations                          |
| React Hot Toast  | ^2.x     | Toast notifications                           |
| CSS Modules      | built-in | Component-scoped styling                      |

### ⚙️ Backend (`/server`) — Spring Boot

| Technology                    | Version | Purpose                                   |
| ----------------------------- | ------- | ----------------------------------------- |
| Java                          | 21 LTS  | Programming language                      |
| Spring Boot                   | 3.3.x   | Application framework                     |
| Spring Web (MVC)              | —       | REST API controllers                      |
| Spring WebSocket + STOMP      | —       | Real-time bidirectional messaging         |
| Spring Security               | —       | Authentication & authorization            |
| Spring Data MongoDB           | —       | MongoDB repository layer                  |
| Spring Data Redis             | —       | Redis caching & presence management       |
| jjwt (io.jsonwebtoken)        | 0.12.x  | JWT token generation & validation         |
| Cloudinary Java SDK           | 1.x     | Cloud media storage (avatars, files)      |
| Lombok                        | —       | Boilerplate reduction (@Getter, @Builder) |
| MapStruct                     | 1.5.x   | DTO ↔ Entity mapping                      |
| Maven                         | 3.9.x   | Build & dependency management             |
| Spring Boot DevTools          | —       | Hot reload in development                 |

### 🗄️ Database & Cache

| Technology   | Purpose                                      |
| ------------ | -------------------------------------------- |
| MongoDB      | Primary NoSQL database (documents)           |
| Spring Data MongoDB | Repository abstraction over MongoDB   |
| Redis        | Online presence caching & session store      |

### 🚀 DevOps & Tooling

| Tool           | Purpose                                  |
| -------------- | ---------------------------------------- |
| Maven Wrapper  | Run Maven without global install         |
| pnpm           | Fast frontend package manager            |
| ESLint         | Frontend code linting                    |
| Prettier       | Frontend code formatting                 |
| IntelliJ IDEA / VS Code | IDE options                   |
| Git            | Version control                          |
| GitHub         | Remote repository                        |
| Postman        | API testing                              |

---

## ⚙️ Prerequisites & Installation

Make sure all of the following are installed **before** running the project.

---

### 1️⃣ Install Node.js (Required — for Frontend)

Node.js powers the React frontend build tools (Vite).

- Download from: **https://nodejs.org**
- Choose the **LTS version** (v20.x or v22.x recommended)
- Run the installer, keep all defaults (especially **"Add to PATH"**)
- **Restart your terminal** after installation

Verify:
```bash
node --version    # v20.x.x or higher
npm --version     # 10.x.x or higher
```

---

### 2️⃣ Install pnpm (Frontend Package Manager)

```bash
npm install -g pnpm
```

Verify:
```bash
pnpm --version    # 9.x.x or higher
```

---

### 3️⃣ Install Java 21 LTS (Required — for Backend)

Spring Boot 3.x requires **Java 17 or higher**. Java 21 LTS is recommended.

- Download from: **https://adoptium.net** (Eclipse Temurin — recommended)
  - Select: **Version 21**, Package: **JDK**, OS: **Windows**, Arch: **x64**
- Run the installer, enable **"Set JAVA_HOME variable"** option
- **Restart your terminal** after installation

Verify:
```bash
java --version    # openjdk 21.x.x
```

---

### 4️⃣ Install Maven (Backend Build Tool)

> ⚠️ You can skip this if you use the **Maven Wrapper** (`mvnw`) included in the project — it downloads Maven automatically.

Optional global install:
- Download from: **https://maven.apache.org/download.cgi**
- Extract and add `bin/` to your system `PATH`

Verify:
```bash
mvn --version    # Apache Maven 3.9.x
```

---

### 5️⃣ Install Git (Required)

- Download from: **https://git-scm.com/downloads**
- Run the installer with all defaults

Verify:
```bash
git --version    # git version 2.x.x
```

---

### 6️⃣ Set Up MongoDB (Required — Choose One Option)

**Option A — MongoDB Atlas (Cloud, Recommended):**
1. Go to **https://www.mongodb.com/atlas**
2. Create a free account → Create a free **M0 cluster**
3. Click **Connect** → **Connect your application** → copy the URI
4. Whitelist your IP address in Network Access

**Option B — MongoDB Locally:**
- Download from: **https://www.mongodb.com/try/download/community**
- Install and run MongoDB as a Windows Service
- Default URI: `mongodb://localhost:27017/hellobuddy`

---

### 7️⃣ Set Up Redis (Required — for Presence / Session)

**Option A — Redis Cloud (Free Tier):**
1. Go to **https://redis.io/try-free/**
2. Create a free account → get host, port, and password

**Option B — Docker (Recommended on Windows):**
```bash
# Requires Docker Desktop installed (https://www.docker.com/products/docker-desktop)
docker run -d -p 6379:6379 redis:alpine
```

Default: `redis://localhost:6379`

---

### 8️⃣ Set Up Cloudinary (For Media Uploads)

1. Go to **https://cloudinary.com** → create a free account
2. From your dashboard, copy:
   - **Cloud Name**
   - **API Key**
   - **API Secret**
3. Add these to `server/src/main/resources/application.properties`

---

### 9️⃣ IDE & Extensions

**For VS Code (Frontend + viewing backend):**

| Extension             | ID                                  |
| --------------------- | ----------------------------------- |
| ESLint                | `dbaeumer.vscode-eslint`            |
| Prettier              | `esbenp.prettier-vscode`            |
| MongoDB for VS Code   | `mongodb.mongodb-vscode`            |
| Thunder Client        | `rangav.vscode-thunder-client`      |
| GitLens               | `eamodio.gitlens`                   |
| Extension Pack for Java | `vscjava.vscode-java-pack`        |
| Spring Boot Extension Pack | `vmware.vscode-boot-dev-pack`  |

**For IntelliJ IDEA (Best for Spring Boot):**
- Download Community or Ultimate from: **https://www.jetbrains.com/idea/**
- Plugins: **Lombok**, **Spring Boot**, **MongoDB Plugin**

---

## 📁 Project Structure

```
HelloBuddyChatApp/
│
├── client/                              # React + Vite Frontend
│   ├── public/
│   │   └── favicon.ico
│   ├── src/
│   │   ├── assets/                      # Static assets (icons, images)
│   │   ├── components/                  # Reusable UI components
│   │   │   ├── ui/                      # Button, Input, Avatar, Modal
│   │   │   ├── chat/                    # MessageBubble, ChatInput, ConversationList
│   │   │   └── layout/                  # Sidebar, Header, MainLayout
│   │   ├── pages/                       # Route-level pages
│   │   │   ├── LoginPage.tsx
│   │   │   ├── RegisterPage.tsx
│   │   │   ├── ChatPage.tsx
│   │   │   └── ProfilePage.tsx
│   │   ├── hooks/                       # Custom React hooks
│   │   │   ├── useAuth.ts
│   │   │   ├── useStompClient.ts        # STOMP WebSocket hook
│   │   │   └── useMessages.ts
│   │   ├── store/                       # Zustand global state
│   │   │   ├── authStore.ts
│   │   │   ├── chatStore.ts
│   │   │   └── uiStore.ts
│   │   ├── services/                    # Axios API calls
│   │   │   ├── api.ts                   # Axios instance (base URL + interceptors)
│   │   │   ├── authService.ts
│   │   │   └── messageService.ts
│   │   ├── websocket/                   # STOMP WebSocket setup
│   │   │   └── stompClient.ts
│   │   ├── types/                       # TypeScript interfaces & types
│   │   │   └── index.ts
│   │   ├── utils/                       # Helper functions
│   │   │   └── formatDate.ts
│   │   ├── styles/                      # Global CSS
│   │   │   ├── globals.css
│   │   │   └── variables.css
│   │   ├── App.tsx
│   │   └── main.tsx
│   ├── index.html
│   ├── vite.config.ts
│   ├── tsconfig.json
│   └── package.json
│
├── server/                              # Spring Boot Backend
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/hellobuddy/
│   │   │   │   ├── HelloBuddyApplication.java   # Entry point
│   │   │   │   ├── config/
│   │   │   │   │   ├── SecurityConfig.java       # Spring Security + JWT
│   │   │   │   │   ├── WebSocketConfig.java      # STOMP WebSocket config
│   │   │   │   │   ├── RedisConfig.java          # Redis connection
│   │   │   │   │   └── CloudinaryConfig.java     # Cloudinary setup
│   │   │   │   ├── controller/
│   │   │   │   │   ├── AuthController.java
│   │   │   │   │   ├── UserController.java
│   │   │   │   │   ├── MessageController.java
│   │   │   │   │   └── ConversationController.java
│   │   │   │   ├── websocket/
│   │   │   │   │   └── ChatWebSocketController.java  # @MessageMapping handlers
│   │   │   │   ├── service/
│   │   │   │   │   ├── AuthService.java
│   │   │   │   │   ├── UserService.java
│   │   │   │   │   ├── MessageService.java
│   │   │   │   │   ├── ConversationService.java
│   │   │   │   │   └── PresenceService.java          # Redis-backed presence
│   │   │   │   ├── repository/
│   │   │   │   │   ├── UserRepository.java
│   │   │   │   │   ├── MessageRepository.java
│   │   │   │   │   └── ConversationRepository.java
│   │   │   │   ├── model/                            # MongoDB documents
│   │   │   │   │   ├── User.java
│   │   │   │   │   ├── Message.java
│   │   │   │   │   └── Conversation.java
│   │   │   │   ├── dto/                              # Data Transfer Objects
│   │   │   │   │   ├── request/
│   │   │   │   │   │   ├── LoginRequest.java
│   │   │   │   │   │   ├── RegisterRequest.java
│   │   │   │   │   │   └── SendMessageRequest.java
│   │   │   │   │   └── response/
│   │   │   │   │       ├── AuthResponse.java
│   │   │   │   │       ├── UserResponse.java
│   │   │   │   │       └── MessageResponse.java
│   │   │   │   ├── security/
│   │   │   │   │   ├── JwtUtil.java                  # JWT generation & parsing
│   │   │   │   │   ├── JwtAuthFilter.java            # JWT request filter
│   │   │   │   │   └── UserDetailsServiceImpl.java
│   │   │   │   └── exception/
│   │   │   │       ├── GlobalExceptionHandler.java
│   │   │   │       └── ResourceNotFoundException.java
│   │   │   └── resources/
│   │   │       ├── application.properties            # Main config
│   │   │       └── application-dev.properties        # Dev overrides
│   │   └── test/
│   │       └── java/com/hellobuddy/
│   ├── pom.xml                                       # Maven dependencies
│   └── mvnw / mvnw.cmd                               # Maven wrapper scripts
│
├── PROJECT.md                           # ← You are here
└── README.md
```

---

## 🔐 Environment Variables

### Backend (`server/src/main/resources/application.properties`)

```properties
# Server
server.port=8080
spring.application.name=hellobuddy

# MongoDB
spring.data.mongodb.uri=mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/hellobuddy

# Redis
spring.data.redis.host=localhost
spring.data.redis.port=6379
# spring.data.redis.password=your_redis_password  # if using Redis Cloud

# JWT
app.jwt.secret=your_super_long_secret_key_at_least_256_bits
app.jwt.expiration-ms=900000
app.jwt.refresh-expiration-ms=604800000

# Cloudinary
cloudinary.cloud-name=your_cloud_name
cloudinary.api-key=your_api_key
cloudinary.api-secret=your_api_secret

# CORS — allow frontend
app.cors.allowed-origins=http://localhost:5173

# File upload limits
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=15MB
```

### Frontend (`client/.env`)

```env
VITE_API_URL=http://localhost:8080/api
VITE_WS_URL=http://localhost:8080/ws
```

---

## ▶️ Running the App

### Backend (Spring Boot)

```bash
# Navigate to server directory
cd server

# Option A — Using Maven Wrapper (no Maven install needed)
./mvnw spring-boot:run          # macOS/Linux
mvnw.cmd spring-boot:run        # Windows

# Option B — Using global Maven
mvn spring-boot:run
```

Backend runs at: **http://localhost:8080**
Health check: **http://localhost:8080/api/health**
WebSocket endpoint: **ws://localhost:8080/ws**

---

### Frontend (React + Vite)

```bash
# Navigate to client directory
cd client

# Install dependencies (first time only)
pnpm install

# Start dev server
pnpm dev
```

Frontend runs at: **http://localhost:5173**

---

## 🗺️ Feature Roadmap

### ✅ Phase 1 — Foundation & Auth (Week 1)
- [ ] Spring Boot project setup with Maven
- [ ] MongoDB + Spring Data MongoDB connection
- [ ] User registration & login (REST API)
- [ ] JWT auth (access + refresh tokens) with Spring Security
- [ ] React frontend scaffolding (Vite + TypeScript)
- [ ] Login / Register pages
- [ ] Protected routes on frontend
- [ ] Basic responsive UI shell (Sidebar + Chat area)

### 💬 Phase 2 — Real-Time Messaging (Week 2)
- [ ] Spring WebSocket + STOMP server setup
- [ ] STOMP client integration in React (STOMP.js + SockJS)
- [ ] One-on-one real-time messaging
- [ ] Message persistence in MongoDB
- [ ] Conversation list with last message preview
- [ ] Message timestamps & read receipts
- [ ] Online/offline presence via Redis

### 👥 Phase 3 — Groups & Media (Week 3)
- [ ] Group chat creation & management
- [ ] Add/remove group participants
- [ ] Image & file sharing (Multipart upload + Cloudinary)
- [ ] Typing indicators via STOMP
- [ ] Emoji picker
- [ ] Unread message badge counts

### 🔔 Phase 4 — Polish & Advanced (Week 4)
- [ ] Push notifications (Web Push API)
- [ ] Message search (MongoDB text index)
- [ ] User profile editing (avatar upload to Cloudinary)
- [ ] Message reactions
- [ ] Dark / Light theme toggle
- [ ] Full mobile-responsive UI polish

---

## 🗄️ Database Schema (MongoDB Documents)

### User (`users` collection)
```json
{
  "_id":          "ObjectId",
  "username":     "String (unique)",
  "email":        "String (unique)",
  "passwordHash": "String (bcrypt)",
  "avatar":       "String (Cloudinary URL)",
  "bio":          "String",
  "isOnline":     "Boolean",
  "lastSeen":     "ISODate",
  "createdAt":    "ISODate",
  "updatedAt":    "ISODate"
}
```

### Conversation (`conversations` collection)
```json
{
  "_id":          "ObjectId",
  "participants": ["ObjectId → User"],
  "isGroupChat":  "Boolean",
  "groupName":    "String",
  "groupAvatar":  "String (URL)",
  "admins":       ["ObjectId → User"],
  "lastMessage": {
    "text":       "String",
    "senderId":   "ObjectId",
    "timestamp":  "ISODate"
  },
  "createdAt":    "ISODate"
}
```

### Message (`messages` collection)
```json
{
  "_id":            "ObjectId",
  "conversationId": "ObjectId → Conversation",
  "senderId":       "ObjectId → User",
  "text":           "String",
  "mediaUrl":       "String (URL)",
  "messageType":    "Enum: TEXT | IMAGE | FILE",
  "readBy":         ["ObjectId → User"],
  "deliveredTo":    ["ObjectId → User"],
  "createdAt":      "ISODate"
}
```

---

## 🌐 API Endpoints

### Auth
| Method | Endpoint                    | Description              | Auth Required |
| ------ | --------------------------- | ------------------------ | ------------- |
| POST   | `/api/auth/register`        | Register new user        | ❌            |
| POST   | `/api/auth/login`           | Login user               | ❌            |
| POST   | `/api/auth/logout`          | Logout user              | ✅            |
| POST   | `/api/auth/refresh-token`   | Refresh access token     | ❌            |

### Users
| Method | Endpoint               | Description                | Auth Required |
| ------ | ---------------------- | -------------------------- | ------------- |
| GET    | `/api/users/me`        | Get current user profile   | ✅            |
| PUT    | `/api/users/me`        | Update profile             | ✅            |
| GET    | `/api/users/search`    | Search users by username   | ✅            |
| POST   | `/api/users/avatar`    | Upload profile picture     | ✅            |

### Conversations
| Method | Endpoint                        | Description                | Auth Required |
| ------ | ------------------------------- | -------------------------- | ------------- |
| GET    | `/api/conversations`            | Get all conversations      | ✅            |
| POST   | `/api/conversations`            | Create DM conversation     | ✅            |
| GET    | `/api/conversations/{id}`       | Get single conversation    | ✅            |
| POST   | `/api/conversations/group`      | Create group chat          | ✅            |
| PUT    | `/api/conversations/{id}/group` | Update group info          | ✅            |

### Messages
| Method | Endpoint                                  | Description              | Auth Required |
| ------ | ----------------------------------------- | ------------------------ | ------------- |
| GET    | `/api/messages/{conversationId}`          | Get messages in a chat   | ✅            |
| POST   | `/api/messages/{conversationId}`          | Send a text message      | ✅            |
| POST   | `/api/messages/{conversationId}/media`    | Send a media message     | ✅            |
| DELETE | `/api/messages/{id}`                      | Delete a message         | ✅            |
| PUT    | `/api/messages/{conversationId}/read`     | Mark messages as read    | ✅            |

---

## 🔌 WebSocket Events (STOMP)

Spring Boot uses **STOMP over WebSocket** instead of Socket.IO.

### WebSocket Connection

```typescript
// Frontend connects to:
const socket = new SockJS('http://localhost:8080/ws');
const stompClient = Stomp.over(socket);
stompClient.connect({ Authorization: `Bearer ${token}` }, onConnected);
```

### Client → Server (Send / Publish)

| Destination                     | Payload                                    | Description             |
| ------------------------------- | ------------------------------------------ | ----------------------- |
| `/app/chat.sendMessage`         | `{ conversationId, text, messageType }`    | Send a new message      |
| `/app/chat.typing`              | `{ conversationId, isTyping }`             | Typing indicator        |
| `/app/chat.markRead`            | `{ conversationId }`                       | Mark messages as read   |

### Server → Client (Subscribe / Listen)

| Topic / Queue                              | Description                     |
| ------------------------------------------ | ------------------------------- |
| `/topic/conversation/{conversationId}`     | Receive messages in a chat      |
| `/topic/presence`                          | User online/offline broadcasts  |
| `/user/queue/notifications`                | Personal notifications (user-specific) |
| `/user/queue/errors`                       | Error messages (user-specific)  |

---

## 🔐 Auth Flow

```
Client                          Spring Boot Server                MongoDB
  |                                     |                            |
  |-- POST /api/auth/register -------->  |                            |
  |                                     |-- Save hashed password --> |
  |<-- { accessToken, refreshToken } --  |                            |
  |                                     |                            |
  |-- POST /api/auth/login ----------->  |                            |
  |                                     |-- Verify credentials ----> |
  |<-- { accessToken, refreshToken } --  |                            |
  |                                     |                            |
  |-- GET /api/users/me                  |                            |
  |   Authorization: Bearer <token> -->  |                            |
  |                                     |-- JwtAuthFilter validates  |
  |<-- { user profile } --------------  |                            |
```

---

## 👨‍💻 Development Notes

- Never commit `application.properties` with real secrets to Git — use `application-dev.properties` and add to `.gitignore`
- Backend runs on port **8080**, Frontend on port **5173**
- STOMP WebSocket endpoint is at `/ws` — configured in `WebSocketConfig.java`
- Message broker prefix is `/topic` (broadcast) and `/user` (personal)
- Application prefix for `@MessageMapping` is `/app`
- JWT tokens are passed in the STOMP `CONNECT` frame headers for WebSocket auth
- Lombok `@Data`, `@Builder`, `@NoArgsConstructor` are used on all model/DTO classes
- All MongoDB `@Document` classes use `@Id` on the `String id` field

---

*Built with ❤️ — HelloBuddy Chat App | Spring Boot + React*
