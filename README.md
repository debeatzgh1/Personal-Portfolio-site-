<!doctype html>


    
    
    Creator OS | Matrix Hub
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" />
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;500;700&family=JetBrains+Mono:wght@400;700&display=swap');
        
        :root {
            --notify-red: #ff3e3e;
            --gist-blue: #58a6ff;
            --glass-dark: rgba(10, 15, 28, 0.9);
        }

        body { background: #02040a; color: #e2e8f0; font-family: 'Space Grotesk', sans-serif; overflow: hidden; margin: 0; }
        #bg-canvas { position: fixed; inset: 0; z-index: -1; }
        .glass { background: var(--glass-dark); backdrop-filter: blur(20px); border: 1px solid rgba(56, 189, 248, 0.1); }

        /* --- LEFT MIDDLE NOTIFICATION --- */
        #notification-wrapper {
            position: fixed;
            left: 20px;
            top: 50%;
            transform: translateY(-50%);
            display: flex;
            align-items: center;
            z-index: 9999;
        }

        #gist-notifier {
            width: 45px; /* Shrunk size */
            height: 45px;
            background: var(--glass-dark);
            border: 1px solid rgba(88, 166, 255, 0.3);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 0 20px rgba(88, 166, 255, 0.2);
            order: 1; /* Bell on left */
            position: relative;
        }

        #notify-bubble {
            background: #fff;
            color: #0d1117;
            padding: 8px 15px;
            border-radius: 10px;
            margin-left: 12px; /* Gap from left bell */
            font-size: 11px;
            font-weight: 800;
            opacity: 0;
            transform: translateX(-10px);
            transition: 0.4s;
            white-space: nowrap;
            order: 2; /* Text on right of bell */
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
            pointer-events: none;
        }
        #notify-bubble.show { opacity: 1; transform: translateX(0); }

        /* --- HUB LAUNCHER (MINI) --- */
        #hub-launcher {
            position: fixed;
            left: 0;
            top: 50%;
            transform: translateY(-50%);
            background: var(--glass-dark);
            color: var(--gist-blue);
            padding: 15px 8px;
            border-radius: 0 10px 10px 0;
            font-size: 9px;
            font-weight: 900;
            writing-mode: vertical-rl;
            text-orientation: mixed;
            letter-spacing: 2px;
            border: 1px solid rgba(88, 166, 255, 0.2);
            border-left: none;
            cursor: pointer;
            display: none;
            z-index: 9998;
            transition: 0.3s;
        }
        #hub-launcher:hover { background: var(--gist-blue); color: #000; }

        /* --- FORM OVERLAY --- */
        #gist-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.95); backdrop-filter: blur(10px); display: none; z-index: 10000; justify-content: center; align-items: center; }
        .modal-container { width: 95%; max-width: 600px; height: 85vh; background: #0d1117; border-radius: 20px; display: flex; flex-direction: column; border: 1px solid #30363d; overflow: hidden; }
        iframe { width: 100%; height: 100%; border: none; display: none; }
        
        .spinner-box { display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 15px; }
        .spinner { width: 30px; height: 30px; border: 2px solid rgba(88, 166, 255, 0.1); border-top: 2px solid var(--gist-blue); border-radius: 50%; animation: spin 0.8s linear infinite; }
        @keyframes spin { 100% { transform: rotate(360deg); } }

        @keyframes shake { 0%, 100% { transform: rotate(0); } 20% { transform: rotate(10deg); } 40% { transform: rotate(-10deg); } }
        .shake { animation: shake 0.5s ease-in-out; }
    </style>



    <audio id="notify-sound" src="https://assets.mixkit.co/active_storage/sfx/2358/2358-preview.mp3"></audio>

    <canvas id="bg-canvas"></canvas>

    <div id="main-ui" class="max-w-4xl mx-auto px-6 pt-20">
        <h1 class="text-4xl font-black text-white">MATRIX_<span class="text-cyan-500">OS</span></h1>
        <p class="text-slate-500 text-xs mt-2 tracking-widest">SYSTEM STATUS: NOMINAL</p>
    </div>

    <div id="notification-wrapper">
        <div id="gist-notifier" onclick="handleFormLaunch()">
            <span id="comment-badge" class="absolute -top-1 -right-1 bg-red-500 w-3 h-3 rounded-full hidden"></span>
            <svg width="18" height="18" viewbox="0 0 24 24" fill="none" stroke="#58a6ff" stroke-width="2.5"><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"></path><path d="M13.73 21a2 2 0 0 1-3.46 0"></path></svg>
        </div>
        <div id="notify-bubble"><span id="type-text"></span></div>
    </div>

    <button id="hub-launcher" onclick="handleFormLaunch()">OPEN_INPUT_FORM</button>

    <div id="gist-overlay">
        <div class="modal-container">
            <div class="flex justify-between items-center p-4 bg-[#161b22] border-b border-[#30363d]">
                <span class="text-[9px] font-bold text-slate-500 tracking-widest uppercase">Secure_Form_Channel</span>
                <button onclick="closeOverlay()" class="text-white text-xl">&times;</button>
            </div>
            <div id="form-loader" class="spinner-box">
                <div class="spinner"></div>
                <div class="text-[9px] text-cyan-500 font-bold uppercase tracking-tighter">Connecting to Server...</div>
            </div>
            <iframe id="form-iframe" src=""></iframe>
        </div>
    </div>

    <script>
        // Background Particles
        const canvas = document.getElementById('bg-canvas');
        const ctx = canvas.getContext('2d');
        let particles = [];
        function initBg() {
            canvas.width = window.innerWidth; canvas.height = window.innerHeight;
            particles = Array.from({length: 40}, () => ({
                x: Math.random()*canvas.width, y: Math.random()*canvas.height,
                vx: (Math.random()-0.5)*0.2, vy: (Math.random()-0.5)*0.2
            }));
        }
        function animateBg() {
            ctx.clearRect(0,0,canvas.width,canvas.height);
            particles.forEach(p => {
                p.x += p.vx; p.y += p.vy;
                if(p.x<0||p.x>canvas.width) p.vx*=-1;
                if(p.y<0||p.y>canvas.height) p.vy*=-1;
                ctx.fillStyle = 'rgba(88,166,255,0.1)';
                ctx.beginPath(); ctx.arc(p.x, p.y, 1, 0, Math.PI*2); ctx.fill();
            });
            requestAnimationFrame(animateBg);
        }

        // Typewriter logic
        function typeWriter(text, i=0) {
            if (i < text.length) {
                document.getElementById('type-text').innerHTML += text.charAt(i);
                setTimeout(() => typeWriter(text, i+1), 40);
            }
        }

        function checkNotification() {
            if (localStorage.getItem('form_seen')) {
                document.getElementById('notification-wrapper').style.display = 'none';
                document.getElementById('hub-launcher').style.display = 'block';
            } else {
                setTimeout(() => {
                    document.getElementById('notify-bubble').classList.add('show');
                    document.getElementById('comment-badge').classList.remove('hidden');
                    document.getElementById('gist-notifier').classList.add('shake');
                    typeWriter(" comment /Request...");
                    document.addEventListener('mouseover', () => { document.getElementById('notify-sound').play().catch(()=>{}) }, {once:true});
                }, 1500);
            }
        }

        function handleFormLaunch() {
            const iframe = document.getElementById('form-iframe');
            const overlay = document.getElementById('gist-overlay');
            const loader = document.getElementById('form-loader');
            
            overlay.style.display = 'flex';
            if (iframe.src === "" || iframe.src !== "https://form.svhrt.com/60f4a0aeedc1993c8c7b3989") {
                iframe.src = "https://form.svhrt.com/60f4a0aeedc1993c8c7b3989";
                iframe.onload = () => {
                    loader.style.display = 'none';
                    iframe.style.display = 'block';
                };
            }
            
            localStorage.setItem('form_seen', 'true');
            document.getElementById('notification-wrapper').style.display = 'none';
            document.getElementById('hub-launcher').style.display = 'block';
        }

        function closeOverlay() { document.getElementById('gist-overlay').style.display = 'none'; }

        window.onload = () => { initBg(); animateBg(); checkNotification(); };
        window.onresize = initBg;
    </script>

</!doctype>


<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        :root {
            --banner-bg: rgba(13, 17, 23, 0.85);
            --accent-pink: #FF1493;
            --accent-glow: rgba(255, 20, 147, 0.4);
            --text-white: #ffffff;
            --border-glass: rgba(255, 255, 255, 0.1);
        }

        /* Floating Banner Container */
        .top-floating-banner {
            position: fixed;
            top: 15px;
            left: 50%;
            transform: translateX(-50%);
            width: 90%;
            max-width: 700px;
            height: 50px;
            background: var(--banner-bg);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid var(--border-glass);
            border-radius: 100px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 10px 0 25px;
            z-index: 10000;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
            overflow: hidden;
            animation: slideDown 0.8s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        /* Carousel Wrapper */
        .carousel-wrapper {
            flex: 1;
            overflow: hidden;
            position: relative;
            margin-right: 15px;
        }

        .carousel-content {
            display: flex;
            white-space: nowrap;
            animation: scrollText 15s linear infinite;
        }

        .carousel-content span {
            font-family: 'Segoe UI', Roboto, sans-serif;
            font-size: 0.85rem;
            color: var(--text-white);
            font-weight: 500;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        /* Launch Button */
        .banner-btn {
            background: var(--accent-pink);
            color: white;
            border: none;
            padding: 8px 20px;
            border-radius: 50px;
            font-size: 0.8rem;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 6px;
            box-shadow: 0 0 15px var(--accent-glow);
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .banner-btn:hover {
            transform: scale(1.05);
            background: #ff69b4;
            box-shadow: 0 0 25px var(--accent-glow);
        }

        /* Modal Overlay for Milkshake */
        #milkshake-modal {
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.9);
            display: none;
            z-index: 10001;
            flex-direction: column;
            animation: fadeIn 0.3s ease;
        }

        .modal-header {
            padding: 15px 25px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: #161b22;
        }

        .close-btn {
            background: #f85149;
            color: white;
            border: none;
            padding: 5px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
        }

        #msha-iframe {
            width: 100%;
            flex-grow: 1;
            border: none;
        }

        /* Animations */
        @keyframes scrollText {
            0% { transform: translateX(100%); }
            100% { transform: translateX(-100%); }
        }

        @keyframes slideDown {
            from { transform: translate(-50%, -100px); opacity: 0; }
            to { transform: translate(-50%, 0); opacity: 1; }
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        /* Mobile Adjustments */
        @media (max-width: 600px) {
            .carousel-content span { font-size: 0.75rem; }
            .banner-btn span { display: none; }
            .banner-btn { padding: 8px 12px; }
        }
    </style>
</head>
<body>

    <div class="top-floating-banner">
        <div class="carousel-wrapper">
            <div class="carousel-content">
                <span>
                    💻 Access your lifestyle, productivity tools and ideas All in one place! 🚀 💡 📈
                </span>
            </div>
        </div>
        
        <button class="banner-btn" onclick="openMilkshake()">
            <span>Launch Hub</span> ⭐️
        </button>
    </div>

    <div id="milkshake-modal">
        <div class="modal-header">
            <span style="color:white; font-family: sans-serif; font-weight: bold;">Debeatzgh Hub</span>
            <button class="close-btn" onclick="closeMilkshake()">✕ Close</button>
        </div>
        <iframe id="msha-iframe" src=""></iframe>
    </div>

    <script>
        function openMilkshake() {
            const modal = document.getElementById('milkshake-modal');
            const iframe = document.getElementById('msha-iframe');
            
            // Set source only when clicked to save performance
            iframe.src = "https://msha.ke/debeatzgh";
            modal.style.display = "flex";
            document.body.style.overflow = "hidden"; // Disable scroll
        }

        function closeMilkshake() {
            const modal = document.getElementById('milkshake-modal');
            const iframe = document.getElementById('msha-iframe');
            
            modal.style.display = "none";
            iframe.src = ""; // Stop the iframe content
            document.body.style.overflow = "auto"; // Re-enable scroll
        }
    </script>

</body>
</html>



<!-- Elfsight All-in-One Chat | All-in-One Chat -->
<script src="https://elfsightcdn.com/platform.js" async></script>
<div class="elfsight-app-477d870c-bbce-4a1c-bcfe-6928b9aa363c" data-elfsight-app-lazy></div>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>David Kumah | Digital Entrepreneur</title>

<style>
:root{
  --bg:#020617;
  --panel:#0f172a;
  --card:#111827;
  --text:#e5e7eb;
  --accent:#38bdf8;
}
.light{
  --bg:#f8fafc;
  --panel:#ffffff;
  --card:#f1f5f9;
  --text:#0f172a;
  --accent:#0284c7;
}
*{box-sizing:border-box;font-family:system-ui}
body{margin:0;background:var(--bg);color:var(--text);}

/* HEADER */
header{
  display:flex;justify-content:space-between;align-items:center;
  padding:12px 14px;border-bottom:1px solid #1f2937;
  background:var(--panel);
}
header h1{font-size:16px;margin:0}
header button{background:none;border:none;color:var(--text);font-size:18px;cursor:pointer}

/* MOBILE MENU */
.menu{position:fixed;inset:0;background:rgba(0,0,0,.6);display:none;z-index:99}
.menu.active{display:block}
.menu-panel{
  width:260px;height:100%;background:var(--panel);
  padding:16px;animation:slide .3s ease
}
@keyframes slide{from{transform:translateX(-100%)}to{transform:translateX(0)}}
.menu-panel h3{margin-top:0;font-size:14px}
.menu-panel button{
  display:block;width:100%;margin-bottom:8px;
  padding:10px;border-radius:10px;
  border:1px solid #1f2937;background:var(--card);
  color:var(--text);text-align:left
}

/* TABS */
.tabs{
  display:flex;gap:8px;padding:10px;overflow-x:auto;
  background:var(--panel);border-bottom:1px solid #1f2937
}
.tabs button{
  display:flex;align-items:center;gap:6px;
  background:var(--card);border:1px solid #1f2937;
  color:var(--text);padding:8px 12px;
  border-radius:10px;font-size:12px;cursor:pointer
}
.tabs button:hover{border-color:var(--accent)}

/* VIEWER */
.viewer{height:calc(100vh - 120px)}
iframe{width:100%;height:100%;border:none;background:#000}

/* BLOG FEED */
.feed{padding:12px;display:grid;gap:10px}
.feed a{
  background:var(--card);padding:10px;border-radius:12px;
  text-decoration:none;color:var(--text);
  border:1px solid #1f2937
}
.feed a:hover{border-color:var(--accent)}

footer{font-size:11px;text-align:center;opacity:.6;padding:6px}
</style>
</head>

<body>

<header>
  <button onclick="toggleMenu()">☰</button>
  <h1>My links</h1>
  <button onclick="toggleTheme()">🌗</button>
</header>

<!-- MOBILE MENU -->
<div class="menu" id="menu" onclick="toggleMenu()">
  <div class="menu-panel" onclick="event.stopPropagation()">
    <h3>Navigation</h3>
    <button onclick="showBlogs()">📰 Blog</button>
    <button onclick="openContent('https://debeatzgh1.github.io/blogs/')">📄 Blogs</button>
    <button onclick="openContent('https://debeatzgh1.github.io/debeatzgh/')">🚀 Tools</button>
    <button onclick="openContent('https://msha.ke/debeatzgh')">🔗 Milkshake</button>
  </div>
</div>

<!-- TABS -->
<div class="tabs">
  <button onclick="showBlogs()">📰 Blog</button>
  <button onclick="openContent('https://debeatzgh1.github.io/blogs/')">📄 Blogs</button>
  <button onclick="openContent('https://debeatzgh1.github.io/debeatzgh/')">🚀 Tools</button>
  <button onclick="openContent('https://msha.ke/debeatzgh')">🔗 Links</button>
</div>

<!-- VIEW -->
<div class="viewer" id="viewerBox">
  <div class="feed">Select a section to begin</div>
</div>

<footer>© Debeatzgh • Tech & AI Solutions</footer>

<script>
const box = document.getElementById('viewerBox');

/* SAFE IFRAME LOADER */
function openContent(url){
  box.innerHTML = `<iframe id="viewer" src="${url}"></iframe>`;
  const iframe = document.getElementById('viewer');
  iframe.onload = () => {
    try { iframe.contentDocument; }
    catch {
      window.open(url,'_blank');
      box.innerHTML = `<div class="feed">Opened in new tab (embedding blocked)</div>`;
    }
  };
}

/* BLOG FEED */
function showBlogs(){
  box.innerHTML = `<div class="feed" id="feed">Loading latest posts...</div>`;
  loadFeed('https://debeatzgh.wordpress.com/feed/');
  loadFeed('http://beatzde4.blogspot.com/feeds/posts/default?alt=rss');
}

function loadFeed(url){
  fetch(`https://api.rss2json.com/v1/api.json?rss_url=${url}`)
    .then(r=>r.json())
    .then(d=>{
      const feed=document.getElementById('feed');
      d.items.slice(0,4).forEach(p=>{
        feed.innerHTML+=`
          <a href="${p.link}" target="_blank">
            <strong>${p.title}</strong><br>
            <small>${p.pubDate.split(' ')[0]}</small>
          </a>`;
      });
    });
}

/* MENU */
function toggleMenu(){
  document.getElementById('menu').classList.toggle('active');
}

/* THEME */
function toggleTheme(){
  document.body.classList.toggle('light');
  localStorage.theme=document.body.classList.contains('light')?'light':'dark';
}
if(localStorage.theme==='light')document.body.classList.add('light');
</script>

</body>
</html>

<div id="booking-fab" style="position: fixed; bottom: 30px; left: 30px; z-index: 1000;">
    <a href="https://debeatzgh1.github.io/Home-/" target="_blank" style="text-decoration: none;">
        <div style="background: #28a745; color: white; padding: 15px 25px; border-radius: 50px; display: flex; align-items: center; box-shadow: 0 4px 15px rgba(0,0,0,0.3); transition: transform 0.3s ease;">
            <span style="margin-right: 10px; font-size: 1.2rem;">📅</span>
            <span style="font-weight: bold; font-family: sans-serif;">Book Discovery Call</span>
        </div>
    </a>
</div>

<style>
    #booking-fab div:hover {
        transform: scale(1.05);
        background: #218838;
    }
    /* Mobile adjustment */
    @media (max-width: 600px) {
        #booking-fab {
            bottom: 20px;
            right: 20px;
        }
        #booking-fab span:last-child {
            display: none; /* Shows only the icon on small screens to save space */
        }
        #booking-fab div {
            padding: 15px;
            border-radius: 50%;
        }
    }
