📨 WebSocket Messenger App
A lightweight WebSocket-based messenger application using a custom binary protocol for efficient real-time messaging. This project includes:

A Python WebSocket server using asyncio and websockets.

A JavaScript-based client with an interactive HTML UI.

Support for direct client-to-client messaging using a structured protocol.

🚀 Features
📡 Real-time messaging via WebSocket.

📦 Custom binary packet protocol for low-overhead communication.

🔐 Client ID-based session management.

📥 Buffering for offline messages.

📤 Explicit message retrieval (pull model).

🧪 Compact error-handling and ACK/NACK responses.

📁 Project Structure
bash
Copy
Edit
.
├── server.py            # Python WebSocket server
├── client.html          # Frontend with embedded JS client logic
├── style.css            # Basic styling (if applicable)
└── README.md            # This file
⚙️ Setup Instructions
📌 Server Requirements
Python 3.7+

websockets library

📦 Install Dependencies
bash
Copy
Edit
pip install websockets
▶️ Run the Server
bash
Copy
Edit
python server.py
The server listens on:

arduino
Copy
Edit
ws://localhost:12345
🌐 For remote access, use port forwarding (e.g., ngrok) and update the WebSocket URL in the client.

🌐 Using the Client
Open client.html in a browser.

Enter a unique Client ID (0–255).

Click Associate to connect.

Use:

Send to push a message to another client.

Get to pull a message from your buffer.

📡 Protocol Overview
1. 📁 MANAGEMENT Packets (Type = 0)
Message Code	Meaning	Direction
0	ASSOCIATE (Request)	Client → Server
1	ASSOCIATIONSUCCESS	Server → Client
2	ASSOCIATIONFAILED	Server → Client
3	UNKNOWNERROR	Server → Client

2. 🔧 CONTROL Packets (Type = 1)
Message Code	Meaning	Direction
0	GET (Request)	Client → Server
1	BUFFEREMPTY	Server → Client
2	POSITIVEACK	Server → Client
3	BUFFERFULL	Server → Client

3. ✉️ DATA Packets (Type = 2)
Message Code	Meaning	Direction
1	PUSH (Send Msg)	Client → Server
0	GETRESPONSE	Server → Client

DATA Packet Format:
css
Copy
Edit
[Type:1][MessageCode:1][SenderID:1][ReceiverID:1][Length:1][Payload:variable]
🧠 Example Workflow
Client A (ID 1) connects and sends a message to Client B (ID 2).

If B is not online, the server buffers it.

When B connects and requests messages via GET, the server responds with the oldest buffered message.

📌 Notes
Maximum payload size: 254 bytes

Message buffer per client: 100 messages

All communication is in binary format using Uint8Array (client) and struct.pack (server).

🛠 Developer Notes
Update client.html → WebSocket URL to your current IP/ngrok tunnel when testing remotely.

Debug logs are printed both in browser console and terminal for ease of development.

This structure can be extended for authentication, chat rooms, or message encryption.
