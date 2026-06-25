
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>

<title>My • Premium FAQ Links Hub</title>

<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">

<style>
*{
  margin:0;
  padding:0;
  box-sizing:border-box;
}

html{
  scroll-behavior:smooth;
}

body{
  font-family:'Poppins',sans-serif;
  background:#06111f;
  color:#fff;
  overflow-x:hidden;
}

/* BACKGROUND */
body::before{
  content:"";
  position:fixed;
  inset:0;
  background:
    radial-gradient(circle at top left,#00d4ff22,transparent 30%),
    radial-gradient(circle at bottom right,#7c4dff22,transparent 30%),
    linear-gradient(135deg,#06111f,#0c1d33,#07111d);
  z-index:-2;
}

body::after{
  content:"";
  position:fixed;
  inset:0;
  backdrop-filter:blur(1px);
  z-index:-1;
}

/* HEADER */
.hero{
  padding:40px 20px 30px;
  text-align:center;
  position:relative;
}

.hero-banner{
  max-width:1200px;
  margin:auto;
  padding:35px 25px;
  border-radius:30px;
  background:rgba(255,255,255,0.08);
  border:1px solid rgba(255,255,255,0.1);
  backdrop-filter:blur(20px);
  box-shadow:0 10px 40px rgba(0,0,0,0.4);
}

.badge{
  display:inline-flex;
  align-items:center;
  gap:8px;
  padding:10px 18px;
  border-radius:50px;
  background:#00d4ff20;
  border:1px solid #00d4ff55;
  color:#8fe8ff;
  font-size:13px;
  margin-bottom:18px;
}

.hero h1{
  font-size:clamp(2rem,5vw,4rem);
  margin-bottom:12px;
  font-weight:700;
}

.hero p{
  max-width:760px;
  margin:auto;
  line-height:1.8;
  color:#d7e4ff;
  font-size:15px;
}

.top-buttons{
  margin-top:28px;
  display:flex;
  flex-wrap:wrap;
  gap:15px;
  justify-content:center;
}

.top-buttons a{
  text-decoration:none;
  color:#fff;
  padding:14px 24px;
  border-radius:16px;
  font-weight:600;
  transition:.3s ease;
}

.btn-primary{
  background:linear-gradient(135deg,#00bfff,#6a5cff);
}

.btn-secondary{
  background:rgba(255,255,255,0.08);
  border:1px solid rgba(255,255,255,0.1);
}

.top-buttons a:hover{
  transform:translateY(-3px) scale(1.02);
}

/* SEARCH */
.search-box{
  max-width:1000px;
  margin:20px auto 10px;
  padding:0 20px;
}

.search-box input{
  width:100%;
  padding:18px 20px;
  border:none;
  outline:none;
  border-radius:20px;
  background:rgba(255,255,255,0.08);
  color:#fff;
  font-size:15px;
  backdrop-filter:blur(10px);
}

.search-box input::placeholder{
  color:#c7d3ea;
}

/* CATEGORY */
.section{
  max-width:1300px;
  margin:50px auto;
  padding:0 20px;
}

.section-title{
  font-size:28px;
  margin-bottom:10px;
  font-weight:700;
}

.section-desc{
  color:#b6c5df;
  margin-bottom:28px;
  line-height:1.7;
}

/* GRID */
.grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(300px,1fr));
  gap:25px;
}

/* CARD */
.card{
  background:rgba(255,255,255,0.06);
  border:1px solid rgba(255,255,255,0.08);
  border-radius:28px;
  overflow:hidden;
  transition:.4s ease;
  position:relative;
  backdrop-filter:blur(16px);
}

.card:hover{
  transform:translateY(-6px);
  box-shadow:0 20px 40px rgba(0,0,0,0.35);
}

.preview{
  height:220px;
  position:relative;
  overflow:hidden;
  background:#0b1728;
}

.preview iframe{
  width:100%;
  height:100%;
  border:none;
  transform:scale(.85);
  transform-origin:top left;
  width:117%;
  height:117%;
  pointer-events:none;
}

.card-content{
  padding:22px;
}

.category-tag{
  display:inline-block;
  padding:8px 14px;
  border-radius:50px;
  font-size:12px;
  background:#ffffff12;
  margin-bottom:15px;
  color:#9fd7ff;
}

.card h3{
  font-size:22px;
  margin-bottom:12px;
}

.card p{
  color:#d3ddf2;
  line-height:1.7;
  font-size:14px;
  margin-bottom:22px;
}

.card-buttons{
  display:flex;
  gap:12px;
  flex-wrap:wrap;
}

.card-buttons a{
  flex:1;
  min-width:130px;
  text-align:center;
  text-decoration:none;
  padding:14px 15px;
  border-radius:14px;
  font-weight:600;
  transition:.3s ease;
}

.open-inside{
  background:linear-gradient(135deg,#00c6ff,#7d4dff);
  color:#fff;
}

.open-outside{
  background:rgba(255,255,255,0.08);
  border:1px solid rgba(255,255,255,0.1);
  color:#fff;
}

.card-buttons a:hover{
  transform:translateY(-2px);
}

/* IFRAME MODAL */
.viewer{
  position:fixed;
  inset:0;
  background:#000d;
  z-index:9999;
  display:none;
  flex-direction:column;
}

.viewer.active{
  display:flex;
}

.viewer-top{
  display:flex;
  align-items:center;
  justify-content:space-between;
  padding:14px 18px;
  background:#07111d;
  border-bottom:1px solid rgba(255,255,255,0.08);
}

.viewer-top h2{
  font-size:16px;
}

.viewer-actions{
  display:flex;
  gap:12px;
}

.viewer-actions button,
.viewer-actions a{
  border:none;
  text-decoration:none;
  color:#fff;
  background:#15263f;
  padding:10px 16px;
  border-radius:12px;
  cursor:pointer;
  font-weight:600;
}

.viewer iframe{
  flex:1;
  width:100%;
  border:none;
  background:#fff;
}

/* FLOATING WHATSAPP */
.whatsapp{
  position:fixed;
  right:18px;
  bottom:20px;
  width:64px;
  height:64px;
  border-radius:50%;
  display:flex;
  align-items:center;
  justify-content:center;
  text-decoration:none;
  font-size:28px;
  background:#25D366;
  color:#fff;
  box-shadow:0 10px 30px rgba(0,0,0,.4);
  z-index:999;
  animation:pulse 3s infinite;
}

@keyframes pulse{
  0%{transform:scale(1);}
  50%{transform:scale(1.08);}
  100%{transform:scale(1);}
}

/* FOOTER */
.footer{
  text-align:center;
  padding:50px 20px;
  color:#aeb9cf;
}

.footer strong{
  color:#fff;
}

@media(max-width:768px){

  .card-buttons{
    flex-direction:column;
  }

  .top-buttons{
    flex-direction:column;
  }

}
</style>
</head>

<body>

<!-- HERO -->
<section class="hero">
  <div class="hero-banner">

    <div class="badge">
      ✨ Premium Personal Showcase Hub
    </div>

    <h1>Kumah David</h1>

    <p>
      Explore premium portfolio interfaces, modern content platforms,
      AI-powered showcase pages, and entertainment experiences in one elegant FAQ-style UI.
      Open every project directly inside this page or launch externally in a new tab.
    </p>

    <div class="top-buttons">
      <a href="https://debeatzgh1.github.io/me-/" target="_blank" class="btn-primary">
        Open Main Portfolio
      </a>

      <a href="https://wa.me/233270201181" target="_blank" class="btn-secondary">
        Contact on WhatsApp
      </a>
    </div>

  </div>
</section>

<!-- SEARCH -->
<div class="search-box">
  <input type="text" id="searchInput" placeholder="Search templates, interfaces, AI tools, portfolio pages..." />
</div>

<!-- PORTFOLIO -->
<section class="section">

  <h2 class="section-title">Portfolio Templates UI</h2>

  <p class="section-desc">
    Premium personal branding interfaces, portfolio showcase layouts,
    modern homepage designs, and interactive profile experiences.
  </p>

  <div class="grid">

    <!-- CARD -->
    <div class="card searchable">
      <div class="preview">
        <iframe src="https://debeatzgh1.github.io/Personal-Portfolio-site-/"></iframe>
      </div>

      <div class="card-content">
        <span class="category-tag">Portfolio UI</span>

        <h3>Personal Portfolio Site</h3>

        <p>
          Elegant responsive portfolio website with modern sections,
          profile showcase, branding blocks, and interactive presentation layout.
        </p>

        <div class="card-buttons">
          <a href="#" class="open-inside"
             onclick="openViewer('Personal Portfolio Site','https://debeatzgh1.github.io/Personal-Portfolio-site-/')">
             Open Here
          </a>

          <a href="https://debeatzgh1.github.io/Personal-Portfolio-site-/"
             target="_blank"
             class="open-outside">
             External
          </a>
        </div>
      </div>
    </div>

    <!-- CARD -->
    <div class="card searchable">
      <div class="preview">
        <iframe src="https://debeatzgh1.github.io/me-/"></iframe>
      </div>

      <div class="card-content">
        <span class="category-tag">Profile Interface</span>

        <h3>Modern Profile Hub</h3>

        <p>
          Stylish personal identity page with premium cards,
          responsive sections, showcase blocks, and digital branding aesthetics.
        </p>

        <div class="card-buttons">
          <a href="#" class="open-inside"
             onclick="openViewer('Modern Profile Hub','https://debeatzgh1.github.io/me-/')">
             Open Here
          </a>

          <a href="https://debeatzgh1.github.io/me-/"
             target="_blank"
             class="open-outside">
             External
          </a>
        </div>
      </div>
    </div>

    <!-- CARD -->
    <div class="card searchable">
      <div class="preview">
        <iframe src="https://debeatzgh1.github.io/Modern-homepage-styling-with-TailwindCSS-/"></iframe>
      </div>

      <div class="card-content">
        <span class="category-tag">Homepage UI</span>

        <h3>Tailwind Homepage Styling</h3>

        <p>
          Modern homepage concept using premium TailwindCSS inspired layouts,
          animations, responsive sections, and elegant visual hierarchy.
        </p>

        <div class="card-buttons">
          <a href="#" class="open-inside"
             onclick="openViewer('Tailwind Homepage Styling','https://debeatzgh1.github.io/Modern-homepage-styling-with-TailwindCSS-/')">
             Open Here
          </a>

          <a href="https://debeatzgh1.github.io/Modern-homepage-styling-with-TailwindCSS-/"
             target="_blank"
             class="open-outside">
             External
          </a>
        </div>
      </div>
    </div>

  </div>
</section>

<!-- CONTENT UI -->
<section class="section">

  <h2 class="section-title">Content Interface UI</h2>

  <p class="section-desc">
    Interactive content hubs, AI starter kits, blogging interfaces,
    productivity pages, and monetization resources.
  </p>

  <div class="grid">

    <!-- Repeatable Cards -->

    <div class="card searchable">
      <div class="preview">
        <iframe src="https://debeatzgh1.github.io/Decode-AI-starter-kit-/"></iframe>
      </div>

      <div class="card-content">
        <span class="category-tag">AI Toolkit</span>

        <h3>Decode AI Starter Kit</h3>

        <p>
          AI learning and productivity interface designed for creators,
          startups, students, and online entrepreneurs.
        </p>

        <div class="card-buttons">
          <a href="#" class="open-inside"
             onclick="openViewer('Decode AI Starter Kit','https://debeatzgh1.github.io/Decode-AI-starter-kit-/')">
             Open Here
          </a>

          <a href="https://debeatzgh1.github.io/Decode-AI-starter-kit-/"
             target="_blank"
             class="open-outside">
             External
          </a>
        </div>
      </div>
    </div>

    <div class="card searchable">
      <div class="preview">
        <iframe src="https://debeatzgh1.github.io/The-Ultimate-Guide-to-Side-Hustle/"></iframe>
      </div>

      <div class="card-content">
        <span class="category-tag">Side Hustle UI</span>

        <h3>Ultimate Side Hustle Guide</h3>

        <p>
          Professional side hustle interface with monetization ideas,
          online business tools, and startup-focused resources.
        </p>

        <div class="card-buttons">
          <a href="#" class="open-inside"
             onclick="openViewer('Ultimate Side Hustle Guide','https://debeatzgh1.github.io/The-Ultimate-Guide-to-Side-Hustle/')">
             Open Here
          </a>

          <a href="https://debeatzgh1.github.io/The-Ultimate-Guide-to-Side-Hustle/"
             target="_blank"
             class="open-outside">
             External
          </a>
        </div>
      </div>
    </div>

    <div class="card searchable">
      <div class="preview">
        <iframe src="https://debeatzgh1.github.io/posts/"></iframe>
      </div>

      <div class="card-content">
        <span class="category-tag">Content Hub</span>

        <h3>Posts Interface</h3>

        <p>
          Responsive blog and content experience featuring modern layouts,
          visual presentation blocks, and mobile optimized UI sections.
        </p>

        <div class="card-buttons">
          <a href="#" class="open-inside"
             onclick="openViewer('Posts Interface','https://debeatzgh1.github.io/posts/')">
             Open Here
          </a>

          <a href="https://debeatzgh1.github.io/posts/"
             target="_blank"
             class="open-outside">
             External
          </a>
        </div>
      </div>
    </div>

  </div>
</section>

<!-- ENTERTAINMENT -->
<section class="section">

  <h2 class="section-title">Entertainment Interface UI</h2>

  <p class="section-desc">
    Creative widgets, smart iframe layouts, AI chat interfaces,
    popup experiences, and interactive entertainment components.
  </p>

  <div class="grid">

    <div class="card searchable">
      <div class="preview">
        <iframe src="https://debeatzgh1.github.io/ai-chat/"></iframe>
      </div>

      <div class="card-content">
        <span class="category-tag">AI Chat UI</span>

        <h3>AI Chat Interface</h3>

        <p>
          Modern AI assistant interface featuring sleek interaction blocks,
          responsive layouts, and immersive user experience.
        </p>

        <div class="card-buttons">
          <a href="#" class="open-inside"
             onclick="openViewer('AI Chat Interface','https://debeatzgh1.github.io/ai-chat/')">
             Open Here
          </a>

          <a href="https://debeatzgh1.github.io/ai-chat/"
             target="_blank"
             class="open-outside">
             External
          </a>
        </div>
      </div>
    </div>

    <div class="card searchable">
      <div class="preview">
        <iframe src="https://debeatzgh1.github.io/popup-html-page-generator-blogger/"></iframe>
      </div>

      <div class="card-content">
        <span class="category-tag">Popup Generator</span>

        <h3>Popup HTML Generator</h3>

        <p>
          Interactive popup creation interface with smart modal systems,
          blogging tools, and premium visual interactions.
        </p>

        <div class="card-buttons">
          <a href="#" class="open-inside"
             onclick="openViewer('Popup HTML Generator','https://debeatzgh1.github.io/popup-html-page-generator-blogger/')">
             Open Here
          </a>

          <a href="https://debeatzgh1.github.io/popup-html-page-generator-blogger/"
             target="_blank"
             class="open-outside">
             External
          </a>
        </div>
      </div>
    </div>

    <div class="card searchable">
      <div class="preview">
        <iframe src="https://debeatzgh1.github.io/-Floating-Dock-Smart-Iframe-Modal/#"></iframe>
      </div>

      <div class="card-content">
        <span class="category-tag">Smart Dock UI</span>

        <h3>Floating Dock Smart Modal</h3>

        <p>
          Advanced floating dock experience with iframe overlays,
          premium modal interactions, and responsive navigation tools.
        </p>

        <div class="card-buttons">
          <a href="#" class="open-inside"
             onclick="openViewer('Floating Dock Smart Modal','https://debeatzgh1.github.io/-Floating-Dock-Smart-Iframe-Modal/#')">
             Open Here
          </a>

          <a href="https://debeatzgh1.github.io/-Floating-Dock-Smart-Iframe-Modal/#"
             target="_blank"
             class="open-outside">
             External
          </a>
        </div>
      </div>
    </div>

  </div>
</section>

<!-- IFRAME VIEWER -->
<div class="viewer" id="viewer">

  <div class="viewer-top">
    <h2 id="viewerTitle">Preview</h2>

    <div class="viewer-actions">

      <a href="#" id="externalBtn" target="_blank">
        Open External
      </a>

      <button onclick="closeViewer()">
        Close
      </button>

    </div>
  </div>

  <iframe id="viewerFrame"></iframe>

</div>

<!-- WHATSAPP -->
<a class="whatsapp"
   href="https://wa.me/233270201181"
   target="_blank">
   💬
</a>

<!-- FOOTER -->
<footer class="footer">
  Designed for <strong>Kumah David</strong> • Premium FAQ Showcase UI
</footer>

<script>

/* IFRAME VIEWER */
function openViewer(title,url){

  document.getElementById("viewer").classList.add("active");

  document.getElementById("viewerTitle").innerText = title;

  document.getElementById("viewerFrame").src = url;

  document.getElementById("externalBtn").href = url;

}

function closeViewer(){

  document.getElementById("viewer").classList.remove("active");

  document.getElementById("viewerFrame").src = "";

}

/* SEARCH */
const searchInput = document.getElementById("searchInput");

searchInput.addEventListener("keyup", function(){

  let filter = this.value.toLowerCase();

  let cards = document.querySelectorAll(".searchable");

  cards.forEach(card => {

    let text = card.innerText.toLowerCase();

    if(text.includes(filter)){
      card.style.display = "block";
    } else {
      card.style.display = "none";
    }

  });

});

</script>

</body>
</html>
