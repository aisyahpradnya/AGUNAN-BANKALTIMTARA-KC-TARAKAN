# AGUNAN-BANKALTIMTARA-KC-TARAKAN
:root{
    /* BRAND COLOR */
    --primary:#004b8d;      /* Biru Bank */
    --primary-soft:#e6f0fa;
    --secondary:#f2c200;    /* Kuning Emas */
    --secondary-soft:#fff6d6;

    /* NEUTRAL */
    --bg:#f4f7fb;
    --card:#ffffff;
    --border:#d9e3f0;
    --text:#1f2d3d;
    --muted:#6c7a89;

    /* STATUS */
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

/* ===== CONTAINER ===== */
.container{
    max-width:900px;
    margin:auto;
    background:var(--card);
    padding:26px;
    border-radius:16px;
    box-shadow:0 12px 30px rgba(0,75,141,0.15);
}

/* ===== HEADING ===== */
h2,h3,h4{
    margin-top:0;
    color:var(--primary);
}

/* ===== FORM ===== */
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
    transition:0.2s;
}

input:focus, select:focus, textarea:focus{
    outline:none;
    border-color:var(--primary);
    box-shadow:0 0 0 3px rgba(0,75,141,0.15);
}

textarea{ resize:vertical; }

/* ===== BUTTON ===== */
button{
    padding:11px 16px;
    border:none;
    border-radius:12px;
    font-size:14px;
    font-weight:600;
    cursor:pointer;
    margin-top:10px;
    transition:0.2s;
}

button{
    background:var(--primary);
    color:#fff;
}

button:hover{
    background:#003a6d;
}

button.secondary{
    background:var(--secondary);
    color:#000;
}

button.secondary:hover{
    background:#ddb100;
}

button:active{ transform:scale(0.98); }

/* ===== ERROR ===== */
.error{
    color:var(--danger);
    font-size:14px;
}

/* ===== IMAGE PREVIEW ===== */
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

.agunan-images{
    display:flex;
    overflow-x:auto;
    max-width:170px;
}

.agunan-images img{
    width:150px;
    height:100px;
    object-fit:cover;
    border-radius:12px;
    margin-right:8px;
    border:1px solid var(--border);
}

.agunan-info{
    flex:1;
}

.agunan-info p{
    margin:5px 0;
    font-size:14px;
}

/* ===== ADMIN BUTTON ===== */
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

.admin-btns button:hover{
    background:var(--secondary-soft);
}

/* ===== SEARCH & FILTER ===== */
#searchInput,
#filterJenis,
#hargaMin,
#hargaMax{
    margin-bottom:10px;
}

/* ===== MODAL ===== */
.modal{
    display:none;
    position:fixed;
    inset:0;
    background:rgba(0,0,0,0.55);
    justify-content:center;
    align-items:center;
    z-index:999;
}

.modal-content{
    background:#fff;
    border-radius:18px;
    padding:22px;
    width:90%;
    max-width:680px;
    max-height:90%;
    overflow:auto;
    box-shadow:0 25px 60px rgba(0,0,0,0.25);
}

.close{
    float:right;
    font-size:22px;
    cursor:pointer;
    color:var(--danger);
}

/* ===== QR CODE ===== */
.qrcode{
    margin-top:12px;
    padding:12px;
    background:var(--secondary-soft);
    border-radius:14px;
    display:inline-block;
}

/* ===== LOGOUT & ACTION ===== */
button.logout{
    background:#bdc3c7;
    color:#000;
}

button.logout:hover{
    background:#aeb6bf;
}
