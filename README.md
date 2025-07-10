# 📨 WebSocket Messenger App

A lightweight WebSocket messenger built with a custom binary protocol, featuring:

- 🔗 Python server using `asyncio` and `websockets`
- 🌐 Browser-based JavaScript client with HTML/CSS
- 🆔 ID-based client association & buffering
- ⏳ Pull-based message retrieval
- ✅ Error handling & acknowledgements

---

## 📁 Project Structure

```
.
├── index.html          #  Optional styling
├── script.js        # Client interface & logic
├── style.css          # Optional styling
├── websocketserver.py       # WebSocket server in Python
└── README.md          # This file
```

---

## ⚙️ Prerequisites

- **Python 3.7+**
- Install dependencies:
  ```bash
  pip install websockets
  ```

---

## ▶️ Running the Server

```bash
python server.py
```

Listens on:
```
ws://localhost:12345
```
> 💡 For remote testing, use tools like ngrok and update the client’s WebSocket URL accordingly (e.g., `wss://<your-tunnel>.ngrok.io`).

---

## 🌐 Using the Client

1. Open `client.html` in a browser.
2. Enter a unique **Client ID** (0–255).
3. Click **Associate** to connect.
4. Use **Send** to send messages and **Get** to fetch buffered messages.

---

## 📡 Packet Protocol

### 1. MANAGEMENT (Type=0)
- `[0, 0, clientID]` → ASSOCIATE
- Server responds: `ASSOCIATIONSUCCESS`, `ASSOCIATIONFAILED`, or `UNKNOWNERROR`

### 2. CONTROL (Type=1)
- `[1, 0, clientID]` → GET request
- Server responds: `BUFFEREMPTY`, `POSITIVEACK`, or `BUFFERFULL`

### 3. DATA (Type=2)
- `[2, 1, senderID, receiverID, length, payload...]` → PUSH
- Server responds with `POSITIVEACK` or error
- Retrieval via GET includes `[2, 0, yourID, senderID, length, payload...]`

> 🔢 Payload max: 254 bytes | 📬 Buffer size: 100 messages/client

---

## 🔁 Example Workflow

1. A connects (ID=1) and B connects (ID=2).
2. A pushes a message: `[2,1,1,2,len,"hello"]`.
3. B fetches via GET and receives the original message.

---

## 🛠 Developer Tips

- Adapt `client.html`’s WebSocket URL for deployment.
- Add enhanced features: authentication, encryption, chat rooms.
- Logging is in browser console and server terminal for debugging.

---
