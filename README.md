
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
