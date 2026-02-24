<div align="center">

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                    BASED · by GabWT333                     -->
<!-- ═══════════════════════════════════════════════════════════ -->

<img src="https://qu.ax/FNs6D" width="90" height="90" style="border-radius:50%"/>

```
╔══════════════════════════════════════════════════════════╗
║                                                          ║
║    ██████╗  █████╗ ███████╗███████╗██████╗               ║
║    ██╔══██╗██╔══██╗██╔════╝██╔════╝██╔══██╗              ║
║    ██████╔╝███████║███████╗█████╗  ██║  ██║              ║
║    ██╔══██╗██╔══██║╚════██║██╔══╝  ██║  ██║              ║
║    ██████╔╝██║  ██║███████║███████╗██████╔╝              ║
║    ╚═════╝ ╚═╝  ╚═╝╚══════╝╚══════╝╚═════╝               ║
║                                                          ║
║              whatsapp web api · by GabWT333              ║
╚══════════════════════════════════════════════════════════╝
```

![version](https://img.shields.io/npm/v/@GabWT333/based?style=flat-square&color=00bfff&labelColor=0a0f1e&label=npm)
![downloads](https://img.shields.io/npm/dm/@GabWT333/based?style=flat-square&color=1e90ff&labelColor=0a0f1e&label=downloads%2Fmo)
![stars](https://img.shields.io/github/stars/GabWT333/based?style=flat-square&color=00bfff&labelColor=0a0f1e&label=stars)
![license](https://img.shields.io/badge/license-MIT-1e90ff?style=flat-square&labelColor=0a0f1e)
![node](https://img.shields.io/badge/node-18%2B-00bfff?style=flat-square&labelColor=0a0f1e)

<br>

> **`@GabWT333/based`** — A fast, modern WhatsApp Web API library built on Baileys.
> Full LID/JID support · Multi-device · TypeScript · Anti-ban ready.

<br>

</div>

---

## `→` What is this?

**based** is a WhatsApp Web API library forked and maintained by **[GabWT333](https://github.com/GabWT333)**, built on top of [Baileys](https://github.com/WhiskeySockets/Baileys) with major improvements:

- ✦ Native **LID/JID** resolution — no more `undefined` sender errors
- ✦ Smart **TTL cache** system — 80–90% fewer API calls
- ✦ Built-in **anti-ban** configuration
- ✦ Full **TypeScript** support
- ✦ All message types including **2025 WhatsApp features**

---

## `→` Installation

```bash
npm install @GabWT333/based
```

```bash
yarn add @GabWT333/based
```

---

## `→` Quickstart

```typescript
import makeWASocket, {
  useMultiFileAuthState,
  DisconnectReason,
  setPerformanceConfig
} from '@GabWT333/based';

setPerformanceConfig({
  performance: { enableCache: true, syncFullHistory: false },
  debug: { enableLidLogging: true, logLevel: 'info' }
});

async function start() {
  const { state, saveCreds } = await useMultiFileAuthState('auth');

  const sock = makeWASocket({
    auth: state,
    printQRInTerminal: true,
    browser: ['GabWT333', 'Chrome', '4.0.0'],
    markOnlineOnConnect: false,   // anti-ban
    syncFullHistory: false        // anti-slowdown
  });

  sock.ev.on('connection.update', ({ connection }) => {
    if (connection === 'open') console.log('✦ connected');
    if (connection === 'close') start();
  });

  sock.ev.on('creds.update', saveCreds);

  sock.ev.on('messages.upsert', async ({ messages }) => {
    const msg = messages[0];
    if (!msg.message) return;
    const jid = msg.key.remoteJid!;
    await sock.sendMessage(jid, { text: 'pong' }, { quoted: msg });
  });
}

start();
```

---

## `→` Anti-Ban Setup

```typescript
setPerformanceConfig({
  performance: {
    enableCache: true,
    batchSize: 50,              // simulate human speed
    maxRetries: 5,
    retryDelay: 5000,
    retryBackoffMultiplier: 1.5,
    maxRetryDelay: 60000,
    maxMsgRetryCount: 3
  }
});

const sock = makeWASocket({
  markOnlineOnConnect: false,  // ← critical
  syncFullHistory: false       // ← critical
});
```

---

## `→` LID / JID Utilities

```typescript
import {
  getSenderLid,
  validateJid,
  toJid,
  normalizeJid,
  isValidJid
} from '@GabWT333/based';

// extract sender from any message
const info = getSenderLid(msg);
// → { jid, lid, user, isValid, timestamp, error }

// validate before use
const check = validateJid(info.jid);
if (check.isValid) {
  await sock.sendMessage(info.jid, { text: 'hello' });
}

// convert LID → JID
const jid = toJid('123456@lid');
// → '123456@s.whatsapp.net'
```

**All events** (`messages.upsert`, `group-participants.update`, etc.) automatically expose:
- `msg.key.remoteJid` — standard JID
- `msg.key.remoteLid` — original LID (when available)
- `msg.key.participantLid` — group participant LID

---

## `→` Cache System

```typescript
import {
  CacheManager,
  getCacheStats,
  clearCache,
  setPerformanceConfig
} from '@GabWT333/based';

// configure
setPerformanceConfig({
  cache: {
    lidCache:           { ttl: 300000, maxSize: 10000, cleanupInterval: 120000 },
    jidCache:           { ttl: 300000, maxSize: 10000, cleanupInterval: 120000 },
    lidToJidCache:      { ttl: 300000, maxSize: 5000,  cleanupInterval: 180000 },
    groupMetadataCache: { ttl: 600000, maxSize: 2000,  cleanupInterval: 300000 }
  }
});

// read stats
const stats = getCacheStats();
console.log(`LID cache: ${stats.lidCache.size}/${stats.lidCache.maxSize}`);

// manual clear
clearCache();
```

---

## `→` Sending Messages

### Text & Formatting

```typescript
await sock.sendMessage(jid, {
  text: '*bold* _italic_ ~strike~ `mono`\n@mention',
  mentions: ['393476686131@s.whatsapp.net']
});
```

### Media

```typescript
// image
await sock.sendMessage(jid, { image: { url: './img.jpg' }, caption: 'caption' });

// video
await sock.sendMessage(jid, { video: { url: './clip.mp4' }, caption: 'clip', gifPlayback: false });

// audio / PTT
await sock.sendMessage(jid, { audio: { url: './voice.mp3' }, mimetype: 'audio/mp4', ptt: true });

// document
await sock.sendMessage(jid, { document: { url: './file.pdf' }, mimetype: 'application/pdf', fileName: 'doc.pdf' });

// album
await sock.sendMessage(jid, { album: bufferArray.map(b => ({ image: b })) });
```

### Buttons & Lists

```typescript
// quick reply buttons
await sock.sendMessage(jid, {
  text: 'pick one:',
  footer: 'GabWT333',
  buttons: [
    { buttonId: 'a', buttonText: { displayText: 'Option A' }, type: 1 },
    { buttonId: 'b', buttonText: { displayText: 'Option B' }, type: 1 }
  ]
});

// list
await sock.sendMessage(jid, {
  text: 'menu',
  title: 'Select',
  buttonText: 'Open',
  sections: [{
    title: 'Section 1',
    rows: [
      { title: 'Item 1', rowId: 'i1', description: 'desc' },
      { title: 'Item 2', rowId: 'i2', description: 'desc' }
    ]
  }]
});
```

### Poll

```typescript
await sock.sendMessage(jid, {
  poll: {
    name: 'Best framework?',
    values: ['Node', 'Deno', 'Bun'],
    selectableCount: 1
  }
});
```

### Stickers

```typescript
// from URL
await sock.sendMessage(jid, { sticker: { url: './sticker.webp' } });

// sticker pack
await sock.sendMessage(jid, {
  stickerPack: {
    name: 'My Pack',
    publisher: 'GabWT333',
    stickers: [
      { sticker: { url: './s1.webp' }, emojis: ['🎉'] },
      { sticker: { url: './s2.webp' }, emojis: ['😄'], isAnimated: true }
    ]
  }
});
```

### Carousel

```typescript
await sock.sendMessage(jid, {
  text: 'Check this out',
  cards: [
    { title: 'Card 1', body: 'desc', image: { url: './img1.jpg' },
      buttons: [{ name: 'quick_reply', buttonParamsJson: '{"display_text":"Go"}' }] },
    { title: 'Card 2', body: 'desc', video: { url: './clip.mp4' } }
  ],
  carouselCardType: 1
});
```

### Payments

```typescript
// request
await sock.sendMessage(jid, {
  requestPayment: { currency: 'EUR', amount1000: 5000, requestFrom: jid, note: 'invoice' }
});

// send
await sock.sendMessage(jid, {
  sendPayment: {
    requestMessageKey: { remoteJid: jid, fromMe: false, id: 'msgId' },
    noteMessage: { text: 'paid' }
  }
});
```

### Pin / Unpin

```typescript
await sock.sendMessage(jid, {
  pin: { key: { remoteJid: jid, fromMe: false, id: 'msgId' }, type: 1, time: 86400 }
});
```

---

## `→` Group Management

```typescript
// create
const g = await sock.groupCreate('My Group', ['user@s.whatsapp.net']);

// participants
await sock.groupParticipantsUpdate(jid, ['user@s.whatsapp.net'], 'add');     // or 'remove'
await sock.groupParticipantsUpdate(jid, ['user@s.whatsapp.net'], 'promote'); // or 'demote'

// settings
await sock.groupUpdateSubject(jid, 'New Name');
await sock.groupUpdateDescription(jid, 'New description');
await sock.groupSettingUpdate(jid, 'announcement');   // admin-only messages
await sock.groupToggleEphemeral(jid, 86400);          // disappearing messages
await sock.groupJoinApprovalMode(jid, 'on');          // require approval

// invite
const code = await sock.groupInviteCode(jid);
await sock.groupRevokeInvite(jid);
await sock.groupAcceptInvite(code);
await sock.groupLeave(jid);
```

---

## `→` Events Reference

| Event | Fires when |
|---|---|
| `connection.update` | Connection opens, closes, or errors |
| `creds.update` | Auth credentials change (save these!) |
| `messages.upsert` | New message received or sent |
| `messages.update` | Message status update (sent/delivered/read) |
| `group-participants.update` | Someone joins/leaves/is promoted |

```typescript
sock.ev.on('messages.update', updates => {
  for (const u of updates) {
    console.log(`msg ${u.key.id} → status ${u.update.status}`);
    // 1 = sent · 2 = delivered · 3 = read
  }
});

sock.ev.on('group-participants.update', update => {
  console.log(update.id, update.action, update.participants);
});
```

---

## `→` Full Socket Options

```typescript
const sock = makeWASocket({
  auth: state,
  printQRInTerminal: true,
  logger: console,
  browser: ['GabWT333', 'Chrome', '4.0.0'],
  defaultQueryTimeoutMs: 60000,
  keepAliveIntervalMs: 30000,
  connectTimeoutMs: 60000,
  retryRequestDelayMs: 250,
  maxMsgRetryCount: 5,
  markOnlineOnConnect: false,           // anti-ban
  syncFullHistory: false,               // anti-slowdown
  fireInitQueries: true,
  generateHighQualityLinkPreview: true
});
```

---

## `→` Troubleshooting

**Connection keeps dropping**
→ Use exponential backoff: `maxRetries: 5`, `retryBackoffMultiplier: 1.5`.

**`undefined` sender in groups**
→ Always use `getSenderLid(msg)` instead of `msg.key.participant` directly.

**Memory growing over time**
→ Lower `maxSize` on cache or reduce `ttl`. Call `clearCache()` periodically.

**Getting banned**
→ Set `markOnlineOnConnect: false`, `syncFullHistory: false`, lower `batchSize`.

---

## `→` API Quick Reference

| Method | Returns | Description |
|---|---|---|
| `makeWASocket(config)` | `WASocket` | Create connection |
| `useMultiFileAuthState(dir)` | `{ state, saveCreds }` | Persistent auth |
| `getSenderLid(msg)` | `SenderInfo` | Extract sender JID/LID |
| `validateJid(jid)` | `{ isValid, error }` | Validate a JID |
| `toJid(lid)` | `string` | LID → JID |
| `normalizeJid(jid)` | `string` | Normalize JID format |
| `isValidJid(jid)` | `boolean` | Simple validity check |
| `getCacheStats()` | `CacheStats` | Cache hit/miss stats |
| `clearCache()` | `void` | Flush all caches |
| `setPerformanceConfig(cfg)` | `void` | Update perf settings |
| `getPerformanceConfig()` | `Config` | Read current settings |

---

## `→` Security

| Layer | Implementation |
|---|---|
| End-to-End Encryption | Signal Protocol |
| Key Management | Auto generation & rotation |
| Authentication | QR code or pairing code |
| Credential Storage | Encrypted local files |

---

## `→` Acknowledgments

| Project | Role |
|---|---|
| [Baileys](https://github.com/WhiskeySockets/Baileys) | Core WhatsApp Web API engine |
| [Yupra](https://www.npmjs.com/package/@yupra/baileys) | LID/JID resolution research |
| [Signal Protocol](https://signal.org/) | End-to-end encryption layer |

---

## `→` License & Disclaimer

**MIT License © 2025 [GabWT333](https://github.com/GabWT333)**

> ⚠ Not affiliated with WhatsApp Inc. or Meta.
> For educational and development use only.
> Misuse may result in account suspension.

---

<div align="center">

<br>

<img src="https://qu.ax/FNs6D" width="72" height="72" style="border-radius:50%">

<br>

**[GabWT333](https://github.com/GabWT333)** · built with 💙

<br>

![](https://img.shields.io/badge/github-GabWT333%2Fbased-00bfff?style=flat-square&logo=github&labelColor=0a0f1e)
&nbsp;
![](https://img.shields.io/badge/npm-@GabWT333%2Fbased-1e90ff?style=flat-square&logo=npm&labelColor=0a0f1e)

<br>

</div>
