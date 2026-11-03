# ntjk-app
<!DOCTYPE html>
<html lang="th" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>NTJK Mobile App</title>
    <!-- Web App Manifest for PWA -->
    <link rel="manifest" href="data:application/json;base64,ewogICJuYW1lIjogIk5USksgS2V5IEFwcCIsCiAgInNob3J0X25hbWUiOiAiTlRKSyIsCiAgInN0YXJ0X3VybCI6ICIuIiwKICAiZGlzcGxheSI6ICJzdGFuZGFsb25lIiwKICAiYmFja2dyb3VuZF9jb2xvciI6ICIjMGEwZDE0IiwKICAidGhlbWVfY29sb3IiOiAiIzBhMGQxNCIKZX0=">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="NTJK App">
    
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        app: {
                            bg: '#090d16',
                            card: '#111726',
                            border: '#1e293b',
                            primary: '#06b6d4',
                            accent: '#10b981'
                        }
                    }
                }
            }
        }
    </script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;600;700&family=Kanit:wght@300;400;500;600;700&display=swap');
        body { 
            font-family: 'Kanit', sans-serif;
            -webkit-tap-highlight-color: transparent;
            user-select: none;
        }
        .font-mono { font-family: 'Fira Code', monospace; }
        .custom-scrollbar::-webkit-scrollbar { width: 4px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: #090d16; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: #1e293b; border-radius: 4px; }
        
        /* Safe area for mobile notches */
        .pt-safe { padding-top: env(safe-area-inset-top, 16px); }
        .pb-safe { padding-bottom: env(safe-area-inset-bottom, 20px); }
    </style>
</head>
<body class="bg-app-bg text-slate-100 min-h-screen flex flex-col justify-between selection:bg-cyan-500 selection:text-black overflow-x-hidden">

    <!-- Top App Bar -->
    <header class="pt-safe bg-app-card/90 backdrop-blur-lg border-b border-app-border sticky top-0 z-40 px-4 py-3 flex items-center justify-between">
        <div class="flex items-center gap-2.5">
            <div class="w-8 h-8 rounded-xl bg-cyan-500/10 border border-cyan-500/30 flex items-center justify-center text-cyan-400 shadow-md shadow-cyan-500/10">
                <i data-lucide="key-round" class="w-4 h-4"></i>
            </div>
            <div>
                <h1 class="text-sm font-bold text-white tracking-wide flex items-center gap-1.5">
                    NTJK APP
                    <span class="text-[9px] font-mono px-1.5 py-0.2 rounded bg-cyan-950 text-cyan-400 border border-cyan-800">MOBILE</span>
                </h1>
                <p class="text-[10px] text-slate-400">License Key & Store Manager</p>
            </div>
        </div>

        <button onclick="triggerVibrate(); clearAllData();" class="p-2 text-slate-400 hover:text-red-400 transition" title="ล้างข้อมูล">
            <i data-lucide="rotate-ccw" class="w-4 h-4"></i>
        </button>
    </header>

    <!-- Main Mobile Content Area -->
    <main class="flex-1 max-w-md w-full mx-auto p-4 pb-24 space-y-4">

        <!-- PAGE 1: KEY GENERATOR (DEFAULT) -->
        <div id="pageKey" class="space-y-4">
            
            <!-- Hero Result Card -->
            <div class="bg-app-card border border-app-border rounded-2xl p-4 shadow-2xl relative overflow-hidden">
                <div class="flex justify-between items-center mb-2">
                    <span class="text-[10px] font-bold uppercase tracking-wider text-cyan-400 flex items-center gap-1">
                        <span class="w-1.5 h-1.5 rounded-full bg-cyan-400 animate-ping"></span>
                        Key Generated
                    </span>
                    <span class="text-[10px] text-slate-500 font-mono">Format: NTJK-XXXX</span>
                </div>

                <div class="relative mb-3">
                    <input type="text" id="singleKey" readonly value="NTJK-XXXX-XXXX-XXXX" 
                        class="w-full bg-slate-950 border border-app-border rounded-xl py-3 px-3 font-mono text-xl font-bold text-cyan-300 text-center focus:outline-none tracking-wider">
                </div>

                <div class="grid grid-cols-2 gap-2">
                    <button onclick="triggerVibrate(); genSingleKey();" class="bg-gradient-to-r from-cyan-500 to-blue-600 active:scale-95 text-slate-950 font-bold py-3 rounded-xl transition flex items-center justify-center gap-1.5 text-xs shadow-lg shadow-cyan-500/20">
                        <i data-lucide="refresh-cw" id="spinIcon" class="w-4 h-4"></i> สุ่มคีย์ใหม่
                    </button>
                    <button onclick="triggerVibrate(); copyText(document.getElementById('singleKey').value);" class="bg-slate-800 active:scale-95 text-slate-200 font-semibold py-3 rounded-xl transition flex items-center justify-center gap-1.5 text-xs border border-app-border">
                        <i data-lucide="copy" class="w-4 h-4"></i> คัดลอกคีย์
                    </button>
                </div>
            </div>

            <!-- Key Config Tool -->
            <div class="bg-app-card border border-app-border rounded-2xl p-4 space-y-3">
                <h2 class="text-xs font-bold text-slate-200 flex items-center gap-1.5">
                    <i data-lucide="sliders" class="w-4 h-4 text-cyan-400"></i> ตั้งค่ารูปแบบ
                </h2>
                
                <div class="grid grid-cols-3 gap-2 text-xs">
                    <div>
                        <label class="block text-slate-400 mb-1 text-[11px]">Prefix</label>
                        <input type="text" id="prefixInput" value="NTJK" class="w-full bg-slate-950 border border-app-border rounded-lg p-2 font-mono text-cyan-300 uppercase text-center focus:outline-none">
                    </div>
                    <div>
                        <label class="block text-slate-400 mb-1 text-[11px]">จำนวนบล็อก</label>
                        <select id="blocksInput" class="w-full bg-slate-950 border border-app-border rounded-lg p-2 font-mono text-slate-200 focus:outline-none">
                            <option value="3" selected>3 บล็อก</option>
                            <option value="4">4 บล็อก</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-slate-400 mb-1 text-[11px]">ความยาว</label>
                        <select id="lengthInput" class="w-full bg-slate-950 border border-app-border rounded-lg p-2 font-mono text-slate-200 focus:outline-none">
                            <option value="4" selected>4 หลัก</option>
                            <option value="5">5 หลัก</option>
                        </select>
                    </div>
                </div>
            </div>

            <!-- Batch Generator Tool -->
            <div class="bg-app-card border border-app-border rounded-2xl p-4 space-y-3">
                <h2 class="text-xs font-bold text-slate-200 flex items-center gap-1.5">
                    <i data-lucide="layers" class="w-4 h-4 text-cyan-400"></i> สุ่มทีละหลายคีย์ (Batch)
                </h2>
                <div class="flex gap-2">
                    <input type="number" id="batchQty" value="5" min="1" max="50" class="w-20 bg-slate-950 border border-app-border rounded-xl text-center font-mono text-xs text-slate-200 focus:outline-none">
                    <button onclick="triggerVibrate(); genBatchKeys();" class="flex-1 bg-cyan-600 hover:bg-cyan-500 active:scale-95 text-slate-950 font-bold py-2.5 rounded-xl text-xs transition">
                        สุ่มตามจำนวน
                    </button>
                </div>
                <textarea id="batchArea" readonly rows="3" placeholder="ผลลัพธ์คีย์หลายตัว..." 
                    class="w-full bg-slate-950 border border-app-border rounded-xl p-3 font-mono text-xs text-cyan-300 focus:outline-none resize-none custom-scrollbar"></textarea>
                <div class="flex justify-end gap-2">
                    <button onclick="triggerVibrate(); copyText(document.getElementById('batchArea').value);" class="px-3 py-1.5 bg-slate-800 active:scale-95 text-[11px] rounded-lg border border-app-border text-slate-300">คัดลอกทั้งหมด</button>
                    <button onclick="triggerVibrate(); downloadTxt();" class="px-3 py-1.5 bg-slate-800 active:scale-95 text-[11px] rounded-lg border border-app-border text-slate-300">โหลด .TXT</button>
                </div>
            </div>

        </div>

        <!-- PAGE 2: VALIDATOR (HIDDEN BY DEFAULT) -->
        <div id="pageValidator" class="space-y-4 hidden">
            <div class="bg-app-card border border-app-border rounded-2xl p-4 space-y-3">
                <h2 class="text-xs font-bold text-slate-200 flex items-center gap-1.5">
                    <i data-lucide="shield-check" class="w-4 h-4 text-cyan-400"></i> ตรวจสอบคีย์ (Validator)
                </h2>
                <p class="text-[11px] text-slate-400">ใส่คีย์เพื่อเช็คโครงสร้างและความปลอดภัย</p>
                <input type="text" id="valInput" placeholder="NTJK-XXXX-XXXX-XXXX" class="w-full bg-slate-950 border border-app-border rounded-xl p-3 font-mono text-sm text-cyan-300 uppercase focus:outline-none">
                <button onclick="triggerVibrate(); checkKey();" class="w-full bg-cyan-500 text-slate-950 font-bold py-2.5 rounded-xl text-xs transition">
                    เริ่มตรวจสอบ
                </button>
                <div id="valResult" class="hidden p-3 rounded-xl bg-slate-950 border border-slate-800 space-y-1 text-xs">
                    <p id="valStatus" class="font-bold text-emerald-400"></p>
                    <p id="valDesc" class="text-[11px] text-slate-400"></p>
                </div>
            </div>
        </div>

        <!-- PAGE 3: STORE (HIDDEN BY DEFAULT) -->
        <div id="pageStore" class="space-y-4 hidden">
            <div class="bg-app-card border border-app-border rounded-2xl p-4">
                <h2 class="text-xs font-bold text-cyan-400 mb-1">NTJK Software Store</h2>
                <p class="text-[11px] text-slate-400">เลือกร้านค้าจำลองสำหรับสั่งซื้อคีย์</p>
            </div>

            <div class="space-y-3">
                <div class="bg-app-card border border-app-border p-3 rounded-2xl flex items-center justify-between">
                    <div>
                        <span class="text-[9px] bg-cyan-950 text-cyan-400 px-1.5 py-0.5 rounded font-mono border border-cyan-800">WINDOWS</span>
                        <h3 class="font-bold text-xs text-slate-100 mt-1">Windows 11 Pro Key</h3>
                        <p class="text-xs font-bold text-cyan-400">฿490</p>
                    </div>
                    <button onclick="triggerVibrate(); addToCart('Windows 11 Pro', 490);" class="bg-cyan-500 text-slate-950 text-xs font-bold px-3 py-2 rounded-xl active:scale-95">
                        + ใส่ตะกร้า
                    </button>
                </div>

                <div class="bg-app-card border border-app-border p-3 rounded-2xl flex items-center justify-between">
                    <div>
                        <span class="text-[9px] bg-purple-950 text-purple-400 px-1.5 py-0.5 rounded font-mono border border-purple-800">OFFICE</span>
                        <h3 class="font-bold text-xs text-slate-100 mt-1">MS Office 2021 Key</h3>
                        <p class="text-xs font-bold text-cyan-400">฿790</p>
                    </div>
                    <button onclick="triggerVibrate(); addToCart('MS Office 2021', 790);" class="bg-cyan-500 text-slate-950 text-xs font-bold px-3 py-2 rounded-xl active:scale-95">
                        + ใส่ตะกร้า
                    </button>
                </div>
            </div>

            <!-- Cart Section -->
            <div class="bg-app-card border border-app-border rounded-2xl p-4 space-y-2">
                <h3 class="text-xs font-bold text-slate-300">ตะกร้าของคุณ</h3>
                <div id="cartList" class="space-y-1 text-xs font-mono text-slate-400">
                    <p class="text-center py-2 text-[11px]">ไม่มีสินค้าในตะกร้า</p>
                </div>
                <div class="flex justify-between items-center pt-2 border-t border-slate-800 text-xs font-mono">
                    <span>รวมทั้งหมด:</span>
                    <span id="cartTotal" class="text-cyan-400 font-bold">฿0</span>
                </div>
            </div>
        </div>

        <!-- PAGE 4: HISTORY (HIDDEN BY DEFAULT) -->
        <div id="pageHistory" class="space-y-4 hidden">
            <div class="bg-app-card border border-app-border rounded-2xl p-4 flex flex-col h-[350px]">
                <div class="flex justify-between items-center mb-3 pb-2 border-b border-app-border">
                    <h2 class="text-xs font-bold text-slate-200 flex items-center gap-1.5">
                        <i data-lucide="history" class="w-4 h-4 text-cyan-400"></i> ประวัติการสุ่ม
                    </h2>
                    <button onclick="triggerVibrate(); clearHist();" class="text-[10px] text-slate-500 hover:text-red-400">ล้างประวัติ</button>
                </div>
                <div id="histContainer" class="flex-1 overflow-y-auto space-y-1.5 pr-1 custom-scrollbar font-mono text-xs">
                    <div class="text-center text-slate-600 py-10">ยังไม่มีประวัติ</div>
                </div>
            </div>
        </div>

    </main>

    <!-- Bottom Mobile App Navigation Bar -->
    <nav class="fixed bottom-0 left-0 right-0 bg-app-card/95 backdrop-blur-lg border-t border-app-border pb-safe z-40">
        <div class="max-w-md mx-auto grid grid-cols-4 px-2 py-2">
            <button id="navKey" onclick="switchPage('pageKey', 'navKey')" class="flex flex-col items-center gap-1 p-2 text-cyan-400 font-bold transition">
                <i data-lucide="key" class="w-5 h-5"></i>
                <span class="text-[10px]">สุ่มคีย์</span>
            </button>
            <button id="navVal" onclick="switchPage('pageValidator', 'navVal')" class="flex flex-col items-center gap-1 p-2 text-slate-500 transition">
                <i data-lucide="shield-check" class="w-5 h-5"></i>
                <span class="text-[10px]">ตรวจสอบ</span>
            </button>
            <button id="navStore" onclick="switchPage('pageStore', 'navStore')" class="flex flex-col items-center gap-1 p-2 text-slate-500 transition">
                <i data-lucide="shopping-bag" class="w-5 h-5"></i>
                <span class="text-[10px]">ร้านค้า</span>
            </button>
            <button id="navHist" onclick="switchPage('pageHistory', 'navHist')" class="flex flex-col items-center gap-1 p-2 text-slate-500 transition">
                <i data-lucide="history" class="w-5 h-5"></i>
                <span class="text-[10px]">ประวัติ</span>
            </button>
        </div>
    </nav>

    <!-- Mobile Toast Alert -->
    <div id="appToast" class="fixed top-14 left-1/2 -translate-x-1/2 bg-cyan-500 text-slate-950 font-bold px-4 py-2 rounded-full shadow-2xl transition-all duration-300 transform -translate-y-20 opacity-0 text-xs flex items-center gap-1.5 z-50 pointer-events-none">
        <i data-lucide="check-circle-2" class="w-4 h-4"></i>
        <span id="toastMsg">คัดลอกสำเร็จ</span>
    </div>

    <!-- Application Script -->
    <script>
        let history = [];
        let cart = [];

        document.addEventListener('DOMContentLoaded', () => {
            lucide.createIcons();
            genSingleKey();
        });

        // Mobile Vibration Feedback
        function triggerVibrate() {
            if (navigator.vibrate) navigator.vibrate(25);
        }

        // Bottom Nav Switcher
        function switchPage(pageId, navId) {
            triggerVibrate();
            ['pageKey', 'pageValidator', 'pageStore', 'pageHistory'].forEach(p => {
                document.getElementById(p).classList.add('hidden');
            });
            ['navKey', 'navVal', 'navStore', 'navHist'].forEach(n => {
                const el = document.getElementById(n);
                el.className = "flex flex-col items-center gap-1 p-2 text-slate-500 transition";
            });

            document.getElementById(pageId).classList.remove('hidden');
            document.getElementById(navId).className = "flex flex-col items-center gap-1 p-2 text-cyan-400 font-bold transition";
        }

        // Key Generator Logic
        function buildKey() {
            const prefix = document.getElementById('prefixInput').value.trim() || 'NTJK';
            const blocks = parseInt(document.getElementById('blocksInput').value);
            const length = parseInt(document.getElementById('lengthInput').value);
            const pool = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";

            let parts = [prefix];
            for(let b=0; b<blocks; b++) {
                let seg = "";
                for(let l=0; l<length; l++) {
                    seg += pool.charAt(Math.floor(Math.random() * pool.length));
                }
                parts.push(seg);
            }
            return parts.join('-');
        }

        function genSingleKey() {
            const icon = document.getElementById('spinIcon');
            if (icon) icon.classList.add('animate-spin');
            setTimeout(() => icon && icon.classList.remove('animate-spin'), 300);

            const key = buildKey();
            document.getElementById('singleKey').value = key;
            saveHistory(key);
        }

        function genBatchKeys() {
            const qty = parseInt(document.getElementById('batchQty').value) || 5;
            let list = [];
            for(let i=0; i<qty; i++) {
                const k = buildKey();
                list.push(k);
                saveHistory(k);
            }
            document.getElementById('batchArea').value = list.join('\n');
            showToast(`สุ่มสำเร็จ ${qty} คีย์`);
        }

        function saveHistory(k) {
            const t = new Date().toLocaleTimeString('th-TH', { hour: '2-digit', minute: '2-digit' });
            history.unshift({ k, t });
            if(history.length > 30) history.pop();
            renderHistory();
        }

        function renderHistory() {
            const box = document.getElementById('histContainer');
            if(history.length === 0) {
                box.innerHTML = `<div class="text-center text-slate-600 py-10">ยังไม่มีประวัติ</div>`;
                return;
            }
            box.innerHTML = history.map(item => `
                <div class="flex justify-between items-center bg-slate-950 p-2.5 rounded-xl border border-app-border text-cyan-300">
                    <span class="truncate font-semibold">${item.k}</span>
                    <span class="text-[10px] text-slate-500">${item.t}</span>
                </div>
            `).join('');
        }

        function clearHist() {
            history = [];
            renderHistory();
            showToast('ล้างประวัติแล้ว');
        }

        function checkKey() {
            const val = document.getElementById('valInput').value.trim();
            const res = document.getElementById('valResult');
            const status = document.getElementById('valStatus');
            const desc = document.getElementById('valDesc');
            
            if(!val) return;
            res.classList.remove('hidden');
            if(val.toUpperCase().startsWith('NTJK-')) {
                status.innerText = "✓ คีย์ถูกต้อง (Valid)";
                status.className = "font-bold text-emerald-400";
                desc.innerText = "ตรงตามมาตรฐานโครงสร้าง NTJK";
            } else {
                status.innerText = "✕ รูปแบบไม่ถูกต้อง";
                status.className = "font-bold text-rose-400";
                desc.innerText = "ควรขึ้นต้นด้วย NTJK-";
            }
        }

        function addToCart(name, price) {
            cart.push({ name, price });
            const list = document.getElementById('cartList');
            const total = document.getElementById('cartTotal');
            let sum = 0;
            list.innerHTML = cart.map(i => {
                sum += i.price;
                return `<div class="flex justify-between bg-slate-950 p-2 rounded-lg"><span>${i.name}</span><span class="text-cyan-400">฿${i.price}</span></div>`;
            }).join('');
            total.innerText = `฿${sum}`;
            showToast(`เพิ่ม ${name} แล้ว`);
        }

        function copyText(txt) {
            if(!txt) return;
            navigator.clipboard.writeText(txt);
            showToast('คัดลอกสำเร็จ!');
        }

        function downloadTxt() {
            const txt = document.getElementById('batchArea').value || document.getElementById('singleKey').value;
            const blob = new Blob([txt], { type: 'text/plain' });
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = 'keys.txt';
            a.click();
            showToast('ดาวน์โหลดไฟล์สำเร็จ');
        }

        function showToast(msg) {
            const toast = document.getElementById('appToast');
            document.getElementById('toastMsg').innerText = msg;
            toast.classList.remove('-translate-y-20', 'opacity-0');
            setTimeout(() => toast.classList.add('-translate-y-20', 'opacity-0'), 2000);
        }

        function clearAllData() {
            history = [];
            cart = [];
            renderHistory();
            document.getElementById('cartList').innerHTML = `<p class="text-center py-2 text-[11px]">ไม่มีสินค้าในตะกร้า</p>`;
            document.getElementById('cartTotal').innerText = `฿0`;
            showToast('รีเซ็ตแอปพลิเคชันแล้ว');
        }
    </script>
</body>
</html>
```

---

### 💡 วิธีเปลี่ยนเป็น Mobile App บนมือถือจริง (Add to Home Screen):
1. นำไฟล์ `index.html` นี้ไปสร้างเป็นลิงก์เว็บบนบริการอย่าง **Netlify Drop** หรือ **CodePen** 
2. เปิดลิงก์เว็บดังกล่าวผ่านเบราว์เซอร์บนมือถือ (เช่น Safari บน iPhone หรือ Chrome บน Android)
3. กดปุ่ม **Share (แชร์)** หรือเมนู 3 จุด แล้วเลือก **"เพิ่มลงในหน้าจอโฮม" (Add to Home Screen)** 
4. คุณจะได้ไอคอนแอปขึ้นมาอยู่บนหน้าจอมือถือ สามารถกดเปิดใช้งานเต็มจอแบบ App ได้เลยครับ