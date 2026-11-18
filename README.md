<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Growblox Market Price Tracker</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0fdf4',
                            100: '#dcfce7',
                            500: '#22c55e',
                            600: '#16a34a',
                            700: '#15803d',
                        },
                        darkbg: '#0f172a',
                        darkcard: '#1e293b',
                        darkborder: '#334155'
                    }
                }
            }
        }
    </script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap');
        body { font-family: 'Inter', sans-serif; }
    </style>
</head>
<body class="bg-slate-50 dark:bg-darkbg text-slate-800 dark:text-slate-100 min-h-screen flex flex-col transition-colors duration-300">

    <!-- Navbar -->
    <header class="sticky top-0 z-40 backdrop-blur-md bg-white/80 dark:bg-darkcard/80 border-b border-slate-200 dark:border-darkborder">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-brand-600 to-emerald-400 flex items-center justify-center text-white font-black text-xl shadow-md shadow-brand-500/20">
                    G
                </div>
                <div>
                    <h1 class="font-extrabold text-base sm:text-lg tracking-tight">Growblox Market</h1>
                    <p class="text-xs text-slate-500 dark:text-slate-400">Live Price Tracker & Calculator</p>
                </div>
            </div>

            <div class="flex items-center gap-2.5">
                <!-- Tombol Ganti Bahasa -->
                <button onclick="toggleLanguage()" class="px-2.5 py-2 rounded-xl bg-slate-100 dark:bg-slate-800 hover:bg-slate-200 dark:hover:bg-slate-700 text-xs font-bold transition flex items-center gap-1" aria-label="Toggle Language">
                    <span id="langLabel">ID</span>
                </button>
                <button onclick="toggleEditMode()" id="editModeBtn" class="px-3 py-2 rounded-xl bg-slate-100 dark:bg-slate-800 hover:bg-slate-200 dark:hover:bg-slate-700 text-xs font-semibold flex items-center gap-1.5 transition">
                    <i data-lucide="settings" class="w-4 h-4 text-brand-500"></i>
                    <span id="txtEditMode">Atur Harga</span>
                </button>
                <button onclick="openItemForm()" id="addItemBtn" class="hidden px-3 py-2 rounded-xl bg-brand-600 hover:bg-brand-700 text-white text-xs font-semibold flex items-center gap-1.5 shadow-sm transition">
                    <i data-lucide="plus" class="w-4 h-4"></i> <span id="txtAddItem">Tambah Item</span>
                </button>
                <button onclick="toggleTheme()" class="p-2 rounded-xl bg-slate-100 dark:bg-slate-800 hover:bg-slate-200 dark:hover:bg-slate-700 transition" aria-label="Toggle Theme">
                    <i data-lucide="sun" class="w-5 h-5 hidden dark:block text-amber-400"></i>
                    <i data-lucide="moon" class="w-5 h-5 block dark:hidden text-slate-600"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="flex-grow max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 space-y-6">

        <!-- 4 Kartu Statistik Pasar (Rotasi Otomatis Bergantian) -->
        <section class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
            <!-- Card 1: Kurs WL per Rupiah (Bisa Diatur) -->
            <div class="p-5 rounded-2xl bg-white dark:bg-darkcard border border-slate-200 dark:border-darkborder shadow-sm flex flex-col justify-between">
                <div class="flex items-center justify-between text-slate-500 dark:text-slate-400 text-xs font-medium">
                    <span id="txtCard1Title">Kurs 1 WL / Rupiah</span>
                    <button onclick="openRateModal()" class="inline-flex items-center gap-1 px-2 py-0.5 rounded-lg bg-blue-500/10 text-blue-600 dark:text-blue-400 hover:bg-blue-500/20 font-semibold transition">
                        <i data-lucide="pencil" class="w-3 h-3"></i> <span id="txtSet">Atur</span>
                    </button>
                </div>
                <div class="mt-2 flex items-baseline gap-2">
                    <span id="statWlRupiah" class="text-2xl font-extrabold text-slate-900 dark:text-white">Rp 150</span>
                    <span class="text-xs text-slate-400">/ 1 WL</span>
                </div>
                <div id="statDlRupiah" class="mt-1 text-xs text-emerald-600 dark:text-emerald-400 flex items-center gap-1">
                    <i data-lucide="banknote" class="w-3.5 h-3.5"></i> 1 DL = Rp 15.000
                </div>
            </div>

            <!-- Card 2: Farmable (Rotasi Bergantian) -->
            <div class="p-5 rounded-2xl bg-white dark:bg-darkcard border border-slate-200 dark:border-darkborder shadow-sm flex flex-col justify-between">
                <div class="flex items-center justify-between text-slate-500 dark:text-slate-400 text-xs font-medium">
                    <span id="txtCard2Title">Featured Farmable</span>
                    <i data-lucide="sprout" class="w-4 h-4 text-emerald-500"></i>
                </div>
                <div class="mt-2">
                    <span id="statFarmableName" class="text-xl font-extrabold block truncate">-</span>
                </div>
                <div id="statFarmableSub" class="mt-1 text-xs text-emerald-600 dark:text-emerald-400 font-medium">
                    -
                </div>
            </div>

            <!-- Card 3: Block & Item (Rotasi Bergantian) -->
            <div class="p-5 rounded-2xl bg-white dark:bg-darkcard border border-slate-200 dark:border-darkborder shadow-sm flex flex-col justify-between">
                <div class="flex items-center justify-between text-slate-500 dark:text-slate-400 text-xs font-medium">
                    <span id="txtCard3Title">Featured Block & Item</span>
                    <i data-lucide="box" class="w-4 h-4 text-purple-500"></i>
                </div>
                <div class="mt-2">
                    <span id="statBlockItemName" class="text-xl font-extrabold block truncate">-</span>
                </div>
                <div id="statBlockItemSub" class="mt-1 text-xs text-purple-600 dark:text-purple-400 font-medium">
                    -
                </div>
            </div>

            <!-- Card 4: IOTM / Rare (Rotasi Bergantian) -->
            <div class="p-5 rounded-2xl bg-white dark:bg-darkcard border border-slate-200 dark:border-darkborder shadow-sm flex flex-col justify-between">
                <div class="flex items-center justify-between text-slate-500 dark:text-slate-400 text-xs font-medium">
                    <span id="txtCard4Title">Featured IOTM / Rare</span>
                    <i data-lucide="crown" class="w-4 h-4 text-amber-500"></i>
                </div>
                <div class="mt-2">
                    <span id="statIotmName" class="text-xl font-extrabold block truncate">-</span>
                </div>
                <div id="statIotmSub" class="mt-1 text-xs text-amber-600 dark:text-amber-400 font-medium">
                    -
                </div>
            </div>
        </section>

        <!-- Search, Filter & Tombol Atur -->
        <section class="flex flex-col md:flex-row gap-3 justify-between items-stretch md:items-center">
            <div class="relative flex-grow max-w-md">
                <i data-lucide="search" class="w-4 h-4 text-slate-400 absolute left-3.5 top-1/2 -translate-y-1/2"></i>
                <input type="text" id="searchInput" oninput="renderTable()" placeholder="Cari item (misal: Magplant, Angel Wings...)" class="w-full pl-10 pr-4 py-2.5 rounded-xl bg-white dark:bg-darkcard border border-slate-200 dark:border-darkborder focus:outline-none focus:ring-2 focus:ring-brand-500 text-sm">
            </div>

            <!-- Kategori Filter -->
            <div class="flex items-center gap-1.5 overflow-x-auto pb-1 md:pb-0">
                <button onclick="filterCategory('all', this)" id="catAllBtn" class="category-btn px-4 py-2 rounded-xl text-xs font-semibold bg-brand-600 text-white shrink-0 transition">Semua</button>
                <button onclick="filterCategory('iotm', this)" id="catIotmBtn" class="category-btn px-4 py-2 rounded-xl text-xs font-semibold bg-white dark:bg-darkcard hover:bg-slate-100 dark:hover:bg-slate-800 border border-slate-200 dark:border-darkborder shrink-0 transition">IOTM / Rare</button>
                <button onclick="filterCategory('wings', this)" id="catWingsBtn" class="category-btn px-4 py-2 rounded-xl text-xs font-semibold bg-white dark:bg-darkcard hover:bg-slate-100 dark:hover:bg-slate-800 border border-slate-200 dark:border-darkborder shrink-0 transition">Wings</button>
                <button onclick="filterCategory('farmable', this)" id="catFarmableBtn" class="category-btn px-4 py-2 rounded-xl text-xs font-semibold bg-white dark:bg-darkcard hover:bg-slate-100 dark:hover:bg-slate-800 border border-slate-200 dark:border-darkborder shrink-0 transition">Farmables</button>
                <button onclick="filterCategory('blocks', this)" id="catBlocksBtn" class="category-btn px-4 py-2 rounded-xl text-xs font-semibold bg-white dark:bg-darkcard hover:bg-slate-100 dark:hover:bg-slate-800 border border-slate-200 dark:border-darkborder shrink-0 transition">Blocks & Item</button>
            </div>
        </section>

        <!-- Layout Utama: Tabel Harga & Kalkulator Trade -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
            <!-- Tabel Harga (2 Kolom) -->
            <div class="lg:col-span-2 bg-white dark:bg-darkcard rounded-2xl border border-slate-200 dark:border-darkborder shadow-sm overflow-hidden">
                <div class="p-4 sm:p-5 border-b border-slate-200 dark:border-darkborder flex flex-wrap items-center justify-between gap-2">
                    <div>
                        <h2 id="txtTableTitle" class="font-bold text-base">Daftar Harga Pasar Live</h2>
                        <p id="txtTableSubtitle" class="text-xs text-slate-500 dark:text-slate-400">Harga per item & rate per 1 World Lock</p>
                    </div>
                    <span class="text-xs px-2.5 py-1 rounded-lg bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 font-medium">1 DL = 100 WL</span>
                </div>

                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="border-b border-slate-200 dark:border-darkborder text-[11px] uppercase tracking-wider text-slate-500 dark:text-slate-400 bg-slate-50/50 dark:bg-slate-800/30">
                                <th id="thItemName" class="py-3.5 px-4 font-semibold">Nama Item</th>
                                <th id="thMarketPrice" class="py-3.5 px-4 font-semibold">Harga Pasar</th>
                                <th id="thAction" class="py-3.5 px-4 font-semibold text-right">Aksi</th>
                            </tr>
                        </thead>
                        <tbody id="marketTableBody" class="divide-y divide-slate-200 dark:divide-darkborder text-sm">
                            <!-- Injected by JS -->
                        </tbody>
                    </table>
                </div>

                <!-- Empty State -->
                <div id="emptyState" class="hidden p-12 text-center">
                    <i data-lucide="package-open" class="w-12 h-12 mx-auto text-slate-300 dark:text-slate-600 mb-3"></i>
                    <p id="txtEmptyTitle" class="font-semibold text-slate-600 dark:text-slate-400">Tidak ada item ditemukan</p>
                    <p id="txtEmptySub" class="text-xs text-slate-400 mt-1">Coba kata kunci lain atau ubah kategori</p>
                </div>
            </div>

            <!-- Kalkulator Trade (1 Kolom) -->
            <div class="bg-white dark:bg-darkcard rounded-2xl border border-slate-200 dark:border-darkborder shadow-sm p-5 flex flex-col justify-between">
                <div>
                    <div class="flex items-center justify-between pb-4 border-b border-slate-200 dark:border-darkborder">
                        <div class="flex items-center gap-2">
                            <div class="p-2 rounded-xl bg-brand-500/10 text-brand-600 dark:text-brand-400">
                                <i data-lucide="calculator" class="w-5 h-5"></i>
                            </div>
                            <div>
                                <h3 id="txtCalcTitle" class="font-bold text-sm">Kalkulator Trade</h3>
                                <p id="txtCalcSub" class="text-[11px] text-slate-400">Hitung total nilai item</p>
                            </div>
                        </div>
                        <div class="flex bg-slate-100 dark:bg-slate-800 p-1 rounded-xl text-xs font-semibold">
                            <button onclick="setCurrency('WL')" id="btnCurrWL" class="px-2.5 py-1 rounded-lg bg-brand-600 text-white transition">WL</button>
                            <button onclick="setCurrency('DL')" id="btnCurrDL" class="px-2.5 py-1 rounded-lg text-slate-600 dark:text-slate-400 transition">DL</button>
                        </div>
                    </div>

                    <div class="mt-4 space-y-4">
                        <div>
                            <label id="txtLabelSelectItem" class="block text-xs font-medium text-slate-500 dark:text-slate-400 mb-1.5">Pilih Item</label>
                            <select id="calcItemSelect" onchange="calculateTrade()" class="w-full px-3.5 py-2.5 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-darkborder text-sm font-medium focus:outline-none focus:ring-2 focus:ring-brand-500">
                                <!-- Options injected by JS -->
                            </select>
                        </div>

                        <div>
                            <label id="txtLabelQty" class="block text-xs font-medium text-slate-500 dark:text-slate-400 mb-1.5">Jumlah Item</label>
                            <input type="number" id="calcQty" min="1" value="1" oninput="calculateTrade()" class="w-full px-3.5 py-2.5 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-darkborder text-sm font-medium focus:outline-none focus:ring-2 focus:ring-brand-500">
                        </div>
                    </div>
                </div>

                <!-- Hasil Kalkulator -->
                <div class="mt-6 pt-4 border-t border-slate-200 dark:border-darkborder space-y-3">
                    <div class="p-4 rounded-xl bg-slate-50 dark:bg-slate-800/60 border border-slate-200/60 dark:border-slate-700/60 space-y-2">
                        <div class="flex justify-between text-xs text-slate-500 dark:text-slate-400">
                            <span id="txtEstValue">Estimasi Nilai:</span>
                            <span id="calcUnitLabel" class="font-medium">-</span>
                        </div>
                        <div class="text-2xl font-extrabold text-brand-600 dark:text-brand-400" id="calcResult">0 WL</div>
                        <div class="text-xs text-slate-500 dark:text-slate-400 flex items-center gap-1 pt-1 border-t border-slate-200/40 dark:border-slate-700/40" id="calcRupiahEst">
                            <i data-lucide="banknote" class="w-3.5 h-3.5 text-emerald-500"></i> Estimasi IDR: Rp 0
                        </div>
                    </div>

                    <button onclick="copyCalcResult()" id="txtBtnCopyTrade" class="w-full py-2.5 rounded-xl bg-slate-100 dark:bg-slate-800 hover:bg-brand-600 hover:text-white text-xs font-semibold flex items-center justify-center gap-1.5 transition">
                        <i data-lucide="copy" class="w-4 h-4"></i> Salin Hasil Trade
                    </button>
                </div>
            </div>
        </div>
    </main>

    <!-- Modal Detail Item -->
    <div id="detailModal" class="fixed inset-0 z-50 bg-slate-900/60 backdrop-blur-sm hidden flex items-center justify-center p-4">
        <div class="bg-white dark:bg-darkcard rounded-2xl max-w-md w-full p-6 border border-slate-200 dark:border-darkborder shadow-xl space-y-5 animate-in fade-in zoom-in-95 duration-200">
            <div class="flex items-start justify-between">
                <div class="flex items-center gap-3">
                    <div id="modalIcon" class="w-12 h-12 rounded-2xl bg-brand-50 dark:bg-slate-800 flex items-center justify-center text-2xl overflow-hidden border border-slate-200 dark:border-slate-700">📦</div>
                    <div>
                        <h3 id="modalTitle" class="font-extrabold text-lg">-</h3>
                        <p id="modalCategory" class="text-xs text-slate-400 uppercase tracking-wider font-semibold">-</p>
                    </div>
                </div>
                <button onclick="toggleModal('detailModal')" class="p-1.5 rounded-lg hover:bg-slate-100 dark:hover:bg-slate-800 text-slate-400 transition">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>

            <div class="grid grid-cols-2 gap-3">
                <div class="p-3.5 rounded-xl bg-slate-50 dark:bg-slate-800/60 border border-slate-200/60 dark:border-slate-700/60">
                    <span id="txtModalPrice" class="text-[11px] text-slate-400 block font-medium">Harga Pasar</span>
                    <span id="modalPrice" class="text-base font-extrabold text-slate-900 dark:text-white mt-0.5 block">-</span>
                </div>
                <div class="p-3.5 rounded-xl bg-slate-50 dark:bg-slate-800/60 border border-slate-200/60 dark:border-slate-700/60">
                    <span id="txtModalDemand" class="text-[11px] text-slate-400 block font-medium">Permintaan</span>
                    <span id="modalDemand" class="text-base font-extrabold text-emerald-600 dark:text-emerald-400 mt-0.5 block">-</span>
                </div>
            </div>

            <!-- Grafik Tren 7 Hari -->
            <div class="space-y-2">
                <span id="txtChartTitle" class="text-xs font-semibold text-slate-500 dark:text-slate-400 flex items-center gap-1.5">
                    <i data-lucide="trending-up" class="w-3.5 h-3.5 text-brand-500"></i> Tren Harga 7 Hari Terakhir
                </span>
                <div id="modalChart" class="h-28 flex items-end gap-1.5 px-2 pt-4 pb-2 bg-slate-50 dark:bg-slate-800/40 rounded-xl border border-slate-200/60 dark:border-slate-700/60">
                    <!-- Bars injected by JS -->
                </div>
            </div>

            <button onclick="copyDetailPrice()" id="txtBtnCopyDetail" class="w-full py-3 rounded-xl bg-brand-600 hover:bg-brand-700 text-white font-semibold text-xs flex items-center justify-center gap-2 shadow-sm transition">
                <i data-lucide="copy" class="w-4 h-4"></i> Salin Informasi Harga
            </button>
        </div>
    </div>

    <!-- Modal Atur Kurs WL ke Rupiah -->
    <div id="rateModal" class="fixed inset-0 z-50 bg-slate-900/60 backdrop-blur-sm hidden flex items-center justify-center p-4">
        <div class="bg-white dark:bg-darkcard rounded-2xl max-w-sm w-full p-6 border border-slate-200 dark:border-darkborder shadow-xl space-y-4">
            <div class="flex items-center justify-between">
                <h3 id="txtRateModalTitle" class="font-extrabold text-base">Atur Kurs Rupiah per 1 WL</h3>
                <button onclick="toggleModal('rateModal')" class="p-1 rounded-lg hover:bg-slate-100 dark:hover:bg-slate-800 text-slate-400">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>
            <div>
                <label id="txtRateLabel" class="block text-xs font-medium text-slate-500 dark:text-slate-400 mb-1.5">Nilai Rupiah (Rp) per 1 WL</label>
                <input type="number" id="inputWlRupiah" min="1" class="w-full px-3.5 py-2.5 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-darkborder text-sm font-medium focus:outline-none focus:ring-2 focus:ring-brand-500">
            </div>
            <button onclick="saveWlRupiah()" id="txtSaveRateBtn" class="w-full py-2.5 rounded-xl bg-brand-600 hover:bg-brand-700 text-white font-semibold text-xs transition">
                Simpan Kurs
            </button>
        </div>
    </div>

    <!-- Modal Form Tambah / Edit Item -->
    <div id="itemFormModal" class="fixed inset-0 z-50 bg-slate-900/60 backdrop-blur-sm hidden flex items-center justify-center p-4">
        <div class="bg-white dark:bg-darkcard rounded-2xl max-w-md w-full p-6 border border-slate-200 dark:border-darkborder shadow-xl space-y-4 max-h-[90vh] overflow-y-auto">
            <div class="flex items-center justify-between">
                <h3 id="formModalTitle" class="font-extrabold text-base">Tambah Item Baru</h3>
                <button onclick="toggleModal('itemFormModal')" class="p-1 rounded-lg hover:bg-slate-100 dark:hover:bg-slate-800 text-slate-400">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>
            <form id="itemForm" onsubmit="saveItemForm(event)" class="space-y-4">
                <input type="hidden" id="formItemId">
                <div>
                    <label id="txtFormName" class="block text-xs font-medium text-slate-500 dark:text-slate-400 mb-1">Nama Item</label>
                    <input type="text" id="formName" required placeholder="Cth: Golden Pickaxe" class="w-full px-3.5 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-darkborder text-sm focus:outline-none focus:ring-2 focus:ring-brand-500">
                </div>
                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label id="txtFormCategory" class="block text-xs font-medium text-slate-500 dark:text-slate-400 mb-1">Kategori</label>
                        <select id="formCategory" class="w-full px-3 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-darkborder text-sm focus:outline-none focus:ring-2 focus:ring-brand-500">
                            <option value="iotm">IOTM / Rare</option>
                            <option value="wings">Wings</option>
                            <option value="farmable">Farmable</option>
                            <option value="blocks">Blocks & Item</option>
                        </select>
                    </div>
                    <div>
                        <label id="txtFormDemand" class="block text-xs font-medium text-slate-500 dark:text-slate-400 mb-1">Permintaan</label>
                        <select id="formDemand" class="w-full px-3 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-darkborder text-sm focus:outline-none focus:ring-2 focus:ring-brand-500">
                            <option value="Sangat Tinggi" id="optDemandVeryHigh">Sangat Tinggi</option>
                            <option value="Tinggi" id="optDemandHigh">Tinggi</option>
                            <option value="Sedang" id="optDemandMed">Sedang</option>
                            <option value="Rendah" id="optDemandLow">Rendah</option>
                        </select>
                    </div>
                </div>

                <div class="grid grid-cols-2 gap-3 items-center">
                    <div>
                        <label id="txtFormEmoji" class="block text-xs font-medium text-slate-500 dark:text-slate-400 mb-1">Emoji Ikon</label>
                        <input type="text" id="formIcon" value="📦" oninput="updateIconPreview()" class="w-full px-3.5 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-darkborder text-sm text-center focus:outline-none focus:ring-2 focus:ring-brand-500">
                    </div>
                    <div class="flex items-center gap-3 pt-4">
                        <div id="formIconPreview" class="w-10 h-10 rounded-xl bg-slate-100 dark:bg-slate-800 flex items-center justify-center text-xl overflow-hidden border border-slate-200 dark:border-slate-700">📦</div>
                        <span id="txtFormPreview" class="text-[11px] text-slate-400">Preview Ikon</span>
                    </div>
                </div>

                <div>
                    <label id="txtFormImgUrl" class="block text-xs font-medium text-slate-500 dark:text-slate-400 mb-1">URL Gambar / Icon (Opsional)</label>
                    <input type="url" id="formImageUrl" placeholder="https://contoh.com/gambar.png" oninput="updateIconPreview()" class="w-full px-3.5 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-darkborder text-sm focus:outline-none focus:ring-2 focus:ring-brand-500">
                </div>

                <div class="pt-2 border-t border-slate-200 dark:border-darkborder space-y-3">
                    <div>
                        <label id="txtFormPriceType" class="block text-xs font-medium text-slate-500 dark:text-slate-400 mb-1">Tipe Harga</label>
                        <select id="formPriceType" onchange="togglePriceInputs()" class="w-full px-3.5 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-darkborder text-sm focus:outline-none focus:ring-2 focus:ring-brand-500">
                            <option value="unit" id="optPriceUnit">Satuan (≥ 1 WL atau DL)</option>
                            <option value="rate" id="optPriceRate">Rate Banyak per 1 WL (Misal: 28 / WL)</option>
                        </select>
                    </div>

                    <div id="unitPriceContainer" class="grid grid-cols-2 gap-3">
                        <div>
                            <label id="txtFormNominal" class="block text-xs font-medium text-slate-500 dark:text-slate-400 mb-1">Nominal Harga</label>
                            <input type="number" id="formPriceVal" step="0.1" min="0.1" value="1" class="w-full px-3.5 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-darkborder text-sm focus:outline-none focus:ring-2 focus:ring-brand-500">
                        </div>
                        <div>
                            <label id="txtFormCurrency" class="block text-xs font-medium text-slate-500 dark:text-slate-400 mb-1">Satuan Mata Uang</label>
                            <select id="formPriceUnit" class="w-full px-3.5 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-darkborder text-sm focus:outline-none focus:ring-2 focus:ring-brand-500">
                                <option value="WL">WL</option>
                                <option value="DL">DL</option>
                            </select>
                        </div>
                    </div>

                    <div id="ratePriceContainer" class="hidden">
                        <label id="txtFormItemsPerWL" class="block text-xs font-medium text-slate-500 dark:text-slate-400 mb-1">Jumlah Item per 1 WL</label>
                        <input type="number" id="formItemsPerWL" min="1" value="25" class="w-full px-3.5 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-darkborder text-sm focus:outline-none focus:ring-2 focus:ring-brand-500">
                        <p id="txtFormRateHint" class="text-[11px] text-slate-400 mt-1">Cth: 28 artinya 28 item untuk 1 World Lock.</p>
                    </div>
                </div>

                <button type="submit" id="txtFormSaveBtn" class="w-full py-2.5 rounded-xl bg-brand-600 hover:bg-brand-700 text-white font-semibold text-xs transition">
                    Simpan Item
                </button>
            </form>
        </div>
    </div>

    <!-- Toast Notifikasi -->
    <div id="toast" class="fixed bottom-6 right-6 z-50 transform translate-y-20 opacity-0 transition-all duration-300 bg-slate-900 dark:bg-white text-white dark:text-slate-900 px-4 py-3 rounded-2xl shadow-xl flex items-center gap-2.5 text-xs font-semibold">
        <i data-lucide="check-circle-2" class="w-4 h-4 text-brand-500"></i>
        <span id="toastMsg">Berhasil!</span>
    </div>

    <!-- JavaScript Script -->
    <script>
        // Data Item Pasar Growblox
        let marketItems = [
            { id: 1, name: "Magplant 5000", category: "iotm", priceWL: 8500, change: 14.5, demand: "Sangat Tinggi", icon: "🌱", imageUrl: "", unit: "85 DL", isRate: false, history: [7200, 7400, 7500, 7800, 8100, 8300, 8500] },
            { id: 2, name: "Rayman's Fist", category: "iotm", priceWL: 11200, change: 6.2, demand: "Sangat Tinggi", icon: "👊", imageUrl: "", unit: "112 DL", isRate: false, history: [10500, 10600, 10800, 10750, 11000, 11100, 11200] },
            { id: 3, name: "Angel Wings", category: "wings", priceWL: 320, change: -2.4, demand: "Tinggi", icon: "🪽", imageUrl: "", unit: "3.2 DL", isRate: false, history: [340, 335, 330, 328, 325, 322, 320] },
            { id: 4, name: "Laser Grid Seed", category: "farmable", priceWL: 0.0357, change: 4.1, demand: "Tinggi", icon: "⚡", imageUrl: "", unit: "28 / WL", isRate: true, itemsPerWL: 28, rateText: "28 / WL", history: [32, 31, 30, 30, 29, 28, 28] },
            { id: 5, name: "Chandlier Block", category: "farmable", priceWL: 0.02, change: 0.0, demand: "Sedang", icon: "🕯️", imageUrl: "", unit: "50 / WL", isRate: true, itemsPerWL: 50, rateText: "50 / WL", history: [50, 50, 50, 50, 50, 50, 50] },
            { id: 6, name: "Vending Machine", category: "blocks", priceWL: 4, change: 1.5, demand: "Tinggi", icon: "🏪", imageUrl: "", unit: "4 WL each", isRate: false, history: [3.8, 3.8, 3.9, 3.9, 4.0, 4.0, 4.0] },
            { id: 7, name: "Diamond Pickaxe", category: "blocks", priceWL: 18, change: -4.0, demand: "Sedang", icon: "⛏️", imageUrl: "", unit: "18 WL each", isRate: false, history: [20, 19.5, 19, 19, 18.5, 18, 18] },
            { id: 8, name: "Neon Devil Wings", category: "wings", priceWL: 1450, change: 9.8, demand: "Tinggi", icon: "🦇", imageUrl: "", unit: "14.5 DL", isRate: false, history: [1280, 1300, 1340, 1380, 1400, 1420, 1450] }
        ];

        let currentCurrency = 'WL';
        let currentCategory = 'all';
        let selectedItemForCopy = null;
        let isEditMode = false;
        let wlToRupiah = parseInt(localStorage.getItem('growblox_wl_idr')) || 150;
        let currentLang = localStorage.getItem('growblox_lang') || 'ID';

        // Kamus Terjemahan (ID / EN)
        const translations = {
            ID: {
                editMode: "Atur Harga",
                exitEditMode: "Keluar Mode Edit",
                addItem: "Tambah Item",
                card1Title: "Kurs 1 WL / Rupiah",
                set: "Atur",
                card2Title: "Featured Farmable",
                card3Title: "Featured Block & Item",
                card4Title: "Featured IOTM / Rare",
                searchPlaceholder: "Cari item (misal: Magplant, Angel Wings...)",
                catAll: "Semua",
                catIotm: "IOTM / Rare",
                catWings: "Wings",
                catFarmable: "Farmables",
                catBlocks: "Blocks & Item",
                tableTitle: "Daftar Harga Pasar Live",
                tableSubtitle: "Harga per item & rate per 1 World Lock",
                thItemName: "Nama Item",
                thMarketPrice: "Harga Pasar",
                thAction: "Aksi",
                emptyTitle: "Tidak ada item ditemukan",
                emptySub: "Coba kata kunci lain atau ubah kategori",
                calcTitle: "Kalkulator Trade",
                calcSub: "Hitung total nilai item",
                labelSelectItem: "Pilih Item",
                labelQty: "Jumlah Item",
                estValue: "Estimasi Nilai:",
                btnCopyTrade: "Salin Hasil Trade",
                modalPrice: "Harga Pasar",
                modalDemand: "Permintaan",
                chartTitle: "Tren Harga 7 Hari Terakhir",
                btnCopyDetail: "Salin Informasi Harga",
                rateModalTitle: "Atur Kurs Rupiah per 1 WL",
                rateLabel: "Nilai Rupiah (Rp) per 1 WL",
                saveRateBtn: "Simpan Kurs",
                formTitleAdd: "Tambah Item Baru",
                formTitleEdit: "Edit Item Pasar",
                formName: "Nama Item",
                formCategory: "Kategori",
                formDemand: "Permintaan",
                formEmoji: "Emoji Ikon",
                formPreview: "Preview Ikon",
                formImgUrl: "URL Gambar / Icon (Opsional)",
                formPriceType: "Tipe Harga",
                optPriceUnit: "Satuan (≥ 1 WL atau DL)",
                optPriceRate: "Rate Banyak per 1 WL (Misal: 28 / WL)",
                formNominal: "Nominal Harga",
                formCurrency: "Satuan Mata Uang",
                formItemsPerWL: "Jumlah Item per 1 WL",
                formRateHint: "Cth: 28 artinya 28 item untuk 1 World Lock.",
                formSaveBtn: "Simpan Item",
                demandVeryHigh: "Sangat Tinggi",
                demandHigh: "Tinggi",
                demandMed: "Sedang",
                demandLow: "Rendah",
                toastSaved: "Berhasil disimpan!",
                toastDeleted: "Item berhasil dihapus.",
                toastRateUpdated: "Kurs WL ke Rupiah berhasil diperbarui!",
                toastCopiedTrade: "Hasil trade berhasil disalin ke clipboard!",
                toastCopiedDetail: "Informasi harga berhasil disalin!",
                toastEditModeActive: "Mode Atur Harga aktif! Anda bisa mengubah atau menambah item."
            },
            EN: {
                editMode: "Manage Prices",
                exitEditMode: "Exit Edit Mode",
                addItem: "Add Item",
                card1Title: "1 WL / IDR Rate",
                set: "Set",
                card2Title: "Featured Farmable",
                card3Title: "Featured Block & Item",
                card4Title: "Featured IOTM / Rare",
                searchPlaceholder: "Search items (e.g., Magplant, Angel Wings...)",
                catAll: "All",
                catIotm: "IOTM / Rare",
                catWings: "Wings",
                catFarmable: "Farmables",
                catBlocks: "Blocks & Item",
                tableTitle: "Live Market Price List",
                tableSubtitle: "Price per item & rate per 1 World Lock",
                thItemName: "Item Name",
                thMarketPrice: "Market Price",
                thAction: "Action",
                emptyTitle: "No items found",
                emptySub: "Try another keyword or change category",
                calcTitle: "Trade Calculator",
                calcSub: "Calculate total item value",
                labelSelectItem: "Select Item",
                labelQty: "Item Quantity",
                estValue: "Estimated Value:",
                btnCopyTrade: "Copy Trade Result",
                modalPrice: "Market Price",
                modalDemand: "Demand",
                chartTitle: "7-Day Price Trend",
                btnCopyDetail: "Copy Price Info",
                rateModalTitle: "Set IDR Rate per 1 WL",
                rateLabel: "IDR Value (Rp) per 1 WL",
                saveRateBtn: "Save Rate",
                formTitleAdd: "Add New Item",
                formTitleEdit: "Edit Market Item",
                formName: "Item Name",
                formCategory: "Category",
                formDemand: "Demand",
                formEmoji: "Emoji Icon",
                formPreview: "Icon Preview",
                formImgUrl: "Image / Icon URL (Optional)",
                formPriceType: "Price Type",
                optPriceUnit: "Unit (≥ 1 WL or DL)",
                optPriceRate: "Rate per 1 WL (e.g., 28 / WL)",
                formNominal: "Price Amount",
                formCurrency: "Currency Unit",
                formItemsPerWL: "Items per 1 WL",
                formRateHint: "Ex: 28 means 28 items for 1 World Lock.",
                formSaveBtn: "Save Item",
                demandVeryHigh: "Very High",
                demandHigh: "High",
                demandMed: "Medium",
                demandLow: "Low",
                toastSaved: "Successfully saved!",
                toastDeleted: "Item successfully deleted.",
                toastRateUpdated: "WL to IDR rate successfully updated!",
                toastCopiedTrade: "Trade result copied to clipboard!",
                toastCopiedDetail: "Price info copied!",
                toastEditModeActive: "Manage Prices mode is active! You can now edit or add items."
            }
        };

        function toggleLanguage() {
            currentLang = currentLang === 'ID' ? 'EN' : 'ID';
            localStorage.setItem('growblox_lang', currentLang);
            updateLanguageUI();
        }

        function updateLanguageUI() {
            const t = translations[currentLang];
            document.getElementById('langLabel').textContent = currentLang;

            // Header & Navbar
            document.getElementById('txtEditMode').textContent = isEditMode ? t.exitEditMode : t.editMode;
            document.getElementById('txtAddItem').textContent = t.addItem;

            // Stat Cards
            document.getElementById('txtCard1Title').textContent = t.card1Title;
            document.getElementById('txtSet').textContent = t.set;
            document.getElementById('txtCard2Title').textContent = t.card2Title;
            document.getElementById('txtCard3Title').textContent = t.card3Title;
            document.getElementById('txtCard4Title').textContent = t.card4Title;

            // Search & Categories
            document.getElementById('searchInput').placeholder = t.searchPlaceholder;
            document.getElementById('catAllBtn').textContent = t.catAll;
            document.getElementById('catIotmBtn').textContent = t.catIotm;
            document.getElementById('catWingsBtn').textContent = t.catWings;
            document.getElementById('catFarmableBtn').textContent = t.catFarmable;
            document.getElementById('catBlocksBtn').textContent = t.catBlocks;

            // Table
            document.getElementById('txtTableTitle').textContent = t.tableTitle;
            document.getElementById('txtTableSubtitle').textContent = t.tableSubtitle;
            document.getElementById('thItemName').textContent = t.thItemName;
            document.getElementById('thMarketPrice').textContent = t.thMarketPrice;
            document.getElementById('thAction').textContent = t.thAction;
            document.getElementById('txtEmptyTitle').textContent = t.emptyTitle;
            document.getElementById('txtEmptySub').textContent = t.emptySub;

            // Calculator
            document.getElementById('txtCalcTitle').textContent = t.calcTitle;
            document.getElementById('txtCalcSub').textContent = t.calcSub;
            document.getElementById('txtLabelSelectItem').textContent = t.labelSelectItem;
            document.getElementById('txtLabelQty').textContent = t.labelQty;
            document.getElementById('txtEstValue').textContent = t.estValue;
            document.getElementById('txtBtnCopyTrade').innerHTML = `<i data-lucide="copy" class="w-4 h-4"></i> ${t.btnCopyTrade}`;

            // Detail Modal
            document.getElementById('txtModalPrice').textContent = t.modalPrice;
            document.getElementById('txtModalDemand').textContent = t.modalDemand;
            document.getElementById('txtChartTitle').innerHTML = `<i data-lucide="trending-up" class="w-3.5 h-3.5 text-brand-500"></i> ${t.chartTitle}`;
            document.getElementById('txtBtnCopyDetail').innerHTML = `<i data-lucide="copy" class="w-4 h-4"></i> ${t.btnCopyDetail}`;

            // Rate Modal
            document.getElementById('txtRateModalTitle').textContent = t.rateModalTitle;
            document.getElementById('txtRateLabel').textContent = t.rateLabel;
            document.getElementById('txtSaveRateBtn').textContent = t.saveRateBtn;

            // Form Modal
            document.getElementById('txtFormName').textContent = t.formName;
            document.getElementById('txtFormCategory').textContent = t.formCategory;
            document.getElementById('txtFormDemand').textContent = t.formDemand;
            document.getElementById('txtFormEmoji').textContent = t.formEmoji;
            document.getElementById('txtFormPreview').textContent = t.formPreview;
            document.getElementById('txtFormImgUrl').textContent = t.formImgUrl;
            document.getElementById('txtFormPriceType').textContent = t.formPriceType;
            document.getElementById('optPriceUnit').textContent = t.optPriceUnit;
            document.getElementById('optPriceRate').textContent = t.optPriceRate;
            document.getElementById('txtFormNominal').textContent = t.formNominal;
            document.getElementById('txtFormCurrency').textContent = t.formCurrency;
            document.getElementById('txtFormItemsPerWL').textContent = t.formItemsPerWL;
            document.getElementById('txtFormRateHint').textContent = t.formRateHint;
            document.getElementById('txtFormSaveBtn').textContent = t.formSaveBtn;

            document.getElementById('optDemandVeryHigh').textContent = t.demandVeryHigh;
            document.getElementById('optDemandHigh').textContent = t.demandHigh;
            document.getElementById('optDemandMed').textContent = t.demandMed;
            document.getElementById('optDemandLow').textContent = t.demandLow;

            renderTable();
            lucide.createIcons();
        }

        // Indeks untuk Rotasi Otomatis Kartu Atas
        let rotationIndices = {
            farmable: 0,
            blockItem: 0,
            iotm: 0
        };

        // Inisialisasi Tema
        if (localStorage.theme === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
            document.documentElement.classList.add('dark');
        } else {
            document.documentElement.classList.remove('dark');
        }

        function toggleTheme() {
            if (document.documentElement.classList.contains('dark')) {
                document.documentElement.classList.remove('dark');
                localStorage.theme = 'light';
            } else {
                document.documentElement.classList.add('dark');
                localStorage.theme = 'dark';
            }
        }

        function showToast(msg) {
            const toast = document.getElementById('toast');
            document.getElementById('toastMsg').textContent = msg;
            toast.classList.remove('translate-y-20', 'opacity-0');
            setTimeout(() => {
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 3000);
        }

        function toggleModal(modalId) {
            const modal = document.getElementById(modalId);
            modal.classList.toggle('hidden');
        }

        // Render Ikon (Gambar URL atau Emoji)
        function renderItemIcon(item) {
            if (item.imageUrl && item.imageUrl.trim() !== '') {
                return `<img src="${item.imageUrl}" alt="${item.name}" class="w-full h-full object-cover" onerror="this.style.display='none'; this.nextElementSibling.style.display='flex';">
                        <span style="display:none;" class="w-full h-full items-center justify-center">${item.icon || '📦'}</span>`;
            }
            return item.icon || '📦';
        }

        function updateIconPreview() {
            const url = document.getElementById('formImageUrl').value.trim();
            const emoji = document.getElementById('formIcon').value.trim() || '📦';
            const previewEl = document.getElementById('formIconPreview');
            if (url) {
                previewEl.innerHTML = `<img src="${url}" class="w-full h-full object-cover" onerror="this.parentElement.textContent='${emoji}'">`;
            } else {
                previewEl.textContent = emoji;
            }
        }

        // Update Kartu Statistik Atas (Rotasi Bergantian Setiap 4 Detik)
        function updateMarketStats() {
            document.getElementById('statWlRupiah').textContent = `Rp ${wlToRupiah.toLocaleString('id-ID')}`;
            document.getElementById('statDlRupiah').innerHTML = `<i data-lucide="banknote" class="w-3.5 h-3.5"></i> 1 DL = Rp ${(wlToRupiah * 100).toLocaleString('id-ID')}`;

            // 1. Farmables
            const farmables = marketItems.filter(i => i.category === 'farmable');
            if (farmables.length > 0) {
                const idx = rotationIndices.farmable % farmables.length;
                const item = farmables[idx];
                document.getElementById('statFarmableName').textContent = item.name;
                document.getElementById('statFarmableSub').textContent = `${item.isRate ? item.rateText : formatPrice(item)} • ${item.demand}`;
            } else {
                document.getElementById('statFarmableName').textContent = '-';
                document.getElementById('statFarmableSub').textContent = '-';
            }

            // 2. Block & Item (blocks & wings)
            const blockItems = marketItems.filter(i => i.category === 'blocks' || i.category === 'wings');
            if (blockItems.length > 0) {
                const idx = rotationIndices.blockItem % blockItems.length;
                const item = blockItems[idx];
                document.getElementById('statBlockItemName').textContent = item.name;
                document.getElementById('statBlockItemSub').textContent = `${item.isRate ? item.rateText : formatPrice(item)} • ${item.demand}`;
            } else {
                document.getElementById('statBlockItemName').textContent = '-';
                document.getElementById('statBlockItemSub').textContent = '-';
            }

            // 3. IOTM / Rare
            const iotms = marketItems.filter(i => i.category === 'iotm');
            if (iotms.length > 0) {
                const idx = rotationIndices.iotm % iotms.length;
                const item = iotms[idx];
                document.getElementById('statIotmName').textContent = item.name;
                document.getElementById('statIotmSub').textContent = `${item.isRate ? item.rateText : formatPrice(item)} • ${item.demand}`;
            } else {
                document.getElementById('statIotmName').textContent = '-';
                document.getElementById('statIotmSub').textContent = '-';
            }
        }

        setInterval(() => {
            rotationIndices.farmable++;
            rotationIndices.blockItem++;
            rotationIndices.iotm++;
            updateMarketStats();
        }, 4000);

        // Pengaturan Kurs Rupiah
        function openRateModal() {
            document.getElementById('inputWlRupiah').value = wlToRupiah;
            toggleModal('rateModal');
        }

        function saveWlRupiah() {
            const val = parseInt(document.getElementById('inputWlRupiah').value);
            if (val && val > 0) {
                wlToRupiah = val;
                localStorage.setItem('growblox_wl_idr', val);
                toggleModal('rateModal');
                updateMarketStats();
                calculateTrade();
                showToast(translations[currentLang].toastRateUpdated);
            }
        }

        // Format Harga
        function formatPrice(item) {
            if (item.isRate) {
                return item.rateText || `${item.itemsPerWL} / WL`;
            }
            if (item.priceWL >= 100) {
                const dl = item.priceWL / 100;
                return `${dl % 1 === 0 ? dl : dl.toFixed(1)} DL`;
            }
            return `${item.priceWL} WL`;
        }

        function getPriceBadge(item) {
            const t = translations[currentLang];
            if (item.isRate) {
                return `<span class="inline-flex items-center px-2 py-0.5 rounded-md text-[11px] font-semibold bg-purple-500/10 text-purple-600 dark:text-purple-400">${currentLang === 'ID' ? 'Rate Per WL' : 'Rate Per WL'}</span>`;
            }
            if (item.priceWL >= 100) {
                return `<span class="inline-flex items-center px-2 py-0.5 rounded-md text-[11px] font-semibold bg-blue-500/10 text-blue-600 dark:text-blue-400">${currentLang === 'ID' ? 'Satuan DL (≥ 1 WL)' : 'DL Unit (≥ 1 WL)'}</span>`;
            }
            return `<span class="inline-flex items-center px-2 py-0.5 rounded-md text-[11px] font-semibold bg-emerald-500/10 text-emerald-600 dark:text-emerald-400">${currentLang === 'ID' ? 'Satuan WL' : 'WL Unit'}</span>`;
        }

        // Mode Edit Publik
        function toggleEditMode() {
            isEditMode = !isEditMode;
            const btn = document.getElementById('editModeBtn');
            const addBtn = document.getElementById('addItemBtn');
            const t = translations[currentLang];

            if (isEditMode) {
                btn.classList.add('bg-brand-500/20', 'text-brand-600');
                document.getElementById('txtEditMode').textContent = t.exitEditMode;
                addBtn.classList.remove('hidden');
                showToast(t.toastEditModeActive);
            } else {
                btn.classList.remove('bg-brand-500/20', 'text-brand-600');
                document.getElementById('txtEditMode').textContent = t.editMode;
                addBtn.classList.add('hidden');
            }
            renderTable();
        }

        function openItemForm(id = null) {
            const form = document.getElementById('itemForm');
            form.reset();
            document.getElementById('formItemId').value = '';
            const t = translations[currentLang];
            document.getElementById('formModalTitle').textContent = t.formTitleAdd;
            document.getElementById('formIconPreview').textContent = '📦';
            document.getElementById('unitPriceContainer').classList.remove('hidden');
            document.getElementById('ratePriceContainer').classList.add('hidden');

            if (id) {
                const item = marketItems.find(i => i.id === id);
                if (item) {
                    document.getElementById('formModalTitle').textContent = t.formTitleEdit;
                    document.getElementById('formItemId').value = item.id;
                    document.getElementById('formName').value = item.name;
                    document.getElementById('formCategory').value = item.category;
                    document.getElementById('formDemand').value = item.demand;
                    document.getElementById('formIcon').value = item.icon || '📦';
                    document.getElementById('formImageUrl').value = item.imageUrl || '';
                    
                    if (item.isRate) {
                        document.getElementById('formPriceType').value = 'rate';
                        document.getElementById('formItemsPerWL').value = item.itemsPerWL || 25;
                        document.getElementById('unitPriceContainer').classList.add('hidden');
                        document.getElementById('ratePriceContainer').classList.remove('hidden');
                    } else {
                        document.getElementById('formPriceType').value = 'unit';
                        if (item.priceWL >= 100) {
                            document.getElementById('formPriceVal').value = item.priceWL / 100;
                            document.getElementById('formPriceUnit').value = 'DL';
                        } else {
                            document.getElementById('formPriceVal').value = item.priceWL;
                            document.getElementById('formPriceUnit').value = 'WL';
                        }
                    }
                    updateIconPreview();
                }
            }
            toggleModal('itemFormModal');
        }

        function togglePriceInputs() {
            const type = document.getElementById('formPriceType').value;
            if (type === 'rate') {
                document.getElementById('unitPriceContainer').classList.add('hidden');
                document.getElementById('ratePriceContainer').classList.remove('hidden');
            } else {
                document.getElementById('unitPriceContainer').classList.remove('hidden');
                document.getElementById('ratePriceContainer').classList.add('hidden');
            }
        }

        function saveItemForm(e) {
            e.preventDefault();
            const idStr = document.getElementById('formItemId').value;
            const name = document.getElementById('formName').value.trim();
            const category = document.getElementById('formCategory').value;
            const demand = document.getElementById('formDemand').value;
            const icon = document.getElementById('formIcon').value.trim() || '📦';
            const imageUrl = document.getElementById('formImageUrl').value.trim();
            const priceType = document.getElementById('formPriceType').value;

            let isRate = false;
            let priceWL = 1;
            let itemsPerWL = null;
            let rateText = '';
            let unitText = 'per item';

            if (priceType === 'rate') {
                isRate = true;
                itemsPerWL = Math.max(1, parseInt(document.getElementById('formItemsPerWL').value) || 1);
                priceWL = parseFloat((1 / itemsPerWL).toFixed(4));
                rateText = `${itemsPerWL} / WL`;
                unitText = rateText;
            } else {
                const rawVal = parseFloat(document.getElementById('formPriceVal').value) || 1;
                const unit = document.getElementById('formPriceUnit').value;
                priceWL = unit === 'DL' ? rawVal * 100 : rawVal;
                unitText = priceWL >= 100 ? `${(priceWL / 100).toLocaleString('id-ID')} DL` : `${priceWL.toLocaleString('id-ID')} WL each`;
            }

            if (idStr) {
                const existing = marketItems.find(i => i.id === parseInt(idStr));
                if (existing) {
                    existing.name = name;
                    existing.category = category;
                    existing.demand = demand;
                    existing.icon = icon;
                    existing.imageUrl = imageUrl;
                    existing.isRate = isRate;
                    existing.priceWL = priceWL;
                    existing.itemsPerWL = itemsPerWL;
                    existing.rateText = rateText;
                    existing.unit = unitText;
                    const chartPoint = isRate ? itemsPerWL : priceWL;
                    existing.history = [...existing.history.slice(1), chartPoint];
                    showToast(translations[currentLang].toastSaved);
                }
            } else {
                const chartPoint = isRate ? itemsPerWL : priceWL;
                marketItems.unshift({
                    id: Date.now(),
                    name,
                    category,
                    priceWL,
                    change: 0.0,
                    demand,
                    icon,
                    imageUrl,
                    unit: unitText,
                    isRate,
                    itemsPerWL,
                    rateText,
                    history: Array(7).fill(chartPoint)
                });
                showToast(translations[currentLang].toastSaved);
            }

            toggleModal('itemFormModal');
            renderTable();
            initCalculator();
        }

        function deleteItem(id) {
            // Menggunakan konfirmasi kustom atau langsung menghapus dengan aman tanpa window.confirm jika diperlukan, atau menggunakan metode pembantu
            marketItems = marketItems.filter(i => i.id !== id);
            renderTable();
            initCalculator();
            showToast(translations[currentLang].toastDeleted);
        }

        // Render Tabel
        function renderTable() {
            const tbody = document.getElementById('marketTableBody');
            const emptyState = document.getElementById('emptyState');
            const query = document.getElementById('searchInput').value.toLowerCase().trim();

            const filtered = marketItems.filter(item => {
                const matchCat = currentCategory === 'all' || item.category === currentCategory;
                const matchQuery = item.name.toLowerCase().includes(query);
                return matchCat && matchQuery;
            });

            updateMarketStats();
            tbody.innerHTML = '';

            if (filtered.length === 0) {
                emptyState.classList.remove('hidden');
                lucide.createIcons();
                return;
            }
            emptyState.classList.add('hidden');

            filtered.forEach(item => {
                const editActions = isEditMode ? `
                    <button type="button" onclick="event.stopPropagation(); openItemForm(${item.id});" class="p-1.5 rounded-lg bg-amber-500/15 hover:bg-amber-500/25 text-amber-600 dark:text-amber-400 transition" title="Edit">
                        <i data-lucide="pencil" class="w-4 h-4"></i>
                    </button>
                    <button type="button" onclick="event.stopPropagation(); deleteItem(${item.id});" class="p-1.5 rounded-lg bg-rose-500/15 hover:bg-rose-500/25 text-rose-600 dark:text-rose-400 transition" title="Delete">
                        <i data-lucide="trash-2" class="w-4 h-4"></i>
                    </button>
                ` : '';

                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-50/80 dark:hover:bg-slate-800/40 transition group";
                tr.innerHTML = `
                    <td class="py-3.5 px-4">
                        <div class="flex items-center gap-3">
                            <div class="w-10 h-10 rounded-xl bg-slate-100 dark:bg-slate-800 flex items-center justify-center text-lg overflow-hidden shrink-0 border border-slate-200/50 dark:border-slate-700/50">
                                ${renderItemIcon(item)}
                            </div>
                            <div>
                                <div class="font-bold text-slate-900 dark:text-white group-hover:text-brand-600 dark:group-hover:text-brand-400 transition cursor-pointer" onclick="openDetail(${item.id})">${item.name}</div>
                                <div class="flex items-center gap-1.5 mt-0.5">
                                    <span class="text-[11px] uppercase text-slate-400 font-medium">${item.category}</span>
                                    <span class="text-slate-300 dark:text-slate-600">•</span>
                                    <span class="text-[11px] text-slate-400">${currentLang === 'ID' ? 'Permintaan' : 'Demand'}: ${item.demand}</span>
                                </div>
                            </div>
                        </div>
                    </td>
                    <td class="py-3.5 px-4">
                        <div class="font-extrabold text-slate-900 dark:text-white">${formatPrice(item)}</div>
                        <div class="mt-1">${getPriceBadge(item)}</div>
                    </td>
                    <td class="py-3.5 px-4 text-right">
                        <div class="inline-flex items-center justify-end gap-1">
                            ${editActions}
                            <button type="button" onclick="openDetail(${item.id})" class="px-2.5 py-1.5 rounded-lg bg-slate-100 dark:bg-slate-800 hover:bg-brand-600 hover:text-white text-xs font-semibold transition">
                                ${currentLang === 'ID' ? 'Detail' : 'Details'}
                            </button>
                        </div>
                    </td>
                `;
                tbody.appendChild(tr);
            });

            lucide.createIcons();
        }

        function filterCategory(cat, btn) {
            currentCategory = cat;
            document.querySelectorAll('.category-btn').forEach(b => {
                b.className = "category-btn px-4 py-2 rounded-xl text-xs font-semibold bg-white dark:bg-darkcard hover:bg-slate-100 dark:hover:bg-slate-800 border border-slate-200 dark:border-darkborder shrink-0 transition";
            });
            btn.className = "category-btn px-4 py-2 rounded-xl text-xs font-semibold bg-brand-600 text-white shrink-0 transition";
            renderTable();
        }

        // Detail Modal & Grafik
        function openDetail(id) {
            const item = marketItems.find(i => i.id === id);
            if (!item) return;
            selectedItemForCopy = item;

            document.getElementById('modalIcon').innerHTML = renderItemIcon(item);
            document.getElementById('modalTitle').textContent = item.name;
            document.getElementById('modalCategory').textContent = item.category;
            document.getElementById('modalPrice').textContent = formatPrice(item);
            document.getElementById('modalDemand').textContent = item.demand;

            const chartContainer = document.getElementById('modalChart');
            chartContainer.innerHTML = '';
            const history = item.history || [1, 2, 3, 4, 5, 6, 7];
            const maxVal = Math.max(...history, 1);

            history.forEach(val => {
                const heightPercent = Math.max(15, Math.round((val / maxVal) * 100));
                const bar = document.createElement('div');
                bar.className = "flex-1 bg-brand-500/20 hover:bg-brand-500 rounded-t-md transition-all duration-300 relative group";
                bar.style.height = `${heightPercent}%`;
                chartContainer.appendChild(bar);
            });

            toggleModal('detailModal');
            lucide.createIcons();
        }

        // Kalkulator Trade
        function initCalculator() {
            const select = document.getElementById('calcItemSelect');
            select.innerHTML = '';
            marketItems.forEach(item => {
                const opt = document.createElement('option');
                opt.value = item.id;
                opt.textContent = `${item.name} (${formatPrice(item)})`;
                select.appendChild(opt);
            });
            calculateTrade();
        }

        function setCurrency(curr) {
            currentCurrency = curr;
            document.getElementById('btnCurrWL').className = curr === 'WL' ? "px-2.5 py-1 rounded-lg bg-brand-600 text-white transition" : "px-2.5 py-1 rounded-lg text-slate-600 dark:text-slate-400 transition";
            document.getElementById('btnCurrDL').className = curr === 'DL' ? "px-2.5 py-1 rounded-lg bg-brand-600 text-white transition" : "px-2.5 py-1 rounded-lg text-slate-600 dark:text-slate-400 transition";
            calculateTrade();
        }

        function calculateTrade() {
            const selectId = document.getElementById('calcItemSelect').value;
            const qty = parseFloat(document.getElementById('calcQty').value) || 0;
            const item = marketItems.find(i => i.id == selectId);

            if (!item) return;

            let totalWL = 0;
            if (item.isRate) {
                totalWL = qty / (item.itemsPerWL || 1);
            } else {
                totalWL = item.priceWL * qty;
            }

            const unitLabel = document.getElementById('calcUnitLabel');
            const resultEl = document.getElementById('calcResult');
            const rupiahEl = document.getElementById('calcRupiahEst');

            unitLabel.textContent = `${qty}x ${item.name}`;

            if (currentCurrency === 'DL') {
                const dlVal = totalWL / 100;
                resultEl.textContent = `${dlVal >= 1 ? dlVal.toFixed(2) : dlVal.toFixed(3)} DL`;
            } else {
                resultEl.textContent = totalWL >= 100 ? `${(totalWL / 100).toFixed(2)} DL (${Math.round(totalWL)} WL)` : `${Math.round(totalWL * 100) / 100} WL`;
            }

            const totalRupiah = Math.round(totalWL * wlToRupiah);
            rupiahEl.innerHTML = `<i data-lucide="banknote" class="w-3.5 h-3.5 text-emerald-500"></i> ${currentLang === 'ID' ? 'Estimasi IDR' : 'Est IDR'}: Rp ${totalRupiah.toLocaleString('id-ID')}`;
            lucide.createIcons();
        }

        function copyCalcResult() {
            const text = document.getElementById('calcResult').textContent;
            const label = document.getElementById('calcUnitLabel').textContent;
            const rupiah = document.getElementById('calcRupiahEst').textContent;
            const temp = document.createElement('textarea');
            temp.value = `Growblox Trade Calculator:\n${label} = ${text} (${rupiah})`;
            document.body.appendChild(temp);
            temp.select();
            document.execCommand('copy');
            document.body.removeChild(temp);
            showToast(translations[currentLang].toastCopiedTrade);
        }

        function copyDetailPrice() {
            if (!selectedItemForCopy) return;
            const item = selectedItemForCopy;
            const temp = document.createElement('textarea');
            temp.value = `Growblox Market Price - ${item.name}:\nPrice: ${formatPrice(item)}\nDemand: ${item.demand}`;
            document.body.appendChild(temp);
            temp.select();
            document.execCommand('copy');
            document.body.removeChild(temp);
            showToast(translations[currentLang].toastCopiedDetail);
        }

        // Inisialisasi awal saat halaman dimuat
        window.onload = function() {
            updateLanguageUI();
            initCalculator();
            lucide.createIcons();
        };
    </script>
</body>
</html>
