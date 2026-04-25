<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>Inventory Management - Mobile | Barang Masuk & Keluar</title>
    <!-- Font Awesome 6 -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,400;500;600;700&display=swap" rel="stylesheet">
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
            padding: 12px;
            color: #0f172a;
        }

        .container {
            max-width: 100%;
            margin: 0 auto;
        }

        /* Header */
        .dashboard-header {
            margin-bottom: 16px;
        }
        .dashboard-header h1 {
            font-size: 1.3rem;
            font-weight: 700;
            background: linear-gradient(135deg, #1e3c72, #2b3b6e);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .sub {
            font-size: 0.7rem;
            color: #475569;
            margin-top: 4px;
            padding-left: 8px;
            border-left: 3px solid #3b82f6;
        }

        /* KARTU INPUT */
        .input-container {
            display: flex;
            flex-direction: column;
            gap: 16px;
            margin-bottom: 20px;
        }
        .input-card {
            background: white;
            border-radius: 20px;
            padding: 14px 16px;
            border: 1px solid #e2e8f0;
            box-shadow: 0 2px 8px rgba(0,0,0,0.03);
        }
        .input-card h3 {
            font-size: 0.95rem;
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            gap: 6px;
            color: #0f3b5c;
        }
        .input-row {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            align-items: flex-end;
        }
        .input-item {
            display: flex;
            flex-direction: column;
            gap: 4px;
            flex: 1;
            min-width: 100px;
        }
        .input-item label {
            font-size: 0.6rem;
            font-weight: 600;
            color: #334155;
        }
        .input-item input, .input-item select {
            padding: 6px 8px;
            border-radius: 12px;
            border: 1px solid #cbd5e1;
            background: white;
            font-size: 0.75rem;
        }
        .btn-submit {
            background: #10b981;
            color: white;
            border: none;
            padding: 6px 16px;
            border-radius: 30px;
            cursor: pointer;
            font-weight: 600;
            font-size: 0.75rem;
            display: inline-flex;
            align-items: center;
            gap: 6px;
            margin-top: 4px;
        }
        .btn-submit-keluar {
            background: #f59e0b;
        }

        /* Dua Panel */
        .two-panels {
            display: flex;
            flex-direction: column;
            gap: 16px;
            margin-bottom: 20px;
        }
        .panel {
            background: white;
            border-radius: 20px;
            overflow: hidden;
            border: 1px solid #e2e8f0;
            width: 100%;
        }
        .panel-header {
            padding: 12px 16px;
            background: #fafcff;
            border-bottom: 1px solid #eef2ff;
        }
        .panel-header h2 {
            font-size: 1rem;
            font-weight: 700;
            color: #0f3b5c;
            display: flex;
            align-items: center;
            gap: 6px;
        }
        /* Tombol manual disembunyikan */
        .btn-add {
            display: none;
        }
        .table-wrapper {
            overflow-x: auto;
            padding: 0 12px 12px 12px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.7rem;
            min-width: 480px;
        }
        th, td {
            padding: 8px 4px;
            text-align: left;
            border-bottom: 1px solid #e9eef3;
        }
        th {
            background-color: #f8fafc;
            font-weight: 600;
        }
        .action-icons i {
            margin: 0 3px;
            cursor: pointer;
            font-size: 0.85rem;
        }
        .fa-edit { color: #3b82f6; }
        .fa-trash-alt { color: #ef4444; }

        /* REKAP SECTION */
        .rekap-section {
            background: white;
            border-radius: 20px;
            padding: 14px 16px;
            border: 1px solid #e2e8f0;
        }
        .rekap-header {
            margin-bottom: 12px;
        }
        .rekap-header h3 {
            font-size: 1rem;
            display: flex;
            align-items: center;
            gap: 6px;
            margin-bottom: 10px;
        }
        .rekap-search {
            display: flex;
            gap: 8px;
            align-items: center;
            background: #f1f5f9;
            padding: 6px 12px;
            border-radius: 40px;
            margin-bottom: 10px;
        }
        .rekap-search input {
            border: none;
            background: transparent;
            padding: 6px 0;
            outline: none;
            width: 100%;
            font-size: 0.8rem;
        }
        .rekap-actions {
            display: flex;
            gap: 8px;
            justify-content: flex-end;
        }
        .btn-excel, .btn-print {
            background: #f1f5f9;
            border: 1px solid #cbd5e1;
            padding: 5px 12px;
            border-radius: 30px;
            cursor: pointer;
            font-weight: 600;
            font-size: 0.65rem;
            display: inline-flex;
            align-items: center;
            gap: 4px;
        }
        .btn-excel { background: #e8f5e9; color: #2e7d32; }
        .btn-print { background: #e3f2fd; color: #1565c0; }
        .badge { background: #eef2ff; padding: 3px 8px; border-radius: 30px; font-size: 0.6rem; }
        .small-note {
            font-size: 0.6rem;
            color: #5b6e8c;
            margin-top: 10px;
            text-align: center;
        }
        button {
            touch-action: manipulation;
        }
        @media print {
            .input-container, .btn-excel, .btn-print, .rekap-actions, .action-icons {
                display: none !important;
            }
        }
    </style>
</head>
<body>
<div class="container">
    <div class="dashboard-header">
        <h1><i class="fas fa-boxes"></i> Inventory Management</h1>
        <div class="sub">Barang Masuk Supplier & Stok Consumable</div>
    </div>

    <!-- KARTU INPUT: Barang Masuk -->
    <div class="input-container">
        <div class="input-card">
            <h3><i class="fas fa-arrow-down" style="color:#10b981;"></i> + Barang Masuk (Supplier)</h3>
            <div class="input-row">
                <div class="input-item"><label>Tanggal</label><input type="date" id="inputTanggalMasuk"></div>
                <div class="input-item"><label>Brand</label><select id="inputBrandMasuk"><option value="">Pilih Brand</option></select></div>
                <div class="input-item"><label>Nama Barang</label><select id="inputNamaBarangMasuk"><option value="">Pilih Barang</option></select></div>
                <div class="input-item"><label>Jumlah</label><input type="number" id="inputJumlahMasuk" value="1" min="1"></div>
                <div class="input-item"><label>Satuan</label><select id="inputSatuanMasuk"><option value="">Pilih Satuan</option></select></div>
                <div class="input-item"><label>Supplier</label><input type="text" id="inputSupplierMasuk" placeholder="Nama supplier"></div>
                <button class="btn-submit" id="btnSubmitMasuk"><i class="fas fa-plus-circle"></i> Tambah</button>
            </div>
        </div>

        <!-- KARTU INPUT: Barang Keluar -->
        <div class="input-card">
            <h3><i class="fas fa-arrow-up" style="color:#f59e0b;"></i> - Barang Keluar (Consumable)</h3>
            <div class="input-row">
                <div class="input-item"><label>Tgl Keluar</label><input type="date" id="inputTanggalKeluar"></div>
                <div class="input-item"><label>Brand</label><select id="inputBrandKeluar"><option value="">Pilih Brand</option></select></div>
                <div class="input-item"><label>Nama Barang</label><select id="inputNamaBarangKeluar"><option value="">Pilih Barang</option></select></div>
                <div class="input-item"><label>Jumlah</label><input type="number" id="inputJumlahKeluar" value="1" min="1"></div>
                <div class="input-item"><label>Satuan</label><select id="inputSatuanKeluar"><option value="">Pilih Satuan</option></select></div>
                <div class="input-item"><label>PIC</label><input type="text" id="inputPicKeluar" placeholder="Penanggung jawab"></div>
                <button class="btn-submit btn-submit-keluar" id="btnSubmitKeluar"><i class="fas fa-minus-circle"></i> Tambah Keluar</button>
            </div>
            <div class="small-note" style="text-align:left; margin-top:6px;">* Stok akan berkurang sesuai jumlah keluar.</div>
        </div>
    </div>

    <!-- Data Barang Masuk -->
    <div class="two-panels">
        <div class="panel">
            <div class="panel-header">
                <h2><i class="fas fa-truck"></i> Data Barang Masuk</h2>
            </div>
            <div class="table-wrapper">
                <table id="tableMasuk">
                    <thead><tr><th>Tanggal</th><th>Brand</th><th>Nama Barang</th><th>Supplier</th><th>Jml</th><th>Satuan</th><th>Aksi</th></tr></thead>
                    <tbody id="tbodyMasuk"></tbody>
                </table>
            </div>
        </div>

        <!-- Data Stock Consumable -->
        <div class="panel">
            <div class="panel-header">
                <h2><i class="fas fa-clipboard-list"></i> Stock Consumable</h2>
            </div>
            <div class="table-wrapper">
                <table id="tableKeluar">
                    <thead><tr><th>Stok Masuk</th><th>Tgl Keluar</th><th>Brand</th><th>Nama Barang</th><th>Jml</th><th>Satuan</th><th>PIC</th><th>Aksi</th></tr></thead>
                    <tbody id="tbodyKeluar"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- REKAP STOK AKHIR -->
    <div class="rekap-section">
        <div class="rekap-header">
            <h3><i class="fas fa-chart-line"></i> Rekap Stok Akhir</h3>
            <div class="rekap-search">
                <i class="fas fa-search"></i>
                <input type="text" id="rekapSearchInput" placeholder="Cari Brand / Nama Barang...">
                <button id="clearRekapSearch" style="background:none; border:none; font-size:1rem;"><i class="fas fa-times-circle"></i></button>
            </div>
            <div class="rekap-actions">
                <button class="btn-excel" id="exportExcelBtn"><i class="fas fa-file-excel"></i> Excel</button>
                <button class="btn-print" id="printRekapBtn"><i class="fas fa-print"></i> Cetak</button>
            </div>
        </div>
        <div class="rekap-grid" id="rekapContainer"></div>
        <div class="small-note">* Stok Akhir = Total Masuk - Total Keluar (berdasarkan Brand + Nama Barang)</div>
    </div>
</div>

<script>
    // ========== DATA MODEL ==========
    let dataMasuk = [];
    let dataKeluar = [];

    function getUniqueKey(brand, nama) { return `${brand}|${nama}`.toLowerCase(); }
    function getTotalMasuk(brand, nama) {
        return dataMasuk.filter(m => m.brand === brand && m.namaBarang === nama).reduce((s, m) => s + m.jumlah, 0);
    }
    function getTotalKeluar(brand, nama) {
        return dataKeluar.filter(k => k.brand === brand && k.barangKeluar === nama).reduce((s, k) => s + k.jumlahKeluar, 0);
    }
    function getAllItems() {
        const map = new Map();
        dataMasuk.forEach(m => map.set(getUniqueKey(m.brand, m.namaBarang), { brand: m.brand, namaBarang: m.namaBarang }));
        dataKeluar.forEach(k => map.set(getUniqueKey(k.brand, k.barangKeluar), { brand: k.brand, namaBarang: k.barangKeluar }));
        return Array.from(map.values());
    }

    function loadFromStorage() {
        const storedMasuk = localStorage.getItem('inv_masuk_mobile');
        const storedKeluar = localStorage.getItem('inv_keluar_mobile');
        if(storedMasuk) dataMasuk = JSON.parse(storedMasuk);
        else {
            dataMasuk = [
                { id: 1, tanggal: '2025-04-01', brand: 'Sidney', namaBarang: 'Kertas A4', supplier: 'PT Maju Jaya', jumlah: 500, satuan: 'Rim' },
                { id: 2, tanggal: '2025-04-02', brand: 'Epson', namaBarang: 'Tinta Hitam', supplier: 'Sumber Makmur', jumlah: 20, satuan: 'Botol' },
                { id: 3, tanggal: '2025-04-10', brand: 'Snowman', namaBarang: 'Spidol', supplier: 'Alat Kantor', jumlah: 50, satuan: 'Pcs' },
                { id: 4, tanggal: '2025-04-12', brand: 'Joyko', namaBarang: 'Stabilo', supplier: 'Stationery', jumlah: 30, satuan: 'Pcs' }
            ];
        }
        if(storedKeluar) dataKeluar = JSON.parse(storedKeluar);
        else {
            dataKeluar = [
                { id: 101, tanggalKeluar: '2025-04-05', brand: 'Sidney', barangKeluar: 'Kertas A4', jumlahKeluar: 120, satuan: 'Rim', namaPic: 'Andi' },
                { id: 102, tanggalKeluar: '2025-04-08', brand: 'Epson', barangKeluar: 'Tinta Hitam', jumlahKeluar: 3, satuan: 'Botol', namaPic: 'Budi' },
                { id: 103, tanggalKeluar: '2025-04-11', brand: 'Snowman', barangKeluar: 'Spidol', jumlahKeluar: 5, satuan: 'Pcs', namaPic: 'Citra' }
            ];
        }
    }
    function saveToStorage() {
        localStorage.setItem('inv_masuk_mobile', JSON.stringify(dataMasuk));
        localStorage.setItem('inv_keluar_mobile', JSON.stringify(dataKeluar));
    }

    function updateAllDropdowns() {
        const items = getAllItems();
        const brandSet = new Set(items.map(i => i.brand));
        const barangByBrand = {};
        items.forEach(i => { if(!barangByBrand[i.brand]) barangByBrand[i.brand] = new Set(); barangByBrand[i.brand].add(i.namaBarang); });
        
        function setupBrandBarang(brandSelectId, barangSelectId) {
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
        setupBrandBarang('inputBrandMasuk', 'inputNamaBarangMasuk');
        setupBrandBarang('inputBrandKeluar', 'inputNamaBarangKeluar');
        
        const allSatuan = [...new Set([...dataMasuk.map(m=>m.satuan), ...dataKeluar.map(k=>k.satuan)])];
        const satuanMasuk = document.getElementById('inputSatuanMasuk');
        const satuanKeluar = document.getElementById('inputSatuanKeluar');
        const satuanOps = '<option value="">Pilih Satuan</option>' + allSatuan.map(s=>`<option value="${escapeHtml(s)}">${escapeHtml(s)}</option>`).join('');
        if(satuanMasuk) satuanMasuk.innerHTML = satuanOps;
        if(satuanKeluar) satuanKeluar.innerHTML = satuanOps;
    }

    function addBarangMasuk() {
        const tanggal = document.getElementById('inputTanggalMasuk').value;
        if(!tanggal) { alert("Isi tanggal masuk"); return; }
        const brand = document.getElementById('inputBrandMasuk').value;
        if(!brand) { alert("Pilih Brand"); return; }
        const namaBarang = document.getElementById('inputNamaBarangMasuk').value;
        if(!namaBarang) { alert("Pilih Nama Barang"); return; }
        let jumlah = parseInt(document.getElementById('inputJumlahMasuk').value);
        if(isNaN(jumlah) || jumlah <= 0) jumlah = 1;
        const satuan = document.getElementById('inputSatuanMasuk').value;
        if(!satuan) { alert("Pilih Satuan"); return; }
        let supplier = document.getElementById('inputSupplierMasuk').value.trim();
        if(!supplier) supplier = "Umum";
        const newId = dataMasuk.length ? Math.max(...dataMasuk.map(m=>m.id)) + 1 : 1;
        dataMasuk.push({ id: newId, tanggal, brand, namaBarang, supplier, jumlah, satuan });
        refreshAll();
        alert("Barang masuk ditambahkan");
        document.getElementById('inputJumlahMasuk').value = 1;
        document.getElementById('inputSupplierMasuk').value = "";
    }

    function addBarangKeluar() {
        const tanggalKeluar = document.getElementById('inputTanggalKeluar').value;
        if(!tanggalKeluar) { alert("Isi tanggal keluar"); return; }
        const brand = document.getElementById('inputBrandKeluar').value;
        if(!brand) { alert("Pilih Brand"); return; }
        const barangKeluar = document.getElementById('inputNamaBarangKeluar').value;
        if(!barangKeluar) { alert("Pilih Nama Barang"); return; }
        let jumlahKeluar = parseInt(document.getElementById('inputJumlahKeluar').value);
        if(isNaN(jumlahKeluar) || jumlahKeluar <= 0) jumlahKeluar = 1;
        const satuan = document.getElementById('inputSatuanKeluar').value;
        if(!satuan) { alert("Pilih Satuan"); return; }
        let pic = document.getElementById('inputPicKeluar').value.trim();
        if(!pic) pic = "Admin";
        const stokTersedia = getTotalMasuk(brand, barangKeluar) - getTotalKeluar(brand, barangKeluar);
        if(stokTersedia < jumlahKeluar) {
            if(!confirm(`Stok saat ini hanya ${stokTersedia} ${satuan}. Tetap catat pengeluaran?`)) return;
        }
        const newId = dataKeluar.length ? Math.max(...dataKeluar.map(k=>k.id)) + 1 : 201;
        dataKeluar.push({ id: newId, tanggalKeluar, brand, barangKeluar, jumlahKeluar, satuan, namaPic: pic });
        refreshAll();
        alert("Barang keluar dicatat");
        document.getElementById('inputJumlahKeluar').value = 1;
        document.getElementById('inputPicKeluar').value = "";
    }

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

    function buildRekapStok() {
        const searchTerm = document.getElementById('rekapSearchInput').value.toLowerCase();
        let items = getAllItems();
        if(searchTerm) items = items.filter(i => i.brand.toLowerCase().includes(searchTerm) || i.namaBarang.toLowerCase().includes(searchTerm));
        let html = '<table style="width:100%"><thead><tr><th>Brand</th><th>Nama Barang</th><th>Total Masuk</th><th>Total Keluar</th><th>Stok Akhir</th><th>Rincian Pengambilan</th></tr></thead><tbody>';
        for(let item of items) {
            const totalMasuk = getTotalMasuk(item.brand, item.namaBarang);
            const totalKeluar = getTotalKeluar(item.brand, item.namaBarang);
            const stokAkhir = totalMasuk - totalKeluar;
            const riwayat = dataKeluar.filter(k => k.brand === item.brand && k.barangKeluar === item.namaBarang);
            let rincian = riwayat.length ? riwayat.map(k => `${k.tanggalKeluar}: ${k.jumlahKeluar} ${k.satuan} (${k.namaPic})`).join('<br/>') : '<span class="badge">Tidak ada</span>';
            html += `<tr><td>${escapeHtml(item.brand)}</td><td>${escapeHtml(item.namaBarang)}</td><td>${totalMasuk}</td><td>${totalKeluar}</td><td style="background:#fef9e3; font-weight:bold;">${stokAkhir}</td><td style="font-size:0.65rem;">${rincian}</td></tr>`;
        }
        html += `</tbody></table>`;
        if(!items.length) html = '<p class="small-note">Tidak ada data sesuai pencarian</p>';
        document.getElementById('rekapContainer').innerHTML = html;
    }

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
        win.document.write(`<html><head><title>Rekap Stok</title><style>body{font-family:Arial;padding:20px;} table{border-collapse:collapse;width:100%} th,td{border:1px solid #aaa;padding:6px;}</style></head><body><h2>Rekap Stok Akhir</h2>${container.innerHTML}</body></html>`);
        win.document.close(); win.print(); win.onafterprint = () => win.close();
    }

    // Edit & Hapus (tanpa tombol manual)
    function editMasuk(id) { 
        let item = dataMasuk.find(m=>m.id===id); 
        if(item){ 
            let t=prompt("Tanggal",item.tanggal); if(t) item.tanggal=t; 
            let b=prompt("Brand",item.brand); if(b) item.brand=b; 
            let n=prompt("Nama Barang",item.namaBarang); if(n) item.namaBarang=n; 
            let sp=prompt("Supplier",item.supplier); if(sp) item.supplier=sp; 
            let j=parseInt(prompt("Jumlah",item.jumlah)); if(!isNaN(j)) item.jumlah=j; 
            let sat=prompt("Satuan",item.satuan); if(sat) item.satuan=sat; 
            refreshAll(); 
        } 
    }
    function hapusMasuk(id) { 
        if(confirm("Hapus data masuk?")) { 
            dataMasuk = dataMasuk.filter(m=>m.id!==id); 
            refreshAll(); 
        } 
    }
    function editKeluar(id) { 
        let item = dataKeluar.find(k=>k.id===id); 
        if(item){ 
            let t=prompt("Tanggal Keluar",item.tanggalKeluar); if(t) item.tanggalKeluar=t; 
            let b=prompt("Brand",item.brand); if(b) item.brand=b; 
            let brg=prompt("Nama Barang",item.barangKeluar); if(brg) item.barangKeluar=brg; 
            let j=parseInt(prompt("Jumlah",item.jumlahKeluar)); if(!isNaN(j)) item.jumlahKeluar=j; 
            let sat=prompt("Satuan",item.satuan); if(sat) item.satuan=sat; 
            let pic=prompt("PIC",item.namaPic); if(pic) item.namaPic=pic; 
            refreshAll(); 
        } 
    }
    function hapusKeluar(id) { 
        if(confirm("Hapus data keluar?")) { 
            dataKeluar = dataKeluar.filter(k=>k.id!==id); 
            refreshAll(); 
        } 
    }

    function refreshAll() {
        renderTabelMasuk();
        renderTabelKeluar();
        updateAllDropdowns();
        buildRekapStok();
        saveToStorage();
    }
    function escapeHtml(str) { if(!str) return ''; return str.replace(/[&<>]/g, m=> m==='&'?'&amp;': m==='<'?'&lt;':'&gt;'); }

    const today = new Date().toISOString().slice(0,10);
    window.onload = () => {
        loadFromStorage();
        refreshAll();
        document.getElementById('inputTanggalMasuk').value = today;
        document.getElementById('inputTanggalKeluar').value = today;
        document.getElementById('btnSubmitMasuk').onclick = addBarangMasuk;
        document.getElementById('btnSubmitKeluar').onclick = addBarangKeluar;
        document.getElementById('exportExcelBtn').onclick = exportToExcel;
        document.getElementById('printRekapBtn').onclick = printRekap;
        const searchInput = document.getElementById('rekapSearchInput');
        searchInput.addEventListener('input', () => buildRekapStok());
        document.getElementById('clearRekapSearch').onclick = () => { searchInput.value = ''; buildRekapStok(); };
    };
    window.editMasuk = editMasuk; window.hapusMasuk = hapusMasuk; window.editKeluar = editKeluar; window.hapusKeluar = hapusKeluar;
</script>
</body>
</html>
