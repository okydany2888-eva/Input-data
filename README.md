<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>Inventory Management - Input Barang Keluar dengan Dropdown</title>
    <!-- Font Awesome 6 -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,400;14..32,500;14..32,600;14..32,700&display=swap" rel="stylesheet">
    <!-- SheetJS (XLSX) -->
    <script src="https://cdn.sheetjs.com/xlsx-0.20.2/package/dist/xlsx.full.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', sans-serif;
        }

        body {
            background: #f1f5f9;
            padding: 24px 20px;
            color: #0f172a;
            transition: all 0.2s ease;
        }

        .container {
            max-width: 1600px;
            margin: 0 auto;
        }

        /* ORIENTASI DETECTION */
        body.portrait .two-panels {
            flex-direction: column;
            gap: 20px;
        }
        body.portrait .panel {
            min-width: 100%;
        }
        body.portrait .filter-bar {
            flex-direction: column;
            align-items: stretch;
        }
        body.portrait .rekap-header {
            flex-direction: column;
            align-items: flex-start;
        }
        body.portrait .input-row {
            flex-direction: column;
        }
        body.landscape .two-panels {
            flex-direction: row;
            flex-wrap: wrap;
        }
        body.landscape .panel {
            flex: 1;
            min-width: 420px;
        }

        .orientation-badge {
            position: fixed;
            bottom: 16px;
            right: 9px;
            background: rgba(0,0,0,0.6);
            backdrop-filter: blur(4px);
            color: white;
            padding: 6px 12px;
            border-radius: 40px;
            font-size: 0.7rem;
            z-index: 999;
            pointer-events: none;
        }

        .dashboard-header {
            margin-bottom: 28px;
        }

        h1 {
            font-size: 1.8rem;
            font-weight: 700;
            background: linear-gradient(135deg, #1e3c72, #2b3b6e);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 8px;
        }

        .sub {
            color: #334155;
            margin-bottom: 20px;
            border-left: 4px solid #3b82f6;
            padding-left: 14px;
            font-weight: 500;
        }

        /* Dua panel utama */
        .two-panels {
            display: flex;
            gap: 28px;
            margin-bottom: 40px;
        }

        .panel {
            background: white;
            border-radius: 28px;
            box-shadow: 0 12px 30px rgba(0,0,0,0.05);
            overflow: hidden;
            border: 1px solid #e2e8f0;
        }

        .panel-header {
            padding: 18px 24px;
            border-bottom: 2px solid #eef2ff;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 12px;
        }

        .panel-header h2 {
            font-size: 1.4rem;
            font-weight: 700;
            color: #0f3b5c;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .btn-add {
            background: #3b82f6;
            border: none;
            color: white;
            padding: 8px 18px;
            border-radius: 40px;
            font-weight: 600;
            cursor: pointer;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            transition: 0.2s;
        }
        .btn-add:hover { background: #2563eb; transform: scale(1.02); }

        .table-wrapper {
            overflow-x: auto;
            padding: 0 16px 20px 16px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.85rem;
        }

        th, td {
            padding: 12px 8px;
            text-align: left;
            border-bottom: 1px solid #e9eef3;
        }

        th {
            background-color: #f8fafc;
            font-weight: 600;
        }

        .action-icons i {
            margin: 0 6px;
            cursor: pointer;
            font-size: 1rem;
        }
        .fa-edit { color: #3b82f6; }
        .fa-trash-alt { color: #ef4444; }

        /* FILTER BAR (untuk supplier & satuan global) */
        .filter-bar {
            background: white;
            border-radius: 24px;
            padding: 16px 24px;
            margin-bottom: 28px;
            display: flex;
            flex-wrap: wrap;
            align-items: flex-end;
            gap: 20px;
            border: 1px solid #e2e8f0;
        }
        .filter-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
            min-width: 160px;
        }
        .filter-group label {
            font-size: 0.75rem;
            font-weight: 600;
            text-transform: uppercase;
            color: #475569;
        }
        select, input {
            padding: 8px 12px;
            border-radius: 16px;
            border: 1px solid #cbd5e1;
            background: white;
        }
        .btn-reset {
            background: #f1f5f9;
            border: 1px solid #cbd5e1;
            padding: 8px 20px;
            border-radius: 30px;
            cursor: pointer;
            font-weight: 500;
        }
        .btn-reset-danger {
            background: #fee2e2;
            border-color: #fecaca;
            color: #b91c1c;
        }

        /* TABEL INPUT DROPDOWN (dua bagian: barang masuk & barang keluar) */
        .input-container {
            display: flex;
            flex-wrap: wrap;
            gap: 24px;
            margin-bottom: 28px;
        }
        .input-card {
            flex: 1;
            background: white;
            border-radius: 28px;
            padding: 20px 24px;
            border: 1px solid #e2e8f0;
            box-shadow: 0 4px 12px rgba(0,0,0,0.03);
        }
        .input-card h3 {
            font-size: 1.2rem;
            margin-bottom: 16px;
            display: flex;
            align-items: center;
            gap: 10px;
            color: #0f3b5c;
        }
        .input-row {
            display: flex;
            flex-wrap: wrap;
            gap: 16px;
            align-items: flex-end;
        }
        .input-item {
            display: flex;
            flex-direction: column;
            gap: 6px;
            flex: 1;
            min-width: 130px;
        }
        .input-item label {
            font-size: 0.7rem;
            font-weight: 600;
            color: #334155;
        }
        .input-item input, .input-item select {
            padding: 8px 10px;
            border-radius: 14px;
            border: 1px solid #cbd5e1;
        }
        .btn-submit {
            background: #10b981;
            color: white;
            border: none;
            padding: 8px 20px;
            border-radius: 30px;
            cursor: pointer;
            font-weight: 600;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }
        .btn-submit-keluar {
            background: #f59e0b;
        }
        .btn-submit:hover { filter: brightness(0.95); }

        /* REKAP SECTION */
        .rekap-section {
            background: white;
            border-radius: 28px;
            padding: 20px 24px;
            margin-top: 20px;
            border: 1px solid #e2e8f0;
        }
        .rekap-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            margin-bottom: 20px;
            gap: 15px;
        }
        .rekap-search {
            display: flex;
            gap: 12px;
            align-items: center;
            background: #f1f5f9;
            padding: 6px 16px;
            border-radius: 40px;
        }
        .rekap-search input {
            border: none;
            background: transparent;
            padding: 8px 0;
            outline: none;
            width: 220px;
        }
        .rekap-actions {
            display: flex;
            gap: 12px;
        }
        .btn-excel, .btn-print {
            background: #f1f5f9;
            border: 1px solid #cbd5e1;
            padding: 8px 18px;
            border-radius: 30px;
            cursor: pointer;
            font-weight: 600;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }
        .btn-excel { background: #e8f5e9; color: #2e7d32; }
        .btn-print { background: #e3f2fd; color: #1565c0; }
        .badge { background: #eef2ff; padding: 4px 10px; border-radius: 40px; font-size: 0.7rem; }

        @media (max-width: 780px) {
            body { padding: 16px; }
            .input-container { flex-direction: column; }
        }
        @media print {
            .filter-bar, .two-panels, .btn-add, .btn-reset, .btn-excel, .btn-print, 
            .rekap-actions, .action-icons, .input-container, .orientation-badge, .rekap-search { display: none !important; }
        }
        .small-note { font-size: 0.7rem; color: #5b6e8c; margin-top: 12px; }
    </style>
</head>
<body>
<div class="container">
    <div class="dashboard-header">
        <h1><i class="fas fa-boxes"></i> Manajemen Inventory + Brand</h1>
        <div class="sub">Input Dropdown Barang Masuk & Barang Keluar/Consumable</div>
    </div>

        </div>
    </div>

    <!-- DUA TABEL INPUT: Barang Masuk & Barang Keluar (dengan dropdown) -->
    <div class="input-container">
        <!-- Card Input Barang Masuk -->
        <div class="input-card">
            <h3><i class="fas fa-arrow-down"></i> Input Barang Masuk (Supplier)</h3>
            <div class="input-row">
                <div class="input-item"><label>Tanggal</label><input type="date" id="inputTanggalMasuk" value=""></div>
                <div class="input-item"><label>Brand</label><select id="inputBrandMasuk"><option value="">Pilih Brand</option></select></div>
                <div class="input-item"><label>Nama Barang</label><select id="inputNamaBarangMasuk"><option value="">Pilih Barang</option></select></div>
                <div class="input-item"><label>Jumlah</label><input type="number" id="inputJumlahMasuk" value="1" min="1"></div>
                <div class="input-item"><label>Satuan</label><select id="inputSatuanMasuk"><option value="">Pilih Satuan</option></select></div>
                <div class="input-item"><label>Supplier</label><input type="text" id="inputSupplierMasuk" placeholder="Supplier"></div>
                <button class="btn-submit" id="btnSubmitMasuk"><i class="fas fa-plus-circle"></i> Tambah Masuk</button>
            </div>
        </div>

        <!-- Card Input Barang Keluar / Consumable (Dropdown Brand + Nama Barang) -->
        <div class="input-card">
            <h3><i class="fas fa-arrow-up"></i> Input Barang Keluar (Consumable)</h3>
            <div class="input-row">
                <div class="input-item"><label>Tanggal Keluar</label><input type="date" id="inputTanggalKeluar" value=""></div>
                <div class="input-item"><label>Brand</label><select id="inputBrandKeluar"><option value="">Pilih Brand</option></select></div>
                <div class="input-item"><label>Nama Barang</label><select id="inputNamaBarangKeluar"><option value="">Pilih Barang</option></select></div>
                <div class="input-item"><label>Jumlah Keluar</label><input type="number" id="inputJumlahKeluar" value="1" min="1"></div>
                <div class="input-item"><label>Satuan</label><select id="inputSatuanKeluar"><option value="">Pilih Satuan</option></select></div>
                <div class="input-item"><label>Nama PIC</label><input type="text" id="inputPicKeluar" placeholder="Penanggung jawab"></div>
                <button class="btn-submit btn-submit-keluar" id="btnSubmitKeluar"><i class="fas fa-minus-circle"></i> Tambah Keluar</button>
            </div>
            <div class="small-note" style="margin-top:8px;">* Data keluar akan mengurangi stok pada rekap.</div>
        </div>
    </div>

    <!-- Dua Panel Utama (tampilan data) -->
    <div class="two-panels">
        <div class="panel">
            <div class="panel-header">
                <h2><i class="fas fa-truck"></i> Data Barang Masuk Supplier</h2>
            </div>
            <div class="table-wrapper">
                <table id="tableMasuk">
                    <thead><tr><th>Tanggal</th><th>Brand</th><th>Nama Barang</th><th>Supplier</th><th>Jumlah</th><th>Satuan</th><th>Aksi</th></tr></thead>
                    <tbody id="tbodyMasuk"></tbody>
                </table>
            </div>
        </div>
        <div class="panel">
            <div class="panel-header">
                <h2><i class="fas fa-clipboard-list"></i> Stock Barang Consumable</h2>
            </div>
            <div class="table-wrapper">
                <table id="tableKeluar">
                    <thead><tr><th>Total Stok Masuk</th><th>Tanggal Keluar</th><th>Brand</th><th>Nama Barang</th><th>Jumlah Keluar</th><th>Satuan</th><th>PIC</th><th>Aksi</th></tr></thead>
                    <tbody id="tbodyKeluar"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- REKAP STOK AKHIR dengan Pencarian -->
    <div class="rekap-section">
        <div class="rekap-header">
            <h3><i class="fas fa-chart-line"></i> Rekap Stok Akhir (Brand + Barang)</h3>
            <div style="display: flex; gap: 12px; flex-wrap: wrap;">
                <div class="rekap-search">
                    <i class="fas fa-search"></i>
                    <input type="text" id="rekapSearchInput" placeholder="Cari Brand / Nama Barang...">
                    <button id="clearRekapSearch" style="background:none; border:none; cursor:pointer;"><i class="fas fa-times-circle"></i></button>
                </div>
                <div class="rekap-actions">
                    <button class="btn-excel" id="exportExcelBtn"><i class="fas fa-file-excel"></i> Export Excel</button>
                    <button class="btn-print" id="printRekapBtn"><i class="fas fa-print"></i> Cetak Rekap</button>
                </div>
            </div>
        </div>
        <div class="rekap-grid" id="rekapContainer"></div>
        <div class="small-note">* Stok Akhir = Total Masuk - Total Keluar. Pencarian hanya pada rekap ini.</div>
    </div>
</div>
<div class="orientation-badge" id="orientationBadge">📱 Portrait</div>

<script>
    // ========== ORIENTASI ==========
    function detectOrientation() {
        const isPortrait = window.matchMedia("(orientation: portrait)").matches;
        document.body.classList.toggle('portrait', isPortrait);
        document.body.classList.toggle('landscape', !isPortrait);
        document.getElementById('orientationBadge').innerHTML = isPortrait ? '📱 Portrait' : '🖥️ Landscape';
    }
    window.addEventListener('resize', detectOrientation);
    window.addEventListener('orientationchange', () => setTimeout(detectOrientation, 50));
    
    // ========== DATA MODEL ==========
    let dataMasuk = [];     // { id, tanggal, brand, namaBarang, supplier, jumlah, satuan }
    let dataKeluar = [];    // { id, tanggalKeluar, brand, barangKeluar, jumlahKeluar, satuan, namaPic }

    // Helper functions
    function getUniqueKey(brand, nama) { return `${brand}|${nama}`.toLowerCase(); }
    function getTotalMasuk(brand, nama) { return dataMasuk.filter(m => m.brand === brand && m.namaBarang === nama).reduce((s, m) => s + m.jumlah, 0); }
    function getTotalKeluar(brand, nama) { return dataKeluar.filter(k => k.brand === brand && k.barangKeluar === nama).reduce((s, k) => s + k.jumlahKeluar, 0); }
    function getAllItems() {
        const map = new Map();
        dataMasuk.forEach(m => map.set(getUniqueKey(m.brand, m.namaBarang), { brand: m.brand, namaBarang: m.namaBarang }));
        dataKeluar.forEach(k => map.set(getUniqueKey(k.brand, k.barangKeluar), { brand: k.brand, namaBarang: k.barangKeluar }));
        return Array.from(map.values());
    }

    // Load / Save Storage
    function loadFromStorage() {
        const storedMasuk = localStorage.getItem('inv_masuk_brand_final2');
        const storedKeluar = localStorage.getItem('inv_keluar_brand_final2');
        if(storedMasuk) dataMasuk = JSON.parse(storedMasuk);
        else {
            dataMasuk = [
                { id: 1, tanggal: '2025-03-10', brand: 'Sidney', namaBarang: 'Kertas A4', supplier: 'PT Maju Jaya', jumlah: 500, satuan: 'Rim' },
                { id: 2, tanggal: '2025-03-15', brand: 'Epson', namaBarang: 'Tinta Hitam', supplier: 'Sumber Makmur', jumlah: 20, satuan: 'Botol' },
                { id: 3, tanggal: '2025-04-05', brand: 'Sidney', namaBarang: 'Kertas A4', supplier: 'PT Maju Jaya', jumlah: 300, satuan: 'Rim' },
                { id: 4, tanggal: '2025-04-12', brand: 'Snowman', namaBarang: 'Spidol Boardmarker', supplier: 'Alat Kantor', jumlah: 50, satuan: 'Pcs' }
            ];
        }
        if(storedKeluar) dataKeluar = JSON.parse(storedKeluar);
        else {
            dataKeluar = [
                { id: 101, tanggalKeluar: '2025-03-20', brand: 'Sidney', barangKeluar: 'Kertas A4', jumlahKeluar: 200, satuan: 'Rim', namaPic: 'Andi' },
                { id: 102, tanggalKeluar: '2025-03-28', brand: 'Epson', barangKeluar: 'Tinta Hitam', jumlahKeluar: 5, satuan: 'Botol', namaPic: 'Budi' },
                { id: 103, tanggalKeluar: '2025-04-10', brand: 'Sidney', barangKeluar: 'Kertas A4', jumlahKeluar: 150, satuan: 'Rim', namaPic: 'Citra' },
                { id: 104, tanggalKeluar: '2025-04-18', brand: 'Snowman', barangKeluar: 'Spidol Boardmarker', jumlahKeluar: 10, satuan: 'Pcs', namaPic: 'Dewi' }
            ];
        }
    }
    function saveToStorage() { localStorage.setItem('inv_masuk_brand_final2', JSON.stringify(dataMasuk)); localStorage.setItem('inv_keluar_brand_final2', JSON.stringify(dataKeluar)); }

    // Update semua dropdown dinamis (untuk input cards)
    function updateAllDropdowns() {
        const items = getAllItems();
        const brandSet = new Set(items.map(i => i.brand));
        const barangByBrand = {};
        items.forEach(i => { if(!barangByBrand[i.brand]) barangByBrand[i.brand] = new Set(); barangByBrand[i.brand].add(i.namaBarang); });
        
        // Fungsi untuk populate dropdown barang berdasarkan brand yang dipilih
        function setupBrandBarangSync(brandSelectId, barangSelectId) {
            const brandSelect = document.getElementById(brandSelectId);
            const barangSelect = document.getElementById(barangSelectId);
            if(!brandSelect || !barangSelect) return;
            const currentBrand = brandSelect.value;
            brandSelect.innerHTML = '<option value="">Pilih Brand</option>' + Array.from(brandSet).map(b => `<option value="${escapeHtml(b)}">${escapeHtml(b)}</option>`).join('');
            if(currentBrand && brandSet.has(currentBrand)) brandSelect.value = currentBrand;
            const updateBarang = () => {
                const selectedBrand = brandSelect.value;
                barangSelect.innerHTML = '<option value="">Pilih Barang</option>';
                if(selectedBrand && barangByBrand[selectedBrand]) {
                    Array.from(barangByBrand[selectedBrand]).forEach(nama => {
                        barangSelect.innerHTML += `<option value="${escapeHtml(nama)}">${escapeHtml(nama)}</option>`;
                    });
                }
            };
            brandSelect.onchange = updateBarang;
            updateBarang();
        }
        
        setupBrandBarangSync('inputBrandMasuk', 'inputNamaBarangMasuk');
        setupBrandBarangSync('inputBrandKeluar', 'inputNamaBarangKeluar');
        
        // Dropdown satuan (global untuk masuk dan keluar)
        const allSatuan = [...new Set([...dataMasuk.map(m=>m.satuan), ...dataKeluar.map(k=>k.satuan)])];
        const satuanMasuk = document.getElementById('inputSatuanMasuk');
        const satuanKeluar = document.getElementById('inputSatuanKeluar');
        const satuanOptions = '<option value="">Pilih Satuan</option>' + allSatuan.map(s=>`<option value="${escapeHtml(s)}">${escapeHtml(s)}</option>`).join('');
        if(satuanMasuk) satuanMasuk.innerHTML = satuanOptions;
        if(satuanKeluar) satuanKeluar.innerHTML = satuanOptions;
        
        const supplierSelect = document.getElementById('dropdownSupplier');
        const allSupplier = [...new Set(dataMasuk.map(m=>m.supplier))];
        supplierSelect.innerHTML = '<option value="">-- Semua Supplier --</option>' + allSupplier.map(s=>`<option value="${escapeHtml(s)}">${escapeHtml(s)}</option>`).join('');
        
        const globalSatuan = document.getElementById('dropdownSatuan');
        globalSatuan.innerHTML = '<option value="">-- Semua Satuan --</option>' + allSatuan.map(s=>`<option value="${escapeHtml(s)}">${escapeHtml(s)}</option>`).join('');
    }
    
    // INPUT DARI DROPDOWN: Barang Masuk
    function addBarangMasuk() {
        const tanggal = document.getElementById('inputTanggalMasuk').value;
        if(!tanggal) { alert("Isi tanggal masuk"); return; }
        const brand = document.getElementById('inputBrandMasuk').value;
        if(!brand) { alert("Pilih Brand"); return; }
        const namaBarang = document.getElementById('inputNamaBarangMasuk').value;
        if(!namaBarang) { alert("Pilih Nama Barang"); return; }
        let jumlah = parseInt(document.getElementById('inputJumlahMasuk').value);
        if(isNaN(jumlah) || jumlah <=0) jumlah = 1;
        const satuan = document.getElementById('inputSatuanMasuk').value;
        if(!satuan) { alert("Pilih Satuan"); return; }
        let supplier = document.getElementById('inputSupplierMasuk').value.trim();
        if(!supplier) supplier = "Umum";
        const newId = dataMasuk.length ? Math.max(...dataMasuk.map(m=>m.id)) + 1 : 1;
        dataMasuk.push({ id: newId, tanggal, brand, namaBarang, supplier, jumlah, satuan });
        refreshAll();
        alert("Barang masuk ditambahkan!");
        document.getElementById('inputJumlahMasuk').value = 1;
        document.getElementById('inputSupplierMasuk').value = "";
    }
    
    // INPUT DROPDOWN KHUSUS BARANG KELUAR / CONSUMABLE
    function addBarangKeluar() {
        const tanggalKeluar = document.getElementById('inputTanggalKeluar').value;
        if(!tanggalKeluar) { alert("Isi tanggal keluar"); return; }
        const brand = document.getElementById('inputBrandKeluar').value;
        if(!brand) { alert("Pilih Brand"); return; }
        const barangKeluar = document.getElementById('inputNamaBarangKeluar').value;
        if(!barangKeluar) { alert("Pilih Nama Barang"); return; }
        let jumlahKeluar = parseInt(document.getElementById('inputJumlahKeluar').value);
        if(isNaN(jumlahKeluar) || jumlahKeluar <=0) jumlahKeluar = 1;
        const satuan = document.getElementById('inputSatuanKeluar').value;
        if(!satuan) { alert("Pilih Satuan"); return; }
        let pic = document.getElementById('inputPicKeluar').value.trim();
        if(!pic) pic = "Admin";
        
        // Validasi stok (opsional peringatan)
        const totalStok = getTotalMasuk(brand, barangKeluar) - getTotalKeluar(brand, barangKeluar);
        if(totalStok < jumlahKeluar) {
            if(!confirm(`Peringatan: Stok saat ini ${totalStok} ${satuan}, Anda akan mengambil ${jumlahKeluar} ${satuan}. Lanjutkan?`)) return;
        }
        
        const newId = dataKeluar.length ? Math.max(...dataKeluar.map(k=>k.id)) + 1 : 201;
        dataKeluar.push({ id: newId, tanggalKeluar, brand, barangKeluar, jumlahKeluar, satuan, namaPic: pic });
        refreshAll();
        alert("Barang keluar dicatat!");
        document.getElementById('inputJumlahKeluar').value = 1;
        document.getElementById('inputPicKeluar').value = "";
    }
    
    // RENDER TABEL MASUK & KELUAR
    function renderTabelMasuk() {
        const tbody = document.getElementById('tbodyMasuk');
        if(!dataMasuk.length) { tbody.innerHTML = '<tr><td colspan="7">Tidak ada data</td></tr>'; return; }
        tbody.innerHTML = dataMasuk.map(m => `
            <tr>
                <td>${escapeHtml(m.tanggal)}</td><td><strong>${escapeHtml(m.brand)}</strong></td><td>${escapeHtml(m.namaBarang)}</td>
                <td>${escapeHtml(m.supplier)}</td><td>${m.jumlah}</td><td>${escapeHtml(m.satuan)}</td>
                <td class="action-icons"><i class="fas fa-edit" onclick="editMasuk(${m.id})"></i><i class="fas fa-trash-alt" onclick="hapusMasuk(${m.id})"></i></td>
            </tr>
        `).join('');
    }
    function renderTabelKeluar() {
        const tbody = document.getElementById('tbodyKeluar');
        if(!dataKeluar.length) { tbody.innerHTML = '<tr><td colspan="8">Tidak ada data</td></tr>'; return; }
        tbody.innerHTML = dataKeluar.map(k => {
            const stokMasuk = getTotalMasuk(k.brand, k.barangKeluar);
            return `<tr>
                <td>${stokMasuk}</td><td>${escapeHtml(k.tanggalKeluar)}</td><td><strong>${escapeHtml(k.brand)}</strong></td>
                <td>${escapeHtml(k.barangKeluar)}</td><td>${k.jumlahKeluar}</td><td>${escapeHtml(k.satuan)}</td>
                <td>${escapeHtml(k.namaPic)}</td>
                <td class="action-icons"><i class="fas fa-edit" onclick="editKeluar(${k.id})"></i><i class="fas fa-trash-alt" onclick="hapusKeluar(${k.id})"></i></td>
            </tr>`;
        }).join('');
    }
    
    // REKAP dengan pencarian
    function buildRekapStok() {
        const searchTerm = document.getElementById('rekapSearchInput').value.toLowerCase();
        let items = getAllItems();
        if(searchTerm) items = items.filter(i => i.brand.toLowerCase().includes(searchTerm) || i.namaBarang.toLowerCase().includes(searchTerm));
        let html = '<table style="width:100%"><thead><tr style="background:#f1f5f9;"><th>Brand</th><th>Nama Barang</th><th>Total Masuk</th><th>Total Keluar</th><th>Stok Akhir</th><th>Rincian Pengambilan</th></tr></thead><tbody>';
        for(let item of items) {
            const totalMasuk = getTotalMasuk(item.brand, item.namaBarang);
            const totalKeluar = getTotalKeluar(item.brand, item.namaBarang);
            const stokAkhir = totalMasuk - totalKeluar;
            const riwayat = dataKeluar.filter(k => k.brand === item.brand && k.barangKeluar === item.namaBarang);
            let rincian = riwayat.length ? riwayat.map(k => `${k.tanggalKeluar} : ${k.jumlahKeluar} ${k.satuan} (PIC: ${escapeHtml(k.namaPic)})`).join('<br/>') : '<span class="badge">Tidak ada</span>';
            html += `<tr><td>${escapeHtml(item.brand)}</td><td>${escapeHtml(item.namaBarang)}</td><td>${totalMasuk}</td><td>${totalKeluar}</td><td style="background:#fef9e3; font-weight:bold;">${stokAkhir}</td><td style="font-size:0.75rem;">${rincian}</td></tr>`;
        }
        html += `</tbody></table>`;
        if(!items.length) html = '<p class="small-note">Tidak ada data sesuai pencarian</p>';
        document.getElementById('rekapContainer').innerHTML = html;
    }
    
    // Export & Print
    function exportToExcel() {
        const items = getAllItems();
        const data = items.map(item => ({
            Brand: item.brand, "Nama Barang": item.namaBarang,
            "Total Masuk": getTotalMasuk(item.brand, item.namaBarang),
            "Total Keluar": getTotalKeluar(item.brand, item.namaBarang),
            "Stok Akhir": getTotalMasuk(item.brand, item.namaBarang) - getTotalKeluar(item.brand, item.namaBarang),
            "Rincian Pengambilan": dataKeluar.filter(k => k.brand === item.brand && k.barangKeluar === item.namaBarang).map(k => `${k.tanggalKeluar}:${k.jumlahKeluar} ${k.satuan} (${k.namaPic})`).join("; ")
        }));
        if(!data.length) { alert("Tidak ada data"); return; }
        const ws = XLSX.utils.json_to_sheet(data);
        const wb = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(wb, ws, "Rekap_Stok");
        XLSX.writeFile(wb, `Rekap_Stok_${new Date().toISOString().slice(0,10)}.xlsx`);
    }
    function printRekap() {
        const container = document.getElementById('rekapContainer').cloneNode(true);
        const win = window.open('', '_blank');
        win.document.write(`<html><head><title>Rekap Stok</title><style>table{border-collapse:collapse; width:100%} th,td{border:1px solid #aaa; padding:6px}</style></head><body><h2>Rekap Stok Akhir</h2>${container.innerHTML}</body></html>`);
        win.document.close(); win.print(); win.onafterprint = () => win.close();
    }
    
    // CRUD Manual (opsional)
    function tambahMasukManual() {
        let newId = dataMasuk.length ? Math.max(...dataMasuk.map(m=>m.id)) + 1 : 1;
        let tanggal = prompt("Tanggal YYYY-MM-DD:", new Date().toISOString().slice(0,10)); if(!tanggal) return;
        let brand = prompt("Brand:"); if(!brand) return;
        let nama = prompt("Nama Barang:"); if(!nama) return;
        let supplier = prompt("Supplier:");
        let jumlah = parseInt(prompt("Jumlah:","0")); if(isNaN(jumlah)) jumlah=0;
        let satuan = prompt("Satuan:");
        dataMasuk.push({ id: newId, tanggal, brand, namaBarang: nama, supplier, jumlah, satuan });
        refreshAll();
    }
    function tambahKeluarManual() {
        let newId = dataKeluar.length ? Math.max(...dataKeluar.map(k=>k.id)) + 1 : 201;
        let tgl = prompt("Tanggal Keluar:"); if(!tgl) return;
        let brand = prompt("Brand Barang:"); if(!brand) return;
        let barang = prompt("Nama Barang:"); if(!barang) return;
        let jml = parseInt(prompt("Jumlah Keluar:")); if(isNaN(jml)) jml=0;
        let satuan = prompt("Satuan:");
        let pic = prompt("Nama PIC:");
        dataKeluar.push({ id: newId, tanggalKeluar: tgl, brand, barangKeluar: barang, jumlahKeluar: jml, satuan, namaPic: pic });
        refreshAll();
    }
    function editMasuk(id) { let item = dataMasuk.find(m=>m.id===id); if(item) { let t=prompt("Tanggal",item.tanggal); if(t) item.tanggal=t; let b=prompt("Brand",item.brand); if(b) item.brand=b; let n=prompt("Nama",item.namaBarang); if(n) item.namaBarang=n; let s=prompt("Supplier",item.supplier); if(s) item.supplier=s; let j=parseInt(prompt("Jumlah",item.jumlah)); if(!isNaN(j)) item.jumlah=j; let sat=prompt("Satuan",item.satuan); if(sat) item.satuan=sat; refreshAll(); } }
    function hapusMasuk(id) { if(confirm("Hapus?")) { dataMasuk = dataMasuk.filter(m=>m.id!==id); refreshAll(); } }
    function editKeluar(id) { let item = dataKeluar.find(k=>k.id===id); if(item){ let t=prompt("Tgl Keluar",item.tanggalKeluar); if(t) item.tanggalKeluar=t; let b=prompt("Brand",item.brand); if(b) item.brand=b; let brg=prompt("Barang",item.barangKeluar); if(brg) item.barangKeluar=brg; let j=parseInt(prompt("Jumlah",item.jumlahKeluar)); if(!isNaN(j)) item.jumlahKeluar=j; let sat=prompt("Satuan",item.satuan); if(sat) item.satuan=sat; let pic=prompt("PIC",item.namaPic); if(pic) item.namaPic=pic; refreshAll(); } }
    function hapusKeluar(id) { if(confirm("Hapus?")) { dataKeluar = dataKeluar.filter(k=>k.id!==id); refreshAll(); } }
    function resetAll() { if(confirm("Reset permanen semua data?")) { dataMasuk=[]; dataKeluar=[]; refreshAll(); alert("Reset complete"); } }
    
    function refreshAll() {
        renderTabelMasuk();
        renderTabelKeluar();
        updateAllDropdowns();
        buildRekapStok();
        saveToStorage();
    }
    function escapeHtml(str) { if(!str) return ''; return str.replace(/[&<>]/g, m=> m==='&'?'&amp;': m==='<'?'&lt;':'&gt;'); }
    
    // INIT
    window.onload = () => {
        loadFromStorage();
        refreshAll();
        detectOrientation();
        // Set default tanggal
        const today = new Date().toISOString().slice(0,10);
        document.getElementById('inputTanggalMasuk').value = today;
        document.getElementById('inputTanggalKeluar').value = today;
        
        document.getElementById('btnSubmitMasuk').onclick = addBarangMasuk;
        document.getElementById('btnSubmitKeluar').onclick = addBarangKeluar;
        document.getElementById('btnAddMasukManual').onclick = tambahMasukManual;
        document.getElementById('btnAddKeluarManual').onclick = tambahKeluarManual;
        document.getElementById('resetPermanenBtn').onclick = resetAll;
        document.getElementById('exportExcelBtn').onclick = exportToExcel;
        document.getElementById('printRekapBtn').onclick = printRekap;
        
        const rekapSearch = document.getElementById('rekapSearchInput');
        rekapSearch.addEventListener('input', () => buildRekapStok());
        document.getElementById('clearRekapSearch').onclick = () => { rekapSearch.value = ''; buildRekapStok(); };
    };
    window.editMasuk = editMasuk; window.hapusMasuk = hapusMasuk; window.editKeluar = editKeluar; window.hapusKeluar = hapusKeluar;
</script>
</body>
</html>
