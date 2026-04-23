<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Input Barang Masuk + Pencarian & Ekspor | Supplier</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: system-ui, 'Segoe UI', 'Inter', 'Poppins', sans-serif;
        }

        body {
            background: linear-gradient(145deg, #e0eafc 0%, #cfdef3 100%);
            min-height: 100vh;
            padding: 2rem 1.5rem;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* Card utama */
        .card {
            max-width: 1400px;
            width: 100%;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.96);
            backdrop-filter: blur(0px);
            border-radius: 36px;
            box-shadow: 0 25px 45px -12px rgba(0, 0, 0, 0.35), 0 4px 12px rgba(0, 0, 0, 0.05);
            overflow: hidden;
            transition: transform 0.2s ease;
        }

        .card:hover {
            transform: translateY(-3px);
        }

        /* Header */
        .header {
            background: #1e2f5e;
            padding: 1.6rem 2rem;
            color: white;
        }

        .header h1 {
            font-size: 1.85rem;
            font-weight: 600;
            letter-spacing: -0.3px;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .header h1::before {
            content: "🗂️";
            font-size: 1.9rem;
        }

        .header p {
            font-size: 0.9rem;
            opacity: 0.85;
            margin-top: 6px;
            font-weight: 400;
        }

        /* form area */
        .form-container {
            padding: 2rem 2rem 1rem;
        }

        .form-group {
            margin-bottom: 1.4rem;
            display: flex;
            flex-direction: column;
        }

        .form-row {
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
            margin-bottom: 1.4rem;
        }

        .form-row .form-group {
            flex: 1;
            margin-bottom: 0;
        }

        label {
            font-weight: 600;
            color: #1f2b48;
            margin-bottom: 6px;
            font-size: 0.85rem;
            letter-spacing: 0.3px;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        input, select, textarea {
            width: 100%;
            padding: 12px 14px;
            font-size: 0.95rem;
            border: 1.5px solid #e2e8f0;
            border-radius: 20px;
            background-color: #ffffff;
            transition: all 0.2s;
            outline: none;
            font-weight: 500;
            color: #0a1c2f;
        }

        input:focus, select:focus, textarea:focus {
            border-color: #1e2f5e;
            box-shadow: 0 0 0 3px rgba(30, 47, 94, 0.2);
        }

        textarea {
            resize: vertical;
            min-height: 80px;
        }

        /* tombol aksi */
        .button-group {
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
            margin-top: 1rem;
            margin-bottom: 0.5rem;
        }

        .btn {
            flex: 1;
            padding: 12px 16px;
            font-weight: 600;
            font-size: 0.95rem;
            border: none;
            border-radius: 40px;
            cursor: pointer;
            transition: all 0.2s;
            background: #f1f5f9;
            color: #1e2f5e;
            border: 1px solid #cbd5e1;
        }

        .btn-primary {
            background: #1e2f5e;
            color: white;
            border: none;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
        }

        .btn-primary:hover {
            background: #0f2147;
            transform: translateY(-2px);
            box-shadow: 0 8px 18px rgba(30, 47, 94, 0.3);
        }

        .btn-secondary {
            background: white;
            border: 1px solid #cbd5e1;
        }

        .btn-secondary:hover {
            background: #f8fafc;
            border-color: #94a3b8;
        }

        /* daftar barang masuk + search */
        .record-section {
            background: #f8fafd;
            border-top: 2px dashed #cbd5e6;
            padding: 1.5rem 2rem 2rem;
        }

        .toolbar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 16px;
            margin-bottom: 1.5rem;
        }

        .record-header {
            display: flex;
            align-items: baseline;
            flex-wrap: wrap;
            gap: 10px;
        }

        .record-header h3 {
            font-weight: 700;
            font-size: 1.3rem;
            color: #0f2b3d;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .badge-count {
            background: #e9eef3;
            padding: 4px 12px;
            border-radius: 60px;
            font-size: 0.8rem;
            font-weight: 600;
            color: #1e2f5e;
        }

        /* Search Panel */
        .search-panel {
            background: white;
            padding: 1rem 1.2rem;
            border-radius: 60px;
            margin-bottom: 1.5rem;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 12px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.05);
            border: 1px solid #e2edf7;
        }
        .search-group {
            flex: 1;
            min-width: 180px;
        }
        .search-group label {
            font-size: 0.7rem;
            margin-bottom: 2px;
            color: #4a5b7a;
        }
        .search-group input, .search-group select {
            padding: 8px 12px;
            font-size: 0.85rem;
            border-radius: 40px;
            background: #f9fcff;
        }
        .clear-search {
            background: none;
            border: 1px solid #cbd5e1;
            padding: 8px 18px;
            border-radius: 40px;
            cursor: pointer;
            font-weight: 500;
            transition: 0.2s;
            margin-top: 18px;
        }
        .clear-search:hover {
            background: #eef2ff;
        }
        .result-info {
            font-size: 0.75rem;
            background: #eef2ff;
            display: inline-block;
            padding: 4px 12px;
            border-radius: 30px;
            margin-left: 8px;
        }

        .action-buttons {
            display: flex;
            gap: 12px;
        }

        .small-btn {
            padding: 8px 20px;
            font-size: 0.85rem;
            background: white;
            border: 1px solid #cbd5e1;
            border-radius: 40px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.2s;
            display: inline-flex;
            align-items: center;
            gap: 6px;
        }

        .small-btn:hover {
            background: #f1f5f9;
            transform: translateY(-1px);
        }

        .table-wrapper {
            overflow-x: auto;
            border-radius: 24px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            background: white;
            border-radius: 24px;
            overflow: hidden;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
        }

        th {
            background-color: #eef2f9;
            padding: 12px 12px;
            text-align: left;
            font-size: 0.75rem;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            color: #2c3e66;
        }

        td {
            padding: 12px 12px;
            border-bottom: 1px solid #eef2f9;
            font-size: 0.85rem;
            color: #1f2a40;
            vertical-align: top;
        }

        .delete-btn {
            background: none;
            border: none;
            font-size: 1.2rem;
            cursor: pointer;
            color: #b91c1c;
            transition: 0.1s;
            padding: 4px 8px;
            border-radius: 30px;
        }

        .delete-btn:hover {
            background: #fee2e2;
            transform: scale(1.05);
        }

        .empty-row td {
            text-align: center;
            padding: 2rem;
            color: #6c757d;
            font-style: italic;
        }

        footer {
            font-size: 0.7rem;
            text-align: center;
            padding: 1rem 2rem 1.5rem;
            color: #6c7a91;
            background: white;
            border-top: 1px solid #edf2f7;
        }

        @media print {
            body { background: white; padding: 0; }
            .card { box-shadow: none; border-radius: 0; }
            .form-container, .button-group, .toolbar .action-buttons, .search-panel, .delete-btn, footer, .btn, #resetFormBtn, .header p {
                display: none !important;
            }
            .record-section { border-top: none; padding: 0; }
            table { box-shadow: none; }
            th, td { border: 1px solid #ccc; }
        }

        @media (max-width: 780px) {
            body { padding: 1rem; }
            .search-panel { flex-direction: column; align-items: stretch; border-radius: 24px; }
            .clear-search { margin-top: 0; align-self: flex-start; }
            .toolbar { flex-direction: column; align-items: stretch; }
        }
    </style>
</head>
<body>

<div class="card">
    <div class="header">
        <h1>Input Barang Masuk</h1>
        <p>From Supplier · Pencarian Nama & Kategori · Cetak & Ekspor</p>
    </div>

    <div class="form-container">
        <form id="barangForm">
            <div class="form-row">
                <div class="form-group">
                    <label>📝 Nama Barang</label>
                    <input type="text" id="namaBarang" placeholder="Contoh: Brand" required>
                </div>
                <div class="form-group">
                    <label>🏷️ Kategori</label>
                    <select id="kategori" required>
                        <option value="" disabled selected>-- Pilih kategori --</option>
                        <option value="Booklet 1 1/4">Booklet 1 1/4</option>
                        <option value="Booklet 1 1/4 tips">Booklet 1 1/4 tips</option>
                        <option value="Booklet Kss">Booklet Kss</option>
                        <option value="Booklet Kss tips">Booklet Kss tips</option>
                        <option value="Display box">Display box</option>
                        <option value="Filter tips 21">Filter tips 21</option>
                        <option value="Filter tips 26">Filter tips 26</option>
                        <option value="Filter tips 30">Filter tips 30</option>
                        <option value="Trapezoid">Trapezoid</option>
                        <option value="Sticker">Sticker</option>
                        <option value="Lainnya">Lainnya</option>
                    </select>
                </div>
            </div>

            <div class="form-row">
                <div class="form-group">
                    <label>🔢 Jumlah</label>
                    <input type="number" id="jumlah" placeholder="0" min="1" required>
                </div>
                <div class="form-group">
                    <label>🔖 Satuan / Unit</label>
                    <input type="text" id="unit" placeholder="pcs, box, kg, sheet, roll" required>
                </div>
            </div>

            <div class="form-row">
                <div class="form-group">
                    <label>🗓️ Tanggal Masuk</label>
                    <input type="date" id="tanggalMasuk" required>
                </div>
                <div class="form-group">
                    <label>✏️ Catatan Supplier</label>
                    <input type="text" id="catatanSupplier" placeholder="Contoh: PO-123 / invoice #INV-001">
                </div>
            </div>

            <div class="button-group">
                <button type="submit" class="btn btn-primary">Tambah</button>
                <button type="button" id="resetFormBtn" class="btn btn-secondary">Reset Form</button>
            </div>
        </form>
    </div>

    <div class="record-section">
        <div class="toolbar">
            <div class="record-header">
                <h3>📋 Log Penerimaan Barang</h3>
                <span class="badge-count" id="totalItemCount">0 item</span>
            </div>
            <div class="action-buttons">
                <button id="printTableBtn" class="small-btn">🖨️ Print</button>
                <button id="downloadCsvBtn" class="small-btn">📎 Download CSV</button>
            </div>
        </div>

        <!-- PANEL PENCARIAN: Nama Barang & Kategori -->
        <div class="search-panel">
            <div class="search-group" style="flex:2;">
                <label>🔍 Cari Nama Barang</label>
                <input type="text" id="searchNama" placeholder="Ketik nama barang / Kategori..." autocomplete="off">
            </div>
            <div class="search-group">
                <label>📂 Filter Kategori</label>
                <select id="searchKategori">
                    <option value="" disabled selected>-- Pilih kategori --</option>
                        <option value="Booklet 1 1/4">Booklet 1 1/4</option>
                        <option value="Booklet 1 1/4 tips">Booklet 1 1/4 tips</option>
                        <option value="Booklet Kss">Booklet Kss</option>
                        <option value="Booklet Kss tips">Booklet Kss tips</option>
                        <option value="Display box">Display box</option>
                        <option value="Filter tips 21">Filter tips 21</option>
                        <option value="Filter tips 26">Filter tips 26</option>
                        <option value="Filter tips 30">Filter tips 30</option>
                        <option value="Trapezoid">Trapezoid</option>
                        <option value="Sticker">Sticker</option>
                        <option value="Lainnya">Lainnya</option>
                </select>
            </div>
            <button id="clearSearchBtn" class="clear-search">✖️ Reset Filter</button>
        </div>
        <div id="searchResultInfo" class="result-info" style="margin-bottom: 12px; display: inline-block;"></div>

        <div class="table-wrapper">
            <table id="dataTable">
                <thead>
                    <tr>
                        <th>Nama Barang</th>
                        <th>Kategori</th>
                        <th>Jumlah</th>
                        <th>Unit</th>
                        <th>Tgl Masuk</th>
                        <th>Catatan Supplier</th>
                        <th>Aksi</th>
                    </tr>
                </thead>
                <tbody id="tableBody">
                    <tr class="empty-row">
                        <td colspan="7">Belum ada data. Silakan tambah barang masuk dari supplier.</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
    <footer>
        ⚡ Data tersimpan di LocalStorage · Pencarian realtime (nama & kategori)
    </footer>
</div>

<script>
    // === Data Storage ===
    let incomingItems = [];           // semua data asli
    let filteredItems = [];           // hasil filter untuk ditampilkan

    // DOM elements
    const form = document.getElementById('barangForm');
    const namaBarangInput = document.getElementById('namaBarang');
    const kategoriSelect = document.getElementById('kategori');
    const jumlahInput = document.getElementById('jumlah');
    const unitInput = document.getElementById('unit');
    const tanggalMasukInput = document.getElementById('tanggalMasuk');
    const catatanSupplierInput = document.getElementById('catatanSupplier');
    const resetFormBtn = document.getElementById('resetFormBtn');
    const tableBody = document.getElementById('tableBody');
    const totalItemCountSpan = document.getElementById('totalItemCount');
    const printBtn = document.getElementById('printTableBtn');
    const downloadCsvBtn = document.getElementById('downloadCsvBtn');

    // Elemen pencarian
    const searchNamaInput = document.getElementById('searchNama');
    const searchKategoriSelect = document.getElementById('searchKategori');
    const clearSearchBtn = document.getElementById('clearSearchBtn');
    const searchResultInfo = document.getElementById('searchResultInfo');

    // Helper: set default tanggal hari ini
    function setDefaultDate() {
        if (!tanggalMasukInput.value) {
            const today = new Date();
            const yyyy = today.getFullYear();
            const mm = String(today.getMonth() + 1).padStart(2, '0');
            const dd = String(today.getDate()).padStart(2, '0');
            tanggalMasukInput.value = `${yyyy}-${mm}-${dd}`;
        }
    }

    // Load from localStorage
    function loadFromStorage() {
        const stored = localStorage.getItem('incoming_supplier_items');
        if (stored) {
            try {
                incomingItems = JSON.parse(stored);
                if (!Array.isArray(incomingItems)) incomingItems = [];
            } catch(e) { incomingItems = []; }
        } else {
            incomingItems = [];
        }
        applyFilters();  // trigger filter & render
        updateTotalCount();
    }

    // Save to localStorage
    function saveToStorage() {
        localStorage.setItem('incoming_supplier_items', JSON.stringify(incomingItems));
    }

    // Escape HTML
    function escapeHtml(str) {
        if (!str) return '';
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        });
    }

    // Filter logic: berdasarkan Nama Barang (case-insensitive) dan Kategori
    function applyFilters() {
        const searchNama = searchNamaInput.value.trim().toLowerCase();
        const searchKategori = searchKategoriSelect.value;

        filteredItems = incomingItems.filter(item => {
            let matchNama = true;
            let matchKategori = true;
            if (searchNama !== "") {
                matchNama = (item.namaBarang || "").toLowerCase().includes(searchNama);
            }
            if (searchKategori !== "") {
                matchKategori = (item.kategori || "") === searchKategori;
            }
            return matchNama && matchKategori;
        });

        // update info jumlah hasil pencarian
        const totalFiltered = filteredItems.length;
        const totalAll = incomingItems.length;
        if (searchNama !== "" || searchKategori !== "") {
            searchResultInfo.textContent = `🔎 Menampilkan ${totalFiltered} dari ${totalAll} item`;
        } else {
            searchResultInfo.textContent = `📋 Total semua: ${totalAll} item`;
        }

        renderTable(filteredItems);
        updateTotalCountDisplay(totalAll, totalFiltered);
    }

    // Render tabel berdasarkan data array (bisa filteredItems atau full)
    function renderTable(itemsToRender) {
        if (!tableBody) return;
        if (!itemsToRender || itemsToRender.length === 0) {
            tableBody.innerHTML = `<tr class="empty-row"><td colspan="7">📭 Tidak ada data sesuai pencarian atau belum ada barang masuk.</td></tr>`;
            return;
        }

        let htmlRows = '';
        itemsToRender.forEach((item, idx) => {
            // kita butuh index asli untuk hapus? hapus harus berdasarkan incomingItems asli.
            // cari index asli dari incomingItems menggunakan referensi id atau perbandingan.
            // Lebih aman: menggunakan properti unik 'id' untuk hapus, karena bisa saja filter mengubah urutan.
            // Gunakan item.id untuk menemukan index di incomingItems.
            const originalIndex = incomingItems.findIndex(i => i.id === item.id);
            const deleteDataIndex = originalIndex !== -1 ? originalIndex : idx;
            htmlRows += `
                <tr>
                    <td><strong>${escapeHtml(item.namaBarang)}</strong></td>
                    <td>${escapeHtml(item.kategori)}</td>
                    <td>${item.jumlah}</td>
                    <td>${escapeHtml(item.unit)}</td>
                    <td>${escapeHtml(item.tanggalMasuk)}</td>
                    <td>${escapeHtml(item.catatanSupplier || '-')}</td>
                    <td style="text-align: center;"><button class="delete-btn" data-id="${item.id}" title="Hapus barang">🗑️</button></td>
                </td>
            `;
        });
        tableBody.innerHTML = htmlRows;

        // Attach delete events menggunakan data-id
        const deleteButtons = document.querySelectorAll('.delete-btn');
        deleteButtons.forEach(btn => {
            btn.addEventListener('click', (e) => {
                const id = btn.getAttribute('data-id');
                if (id) deleteItemById(id);
                e.stopPropagation();
            });
        });
    }

    function deleteItemById(id) {
        const index = incomingItems.findIndex(item => item.id == id);
        if (index !== -1) {
            incomingItems.splice(index, 1);
            saveToStorage();
            applyFilters();   // refresh filter & tampilan
            showTemporaryMessage("✅ Item berhasil dihapus");
        }
    }

    function addNewItem(event) {
        event.preventDefault();

        const namaBarang = namaBarangInput.value.trim();
        const kategori = kategoriSelect.value;
        const jumlahRaw = jumlahInput.value.trim();
        const unit = unitInput.value.trim();
        const tanggalMasuk = tanggalMasukInput.value;
        const catatanSupplier = catatanSupplierInput.value.trim();

        if (!namaBarang) { alert("❌ Nama barang harus diisi!"); namaBarangInput.focus(); return; }
        if (!kategori) { alert("❌ Silakan pilih kategori!"); kategoriSelect.focus(); return; }
        if (!jumlahRaw || parseInt(jumlahRaw) <= 0) { alert("❌ Jumlah harus diisi angka positif!"); jumlahInput.focus(); return; }
        if (!unit) { alert("❌ Unit/satuan wajib diisi!"); unitInput.focus(); return; }
        if (!tanggalMasuk) { alert("❌ Tanggal masuk harus dipilih!"); tanggalMasukInput.focus(); return; }

        const jumlah = parseInt(jumlahRaw, 10);
        const newItem = {
            id: Date.now() + Math.random() * 10000,   // unique id
            namaBarang: namaBarang,
            kategori: kategori,
            jumlah: jumlah,
            unit: unit,
            tanggalMasuk: tanggalMasuk,
            catatanSupplier: catatanSupplier || ""
        };

        incomingItems.push(newItem);
        saveToStorage();
        resetFormFields();
        setDefaultDate();
        namaBarangInput.focus();
        // reset filter agar barang baru langsung terlihat (opsional: bersihkan pencarian)
        // biar user senang, kita reset filter biar langsung kelihatan: 
        searchNamaInput.value = '';
        searchKategoriSelect.value = '';
        applyFilters();
        showTemporaryMessage("✨ Barang berhasil ditambahkan!");
    }

    function resetFormFields() {
        namaBarangInput.value = '';
        kategoriSelect.value = '';
        jumlahInput.value = '';
        unitInput.value = '';
        catatanSupplierInput.value = '';
        tanggalMasukInput.value = '';
        setDefaultDate();
    }

    function updateTotalCountDisplay(totalAll, filteredTotal) {
        totalItemCountSpan.innerText = `${totalAll} ${totalAll === 1 ? 'item' : 'item'}`;
    }

    function updateTotalCount() {
        updateTotalCountDisplay(incomingItems.length, filteredItems.length);
    }

    let messageTimeout = null;
    function showTemporaryMessage(msg) {
        let toastEl = document.getElementById('dynamicToast');
        if (!toastEl) {
            toastEl = document.createElement('div');
            toastEl.id = 'dynamicToast';
            toastEl.style.position = 'fixed';
            toastEl.style.bottom = '25px';
            toastEl.style.left = '50%';
            toastEl.style.transform = 'translateX(-50%)';
            toastEl.style.backgroundColor = '#1e2f5e';
            toastEl.style.color = 'white';
            toastEl.style.padding = '10px 20px';
            toastEl.style.borderRadius = '60px';
            toastEl.style.fontSize = '0.85rem';
            toastEl.style.fontWeight = '500';
            toastEl.style.zIndex = '999';
            toastEl.style.boxShadow = '0 8px 20px rgba(0,0,0,0.2)';
            toastEl.style.background = '#0f2a3ecc';
            document.body.appendChild(toastEl);
        }
        toastEl.textContent = msg;
        toastEl.style.opacity = '1';
        if (messageTimeout) clearTimeout(messageTimeout);
        messageTimeout = setTimeout(() => {
            if (toastEl) toastEl.style.opacity = '0';
        }, 2000);
    }

    // ==================== PRINT FUNCTION ====================
    function printTable() {
        // cetak berdasarkan data yang sedang tampil (filteredItems) lebih relevan
        const dataToPrint = filteredItems.length > 0 ? filteredItems : incomingItems;
        if (dataToPrint.length === 0) {
            alert("Tidak ada data untuk dicetak.");
            return;
        }
        const printWindow = window.open('', '_blank');
        if (!printWindow) {
            alert("Harap izinkan pop-up untuk mencetak.");
            return;
        }
        const todayDate = new Date().toLocaleDateString('id-ID');
        let rowsHtml = '';
        dataToPrint.forEach(item => {
            rowsHtml += `
                <tr>
                    <td style="border:1px solid #ccc; padding:8px;">${escapeHtml(item.namaBarang)}</td>
                    <td style="border:1px solid #ccc; padding:8px;">${escapeHtml(item.kategori)}</td>
                    <td style="border:1px solid #ccc; padding:8px; text-align:right;">${item.jumlah}</td>
                    <td style="border:1px solid #ccc; padding:8px;">${escapeHtml(item.unit)}</td>
                    <td style="border:1px solid #ccc; padding:8px;">${escapeHtml(item.tanggalMasuk)}</td>
                    <td style="border:1px solid #ccc; padding:8px;">${escapeHtml(item.catatanSupplier || '-')}</td>
                </tr>
            `;
        });
        const printHtml = `
            <!DOCTYPE html>
            <html>
            <head><meta charset="UTF-8"><title>Laporan Barang Masuk</title>
            <style>
                body { font-family: 'Segoe UI', Arial; margin: 30px; }
                h1 { color: #1e2f5e; }
                .sub { margin-bottom: 1.5rem; border-bottom: 2px solid #aaa; padding-bottom: 8px; }
                table { width: 100%; border-collapse: collapse; }
                th { background: #eef2f9; padding: 10px; border: 1px solid #aaa; text-align:left; }
                td { border: 1px solid #ccc; padding: 8px; }
                .footer { margin-top: 25px; text-align: center; font-size: 0.75rem; }
            </style>
            </head>
            <body>
                <h1>🗂️ Laporan Penerimaan Barang</h1>
                <div class="sub">Supplier · Dicetak: ${todayDate} | Total tampil: ${dataToPrint.length} item</div>
                <table><thead><tr><th>Nama Barang</th><th>Kategori</th><th>Jumlah</th><th>Unit</th><th>Tgl Masuk</th><th>Catatan Supplier</th></tr></thead>
                <tbody>${rowsHtml}</tbody></table>
                <div class="footer">Sistem Manajemen Inventory</div>
            </body>
            </html>
        `;
        printWindow.document.write(printHtml);
        printWindow.document.close();
        printWindow.focus();
        printWindow.print();
    }

    // ==================== DOWNLOAD CSV ====================
    function downloadCSV() {
        const dataToExport = filteredItems.length > 0 ? filteredItems : incomingItems;
        if (dataToExport.length === 0) {
            alert("Tidak ada data untuk di-download.");
            return;
        }
        const headers = ["Nama Barang", "Kategori", "Jumlah", "Unit", "Tanggal Masuk", "Catatan Supplier"];
        const rows = dataToExport.map(item => [
            `"${(item.namaBarang || '').replace(/"/g, '""')}"`,
            `"${(item.kategori || '').replace(/"/g, '""')}"`,
            item.jumlah,
            `"${(item.unit || '').replace(/"/g, '""')}"`,
            `"${(item.tanggalMasuk || '').replace(/"/g, '""')}"`,
            `"${(item.catatanSupplier || '').replace(/"/g, '""')}"`
        ]);
        const csvContent = [headers.join(','), ...rows.map(row => row.join(','))].join('\n');
        const blob = new Blob(["\uFEFF" + csvContent], { type: "text/csv;charset=utf-8;" });
        const link = document.createElement("a");
        const url = URL.createObjectURL(blob);
        link.setAttribute("href", url);
        const now = new Date();
        const timestamp = `${now.getFullYear()}-${now.getMonth()+1}-${now.getDate()}_${now.getHours()}-${now.getMinutes()}`;
        link.setAttribute("download", `barang_masuk_${timestamp}.csv`);
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
        URL.revokeObjectURL(url);
        showTemporaryMessage("📁 File CSV berhasil diunduh!");
    }

    // reset filter
    function resetFilters() {
        searchNamaInput.value = '';
        searchKategoriSelect.value = '';
        applyFilters();
        showTemporaryMessage("Filter direset, semua data tampil");
    }

    // Event listeners
    form.addEventListener('submit', addNewItem);
    resetFormBtn.addEventListener('click', () => { resetFormFields(); showTemporaryMessage("🧹 Form dibersihkan"); });
    printBtn.addEventListener('click', printTable);
    downloadCsvBtn.addEventListener('click', downloadCSV);
    searchNamaInput.addEventListener('input', applyFilters);
    searchKategoriSelect.addEventListener('change', applyFilters);
    clearSearchBtn.addEventListener('click', resetFilters);

    function init() {
        loadFromStorage();
        setDefaultDate();
    }
    init();
</script>
</body>
</html>
