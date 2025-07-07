<p align="center">
  <img src="https://media.giphy.com/media/ICOgUNjpvO0PC/giphy.gif" width="200" alt="Coding Cat" />
  <br/>
  <strong>Welcome to the WhatsApp Bot Base!</strong>  
  <em>(Brought to you by ✨Nathan.Astralune✨)</em>
</p>

---

## 📖 Table of Contents

1. [About](#about)  
2. [Features](#features)  
3. [Requirements](#requirements)  
4. [Installation](#installation)  
5. [Usage](#usage)  
6. [Plugin Development](#plugin-development)  
7. [Menu System](#menu-system)  
8. [Contributing](#contributing)  
9. [License](#license)  

---

## 🤖 About

This is a **modular**, **plugin-based** WhatsApp bot skeleton written in modern ESM/CJS style, ready for **Node.js v17–22**, PM2, Heroku, Replit, and more.  
It features:

- 🔌 **Auto-reconnect** on disconnect  
- 🛡️ **Pairing-code** authentication (no QR!)  
- 🖥️ **Fake browser** fingerprint (`Chrome`, `Ubuntu`, `24.0.0`)  
- 📦 **Plugin loader** supporting ESM, CJS & switch-case styles  
- 📊 **System info** and **menu** plugins out of the box  
- 🚀 **PM2** setup for production  
- 😂 And—of course—a dash of humor!  

<p align="center">
  <img src="https://media.giphy.com/media/LHZyixOnHwDDy/giphy.gif" width="300" alt="Happy Bot" />
</p>

---

## ✨ Features

- **ESM Plugins**: `export default { commands, tags, help, execute }`  
- **CJS Plugins**: `module.exports = { command, tags, handler }`  
- **Switch-case Plugins**: function with `.commands = […]`  
- **System Report**: `.ping` or `.os` gives a “Kali Linux Style” report  
- **Dynamic Menu**: `.menu` groups plugins by `tags`  
- **Scraper Utility**: built-in `scraper/scraper.js` (Axios + Cheerio)  

---

## 🛠️ Requirements

- Node.js **17 ≤ v22**  
- npm or yarn  
- PM2 (optional, for production)  

---

## 🚀 Installation

```bash
git clone https://github.com/yourusername/whatsapp-bot-base.git
cd whatsapp-bot-base
npm install
```

To run in development (auto-reload):  
```bash
npm run dev
```

To run with PM2 in production:  
```bash
npm run start:pm2
```

---

## 💡 Usage

1. **Set your config**  
   Edit `config.js` and set:
   ```js
   export default {
     ownerNumber: "6281234567890@s.whatsapp.net",
     prefix: ".",
     authFolder: "./auth_info",
     browser: ["Chrome","Ubuntu","24.0.0"]
   };
   ```

2. **Start the bot**  
   ```bash
   npm start
   ```
   You’ll be prompted:
   ```
   ➤ Nomor WA (e.g. 6281234xxxx):
   ```
   Enter your WhatsApp number (without `@s.whatsapp.net`).  
   Wait 3 seconds, then copy the pairing code into your WhatsApp session.

3. **Interact!**  
   - `.ping` or `.os` → system report  
   - `.menu` → show all commands  

---

## 🧩 Plugin Development

Plugins live in the `plugins/` directory:

### ESM Style
```js
// plugins/sysinfo.js
import os from 'node:os';
import process from 'node:process';
import dns from 'node:dns/promises';

const fmtMs = ms => {
  const s = Math.floor((ms / 1000) % 60).toString().padStart(2,'0');
  const m = Math.floor((ms / 60000) % 60).toString().padStart(2,'0');
  const h = Math.floor(ms / 3600000).toString().padStart(2,'0');
  return `${h}h ${m}m ${s}s`;
};
const MB = b => (b/1048576).toFixed(2) + ' MB';
const line = '─'.repeat(42);

async function handler(sock, msg) {
  const t0 = Date.now();
  const wait = await sock.sendMessage(msg.key.remoteJid, { text: '┌─[ Getting system data... ]' });
  const latency = Date.now() - t0;

  const cpuInfo = os.cpus()[0];
  const cpuLoad = os.loadavg().map(n => n.toFixed(2)).join('  ');
  const coreCnt = os.cpus().length;
  const totalMem = MB(os.totalmem());
  const freeMem = MB(os.freemem());
  const usedMem = MB(os.totalmem() - os.freemem());
  const procUp = fmtMs(process.uptime() * 1000);
  const machUp = fmtMs(os.uptime() * 1000);

  let ip4 = 'N/A';
  for (const list of Object.values(os.networkInterfaces())) {
    const v4 = list.find(x => x.family==='IPv4' && !x.internal);
    if (v4) { ip4 = v4.address; break; }
  }
  const pubIP = await dns.lookupService('8.8.8.8', 53)
                       .then(r => r.address).catch(_=>'N/A');

  const txt = `
\`\`\`bash
${line}
| KALI SYSTEM REPORT 🔰
${line}
 Latency   : ${latency} ms
 Proc Uptime: ${procUp}
 Mach Uptime: ${machUp}

 CPU: ${cpuInfo.model.trim()} | ${coreCnt} cores | ${cpuInfo.speed}MHz | Load: ${cpuLoad}
 Mem: ${usedMem} / ${totalMem} Free: ${freeMem}

 Hostname : ${os.hostname()}
 OS       : ${os.platform()} ${os.release()} (${os.arch()})
 Node.js  : ${process.version}
 LAN IP   : ${ip4}
 WAN IP   : ${pubIP}
${line}
\`\`\`
`.trim();

  await sock.sendMessage(msg.key.remoteJid, { text: txt }, { quoted: wait });
}

handler.commands = ['ping','os'];
handler.tags     = ['esm'];
handler.help     = ['ping','os'];

export default handler;
```

### Switch-case Style
```js
// plugins/caseSysinfo.js
import os from 'node:os';
import { performance } from 'node:perf_hooks';

export default function caseSysinfo(sock, msg, args) {
  switch (args[0]?.toLowerCase()) {
    case 'ping': {
      const start = performance.now();
      const used = process.memoryUsage();
      const lat = (performance.now() - start).toFixed(2);
      const txt = `Pong! Latency: ${lat} ms\nRAM: ${(used.rss/1048576).toFixed(2)} MB`;
      sock.sendMessage(msg.key.remoteJid, { text: txt });
      break;
    }
    default:
      break;
  }
}

caseSysinfo.commands = ['ping'];
caseSysinfo.tags     = ['case'];
caseSysinfo.help     = ['ping'];
```

### CommonJS Style
```js
// plugins/cjsSysinfo.cjs
const os = require('node:os');

module.exports = {
  command: 'ping',
  tags: ['cjs'],
  help: ['ping'],

  handler: async function(sock, msg) {
    const used = process.memoryUsage();
    const txt = `Pong CJS!\nRAM: ${(used.heapUsed/1048576).toFixed(2)} MB`;
    await sock.sendMessage(msg.key.remoteJid, { text: txt });
  }
};
```

---

## 📋 Menu System

The **`.menu`** command auto-groups plugins by their `tags`.  
Try it:

```text
Menu ESM
 • ping
 • os

Menu CASE
 • ping

Menu CJS
 • ping

Menu SYSTEM
 • menu
```

---

## 🛠 Contributing

1. Fork this repo 👍  
2. Create your plugin in `plugins/`  
3. Add tests (optional)  
4. Submit a PR and make us laugh with your code! 😂  


## Download:
## https://www.mediafire.com/file/2g1up7yhbgtwnl6/Base+Nathan.zip/file
---

## 📜 License

MIT © Nathan.Astralune  

---

> “Keep calm and automate everything!” 🚀  
> ![Laughing Emoji](https://media.giphy.com/media/5GoVLqeAOo6PK/giphy.gif)
 

 
 Contact & Socials
[![WhatsApp Icon](https://img.icons8.com/color/48/000000/whatsapp--v1.png)](https://wa.me/0895416602000)  [![Instagram Icon](https://img.icons8.com/fluent/48/000000/instagram-new.png)](https://instagram.com/chrisjoo_uww)
