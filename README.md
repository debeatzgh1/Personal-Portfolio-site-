
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

body{
  margin:0;
  background:var(--bg);
  color:var(--text);
}

/* HEADER */
header{
  display:flex;
  justify-content:space-between;
  align-items:center;
  padding:12px 14px;
  border-bottom:1px solid #1f2937;
  background:var(--panel);
}

header h1{
  font-size:16px;
  margin:0;
}

header button{
  background:none;
  border:none;
  color:var(--text);
  font-size:18px;
  cursor:pointer;
}

/* MOBILE MENU */
.menu{
  position:fixed;
  inset:0;
  background:rgba(0,0,0,.6);
  display:none;
  z-index:99;
}

.menu.active{display:block;}

.menu-panel{
  width:260px;
  height:100%;
  background:var(--panel);
  padding:16px;
  animation:slide .3s ease;
}

@keyframes slide{
  from{transform:translateX(-100%)}
  to{transform:translateX(0)}
}

.menu-panel h3{
  margin-top:0;
  font-size:14px;
}

.menu-panel button{
  display:block;
  width:100%;
  margin-bottom:8px;
  padding:10px;
  border-radius:10px;
  border:1px solid #1f2937;
  background:var(--card);
  color:var(--text);
  text-align:left;
}

/* TABS */
.tabs{
  display:flex;
  gap:8px;
  padding:10px;
  overflow-x:auto;
  background:var(--panel);
  border-bottom:1px solid #1f2937;
}

.tabs button{
  display:flex;
  align-items:center;
  gap:6px;
  background:var(--card);
  border:1px solid #1f2937;
  color:var(--text);
  padding:8px 12px;
  border-radius:10px;
  font-size:12px;
  cursor:pointer;
  white-space:nowrap;
}

.tabs button:hover{
  border-color:var(--accent);
}

/* VIEWER */
.viewer{
  height:calc(100vh - 120px);
}

iframe{
  width:100%;
  height:100%;
  border:none;
  background:#000;
}

/* BLOG FEED */
.feed{
  padding:12px;
  display:grid;
  gap:10px;
}

.feed a{
  background:var(--card);
  padding:10px;
  border-radius:12px;
  text-decoration:none;
  color:var(--text);
  border:1px solid #1f2937;
}

.feed a:hover{
  border-color:var(--accent);
}

footer{
  font-size:11px;
  text-align:center;
  opacity:.6;
  padding:6px;
}
</style>
</head>

<body>

<header>
  <button onclick="toggleMenu()">☰</button>
  <h1>David Kumah</h1>
  <button onclick="toggleTheme()">🌗</button>
</header>

<!-- MOBILE MENU -->
<div class="menu" id="menu" onclick="toggleMenu()">
  <div class="menu-panel" onclick="event.stopPropagation()">
    <h3>Navigation</h3>
    <button onclick="openContent('https://debeatzgh1.github.io/Home-/')">🤖 AI Hub</button>
    <button onclick="openContent('https://debeatzgh1.github.io/-My-Brand-Online-Digital-Products-Affiliate-Shop/')">🛒 Products</button>
    <button onclick="openContent('https://debeatzgh1.github.io/The-Ultimate-Guide-to-Side-Hustle/')">🚀 Side Hustles</button>
    <button onclick="showBlogs()">📰 Latest Blogs</button>
  </div>
</div>

<!-- TABS -->
<div class="tabs">
  <button onclick="openContent('https://debeatzgh1.github.io/Home-/')">🤖 AI</button>
  <button onclick="openContent('https://debeatzgh1.github.io/-My-Brand-Online-Digital-Products-Affiliate-Shop/')">🛒 Products</button>
  <button onclick="openContent('https://debeatzgh1.github.io/The-Ultimate-Guide-to-Side-Hustle/')">🚀 Hustles</button>
  <button onclick="showBlogs()">📰 Blogs</button>
  <button onclick="openConten('https://milkshake.debeatzgh/')">🔗 Links</button>
</div>

<!-- VIEW -->
<div class="viewer" id="viewerBox">
  <iframe id="viewer" src="https://debeatzgh1.github.io/debeatzgh-/"></iframe>
</div>

<footer>
  © Debeatzgh • Tech & AI Solutions
</footer>

<script>
const viewer = document.getElementById('viewer');
const box = document.getElementById('viewerBox');

/* NAV */
function openContent(url){
  box.innerHTML = `<iframe id="viewer" src="${url}"></iframe>`;
}

function openExternal(url){
  window.open(url,'_blank');
}

/* BLOG FEEDS */
function showBlogs(){
  box.innerHTML = `<div class="feed" id="feed">Loading latest posts...</div>`;
  loadFeed('https://debeatzgh.wordpress.com/feed/');
  loadFeed('http://beatzde4.blogspot.com/feeds/posts/default?alt=rss');
}

function loadFeed(url){
  fetch(`https://api.rss2json.com/v1/api.json?rss_url=${url}`)
    .then(res=>res.json())
    .then(data=>{
      const feed=document.getElementById('feed');
      data.items.slice(0,4).forEach(post=>{
        feed.innerHTML += `
          <a href="${post.link}" target="_blank">
            <strong>${post.title}</strong><br>
            <small>${post.pubDate.split(' ')[0]}</small>
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
  localStorage.theme = document.body.classList.contains('light') ? 'light':'dark';
}

if(localStorage.theme==='light'){
  document.body.classList.add('light');
}
</script>

</body>
</html>
