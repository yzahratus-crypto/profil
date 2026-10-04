<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gerai Habani - System Penjualan & Size Tracking</title>
  <!-- Tailwind CSS via CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- FontAwesome Icons -->
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            brand: {
              50: '#f0fdf4',
              100: '#dcfce7',
              600: '#059669',
              700: '#047857',
              800: '#065f46',
            }
          }
        }
      }
    }
  </script>
  <style>
    /* Watermark Logo Transparan di Background */
    .watermark-bg {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 50vw;
      max-width: 450px;
      height: auto;
      opacity: 0.08;
      pointer-events: none;
      z-index: 0;
    }
    .content-layer {
      position: relative;
      z-index: 10;
    }
    /* Sembunyikan spinner input number */
    input[type=number]::-webkit-inner-spin-button, 
    input[type=number]::-webkit-outer-spin-button { 
      -webkit-appearance: none; 
      margin: 0; 
    }
    input[type=number] {
      -moz-appearance: textfield;
    }
  </style>
</head>
<body class="bg-slate-50 text-slate-800 font-sans min-h-screen relative flex flex-col overflow-x-hidden">

  <!-- LOGO WATERMARK TRANSPARAN BACKGROUND -->
  <img id="appWatermark" src="https://via.placeholder.com/400x400/047857/ffffff?text=GERAI+HABANI" alt="Watermark Gerai Habani" class="watermark-bg">

  <!-- ================= TAMPILAN LOGIN ================= -->
  <div id="loginPage" class="content-layer flex-1 flex items-center justify-center p-4 transition-all duration-300">
    <div class="bg-white/95 backdrop-blur-sm p-8 rounded-3xl shadow-2xl border border-emerald-100 w-full max-w-md text-center transform scale-100 opacity-100 transition-all duration-300">
      <div class="mb-6 flex flex-col items-center">
        <!-- LOGO PROFIL TAMPILAN LOGIN -->
        <img id="loginLogo" src="https://via.placeholder.com/120x120/047857/ffffff?text=HABANI" alt="Logo Gerai Habani" class="h-28 w-28 object-cover mb-4 rounded-full border-4 border-amber-400 shadow-md">
        <h1 class="text-3xl font-bold text-brand-800 tracking-wide">Gerai Habani</h1>
        <p class="text-xs text-slate-500 mt-1">Sistem Penjualan, Stok & Size XS-XXL</p>
      </div>

      <form id="loginForm" onsubmit="handleLogin(event)" class="space-y-4 text-left">
        <div>
          <label class="block text-xs font-semibold text-slate-600 uppercase tracking-wider mb-1">Username</label>
          <div class="relative">
            <span class="absolute inset-y-0 left-0 flex items-center pl-3 text-slate-400"><i class="fas fa-user"></i></span>
            <input type="text" id="username" required value="admin" class="w-full pl-10 pr-4 py-3 rounded-xl border border-slate-300 focus:ring-2 focus:ring-brand-600 outline-none text-sm transition">
          </div>
        </div>

        <div>
          <label class="block text-xs font-semibold text-slate-600 uppercase tracking-wider mb-1">Password</label>
          <div class="relative">
            <span class="absolute inset-y-0 left-0 flex items-center pl-3 text-slate-400"><i class="fas fa-lock"></i></span>
            <input type="password" id="password" required value="habani123" class="w-full pl-10 pr-12 py-3 rounded-xl border border-slate-300 focus:ring-2 focus:ring-brand-600 outline-none text-sm transition">
            <button type="button" onclick="togglePassword()" class="absolute inset-y-0 right-0 pr-4 flex items-center text-slate-400 hover:text-slate-600 transition">
              <i id="eyeIcon" class="fas fa-eye"></i>
            </button>
          </div>
        </div>

        <button type="submit" class="w-full bg-brand-700 hover:bg-brand-800 text-white font-semibold py-3.5 rounded-xl shadow-lg transition duration-200 flex items-center justify-center gap-2 mt-3 transform hover:scale-[1.02]">
          <i class="fas fa-sign-in-alt"></i> Masuk Aplikasi
        </button>
      </form>
    </div>
  </div>

  <!-- ================= TAMPILAN DASHBOARD ================= -->
  <div id="dashboardPage" class="hidden content-layer flex-1 flex flex-col">
    <!-- NAVBAR HEADER (STICKY FIXED TOP) -->
    <header class="bg-brand-800 text-white shadow-lg sticky top-0 z-30 border-b border-brand-700">
      <div class="max-w-[98rem] mx-auto px-4 py-3 flex flex-wrap justify-between items-center gap-4">
        <div class="flex items-center gap-3">
          <img id="navLogo" src="https://via.placeholder.com/120x120/047857/ffffff?text=HABANI" alt="Logo" class="h-11 w-11 rounded-full border-2 border-amber-400 object-cover bg-white shadow-sm">
          <div>
            <h1 class="font-bold text-xl leading-tight tracking-wide">Gerai Habani</h1>
            <p class="text-xs text-amber-300">Penjualan, Size Tracking & Stok</p>
          </div>
        </div>

        <div class="flex items-center gap-2.5">
          <button onclick="openLogoModal()" class="bg-emerald-700 hover:bg-emerald-600 text-xs px-3.5 py-2.5 rounded-xl transition border border-emerald-500 flex items-center gap-1.5 shadow-sm">
            <i class="fas fa-image"></i> Ganti Logo
          </button>
          <button onclick="openBankModal()" class="bg-amber-600 hover:bg-amber-500 text-xs px-3.5 py-2.5 rounded-xl transition font-medium flex items-center gap-1.5 shadow-sm">
            <i class="fas fa-university"></i> Rekening
          </button>
          <button onclick="openStockModal()" class="bg-teal-700 hover:bg-teal-600 text-xs px-3.5 py-2.5 rounded-xl transition border border-teal-500 flex items-center gap-1.5 shadow-sm">
            <i class="fas fa-boxes"></i> Master Stok
          </button>
          <button onclick="handleLogout()" class="bg-red-600 hover:bg-red-700 text-xs px-3.5 py-2.5 rounded-xl transition ml-2.5 flex items-center gap-1.5 shadow-sm">
            <i class="fas fa-sign-out-alt"></i> Keluar
          </button>
        </div>
      </div>
    </header>

    <!-- CONTENT BODY -->
    <main class="max-w-[98rem] mx-auto px-4 py-6 flex-1 w-full space-y-6">
      
      <!-- ACTION BAR & SEARCH (NON-STICKY AGAR TIDAK MENUTUPI KONTEN DI BAWAHNYA) -->
      <div class="bg-white/90 backdrop-blur-sm p-5 rounded-2xl shadow border border-slate-200 flex flex-wrap gap-4 justify-between items-center">
        <div class="flex flex-wrap items-center gap-2.5 w-full md:w-auto">
          <button onclick="openOrderModal()" class="bg-brand-700 hover:bg-brand-800 text-white font-semibold text-sm px-5 py-3 rounded-xl shadow-lg flex items-center gap-2 transition transform hover:scale-[1.02]">
            <i class="fas fa-plus-circle"></i> Tambah Orderan Baru
          </button>
          <span class="text-slate-300 hidden md:inline">|</span>
          <button onclick="filterStatus('all')" class="bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs font-medium px-4 py-2.5 rounded-xl transition">Semua</button>
          <button onclick="filterStatus('ready')" class="bg-emerald-100 hover:bg-emerald-200 text-emerald-900 text-xs font-medium px-4 py-2.5 rounded-xl transition"><i class="fas fa-check-circle text-emerald-600 mr-1"></i> Barang Ready</button>
          <button onclick="filterStatus('pending')" class="bg-amber-100 hover:bg-amber-200 text-amber-900 text-xs font-medium px-4 py-2.5 rounded-xl transition"><i class="fas fa-clock text-amber-600 mr-1"></i> Belum Lunas</button>
        </div>

        <div class="relative w-full md:w-80">
          <span class="absolute inset-y-0 left-0 flex items-center pl-4 text-slate-400"><i class="fas fa-search"></i></span>
          <input type="text" id="searchInput" oninput="renderOrders()" placeholder="Cari Nama Customer, No Resi, Produk..." class="w-full pl-11 pr-4 py-3 border border-slate-300 rounded-xl text-sm outline-none focus:ring-2 focus:ring-brand-600 transition shadow-inner bg-white/50">
        </div>
      </div>

      <!-- TABEL TRANSAKSI -->
      <div class="bg-white/95 backdrop-blur-sm rounded-2xl shadow-xl border border-slate-200 overflow-hidden">
        <div class="overflow-x-auto">
          <table class="w-full text-left text-xs border-collapse min-w-[1700px]">
            <thead class="bg-brand-800 text-white uppercase text-[11px] tracking-wider font-semibold">
              <tr>
                <th class="p-4 text-center border-b border-brand-700 w-12 bg-brand-800">No</th>
                <th class="p-4 border-b border-brand-700 min-w-[150px]">Nama Customer</th>
                <th class="p-4 border-b border-brand-700 min-w-[200px]">Alamat Rumah Customer</th>
                <th class="p-4 border-b border-brand-700 w-32">No Tlpn</th>
                <th class="p-4 border-b border-brand-700 w-32">No Resi</th>
                <th class="p-4 border-b border-brand-700 min-w-[300px]">Produk & Size XS-XXL</th>
                <th class="p-4 border-b border-brand-700 w-32">Estimasi Ready</th>
                <th class="p-4 border-b border-brand-700 w-32">Harga Total</th>
                <th class="p-4 border-b border-brand-700 min-w-[170px]">Riwayat DP & Tanggal</th>
                <th class="p-4 border-b border-brand-700 w-32 font-bold text-amber-300">Sisa DP</th>
                <th class="p-4 border-b border-brand-700 w-28">Diskon</th>
                <th class="p-4 border-b border-brand-700 text-center w-24">Quantity</th>
                <th class="p-4 border-b border-brand-700 w-28">Delivery</th>
                <th class="p-4 border-b border-brand-700 w-40">Pembayaran</th>
                <th class="p-4 border-b border-brand-700 text-center w-36 sticky right-0 bg-brand-800 z-10">Aksi</th>
              </tr>
            </thead>
            <tbody id="orderTableBody" class="divide-y divide-slate-200">
              <!-- Rendered Dynamically -->
            </tbody>
          </table>
        </div>
      </div>
    </main>
  </div>

  <!-- MODAL GANTI LOGO -->
  <div id="logoModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm hidden z-50 flex items-center justify-center p-4 transition-opacity duration-300">
    <div class="bg-white rounded-2xl max-w-sm w-full p-7 shadow-2xl transform transition-all duration-300 scale-95 opacity-0 modal-content">
      <div class="flex justify-between items-center mb-4">
        <h3 class="font-bold text-slate-900 text-lg"><i class="fas fa-image text-brand-700 mr-2"></i> Pengaturan Logo Aplikasi</h3>
        <button onclick="closeModal('logoModal')" class="text-slate-400 hover:text-slate-600 transition"><i class="fas fa-times"></i></button>
      </div>
      <p class="text-xs text-slate-500 mb-5">Upload logo baru. Format transparan (PNG) direkomendasikan untuk watermark.</p>
      
      <input type="file" id="logoFileInput" accept="image/*" class="text-xs w-full mb-5 file:mr-3 file:py-2 file:px-4 file:rounded-xl file:border-0 file:text-xs file:font-semibold file:bg-brand-50 file:text-brand-700 hover:file:bg-brand-100 transition cursor-pointer">

      <div class="flex justify-end gap-2.5 pt-2 border-t">
        <button onclick="closeModal('logoModal')" class="px-4 py-2 text-sm text-slate-600 hover:bg-slate-100 rounded-xl transition">Batal</button>
        <button onclick="saveCustomLogo()" class="px-5 py-2 text-sm bg-brand-700 hover:bg-brand-800 text-white font-semibold rounded-xl shadow transition">Simpan Logo</button>
      </div>
    </div>
  </div>

  <!-- MODAL KELOLA REKENING BANK -->
  <div id="bankModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm hidden z-50 flex items-center justify-center p-4 transition-opacity duration-300">
    <div class="bg-white rounded-2xl max-w-md w-full p-7 shadow-2xl transform transition-all duration-300 scale-95 opacity-0 modal-content space-y-5">
      <div class="flex justify-between items-center border-b pb-3 sticky top-0 bg-white">
        <h3 class="font-bold text-slate-900 text-lg"><i class="fas fa-university text-amber-600 mr-2"></i> Pengaturan Rekening Bank</h3>
        <button onclick="closeModal('bankModal')" class="text-slate-400 hover:text-slate-600 transition"><i class="fas fa-times"></i></button>
      </div>

      <div class="space-y-2.5 max-h-64 overflow-y-auto pr-2" id="bankListContainer">
        <!-- Render Rekening -->
      </div>

      <div class="border-t pt-4 space-y-3 bg-slate-50 p-4 rounded-xl border">
        <p class="text-sm font-semibold text-slate-800"><i class="fas fa-plus-circle text-brand-600 mr-1.5"></i> Tambah / Ubah Rekening</p>
        <div class="grid grid-cols-2 gap-3">
          <input type="text" id="newBankName" placeholder="Nama Bank (mis: BCA)" class="p-3 border rounded-xl text-xs outline-none focus:ring-1 focus:ring-brand-600 transition">
          <input type="text" id="newBankNo" placeholder="Nomor Rekening" class="p-3 border rounded-xl text-xs outline-none focus:ring-1 focus:ring-brand-600 transition">
        </div>
        <button onclick="addBank()" class="w-full bg-amber-600 hover:bg-amber-700 text-white font-semibold text-sm py-3 rounded-xl shadow transition"><i class="fas fa-plus mr-1.5"></i> Simpan Rekening</button>
      </div>
    </div>
  </div>

  <!-- MODAL MASTER STOK PRODUK -->
  <div id="stockModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm hidden z-50 flex items-center justify-center p-4 transition-opacity duration-300">
    <div class="bg-white rounded-2xl max-w-2xl w-full p-7 shadow-2xl transform transition-all duration-300 scale-95 opacity-0 modal-content space-y-5">
      <div class="flex justify-between items-center border-b pb-3 sticky top-0 bg-white">
        <h3 class="font-bold text-slate-900 text-lg"><i class="fas fa-boxes text-teal-700 mr-2"></i> Master Stok Produk & Harga</h3>
        <button onclick="closeModal('stockModal')" class="text-slate-400 hover:text-slate-600 transition"><i class="fas fa-times"></i></button>
      </div>

      <div class="space-y-2.5 max-h-80 overflow-y-auto pr-2" id="stockListContainer">
        <!-- Render Stok -->
      </div>

      <div class="border-t pt-4 space-y-3 bg-slate-50 p-4 rounded-xl border border-slate-200">
        <p class="text-sm font-semibold text-slate-800"><i class="fas fa-plus-circle text-teal-600 mr-1.5"></i> Tambah Produk Master</p>
        <div class="grid grid-cols-1 md:grid-cols-4 gap-3">
          <input type="text" id="newProdName" placeholder="Nama Produk..." class="col-span-2 p-3 border rounded-xl text-xs outline-none">
          <input type="number" id="newProdPrice" placeholder="Harga Standar Rp" class="p-3 border rounded-xl text-xs outline-none">
          <input type="number" id="newProdStock" placeholder="Total Stok" class="p-3 border rounded-xl text-xs outline-none">
        </div>
        <button onclick="addProductMaster()" class="w-full bg-teal-700 hover:bg-teal-800 text-white font-semibold text-sm py-3 rounded-xl shadow transition"><i class="fas fa-check-circle mr-1.5"></i> Simpan Produk Master</button>
      </div>
    </div>
  </div>

  <!-- MODAL ORDER FORM (INPUT PESANAN BARU) -->
  <div id="orderModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm hidden z-50 flex items-center justify-center p-4 overflow-y-auto transition-opacity duration-300">
    <div class="bg-white rounded-3xl max-w-5xl w-full p-8 shadow-2xl my-8 transform transition-all duration-300 scale-95 opacity-0 modal-content space-y-6 max-h-[90vh] overflow-y-auto pr-3">
      <div class="flex justify-between items-center border-b pb-4 sticky top-0 bg-white z-10 -mt-2 pt-2">
        <h3 id="orderModalTitle" class="font-bold text-brand-800 text-xl flex items-center gap-2.5"><i class="fas fa-cart-plus text-2xl"></i> Input Orderan Baru</h3>
        <button onclick="closeModal('orderModal')" class="text-slate-400 hover:text-slate-600 transition"><i class="fas fa-times text-lg"></i></button>
      </div>

      <form id="orderForm" onsubmit="saveOrder(event)" class="space-y-6 text-sm">
        <input type="hidden" id="editIndex" value="-1">

        <!-- DATA CUSTOMER -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4 bg-slate-50 p-5 rounded-2xl border border-slate-200 shadow-inner">
          <div class="md:col-span-3 pb-2 border-b mb-1">
            <h4 class="font-bold text-slate-800 flex items-center gap-2"><i class="fas fa-user-circle text-brand-600"></i> Informasi Data Customer</h4>
          </div>
          <div>
            <label class="block font-semibold mb-1.5 text-slate-700">Nama Customer*</label>
            <input type="text" id="custName" required list="customerSuggestions" oninput="autoFillCustomer()" placeholder="Ketik nama customer..." class="w-full p-3 border rounded-xl outline-none focus:ring-2 focus:ring-brand-600 transition">
            <datalist id="customerSuggestions"></datalist>
          </div>
          <div>
            <label class="block font-semibold mb-1.5 text-slate-700">No Tlpn (WhatsApp)*</label>
            <input type="text" id="custPhone" required placeholder="08123456789" class="w-full p-3 border rounded-xl outline-none focus:ring-2 focus:ring-brand-600 transition">
          </div>
          <div>
            <label class="block font-semibold mb-1.5 text-slate-700">No Resi</label>
            <input type="text" id="custResi" placeholder="Contoh: JNT123456" class="w-full p-3 border rounded-xl outline-none focus:ring-2 focus:ring-brand-600 transition">
          </div>
          <div class="md:col-span-3">
            <label class="block font-semibold mb-1.5 text-slate-700">Alamat Rumah Customer*</label>
            <textarea id="custAddress" required rows="2" placeholder="Alamat lengkap rumah customer" class="w-full p-3 border rounded-xl outline-none focus:ring-2 focus:ring-brand-600 transition resize-none"></textarea>
          </div>
        </div>

        <!-- LIST PRODUK & SIZE DENGAN TOMBOL SET SIZE XS-XXL -->
        <div class="bg-slate-50 p-5 rounded-2xl border border-slate-200 shadow-inner space-y-3">
          <div class="flex justify-between items-center pb-2 border-b mb-1">
            <h4 class="font-bold text-slate-800 flex items-center gap-2"><i class="fas fa-box text-brand-600"></i> Detail Produk & Size XS-XXL</h4>
            <button type="button" onclick="addProductRow()" class="text-xs bg-brand-700 text-white px-3 py-1.5 rounded-lg hover:bg-brand-800 transition shadow"><i class="fas fa-plus mr-1"></i> Tambah Baris Produk</button>
          </div>
          <div id="productRowsContainer" class="space-y-3">
            <!-- Row Items dynamic -->
          </div>
        </div>

        <!-- ESTIMASI READY, DISKON & DELIVERY -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4 p-1">
          <div>
            <label class="block font-semibold mb-1.5 text-slate-700">Estimasi Ready</label>
            <input type="date" id="estReady" class="w-full p-3 border rounded-xl outline-none focus:ring-1">
          </div>
          <div>
            <label class="block font-semibold mb-1.5 text-slate-700">Diskon (Rp)</label>
            <input type="number" id="orderDiscount" value="0" oninput="calculateFormTotals()" class="w-full p-3 border rounded-xl outline-none focus:ring-1 text-red-600 font-medium bg-red-50">
          </div>
          <div>
            <label class="block font-semibold mb-1.5 text-slate-700">Delivery / Ongkir (Rp)</label>
            <input type="number" id="orderDelivery" value="0" oninput="calculateFormTotals()" class="w-full p-3 border rounded-xl outline-none focus:ring-1 font-medium bg-brand-50">
          </div>
        </div>

        <!-- 3 KALI TAHAP PENAGIHAN DP & TANGGAL DP -->
        <div class="bg-amber-50 p-5 rounded-2xl border border-amber-200 shadow-inner space-y-3.5">
          <p class="font-bold text-amber-950 flex items-center gap-2"><i class="fas fa-calendar-alt text-amber-600 text-lg"></i> Riwayat DP & Tanggal (3 Kali Penagihan)</p>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
            <div class="bg-white p-3.5 rounded-xl border border-amber-100 shadow-sm space-y-2">
              <span class="block text-[10px] font-extrabold text-amber-800 uppercase tracking-wider mb-1">PENAGIHAN DP 1</span>
              <input type="date" id="dp1Date" class="w-full p-1.5 border border-amber-200 rounded-lg text-xs outline-none">
              <input type="number" id="dp1Amount" placeholder="Nominal Rp" value="0" oninput="calculateFormTotals()" class="w-full p-1.5 border border-amber-200 rounded-lg text-xs outline-none font-semibold text-brand-800 bg-brand-50/50">
            </div>
            <div class="bg-white p-3.5 rounded-xl border border-amber-100 shadow-sm space-y-2">
              <span class="block text-[10px] font-extrabold text-amber-800 uppercase tracking-wider mb-1">PENAGIHAN DP 2</span>
              <input type="date" id="dp2Date" class="w-full p-1.5 border border-amber-200 rounded-lg text-xs outline-none">
              <input type="number" id="dp2Amount" placeholder="Nominal Rp" value="0" oninput="calculateFormTotals()" class="w-full p-1.5 border border-amber-200 rounded-lg text-xs outline-none font-semibold text-brand-800 bg-brand-50/50">
            </div>
            <div class="bg-white p-3.5 rounded-xl border border-amber-100 shadow-sm space-y-2">
              <span class="block text-[10px] font-extrabold text-amber-800 uppercase tracking-wider mb-1">PENAGIHAN DP 3</span>
              <input type="date" id="dp3Date" class="w-full p-1.5 border border-amber-200 rounded-lg text-xs outline-none">
              <input type="number" id="dp3Amount" placeholder="Nominal Rp" value="0" oninput="calculateFormTotals()" class="w-full p-1.5 border border-amber-200 rounded-lg text-xs outline-none font-semibold text-brand-800 bg-brand-50/50">
            </div>
          </div>
        </div>

        <!-- REKENING PEMBAYARAN & STATUS -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div>
            <label class="block font-semibold mb-1.5 text-slate-700">Pembayaran (Rekening)*</label>
            <select id="orderBank" required class="w-full p-3 border border-slate-300 rounded-xl outline-none focus:ring-1 focus:ring-brand-600 transition font-medium">
              <!-- Rendered dynamic -->
            </select>
          </div>
          <div>
            <label class="block font-semibold mb-1.5 text-slate-700">Status Barang Pesanan*</label>
            <select id="orderStatus" required class="w-full p-3 border border-slate-300 rounded-xl outline-none focus:ring-1 focus:ring-brand-600 transition font-bold text-brand-800">
              <option value="Proses PO">Dalam Proses PO</option>
              <option value="Mau Ready">Mau Ready (Penagihan Pelunasan)</option>
              <option value="Ready">Barang Ready</option>
              <option value="Terkirim">Sudah Terkirim</option>
            </select>
          </div>
        </div>

        <!-- RINGKASAN SUB-TOTAL & SISA TAGIHAN (DINAMIS) -->
        <div class="bg-brand-50 p-5 rounded-2xl border-l-4 border-brand-700 shadow-inner flex justify-between items-center font-bold text-slate-900 border border-brand-200">
          <div>Total Pesanan: <span id="formSubTotal" class="text-xl">Rp 0</span></div>
          <div class="text-center">Total DP: <span id="formTotalDP" class="text-teal-700 text-xl">Rp 0</span></div>
          <div class="text-right">Sisa Tagihan: <span id="formSisaDP" class="text-red-600 text-2xl font-black bg-red-100 px-3 py-1 rounded-lg">Rp 0</span></div>
        </div>

        <div class="flex justify-end gap-3 pt-4 border-t sticky bottom-0 bg-white z-10 -mb-2 pb-2">
          <button type="button" onclick="closeModal('orderModal')" class="px-5 py-3 text-slate-600 hover:bg-slate-100 rounded-xl transition">Batal / Tutup</button>
          <button type="submit" class="px-6 py-3 bg-brand-700 hover:bg-brand-800 text-white font-bold rounded-xl shadow transition transform hover:scale-[1.02]"><i class="fas fa-save mr-1.5"></i> Simpan Transaksi Orderan</button>
        </div>
      </form>
    </div>
  </div>

  <!-- MODAL SET SIZE ( XS, S, M, L, XL, XXL) -->
  <div id="sizeModal" class="fixed inset-0 bg-black/70 backdrop-blur-sm hidden z-50 flex items-center justify-center p-4 transition-opacity duration-300">
    <div class="bg-white rounded-2xl max-w-sm w-full p-6 shadow-2xl transform transition-all duration-300 scale-95 opacity-0 modal-content space-y-4">
      <div class="flex justify-between items-center border-b pb-2.5">
        <h4 class="font-bold text-slate-800 flex items-center gap-1.5"><i class="fas fa-ruler-combined text-brand-700"></i> Set Quantity Per Size (XS-XXL)</h4>
        <button onclick="closeSizeModal()" class="text-slate-400 hover:text-slate-600 transition"><i class="fas fa-times"></i></button>
      </div>

      <p id="sizeModalProdName" class="text-xs font-semibold text-brand-900 bg-brand-50 p-2 rounded-lg"></p>
      
      <div class="grid grid-cols-3 gap-2 text-center" id="sizeInputContainer">
        <!-- Rendered dynamically XS-XXL -->
      </div>

      <div class="border-t pt-3 flex justify-between items-center font-bold text-sm bg-slate-50 p-2 rounded-lg">
        <span>Total Qty:</span>
        <span id="sizeModalTotalQty" class="text-brand-800 text-lg">0</span>
      </div>

      <button onclick="confirmSizeQty()" class="w-full bg-brand-700 hover:bg-brand-800 text-white font-semibold text-sm py-2.5 rounded-xl shadow transition">Konfirmasi Quantity</button>
    </div>
  </div>

  <script>
    // INITIAL DATABASE & LOCAL STORAGE KEYS
    const STORAGE_LOGO = 'geraihabani_logo_v3';
    const STORAGE_BANKS = 'geraihabani_banks_v3';
    const STORAGE_PRODUCTS = 'geraihabani_products_v3';
    const STORAGE_ORDERS = 'geraihabani_orders_v3';

    let logoUrl = localStorage.getItem(STORAGE_LOGO) || 'https://via.placeholder.com/400x400/047857/ffffff?text=GERAI+HABANI';
    
    let bankAccounts = JSON.parse(localStorage.getItem(STORAGE_BANKS)) || [
      { id: 1, bank: 'BCA', number: '1234567890' },
      { id: 2, bank: 'MANDIRI', number: '124567890' }
    ];

    let productsMaster = JSON.parse(localStorage.getItem(STORAGE_PRODUCTS)) || [
      { id: 1, name: 'Gamis Habani Premium', price: 250000, stock: 12 },
      { id: 2, name: 'Koko Habani Executive', price: 180000, stock: 5 },
      { id: 3, name: 'Hijab Pashmina Silk', price: 65000, stock: 25 }
    ];

    let orders = JSON.parse(localStorage.getItem(STORAGE_ORDERS)) || [];

    const SIZES = ['XS', 'S', 'M', 'L', 'XL', 'XXL'];
    let currentActiveSizeRowId = null;
    let currentFilter = 'all';

    // INIT APPLICATION
    window.onload = function() {
      applyLogoUrl(logoUrl);
      renderBankList();
      renderStockList();
      populateCustomerSuggestions();
      renderOrders();
    };

    // LOGIN & LOGOUT
    function handleLogin(e) {
      e.preventDefault();
      document.getElementById('loginPage').classList.add('hidden');
      document.getElementById('dashboardPage').classList.remove('hidden');
    }

    function handleLogout() {
      document.getElementById('dashboardPage').classList.add('hidden');
      document.getElementById('loginPage').classList.remove('hidden');
    }

    function togglePassword() {
      const pwd = document.getElementById('password');
      const eye = document.getElementById('eyeIcon');
      if (pwd.type === 'password') {
        pwd.type = 'text';
        eye.classList.replace('fa-eye', 'fa-eye-slash');
      } else {
        pwd.type = 'password';
        eye.classList.replace('fa-eye-slash', 'fa-eye');
      }
    }

    // LOGO CUSTOMIZATION
    function openLogoModal() { openModal('logoModal'); }
    
    function saveCustomLogo() {
      const file = document.getElementById('logoFileInput').files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function(e) {
          logoUrl = e.target.result;
          localStorage.setItem(STORAGE_LOGO, logoUrl);
          applyLogoUrl(logoUrl);
          closeModal('logoModal');
        };
        reader.readAsDataURL(file);
      }
    }

    function applyLogoUrl(url) {
      document.getElementById('loginLogo').src = url;
      document.getElementById('navLogo').src = url;
      document.getElementById('appWatermark').src = url;
    }

    // REKENING MANAGEMENT
    function openBankModal() { renderBankList(); openModal('bankModal'); }

    function renderBankList() {
      const container = document.getElementById('bankListContainer');
      container.innerHTML = bankAccounts.map((b, i) => `
        <div class="flex justify-between items-center p-2.5 bg-slate-50 border rounded-xl text-xs group">
          <div><span class="font-bold text-slate-800 text-sm">${b.bank}</span>: <span class="text-slate-600">${b.number}</span></div>
          <button onclick="deleteBank(${i})" class="text-red-500 hover:text-red-700 opacity-0 group-hover:opacity-100 transition p-1"><i class="fas fa-trash-alt"></i></button>
        </div>
      `).join('');
      localStorage.setItem(STORAGE_BANKS, JSON.stringify(bankAccounts));
      populateBankDropdown();
    }

    function addBank() {
      const name = document.getElementById('newBankName').value.trim();
      const no = document.getElementById('newBankNo').value.trim();
      if(name && no) {
        bankAccounts.push({ id: Date.now(), bank: name.toUpperCase(), number: no });
        document.getElementById('newBankName').value = '';
        document.getElementById('newBankNo').value = '';
        renderBankList();
      }
    }

    function deleteBank(index) {
      bankAccounts.splice(index, 1);
      renderBankList();
    }

    function populateBankDropdown() {
      const select = document.getElementById('orderBank');
      select.innerHTML = '<option value="">-- Pilih Rekening Pembayaran --</option>' + bankAccounts.map(b => `<option value="${b.bank} - ${b.number}">${b.bank} (${b.number})</option>`).join('');
    }

    // MASTER STOK PRODUK MANAGEMENT
    function openStockModal() { renderStockList(); openModal('stockModal'); }

    function renderStockList() {
      const container = document.getElementById('stockListContainer');
      container.innerHTML = productsMaster.map((p, i) => `
        <div class="flex justify-between items-center p-3 bg-slate-50 border rounded-xl text-xs group">
          <div>
            <p class="font-bold text-slate-800 text-sm">${p.name}</p>
            <p class="text-[10px] text-teal-800 font-semibold">Harga: Rp ${p.price.toLocaleString()} | Total Stok: <span class="font-extrabold ${p.stock < 5 ? 'text-red-600':'text-emerald-600'}">${p.stock}</span></p>
          </div>
          <button onclick="deleteProductMaster(${i})" class="text-red-500 hover:text-red-700 opacity-0 group-hover:opacity-100 transition p-1"><i class="fas fa-trash-alt"></i></button>
        </div>
      `).join('');
      localStorage.setItem(STORAGE_PRODUCTS, JSON.stringify(productsMaster));
      populateProductSuggestionsMaster();
    }

    function addProductMaster() {
      const name = document.getElementById('newProdName').value.trim();
      const price = parseFloat(document.getElementById('newProdPrice').value) || 0;
      const stock = parseInt(document.getElementById('newProdStock').value) || 0;
      if(name) {
        productsMaster.push({ id: Date.now(), name, price, stock });
        document.getElementById('newProdName').value = '';
        document.getElementById('newProdPrice').value = '';
        document.getElementById('newProdStock').value = '';
        renderStockList();
      }
    }

    function deleteProductMaster(index) {
      productsMaster.splice(index, 1);
      renderStockList();
    }

    // AUTOFILL CUSTOMER & PRODUK SUGGESTIONS
    function populateCustomerSuggestions() {
      const datalist = document.getElementById('customerSuggestions');
      const uniqueNames = [...new Set(orders.map(o => o.custName).filter(Boolean))];
      datalist.innerHTML = uniqueNames.map(name => `<option value="${name}">`).join('');
    }

    function autoFillCustomer() {
      const name = document.getElementById('custName').value.trim();
      if(!name) return;
      const existing = orders.find(o => o.custName && o.custName.toLowerCase() === name.toLowerCase());
      if(existing) {
        document.getElementById('custPhone').value = existing.custPhone || '';
        document.getElementById('custAddress').value = existing.custAddress || '';
      }
    }

    function populateProductSuggestionsMaster() {
      const datalist = document.getElementById('productSuggestionsMaster');
      if(!datalist) return;
      datalist.innerHTML = productsMaster.map(p => `<option value="${p.name}" data-price="${p.price}" data-stock="${p.stock}">Harga: Rp ${p.price.toLocaleString()} | Stok: ${p.stock}</option>`).join('');
    }

    // MULTI PRODUCT DYNAMIC ROWS IN FORM (WITH SIZE FEATURE)
    function addProductRow(prodName = '', sizeQty = {}, price = 0) {
      const container = document.getElementById('productRowsContainer');
      const rowId = `row-${Date.now()}-${Math.random().toString(16).slice(2)}`;
      
      const div = document.createElement('div');
      div.className = "flex flex-col md:flex-row gap-3 items-start md:items-center bg-white p-4 rounded-xl border border-slate-200 product-row shadow-sm relative";
      div.id = rowId;
      div.setAttribute('data-size-qty', JSON.stringify(sizeQty));
      
      div.innerHTML = `
        <div class="flex-1 w-full space-y-1">
          <input type="text" placeholder="Nama Produk..." value="${prodName}" list="productSuggestionsMaster" oninput="updateRowPriceFromMaster(this)" class="prod-select w-full p-2 border border-slate-200 rounded-lg text-xs outline-none focus:ring-1 focus:ring-teal-500 font-medium bg-white">
          <p class="text-[9px] text-slate-500 stock-warning px-1 hidden"></p>
        </div>
        
        <!-- BAGIAN SIZE XS-XXL -->
        <div class="flex flex-col gap-1 w-full md:w-auto min-w-[140px]">
          <button type="button" onclick="openSizeModal('${rowId}')" class="text-xs font-bold bg-brand-50 hover:bg-brand-100 text-brand-800 px-3 py-2 rounded-lg border border-brand-200 flex items-center justify-between gap-1">
            <span>Set Size (XS-XXL)</span>
            <i class="fas fa-edit text-[10px]"></i>
          </button>
          <span class="prod-size-summary text-[10px] text-slate-600 italic px-1 truncate max-w-[160px]" title="">Belum Set Size</span>
        </div>

        <div class="flex gap-2 w-full md:w-auto items-center justify-between">
          <input type="number" placeholder="Harga" value="${price}" oninput="calculateFormTotals()" class="prod-price w-28 p-2 border border-slate-200 rounded-lg text-xs outline-none focus:ring-1 text-teal-800 font-semibold text-right">
          
          <div class="flex items-center gap-1 font-bold text-slate-700">
            <span class="text-xs">Qty:</span>
            <span class="prod-qty text-base w-10 text-center">${calculateTotalQty(sizeQty)}</span>
          </div>

          <button type="button" onclick="removeProductRow('${rowId}')" class="text-red-400 hover:text-red-700 transition p-2"><i class="fas fa-trash-alt"></i></button>
        </div>
      `;
      container.appendChild(div);
      updateSizeSummaryForRow(rowId);
      calculateFormTotals();
    }

    function removeProductRow(rowId) {
      document.getElementById(rowId).remove();
      calculateFormTotals();
    }

    function calculateTotalQty(sizeQty) {
      let total = 0;
      SIZES.forEach(s => total += (sizeQty[s] || 0));
      return total;
    }

    function updateRowPriceFromMaster(inputEl) {
      const name = inputEl.value.trim();
      const row = inputEl.closest('.product-row');
      const stockWarnEl = row.querySelector('.stock-warning');
      const priceInput = row.querySelector('.prod-price');
      
      stockWarnEl.classList.add('hidden');
      if(!name) return;

      const master = productsMaster.find(p => p.name.toLowerCase() === name.toLowerCase());
      if(master) {
        priceInput.value = master.price;
        checkStockForRow(row.id);
      }
      calculateFormTotals();
    }

    function calculateFormTotals() {
      let grandTotal = 0;
      document.querySelectorAll('.product-row').forEach(row => {
        const p = parseFloat(row.querySelector('.prod-price').value) || 0;
        const sizeQty = JSON.parse(row.getAttribute('data-size-qty') || '{}');
        grandTotal += (p * calculateTotalQty(sizeQty));
      });

      const discount = parseFloat(document.getElementById('orderDiscount').value) || 0;
      const delivery = parseFloat(document.getElementById('orderDelivery').value) || 0;

      const dp1 = parseFloat(document.getElementById('dp1Amount').value) || 0;
      const dp2 = parseFloat(document.getElementById('dp2Amount').value) || 0;
      const dp3 = parseFloat(document.getElementById('dp3Amount').value) || 0;

      const totalDP = dp1 + dp2 + dp3;
      const totalBill = grandTotal - discount + delivery;
      const sisaDP = totalBill - totalDP;

      document.getElementById('formSubTotal').innerText = `Rp ${totalBill.toLocaleString()}`;
      document.getElementById('formTotalDP').innerText = `Rp ${totalDP.toLocaleString()}`;
      document.getElementById('formSisaDP').innerText = `Rp ${sisaDP.toLocaleString()}`;
    }

    // MODAL SET SIZE LOGIC
    function openSizeModal(rowId) {
      currentActiveSizeRowId = rowId;
      const row = document.getElementById(rowId);
      const prodName = row.querySelector('.prod-select').value.trim() || 'Produk Tanpa Nama';
      const sizeQtyData = JSON.parse(row.getAttribute('data-size-qty') || '{}');

      document.getElementById('sizeModalProdName').innerText = `Set ukuran untuk: ${prodName}`;
      
      const container = document.getElementById('sizeInputContainer');
      container.innerHTML = SIZES.map(s => `
        <div class="bg-slate-50 p-2 rounded-lg border">
          <label class="block text-[10px] font-bold text-slate-500 mb-0.5">${s}</label>
          <input type="number" data-size="${s}" value="${sizeQtyData[s] || ''}" min="0" oninput="calculateModalTotalQty()" class="size-input w-full p-1.5 border border-slate-200 rounded text-center text-xs outline-none focus:ring-1 font-bold text-brand-800">
        </div>
      `).join('');

      calculateModalTotalQty();
      openModal('sizeModal');
    }

    function closeSizeModal() {
      closeModal('sizeModal');
      currentActiveSizeRowId = null;
    }

    function calculateModalTotalQty() {
      let total = 0;
      document.querySelectorAll('#sizeInputContainer .size-input').forEach(input => {
        total += (parseInt(input.value) || 0);
      });
      document.getElementById('sizeModalTotalQty').innerText = total;
    }

    function confirmSizeQty() {
      if(!currentActiveSizeRowId) return;
      const row = document.getElementById(currentActiveSizeRowId);
      const newSizeQtyData = {};
      let totalQty = 0;

      document.querySelectorAll('#sizeInputContainer .size-input').forEach(input => {
        const size = input.getAttribute('data-size');
        const qty = (parseInt(input.value) || 0);
        if(qty > 0) newSizeQtyData[size] = qty;
        totalQty += qty;
      });

      row.setAttribute('data-size-qty', JSON.stringify(newSizeQtyData));
      row.querySelector('.prod-qty').innerText = totalQty;
      updateSizeSummaryForRow(currentActiveSizeRowId);
      
      calculateFormTotals();
      checkStockForRow(currentActiveSizeRowId);
      closeSizeModal();
    }

    function updateSizeSummaryForRow(rowId) {
      const row = document.getElementById(rowId);
      const sizeQtyData = JSON.parse(row.getAttribute('data-size-qty') || '{}');
      const summaryEl = row.querySelector('.prod-size-summary');

      let text = '';
      let parts = [];
      SIZES.forEach(s => {
        if(sizeQtyData[s] > 0) parts.push(`${s}:${sizeQtyData[s]}`);
      });

      if(parts.length > 0) {
        text = `Set: ${parts.join(', ')}`;
        summaryEl.setAttribute('title', text);
      } else {
        text = 'Belum Set Size';
        summaryEl.setAttribute('title', '');
      }
      summaryEl.innerText = text;
    }

    function checkStockForRow(rowId) {
      const row = document.getElementById(rowId);
      const masterName = row.querySelector('.prod-select').value.trim();
      const stockWarnEl = row.querySelector('.stock-warning');
      const qty = parseInt(row.querySelector('.prod-qty').innerText) || 0;

      if(!masterName || qty === 0) return;

      const master = productsMaster.find(p => p.name.toLowerCase() === masterName.toLowerCase());
      if(master) {
        stockWarnEl.classList.remove('hidden');
        if(qty > master.stock) {
          stockWarnEl.innerHTML = `<i class="fas fa-exclamation-triangle mr-1"></i> Stok tidak cukup! Master <b class="text-red-700">${master.stock}</b>.`;
          stockWarnEl.className = 'text-[9px] stock-warning px-1 text-red-600 font-medium';
        } else {
          stockWarnEl.innerHTML = `<i class="fas fa-check-circle mr-1"></i> Stok Aman. Sisa Master <b class="text-emerald-800">${master.stock - qty}</b>.`;
          stockWarnEl.className = 'text-[9px] stock-warning px-1 text-emerald-600';
        }
      } else {
        stockWarnEl.classList.add('hidden');
      }
    }

    // ORDER CRUD
    function openOrderModal(editIdx = -1) {
      document.getElementById('editIndex').value = editIdx;
      const form = document.getElementById('orderForm');
      form.reset();
      document.getElementById('productRowsContainer').innerHTML = '';
      populateBankDropdown();
      populateCustomerSuggestions();

      const modalTitle = document.getElementById('orderModalTitle');

      if(editIdx >= 0 && orders[editIdx]) {
        const o = orders[editIdx];
        modalTitle.innerHTML = `<i class="fas fa-edit text-amber-600 mr-2"></i> Edit Transaksi Orderan`;
        
        document.getElementById('custName').value = o.custName || '';
        document.getElementById('custPhone').value = o.custPhone || '';
        document.getElementById('custAddress').value = o.custAddress || '';
        document.getElementById('custResi').value = o.custResi || '';
        document.getElementById('estReady').value = o.estReady || '';
        document.getElementById('orderDiscount').value = o.discount || 0;
        document.getElementById('orderDelivery').value = o.delivery || 0;

        document.getElementById('dp1Date').value = o.dp1Date || '';
        document.getElementById('dp1Amount').value = o.dp1Amount || 0;
        document.getElementById('dp2Date').value = o.dp2Date || '';
        document.getElementById('dp2Amount').value = o.dp2Amount || 0;
        document.getElementById('dp3Date').value = o.dp3Date || '';
        document.getElementById('dp3Amount').value = o.dp3Amount || 0;

        document.getElementById('orderBank').value = o.bank || '';
        document.getElementById('orderStatus').value = o.status || 'Proses PO';

        if(o.items && o.items.length > 0) {
          o.items.forEach(item => addProductRow(item.prodName, item.sizeQty, item.price));
        } else {
          addProductRow();
        }
      } else {
        modalTitle.innerHTML = `<i class="fas fa-cart-plus text-brand-600 mr-2"></i> Input Orderan Baru`;
        document.getElementById('editIndex').value = -1;
        addProductRow();
      }

      calculateFormTotals();
      openModal('orderModal');
    }

    function saveOrder(e) {
      e.preventDefault();
      const editIdx = parseInt(document.getElementById('editIndex').value);

      const items = [];

      document.querySelectorAll('.product-row').forEach(row => {
        const prodEl = row.querySelector('.prod-select');
        const prodName = prodEl ? prodEl.value.trim() : '';
        const price = parseFloat(row.querySelector('.prod-price').value) || 0;
        const sizeQty = JSON.parse(row.getAttribute('data-size-qty') || '{}');
        const qty = calculateTotalQty(sizeQty);
        
        if(prodName && qty > 0) {
          items.push({ prodName, sizeQty, price });
        }
      });

      if(items.length === 0) {
        alert("Pilih minimal 1 produk dan Set Size XS-XXL!");
        return;
      }

      const orderData = {
        id: editIdx >= 0 && orders[editIdx] ? orders[editIdx].id : Date.now(),
        custName: document.getElementById('custName').value.trim() || '-',
        custPhone: document.getElementById('custPhone').value.trim() || '-',
        custAddress: document.getElementById('custAddress').value.trim() || '-',
        custResi: document.getElementById('custResi').value.trim() || '',
        items: items,
        estReady: document.getElementById('estReady').value || '',
        discount: parseFloat(document.getElementById('orderDiscount').value) || 0,
        delivery: parseFloat(document.getElementById('orderDelivery').value) || 0,
        dp1Date: document.getElementById('dp1Date').value || '',
        dp1Amount: parseFloat(document.getElementById('dp1Amount').value) || 0,
        dp2Date: document.getElementById('dp2Date').value || '',
        dp2Amount: parseFloat(document.getElementById('dp2Amount').value) || 0,
        dp3Date: document.getElementById('dp3Date').value || '',
        dp3Amount: parseFloat(document.getElementById('dp3Amount').value) || 0,
        bank: document.getElementById('orderBank').value || '-',
        status: document.getElementById('orderStatus').value || 'Proses PO'
      };

      if(editIdx >= 0) {
        orders[editIdx] = orderData;
      } else {
        orders.unshift(orderData);
      }

      localStorage.setItem(STORAGE_ORDERS, JSON.stringify(orders));
      
      document.getElementById('searchInput').value = '';
      currentFilter = 'all';

      closeModal('orderModal');
      populateCustomerSuggestions();
      renderOrders();
    }

    function deleteOrder(index) {
      if(confirm("Yakin ingin menghapus data orderan ini?")) {
        orders.splice(index, 1);
        localStorage.setItem(STORAGE_ORDERS, JSON.stringify(orders));
        renderOrders();
      }
    }

    // RENDER MAIN TABLE
    function renderOrders() {
      const tbody = document.getElementById('orderTableBody');
      const search = document.getElementById('searchInput').value.toLowerCase().trim();

      let filtered = orders.filter(o => {
        const matchesSearch = (o.custName && o.custName.toLowerCase().includes(search)) || 
                              (o.custPhone && o.custPhone.toLowerCase().includes(search)) || 
                              (o.custResi && o.custResi.toLowerCase().includes(search)) ||
                              (o.items && o.items.some(i => i.prodName.toLowerCase().includes(search)));
                              
        const totalDP = (o.dp1Amount || 0) + (o.dp2Amount || 0) + (o.dp3Amount || 0);
        const totalHargaItem = (o.items || []).reduce((a, b) => a + (b.price * calculateTotalQty(b.sizeQty)), 0);
        const totalHargaBill = totalHargaItem - (o.discount || 0) + (o.delivery || 0);
        const sisaDP = totalHargaBill - totalDP;

        if(currentFilter === 'ready') return matchesSearch && (o.status === 'Ready');
        if(currentFilter === 'pending') return matchesSearch && (sisaDP > 0);
        return matchesSearch;
      });

      if(filtered.length === 0) {
        tbody.innerHTML = `<tr><td colspan="15" class="p-8 text-center text-slate-400 text-xs">Belum ada transaksi / orderan sesuai filter Gerai Habani.</td></tr>`;
        return;
      }

      tbody.innerHTML = filtered.map((o, idx) => {
        const totalHargaItem = (o.items || []).reduce((a, b) => a + (b.price * calculateTotalQty(b.sizeQty)), 0);
        const totalQty = (o.items || []).reduce((a, b) => a + calculateTotalQty(b.sizeQty), 0);
        const totalHargaBill = totalHargaItem - (o.discount || 0) + (o.delivery || 0);
        const totalDP = (o.dp1Amount || 0) + (o.dp2Amount || 0) + (o.dp3Amount || 0);
        const sisaDP = totalHargaBill - totalDP;

        const isReady = o.status === 'Ready';

        return `
          <tr class="hover:bg-slate-50 transition ${isReady ? 'bg-emerald-50/50':''}">
            <td class="p-4 text-center border-b font-medium text-slate-500">${idx + 1}</td>
            <td class="p-4 border-b font-semibold text-slate-900">${o.custName}</td>
            <td class="p-4 border-b text-slate-600 max-w-xs truncate" title="${o.custAddress}">${o.custAddress}</td>
            <td class="p-4 border-b">
              <a href="https://wa.me/${formatWA(o.custPhone)}" target="_blank" class="text-emerald-700 hover:text-emerald-900 hover:underline font-semibold flex items-center gap-1">
                <i class="fab fa-whatsapp"></i> ${o.custPhone}
              </a>
            </td>
            <td class="p-4 border-b text-slate-600 font-medium">${o.custResi || '<i class="text-slate-400">Belum Ada</i>'}</td>
            
            <td class="p-3 border-b space-y-1.5">
              ${(o.items || []).map(i => {
                let sizeParts = [];
                SIZES.forEach(s => { if(i.sizeQty[s] > 0) sizeParts.push(`${s}:${i.sizeQty[s]}`); });
                return `<div class="bg-white p-2 rounded-lg border text-[11px] font-medium text-slate-700">
                          • <span class="font-bold text-slate-900">${i.prodName}</span> 
                          <span class="text-amber-800 font-extrabold text-[10px]">[ ${sizeParts.join(', ') || 'N/S'} ]</span>
                        </div>`;
              }).join('')}
            </td>

            <td class="p-4 border-b text-slate-600">${o.estReady || '-'}</td>
            <td class="p-4 border-b font-semibold text-teal-800">Rp ${totalHargaBill.toLocaleString()}</td>
            <td class="p-3 border-b text-[10px] space-y-0.5 bg-brand-50/20">
              ${o.dp1Amount ? `<div>DP1 (${o.dp1Date}): <b>Rp${o.dp1Amount.toLocaleString()}</b></div>`:''}
              ${o.dp2Amount ? `<div>DP2 (${o.dp2Date}): <b>Rp${o.dp2Amount.toLocaleString()}</b></div>`:''}
              ${o.dp3Amount ? `<div>DP3 (${o.dp3Date}): <b>Rp${o.dp3Amount.toLocaleString()}</b></div>`:''}
              ${!totalDP ? '<span class="text-slate-400">Belum Ada DP</span>':''}
            </td>
            <td class="p-4 border-b font-bold ${sisaDP > 0 ? 'text-red-600':'text-emerald-600'}">
              Rp ${sisaDP.toLocaleString()}
            </td>
            <td class="p-4 border-b text-red-600 font-medium">Rp ${(o.discount || 0).toLocaleString()}</td>
            <td class="p-4 border-b text-center font-bold text-slate-700">${totalQty}</td>
            <td class="p-4 border-b text-brand-800 font-medium">Rp ${(o.delivery || 0).toLocaleString()}</td>
            <td class="p-4 border-b text-[10px] text-brand-900 font-semibold bg-brand-50/40">${o.bank}</td>
            <td class="p-4 border-b text-center space-x-1 sticky right-0 bg-white group-hover:bg-slate-50 ${isReady ? 'bg-emerald-50 group-hover:bg-emerald-100':''}">
              <button onclick="sendWABilling(${orders.indexOf(o)})" title="Kirim WhatsApp Penagihan" class="bg-emerald-600 hover:bg-emerald-700 text-white p-2 rounded-xl transition shadow"><i class="fab fa-whatsapp"></i></button>
              <button onclick="openOrderModal(${orders.indexOf(o)})" title="Edit Data Order" class="bg-amber-500 hover:bg-amber-600 text-white p-2 rounded-xl transition shadow"><i class="fas fa-edit"></i></button>
              <button onclick="deleteOrder(${orders.indexOf(o)})" title="Hapus Data Order" class="bg-red-500 hover:bg-red-600 text-white p-2 rounded-xl transition shadow"><i class="fas fa-trash-alt"></i></button>
            </td>
          </tr>
        `;
      }).join('');
    }

    function filterStatus(status) {
      currentFilter = status;
      renderOrders();
    }

    // WA BILLING TEXT GENERATOR
    function sendWABilling(index) {
      const o = orders[index];
      if(!o) return;
      
      const totalHargaItem = (o.items || []).reduce((a, b) => a + (b.price * calculateTotalQty(b.sizeQty)), 0);
      const totalHargaBill = totalHargaItem - (o.discount || 0) + (o.delivery || 0);
      const totalDP = (o.dp1Amount || 0) + (o.dp2Amount || 0) + (o.dp3Amount || 0);
      const sisaDP = totalHargaBill - totalDP;

      let msg = `Halo Kak *${o.custName}*,\nBerikut rincian orderan Kakak di *Gerai Habani*:\n\n`;
      msg += `*Rincian Pesanan:*\n`;
      (o.items || []).forEach(i => {
        let sizeParts = [];
        SIZES.forEach(s => { if(i.sizeQty[s] > 0) sizeParts.push(`${s}:${i.sizeQty[s]}`); });
        msg += `- ${i.prodName} *[ ${sizeParts.join(', ') || 'No Size'} ]* (${calculateTotalQty(i.sizeQty)}x)\n`;
      });
      if(o.estReady) msg += `Estimasi Ready: *${o.estReady}*\n`;
      msg += `\n*Detail Hitungan:*\n`;
      msg += `Total Harga Barang: Rp ${totalHargaItem.toLocaleString()}\n`;
      if(o.discount) msg += `Potongan Diskon: -Rp ${o.discount.toLocaleString()}\n`;
      if(o.delivery) msg += `Biaya Delivery/Ongkir: +Rp ${o.delivery.toLocaleString()}\n`;
      msg += `*TOTAL TAGIHAN:* Rp ${totalHargaBill.toLocaleString()}\n\n`;

      msg += `*Riwayat Pembayaran DP:*\n`;
      if(o.dp1Amount) msg += `- DP 1 (${o.dp1Date}): Rp ${o.dp1Amount.toLocaleString()}\n`;
      if(o.dp2Amount) msg += `- DP 2 (${o.dp2Date}): Rp ${o.dp2Amount.toLocaleString()}\n`;
      if(o.dp3Amount) msg += `- DP 3 (${o.dp3Date}): Rp ${o.dp3Amount.toLocaleString()}\n`;
      if(!totalDP) msg += `- Belum ada DP masuk.\n`;
      
      msg += `*TOTAL DP MASUK:* Rp ${totalDP.toLocaleString()}\n`;
      msg += `*SISA PELUNASAN:* *Rp ${sisaDP.toLocaleString()}*\n\n`;

      if(sisaDP > 0) {
        msg += `Pembayaran sisa tagihan dapat ditransfer via rekening:\n*${o.bank}*\n\nTerima kasih banyak! 🙏🏼`;
      } else {
        msg += `Orderan Kakak sudah LUNAS. Terima kasih banyak! 🙏🏼`;
      }

      window.open(`https://wa.me/${formatWA(o.custPhone)}?text=${encodeURIComponent(msg)}`, '_blank');
    }

    function formatWA(phone) {
      if(!phone) return '';
      let cleaned = phone.replace(/\D/g, '');
      if(cleaned.startsWith('0')) cleaned = '62' + cleaned.substring(1);
      return cleaned;
    }

    // MODAL HELPERS
    function openModal(id) {
      const modal = document.getElementById(id);
      modal.classList.remove('hidden');
      setTimeout(() => {
        const content = modal.querySelector('.modal-content');
        if(content) {
          content.classList.remove('scale-95', 'opacity-0');
          content.classList.add('scale-100', 'opacity-100');
        }
      }, 10);
    }

    function closeModal(id) {
      const modal = document.getElementById(id);
      const content = modal.querySelector('.modal-content');
      if(content) {
        content.classList.remove('scale-100', 'opacity-100');
        content.classList.add('scale-95', 'opacity-0');
      }
      setTimeout(() => {
        modal.classList.add('hidden');
      }, 300);
    }
  </script>
  
  <datalist id="productSuggestionsMaster"></datalist>

</body>
</html>
