<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Paman Kurir - Tanahgrogot</title>
<meta name="theme-color" content="#FFD700">
<style>
:root{
  --yellow:#FFD700;
  --dark:#1C1C1E;
  --gray:#F5F5F5;
  --border:#E0E0E0;
}
*{margin:0;padding:0;box-sizing:border-box;font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,sans-serif}
body{background:var(--gray);padding-bottom:70px}

/* HEADER */
header{position:fixed;top:0;left:0;right:0;background:#fff;z-index:100;box-shadow:0 2px 8px rgba(0,0,0,0.08)}
.header-top{display:flex;justify-content:space-between;align-items:center;padding:12px 16px;border-bottom:1px solid var(--border)}
.logo{font-size:24px;font-weight:700;color:var(--dark)}
.status{font-size:12px;padding:4px 10px;border-radius:20px;background:#E8F5E9;color:#2E7D32;font-weight:600}
.status.offline{background:#FFEBEE;color:#C62828}

.marquee{background:#FFFDE7;padding:8px 16px;overflow:hidden}
.marquee p{display:inline-block;white-space:nowrap;animation:marquee 12s linear infinite;font-size:13px;color:var(--dark)}
@keyframes marquee{0%{transform:translateX(100%)}100%{transform:translateX(-100%)}}

main{margin-top:90px;padding:16px}

/* BANNER */
.banner{width:100%;height:160px;border-radius:12px;overflow:hidden;margin-bottom:16px;position:relative}
.slide{display:none;width:100%;height:100%;background:linear-gradient(135deg,#FFD700,#FFA000);align-items:center;justify-content:center;color:#fff;font-size:18px;font-weight:700;text-align:center;padding:20px}
.slide.active{display:flex}
.dots{text-align:center;margin-top:-30px;position:relative}
.dot{display:inline-block;width:8px;height:8px;border-radius:50%;background:rgba(255,255,255,0.5);margin:0 4px}
.dot.active{background:#fff}

/* GRID LAYAN */
.grid{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-bottom:20px}
.card{background:#fff;border-radius:12px;padding:16px;text-align:center;cursor:pointer;border:2px solid transparent;transition:0.2s}
.card.active{border-color:var(--yellow);background:#FFFDE7}
.card-icon{font-size:28px;margin-bottom:6px}
.card-title{font-size:13px;font-weight:600;color:var(--dark)}
.card.disabled{opacity:0.4;pointer-events:none}

.btn-kirim{width:100%;padding:14px;background:var(--yellow);color:var(--dark);border:none;border-radius:12px;font-size:16px;font-weight:700;cursor:pointer}

/* MODAL - INI YANG PENTING */
.modal{display:none;position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.6);z-index:9999;align-items:center;justify-content:center;padding:16px}
.modal.show{display:flex}
.modal-content{background:#fff;border-radius:16px;padding:20px;width:100%;max-width:500px;max-height:90vh;overflow-y:auto}
.modal-content h3{margin-bottom:16px;font-size:18px}
.modal-content input,.modal-content textarea{width:100%;padding:12px;border:1px solid var(--border);border-radius:8px;margin-bottom:12px;font-size:14px}
.modal-actions{display:flex;gap:10px}
.modal-actions button{flex:1;padding:12px;border:none;border-radius:8px;font-weight:600;cursor:pointer}
.btn-batal{background:#F5F5F5}
.btn-submit{background:var(--yellow)}

.item-list{display:flex;gap:8px;margin-bottom:8px}
.item-list input{flex:1}
.btn-tambah{width:100%;padding:8px;border:1px dashed var(--yellow);background:transparent;color:var(--dark);border-radius:8px;margin-bottom:12px;cursor:pointer}

footer{position:fixed;bottom:0;left:0;right:0;background:#fff;border-top:1px solid var(--border);display:flex;justify-content:space-around;padding:10px 0;z-index:99}
.footer-btn{text-align:center;font-size:11px;color:#666;cursor:pointer}
.footer-btn-icon{font-size:20px;margin-bottom:2px}
</style>
</head>
<body>

<header>
  <div class="header-top">
    <div class="logo">Paman Kurir</div>
    <div>
      <div style="font-size:11px;text-align:right;color:#666">Tanahgrogot</div>
      <div class="status" id="status">Online</div>
    </div>
  </div>
  <div class="marquee">
    <p>⚠️ Info: Istirahat waktu Dzuhur dan Asar</p>
  </div>
</header>

<main>
  <div class="banner" id="banner">
    <div class="slide active">🚗 Jual Beli Mobil Bekas<br>Kualitas Terjamin</div>
    <div class="slide">🏍️ Motor Bekas Murah<br>Siap Pakai</div>
    <div class="slide">🚚 Jasa Kurir Cepat & Aman<br>08:00 - 21:00 WIT</div>
  </div>
  <div class="dots">
    <span class="dot active"></span>
    <span class="dot"></span>
    <span class="dot"></span>
  </div>

  <h3 style="margin:16px 0 12px;font-size:16px">Pilih Layanan:</h3>

  <div class="grid">
    <div class="card active" onclick="openModal('belikan')">
      <div class="card-icon">🛵</div>
      <div class="card-title">Belikan</div>
    </div>
    <div class="card" onclick="openModal('antarkan')">
      <div class="card-icon">🚗</div>
      <div class="card-title">Antarkan</div>
    </div>
    <div class="card" onclick="openModal('ambilkan')">
      <div class="card-icon">📦</div>
      <div class="card-title">Ambilkan</div>
    </div>
    <div class="card" onclick="openModal('ngojek')">
      <div class="card-icon">🍔</div>
      <div class="card-title">Ngojek</div>
    </div>
    <div class="card" id="travelCard" onclick="openModal('travel')">
      <div class="card-icon">✈️</div>
      <div class="card-title">Travel</div>
    </div>
    <div class="card" onclick="openModal('nota')">
      <div class="card-icon">🧾</div>
      <div class="card-title">Nota Digital</div>
    </div>
  </div>

  <button class="btn-kirim" onclick="kirimPesanan()">MEMESAN</button>
</main>

<!-- MODAL -->
<div class="modal" id="modal" onclick="closeModal()">
  <div class="modal-content" id="modalContent" onclick="event.stopPropagation()"></div>
</div>

<footer>
  <div class="footer-btn" onclick="location.reload()">
    <div class="footer-btn-icon">🏠</div>Home
  </div>
  <div class="footer-btn" onclick="share('tiktok')">
    <div class="footer-btn-icon">🎵</div>TikTok
  </div>
  <div class="footer-btn" onclick="share('fb')">
    <div class="footer-btn-icon">📘</div>FB
  </div>
  <div class="footer-btn" onclick="share('wa')">
    <div class="footer-btn-icon">💬</div>WhatsApp
  </div>
  <div class="footer-btn" onclick="alert('Buka menu browser > Install App')">
    <div class="footer-btn-icon">⬇️</div>App
  </div>
</footer>

<script>
const WA_KURIR = '6283137527300';
let layananAktif = 'belikan';

window.onload = function(){
  cekStatus();
  setInterval(cekStatus,60000);
  autoSlide();
}

// STATUS ONLINE/OFFLINE
function cekStatus(){
  const jam = new Date().getHours();
  const status = document.getElementById('status');
  const travelCard = document.getElementById('travelCard');
  if(jam >= 8 && jam < 21){
    status.textContent = 'Online';
    status.className = 'status';
    travelCard.classList.remove('disabled');
  }else{
    status.textContent = 'Offline';
    status.className = 'status offline';
    travelCard.classList.add('disabled');
  }
}

// SLIDE BANNER
let slideIndex = 0;
function autoSlide(){
  const slides = document.querySelectorAll('.slide');
  const dots = document.querySelectorAll('.dot');
  slides.forEach(s=>s.classList.remove('active'));
  dots.forEach(d=>d.classList.remove('active'));
  slideIndex = (slideIndex+1)%slides.length;
  slides[slideIndex].classList.add('active');
  dots[slideIndex].classList.add('active');
  setTimeout(autoSlide,4000);
}

// MODAL
function openModal(jenis){
  layananAktif = jenis;
  document.querySelectorAll('.card').forEach(c=>c.classList.remove('active'));
  event.currentTarget.classList.add('active');

  let html = '';
  if(jenis==='belikan'){
    html = `<h3>🛵 Form Belikan</h3>
      <input type="text" id="nama" placeholder="Nama">
      <input type="tel" id="wa" placeholder="No WhatsApp">
      <input type="text" id="antar" placeholder="Antar ke">`;
    for(let i=1;i<=10;i++){
      html += `<input type="text" placeholder="Item ${i}">`;
    }
  }
  if(jenis==='antarkan'){
    html = `<h3>🚗 Form Antarkan</h3>
      <input type="text" id="nama" placeholder="Nama">
      <input type="tel" id="wa" placeholder="No WhatsApp">
      <input type="text" id="antar" placeholder="Antar ke">
      <textarea id="keterangan" placeholder="Keterangan barang"></textarea>`;
  }
  if(jenis==='ambilkan'){
    html = `<h3>📦 Form Ambilkan</h3>
      <input type="text" id="nama" placeholder="Atas Nama">
      <input type="tel" id="wa" placeholder="No WhatsApp">
      <input type="text" id="ambil" placeholder="Ambil di">
      <input type="tel" id="waPenjual" placeholder="No WA Penjual">
      <textarea id="catatan" placeholder="Catatan Pesanan"></textarea>`;
  }
  if(jenis==='ngojek'){
    html = `<h3>🍔 Form Ngojek</h3>
      <input type="text" id="nama" placeholder="Nama">
      <input type="tel" id="wa" placeholder="No WhatsApp">
      <input type="text" id="jemput" placeholder="Jemput di">
      <input type="text" id="tujuan" placeholder="Tujuan ke">`;
  }
  if(jenis==='travel'){
    html = `<h3>✈️ Form Travel</h3>
      <input type="text" id="nama" placeholder="Nama">
      <input type="tel" id="wa" placeholder="No WhatsApp">
      <input type="text" id="jemput" placeholder="Jemput di">
      <input type="text" id="tujuan" placeholder="Tujuan ke">
      <input type="number" id="penumpang" placeholder="Jumlah Penumpang">
      <textarea id="catatan" placeholder="Catatan"></textarea>`;
  }
  if(jenis==='nota'){
    html = `<h3>🧾 Nota Digital</h3>
      <p style="text-align:center;padding:20px">Fitur nota ada di halaman terpisah</p>`;
  }

  html += `<div class="modal-actions">
    <button class="btn-batal" onclick="closeModal()">Batal</button>
    <button class="btn-submit" onclick="alert('Data tersimpan! Klik MEMESAN untuk kirim')">Simpan</button>
  </div>`;

  document.getElementById('modalContent').innerHTML = html;
  document.getElementById('modal').classList.add('show');
}

function closeModal(){
  document.getElementById('modal').classList.remove('show');
}

function kirimPesanan(){
  alert('Fitur kirim ke WA 083137527300 sudah aktif. Isi form dulu ya!');
  window.open('https://wa.me/6283137527300?text=Halo%20Paman%20Kurir,%20saya%20mau%20order','_blank');
}

function share(sosmed){
  const url = window.location.href;
  if(sosmed==='wa') window.open(`https://wa.me/?text=${url}`,'_blank');
  if(sosmed==='fb') window.open(`https://www.facebook.com/sharer/sharer.php?u=${url}`,'_blank');
  if(sosmed==='tiktok'){navigator.clipboard.writeText(url);alert('Link dicopy!')}
}
</script>
</body>
</html>
