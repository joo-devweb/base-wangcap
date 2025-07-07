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
// plugins/hello.js
export default {
  commands: ["hello"],
  tags: ["fun"],
  help: ["hello"],
  execute: async (sock, msg) => {
    await sock.sendMessage(msg.key.remoteJid, { text: "👋 Hello, world!" });
  }
};
```

### Switch-case Style
```js
// plugins/caseExample.js
export default function caseExample(sock,msg,args) {
  switch(args[0]) {
    case "hi":
      sock.sendMessage(msg.key.remoteJid,{text:"Hi there!"});
      break;
  }
}
caseExample.commands = ["hi"];
caseExample.tags = ["fun"];
caseExample.help = ["hi"];
```

### CommonJS Style
```js
// plugins/cjsExample.js
module.exports = {
  command: "joke",
  tags: ["fun"],
  help: ["joke"],
  handler: async (sock,msg) => {
    await sock.sendMessage(msg.key.remoteJid,{text:"😂 Why did the bot cross the road? To reconnect on the other side!"});
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

---

## 📜 License

MIT © Nathan.Astralune  

---

> “Keep calm and automate everything!” 🚀  
> ![Laughing Emoji](https://media.giphy.com/media/5GoVLqeAOo6PK/giphy.gif)
