<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>Inventory Barang Masuk | Supplier Management</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background: #eef2f7;
            background: linear-gradient(145deg, #e9eef4 0%, #dce5ef 100%);
            min-height: 100vh;
            padding: 12px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: system-ui, -apple-system, 'Segoe UI', 'Roboto', 'Helvetica Neue', sans-serif;
        }

        /* MAIN CARD */
        .app-container {
            max-width: 1280px;
            width: 100%;
            margin: 0 auto;
            background: #ffffff;
            border-radius: 36px;
            box-shadow: 0 20px 35px -12px rgba(0, 0, 0, 0.25);
            overflow: hidden;
            transition: all 0.2s;
        }

        /* HEADER */
        .app-header {
            background: linear-gradient(115deg, #0f2b3d 0%, #1e4068 100%);
            padding: 20px 22px;
            color: white;
        }
        .app-header h1 {
            font-size: 1.65rem;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 10px;
            flex-wrap: wrap;
        }
        .app-header h1:before {
            content: "📦";
            font-size: 1.8rem;
        }
        .app-header p {
            font-size: 0.8rem;
            opacity: 0.85;
            margin-top: 6px;
            letter-spacing: 0.3px;
        }

        /* FORM SECTION */
        .form-wrapper {
            padding: 22px 24px 16px;
            background: #ffffff;
        }
        .grid-2cols {
            display: flex;
            flex-wrap: wrap;
            gap: 16px;
            margin-bottom: 18px;
        }
        .input-field {
            flex: 1;
            min-width: 150px;
            display: flex;
            flex-direction: column;
        }
        .input-field label {
            font-size: 0.75rem;
            font-weight: 700;
            color: #1e2f5e;
            margin-bottom: 5px;
            letter-spacing: 0.3px;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        .input-field input, .input-field select {
            padding: 11px 14px;
            border: 1.5px solid #e2edf2;
            border-radius: 24px;
            font-size: 0.85rem;
            background: #ffffff;
            transition: 0.2s;
            outline: none;
            font-weight: 500;
        }
        .input-field input:focus, .input-field select:focus {
            border-color: #1e4068;
            box-shadow: 0 0 0 3px rgba(30, 64, 104, 0.15);
        }
        .btn-row {
            display: flex;
            gap: 14px;
            flex-wrap: wrap;
            margin: 16px 0 8px;
        }
        .btn {
            flex: 1;
            padding: 11px 18px;
            border-radius: 44px;
            font-weight: 700;
            font-size: 0.85rem;
            border: none;
            cursor: pointer;
            transition: all 0.2s ease;
            background: #f1f5f9;
            color: #1e2f5e;
            border: 1px solid #cbd5e6;
        }
        .btn-primary {
            background: #1e3a5f;
            color: white;
            border: none;
            box-shadow: 0 4px 8px rgba(0,0,0,0.05);
        }
        .btn-primary:active { transform: scale(0.97); background: #0f2b44; }
        .btn-secondary:active { transform: scale(0.97); }

        /* RECORD AREA */
        .record-panel {
            background: #fbfdff;
            border-top: 2px solid #eef3fc;
            padding: 20px 24px 24px;
        }
        .toolbar-flex {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 14px;
            margin-bottom: 18px;
        }
        .title-count {
            display: flex;
            align-items: baseline;
            gap: 12px;
            flex-wrap: wrap;
        }
        .title-count h3 {
            font-size: 1.25rem;
            color: #0a2b3e;
            font-weight: 700;
        }
        .badge-total {
            background: #e4eaf2;
            padding: 5px 14px;
            border-radius: 60px;
            font-size: 0.7rem;
            font-weight: 700;
            color: #1e2f5e;
        }
        .action-btns {
            display: flex;
            gap: 12px;
        }
        .icon-action {
            background: white;
            border: 1px solid #cbdcec;
            padding: 7px 18px;
            border-radius: 40px;
            font-size: 0.75rem;
            font-weight: 600;
            cursor: pointer;
            display: inline-flex;
            align-items: center;
            gap: 6px;
            transition: 0.2s;
        }
        .icon-action:active { transform: scale(0.96); background: #f8fafc; }

        /* SEARCH ZONE */
        .search-zone {
            background: #ffffff;
            border-radius: 32px;
            padding: 14px 18px;
            margin-bottom: 22px;
            display: flex;
            flex-wrap: wrap;
            align-items: flex-end;
            gap: 14px;
            border: 1px solid #e2edf5;
            box-shadow: 0 2px 6px rgba(0,0,0,0.02);
        }
        .search-group {
            flex: 2;
            min-width: 150px;
        }
        .search-group label {
            font-size: 0.65rem;
            font-weight: 700;
            color: #3b4e6e;
            margin-bottom: 4px;
            display: block;
        }
        .search-group input, .search-group select {
            width: 100%;
            padding: 8px 14px;
            border-radius: 32px;
            border: 1px solid #cbdcec;
            background: #fefefe;
            font-size: 0.8rem;
        }
        .btn-reset-filter {
            background: none;
            border: 1px solid #cbdcec;
            padding: 8px 24px;
            border-radius: 40px;
            font-weight: 600;
            font-size: 0.75rem;
            cursor: pointer;
            margin-bottom: 0;
            white-space: nowrap;
            transition: 0.2s;
        }
        .info-filter {
            font-size: 0.7rem;
            background: #eef2fc;
            display: inline-block;
            padding: 5px 14px;
            border-radius: 40px;
            margin-bottom: 14px;
        }

        /* TABLE WRAPPER */
        .table-scroll {
            overflow-x: auto;
            border-radius: 24px;
            -webkit-overflow-scrolling: touch;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            background: white;
            border-radius: 24px;
            min-width: 820px;
        }
        th {
            background: #eef2fa;
            padding: 14px 10px;
            font-size: 0.7rem;
            font-weight: 800;
            color: #1e3a5f;
            text-transform: uppercase;
            letter-spacing: 0.4px;
        }
        td {
            padding: 12px 10px;
            border-bottom: 1px solid #eef2f8;
            font-size: 0.8rem;
            color: #1f2a44;
        }
        .delete-cell {
            text-align: center;
        }
        .delete-btn {
            background: none;
            border: none;
            font-size: 1.2rem;
            cursor: pointer;
            color: #bc1c2e;
            padding: 5px 8px;
            border-radius: 30px;
            transition: 0.1s;
        }
        .delete-btn:active { background: #ffe3e3; transform: scale(0.95);}
        .empty-row td {
            text-align: center;
            padding: 36px;
            color: #6c7a91;
            font-style: italic;
        }
        footer {
            font-size: 0.6rem;
            text-align: center;
            padding: 12px 20px;
            background: white;
            color: #6f7c98;
            border-top: 1px solid #ecf3fa;
        }

        @media (min-width: 768px) {
            body {
                padding: 20px;
            }
            .app-container {
                padding: 24px 28px;
            }
            .form-grid {
                gap: 16px;
            }
            .input-group {
                flex: 1 1 auto;
            }
            button {
                width: auto;
                padding: 10px 24px;
            }
            .btn-group button {
                width: auto;
            }
            .search-box {
                flex: 2;
            }
            table {
                font-size: 0.8rem;
                min-width: auto;
            }
            td, th {
                padding: 10px 8px;
            }
        }

        @media (max-width: 900px) and (orientation: landscape) {
            body {
                padding: 8px;
            }
            .app-container {
                padding: 12px 16px;
            }
            .form-card {
                padding: 12px;
                margin: 8px 0;
            }
            .input-group input, .input-group select {
                padding: 8px 10px;
            }
            button {
                padding: 8px 16px;
            }
            th, td {
                padding: 6px 4px;
            }
        }

        @media (max-width: 480px) {
            .input-group {
                flex: 1 1 100%;
            }
            .search-box {
                flex-direction: column;
                align-items: stretch;
                border-radius: 28px;
                background: #f1f5f9;
                padding: 12px;
            }
            .search-box input {
                background: white;
                border-radius: 30px;
                padding: 10px 12px;
                margin: 2px 0;
            }
            .btn-group {
                width: 100%;
                justify-content: space-between;
            }
            .btn-group button {
                flex: 1;
            }
        }

        .info-note {
            font-size: 0.65rem;
            color: #5c7a94;
            margin-top: 8px;
            text-align: center;
        }
    </style>
</head>
<body>
<div class="app-container">
    <div class="app-header">
        <h1>Manajemen Barang Masuk</h1>
        <p>Data Supplier + Filter Nama, Supplier & Kategori • Optimasi Android</p>
    </div>

    <div class="form-wrapper">
        <form id="itemForm">
            <div class="grid-2cols">
                <div class="input-field">
                    <label>🏢 Nama Supplier</label>
                    <input type="text" id="supplierName" placeholder="PT. Maju Jaya" required>
                </div>
                <div class="input-field">
                    <label>📦 Nama Barang</label>
                    <input type="text" id="productName" placeholder="Kertas A4 / Box Display" required>
                </div>
            </div>
            <div class="grid-2cols">
                <div class="input-field">
                    <label>📂 Kategori</label>
                    <select id="category" required>
                        <option value="" disabled selected>Pilih Kategori</option>
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
                <div class="input-field">
                    <label>🔢 Jumlah</label>
                    <input type="number" id="quantity" placeholder="Jumlah" min="1" required>
                </div>
                <div class="input-field">
                    <label>📏 Satuan</label>
                    <input type="text" id="unit" placeholder="pcs / box / kg" required>
                </div>
            </div>
            <div class="grid-2cols">
                <div class="input-field">
                    <label>📅 Tanggal Masuk</label>
                    <input type="date" id="entryDate" required>
                </div>
                <div class="input-field">
                    <label>📝 Catatan / PO</label>
                    <input type="text" id="notes" placeholder="No Invoice / PO">
                </div>
            </div>
            <div class="btn-row">
                <button type="submit" class="btn btn-primary">➕ Tambah Barang</button>
                <button type="button" id="resetFormBtn" class="btn btn-secondary">🔄 Reset Form</button>
            </div>
        </form>
    </div>

    <div class="record-panel">
        <div class="toolbar-flex">
            <div class="title-count">
                <h3>📋 Daftar Penerimaan</h3>
                <span class="badge-total" id="totalItemBadge">0 item</span>
            </div>
            <div class="action-btns">
                <button id="printTable" class="icon-action">🖨️ Print</button>
                <button id="exportCSV" class="icon-action">📎 Ekspor CSV</button>
            </div>
        </div>

        <!-- filter panel -->
        <div class="search-zone">
            <div class="search-group">
                <label>🔍 Cari Nama Barang</label>
                <input type="text" id="searchProductName" placeholder="Nama barang ...">
            </div>
            <div class="search-group">
                <label>🏭 Cari Supplier</label>
                <input type="text" id="searchSupplierName" placeholder="Supplier ...">
            </div>
            <div class="search-group">
                <label>📂 Filter Kategori</label>
                <select id="filterCategory">
                    <option value="">Semua Kategori</option>
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
            <button id="clearFilters" class="btn-reset-filter">✖️ Reset Filter</button>
        </div>
        <div id="filterResultsInfo" class="info-filter"></div>

        <div class="table-scroll">
            <table id="dataTable">
                <thead>
                    <tr><th>Supplier</th><th>Nama Barang</th><th>Kategori</th><th>Jumlah</th><th>Satuan</th><th>Tgl Masuk</th><th>Catatan</th><th>Aksi</th></tr>
                </thead>
                <tbody id="tableBody">
                    <tr class="empty-row"><td colspan="8">✨ Belum ada data, silakan tambah barang masuk.✨</td></tr>
                </tbody>
            </table>
        </div>
    </div>
    <footer>📱 Responsif Android (Portrait & Landscape) · Data tersimpan di LocalStorage · Real-time Filter</footer>
</div>

<script>
    // ----- DATA GLOBAL -----
    let allItems = [];           // seluruh data
    let filteredList = [];       // hasil filter

    // DOM Elements
    const form = document.getElementById('itemForm');
    const supplierInp = document.getElementById('supplierName');
    const productInp = document.getElementById('productName');
    const categorySelect = document.getElementById('category');
    const qtyInp = document.getElementById('quantity');
    const unitInp = document.getElementById('unit');
    const dateInp = document.getElementById('entryDate');
    const notesInp = document.getElementById('notes');
    const resetFormBtn = document.getElementById('resetFormBtn');
    const tbodyEl = document.getElementById('tableBody');
    const totalBadgeSpan = document.getElementById('totalItemBadge');
    const printBtnEl = document.getElementById('printTable');
    const exportBtnEl = document.getElementById('exportCSV');

    // filter elements
    const searchProductField = document.getElementById('searchProductName');
    const searchSupplierField = document.getElementById('searchSupplierName');
    const filterCategorySelect = document.getElementById('filterCategory');
    const clearFilterBtn = document.getElementById('clearFilters');
    const filterInfoSpan = document.getElementById('filterResultsInfo');

    // Helper: set default date today
    function setDefaultDate() {
        if (!dateInp.value) {
            const today = new Date();
            const yyyy = today.getFullYear();
            const mm = String(today.getMonth() + 1).padStart(2, '0');
            const dd = String(today.getDate()).padStart(2, '0');
            dateInp.value = `${yyyy}-${mm}-${dd}`;
        }
    }

    // Load from localStorage
    function loadDataFromStorage() {
        const stored = localStorage.getItem('inventory_supplier_app');
        if (stored) {
            try {
                allItems = JSON.parse(stored);
                if (!Array.isArray(allItems)) allItems = [];
            } catch(e) { allItems = []; }
        } else {
            allItems = [];
        }
        applyFilterAndRender();
    }

    function saveToStorage() {
        localStorage.setItem('inventory_supplier_app', JSON.stringify(allItems));
    }

    function escapeHtml(str) {
        if (!str) return '';
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        });
    }

    function formatDateForDisplay(dateStr) {
        if (!dateStr) return '-';
        let parts = dateStr.split('-');
        if (parts.length !== 3) return dateStr;
        return `${parts[2]}/${parts[1]}/${parts[0]}`;
    }

    // Filter + Render utama
    function applyFilterAndRender() {
        const keywordProduct = searchProductField.value.trim().toLowerCase();
        const keywordSupplier = searchSupplierField.value.trim().toLowerCase();
        const selectedCat = filterCategorySelect.value;

        filteredList = allItems.filter(item => {
            let matchProduct = true, matchSupplier = true, matchCat = true;
            if (keywordProduct) matchProduct = (item.nama || "").toLowerCase().includes(keywordProduct);
            if (keywordSupplier) matchSupplier = (item.supplier || "").toLowerCase().includes(keywordSupplier);
            if (selectedCat && selectedCat !== "") matchCat = (item.kategori || "") === selectedCat;
            return matchProduct && matchSupplier && matchCat;
        });

        const totalAll = allItems.length;
        const showing = filteredList.length;
        if (keywordProduct || keywordSupplier || (selectedCat && selectedCat !== "")) {
            filterInfoSpan.textContent = `🔎 Menampilkan ${showing} dari ${totalAll} item`;
        } else {
            filterInfoSpan.textContent = `📋 Total semua: ${totalAll} item`;
        }
        totalBadgeSpan.textContent = (showing === totalAll) ? `${totalAll} item` : `${showing} / ${totalAll}`;
        renderTable(filteredList);
    }

    function renderTable(items) {
        if (!tbodyEl) return;
        if (!items.length) {
            tbodyEl.innerHTML = `<tr class="empty-row"><td colspan="8">📭 Tidak ada data / tidak ditemukan berdasarkan filter.📭</td></tr>`;
            return;
        }
        let rows = '';
        for (let item of items) {
            rows += `
                <tr data-id="${item.id}">
                    <td>${escapeHtml(item.supplier)}</td>
                    <td>${escapeHtml(item.nama)}</td>
                    <td>${escapeHtml(item.kategori)}</td>
                    <td>${escapeHtml(String(item.jumlah))}</td>
                    <td>${escapeHtml(item.unit)}</td>
                    <td>${escapeHtml(formatDateForDisplay(item.tanggal))}</td>
                    <td>${escapeHtml(item.catatan || '-')}</td>
                    <td class="delete-cell"><button class="delete-btn" data-id="${item.id}" aria-label="Hapus">🗑️</button></td>
                </tr>
            `;
        }
        tbodyEl.innerHTML = rows;
        document.querySelectorAll('.delete-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                e.stopPropagation();
                const id = btn.getAttribute('data-id');
                if (id) deleteItemById(id);
            });
        });
    }

    function deleteItemById(id) {
        const updated = allItems.filter(it => String(it.id) !== String(id));
        if (updated.length !== allItems.length) {
            allItems = updated;
            saveToStorage();
            applyFilterAndRender();
        }
    }

    function addNewItem(data) {
        const newId = Date.now() + '-' + Math.random().toString(36).substring(2, 12);
        const newEntry = {
            id: newId,
            supplier: data.supplier,
            nama: data.nama,
            kategori: data.kategori,
            jumlah: data.jumlah,
            unit: data.unit,
            tanggal: data.tanggal,
            catatan: data.catatan || ''
        };
        allItems.unshift(newEntry);
        saveToStorage();
        applyFilterAndRender();
    }

    function resetFormFields() {
        form.reset();
        setDefaultDate();
        categorySelect.value = "";
        supplierInp.focus();
    }

    // Submit handler
    form.addEventListener('submit', (e) => {
        e.preventDefault();

        const supplier = supplierInp.value.trim();
        const nama = productInp.value.trim();
        const kategori = categorySelect.value;
        const jumlahRaw = qtyInp.value.trim();
        const unit = unitInp.value.trim();
        const tanggal = dateInp.value;
        const catatan = notesInp.value.trim();

        if (!supplier) { alert("Nama Supplier wajib diisi!"); supplierInp.focus(); return; }
        if (!nama) { alert("Nama barang harus diisi!"); productInp.focus(); return; }
        if (!kategori) { alert("Pilih kategori!"); return; }
        if (!jumlahRaw || parseInt(jumlahRaw) <= 0) { alert("Jumlah harus lebih dari 0"); qtyInp.focus(); return; }
        if (!unit) { alert("Satuan wajib diisi!"); unitInp.focus(); return; }
        if (!tanggal) { alert("Tanggal masuk wajib"); return; }

        addNewItem({
            supplier: supplier,
            nama: nama,
            kategori: kategori,
            jumlah: parseInt(jumlahRaw),
            unit: unit,
            tanggal: tanggal,
            catatan: catatan
        });
        resetFormFields();
    });

    resetFormBtn.addEventListener('click', () => resetFormFields());

    // Filter events
    searchProductField.addEventListener('input', () => applyFilterAndRender());
    searchSupplierField.addEventListener('input', () => applyFilterAndRender());
    filterCategorySelect.addEventListener('change', () => applyFilterAndRender());
    clearFilterBtn.addEventListener('click', () => {
        searchProductField.value = '';
        searchSupplierField.value = '';
        filterCategorySelect.value = '';
        applyFilterAndRender();
    });

    // Print (hilangkan kolom aksi)
    printBtnEl.addEventListener('click', () => {
        const originalTable = document.getElementById('dataTable');
        const cloneTable = originalTable.cloneNode(true);
        const theadRow = cloneTable.querySelector('thead tr');
        if (theadRow && theadRow.children.length >= 8) theadRow.children[7].textContent = '';
        cloneTable.querySelectorAll('tbody tr').forEach(row => {
            const lastCell = row.querySelector('td:last-child');
            if (lastCell) lastCell.textContent = '';
        });
        const printWindow = window.open('', '_blank');
        printWindow.document.write(`
            <html><head><title>Laporan Barang Masuk Supplier</title>
            <style>
                body { font-family: system-ui, sans-serif; margin: 20px; }
                table { width: 100%; border-collapse: collapse; }
                th, td { border: 1px solid #aaa; padding: 8px; text-align: left; }
                th { background: #eef2fa; }
                h2 { color: #1e3a5f; }
            </style>
            </head><body>
            <h2>📦 Laporan Penerimaan Barang (Supplier)</h2>
            ${cloneTable.outerHTML}
            <p><small>Dicetak: ${new Date().toLocaleString()}</small></p>
            </body></html>
        `);
        printWindow.document.close();
        printWindow.print();
    });

    // Export CSV berdasarkan data yang sedang difilter
    exportBtnEl.addEventListener('click', () => {
        const dataToExport = filteredList.length ? filteredList : allItems;
        if (!dataToExport.length) {
            alert("Tidak ada data untuk diekspor.");
            return;
        }
        const headers = ['Supplier', 'Nama Barang', 'Kategori', 'Jumlah', 'Satuan', 'Tanggal Masuk', 'Catatan'];
        const csvRows = [headers.join(',')];
        for (const it of dataToExport) {
            const row = [
                `"${(it.supplier || '').replace(/"/g, '""')}"`,
                `"${(it.nama || '').replace(/"/g, '""')}"`,
                `"${(it.kategori || '').replace(/"/g, '""')}"`,
                it.jumlah,
                `"${(it.unit || '').replace(/"/g, '""')}"`,
                `"${it.tanggal || ''}"`,
                `"${(it.catatan || '').replace(/"/g, '""')}"`
            ];
            csvRows.push(row.join(','));
        }
        const blob = new Blob(["\uFEFF" + csvRows.join('\n')], { type: 'text/csv;charset=utf-8;' });
        const link = document.createElement('a');
        const url = URL.createObjectURL(blob);
        link.href = url;
        link.setAttribute('download', `barang_masuk_${new Date().toISOString().slice(0,19)}.csv`);
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
        URL.revokeObjectURL(url);
    });

    // init
    setDefaultDate();
    loadDataFromStorage();
</script>
</body>
</html>
