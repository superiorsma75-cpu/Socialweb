<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ChatConnect - Aplikasi Chat Lengkap</title>
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

        body {
            background-color: var(--bg-color);
            color: var(--text-dark);
            height: 100vh;
            display: flex;
            overflow: hidden;
        }

        /* --- Sidebar --- */
        .sidebar {
            width: 300px;
            background-color: var(--sidebar-bg);
            border-right: 1px solid var(--border-color);
            display: flex;
            flex-direction: column;
            transition: var(--transition);
            z-index: 100;
        }

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
        .main-content {
            flex: 1; display: flex; flex-direction: column; position: relative;
            background-image: url('https://picsum.photos/seed/bgpattern/800/600?blur=5');
            background-size: cover; background-blend-mode: overlay; background-color: rgba(255, 255, 255, 0.9);
        }
        body.dark-theme .main-content, body.high-contrast .main-content { background-blend-mode: normal; background-color: var(--sidebar-bg); background-image: none; }
        .mobile-header { display: none; padding: 15px; background-color: var(--primary-color); color: white; align-items: center; justify-content: space-between; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        
        .section { display: none; flex: 1; flex-direction: column; height: 100%; overflow: hidden; animation: fadeIn 0.3s ease; }
        .section.active-section { display: flex; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* --- Components --- */
        h2 { margin-bottom: 20px; color: var(--primary-color); border-bottom: 2px solid var(--border-color); padding-bottom: 10px; }
        .card { background: var(--white); border-radius: 8px; padding: 20px; box-shadow: var(--shadow); margin-bottom: 20px; }
        
        /* --- Home: Tabs (Chat | Status | Calls) --- */
        #section-home { display: none; flex-direction: row; overflow: hidden; position: relative; }
        #section-home.active-section { display: flex; }
        
        /* Panel Kiri (List) */
        .left-panel {
            width: 400px; background: var(--white); border-right: 1px solid var(--border-color);
            display: flex; flex-direction: column; position: relative; z-index: 10;
        }

        /* Tab Header */
        .app-tabs {
            display: flex; background: var(--bg-color); padding: 10px 15px; border-bottom: 1px solid var(--border-color);
            justify-content: space-between; color: var(--text-gray); font-weight: 600; cursor: pointer;
        }
        .app-tab { padding: 5px 15px; border-radius: 10px; color: var(--text-gray); transition: 0.2s; position: relative; }
        .app-tab.active { color: var(--text-dark); background-color: #d1d7db; }
        body.dark-theme .app-tab.active { background-color: #202c33; }
        .app-tab:hover:not(.active) { background-color: rgba(0,0,0,0.05); }

        /* Tab Content Areas */
        .tab-content { flex: 1; overflow-y: auto; display: none; }
        .tab-content.active { display: block; }

        /* Chat List */
        .chat-item { display: flex; align-items: center; padding: 15px; border-bottom: 1px solid var(--border-color); cursor: pointer; position: relative; }
        .chat-item:hover, .chat-item.active-chat { background-color: var(--bg-color); }
        .avatar-wrapper { position: relative; }
        .avatar { width: 50px; height: 50px; border-radius: 50%; object-fit: cover; margin-right: 15px; background-color: #ddd; }
        .status-dot { width: 12px; height: 12px; border-radius: 50%; border: 2px solid var(--white); position: absolute; bottom: 2px; right: 2px; }
        .status-dot.online { background-color: #25D366; }
        .status-dot.offline { background-color: #aebac1; }
        .chat-info h4 { font-size: 1rem; margin-bottom: 4px; }
        .chat-info p { font-size: 0.85rem; color: var(--text-gray); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; max-width: 200px; }

        /* Status List */
        .status-scroll { display: flex; gap: 15px; padding: 20px 15px; overflow-x: auto; background: var(--bg-color); }
        .status-scroll::-webkit-scrollbar { height: 0; }
        
        .status-item { display: flex; flex-direction: column; align-items: center; cursor: pointer; min-width: 70px; }
        .status-ring {
            width: 60px; height: 60px; border-radius: 50%; padding: 3px; 
            background: linear-gradient(45deg, #f09433, #e6683c, #dc2743, #cc2366, #bc1888); 
            display: flex; justify-content: center; align-items: center;
        }
        .status-ring.seen { background: var(--border-color); }
        .status-ring img { width: 100%; height: 100%; border-radius: 50%; object-fit: cover; border: 2px solid var(--white); }
        
        .my-status {
            width: 60px; height: 60px; border-radius: 50%; border: 2px dashed var(--text-gray); 
            display: flex; justify-content: center; align-items: center; 
            background: var(--white); color: var(--primary-color); font-size: 1.5rem; cursor: pointer;
        }
        .status-name { font-size: 0.8rem; margin-top: 5px; color: var(--text-dark); text-align: center; width: 70px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

        .recent-updates { padding: 10px 15px; font-size: 0.8rem; color: var(--primary-color); font-weight: bold; border-bottom: 1px solid var(--border-color); }
        .status-list-item { padding: 10px 15px; display: flex; align-items: center; cursor: pointer; border-bottom: 1px solid var(--border-color); }
        .status-list-item:hover { background-color: var(--bg-color); }

        /* Calls List */
        .call-item { display: flex; align-items: center; padding: 15px; border-bottom: 1px solid var(--border-color); cursor: pointer; }
        .call-item:hover { background-color: var(--bg-color); }
        .call-icon { width: 40px; text-align: center; margin-right: 15px; font-size: 1.2rem; }
        .call-icon.incoming { color: var(--primary-color); }
        .call-icon.missed { color: var(--danger); }
        .call-icon.outgoing { color: var(--primary-light); }
        .call-info { flex: 1; }
        .call-meta { display: flex; align-items: center; font-size: 0.8rem; color: var(--text-gray); }
        .call-meta i { margin-right: 5px; }

        /* Chat Window */
        .chat-window {
            flex: 1; display: flex; flex-direction: column;
            background-color: #efe7dd; background-image: url('https://user-images.githubusercontent.com/15075759/28719144-86dc0f70-73b1-11e7-911d-60d70fcded21.png'); position: relative;
        }
        body.dark-theme .chat-window, body.high-contrast .chat-window { background-color: var(--sidebar-bg); background-image: none; }
        
        .chat-header {
            padding: 10px 20px; background: var(--bg-color); border-bottom: 1px solid var(--border-color);
            display: flex; align-items: center; justify-content: space-between; z-index: 5;
        }
        .chat-header-actions i { margin-left: 15px; color: var(--primary-color); cursor: pointer; font-size: 1.2rem; }

        .chat-messages { flex: 1; padding: 20px; overflow-y: auto; display: flex; flex-direction: column; gap: 10px; z-index: 1; }
        .message { max-width: 70%; padding: 10px 15px; border-radius: 8px; position: relative; font-size: 0.95rem; line-height: 1.4; display: flex; flex-direction: column; word-wrap: break-word; }
        .message.received { background-color: var(--white); align-self: flex-start; border-top-left-radius: 0; }
        .message.sent { background-color: #dcf8c6; align-self: flex-end; border-top-right-radius: 0; }
        body.dark-theme .message.sent, body.high-contrast .message.sent { background-color: #005c4b; color: white; }
        body.dark-theme .message.received, body.high-contrast .message.received { background-color: #202c33; color: white; }

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

        /* --- Modals (Call & Status) --- */
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.8); z-index: 2000;
            display: none; flex-direction: column; align-items: center; justify-content: center;
            color: white;
        }
        
        /* Call Modal */
        .call-screen { text-align: center; animation: scaleIn 0.3s ease; }
        @keyframes scaleIn { from { transform: scale(0.8); opacity: 0; } to { transform: scale(1); opacity: 1; } }
        .call-avatar { width: 120px; height: 120px; border-radius: 50%; margin-bottom: 20px; border: 4px solid var(--white); }
        .call-status { font-size: 1.2rem; margin-bottom: 10px; color: #ddd; }
        .call-timer { font-size: 2.5rem; margin-bottom: 30px; font-weight: bold; }
        .call-actions { display: flex; gap: 20px; justify-content: center; margin-top: 20px; }
        .call-btn { width: 60px; height: 60px; border-radius: 50%; border: none; cursor: pointer; display: flex; align-items: center; justify-content: center; font-size: 1.5rem; transition: 0.2s; }
        .btn-mute { background: rgba(255,255,255,0.2); color: white; }
        .btn-end { background: var(--danger); color: white; }
        .call-btn:hover { transform: scale(1.1); }

        /* Status Modal */
        .status-modal { width: 100%; max-width: 400px; height: 80vh; max-height: 700px; background: black; position: relative; border-radius: 8px; overflow: hidden; display: flex; align-items: center; justify-content: center; }
        .status-img { width: 100%; height: 100%; object-fit: contain; }
        .status-progress { position: absolute; top: 20px; left: 20px; right: 20px; height: 3px; background: rgba(255,255,255,0.3); border-radius: 2px; overflow: hidden; }
        .status-bar-fill { height: 100%; background: var(--white); width: 0%; }
        .close-status { position: absolute; top: 30px; right: 20px; font-size: 2rem; cursor: pointer; color: white; z-index: 10; }

        /* Emoji Picker */
        .emoji-picker-container {
            position: absolute; bottom: 70px; right: 20px; background-color: var(--white);
            border: 1px solid var(--border-color); border-radius: 8px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2); padding: 10px;
            width: 280px; display: none; flex-wrap: wrap; gap: 5px; z-index: 999;
        }
        body.dark-theme .emoji-picker-container, body.high-contrast .emoji-picker-container { background-color: var(--sidebar-bg); border-color: var(--border-color); }
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
        
        .btn { padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer; font-weight: bold; transition: opacity 0.2s; display: inline-flex; align-items: center; justify-content: center; gap: 8px; background: var(--primary-color); color: white; }
        .btn:hover { opacity: 0.9; }
        .btn-secondary { background: #f0f2f5; color: var(--text-dark); }
        .btn-danger { background-color: var(--danger); }

        .toast { position: fixed; bottom: 20px; right: 20px; background-color: #333; color: white; padding: 12px 24px; border-radius: 5px; box-shadow: 0 4px 10px rgba(0,0,0,0.2); transform: translateY(100px); opacity: 0; transition: all 0.4s cubic-bezier(0.68, -0.55, 0.27, 1.55); z-index: 1000; }
        .toast.show { transform: translateY(0); opacity: 1; }

        @media (max-width: 768px) {
            .sidebar { position: fixed; left: -300px; height: 100%; box-shadow: 2px 0 10px rgba(0,0,0,0.2); }
            .sidebar.open { left: 0; }
            .mobile-header { display: flex; }
            #section-home { flex-direction: column; }
            .left-panel { width: 100%; display: none; }
            .left-panel.active { display: flex; }
            .chat-window { display: none; }
            .chat-window.active { display: flex; width: 100%; }
            .emoji-picker-container { width: 90%; right: 5%; bottom: 60px; }
        }
    </style>
</head>
<body>

    <!-- Toast Notification -->
    <div id="toast" class="toast">Berhasil</div>

    <!-- Modal Panggilan -->
    <div id="callModal" class="modal-overlay">
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

    <!-- Modal Lihat Status -->
    <div id="statusModal" class="modal-overlay" style="background: rgba(0,0,0,0.9);">
        <div class="status-modal">
            <span class="close-status" onclick="closeStatusViewer()">&times;</span>
            <div class="status-progress"><div id="statusBar" class="status-bar-fill"></div></div>
            <img id="statusImage" src="" class="status-img">
        </div>
    </div>

    <!-- Header Mobile -->
    <header class="mobile-header">
        <i class="fas fa-bars fa-lg" onclick="toggleSidebar()"></i>
        <span>ChatConnect</span>
        <div style="width: 24px;"></div> 
    </header>

    <!-- Sidebar -->
    <nav class="sidebar" id="sidebar">
        <div class="brand"><i class="fas fa-comments"></i> ChatConnect</div>
        <div class="nav-menu">
            <div class="nav-category">Utama</div>
            <a onclick="showSection('home')" class="nav-item active" id="nav-home"><i class="fas fa-home"></i> Home</a>
            <a onclick="showSection('profile')" class="nav-item" id="nav-profile"><i class="fas fa-user"></i> Profil</a>
            <a onclick="showSection('about')" class="nav-item" id="nav-about"><i class="fas fa-info-circle"></i> Tentang Kami</a>

            <div class="nav-category">Pengaturan</div>
            <a onclick="toggleSettings()" class="nav-item" id="nav-settings"><i class="fas fa-cog"></i> Pengaturan</a>
            <a onclick="showSection('display')" class="nav-item nav-subitem" id="nav-display"><i class="fas fa-paint-brush"></i> Tampilan</a>
            <a onclick="showSection('notifications')" class="nav-item nav-subitem" id="nav-notifications"><i class="fas fa-bell"></i> Notifikasi</a>
            <a onclick="showSection('privacy')" class="nav-item nav-subitem" id="nav-privacy"><i class="fas fa-lock"></i> Privasi</a>
            <a onclick="showSection('data')" class="nav-item nav-subitem" id="nav-data"><i class="fas fa-database"></i> Penyimpanan Data</a>
            <a onclick="showSection('language')" class="nav-item nav-subitem" id="nav-language"><i class="fas fa-globe"></i> Bahasa</a>
            <a onclick="showSection('accessibility')" class="nav-item nav-subitem" id="nav-accessibility"><i class="fas fa-universal-access"></i> Aksesibilitas</a>
            <a onclick="showSection('updates')" class="nav-item nav-subitem" id="nav-updates"><i class="fas fa-download"></i> Pembaharuan Aplikasi</a>

            <div class="nav-category">Dukungan</div>
            <a onclick="showSection('help')" class="nav-item" id="nav-help"><i class="fas fa-question-circle"></i> Bantuan</a>
            <a onclick="showSection('videos')" class="nav-item" id="nav-videos"><i class="fas fa-video"></i> Video</a>
        </div>
    </nav>

    <!-- Main Content -->
    <main class="main-content">
        
        <!-- SECTION: HOME (CHTAT | STATUS | CALLS) -->
        <section id="section-home" class="section active-section">
            
            <!-- Left Panel (Tabs) -->
            <div class="left-panel active" id="leftPanel">
                <!-- Tab Headers -->
                <div class="app-tabs">
                    <div class="app-tab active" onclick="switchMainTab('chat')">Chat</div>
                    <div class="app-tab" onclick="switchMainTab('status')">Status</div>
                    <div class="app-tab" onclick="switchMainTab('calls')">Panggilan</div>
                </div>

                <!-- Tab 1: Chat List -->
                <div id="tab-chat" class="tab-content active">
                    <div class="chat-item active-chat" onclick="selectChat('Budi Santoso', 'https://picsum.photos/seed/budi/50/50', 'online')">
                        <div class="avatar-wrapper">
                            <img src="https://picsum.photos/seed/budi/50/50" class="avatar">
                            <div class="status-dot online"></div>
                        </div>
                        <div class="chat-info">
                            <h4>Budi Santoso</h4>
                            <p>Halo, apa kabar? Lama tak jumpa!</p>
                        </div>
                    </div>
                    <div class="chat-item" onclick="selectChat('Siti Aminah', 'https://picsum.photos/seed/siti/50/50', 'offline')">
                        <div class="avatar-wrapper">
                            <img src="https://picsum.photos/seed/siti/50/50" class="avatar">
                            <div class="status-dot offline"></div>
                        </div>
                        <div class="chat-info">
                            <h4>Siti Aminah</h4>
                            <p>Dokumen sudah saya kirim ya.</p>
                        </div>
                    </div>
                </div>

                <!-- Tab 2: Status -->
                <div id="tab-status" class="tab-content">
                    <div class="status-scroll">
                        <div class="status-item" onclick="showToast('Fitur tambah status (demo)')">
                            <div class="my-status"><i class="fas fa-plus"></i></div>
                            <span class="status-name">Status Saya</span>
                        </div>
                        <div class="status-item" onclick="viewStatus('https://picsum.photos/seed/status1/400/800', 'Budi')">
                            <div class="status-ring">
                                <img src="https://picsum.photos/seed/budi/60/60">
                            </div>
                            <span class="status-name">Budi</span>
                        </div>
                        <div class="status-item" onclick="viewStatus('https://picsum.photos/seed/status2/400/800', 'Siti')">
                            <div class="status-ring seen">
                                <img src="https://picsum.photos/seed/siti/60/60">
                            </div>
                            <span class="status-name">Siti</span>
                        </div>
                    </div>
                    <div class="recent-updates">Pembaruan Terbaru</div>
                    <div class="status-list-item" onclick="viewStatus('https://picsum.photos/seed/status1/400/800', 'Budi')">
                        <div class="avatar-wrapper">
                            <img src="https://picsum.photos/seed/budi/40/40" class="avatar" style="width:40px;height:40px;margin-right:10px;">
                        </div>
                        <div class="chat-info">
                            <h4>Budi Santoso</h4>
                            <p>Hari ini, 10:30</p>
                        </div>
                    </div>
                </div>

                <!-- Tab 3: Calls -->
                <div id="tab-calls" class="tab-content">
                    <div class="call-item" onclick="startCall('Budi Santoso', 'video', 'https://picsum.photos/seed/budi/150/150')">
                        <div class="call-icon incoming"><i class="fas fa-video"></i></div>
                        <div class="call-info">
                            <h4>Budi Santoso</h4>
                            <div class="call-meta"><i class="fas fa-arrow-down"></i> 5 Januari, 10:30 • 5 Menit</div>
                        </div>
                        <i class="fas fa-video" style="color: var(--primary-color); cursor: pointer;"></i>
                    </div>
                    <div class="call-item" onclick="startCall('Siti Aminah', 'voice', 'https://picsum.photos/seed/siti/150/150')">
                        <div class="call-icon missed"><i class="fas fa-phone-slash"></i></div>
                        <div class="call-info">
                            <h4 style="color: var(--danger);">Siti Aminah</h4>
                            <div class="call-meta"><i class="fas fa-phone-slash"></i> 4 Januari, 14:00 • Tak Terjawab</div>
                        </div>
                        <i class="fas fa-info-circle" style="color: var(--text-gray);"></i>
                    </div>
                    <div class="call-item" onclick="startCall('Tim Proyek', 'voice', 'https://picsum.photos/seed/tim/150/150')">
                        <div class="call-icon outgoing"><i class="fas fa-phone"></i></div>
                        <div class="call-info">
                            <h4>Tim Proyek</h4>
                            <div class="call-meta"><i class="fas fa-arrow-up"></i> 3 Januari, 09:00 • 12 Menit</div>
                        </div>
                        <i class="fas fa-info-circle" style="color: var(--text-gray);"></i>
                    </div>
                </div>
            </div>

            <!-- Chat Window (Right Panel) -->
            <div class="chat-window" id="chatWindow">
                <div class="chat-header">
                    <div style="display:flex; align-items:center;">
                        <!-- Tombol Back di Mobile -->
                        <i class="fas fa-arrow-left" onclick="backToList()" style="margin-right:10px; display:none; cursor:pointer;" id="mobileBackBtn"></i>
                        <div>
                            <h3 id="currentChatName">Budi Santoso</h3>
                            <small id="currentChatStatus" style="color:var(--text-gray);">Online</small>
                        </div>
                    </div>
                    <div class="chat-header-actions">
                        <i class="fas fa-video" onclick="startCall(currentChatName.innerText, 'video', '')"></i>
                        <i class="fas fa-phone" onclick="startCall(currentChatName.innerText, 'voice', '')"></i>
                        <i class="fas fa-search"></i>
                    </div>
                </div>

                <div class="chat-messages" id="messageContainer">
                    <div class="message received">
                        Halo, apa kabar? Lama tak jumpa!
                        <div class="message-meta"><span>10:30</span></div>
                    </div>
                    <div class="message sent">
                        Kabar baik! Kamu gimana?
                        <div class="message-meta"><span>10:31</span><i class="fas fa-check read"></i><i class="fas fa-check read"></i></div>
                    </div>
                </div>

                <div id="emojiPicker" class="emoji-picker-container"></div>

                <div class="chat-input-area">
                    <button class="btn-icon" onclick="toggleEmojiPicker()"><i class="far fa-smile"></i></button>
                    <button class="btn-icon"><i class="fas fa-paperclip"></i></button>
                    <input type="text" class="chat-input" id="messageInput" placeholder="Ketik pesan..." onkeypress="handleEnter(event)">
                    <button class="btn-send" onclick="sendMessage()"><i class="fas fa-paper-plane"></i></button>
                </div>
            </div>
        </section>

        <!-- SECTIONS LAINNYA (Tetap sama seperti sebelumnya) -->
        <section id="section-profile" class="section">
            <h2>Profil Pengguna</h2>
            <div class="card" style="text-align: center;">
                <img id="displayProfileImg" src="https://picsum.photos/seed/me/150/150" alt="Profile" style="border-radius: 50%; width: 100px; height: 100px; margin-bottom: 15px; object-fit: cover;">
                <h3 id="displayProfileName">Nama Pengguna</h3>
                <p id="displayProfilePhone" style="color: var(--text-gray); margin-bottom: 20px;">+62 812 3456 7890</p>
                <input type="file" id="profilePhotoInput" accept="image/*" style="display: none;" onchange="handlePhotoUpload(event)">
                <button class="btn" onclick="document.getElementById('profilePhotoInput').click()">Ganti Foto Profil</button>
            </div>
            <div class="card">
                <div class="form-group"><label>Nama</label><input type="text" class="form-control" id="inputProfileName" value="Nama Pengguna"></div>
                <div class="form-group"><label>Nomor Telepon</label><div class="input-group"><input type="text" class="form-control" id="inputProfilePhone" value="+62 812 3456 7890"><button class="btn" onclick="changePhoneNumber()">Ganti</button></div></div>
                <button class="btn" onclick="saveProfile()">Simpan Profil</button>
            </div>
        </section>

        <section id="section-display" class="section">
            <h2>Tampilan</h2>
            <div class="card">
                <div class="toggle-row"><span>Mode Gelap</span><label class="switch"><input type="checkbox" id="darkThemeToggle" onchange="toggleDarkTheme()"><span class="slider"></span></label></div>
            </div>
        </section>
        
        <!-- Placeholder sections for brevity -->
        <section id="section-about" class="section"><h2>Tentang Kami</h2><div class="card"><p>ChatConnect v1.2.0</p></div></section>
        <section id="section-notifications" class="section"><h2>Notifikasi</h2><div class="card"><div class="toggle-row"><span>Notifikasi Pesan</span><label class="switch"><input type="checkbox" checked><span class="slider"></span></label></div></div></section>
        <section id="section-privacy" class="section"><h2>Privasi</h2><div class="card"><p>Pengaturan privasi pengguna.</p></div></section>
        <section id="section-data" class="section"><h2>Data</h2><div class="card"><p>Kelola penyimpanan.</p></div></section>
        <section id="section-language" class="section"><h2>Bahasa</h2><div class="card"><p>Indonesia (Default)</p></div></section>
        <section id="section-accessibility" class="section">
            <h2>Aksesibilitas</h2>
            <div class="card">
                <div class="toggle-row"><span>Mode Kontras Tinggi</span><label class="switch"><input type="checkbox" id="contrastToggle" onchange="toggleHighContrast()"><span class="slider"></span></label></div>
            </div>
        </section>
        <section id="section-updates" class="section"><h2>Pembaharuan</h2><div class="card"><p>Versi terbaru terinstall.</p></div></section>
        <section id="section-help" class="section"><h2>Bantuan</h2><div class="card"><p>Pusat bantuan.</p></div></section>
        <section id="section-videos" class="section">
            <h2>Video</h2>
            <div class="card">
                <iframe width="100%" height="200" src="https://www.youtube.com/embed/jfKfPfyJRdk?autoplay=1&mute=1" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
            </div>
        </section>

    </main>

    <script>
        // --- Navigation Logic ---
        function showSection(sectionId) {
            document.querySelectorAll('.section').forEach(sec => sec.classList.remove('active-section'));
            document.querySelectorAll('.nav-item').forEach(item => item.classList.remove('active'));
            document.getElementById('section-' + sectionId).classList.add('active-section');
            const navItem = document.getElementById('nav-' + sectionId);
            if(navItem) navItem.classList.add('active');
            if(['display','notifications','privacy','data','language','accessibility','updates'].includes(sectionId)) openSettings(); else closeSettings();
            if (window.innerWidth <= 768) document.getElementById('sidebar').classList.remove('open');
        }

        function toggleSidebar() { document.getElementById('sidebar').classList.toggle('open'); }
        function toggleSettings() { const subitems = document.querySelectorAll('.nav-subitem'); const isHidden = subitems[0].style.display === 'none'; subitems.forEach(el => el.style.display = isHidden ? 'flex' : 'none'); }
        function openSettings() { document.querySelectorAll('.nav-subitem').forEach(el => el.style.display = 'flex'); }
        function closeSettings() { document.querySelectorAll('.nav-subitem').forEach(el => el.style.display = 'none'); }

        // --- Main Tabs Logic (Chat | Status | Calls) ---
        function switchMainTab(tabName) {
            // Tab Header UI
            document.querySelectorAll('.app-tab').forEach(t => t.classList.remove('active'));
            event.target.classList.add('active');
            
            // Content UI
            document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
            document.getElementById('tab-' + tabName).classList.add('active');

            // Mobile Logic: Hide chat window if on mobile
            if(window.innerWidth <= 768) {
                document.getElementById('leftPanel').classList.add('active');
                document.getElementById('chatWindow').classList.remove('active');
            }
        }

        // --- Chat Logic ---
        let currentChatName = document.getElementById('currentChatName');
        let currentChatStatus = document.getElementById('currentChatStatus');

        function selectChat(name, imgUrl, status) {
            document.querySelectorAll('.chat-item').forEach(i => i.classList.remove('active-chat'));
            event.currentTarget.classList.add('active-chat');
            
            currentChatName.innerText = name;
            currentChatStatus.innerText = status === 'online' ? 'Online' : 'Terakhir dilihat hari ini';
            
            if (window.innerWidth <= 768) {
                document.getElementById('leftPanel').classList.remove('active');
                document.getElementById('chatWindow').classList.add('active');
                document.getElementById('mobileBackBtn').style.display = 'block';
            } else {
                document.getElementById('mobileBackBtn').style.display = 'none';
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
            if(text) {
                const container = document.getElementById('messageContainer'); const msgId = 'msg-' + Date.now();
                const msgDiv = document.createElement('div'); msgDiv.className = 'message sent'; msgDiv.id = msgId;
                const now = new Date(); const timeString = now.getHours() + ':' + String(now.getMinutes()).padStart(2, '0');
                msgDiv.innerHTML = `${text}<div class="message-meta"><span>${timeString}</span><i class="fas fa-check"></i></div>`;
                container.appendChild(msgDiv); input.value = ""; container.scrollTop = container.scrollHeight;
                setTimeout(() => {
                    const metaDiv = document.querySelector(`#${msgId} .message-meta`);
                    if(metaDiv) metaDiv.innerHTML = `<span>${timeString}</span><i class="fas fa-check read"></i><i class="fas fa-check read"></i>`;
                }, 2000);
            }
        }

        // --- Call Logic (Simulation) ---
        let callInterval;
        function startCall(name, type, avatarUrl) {
            if(!avatarUrl) avatarUrl = 'https://picsum.photos/seed/default/150/150';
            document.getElementById('callAvatar').src = avatarUrl;
            document.getElementById('callName').innerText = name;
            document.getElementById('callStatus').innerText = type === 'video' ? 'Memanggil Video...' : 'Memanggil Suara...';
            document.getElementById('callTimer').innerText = '00:00';
            document.getElementById('callModal').style.display = 'flex';
            
            // Simulate connected after 2 seconds
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

        function endCall() {
            clearInterval(callInterval);
            document.getElementById('callModal').style.display = 'none';
            showToast('Panggilan Berakhir');
        }
        
        function toggleMute() {
            showToast('Mute diaktifkan (simulasi)');
        }

        // --- Status Logic ---
        function viewStatus(imgUrl, name) {
            document.getElementById('statusImage').src = imgUrl;
            document.getElementById('statusModal').style.display = 'flex';
            
            // Progress Bar Animation
            const bar = document.getElementById('statusBar');
            bar.style.width = '0%';
            setTimeout(() => { bar.style.transition = 'width 5s linear'; bar.style.width = '100%'; }, 100);
            
            // Auto Close
            setTimeout(() => {
                closeStatusViewer();
            }, 5100);
        }

        function closeStatusViewer() {
            document.getElementById('statusModal').style.display = 'none';
            const bar = document.getElementById('statusBar');
            bar.style.transition = 'none';
            bar.style.width = '0%';
        }

        // --- Emoji Logic ---
        const emojis = ["😀","😂","😍","😎","👍","👎","❤️","🔥","👋","🎉"];
        function initEmojiPicker() {
            const picker = document.getElementById('emojiPicker');
            emojis.forEach(emoji => {
                const btn = document.createElement('button'); btn.className = 'emoji-btn'; btn.textContent = emoji;
                btn.onclick = function() { document.getElementById('messageInput').value += emoji; document.getElementById('messageInput').focus(); };
                picker.appendChild(btn);
            });
        }
        function toggleEmojiPicker() { document.getElementById('emojiPicker').classList.toggle('active'); }

        // --- Profile & Utils ---
        function handlePhotoUpload(event) {
            const file = event.target.files[0];
            if(file) {
                const reader = new FileReader();
                reader.onload = function(e) { document.getElementById('displayProfileImg').src = e.target.result; showToast('Foto diubah'); };
                reader.readAsDataURL(file);
            }
        }
        function changePhoneNumber() { const input = document.getElementById('inputProfilePhone').value; document.getElementById('displayProfilePhone').innerText = input; showToast('Nomor diubah'); }
        function saveProfile() { document.getElementById('displayProfileName').innerText = document.getElementById('inputProfileName').value; showToast('Profil disimpan'); }
        function toggleDarkTheme() { document.body.classList.toggle('dark-theme'); showToast('Tema diubah'); }
        function toggleHighContrast() { document.body.classList.toggle('high-contrast'); showToast('Kontras diubah'); }
        function showToast(msg) { const t = document.getElementById('toast'); t.innerText = msg; t.classList.add('show'); setTimeout(()=>t.classList.remove('show'),3000); }

        document.addEventListener('DOMContentLoaded', () => {
            initEmojiPicker(); closeSettings();
            if(window.innerWidth > 768) { document.getElementById('chatWindow').style.display = 'flex'; } else { document.getElementById('leftPanel').classList.add('active'); }
        });
        
        window.addEventListener('resize', () => {
             if(window.innerWidth > 768) {
                document.getElementById('leftPanel').classList.remove('active'); 
                document.getElementById('leftPanel').style.display = 'flex';
                document.getElementById('chatWindow').style.display = 'flex';
                document.getElementById('chatWindow').classList.remove('active');
                document.getElementById('mobileBackBtn').style.display = 'none';
            } else {
                document.getElementById('leftPanel').classList.add('active');
                document.getElementById('leftPanel').style.display = 'flex';
                document.getElementById('chatWindow').classList.remove('active');
                document.getElementById('chatWindow').style.display = 'none';
            }
        });
    </script>
</body>
</html>
