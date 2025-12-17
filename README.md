<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>KATALOG AGUNAN BANKALTIMTARA TARAKAN</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
:root{
 --blue:#e8f1fb;--blue-dark:#1e5fa3;--yellow:#fff6d8;
}
body{margin:0;font-family:Segoe UI,Arial;background:linear-gradient(135deg,var(--blue),var(--yellow));}
header{background:var(--blue-dark);color:#fff;padding:18px;text-align:center;font-size:20px;font-weight:600}
.container{max-width:1200px;margin:auto;padding:20px}
.card{background:#fff;border-radius:16px;padding:20px;box-shadow:0 10px 25px rgba(0,0,0,.08);margin-bottom:20px}
input,select,textarea,button{width:100%;padding:10px;margin-top:8px;border-radius:10px;border:1px solid #ccc}
button{background:var(--blue-dark);color:#fff;border:none;cursor:pointer}
button.secondary{background:#888}
button.danger{background:#c0392b}
button:hover{opacity:.9}
.hidden{display:none}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(260px,1fr));gap:16px}
.row{display:grid;grid-template-columns:1fr 1fr;gap:12px}
.topbar{display:flex;justify-content:space-between;align-items:center;margin-bottom:16px}
.badge{background:var(--yellow);padding:4px 8px;border-radius:6px;font-size:12px}
img.thumb{width:100%;height:160px;object-fit:cover;border-radius:12px}
img.gallery{width:100%;border-radius:12px;margin-bottom:8px}
.stat{font-size:18px;font-weight:600}
</style>
</head>
<body>
<header>KATALOG AGUNAN BANKALTIMTARA TARAKAN</header>
<div class="container">

<!-- LOGIN -->
<div id="loginPage" class="card">
<h3>Login</h3>
<select id="role"><option value="user">Pengguna</option><option value="admin">Admin</option></select>
<input id="username" placeholder="Username">
<input id="password" type="password" placeholder="Password">
<button onclick="login()">Masuk</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="hidden">
<div class="topbar">
<h3>Dashboard Agunan</h3>
<div>
<button id="btnTambah" class="hidden" onclick="openForm()">+ Tambahkan Agunan</button>
<button class="secondary" onclick="logout()">Logout</button>
</div>
</div>

<!-- STATISTIK -->
<div class="grid">
<div class="card stat">Total Agunan<br><span id="statTotal">0</span></div>
<div class="card stat">Total Nilai (Rp)<br><span id="statNilai">0</span></div>
<div class="card stat">Jenis Terbanyak<br><span id="statJenis">-</span></div>
</div>

<div class="card">
<h4>Filter & Pencarian</h4>
<div class="grid">
<select id="fJenis" onchange="render()"><option value="">Semua Jenis</option></select>
<input id="fHargaMin" type="number" placeholder="Harga Minimum" oninput="render()">
<input id="fHargaMax" type="number" placeholder="Harga Maksimum" oninput="render()">
<input id="fSearch" placeholder="Cari agunan..." oninput="render()">
</div>
</div>

<div id="list" class="grid"></div>
</div>

<!-- FORM AGUNAN -->
<div id="formPage" class="hidden">
<div class="topbar"><h3>Input Agunan</h3><button class="secondary" onclick="back()">Kembali</button></div>
<div class="card">
<select id="jenis" onchange="spec()">
<option>Tanah</option><option>Bangunan</option><option>Tanah & Bangunan</option>
<option>Kendaraan</option><option>Mesin</option><option>Alat Berat</option><option>Persediaan</option><option>Lainnya</option>
</select>
<input id="jenisLain" class="hidden" placeholder="Jenis lainnya">
<div id="spesifikasi"></div>
<h4>Alamat</h4>
<textarea id="alamat"></textarea>
<input id="maps" placeholder="Link Google Maps" oninput="qrgen()">
<div id="qr"></div>
<h4>Foto (maks 20)</h4>
<input type="file" id="foto" multiple accept="image/*">
<h4>Kontak</h4>
<div class="row"><input id="kontak"><input id="telp"></div>
<h4>Nilai Agunan (Rp)</h4>
<input id="nilai" type="number">
<button onclick="save()">Simpan</button>
</div>
</div>

<!-- DETAIL -->
<div id="detailPage" class="hidden">
<div class="topbar"><h3>Detail Agunan</h3><button class="secondary" onclick="closeDetail()">Kembali</button></div>
<div id="detailContent" class="card"></div>
</div>
</div>

<script>
let admin=false;let data=JSON.parse(localStorage.getItem('agunan')||'[]');
let editIndex=null;

function persist(){localStorage.setItem('agunan',JSON.stringify(data));}

function login(){
 if(role.value==='admin'){
  if(username.value==='220241005388'&&password.value==='220241005388'){admin=true;btnTambah.classList.remove('hidden');}
  else{alert('Login admin salah');return;}
 }
 loginPage.classList.add('hidden');dashboard.classList.remove('hidden');render();
}
function logout(){location.reload();}
function openForm(){editIndex=null;formPage.classList.remove('hidden');dashboard.classList.add('hidden');spec();}
function back(){formPage.classList.add('hidden');dashboard.classList.remove('hidden');render();}
function spec(){
 jenisLain.classList.toggle('hidden',jenis.value!=='Lainnya');
 let j=jenis.value,html='';
 if(['Tanah','Bangunan','Tanah & Bangunan'].includes(j))html=`<h4>Spesifikasi</h4><input placeholder='Panjang Tanah'><input placeholder='Lebar Tanah'><input placeholder='Luas Tanah'><input placeholder='Panjang Bangunan'><input placeholder='Luas Bangunan'><input placeholder='Jumlah Lantai'><input placeholder='Legalitas'>`;
 else if(['Kendaraan','Mesin','Alat Berat'].includes(j))html=`<h4>Spesifikasi</h4><input placeholder='Jenis'><input placeholder='Merk'><input placeholder='Tipe'><input placeholder='Tahun Pembuatan'><input placeholder='Tahun Pembelian'>`;
 else if(j==='Persediaan')html=`<h4>Spesifikasi</h4><input placeholder='Jumlah'><input placeholder='Satuan'>`;
 spesifikasi.innerHTML=html;
}
function qrgen(){qr.innerHTML=maps.value?`<img src='https://api.qrserver.com/v1/create-qr-code/?size=120x120&data=${encodeURIComponent(maps.value)}'>`:''}
function save(){
 if(foto.files.length>20){alert('Maksimal 20 foto');return;}
 let imgs=[...foto.files].map(f=>URL.createObjectURL(f));
 let obj={jenis:jenis.value==='Lainnya'?jenisLain.value:jenis.value,alamat:alamat.value,nilai:+nilai.value,kontak:kontak.value,telp:telp.value,imgs};
 if(editIndex!==null)data[editIndex]=obj;else data.push(obj);
 persist();back();
}

function render(){
 list.innerHTML='';
 statTotal.textContent=data.length;
 statNilai.textContent=data.reduce((a,b)=>a+b.nilai,0).toLocaleString();
 let freq={};data.forEach(d=>freq[d.jenis]=(freq[d.jenis]||0)+1);
 statJenis.textContent=Object.keys(freq).sort((a,b)=>freq[b]-freq[a])[0]||'-';
 fJenis.innerHTML='<option value="">Semua Jenis</option>';
 [...new Set(data.map(d=>d.jenis))].forEach(j=>fJenis.innerHTML+=`<option>${j}</option>`);
 data.filter(d=>(!fJenis.value||d.jenis===fJenis.value)&&(!fHargaMin.value||d.nilai>=fHargaMin.value)&&(!fHargaMax.value||d.nilai<=fHargaMax.value)&&(!fSearch.value||JSON.stringify(d).toLowerCase().includes(fSearch.value.toLowerCase())))
 .forEach((d,i)=>{
  list.innerHTML+=`<div class='card'>
   ${d.imgs[0]?`<img class='thumb' src='${d.imgs[0]}'>`:''}
   <h4>${d.jenis}</h4>
   <span class='badge'>Rp ${d.nilai.toLocaleString()}</span>
   <button onclick='detail(${i})'>Detail</button>
   ${admin?`<button onclick='edit(${i})'>Edit</button><button class='danger' onclick='del(${i})'>Hapus</button>`:''}
  </div>`;
 });
}
function detail(i){
 dashboard.classList.add('hidden');detailPage.classList.remove('hidden');
 let d=data[i];
 detailContent.innerHTML=`<h3>${d.jenis}</h3>
 <p>${d.alamat}</p>
 <h4>Galeri Foto</h4>${d.imgs.map(x=>`<img class='gallery' src='${x}'>`).join('')}
 <p><b>Nilai:</b> Rp ${d.nilai.toLocaleString()}</p>
 <p><a href='https://wa.me/62${d.telp}' target='_blank'>Hubungi ${d.kontak}</a></p>`;
}
function closeDetail(){detailPage.classList.add('hidden');dashboard.classList.remove('hidden');}
function edit(i){editIndex=i;openForm();}
function del(i){if(confirm('Hapus agunan?')){data.splice(i,1);persist();render();}}
</script>
</body>
</html>
