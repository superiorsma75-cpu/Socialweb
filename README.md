<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ChatConnect - Kirim Pesan Singkat</title>
    <!-- Menggunakan FontAwesome untuk Ikon -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        :root {
            --primary-color: #008069; /* Warna hijau ala aplikasi chat populer */
            --primary-light: #25D366;
            --bg-color: #e9edef;
            --sidebar-bg: #ffffff;
            --text-dark: #111b21;
            --text-gray: #54656f;
            --border-color: #e9edef;
            --white: #ffffff;
            --danger: #ef5350;
            --shadow: 0 2px 5px rgba(0,0,0,0.05);
            --transition: all 0.3s ease;
        }

        /* Aksesibilitas: Mode Kontras Tinggi (Aktif via JS) */
        body.high-contrast {
            --primary-color: #004d40;
            --bg-color: #000000;
            --sidebar-bg: #121212;
            --text-dark: #ffffff;
            --text-gray: #eeeeee;
            --border-color: #333333;
            --white: #1e1e1e;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-dark);
            height: 100vh;
            display: flex;
            overflow: hidden;
        }

        /* --- Sidebar Navigasi --- */
        .sidebar {
            width: 300px;
            background-color: var(--sidebar-bg);
            border-right: 1px solid var(--border-color);
            display: flex;
            flex-direction: column;
            transition: var(--transition);
            z-index: 100;
        }

        .brand {
            padding: 20px;
            background-color: var(--primary-color);
            color: white;
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 1.2rem;
            font-weight: bold;
        }

        .nav-menu {
            flex: 1;
            overflow-y: auto;
            padding: 10px 0;
        }

        .nav-category {
            font-size: 0.8rem;
            text-transform: uppercase;
            color: var(--primary-color);
            padding: 15px 20px 5px;
            font-weight: bold;
            letter-spacing: 0.5px;
        }
        
        body.high-contrast .nav-category {
            color: var(--primary-light);
        }

        .nav-item {
            display: flex;
            align-items: center;
            padding: 12px 20px;
            cursor: pointer;
            color: var(--text-dark);
            text-decoration: none;
            transition: background 0.2s;
            font-size: 0.95rem;
            border-left: 4px solid transparent;
        }

        .nav-item:hover {
            background-color: rgba(0, 128, 105, 0.1);
        }

        .nav-item.active {
            background-color: rgba(0, 128, 105, 0.1);
            border-left-color: var(--primary-color);
            font-weight: 600;
        }

        .nav-item i {
            width: 25px;
            color: var(--text-gray);
            margin-right: 10px;
        }

        .nav-item.active i {
            color: var(--primary-color);
        }

        /* --- Main Content Area --- */
        .main-content {
            flex: 1;
            display: flex;
            flex-direction: column;
            position: relative;
            background-image: url('https://picsum.photos/seed/bgpattern/800/600?blur=5'); /* Pattern background subtle */
            background-size: cover;
            background-blend-mode: overlay;
            background-color: rgba(255, 255, 255, 0.9);
        }
        
        body.high-contrast .main-content {
            background-blend-mode: normal;
            background-color: var(--sidebar-bg);
            background-image: none;
        }

        /* Header Mobile */
        .mobile-header {
            display: none;
            padding: 15px;
            background-color: var(--primary-color);
            color: white;
            align-items: center;
            justify-content: space-between;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        /* Sections (Pages) */
        .section {
            display: none; /* Hidden by default */
            flex: 1;
            flex-direction: column;
            height: 100%;
            overflow-y: auto;
            padding: 20px;
            animation: fadeIn 0.3s ease;
        }

        .section.active-section {
            display: flex;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* --- Components Styles --- */
        h2 {
            margin-bottom: 20px;
            color: var(--primary-color);
            border-bottom: 2px solid var(--border-color);
            padding-bottom: 10px;
        }

        .card {
            background: var(--white);
            border-radius: 8px;
            padding: 20px;
            box-shadow: var(--shadow);
            margin-bottom: 20px;
        }

        /* --- Home: Chat Interface --- */
        #section-home {
            padding: 0;
            display: none; /* Controlled by JS */
            flex-direction: row;
            overflow: hidden;
        }
        #section-home.active-section {
            display: flex;
        }

        .chat-list {
            width: 350px;
            background: var(--white);
            border-right: 1px solid var(--border-color);
            overflow-y: auto;
            display: flex;
            flex-direction: column;
        }

        .chat-item {
            display: flex;
            align-items: center;
            padding: 15px;
            border-bottom: 1px solid var(--border-color);
            cursor: pointer;
        }

        .chat-item:hover, .chat-item.active-chat {
            background-color: #f0f2f5;
        }

        .avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            object-fit: cover;
            margin-right: 15px;
            background-color: #ddd;
        }

        .chat-info h4 {
            font-size: 1rem;
            margin-bottom: 4px;
        }

        .chat-info p {
            font-size: 0.85rem;
            color: var(--text-gray);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 200px;
        }

        .chat-window {
            flex: 1;
            display: flex;
            flex-direction: column;
            background-color: #efe7dd; /* Warna background chat klasik */
            background-image: url('https://user-images.githubusercontent.com/15075759/28719144-86dc0f70-73b1-11e7-911d-60d70fcded21.png'); /* Pattern WA */
            position: relative;
        }

        .chat-messages {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .message {
            max-width: 60%;
            padding: 10px 15px;
            border-radius: 8px;
            position: relative;
            font-size: 0.95rem;
            line-height: 1.4;
        }

        .message.received {
            background-color: var(--white);
            align-self: flex-start;
            border-top-left-radius: 0;
        }

        .message.sent {
            background-color: #dcf8c6;
            align-self: flex-end;
            border-top-right-radius: 0;
        }
        
        body.high-contrast .message.sent {
            background-color: #004d40;
            color: white;
        }
        
        body.high-contrast .message.received {
            background-color: #333;
            color: white;
        }

        .message-time {
            font-size: 0.7rem;
            color: var(--text-gray);
            float: right;
            margin-top: 5px;
            margin-left: 10px;
        }

        .chat-input-area {
            background: var(--bg-color);
            padding: 10px 20px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .chat-input {
            flex: 1;
            padding: 12px;
            border-radius: 20px;
            border: 1px solid var(--border-color);
            outline: none;
        }

        .btn-send {
            background: var(--primary-color);
            color: white;
            border: none;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: transform 0.2s;
        }

        .btn-send:hover {
            transform: scale(1.1);
        }

        /* --- Forms & Settings --- */
        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
        }

        .form-control {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            font-size: 1rem;
        }

        .toggle-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid var(--border-color);
        }

        /* Toggle Switch */
        .switch {
            position: relative;
            display: inline-block;
            width: 50px;
            height: 24px;
        }

        .switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }

        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            transition: .4s;
            border-radius: 24px;
        }

        .slider:before {
            position: absolute;
            content: "";
            height: 16px;
            width: 16px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }

        input:checked + .slider {
            background-color: var(--primary-color);
        }

        input:checked + .slider:before {
            transform: translateX(26px);
        }

        /* Buttons */
        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            transition: opacity 0.2s;
        }

        .btn-primary {
            background-color: var(--primary-color);
            color: white;
        }
        
        .btn-danger {
            background-color: var(--danger);
            color: white;
        }

        .btn:hover {
            opacity: 0.9;
        }

        /* Toast Notification */
        .toast {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background-color: #333;
            color: white;
            padding: 12px 24px;
            border-radius: 5px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.2);
            transform: translateY(100px);
            opacity: 0;
            transition: all 0.4s cubic-bezier(0.68, -0.55, 0.27, 1.55);
            z-index: 1000;
        }
        
        .toast.show {
            transform: translateY(0);
            opacity: 1;
        }

        /* Responsive Mobile */
        @media (max-width: 768px) {
            .sidebar {
                position: fixed;
                left: -300px;
                height: 100%;
                box-shadow: 2px 0 10px rgba(0,0,0,0.2);
            }
            
            .sidebar.open {
                left: 0;
            }

            .mobile-header {
                display: flex;
            }

            .chat-list {
                width: 100%;
                display: none;
            }

            .chat-list.active {
                display: flex;
            }

            .chat-window {
                display: none;
            }

            .chat-window.active {
                display: flex;
                width: 100%;
            }
            
            #section-home {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>

    <!-- Toast Notification -->
    <div id="toast" class="toast">Berhasil disimpan</div>

    <!-- Header Mobile -->
    <header class="mobile-header">
        <i class="fas fa-bars fa-lg" onclick="toggleSidebar()"></i>
        <span>ChatConnect</span>
        <div style="width: 24px;"></div> <!-- Spacer for balance -->
    </header>

    <!-- Sidebar Navigation -->
    <nav class="sidebar" id="sidebar">
        <div class="brand">
            <i class="fas fa-comments"></i> ChatConnect
        </div>

        <div class="nav-menu">
            <div class="nav-category">Utama</div>
            <a onclick="showSection('home')" class="nav-item active" id="nav-home">
                <i class="fas fa-home"></i> Home
            </a>
            <a onclick="showSection('profile')" class="nav-item" id="nav-profile">
                <i class="fas fa-user"></i> Profil
            </a>
            <a onclick="showSection('about')" class="nav-item" id="nav-about">
                <i class="fas fa-info-circle"></i> Tentang Kami
            </a>

            <div class="nav-category">Pengaturan</div>
            <a onclick="showSection('notifications')" class="nav-item" id="nav-notifications">
                <i class="fas fa-bell"></i> Notifikasi
            </a>
            <a onclick="showSection('privacy')" class="nav-item" id="nav-privacy">
                <i class="fas fa-lock"></i> Privasi
            </a>
            <a onclick="showSection('data')" class="nav-item" id="nav-data">
                <i class="fas fa-database"></i> Penyimpanan Data
            </a>
            <a onclick="showSection('language')" class="nav-item" id="nav-language">
                <i class="fas fa-globe"></i> Bahasa
            </a>
            <a onclick="showSection('accessibility')" class="nav-item" id="nav-accessibility">
                <i class="fas fa-universal-access"></i> Aksesibilitas
            </a>
            <a onclick="showSection('updates')" class="nav-item" id="nav-updates">
                <i class="fas fa-download"></i> Pembaharuan Aplikasi
            </a>

            <div class="nav-category">Dukungan</div>
            <a onclick="showSection('help')" class="nav-item" id="nav-help">
                <i class="fas fa-question-circle"></i> Bantuan dan Masukan
            </a>
        </div>
    </nav>

    <!-- Main Content Container -->
    <main class="main-content">
        
        <!-- SECTION: HOME (CHAT) -->
        <section id="section-home" class="section active-section">
            <!-- Chat List -->
            <div class="chat-list" id="chatList">
                <div class="chat-item active-chat" onclick="selectChat('Budi Santoso', 'https://picsum.photos/seed/budi/50/50')">
                    <img src="https://picsum.photos/seed/budi/50/50" alt="Avatar" class="avatar">
                    <div class="chat-info">
                        <h4>Budi Santoso</h4>
                        <p>Halo, apa kabar? Lama tak jumpa!</p>
                    </div>
                </div>
                <div class="chat-item" onclick="selectChat('Siti Aminah', 'https://picsum.photos/seed/siti/50/50')">
                    <img src="https://picsum.photos/seed/siti/50/50" alt="Avatar" class="avatar">
                    <div class="chat-info">
                        <h4>Siti Aminah</h4>
                        <p>Dokumen sudah saya kirim ya.</p>
                    </div>
                </div>
                <div class="chat-item" onclick="selectChat('Tim Proyek', 'https://picsum.photos/seed/tim/50/50')">
                    <img src="https://picsum.photos/seed/tim/50/50" alt="Avatar" class="avatar">
                    <div class="chat-info">
                        <h4>Tim Proyek</h4>
                        <p>Meeting pukul 14.00 jangan lupa.</p>
                    </div>
                </div>
            </div>

            <!-- Chat Window -->
            <div class="chat-window active" id="chatWindow">
                <div class="chat-messages" id="messageContainer">
                    <div class="message received">
                        Halo, apa kabar? Lama tak jumpa!
                        <span class="message-time">10:30</span>
                    </div>
                    <div class="message sent">
                        Kabar baik! Kamu gimana?
                        <span class="message-time">10:31</span>
                    </div>
                </div>
                <div class="chat-input-area">
                    <i class="far fa-smile fa-lg text-gray"></i>
                    <i class="fas fa-paperclip fa-lg text-gray"></i>
                    <input type="text" class="chat-input" id="messageInput" placeholder="Ketik pesan..." onkeypress="handleEnter(event)">
                    <button class="btn-send" onclick="sendMessage()">
                        <i class="fas fa-paper-plane"></i>
                    </button>
                </div>
            </div>
        </section>

        <!-- SECTION: PROFIL -->
        <section id="section-profile" class="section">
            <h2>Profil Pengguna</h2>
            <div class="card" style="text-align: center;">
                <img src="https://picsum.photos/seed/me/150/150" alt="My Profile" style="border-radius: 50%; width: 100px; height: 100px; margin-bottom: 15px;">
                <h3>Nama Pengguna</h3>
                <p style="color: var(--text-gray); margin-bottom: 20px;">+62 812 3456 7890</p>
                <button class="btn btn-primary">Ganti Foto Profil</button>
            </div>

            <div class="card">
                <div class="form-group">
                    <label>Nama</label>
                    <input type="text" class="form-control" value="Nama Pengguna">
                </div>
                <div class="form-group">
                    <label>Tentang Saya</label>
                    <input type="text" class="form-control" value="Hai, saya ada di ChatConnect">
                </div>
                <button class="btn btn-primary" onclick="showToast('Profil diperbarui')">Simpan</button>
            </div>
        </section>

        <!-- SECTION: TENTANG KAMI -->
        <section id="section-about" class="section">
            <h2>Tentang Kami</h2>
            <div class="card">
                <p><strong>ChatConnect v1.0.0</strong></p>
                <p style="margin-top: 10px;">
                    ChatOnline adalah platform komunikasi digital yang aman dan cepat. Kami didedikasikan untuk menghubungkan orang-orang di seluruh dunia tanpa batas.
                    Misi kami adalah menyediakan layanan pesan yang sederhana, andal, dan menghargai privasi pengguna.
                </p>
                <br>
                <p>Dikembangkan dengan ❤️ oleh Tim ChatOnline.</p>
                <br>
                <p>&copy; 2026 ChatOnline Inc. Hak Cipta Dilindungi.</p>
            </div>
        </section>

        <!-- SECTION: NOTIFIKASI -->
        <section id="section-notifications" class="section">
            <h2>Notifikasi</h2>
            <div class="card">
                <div class="toggle-row">
                    <span>Notifikasi Pesan</span>
                    <label class="switch">
                        <input type="checkbox" checked onchange="showToast('Pengaturan notifikasi disimpan')">
                        <span class="slider"></span>
                    </label>
                </div>
                <div class="toggle-row">
                    <span>Notifikasi Grup</span>
                    <label class="switch">
                        <input type="checkbox" checked onchange="showToast('Pengaturan notifikasi disimpan')">
                        <span class="slider"></span>
                    </label>
                </div>
                <div class="toggle-row">
                    <span>Suara Panggilan Masuk</span>
                    <label class="switch">
                        <input type="checkbox" checked onchange="showToast('Pengaturan notifikasi disimpan')">
                        <span class="slider"></span>
                    </label>
                </div>
                <div class="toggle-row">
                    <span>Getar</span>
                    <label class="switch">
                        <input type="checkbox" onchange="showToast('Pengaturan notifikasi disimpan')">
                        <span class="slider"></span>
                    </label>
                </div>
                <div class="toggle-row">
                    <span>Preview Pesan di Layar Terkunci</span>
                    <label class="switch">
                        <input type="checkbox" onchange="showToast('Pengaturan notifikasi disimpan')">
                        <span class="slider"></span>
                    </label>
                </div>
            </div>
        </section>

        <!-- SECTION: PRIVASI -->
        <section id="section-privacy" class="section">
            <h2>Privasi</h2>
            <div class="card">
                <div class="toggle-row">
                    <span>Tampilkan Foto Profil</span>
                    <label class="switch">
                        <input type="checkbox" checked>
                        <span class="slider"></span>
                    </label>
                </div>
                <div class="toggle-row">
                    <span>Tampilkan Status Terakhir Dilihat</span>
                    <label class="switch">
                        <input type="checkbox" checked>
                        <span class="slider"></span>
                    </label>
                </div>
                <div class="toggle-row">
                    <span>Read Receipts (Centang Biru)</span>
                    <label class="switch">
                        <input type="checkbox" checked>
                        <span class="slider"></span>
                    </label>
                </div>
            </div>
            <div class="card">
                <button class="btn btn-danger" onclick="showToast('Pengaturan privasi dikunci')">Kunci Akun</button>
            </div>
        </section>

        <!-- SECTION: PENYIMPANAN DATA -->
        <section id="section-data" class="section">
            <h2>Penyimpanan Data</h2>
            <div class="card">
                <p>Kelola penggunaan data dan penyimpanan media Anda.</p>
                <div style="margin: 20px 0; background: #eee; height: 10px; border-radius: 5px;">
                    <div style="width: 40%; background: var(--primary-color); height: 100%; border-radius: 5px;"></div>
                </div>
                <p style="font-size: 0.8rem;">1.2 GB dari 5 GB digunakan</p>
                <hr style="margin: 15px 0; border: 0; border-top: 1px solid var(--border-color);">
                <div class="toggle-row">
                    <span>Gunakan Sedikit Data untuk Panggilan</span>
                    <label class="switch">
                        <input type="checkbox">
                        <span class="slider"></span>
                    </label>
                </div>
                <div class="toggle-row">
                    <span>Unduh Otomatis Media</span>
                    <label class="switch">
                        <input type="checkbox" checked>
                        <span class="slider"></span>
                    </label>
                </div>
            </div>
            <div class="card">
                <button class="btn btn-primary" onclick="showToast('Memeriksa ruang penyimpanan...')">Kelola Penyimpanan</button>
                <button class="btn" style="background: #eee; margin-left: 10px;" onclick="showToast('Cache dibersihkan')">Bersihkan Cache</button>
            </div>
        </section>

        <!-- SECTION: BAHASA -->
        <section id="section-language" class="section">
            <h2>Bahasa</h2>
            <div class="card">
                <div class="form-group">
                    <label>Pilih Bahasa Aplikasi</label>
                    <select class="form-control">
                        <option value="id" selected>Indonesia</option>
                        <option value="en">English</option>
                        <option value="jv">Basa Jawa</option>
                        <option value="su">Basa Sunda</option>
                    </select>
                </div>
                <button class="btn btn-primary" onclick="showToast('Bahasa diubah')">Terapkan</button>
            </div>
        </section>

        <!-- SECTION: BANTUAN DAN MASUKAN -->
        <section id="section-help" class="section">
            <h2>Bantuan dan Masukan</h2>
            <div class="card">
                <p>Temukan jawaban untuk pertanyaan umum atau hubungi kami.</p>
                <div style="margin-top: 15px;">
                    <a href="#" style="display: block; padding: 10px; border-bottom: 1px solid #eee; text-decoration: none; color: var(--text-dark);">Cara Mengirim Pesan</a>
                    <a href="#" style="display: block; padding: 10px; border-bottom: 1px solid #eee; text-decoration: none; color: var(--text-dark);">Pusat Keamanan</a>
                    <a href="#" style="display: block; padding: 10px; border-bottom: 1px solid #eee; text-decoration: none; color: var(--text-dark);">Hubungi Kami</a>
                </div>
            </div>
            <div class="card">
                <h3>Kirim Masukan</h3>
                <div class="form-group" style="margin-top: 10px;">
                    <textarea class="form-control" rows="4" placeholder="Tuliskan masukan Anda di sini..."></textarea>
                </div>
                <button class="btn btn-primary" onclick="showToast('Masukan terkirim')">Kirim</button>
            </div>
        </section>

        <!-- SECTION: PEMBAHARUAN APLIKASI -->
        <section id="section-updates" class="section">
            <h2>Pembaharuan Aplikasi</h2>
            <div class="card">
                <div style="display: flex; justify-content: space-between; align-items: center;">
                    <div>
                        <h4>Versi Terbaru</h4>
                        <p style="color: var(--text-gray); font-size: 0.9rem;">Versi 1.0.0 (Rilis Stabil)</p>
                    </div>
                    <i class="fas fa-check-circle fa-2x" style="color: var(--primary-color);"></i>
                </div>
                <hr style="margin: 15px 0; border: 0; border-top: 1px solid var(--border-color);">
                <p>ChatConnect Anda sudah mutakhir. Anda akan menerima notifikasi ketika versi baru tersedia.</p>
                <button class="btn btn-primary" style="margin-top: 15px;" onclick="showToast('Tidak ada pembaruan baru')">Cek Pembaruan</button>
            </div>
        </section>

        <!-- SECTION: AKSESIBILITAS -->
        <section id="section-accessibility" class="section">
            <h2>Aksesibilitas</h2>
            <div class="card">
                <div class="toggle-row">
                    <span>Mode Kontras Tinggi</span>
                    <label class="switch">
                        <input type="checkbox" id="contrastToggle" onchange="toggleHighContrast()">
                        <span class="slider"></span>
                    </label>
                </div>
                <div class="form-group" style="margin-top: 20px;">
                    <label>Ukuran Teks</label>
                    <input type="range" min="1" max="5" value="1" class="form-control" oninput="changeFontSize(this.value)">
                    <div style="display: flex; justify-content: space-between; font-size: 0.8rem; color: var(--text-gray); margin-top: 5px;">
                        <span>Kecil</span>
                        <span>Besar</span>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <script>
        // --- Navigation Logic ---
        function showSection(sectionId) {
            // Hide all sections
            document.querySelectorAll('.section').forEach(sec => {
                sec.classList.remove('active-section');
            });
            // Remove active class from nav items
            document.querySelectorAll('.nav-item').forEach(item => {
                item.classList.remove('active');
            });

            // Show target section
            document.getElementById('section-' + sectionId).classList.add('active-section');
            
            // Add active class to nav item (if it exists in sidebar)
            const navItem = document.getElementById('nav-' + sectionId);
            if(navItem) navItem.classList.add('active');

            // On mobile, close sidebar after selection
            if (window.innerWidth <= 768) {
                document.getElementById('sidebar').classList.remove('open');
            }
        }

        // --- Mobile Sidebar Toggle ---
        function toggleSidebar() {
            document.getElementById('sidebar').classList.toggle('open');
        }

        // --- Chat Simulation Logic ---
        function handleEnter(e) {
            if (e.key === 'Enter') {
                sendMessage();
            }
        }

        function sendMessage() {
            const input = document.getElementById('messageInput');
            const text = input.value.trim();
            
            if (text !== "") {
                const container = document.getElementById('messageContainer');
                
                // Create Message Bubble (Sent)
                const msgDiv = document.createElement('div');
                msgDiv.className = 'message sent';
                msgDiv.innerHTML = text + '<span class="message-time">Sekarang</span>';
                
                container.appendChild(msgDiv);
                input.value = "";
                
                // Scroll to bottom
                container.scrollTop = container.scrollHeight;

                // Auto reply simulation after 1.5 seconds
                setTimeout(() => {
                    const replyDiv = document.createElement('div');
                    replyDiv.className = 'message received';
                    replyDiv.innerHTML = 'Terima kasih atas pesan Anda! (Otomatis)<span class="message-time">Sekarang</span>';
                    container.appendChild(replyDiv);
                    container.scrollTop = container.scrollHeight;
                    showToast('Pesan baru diterima');
                }, 1500);
            }
        }

        function selectChat(name, imgUrl) {
            // Visual feedback on chat list
            const items = document.querySelectorAll('.chat-item');
            items.forEach(item => item.classList.remove('active-chat'));
            event.currentTarget.classList.add('active-chat');

            // Update header (optional, not implemented fully for this showcase)
            // In a real app, this would load conversation history
            
            // On mobile view, switch to chat window
            if (window.innerWidth <= 768) {
                document.getElementById('chatList').classList.remove('active');
                document.getElementById('chatWindow').classList.add('active');
            }
        }
        
        // Mobile Back Button Logic (Implicitly handled by refreshing home or adding a back button, 
        // but for simplicity here clicking 'Home' resets views)
        const originalShowSection = showSection;
        showSection = function(id) {
            if(id === 'home' && window.innerWidth <= 768) {
                document.getElementById('chatList').classList.add('active');
                document.getElementById('chatWindow').classList.remove('active');
            }
            originalShowSection(id);
        }

        // --- Accessibility Logic ---
        function toggleHighContrast() {
            const isChecked = document.getElementById('contrastToggle').checked;
            if (isChecked) {
                document.body.classList.add('high-contrast');
                showToast('Mode kontras tinggi aktif');
            } else {
                document.body.classList.remove('high-contrast');
                showToast('Mode kontras tinggi non-aktif');
            }
        }

        function changeFontSize(val) {
            // Simple implementation: scale body font size slightly based on value
            // 1 = 100% (16px), 5 = 120% (approx)
            const scale = 1 + ((val - 1) * 0.05); 
            document.body.style.fontSize = (16 * scale) + 'px';
        }

        // --- Utility: Toast Notification ---
        function showToast(message) {
            const toast = document.getElementById('toast');
            toast.textContent = message;
            toast.classList.add('show');
            
            setTimeout(() => {
                toast.classList.remove('show');
            }, 3000);
        }

        // Initialize: Ensure Home is visible logic on load
        document.addEventListener('DOMContentLoaded', () => {
            if(window.innerWidth > 768) {
                document.getElementById('chatList').style.display = 'flex';
                document.getElementById('chatWindow').style.display = 'flex';
            } else {
                document.getElementById('chatList').classList.add('active');
            }
        });
        
        // Window resize handler to fix chat layout
        window.addEventListener('resize', () => {
            if(window.innerWidth > 768) {
                document.getElementById('chatList').style.display = 'flex';
                document.getElementById('chatList').classList.remove('active');
                document.getElementById('chatWindow').style.display = 'flex';
                document.getElementById('chatWindow').classList.remove('active');
            } else {
                 // Reset to list view if switching to mobile from desktop
                 document.getElementById('chatList').classList.add('active');
                 document.getElementById('chatWindow').classList.remove('active');
            }
        });

    </script>
</body>
</html>
