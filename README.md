<!DOCTYPE html>
<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    
    <!-- iOS Native App / PWA Meta Tags -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Nova AI Studio">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="theme-color" content="#000000">
    <link rel="apple-touch-icon" href="https://placehold.co/180x180/0A84FF/ffffff?text=Nova+AI">

    <title>Nova AI Video Editor - Mobile & PWA Edition</title>
    
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        ios: {
                            bg: '#000000',
                            card: 'rgba(28, 28, 30, 0.85)',
                            glass: 'rgba(44, 44, 46, 0.70)',
                            accent: '#0A84FF',
                            purple: '#BF5AF2',
                            pink: '#FF375F',
                            green: '#30D158',
                            yellow: '#FFD60A',
                            orange: '#FF9F0A'
                        }
                    },
                    fontFamily: {
                        sans: ['-apple-system', 'BlinkMacSystemFont', '"SF Pro Display"', '"SF Pro Text"', 'Inter', 'sans-serif']
                    }
                }
            }
        }
    </script>

    <!-- Inline Dynamic Web Manifest Generator for PWA Installation -->
    <script>
        (function() {
            const manifest = {
                name: "Nova AI Studio Video Editor",
                short_name: "Nova AI",
                start_url: "./",
                display: "standalone",
                background_color: "#000000",
                theme_color: "#000000",
                icons: [
                    { src: "https://placehold.co/192x192/0A84FF/ffffff?text=Nova+AI", sizes: "192x192", type: "image/png" },
                    { src: "https://placehold.co/512x512/BF5AF2/ffffff?text=Nova+AI", sizes: "512x512", type: "image/png" }
                ]
            };
            const stringManifest = JSON.stringify(manifest);
            const blob = new Blob([stringManifest], {type: 'application/json'});
            const manifestURL = URL.createObjectURL(blob);
            const link = document.createElement('link');
            link.rel = 'manifest';
            link.href = manifestURL;
            document.head.appendChild(link);
        })();
    </script>

    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        /* Glassmorphism Styles */
        .glass-panel {
            background: rgba(28, 28, 30, 0.80);
            backdrop-filter: blur(25px);
            -webkit-backdrop-filter: blur(25px);
            border: 1px solid rgba(255, 255, 255, 0.12);
        }
        .glass-pill {
            background: rgba(44, 44, 46, 0.65);
            backdrop-filter: blur(14px);
            -webkit-backdrop-filter: blur(14px);
            border: 1px solid rgba(255, 255, 255, 0.08);
        }
        /* Touch safety & safe areas for iPhone notch */
        .safe-pb {
            padding-bottom: max(1rem, env(safe-area-inset-bottom));
        }
        .safe-pt {
            padding-top: max(0.5rem, env(safe-area-inset-top));
        }
        /* Custom scrollbars */
        ::-webkit-scrollbar { width: 4px; height: 4px; }
        ::-webkit-scrollbar-track { background: rgba(0, 0, 0, 0.2); }
        ::-webkit-scrollbar-thumb { background: rgba(255, 255, 255, 0.25); border-radius: 9999px; }
    </style>