</style><div id="calculator-container" style="font-family: sans-serif; max-width: 400px; border: 1px solid #ddd; padding: 20px; border-radius: 12px; background: #ffo; shadow: 0 4px 6px rgba(0,0,0,0.1);">
    <h3 style="margin-top: 0; color: #333;">Project Estimator</h3>
    <p style="font-size: 0.9rem; color: #666;">Select the services you need:</p>
    
    <div style="margin-bottom: 15px;">
        <label style="display: block; margin-bottom: 8px;">
            <input type="checkbox" class="service-item" data-price="500" onchange="calculateTotal()" /> Landing Page Design (¢850)
        </label>
        <label style="display: block; margin-bottom: 8px;">
            <input type="checkbox" class="service-item" data-price="1200" onchange="calculateTotal()" /> E-commerce Setup (¢1,200)
        </label>
        <label style="display: block; margin-bottom: 8px;">
            <input type="checkbox" class="service-item" data-price="300" onchange="calculateTotal()" /> SEO Optimization (¢300)
        </label>
        <label style="display: block; margin-bottom: 8px;">
            <input type="checkbox" class="service-item" data-price="800" onchange="calculateTotal()" /> Custom web app design (¢900)
        </label>
    </div>

    <hr style="border: 0; border-top: 1px solid #eee;" />
    
    <div style="display: flex; justify-content: space-between; align-items: center; margin-top: 15px;">
        <span style="font-weight: bold; font-size: 1.1rem;">Estimated Total:</span>
        <span id="total-price" style="font-size: 1.4rem; color: #007bff; font-weight: bold;">¢0</span>
    </div>

    <button onclick="location.href='mailto: debeatz4@gmail.com?subject=Project Quote'" style="width: 100%; margin-top: 20px; padding: 12px; background: #007bff; color: white; border: none; border-radius: 6px; cursor: pointer; font-weight: bold;">
        Request Formal Quote
    </button>
