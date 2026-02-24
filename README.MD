<div align="center">

<!-- BANNER -->
<img src="https://capsule-render.vercel.app/api?type=venom&color=0:020b18,50:00bfff,100:1e90ff&height=280&section=header&text=BAILEYS&fontSize=130&fontColor=ffffff&animation=fadeIn&fontAlignY=55&stroke=00bfff&strokeWidth=2&desc=@GabWT333%20·%20WhatsApp%20Web%20API&descSize=18&descAlignY=78&descColor=7ec8e3" width="100%"/>

<!-- LOGO -->
<br>

<img src="https://qu.ax/FNs6D" width="100" height="100" style="border-radius:50%; border: 3px solid #00bfff; box-shadow: 0 0 20px #00bfff;"/>

<br><br>

<!-- ANIMATED TYPING -->
<img src="https://readme-typing-svg.herokuapp.com?font=JetBrains+Mono&weight=800&size=22&duration=3000&pause=1500&color=00BFFF&center=true&vCenter=true&repeat=true&width=600&height=45&lines=⚡+Fast+WhatsApp+Web+API;🔵+Full+LID+%2F+JID+Support;🛡️+Anti-Ban+Ready;🧠+Smart+TTL+Cache+System;📦+Built+on+Baileys+%2B+Improved" alt="Typing SVG"/>

<br><br>

<!-- BADGES ROW 1 -->
<a href="https://www.npmjs.com/package/@GabWT333/based">
  <img src="https://img.shields.io/npm/v/@GabWT333/based?style=for-the-badge&logo=npm&logoColor=white&label=VERSION&color=00bfff&labelColor=020b18"/>
</a>
<a href="https://www.npmjs.com/package/@GabWT333/based">
  <img src="https://img.shields.io/npm/dm/@GabWT333/based?style=for-the-badge&logo=npm&logoColor=white&label=DOWNLOADS%2FMO&color=1e90ff&labelColor=020b18"/>
</a>
<a href="https://www.npmjs.com/package/@GabWT333/based">
  <img src="https://img.shields.io/npm/dt/@GabWT333/based?style=for-the-badge&logo=npm&logoColor=white&label=TOTAL+DL&color=00bfff&labelColor=020b18"/>
</a>
<a href="https://github.com/GabWT333/based/stargazers">
  <img src="https://img.shields.io/github/stars/GabWT333/based?style=for-the-badge&logo=github&logoColor=white&label=STARS&color=00bfff&labelColor=020b18"/>
</a>
<a href="https://github.com/GabWT333/based/forks">
  <img src="https://img.shields.io/github/forks/GabWT333/based?style=for-the-badge&logo=git&logoColor=white&label=FORKS&color=1e90ff&labelColor=020b18"/>
</a>

<br>

<!-- BADGES ROW 2 -->
<a href="https://github.com/GabWT333/based/blob/main/LICENSE">
  <img src="https://img.shields.io/badge/LICENSE-MIT-1e90ff?style=for-the-badge&labelColor=020b18"/>
</a>
<a href="https://nodejs.org">
  <img src="https://img.shields.io/badge/NODE-18%2B-00bfff?style=for-the-badge&logo=nodedotjs&logoColor=white&labelColor=020b18"/>
</a>
<a href="https://www.typescriptlang.org">
  <img src="https://img.shields.io/badge/TYPESCRIPT-READY-1e90ff?style=for-the-badge&logo=typescript&logoColor=white&labelColor=020b18"/>
</a>
<a href="https://github.com/GabWT333/based/issues">
  <img src="https://img.shields.io/github/issues/GabWT333/based?style=for-the-badge&logo=github&logoColor=white&label=ISSUES&color=00bfff&labelColor=020b18"/>
</a>

<br><br>

