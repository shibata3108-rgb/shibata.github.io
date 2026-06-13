# shibata.github.io
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ミラクルくじ引き (Miracle Draw)</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts Inter & Zen Maru Gothic -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&family=Zen+Maru+Gothic:wght@500;700;900&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Zen Maru Gothic', 'Inter', sans-serif;
            background: radial-gradient(circle at center, #1e1b4b 0%, #0f0b25 100%);
        }
        /* Custom scrollbar for dark theme */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(15, 11, 37, 0.5);
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(124, 58, 237, 0.5);
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: rgba(124, 58, 237, 0.8);
        }
        /* 3D Box & Glow Effects */
        .neon-glow {
            box-shadow: 0 0 20px rgba(139, 92, 246, 0.4);
        }
        .neon-text {
            text-shadow: 0 0 10px rgba(192, 132, 252, 0.6);
        }
        /* Box Shaking & Opening Animations */
        @keyframes shake {
            0%, 100% { transform: rotate(0deg) scale(1); }
            20% { transform: rotate(-8deg) scale(1.05); }
            40% { transform: rotate(6deg) scale(1.05); }
            60% { transform: rotate(-5deg) scale(1.05); }
            80% { transform: rotate(4deg) scale(1.05); }
        }
        .animate-shake {
            animation: shake 0.5s ease-in-out infinite;
        }
        /* Lucky Box Lid Animation */
        @keyframes openLid {
            0% { transform: translateY(0) rotate(0); }
            100% { transform: translateY(-30px) rotate(-15deg) scale(0.9); opacity: 0; }
        }
        .open-lid {
            animation: openLid 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
        }
    </style>
