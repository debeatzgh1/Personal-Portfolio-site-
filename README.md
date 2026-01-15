import { useEffect, useState } from "react";

export default function App() {
  const [url, setUrl] = useState("https://debeatzgh1.github.io/Home-/");
  const [view, setView] = useState("iframe");
  const [menu, setMenu] = useState(false);
  const [theme, setTheme] = useState("dark");
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    const saved = localStorage.getItem("theme");
    if (saved) setTheme(saved);
  }, []);

  useEffect(() => {
    localStorage.setItem("theme", theme);
  }, [theme]);

  const loadBlogs = async () => {
    setView("blogs");
    const feeds = [
      "https://debeatzgh.wordpress.com/feed/",
      "http://beatzde4.blogspot.com/feeds/posts/default?alt=rss",
    ];

    const all = [];
    for (const f of feeds) {
      const res = await fetch(
        `https://api.rss2json.com/v1/api.json?rss_url=${f}`
      );
      const data = await res.json();
      all.push(...data.items.slice(0, 4));
    }
    setPosts(all);
  };

  const openIframe = (link) => {
    setView("iframe");
    setUrl(link);
  };

  return (
    <div className={theme === "light" ? "light" : "dark"}>
      <header className="header">
        <button onClick={() => setMenu(true)}>☰</button>
        <h1>David Kumah</h1>
        <button onClick={() => setTheme(theme === "dark" ? "light" : "dark")}>
          🌗
        </button>
      </header>

      {menu && (
        <div className="menu" onClick={() => setMenu(false)}>
          <div className="panel" onClick={(e) => e.stopPropagation()}>
            <button onClick={() => openIframe("https://debeatzgh1.github.io/Home-/")}>🤖 AI Hub</button>
            <button onClick={() => openIframe("https://debeatzgh1.github.io/-My-Brand-Online-Digital-Products-Affiliate-Shop/")}>🛒 Products</button>
            <button onClick={() => openIframe("https://debeatzgh1.github.io/The-Ultimate-Guide-to-Side-Hustle/")}>🚀 Hustles</button>
            <button onClick={loadBlogs}>📰 Blogs</button>
          </div>
        </div>
      )}

      <nav className="tabs">
        <button onClick={() => openIframe("https://debeatzgh1.github.io/Home-/")}>🤖 AI</button>
        <button onClick={() => openIframe("https://debeatzgh1.github.io/-My-Brand-Online-Digital-Products-Affiliate-Shop/")}>🛒 Products</button>
        <button onClick={() => openIframe("https://debeatzgh1.github.io/The-Ultimate-Guide-to-Side-Hustle/")}>🚀 Hustles</button>
        <button onClick={loadBlogs}>📰 Blogs</button>
        <button onClick={() => window.open("https://milkshake.app/", "_blank")}>🔗 Links</button>
      </nav>

      <main className="viewer">
        {view === "iframe" && <iframe src={url} title="viewer" />}

        {view === "blogs" && (
          <div className="feed">
            {posts.map((p, i) => (
              <a key={i} href={p.link} target="_blank">
                <strong>{p.title}</strong>
                <small>{p.pubDate.split(" ")[0]}</small>
              </a>
            ))}
          </div>
        )}
      </main>

      <style>{`
        body { margin:0 }
        .dark { --bg:#020617; --card:#111827; --text:#e5e7eb }
        .light{ --bg:#f8fafc; --card:#f1f5f9; --text:#0f172a }
        div{ background:var(--bg); color:var(--text); min-height:100vh }
        .header{ display:flex; justify-content:space-between; padding:12px }
        .tabs{ display:flex; gap:8px; padding:10px; overflow-x:auto }
        button{ background:var(--card); border:none; padding:8px 12px; border-radius:10px; color:var(--text) }
        .viewer{ height:calc(100vh - 120px) }
        iframe{ width:100%; height:100%; border:none }
        .menu{ position:fixed; inset:0; background:rgba(0,0,0,.6) }
        .panel{ width:260px; background:var(--card); padding:16px }
        .feed{ padding:12px; display:grid; gap:10px }
        .feed a{ background:var(--card); padding:10px; border-radius:12px; color:var(--text); text-decoration:none }
      `}</style>
    </div>
  );
}
