
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>G-Dev Portfolio | Hiring Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Google+Sans:wght@400;500;700&display=swap');

        :root {
            --g-blue: #8ab4f8;
            --g-green: #34A853;
            --dark-bg: #1a1c1e;
            --dark-card: #2d2f31;
            --glass-border: rgba(255, 255, 255, 0.08);
        }

        body { font-family: 'Google Sans', sans-serif; background: #121212; margin: 0; }

        /* 1. LAUNCHER */
        #gdev-launcher {
            position: fixed; left: 20px; top: 50%; transform: translateY(-50%);
            display: flex; align-items: center; gap: 12px;
            background: var(--dark-card); padding: 8px 18px 8px 8px;
            border-radius: 40px; cursor: pointer; z-index: 9999;
            box-shadow: 0 10px 40px rgba(0,0,0,0.5);
            transition: 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            border: 1px solid var(--glass-border);
        }

        #gdev-launcher:hover { transform: translateY(-50%) scale(1.08) translateX(5px); border-color: var(--g-blue); }

        .dev-avatar {
            width: 42px; height: 42px; background: #121212; border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            border: 2px solid var(--g-blue); color: var(--g-blue); position: relative;
        }

        .status-dot {
            position: absolute; bottom: 2px; right: 2px; width: 10px; height: 10px;
            background: var(--g-green); border-radius: 50%; border: 2px solid var(--dark-card);
        }

        .status-glow {
            position: absolute; inset: 0; background: var(--g-green);
            border-radius: 50%; animation: status-pulse 2s infinite;
        }

        @keyframes status-pulse { 0% { transform: scale(1); opacity: 0.8; } 100% { transform: scale(2.5); opacity: 0; } }

        /* 2. OVERLAY MODAL */
        #gdev-overlay {
            position: fixed; inset: 0; background: rgba(0,0,0,0.85);
            backdrop-filter: blur(12px); display: none; z-index: 10000;
            justify-content: center; align-items: center; padding: 20px;
            opacity: 0; transition: opacity 0.4s ease;
        }

        #gdev-overlay.active { display: flex; opacity: 1; }

        .gdev-modal {
            width: 100%; max-width: 950px; height: 92vh;
            background: var(--dark-bg); border-radius: 28px;
            display: flex; flex-direction: column; overflow: hidden;
            border: 1px solid var(--glass-border); position: relative;
        }

        /* 3. IFRAME & FOOTER */
        #gdev-frame { flex-grow: 1; width: 100%; border: none; background: #fff; }

        .close-gdev {
            position: absolute; top: 20px; right: 25px; width: 40px; height: 30px;
            background: var(--dark-card); border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            cursor: pointer; color: #fff; z-index: 20;
        }

        /* CONTACT ME BUTTON STYLE */
        .hire-btn {
            background: var(--g-green);
            color: white;
            padding: 8px 18px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 800;
            text-transform: uppercase;
            letter-spacing: 1px;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: 0.3s;
            box-shadow: 0 4px 15px rgba(52, 168, 83, 0.3);
        }
        .hire-btn:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(52, 168, 83, 0.4); background: #2e964a; }

        @media (max-width: 768px) {
            .gdev-modal { height: 100vh; border-radius: 0; }
            .footer-controls { flex-direction: column; gap: 10px; padding: 15px; }
        }
    </style>
</head>
<body>

    <div id="gdev-launcher" onclick="toggleGDev()">
        <div class="dev-avatar">
            <i class="fab fa-google"></i>
            <div class="status-dot"><div class="status-glow"></div></div>
        </div>
        <div class="hidden sm:block">
            <div class="flex items-center gap-2">
                <span class="text-[9px] text-[#34A853] font-bold uppercase tracking-widest">Active</span>
            </div>
            <p class="text-[14px] font-medium text-gray-200">@debeatzgh</p>
        </div>
    </div>

    <div id="gdev-overlay">
        <div class="gdev-modal">
            <div class="close-gdev" onclick="toggleGDev()"><i class="fas fa-times"></i></div>
            
            <iframe id="gdev-frame" src=""></iframe>

            <div class="footer-controls p-4 bg-[#1a1c1e] border-t border-white/5 flex justify-between items-center px-8">
                <span class="text-[9px] text-gray-600 font-bold uppercase tracking-[2px]">G-Dev Protocol v4.0</span>
                
                <div class="flex items-center gap-3">
                    <button id="copy-btn" class="text-[11px] text-[#8ab4f8] font-bold px-4 py-2 hover:text-white transition" onclick="copyProfileLink()">
                        Copy Profile
                    </button>
                    <a href="https://wa.me/233549757544?text=Hi%20DeBeatz,%20I%20saw%20your%20Google%20Dev%20profile%20and%20would%20like%20to%20discuss%20a%20project." 
                       target="_blank" class="hire-btn">
                        <i class="fas fa-briefcase"></i> Contact Me
                    </a>
                </div>
            </div>
        </div>
    </div>

    <script>
        const overlay = document.getElementById('gdev-overlay');
        const frame = document.getElementById('gdev-frame');
        const profileUrl = "https://tally.so/r/3jkE29";

        function toggleGDev() {
            if (overlay.style.display === 'flex') {
                overlay.classList.remove('active');
                setTimeout(() => { overlay.style.display = 'none'; frame.src = ""; }, 400);
                document.body.style.overflow = 'auto';
            } else {
                overlay.style.display = 'flex';
                setTimeout(() => overlay.classList.add('active'), 10);
                frame.src = profileUrl;
                document.body.style.overflow = 'hidden';
            }
        }

        async function copyProfileLink() {
            await navigator.clipboard.writeText(profileUrl);
            const btn = document.getElementById('copy-btn');
            btn.innerText = "Copied!";
            setTimeout(() => { btn.innerText = "Copy Profile"; }, 2000);
        }

        overlay.onclick = (e) => { if (e.target === overlay) toggleGDev(); };
    </script>
