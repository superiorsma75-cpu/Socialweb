<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ChatConnect - Ultimate Communication</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        :root {
            --primary-color: #008069;
            --primary-light: #25D366;
            --check-read-color: #53bdeb;
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

        body.dark-theme {
            --primary-color: #00a884;
            --primary-light: #008069;
            --bg-color: #0b141a;
            --sidebar-bg: #111b21;
            --text-dark: #f0f2f5;
            --text-gray: #aebac1;
            --border-color: #202c33;
            --white: #202c33;
        }

        body.high-contrast {
            --primary-color: #004d40;
            --bg-color: #000000;
            --sidebar-bg: #000000;
            --text-dark: #ffffff;
            --text-gray: #eeeeee;
            --border-color: #ffffff;
            --white: #000000;
            --check-read-color: #00ff00;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body { background-color: var(--bg-color); color: var(--text-dark); height: 100vh; display: flex; overflow: hidden; }

        /* --- Sidebar --- */
        .sidebar { width: 300px; background-color: var(--sidebar-bg); border-right: 1px solid var(--border-color); display: flex; flex-direction: column; transition: var(--transition); z-index: 100; }
        .brand { padding: 20px; background-color: var(--primary-color); color: white; display: flex; align-items: center; gap: 10px; font-size: 1.2rem; font-weight: bold; }
        .nav-menu { flex: 1; overflow-y: auto; padding: 10px 0; }
        .nav-category { font-size: 0.8rem; text-transform: uppercase; color: var(--primary-color); padding: 15px 20px 5px; font-weight: bold; letter-spacing: 0.5px; }
        body.dark-theme .nav-category, body.high-contrast .nav-category { color: var(--primary-light); }
        .nav-item { display: flex; align-items: center; padding: 12px 20px; cursor: pointer; color: var(--text-dark); text-decoration: none; transition: background 0.2s; font-size: 0.95rem; border-left: 4px solid transparent; }
        .nav-item:hover { background-color: rgba(0, 128, 105, 0.1); }
        .nav-item.active { background-color: rgba(0, 128, 105, 0.1); border-left-color: var(--primary-color); font-weight: 600; }
        .nav-item i { width: 25px; color: var(--text-gray); margin-right: 10px; }
        .nav-item.active i { color: var(--primary-color); }
        .nav-subitem { padding-left: 56px; font-size: 0.9rem; }

        /* --- Main Content --- */
        .main-content { flex: 1; display: flex; flex-direction: column; position: relative; background-image: url('https://picsum.photos/seed/bgpattern/800/600?blur=5'); background-size: cover; background-blend-mode: overlay; background-color: rgba(255, 255, 255, 0.9); }
        body.dark-theme .main-content, body.high-contrast .main-content { background-blend-mode: normal; background-color: var(--sidebar-bg); background-image: none; }
        .mobile-header { display: none; padding: 15px; background-color: var(--primary-color); color: white; align-items: center; justify-content: space-between; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        .section { display: none; flex: 1; flex-direction: column; height: 100%; overflow: hidden; animation: fadeIn 0.3s ease; }
        .section.active-section { display: flex; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* --- Components --- */
        h2 { margin-bottom: 20px; color: var(--primary-color); border-bottom: 2px solid var(--border-color); padding-bottom: 10px; }
        .card { background: var(--white); border-radius: 8px; padding: 20px; box-shadow: var(--shadow); margin-bottom: 20px; }
        
        /* --- Security Section --- */
        #section-security { background-color: var(--bg-color); align-items: center; justify-content: center; background-image: none; background-blend-mode: normal; }
        body.dark-theme #section-security { background-color: #0b141a; }
        body.high-contrast #section-security { background-color: #000000; }
        .security-card { background: var(--white); padding: 40px; border-radius: 12px; box-shadow: 0 10px 25px rgba(0,0,0,0.15); width: 100%; max-width: 400px; text-align: center; border-top: 5px solid var(--primary-color); }
        body.dark-theme .security-card { background: var(--sidebar-bg); }
        .security-icon { font-size: 4rem; color: var(--primary-color); margin-bottom: 20px; }
        .otp-input { letter-spacing: 8px; font-size: 1.5rem; text-align: center; font-weight: bold; }

        /* --- Home: Tabs (Chat | Status | Calls) --- */
        #section-home { display: none; flex-direction: row; overflow: hidden; position: relative; }
        #section-home.active-section { display: flex; }
        .left-panel { width: 400px; background: var(--white); border-right: 1px solid var(--border-color); display: flex; flex-direction: column; position: relative; z-index: 10; }
        
        .app-tabs { display: flex; background: var(--bg-color); padding: 10px 15px; border-bottom: 1px solid var(--border-color); justify-content: space-between; color: var(--text-gray); font-weight: 600; cursor: pointer; }
        .app-tab { padding: 5px 15px; border-radius: 10px; color: var(--text-gray); transition: 0.2s; position: relative; }
        .app-tab.active { color: var(--text-dark); background-color: #d1d7db; }
        body.dark-theme .app-tab.active { background-color: #202c33; }
        .tab-content { flex: 1; overflow-y: auto; display: none; }
        .tab-content.active { display: flex; flex-direction: column; }

        /* --- Action Bar --- */
        .chat-action-bar { padding: 10px 15px; background: var(--white); border-bottom: 1px solid var(--border-color); display: flex; gap: 10px; flex-wrap: wrap; }
        .action-btn { flex: 1; padding: 8px; border: none; background: none; color: var(--text-dark); font-size: 0.85rem; display: flex; align-items: center; justify-content: center; gap: 5px; cursor: pointer; border-radius: 5px; transition: background 0.2s; }
        .action-btn:hover { background-color: var(--bg-color); }
        .action-btn i { color: var(--primary-color); }

        /* Chat List */
        .chat-list-container { flex: 1; overflow-y: auto; }
        .chat-item { display: flex; align-items: center; padding: 15px; border-bottom: 1px solid var(--border-color); cursor: pointer; position: relative; }
        .chat-item:hover, .chat-item.active-chat { background-color: var(--bg-color); }
        .avatar-wrapper { position: relative; }
        .avatar { width: 50px; height: 50px; border-radius: 50%; object-fit: cover; margin-right: 15px; background-color: #ddd; }
        .status-dot { width: 12px; height: 12px; border-radius: 50%; border: 2px solid var(--white); position: absolute; bottom: 2px; right: 2px; }
        .status-dot.online { background-color: #25D366; }
        .chat-info h4 { font-size: 1rem; margin-bottom: 4px; }
        .chat-info p { font-size: 0.85rem; color: var(--text-gray); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; max-width: 200px; }

        /* Chat Window */
        .chat-window { flex: 1; display: flex; flex-direction: column; background-color: #efe7dd; background-image: url('https://user-images.githubusercontent.com/15075759/28719144-86dc0f70-73b1-11e7-911d-60d70fcded21.png'); position: relative; }
        body.dark-theme .chat-window, body.high-contrast .chat-window { background-color: var(--sidebar-bg); background-image: none; }
        
        .chat-header { padding: 10px 20px; background: var(--bg-color); border-bottom: 1px solid var(--border-color); display: flex; align-items: center; justify-content: space-between; z-index: 5; }
        .chat-header-actions i { margin-left: 15px; color: var(--primary-color); cursor: pointer; font-size: 1.2rem; }

        .chat-messages { flex: 1; padding: 20px; overflow-y: scroll; scroll-behavior: smooth; display: flex; flex-direction: column; gap: 10px; z-index: 1; scrollbar-width: thin; scrollbar-color: var(--text-gray) var(--bg-color); }
        body.dark-theme .chat-messages { scrollbar-color: var(--text-gray) var(--sidebar-bg); }
        .chat-messages::-webkit-scrollbar { width: 8px; }
        .chat-messages::-webkit-scrollbar-track { background: transparent; }
        .chat-messages::-webkit-scrollbar-thumb { background-color: rgba(0,0,0,0.2); border-radius: 4px; }

        .message { max-width: 80%; padding: 10px 15px; border-radius: 8px; position: relative; font-size: 0.95rem; line-height: 1.4; display: flex; flex-direction: column; word-wrap: break-word; }
        .message.received { background-color: var(--white); align-self: flex-start; border-top-left-radius: 0; }
        .message.sent { background-color: #dcf8c6; align-self: flex-end; border-top-right-radius: 0; }
        body.dark-theme .message.sent, body.high-contrast .message.sent { background-color: #005c4b; color: white; }
        body.dark-theme .message.received, body.high-contrast .message.received { background-color: #202c33; color: white; }

        .msg-image { max-width: 100%; border-radius: 5px; cursor: pointer; }
        .msg-video { max-width: 100%; border-radius: 5px; display: block; }
        .doc-preview { display: flex; align-items: center; gap: 10px; background: rgba(0,0,0,0.05); padding: 10px; border-radius: 5px; }
        .doc-preview i { font-size: 1.5rem; color: var(--text-gray); }
        .voice-message { display: flex; align-items: center; gap: 10px; width: 200px; }
        .voice-wave { flex: 1; height: 20px; display: flex; align-items: center; gap: 2px; }
        .voice-bar { width: 3px; background: var(--text-dark); height: 10px; border-radius: 2px; }
        .location-img { width: 100%; border-radius: 5px; margin-bottom: 5px; }
        .contact-card { background: rgba(255,255,255,0.1); padding: 10px; border-radius: 5px; display: flex; align-items: center; gap: 10px; border-left: 3px solid var(--primary-color); }
        .contact-avatar { width: 40px; height: 40px; border-radius: 50%; }

        .message-meta { align-self: flex-end; display: flex; align-items: center; margin-top: 4px; gap: 4px; font-size: 0.75rem; color: rgba(0,0,0,0.45); }
        body.dark-theme .message-meta, body.high-contrast .message-meta { color: rgba(255,255,255,0.6); }
        .fa-check { font-size: 0.9rem; color: rgba(0,0,0,0.45); }
        body.dark-theme .fa-check, body.high-contrast .fa-check { color: rgba(255,255,255,0.6); }
        .fa-check.read { color: var(--check-read-color); }

        .chat-input-area { background: var(--bg-color); padding: 10px 20px; display: flex; align-items: center; gap: 10px; position: relative; z-index: 10; }
        .chat-input { flex: 1; padding: 12px; border-radius: 20px; border: 1px solid var(--border-color); background: var(--white); color: var(--text-dark); outline: none; }
        .btn-icon { background: none; border: none; color: var(--text-gray); cursor: pointer; font-size: 1.2rem; padding: 5px; }
        .btn-icon:hover { color: var(--primary-color); }
        .btn-send { background: var(--primary-color); color: white; border: none; width: 40px; height: 40px; border-radius: 50%; cursor: pointer; display: flex; align-items: center; justify-content: center; transition: transform 0.2s; }
        .btn-send:hover { transform: scale(1.1); }

        /* --- Modals --- */
        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); z-index: 2000; display: none; align-items: center; justify-content: center; }
        .modal-card { background: var(--white); width: 100%; max-width: 400px; border-radius: 8px; overflow: hidden; display: flex; flex-direction: column; animation: slideUp 0.3s ease; }
        body.dark-theme .modal-card { background: var(--sidebar-bg); }
        @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
        
        .modal-header { padding: 15px; border-bottom: 1px solid var(--border-color); display: flex; justify-content: space-between; align-items: center; font-weight: bold; }
        .modal-body { padding: 20px; max-height: 70vh; overflow-y: auto; }
        .modal-footer { padding: 15px; border-top: 1px solid var(--border-color); text-align: right; }

        /* Camera Options Modal Specifics */
        .camera-options-grid { display: grid; grid-template-columns: 1fr; gap: 15px; }
        .camera-option-btn { width: 100%; padding: 15px; border: 1px solid var(--border-color); border-radius: 8px; background: var(--white); color: var(--text-dark); cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 10px; font-size: 1rem; transition: 0.2s; }
        .camera-option-btn:hover { background: var(--bg-color); }
        body.dark-theme .camera-option-btn { background: var(--sidebar-bg); border-color: var(--border-color); color: var(--text-dark); }
        .camera-option-btn:hover { background: rgba(255,255,255,0.1); }

        .call-screen { text-align: center; color: white; }
        .call-avatar { width: 120px; height: 120px; border-radius: 50%; margin-bottom: 20px; border: 4px solid var(--white); }
        .call-timer { font-size: 2.5rem; margin-bottom: 30px; font-weight: bold; }
        .call-actions { display: flex; gap: 20px; justify-content: center; margin-top: 20px; }
        .call-btn { width: 60px; height: 60px; border-radius: 50%; border: none; cursor: pointer; display: flex; align-items: center; justify-content: center; font-size: 1.5rem; transition: 0.2s; }
        .btn-mute { background: rgba(255,255,255,0.2); color: white; }
        .btn-end { background: var(--danger); color: white; }

        /* Attachment Menu */
        .attach-menu-container { position: absolute; bottom: 70px; left: 20px; background-color: var(--white); border: 1px solid var(--border-color); border-radius: 8px; box-shadow: 0 4px 15px rgba(0,0,0,0.2); padding: 15px; width: 300px; display: none; grid-template-columns: repeat(3, 1fr); gap: 15px; z-index: 999; }
        body.dark-theme .attach-menu-container { background-color: var(--sidebar-bg); }
        .attach-menu-container.active { display: grid; }
        .attach-item { display: flex; flex-direction: column; align-items: center; cursor: pointer; gap: 5px; }
        .attach-icon-box { width: 40px; height: 40px; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-size: 1.2rem; }
        .attach-item span { font-size: 0.75rem; color: var(--text-dark); text-align: center; }
        .bg-gallery { background-color: #008069; }
        .bg-video { background-color: #ff0000; }
        .bg-location { background-color: #4285F4; }
        .bg-contact { background-color: #25D366; }
        .bg-doc { background-color: #f7bb07; color: #333; }
        .bg-audio { background-color: #00a884; }

        /* Status Modal */
        .status-modal { width: 100%; max-width: 400px; height: 80vh; max-height: 700px; background: black; position: relative; border-radius: 8px; overflow: hidden; display: flex; align-items: center; justify-content: center; }
        .status-img { width: 100%; height: 100%; object-fit: contain; }
        .status-progress { position: absolute; top: 20px; left: 20px; right: 20px; height: 3px; background: rgba(255,255,255,0.3); border-radius: 2px; overflow: hidden; }
        .status-bar-fill { height: 100%; background: var(--white); width: 0%; }

        /* Emoji Picker */
        .emoji-picker-container { position: absolute; bottom: 70px; right: 20px; background-color: var(--white); border: 1px solid var(--border-color); border-radius: 8px; box-shadow: 0 4px 15px rgba(0,0,0,0.2); padding: 10px; width: 280px; display: none; flex-wrap: wrap; gap: 5px; z-index: 999; }
        body.dark-theme .emoji-picker-container { background-color: var(--sidebar-bg); }
        .emoji-picker-container.active { display: flex; }
        .emoji-btn { font-size: 1.5rem; cursor: pointer; padding: 5px; border: none; background: none; transition: background 0.2s; }
        .emoji-btn:hover { background-color: rgba(0,0,0,0.1); }

        /* Form & Toggles */
        .form-group { margin-bottom: 20px; }
        .form-group label { display: block; margin-bottom: 8px; font-weight: 600; }
        .form-control { width: 100%; padding: 10px; border: 1px solid var(--border-color); border-radius: 5px; font-size: 1rem; background: var(--white); color: var(--text-dark); }
        .input-group { display: flex; gap: 10px; }
        .toggle-row { display: flex; justify-content: space-between; align-items: center; padding: 10px 0; border-bottom: 1px solid var(--border-color); }
        .switch { position: relative; display: inline-block; width: 50px; height: 24px; }
        .switch input { opacity: 0; width: 0; height: 0; }
        .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: #ccc; transition: .4s; border-radius: 24px; }
        .slider:before { position: absolute; content: ""; height: 16px; width: 16px; left: 4px; bottom: 4px; background-color: white; transition: .4s; border-radius: 50%; }
        input:checked + .slider { background-color: var(--primary-color); }
        input:checked + .slider:before { transform: translateX(26px); }
        
        .btn { padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer; font-weight: bold; transition: opacity 0.2s; display: inline-flex; align-items: center; justify-content: center; gap: 8px; background: var(--primary-color); color: white; width: 100%; }
        .btn:hover { opacity: 0.9; }
        .btn-secondary { background: #f0f2f5; color: var(--text-dark); }
        .btn-danger { background-color: var(--danger); }
        .btn-text { background: transparent; color: var(--primary-color); text-align: left; width: 100%; padding-left: 0; }
        .btn-text:hover { text-decoration: underline; }

        .toast { position: fixed; bottom: 20px; right: 20px; background-color: #333; color: white; padding: 12px 24px; border-radius: 5px; box-shadow: 0 4px 10px rgba(0,0,0,0.2); transform: translateY(100px); opacity: 0; transition: all 0.4s cubic-bezier(0.68, -0.55, 0.27, 1.55); z-index: 1000; }
        .toast.show { transform: translateY(0); opacity: 1; }

        /* Mobile */
        @media (max-width: 768px) {
            .sidebar { position: fixed; left: -300px; height: 100%; box-shadow: 2px 0 10px rgba(0,0,0,0.2); }
            .sidebar.open { left: 0; }
            .mobile-header { display: flex; }
            #section-home { flex-direction: column; }
            .left-panel { width: 100%; display: none; }
            .left-panel.active { display: flex; }
            .chat-window { display: none; }
            .chat-window.active { display: flex; width: 100%; }
            .emoji-picker-container, .attach-menu-container { width: 90%; right: 5%; bottom: 60px; }
            .attach-menu-container { left: auto; }
        }
    </style>
</head>
<body>

    <div id="toast" class="toast">Berhasil</div>

    <!-- SECTION: SECURITY -->
    <section id="section-security" class="section active-section">
        <div class="security-card">
            <i class="fas fa-shield-alt security-icon"></i>
            <h2 class="security-title">Verifikasi Keamanan</h2>
            <p class="security-desc">Anda masuk sebagai pengguna baru.<br>Silakan verifikasi nomor telepon Anda.</p>
            <div id="step1-phone">
                <div class="form-group" style="text-align: left;"><label>Nomor Telepon</label><input type="tel" class="form-control" id="verifyPhone" placeholder="+62 812 XXXX XXXX"></div>
                <button class="btn" onclick="sendVerificationCode()"><i class="fas fa-paper-plane"></i> Kirim Kode</button>
            </div>
            <div id="step2-otp" style="display: none;">
                <div class="form-group" style="text-align: left;"><label>Kode Verifikasi (OTP)</label><input type="text" class="form-control otp-input" id="verifyCode" placeholder="123456" maxlength="6"><small style="color: var(--text-gray); display: block; margin-top: 5px;">Gunakan kode <strong>123456</strong></small></div>
                <button class="btn" onclick="verifyCode()"><i class="fas fa-check-circle"></i> Verifikasi</button>
            </div>
        </div>
    </section>

    <!-- MODAL: Pilihan Kamera (NEW) -->
    <div id="cameraOptionsModal" class="modal-overlay">
        <div class="modal-card">
            <div class="modal-header"><span>Pilih Sumber Foto</span><span style="cursor: pointer;" onclick="closeModal('cameraOptionsModal')">&times;</span></div>
            <div class="modal-body camera-options-grid">
                <!-- 1. GALERI -->
                <button class="camera-option-btn" onclick="triggerCamera('gallery')">
                    <i class="fas fa-images fa-lg"></i>
                    <div>Buka Galeri</div>
                </button>
                
                <!-- 2. KAMERA DEPAN -->
                <button class="camera-option-btn" style="color: #4285F4;" onclick="triggerCamera('front')">
                    <i class="fas fa-user fa-lg"></i>
                    <div>Kamera Depan</div>
                </button>
                
                <!-- 3. KAMERA BELAKANG -->
                <button class="camera-option-btn" style="color: var(--primary-color);" onclick="triggerCamera('rear')">
                    <i class="fas fa-video fa-lg"></i>
                    <div>Kamera Belakang</div>
                </button>
            </div>
        </div>
    </div>

    <!-- MODAL: Buat Grup Baru -->
    <div id="newGroupModal" class="modal-overlay">
        <div class="modal-card">
            <div class="modal-header"><span>Buat Grup Baru</span><span style="cursor: pointer;" onclick="closeModal('newGroupModal')">&times;</span></div>
            <div class="modal-body">
                <div class="form-group"><label>Nama Grup</label><input type="text" id="groupNameInput" class="form-control" placeholder="Contoh: Alumni SMA 2020"></div>
                <div class="form-group"><label>Pilih Anggota</label><div id="memberSelectionList"></div></div>
            </div>
            <div class="modal-footer"><button class="btn btn-secondary" onclick="closeModal('newGroupModal')">Batal</button><button class="btn" onclick="confirmCreateGroup()">Buat Grup</button></div>
        </div>
    </div>

    <!-- MODAL: Chat Baru -->
    <div id="newChatModal" class="modal-overlay">
        <div class="modal-card">
            <div class="modal-header"><span>Mulai Chat Baru</span><span style="cursor: pointer;" onclick="closeModal('newChatModal')">&times;</span></div>
            <div class="modal-body">
                <div class="form-group"><label>Cari Kontak / Masukkan Nomor</label><input type="text" id="newChatName" class="form-control" placeholder="Nama atau +62..."></div>
            </div>
            <div class="modal-footer"><button class="btn" onclick="confirmNewChat()">Lanjut</button></div>
        </div>
    </div>

    <!-- MODAL: Panggilan -->
    <div id="callModal" class="modal-overlay" style="background: rgba(0,0,0,0.8);">
        <div class="call-screen">
            <img id="callAvatar" src="https://picsum.photos/seed/budi/150/150" class="call-avatar">
            <h2 id="callName">Budi Santoso</h2>
            <p id="callStatus" class="call-status">Memanggil...</p>
            <div id="callTimer" class="call-timer">00:00</div>
            <div class="call-actions">
                <button class="call-btn btn-mute" onclick="toggleMute()"><i class="fas fa-microphone-slash"></i></button>
                <button class="call-btn btn-end" onclick="endCall()"><i class="fas fa-phone-slash"></i></button>
            </div>
        </div>
    </div>

    <!-- MODAL: Status -->
    <div id="statusModal" class="modal-overlay" style="background: rgba(0,0,0,0.9);">
        <div class="status-modal">
            <span class="close-status" onclick="closeStatusViewer()" style="position:absolute;top:30px;right:20px;font-size:2rem;cursor:pointer;color:white;z-index:10;">&times;</span>
            <div class="status-progress"><div id="statusBar" class="status-bar-fill"></div></div>
            <img id="statusImage" src="" class="status-img">
        </div>
    </div>

    <header class="mobile-header">
        <i class="fas fa-bars fa-lg" onclick="toggleSidebar()"></i>
        <span>ChatConnect</span>
        <div style="width: 24px;"></div> 
    </header>

    <nav class="sidebar" id="sidebar">
        <div class="brand"><i class="fas fa-comments"></i> ChatConnect</div>
        <div class="nav-menu">
            <div class="nav-category">Utama</div>
            <a onclick="showSection('home')" class="nav-item" id="nav-home"><i class="fas fa-home"></i> Home</a>
            <a onclick="showSection('profile')" class="nav-item" id="nav-profile"><i class="fas fa-user"></i> Profil</a>
            <a onclick="showSection('about')" class="nav-item" id="nav-about"><i class="fas fa-info-circle"></i> Tentang Kami</a>

            <div class="nav-category">Pengaturan</div>
            <a onclick="toggleSettings()" class="nav-item" id="nav-settings"><i class="fas fa-cog"></i> Pengaturan</a>
            <a onclick="showSection('privacy')" class="nav-item nav-subitem" id="nav-privacy"><i class="fas fa-lock"></i> Privasi</a>
            <a onclick="showSection('data')" class="nav-item nav-subitem" id="nav-data"><i class="fas fa-database"></i> Penyimpanan Data</a>
            <a onclick="showSection('display')" class="nav-item nav-subitem" id="nav-display"><i class="fas fa-paint-brush"></i> Tampilan</a>

            <div class="nav-category">Dukungan</div>
            <a onclick="showSection('help')" class="nav-item" id="nav-help"><i class="fas fa-question-circle"></i> Bantuan</a>
        </div>
    </nav>

    <main class="main-content">
        
        <!-- SECTION: HOME -->
        <section id="section-home" class="section">
            <div class="left-panel active" id="leftPanel">
                <div class="app-tabs">
                    <div class="app-tab active" onclick="switchMainTab('chat')">Chat</div>
                    <div class="app-tab" onclick="switchMainTab('status')">Status</div>
                    <div class="app-tab" onclick="switchMainTab('calls')">Panggilan</div>
                </div>

                <!-- Tab 1: Chat -->
                <div id="tab-chat" class="tab-content active">
                    <div class="chat-action-bar">
                        <button class="action-btn" onclick="openNewGroupModal()"><i class="fas fa-users"></i> Buat Grup</button>
                        <button class="action-btn" onclick="openNewChatModal()"><i class="fas fa-plus-circle"></i> Chat Baru</button>
                    </div>
                    <div class="chat-list-container" id="chatList">
                        <div class="chat-item active-chat" onclick="selectChat('Budi Santoso', 'https://picsum.photos/seed/budi/50/50', 'online')">
                            <div class="avatar-wrapper">
                                <img src="https://picsum.photos/seed/budi/50/50" class="avatar">
                                <div class="status-dot online"></div>
                            </div>
                            <div class="chat-info"><h4>Budi Santoso</h4><p>Halo, apa kabar?</p></div>
                        </div>
                    </div>
                </div>

                <!-- Tab 2: Status -->
                <div id="tab-status" class="tab-content">
                    <div class="status-scroll"><div class="status-item"><div class="my-status"><i class="fas fa-plus"></i></div><span class="status-name">Status Saya</span></div></div>
                </div>

                <!-- Tab 3: Calls -->
                <div id="tab-calls" class="tab-content">
                    <div class="call-item" onclick="startCall('Budi Santoso', 'video', 'https://picsum.photos/seed/budi/150/150')">
                        <div class="call-icon incoming"><i class="fas fa-video"></i></div>
                        <div class="call-info"><h4>Budi Santoso</h4><div style="font-size:0.8rem;color:var(--text-gray);">Hari ini, 10:30</div></div>
                    </div>
                </div>
            </div>

            <!-- Chat Window (Right Panel) -->
            <div class="chat-window" id="chatWindow">
                <div class="chat-header">
                    <div style="display:flex; align-items:center;">
                        <i class="fas fa-arrow-left" onclick="backToList()" style="margin-right:10px; display:none; cursor:pointer;" id="mobileBackBtn"></i>
                        <div>
                            <h3 id="currentChatName">Budi Santoso</h3>
                            <small id="currentChatStatus" style="color:var(--text-gray);">Online</small>
                        </div>
                    </div>
                    <div class="chat-header-actions">
                        <i class="fas fa-video" onclick="startCall(currentChatName.innerText, 'video', '')"></i>
                        <i class="fas fa-phone" onclick="startCall(currentChatName.innerText, 'voice', '')"></i>
                        <i class="fas fa-ellipsis-v" onclick="showSection('privacy')"></i>
                    </div>
                </div>

                <div class="chat-messages" id="messageContainer">
                    <div class="message received">Halo, apa kabar?<div class="message-meta"><span>10:30</span></div></div>
                </div>

                <div id="emojiPicker" class="emoji-picker-container"></div>
                <div id="attachMenu" class="attach-menu-container">
                    <div class="attach-item" onclick="document.getElementById('imgInput').click()"><div class="attach-icon-box bg-gallery"><i class="fas fa-image"></i></div><span>Galeri</span></div>
                    <div class="attach-item" onclick="document.getElementById('videoInput').click()"><div class="attach-icon-box bg-video"><i class="fas fa-video"></i></div><span>Video</span></div>
                    <div class="attach-item" onclick="document.getElementById('docInput').click()"><div class="attach-icon-box bg-doc"><i class="fas fa-file-alt"></i></div><span>Dokumen</span></div>
                    <div class="attach-item" onclick="sendLocation()"><div class="attach-icon-box bg-location"><i class="fas fa-map-marker-alt"></i></div><span>Lokasi</span></div>
                    <div class="attach-item" onclick="sendContact()"><div class="attach-icon-box bg-contact"><i class="fas fa-user"></i></div><span>Kontak</span></div>
                    <div class="attach-item" onclick="document.getElementById('audioInput').click()"><div class="attach-icon-box bg-audio"><i class="fas fa-music"></i></div><span>Audio</span></div>
                </div>

                <!-- Hidden Inputs -->
                <input type="file" id="imgInput" accept="image/*" style="display:none;" onchange="handleImageUpload(this)">
                <input type="file" id="videoInput" accept="video/*" style="display:none;" onchange="handleVideoUpload(this)">
                <input type="file" id="docInput" accept=".pdf,.doc,.docx" style="display:none;" onchange="handleDocUpload(this)">
                <input type="file" id="audioInput" accept="audio/*" style="display:none;" onchange="handleAudioUpload(this)">
                
                <!-- KAMERA: Input untuk Depan & Belakang -->
                <input type="file" id="frontCameraInput" accept="image/*" capture="user" style="display:none;" onchange="handleImageUpload(this)">
                <input type="file" id="rearCameraInput" accept="image/*" capture="environment" style="display:none;" onchange="handleImageUpload(this)">

                <div class="chat-input-area">
                    <button class="btn-icon" onclick="toggleAttachMenu()"><i class="fas fa-plus"></i></button>
                    <button class="btn-icon" onclick="toggleEmojiPicker()"><i class="far fa-smile"></i></button>
                    <input type="text" class="chat-input" id="messageInput" placeholder="Ketik pesan..." onkeypress="handleEnter(event)">
                    <button class="btn-icon" id="micBtn" onclick="toggleMicRecording()"><i class="fas fa-microphone"></i></button>
                    
                    <!-- Tombol Kamera (Update) -->
                    <button class="btn-icon" onclick="openCameraOptions()"><i class="fas fa-camera"></i></button>
                    
                    <button class="btn-send" id="sendBtn" onclick="sendMessage()"><i class="fas fa-paper-plane"></i></button>
                </div>
            </div>
        </section>

        <!-- SECTIONS LAINNYA (RINGKAS) -->
        <section id="section-privacy" class="section"><h2>Privasi</h2><div class="card"><div class="toggle-row"><span>Blokir Pengguna</span><label class="switch"><input type="checkbox" id="blockToggle" onchange="toggleBlockUser()"><span class="slider"></span></label></div></div></section>
        <section id="section-data" class="section"><h2>Data</h2><div class="card"><div class="toggle-row"><span>Bersihkan Obrolan</span><button class="btn btn-text" onclick="clearChat()"><i class="fas fa-trash"></i></button></div></div></section>
        <section id="section-profile" class="section"><h2>Profil</h2><div class="card" style="text-align:center;"><img id="displayProfileImg" src="https://picsum.photos/seed/me/150/150" style="border-radius:50%;width:100px;margin-bottom:15px;"><h3 id="displayProfileName">Nama</h3></div><div class="card"><input type="text" class="form-control" id="inputProfileName" value="Nama Pengguna"><button class="btn" onclick="saveProfileName()">Simpan</button></div></section>
        <section id="section-display" class="section"><h2>Tampilan</h2><div class="card"><div class="toggle-row"><span>Mode Gelap</span><label class="switch"><input type="checkbox" id="darkThemeToggle" onchange="toggleDarkTheme()"><span class="slider"></span></label></div></div></section>
        <section id="section-about" class="section"><h2>Tentang Kami</h2><div class="card"><p>ChatConnect v3.0</p></div></section>
        <section id="section-help" class="section"><h2>Bantuan</h2><div class="card"><p>Bantuan.</p></div></section>
    </main>

    <script>
        // --- CAMERA OPTIONS LOGIC (NEW) ---
        function openCameraOptions() {
            document.getElementById('cameraOptionsModal').style.display = 'flex';
        }

        function triggerCamera(mode) {
            let inputId = '';
            if(mode === 'gallery') {
                inputId = 'imgInput'; // Input Galeri biasa (tanpa capture)
            } else if (mode === 'front') {
                inputId = 'frontCameraInput'; // Input Kamera Depan
            } else if (mode === 'rear') {
                inputId = 'rearCameraInput'; // Input Kamera Belakang
            }

            document.getElementById(inputId).click();
            closeModal('cameraOptionsModal');
        }

        // --- SECURITY ---
        function sendVerificationCode() {
            const phone = document.getElementById('verifyPhone').value;
            if (phone === "") { showToast("Mohon masukkan nomor telepon!"); return; }
            document.getElementById('step1-phone').style.display = 'none';
            document.getElementById('step2-otp').style.display = 'block';
            showToast(`Kode verifikasi dikirim ke ${phone}`); showToast(`Gunakan kode: 123456`);
        }
        function verifyCode() {
            const code = document.getElementById('verifyCode').value;
            if (code === "123456") {
                showToast("Verifikasi Berhasil! Selamat datang.");
                document.getElementById('section-security').classList.remove('active-section');
                document.getElementById('section-home').classList.add('active-section');
                if(window.innerWidth > 768) { document.getElementById('chatWindow').style.display = 'flex'; }
            } else { showToast("Kode salah! Coba: 123456"); }
        }

        // --- Navigation ---
        function showSection(sectionId) {
            document.querySelectorAll('.section').forEach(sec => sec.classList.remove('active-section'));
            document.querySelectorAll('.nav-item').forEach(item => item.classList.remove('active'));
            document.getElementById('section-' + sectionId).classList.add('active-section');
            const navItem = document.getElementById('nav-' + sectionId);
            if(navItem) navItem.classList.add('active');
            if(['display','privacy','data'].includes(sectionId)) openSettings(); else closeSettings();
            if (window.innerWidth <= 768) document.getElementById('sidebar').classList.remove('open');
        }
        function toggleSidebar() { document.getElementById('sidebar').classList.toggle('open'); }
        function toggleSettings() { const subitems = document.querySelectorAll('.nav-subitem'); const isHidden = subitems[0].style.display === 'none'; subitems.forEach(el => el.style.display = isHidden ? 'flex' : 'none'); }
        function openSettings() { document.querySelectorAll('.nav-subitem').forEach(el => el.style.display = 'flex'); }
        function closeSettings() { document.querySelectorAll('.nav-subitem').forEach(el => el.style.display = 'none'); }

        function switchMainTab(tabName) {
            document.querySelectorAll('.app-tab').forEach(t => t.classList.remove('active'));
            event.target.classList.add('active');
            document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
            document.getElementById('tab-' + tabName).classList.add('active');
            if(window.innerWidth <= 768) {
                document.getElementById('leftPanel').classList.add('active');
                document.getElementById('chatWindow').classList.remove('active');
            }
        }

        // --- CHAT & GROUP ---
        let currentChatName = document.getElementById('currentChatName');
        function selectChat(name, imgUrl, status) {
            document.querySelectorAll('.chat-item').forEach(i => i.classList.remove('active-chat'));
            event.currentTarget.classList.add('active-chat');
            currentChatName.innerText = name;
            if (window.innerWidth <= 768) {
                document.getElementById('leftPanel').classList.remove('active');
                document.getElementById('chatWindow').classList.add('active');
                document.getElementById('mobileBackBtn').style.display = 'block';
            }
        }
        function backToList() {
            if (window.innerWidth <= 768) {
                document.getElementById('leftPanel').classList.add('active');
                document.getElementById('chatWindow').classList.remove('active');
                document.getElementById('mobileBackBtn').style.display = 'none';
            }
        }
        function handleEnter(e) { if(e.key === 'Enter') sendMessage(); }
        
        function sendMessage() {
            const input = document.getElementById('messageInput'); const text = input.value.trim();
            if(text) { appendMessage(text, 'text'); input.value = ""; }
        }

        function appendMessage(content, type) {
            const container = document.getElementById('messageContainer'); const msgId = 'msg-' + Date.now();
            const msgDiv = document.createElement('div'); msgDiv.className = 'message sent'; msgDiv.id = msgId;
            const now = new Date(); const timeString = now.getHours() + ':' + String(now.getMinutes()).padStart(2, '0');
            
            let contentHtml = '';
            if(type === 'text') contentHtml = content;
            else if(type === 'image') contentHtml = `<img src="${content}" class="msg-image">`;
            else if(type === 'video') contentHtml = `<video src="${content}" controls class="msg-video"><small>Video</small></video>`;
            else if(type === 'location') contentHtml = `<img src="https://picsum.photos/seed/map/300/200" class="location-img"><small>Lokasi Saat Ini</small>`;
            else if(type === 'contact') contentHtml = `<div class="contact-card"><img src="${content}" class="contact-avatar"><div><strong>Kontak Baru</strong><br>+62 812 XXX</div></div>`;
            else if(type === 'voice' || type === 'audio') contentHtml = `<div class="voice-message"><i class="fas fa-play"></i><div class="voice-wave">${'<div class="voice-bar"></div>'.repeat(15)}</div><span>0:10</span></div>`;
            else if(type === 'doc') contentHtml = `<div class="doc-preview"><i class="fas fa-file-alt fa-2x"></i><div>${content}<br><small>10 KB</small></div></div>`;

            msgDiv.innerHTML = `${contentHtml}<div class="message-meta"><span>${timeString}</span><i class="fas fa-check"></i></div>`;
            container.appendChild(msgDiv); container.scrollTop = container.scrollHeight;
            setTimeout(() => {
                const metaDiv = document.querySelector(`#${msgId} .message-meta`);
                if(metaDiv) metaDiv.innerHTML = `<span>${timeString}</span><i class="fas fa-check read"></i><i class="fas fa-check read"></i>`;
            }, 2000);
        }

        function toggleAttachMenu() { document.getElementById('attachMenu').classList.toggle('active'); document.getElementById('emojiPicker').classList.remove('active'); }
        function toggleEmojiPicker() { document.getElementById('emojiPicker').classList.toggle('active'); document.getElementById('attachMenu').classList.remove('active'); }

        let isRecording = false;
        function toggleMicRecording() {
            const btn = document.getElementById('micBtn'); const sendBtn = document.getElementById('sendBtn'); const input = document.getElementById('messageInput');
            if(!isRecording) {
                isRecording = true; input.style.display = 'none'; sendBtn.style.display = 'flex'; btn.innerHTML = '<i class="fas fa-times"></i>'; btn.style.color = 'var(--danger)'; showToast('Merekam pesan suara...');
            } else {
                isRecording = false; input.style.display = 'block'; btn.innerHTML = '<i class="fas fa-microphone"></i>'; btn.style.color = 'var(--text-gray)'; appendMessage('', 'voice');
            }
        }

        function sendLocation() { appendMessage('loc', 'location'); toggleAttachMenu(); }
        function sendContact() { appendMessage('https://picsum.photos/seed/contact/50/50', 'contact'); toggleAttachMenu(); }
        function handleImageUpload(input) { if(input.files && input.files[0]) { const reader = new FileReader(); reader.onload = function(e) { appendMessage(e.target.result, 'image'); }; reader.readAsDataURL(input.files[0]); } }
        function handleVideoUpload(input) { if(input.files && input.files[0]) { const file = input.files[0]; if(file.size > 20 * 1024 * 1024) { showToast("Video terlalu besar! Max 20MB."); return; } const reader = new FileReader(); reader.onload = function(e) { appendMessage(e.target.result, 'video'); showToast("Video terkirim"); }; reader.readAsDataURL(input.files[0]); } }
        function handleDocUpload(input) { if(input.files && input.files[0]) { appendMessage(input.files[0].name, 'doc'); showToast("Dokumen terkirim"); } }
        function handleAudioUpload(input) { if(input.files && input.files[0]) { appendMessage('', 'audio'); } }

        function toggleBlockUser() {
            const isBlocked = document.getElementById('blockToggle').checked;
            const status = isBlocked ? 'Kontak diblokir.' : 'Kontak dibuka blokir.';
            showToast(status);
            if(isBlocked) { document.getElementById('messageInput').disabled = true; document.getElementById('messageInput').placeholder = "Tidak dapat mengirim pesan ke kontak yang diblokir."; }
            else { document.getElementById('messageInput').disabled = false; document.getElementById('messageInput').placeholder = "Ketik pesan..."; }
        }
        function clearChat() { if(confirm('Apakah Anda yakin ingin menghapus semua pesan?')) { document.getElementById('messageContainer').innerHTML = ''; showToast('Obrolan dibersihkan.'); } }

        function saveProfileName() { document.getElementById('displayProfileName').innerText = document.getElementById('inputProfileName').value; showToast('Nama disimpan'); }
        function toggleDarkTheme() { document.body.classList.toggle('dark-theme'); showToast('Tema diubah'); }
        
        // Group & New Chat Logic
        function openNewGroupModal() {
            const list = document.getElementById('memberSelectionList'); list.innerHTML = '';
            const members = [{name: "Budi Santoso", img: "https://picsum.photos/seed/budi/40/40"}];
            members.forEach((m, idx) => {
                const div = document.createElement('div'); div.className = 'member-list-item';
                div.style.display = 'flex'; div.style.alignItems = 'center'; div.style.padding = '10px 0'; div.style.borderBottom = '1px solid var(--border-color)';
                div.innerHTML = `<input type="checkbox" id="member-${idx}" style="margin-right:10px;"><img src="${m.img}" class="member-avatar"> ${m.name}`;
                list.appendChild(div);
            });
            document.getElementById('newGroupModal').style.display = 'flex';
        }
        function confirmCreateGroup() {
            const groupName = document.getElementById('groupNameInput').value;
            if(!groupName) { showToast("Masukkan nama grup!"); return; }
            const chatList = document.getElementById('chatList');
            const newItem = document.createElement('div'); newItem.className = 'chat-item';
            newItem.style.background = `linear-gradient(45deg, #f09433, #e6683c, #dc2743, #cc2366, #bc1888)`;
            newItem.onclick = function() {
                document.querySelectorAll('.chat-item').forEach(i => i.classList.remove('active-chat'));
                this.classList.add('active-chat');
                currentChatName.innerText = groupName;
                document.getElementById('messageContainer').innerHTML = ''; 
                appendMessage(`Selamat datang di grup ${groupName}`, 'received'); 
            };
            newItem.innerHTML = `<div class="avatar-wrapper"><img src="" class="avatar" style="opacity:0; pointer-events:none;"><div style="position:absolute; top:0; left:0; width:100%; height:100%; border-radius:50%; display:flex; align-items:center; justify-content:center; color:white; font-weight:bold; font-size:1.2rem; background:rgba(0,0,0,0.2);">${groupName.charAt(0).toUpperCase()}</div></div><div class="chat-info"><h4>${groupName}</h4><p>Klik untuk mulai chat grup...</p></div>`;
            chatList.insertBefore(newItem, chatList.children[0]);
            closeModal('newGroupModal'); showToast("Grup berhasil dibuat!");
        }
        function openNewChatModal() { document.getElementById('newChatName').value = ''; document.getElementById('newChatModal').style.display = 'flex'; }
        function confirmNewChat() {
            const name = document.getElementById('newChatName').value;
            if(!name) { showToast("Masukkan nama kontak!"); return; }
            const chatList = document.getElementById('chatList');
            const newItem = document.createElement('div'); newItem.className = 'chat-item';
            newItem.onclick = function() { selectChat(name, 'https://picsum.photos/seed/'+name+'/50/50', 'offline'); };
            newItem.innerHTML = `<div class="avatar-wrapper"><img src="https://picsum.photos/seed/${name}/50/50" class="avatar"></div><div class="chat-info"><h4>${name}</h4><p>Pesan baru...</p></div>`;
            chatList.insertBefore(newItem, chatList.children[0]);
            closeModal('newChatModal'); showToast("Chat baru dimulai!"); newItem.click();
        }
        function closeModal(id) { document.getElementById(id).style.display = 'none'; }

        // --- Calls & Status ---
        let callInterval;
        function startCall(name, type, avatarUrl) {
            if(!avatarUrl) avatarUrl = 'https://picsum.photos/seed/default/150/150';
            document.getElementById('callAvatar').src = avatarUrl;
            document.getElementById('callName').innerText = name;
            document.getElementById('callStatus').innerText = type === 'video' ? 'Memanggil Video...' : 'Memanggil Suara...';
            document.getElementById('callTimer').innerText = '00:00';
            document.getElementById('callModal').style.display = 'flex';
            setTimeout(() => {
                document.getElementById('callStatus').innerText = 'Terhubung';
                let seconds = 0;
                callInterval = setInterval(() => {
                    seconds++;
                    const mins = Math.floor(seconds / 60).toString().padStart(2, '0');
                    const secs = (seconds % 60).toString().padStart(2, '0');
                    document.getElementById('callTimer').innerText = `${mins}:${secs}`;
                }, 1000);
            }, 2000);
        }
        function endCall() { clearInterval(callInterval); document.getElementById('callModal').style.display = 'none'; showToast('Panggilan Berakhir'); }
        function toggleMute() { showToast('Mute diaktifkan'); }
        function viewStatus(imgUrl, name) {
            document.getElementById('statusImage').src = imgUrl;
            document.getElementById('statusModal').style.display = 'flex';
            const bar = document.getElementById('statusBar'); bar.style.width = '0%';
            setTimeout(() => { bar.style.transition = 'width 5s linear'; bar.style.width = '100%'; }, 100);
            setTimeout(() => { document.getElementById('statusModal').style.display = 'none'; bar.style.width = '0%'; }, 5100);
        }
        function closeStatusViewer() {
            document.getElementById('statusModal').style.display = 'none';
            const bar = document.getElementById('statusBar');
            bar.style.transition = 'none'; bar.style.width = '0%';
        }
        function showToast(msg) { const t = document.getElementById('toast'); t.innerText = msg; t.classList.add('show'); setTimeout(()=>t.classList.remove('show'),3000); }
        
        const emojis = ["😀","😂","😍","😎","👍","👎","❤️","🔥"];
        function initEmojiPicker() {
            const picker = document.getElementById('emojiPicker');
            emojis.forEach(emoji => {
                const btn = document.createElement('button'); btn.className = 'emoji-btn'; btn.textContent = emoji;
                btn.onclick = function() { document.getElementById('messageInput').value += emoji; document.getElementById('messageInput').focus(); };
                picker.appendChild(btn);
            });
        }

        document.addEventListener('DOMContentLoaded', () => {
            initEmojiPicker(); closeSettings();
            if(window.innerWidth > 768) { document.getElementById('chatWindow').style.display = 'flex'; } else { document.getElementById('leftPanel').classList.add('active'); }
        });
        window.addEventListener('resize', () => {
             if(window.innerWidth > 768) {
                document.getElementById('leftPanel').classList.remove('active'); document.getElementById('leftPanel').style.display = 'flex';
                document.getElementById('chatWindow').style.display = 'flex'; document.getElementById('chatWindow').classList.remove('active');
                document.getElementById('mobileBackBtn').style.display = 'none';
            } else {
                document.getElementById('leftPanel').classList.add('active'); document.getElementById('leftPanel').style.display = 'flex';
                document.getElementById('chatWindow').classList.remove('active'); document.getElementById('chatWindow').style.display = 'none';
            }
        });
    </script>
</body>
</html>