</head>
<body class="text-slate-100 min-h-screen flex flex-col justify-between overflow-x-hidden">

    <!-- Header -->
    <header class="border-b border-indigo-950/80 bg-slate-950/60 backdrop-blur-md sticky top-0 z-40 px-4 py-3">
        <div class="max-w-7xl mx-auto flex items-center justify-between">
            <div class="flex items-center space-x-3">
                <div class="p-2 bg-gradient-to-tr from-purple-600 to-pink-500 rounded-xl shadow-lg">
                    <!-- Present Icon (SVG) -->
                    <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v13m0-13V6a2 2 0 112 2h-2zm0 0V5a2 2 0 10-2 2h2zm-2 4h4M5 20h14a2 2 0 002-2V9a2 2 0 00-2-2H5a2 2 0 00-2 2v9a2 2 0 002 2z" />
                    </svg>
                </div>
                <div>
                    <h1 class="text-xl md:text-2xl font-black bg-clip-text text-transparent bg-gradient-to-r from-purple-400 via-pink-400 to-amber-300 neon-text">
                        ミラクルくじ引き
                    </h1>
                    <p class="text-xs text-indigo-300">Miracle Draw - 運命を決める瞬間</p>
                </div>
            </div>

            <!-- Audio & System Status -->
            <div class="flex items-center space-x-3">
                <button id="soundToggle" class="flex items-center space-x-1.5 px-3 py-1.5 rounded-lg bg-indigo-950/60 hover:bg-indigo-900/80 border border-indigo-800 transition-all text-xs md:text-sm">
                    <span id="soundIcon">🔊</span>
                    <span id="soundStatus" class="hidden sm:inline">サウンド: ON</span>
                </button>
                <button id="resetApp" class="px-3 py-1.5 rounded-lg bg-red-950/50 hover:bg-red-900/80 border border-red-800/60 transition-all text-xs md:text-sm text-red-200">
                    初期化
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content Grid -->
    <main class="flex-grow max-w-7xl w-full mx-auto px-4 py-6 grid grid-cols-1 lg:grid-cols-12 gap-6 items-start">
        
        <!-- Left Column: Inputs & Customizer (5 cols on lg) -->
        <section class="lg:col-span-5 space-y-6">
            <!-- Lottery Settings Card -->
            <div class="bg-indigo-950/40 border border-indigo-800/40 rounded-2xl p-5 backdrop-blur-md shadow-xl">
                <h2 class="text-lg font-bold text-indigo-200 mb-4 flex items-center space-x-2">
                    <span>⚙️</span>
                    <span>1. くじの準備をする</span>
                </h2>

                <!-- Presets Quick buttons -->
                <div class="mb-4">
                    <label class="text-xs text-indigo-300 block mb-2">クイック作成テンプレート:</label>
                    <div class="flex flex-wrap gap-2">
                        <button onclick="loadPreset('yesno')" class="text-xs bg-indigo-900/50 hover:bg-indigo-800 text-indigo-200 px-2.5 py-1 rounded-md border border-indigo-700/50 transition">
                            ⭕️ Yes / No
                        </button>
                        <button onclick="loadPreset('numbers')" class="text-xs bg-indigo-900/50 hover:bg-indigo-800 text-indigo-200 px-2.5 py-1 rounded-md border border-indigo-700/50 transition">
                            🔢 数字 (1-10)
                        </button>
                        <button onclick="loadPreset('lunch')" class="text-xs bg-indigo-900/50 hover:bg-indigo-800 text-indigo-200 px-2.5 py-1 rounded-md border border-indigo-700/50 transition">
                            🍔 今日のランチ
                        </button>
                        <button onclick="loadPreset('penalty')" class="text-xs bg-indigo-900/50 hover:bg-indigo-800 text-indigo-200 px-2.5 py-1 rounded-md border border-indigo-700/50 transition">
                            🍺 罰ゲーム
                        </button>
                    </div>
                </div>

                <!-- Input Textarea -->
                <div class="mb-4">
                    <div class="flex justify-between items-center mb-1.5">
                        <label for="candidatesInput" class="text-xs text-indigo-300 block">くじの候補（1行に1項目）:</label>
                        <span id="candidateCount" class="text-xs text-pink-400 font-bold">0項目登録</span>
                    </div>
                    <textarea id="candidatesInput" rows="6" class="w-full bg-slate-950/80 border border-indigo-700/50 rounded-xl p-3 text-sm focus:outline-none focus:border-pink-500 text-slate-100 placeholder-slate-500 font-sans transition-all" placeholder="ここに選択肢を改行区切りで入力してください&#10;例:&#10;りんご&#10;バナナ&#10;メロン"></textarea>
                </div>

                <!-- Advanced Options -->
                <div class="space-y-3 pt-3 border-t border-indigo-900/60 text-sm">
                    <div class="flex items-center justify-between">
                        <label for="removeOnDraw" class="text-xs text-indigo-200 flex items-center space-x-2 cursor-pointer">
                            <input type="checkbox" id="removeOnDraw" class="rounded border-indigo-600 text-purple-600 focus:ring-purple-500 bg-slate-900 w-4 h-4" checked>
                            <span>当選者を次回から除外する (重複防止)</span>
                        </label>
                    </div>

                    <div class="flex items-center justify-between">
                        <span class="text-xs text-indigo-200">一度に引く人数:</span>
                        <div class="flex items-center space-x-1">
                            <button onclick="adjustDrawCount(-1)" class="w-8 h-8 bg-indigo-900 hover:bg-indigo-800 rounded-lg flex items-center justify-center font-bold text-slate-200 transition">-</button>
                            <span id="drawCountVal" class="w-10 text-center font-bold text-pink-400">1</span>
                            <button onclick="adjustDrawCount(1)" class="w-8 h-8 bg-indigo-900 hover:bg-indigo-800 rounded-lg flex items-center justify-center font-bold text-slate-200 transition">+</button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Draw Mode Selector -->
            <div class="bg-indigo-950/40 border border-indigo-800/40 rounded-2xl p-5 backdrop-blur-md shadow-xl">
                <h2 class="text-lg font-bold text-indigo-200 mb-4 flex items-center space-x-2">
                    <span>✨</span>
                    <span>2. 演出モードを選ぶ</span>
                </h2>
                <div class="grid grid-cols-3 gap-2.5">
                    <!-- Mode Button: Wheel -->
                    <button id="btnModeWheel" onclick="setDrawMode('wheel')" class="flex flex-col items-center p-3 rounded-xl bg-gradient-to-b from-purple-600/30 to-purple-800/10 border-2 border-purple-500/80 text-purple-200 transition-all shadow-md">
                        <span class="text-2xl mb-1">🎡</span>
                        <span class="text-xs font-bold">ルーレット</span>
                    </button>
                    <!-- Mode Button: Box -->
                    <button id="btnModeBox" onclick="setDrawMode('box')" class="flex flex-col items-center p-3 rounded-xl bg-indigo-950/30 border border-indigo-800/50 hover:border-indigo-700 text-indigo-300 hover:text-indigo-100 transition-all">
                        <span class="text-2xl mb-1">🎁</span>
                        <span class="text-xs font-bold">千両宝箱</span>
                    </button>
                    <!-- Mode Button: Slot -->
                    <button id="btnModeSlot" onclick="setDrawMode('slot')" class="flex flex-col items-center p-3 rounded-xl bg-indigo-950/30 border border-indigo-800/50 hover:border-indigo-700 text-indigo-300 hover:text-indigo-100 transition-all">
                        <span class="text-2xl mb-1">⚡</span>
                        <span class="text-xs font-bold">高速スロット</span>
                    </button>
                </div>
            </div>
        </section>

        <!-- Right Column: Draw Execution Zone (7 cols on lg) -->
        <section class="lg:col-span-7 space-y-6">
            <!-- Play Zone Card -->
            <div class="bg-indigo-950/40 border border-indigo-800/40 rounded-2xl p-6 backdrop-blur-md shadow-2xl min-h-[480px] flex flex-col justify-between relative overflow-hidden neon-glow">
                
                <!-- Background visual grids -->
                <div class="absolute inset-0 bg-[linear-gradient(to_right,rgba(255,255,255,0.02)_1px,transparent_1px),linear-gradient(to_bottom,rgba(255,255,255,0.02)_1px,transparent_1px)] bg-[size:24px_24px] pointer-events-none"></div>

                <!-- Mode Title Indicator -->
                <div class="relative z-10 flex justify-between items-center border-b border-indigo-900/60 pb-3">
                    <div class="flex items-center space-x-2">
                        <span class="animate-pulse inline-block w-2.5 h-2.5 rounded-full bg-emerald-400"></span>
                        <span id="activeModeLabel" class="text-sm font-bold text-indigo-300">ルーレット・抽選ステージ</span>
                    </div>
                    <div class="text-xs text-indigo-400 font-mono" id="modeTip">矢印が止まった場所が当選！</div>
                </div>

                <!-- Stage Container (Contains changing views) -->
                <div class="relative flex-grow flex items-center justify-center py-6 z-10">

                    <!-- VIEW 1: ROULETTE (WHEEL) -->
                    <div id="viewWheel" class="w-full flex flex-col items-center justify-center space-y-4">
                        <div class="relative w-72 h-72 md:w-80 md:h-80 flex items-center justify-center">
                            <!-- Wheel Pin Indicator (Top) -->
                            <div class="absolute -top-1 z-20 w-8 h-8 drop-shadow-lg flex items-center justify-center transform origin-bottom">
                                <svg class="w-8 h-8 text-pink-500 fill-current" viewBox="0 0 24 24">
                                    <path d="M12 2C8.13 2 5 5.13 5 9c0 5.25 7 13 7 13s7-7.75 7-13c0-3.87-3.13-7-7-7zm0 9.5c-1.38 0-2.5-1.12-2.5-2.5s1.12-2.5 2.5-2.5 2.5 1.12 2.5 2.5-1.12 2.5-2.5 2.5z" />
                                </svg>
                            </div>
                            <!-- Canvas -->
                            <canvas id="wheelCanvas" width="360" height="360" class="w-full h-full rounded-full border-4 border-indigo-600/80 shadow-[0_0_30px_rgba(99,102,241,0.3)] bg-slate-900/50"></canvas>
                        </div>
                    </div>

                    <!-- VIEW 2: LUCKY BOX (GACHA) -->
                    <div id="viewBox" class="hidden w-full flex flex-col items-center justify-center space-y-6">
                        <p class="text-sm text-indigo-200">お好きな宝箱を1つ選んでタップしてください！</p>
                        <div class="grid grid-cols-2 sm:grid-cols-3 gap-6 max-w-md w-full px-4" id="boxesContainer">
                            <!-- Dynamic Boxes will be loaded here -->
                        </div>
                    </div>

                    <!-- VIEW 3: SLOT MACHINE (SHUFFLER) -->
                    <div id="viewSlot" class="hidden w-full flex flex-col items-center justify-center space-y-6">
                        <div class="w-full max-w-md bg-slate-950/80 rounded-2xl border-4 border-indigo-500/60 p-6 md:p-8 text-center relative shadow-[inset_0_0_20px_rgba(0,0,0,0.8)]">
                            <!-- Slot Top Border Ribbon -->
                            <div class="absolute -top-3 left-1/2 transform -translate-x-1/2 bg-indigo-500 text-slate-950 font-black px-4 py-0.5 rounded-full text-xs tracking-wider">
                                MEGASLOT
                            </div>
                            <!-- Slot Row -->
                            <div class="h-28 overflow-hidden flex items-center justify-center relative">
                                <div id="slotScroller" class="text-3xl md:text-5xl font-black text-pink-400 font-sans tracking-wide">
                                    準備完了!
                                </div>
                            </div>
                        </div>
                    </div>

                </div>

                <!-- Big Action Trigger Button -->
                <div class="relative z-10 pt-4 border-t border-indigo-900/60 flex flex-col sm:flex-row gap-3 items-center">
                    <button id="drawButton" onclick="triggerDraw()" class="w-full sm:flex-1 py-4 px-6 rounded-2xl bg-gradient-to-r from-purple-600 via-pink-500 to-amber-500 hover:from-purple-500 hover:via-pink-400 hover:to-amber-400 text-white font-black text-lg shadow-[0_4px_20px_rgba(219,39,119,0.4)] hover:shadow-[0_4px_25px_rgba(219,39,119,0.6)] transform hover:-translate-y-0.5 active:translate-y-0 transition-all flex items-center justify-center space-x-3">
                        <span class="text-2xl" id="btnIcon">🎉</span>
                        <span id="btnText">抽選スタート！</span>
                    </button>
                    <!-- Fast Draw Switch Option -->
                    <label class="text-xs text-indigo-300 flex items-center space-x-2 bg-indigo-950/80 px-4 py-3 rounded-xl border border-indigo-800/60 cursor-pointer hover:bg-indigo-900/50 transition">
                        <input type="checkbox" id="fastDraw" class="rounded border-indigo-600 text-purple-600 bg-slate-900 w-4 h-4">
                        <span>演出をスキップする</span>
                    </label>
                </div>

            </div>
        </section>
    </main>

    <!-- Log & History Section (Bottom Full-Width) -->
    <section class="max-w-7xl w-full mx-auto px-4 pb-12 mt-4">
        <div class="grid grid-cols-1 md:grid-cols-12 gap-6">
            <!-- Left: Current Candidates quick viewer -->
            <div class="md:col-span-5 bg-indigo-950/20 border border-indigo-900/40 rounded-2xl p-5 backdrop-blur-sm">
                <h3 class="text-sm font-bold text-indigo-300 mb-2.5 flex items-center justify-between">
                    <span>📋 現在の候補リスト</span>
                    <span id="activeCountLabel" class="text-xs bg-indigo-900/50 text-indigo-200 px-2 py-0.5 rounded-full">0件</span>
                </h3>
                <div id="activeCandidatesViewer" class="max-h-36 overflow-y-auto flex flex-wrap gap-1.5 p-2 bg-slate-950/40 rounded-xl text-xs text-indigo-200">
                    <span class="text-slate-500 italic">候補を入力するとここに表示されます</span>
                </div>
            </div>

            <!-- Right: Winner Logs -->
            <div class="md:col-span-7 bg-indigo-950/20 border border-indigo-900/40 rounded-2xl p-5 backdrop-blur-sm">
                <div class="flex justify-between items-center mb-2.5">
                    <h3 class="text-sm font-bold text-indigo-300 flex items-center space-x-1.5">
                        <span>👑</span>
                        <span>当選履歴</span>
                    </h3>
                    <button onclick="clearHistory()" class="text-xs text-indigo-400 hover:text-red-400 transition-colors">
                        履歴クリア
                    </button>
                </div>
                <!-- History Container -->
                <div id="historyContainer" class="max-h-36 overflow-y-auto space-y-1.5 p-1">
                    <div class="text-xs text-slate-500 italic py-4 text-center">まだ当選履歴はありません</div>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer class="text-center py-4 border-t border-indigo-950 bg-slate-950/80 text-xs text-indigo-400/80">
        <p>© 2026 ミラクルくじ引き App. All Rights Reserved. (Procedural Audio Synth Engine)</p>
    </footer>

    <!-- WINNER MODAL (Hidden by default) -->
    <div id="winnerModal" class="fixed inset-0 bg-slate-950/80 backdrop-blur-md z-50 flex items-center justify-center p-4 hidden opacity-0 transition-opacity duration-300">
        <div class="bg-gradient-to-tr from-indigo-900 via-indigo-950 to-slate-900 border-2 border-amber-400 rounded-3xl p-6 md:p-10 max-w-lg w-full text-center relative shadow-[0_0_50px_rgba(245,158,11,0.3)] transform scale-95 transition-transform duration-300" id="winnerModalBody">
            
            <!-- Close Button -->
            <button onclick="closeWinnerModal()" class="absolute top-4 right-4 text-indigo-300 hover:text-white transition bg-slate-950/40 w-8 h-8 rounded-full flex items-center justify-center">
                ✕
            </button>

            <!-- Fireworks / Confetti Canvas -->
            <canvas id="modalConfettiCanvas" class="absolute inset-0 pointer-events-none w-full h-full rounded-3xl z-0"></canvas>

            <!-- Inside Modal Content -->
            <div class="relative z-10">
                <div class="inline-block px-4 py-1.5 bg-amber-500 text-slate-950 font-black text-xs md:text-sm rounded-full uppercase tracking-widest animate-bounce mb-3">
                    Congratulations!
                </div>
                <h2 class="text-2xl md:text-3xl font-extrabold text-amber-300 neon-text mb-6">🎉 おめでとうございます！ 🎉</h2>
                
                <p class="text-xs text-indigo-300 mb-2">厳正なる抽選の結果、以下の通り当選しました！</p>
                
                <!-- Winner(s) Display Board -->
                <div id="winnerResultContainer" class="my-6 bg-slate-950/70 rounded-2xl p-6 border border-indigo-700/50 shadow-inner flex flex-col gap-2 max-h-48 overflow-y-auto">
                    <!-- Dynamic Winners insert -->
                </div>

                <!-- Modal Bottom buttons -->
                <div class="flex flex-col sm:flex-row gap-3 mt-6">
                    <button onclick="closeWinnerModal()" class="w-full sm:flex-1 py-3 px-6 rounded-xl bg-indigo-800 hover:bg-indigo-700 text-indigo-100 font-bold text-sm transition-all">
                        閉じる
                    </button>
                    <button id="drawAgainBtn" onclick="drawAgainFromModal()" class="w-full sm:flex-1 py-3 px-6 rounded-xl bg-gradient-to-r from-pink-500 to-amber-500 hover:from-pink-400 hover:to-amber-400 text-slate-950 font-black text-sm transition-all shadow-md">
                        もう一回引く！
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Script Logic -->
    <script>
        // Sound Engine Using Web Audio API (procedural generation)
        class SoundEngine {
            constructor() {
                this.ctx = null;
                this.enabled = true;
            }

            init() {
                if (!this.ctx) {
                    this.ctx = new (window.AudioContext || window.webkitAudioContext)();
                }
            }

            playTick() {
                if (!this.enabled) return;
                this.init();
                try {
                    let osc = this.ctx.createOscillator();
                    let gain = this.ctx.createGain();
                    osc.connect(gain);
                    gain.connect(this.ctx.destination);

                    osc.frequency.setValueAtTime(800, this.ctx.currentTime);
                    osc.frequency.exponentialRampToValueAtTime(100, this.ctx.currentTime + 0.05);

                    gain.gain.setValueAtTime(0.08, this.ctx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + 0.05);

                    osc.start();
                    osc.stop(this.ctx.currentTime + 0.05);
                } catch (e) { console.warn(e); }
            }

            playDrumroll() {
                if (!this.enabled) return null;
                this.init();
                try {
                    // Quick low pulse simulation
                    let osc = this.ctx.createOscillator();
                    let gain = this.ctx.createGain();
                    osc.type = 'sawtooth';
                    osc.frequency.value = 65;
                    
                    // Filter to make it sound like low drum
                    let filter = this.ctx.createBiquadFilter();
                    filter.type = 'lowpass';
                    filter.frequency.value = 150;

                    osc.connect(filter);
                    filter.connect(gain);
                    gain.connect(this.ctx.destination);

                    gain.gain.setValueAtTime(0.15, this.ctx.currentTime);
                    osc.start();

                    return { osc, gain };
                } catch (e) { console.warn(e); return null; }
            }

            playCymbal() {
                if (!this.enabled) return;
                this.init();
                try {
                    // Simple synthesized metallic noise
                    let osc = this.ctx.createOscillator();
                    let gain = this.ctx.createGain();
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(1200, this.ctx.currentTime);
                    
                    let highpass = this.ctx.createBiquadFilter();
                    highpass.type = 'highpass';
                    highpass.frequency.value = 1000;

                    osc.connect(highpass);
                    highpass.connect(gain);
                    gain.connect(this.ctx.destination);

                    gain.gain.setValueAtTime(0.15, this.ctx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + 0.6);

                    osc.start();
                    osc.stop(this.ctx.currentTime + 0.6);
                } catch (e) { console.warn(e); }
            }

            playFanfare() {
                if (!this.enabled) return;
                this.init();
                try {
                    const notes = [261.63, 329.63, 392.00, 523.25]; // C4, E4, G4, C5
                    const now = this.ctx.currentTime;
                    
                    notes.forEach((freq, idx) => {
                        let osc = this.ctx.createOscillator();
                        let gain = this.ctx.createGain();
                        osc.type = 'sine';
                        osc.frequency.value = freq;
                        
                        osc.connect(gain);
                        gain.connect(this.ctx.destination);

                        let start = now + idx * 0.12;
                        gain.gain.setValueAtTime(0, now);
                        gain.gain.setValueAtTime(0.1, start);
                        gain.gain.exponentialRampToValueAtTime(0.001, start + 0.5);

                        osc.start(start);
                        osc.stop(start + 0.5);
                    });
                } catch (e) { console.warn(e); }
            }

            playBoxTap() {
                if (!this.enabled) return;
                this.init();
                try {
                    let osc = this.ctx.createOscillator();
                    let gain = this.ctx.createGain();
                    osc.frequency.setValueAtTime(300, this.ctx.currentTime);
                    osc.frequency.exponentialRampToValueAtTime(80, this.ctx.currentTime + 0.15);
                    
                    gain.connect(this.ctx.destination);
                    osc.connect(gain);
                    gain.gain.setValueAtTime(0.12, this.ctx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + 0.15);
                    osc.start();
                    osc.stop(this.ctx.currentTime + 0.15);
                } catch (e) { console.warn(e); }
            }
        }

        const sound = new SoundEngine();

        // App State
        const state = {
            candidates: [],
            winners: [],
            history: [],
            drawCount: 1,
            mode: 'wheel', // 'wheel' | 'box' | 'slot'
            isDrawing: false,
            wheelAngle: 0,
            wheelSpeed: 0,
            chosenBoxIdx: -1,
            presets: {
                yesno: ["⭕️ Yes / はい", "❌ No / いいえ"],
                numbers: ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10"],
                lunch: ["🍔 ハンバーガー", "🍜 ラーメン", "🍛 カレー", "🍣 お寿司", "🍝 パスタ", "🥗 サラダプレート", "🍱 定食"],
                penalty: ["💀 モノマネをする", "🥤 センブリ茶を飲む", "🎤 1曲歌う", "🤫 秘密を1つ暴露", "📱 最後に届いたLINEを公開"]
            }
        };

        // Cache DOM
        const candidatesInput = document.getElementById('candidatesInput');
        const candidateCount = document.getElementById('candidateCount');
        const activeCountLabel = document.getElementById('activeCountLabel');
        const activeCandidatesViewer = document.getElementById('activeCandidatesViewer');
        const removeOnDraw = document.getElementById('removeOnDraw');
        const drawCountVal = document.getElementById('drawCountVal');
        const drawButton = document.getElementById('drawButton');
        const btnText = document.getElementById('btnText');
        const btnIcon = document.getElementById('btnIcon');
        const soundToggle = document.getElementById('soundToggle');
        const soundStatus = document.getElementById('soundStatus');
        const soundIcon = document.getElementById('soundIcon');
        const historyContainer = document.getElementById('historyContainer');
        const fastDrawCheck = document.getElementById('fastDraw');

        // Stage Views
        const viewWheel = document.getElementById('viewWheel');
        const viewBox = document.getElementById('viewBox');
        const viewSlot = document.getElementById('viewSlot');
        const activeModeLabel = document.getElementById('activeModeLabel');
        const modeTip = document.getElementById('modeTip');

        // Modal Views
        const winnerModal = document.getElementById('winnerModal');
        const winnerModalBody = document.getElementById('winnerModalBody');
        const winnerResultContainer = document.getElementById('winnerResultContainer');

        // Canvas setups
        const wheelCanvas = document.getElementById('wheelCanvas');
        const wheelCtx = wheelCanvas.getContext('2d');

        // Colorful Theme Palette
        const palette = [
            '#8b5cf6', '#ec4899', '#3b82f6', '#10b981', 
            '#f59e0b', '#ef4444', '#06b6d4', '#a855f7'
        ];

        window.onload = function() {
            // Setup Textarea listener
            candidatesInput.addEventListener('input', () => {
                parseCandidates();
                drawWheel();
            });

            // Initial load
            loadPreset('lunch');
            initSoundToggle();
            initConfetti();
            
            // Render initial state
            drawWheel();

            // Resize listeners to update UI if necessary
            window.addEventListener('resize', () => {
                drawWheel();
            });
        };

        // Parse Textarea into Candidates
        function parseCandidates() {
            const val = candidatesInput.value;
            state.candidates = val.split('\n')
                .map(item => item.trim())
                .filter(item => item.length > 0);

            // Update Counts
            candidateCount.innerText = `${state.candidates.length}項目登録`;
            activeCountLabel.innerText = `${state.candidates.length}件`;

            // Update Viewer Chips
            if (state.candidates.length === 0) {
                activeCandidatesViewer.innerHTML = `<span class="text-slate-500 italic">候補を入力するとここに表示されます</span>`;
            } else {
                activeCandidatesViewer.innerHTML = state.candidates.map(c => 
                    `<span class="bg-indigo-950/80 border border-indigo-800 text-indigo-200 px-2 py-1 rounded-md max-w-[120px] truncate" title="${c}">${c}</span>`
                ).join('');
            }

            // Update boxes view if active mode is Box
            if (state.mode === 'box') {
                renderLuckyBoxes();
            }
        }

        // Preset Loader
        function loadPreset(key) {
            if (state.presets[key]) {
                candidatesInput.value = state.presets[key].join('\n');
                parseCandidates();
                drawWheel();
                
                // Extra feedback sound
                sound.playBoxTap();
            }
        }

        // Adjust Draw Count
        function adjustDrawCount(amount) {
            let nextVal = state.drawCount + amount;
            if (nextVal < 1) nextVal = 1;
            if (nextVal > 10) nextVal = 10; // limit 10 draws at once
            state.drawCount = nextVal;
            drawCountVal.innerText = state.drawCount;
            sound.playBoxTap();
        }

        // Toggle Sound Settings
        function initSoundToggle() {
            soundToggle.addEventListener('click', () => {
                sound.enabled = !sound.enabled;
                if (sound.enabled) {
                    soundStatus.innerText = "サウンド: ON";
                    soundIcon.innerText = "🔊";
                    sound.playBoxTap();
                } else {
                    soundStatus.innerText = "サウンド: OFF";
                    soundIcon.innerText = "🔇";
                }
            });
        }

        function setDrawMode(modeName) {
            if (state.isDrawing) return;
            state.mode = modeName;

            // Update CSS for Mode Tabs
            const modes = {
                wheel: { btn: 'btnModeWheel', view: viewWheel, title: 'ルーレット・抽選ステージ', tip: '矢印が止まった場所が当選！' },
                box: { btn: 'btnModeBox', view: viewBox, title: '千両宝箱・抽選ステージ', tip: 'お好きな宝箱を1つ開けよう！' },
                slot: { btn: 'btnModeSlot', view: viewSlot, title: '高速スロット・抽選ステージ', tip: '超高速シャッフルスタート！' }
            };

            Object.keys(modes).forEach(m => {
                const config = modes[m];
                const btn = document.getElementById(config.btn);
                if (m === modeName) {
                    btn.className = "flex flex-col items-center p-3 rounded-xl bg-gradient-to-b from-purple-600/30 to-purple-800/10 border-2 border-purple-500/80 text-purple-200 transition-all shadow-md";
                    config.view.classList.remove('hidden');
                    activeModeLabel.innerText = config.title;
                    modeTip.innerText = config.tip;
                } else {
                    btn.className = "flex flex-col items-center p-3 rounded-xl bg-indigo-950/30 border border-indigo-800/50 hover:border-indigo-700 text-indigo-300 hover:text-indigo-100 transition-all";
                    config.view.classList.add('hidden');
                }
            });

            sound.playBoxTap();

            if (modeName === 'wheel') {
                drawWheel();
            } else if (modeName === 'box') {
                renderLuckyBoxes();
            } else if (modeName === 'slot') {
                document.getElementById('slotScroller').innerText = "READY!";
            }
        }

        function drawWheel() {
            if (state.mode !== 'wheel') return;
            const size = wheelCanvas.width;
            const center = size / 2;
            const radius = center - 12;

            wheelCtx.clearRect(0, 0, size, size);

            const items = state.candidates;
            if (items.length === 0) {
                // Draw empty state
                wheelCtx.beginPath();
                wheelCtx.arc(center, center, radius, 0, 2 * Math.PI);
                wheelCtx.fillStyle = '#1e1b4b';
                wheelCtx.fill();
                wheelCtx.strokeStyle = '#4338ca';
                wheelCtx.lineWidth = 4;
                wheelCtx.stroke();

                wheelCtx.fillStyle = '#6366f1';
                wheelCtx.font = 'bold 16px "Zen Maru Gothic"';
                wheelCtx.textAlign = 'center';
                wheelCtx.fillText('候補を入力してください', center, center);
                return;
            }

            const sliceAngle = (2 * Math.PI) / items.length;

            items.forEach((item, index) => {
                const angle = state.wheelAngle + index * sliceAngle;

                // Draw Slice
                wheelCtx.beginPath();
                wheelCtx.moveTo(center, center);
                wheelCtx.arc(center, center, radius, angle, angle + sliceAngle);
                wheelCtx.closePath();

                wheelCtx.fillStyle = palette[index % palette.length];
                wheelCtx.fill();

                // Darken border slice
                wheelCtx.strokeStyle = 'rgba(15, 11, 37, 0.4)';
                wheelCtx.lineWidth = 1.5;
                wheelCtx.stroke();

                // Draw Text
                wheelCtx.save();
                wheelCtx.translate(center, center);
                wheelCtx.rotate(angle + sliceAngle / 2);

                wheelCtx.fillStyle = '#ffffff';
                wheelCtx.font = 'bold 15px "Zen Maru Gothic"';
                wheelCtx.textAlign = 'right';

                // Truncate text if too long for wheel
                let text = item;
                if (text.length > 8) {
                    text = text.substring(0, 7) + '...';
                }
                
                wheelCtx.fillText(text, radius - 20, 5);
                wheelCtx.restore();
            });

            // Outer ring
            wheelCtx.beginPath();
            wheelCtx.arc(center, center, radius, 0, 2 * Math.PI);
            wheelCtx.strokeStyle = '#6366f1';
            wheelCtx.lineWidth = 4;
            wheelCtx.stroke();

            // Center Pin
            wheelCtx.beginPath();
            wheelCtx.arc(center, center, 14, 0, 2 * Math.PI);
            wheelCtx.fillStyle = '#ffffff';
            wheelCtx.fill();
            wheelCtx.strokeStyle = '#4338ca';
            wheelCtx.lineWidth = 3;
            wheelCtx.stroke();
        }

        function renderLuckyBoxes() {
            const container = document.getElementById('boxesContainer');
            container.innerHTML = '';

            const boxCount = Math.max(3, Math.min(6, state.candidates.length));
            if (state.candidates.length === 0) {
                container.innerHTML = `<div class="col-span-2 sm:col-span-3 text-center text-slate-500 py-6">候補を入力すると宝箱が出現します</div>`;
                return;
            }

            for (let i = 0; i < boxCount; i++) {
                const boxDiv = document.createElement('div');
                boxDiv.className = "flex flex-col items-center cursor-pointer group";
                boxDiv.setAttribute('onclick', `triggerBoxDraw(${i})`);

                const boxColors = [
                    'from-amber-400 to-yellow-600',
                    'from-purple-500 to-indigo-700',
                    'from-emerald-400 to-teal-600',
                    'from-pink-500 to-rose-700',
                    'from-cyan-400 to-blue-600',
                    'from-orange-400 to-red-600'
                ];
                const activeColor = boxColors[i % boxColors.length];

                boxDiv.innerHTML = `
                    <div id="boxWrapper_${i}" class="relative w-20 h-20 transition-transform duration-200 transform group-hover:scale-110 active:scale-95">
                        <!-- Box Lid -->
                        <div id="boxLid_${i}" class="absolute top-0 left-0 right-0 h-6 bg-gradient-to-r ${activeColor} rounded-t-lg border-b-2 border-indigo-950 z-10 transition-transform duration-200 shadow-md"></div>
                        <!-- Box Base -->
                        <div class="absolute bottom-0 left-0 right-0 h-14 bg-gradient-to-r ${activeColor} rounded-b-lg flex items-center justify-center border-t border-yellow-300/30 shadow-lg">
                            <span class="text-white text-lg font-black font-sans">🎁</span>
                        </div>
                    </div>
                    <span class="text-[10px] text-indigo-300 mt-2 font-bold bg-indigo-950/80 px-2 py-0.5 rounded-full border border-indigo-800">箱 #${i+1}</span>
                `;
                container.appendChild(boxDiv);
            }
        }

        function triggerDraw() {
            if (state.isDrawing) return;
            if (state.candidates.length === 0) {
                showToast("抽選する候補を入力してください！", "warning");
                return;
            }

            // Lock Draw count to physical candidates
            if (state.candidates.length < state.drawCount) {
                showToast(`候補数が足りません（現在${state.candidates.length}個、引きたい数${state.drawCount}個）`, "warning");
                return;
            }

            // Audio context start trigger (safari/chrome security)
            sound.init();

            // Set state drawing
            state.isDrawing = true;
            disableUI(true);

            // Fast mode skip
            if (fastDrawCheck.checked) {
                executeImmediateDraw();
                return;
            }

            if (state.mode === 'wheel') {
                startWheelSpin();
            } else if (state.mode === 'box') {
                // In box mode, user has to click a box. We trigger a random box on behalf of "Start Button"
                const randomBoxIdx = Math.floor(Math.random() * Math.max(3, Math.min(6, state.candidates.length)));
                triggerBoxDraw(randomBoxIdx);
            } else if (state.mode === 'slot') {
                startSlotShuffle();
            }
        }

        // Fast Immediate Draw
        function executeImmediateDraw() {
            const chosen = getRandomWinners(state.drawCount);
            finalizeDraw(chosen);
        }

        // Helper: pick randomly
        function getRandomWinners(count) {
            let pool = [...state.candidates];
            let winners = [];
            for (let i = 0; i < count; i++) {
                if (pool.length === 0) break;
                const randIdx = Math.floor(Math.random() * pool.length);
                winners.push(pool[randIdx]);
                pool.splice(randIdx, 1);
            }
            return winners;
        }

        function startWheelSpin() {
            // Spin Physics Configuration
            state.wheelSpeed = 0.4 + Math.random() * 0.3; // Initial angular speed
            const friction = 0.982; // Constant drag deceleration
            let lastTickAngle = 0;
            const sliceAngle = (2 * Math.PI) / state.candidates.length;

            const drumSynth = sound.playDrumroll();

            function animateSpin() {
                state.wheelAngle += state.wheelSpeed;
                state.wheelSpeed *= friction;

                // Ticking sound when wheel moves past a partition
                const totalSlicesMoved = Math.floor(state.wheelAngle / sliceAngle);
                if (totalSlicesMoved !== lastTickAngle) {
                    sound.playTick();
                    lastTickAngle = totalSlicesMoved;
                }

                drawWheel();

                if (state.wheelSpeed > 0.001) {
                    requestAnimationFrame(animateSpin);
                } else {
                    // Stopped! Stop drumroll synth
                    if (drumSynth) {
                        try {
                            drumSynth.gain.gain.exponentialRampToValueAtTime(0.001, sound.ctx.currentTime + 0.1);
                            drumSynth.osc.stop(sound.ctx.currentTime + 0.15);
                        } catch (e){}
                    }

                    // Find Winner based on wheel pin (pointing straight top: -Math.PI / 2)
                    // Angle of pin in canvas space: -90 degrees
                    const pinAngle = -Math.PI / 2;
                    let normalizedAngle = (pinAngle - state.wheelAngle) % (2 * Math.PI);
                    if (normalizedAngle < 0) normalizedAngle += 2 * Math.PI;

                    const winnerIndex = Math.floor(normalizedAngle / sliceAngle) % state.candidates.length;
                    
                    // We must ensure the principal winner is at the computed index, 
                    // and other winners are randomly picked if drawing multiple.
                    const primaryWinner = state.candidates[winnerIndex];
                    let winnersList = [primaryWinner];

                    if (state.drawCount > 1) {
                        // Pick others
                        const remainingPool = state.candidates.filter(c => c !== primaryWinner);
                        const rest = getRandomWinnersFromPool(remainingPool, state.drawCount - 1);
                        winnersList = winnersList.concat(rest);
                    }

                    finalizeDraw(winnersList);
                }
            }

            animateSpin();
        }

        // Pick winners from custom pool
        function getRandomWinnersFromPool(pool, count) {
            let arr = [...pool];
            let out = [];
            for (let i = 0; i < count; i++) {
                if (arr.length === 0) break;
                const idx = Math.floor(Math.random() * arr.length);
                out.push(arr[idx]);
                arr.splice(idx, 1);
            }
            return out;
        }

        function triggerBoxDraw(boxIdx) {
            if (!state.isDrawing && state.mode !== 'box') return;
            
            // If they just clicked a box on stand-by, trigger draw
            if (!state.isDrawing) {
                if (state.candidates.length === 0) {
                    showToast("候補がありません", "warning");
                    return;
                }
                state.isDrawing = true;
                disableUI(true);
            }

            sound.playBoxTap();

            const wrapper = document.getElementById(`boxWrapper_${boxIdx}`);
            const lid = document.getElementById(`boxLid_${boxIdx}`);
            
            if (!wrapper || !lid) {
                // Fallback if elements not fully rendered
                executeImmediateDraw();
                return;
            }

            // Drum roll sound
            const drumSynth = sound.playDrumroll();

            // 1. Shake Box
            wrapper.classList.add('animate-shake');

            setTimeout(() => {
                // Stop Drum roll
                if (drumSynth) {
                    try {
                        drumSynth.gain.gain.exponentialRampToValueAtTime(0.001, sound.ctx.currentTime + 0.05);
                        drumSynth.osc.stop(sound.ctx.currentTime + 0.1);
                    } catch(e){}
                }

                // 2. Open Lid
                wrapper.classList.remove('animate-shake');
                lid.classList.add('open-lid');

                // 3. Complete
                setTimeout(() => {
                    const winnersList = getRandomWinners(state.drawCount);
                    finalizeDraw(winnersList);
                }, 500);

            }, 1200);
        }

        function startSlotShuffle() {
            const scroller = document.getElementById('slotScroller');
            let duration = 2000; // ms
            let startTime = null;
            let speed = 60; // initial tick speed ms

            const drumSynth = sound.playDrumroll();

            function updateSlot(timestamp) {
                if (!startTime) startTime = timestamp;
                let elapsed = timestamp - startTime;

                // Pick a rapid name
                const randomName = state.candidates[Math.floor(Math.random() * state.candidates.length)];
                scroller.innerText = randomName;
                scroller.style.transform = `translateY(${Math.sin(timestamp) * 5}px)`; // light jitter
                sound.playTick();

                if (elapsed < duration) {
                    // Slow down progressively
                    let progress = elapsed / duration;
                    speed = 60 + (progress * 300); // gets slower
                    setTimeout(() => requestAnimationFrame(updateSlot), speed);
                } else {
                    // Finish
                    if (drumSynth) {
                        try {
                            drumSynth.gain.gain.exponentialRampToValueAtTime(0.001, sound.ctx.currentTime + 0.05);
                            drumSynth.osc.stop(sound.ctx.currentTime + 0.1);
                        } catch(e){}
                    }

                    const winnersList = getRandomWinners(state.drawCount);
                    finalizeDraw(winnersList);
                }
            }

            requestAnimationFrame(updateSlot);
        }

        function finalizeDraw(winnersList) {
            state.winners = winnersList;

            // SFX & Confetti
            sound.playCymbal();
            sound.playFanfare();

            // Store History with date timestamp
            const timeStr = new Date().toLocaleTimeString('ja-JP', { hour: '2-digit', minute: '2-digit', second: '2-digit' });
            state.winners.forEach(w => {
                state.history.unshift({ name: w, time: timeStr });
            });
            updateHistoryLog();

            // Remove from input if "removeOnDraw" option is checked
            if (removeOnDraw.checked) {
                const remaining = state.candidates.filter(c => !winnersList.includes(c));
                candidatesInput.value = remaining.join('\n');
                parseCandidates();
            }

            // Show Winner Modal
            openWinnerModal();

            // Re-enable interactive controls
            state.isDrawing = false;
            disableUI(false);

            // Re-render structures
            if (state.mode === 'wheel') {
                drawWheel();
            } else if (state.mode === 'box') {
                renderLuckyBoxes();
            } else if (state.mode === 'slot') {
                document.getElementById('slotScroller').innerText = winnersList.join(', ');
            }
        }

        // Confetti Canvas Particle Logic
        let confettiInterval = null;
        let confettiCtx = null;
        let confettiParticles = [];
        
        function initConfetti() {
            const canvas = document.getElementById('modalConfettiCanvas');
            confettiCtx = canvas.getContext('2d');
            
            // Scale dynamically
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight;
        }

        function triggerConfettiBurst() {
            const canvas = document.getElementById('modalConfettiCanvas');
            canvas.width = canvas.offsetParent ? canvas.offsetParent.clientWidth : 480;
            canvas.height = canvas.offsetParent ? canvas.offsetParent.clientHeight : 400;

            confettiParticles = [];
            const colors = ['#f59e0b', '#ec4899', '#3b82f6', '#10b981', '#a855f7', '#f43f5e'];

            // Generate 120 particles from various positions
            for (let i = 0; i < 120; i++) {
                confettiParticles.push({
                    x: Math.random() * canvas.width,
                    y: canvas.height + 10,
                    vx: (Math.random() - 0.5) * 12,
                    vy: -Math.random() * 15 - 5,
                    size: Math.random() * 6 + 4,
                    color: colors[Math.floor(Math.random() * colors.length)],
                    rotation: Math.random() * 360,
                    rotSpeed: (Math.random() - 0.5) * 10
                });
            }

            if (confettiInterval) clearInterval(confettiInterval);
            confettiInterval = setInterval(drawConfettiParticles, 16);
        }

        function drawConfettiParticles() {
            const canvas = document.getElementById('modalConfettiCanvas');
            confettiCtx.clearRect(0, 0, canvas.width, canvas.height);

            let active = false;

            confettiParticles.forEach(p => {
                p.x += p.vx;
                p.y += p.vy;
                p.vy += 0.3; // gravity
                p.vx *= 0.99; // drag
                p.rotation += p.rotSpeed;

                if (p.y < canvas.height + 20) {
                    active = true;
                }

                confettiCtx.save();
                confettiCtx.translate(p.x, p.y);
                confettiCtx.rotate((p.rotation * Math.PI) / 180);
                confettiCtx.fillStyle = p.color;
                // Draw small rectangular foil
                confettiCtx.fillRect(-p.size / 2, -p.size, p.size, p.size * 1.5);
                confettiCtx.restore();
            });

            if (!active) {
                clearInterval(confettiInterval);
                confettiCtx.clearRect(0, 0, canvas.width, canvas.height);
            }
        }

        function updateHistoryLog() {
            if (state.history.length === 0) {
                historyContainer.innerHTML = `<div class="text-xs text-slate-500 italic py-4 text-center">まだ当選履歴はありません</div>`;
                return;
            }

            historyContainer.innerHTML = state.history.map((h, i) => `
                <div class="flex items-center justify-between p-2 bg-indigo-950/40 rounded-xl border border-indigo-900/40 text-xs text-slate-200 hover:bg-indigo-900/30 transition">
                    <div class="flex items-center space-x-2">
                        <span class="text-amber-400 font-bold">#${state.history.length - i}</span>
                        <span class="font-bold text-slate-100">${escapeHtml(h.name)}</span>
                    </div>
                    <span class="text-slate-500 text-[10px] font-mono">${h.time}</span>
                </div>
            `).join('');
        }

        function clearHistory() {
            state.history = [];
            updateHistoryLog();
            sound.playBoxTap();
        }

        function openWinnerModal() {
            // Render Winner Cards in Grid
            const gridCols = state.winners.length > 4 ? 'grid-cols-2' : 'grid-cols-1';
            winnerResultContainer.className = `my-6 bg-slate-950/70 rounded-2xl p-6 border border-indigo-700/50 shadow-inner grid ${gridCols} gap-3 max-h-56 overflow-y-auto`;
            
            winnerResultContainer.innerHTML = state.winners.map((w, idx) => `
                <div class="p-4 bg-gradient-to-tr from-purple-950/60 to-indigo-950/80 rounded-xl border border-pink-500/30 flex items-center justify-center space-x-3 shadow-lg transform hover:scale-102 transition">
                    <span class="text-2xl">${idx === 0 ? '🏆' : '⭐'}</span>
                    <span class="text-lg md:text-xl font-extrabold text-white truncate max-w-[180px]" title="${w}">${escapeHtml(w)}</span>
                </div>
            `).join('');

            // Display Transition
            winnerModal.classList.remove('hidden');
            setTimeout(() => {
                winnerModal.classList.remove('opacity-0');
                winnerModalBody.classList.remove('scale-95');
                winnerModalBody.classList.add('scale-100');
                // Trigger burst confetti inside modal
                triggerConfettiBurst();
            }, 50);
        }

        function closeWinnerModal() {
            winnerModal.classList.add('opacity-0');
            winnerModalBody.classList.remove('scale-100');
            winnerModalBody.classList.add('scale-95');
            setTimeout(() => {
                winnerModal.classList.add('hidden');
                if (confettiInterval) clearInterval(confettiInterval);
            }, 300);
            sound.playBoxTap();
        }

        function drawAgainFromModal() {
            closeWinnerModal();
            setTimeout(() => {
                triggerDraw();
            }, 400);
        }

        function disableUI(disable) {
            drawButton.disabled = disable;
            candidatesInput.disabled = disable;
            document.querySelectorAll('button[onclick^="loadPreset"]').forEach(b => b.disabled = disable);
            document.querySelectorAll('button[onclick^="adjustDrawCount"]').forEach(b => b.disabled = disable);
            document.querySelectorAll('button[onclick^="setDrawMode"]').forEach(b => b.disabled = disable);
            
            if (disable) {
                drawButton.classList.add('opacity-60', 'cursor-not-allowed');
                btnText.innerText = "抽選中...";
                btnIcon.innerText = "⏳";
            } else {
                drawButton.classList.remove('opacity-60', 'cursor-not-allowed');
                btnText.innerText = "抽選スタート！";
                btnIcon.innerText = "🎉";
            }
        }

        // Toast Alerts system (No Native alerts allowed)
        function showToast(message, type = "info") {
            const toast = document.createElement('div');
            toast.className = `fixed bottom-5 left-1/2 transform -translate-x-1/2 px-4 py-3 rounded-xl shadow-2xl border flex items-center space-x-2 text-sm z-50 transition-all duration-300 transform translate-y-10 opacity-0`;
            
            if (type === "warning") {
                toast.className += " bg-red-950/90 border-red-800 text-red-200 shadow-red-900/30";
                toast.innerHTML = `<span>⚠️</span> <span>${message}</span>`;
            } else {
                toast.className += " bg-indigo-950/90 border-indigo-800 text-indigo-200 shadow-indigo-900/30";
                toast.innerHTML = `<span>💡</span> <span>${message}</span>`;
            }

            document.body.appendChild(toast);
            
            // animate in
            setTimeout(() => {
                toast.classList.remove('translate-y-10', 'opacity-0');
            }, 10);

            // destroy
            setTimeout(() => {
                toast.classList.add('translate-y-10', 'opacity-0');
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }

        // Full app reset button
        document.getElementById('resetApp').addEventListener('click', () => {
            if (confirm("入力したデータや当選履歴をすべてクリアしますか？")) {
                candidatesInput.value = '';
                state.candidates = [];
                state.history = [];
                state.winners = [];
                state.drawCount = 1;
                drawCountVal.innerText = "1";
                removeOnDraw.checked = true;
                fastDrawCheck.checked = false;
                parseCandidates();
                clearHistory();
                setDrawMode('wheel');
                showToast("アプリの状態を初期化しました");
            }
        });

        // Escaped HTML to prevent injections
        function escapeHtml(str) {
            return str
                .replace(/&/g, "&amp;")
                .replace(/</g, "&lt;")
                .replace(/>/g, "&gt;")
                .replace(/"/g, "&quot;")
                .replace(/'/g, "&#039;");
        }
    </script>
</body>
</html>
