<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NEXUS - Bug WA Simulation v1 (Beta)</title>
    <style>
        :root {
            --primary: #00ff88;
            --primary-dark: #00cc66;
            --bg: #0a0a0a;
            --panel: #111111;
            --text: #e0e0e0;
            --text-muted: #aaaaaa;
            --danger: #ff4444;
            --accent: #00ff88;
            --premium: #ffd700;
            --guide-bg: rgba(0, 0, 0, 0.9);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background-color: var(--bg);
            color: var(--text);
            display: flex;
            justify-content: center;
            align-items: flex-start;
            min-height: 100vh;
            overflow-x: hidden;
            width: 100%;
        }

        .container {
            width: 100%;
            max-width: 480px;
            background: var(--panel);
            padding: 20px;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            position: relative;
            box-shadow: 0 0 30px rgba(0, 255, 136, 0.1);
        }

        h1, h2 {
            text-align: center;
            color: var(--primary);
            margin-bottom: 15px;
            text-transform: uppercase;
            letter-spacing: 2px;
            text-shadow: 0 0 10px rgba(0, 255, 136, 0.4);
        }

        .input-group { 
            margin-bottom: 12px; 
        }
        label { 
            display: block; 
            margin-bottom: 4px; 
            font-size: 0.7em; 
            color: var(--text-muted); 
            text-transform: uppercase;
        }
        
        input {
            width: 100%;
            padding: 10px;
            background: #1a1a1a;
            border: 1px solid #333;
            color: var(--primary);
            outline: none;
            border-radius: 6px;
            font-size: 0.9em;
            transition: border-color 0.3s;
        }
        input:focus { 
            border-color: var(--primary); 
        }

        button {
            width: 100%;
            padding: 12px;
            background: transparent;
            color: var(--primary);
            border: 1px solid var(--primary);
            cursor: pointer;
            margin-top: 8px;
            text-transform: uppercase;
            font-weight: bold;
            border-radius: 6px;
            transition: all 0.3s;
            font-size: 0.85em;
        }
        button:active { 
            background: var(--primary); 
            color: #000; 
            transform: scale(0.98);
        }
        button.premium-btn {
            border-color: var(--premium);
            color: var(--premium);
        }
        button.premium-btn:active {
            background: var(--premium);
            color: #000;
        }
        button.secondary { 
            border-color: #555; 
            color: #aaa; 
        }
        button.danger { 
            border-color: var(--danger); 
            color: var(--danger); 
        }

        .hidden { display: none !important; }

        /* Notification */
        .notification {
            position: fixed; 
            top: 20px; 
            left: 50%; 
            transform: translateX(-50%);
            background: #111; 
            border: 1px solid var(--primary); 
            padding: 10px 20px;
            z-index: 10000; 
            display: none; 
            font-size: 0.8em;
            border-radius: 20px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.5);
            width: 85%;
            text-align: center;
        }

        /* Bug Grid */
        .bug-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
            margin-bottom: 20px;
            max-height: 400px;
            overflow-y: auto;
            padding-right: 5px;
        }
        .bug-btn {
            background: #151515;
            border: 1px solid #333;
            color: var(--primary);
            padding: 12px 5px;
            border-radius: 8px;
            text-align: center;
            font-size: 0.7em;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 3px;
            cursor: pointer;
        }
        .bug-btn:active {
            background: var(--primary);
            color: #000;
        }
        .bug-btn.prem-only { border-color: var(--premium); color: var(--premium); }

        /* Console */
        .console-log {
            background: #050505;
            border: 1px solid #222;
            height: 100px;
            overflow-y: auto;
            padding: 8px;
            font-size: 0.65em;
            color: #00ff44;
            font-family: monospace;
            border-radius: 6px;
            margin-top: 10px;
        }

        .status-bar {
            display: flex;
            justify-content: space-between;
            font-size: 0.65em;
            color: #888;
            margin-bottom: 10px;
            background: #000;
            padding: 6px;
            border-radius: 4px;
        }

        ::-webkit-scrollbar { width: 4px; }
        ::-webkit-scrollbar-track { background: #000; }
        ::-webkit-scrollbar-thumb { background: var(--primary); border-radius: 10px; }
    </style>
</head>
<body>

    <div id="notification" class="notification"></div>

    <!-- LOGIN PAGE -->
    <div id="login-page" class="container">
        <h1>NEXUS V1 (BETA)</h1>
        <p style="text-align:center; font-size:0.65em; margin-bottom:20px; color:#666;">ULTIMATE BUG INJECTOR</p>
        
        <div class="input-group">
            <label>USERNAME</label>
            <input type="text" id="login-user" placeholder="Masukkan username">
        </div>
        <div class="input-group">
            <label>PASSWORD</label>
            <input type="password" id="login-pass" placeholder="Masukkan password">
        </div>
        
        <button onclick="handleLogin()">LOGIN</button>
        
        <div style="margin-top: 15px;">
            <button class="premium-btn" onclick="navigateTo('register-premium-page')">Daftar Akun Premium</button>
            <button class="secondary" onclick="navigateTo('reg-step-tiktok')">Daftar Akun Free</button>
        </div>
    </div>

    <!-- REGISTER PREMIUM -->
    <div id="register-premium-page" class="container hidden">
        <h2 style="color:var(--premium);">DAFTAR PREMIUM</h2>
        <div class="input-group">
            <label>KODE VERIFIKASI</label>
            <input type="password" id="reg-prem-code" placeholder="••••••">
        </div>
        <div class="input-group">
            <label>USERNAME BARU</label>
            <input type="text" id="reg-prem-user">
        </div>
        <div class="input-group">
            <label>PASSWORD BARU</label>
            <input type="password" id="reg-prem-pass">
        </div>
        <button class="premium-btn" onclick="processRegisterPremium()">AKTIVASI PREMIUM</button>
        <button class="secondary" onclick="navigateTo('login-page')">KEMBALI</button>
    </div>

    <!-- REGISTRATION FLOW FREE - STEP 1 (TIKTOK) -->
    <div id="reg-step-tiktok" class="container hidden">
        <h2>VERIFIKASI TIKTOK</h2>
        <p style="font-size:0.8em; text-align:center; margin-bottom:15px;">Follow <b>kiboyaslinofake</b> untuk akses 4 hari.</p>
        <button onclick="openTikTok()">FOLLOW TIKTOK</button>
        <button id="tiktok-next" class="hidden" onclick="navigateTo('reg-step-wa')">LANJUTKAN</button>
        <button class="secondary" onclick="navigateTo('login-page')">BATAL</button>
    </div>

    <!-- REGISTRATION FLOW FREE - STEP 2 (WHATSAPP) -->
    <div id="reg-step-wa" class="container hidden">
        <h2>VERIFIKASI KOMUNITAS</h2>
        <p style="font-size:0.8em; text-align:center; margin-bottom:15px;">Gabung Saluran WA kami.</p>
        <button onclick="openWAChannel()">GABUNG SALURAN</button>
        <button id="wa-next" class="hidden" onclick="navigateTo('register-free-page')">LANJUTKAN KE PENDAFTARAN</button>
        <button class="secondary" onclick="navigateTo('reg-step-tiktok')">KEMBALI</button>
    </div>

    <!-- REGISTER FREE FORM -->
    <div id="register-free-page" class="container hidden">
        <h2>DAFTAR AKUN FREE</h2>
        <p style="text-align:center; font-size:0.65em; color:var(--danger); margin-bottom:10px;">MASA AKTIF: 4 HARI</p>
        <div class="input-group">
            <label>USERNAME BARU</label>
            <input type="text" id="reg-free-user">
        </div>
        <div class="input-group">
            <label>PASSWORD BARU</label>
            <input type="password" id="reg-free-pass">
        </div>
        <button onclick="processRegisterFree()">BUAT AKUN FREE</button>
        <button class="secondary" onclick="navigateTo('login-page')">KEMBALI</button>
    </div>

    <!-- DASHBOARD -->
    <div id="dashboard-page" class="container hidden">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:10px;">
            <h2 style="margin:0; font-size:1.1em;">DASHBOARD CORE</h2>
            <span id="user-badge" style="font-size:0.55em; border:1px solid #555; padding:2px 6px; border-radius:8px;">STD</span>
        </div>

        <div class="status-bar">
            <span>USER: <b id="display-username">-</b></span>
            <span>EXP: <b id="expiry-date">-</b></span>
        </div>

        <!-- DEVICE LOCK SECTION (PREMIUM ONLY) -->
        <div id="lock-section" class="hidden" style="border:1px solid var(--premium); padding:12px; border-radius:8px; margin-bottom:15px; background:rgba(255,215,0,0.05);">
            <h3 style="color:var(--premium); font-size:0.8em; margin-bottom:10px; text-align:center;">🔒 LOCK PERANGKAT TARGET</h3>
            <div class="input-group">
                <label>NOMOR ANDA (PENGIRIM)</label>
                <input type="tel" id="lock-sender" placeholder="628xxx">
            </div>
            <div class="input-group">
                <label>NOMOR TARGET (UNTUK DI-LOCK)</label>
                <input type="tel" id="lock-target" placeholder="628xxx">
            </div>
            <div class="input-group">
                <label>SET KODE PEMBUKA (UNLOCK CODE)</label>
                <input type="text" id="lock-code" placeholder="Contoh: 123456">
            </div>
            <button class="premium-btn" onclick="executeDeviceLock()" style="font-size:0.75em;">INJEKSI DEVICE LOCK</button>
        </div>

        <div class="input-group">
            <label>NOMOR TARGET BUG (FORMAT 62)</label>
            <input type="tel" id="target-number" placeholder="628xxxxxxxxxx">
        </div>

        <h2 style="font-size:0.8em; text-align:left; margin:10px 0; color:var(--text-muted);">BUG LIST (<span id="bug-count">0</span>):</h2>
        <div class="bug-grid" id="bug-list">
            <!-- Bugs generated by JS -->
        </div>

        <div class="console-log" id="console-output">
            <div>> System Ready...</div>
        </div>

        <button class="danger" style="margin-top:10px;" onclick="logout()">LOGOUT</button>
    </div>

    <script>
        // --- CONFIG & ENCRYPTION ---
        const DB_KEY = "nexus_v1_data";
        const SESSION_KEY = "nexus_v1_session";
        
        // Kode "NEXUS-BUG" di-encode ke Base64
        const _0x5a2e = "TkVYVVMtQlVH"; 
        const getSecret = () => atob(_0x5a2e);

        // --- DATA BUGS (100 Unik) ---
        const BUG_TYPES = [
            "GHOST_CALL", "MEDIA_BURST", "RAM_EATER", "TEXT_BOMB", "SENSOR_BUG", "DB_CORRUPT", "NOTIF_SPAM", "FLICKER", "DARK_LOCK", "REPLY_LOOP",
            "BATTERY_DRAIN", "CPU_OVERHEAT", "GPS_GLITCH", "MIC_MUTE", "CAM_FREEZE", "STORAGE_FULL", "WIFI_DROP", "BLUETOOTH_LAG", "UI_SHAKE", "FONT_MESS",
            "KEYBOARD_STUCK", "STATUS_ERROR", "BIO_GLITCH", "PP_REMOVER", "CALL_BLOCK", "LINK_CORRUPT", "STICKER_BOMB", "VOICE_DISTORT", "THEME_CRASH", "PROXY_INJECT",
            "PACKET_LOSS", "RESTART_LOOP", "BOOT_DELAY", "TOUCH_FREEZE", "VOLUME_MAX", "BRIGHTNESS_LOW", "SCREEN_INVERT", "APP_FORCLOSE", "LOG_OVERFLOW", "CACHE_BOMB",
            "CONTACT_HIDE", "GROUP_EXIT", "READ_RECEIPT_BUG", "TYPING_GHOST", "ONLINE_STUCK", "OFFLINE_FORCE", "VERIFY_FAIL", "OTP_BLOCK", "E2EE_ERROR", "SERVER_TIMEOUT",
            "DNS_POISON", "SSL_INVALID", "API_ERROR", "TOKEN_EXPIRE", "AUTH_FAIL", "SQL_INJECT_SIM", "JS_ERROR_SIM", "CSS_MESS", "HTML_STRIP", "DOM_CRASH",
            "XSS_SIMULATE", "FRAME_DROP", "PING_SPIKE", "LATENCY_HIGH", "PORT_BLOCK", "FIREWALL_BYPASS", "SHELL_EXEC", "CMD_ERROR", "BASH_CRASH", "ROOT_FAIL",
            "SYSTEM_LAG", "KERNEL_PANIC", "DRV_ERROR", "BIOS_GLITCH", "GPU_ARTIFACT", "RAM_LEAK", "SWAP_FULL", "ZOMBIE_PROC", "THREAD_LOCK", "MUTEX_WAIT",
            "DEADLOCK_SIM", "IO_WAIT", "FS_CORRUPT", "MBR_WIPE_SIM", "PARTITION_ERROR", "BOOTLOADER_BUG", "RECOVERY_FAIL", "ADB_DISABLE", "FASTBOOT_ERROR", "ODIN_FAIL",
            "WIPE_SIMULATE", "FACTORY_RESET_BUG", "OTA_BLOCK", "PATCH_FAIL", "SECURITY_BREACH", "ENCRYPT_ERROR", "DECRYPT_FAIL", "RSA_MESS", "AES_CORRUPT", "MD5_COLLISION"
        ];

        // --- CORE FUNCTIONS ---
        function showNotification(msg) {
            const n = document.getElementById('notification');
            n.innerText = msg;
            n.style.display = 'block';
            setTimeout(() => n.style.display = 'none', 3000);
        }

        function navigateTo(id) {
            document.querySelectorAll('.container').forEach(c => c.classList.add('hidden'));
            document.getElementById(id).classList.remove('hidden');
        }

        // --- REGISTRATION FUNCTIONS ---
        function openTikTok() { 
            window.open('https://www.tiktok.com/@kiboyaslinofake', '_blank'); 
            setTimeout(() => {
                document.getElementById('tiktok-next').classList.remove('hidden');
                showNotification("Tombol Lanjutkan sudah aktif!");
            }, 500);
        }

        function openWAChannel() { 
            window.open('https://whatsapp.com/channel/0029VbDBArY9MF8uLpmviF1S', '_blank'); 
            setTimeout(() => {
                document.getElementById('wa-next').classList.remove('hidden');
                showNotification("Tombol Lanjutkan sudah aktif!");
            }, 500);
        }

        function getUsers() { 
            return JSON.parse(localStorage.getItem(DB_KEY) || "[]"); 
        }

        function saveUsers(u) { 
            localStorage.setItem(DB_KEY, JSON.stringify(u)); 
        }

        function processRegisterPremium() {
            const code = document.getElementById('reg-prem-code').value;
            const u = document.getElementById('reg-prem-user').value;
            const p = document.getElementById('reg-prem-pass').value;
            
            if (code !== getSecret()) {
                showNotification("KODE VERIFIKASI TIDAK VALID!");
                return;
            }
            if (u.length < 4) {
                showNotification("Username min 4 karakter!");
                return;
            }
            
            let users = getUsers();
            if (users.find(x => x.username === u)) {
                showNotification("Username sudah ada!");
                return;
            }
            
            users.push({ username: u, password: p, type: "PREMIUM", expiredAt: "LIFETIME" });
            saveUsers(users);
            showNotification("Premium Aktif!");
            navigateTo('login-page');
            document.getElementById('reg-prem-code').value = '';
            document.getElementById('reg-prem-user').value = '';
            document.getElementById('reg-prem-pass').value = '';
        }

        function processRegisterFree() {
            const u = document.getElementById('reg-free-user').value;
            const p = document.getElementById('reg-free-pass').value;
            
            if (u.length < 4) {
                showNotification("Username min 4 karakter!");
                return;
            }
            
            let users = getUsers();
            if (users.find(x => x.username === u)) {
                showNotification("Username sudah ada!");
                return;
            }
            
            const exp = new Date();
            exp.setDate(exp.getDate() + 4);
            
            users.push({ username: u, password: p, type: "FREE", expiredAt: exp.toISOString() });
            saveUsers(users);
            showNotification("Akun Free Aktif 4 Hari!");
            navigateTo('login-page');
            document.getElementById('reg-free-user').value = '';
            document.getElementById('reg-free-pass').value = '';
        }

        function handleLogin() {
            const u = document.getElementById('login-user').value;
            const p = document.getElementById('login-pass').value;
            
            if (!u || !p) {
                showNotification("Username dan Password harus diisi!");
                return;
            }
            
            const users = getUsers();
            const found = users.find(x => x.username === u && x.password === p);
            
            if (found) {
                if (found.expiredAt !== "LIFETIME" && new Date() > new Date(found.expiredAt)) {
                    showNotification("Akun Kadaluarsa!");
                    return;
                }
                localStorage.setItem(SESSION_KEY, JSON.stringify(found));
                loadDashboard(found);
            } else {
                showNotification("Login Gagal!");
            }
        }

        // --- DASHBOARD LOGIC ---
        function loadDashboard(acc) {
            navigateTo('dashboard-page');
            document.getElementById('display-username').innerText = acc.username;
            const badge = document.getElementById('user-badge');
            const exp = document.getElementById('expiry-date');
            const lockSec = document.getElementById('lock-section');
            const bugList = document.getElementById('bug-list');
            bugList.innerHTML = "";

            const isPrem = acc.type === "PREMIUM";
            const limit = isPrem ? 100 : 50;
            document.getElementById('bug-count').innerText = limit;

            if (isPrem) {
                badge.innerText = "PREMIUM";
                badge.style.color = "var(--premium)";
                badge.style.borderColor = "var(--premium)";
                exp.innerText = "PERMANENT";
                exp.style.color = "var(--premium)";
                lockSec.classList.remove('hidden');
            } else {
                badge.innerText = "FREE";
                badge.style.color = "var(--primary)";
                exp.innerText = new Date(acc.expiredAt).toLocaleDateString('id-ID');
                exp.style.color = "#888";
                lockSec.classList.add('hidden');
            }

            for (let i = 0; i < limit; i++) {
                const name = BUG_TYPES[i] || `BUG_EXT_${i}`;
                const btn = document.createElement('div');
                btn.className = "bug-btn" + (i >= 50 ? " prem-only" : "");
                btn.innerHTML = `<span style="font-size:1.5em;">${i % 2 === 0 ? '💥' : '🔥'}</span><span>${name.replace(/_/g, ' ')}</span>`;
                btn.onclick = () => sendBug(name);
                bugList.appendChild(btn);
            }
        }

        function executeDeviceLock() {
            const sender = document.getElementById('lock-sender').value;
            const target = document.getElementById('lock-target').value;
            const code = document.getElementById('lock-code').value;
            
            if (!sender || !target || !code) {
                showNotification("Lengkapi semua data Lock!");
                return;
            }
            
            addLog(`MENYIAPKAN DEVICE LOCK...`);
            setTimeout(() => {
                addLog(`✓ DEVICE ${target} BERHASIL DI-LOCK!`);
                showNotification("TARGET BERHASIL DI-LOCK!");
            }, 2000);
        }

        function addLog(msg) {
            const out = document.getElementById('console-output');
            const div = document.createElement('div');
            div.innerText = "> " + msg;
            out.appendChild(div);
            out.scrollTop = out.scrollHeight;
        }

        function sendBug(type) {
            const target = document.getElementById('target-number').value;
            if (!target.startsWith('62') || target.length < 11) {
                showNotification("Nomor target tidak valid!");
                return;
            }
            
            addLog(`INJEKSI: ${type} KE ${target}...`);
            setTimeout(() => {
                addLog(`✓ PAYLOAD ${type} TERKIRIM!`);
                showNotification("BUG BERHASIL DIKIRIM!");
            }, 1500);
        }

        function logout() {
            localStorage.removeItem(SESSION_KEY);
            navigateTo('login-page');
            document.getElementById('login-user').value = '';
            document.getElementById('login-pass').value = '';
        }

        // --- INIT ---
        window.onload = () => {
            const s = localStorage.getItem(SESSION_KEY);
            if (s) {
                const acc = JSON.parse(s);
                if (acc.expiredAt === "LIFETIME" || new Date() < new Date(acc.expiredAt)) {
                    loadDashboard(acc);
                } else {
                    localStorage.removeItem(SESSION_KEY);
                    navigateTo('login-page');
                }
            } else 
                navigateTo('login-page');
            }
        };
    </script>
</body>
</html>
j
