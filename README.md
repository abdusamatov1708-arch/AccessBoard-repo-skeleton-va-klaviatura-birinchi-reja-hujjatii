# AccessBoard-repo-skeleton-va-klaviatura-birinchi-reja-hujjatii
Plaintext
accessboard/
├── frontend/
│   ├── index.html
│   ├── style.css
│   └── script.js
├── server/
│   ├── server.js
│   └── package.json
├── .gitignore
└── README.md

# Node modules
node_modules/

# Environment variables
.env

# OS specific files
.DS_Store
Thumbs.db

{
  "name": "accessboard-server",
  "version": "1.0.0",
  "description": "AccessBoard WebSocket relay server",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.19.2",
    "ws": "^8.16.0"
  }
}

const express = require('express');
const http = require('http');
const WebSocket = require('ws');
const path = require('path');

const app = express();
const server = http.createServer(app);
const wss = new WebSocket.Server({ server });

// Frontend statik fayllarini ulash
app.use(express.static(path.join(__dirname, '../frontend')));

// WebSocket ulanishlarini boshqarish (Relay server)
wss.on('connection', (ws) => {
    console.log('🟢 Yangi mijoz ulandi.');

    ws.on('message', (message) => {
        // Xabarni o'zidan boshqa barcha ulangan klientlarga tarqatish
        wss.clients.forEach((client) => {
            if (client !== ws && client.readyState === WebSocket.OPEN) {
                client.send(message);
            }
        });
    });

    ws.on('close', () => {
        console.log('🔴 Mijoz uzildi.');
    });
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
    console.log(`🚀 Server ishga tushdi: http://localhost:${PORT}`);
});

# 🚀 AccessBoard

**AccessBoard** — bu cheklangan imkoniyatlarga ega foydalanuvchilar va umumiy auditoriya uchun mo'ljallangan, to'liq **klaviatura-birinchi (keyboard-first)** tamoyili asosida ishlaydigan interaktiv doska (whiteboard) ilovasi. Real vaqt rejimida WebSocket orqali ma'lumot almashadi.

---

## 🛠 Texnologiyalar To'plami
- **Frontend:** HTML5, CSS3, Vanilla JavaScript
- **Backend:** Node.js, Express.js
- **Real-time:** WebSocket (`ws` kutubxonasi)
- **Versiyalarni boshqarish:** Git, GitHub

---

## 📑 Semantik Hujjat Sxemasi (Semantic Document Schema)
Ilovaning HTML tuzilishi quyidagi semantik teglarga asoslangan bo'lib, ekran o'quvchilari (screen readers) va maxsus yordamchi vositalar uchun optimallashtirilgan:

```html
<body>
  <header role="banner">
    <h1>AccessBoard</h1>
    <nav role="navigation" aria-label="Asosiy menyu">...</nav>
  </header>
  <main role="main">
    <section aria-labelledby="board-title">
      <h2 id="board-title">Ishchi doska</h2>
      <div id="canvas-container" tabindex="0">...</div>
    </section>
    <aside aria-label="Asboblar paneli">...</aside>
  </main>
  <footer role="contentinfo">...</footer>
</body>

AccessBoard sichqoncha (mouse) ishlatmasdan, faqat klaviatura yordamida to'liq boshqarilishini ta'minlaydi:Amal / FunksiyaKlaviatura Qisqartmasi (Shortcut)TavsifFokusni boshqarishTab / Shift + TabInteraktiv elementlar bo'ylab oldinga/orqaga harakatlanishChizma / Element qo'shishEnter yoki SpaceTanlangan asbobni faollashtirish yoki nuqta qo'yishQalam rejimi (Pen)Alt + PErkin chizish rejimiga o'tishMatn qo'shish (Text)Alt + TDoskaga matn kiritish oynasini ochishO'chirgich (Eraser)Alt + EElementlarni o'chirish rejimini faollashtirishOrqaga qaytarishCtrl + ZOxirgi amalni bekor qilish (Undo)Oldinga o'tishCtrl + YBekor qilingan amalni qaytarish (Redo)Yordam oynasiF1Barcha klaviatura buyruqlari ro'yxatini ochish

AccessBoard sichqoncha (mouse) ishlatmasdan, faqat klaviatura yordamida to'liq boshqarilishini ta'minlaydi:Amal / FunksiyaKlaviatura Qisqartmasi (Shortcut)TavsifFokusni boshqarishTab / Shift + TabInteraktiv elementlar bo'ylab oldinga/orqaga harakatlanishChizma / Element qo'shishEnter yoki SpaceTanlangan asbobni faollashtirish yoki nuqta qo'yishQalam rejimi (Pen)Alt + PErkin chizish rejimiga o'tishMatn qo'shish (Text)Alt + TDoskaga matn kiritish oynasini ochishO'chirgich (Eraser)Alt + EElementlarni o'chirish rejimini faollashtirishOrqaga qaytarishCtrl + ZOxirgi amalni bekor qilish (Undo)Oldinga o'tishCtrl + YBekor qilingan amalni qaytarish (Redo)Yordam oynasiF1Barcha klaviatura buyruqlari ro'yxatini ochish
