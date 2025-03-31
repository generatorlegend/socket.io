---
title: Getting Started with Socket.IO
---

# Getting Started with Socket.IO

Socket.IO is a powerful library that enables real-time, bidirectional communication between web clients and servers. This guide will help you get started with Socket.IO by walking you through the installation process, basic setup for both server and client, and a simple example of real-time communication.

## Installation

To begin using Socket.IO, you need to install it in your project. You can do this using npm (Node Package Manager) for both the server and client sides.

### Server-side installation

```bash
npm install socket.io
```

### Client-side installation

```bash
npm install socket.io-client
```

## Basic Setup

### Server-side Setup

Here's a basic example of setting up a Socket.IO server:

```javascript
const express = require('express');
const app = express();
const http = require('http').createServer(app);
const io = require('socket.io')(http);

app.get('/', (req, res) => {
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', (socket) => {
  console.log('A user connected');
  
  socket.on('disconnect', () => {
    console.log('User disconnected');
  });
});

http.listen(3000, () => {
  console.log('Server listening on *:3000');
});
```

### Client-side Setup

Here's a basic example of setting up a Socket.IO client in your HTML file:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Socket.IO Chat</title>
</head>
<body>
  <ul id="messages"></ul>
  <form id="chat-form">
    <input id="chat-input" type="text" autocomplete="off" />
    <button>Send</button>
  </form>

  <script src="/socket.io/socket.io.js"></script>
  <script>
    const socket = io();
    
    const form = document.getElementById('chat-form');
    const input = document.getElementById('chat-input');
    const messages = document.getElementById('messages');

    form.addEventListener('submit', (e) => {
      e.preventDefault();
      if (input.value) {
        socket.emit('chat message', input.value);
        input.value = '';
      }
    });

    socket.on('chat message', (msg) => {
      const li = document.createElement('li');
      li.textContent = msg;
      messages.appendChild(li);
    });
  </script>
</body>
</html>
```

## Simple Example: Real-time Chat

Let's create a simple real-time chat application to demonstrate how Socket.IO works. We'll expand on the basic setup we've already created.

### Server-side Code

Update your server-side code to handle chat messages:

```javascript
const express = require('express');
const app = express();
const http = require('http').createServer(app);
const io = require('socket.io')(http);

app.get('/', (req, res) => {
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', (socket) => {
  console.log('A user connected');
  
  socket.on('chat message', (msg) => {
    io.emit('chat message', msg);
  });

  socket.on('disconnect', () => {
    console.log('User disconnected');
  });
});

http.listen(3000, () => {
  console.log('Server listening on *:3000');
});
```

### Client-side Code

The client-side code in the HTML file we created earlier already handles sending and receiving chat messages.

## Running the Application

1. Save the server-side code in a file named `server.js`.
2. Save the client-side code in a file named `index.html` in the same directory.
3. Run the server using Node.js:

```bash
node server.js
```

4. Open a web browser and navigate to `http://localhost:3000`.
5. Open multiple browser windows or tabs to simulate different users.
6. Start chatting! You should see messages appear in real-time across all open windows.

## Conclusion

This guide has walked you through the basics of setting up Socket.IO for both the server and client sides, and demonstrated a simple real-time chat application. Socket.IO offers many more features for building robust real-time applications, including room-based communication, custom events, and more. 

For more advanced usage and detailed API documentation, please refer to the [official Socket.IO documentation](https://socket.io/docs/).

Happy coding with Socket.IO!