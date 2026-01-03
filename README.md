#**SocialWeb**
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ChatConnect - Pesan Singkat Cepat</title>
    <style>
        /* --- 1. CSS VARIABLES & RESET --- */
        :root {
            --primary-color: #00a884; /* Warna hijau khas aplikasi chat */
            --bg-body: #d1d7db;
            --bg-sidebar: #ffffff;
            --bg-chat: #efeae2;
            --message-out: #d9fdd3;
            --message-in: #ffffff;
            --text-primary: #111b21;
            --text-secondary: #667781;
            --border-color: #e9edef;
            --danger: #ef5350;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; }
        body { background-color: var(--bg-body); height: 100vh; display: flex; align-items: center; justify-content: center; overflow: hidden; }
        
        /* --- 2. LAYOUT UTAMA --- */
        .app-container {
            width: 100%; height: 100%;
            max-width: 1400px;
            background: white;
            display: flex;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
            overflow: hidden;
        }
        
        @media (min-width: 1000px) { .app-container { height: 95vh; width: 95vw; border-radius: 0; } }

        /* --- 3. SIDEBAR (MENU) --- */
        .sidebar {
            width: 350px;
            background: var(--bg-sidebar);
            border-right: 1px solid var(--border-color);
            display: flex;
            flex-direction: column;
            transition: transform 0.3s ease;
        }
        
        .sidebar-header {
            padding: 15px 20px;
            background: #f0f2f5;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-bottom: 1px solid var(--border-color);
        }
        
        .user-avatar { width: 40px; height: 40px; background: #ccc; border-radius: 50%; cursor: pointer; }
        .nav-icons { display: flex; gap: 15px; color: var(--text-secondary); cursor: pointer; }

        .menu-list { overflow-y: auto; flex: 1; }
        .menu-item {
            padding: 15px 20px;
            display: flex;
            align-items: center;
            cursor: pointer;
            border-bottom: 1px solid #f5f5f5;
            transition: 0.2s;
        }
        .menu-item:hover { background-color: #f5f6f6; }
        .menu-item i { margin-right: 15px; width: 20px; text-align: center; color: var(--text-secondary); }
        .menu-text { font-size: 15px; color: var(--text-primary); }
        .active-menu { background-color: #f0f2f5; border-left: 4px solid var(--primary-color); }

        /* --- 4. MAIN CONTENT AREA --- */
        .main-content {
            flex: 1;
            background: var(--bg-chat);
            position: relative;
            display: flex;
            flex-direction: column;
        }

        /* Tampilan Chat (Home) */
        .chat-header {
            padding: 10px 20px;
            background: #f0f2f5;
            display: flex;
            align-items: center;
            border-bottom: 1px solid var(--border-color);
            height: 60px;
        }
        .chat-area {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            background-image: url('https://user-images.githubusercontent.com/15075759/28719144-86dc0f70-73b1-11e7-911d-60d70fcded21.png'); /* Pattern background */
            background-repeat: repeat;
            opacity: 0.9;
        }
        .message {
            max-width: 60%;
            padding: 8px 12px;
            margin-bottom: 10px;
            border-radius: 8px;
            font-size: 14px;
            line-height: 1.4;
            position: relative;
            box-shadow: 0 1px 1px rgba(0,0,0,0.1);
        }
        .msg-in { background: var(--message-in); align-self: flex-start; float: left; clear: both; }
        .msg-out { background: var(--message-out); align-self: flex-end; float: right; clear: both; }
        .msg-time { font-size: 10px; color: var(--text-secondary); float: right; margin-top: 5px; margin-left: 10px; }

        .chat-input-area {
            padding: 10px 20px;
            background: #f0f2f5;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .chat-input {
            flex: 1;
            padding: 12px;
            border: none;
            border-radius: 8px;
            outline: none;
            font-size: 15px;
        }
        .btn-send {
            background: none; border: none; font-size: 24px; color: var(--text-secondary); cursor: pointer;
        }
        .btn-send:hover { color: var(--primary-color); }

        /* Tampilan Halaman Settings/Lainnya */
        .page-section {
            display: none; /* Hidden by default */
            padding: 40px;
            background: white;
            height: 100%;
            overflow-y: auto;
            animation: fadeIn 0.3s;
        }
        .page-section.active { display: block; }
        .page-title { font-size: 24px; margin-bottom: 20px; color: var(--primary-color); border-bottom: 2px solid #eee; padding-bottom: 10px;}
        
        .settings-group { margin-bottom: 25px; }
        .settings-row {
            display: flex; justify-content: space-between; align-items: center;
            padding: 15px 0; border-bottom: 1px solid #f0f0f0;
        }
        .settings-label { font-weight: 500; color: var(--text-primary); }
        .settings-desc { font-size: 12px; color: var(--text-secondary); display: block; margin-top: 4px; }
        
        /* Elements */
        input[type="checkbox"] { transform: scale(1.2); accent-color: var(--primary-color); }
        select { padding: 8px; border-radius: 5px; border: 1px solid #ccc; }
        .btn { padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer; font-weight: bold; }
        .btn-primary { background: var(--primary-color); color: white; }
        .btn-danger { background: var(--danger); color: white; }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* RESPONSIVE MOBILE */
        .mobile-toggle { display: none; font-size: 24px; cursor: pointer; margin-right: 15px; }
        
        @media (max-width: 768px) {
            .app-container { flex-direction: column; height: 100vh; width: 100vw; }
            .sidebar { 
                position: absolute; z-index: 100; height: 100%; width: 80%; 
                transform: translateX(-100%); box-shadow: 2px 0 10px rgba(0,0,0,0.2);
            }
            .sidebar.open { transform: translateX(0); }
            .mobile-toggle { display: block; }
            .chat-header { padding-left: 10px; }
        }
    </style>
</head>
<body>

    <div class="app-container">
        
        <nav class="sidebar" id="sidebar">
            <div class="sidebar-header">
                <div style="display:flex; align-items:center; gap:10px;">
                    <div class="user-avatar" onclick="showPage('profile')" style="display:flex; align-items:center; justify-content:center; color:white; font-weight:bold;">S</div>
                    <b>Menu Utama</b>
                </div>
                <div class="nav-icons" onclick="toggleSidebar()">✖</div>
            </div>

            <div class="menu-list">
                <div class="menu-item active-menu" onclick="showPage('home')">
                    <i>💬</i> <span class="menu-text">Home (Pesan)</span>
                </div>
                <div class="menu-item" onclick="showPage('notifications')">
                    <i>🔔</i> <span class="menu-text">Notifikasi</span>
                </div>
                <div class="menu-item" onclick="showPage('profile')">
                    <i>👤</i> <span class="menu-text">Profil Saya</span>
                </div>
                
                <div style="padding:15px; font-size:12px; color:gray; font-weight:bold;">PENGATURAN</div>
                
                <div class="menu-item" onclick="showPage('privacy')">
                    <i>🔒</i> <span class="menu-text">Privasi</span>
                </div>
                <div class="menu-item" onclick="showPage('storage')">
                    <i>💾</i> <span class="menu-text">Penyimpanan Data</span>
                </div>
                <div class="menu-item" onclick="showPage('language')">
                    <i>🌍</i> <span class="menu-text">Bahasa</span>
                </div>
                <div class="menu-item" onclick="showPage('accessibility')">
                    <i>👓</i> <span class="menu-text">Aksesibilitas</span>
                </div>
                
                <div style="padding:15px; font-size:12px; color:gray; font-weight:bold;">LAINNYA</div>

                <div class="menu-item" onclick="showPage('updates')">
                    <i>🔄</i> <span class="menu-text">Pembaharuan Aplikasi</span>
                </div>
                <div class="menu-item" onclick="showPage('help')">
                    <i>❓</i> <span class="menu-text">Bantuan & Masukan</span>
                </div>
                <div class="menu-item" onclick="showPage('about')">
                    <i>ℹ️</i> <span class="menu-text">Tentang Kami</span>
                </div>
            </div>
        </nav>

        <main class="main-content">
            
            <div id="home" class="page-section active" style="padding:0; display:flex; flex-direction:column; height:100%;">
                <div class="chat-header">
                    <div class="mobile-toggle" onclick="toggleSidebar()">☰</div>
                    <div class="user-avatar" style="width:35px; height:35px; margin-right:10px;"></div>
                    <div>
                        <div style="font-weight:bold;">Grup Diskusi</div>
                        <div style="font-size:12px; color:gray;">Budi, Siti, Anda...</div>
                    </div>
                </div>
                
                <div class="chat-area" id="chatArea">
                    <div class="message msg-in">
                        Halo semuanya, selamat datang di aplikasi ChatConnect!
                        <span class="msg-time">09:00</span>
                    </div>
                    <div class="message msg-in">
                        Tampilannya bersih banget ya.
                        <span class="msg-time">09:01</span>
                    </div>
                    <div class="message msg-out">
                        Iya, ini versi siap publish untuk frontend.
                        <span class="msg-time">09:05</span>
                    </div>
                </div>

                <div class="chat-input-area">
                    <button style="font-size:20px; border:none; background:none;">😊</button>
                    <input type="text" class="chat-input" id="msgInput" placeholder="Ketik pesan..." onkeypress="handleEnter(event)">
                    <button class="btn-send" onclick="sendMessage()">➤</button>
                </div>
            </div>

            <div id="profile" class="page-section">
                <h2 class="page-title">Profil Saya</h2>
                <div style="text-align:center; margin-bottom:30px;">
                    <div style="width:100px; height:100px; background:#ddd; border-radius:50%; margin:0 auto 15px;"></div>
                    <button class="btn btn-primary">Ganti Foto</button>
                </div>
                <div class="settings-group">
                    <div class="settings-label">Nama Pengguna</div>
                    <input type="text" value="User ChatConnect" style="width:100%; padding:10px; margin-top:5px;">
                </div>
                <div class="settings-group">
                    <div class="settings-label">Info (Status)</div>
                    <input type="text" value="Ada untuk bekerja" style="width:100%; padding:10px; margin-top:5px;">
                </div>
            </div>

            <div id="notifications" class="page-section">
                <h2 class="page-title">Notifikasi</h2>
                <div class="settings-row">
                    <div>
                        <div class="settings-label">Suara Pesan Masuk</div>
                        <span class="settings-desc">Mainkan suara saat pesan diterima</span>
                    </div>
                    <input type="checkbox" checked>
                </div>
                <div class="settings-row">
                    <div>
                        <div class="settings-label">Tampilkan Pratinjau</div>
                        <span class="settings-desc">Lihat isi pesan di notifikasi bar</span>
                    </div>
                    <input type="checkbox" checked>
                </div>
            </div>

            <div id="privasi" class="page-section">
                <h2 class="page-title">Privasi</h2>
                <div class="settings-row">
                    <div class="settings-label">Terakhir Dilihat (Last Seen)</div>
                    <select><option>Semua Orang</option><option>Kontak Saya</option><option>Tidak Ada</option></select>
                </div>
                <div class="settings-row">
                    <div class="settings-label">Laporan Dibaca (Centang Biru)</div>
                    <input type="checkbox" checked>
                </div>
                <div class="settings-row">
                    <div class="settings-label">Kunci Aplikasi (Fingerprint)</div>
                    <input type="checkbox">
                </div>
            </div>

            <div id="storage" class="page-section">
                <h2 class="page-title">Penyimpanan & Data</h2>
                <div class="settings-group">
                    <div class="settings-label" style="margin-bottom:10px;">Penggunaan Penyimpanan</div>
                    <div style="background:#eee; height:20px; border-radius:10px; overflow:hidden;">
                        <div style="width:30%; background:var(--primary-color); height:100%;"></div>
                    </div>
                    <span class="settings-desc">300 MB terpakai dari 2 GB</span>
                </div>
                <div class="settings-row">
                    <div class="settings-label">Unduh Otomatis Media</div>
                    <input type="checkbox" checked>
                </div>
                <button class="btn btn-danger">Hapus Cache</button>
            </div>

            <div id="language" class="page-section">
                <h2 class="page-title">Bahasa Aplikasi</h2>
                <div class="settings-row">
                    <div class="settings-label">Pilih Bahasa</div>
                    <select>
                        <option>Bahasa Indonesia (Dipilih)</option>
                        <option>English (US)</option>
                        <option>Jawa</option>
                    </select>
                </div>
            </div>

            <div id="accessibility" class="page-section">
                <h2 class="page-title">Aksesibilitas</h2>
                <div class="settings-row">
                    <div class="settings-label">Ukuran Font</div>
                    <input type="range" min="1" max="3">
                </div>
                <div class="settings-row">
                    <div class="settings-label">Mode Kontras Tinggi</div>
                    <input type="checkbox">
                </div>
            </div>

            <div id="updates" class="page-section">
                <h2 class="page-title">Pembaharuan Aplikasi</h2>
                <div style="text-align:center; padding:20px;">
                    <img src="https://via.placeholder.com/100" alt="Logo" style="margin-bottom:10px;">
                    <h3>Versi 1.0.2 (Terbaru)</h3>
                    <p style="color:green; margin:10px 0;">Sistem Anda sudah diperbarui.</p>
                    <button class="btn btn-primary">Cek Update Manual</button>
                </div>
            </div>

            <div id="help" class="page-section">
                <h2 class="page-title">Bantuan & Masukan</h2>
                <div class="settings-group">
                    <div class="settings-label">Kirim Masukan</div>
                    <textarea style="width:100%; height:100px; padding:10px; margin-top:5px; border:1px solid #ccc; border-radius:5px;" placeholder="Jelaskan masalah Anda..."></textarea>
                    <button class="btn btn-primary" style="margin-top:10px;">Kirim</button>
                </div>
                <div class="settings-group">
                    <h3>FAQ</h3>
                    <ul style="margin-left:20px; color:var(--text-secondary); margin-top:10px;">
                        <li>Bagaimana cara memblokir kontak?</li>
                        <li>Kenapa notifikasi tidak muncul?</li>
                    </ul>
                </div>
            </div>

            <div id="about" class="page-section">
                <h2 class="page-title">Tentang Kami</h2>
                <p><b>ChatConnect</b> adalah platform pengiriman pesan cepat yang mengutamakan privasi dan kemudahan penggunaan.</p>
                <br>
                <p>Dikembangkan oleh Tim Developer (Prototype).</p>
                <p>© 2024 All Rights Reserved.</p>
            </div>

            <script>
                // Logic Navigasi Halaman
                function showPage(pageId) {
                    // 1. Sembunyikan semua section
                    document.querySelectorAll('.page-section').forEach(el => el.classList.remove('active'));
                    
                    // 2. Fix typo ID manual jika ada
                    let targetId = pageId;
                    if(pageId === 'privacy') targetId = 'privasi'; 

                    // 3. Tampilkan section target
                    const target = document.getElementById(targetId);
                    if(target) target.classList.add('active');

                    // 4. Update Sidebar Active State
                    document.querySelectorAll('.menu-item').forEach(el => el.classList.remove('active-menu'));
                    // (Logic visual active state bisa disempurnakan dengan ID unik per menu)
                    
                    // 5. Tutup sidebar di mobile
                    if(window.innerWidth <= 768) {
                        document.getElementById('sidebar').classList.remove('open');
                    }
                }

                // Logic Kirim Pesan (Simulasi)
                function sendMessage() {
                    const input = document.getElementById('msgInput');
                    const text = input.value.trim();
                    
                    if(text) {
                        const chatArea = document.getElementById('chatArea');
                        
                        // Buat elemen pesan baru
                        const div = document.createElement('div');
                        div.className = 'message msg-out';
                        
                        // Waktu saat ini
                        const now = new Date();
                        const time = now.getHours() + ":" + String(now.getMinutes()).padStart(2, '0');
                        
                        div.innerHTML = `${text} <span class="msg-time">${time}</span>`;
                        
                        chatArea.appendChild(div);
                        input.value = '';
                        
                        // Scroll ke bawah
                        chatArea.scrollTop = chatArea.scrollHeight;
                    }
                }

                function handleEnter(e) {
                    if(e.key === 'Enter') sendMessage();
                }

                // Logic Sidebar Mobile
                function toggleSidebar() {
                    document.getElementById('sidebar').classList.toggle('open');
                }
            </script>

        </main>
    </div>

</body>
</html>
