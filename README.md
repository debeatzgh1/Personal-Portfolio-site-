# 💎 Digital Ecosystem
**Professional Software Development & AI Content Strategy Hub**

This repository serves as the central command center for the eg **DeBeatzGH** brand. It features a high-end, responsive UI built with Tailwind CSS and a custom JavaScript-driven overlay system.

## 🚀 Key Features
- **Master Overlay Engine:** Opens external tools (WordPress Signup, Firebase Distribution, Sales Shop) in a professional, branded iframe without leaving the site.
- **Glassmorphic Bento Grid:** A modern layout showcasing services like Custom Dev, AI Automation, and Passive Income Blueprints.
- **Smart Dock:** A persistent floating navigation bar for mobile-first user engagement.
- **Session Intelligence:** Uses `localStorage` to ensure popups are non-intrusive and respect the user's journey.

## 🛠️ Tech Stack
- **Styling:** [Tailwind CSS](https://tailwindcss.com/)
- **Icons:** [FontAwesome](https://fontawesome.com/)
- **Typography:** Plus Jakarta Sans & JetBrains Mono
- **Deployment:** GitHub Pages

## 📂 Architecture
- `index.html`: Main landing page and UI logic.
- `Blogger-sign-up-button/`: Repository for WordPress subdomain claims.
- `Side-hustle-starter-kit/`: Educational resource distribution.

## ⚖️ Legal & Privacy
This site uses local storage to optimize performance and track session status for authentication popups. No personal data is stored outside the user's browser.

---
© 2026  Engineered for Performance.





<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DeBeatzGH | Software Developer & AI Strategist</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;700;800&family=JetBrains+Mono&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #030712; color: #f0f6fc; overflow-x: hidden; }
        .glass { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(12px); border: 1px solid rgba(255, 255, 255, 0.1); }
        .text-gradient { background: linear-gradient(to right, #22d3ee, #818cf8); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        
        /* Master Overlay Styles */
        #master-overlay {
            position: fixed; inset: 0; background: rgba(0,0,0,0.95);
            backdrop-filter: blur(15px); z-index: 50000; display: none; flex-direction: column;
        }
        .loader-bar { height: 2px; width: 0%; background: #00f2ff; position: absolute; top: 0; transition: width 0.5s; }
    </style>
</head>
<body>

    <div class="fixed inset-0 -z-10">
        <div class="absolute top-0 left-0 w-full h-full bg-[radial-gradient(circle_at_50%_50%,rgba(6,182,212,0.1),transparent_50%)]"></div>
    </div>

    <nav class="flex justify-between items-center px-8 py-6 max-w-7xl mx-auto">
        <div class="text-xl font-black tracking-tighter uppercase">DeBeatz<span class="text-cyan-400">GH</span></div>
        <div class="hidden md:flex space-x-8 text-[10px] font-bold uppercase tracking-widest text-slate-500">
            <a href="https://beatzde4.blogspot.com" class="hover:text-cyan-400 transition">Blog</a>
            <a href="https://debeatzgh1.github.io/sales" class="hover:text-cyan-400 transition">Shop</a>
            <a href="#services" class="hover:text-cyan-400 transition">Services</a>
        </div>
        <button onclick="openGlobalOverlay('https://debeatzgh1.github.io/Blogger-sign-up-button-/')" class="glass px-6 py-2 rounded-xl text-[10px] font-black uppercase tracking-widest border-cyan-500/30 hover:bg-cyan-500/10 transition">Sign Up</button>
    </nav>

    <header class="max-w-4xl mx-auto text-center pt-20 pb-20 px-6">
        <h1 class="text-5xl md:text-7xl font-extrabold mb-6 tracking-tighter leading-tight">
            The AI Prompt <span class="text-gradient">Advantage</span>
        </h1>
        <p class="text-slate-400 mb-10 max-w-xl mx-auto text-sm leading-relaxed">
            Professional software development and AI-driven content strategies for the modern digital ecosystem.
        </p>
        <div class="flex flex-wrap justify-center gap-4">
            <button onclick="openGlobalOverlay('https://debeatzgh1.github.io/Side-hustle-starter-kit-/')" class="bg-cyan-500 text-slate-950 px-8 py-4 rounded-2xl font-black uppercase text-[10px] tracking-widest hover:scale-105 transition">Start Hustle Kit</button>
            <button onclick="openGlobalOverlay('https://appdistribution.firebase.dev/i/dc2da2d4d3766b8a')" class="glass px-8 py-4 rounded-2xl font-black uppercase text-[10px] tracking-widest hover:bg-white/5 transition">Get App Build</button>
        </div>
    </header>

    <section id="services" class="max-w-6xl mx-auto px-6 pb-40 grid grid-cols-1 md:grid-cols-3 gap-6">
        <div class="md:col-span-2 glass p-10 rounded-[2.5rem] flex flex-col justify-end min-h-[300px]">
            <h3 class="text-2xl font-bold mb-2">Custom Software Dev</h3>
            <p class="text-slate-500 text-xs">Responsive UI components, Tailwind CSS integration, and cross-platform mobile app development.</p>
        </div>
        <div class="glass p-10 rounded-[2.5rem] border-cyan-500/20">
            <i class="fas fa-robot text-cyan-400 text-2xl mb-6"></i>
            <h3 class="text-lg font-bold mb-2 uppercase italic">AI Automation</h3>
            <p class="text-slate-500 text-[11px]">Streamlining content workflows with high-level prompt engineering.</p>
        </div>
    </section>

    <div class="fixed bottom-6 left-1/2 -translate-x-1/2 w-[90%] max-w-md glass p-3 rounded-2xl flex items-center justify-between z-40">
        <div class="flex items-center gap-3">
            <div class="w-2 h-2 bg-green-500 rounded-full animate-ping"></div>
            <span class="text-[10px] font-bold uppercase tracking-tighter text-slate-400">Network Active</span>
        </div>
        <button onclick="openGlobalOverlay('https://debeatzgh1.github.io/me-/')" class="bg-white text-black px-4 py-2 rounded-xl text-[9px] font-black uppercase">Open Hub</button>
    </div>

    <div id="master-overlay">
        <div class="loader-bar" id="load-bar"></div>
        <div class="p-4 border-b border-white/5 flex justify-between items-center">
            <span class="text-[9px] font-mono text-cyan-500 uppercase">Secure_Portal // DeBeatzGH_Link</span>
            <button id="close-btn" class="bg-red-500/10 text-red-500 px-4 py-1.5 rounded-lg text-[10px] font-bold uppercase disabled:opacity-50">Wait (3s)</button>
        </div>
        <iframe id="master-frame" class="w-full flex-grow" src=""></iframe>
    </div>

    <script>
        const overlay = document.getElementById('master-overlay');
        const frame = document.getElementById('master-frame');
        const closeBtn = document.getElementById('close-btn');
        const loadBar = document.getElementById('load-bar');

        function openGlobalOverlay(url) {
            frame.src = url;
            overlay.style.display = 'flex';
            loadBar.style.width = '100%';
            
            let sec = 3;
            closeBtn.disabled = true;
            closeBtn.innerText = `Wait (${sec}s)`;
            
            const timer = setInterval(() => {
                sec--;
                if(sec > 0) { closeBtn.innerText = `Wait (${sec}s)`; }
                else {
                    clearInterval(timer);
                    closeBtn.disabled = false;
                    closeBtn.innerText = "Close [X]";
                    closeBtn.onclick = () => { overlay.style.display = 'none'; frame.src = ''; loadBar.style.width = '0%'; };
                }
            }, 1000);
        }

        // Auto-popup logic for new visitors
        window.addEventListener('load', () => {
            if(!localStorage.getItem('popup_seen')) {
                setTimeout(() => {
                    openGlobalOverlay('https://debeatzgh1.github.io/Blogger-sign-up-button-/');
                    localStorage.setItem('popup_seen', 'true');
                }, 5000);
            }
        });
    </script>
</body>
</html>
