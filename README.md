<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>Inventory Management - Brand & Orientasi Responsif</title>
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
            min-height: 100vh;
        }

        /* Container responsif utama */
        .container {
            max-width: 1600px;
            margin: 0 auto;
            transition: all 0.2s;
        }

        /* ORIENTASI DETECTION - STYLE DINAMIS */
        /* Mode Portrait (vertical) - default tampilan mobile/tablet portrait */
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
        body.portrait .filter-group {
            width: 100%;
        }
        body.portrait .rekap-header {
            flex-direction: column;
            align-items: flex-start;
        }
        body.portrait .table-wrapper {
            font-size: 0.75rem;
        }
        body.portrait th, body.portrait td {
            padding: 8px 4px;
        }
        
        /* Mode Landscape (horizontal) - tampilan lebar */
        body.landscape .two-panels {
            flex-direction: row;
            flex-wrap: wrap;
        }
        body.landscape .panel {
            flex: 1;
            min-width: 420px;
        }
        body.landscape .filter-bar {
            flex-direction: row;
            flex-wrap: wrap;
        }
        
        /* Indikator orientasi kecil (opsional) */
        .orientation-badge {
            position: fixed;
            bottom: 16px;
            right: 16px;
            background: rgba(0,0,0,0.6);
            backdrop-filter: blur(4px);
            color: white;
            padding: 6px 12px;
            border-radius: 40px;
            font-size: 0.7rem;
            font-weight: 500;
            z-index: 999;
            pointer-events: none;
            font-family: monospace;
            letter-spacing: 0.5px;
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
            font-size: 0.9rem;
        }

        .two-panels {
            display: flex;
            gap: 28px;
            margin-bottom: 40px;
            transition: all 0.2s;
        }

        .panel {
            background: white;
            border-radius: 28px;
            box-shadow: 0 12px 30px rgba(0,0,0,0.05);
            overflow: hidden;
            border: 1px solid #e2e8f0;
            transition: all 0.2s;
        }

        .panel-header {
            background: #ffffff;
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
            font-size: 0.85rem;
            cursor: pointer;
            transition: 0.2s;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        .btn-add:hover {
            background: #2563eb;
            transform: scale(1.02);
        }

        .table-wrapper {
            overflow-x: auto;
            padding: 0 16px 20px 16px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.85rem;
            min-width: 580px;
        }

        th, td {
            padding: 12px 8px;
            text-align: left;
            border-bottom: 1px solid #e9eef3;
        }

        th {
            background-color: #f8fafc;
            color: #1e293b;
            font-weight: 600;
        }

        .action-icons i {
            margin: 0 6px;
            cursor: pointer;
            font-size: 1rem;
            transition: 0.1s;
        }

        .fa-edit { color: #3b82f6; }
        .fa-trash-alt { color: #ef4444; }
        .fa-edit:hover { color: #1e40af; }
        .fa-trash-alt:hover { color: #b91c1c; }

        .filter-bar {
            background: white;
            border-radius: 24px;
            padding: 16px 24px;
            margin-bottom: 28px;
            display: flex;
            flex-wrap: wrap;
            align-items: flex-end;
            gap: 20px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.03);
            border: 1px solid #e2e8f0;
            transition: all 0.2s;
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
            font-size: 0.85rem;
        }

        .btn-reset {
            background: #f1f5f9;
            border: 1px solid #cbd5e1;
            padding: 8px 20px;
            border-radius: 30px;
            cursor: pointer;
            font-weight: 500;
            transition: 0.2s;
        }

        .btn-reset-danger {
            background: #fee2e2;
            border-color: #fecaca;
            color: #b91c1c;
        }

        .btn-reset:hover {
            background: #e2e8f0;
        }

        .rekap-section {
            background: white;
            border-radius: 28px;
            padding: 20px 24px;
            margin-top: 20px;
            border: 1px solid #e2e8f0;
            box-shadow: 0 6px 14px rgba(0,0,0,0.03);
        }

        .rekap-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            margin-bottom: 20px;
            gap: 15px;
        }

        .rekap-section h3 {
            font-size: 1.3rem;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 12px;
            margin: 0;
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
            font-size: 0.8rem;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            transition: 0.2s;
        }

        .btn-excel {
            background: #e8f5e9;
            color: #2e7d32;
            border-color: #a5d6a7;
        }

        .btn-print {
            background: #e3f2fd;
            color: #1565c0;
            border-color: #90caf9;
        }

        .btn-excel:hover, .btn-print:hover {
            transform: translateY(-1px);
            filter: brightness(0.97);
        }

        .badge {
            background: #eef2ff;
            padding: 4px 10px;
            border-radius: 40px;
            font-size: 0.7rem;
        }

        @media (max-width: 640px) {
            body {
                padding: 16px 12px;
            }
            .panel-header h2 {
                font-size: 1.1rem;
            }
            .btn-add {
                padding: 6px 12px;
                font-size: 0.75rem;
            }
            .rekap-section h3 {
                font-size: 1.1rem;
            }
        }

        /* Print tetap rapi */
        @media print {
            body {
                background: white;
                padding: 0;
                margin: 0;
            }
            .filter-bar, .two-panels, .btn-add, .btn-reset, .btn-excel, .btn-print, 
            .rekap-actions, .action-icons, .panel-header button, #resetPermanenBtn, 
            .btn-reset-danger, .filter-group, .orientation-badge {
                display: none !important;
            }
            .rekap-section {
                box-shadow: none;
                padding: 0;
                margin: 0;
                border: none;
            }
            .rekap-grid table {
                width: 100%;
                border: 1px solid #ddd;
            }
            th, td {
                border: 1px solid #ccc;
            }
        }
    </style>
</head>
<body>
<div class="container">
    <div class="dashboard-header">
        <h1><i class="fas fa-boxes"></i> Manajemen Inventory + Brand</h1>
        <div class="sub">Data Barang Masuk Supplier & Stok Consumable | Orientasi Otomatis | Cetak & Export</div>
    </div>

    <!-- Filter & Dropdowns -->
    <div class="filter-bar">
        <div class="filter-group">
            <label><i class="fas fa-filter"></i> Filter Nama / Brand</label>
            <input type="text" id="globalSearchBarang" placeholder="Cari brand atau nama barang..." style="width:220px;">
        </div>
        <div class="filter-group">
            <label>Dropdown Brand + Barang</label>
            <select id="dropdownNamaBarang"></select>
        </div>
        <div class="filter-group">
            <label>Dropdown Supplier</label>
            <select id="dropdownSupplier"></select>
        </div>
        <div class="filter-group">
            <label>Dropdown Satuan</label>
            <select id="dropdownSatuan"></select>
        </div>
        <div class="filter-group">
            <label>&nbsp;</label>
            <button id="resetPermanenBtn" class="btn-reset btn-reset-danger"><i class="fas fa-database"></i> Reset Semua</button>
        </div>
    </div>

    <!-- Dua Panel Utama -->
    <div class="two-panels">
        <!-- PANEL 1: Data Barang Masuk Supplier -->
        <div class="panel">
            <div class="panel-header">
                <h2><i class="fas fa-truck"></i> Data Barang Masuk Supplier</h2>
                <button class="btn-add" id="btnAddMasuk"><i class="fas fa-plus"></i> Tambah Masuk</button>
            </div>
            <div class="table-wrapper">
                <table id="tableMasuk">
                    <thead>
                        <tr><th>Tanggal</th><th>Brand</th><th>Nama Barang</th><th>Supplier</th><th>Jumlah</th><th>Satuan</th><th>Aksi</th></tr>
                    </thead>
                    <tbody id="tbodyMasuk"></tbody>
                </table>
            </div>
        </div>

        <!-- PANEL 2: Data Stock Barang Consumable -->
        <div class="panel">
            <div class="panel-header">
                <h2><i class="fas fa-clipboard-list"></i> Stock Barang Consumable</h2>
                <button class="btn-add" id="btnAddKeluar"><i class="fas fa-plus"></i> Tambah Keluar</button>
            </div>
            <div class="table-wrapper">
                <table id="tableKeluar">
                    <thead><tr><th>Total Stok Masuk</th><th>Tanggal Keluar</th><th>Brand</th><th>Nama Barang</th><th>Jumlah Keluar</th><th>Satuan</th><th>PIC</th><th>Aksi</th></tr></thead>
                    <tbody id="tbodyKeluar"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- REKAP STOK AKHIR PER BULAN + TOMBOL PRINT & EXCEL -->
    <div class="rekap-section" id="rekapSection">
        <div class="rekap-header">
            <h3><i class="fas fa-chart-line"></i> Rekap Stok Akhir (Brand + Barang)</h3>
            <div class="rekap-actions">
                <button class="btn-excel" id="exportExcelBtn"><i class="fas fa-file-excel"></i> Export Excel</button>
                <button class="btn-print" id="printRekapBtn"><i class="fas fa-print"></i> Cetak Rekap</button>
            </div>
        </div>
        <div class="rekap-grid" id="rekapContainer"></div>
        <div class="small-note">* Stok Akhir = Total Masuk - Total Keluar. Identifikasi berdasarkan Brand + Nama Barang.</div>
    </div>
</div>
<div class="orientation-badge" id="orientationBadge">📱 Portrait</div>

<script>
    // ========== DETEKSI ORIENTASI PERANGKAT ==========
    function detectAndApplyOrientation() {
        const isPortrait = window.matchMedia("(orientation: portrait)").matches;
        const body = document.body;
        const badge = document.getElementById('orientationBadge');
        if (isPortrait) {
            body.classList.remove('landscape');
            body.classList.add('portrait');
            if(badge) badge.innerHTML = '📱 Portrait Mode';
        } else {
            body.classList.remove('portrait');
            body.classList.add('landscape');
            if(badge) badge.innerHTML = '🖥️ Landscape Mode';
        }
        // minor: pastikan tabel tetap nyaman
        const tables = document.querySelectorAll('.table-wrapper table');
        tables.forEach(table => {
            if (isPortrait && window.innerWidth < 700) {
                table.style.minWidth = '580px';
            } else {
                table.style.minWidth = '';
            }
        });
    }
    // Event listener untuk resize dan orientasi change
    window.addEventListener('resize', detectAndApplyOrientation);
    window.addEventListener('orientationchange', function() {
        setTimeout(detectAndApplyOrientation, 50);
    });
    
    // ========== DATA MODEL dengan BRAND ==========
    let dataMasuk = [];     // id, tanggal, brand, namaBarang, supplier, jumlah, satuan
    let dataKeluar = [];    // id, tanggalKeluar, brand, barangKeluar, jumlahKeluar, satuan, namaPic

    function getUniqueKey(brand, namaBarang) {
        return `${brand}|${namaBarang}`.toLowerCase();
    }

    function loadFromStorage() {
        const storedMasuk = localStorage.getItem('inv_masuk_brand');
        const storedKeluar = localStorage.getItem('inv_keluar_brand');
        if(storedMasuk) dataMasuk = JSON.parse(storedMasuk);
        else {
            dataMasuk = [
                { id: 1, tanggal: '2025-03-10', brand: 'Sidney', namaBarang: 'Kertas A4', supplier: 'PT Maju Jaya', jumlah: 500, satuan: 'Rim' },
                { id: 2, tanggal: '2025-03-15', brand: 'Epson', namaBarang: 'Tinta Hitam', supplier: 'Sumber Makmur', jumlah: 20, satuan: 'Botol' },
                { id: 3, tanggal: '2025-04-05', brand: 'Sidney', namaBarang: 'Kertas A4', supplier: 'PT Maju Jaya', jumlah: 300, satuan: 'Rim' },
                { id: 4, tanggal: '2025-04-12', brand: 'Snowman', namaBarang: 'Spidol Boardmarker', supplier: 'Alat Kantor', jumlah: 50, satuan: 'Pcs' },
                { id: 5, tanggal: '2025-04-15', brand: 'Joyko', namaBarang: 'Stabilo', supplier: 'Stationery Prima', jumlah: 100, satuan: 'Pcs' }
            ];
        }
        if(storedKeluar) dataKeluar = JSON.parse(storedKeluar);
        else {
            dataKeluar = [
                { id: 101, tanggalKeluar: '2025-03-20', brand: 'Sidney', barangKeluar: 'Kertas A4', jumlahKeluar: 200, satuan: 'Rim', namaPic: 'Andi' },
                { id: 102, tanggalKeluar: '2025-03-28', brand: 'Epson', barangKeluar: 'Tinta Hitam', jumlahKeluar: 5, satuan: 'Botol', namaPic: 'Budi' },
                { id: 103, tanggalKeluar: '2025-04-10', brand: 'Sidney', barangKeluar: 'Kertas A4', jumlahKeluar: 150, satuan: 'Rim', namaPic: 'Citra' },
                { id: 104, tanggalKeluar: '2025-04-18', brand: 'Snowman', barangKeluar: 'Spidol Boardmarker', jumlahKeluar: 10, satuan: 'Pcs', namaPic: 'Dewi' },
                { id: 105, tanggalKeluar: '2025-04-20', brand: 'Joyko', barangKeluar: 'Stabilo', jumlahKeluar: 20, satuan: 'Pcs', namaPic: 'Eko' }
            ];
        }
    }

    function saveToStorage() {
        localStorage.setItem('inv_masuk_brand', JSON.stringify(dataMasuk));
        localStorage.setItem('inv_keluar_brand', JSON.stringify(dataKeluar));
    }

    function getTotalMasukPerItem(brand, namaBarang) {
        return dataMasuk.filter(m => m.brand === brand && m.namaBarang === namaBarang).reduce((s, m) => s + m.jumlah, 0);
    }
    function getTotalKeluarPerItem(brand, namaBarang) {
        return dataKeluar.filter(k => k.brand === brand && k.barangKeluar === namaBarang).reduce((s, k) => s + k.jumlahKeluar, 0);
    }
    function getUniqueItems() {
        const map = new Map();
        dataMasuk.forEach(m => { const key = getUniqueKey(m.brand, m.namaBarang); if(!map.has(key)) map.set(key, { brand: m.brand, namaBarang: m.namaBarang }); });
        dataKeluar.forEach(k => { const key = getUniqueKey(k.brand, k.barangKeluar); if(!map.has(key)) map.set(key, { brand: k.brand, namaBarang: k.barangKeluar }); });
        return Array.from(map.values());
    }

    function buildRekapStok() {
        const items = getUniqueItems();
        let html = '<table style="width:100%; border-collapse:collapse;"><thead><tr style="background:#f1f5f9;"><th>Brand</th><th>Nama Barang</th><th>Total Masuk</th><th>Total Keluar</th><th>Stok Akhir</th><th>Rincian Pengambilan</th></tr></thead><tbody>';
        for(let item of items) {
            const totalMasuk = getTotalMasukPerItem(item.brand, item.namaBarang);
            const totalKeluar = getTotalKeluarPerItem(item.brand, item.namaBarang);
            const stokAkhir = totalMasuk - totalKeluar;
            const riwayat = dataKeluar.filter(k => k.brand === item.brand && k.barangKeluar === item.namaBarang);
            let rincian = riwayat.length ? riwayat.map(k => `${k.tanggalKeluar} : ${k.jumlahKeluar} ${k.satuan} (PIC: ${escapeHtml(k.namaPic)})`).join('<br/>') : '<span class="badge">Tidak ada</span>';
            html += `<tr><td style="font-weight:600;">${escapeHtml(item.brand)}</td><td>${escapeHtml(item.namaBarang)}</td><td>${totalMasuk}</td><td>${totalKeluar}</td><td style="background:#fef9e3; font-weight:bold;">${stokAkhir}</td><td style="font-size:0.75rem;">${rincian}</td></tr>`;
        }
        html += `</tbody></table>`;
        if(items.length===0) html = '<p class="small-note">Tidak ada data</p>';
        document.getElementById('rekapContainer').innerHTML = html;
    }

    function getRekapDataArray() {
        const items = getUniqueItems();
        return items.map(item => {
            const totalMasuk = getTotalMasukPerItem(item.brand, item.namaBarang);
            const totalKeluar = getTotalKeluarPerItem(item.brand, item.namaBarang);
            const stokAkhir = totalMasuk - totalKeluar;
            const riwayat = dataKeluar.filter(k => k.brand === item.brand && k.barangKeluar === item.namaBarang);
            const rincian = riwayat.map(k => `${k.tanggalKeluar} : ${k.jumlahKeluar} ${k.satuan} (PIC: ${k.namaPic})`).join("; ") || "Tidak ada";
            return { Brand: item.brand, "Nama Barang": item.namaBarang, "Total Masuk": totalMasuk, "Total Keluar": totalKeluar, "Stok Akhir": stokAkhir, "Rincian Pengambilan": rincian };
        });
    }

    function exportToExcel() {
        const data = getRekapDataArray();
        if(!data.length) { alert("Tidak ada data rekap."); return; }
        const ws = XLSX.utils.json_to_sheet(data);
        ws['!cols'] = [{wch:15},{wch:25},{wch:12},{wch:12},{wch:12},{wch:50}];
        const wb = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(wb, ws, "Rekap_Stok_Brand");
        XLSX.writeFile(wb, `Rekap_Stok_${new Date().toISOString().slice(0,19).replace(/:/g, '-')}.xlsx`);
    }

    function printRekapOnly() {
        const rekapContainer = document.getElementById('rekapContainer');
        const printWin = window.open('', '_blank');
        printWin.document.write(`
            <html><head><title>Laporan Rekap Stok Akhir</title>
            <style>body{font-family:Inter, Arial; padding:20px;} table{border-collapse:collapse; width:100%;} th,td{border:1px solid #aaa; padding:8px;} th{background:#f2f2f2;}</style>
            </head><body><h2>Rekap Stok Akhir (Brand + Barang)</h2><p>Tanggal cetak: ${new Date().toLocaleString()}</p>${rekapContainer.outerHTML}<div><small>*Stok Akhir = Total Masuk - Total Keluar</small></div></body></html>
        `);
        printWin.document.close();
        printWin.print();
        printWin.onafterprint = () => printWin.close();
    }

    // RENDER TABEL MASUK & KELUAR
    function renderTabelMasuk() {
        const search = document.getElementById('globalSearchBarang').value.toLowerCase();
        let filtered = dataMasuk.filter(m => m.brand.toLowerCase().includes(search) || m.namaBarang.toLowerCase().includes(search));
        const tbody = document.getElementById('tbodyMasuk');
        if(!filtered.length) { tbody.innerHTML = '<tr><td colspan="7" class="text-center">Tidak ada data</td></tr>'; return; }
        tbody.innerHTML = filtered.map(m => `
            <tr><td>${escapeHtml(m.tanggal)}</td><td><strong>${escapeHtml(m.brand)}</strong></td><td>${escapeHtml(m.namaBarang)}</td><td>${escapeHtml(m.supplier)}</td><td>${m.jumlah}</td><td>${escapeHtml(m.satuan)}</td>
            <td class="action-icons"><i class="fas fa-edit" onclick="editMasuk(${m.id})"></i><i class="fas fa-trash-alt" onclick="hapusMasuk(${m.id})"></i></td></tr>
        `).join('');
    }
    function renderTabelKeluar() {
        const search = document.getElementById('globalSearchBarang').value.toLowerCase();
        let filtered = dataKeluar.filter(k => k.brand.toLowerCase().includes(search) || k.barangKeluar.toLowerCase().includes(search));
        const tbody = document.getElementById('tbodyKeluar');
        if(!filtered.length) { tbody.innerHTML = '<tr><td colspan="8" class="text-center">Tidak ada data</td></tr>'; return; }
        tbody.innerHTML = filtered.map(k => {
            const stokMasuk = getTotalMasukPerItem(k.brand, k.barangKeluar);
            return `<tr><td>${stokMasuk}</td><td>${escapeHtml(k.tanggalKeluar)}</td><td><strong>${escapeHtml(k.brand)}</strong></td><td>${escapeHtml(k.barangKeluar)}</td><td>${k.jumlahKeluar}</td><td>${escapeHtml(k.satuan)}</td><td>${escapeHtml(k.namaPic)}</td>
            <td class="action-icons"><i class="fas fa-edit" onclick="editKeluar(${k.id})"></i><i class="fas fa-trash-alt" onclick="hapusKeluar(${k.id})"></i></td></tr>`;
        }).join('');
    }

    function updateDropdowns() {
        const items = getUniqueItems();
        const displayItems = items.map(i => `${i.brand} - ${i.namaBarang}`);
        const dropBarang = document.getElementById('dropdownNamaBarang');
        dropBarang.innerHTML = '<option value="">-- Semua --</option>' + displayItems.map(d => `<option value="${escapeHtml(d)}">${escapeHtml(d)}</option>`).join('');
        dropBarang.onchange = (e) => {
            const val = e.target.value;
            if(val) document.getElementById('globalSearchBarang').value = val.split(' - ')[0] + ' ' + (val.split(' - ').slice(1).join(' '));
            else document.getElementById('globalSearchBarang').value = '';
            refreshAll();
        };
        const uniqueSupplier = [...new Set(dataMasuk.map(m=>m.supplier))];
        document.getElementById('dropdownSupplier').innerHTML = '<option value="">-- Supplier --</option>' + uniqueSupplier.map(s=>`<option value="${escapeHtml(s)}">${escapeHtml(s)}</option>`).join('');
        const uniqueSatuan = [...new Set([...dataMasuk.map(m=>m.satuan), ...dataKeluar.map(k=>k.satuan)])];
        document.getElementById('dropdownSatuan').innerHTML = '<option value="">-- Satuan --</option>' + uniqueSatuan.map(s=>`<option value="${escapeHtml(s)}">${escapeHtml(s)}</option>`).join('');
    }

    // CRUD
    function tambahMasuk() {
        let newId = dataMasuk.length ? Math.max(...dataMasuk.map(m=>m.id)) + 1 : 1;
        let tanggal = prompt("Tanggal (YYYY-MM-DD):", new Date().toISOString().slice(0,10)); if(!tanggal) return;
        let brand = prompt("Brand / Merk:", "Contoh: Sidney"); if(!brand) return;
        let nama = prompt("Nama Barang:"); if(!nama) return;
        let supplier = prompt("Supplier:"); if(!supplier) return;
        let jml = parseInt(prompt("Jumlah:", "0")); if(isNaN(jml)) jml=0;
        let satuan = prompt("Satuan:", "Pcs");
        dataMasuk.push({ id: newId, tanggal, brand, namaBarang: nama, supplier, jumlah: jml, satuan });
        refreshAll();
    }
    function editMasuk(id) { let item = dataMasuk.find(m=>m.id===id); if(item){ let t=prompt("Tgl",item.tanggal); if(t) item.tanggal=t; let b=prompt("Brand",item.brand); if(b) item.brand=b; let n=prompt("Nama",item.namaBarang); if(n) item.namaBarang=n; let s=prompt("Supplier",item.supplier); if(s) item.supplier=s; let j=parseInt(prompt("Jumlah",item.jumlah)); if(!isNaN(j)) item.jumlah=j; let sat=prompt("Satuan",item.satuan); if(sat) item.satuan=sat; refreshAll(); } }
    function hapusMasuk(id) { if(confirm("Hapus?")) { dataMasuk = dataMasuk.filter(m=>m.id!==id); refreshAll(); } }
    function tambahKeluar() {
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
    function editKeluar(id) { let item = dataKeluar.find(k=>k.id===id); if(item){ let t=prompt("Tgl Keluar",item.tanggalKeluar); if(t) item.tanggalKeluar=t; let b=prompt("Brand",item.brand); if(b) item.brand=b; let brg=prompt("Barang",item.barangKeluar); if(brg) item.barangKeluar=brg; let j=parseInt(prompt("Jumlah",item.jumlahKeluar)); if(!isNaN(j)) item.jumlahKeluar=j; let sat=prompt("Satuan",item.satuan); if(sat) item.satuan=sat; let pic=prompt("PIC",item.namaPic); if(pic) item.namaPic=pic; refreshAll(); } }
    function hapusKeluar(id) { if(confirm("Hapus?")) { dataKeluar = dataKeluar.filter(k=>k.id!==id); refreshAll(); } }
    function resetDataPermanen() { if(confirm("Hapus SEMUA data?")){ dataMasuk=[]; dataKeluar=[]; refreshAll(); alert("Data direset permanen."); } }
    function refreshAll() { renderTabelMasuk(); renderTabelKeluar(); buildRekapStok(); updateDropdowns(); saveToStorage(); }
    function escapeHtml(str) { if(!str) return ''; return str.replace(/[&<>]/g, m=> m==='&'?'&amp;': m==='<'?'&lt;':'&gt;'); }

    window.onload = () => {
        loadFromStorage();
        refreshAll();
        detectAndApplyOrientation();
        document.getElementById('btnAddMasuk').onclick = tambahMasuk;
        document.getElementById('btnAddKeluar').onclick = tambahKeluar;
        document.getElementById('resetPermanenBtn').onclick = resetDataPermanen;
        document.getElementById('exportExcelBtn').onclick = exportToExcel;
        document.getElementById('printRekapBtn').onclick = printRekapOnly;
        document.getElementById('globalSearchBarang').addEventListener('input', () => refreshAll());
        const resetFilter = document.createElement('button');
        resetFilter.innerText = 'Reset Pencarian';
        resetFilter.className = 'btn-reset';
        resetFilter.style.marginLeft = 'auto';
        resetFilter.onclick = () => { document.getElementById('globalSearchBarang').value = ''; document.getElementById('dropdownNamaBarang').value = ''; refreshAll(); };
        document.querySelector('.filter-bar').appendChild(resetFilter);
    };
    window.editMasuk = editMasuk; window.hapusMasuk = hapusMasuk; window.editKeluar = editKeluar; window.hapusKeluar = hapusKeluar;
</script>
</body>
</html>