</head>
<body class="bg-black text-white font-sans antialiased select-none overflow-x-hidden min-h-screen flex justify-center items-center">

    <!-- Container Frame -->
    <div id="app-frame" class="w-full max-w-md h-[100vh] sm:h-[880px] sm:max-h-[96vh] sm:border-[8px] sm:border-[#1c1c1e] sm:rounded-[48px] overflow-hidden flex flex-col relative shadow-2xl bg-black">
        
        <!-- PWA Install Alert Banner (Appears if running in browser) -->
        <div id="pwa-install-banner" class="bg-gradient-to-r from-ios-accent to-ios-purple text-white px-4 py-2 flex items-center justify-between text-xs font-medium z-50">
            <div class="flex items-center space-x-2">
                <i class="fa-solid fa-mobile-screen-button animate-bounce"></i>
                <span>Install app for full screen mode</span>
            </div>
            <button onclick="openInstallModal()" class="px-2.5 py-1 bg-white text-black font-bold text-[10px] rounded-full shadow hover:bg-slate-200 transition">
                Add to Home Screen
            </button>
        </div>

        <!-- Top Header Navigation -->
        <header class="safe-pt px-4 py-2.5 flex items-center justify-between z-40 bg-black/70 backdrop-blur-md border-b border-white/10 shrink-0">
            <div class="flex items-center space-x-2">
                <div class="w-8 h-8 rounded-xl bg-gradient-to-tr from-ios-accent via-ios-purple to-ios-pink flex items-center justify-center font-bold text-sm shadow-md">
                    <i class="fa-solid fa-wand-magic-sparkles text-white"></i>
                </div>
                <div>
                    <h1 class="text-sm font-bold tracking-tight bg-gradient-to-r from-white via-slate-200 to-slate-400 bg-clip-text text-transparent">Nova AI Mobile</h1>
                    <p class="text-[9px] text-ios-purple font-semibold tracking-wider uppercase">PWA Web Edition</p>
                </div>
            </div>
            
            <div class="flex items-center space-x-2">
                <!-- Help / How to Use Button -->
                <button onclick="openGuideModal()" class="w-8 h-8 rounded-full glass-pill flex items-center justify-center text-xs text-white/80 hover:text-white hover:bg-white/20 transition" title="How to Use">
                    <i class="fa-solid fa-circle-question"></i>
                </button>
                <!-- Export Button -->
                <button id="export-btn" class="px-3 py-1.5 rounded-full bg-gradient-to-r from-ios-accent to-blue-600 text-white font-semibold text-xs shadow-lg hover:brightness-110 active:scale-95 transition flex items-center space-x-1.5">
                    <i class="fa-solid fa-arrow-up-from-bracket"></i>
                    <span>Export</span>
                </button>
            </div>
        </header>

        <!-- Main Workspace -->
        <main class="flex-1 overflow-y-auto px-4 py-3 space-y-3.5 pb-20">

            <!-- Dynamic Aspect Ratio Preview Box -->
            <div class="relative w-full flex flex-col items-center justify-center bg-zinc-950 rounded-2xl overflow-hidden shadow-2xl border border-white/10 group">
                <!-- Canvas Element -->
                <div id="canvas-wrapper" class="relative w-full aspect-[9/16] max-h-[300px] flex items-center justify-center bg-black">
                    <canvas id="preview-canvas" class="w-full h-full object-contain cursor-pointer"></canvas>
                    
                    <!-- Caption Overlay Badge -->
                    <div id="caption-badge" class="absolute top-2 right-2 px-2 py-0.5 rounded-full bg-ios-purple/80 backdrop-blur-md text-[9px] text-white font-semibold">
                        <i class="fa-solid fa-closed-captioning mr-1"></i> <span id="caption-style-name">KINETIC POP</span>
                    </div>

                    <!-- Watermark / Format Badge -->
                    <div class="absolute top-2 left-2 px-2 py-0.5 rounded-full glass-pill text-[9px] text-white/80 font-medium">
                        <span id="aspect-ratio-label">9:16 Vertical Shorts</span>
                    </div>

                    <!-- Video Controls Overlay -->
                    <div id="video-overlay" class="absolute inset-0 bg-black/30 backdrop-blur-[1px] flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity">
                        <button id="play-pause-btn" class="w-12 h-12 rounded-full bg-white/20 backdrop-blur-md border border-white/40 text-white text-lg flex items-center justify-center hover:scale-110 active:scale-95 transition">
                            <i class="fa-solid fa-play ml-0.5" id="play-icon"></i>
                        </button>
                    </div>
                </div>

                <!-- Aspect Ratio Switcher Controls -->
                <div class="w-full bg-zinc-900/90 px-3 py-1.5 flex items-center justify-between border-t border-white/10 text-[10px]">
                    <span class="text-white/60 font-medium">Canvas Aspect Ratio:</span>
                    <div class="flex space-x-1.5">
                        <button onclick="changeAspectRatio('9/16')" id="ratio-916" class="px-2 py-0.5 rounded bg-ios-accent text-white font-bold">9:16</button>
                        <button onclick="changeAspectRatio('16/9')" id="ratio-169" class="px-2 py-0.5 rounded bg-white/10 text-white/70 hover:bg-white/20">16:9</button>
                        <button onclick="changeAspectRatio('1/1')" id="ratio-11" class="px-2 py-0.5 rounded bg-white/10 text-white/70 hover:bg-white/20">1:1</button>
                    </div>
                </div>
            </div>

            <!-- Media Upload Bar -->
            <div class="p-2.5 rounded-2xl glass-panel flex items-center space-x-2">
                <label class="flex-1 py-2 px-3 rounded-xl bg-ios-accent/20 border border-ios-accent/30 text-xs font-semibold text-ios-accent text-center cursor-pointer hover:bg-ios-accent/30 transition flex items-center justify-center space-x-2">
                    <i class="fa-solid fa-cloud-arrow-up"></i>
                    <span>Upload Video File</span>
                    <input type="file" id="video-file-input" accept="video/*" class="hidden">
                </label>
                <button id="load-sample-btn" class="py-2 px-3 rounded-xl bg-white/10 border border-white/10 text-xs text-white/80 font-medium hover:bg-white/15 transition">
                    Sample Track
                </button>
            </div>

            <!-- Visual Audio Waveform Scrubber -->
            <div class="p-3 rounded-2xl glass-panel space-y-2">
                <div class="flex justify-between items-center text-xs">
                    <span class="font-bold text-white flex items-center space-x-1.5">
                        <i class="fa-solid fa-wave-square text-ios-accent"></i>
                        <span>Smart Timeline Waveform</span>
                    </span>
                    <span id="silence-count-badge" class="text-[10px] bg-ios-green/20 text-ios-green border border-ios-green/30 px-2 py-0.5 rounded-full font-semibold">
                        3 Dead Zones Detected
                    </span>
                </div>
                
                <div class="relative w-full h-14 bg-black/70 rounded-xl overflow-hidden border border-white/10 flex items-center p-1 cursor-pointer" id="waveform-container">
                    <canvas id="waveform-canvas" class="w-full h-full"></canvas>
                    <div id="timeline-playhead" class="absolute top-0 bottom-0 w-0.5 bg-ios-accent shadow-[0_0_8px_#0A84FF] left-0 pointer-events-none"></div>
                </div>
                
                <div class="flex items-center justify-between text-[10px] text-white/50 font-mono">
                    <span id="time-current">00:00.0</span>
                    <span>Red = Auto Silence Cut</span>
                    <span id="time-duration">00:15.0</span>
                </div>
            </div>

            <!-- Tabbed Controls Navigation -->
            <div class="space-y-3">
                <div class="flex space-x-1 p-1 bg-white/5 rounded-xl text-xs font-medium border border-white/5">
                    <button id="tab-btn-silence" onclick="switchTab('silence')" class="flex-1 py-1.5 rounded-lg bg-white/20 text-white font-semibold transition text-center">Silence Cut</button>
                    <button id="tab-btn-audio" onclick="switchTab('audio')" class="flex-1 py-1.5 rounded-lg text-white/60 hover:text-white transition text-center">Audio Filters</button>
                    <button id="tab-btn-captions" onclick="switchTab('captions')" class="flex-1 py-1.5 rounded-lg text-white/60 hover:text-white transition text-center">Captions</button>
                    <button id="tab-btn-sfx" onclick="switchTab('sfx')" class="flex-1 py-1.5 rounded-lg text-white/60 hover:text-white transition text-center">SFX Board</button>
                </div>

                <!-- Tab 1: Silence Remover & Jump Cuts -->
                <div id="tab-silence" class="space-y-3">
                    <div class="p-3.5 rounded-2xl glass-panel space-y-3">
                        <div class="flex items-center justify-between">
                            <div class="flex items-center space-x-2">
                                <div class="w-7 h-7 rounded-lg bg-ios-purple/20 text-ios-purple flex items-center justify-center text-xs font-bold">
                                    <i class="fa-solid fa-scissors"></i>
                                </div>
                                <div>
                                    <div class="text-xs font-bold text-white">Auto Remove Silence & Pauses</div>
                                    <div class="text-[10px] text-white/50">Auto jump-cut dead gaps</div>
                                </div>
                            </div>
                            <label class="relative inline-flex items-center cursor-pointer">
                                <input type="checkbox" id="silence-toggle" class="sr-only peer" checked>
                                <div class="w-9 h-5 bg-zinc-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-ios-green"></div>
                            </label>
                        </div>
                        <div class="space-y-1">
                            <div class="flex justify-between text-[10px] text-white/70">
                                <span>Silence Detection Cut Threshold</span>
                                <span id="silence-thresh-label" class="font-mono text-ios-purple">-35 dB</span>
                            </div>
                            <input type="range" id="silence-slider" min="-60" max="-15" value="-35" class="w-full h-1.5 bg-white/20 rounded-lg appearance-none cursor-pointer accent-ios-purple">
                        </div>
                    </div>
                </div>

                <!-- Tab 2: Audio Filters & Enhancement -->
                <div id="tab-audio" class="space-y-3 hidden">
                    <div class="p-3.5 rounded-2xl glass-panel space-y-3">
                        <div class="flex items-center justify-between">
                            <div class="flex items-center space-x-2">
                                <i class="fa-solid fa-headset text-ios-accent text-sm"></i>
                                <span class="text-xs font-bold text-white">AI Background Noise Denoiser</span>
                            </div>
                            <label class="relative inline-flex items-center cursor-pointer">
                                <input type="checkbox" id="denoise-toggle" class="sr-only peer" checked>
                                <div class="w-9 h-5 bg-zinc-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-ios-accent"></div>
                            </label>
                        </div>
                        <div class="flex items-center justify-between pt-1 border-t border-white/10">
                            <div class="flex items-center space-x-2">
                                <i class="fa-solid fa-microphone-lines text-ios-green text-sm"></i>
                                <span class="text-xs font-bold text-white">Crisp Dialogue Equalizer Boost</span>
                            </div>
                            <label class="relative inline-flex items-center cursor-pointer">
                                <input type="checkbox" id="eq-toggle" class="sr-only peer" checked>
                                <div class="w-9 h-5 bg-zinc-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-ios-green"></div>
                            </label>
                        </div>
                    </div>
                </div>

                <!-- Tab 3: Subtitle Captions -->
                <div id="tab-captions" class="space-y-3 hidden">
                    <div class="p-3.5 rounded-2xl glass-panel space-y-3">
                        <div class="flex items-center justify-between">
                            <span class="text-xs font-bold text-white flex items-center space-x-1.5">
                                <i class="fa-solid fa-closed-captioning text-ios-pink"></i>
                                <span>Animated Caption Styles</span>
                            </span>
                            <label class="relative inline-flex items-center cursor-pointer">
                                <input type="checkbox" id="captions-toggle" class="sr-only peer" checked>
                                <div class="w-9 h-5 bg-zinc-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-ios-pink"></div>
                            </label>
                        </div>

                        <div class="grid grid-cols-3 gap-2">
                            <button onclick="setCaptionPreset('kinetic')" id="preset-kinetic" class="p-2 rounded-xl bg-ios-pink/20 border-2 border-ios-pink text-center space-y-0.5">
                                <div class="text-[11px] font-black text-yellow-300 italic">KINETIC</div>
                                <div class="text-[8px] text-white/70">Pop FX</div>
                            </button>
                            <button onclick="setCaptionPreset('meme')" id="preset-meme" class="p-2 rounded-xl bg-white/5 border border-white/10 text-center space-y-0.5 hover:bg-white/10">
                                <div class="text-[11px] font-black text-white uppercase">MEME</div>
                                <div class="text-[8px] text-white/70">Top/Bottom</div>
                            </button>
                            <button onclick="setCaptionPreset('karaoke')" id="preset-karaoke" class="p-2 rounded-xl bg-white/5 border border-white/10 text-center space-y-0.5 hover:bg-white/10">
                                <div class="text-[11px] font-bold text-ios-green">Karaoke</div>
                                <div class="text-[8px] text-white/70">Highlight</div>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Tab 4: Sound Effects Board -->
                <div id="tab-sfx" class="space-y-3 hidden">
                    <div class="p-3.5 rounded-2xl glass-panel space-y-2">
                        <span class="text-xs font-bold text-white flex items-center space-x-1.5">
                            <i class="fa-solid fa-compact-disc text-ios-orange"></i>
                            <span>Tap to Trigger Sound FX</span>
                        </span>
                        <div class="grid grid-cols-3 gap-2">
                            <button onclick="triggerSFX('vineboom')" class="p-2 rounded-xl bg-white/5 border border-white/10 hover:bg-ios-orange/20 transition text-xs flex flex-col items-center justify-center">
                                <span class="text-base">💥</span>
                                <span class="text-[9px] text-white/80 font-semibold mt-1">Vine Boom</span>
                            </button>
                            <button onclick="triggerSFX('airhorn')" class="p-2 rounded-xl bg-white/5 border border-white/10 hover:bg-ios-orange/20 transition text-xs flex flex-col items-center justify-center">
                                <span class="text-base">📢</span>
                                <span class="text-[9px] text-white/80 font-semibold mt-1">Air Horn</span>
                            </button>
                            <button onclick="triggerSFX('scratch')" class="p-2 rounded-xl bg-white/5 border border-white/10 hover:bg-ios-orange/20 transition text-xs flex flex-col items-center justify-center">
                                <span class="text-base">🎧</span>
                                <span class="text-[9px] text-white/80 font-semibold mt-1">Scratch</span>
                            </button>
                            <button onclick="triggerSFX('laugh')" class="p-2 rounded-xl bg-white/5 border border-white/10 hover:bg-ios-orange/20 transition text-xs flex flex-col items-center justify-center">
                                <span class="text-base">😂</span>
                                <span class="text-[9px] text-white/80 font-semibold mt-1">Laugh Track</span>
                            </button>
                            <button onclick="triggerSFX('dramatic')" class="p-2 rounded-xl bg-white/5 border border-white/10 hover:bg-ios-orange/20 transition text-xs flex flex-col items-center justify-center">
                                <span class="text-base">😱</span>
                                <span class="text-[9px] text-white/80 font-semibold mt-1">Dramatic</span>
                            </button>
                            <button onclick="triggerSFX('ding')" class="p-2 rounded-xl bg-white/5 border border-white/10 hover:bg-ios-orange/20 transition text-xs flex flex-col items-center justify-center">
                                <span class="text-base">✨</span>
                                <span class="text-[9px] text-white/80 font-semibold mt-1">Bell Ding</span>
                            </button>
                        </div>
                    </div>
                </div>
            </div>

        </main>

        <!-- Bottom Primary Master Render CTA Button -->
        <footer class="safe-pb px-4 py-3 bg-black/80 backdrop-blur-xl border-t border-white/10 shrink-0">
            <button id="master-render-btn" class="w-full py-3 rounded-2xl bg-gradient-to-r from-ios-accent via-ios-purple to-ios-pink text-white font-bold text-sm shadow-xl flex items-center justify-center space-x-2 active:scale-98 transition">
                <i class="fa-solid fa-bolt animate-pulse"></i>
                <span>Render & Auto-Cut AI Video</span>
            </button>
        </footer>

        <!-- Modal 1: How to Add to Home Screen (PWA Guide) -->
        <div id="install-modal" class="fixed inset-0 bg-black/80 backdrop-blur-xl z-50 flex items-center justify-center p-5 hidden">
            <div class="w-full max-w-xs glass-panel p-5 rounded-3xl space-y-4 border border-white/20 shadow-2xl text-center">
                <div class="w-12 h-12 rounded-2xl bg-ios-accent/20 text-ios-accent flex items-center justify-center mx-auto text-xl">
                    <i class="fa-solid fa-mobile-screen-button"></i>
                </div>
                <div>
                    <h3 class="text-base font-bold text-white">Add to Home Screen</h3>
                    <p class="text-xs text-white/70 mt-1">Use Nova AI like a native iOS / iPhone application without app store installs!</p>
                </div>
                <div class="space-y-2 text-left bg-black/40 p-3 rounded-xl border border-white/10 text-xs">
                    <div class="flex items-start space-x-2">
                        <span class="font-bold text-ios-accent">1.</span>
                        <span class="text-white/90">Tap the <i class="fa-solid fa-share-nodes text-ios-accent"></i> <b>Share</b> button in Safari toolbar below.</span>
                    </div>
                    <div class="flex items-start space-x-2">
                        <span class="font-bold text-ios-accent">2.</span>
                        <span class="text-white/90">Scroll down and tap <i class="fa-regular fa-square-plus text-ios-accent"></i> <b>Add to Home Screen</b>.</span>
                    </div>
                    <div class="flex items-start space-x-2">
                        <span class="font-bold text-ios-accent">3.</span>
                        <span class="text-white/90">Launch from your Home Screen in native full-screen mode!</span>
                    </div>
                </div>
                <button onclick="closeInstallModal()" class="w-full py-2.5 rounded-xl bg-ios-accent text-white font-bold text-xs shadow-lg">
                    Got it!
                </button>
            </div>
        </div>

        <!-- Modal 2: How to Use Interactive Guide & Onboarding Tour -->
        <div id="guide-modal" class="fixed inset-0 bg-black/85 backdrop-blur-xl z-50 flex items-center justify-center p-5 hidden">
            <div class="w-full max-w-xs glass-panel p-5 rounded-3xl space-y-4 border border-white/20 shadow-2xl text-center">
                <div class="flex justify-between items-center border-b border-white/10 pb-2">
                    <span class="text-xs font-bold text-ios-purple uppercase tracking-wider">How To Use Nova AI</span>
                    <button onclick="closeGuideModal()" class="text-white/60 hover:text-white text-sm">
                        <i class="fa-solid fa-xmark"></i>
                    </button>
                </div>

                <div id="guide-step-content" class="space-y-3 min-h-[160px] flex flex-col justify-center">
                    <!-- Step contents inserted via JS -->
                </div>

                <div class="flex items-center justify-between pt-2 border-t border-white/10">
                    <button id="guide-prev-btn" onclick="prevGuideStep()" class="px-3 py-1.5 rounded-lg bg-white/10 text-xs text-white/70 font-semibold disabled:opacity-30">Previous</button>
                    <span id="guide-step-indicator" class="text-[10px] text-white/50 font-mono">1 / 5</span>
                    <button id="guide-next-btn" onclick="nextGuideStep()" class="px-3 py-1.5 rounded-lg bg-ios-accent text-xs text-white font-bold">Next</button>
                </div>
            </div>
        </div>

        <!-- Modal 3: Render / Export Progress -->
        <div id="export-modal" class="fixed inset-0 bg-black/85 backdrop-blur-xl z-50 flex items-center justify-center p-5 hidden">
            <div class="w-full max-w-xs glass-panel p-6 rounded-3xl space-y-4 border border-white/20 shadow-2xl text-center">
                <div class="relative w-16 h-16 mx-auto flex items-center justify-center">
                    <svg class="w-full h-full transform -rotate-90" viewBox="0 0 36 36">
                        <path class="text-white/10" stroke-width="3" stroke="currentColor" fill="none" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831" />
                        <path id="export-progress-circle" class="text-ios-accent stroke-current" stroke-dasharray="0, 100" stroke-width="3" stroke-linecap="round" fill="none" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831" />
                    </svg>
                    <span id="export-percent-text" class="absolute font-bold text-xs text-white font-mono">0%</span>
                </div>
                <div>
                    <h3 class="text-sm font-bold text-white" id="export-status-heading">Processing AI Video</h3>
                    <p class="text-[11px] text-white/60 mt-0.5">Exporting full quality file...</p>
                </div>
            </div>
        </div>

    </div>

    <!-- Tone.js Sound Synthesizer -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.8.49/Tone.js"></script>

    <!-- App Scripts -->
    <script>
        // State Management
        const appState = {
            isPlaying: false,
            currentTime: 0,
            duration: 15.0,
            aspectRatio: '9/16',
            silenceCut: true,
            silenceThreshold: -35,
            denoise: true,
            voiceBoost: true,
            captionsEnabled: true,
            captionStyle: 'kinetic',
            silences: [
                { start: 2.2, end: 3.6 },
                { start: 6.9, end: 8.4 },
                { start: 11.2, end: 12.5 }
            ],
            captions: [
                { start: 0.2, end: 2.1, text: "Welcome to Nova AI Video Editor!" },
                { start: 3.7, end: 6.8, text: "Automatically remove dead silence in 1 tap." },
                { start: 8.5, end: 11.1, text: "Add voice noise filters and kinetic captions." },
                { start: 12.6, end: 14.8, text: "Export ready for TikTok, Reels & Shorts!" }
            ]
        };

        // DOM elements
        const canvas = document.getElementById('preview-canvas');
        const ctx = canvas.getContext('2d');
        const canvasWrapper = document.getElementById('canvas-wrapper');
        const waveformCanvas = document.getElementById('waveform-canvas');
        const playPauseBtn = document.getElementById('play-pause-btn');
        const playIcon = document.getElementById('play-icon');

        // Setup Canvas Resolution
        function resizeCanvas() {
            const rect = canvasWrapper.getBoundingClientRect();
            canvas.width = rect.width * (window.devicePixelRatio || 2);
            canvas.height = rect.height * (window.devicePixelRatio || 2);
            
            const wRect = document.getElementById('waveform-container').getBoundingClientRect();
            waveformCanvas.width = wRect.width * (window.devicePixelRatio || 2);
            waveformCanvas.height = wRect.height * (window.devicePixelRatio || 2);
            
            renderWaveform();
        }
        window.addEventListener('resize', resizeCanvas);

        // Aspect Ratio Selector Logic
        function changeAspectRatio(ratio) {
            appState.aspectRatio = ratio;
            document.getElementById('ratio-916').className = ratio === '9/16' ? 'px-2 py-0.5 rounded bg-ios-accent text-white font-bold' : 'px-2 py-0.5 rounded bg-white/10 text-white/70 hover:bg-white/20';
            document.getElementById('ratio-169').className = ratio === '16/9' ? 'px-2 py-0.5 rounded bg-ios-accent text-white font-bold' : 'px-2 py-0.5 rounded bg-white/10 text-white/70 hover:bg-white/20';
            document.getElementById('ratio-11').className = ratio === '1/1' ? 'px-2 py-0.5 rounded bg-ios-accent text-white font-bold' : 'px-2 py-0.5 rounded bg-white/10 text-white/70 hover:bg-white/20';

            if (ratio === '9/16') {
                canvasWrapper.style.aspectRatio = '9/16';
                document.getElementById('aspect-ratio-label').textContent = '9:16 Vertical Shorts';
            } else if (ratio === '16/9') {
                canvasWrapper.style.aspectRatio = '16/9';
                document.getElementById('aspect-ratio-label').textContent = '16:9 Landscape Video';
            } else if (ratio === '1/1') {
                canvasWrapper.style.aspectRatio = '1/1';
                document.getElementById('aspect-ratio-label').textContent = '1:1 Square Post';
            }
            setTimeout(resizeCanvas, 50);
        }

        // Draw Video Frame & Captions
        function renderVideoFrame(time) {
            ctx.fillStyle = '#050507';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // Dynamic Background Gradient
            const grad = ctx.createRadialGradient(
                canvas.width / 2, canvas.height / 2, 30,
                canvas.width / 2, canvas.height / 2, canvas.width
            );
            const hue = (time * 30) % 360;
            grad.addColorStop(0, `hsla(${hue}, 65%, 22%, 0.85)`);
            grad.addColorStop(1, '#000000');
            ctx.fillStyle = grad;
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // Animated Visual Presenter Avatar
            ctx.save();
            ctx.translate(canvas.width / 2, canvas.height / 2 - 20);

            const isSilence = appState.silences.some(s => time >= s.start && time <= s.end);
            const bounce = 1 + Math.sin(time * 6) * 0.04;
            ctx.scale(bounce, bounce);

            // Speaker Ring
            ctx.beginPath();
            ctx.arc(0, 0, canvas.width * 0.18, 0, Math.PI * 2);
            ctx.strokeStyle = appState.denoise ? '#0A84FF' : '#444';
            ctx.lineWidth = 5;
            ctx.stroke();

            // Head
            ctx.beginPath();
            ctx.arc(0, -10, canvas.width * 0.10, 0, Math.PI * 2);
            ctx.fillStyle = '#1c1c1e';
            ctx.fill();

            // Animated Speaking Mouth
            ctx.beginPath();
            if (!isSilence) {
                const mouthH = 6 + Math.sin(time * 22) * 5;
                ctx.ellipse(0, -2, 8, mouthH, 0, 0, Math.PI * 2);
                ctx.fillStyle = '#FF375F';
            } else {
                ctx.rect(-6, -2, 12, 2);
                ctx.fillStyle = '#888';
            }
            ctx.fill();

            ctx.restore();

            // Render Subtitle Overlay
            if (appState.captionsEnabled) {
                renderSubtitles(time);
            }
        }

        // Subtitle Preset Renderer
        function renderSubtitles(time) {
            const cap = appState.captions.find(c => time >= c.start && time <= c.end);
            if (!cap) return;

            ctx.save();
            const text = cap.text;

            if (appState.captionStyle === 'kinetic') {
                ctx.font = `900 ${canvas.width * 0.06}px "-apple-system", sans-serif`;
                ctx.textAlign = 'center';
                ctx.fillStyle = '#FFD60A';
                ctx.strokeStyle = '#000000';
                ctx.lineWidth = 6;
                ctx.strokeText(text, canvas.width / 2, canvas.height * 0.80);
                ctx.fillText(text, canvas.width / 2, canvas.height * 0.80);
            } else if (appState.captionStyle === 'meme') {
                ctx.font = `900 ${canvas.width * 0.055}px "Impact", sans-serif`;
                ctx.textAlign = 'center';
                ctx.fillStyle = '#FFFFFF';
                ctx.strokeStyle = '#000000';
                ctx.lineWidth = 5;
                ctx.strokeText(text.toUpperCase(), canvas.width / 2, canvas.height * 0.82);
                ctx.fillText(text.toUpperCase(), canvas.width / 2, canvas.height * 0.82);
            } else if (appState.captionStyle === 'karaoke') {
                ctx.font = `700 ${canvas.width * 0.05}px "-apple-system", sans-serif`;
                ctx.textAlign = 'center';
                ctx.fillStyle = '#30D158';
                ctx.fillText(text, canvas.width / 2, canvas.height * 0.80);
            }
            ctx.restore();
        }

        // Draw Waveform Timeline
        function renderWaveform() {
            const w = waveformCanvas.width;
            const h = waveformCanvas.height;
            const wCtx = waveformCanvas.getContext('2d');
            wCtx.clearRect(0, 0, w, h);

            const numBars = 65;
            const barW = w / numBars;

            for (let i = 0; i < numBars; i++) {
                const t = (i / numBars) * appState.duration;
                const isSilent = appState.silences.some(s => t >= s.start && t <= s.end);

                let barH = isSilent ? (h * 0.15) : (Math.sin(i * 0.5) * 0.35 + 0.45) * h;
                
                if (isSilent && appState.silenceCut) {
                    wCtx.fillStyle = '#FF375F'; // Silent cuts highlighted red
                } else if (!isSilent) {
                    wCtx.fillStyle = '#30D158'; // Active voice green
                } else {
                    wCtx.fillStyle = '#444';
                }

                wCtx.fillRect(i * barW, (h - barH) / 2, barW - 1.5, barH);
            }
        }

        // Loop Animation
        function animate() {
            if (appState.isPlaying) {
                appState.currentTime += 0.033;

                // Auto Cut Silence Fast-Forward
                if (appState.silenceCut) {
                    const silenceSegment = appState.silences.find(s => appState.currentTime >= s.start && appState.currentTime <= s.end);
                    if (silenceSegment) {
                        appState.currentTime = silenceSegment.end + 0.05;
                    }
                }

                if (appState.currentTime >= appState.duration) {
                    appState.currentTime = 0;
                }
                updateTimeUI();
            }

            renderVideoFrame(appState.currentTime);
            requestAnimationFrame(animate);
        }

        function updateTimeUI() {
            const mins = Math.floor(appState.currentTime / 60);
            const secs = (appState.currentTime % 60).toFixed(1);
            document.getElementById('time-current').textContent = `${mins.toString().padStart(2, '0')}:${secs.padStart(4, '0')}`;
            
            const progressPct = (appState.currentTime / appState.duration) * 100;
            document.getElementById('timeline-playhead').style.left = `${progressPct}%`;
        }

        // Soundboard Generator
        function triggerSFX(type) {
            Tone.start();
            if (type === 'vineboom') {
                const synth = new Tone.MembraneSynth().toDestination();
                synth.triggerAttackRelease("C1", "8n");
            } else if (type === 'airhorn') {
                const synth = new Tone.PolySynth().toDestination();
                synth.triggerAttackRelease(["F5", "F5", "F5"], "16n");
            } else if (type === 'scratch') {
                const noise = new Tone.NoiseSynth({ envelope: { attack: 0.01, decay: 0.1, sustain: 0 } }).toDestination();
                noise.triggerAttackRelease("8n");
            } else if (type === 'laugh') {
                const synth = new Tone.PolySynth().toDestination();
                synth.triggerAttackRelease(["C4", "E4", "G4"], "16n");
            } else if (type === 'dramatic') {
                const synth = new Tone.MetalSynth().toDestination();
                synth.triggerAttackRelease("C2", "8n");
            } else if (type === 'ding') {
                const synth = new Tone.Synth().toDestination();
                synth.triggerAttackRelease("E6", "16n");
            }
        }

        // Navigation Tabs
        function switchTab(tabName) {
            ['silence', 'audio', 'captions', 'sfx'].forEach(t => {
                document.getElementById(`tab-${t}`).classList.add('hidden');
                document.getElementById(`tab-btn-${t}`).className = 'flex-1 py-1.5 rounded-lg text-white/60 hover:text-white transition text-center';
            });
            document.getElementById(`tab-${tabName}`).classList.remove('hidden');
            document.getElementById(`tab-btn-${tabName}`).className = 'flex-1 py-1.5 rounded-lg bg-white/20 text-white font-semibold transition text-center';
        }

        function setCaptionPreset(preset) {
            appState.captionStyle = preset;
            ['kinetic', 'meme', 'karaoke'].forEach(p => {
                document.getElementById(`preset-${p}`).className = p === preset ? 
                    'p-2 rounded-xl bg-ios-pink/20 border-2 border-ios-pink text-center space-y-0.5' : 
                    'p-2 rounded-xl bg-white/5 border border-white/10 text-center space-y-0.5 hover:bg-white/10';
            });
            document.getElementById('caption-style-name').textContent = preset.toUpperCase();
        }

        // --- Modals & Onboarding Tour Logic ---
        function openInstallModal() { document.getElementById('install-modal').classList.remove('hidden'); }
        function closeInstallModal() { document.getElementById('install-modal').classList.add('hidden'); }

        const guideSteps = [
            { title: "Step 1: Upload Media", text: "Upload your video file or tap 'Sample Track' to start editing instantly.", icon: "fa-cloud-arrow-up text-ios-accent" },
            { title: "Step 2: Auto Silence Cut", text: "Enable Silence Remover to detect and jump-cut dead gaps automatically.", icon: "fa-scissors text-ios-purple" },
            { title: "Step 3: AI Audio Filters", text: "Toggle AI Noise Denoiser and Crisp Dialogue Equalizer for studio voice clarity.", icon: "fa-headset text-ios-green" },
            { title: "Step 4: Subtitles & SFX", text: "Select Kinetic or Meme subtitle presets and tap the SFX board for instant sound triggers.", icon: "fa-closed-captioning text-ios-pink" },
            { title: "Step 5: Render & Export", text: "Tap 'Render & Auto-Cut AI Video' to process and export your finalized video!", icon: "fa-bolt text-ios-yellow" }
        ];
        let currentGuideStep = 0;

        function openGuideModal() {
            currentGuideStep = 0;
            renderGuideStep();
            document.getElementById('guide-modal').classList.remove('hidden');
        }
        function closeGuideModal() { document.getElementById('guide-modal').classList.add('hidden'); }

        function renderGuideStep() {
            const step = guideSteps[currentGuideStep];
            document.getElementById('guide-step-content').innerHTML = `
                <div class="w-12 h-12 rounded-2xl bg-white/10 flex items-center justify-center mx-auto text-2xl">
                    <i class="fa-solid ${step.icon}"></i>
                </div>
                <h4 class="text-sm font-bold text-white">${step.title}</h4>
                <p class="text-xs text-white/70">${step.text}</p>
            `;
            document.getElementById('guide-step-indicator').textContent = `${currentGuideStep + 1} / ${guideSteps.length}`;
            document.getElementById('guide-prev-btn').disabled = currentGuideStep === 0;
            document.getElementById('guide-next-btn').textContent = currentGuideStep === guideSteps.length - 1 ? 'Finish' : 'Next';
        }
        function prevGuideStep() { if (currentGuideStep > 0) { currentGuideStep--; renderGuideStep(); } }
        function nextGuideStep() {
            if (currentGuideStep < guideSteps.length - 1) {
                currentGuideStep++;
                renderGuideStep();
            } else {
                closeGuideModal();
            }
        }

        // Initialize App
        document.addEventListener('DOMContentLoaded', () => {
            resizeCanvas();
            animate();

            // Detect if running as standalone PWA
            const isPWA = window.matchMedia('(display-mode: standalone)').matches || window.navigator.standalone;
            if (isPWA) {
                document.getElementById('pwa-install-banner').style.display = 'none';
            }

            // Playback controls
            const togglePlay = () => {
                appState.isPlaying = !appState.isPlaying;
                playIcon.className = appState.isPlaying ? "fa-solid fa-pause" : "fa-solid fa-play ml-0.5";
            };
            playPauseBtn.addEventListener('click', togglePlay);
            canvas.addEventListener('click', togglePlay);

            // Timeline Waveform click to seek
            document.getElementById('waveform-container').addEventListener('click', (e) => {
                const rect = waveformCanvas.getBoundingClientRect();
                const clickPos = (e.clientX - rect.left) / rect.width;
                appState.currentTime = clickPos * appState.duration;
                updateTimeUI();
            });

            // Toggles & Sliders
            document.getElementById('silence-toggle').addEventListener('change', (e) => {
                appState.silenceCut = e.target.checked;
                renderWaveform();
            });
            document.getElementById('silence-slider').addEventListener('input', (e) => {
                appState.silenceThreshold = e.target.value;
                document.getElementById('silence-thresh-label').textContent = `${e.target.value} dB`;
            });
            document.getElementById('denoise-toggle').addEventListener('change', (e) => { appState.denoise = e.target.checked; });
            document.getElementById('eq-toggle').addEventListener('change', (e) => { appState.voiceBoost = e.target.checked; });
            document.getElementById('captions-toggle').addEventListener('change', (e) => { appState.captionsEnabled = e.target.checked; });

            // Render / Export simulation
            const startExport = () => {
                appState.isPlaying = false;
                playIcon.className = "fa-solid fa-play ml-0.5";
                const modal = document.getElementById('export-modal');
                const circle = document.getElementById('export-progress-circle');
                const text = document.getElementById('export-percent-text');
                const heading = document.getElementById('export-status-heading');
                
                modal.classList.remove('hidden');
                let progress = 0;
                
                const interval = setInterval(() => {
                    progress += 5;
                    circle.setAttribute('stroke-dasharray', `${progress}, 100`);
                    text.textContent = `${progress}%`;

                    if (progress === 40) heading.textContent = "Removing Silent Gaps...";
                    if (progress === 75) heading.textContent = "Applying Kinetic Captions...";

                    if (progress >= 100) {
                        clearInterval(interval);
                        modal.classList.add('hidden');
                        alert("Video successfully processed and ready for download!");
                    }
                }, 80);
            };

            document.getElementById('master-render-btn').addEventListener('click', startExport);
            document.getElementById('export-btn').addEventListener('click', startExport);
        });
    </script>
</body>
</html>
