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

.grid{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-bottom:20px}
.card{background:#fff;border-radius:12px;padding:16px;text-align:center;cursor:pointer;border:2px solid transparent;transition:0.2s}
.card.active{border-color:var(--yellow);background:#FFFDE7}
.card.disabled{opacity:0.4;pointer-events:none}

/* MODAL */
.modal{display:none;position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.6);z-index:9999;align-items:center;justify-content:center;padding:16px}
.modal.show{display:flex}
.modal-content{background:#fff;border-radius:16px;padding:20px;width:100%;max-width:500px;max-height:90vh;overflow-y:auto}
.modal-content h3{margin-bottom:16px;font-size:18px}
.modal-content input,.modal-content textarea{width:100%;padding:12px;border:1px solid var(--border);border-radius:8px;margin-bottom:12px;font-size:14px}
.modal-actions{display:flex;gap:10px}
.modal-actions button{flex:1;padding:12px;border:none;border-radius:8px;font-weight:600;cursor:pointer}
.btn-batal{background:#F5F5F5}
.btn-submit{background:var(--yellow)}
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
</header>

<main>
  <div class="grid">
    <div class="card active" onclick="openModal('belikan', this)">
      <div>🛵</div>
      <div>Belikan</div>
    </div>
    <div class="card" onclick="openModal('antarkan', this)">
      <div>🚗</div>
      <div>Antarkan</div>
    </div>
    <div class="card" onclick="openModal('ambilkan', this)">
      <div>📦</div>
      <div>Ambilkan</div>
    </div>
    <div class="card" onclick="openModal('ngojek', this)">
      <div>🍔</div>
      <div>Ngojek</div>
    </div>
    <div class="card" id="travelCard" onclick="openModal('travel', this)">
      <div>✈️</div>
      <div>Travel</div>
    </div>
    <div class="card" onclick="openModal('nota', this)">
      <div>🧾</div>
      <div>Nota Digital</div>
    </div>
  </div>
</main>

<div class="modal" id="modal" onclick="closeModal()">
  <div class="modal-content" id="modalContent" onclick="event.stopPropagation()"></div>
</div>

<script>
const WA_KURIR = '6283137527300';
let layananAktif = 'belikan';

function openModal(jenis, el){
  layananAktif = jenis;
  
  document.querySelectorAll('.card').forEach(c=>c.classList.remove('active'));
  el.classList.add('active');

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
  
  html += `<div class="modal-actions">
    <button class="btn-batal" onclick="closeModal()">Batal</button>
    <button class="btn-submit" onclick="kirimKeWA()">Kirim ke WhatsApp</button>
  </div>`;

  document.getElementById('modalContent').innerHTML = html;
  document.getElementById('modal').classList.add('show');
  
  document.getElementById('nama').value = localStorage.getItem('nama') || '';
  document.getElementById('wa').value = localStorage.getItem('wa') || '';
}

function closeModal(){
  document.getElementById('modal').classList.remove('show');
}

function kirimKeWA(){
  const nama = document.getElementById('nama').value;
  const wa = document.getElementById('wa').value;
  
  if(!nama || !wa){
    alert('Isi nama dan WA dulu!');
    return;
  }
  
  let pesan = `*ORDER PAMAN KURIR*%0A`;
  pesan += `Layanan: ${layananAktif}%0A`;
  pesan += `Nama: ${nama}%0A`;
  pesan += `WA: ${wa}%0A`;
  
  localStorage.setItem('nama', nama);
  localStorage.setItem('wa', wa);
  
  closeModal();
  window.open(`https://wa.me/${WA_KURIR}?text=${pesan}`, '_blank');
}

function cekStatus(){
  const jam = new Date().getHours();
  const status = document.getElementById('status');
  if(jam >= 8 && jam < 21){
    status.textContent = 'Online';
  }else{
    status.textContent = 'Offline';
    document.getElementById('travelCard').classList.add('disabled');
  }
}
cekStatus();
setInterval(cekStatus,60000);
</script>
</body>
</html>
