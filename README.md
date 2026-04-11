<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Life Lab — AIで毎日をもっとうまく生きる</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Noto+Serif+JP:wght@400;600&family=Noto+Sans+JP:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --g50:#f2f8f4; --g100:#dff0e6; --g200:#b8ddc5;
    --g400:#6db88a; --g600:#3a8a58; --g800:#1f5236; --g900:#0f2e1e;
    --gray50:#f9f9f7; --gray100:#f0f0ec; --gray200:#e0e0d8; --gray400:#9a9a90;
    --text:#1e2820; --sub:#5a6055; --bg:#f7f9f7; --card:#fff; --border:#dce8e0;
    --r:12px; --sh:0 2px 12px rgba(30,40,32,.07);
  }
  *{box-sizing:border-box;margin:0;padding:0;}
  body{font-family:'Noto Sans JP',sans-serif;background:var(--bg);color:var(--text);line-height:1.8;font-size:16px;}

  header{background:var(--card);border-bottom:1px solid var(--border);padding:0 2rem;display:flex;align-items:center;justify-content:space-between;height:60px;position:sticky;top:0;z-index:100;}
  .logo{font-family:'Noto Serif JP',serif;font-size:19px;font-weight:600;color:var(--g800);}
  .logo em{color:var(--g400);font-style:normal;}
  nav{display:flex;gap:1.5rem;align-items:center;}
  nav a{text-decoration:none;color:var(--sub);font-size:14px;transition:color .2s;}
  nav a:hover{color:var(--g600);}
  .btn{background:var(--g600);color:#fff;border:none;padding:8px 16px;border-radius:8px;font-size:13px;cursor:pointer;font-family:inherit;font-weight:500;transition:background .2s;}
  .btn:hover{background:var(--g800);}
  .btn:disabled{opacity:.5;cursor:not-allowed;}

  .hero{background:linear-gradient(135deg,var(--g50) 0%,var(--g100) 100%);padding:3.5rem 2rem 2.5rem;text-align:center;border-bottom:1px solid var(--border);}
  .hero-badge{display:inline-block;background:var(--g100);color:var(--g800);font-size:12px;padding:3px 12px;border-radius:20px;border:1px solid var(--g200);margin-bottom:1rem;}
  .hero h1{font-family:'Noto Serif JP',serif;font-size:clamp(20px,4vw,34px);font-weight:600;color:var(--g900);margin-bottom:.75rem;line-height:1.5;}
  .hero p{color:var(--sub);font-size:14px;max-width:460px;margin:0 auto;}

  .wrap{max-width:900px;margin:0 auto;padding:2.5rem 1.5rem;display:grid;grid-template-columns:1fr 260px;gap:2rem;}
  @media(max-width:680px){.wrap{grid-template-columns:1fr;}.sidebar{display:none;}header{padding:0 1rem;}nav a{display:none;}}

  .sec-title{font-size:12px;font-weight:500;color:var(--g600);letter-spacing:.08em;text-transform:uppercase;margin-bottom:1.25rem;padding-bottom:.5rem;border-bottom:2px solid var(--g100);}

  .post-card{background:var(--card);border:1px solid var(--border);border-radius:var(--r);padding:1.5rem;margin-bottom:1.25rem;box-shadow:var(--sh);cursor:pointer;transition:box-shadow .2s,transform .2s;}
  .post-card:hover{box-shadow:0 4px 20px rgba(30,40,32,.12);transform:translateY(-2px);}
  .post-meta{display:flex;align-items:center;gap:10px;margin-bottom:.5rem;}
  .cat-badge{font-size:11px;padding:2px 10px;border-radius:12px;background:var(--g100);color:var(--g800);font-weight:500;}
  .post-date{font-size:12px;color:var(--gray400);}
  .post-title{font-family:'Noto Serif JP',serif;font-size:17px;font-weight:600;color:var(--text);margin-bottom:.5rem;line-height:1.5;}
  .post-excerpt{font-size:14px;color:var(--sub);line-height:1.75;}
  .post-footer{margin-top:1rem;display:flex;justify-content:flex-end;}
  .read-more{font-size:13px;color:var(--g600);font-weight:500;}
  .empty{text-align:center;padding:3rem 1rem;color:var(--gray400);font-size:15px;}
  .loading{text-align:center;padding:3rem 1rem;color:var(--gray400);font-size:14px;}

  .sb-card{background:var(--card);border:1px solid var(--border);border-radius:var(--r);padding:1.25rem;margin-bottom:1.25rem;box-shadow:var(--sh);}
  .sb-card h3{font-size:12px;font-weight:500;color:var(--g600);margin-bottom:.75rem;letter-spacing:.05em;}
  .tag-wrap{display:flex;flex-wrap:wrap;gap:6px;}
  .tag{font-size:12px;padding:4px 10px;border-radius:20px;background:var(--g50);border:1px solid var(--g200);color:var(--g800);cursor:pointer;transition:background .2s;}
  .tag:hover{background:var(--g100);}
  .tag.active{background:var(--g600);color:#fff;border-color:var(--g600);}
  .recent-ul{list-style:none;}
  .recent-ul li{padding:.45rem 0;border-bottom:1px solid var(--gray100);font-size:13px;color:var(--sub);cursor:pointer;transition:color .2s;}
  .recent-ul li:last-child{border-bottom:none;}
  .recent-ul li:hover{color:var(--g600);}

  .overlay{display:none;position:fixed;inset:0;background:rgba(10,30,20,.45);z-index:200;align-items:flex-start;justify-content:center;padding:2rem 1rem;overflow-y:auto;}
  .overlay.open{display:flex;}
  .modal{background:var(--card);border-radius:16px;padding:2rem;max-width:680px;width:100%;position:relative;margin:auto;}
  .btn-x{position:absolute;top:1rem;right:1rem;background:var(--gray100);border:none;border-radius:50%;width:32px;height:32px;font-size:18px;cursor:pointer;color:var(--sub);display:flex;align-items:center;justify-content:center;}
  .btn-x:hover{background:var(--gray200);}
  #modal-cat{font-size:12px;color:var(--g600);margin-bottom:.4rem;}
  #modal-title{font-family:'Noto Serif JP',serif;font-size:22px;font-weight:600;margin-bottom:.4rem;line-height:1.5;}
  #modal-date{font-size:13px;color:var(--gray400);margin-bottom:1.25rem;}
  #modal-body{font-size:15px;line-height:1.9;color:var(--text);white-space:pre-wrap;}

  .write-panel{background:var(--card);border-radius:16px;padding:2rem;max-width:660px;width:100%;margin:auto;position:relative;}
  .write-panel h2{font-family:'Noto Serif JP',serif;font-size:20px;margin-bottom:.75rem;color:var(--g900);}
  .write-panel p.desc{font-size:13px;color:var(--sub);margin-bottom:1.5rem;line-height:1.7;}
  .fg{margin-bottom:1.25rem;}
  .fl{display:block;font-size:13px;font-weight:500;color:var(--sub);margin-bottom:6px;}
  .fi,.fsel,.fta{width:100%;padding:10px 14px;border:1px solid var(--border);border-radius:8px;font-family:inherit;font-size:15px;color:var(--text);background:var(--gray50);outline:none;transition:border-color .2s;}
  .fi:focus,.fsel:focus,.fta:focus{border-color:var(--g400);background:#fff;}
  .fta{resize:vertical;min-height:220px;}
  .token-note{background:var(--g50);border:1px solid var(--g200);border-radius:8px;padding:12px 14px;font-size:13px;color:var(--g800);margin-bottom:1.25rem;line-height:1.7;}
  .token-note a{color:var(--g600);}
  .btn-pub{background:var(--g600);color:#fff;border:none;padding:12px 24px;border-radius:8px;font-size:15px;cursor:pointer;font-family:inherit;font-weight:500;transition:background .2s;width:100%;}
  .btn-pub:hover{background:var(--g800);}
  .btn-pub:disabled{opacity:.5;cursor:not-allowed;}
  .pub-status{font-size:13px;color:var(--g600);margin-top:.75rem;min-height:20px;text-align:center;}
  .pub-error{color:#c0392b;}

  footer{text-align:center;padding:2rem;font-size:13px;color:var(--gray400);border-top:1px solid var(--border);margin-top:2rem;}
</style>
</head>
<body>

<header>
  <div class="logo">AI<em>Life</em>Lab</div>
  <nav>
    <a href="#" onclick="filterCat(null);return false;">すべて</a>
    <a href="#">について</a>
    <button class="btn" onclick="openWrite()">＋ 記事を投稿</button>
  </nav>
</header>

<div class="hero">
  <div class="hero-badge">AIで毎日をもっとうまく生きる</div>
  <h1>AIツールの使い方・体験談を<br>わかりやすく発信するブログ</h1>
  <p>ChatGPT・Claude・画像生成AIなど、実際に使ってみた感想や活用法を正直にレポートします。</p>
</div>

<div class="wrap">
  <main>
    <div class="sec-title" id="sec-title">最新記事</div>
    <div id="posts-list"><div class="loading">記事を読み込み中...</div></div>
  </main>
  <aside class="sidebar">
    <div class="sb-card">
      <h3>カテゴリ</h3>
      <div class="tag-wrap" id="cat-cloud"></div>
    </div>
    <div class="sb-card">
      <h3>最近の記事</h3>
      <ul class="recent-ul" id="recent-ul"></ul>
    </div>
    <div class="sb-card">
      <h3>このブログについて</h3>
      <p style="font-size:13px;color:var(--sub);line-height:1.7;">AIツールを日常・筋トレ・ゲームに取り入れながら、生活を最適化する実験ブログです。</p>
    </div>
  </aside>
</div>

<footer>© 2026 AI Life Lab — AIで毎日をもっとうまく生きる</footer>

<div class="overlay" id="post-ov" onclick="if(event.target===this)closePost()">
  <div class="modal">
    <button class="btn-x" onclick="closePost()">✕</button>
    <div id="modal-cat"></div>
    <div id="modal-title"></div>
    <div id="modal-date"></div>
    <div id="modal-body"></div>
  </div>
</div>

<div class="overlay" id="write-ov" onclick="if(event.target===this)closeWrite()">
  <div class="write-panel">
    <button class="btn-x" onclick="closeWrite()">✕</button>
    <h2>新しい記事を投稿する</h2>
    <p class="desc">GitHubに直接保存されるので、どのデバイスからでも記事が読めます。</p>
    <div class="token-note">
      初回のみ：<a href="https://github.com/settings/tokens/new" target="_blank">GitHubのトークンページ</a>でトークンを発行してください。<br>
      スコープは <strong>repo</strong> にチェックを入れて生成し、下の欄に貼り付けてください。
    </div>
    <div class="fg">
      <label class="fl">GitHub Token（初回のみ・自動保存されます）</label>
      <input class="fi" id="w-token" type="password" placeholder="ghp_xxxxxxxxxxxxxxxxxxxx">
    </div>
    <div class="fg">
      <label class="fl">タイトル *</label>
      <input class="fi" id="w-title" placeholder="例：ChatGPTで筋トレメニューを自動化してみた">
    </div>
    <div class="fg">
      <label class="fl">カテゴリ</label>
      <select class="fsel" id="w-cat">
        <option value="AI活用">AI活用</option>
        <option value="ツール紹介">ツール紹介</option>
        <option value="ChatGPT">ChatGPT</option>
        <option value="Claude">Claude</option>
        <option value="画像生成AI">画像生成AI</option>
        <option value="筋トレ×AI">筋トレ×AI</option>
        <option value="ゲーム×AI">ゲーム×AI</option>
        <option value="雑記">雑記</option>
      </select>
    </div>
    <div class="fg">
      <label class="fl">本文 *</label>
      <textarea class="fta" id="w-body" placeholder="ここに記事を書いてください。"></textarea>
    </div>
    <button class="btn-pub" id="btn-pub" onclick="publish()">記事を公開する</button>
    <div class="pub-status" id="pub-status"></div>
  </div>
</div>

<script>
const GH_USER = 'babble1-max';
const GH_REPO = 'babble1.github.io';
const POSTS_FILE = 'posts.json';
const API = `https://api.github.com/repos/${GH_USER}/${GH_REPO}/contents/${POSTS_FILE}`;

let posts = [];
let activeCat = null;

// 日本語対応のBase64エンコード
function toBase64(str) {
  const bytes = new TextEncoder().encode(str);
  let bin = '';
  bytes.forEach(b => bin += String.fromCharCode(b));
  return btoa(bin);
}

// 日本語対応のBase64デコード
function fromBase64(b64) {
  const bin = atob(b64.replace(/\n/g, ''));
  const bytes = new Uint8Array(bin.length);
  for (let i = 0; i < bin.length; i++) bytes[i] = bin.charCodeAt(i);
  return new TextDecoder().decode(bytes);
}

function getToken() {
  const input = document.getElementById('w-token').value.trim();
  if (input) { localStorage.setItem('gh_token', input); return input; }
  return localStorage.getItem('gh_token') || '';
}

async function loadPosts() {
  try {
    const res = await fetch(API + '?t=' + Date.now());
    if (res.status === 404) { posts = getSamples(); render(); return; }
    const data = await res.json();
    posts = JSON.parse(fromBase64(data.content));
    render();
  } catch (e) {
    posts = getSamples();
    render();
  }
}

async function publish() {
  const token  = getToken();
  const title  = document.getElementById('w-title').value.trim();
  const body   = document.getElementById('w-body').value.trim();
  const cat    = document.getElementById('w-cat').value;
  const status = document.getElementById('pub-status');
  const btn    = document.getElementById('btn-pub');

  if (!token) { status.textContent = 'GitHubトークンを入力してください'; status.className = 'pub-status pub-error'; return; }
  if (!title) { status.textContent = 'タイトルを入力してください'; status.className = 'pub-status pub-error'; return; }
  if (!body)  { status.textContent = '本文を入力してください'; status.className = 'pub-status pub-error'; return; }

  btn.disabled = true;
  status.textContent = '保存中...';
  status.className = 'pub-status';

  const newPost  = { id: Date.now(), cat, date: new Date().toISOString().slice(0,10), title, body };
  const newPosts = [...posts, newPost];
  const content  = toBase64(JSON.stringify(newPosts, null, 2));

  try {
    let sha = '';
    const check = await fetch(API, { headers: { Authorization: 'token ' + token } });
    if (check.ok) { const d = await check.json(); sha = d.sha; }

    const payload = { message: `add: ${title}`, content };
    if (sha) payload.sha = sha;

    const res = await fetch(API, {
      method: 'PUT',
      headers: { Authorization: 'token ' + token, 'Content-Type': 'application/json' },
      body: JSON.stringify(payload)
    });

    if (res.ok) {
      posts = newPosts;
      render();
      closeWrite();
      document.getElementById('w-title').value = '';
      document.getElementById('w-body').value  = '';
      status.textContent = '';
    } else {
      const err = await res.json();
      status.textContent = 'エラー：' + (err.message || '投稿に失敗しました');
      status.className = 'pub-status pub-error';
    }
  } catch (e) {
    status.textContent = '通信エラーが発生しました';
    status.className = 'pub-status pub-error';
  } finally {
    btn.disabled = false;
  }
}

function render() {
  const list    = document.getElementById('posts-list');
  const secttl  = document.getElementById('sec-title');
  const recent  = document.getElementById('recent-ul');
  const cloud   = document.getElementById('cat-cloud');
  const filtered = activeCat ? posts.filter(p => p.cat === activeCat) : posts;
  secttl.textContent = activeCat ? `カテゴリ：${activeCat}` : '最新記事';
  list.innerHTML = filtered.length === 0
    ? '<div class="empty">まだ記事がありません。右上の「＋ 記事を投稿」から書いてみましょう。</div>'
    : [...filtered].reverse().map(p => `
        <div class="post-card" onclick="openPost(${p.id})">
          <div class="post-meta"><span class="cat-badge">${p.cat}</span><span class="post-date">${p.date}</span></div>
          <div class="post-title">${p.title}</div>
          <div class="post-excerpt">${p.body.substring(0,120)}…</div>
          <div class="post-footer"><span class="read-more">続きを読む →</span></div>
        </div>`).join('');
  recent.innerHTML = [...posts].reverse().slice(0,5).map(p => `<li onclick="openPost(${p.id})">${p.title}</li>`).join('');
  const cats = [...new Set(posts.map(p => p.cat))];
  cloud.innerHTML = cats.map(c => `<span class="tag${activeCat===c?' active':''}" onclick="filterCat('${c}')">${c}</span>`).join('');
}

function filterCat(cat) { activeCat = (activeCat === cat) ? null : cat; render(); }

function openPost(id) {
  const p = posts.find(x => x.id === id); if (!p) return;
  document.getElementById('modal-cat').textContent   = p.cat;
  document.getElementById('modal-title').textContent = p.title;
  document.getElementById('modal-date').textContent  = p.date;
  document.getElementById('modal-body').textContent  = p.body;
  document.getElementById('post-ov').classList.add('open');
}
function closePost()  { document.getElementById('post-ov').classList.remove('open'); }
function openWrite()  {
  document.getElementById('write-ov').classList.add('open');
  const saved = localStorage.getItem('gh_token');
  if (saved) document.getElementById('w-token').value = saved;
}
function closeWrite() {
  document.getElementById('write-ov').classList.remove('open');
  document.getElementById('pub-status').textContent = '';
}

function getSamples() {
  return [
    { id:1, cat:'ChatGPT', date:'2026-04-06',
      title:'ChatGPTで筋トレメニューを自動化してみた【3週間レポート】',
      body:'ChatGPTに筋トレメニューを組んでもらう試みを始めて3週間が経ちました。\n\n「体重70kg、週4回、器具はダンベルのみ、目標は筋肥大」と入力するだけで、かなり実用的なメニューが返ってきました。\n\n【4分割メニューの中身】\n胸・背中・脚・肩の4分割で組んでもらい、各種目のセット数・レップ数・インターバルまで指定してくれます。\n\n【おすすめのプロンプト】\n「私は〇〇kg、週〇回トレーニングできます。目標は〇〇です。科学的根拠のある筋トレメニューを種目・セット・レップ・インターバルつきで作ってください」'},
    { id:2, cat:'ツール紹介', date:'2026-04-03',
      title:'無料で使えるAIツール5選【2026年版・初心者向け】',
      body:'AIを使い始めたいけど、どれから手をつければいいかわからない。そんな人のために、実際に使ってみた無料ツールを5つ紹介します。\n\n【1. ChatGPT（無料版）】\n文章作成・質問応答に万能。まずここから始めるのがおすすめです。\n\n【2. Claude.ai（無料版）】\n長い文章の要約や丁寧な説明が得意。ブログ記事の構成を考えてもらうのに向いています。\n\n【3. Canva AI】\nデザイン初心者でも画像やSNS投稿をAIで自動生成できます。\n\n【4. Perplexity AI】\nAIがウェブを検索して答えてくれる検索エンジンです。\n\n【5. Notion AI】\nメモやタスク管理アプリのNotionにAI機能が内蔵されています。'},
    { id:3, cat:'ゲーム×AI', date:'2026-03-28',
      title:'グラブルの周回効率をAIで計算してみたら意外な結果に',
      body:'グラブルの素材集め、どのクエストが一番効率いいの？という疑問をChatGPTにぶつけてみました。\n\n【やったこと】\n各クエストのドロップ率・消費APをスプレッドシートにまとめてChatGPTに貼り付け、「AP当たりの期待ドロップ数が最も高いクエストはどれ？」と聞きました。\n\n【結果】\n自分の感覚とは違うクエストが最効率と判明しました。計算で見える化すると気づきが多いです。\n\nOWのキャラ相性分析にも応用できそうなので、次回試してみます。'}
  ];
}

document.addEventListener('keydown', e => { if (e.key === 'Escape') { closePost(); closeWrite(); } });
loadPosts();
</script>
</body>
</html>
