# Socialweb
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SocialVid - Connect & Watch</title>
    <style>
        /* --- CSS VARIABLES & RESET --- */
        :root {
            --primary: #6c5ce7;
            --bg-body: #f4f6f8;
            --bg-card: #ffffff;
            --text-main: #2d3436;
            --text-sec: #636e72;
            --border: #dfe6e9;
            --sidebar-width: 250px;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', sans-serif; }
        body { background-color: var(--bg-body); color: var(--text-main); display: flex; height: 100vh; overflow: hidden; }
        a { text-decoration: none; color: inherit; }
        ul { list-style: none; }

        /* --- SIDEBAR (MENU) --- */
        .sidebar {
            width: var(--sidebar-width);
            background: var(--bg-card);
            border-right: 1px solid var(--border);
            display: flex;
            flex-direction: column;
            padding: 20px;
            overflow-y: auto;
        }
        .logo { font-size: 24px; font-weight: bold; color: var(--primary); margin-bottom: 30px; }
        .menu-item {
            padding: 12px 15px;
            margin-bottom: 5px;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.2s;
            display: flex;
            align-items: center;
            gap: 10px;
            color: var(--text-sec);
        }
        .menu-item:hover, .menu-item.active { background-color: #eef2ff; color: var(--primary); font-weight: 600; }
        .menu-section-title { font-size: 12px; text-transform: uppercase; color: #b2bec3; margin: 20px 0 10px; font-weight: bold; }

        /* --- MAIN CONTENT --- */
        .main-content {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            position: relative;
        }
        
        /* HEADER MOBILE */
        .mobile-header { display: none; padding: 15px; background: white; border-bottom: 1px solid var(--border); align-items: center; justify-content: space-between; }
        
        /* SECTIONS (Dynamic Content) */
        .section { display: none; animation: fadeIn 0.3s ease; }
        .section.active { display: block; }
        
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        h2 { margin-bottom: 20px; }

        /* VIDEO FEED STYLE */
        .video-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px; }
        .video-card { background: var(--bg-card); border-radius: 12px; overflow: hidden; box-shadow: 0 2px 5px rgba(0,0,0,0.05); }
        .video-thumbnail { width: 100%; height: 160px; background-color: #ddd; display: flex; align-items: center; justify-content: center; color: #aaa; }
        .video-info { padding: 15px; }
        .video-title { font-weight: bold; margin-bottom: 5px; }
        .video-meta { font-size: 12px; color: var(--text-sec); }

        /* CHAT STYLE */
        .chat-container { display: flex; height: 80vh; background: var(--bg-card); border-radius: 12px; border: 1px solid var(--border); overflow: hidden; }
        .chat-list { width: 30%; border-right: 1px solid var(--border); overflow-y: auto; }
        .chat-user { padding: 15px; border-bottom: 1px solid var(--border); cursor: pointer; }
        .chat-user:hover { background: #f9f9f9; }
        .chat-area { width: 70%; display: flex; flex-direction: column; }
        .chat-messages { flex: 1; padding: 20px; background: #fdfdfd; }
        .chat-input { padding: 15px; border-top: 1px solid var(--border); display: flex; gap: 10px; }
        .chat-input input { flex: 1; padding: 10px; border: 1px solid var(--border); border-radius: 20px; outline: none; }
        .btn { padding: 8px 20px; background: var(--primary); color: white; border: none; border-radius: 20px; cursor: pointer; }

        /* SETTINGS & OTHER PAGES */
        .settings-card { background: white; padding: 20px; border-radius: 10px; border: 1px solid var(--border); max-width: 600px; margin-bottom: 15px; }
        .setting-row { display: flex; justify-content: space-between; align-items: center; padding: 10px 0; border-bottom: 1px solid #eee; }
        .setting-row:last-child { border-bottom: none; }
        
        /* RESPONSIVE */
        @media (max-width: 768px) {
            .sidebar { display: none; position: absolute; z-index: 100; height: 100%; box-shadow: 2px 0 10px rgba(0,0,0,0.1); }
            .sidebar.show { display: flex; }
            .mobile-header { display: flex; }
            .chat-list { width: 100%; } .chat-area { display: none; } /* Simplified for mobile demo */
        }
    </style>
</head>
<body>

    <div class="mobile-header">
        <div class="logo">SocialVid</div>
        <button onclick="toggleSidebar()" style="background:none; border:none; font-size:20px;">☰</button>
    </div>

    <nav class="sidebar" id="sidebar">
        <div class="logo">SocialVid</div>
        
        <div class="menu-section-title">Utama</div>
        <div class="menu-item active" onclick="nav('home')">🏠 Home (Video)</div>
        <div class="menu-item" onclick="nav('messages')">💬 Pesan</div>
        <div class="menu-item" onclick="nav('notifications')">🔔 Notifikasi</div>
        <div class="menu-item" onclick="nav('profile')">👤 Profil</div>

        <div class="menu-section-title">Pengaturan & Bantuan</div>
        <div class="menu-item" onclick="nav('privacy')">🔒 Privasi</div>
        <div class="menu-item" onclick="nav('storage')">💾 Penyimpanan Data</div>
        <div class="menu-item" onclick="nav('language')">🌍 Bahasa</div>
        <div class="menu-item" onclick="nav('accessibility')">👓 Aksesibilitas</div>
        <div class="menu-item" onclick="nav('updates')">🔄 Pembaharuan</div>
        <div class="menu-item" onclick="nav('help')">❓ Bantuan & Masukan</div>
        <div class="menu-item" onclick="nav('about')">ℹ️ Tentang Kami</div>
    </nav>

    <main class="main-content">
        
        <section id="home" class="section active">
            <h2>Beranda - Video Terbaru</h2>
            <div class="video-grid">
                <div class="video-card">
                    <div class="video-thumbnail">▶ Video Player</div>
                    <div class="video-info">
                        <div class="video-title">Tutorial Coding Pemula</div>
                        <div class="video-meta">12k views • 2 jam yang lalu</div>
                    </div>
                </div>
                <div class="video-card">
                    <div class="video-thumbnail">▶ Video Player</div>
                    <div class="video-info">
                        <div class="video-title">Vlog Liburan ke Bali</div>
                        <div class="video-meta">50k views • 1 hari yang lalu</div>
                    </div>
                </div>
                <div class="video-card">
                    <div class="video-thumbnail">▶ Video Player</div>
                    <div class="video-info">
                        <div class="video-title">Resep Masakan Padang</div>
                        <div class="video-meta">8k views • 5 jam yang lalu</div>
                    </div>
                </div>
                 <div class="video-card">
                    <div class="video-thumbnail">▶ Video Player</div>
                    <div class="video-info">
                        <div class="video-title">Highlights Sepakbola</div>
                        <div class="video-meta">100k views • 30 menit yang lalu</div>
                    </div>
                </div>
            </div>
        </section>

        <section id="messages" class="section">
            <h2>Pesan Pribadi</h2>
            <div class="chat-container">
                <div class="chat-list">
                    <div class="chat-user"><b>Budi Santoso</b><br><small>Halo, apa kabar?</small></div>
                    <div class="chat-user"><b>Siti Aminah</b><br><small>Videonya keren banget!</small></div>
                    <div class="chat-user"><b>Andi Tech</b><br><small>Kapan collab?</small></div>
                </div>
                <div class="chat-area">
                    <div style="padding:15px; border-bottom:1px solid #eee; font-weight:bold;">Budi Santoso</div>
                    <div class="chat-messages">
                        <p style="background:#eee; padding:10px; border-radius:10px; width:fit-content;">Halo, apa kabar bro?</p>
                        <p style="background:var(--primary); color:white; padding:10px; border-radius:10px; width:fit-content; margin-left:auto; margin-top:10px;">Halo Budi! Baik, kamu gimana?</p>
                    </div>
                    <div class="chat-input">
                        <input type="text" placeholder="Ketik pesan...">
                        <button class="btn">Kirim</button>
                    </div>
                </div>
            </div>
        </section>

        <section id="notifications" class="section">
            <h2>Notifikasi</h2>
            <div class="settings-card">
                <div class="setting-row">🎉 <b>Rina</b> menyukai video Anda <small>2m lalu</small></div>
                <div class="setting-row">💬 <b>Joko</b> mengomentari postingan Anda <small>10m lalu</small></div>
                <div class="setting-row">📹 <b>TechDaily</b> mengupload video baru <small>1j lalu</small></div>
            </div>
        </section>

        <section id="profile" class="section">
            <h2>Profil Saya</h2>
            <div class="settings-card" style="text-align:center;">
                <div style="width:100px; height:100px; background:#ddd; border-radius:50%; margin:0 auto 15px;"></div>
                <h3>User SocialVid</h3>
                <p>Content Creator | Jakarta</p>
                <button class="btn" style="margin-top:10px;">Edit Profil</button>
            </div>
        </section>

        <section id="privacy" class="section">
            <h2>Privasi & Keamanan</h2>
            <div class="settings-card">
                <div class="setting-row"><span>Akun Privat</span> <input type="checkbox"></div>
                <div class="setting-row"><span>Status Online</span> <input type="checkbox" checked></div>
                <div class="setting-row"><span>Blokir Pengguna</span> <button class="btn" style="font-size:12px;">Lihat</button></div>
            </div>
        </section>

        <section id="storage" class="section">
            <h2>Penyimpanan Data</h2>
            <div class="settings-card">
                <div class="setting-row"><span>Cache Aplikasi</span> <span>120 MB</span></div>
                <div class="setting-row"><span>Unduhan Video</span> <span>1.2 GB</span></div>
                <div class="setting-row"><button class="btn" style="background:red;">Hapus Cache</button></div>
            </div>
        </section>

        <section id="language" class="section">
            <h2>Bahasa</h2>
            <div class="settings-card">
                <select style="width:100%; padding:10px;">
                    <option>Bahasa Indonesia (Dipilih)</option>
                    <option>English</option>
                    <option>Español</option>
                </select>
            </div>
        </section>

        <section id="accessibility" class="section">
            <h2>Aksesibilitas</h2>
            <div class="settings-card">
                <div class="setting-row"><span>Mode Kontras Tinggi</span> <input type="checkbox"></div>
                <div class="setting-row"><span>Ukuran Teks</span> <input type="range"></div>
                <div class="setting-row"><span>Pembaca Layar</span> <input type="checkbox"></div>
            </div>
        </section>

        <section id="updates" class="section">
            <h2>Pembaharuan Aplikasi</h2>
            <div class="settings-card">
                <p>Versi Saat Ini: <b>1.0.0 (Beta)</b></p>
                <p style="color:green; margin-top:10px;">Aplikasi Anda sudah versi terbaru.</p>
                <button class="btn" style="margin-top:15px;">Cek Pembaharuan</button>
            </div>
        </section>

        <section id="help" class="section">
            <h2>Bantuan & Masukan</h2>
            <div class="settings-card">
                <textarea style="width:100%; height:100px; padding:10px; margin-bottom:10px;" placeholder="Tulis masalah atau masukan Anda..."></textarea>
                <button class="btn">Kirim Masukan</button>
            </div>
            <div class="settings-card">
                <h3>FAQ</h3>
                <p>Bagaimana cara mengganti password?</p>
                <p>Kenapa video tidak berputar?</p>
            </div>
        </section>

        <section id="about" class="section">
            <h2>Tentang Kami</h2>
            <div class="settings-card">
                <p><b>SocialVid</b> adalah platform media sosial masa depan yang menggabungkan hiburan video pendek dan komunikasi pesan instan yang lancar.</p>
                <br>
                <p>© 2023 SocialVid Inc. Dibuat di Indonesia.</p>
            </div>
        </section>

    </main>

    <script>
        // Fungsi Navigasi Tab
        function nav(sectionId) {
            // 1. Sembunyikan semua section
            document.querySelectorAll('.section').forEach(sec => {
                sec.classList.remove('active');
            });

            // 2. Tampilkan section yang dipilih
            document.getElementById(sectionId).classList.add('active');

            // 3. Update status aktif di menu sidebar
            document.querySelectorAll('.menu-item').forEach(item => {
                item.classList.remove('active');
            });
            // (Opsional: logic untuk highlight menu yang diklik bisa ditambahkan di sini dengan ID)
            
            // 4. Tutup sidebar di mobile setelah klik
            if(window.innerWidth <= 768) {
                document.getElementById('sidebar').classList.remove('show');
            }
        }

        // Fungsi Toggle Sidebar Mobile
        function toggleSidebar() {
            document.getElementById('sidebar').classList.toggle('show');
        }
    </script>
</body>
</html>