</body>
</html>

 
 
 
 
 <nav id="slim-nav" class="slim-nav-container">
    <div class="nav-left">
        <button class="theme-toggle" onclick="toggleTheme()" title="Switch Theme">
            <i id="theme-icon" class="fas fa-moon"></i>
        </button>
        <div class="nav-divider"></div>
        <span class="nav-inscription" onclick="openForm()">
            Browse modern UI layout and pages <i class="fas fa-external-link-alt ml-1 opacity-40"></i>
        </span>
    </div>

    <div class="nav-right">
        <button onclick="scrollToTop()" class="nav-ctrl" title="Scroll to Top">
            <i class="fas fa-chevron-up"></i>
        </button>
        <button onclick="scrollToBottom()" class="nav-ctrl" title="Scroll to Bottom">
            <i class="fas fa-chevron-down"></i>
        </button>
    </div>
</nav>

<style>
    /* 1. THEME & TRANSITION LOGIC */
    :root {
        --nav-bg: rgba(10, 10, 12, 0.85);
        --nav-text: #f0f6fc;
        --nav-accent: #00f2ff;
        --nav-border: rgba(0, 242, 255, 0.15);
        --page-bg: #030712;
    }

    body[data-theme="light"] {
        --nav-bg: rgba(255, 255, 255, 0.9);
        --nav-text: #0f172a;
        --nav-accent: #2563eb;
        --nav-border: rgba(0, 0, 0, 0.08);
        --page-bg: #f8fafc;
    }

    body {
        background-color: var(--page-bg);
        transition: background-color 0.4s ease, color 0.4s ease;
    }

    /* 2. STICKY-HIDE ANIMATION */
    .slim-nav-container {
        position: fixed;
        top: 15px;
        left: 50%;
        transform: translateX(-50%);
        width: 92%;
        max-width: 750px;
        height: 44px;
        background: var(--nav-bg);
        backdrop-filter: blur(15px);
        -webkit-backdrop-filter: blur(15px);
        border: 1px solid var(--nav-border);
        border-radius: 14px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 0 12px;
        z-index: 10000;
        box-shadow: 0 10px 30px -10px rgba(0, 0, 0, 0.5);
        transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1), 
                    top 0.4s ease, 
                    background 0.3s ease;
    }

    /* The 'Hidden' State */
    .nav-up {
        transform: translateX(-50%) translateY(-100px);
    }

    /* 3. COMPONENT STYLING */
    .nav-left, .nav-right { display: flex; align-items: center; gap: 8px; }

    .nav-inscription {
        font-size: 11px;
        font-weight: 800;
        letter-spacing: 0.3px;
        color: var(--nav-text);
        cursor: pointer;
        transition: 0.2s;
        text-transform: uppercase;
    }

    .nav-inscription:hover { color: var(--nav-accent); }

    .nav-divider { width: 1px; height: 16px; background: var(--nav-border); margin: 0 4px; }

    .theme-toggle, .nav-ctrl {
        width: 32px; height: 32px;
        border-radius: 10px;
        display: flex; align-items: center; justify-content: center;
        cursor: pointer; color: var(--nav-text);
        transition: 0.2s; border: none; background: transparent;
    }

    .theme-toggle:hover, .nav-ctrl:hover {
        background: rgba(255, 255, 255, 0.08);
        color: var(--nav-accent);
    }

    body[data-theme="light"] .theme-toggle:hover,
    body[data-theme="light"] .nav-ctrl:hover {
        background: rgba(0, 0, 0, 0.05);
    }

    @media (max-width: 480px) {
        .nav-inscription { font-size: 9px; max-width: 180px; }
        .slim-nav-container { width: 96%; }
    }
</style>

<script>
    // --- 1. STICKY HIDE LOGIC ---
    let lastScrollY = window.scrollY;
    const nav = document.getElementById('slim-nav');

    window.addEventListener('scroll', () => {
        const currentScrollY = window.scrollY;

        if (currentScrollY > lastScrollY && currentScrollY > 100) {
            // Scrolling Down - Hide Nav
            nav.classList.add('nav-up');
        } else {
            // Scrolling Up - Show Nav
            nav.classList.remove('nav-up');
        }
        lastScrollY = currentScrollY;
    });

    // --- 2. EXTERNAL REDIRECT ---
    function openForm() {
        window.open('https://debeatzgh1.github.io/Floating-Flashcards-Widget/', '_blank');
    }

    // --- 3. THEME ENGINE ---
    function toggleTheme() {
        const body = document.body;
        const icon = document.getElementById('theme-icon');
        const isLight = body.getAttribute('data-theme') === 'light';
        
        if (isLight) {
            body.removeAttribute('data-theme');
            icon.className = 'fas fa-moon';
            localStorage.setItem('debeatz_theme', 'dark');
        } else {
            body.setAttribute('data-theme', 'light');
            icon.className = 'fas fa-sun';
            localStorage.setItem('debeatz_theme', 'light');
        }
    }

    // --- 4. NAVIGATION ---
    function scrollToTop() { window.scrollTo({ top: 0, behavior: 'smooth' }); }
    function scrollToBottom() { window.scrollTo({ top: document.body.scrollHeight, behavior: 'smooth' }); }

    // Init Theme on Load
    if (localStorage.getItem('debeatz_theme') === 'light') toggleTheme();
</script>



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
