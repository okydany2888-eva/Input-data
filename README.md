# Input-data
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Form Barang Masuk - Sistem Inventaris Mini</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: system-ui, 'Segoe UI', 'Inter', 'Roboto', sans-serif;
        }

        body {
            background: linear-gradient(145deg, #e9f0f5 0%, #d9e2ec 100%);
            min-height: 100vh;
            padding: 2rem 1.5rem;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* container utama */
        .container {
            max-width: 820px;
            width: 100%;
            margin: 0 auto;
        }

        /* kartu form */
        .card {
            background: rgba(255, 255, 255, 0.96);
            backdrop-filter: blur(0px);
            border-radius: 2rem;
            box-shadow: 0 25px 45px -12px rgba(0, 0, 0, 0.3), 0 4px 12px rgba(0, 0, 0, 0.05);
            overflow: hidden;
            transition: transform 0.2s ease;
        }

        .card:hover {
            transform: translateY(-3px);
        }

        /* header */
        .header {
            background: #1e3a5f;
            background: linear-gradient(135deg, #0f2c44, #1e4a6e);
            padding: 1.8rem 2rem;
            color: white;
        }

        .header h1 {
            font-size: 1.9rem;
            font-weight: 600;
            letter-spacing: -0.3px;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .header h1:before {
            content: "📦";
            font-size: 2rem;
        }

        .header p {
            font-size: 0.9rem;
            opacity: 0.85;
            margin-top: 6px;
            font-weight: 400;
        }

        /* konten form */
        .form-body {
            padding: 2rem 2rem 1.8rem;
        }

        .form-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 1.4rem 2rem;
        }

        .full-width {
            grid-column: span 2;
        }

        .field-group {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
        }

        .field-group label {
            font-weight: 600;
            font-size: 0.85rem;
            text-transform: uppercase;
            letter-spacing: 0.6px;
            color: #2c3e4e;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .field-group label i {
            font-style: normal;
            font-size: 1.1rem;
        }

        .field-group input,
        .field-group select,
        .field-group textarea {
            padding: 0.85rem 1rem;
            border: 1.5px solid #e2e8f0;
            border-radius: 1rem;
            font-size: 0.95rem;
            transition: all 0.2s;
            background-color: #fefefe;
            outline: none;
            font-weight: 500;
            color: #1e293b;
        }

        .field-group input:focus,
        .field-group select:focus,
        .field-group textarea:focus {
            border-color: #2c7da0;
            box-shadow: 0 0 0 3px rgba(44, 125, 160, 0.2);
            background-color: #ffffff;
        }

        .field-group input[type="number"] {
            -moz-appearance: textfield;
        }

        .field-group input[type="number"]::-webkit-inner-spin-button,
        .field-group input[type="number"]::-webkit-outer-spin-button {
            opacity: 0.5;
        }

        textarea {
            resize: vertical;
            min-height: 80px;
        }

        /* tombol aksi */
        .action-buttons {
            display: flex;
            flex-wrap: wrap;
            justify-content: flex-end;
            gap: 1rem;
            margin-top: 2rem;
            border-top: 1px solid #e9edf2;
            padding-top: 1.8rem;
        }

        .btn {
            border: none;
            padding: 0.75rem 1.8rem;
            border-radius: 2rem;
            font-weight: 600;
            font-size: 0.9rem;
            cursor: pointer;
            transition: 0.2s;
            background: #f1f5f9;
            color: #1e293b;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        .btn-primary {
            background: #1e5f7a;
            color: white;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }

        .btn-primary:hover {
            background: #0f4b62;
            transform: translateY(-2px);
            box-shadow: 0 8px 18px rgba(0, 0, 0, 0.1);
        }

        .btn-secondary {
            background: #eef2f6;
            border: 1px solid #cbd5e1;
        }

        .btn-secondary:hover {
            background: #e2e8f0;
            transform: translateY(-1px);
        }

        .btn-danger-outline {
            background: transparent;
            border: 1px solid #dc2626;
            color: #b91c1c;
        }

        .btn-danger-outline:hover {
            background: #fee2e2;
            border-color: #b91c1c;
        }

        /* tabel riwayat barang masuk */
        .history-section {
            margin-top: 2rem;
            background: white;
            border-radius: 1.5rem;
            overflow: hidden;
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.05);
        }

        .history-header {
            background: #f8fafc;
            padding: 1rem 1.5rem;
            border-bottom: 1px solid #e2edf2;
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            flex-wrap: wrap;
        }

        .history-header h3 {
            font-size: 1.2rem;
            font-weight: 600;
            color: #0f2c3f;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .badge-count {
            background: #1e5f7a20;
            color: #1e5f7a;
            padding: 4px 12px;
            border-radius: 60px;
            font-size: 0.8rem;
            font-weight: 600;
        }

        .table-wrapper {
            overflow-x: auto;
            padding: 0 0.2rem;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.85rem;
        }

        th {
            text-align: left;
            padding: 1rem 1rem;
            background-color: #f9fbfd;
            color: #2c5368;
            font-weight: 600;
            border-bottom: 1.5px solid #e0e9f0;
        }

        td {
            padding: 0.9rem 1rem;
            border-bottom: 1px solid #edf2f7;
            color: #1e2f3c;
            vertical-align: middle;
        }

        .delete-row-btn {
            background: none;
            border: none;
            font-size: 1.2rem;
            cursor: pointer;
            color: #a0aec0;
            transition: 0.2s;
            padding: 5px 8px;
            border-radius: 30px;
        }

        .delete-row-btn:hover {
            color: #e53e3e;
            background: #fff0f0;
            transform: scale(1.05);
        }

        .empty-row td {
            text-align: center;
            padding: 2rem;
            color: #7f8c8d;
            font-style: italic;
        }

        .footer-note {
            font-size: 0.7rem;
            text-align: center;
            margin-top: 1rem;
            color: #5c7a8c;
        }

        @media (max-width: 680px) {
            body {
                padding: 1rem;
            }
            .form-grid {
                grid-template-columns: 1fr;
                gap: 1rem;
            }
            .full-width {
                grid-column: span 1;
            }
            .action-buttons {
                justify-content: stretch;
            }
            .btn {
                flex: 1;
                justify-content: center;
            }
            .header h1 {
                font-size: 1.5rem;
            }
        }

        /* animasi sukses mini */
        .toast-msg {
            position: fixed;
            bottom: 25px;
            right: 25px;
            background: #1f5e3c;
            color: white;
            padding: 12px 24px;
            border-radius: 60px;
            font-weight: 500;
            font-size: 0.85rem;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
            z-index: 1000;
            transform: translateY(120%);
            transition: transform 0.25s ease;
            backdrop-filter: blur(8px);
            background: #0f3b2ccc;
            background: #1e5f7a;
        }
        .toast-msg.show {
            transform: translateY(0);
        }
    </style>
</head>
<body>

<div class="container">
    <div class="card">
        <div class="header">
            <h1>Input Barang Masuk</h1>
            <p>Catat setiap kedatangan stok / barang baru | Data tersimpan secara lokal</p>
        </div>
        <div class="form-body">
            <form id="barangMasukForm">
                <div class="form-grid">
                    <div class="field-group">
                        <label><i>📑</i> Nama Barang *</label>
                        <input type="text" id="namaBarang" placeholder="Contoh: smoke box, lume, dll" required>
                    </div>
                    <div class="field-group">
                        <label><i>📄</i> Kategori</label>
                        <select id="kategori">
                            <option value="Booklet 1 ¼">Booklet 1 ¼</option>
                            <option value="Booklet 1 ¼ + tips">Booklet 1 ¼ + tips</option>
                            <option value="Display box">Display box</option>
                            <option value="Booket kss">Booklet kss</option>
                            <option value="Booklet kss + tips">Booklet kss + tips</option>
                        </select>
                    </div>
                    <div class="field-group">
                        <label><i>📥</i> Jumlah Masuk *</label>
                        <input type="number" id="jumlah" min="1" step="1" placeholder="Satuan unit" required>
                    </div>
                    <div class="field-group">
                        <label><i>🏷️</i> Satuan Unit </label>
                        <input type="letter" id="huruf" min="pcs, sheet, kg, roll">
                    </div>
                    <div class="field-group full-width">
                        <label><i>📅</i> Tanggal Masuk</label>
                        <input type="date" id="tanggalMasuk" required>
                    </div>
                    <div class="field-group full-width">
                        <label><i>📝</i> Catatan / Supplier</label>
                        <textarea id="catatan" placeholder="Opsional: nama supplier, nomor PO, keterangan"></textarea>
                    </div>
                </div>
                <div class="action-buttons">
                    <button type="button" class="btn btn-secondary" id="resetBtn">↻ Reset Form</button>
                    <button type="submit" class="btn btn-primary">➕ Tambah Barang Masuk</button>
                </div>
            </form>
        </div>
    </div>

    <!-- riwayat barang masuk -->
    <div class="history-section">
        <div class="history-header">
            <h3>📋 Riwayat Barang Masuk</h3>
            <span class="badge-count" id="totalItemCount">0 item</span>
        </div>
        <div class="table-wrapper">
            <table id="historyTable">
                <thead>
                    <tr>
                        <th>Nama Barang</th>
                        <th>Kategori</th>
                        <th>Jumlah</th>
                        <th>Satuan Unit</th>
                        <th>Tanggal Masuk</th>
                        <th>Catatan</th>
                        <th style="width:50px">Aksi</th>
                    </tr>
                </thead>
                <tbody id="tableBody">
                    <tr class="empty-row">
                        <td colspan="7">Belum ada data barang masuk. Silakan tambah melalui form di atas.</td>
                    </tr>
                </tbody>
            </table>
        </div>
        <div class="footer-note">
            ✅ Data disimpan di LocalStorage browser | Klik hapus untuk menghapus satu record.
        </div>
    </div>
</div>

<div id="toastMsg" class="toast-msg">✨ Barang berhasil ditambahkan</div>

<script>
    // ----- Data handling menggunakan localStorage -----
    const STORAGE_KEY = "incoming_goods_list";

    // Model: array of objects
    let incomingItems = [];

    // DOM elements
    const form = document.getElementById('barangMasukForm');
    const namaInput = document.getElementById('namaBarang');
    const kategoriSelect = document.getElementById('kategori');
    const jumlahInput = document.getElementById('jumlah');
    const hargaInput = document.getElementById('harga');
    const tanggalInput = document.getElementById('tanggalMasuk');
    const catatanTextarea = document.getElementById('catatan');
    const tableBody = document.getElementById('tableBody');
    const totalItemCountSpan = document.getElementById('totalItemCount');
    const resetBtn = document.getElementById('resetBtn');
    const toast = document.getElementById('toastMsg');

    // Helper: tampilkan toast notifikasi
    function showToast(message, isError = false) {
        toast.textContent = message || (isError ? "Gagal!" : "Berhasil");
        if (isError) {
            toast.style.background = "#b91c1c";
            toast.style.backgroundColor = "#b91c1c";
        } else {
            toast.style.background = "#1e5f7a";
            toast.style.backgroundColor = "#1e5f7a";
        }
        toast.classList.add('show');
        setTimeout(() => {
            toast.classList.remove('show');
            setTimeout(() => {
                if (!isError) toast.style.background = "#1e5f7a";
            }, 200);
        }, 2000);
    }

    // load data dari localStorage
    function loadDataFromStorage() {
        const stored = localStorage.getItem(STORAGE_KEY);
        if (stored) {
            try {
                incomingItems = JSON.parse(stored);
                if (!Array.isArray(incomingItems)) incomingItems = [];
            } catch(e) { incomingItems = []; }
        } else {
            // data contoh awal (biar tidak kosong total, optional)
            incomingItems = [];
            // opsional: jika ingin demo data bisa diisi
            // tapi biarkan kosong dulu agar clean state
        }
        renderTable();
        updateTotalBadge();
    }

    // simpan ke localStorage
    function persistData() {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(incomingItems));
        updateTotalBadge();
    }

    // update badge jumlah record
    function updateTotalBadge() {
        const total = incomingItems.length;
        totalItemCountSpan.textContent = `${total} item`;
    }

    // render tabel berdasarkan array incomingItems
    function renderTable() {
        if (!tableBody) return;
        if (incomingItems.length === 0) {
            tableBody.innerHTML = `<tr class="empty-row"><td colspan="7">Belum ada data barang masuk. Silakan tambah melalui form di atas.</td></tr>`;
            return;
        }

        let html = '';
        incomingItems.forEach((item, index) => {
            // format harga ke rupiah
            const hargaValue = item.harga ? parseInt(item.harga) : 0;
            const formattedHarga = hargaValue > 0 ? new Intl.NumberFormat('id-ID').format(hargaValue) : '-';
            const catatanTeks = item.catatan ? (item.catatan.length > 35 ? item.catatan.substring(0,32)+'...' : item.catatan) : '-';
            const fullCatatan = item.catatan || '';
            html += `
                <tr data-idx="${index}">
                    <td><strong>${escapeHtml(item.namaBarang)}</strong></td>
                    <td>${escapeHtml(item.kategori)}</td>
                    <td style="font-weight:500;">${item.jumlah} unit</td>
                    <td>${formattedHarga !== '-' ? 'Rp ' + formattedHarga : '-'}</td>
                    <td>${item.tanggalMasuk || '-'}</td>
                    <td title="${escapeHtml(fullCatatan)}">${escapeHtml(catatanTeks)}</td>
                    <td><button class="delete-row-btn" data-index="${index}" title="Hapus item">🗑️</button></td>
                </tr>
            `;
        });
        tableBody.innerHTML = html;

        // attach event listener untuk tombol hapus
        const deleteBtns = document.querySelectorAll('.delete-row-btn');
        deleteBtns.forEach(btn => {
            btn.addEventListener('click', (e) => {
                e.stopPropagation();
                const idx = btn.getAttribute('data-index');
                if (idx !== null) deleteItemByIndex(parseInt(idx));
            });
        });
    }

    // fungsi hapus berdasarkan index array
    function deleteItemByIndex(index) {
        if (index >= 0 && index < incomingItems.length) {
            const deletedItem = incomingItems[index];
            incomingItems.splice(index, 1);
            persistData();
            renderTable();
            showToast(`🗑️ "${deletedItem.namaBarang}" dihapus`, false);
        } else {
            showToast("Gagal menghapus item", true);
        }
    }

    // helper sederhana untuk menghindari XSS
    function escapeHtml(str) {
        if (!str) return '';
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        }).replace(/[\uD800-\uDBFF][\uDC00-\uDFFF]/g, function(c) {
            return c;
        });
    }

    // validasi dan tambah item baru
    function addNewItem(event) {
        event.preventDefault();

        // ambil value
        let namaBarang = namaInput.value.trim();
        const kategori = kategoriSelect.value;
        let jumlah = parseInt(jumlahInput.value);
        let harga = hargaInput.value.trim() === '' ? 0 : parseInt(hargaInput.value);
        const tanggalMasuk = tanggalInput.value;
        const catatan = catatanTextarea.value.trim();

        // validasi mandatory
        if (!namaBarang) {
            showToast("❌ Nama barang harus diisi", true);
            namaInput.focus();
            return;
        }
        if (isNaN(jumlah) || jumlah <= 0) {
            showToast("⚠️ Jumlah harus angka positif", true);
            jumlahInput.focus();
            return;
        }
        if (!tanggalMasuk) {
            showToast("📆 Silakan pilih tanggal masuk", true);
            tanggalInput.focus();
            return;
        }
        if (isNaN(harga)) harga = 0;
        if (harga < 0) harga = 0;

        // membuat objek barang masuk
        const newItem = {
            id: Date.now() + Math.random() * 1000, // id unik
            namaBarang: namaBarang,
            kategori: kategori,
            jumlah: jumlah,
            harga: harga,
            tanggalMasuk: tanggalMasuk,
            catatan: catatan,
            createdAt: new Date().toISOString()
        };

        incomingItems.unshift(newItem); // item baru muncul di atas (paling baru)
        persistData();
        renderTable();

        // reset form setelah submit (opsi keep beberapa field? sesuai kebutuhan, reset sebagian)
        resetFormFields();

        showToast(`✅ "${namaBarang}" (${jumlah} unit) berhasil ditambahkan`);
    }

    // reset form ke default (kosongkan / set default)
    function resetFormFields() {
        namaInput.value = '';
        kategoriSelect.value = 'Elektronik';
        jumlahInput.value = '';
        hargaInput.value = '';
        // set tanggal hari ini sebagai default (lebih rapi)
        const today = new Date();
        const yyyy = today.getFullYear();
        const mm = String(today.getMonth() + 1).padStart(2, '0');
        const dd = String(today.getDate()).padStart(2, '0');
        tanggalInput.value = `${yyyy}-${mm}-${dd}`;
        catatanTextarea.value = '';
        // hilangkan error style jika perlu
        namaInput.focus();
    }

    // set default tanggal ketika halaman pertama
    function setDefaultDate() {
        if (!tanggalInput.value) {
            const today = new Date();
            const yyyy = today.getFullYear();
            const mm = String(today.getMonth() + 1).padStart(2, '0');
            const dd = String(today.getDate()).padStart(2, '0');
            tanggalInput.value = `${yyyy}-${mm}-${dd}`;
        }
    }

    // event listener untuk reset manual (tombol reset)
    function handleResetForm() {
        resetFormFields();
        showToast("Form telah direset", false);
    }

    // initial load dan event binding
    function init() {
        loadDataFromStorage();
        setDefaultDate();

        form.addEventListener('submit', addNewItem);
        resetBtn.addEventListener('click', handleResetForm);

        // tambahan validasi interaktif (optional)
        jumlahInput.addEventListener('input', function() {
            if (jumlahInput.value < 0) jumlahInput.value = 1;
        });
        hargaInput.addEventListener('input', function() {
            if (hargaInput.value < 0) hargaInput.value = 0;
        });
    }

    // jalankan init
    init();
</script>
</body>
</html>
