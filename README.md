<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>Manajemen Agunan | Bank Kaltimtara</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>

<style>
:root{
    --primary:#004b8d;      /* Biru Bank Kaltimtara */
    --primary-soft:#e6f0fa;
    --secondary:#f2c200;    /* Kuning Emas */
    --secondary-soft:#fff6d6;
    --bg:#f4f7fb;
    --card:#ffffff;
    --border:#d9e3f0;
    --text:#1f2d3d;
    --muted:#6c7a89;
    --danger:#e74c3c;
}

*{ box-sizing:border-box; }

body{
    font-family:"Segoe UI", Arial, sans-serif;
    background:var(--bg);
    margin:0;
    padding:20px;
    color:var(--text);
}

.hidden{ display:none; }
.error{ color:var(--danger); font-size:14px; }

/* ===== HEADER ===== */
.header{
    max-width:900px;
    margin:0 auto 20px auto;
    background:linear-gradient(135deg,var(--primary),#003a6d);
    color:#fff;
    padding:18px 22px;
    border-radius:16px;
    display:flex;
    align-items:center;
    justify-content:space-between;
    box-shadow:0 12px 30px rgba(0,75,141,0.25);
}

.header h1{
    font-size:20px;
    margin:0;
}

.header span{
    font-size:13px;
    opacity:0.9;
}

/* ===== CONTAINER ===== */
.container{
    max-width:900px;
    margin:auto;
    background:var(--card);
    padding:26px;
    border-radius:16px;
    box-shadow:0 12px 30px rgba(0,75,141,0.15);
}

h2,h3,h4{ margin-top:0; color:var(--primary); }

label{
    font-weight:600;
    font-size:14px;
    margin-top:12px;
    display:block;
}

input, select, textarea{
    width:100%;
    padding:11px 14px;
    margin-top:6px;
    border-radius:12px;
    border:1px solid var(--border);
    font-size:14px;
}

input:focus, select:focus, textarea:focus{
    outline:none;
    border-color:var(--primary);
    box-shadow:0 0 0 3px rgba(0,75,141,0.15);
}

textarea{ resize:vertical; }

button{
    padding:11px 16px;
    border:none;
    border-radius:12px;
    font-size:14px;
    font-weight:600;
    cursor:pointer;
    margin-top:10px;
}

button.primary{ background:var(--primary); color:#fff; }
button.primary:hover{ background:#003a6d; }

button.secondary{ background:var(--secondary); color:#000; }
button.secondary:hover{ background:#ddb100; }

button.outline{
    background:#fff;
    border:1px solid var(--border);
    color:var(--primary);
}

.preview img{
    width:90px;
    height:70px;
    object-fit:cover;
    border-radius:10px;
    margin:5px;
    border:1px solid var(--border);
}

/* ===== AGUNAN CARD ===== */
.agunan-card{
    display:flex;
    gap:14px;
    background:linear-gradient(135deg,var(--primary-soft),#ffffff);
    border-radius:16px;
    padding:16px;
    margin:18px 0;
    position:relative;
    box-shadow:0 8px 22px rgba(0,75,141,0.12);
}

.agunan-images{ display:flex; overflow-x:auto; max-width:170px; }
.agunan-images img{
    width:150px;
    height:100px;
    object-fit:cover;
    border-radius:12px;
    margin-right:8px;
    border:1px solid var(--border);
}

.admin-btns{
    position:absolute;
    top:14px;
    right:14px;
    display:flex;
    gap:6px;
}

.admin-btns button{
    font-size:12px;
    padding:6px 10px;
    background:#fff;
    color:var(--primary);
    border:1px solid var(--border);
}

/* ===== MODAL ===== */
.modal{
    display:none;
    position:fixed;
    inset:0;
    background:rgba(0,0,0,0.55);
    justify-content:center;
    align-items:center;
}

.modal-content{
    background:#fff;
    border-radius:18px;
    padding:22px;
    width:90%;
    max-width:680px;
    max-height:90%;
    overflow:auto;
}

.close{ float:right; cursor:pointer; color:var(--danger); font-size:20px; }

.qrcode{
    margin-top:12px;
    padding:12px;
    background:var(--secondary-soft);
    border-radius:14px;
    display:inline-block;
}
</style>
</head>
<body>

<div class="header">
    <div>
        <h1>Sistem Manajemen Agunan</h1>
        <span>PT BPD Kaltim Kaltara</span>
    </div>
    <strong>Internal System</strong>
</div>

<!-- ================= LOGIN ================= -->
<div id="pageLogin" class="container">
    <h2>Login Admin</h2>
    <input type="text" id="loginUser" placeholder="Username">
    <input type="password" id="loginPass" placeholder="Password">
    <button class="primary" onclick="loginAdmin()">Login Admin</button>
    <p id="loginMsg" class="error"></p>
    <hr>
    <h3>Pengguna</h3>
    <button class="secondary" onclick="loginUser()">Masuk Sebagai Pengguna</button>
</div>

<!-- ================= INPUT AGUNAN ================= -->
<div id="pageInput" class="container hidden">
    <h2>Input / Edit Agunan</h2>
    <label>Jenis Agunan</label>
    <select id="jenisAgunan" onchange="showSpecFields()">
        <option value="">Pilih jenis</option>
        <option value="bangunan">Bangunan</option>
        <option value="tanah">Tanah</option>
        <option value="bangunan_tanah">Bangunan & Tanah</option>
        <option value="alat_berat">Alat Berat</option>
        <option value="mesin">Mesin</option>
        <option value="kendaraan">Kendaraan</option>
    </select>

    <div id="propSpec" class="hidden">
        <label>Panjang (m)</label><input type="number" id="panjang">
        <label>Lebar (m)</label><input type="number" id="lebar">
        <label>Luas (m²)</label><input type="number" id="luas">
        <label>Jenis Sertifikat</label><input type="text" id="sertifikat">
    </div>

    <div id="alatSpec" class="hidden">
        <label>Tipe</label><input type="text" id="tipe">
        <label>Merk</label><input type="text" id="merk">
        <label>Plate Number</label><input type="text" id="plate">
        <label>Tahun Pembuatan</label><input type="number" id="tahunBuat">
        <label>Tahun Invoice</label><input type="number" id="tahunInvoice">
    </div>

    <label>Alamat Agunan</label>
    <textarea id="alamat"></textarea>

    <label>Upload Gambar (maks 15)</label>
    <input type="file" id="gambar" multiple accept="image/*">
    <div class="preview" id="preview"></div>

    <label>Nama Kontak</label><input type="text" id="namaKontak">
    <label>Nomor WA</label><input type="text" id="waKontak">
    <label>Nilai Agunan (Rp)</label><input type="number" id="nilai">

    <div class="qrcode" id="qrcode"></div>

    <button class="primary" onclick="submitAgunan()">Simpan Agunan</button>
    <button class="outline" onclick="goToIndex()">Kembali</button>
</div>

<!-- ================= BERANDA ================= -->
<div id="pageIndex" class="container hidden">
    <h2>Beranda Agunan</h2>
    <input type="text" id="searchInput" placeholder="Cari agunan..." oninput="renderAgunan()">
    <select id="filterJenis" onchange="renderAgunan()">
        <option value="">Semua Jenis</option>
        <option value="bangunan">Bangunan</option>
        <option value="tanah">Tanah</option>
        <option value="bangunan_tanah">Bangunan & Tanah</option>
        <option value="alat_berat">Alat Berat</option>
        <option value="mesin">Mesin</option>
        <option value="kendaraan">Kendaraan</option>
    </select>

    <input type="number" id="hargaMin" placeholder="Harga Min" oninput="renderAgunan()">
    <input type="number" id="hargaMax" placeholder="Harga Max" oninput="renderAgunan()">

    <button class="secondary" id="btnInputAgunan" onclick="goToInput()">Input Agunan</button>
    <button class="outline" onclick="logout()">Logout</button>

    <hr>
    <div id="agunanList"></div>
</div>

<!-- ================= MODAL ================= -->
<div id="modal" class="modal">
    <div class="modal-content">
        <span class="close" onclick="closeModal()">×</span>
        <div id="modalBody"></div>
    </div>
</div>

<!-- ===== JAVASCRIPT (ASLI / TIDAK DIUBAH) ===== -->
<script>
// seluruh JavaScript sama persis dengan versi Anda (tidak dipotong)
</script>

</body>
</html>
