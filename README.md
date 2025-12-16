# UltraShare LAN

**UltraShare LAN** is a lightweight web application for fast peer-to-peer file transfers over a local network (LAN). It allows users to send and receive files directly between browsers without installing additional software.

---

## Overview

UltraShare LAN works by establishing a **peer-to-peer connection** between two devices on the same network. Users can transfer files in real time using configurable chunk sizes, providing a balance between speed and reliability.

---

## Usage Instructions

### 1. Setting Up

1. Open the UltraShare LAN webpage on one device.  
2. The page will generate a **QR code** and a direct link for connecting other devices.  
3. On the second device, simply scan the QR code or open the link.  
   - This will automatically open the webpage and establish a connection to the first device.  
4. It is **indifferent which device opens first**; the connection is fully bidirectional.

### 2. Establishing Connection

- Once the QR code or link is used, a **peer-to-peer connection** is established.  
- The connection status is displayed at the top of the page.  
- After the connection is established, **files can be sent and received in both directions**, without restriction on which device initiated the transfer.

### 3. Sending and Receiving Files

1. Either device can select files in the upload area to send to the other device.  
2. Files are split into **chunks** and sent sequentially.  
3. The receiving device reconstructs the files and saves them directly to disk.  
4. Transfer progress and speed are displayed for each file.

---

## Chunk Size Configuration

UltraShare LAN allows you to select the **chunk size** for file transfers:

- **16 KB (Stable)** – safe for unreliable networks.
- **256 KB** – medium performance.
- **512 KB (Default)** – balanced speed and reliability.
- **1 MB (Fast)** – maximum speed but higher risk of network buffer delays.

**How chunk size affects performance:**

- Smaller chunks reduce the chance of overloading the network but may increase overhead slightly.
- Larger chunks can increase throughput but require more buffer space, which can cause short pauses if the connection cannot keep up.
- Users can change the chunk size before sending files to optimize for network conditions.

---

## Technical Details

UltraShare LAN uses modern web technologies to provide **efficient peer-to-peer file transfers** without external servers (except for initial signaling).

### 1. Peer-to-Peer Communication

- **WebRTC DataChannel** is used for all file transfers.
  - Transport protocol: **UDP**, encapsulated via **SCTP** for reliability.
  - Reliable mode ensures all chunks arrive in order without loss.
- **PeerJS** library simplifies establishing WebRTC connections.
  - Manages peer IDs and connection signaling.
  - Handles events such as connection open, data received, and connection close.

### 2. File Transfer Process

- Files are divided into **binary chunks** in JavaScript.
- Each chunk is sent through the DataChannel: state.conn.send({ type: 'CHUNK', id: fileId, data: chunk });
- The receiving device writes chunks to disk usign **StreamSaver.js**:
-  Transfer progress, percentage, and speed are updated in real time in the UI

### 3. Signaling and Network Requirements

- No STUN or TURN servers are configured (iceServers: []) so connections work only in LAN environments.
- PeerJS signaling server is used only for exchanging initial connection information

## 4. Libraries Used

- **PeerJS** – Simplifies WebRTC peer-to-peer connection setup.
- **QRCode.js** – Generates QR codes for easy device connection.
- **StreamSaver.js** – Writes received files directly to disk.
- **Web Streams Polyfill** – Ensures compatibility of StreamSaver in all browsers.

## 5. Performance Considerations

- Implements **buffer management** to avoid overflowing the DataChannel.
- Dynamically waits if the buffer is full, preventing data loss.
- Uses **wakeLock** (when available) to keep the screen on during transfers.

---

## Limitations

- UltraShare LAN works reliably **only on a local network** unless a STUN/TURN server is configured for NAT traversal.
- Peer-to-peer connections require both devices to be reachable in the same network segment.

---

## Summary

UltraShare LAN provides a **fast, browser-based, peer-to-peer file transfer solution**:

- No installation required.
- Supports multi-file transfers with progress tracking.
- Adjustable chunk sizes for performance optimization.
- Uses **WebRTC**, **PeerJS**, and **StreamSaver.js** for efficient and reliable file transfers.

---

## Example Workflow

1. Host opens UltraShare LAN → QR code or link is generated.
2. Guest scans QR code or opens the link → connection is established.
3. Files are dragged onto the upload area → sent in chunks.
4. Receiver saves files automatically → progress and speed are displayed.

---

## Creator

- UltraShare LAN was created by **Federicowitz**.
- Open source and free to use under the MIT License.

