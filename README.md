WhatsApp Bot Base
by Nathan.Astralune

![Coding Cat](https://media.giphy.com/media/ICOgUNjpvO0PC/giphy.gif)
![Happy Bot](https://media.giphy.com/media/LHZyixOnHwDDy/giphy.gif)

Table of Contents
1. About
2. Features
3. Requirements
4. Installation & Run
5. Configuration
6. Authentication & Pairing Code
7. Plugin Styles & Examples
8. Menu System
9. Logging & LID Support
10. Contact & Socials
11. Contributing
12. License

1. About
![Laughing](https://media.giphy.com/media/5GoVLqeAOo6PK/giphy.gif)
This is a lightweight, modular, plugin-based WhatsApp bot skeleton optimized for long-term use and thousand+ features.
Runs on Node.js v17–22, supports PM2, Heroku, Replit, with humor, animations, and GIFs!

2. Features
![Rocket](https://media.giphy.com/media/26FPJGjhefSJuaRhu/giphy.gif)
- Modular Plugins: drop-in .js or .cjs in /plugins/
- Three Code Styles:
  * ESM (import/export)
  * Switch–Case
  * CJS (module.exports)
- Auto-Reconnect on disconnect
- Pairing Code Flow (no QR by default; optional QR)
- Dynamic Menu grouping by tags
- Full Logging: Name, Message, Type, Scope, Time, LID
- Scraper Utility: Axios + Cheerio
- PM2 ready (ecosystem.config.js)
- Animations & Emojis for fun

3. Requirements
![Wait 3s](https://media.giphy.com/media/xUOrw5LIJ4fB2A4IVK/giphy.gif)
- Node.js >=17 <=22
- npm or yarn
- PM2 (optional)
- Internet for pairing

4. Installation & Run
- git clone https://github.com/yourusername/whatsapp-bot-base.git
- cd whatsapp-bot-base
- npm install
- npm run dev        (for development)
- npm start          (production)
- npm run start:pm2  (via PM2)

5. Configuration
- Edit config.js:
  export default {
    ownerNumber: "6281234567890@s.whatsapp.net",
    prefix: ".",
    authFolder: "./auth_info",
    databaseFile: "./database.json",
    browser: ["Chrome","Ubuntu","24.0.0"]
  };

6. Authentication & Pairing Code
![Logging](https://media.giphy.com/media/3oEjI6SIIHBdRxXI40/giphy.gif)
1. On first run, bot prompts number
2. Enter country-code number (e.g. 6281234xxxx)
3. Wait 3s, pairing code appears
4. Paste code in WhatsApp > Linked Devices > Use pairing code
5. Bot reconnects and saves session

7. Plugin Styles & Examples
7.1 ESM Style (hello.js)
export default { commands: ["hello"], tags: ["fun"], help: ["hello"], execute: async(sock,msg)=>{ await sock.sendMessage(msg.key.remoteJid,{ text: "Hello!" }); } };

7.2 Switch–Case Style (casePing.js)
export default function casePing(sock,msg,args){ switch(args[0]){ case "ping": sock.sendMessage(msg.key.remoteJid,{ text:"Pong!" }); break; } }
casePing.commands=["ping"]; casePing.tags=["case"]; casePing.help=["ping"];

7.3 CJS Style (cjsJoke.cjs)
module.exports={ command:"joke", tags:["fun","cjs"], help:["joke"], handler: async(sock,msg)=>{ await sock.sendMessage(msg.key.remoteJid,{ text:"Joke!" }); } };

8. Menu System
Use .menu to see grouped features by tags.

9. Logging & LID Support
Logs Name, Message, Type, Scope, Time, LID for each incoming message.

10. Contact & Socials
[![WhatsApp Icon](https://img.icons8.com/color/48/000000/whatsapp--v1.png)](https://wa.me/0895416602000)  [![Instagram Icon](https://img.icons8.com/fluent/48/000000/instagram-new.png)](https://instagram.com/chrisjoo_uww)

11. Contributing
Fork, add plugins, submit PR with emojis!

12. License
MIT © Nathan.Astralune
