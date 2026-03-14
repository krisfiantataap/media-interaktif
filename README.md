<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Media Interaktif — Sistem Komputer (Kelas 7)</title>
<style>
  :root{
    --bg: #0f172a; /* slate-900 */
    --card: #111827; /* gray-900 */
    --muted: #1f2937; /* gray-800 */
    --text: #e5e7eb; /* gray-200 */
    --accent: #06b6d4; /* cyan-500 */
    --accent-2: #8b5cf6; /* violet-500 */
    --good: #22c55e; /* green-500 */
    --bad: #ef4444; /* red-500 */
    --warn: #f59e0b; /* amber-500 */
  }
  *{box-sizing:border-box}
  body{margin:0;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,Ubuntu,'Helvetica Neue',Arial,'Noto Sans',sans-serif;background:linear-gradient(180deg,#0b1220,#0f172a 30%,#0b1220);color:var(--text)}
  header{position:sticky;top:0;z-index:10;background:rgba(15,23,42,0.9);backdrop-filter: blur(8px);border-bottom:1px solid #1f2937}
  .wrap{max-width:1100px;margin:0 auto;padding:16px}
  .title{display:flex;align-items:center;gap:12px}
  .logo{width:40px;height:40px;display:grid;place-items:center;border-radius:12px;background:linear-gradient(135deg,var(--accent),var(--accent-2));box-shadow:0 8px 30px rgba(6,182,212,0.2)}
  h1{font-size: clamp(18px,3.2vw,26px);margin:0}
  .meta{display:flex;gap:12px;flex-wrap:wrap;margin-top:10px}
  .badge{background:var(--muted);border:1px solid #283244;padding:8px 12px;border-radius:999px;display:flex;align-items:center;gap:8px}
  .bar{height:10px;background:#111827;border:1px solid #283244;border-radius:999px;overflow:hidden}
  .bar>span{display:block;height:100%;width:0%;background:linear-gradient(90deg,var(--accent),var(--accent-2));transition:width .6s ease}

  main{max-width:1100px;margin:18px auto;padding:16px}
  .screen{display:none}
  .screen.active{display:block}

  .grid{display:grid;gap:16px}
  .grid.cols-2{grid-template-columns:repeat(2,1fr)}
  .grid.cols-3{grid-template-columns:repeat(3,1fr)}
  @media (max-width:900px){
    .grid.cols-2,.grid.cols-3{grid-template-columns:1fr}
  }

  .card{background:linear-gradient(180deg,#0b1220,#0e1628);border:1px solid #1f2a3d;border-radius:16px;padding:16px;box-shadow:0 8px 30px rgba(0,0,0,0.25)}
  .card h2,.card h3{margin-top:0}

  .btn{background:linear-gradient(180deg,#0ea5e9,#06b6d4);border:none;color:white;padding:12px 16px;border-radius:12px;font-weight:700;cursor:pointer;transition:transform .1s,opacity .2s}
  .btn:hover{transform:translateY(-1px)}
  .btn.secondary{background:#1f2937;color:#e5e7eb;border:1px solid #334155}
  .btn.ghost{background:transparent;border:1px dashed #334155}
  .btn.small{padding:8px 12px;font-size:14px}

  .tip{color:#cbd5e1}
  .kbd{padding:2px 8px;border:1px solid #334155;border-radius:8px;background:#0b1220;color:#e5e7eb}

  .flex{display:flex;gap:12px;align-items:center}
  .flex-between{display:flex;justify-content:space-between;align-items:center;gap:12px}

  /* Drag & Drop */
  .bank,.bins{display:grid;gap:12px}
  .bank{grid-template-columns:repeat(auto-fit,minmax(140px,1fr))}
  .item{background:linear-gradient(180deg,#0f1a2e,#0b1220);border:1px solid #243247;border-radius:12px;padding:12px;min-height:76px;display:flex;flex-direction:column;justify-content:center;align-items:center;text-align:center;gap:6px;cursor:grab}
  .item.dragging{opacity:.6}
  .item .emoji{font-size:22px}
  .bin{min-height:150px;border:2px dashed #334155;border-radius:14px;padding:10px;display:flex;flex-wrap:wrap;gap:10px;align-content:flex-start}
  .bin.over{border-color:var(--accent)}
  .bin h4{margin:6px 0 10px 0;width:100%}

  /* Quiz */
  .q{border:1px solid #233146;background:#0b1220;border-radius:12px;padding:12px;margin:10px 0}
  .q h4{margin:0 0 8px}
  .opts{display:grid;gap:8px}
  .opt{padding:10px;border:1px solid #2a3a52;border-radius:10px;cursor:pointer}
  .opt.correct{border-color:var(--good);background:#082b19}
  .opt.wrong{border-color:var(--bad);background:#2b0c0c}
  .explain{font-size:14px;color:#cbd5e1;margin-top:6px}

  /* Modal */
  .modal{position:fixed;inset:0;display:none;place-items:center;background:rgba(0,0,0,.5)}
  .modal.show{display:grid}
  .modal .dialog{background:#0b1220;border:1px solid #203049;border-radius:16px;padding:18px;max-width:560px}

  /* Badges */
  .badge-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:12px}
  .ach{background:#0b1220;border:1px solid #233146;border-radius:14px;padding:12px;text-align:center;opacity:.8}
  .ach.earned{opacity:1;outline:2px solid var(--accent)}
  .ach .big{font-size:28px}

  .feedback{margin:10px 0;padding:12px;border-radius:12px;border:1px solid #2a3a52}
  .feedback.good{background:#082b19;border-color:var(--good)}
  .feedback.bad{background:#2b0c0c;border-color:var(--bad)}

  footer{max-width:1100px;margin:30px auto 60px;padding:0 16px;color:#94a3b8}

  /* Simple animated confetti using emojis */
  .confetti{position:fixed;inset:0;pointer-events:none;overflow:hidden}
  .confetti span{position:absolute;animation:fall 2.8s linear forwards;font-size:22px}
  @keyframes fall{to{transform:translateY(100vh) rotate(720deg);opacity:0}}
</style>
</head>
<body>
<header>
  <div class="wrap">
    <div class="flex-between">
      <div class="title">
        <div class="logo" aria-hidden="true">💻</div>
        <div>
          <h1>Media Interaktif: Sistem Komputer (Kelas 7)</h1>
          <div class="tip">Belajar seru tentang <b>Hardware</b> & <b>Software</b> dengan permainan, kuis, dan studi kasus.</div>
        </div>
      </div>
      <div class="meta">
        <div class="badge" id="score"><span>⭐</span> Poin: <b><span id="points">0</span></b></div>
        <div class="badge"><span>🎚️</span> Level: <b><span id="level">1</span>/4</b></div>
        <div class="badge"><span>🎖️</span> Badges: <b><span id="badgeCount">0</span></b></div>
      </div>
    </div>
    <div class="bar" aria-label="Kemajuan belajar, dalam persen">
      <span id="progress" style="width:0%"></span>
    </div>
  </div>
</header>

<main>
  <!-- Intro -->
  <section id="intro" class="screen active">
    <div class="grid cols-2">
      <div class="card">
        <h2>Halo! Siap jadi <span style="color:var(--accent)">Master Sistem Komputer</span>?</h2>
        <p>Kamu akan menjelajah dunia komputer melalui 3 level: <b>Hardware</b>, <b>Software</b>, dan <b>Studi Kasus</b>. Setiap level punya misi, poin, dan lencana. Di akhir, ada <b>Evaluasi Akhir</b> untuk menguji pemahamanmu.</p>
        <ul>
          <li>🎯 <b>Tujuan</b>: mengenal <b>perangkat masukan (input)</b>, <b>pemrosesan (process)</b>, <b>keluaran (output)</b>, serta jenis <b>software</b>.</li>
          <li>🕹️ <b>Cara main</b>: seret-lepas, pilih jawaban, dan pecahkan masalah dari kehidupan nyata.</li>
          <li>🏅 <b>Gamifikasi</b>: kumpulkan poin, naik level, dan raih lencana khusus!</li>
        </ul>
        <div class="flex" style="margin-top:10px">
          <button class="btn" onclick="startJourney()">Mulai Petualangan</button>
          <button class="btn secondary" onclick="showScreen('peta')">Lihat Peta Konsep</button>
          <button class="btn ghost" onclick="resetProgress()">🔁 Reset Progres</button>
        </div>
      </div>
      <div class="card">
        <h3>Peta Konsep (ringkas)</h3>
        <div id="miniMap"></div>
        <p class="tip">Klik <b>Lihat Peta Konsep</b> untuk versi interaktifnya.</p>
      </div>
    </div>
  </section>

  <!-- Peta Konsep -->
  <section id="peta" class="screen">
    <div class="card">
      <h2>🧭 Peta Konsep: Sistem Komputer</h2>
      <p><b>Sistem komputer</b> adalah gabungan <b>hardware</b> (perangkat keras) dan <b>software</b> (perangkat lunak) yang bekerja dengan alur <b>Input → Process → Output</b>.</p>
      <div class="grid cols-2">
        <div class="card">
          <h3>Hardware (Perangkat Keras)</h3>
          <ul>
            <li><b>Input</b>: keyboard ⌨️, mouse 🖱️, mikrofon 🎤, kamera/webcam 📷, scanner 🖨️</li>
            <li><b>Process</b>: CPU 🧠, GPU 🎮, RAM ⚡, motherboard 🧩</li>
            <li><b>Output</b>: monitor 🖥️, printer 🖨️, speaker 🔊, proyektor 📽️, headset 🎧</li>
          </ul>
        </div>
        <div class="card">
          <h3>Software (Perangkat Lunak)</h3>
          <ul>
            <li><b>Sistem Operasi</b>: Windows, Linux, Android, iOS</li>
            <li><b>Aplikasi</b>: pengolah kata, peramban web, editor gambar/video, presentasi</li>
            <li><b>Pemrograman</b>: bahasa (Python, C++), IDE (VS Code), visual (Scratch)</li>
          </ul>
        </div>
      </div>
      <div class="flex" style="margin-top:10px"><button class="btn" onclick="showScreen('lvl1')">Mulai Level 1: Hardware</button><button class="btn secondary" onclick="showScreen('intro')">Kembali</button></div>
    </div>
  </section>

  <!-- Level 1: Hardware IPO Drag & Drop -->
  <section id="lvl1" class="screen">
    <div class="card">
      <div class="flex-between">
        <h2>Level 1 — Klasifikasi Hardware: Input · Process · Output</h2>
        <span class="tip">Seret setiap kartu ke kategori yang tepat. +10 benar, -5 salah.</span>
      </div>
      <div class="grid cols-2">
        <div>
          <h3>Bank Perangkat</h3>
          <div id="bank1" class="bank"></div>
        </div>
        <div>
          <h3>Taruh di Kategori</h3>
          <div class="bins">
            <div class="bin" data-accept="Input"><h4>🟦 Input</h4></div>
            <div class="bin" data-accept="Process"><h4>🟩 Process</h4></div>
            <div class="bin" data-accept="Output"><h4>🟨 Output</h4></div>
          </div>
        </div>
      </div>
      <div id="fb1" class="feedback" style="display:none"></div>
      <div class="flex" style="margin-top:10px">
        <button class="btn" onclick="checkLvl1()">Cek Jawaban</button>
        <button class="btn secondary" onclick="shuffleLvl1()">Acak Ulang</button>
        <button class="btn ghost" onclick="hintLvl1()">💡 Petunjuk</button>
      </div>
    </div>

    <div class="card">
      <h3>Kuis Formatif Level 1</h3>
      <div id="quiz1"></div>
      <div class="flex" style="margin-top:10px">
        <button class="btn" onclick="gradeQuiz('quiz1')">Periksa Kuis</button>
        <button class="btn secondary" onclick="showScreen('lvl2')">Lanjut ke Level 2</button>
      </div>
    </div>
  </section>

  <!-- Level 2: Software Sorting -->
  <section id="lvl2" class="screen">
    <div class="card">
      <div class="flex-between">
        <h2>Level 2 — Klasifikasi Software</h2>
        <span class="tip">Seret ke: Sistem Operasi · Aplikasi · Pemrograman</span>
      </div>
      <div class="grid cols-2">
        <div>
          <h3>Bank Software</h3>
          <div id="bank2" class="bank"></div>
        </div>
        <div>
          <h3>Taruh di Kategori</h3>
          <div class="bins">
            <div class="bin" data-accept="Sistem Operasi"><h4>🟦 Sistem Operasi</h4></div>
            <div class="bin" data-accept="Aplikasi"><h4>🟩 Aplikasi</h4></div>
            <div class="bin" data-accept="Pemrograman"><h4>🟨 Pemrograman</h4></div>
          </div>
        </div>
      </div>
      <div id="fb2" class="feedback" style="display:none"></div>
      <div class="flex" style="margin-top:10px">
        <button class="btn" onclick="checkLvl2()">Cek Jawaban</button>
        <button class="btn secondary" onclick="shuffleLvl2()">Acak Ulang</button>
        <button class="btn ghost" onclick="hintLvl2()">💡 Petunjuk</button>
      </div>
    </div>

    <div class="card">
      <h3>Kuis Formatif Level 2</h3>
      <div id="quiz2"></div>
      <div class="flex" style="margin-top:10px">
        <button class="btn" onclick="gradeQuiz('quiz2')">Periksa Kuis</button>
        <button class="btn secondary" onclick="showScreen('lvl3')">Lanjut ke Level 3</button>
      </div>
    </div>
  </section>

  <!-- Level 3: Studi Kasus Real-Life -->
  <section id="lvl3" class="screen">
    <div class="card">
      <h2>Level 3 — Studi Kasus Kehidupan Nyata</h2>
      <p>Pilih kombinasi hardware & software yang paling cocok. Dapat poin +15 tiap kasus.</p>
      <div id="cases"></div>
      <div id="fb3" class="feedback" style="display:none"></div>
      <div class="flex" style="margin-top:10px">
        <button class="btn" onclick="checkCases()">Cek Pilihan</button>
        <button class="btn secondary" onclick="showScreen('eval')">Ke Evaluasi Akhir</button>
      </div>
    </div>

    <div class="card">
      <h3>Kuis Formatif Level 3</h3>
      <div id="quiz3"></div>
      <div class="flex" style="margin-top:10px">
        <button class="btn" onclick="gradeQuiz('quiz3')">Periksa Kuis</button>
      </div>
    </div>
  </section>

  <!-- Evaluasi Akhir -->
  <section id="eval" class="screen">
    <div class="card">
      <h2>Evaluasi Akhir — Sistem Komputer</h2>
      <p>Jawablah 10 soal berikut. Poin +10 per jawaban benar. Raih lencana <b>🎓 Bintang Evaluasi</b> jika skormu ≥ 90.</p>
      <div id="finalQuiz"></div>
      <div class="flex" style="margin-top:10px">
        <button class="btn" onclick="gradeFinal()">Kumpulkan & Nilai</button>
        <button class="btn secondary" onclick="showScreen('intro')">Kembali ke Beranda</button>
      </div>
    </div>

    <div class="card">
      <h3>Riwayat & Lencana</h3>
      <div class="badge-grid" id="badgeGrid"></div>
      <div class="flex" style="margin-top:10px">
        <button class="btn ghost" onclick="resetProgress()">🔁 Reset Progres</button>
      </div>
    </div>
  </section>
</main>

<footer>
  <p><b>Catatan Guru</b>: Media ini menyertakan <i>kuis formatif</i> per level dan <i>evaluasi sumatif</i> di akhir. Progres & poin tersimpan otomatis di <code>localStorage</code>. Cocok ditayangkan via proyektor; siswa dapat mengakses via gawai masing-masing.</p>
  <p>👉 Saran penggunaan nyata: kaitkan studi kasus dengan kegiatan sekolah (majalah dinding digital, lomba presentasi, vlog ekstrakurikuler), sehingga siswa memahami <b>mengapa</b> memilih hardware & software tertentu.</p>
</footer>

<div id="modal" class="modal" role="dialog" aria-modal="true" aria-labelledby="modalTitle">
  <div class="dialog">
    <h3 id="modalTitle">🎉 Selamat!</h3>
    <div id="modalBody">Kamu meraih lencana baru.</div>
    <div class="flex" style="margin-top:10px"><button class="btn" onclick="hideModal()">Tutup</button></div>
  </div>
</div>
<div class="confetti" id="confetti" aria-hidden="true"></div>
<div aria-live="polite" style="position:absolute;left:-9999px;top:auto;width:1px;height:1px;overflow:hidden" id="live"></div>

<script>
// ==========================
// State & Gamification
// ==========================
const state = {
  points: 0,
  level: 1, // 1..4 (4 = selesai evaluasi)
  badges: new Set(),
  progress: 0
};

const BADGES = [
  { key: 'ipo', name: '🔷 Pemula IPO', desc: 'Menuntaskan Level Hardware' },
  { key: 'soft', name: '🟣 Master Software', desc: 'Menuntaskan Level Software' },
  { key: 'cases', name: '🧩 Solver Kasus', desc: 'Menuntaskan Level Studi Kasus' },
  { key: 'final90', name: '🎓 Bintang Evaluasi', desc: 'Skor Evaluasi Akhir ≥ 90' },
];

function saveState(){
  const toSave = { points: state.points, level: state.level, badges: Array.from(state.badges), progress: state.progress };
  localStorage.setItem('siskom7', JSON.stringify(toSave));
}
function loadState(){
  const raw = localStorage.getItem('siskom7');
  if(!raw) return;
  try{
    const data = JSON.parse(raw);
    state.points = data.points||0;
    state.level = data.level||1;
    state.badges = new Set(data.badges||[]);
    state.progress = data.progress||0;
  }catch(e){console.warn('restore fail', e)}
}
function updateUI(){
  document.getElementById('points').textContent = state.points;
  document.getElementById('level').textContent = state.level;
  document.getElementById('badgeCount').textContent = state.badges.size;
  document.getElementById('progress').style.width = Math.min(100,state.progress)+"%";
  renderBadges();
  saveState();
}
function addPoints(n){
  state.points = Math.max(0, state.points + n);
  flashScore(n);
  updateUI();
}
function setProgress(p){ state.progress = Math.max(state.progress, p); updateUI(); }
function awardBadge(key){
  const found = BADGES.find(b=>b.key===key);
  if(found && !state.badges.has(key)){
    state.badges.add(key);
    showModal('🎖️ Lencana Baru!', `<p>Kamu mendapatkan lencana <b>${found.name}</b><br><small>${found.desc}</small></p>`);
    confettiBurst();
    updateUI();
  }
}
function nextLevel(min){
  if(state.level < min){ state.level = min; updateUI(); }
}
function flashScore(delta){
  const el = document.getElementById('score');
  el.style.transition = 'transform .2s';
  el.style.transform = 'scale(1.06)';
  setTimeout(()=>{el.style.transform='scale(1)';},200);
}

function showModal(title, html){
  document.getElementById('modalTitle').textContent = title;
  document.getElementById('modalBody').innerHTML = html;
  document.getElementById('modal').classList.add('show');
}
function hideModal(){ document.getElementById('modal').classList.remove('show'); }

function confettiBurst(){
  const c = document.getElementById('confetti');
  for(let i=0;i<30;i++){
    const s = document.createElement('span');
    s.textContent = ['🎉','✨','🎊','💫','🌟'][Math.floor(Math.random()*5)];
    s.style.left = Math.random()*100 + 'vw';
    s.style.top = '-10vh';
    s.style.animationDelay = (Math.random()*0.8)+'s';
    c.appendChild(s);
    setTimeout(()=>s.remove(), 3200);
  }
}

function showScreen(id){
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  window.scrollTo({top:0,behavior:'smooth'});
}
function startJourney(){ showScreen('peta'); }
function resetProgress(){
  if(!confirm('Hapus semua progres, poin, dan lencana?')) return;
  localStorage.removeItem('siskom7');
  location.reload();
}

// ==========================
// Visual Mini Map (Intro)
// ==========================
function renderMiniMap(){
  const box = document.getElementById('miniMap');
  box.innerHTML = `
    <svg viewBox="0 0 600 240" style="width:100%;height:auto">
      <defs>
        <linearGradient id="g1" x1="0" y1="0" x2="1" y2="1">
          <stop offset="0%" stop-color="#06b6d4"/>
          <stop offset="100%" stop-color="#8b5cf6"/>
        </linearGradient>
      </defs>
      <rect x="10" y="10" width="580" height="220" rx="16" fill="#0b1220" stroke="#1f2a3d"/>
      <text x="300" y="40" fill="#e5e7eb" text-anchor="middle" font-size="18" font-weight="700">Sistem Komputer</text>
      <rect x="60" y="70" width="200" height="60" rx="12" fill="#0f1a2e" stroke="#243247"/>
      <text x="160" y="105" fill="#93c5fd" text-anchor="middle" font-weight="700">Hardware</text>
      <rect x="340" y="70" width="200" height="60" rx="12" fill="#0f1a2e" stroke="#243247"/>
      <text x="440" y="105" fill="#c4b5fd" text-anchor="middle" font-weight="700">Software</text>
      <line x1="160" y1="130" x2="160" y2="200" stroke="url(#g1)" stroke-width="2"/>
      <line x1="440" y1="130" x2="440" y2="200" stroke="url(#g1)" stroke-width="2"/>
      <text x="100" y="200" fill="#e5e7eb">Input</text>
      <text x="160" y="200" fill="#e5e7eb" text-anchor="middle">Process</text>
      <text x="220" y="200" fill="#e5e7eb" text-anchor="end">Output</text>
      <text x="390" y="200" fill="#e5e7eb">Sistem Operasi</text>
      <text x="440" y="200" fill="#e5e7eb" text-anchor="middle">Aplikasi</text>
      <text x="510" y="200" fill="#e5e7eb" text-anchor="end">Pemrograman</text>
    </svg>`;
}

// ==========================
// Level 1 — Hardware IPO
// ==========================
const hardwareItems = [
  {name:'Keyboard', cat:'Input', emoji:'⌨️', explain:'Keyboard memasukkan data huruf/angka.'},
  {name:'Mouse', cat:'Input', emoji:'🖱️', explain:'Mouse menggerakkan pointer dan memilih.'},
  {name:'Mikrofon', cat:'Input', emoji:'🎤', explain:'Mikrofon memasukkan suara.'},
  {name:'Webcam', cat:'Input', emoji:'📷', explain:'Webcam memasukkan gambar/video.'},
  {name:'Scanner', cat:'Input', emoji:'🖨️', explain:'Scanner mengubah dokumen fisik jadi digital.'},
  {name:'CPU', cat:'Process', emoji:'🧠', explain:'CPU memproses instruksi (otak komputer).'},
  {name:'GPU', cat:'Process', emoji:'🎮', explain:'GPU mempercepat grafis/perhitungan paralel.'},
  {name:'RAM', cat:'Process', emoji:'⚡', explain:'RAM menyimpan data kerja sementara selama proses.'},
  {name:'Motherboard', cat:'Process', emoji:'🧩', explain:'Papan utama yang menghubungkan komponen proses.'},
  {name:'Monitor', cat:'Output', emoji:'🖥️', explain:'Monitor menampilkan gambar/tampilan.'},
  {name:'Printer', cat:'Output', emoji:'🖨️', explain:'Printer menghasilkan keluaran di kertas.'},
  {name:'Speaker', cat:'Output', emoji:'🔊', explain:'Speaker mengeluarkan suara.'},
  {name:'Proyektor', cat:'Output', emoji:'📽️', explain:'Proyektor menampilkan ke layar besar.'},
];

function buildBank(elId, items, dndGroup){
  const el = document.getElementById(elId);
  el.innerHTML = '';
  items.forEach((it,i)=>{
    const card = document.createElement('div');
    card.className = 'item';
    card.draggable = true;
    card.dataset.name = it.name;
    card.dataset.cat = it.cat;
    card.dataset.group = dndGroup;
    card.innerHTML = `<div class="emoji">${it.emoji}</div><div><b>${it.name}</b></div>`;
    addDnD(card);
    el.appendChild(card);
  });
}

function addDnD(node){
  node.addEventListener('dragstart', e=>{
    node.classList.add('dragging');
    e.dataTransfer.setData('text/plain', JSON.stringify({ name: node.dataset.name, cat: node.dataset.cat, group: node.dataset.group }));
  });
  node.addEventListener('dragend', ()=>node.classList.remove('dragging'));
}

function makeBins(scope){
  document.querySelectorAll(`.bin`).forEach(bin=>{
    bin.addEventListener('dragover', e=>{
      e.preventDefault();
      const overOK = true; // allow, check on drop
      if(overOK) bin.classList.add('over');
    });
    bin.addEventListener('dragleave', ()=>bin.classList.remove('over'));
    bin.addEventListener('drop', e=>{
      e.preventDefault();
      bin.classList.remove('over');
      try{
        const data = JSON.parse(e.dataTransfer.getData('text/plain'));
        const item = document.querySelector(`.item[data-name="${CSS.escape(data.name)}"]`);
        if(item){ bin.appendChild(item); addPoints(0); live(`Ditaruh: ${data.name}`); }
      }catch(err){console.warn(err)}
    });
  });
}

function shuffle(a){ for(let i=a.length-1;i>0;i--){ const j=Math.floor(Math.random()*(i+1)); [a[i],a[j]]=[a[j],a[i]] } return a; }
function shuffleLvl1(){ buildBank('bank1', shuffle([...hardwareItems]), 'hw'); }
function hintLvl1(){
  showModal('Petunjuk Level 1', `<ul>
    <li><b>Input</b>: alat untuk <i>memasukkan</i> data ke komputer (contoh: keyboard, mouse, mikrofon).</li>
    <li><b>Process</b>: komponen yang <i>mengolah</i> data (contoh: CPU, RAM, GPU).</li>
    <li><b>Output</b>: perangkat untuk <i>menampilkan</i> hasil (contoh: monitor, speaker, printer).</li>
  </ul>`);
}

function checkLvl1(){
  let correct = 0, total = hardwareItems.length, msg = [];
  document.querySelectorAll('#lvl1 .bin').forEach(bin=>{
    const accept = bin.dataset.accept;
    bin.querySelectorAll('.item').forEach(it=>{
      if(it.dataset.cat===accept){ correct++; addPoints(10); msg.push(`✅ <b>${it.dataset.name}</b> benar (${accept}).`); }
      else { addPoints(-5); msg.push(`❌ <b>${it.dataset.name}</b> bukan ${accept}. <i>${it.dataset.cat}</i>.`); }
    })
  });
  const fb = document.getElementById('fb1');
  fb.style.display='block';
  fb.className = 'feedback ' + (correct>= Math.ceil(total*0.8) ? 'good' : 'bad');
  fb.innerHTML = `<b>Hasil:</b> ${correct}/${total}<br>` + msg.map(m=>`<div>${m}</div>`).join('');
  if(correct===total){ awardBadge('ipo'); nextLevel(2); setProgress(35); }
}

// Build quiz 1
const QUIZ1 = [
  { q: 'Urutan kerja komputer yang benar adalah…', options:['Process → Input → Output','Input → Process → Output','Output → Input → Process','Input → Output → Process'], answer:1,
    expC:'Betul! Data masuk (Input) diolah (Process) lalu ditampilkan (Output).',
    expW:'Ingat alurnya: Input (masuk) → Process (diolah) → Output (keluar).' },
  { q: 'Mana yang <b>bukan</b> perangkat input?', options:['Keyboard','Printer','Mouse','Mikrofon'], answer:1,
    expC:'Tepat, printer adalah perangkat <b>output</b>.',
    expW:'Printer mengeluarkan hasil di kertas, jadi output, bukan input.' },
  { q: 'Fungsi RAM adalah…', options:['Menyimpan data kerja sementara','Mencetak dokumen','Menangkap gambar','Memutar suara'], answer:0,
    expC:'Ya! RAM menyimpan data sementara saat pemrosesan.',
    expW:'RAM bukan output/input; ia bagian proses untuk data sementara.' },
];

// ==========================
// Level 2 — Software Sorting
// ==========================
const softwareItems = [
  {name:'Windows', cat:'Sistem Operasi', emoji:'🪟'},
  {name:'Linux', cat:'Sistem Operasi', emoji:'🐧'},
  {name:'Android', cat:'Sistem Operasi', emoji:'🤖'},
  {name:'iOS', cat:'Sistem Operasi', emoji:'📱'},
  {name:'Pengolah Kata', cat:'Aplikasi', emoji:'📝'},
  {name:'Peramban Web', cat:'Aplikasi', emoji:'🌐'},
  {name:'Editor Gambar', cat:'Aplikasi', emoji:'🖌️'},
  {name:'Presentasi', cat:'Aplikasi', emoji:'📊'},
  {name:'Python', cat:'Pemrograman', emoji:'🐍'},
  {name:'C++', cat:'Pemrograman', emoji:'💠'},
  {name:'Scratch', cat:'Pemrograman', emoji:'🧩'},
  {name:'IDE (Code Editor)', cat:'Pemrograman', emoji:'🧑‍💻'},
];

function shuffleLvl2(){ buildBank('bank2', shuffle([...softwareItems]), 'sw'); }
function hintLvl2(){
  showModal('Petunjuk Level 2', `<ul>
    <li><b>Sistem Operasi</b>: mengelola perangkat & aplikasi.</li>
    <li><b>Aplikasi</b>: dipakai untuk tugas tertentu (menulis, berselancar, desain, presentasi).</li>
    <li><b>Pemrograman</b>: bahasa/alat untuk membuat software (Python, C++, Scratch, IDE).</li>
  </ul>`);
}
function checkLvl2(){
  let correct = 0, total = softwareItems.length, msg = [];
  document.querySelectorAll('#lvl2 .bin').forEach(bin=>{
    const accept = bin.dataset.accept;
    bin.querySelectorAll('.item').forEach(it=>{
      if(it.dataset.cat===accept){ correct++; addPoints(10); msg.push(`✅ <b>${it.dataset.name}</b> benar (${accept}).`); }
      else { addPoints(-5); msg.push(`❌ <b>${it.dataset.name}</b> bukan ${accept}. <i>${it.dataset.cat}</i>.`); }
    })
  });
  const fb = document.getElementById('fb2');
  fb.style.display='block';
  fb.className = 'feedback ' + (correct>= Math.ceil(total*0.8) ? 'good' : 'bad');
  fb.innerHTML = `<b>Hasil:</b> ${correct}/${total}<br>` + msg.map(m=>`<div>${m}</div>`).join('');
  if(correct===total){ awardBadge('soft'); nextLevel(3); setProgress(65); }
}

const QUIZ2 = [
  { q:'Yang <b>bukan</b> termasuk software pemrograman adalah…', options:['Python','IDE (Code Editor)','Editor Gambar','Scratch'], answer:2,
    expC:'Betul, editor gambar adalah aplikasi, bukan untuk pemrograman.',
    expW:'Ingat: pemrograman = bahasa/alat membuat software (Python, Scratch, IDE).' },
  { q:'Peran <b>sistem operasi</b> adalah…', options:['Mengelola perangkat & aplikasi','Membuat presentasi','Mengedit video','Mencetak dokumen'], answer:0,
    expC:'Tepat! OS mengelola perangkat keras & menjalankan aplikasi.',
    expW:'Hanya OS yang tugasnya mengelola hardware & aplikasi.' },
  { q:'Contoh <b>aplikasi</b> adalah…', options:['Peramban Web','Linux','C++','Keyboard'], answer:0,
    expC:'Ya, peramban web adalah aplikasi untuk menjelajah internet.',
    expW:'Linux adalah OS; C++ bahasa; keyboard adalah hardware.' },
];

// ==========================
// Level 3 — Studi Kasus
// ==========================
const CASES = [
  { id:'majalah', title:'Majalah Dinding Digital Sekolah',
    text:'Tim kelas ingin membuat mading digital dengan teks & gambar.',
    rightHW:['Keyboard','Mouse','Monitor'],
    rightSW:['Pengolah Kata','Editor Gambar','Presentasi'],
    choicesHW:['Keyboard','Mouse','Webcam','Mikrofon','Monitor','Printer'],
    choicesSW:['Pengolah Kata','Peramban Web','Editor Gambar','Presentasi','Python'] },
  { id:'vlog', title:'Vlogger Ekstrakurikuler',
    text:'Siswa membuat video testimoni kegiatan sekolah untuk diunggah.',
    rightHW:['Webcam','Mikrofon','GPU'],
    rightSW:['Editor Gambar','Presentasi'],
    choicesHW:['Webcam','Mikrofon','Speaker','GPU','Printer'],
    choicesSW:['Editor Gambar','Peramban Web','Presentasi','IDE (Code Editor)'] },
  { id:'kasir', title:'Kasir Koperasi Sekolah',
    text:'Mencatat transaksi cepat dan mencetak struk.',
    rightHW:['Keyboard','Monitor','Printer'],
    rightSW:['Peramban Web','Pengolah Kata'],
    choicesHW:['Keyboard','Mouse','Monitor','Printer','Scanner'],
    choicesSW:['Peramban Web','Pengolah Kata','Scratch','C++'] },
];

function renderCases(){
  const wrap = document.getElementById('cases');
  wrap.innerHTML = '';
  CASES.forEach((c,idx)=>{
    const div = document.createElement('div');
    div.className='q';
    const hwId = `case_${c.id}_hw`;
    const swId = `case_${c.id}_sw`;
    div.innerHTML = `
      <h4>Kasus ${idx+1}: ${c.title}</h4>
      <p class="tip">${c.text}</p>
      <div class="grid cols-2">
        <div>
          <b>Pilih 2–3 Hardware</b>
          <div class="opts">${c.choicesHW.map(x=>`<label class="opt"><input type="checkbox" name="${hwId}" value="${x}"> ${x}</label>`).join('')}</div>
        </div>
        <div>
          <b>Pilih 1–2 Software</b>
          <div class="opts">${c.choicesSW.map(x=>`<label class="opt"><input type="checkbox" name="${swId}" value="${x}"> ${x}</label>`).join('')}</div>
        </div>
      </div>`;
    wrap.appendChild(div);
  });
}

function checkCases(){
  let total=0, got=0, messages=[];
  CASES.forEach(c=>{
    total++;
    const hwSel = Array.from(document.querySelectorAll(`input[name="case_${c.id}_hw"]:checked`)).map(i=>i.value);
    const swSel = Array.from(document.querySelectorAll(`input[name="case_${c.id}_sw"]:checked`)).map(i=>i.value);

    const hwOK = hwSel.length>=2 && hwSel.every(v=>c.rightHW.includes(v));
    const swOK = swSel.length>=1 && swSel.every(v=>c.rightSW.includes(v));

    if(hwOK && swOK){
      got++;
      addPoints(15);
      messages.push(`✅ <b>${c.title}</b>: Pilihan tepat! <i>Alasan:</i> hardware & software mendukung tujuan kasus.`);
    } else {
      const tip = `Coba ulang: pertimbangkan <b>alat input</b> (untuk memasukkan data/suara/foto), <b>proses</b> (GPU untuk grafis/video), dan <b>output</b> (monitor/printer). Pilih software yang sesuai tujuannya.`;
      messages.push(`❌ <b>${c.title}</b>: Belum pas. ${tip}`);
      addPoints(-5);
    }
  });
  const fb = document.getElementById('fb3');
  fb.style.display='block';
  fb.className = 'feedback ' + (got===total ? 'good' : 'bad');
  fb.innerHTML = `<b>Hasil Kasus:</b> ${got}/${total}<br>` + messages.map(m=>`<div>${m}</div>`).join('');
  if(got===total){ awardBadge('cases'); nextLevel(4); setProgress(85); }
}

const QUIZ3 = [
  { q:'Untuk <b>siaran langsung</b> (live), perangkat input yang perlu adalah…', options:['Printer','Mikrofon & Webcam','Speaker & Proyektor','Scanner'], answer:1,
    expC:'Betul! Live butuh mikrofon (suara) & webcam (video).',
    expW:'Live memerlukan alat untuk memasukkan suara & gambar (mikrofon & webcam).'},
  { q:'Membuat poster digital lebih cocok menggunakan…', options:['Editor Gambar','Peramban Web','Sistem Operasi','C++'], answer:0,
    expC:'Tepat! Editor gambar untuk desain poster.',
    expW:'Poster dibuat dengan aplikasi desain, bukan OS atau bahasa pemrograman.'},
  { q:'Mencetak struk di koperasi memerlukan perangkat…', options:['Speaker','Printer','Webcam','GPU'], answer:1,
    expC:'Ya, printer menghasilkan keluaran di kertas.',
    expW:'Struk adalah keluaran fisik yang dicetak oleh printer.'},
];

// ==========================
// Kuis & Evaluasi
// ==========================
function renderQuiz(containerId, data){
  const box = document.getElementById(containerId);
  box.innerHTML = '';
  data.forEach((q,qi)=>{
    const div = document.createElement('div');
    div.className='q';
    div.innerHTML = `<h4>Soal ${qi+1}.</h4><div>${q.q}</div>` +
      `<div class="opts">`+
      q.options.map((op,oi)=>`<label class="opt"><input type="radio" name="${containerId}_q${qi}" value="${oi}"> ${op}</label>`).join('')+
      `</div><div class="explain" id="${containerId}_exp_${qi}"></div>`;
    box.appendChild(div);
  });
}
function gradeQuiz(containerId){
  const data = containerId==='quiz1'?QUIZ1:containerId==='quiz2'?QUIZ2:QUIZ3;
  let score=0, total=data.length;
  data.forEach((q,qi)=>{
    const sel = document.querySelector(`input[name="${containerId}_q${qi}"]:checked`);
    const exp = document.getElementById(`${containerId}_exp_${qi}`);
    if(!sel){ exp.textContent='Belum memilih jawaban.'; return; }
    const v = parseInt(sel.value,10);
    const labels = sel.closest('.opts').querySelectorAll('.opt');
    labels.forEach((lb,idx)=>{
      lb.classList.remove('correct','wrong');
      if(idx===q.answer) lb.classList.add('correct');
      if(idx===v && v!==q.answer) lb.classList.add('wrong');
    });
    if(v===q.answer){ score++; addPoints(5); exp.textContent=q.expC; }
    else { addPoints(0); exp.textContent=q.expW+` (Jawaban benar: "${q.options[q.answer]}")`; }
  });
  live(`Skor kuis ${containerId}: ${score}/${total}`);
  if(containerId==='quiz1' && score===total){ setProgress(45); }
  if(containerId==='quiz2' && score===total){ setProgress(75); }
  if(containerId==='quiz3' && score===total){ setProgress(90); }
}

const FINAL = [
  { q:'Alur yang benar pada sistem komputer adalah…', options:['Input → Process → Output','Output → Input → Process','Process → Output → Input','Input → Output → Process'], answer:0,
    exp:'Komputer menerima data (Input), mengolah (Process), lalu menghasilkan (Output).'},
  { q:'Contoh perangkat <b>input</b> adalah…', options:['Monitor','Printer','Keyboard','Speaker'], answer:2,
    exp:'Keyboard memasukkan data huruf/angka → input.'},
  { q:'Yang termasuk <b>sistem operasi</b>…', options:['Peramban Web','Linux','Editor Gambar','Presentasi'], answer:1,
    exp:'Linux/Windows/Android adalah sistem operasi.'},
  { q:'Aplikasi untuk mengetik surat termasuk kategori…', options:['Pemrograman','Sistem Operasi','Aplikasi','Hardware'], answer:2,
    exp:'Pengolah kata adalah aplikasi.'},
  { q:'Fungsi RAM yang paling tepat adalah…', options:['Mencetak dokumen','Menyimpan data kerja sementara','Menampilkan gambar','Memasukkan suara'], answer:1,
    exp:'RAM adalah memori kerja sementara saat proses berlangsung.'},
  { q:'Untuk streaming game, komponen <b>proses</b> yang membantu grafis adalah…', options:['GPU','Printer','Speaker','Scanner'], answer:0,
    exp:'GPU mempercepat pemrosesan grafis.'},
  { q:'Contoh software <b>pemrograman</b>…', options:['Python','Peramban Web','Editor Gambar','Presentasi'], answer:0,
    exp:'Python adalah bahasa pemrograman.'},
  { q:'Perangkat untuk <b>keluaran</b> visual di layar besar…', options:['Proyektor','Mikrofon','Webcam','Keyboard'], answer:0,
    exp:'Proyektor menampilkan gambar ke layar besar → output.'},
  { q:'Menjalankan aplikasi membutuhkan…', options:['Sistem Operasi','GPU saja','Printer','Keyboard'], answer:0,
    exp:'Aplikasi berjalan di atas sistem operasi.'},
  { q:'Untuk menulis kode, kamu memerlukan…', options:['IDE/Code Editor','Printer','Webcam','Speaker'], answer:0,
    exp:'IDE/Code Editor adalah alat untuk menulis & menjalankan kode.'},
];

function renderFinal(){ renderQuiz('finalQuiz', FINAL); }

function gradeFinal(){
  let score=0, total=FINAL.length;
  FINAL.forEach((q,qi)=>{
    const sel = document.querySelector(`input[name="finalQuiz_q${qi}"]:checked`);
    const exp = document.getElementById(`finalQuiz_exp_${qi}`);
    if(!sel){ exp.textContent='Belum memilih jawaban.'; return; }
    const v = parseInt(sel.value,10);
    const labels = sel.closest('.opts').querySelectorAll('.opt');
    labels.forEach((lb,idx)=>{
      lb.classList.remove('correct','wrong');
      if(idx===q.answer) lb.classList.add('correct');
      if(idx===v && v!==q.answer) lb.classList.add('wrong');
    });
    if(v===q.answer){ score++; addPoints(10); exp.textContent = 'Hebat! '+q.exp; }
    else { exp.textContent = 'Perhatikan lagi: '+q.exp; }
  });
  const percent = Math.round(100*score/total);
  setProgress(100);
  if(percent>=90){ awardBadge('final90'); }
  showModal('Hasil Evaluasi Akhir', `<p>Skor kamu: <b>${score}/${total}</b> (${percent}%)</p>
    <p>${percent>=90?'Luar biasa! Kamu menguasai materi.':'Terus semangat! Coba pelajari ulang bagian yang masih keliru.'}</p>`);
}

function renderBadges(){
  const grid = document.getElementById('badgeGrid');
  if(!grid) return;
  grid.innerHTML = '';
  BADGES.forEach(b=>{
    const d = document.createElement('div');
    d.className = 'ach ' + (state.badges.has(b.key)?'earned':'');
    d.innerHTML = `<div class="big">${b.name.split(' ')[0]}</div><div><b>${b.name}</b></div><div class="tip">${b.desc}</div>`;
    grid.appendChild(d);
  });
}

// ==========================
// Accessibility
// ==========================
function live(msg){ const r = document.getElementById('live'); r.textContent = msg; }

// ==========================
// Bootstrap
// ==========================
function init(){
  loadState();
  renderMiniMap();
  shuffleLvl1();
  shuffleLvl2();
  makeBins();
  renderQuiz('quiz1', QUIZ1);
  renderQuiz('quiz2', QUIZ2);
  renderCases();
  renderQuiz('quiz3', QUIZ3);
  renderFinal();
  updateUI();
}

document.addEventListener('DOMContentLoaded', init);
</script>
</body>
</html>
