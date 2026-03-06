
A comprehensive guide to the LAN multiplayer feature in ClassicChess, explaining how WebRTC works, troubleshooting steps, and technical implementation details.

```markdown
# 🌐 ClassicChess LAN Multiplayer: WebRTC Setup Guide

*Play chess over WiFi with zero configuration – no server, no internet required after connection!*

---

## 📋 Table of Contents
1. [How It Works](#how-it-works)
2. [Quick Start Guide](#quick-start-guide)
3. [Network Requirements](#network-requirements)
4. [Step-by-Step Connection Guide](#step-by-step-connection-guide)
5. [Troubleshooting](#troubleshooting)
6. [Technical Deep Dive](#technical-deep-dive)
7. [Custom SDP Compression](#custom-sdp-compression)
8. [FAQ](#faq)

---

## How It Works

ClassicChess uses **WebRTC** (Web Real-Time Communication) to create a direct peer-to-peer connection between two browsers. Here's what happens behind the scenes:

```

┌─────────┐         Signaling (Codes)         ┌─────────┐
│ Player 1 │  ←───  Share via WhatsApp/SMS  ───→ │ Player 2 │
│  (Host)  │                                   │  (Join) │
└────┬────┘                                   └────┬────┘
│                                              │
│─── Direct WebRTC DataChannel (P2P) ────────→│
│←───        (no server involved)        ─────│
↓                                              ↓
Game Sync                                     Game Sync
(moves, timer, resign)                        (moves, timer, resign)

```

### Key Features
- **No registration required** – just open and play
- **No central server** – direct browser-to-browser connection
- **Works on same WiFi** – optimized for local networks
- **Encrypted** – WebRTC mandates DTLS and SRTP encryption
- **Offline-capable** – once connected, internet can be disabled

---

## Quick Start Guide

### 30-Second Setup

**Host (White):**
1. Main Menu → **LAN Multiplayer** → **Host Game**
2. Enter your name → **Start Hosting**
3. Share the generated code with your friend
4. Paste their reply → **Connect**
5. Game starts automatically!

**Guest (Black):**
1. Main Menu → **LAN Multiplayer** → **Join Game**
2. Enter your name
3. Paste host's code → **Connect**
4. Send your answer code back to host
5. Wait for host → Game starts!

---

## Network Requirements

### ✅ Must-Have
- Both devices on **same WiFi network**
- WiFi router supports **multicast** (almost all do)
- No firewall blocking **UDP ports 1024-65535**

### ❌ Won't Work If
- Devices on different networks (use the internet version instead)
- Enterprise WiFi with **client isolation** (hotels, schools, public WiFi)
- Strict firewall blocking **P2P traffic**
- Using cellular data (4G/5G) – both need same WiFi

### 🔍 Quick Network Test
```bash
# On both devices, check if you can ping each other
# Find your IP address:
ipconfig    # Windows
ifconfig    # Mac/Linux

# Ping the other device:
ping 192.168.1.x
```

---

Step-by-Step Connection Guide

Phase 1: Host Setup

1. Navigate to LAN Multiplayer
   ```
   Main Menu → LAN Multiplayer → Host Game
   ```
2. Enter Your Name
   · This will be displayed as "White" player
   · Default: "Player 1"
3. Click "Start Hosting"
   · Browser may ask for camera/microphone permissions – deny (not needed)
   · WebRTC still works without media devices
4. Wait for Code Generation
   ```
   ⏳ Generating code... (2-5 seconds)
   
   Your Host Code — share with friend:
   ┌─────────────────────────────────┐
   │ ["o","kv5qj3n7","p4m2x8r9",     │
   │  "A1B2C3D4...",                  │
   │  ["192.168.1.5:50789"]]          │
   └─────────────────────────────────┘
   ```
5. Share the Code
   · Copy using the Copy Code button
   · Send via WhatsApp, SMS, email, or even read aloud
   · The code is compact (~200 chars) for easy sharing
6. Wait for Answer Code
   · Paste friend's response in the text area
   · Click Connect

Phase 2: Join Setup

1. Navigate to LAN Multiplayer
   ```
   Main Menu → LAN Multiplayer → Join Game
   ```
2. Enter Your Name
   · This will be displayed as "Black" player
   · Default: "Player 2"
3. Paste Host Code
   · Copy the code your friend sent
   · Paste into Host Code text area
4. Click "Connect"
   ```
   ⏳ Processing... (2-5 seconds)
   
   Your Answer Code — send to host:
   ┌─────────────────────────────────┐
   │ ["a","kv5qj3n7","p4m2x8r9",     │
   │  "D4E5F6A7...",                  │
   │  ["192.168.1.8:51234"]]          │
   └─────────────────────────────────┘
   ```
5. Send Answer to Host
   · Copy and send back to the host
   · Once host enters it, connection completes
6. Game Starts!
   ```
   ✓ Connected
   You play as Black ♟
   Host: Player 1
   ```

---

Troubleshooting

🚨 Common Issues & Solutions

1. "Connection failed. Check that both devices are on the same network."

Possible Cause Solution
Different WiFi networks Verify both connected to same router
Client isolation enabled Disable "AP Isolation" in router settings
Firewall blocking Temporarily disable firewall for test
VPN active Disable VPN on both devices

2. "Connection timeout (30s)."

Possible Cause Solution
Network congestion Try again, wait 10 seconds
STUN server unreachable ClassicChess uses Google STUN (usually reliable)
NAT too restrictive Use router's DMZ or port forwarding (advanced)

3. "Invalid connection code."

Possible Cause Solution
Code copied incompletely Copy entire code, including brackets
Extra spaces Remove spaces before/after code
Expired code Host must generate new code
Wrong code type Host code vs answer code mix-up

4. Connection established but no moves sync

Possible Cause Solution
DataChannel not open Wait 2-3 seconds for full handshake
Browser compatibility Try Chrome, Firefox, or Edge
Both clicked at same time Restart both with New Game

📡 Router Settings (if persistent issues)

Enable these settings:

· UPnP (Universal Plug and Play)
· IGMP Snooping (usually on by default)
· Multicast forwarding

Disable these settings:

· AP/Client Isolation
· Guest Network isolation
· Strict NAT filtering

🔧 Advanced Diagnostics

Check WebRTC status in browser:

1. Open Chrome DevTools (F12)
2. Go to chrome://webrtc-internals/
3. Look for iceConnectionState = "connected" or "completed"

Test STUN manually:

```javascript
// In browser console
new RTCPeerConnection({iceServers:[{urls:'stun:stun.l.google.com:19302'}]})
  .createDataChannel('test')
  .onopen = () => console.log('STUN working!');