</div>

<script>
    function calculateTotal() {
        let total = 0;
        const checkboxes = document.querySelectorAll('.service-item');
        checkboxes.forEach(item => {
            if (item.checked) {
                total += parseInt(item.getAttribute('data-price'));
            }
        });
        document.getElementById('total-price').innerText = '¢' + total.toLocaleString();
    }
</script>


<!-- Floating Menu Button with Modals: Place in HTML/JavaScript widget on Blogger for site-wide use -->
<style>
  :root{
    --bg-dark: #0b1220;
    --bg-dark-soft: rgba(15,23,42,0.92);
    --card-dark: #111827;
    --border-dark: rgba(255,255,255,0.08);
    --accent: #38bdf8;
    --accent-hover: #0ea5e9;
    --text-light: #e5e7eb;
    --text-muted: #9ca3af;
  }

  #floatingMenu {
    position: fixed;
    top: 50%;
    right: 15px;
    transform: translateY(-50%);
    z-index: 9999;
  }

  /* Floating Button */
  .menu-btn {
    background: linear-gradient(135deg, #0ea5e9, #2563eb);
    color: #fff;
    border: none;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    font-size: 26px;
    cursor: pointer;
    box-shadow: 0 10px 25px rgba(37,99,235,.45);
    transition: transform .25s ease, box-shadow .25s ease;
  }

  .menu-btn:hover {
    transform: scale(1.08);
    box-shadow: 0 14px 35px rgba(37,99,235,.65);
  }

  /* Icon Menu */
  .icon-menu {
    display: none;
    flex-direction: column;
    align-items: flex-end;
    margin-bottom: 12px;
  }

  .icon-menu a {
    display: flex;
    align-items: center;
    text-decoration: none;
    color: var(--text-light);
    background: var(--card-dark);
    padding: 12px 16px;
    margin: 6px 0;
    border-radius: 12px;
    border: 1px solid var(--border-dark);
    box-shadow: 0 6px 18px rgba(0,0,0,.45);
    transition: all .25s ease;
    width: 250px;
    cursor: pointer;
  }

  .icon-menu a:hover {
    background: #020617;
    border-color: var(--accent);
    color: #fff;
    transform: translateX(-6px);
  }

  .icon-menu i {
    font-size: 18px;
    margin-right: 12px;
    color: var(--accent);
  }

  /* Modal Overlay */
  .custom-modal {
    display: none;
    position: fixed;
    z-index: 10000;
    inset: 0;
    background: rgba(2,6,23,.85);
    backdrop-filter: blur(8px);
    justify-content: center;
    align-items: center;
  }

  /* Modal Box */
  .custom-modal .modal-content {
    background: var(--bg-dark-soft);
    margin: 60px auto;
    padding: 30px 22px;
    border-radius: 16px;
    max-width: 640px;
    box-shadow: 0 30px 80px rgba(0,0,0,.65);
    position: relative;
    animation: fadeIn .35s ease;
    color: var(--text-light);
    border: 1px solid var(--border-dark);
  }

  @keyframes fadeIn {
    from {transform:translateY(25px); opacity:0;}
    to {transform:translateY(0); opacity:1;}
  }

  .custom-modal .close {
    position: absolute;
    top: 12px;
    right: 18px;
    font-size: 26px;
    color: var(--text-muted);
    cursor: pointer;
  }

  .custom-modal .close:hover {
    color: #fff;
  }

  .custom-modal h2 {
    color: var(--accent);
    text-align: center;
    margin-bottom: 20px;
  }

  .custom-modal p,
  .custom-modal li,
  .custom-modal summary {
    color: var(--text-light);
  }

  .custom-modal a {
    color: var(--accent);
    word-break: break-word;
  }

  .custom-modal a:hover {
    text-decoration: underline;
    color: var(--accent-hover);
  }

  details {
    background: rgba(255,255,255,0.03);
    border-radius: 10px;
    padding: 10px 14px;
    margin-bottom: 10px;
    border: 1px solid var(--border-dark);
  }

  details summary {
    cursor: pointer;
    font-weight: 600;
  }

  @media (max-width: 700px) {
    .custom-modal .modal-content { max-width: 96vw; }
    .icon-menu a { width: 92vw; }
  }
</style>
<!-- Font Awesome (for icons) -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
<div id="floatingMenu">
  <div class="icon-menu" id="iconLinks">
    <a onclick="openModal('aboutModal')"><i class="fas fa-user"></i> About Us</a>
    <a onclick="openModal('contactModal')"><i class="fas fa-envelope"></i> Contact</a>
    <a onclick="openModal('toolsModal')"><i class="fas fa-toolbox"></i> Tools & Widgets</a>
    <a onclick="openModal('privacyModal')"><i class="fas fa-shield-alt"></i> Privacy Policy</a>
    <a onclick="openModal('termsModal')"><i class="fas fa-file-contract"></i> Terms of Use</a>
    <a onclick="openModal('resourcesModal')"><i class="fas fa-book-open"></i> Resources</a>
    <a onclick="openModal('faqModal')"><i class="fas fa-question-circle"></i> FAQs</a>
    <a onclick="openModal('aiModal')"><i class="fas fa-robot"></i> AI Articles</a>
    <a onclick="openModal('galleryModal')"><i class="fas fa-images"></i> Gallery</a>
  </div>
  <button class="menu-btn" onclick="toggleMenu()">
    <i class="fas fa-bars"></i>
  </button>
</div>
<!-- About Modal -->
<div id="aboutModal" class="custom-modal">
  <div class="modal-content">
    <span class="close" onclick="closeModal('aboutModal')">&times;</span>
    <h2>About Us</h2>
    <p><strong>debeatzgh1</strong> is a passionate developer, designer, and content creator dedicated to empowering creators and entrepreneurs through open-source tools, digital products, and educational content. Across multiple platforms (GitHub and Blogger), debeatzgh1 shares curated front-end components, productivity resources, and guides for web development, AI, and digital business growth.</p>
  </div>
</div>
<!-- Contact Modal -->
<div id="contactModal" class="custom-modal">
  <div class="modal-content">
    <span class="close" onclick="closeModal('contactModal')">&times;</span>
    <h2>Contact</h2>
    <p>You can connect via:</p>
    <ul>
      <li><a href="https://github.com/debeatzgh1" target="_blank">GitHub Profile</a></li>
      <li><a href="https://beatzde4.blogspot.com/p/contact.html" target="_blank">Beatzde4 Blog Contact</a></li>
      <li>Email: <a href="mailto:your@email.com">Use blog contact form</a></li>
    </ul>
  </div>
</div>
<!-- Tools & Widgets Modal -->
<div id="toolsModal" class="custom-modal">
  <div class="modal-content">
    <span class="close" onclick="closeModal('toolsModal')">&times;</span>
    <h2>Tools & Widgets</h2>
    <ul>
      <li><a href="https://github.com/debeatzgh1/firebase-front-end-components" target="_blank">Firebase Front-End Components</a>: Reusable UI components, HTML/CSS/JS, and Python resources.</li>
      <li><a href="https://beatzde4.blogspot.com/p/tools.html" target="_blank">Beatzde4 Blog Tools</a>: Widgets, templates, and utilities for bloggers and creators.</li>
    </ul>
  </div>
</div>
<!-- Privacy Policy Modal -->
<div id="privacyModal" class="custom-modal">
  <div class="modal-content">
    <span class="close" onclick="closeModal('privacyModal')">&times;</span>
    <h2>Privacy Policy</h2>
    <p>Your privacy matters! We use cookies and analytics to enhance your experience. No personal data is shared or sold. For details, visit:</p>
    <ul>
      <li><a href="https://beatzde4.blogspot.com/p/privacy-policy.html" target="_blank">Beatzde4 Blog Privacy Policy</a></li>
      <li><a href="https://appdategh1.blogspot.com/p/privacy-policy.html" target="_blank">AppdateGH Privacy Policy</a></li>
    </ul>
  </div>
</div>
<!-- Terms of Use Modal -->
<div id="termsModal" class="custom-modal">
  <div class="modal-content">
    <span class="close" onclick="closeModal('termsModal')">&times;</span>
    <h2>Terms of Use</h2>
    <p>By using our tools, templates, and content, you agree to use them for lawful and ethical purposes. Please credit the author when using open-source code. See the full terms:</p>
    <ul>
      <li><a href="https://beatzde4.blogspot.com/p/terms.html" target="_blank">Beatzde4 Blog Terms</a></li>
    </ul>
  </div>
</div>
<!-- Resources Modal -->
<div id="resourcesModal" class="custom-modal">
  <div class="modal-content">
    <span class="close" onclick="closeModal('resourcesModal')">&times;</span>
    <h2>Resources</h2>
    <ul>
      <li><a href="https://beatzde4.blogspot.com/p/firebase-curated-front-end-components.html" target="_blank">Firebase Curated Front-End Components</a></li>
      <li><a href="https://mybrandsonline.blogspot.com/" target="_blank">MyBrandsOnline Blog</a>: Branding advice and online business tips.</li>
      <li><a href="http://digimartgh.blogspot.com/" target="_blank">DigimartGH</a>: Digital marketing, business tools, and guides.</li>
    </ul>
  </div>
</div>
<!-- FAQs Modal -->
<div id="faqModal" class="custom-modal">
  <div class="modal-content">
    <span class="close" onclick="closeModal('faqModal')">&times;</span>
    <h2>Frequently Asked Questions</h2>
    <details>
      <summary>Who is debeatzgh1?</summary>
      <div>
        <p>debeatzgh1 is a developer, designer, and content creator focused on building modern web apps, sharing productivity tools, and providing digital solutions. Find my projects on <a href="https://github.com/debeatzgh1" target="_blank">GitHub</a> and insights on my <a href="https://beatzde4.blogspot.com/" target="_blank">blog</a>.</p>
      </div>
    </details>
    <details>
      <summary>What is the firebase-front repository?</summary>
      <div>
        <p>The <a href="https://github.com/debeatzgh1/firebase-front-end-components" target="_blank">firebase-front-end-components</a> repository is an open-source front-end template or starter kit for integrating Firebase services into web projects. It provides modern UI components and ready-to-use code for developers.</p>
      </div>
    </details>
    <details>
      <summary>How can I use your templates or code?</summary>
      <div>
        <p>Browse my <a href="https://github.com/debeatzgh1?tab=repositories" target="_blank">GitHub repositories</a> and follow the README instructions to clone or download templates and starter projects. Most projects are open source and free to use with attribution.</p>
      </div>
    </details>
    <details>
      <summary>Do you provide guides and tutorials?</summary>
      <div>
        <p>Yes! I share detailed guides, tech tips, and tutorials on my <a href="https://beatzde4.blogspot.com/" target="_blank">blog</a> covering Firebase, web development, productivity tools, and AI-powered solutions.</p>
      </div>
    </details>
    <details>
      <summary>How do I contact you for custom services?</summary>
      <div>
        <p>You can reach out via my blog's contact form, or through my email and social links provided on my <a href="https://github.com/debeatzgh1" target="_blank">GitHub profile</a>.</p>
      </div>
    </details>
    <details>
      <summary>Can I request a new feature or report a bug?</summary>
      <div>
        <p>Absolutely! Please open an issue on the relevant GitHub repository, or leave a comment on my blog post related to your request.</p>
      </div>
    </details>
    <details>
      <summary>Are your projects free to use?</summary>
      <div>
        <p>Most projects are open source and free for personal or educational use. Please check the license in each repository for details.</p>
      </div>
    </details>
    <details>
      <summary>What topics do you cover on your blog?</summary>
      <div>
        <p>I cover digital design, productivity tools, web development (especially Firebase), AI, and online business tips.</p>
      </div>
    </details>
    <details>
      <summary>Where can I follow your updates?</summary>
      <div>
        <p>Follow me on <a href="https://github.com/debeatzgh1" target="_blank">GitHub</a> for code/project updates and on <a href="https://beatzde4.blogspot.com/" target="_blank">Blogger</a> for articles and announcements.</p>
      </div>
    </details>
  </div>
</div>
<!-- AI Articles Modal -->
<div id="aiModal" class="custom-modal">
  <div class="modal-content">
    <span class="close" onclick="closeModal('aiModal')">&times;</span>
    <h2>AI Articles</h2>
    <ul>
      <li><a href="https://beatzde4.blogspot.com/search/label/AI" target="_blank">AI Articles on Beatzde4</a></li>
      <li><a href="https://appdategh1.blogspot.com/search/label/AI" target="_blank">AppdateGH AI Section</a></li>
    </ul>
    <p>Stay updated with the latest in AI, automation, and tech trends!</p>
  </div>
</div>
<!-- Gallery Modal -->
<div id="galleryModal" class="custom-modal">
  <div class="modal-content">
    <span class="close" onclick="closeModal('galleryModal')">&times;</span>
    <h2>Gallery</h2>
    <p>View creative inspiration, UI/UX samples, and project showcases:</p>
    <ul>
      <li><a href="https://pin.it/7iRoE2LKj" target="_blank">Pinterest Gallery</a></li>
    </ul>
  </div>
</div>
<script>
  function toggleMenu() {
    const menu = document.getElementById('iconLinks');
    menu.style.display = menu.style.display === 'flex' ? 'none' : 'flex';
  }
  function openModal(id) {
    document.getElementById(id).style.display = 'flex';
    document.body.style.overflow = 'hidden';
  }
  function closeModal(id) {
    document.getElementById(id).style.display = 'none';
    document.body.style.overflow = '';
  }
  window.onclick = function(event) {
    const modals = document.querySelectorAll('.custom-modal');
    modals.forEach(function(modal) {
      if (event.target === modal) {
        modal.style.display = 'none';
        document.body.style.overflow = '';
      }
    });
  }
  document.addEventListener('keydown', function(event) {
    if (event.key === 'Escape') {
      document.querySelectorAll('.custom-modal').forEach(function(modal){
        modal.style.display = 'none';
      });
      document.body.style.overflow = '';
    }
  });
</script>