<!-- NAV PILLS -->
[![](https://img.shields.io/badge/·─·%20INSTALL-020b18?style=flat-square&color=020b18&labelColor=00bfff)](#-install)
[![](https://img.shields.io/badge/·─·%20QUICKSTART-020b18?style=flat-square&color=020b18&labelColor=1e90ff)](#-quickstart)
[![](https://img.shields.io/badge/·─·%20LID%2FJID-020b18?style=flat-square&color=020b18&labelColor=00bfff)](#-lidjid)
[![](https://img.shields.io/badge/·─·%20MESSAGES-020b18?style=flat-square&color=020b18&labelColor=1e90ff)](#-messages)
[![](https://img.shields.io/badge/·─·%20GROUPS-020b18?style=flat-square&color=020b18&labelColor=00bfff)](#-groups)
[![](https://img.shields.io/badge/·─·%20API%20REF-020b18?style=flat-square&color=020b18&labelColor=1e90ff)](#-api-reference)

</div>

<br>

---

<div align="center">

```
  ╭──────────────────────────────────────────────────────────────────╮
  │                                                                  │
  │   > based is a WhatsApp Web API library maintained by           │
  │     GabWT333, forked from Baileys with serious improvements.    │
  │                                                                  │
  │   > native LID/JID resolution  ·  smart cache  ·  anti-ban      │
  │   > full TypeScript  ·  multi-device  ·  2025 features          │
  │                                                                  │
  ╰──────────────────────────────────────────────────────────────────╯
```

</div>

---

## ⚡ Install

```bash
npm install @GabWT333/based
```
```bash
yarn add @GabWT333/based
```
```bash
pnpm add @GabWT333/based
```

---

## 🚀 Quickstart

<details>
<summary><b>▶ Basic bot (click to expand)</b></summary>

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
    markOnlineOnConnect: false,
    syncFullHistory: false
  });

  sock.ev.on('connection.update', ({ connection, lastDisconnect }) => {
    if (connection === 'open') console.log('✦ connected!');
    if (connection === 'close') {
      const shouldReconnect =
        (lastDisconnect?.error as any)?.output?.statusCode !== DisconnectReason.loggedOut;
      if (shouldReconnect) start();
    }
  });

  sock.ev.on('creds.update', saveCreds);

  sock.ev.on('messages.upsert', async ({ messages }) => {
    const msg = messages[0];
    if (!msg.message) return;
    await sock.sendMessage(msg.key.remoteJid!, { text: 'pong 🏓' }, { quoted: msg });
  });
}

start();
```

</details>

<details>
<summary><b>🛡️ Anti-ban hardened setup (click to expand)</b></summary>

```typescript
setPerformanceConfig({
  performance: {
    enableCache: true,
    batchSize: 50,
    maxRetries: 5,
    retryDelay: 5000,
    retryBackoffMultiplier: 1.5,
    maxRetryDelay: 60000,
    maxMsgRetryCount: 3
  }
});

const sock = makeWASocket({
  markOnlineOnConnect: false, // ← never appear online automatically
  syncFullHistory: false,     // ← don't pull chat history on connect
  generateHighQualityLinkPreview: true
});
```

</details>

---

## 🔵 LID/JID

> WhatsApp is migrating to a new identifier format called **LID**. Without proper handling, group messages return `undefined` senders. **based** solves this natively.

```typescript
import { getSenderLid, validateJid, toJid, normalizeJid } from '@GabWT333/based';

sock.ev.on('messages.upsert', ({ messages }) => {
  for (const msg of messages) {

    const info = getSenderLid(msg);
    //  info.jid       → '123@s.whatsapp.net'
    //  info.lid       → '123@lid'
    //  info.isValid   → true / false
    //  info.timestamp → Date.now()

    const { isValid, error } = validateJid(info.jid);
    if (!isValid) { console.warn('bad jid:', error); continue; }

    const normalized = toJid(info.lid); // always get a safe JID
    sock.sendMessage(normalized, { text: 'received ✓' });
  }
});
```

**Fields auto-injected into every event payload:**

| Field | Type | Description |
|---|---|---|
| `msg.key.remoteJid` | `string` | Standard JID (`@s.whatsapp.net`) |
| `msg.key.remoteLid` | `string?` | Raw LID when available |
| `msg.key.participantLid` | `string?` | Group sender LID |
| `msg.key.remoteJidNormalized` | `string` | Always-safe normalized JID |

---

## 🧠 Cache

```typescript
setPerformanceConfig({
  cache: {
    lidCache:           { ttl: 300_000, maxSize: 10000, cleanupInterval: 120_000 },
    jidCache:           { ttl: 300_000, maxSize: 10000, cleanupInterval: 120_000 },
    lidToJidCache:      { ttl: 300_000, maxSize: 5000,  cleanupInterval: 180_000 },
    groupMetadataCache: { ttl: 600_000, maxSize: 2000,  cleanupInterval: 300_000 }
  }
});

// monitor
const stats = getCacheStats();
console.log(`hit rate: ${stats.lidCache.hits / (stats.lidCache.hits + stats.lidCache.misses) * 100}%`);

// flush
clearCache();
```

<div align="center">

```
  lidCache ──────────── 80–90% fewer API calls
  jidCache ──────────── instant normalization
  lidToJidCache ──────── zero-latency conversion
  groupMetadataCache ─── group ops at warp speed
```

</div>

---

## 💬 Messages

<details>
<summary><b>📝 Text & Mentions</b></summary>

```typescript
await sock.sendMessage(jid, {
  text: '*bold* _italic_ ~strike~ `mono`',
  mentions: ['user@s.whatsapp.net']
});
```

</details>

<details>
<summary><b>🖼️ Media (image / video / audio / doc / album)</b></summary>

```typescript
// image
await sock.sendMessage(jid, { image: { url: './img.jpg' }, caption: 'caption' });

// video
await sock.sendMessage(jid, { video: { url: './clip.mp4' }, gifPlayback: false });

// voice note
await sock.sendMessage(jid, { audio: { url: './voice.mp3' }, mimetype: 'audio/mp4', ptt: true });

// document
await sock.sendMessage(jid, {
  document: { url: './file.pdf' },
  mimetype: 'application/pdf',
  fileName: 'doc.pdf'
});

// album (multi-image)
await sock.sendMessage(jid, { album: buffers.map(b => ({ image: b })) });
```

</details>

<details>
<summary><b>🔘 Buttons & Lists</b></summary>

```typescript
// quick reply buttons
await sock.sendMessage(jid, {
  text: 'choose:',
  footer: 'GabWT333',
  buttons: [
    { buttonId: 'a', buttonText: { displayText: 'Option A' }, type: 1 },
    { buttonId: 'b', buttonText: { displayText: 'Option B' }, type: 1 }
  ]
});

// list menu
await sock.sendMessage(jid, {
  text: 'menu', title: 'Pick one', buttonText: 'Open',
  sections: [{
    title: 'Options',
    rows: [
      { title: 'Item 1', rowId: 'i1', description: 'subtitle' },
      { title: 'Item 2', rowId: 'i2', description: 'subtitle' }
    ]
  }]
});
```

</details>

<details>
<summary><b>📊 Polls</b></summary>

```typescript
await sock.sendMessage(jid, {
  poll: {
    name: 'Best runtime?',
    values: ['Node', 'Deno', 'Bun'],
    selectableCount: 1
  }
});

// poll result snapshot
await sock.sendMessage(jid, {
  pollResultSnapshot: {
    name: 'Best runtime?',
    pollVotes: [
      { optionName: 'Node', optionVoteCount: 12 },
      { optionName: 'Deno', optionVoteCount: 4 }
    ],
    pollType: 0
  }
});
```

</details>

<details>
<summary><b>🎠 Carousel</b></summary>

```typescript
await sock.sendMessage(jid, {
  text: 'swipe →',
  cards: [
    {
      title: 'Card 1', body: 'description',
      image: { url: './img1.jpg' },
      buttons: [{ name: 'quick_reply', buttonParamsJson: '{"display_text":"Go"}' }]
    },
    { title: 'Card 2', body: 'description', video: { url: './clip.mp4' } }
  ],
  carouselCardType: 1 // 1 = horizontal scroll
});
```

</details>

<details>
<summary><b>🎨 Stickers & Sticker Packs</b></summary>

```typescript
// single sticker
await sock.sendMessage(jid, { sticker: { url: './s.webp' } });

// full sticker pack
await sock.sendMessage(jid, {
  stickerPack: {
    name: 'My Pack', publisher: 'GabWT333',
    stickers: [
      { sticker: { url: './s1.webp' }, emojis: ['🔥'] },
      { sticker: { url: './s2.webp' }, emojis: ['💙'], isAnimated: true }
    ]
  }
});
```

</details>

<details>
<summary><b>💸 Payments</b></summary>

```typescript
// request
await sock.sendMessage(jid, {
  requestPayment: { currency: 'EUR', amount1000: 5000, requestFrom: jid, note: 'invoice' }
});

// send payment
await sock.sendMessage(jid, {
  sendPayment: {
    requestMessageKey: { remoteJid: jid, fromMe: false, id: 'msgId' },
    noteMessage: { text: 'paid ✓' }
  }
});

// decline / cancel
await sock.sendMessage(jid, { declinePayment: { key: { remoteJid: jid, fromMe: false, id: 'msgId' } } });
await sock.sendMessage(jid, { cancelPayment:  { key: { remoteJid: jid, fromMe: true,  id: 'msgId' } } });
```

</details>

<details>
<summary><b>📌 Pin · 🤖 AI Rich · 🔐 Encrypted · 📅 Scheduled Call · and more</b></summary>

```typescript
// pin / unpin
await sock.sendMessage(jid, {
  pin: { key: { remoteJid: jid, fromMe: false, id: 'msgId' }, type: 1, time: 86400 }
});

// AI rich response
await sock.sendMessage(jid, {
  ai: true,
  richResponse: {
    messageType: 1,
    submessages: [
      { messageType: 2, messageText: 'Here is the answer' },
      { messageType: 5, codeMetadata: { codeLanguage: 'js', codeBlocks: [{ codeContent: 'console.log("hi")' }] } }
    ]
  }
});

// scheduled call
await sock.sendMessage(jid, {
  call: {
    callKey: { fromMe: true, id: Date.now().toString(), remoteJid: jid },
    type: 'OFFER',
    time: Date.now() + 3_600_000,
    title: 'Team sync'
  }
});

// status sticker on broadcast
await sock.sendMessage('status@broadcast',
  { sticker: { url: './s.webp' } },
  { statusJidList: [jid] }
);
```

</details>

---

## 👥 Groups

```typescript
// ── create ──────────────────────────────────────────────────────────
const g = await sock.groupCreate('My Group 🔵', ['user@s.whatsapp.net']);

// ── participants ─────────────────────────────────────────────────────
await sock.groupParticipantsUpdate(jid, [user], 'add');
await sock.groupParticipantsUpdate(jid, [user], 'remove');
await sock.groupParticipantsUpdate(jid, [user], 'promote');
await sock.groupParticipantsUpdate(jid, [user], 'demote');

// ── settings ─────────────────────────────────────────────────────────
await sock.groupUpdateSubject(jid, 'New name');
await sock.groupUpdateDescription(jid, 'New description');
await sock.groupSettingUpdate(jid, 'announcement'); // admin-only
await sock.groupToggleEphemeral(jid, 86400);        // 24h disappearing
await sock.groupJoinApprovalMode(jid, 'on');        // approval required
await sock.groupMemberAddMode(jid, 'admin_add');    // admin-only invites

// ── invite ───────────────────────────────────────────────────────────
const code = await sock.groupInviteCode(jid);
await sock.groupRevokeInvite(jid);
await sock.groupAcceptInvite(code);
await sock.groupLeave(jid);
```

---

## 📡 Events

| Event | Trigger |
|---|---|
| `connection.update` | Connection state changed |
| `creds.update` | Auth credentials rotated — **always save** |
| `messages.upsert` | New or updated message |
| `messages.update` | Delivery / read receipt |
| `group-participants.update` | Member join / leave / promote |

```typescript
// status tracking
sock.ev.on('messages.update', updates => {
  for (const { key, update } of updates) {
    const status = ['', 'sent', 'delivered', 'read'][update.status ?? 0];
    console.log(`[${key.id}] → ${status}`);
  }
});

// group events
sock.ev.on('group-participants.update', ({ id, action, participants }) => {
  console.log(`[${id}] ${action}:`, participants);
});
```

---

## 📖 API Reference

| Method | Returns | Description |
|---|---|---|
| `makeWASocket(cfg)` | `WASocket` | Create the socket |
| `useMultiFileAuthState(dir)` | `{ state, saveCreds }` | Persistent auth |
| `getSenderLid(msg)` | `SenderInfo` | Safe sender extraction |
| `validateJid(jid)` | `{ isValid, error }` | JID validation |
| `toJid(lid)` | `string` | LID → JID conversion |
| `normalizeJid(jid)` | `string` | Normalize any JID |
| `isValidJid(jid)` | `boolean` | Quick validity check |
| `getCacheStats()` | `CacheStats` | Hit/miss/size metrics |
| `clearCache()` | `void` | Flush all caches |
| `setPerformanceConfig(cfg)` | `void` | Tune perf settings |
| `getPerformanceConfig()` | `Config` | Read live config |

---

## 🔧 Troubleshooting

<details>
<summary><b>Connection keeps dropping</b></summary>

Use exponential backoff — set `maxRetries: 5` and `retryBackoffMultiplier: 1.5`. Never reconnect immediately on close.

</details>

<details>
<summary><b>Sender is undefined in group messages</b></summary>

Always use `getSenderLid(msg)` instead of reading `msg.key.participant` raw. The library patches this automatically in events but getSenderLid provides additional metadata.

</details>

<details>
<summary><b>Memory keeps growing</b></summary>

Reduce `maxSize` or `ttl` on cache entries. Call `clearCache()` periodically or on `connection.open`.

</details>

<details>
<summary><b>Account getting banned</b></summary>

Critical settings: `markOnlineOnConnect: false` + `syncFullHistory: false`. Lower `batchSize` to 30–50. Add natural delays between bulk sends.

</details>

---

## 🛡️ Security

| Layer | Tech |
|---|---|
| End-to-End Encryption | Signal Protocol |
| Key Management | Auto-generation & rotation |
| Authentication | QR code or pairing code |
| Storage | Encrypted local credentials |

---

## 🙏 Credits

| Project | Contribution |
|---|---|
| [Baileys](https://github.com/WhiskeySockets/Baileys) | Core WhatsApp Web engine |
| [Yupra](https://www.npmjs.com/package/@yupra/baileys) | LID/JID resolution research |
| [Signal Protocol](https://signal.org/) | E2E encryption layer |

---

## ⚠️ Disclaimer

> Not affiliated with WhatsApp Inc. or Meta Platforms.
> For educational and personal development use only.
> Use responsibly — abuse may result in account termination.

**MIT License © 2025 [GabWT333](https://github.com/GabWT333)**

---

<div align="center">

<br>

<!-- FOOTER BANNER -->
<img src="https://capsule-render.vercel.app/api?type=shark&color=0:1e90ff,100:020b18&height=100&section=footer&reversal=false" width="100%"/>

<br>

<img src="https://qu.ax/FNs6D" width="64" height="64" style="border-radius:50%"/>

<br>

<img src="https://readme-typing-svg.herokuapp.com?font=JetBrains+Mono&size=14&duration=4000&pause=2000&color=00BFFF&center=true&vCenter=true&width=400&lines=built+with+%F0%9F%92%99+by+GabWT333;github.com%2FGabWT333%2Fbased" alt="footer typing"/>

<br>

[![GitHub](https://img.shields.io/badge/GitHub-GabWT333%2Fbased-00bfff?style=for-the-badge&logo=github&logoColor=white&labelColor=020b18)](https://github.com/GabWT333/based)
&nbsp;
[![NPM](https://img.shields.io/badge/NPM-@GabWT333%2Fbased-1e90ff?style=for-the-badge&logo=npm&logoColor=white&labelColor=020b18)](https://www.npmjs.com/package/@GabWT333/based)
&nbsp;
[![Donate](https://img.shields.io/badge/DONATE-PayPal-00bfff?style=for-the-badge&logo=paypal&logoColor=white&labelColor=020b18)](https://paypal.me/GabWT333)

<br><br>

</div>