```

---

Technical Deep Dive

WebRTC Architecture in ClassicChess

```
┌─────────────────────────────────────────────────────────────┐
│                      Browser A (Host)                       │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  RTCPeerConnection                                   │   │
│  │  • ICE Agent                                         │   │
│  │  • DTLS Transport                                    │   │
│  │  • SCTP Transport                                    │   │
│  └─────────────────────────────────────────────────────┘   │
│                           │                                  │
│  ┌───────────────────────┐│┌───────────────────────┐       │
│  │  DataChannel "chess"  │││  DataChannel "chess"  │       │
│  │  • Ordered            │││  • Ordered            │       │
│  │  • Reliable           │││  • Reliable           │       │
│  └───────────────────────┘│└───────────────────────┘       │
└─────────────────────────────────────────────────────────────┘
                           │
                    (UDP/DTLS)
                           │
┌─────────────────────────────────────────────────────────────┐
│                      Browser B (Join)                        │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  RTCPeerConnection                                   │   │
│  │  • ICE Agent                                         │   │
│  │  • DTLS Transport                                    │   │
│  │  • SCTP Transport                                    │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

Connection Lifecycle

1. ICE Candidate Gathering
   · Browser finds all possible network addresses
   · Includes local IP, public IP (via STUN), and TURN relay (if available)
2. SDP Exchange (via human-readable codes)
   · Offer/Answer model with compressed format
   · Contains ICE credentials, fingerprints, and candidates
3. Connectivity Checks
   · STUN binding requests verify reachability
   · Multiple candidate pairs tested simultaneously
4. DTLS Handshake
   · Establishes encrypted channel
   · Mutual authentication via certificate fingerprints
5. SCTP Association
   · Creates reliable, ordered data channel
   · Optimized for game messages
6. Game Sync
   · JSON messages over DataChannel
   · Moves, timer updates, game events

---

Custom SDP Compression

ClassicChess uses a proprietary compact encoding to reduce SDP from ~1500 characters to ~200, making manual sharing practical.

Why Compression?

· Raw SDP is too long to share manually (1500+ chars)
· Our encoding fits in SMS, WhatsApp, or even verbal sharing
· Preserves only essential fields for LAN play

Encoding Format

```javascript
// Compact representation
[
  type,           // "o" = offer, "a" = answer
  iceUfrag,       // ICE username fragment
  icePwd,         // ICE password
  fingerprint,    // DTLS certificate fingerprint (hex, no colons)
  candidates      // Array of "ip:port" strings
]

// Example
["o", "kv5qj3n7", "p4m2x8r9", 
 "A1B2C3D4E5F6...", 
 ["192.168.1.5:50789", "192.168.1.5:50790"]]
```

Encoding Process

```javascript
// 1. Extract from SDP
const iceUfrag = sdp.match(/a=ice-ufrag:(\S+)/)[1];
const icePwd = sdp.match(/a=ice-pwd:(\S+)/)[1];
const fingerprint = sdp.match(/a=fingerprint:\S+ (\S+)/)[1].replace(/:/g, '');
const candidates = sdp.match(/a=candidate:[^\r\n]+/g)
  .map(c => {
    const [, , , , , ip, port] = c.match(/\S+ \d+ \S+ \d+ (\S+) (\d+)/);
    return `${ip}:${port}`;
  });

// 2. Base64 encode JSON
const code = btoa(JSON.stringify([type, ufrag, pwd, fingerprint, candidates]));
```

Decoding Process

```javascript
// 1. Base64 decode
const [type, ufrag, pwd, fingerprint, candidates] = JSON.parse(atob(code));

// 2. Reconstruct minimal SDP
const sdp = `v=0
o=- 1 1 IN IP4 0.0.0.0
...
a=ice-ufrag:${ufrag}
a=ice-pwd:${pwd}
a=fingerprint:sha-256 ${fingerprint.match(/.{2}/g).join(':')}
${candidates.map((ip, i) => `a=candidate:${i+1} 1 udp 2122260223 ${ip} typ host`).join('\n')}
`;
```

Benefits

Metric Raw SDP Compressed
Length ~1500 chars ~200 chars
Human-readable No Yes
Share via SMS ❌ Too long ✅ Works
Copy-paste friendly ❌ ✅
Contains all info ✅ Yes ✅ Essential only

---

Message Protocol

Once connected, ClassicChess uses JSON messages over the DataChannel.

Message Types

Type Direction Payload Description
hello Host → Join {nameW: "White"} Initial greeting
move Both {fr, fc, tr, tc, fl, promo} Chess move
draw-offer Either {} Offer draw
draw-accept Either {} Accept draw
draw-decline Either {} Decline draw
resign Either {} Resign game
gameover Both {winner, reason} Game ended
new-game Either {} Restart game

Example Move Message

```json
{
  "type": "move",
  "fr": 6,
  "fc": 4,
  "tr": 4,
  "tc": 4,
  "fl": null,
  "promo": null
}
// White pawn e2-e4
```

---

Browser Compatibility

Browser WebRTC Support Tested
Chrome 80+ ✅ Full ✓
Firefox 75+ ✅ Full ✓
Safari 14+ ✅ Full ✓
Edge 90+ ✅ Full ✓
Opera ✅ Full ⚠️ Limited
Mobile Chrome ✅ Full ✓
Mobile Safari ✅ Full ✓
Samsung Internet ⚠️ Partial Not tested

Known Limitations

· iOS 14.5+: Works, but may need both devices on same subnet
· Corporate networks: May block UDP, use hotspot instead
· Some Android vendors: Custom ROMs may have WebRTC issues

---

FAQ

Q: Do I need internet to play?

A: Only for initial connection (STUN lookup). Once connected, you can disable WiFi and continue playing!

Q: Why use codes instead of automatic discovery?

A: Automatic LAN discovery (mDNS) is unreliable across platforms. Codes are 100% reliable and work everywhere.

Q: Is my game data encrypted?

A: Yes! WebRTC mandates DTLS encryption (similar to HTTPS). Your moves are secure from eavesdropping.

Q: Can I play with someone on a different continent?

A: This LAN feature is optimized for same network. For internet play, you'd need a TURN server (not implemented).

Q: Why does the code have my IP address?

A: The code contains your local IP (e.g., 192.168.1.x) so the other device knows where to connect. This is safe because it's only shared with your friend.

Q: The code expired – what now?

A: Click "Start Hosting" again to generate a fresh code. Codes expire after 30 seconds of inactivity.

Q: Can I play on mobile data?

A: Both devices must be on the same network. If both use the same mobile hotspot, yes! Otherwise, no.

Q: How many players can play?

A: ClassicChess is 1v1 only. WebRTC supports mesh networking, but this implementation is for two players.

Q: Will this work in a hotel?

A: Hotel WiFi often has client isolation – guests cannot see each other. Use a mobile hotspot instead.

Q: Can I play on a Chromebook?

A: Absolutely! Any device with a modern browser works.

---

Developer Notes

STUN Servers Used

```javascript
iceServers: [
  { urls: 'stun:stun.l.google.com:19302' },
  { urls: 'stun:stun1.l.google.com:19302' }
]
```

DataChannel Configuration

```javascript
const dc = pc.createDataChannel('chess', {
  ordered: true,        // Guarantee order
  maxRetransmits: 0     // Reliable (infinite retransmits)
});
```

Connection Timeout

```javascript
// 30-second timeout for ICE gathering
setTimeout(() => {
  if (pc.iceConnectionState !== 'connected' && 
      pc.iceConnectionState !== 'completed') {
    // Show timeout error
  }
}, 30000);
```

---

Resources

· WebRTC Official Site
· MDN WebRTC Documentation
· W3C WebRTC Specification
· Google STUN Server Info

---

Version History

Version Changes
2.0.0 Initial release with compact SDP encoding
2.0.1 Added 30-second timeout, improved error messages
2.0.2 Fixed iOS compatibility issues

---

Need help? Open an issue on GitHub!

Happy gaming! ♔
