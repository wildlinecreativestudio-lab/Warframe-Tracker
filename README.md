```html
<!DOCTYPE html>
<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Warframe Master Companion & Progress Tracker</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Font Awesome Icons fallback -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600;800;900&family=Rajdhani:wght@400;500;600;700&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        orokin: {
                            gold: '#d4af37',
                            lightgold: '#f3e5ab',
                            darkgold: '#8c7321',
                        },
                        lotus: {
                            cyan: '#00f0ff',
                            blue: '#0088ff',
                            purple: '#8a2be2',
                            darkbg: '#0a0d14',
                            card: '#121824',
                            border: '#1f293d'
                        }
                    },
                    fontFamily: {
                        orbitron: ['Orbitron', 'sans-serif'],
                        rajdhani: ['Rajdhani', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Rajdhani', sans-serif;
            background-color: #07090e;
            color: #d1d5db;
            background-image: 
                radial-gradient(circle at 50% 0%, rgba(0, 240, 255, 0.08) 0%, transparent 50%),
                radial-gradient(circle at 100% 100%, rgba(212, 175, 55, 0.05) 0%, transparent 40%);
            background-attachment: fixed;
        }
        
        .font-orbitron {
            font-family: 'Orbitron', sans-serif;
        }

        .gold-glow {
            box-shadow: 0 0 15px rgba(212, 175, 55, 0.25);
        }
        .cyan-glow {
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.25);
        }
        .gold-border-glow {
            border-color: rgba(212, 175, 55, 0.5);
            box-shadow: inset 0 0 10px rgba(212, 175, 55, 0.15), 0 0 10px rgba(212, 175, 55, 0.2);
        }
        .cyan-border-glow {
            border-color: rgba(0, 240, 255, 0.5);
            box-shadow: inset 0 0 10px rgba(0, 240, 255, 0.15), 0 0 10px rgba(0, 240, 255, 0.2);
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #0d1117;
        }
        ::-webkit-scrollbar-thumb {
            background: #1f293d;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #00f0ff;
        }

        /* Orokin Geometric Accent Pattern */
        .orokin-header {
            background: linear-gradient(135deg, rgba(18, 24, 36, 0.95) 0%, rgba(10, 13, 20, 0.98) 100%);
            border-bottom: 2px solid #d4af37;
            position: relative;
        }
        .orokin-header::after {
            content: '';
            position: absolute;
            bottom: -2px;
            left: 50%;
            transform: translateX(-50%);
            width: 200px;
            height: 2px;
            background: #00f0ff;
            box-shadow: 0 0 10px #00f0ff;
        }
        
        /* Modal Backdrop Fade */
        .modal-fade {
            background-color: rgba(5, 7, 12, 0.85);
            backdrop-filter: blur(8px);
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between text-gray-200">

    <header class="orokin-header sticky top-0 z-40 shadow-2xl backdrop-blur-md">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-3">
            <div class="flex flex-col md:flex-row justify-between items-center gap-4">
                
                <!-- Logo & System Designation -->
                <div class="flex items-center space-x-3 w-full md:w-auto justify-between md:justify-start">
                    <div class="flex items-center space-x-3">
                        <div class="w-10 h-10 rounded-lg bg-gradient-to-br from-orokin-gold to-yellow-700 flex items-center justify-center shadow-lg shadow-orokin-gold/20 border border-orokin-lightgold">
                            <i class="fa-solid fa-shield-halved text-gray-950 text-xl font-bold"></i>
                        </div>
                        <div>
                            <h1 class="font-orbitron font-extrabold text-xl tracking-wider bg-clip-text text-transparent bg-gradient-to-r from-orokin-gold via-yellow-200 to-lotus-cyan">
                                TENNO NEXUS
                            </h1>
                            <p class="text-xs text-cyan-400/80 tracking-widest uppercase font-semibold">Mastery & Codex Companion</p>
                        </div>
                    </div>

                    <!-- Mobile Action Controls -->
                    <div class="flex items-center space-x-2 md:hidden">
                        <button onclick="openBackupModal()" class="p-2 rounded bg-gray-800 text-orokin-gold border border-orokin-gold/30 hover:bg-gray-700">
                            <i data-lucide="database" class="w-4 h-4"></i>
                        </button>
                    </div>
                </div>

                <!-- Profile Summary & Mastery Rank Widget -->
                <div class="flex items-center bg-gray-900/90 border border-gray-800 rounded-xl p-2 sm:px-4 sm:py-2.5 w-full md:w-auto justify-between gap-4 shadow-inner">
                    <div class="flex items-center space-x-3">
                        <div class="relative flex items-center justify-center">
                            <div class="w-12 h-12 rounded-full border-2 border-orokin-gold flex items-center justify-center bg-gray-950 text-orokin-gold font-orbitron font-bold text-lg shadow-md shadow-orokin-gold/30" id="mrRankBadge">
                                30
                            </div>
                            <div class="absolute -bottom-1 bg-orokin-gold text-gray-950 text-[9px] font-extrabold px-1.5 py-0.2 rounded uppercase" id="mrTitleBadge">
                                Grand Master
                            </div>
                        </div>

                        <div class="flex flex-col">
                            <div class="flex items-center space-x-2">
                                <span class="font-orbitron font-bold text-sm sm:text-base text-white tracking-wide" id="profileNameDisplay">Tenno</span>
                                <button onclick="editProfileName()" class="text-gray-500 hover:text-orokin-gold transition-colors">
                                    <i data-lucide="edit-2" class="w-3.5 h-3.5"></i>
                                </button>
                            </div>
                            <div class="flex items-center space-x-2 text-xs text-gray-400">
                                <span>MR XP: <strong class="text-orokin-gold" id="mrXpDisplay">2,250,000</strong></span>
                                <span class="text-gray-600">|</span>
                                <span>Next: <span class="text-cyan-400" id="mrNextXpDisplay">147,500</span></span>
                            </div>
                        </div>
                    </div>

                    <div class="hidden sm:flex flex-col items-end pl-4 border-l border-gray-800">
                        <div class="text-xs text-gray-400">Overall Mastery</div>
                        <div class="font-orbitron font-bold text-lg text-lotus-cyan" id="totalCompletionPct">68.4%</div>
                    </div>

                    <div class="flex items-center gap-2 pl-2 border-l border-gray-800">
                        <button onclick="openBackupModal()" class="hidden md:flex items-center space-x-1 px-3 py-1.5 text-xs rounded bg-gray-800 hover:bg-gray-700 text-orokin-gold border border-orokin-gold/30 transition">
                            <i data-lucide="hard-drive" class="w-3.5 h-3.5"></i>
                            <span>Data Sync</span>
                        </button>
                    </div>
                </div>
            </div>

            <!-- Master Search & Dynamic Filters Bar -->
            <div class="mt-3 pt-3 border-t border-gray-800/80 flex flex-col md:flex-row items-center justify-between gap-3">
                <div class="relative w-full md:w-96">
                    <i data-lucide="search" class="w-4 h-4 absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-500"></i>
                    <input type="text" id="globalSearchInput" placeholder="Search Warframes, Weapons, Mods, Relics..." 
                           class="w-full pl-9 pr-8 py-1.5 text-sm bg-gray-950/80 border border-gray-800 rounded-lg focus:outline-none focus:border-lotus-cyan text-gray-200 placeholder-gray-500 transition">
                    <button id="clearSearchBtn" onclick="clearGlobalSearch()" class="hidden absolute right-2.5 top-1/2 transform -translate-y-1/2 text-gray-500 hover:text-white">
                        <i data-lucide="x" class="w-4 h-4"></i>
                    </button>
                </div>

                <div class="flex flex-wrap items-center justify-start md:justify-end gap-1.5 w-full md:w-auto text-xs">
                    <span class="text-gray-500 text-xs font-semibold mr-1 uppercase tracking-wider">Filter:</span>
                    <button class="filter-btn active px-2.5 py-1 rounded bg-gray-800 text-gray-300 border border-gray-700 hover:border-lotus-cyan transition" data-filter="all">All</button>
                    <button class="filter-btn px-2.5 py-1 rounded bg-gray-900 text-gray-400 border border-gray-800 hover:border-lotus-cyan transition" data-filter="mastered">Mastered</button>
                    <button class="filter-btn px-2.5 py-1 rounded bg-gray-900 text-gray-400 border border-gray-800 hover:border-lotus-cyan transition" data-filter="unmastered">Unmastered</button>
                    <button class="filter-btn px-2.5 py-1 rounded bg-gray-900 text-gray-400 border border-gray-800 hover:border-lotus-cyan transition" data-filter="prime">Prime</button>
                    <button class="filter-btn px-2.5 py-1 rounded bg-gray-900 text-gray-400 border border-gray-800 hover:border-lotus-cyan transition" data-filter="craftable">Craftable</button>
                    <button class="filter-btn px-2.5 py-1 rounded bg-gray-900 text-gray-400 border border-gray-800 hover:border-lotus-cyan transition" data-filter="vaulted">Vaulted</button>
                </div>
            </div>
        </div>
    </header>

    <nav class="bg-gray-950/90 border-b border-gray-800 sticky top-[105px] z-30 backdrop-blur-md">
        <div class="max-w-7xl mx-auto px-4 overflow-x-auto no-scrollbar">
            <div class="flex space-x-1 sm:space-x-2 py-2 min-w-max" id="mainTabContainer">
                <button onclick="switchTab('dashboard')" class="tab-btn active px-3.5 py-2 rounded-lg font-orbitron text-xs sm:text-sm font-semibold tracking-wider flex items-center space-x-2 bg-gradient-to-r from-orokin-gold/20 to-transparent text-orokin-gold border border-orokin-gold/40">
                    <i data-lucide="layout-dashboard" class="w-4 h-4"></i>
                    <span>Overview</span>
                </button>
                <button onclick="switchTab('starchart')" class="tab-btn px-3.5 py-2 rounded-lg font-orbitron text-xs sm:text-sm font-semibold tracking-wider flex items-center space-x-2 text-gray-400 hover:text-gray-200 hover:bg-gray-900 border border-transparent">
                    <i data-lucide="globe" class="w-4 h-4"></i>
                    <span>Star Chart</span>
                </button>
                <button onclick="switchTab('warframes')" class="tab-btn px-3.5 py-2 rounded-lg font-orbitron text-xs sm:text-sm font-semibold tracking-wider flex items-center space-x-2 text-gray-400 hover:text-gray-200 hover:bg-gray-900 border border-transparent">
                    <i data-lucide="user-check" class="w-4 h-4"></i>
                    <span>Warframes</span>
                </button>
                <button onclick="switchTab('weapons')" class="tab-btn px-3.5 py-2 rounded-lg font-orbitron text-xs sm:text-sm font-semibold tracking-wider flex items-center space-x-2 text-gray-400 hover:text-gray-200 hover:bg-gray-900 border border-transparent">
                    <i data-lucide="crosshair" class="w-4 h-4"></i>
                    <span>Weapons</span>
                </button>
                <button onclick="switchTab('inventory')" class="tab-btn px-3.5 py-2 rounded-lg font-orbitron text-xs sm:text-sm font-semibold tracking-wider flex items-center space-x-2 text-gray-400 hover:text-gray-200 hover:bg-gray-900 border border-transparent">
                    <i data-lucide="boxes" class="w-4 h-4"></i>
                    <span>Resources & Relics</span>
                </button>
                <button onclick="switchTab('mods')" class="tab-btn px-3.5 py-2 rounded-lg font-orbitron text-xs sm:text-sm font-semibold tracking-wider flex items-center space-x-2 text-gray-400 hover:text-gray-200 hover:bg-gray-900 border border-transparent">
                    <i data-lucide="zap" class="w-4 h-4"></i>
                    <span>Mods & Arcanes</span>
                </button>
                <button onclick="switchTab('syndicates')" class="tab-btn px-3.5 py-2 rounded-lg font-orbitron text-xs sm:text-sm font-semibold tracking-wider flex items-center space-x-2 text-gray-400 hover:text-gray-200 hover:bg-gray-900 border border-transparent">
                    <i data-lucide="award" class="w-4 h-4"></i>
                    <span>Quests & Factions</span>
                </button>
            </div>
        </div>
    </nav>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 flex-grow w-full">
        
        <!-- DASHBOARD / OVERVIEW VIEW -->
        <section id="view-dashboard" class="tab-content block space-y-6">
            
            <!-- Daily Cap & Standing Status Grid -->
            <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                <div class="bg-gray-900/80 border border-gray-800 rounded-xl p-4 flex items-center space-x-4">
                    <div class="p-3 bg-cyan-950/60 rounded-lg border border-cyan-500/30 text-cyan-400">
                        <i data-lucide="sun" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <div class="text-xs text-gray-400 font-semibold uppercase">Daily Standing Cap</div>
                        <div class="font-orbitron font-bold text-xl text-white" id="dailyStandingCap">31,000</div>
                        <div class="text-[11px] text-gray-500">Based on Mastery Rank <span id="standingMrRef">30</span></div>
                    </div>
                </div>

                <div class="bg-gray-900/80 border border-gray-800 rounded-xl p-4 flex items-center space-x-4">
                    <div class="p-3 bg-purple-950/60 rounded-lg border border-purple-500/30 text-purple-400">
                        <i data-lucide="sparkles" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <div class="text-xs text-gray-400 font-semibold uppercase">Daily Focus Cap</div>
                        <div class="font-orbitron font-bold text-xl text-white" id="dailyFocusCap">400,000</div>
                        <div class="text-[11px] text-gray-500">250k + (5,000 × MR)</div>
                    </div>
                </div>

                <div class="bg-gray-900/80 border border-gray-800 rounded-xl p-4 flex items-center space-x-4">
                    <div class="p-3 bg-amber-950/60 rounded-lg border border-amber-500/30 text-amber-400">
                        <i data-lucide="dna" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <div class="text-xs text-gray-400 font-semibold uppercase">Helminth Subjugated</div>
                        <div class="font-orbitron font-bold text-xl text-white"><span id="helminthCount">38</span> / <span id="helminthTotal">57</span></div>
                        <div class="text-[11px] text-amber-400/80 font-medium" id="helminthPct">66.6% Absorbed</div>
                    </div>
                </div>

                <div class="bg-gray-900/80 border border-gray-800 rounded-xl p-4 flex items-center space-x-4">
                    <div class="p-3 bg-red-950/60 rounded-lg border border-red-500/30 text-red-400">
                        <i data-lucide="skull" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <div class="text-xs text-gray-400 font-semibold uppercase">Steel Path Cleared</div>
                        <div class="font-orbitron font-bold text-xl text-white"><span id="spNodesCleared">228</span> / 238</div>
                        <div class="text-[11px] text-red-400/80 font-medium" id="spNodesPct">95.8% Complete</div>
                    </div>
                </div>
            </div>

            <!-- Master Category Breakdown Grid -->
            <div class="bg-gray-900/80 border border-gray-800 rounded-2xl p-6 shadow-xl backdrop-blur-md">
                <div class="flex items-center justify-between mb-6 pb-3 border-b border-gray-800">
                    <div class="flex items-center space-x-2">
                        <i data-lucide="pie-chart" class="w-5 h-5 text-orokin-gold"></i>
                        <h2 class="font-orbitron font-bold text-lg text-white tracking-wide">Category Mastery Overview</h2>
                    </div>
                    <span class="text-xs text-gray-400">Calculated from total Codex gear entries</span>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6" id="dashboardProgressGrid">
                    <!-- Dynamic rendering via JavaScript -->
                </div>
            </div>

            <!-- Quick Action / Priority Tasks Bar -->
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <!-- Ready to Craft Panel -->
                <div class="bg-gray-900/80 border border-gray-800 rounded-2xl p-5 lg:col-span-2">
                    <div class="flex items-center justify-between mb-4">
                        <div class="flex items-center space-x-2">
                            <i data-lucide="anvil" class="w-5 h-5 text-lotus-cyan"></i>
                            <h3 class="font-orbitron font-bold text-base text-white">Craftable Gear in Foundry</h3>
                        </div>
                        <span class="px-2 py-0.5 rounded text-xs bg-cyan-950 text-cyan-400 border border-cyan-800" id="craftableCountBadge">3 Items Ready</span>
                    </div>

                    <div class="space-y-2.5 max-h-64 overflow-y-auto pr-1" id="craftableListContainer">
                        <!-- Populated JS -->
                    </div>
                </div>

                <!-- Recommended Codex Targets -->
                <div class="bg-gray-900/80 border border-gray-800 rounded-2xl p-5">
                    <div class="flex items-center space-x-2 mb-4">
                        <i data-lucide="target" class="w-5 h-5 text-orokin-gold"></i>
                        <h3 class="font-orbitron font-bold text-base text-white">MR XP Priority Recommendations</h3>
                    </div>

                    <div class="space-y-3" id="recommendationsList">
                        <!-- Populated JS -->
                    </div>
                </div>
            </div>
        </section>

        <!-- STAR CHART TRACKER VIEW -->
        <section id="view-starchart" class="tab-content hidden space-y-6">
            <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-4 bg-gray-900/80 p-5 rounded-2xl border border-gray-800">
                <div>
                    <h2 class="font-orbitron font-bold text-xl text-white flex items-center gap-2">
                        <i data-lucide="map" class="w-5 h-5 text-lotus-cyan"></i>
                        Star Chart & System Progression
                    </h2>
                    <p class="text-xs text-gray-400 mt-1">Track Origin System Solar Nodes, Steel Path, Railjack Sectors, and Open-World Hub Standing</p>
                </div>
                
                <div class="flex items-center gap-4 text-sm font-orbitron">
                    <div class="bg-gray-950 px-4 py-2 rounded-xl border border-gray-800">
                        <span class="text-gray-400 text-xs block">Normal Solar Map</span>
                        <span class="text-lotus-cyan font-bold text-lg" id="starchartNormalCount">238 / 238</span>
                    </div>
                    <div class="bg-gray-950 px-4 py-2 rounded-xl border border-gray-800">
                        <span class="text-gray-400 text-xs block">Steel Path Nodes</span>
                        <span class="text-red-400 font-bold text-lg" id="starchartSpCount">228 / 238</span>
                    </div>
                </div>
            </div>

            <!-- Planets & Regions Grid -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5" id="planetsGrid">
                <!-- Dynamically Populated JS -->
            </div>
        </section>

        <!-- WARFRAMES TRACKER VIEW -->
        <section id="view-warframes" class="tab-content hidden space-y-6">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 bg-gray-900/80 p-4 rounded-xl border border-gray-800">
                <div class="flex items-center space-x-3">
                    <div class="p-2.5 bg-orokin-gold/10 border border-orokin-gold/30 rounded-lg text-orokin-gold">
                        <i data-lucide="user-check" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <h2 class="font-orbitron font-bold text-lg text-white">Warframe Mastery & Subjugation Checklist</h2>
                        <p class="text-xs text-gray-400">Track Rank (0-30), Parts (BP, Neuro, Chassis, Sys), Helminth Subsume, and Prime Vault status</p>
                    </div>
                </div>

                <div class="flex items-center space-x-2 text-xs">
                    <button onclick="toggleAllFramesMastery(true)" class="px-3 py-1.5 bg-gray-800 hover:bg-gray-700 text-gray-300 rounded border border-gray-700">Mark All Mastered</button>
                    <button onclick="toggleAllFramesMastery(false)" class="px-3 py-1.5 bg-gray-800 hover:bg-gray-700 text-gray-300 rounded border border-gray-700">Clear All</button>
                </div>
            </div>

            <!-- Warframe Cards Grid -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="warframesGrid">
                <!-- Dynamically populated JS -->
            </div>
        </section>

        <!-- WEAPONS TRACKER VIEW -->
        <section id="view-weapons" class="tab-content hidden space-y-6">
            <div class="bg-gray-900/80 p-4 rounded-xl border border-gray-800 flex flex-wrap items-center justify-between gap-4">
                <div class="flex items-center space-x-3">
                    <div class="p-2.5 bg-cyan-950 border border-cyan-500/30 rounded-lg text-lotus-cyan">
                        <i data-lucide="crosshair" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <h2 class="font-orbitron font-bold text-lg text-white">Weapons Armory</h2>
                        <p class="text-xs text-gray-400">Primary, Secondary, Melee, Archwing, Robotic, Kuva/Tenet (Rank 40 / 5 Forma)</p>
                    </div>
                </div>

                <!-- Subtabs for Weapon Types -->
                <div class="flex flex-wrap gap-1 bg-gray-950 p-1 rounded-lg border border-gray-800 text-xs" id="weaponSubtabs">
                    <button onclick="filterWeaponType('all')" class="weapon-subtab active px-3 py-1.5 rounded font-semibold text-orokin-gold bg-gray-800">All</button>
                    <button onclick="filterWeaponType('Primary')" class="weapon-subtab px-3 py-1.5 rounded font-semibold text-gray-400 hover:text-white">Primary</button>
                    <button onclick="filterWeaponType('Secondary')" class="weapon-subtab px-3 py-1.5 rounded font-semibold text-gray-400 hover:text-white">Secondary</button>
                    <button onclick="filterWeaponType('Melee')" class="weapon-subtab px-3 py-1.5 rounded font-semibold text-gray-400 hover:text-white">Melee</button>
                    <button onclick="filterWeaponType('Kuva/Tenet')" class="weapon-subtab px-3 py-1.5 rounded font-semibold text-gray-400 hover:text-white">Kuva/Tenet</button>
                    <button onclick="filterWeaponType('Archwing')" class="weapon-subtab px-3 py-1.5 rounded font-semibold text-gray-400 hover:text-white">Archwing</button>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="weaponsGrid">
                <!-- Dynamically populated JS -->
            </div>
        </section>

        <!-- INVENTORY, RESOURCES & RELICS TRACKER VIEW -->
        <section id="view-inventory" class="tab-content hidden space-y-6">
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <!-- Critical Resources Counter Panel -->
                <div class="bg-gray-900/80 border border-gray-800 rounded-2xl p-5 space-y-4">
                    <div class="flex items-center justify-between pb-3 border-b border-gray-800">
                        <div class="flex items-center space-x-2">
                            <i data-lucide="gem" class="w-5 h-5 text-orokin-gold"></i>
                            <h3 class="font-orbitron font-bold text-base text-white">Rare & Key Crafting Resources</h3>
                        </div>
                        <span class="text-xs text-gray-500">Auto-saved to storage</span>
                    </div>

                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-3" id="resourcesGrid">
                        <!-- Populated JS -->
                    </div>
                </div>

                <!-- Prime Relic Vault Manager -->
                <div class="bg-gray-900/80 border border-gray-800 rounded-2xl p-5 space-y-4">
                    <div class="flex items-center justify-between pb-3 border-b border-gray-800">
                        <div class="flex items-center space-x-2">
                            <i data-lucide="layers" class="w-5 h-5 text-lotus-cyan"></i>
                            <h3 class="font-orbitron font-bold text-base text-white">Prime Relic Vault Inventory</h3>
                        </div>
                        <span class="text-xs text-cyan-400 font-semibold" id="totalRelicsCount">Total: 142 Relics</span>
                    </div>

                    <div class="space-y-3" id="relicsList">
                        <!-- Populated JS -->
                    </div>
                </div>
            </div>
        </section>

        <!-- MODS & ARCANES TRACKER VIEW -->
        <section id="view-mods" class="tab-content hidden space-y-6">
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <!-- Mods Upgrade Tracker (Primed, Galvanized, Archon) -->
                <div class="bg-gray-900/80 border border-gray-800 rounded-2xl p-5 space-y-4">
                    <div class="flex items-center justify-between pb-3 border-b border-gray-800">
                        <div class="flex items-center space-x-2">
                            <i data-lucide="zap" class="w-5 h-5 text-yellow-400"></i>
                            <h3 class="font-orbitron font-bold text-base text-white">Primed & Galvanized Mods (Rank 0-10)</h3>
                        </div>
                        <span class="text-xs text-yellow-400 font-semibold" id="maxModsCount">0 Maxed</span>
                    </div>

                    <div class="space-y-3 max-h-[600px] overflow-y-auto pr-1" id="modsContainer">
                        <!-- Populated JS -->
                    </div>
                </div>

                <!-- Arcanes Tracker (Rank 0-5 / 21 copies) -->
                <div class="bg-gray-900/80 border border-gray-800 rounded-2xl p-5 space-y-4">
                    <div class="flex items-center justify-between pb-3 border-b border-gray-800">
                        <div class="flex items-center space-x-2">
                            <i data-lucide="shield-alert" class="w-5 h-5 text-purple-400"></i>
                            <h3 class="font-orbitron font-bold text-base text-white">Arcanes Collection (Rank 0-5)</h3>
                        </div>
                        <span class="text-xs text-purple-400 font-semibold" id="maxArcanesCount">0 Rank 5</span>
                    </div>

                    <div class="space-y-3 max-h-[600px] overflow-y-auto pr-1" id="arcanesContainer">
                        <!-- Populated JS -->
                    </div>
                </div>
            </div>
        </section>

        <!-- QUESTS & FACTION SYNDICATES TRACKER VIEW -->
        <section id="view-syndicates" class="tab-content hidden space-y-6">
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <!-- Quests Progression Log -->
                <div class="bg-gray-900/80 border border-gray-800 rounded-2xl p-5 space-y-4">
                    <div class="flex items-center justify-between pb-3 border-b border-gray-800">
                        <div class="flex items-center space-x-2">
                            <i data-lucide="scroll" class="w-5 h-5 text-orokin-gold"></i>
                            <h3 class="font-orbitron font-bold text-base text-white">Main Story & Side Quest Codex</h3>
                        </div>
                        <span class="text-xs text-orokin-gold font-semibold" id="questProgressPct">100% Completed</span>
                    </div>

                    <div class="space-y-2.5 max-h-[600px] overflow-y-auto pr-1" id="questsContainer">
                        <!-- Populated JS -->
                    </div>
                </div>

                <!-- Open World & Syndicate Faction Ranks -->
                <div class="bg-gray-900/80 border border-gray-800 rounded-2xl p-5 space-y-4">
                    <div class="flex items-center justify-between pb-3 border-b border-gray-800">
                        <div class="flex items-center space-x-2">
                            <i data-lucide="award" class="w-5 h-5 text-lotus-cyan"></i>
                            <h3 class="font-orbitron font-bold text-base text-white">Open World & Faction Syndicate Ranks</h3>
                        </div>
                        <span class="text-xs text-cyan-400 font-semibold">Max Rank 5</span>
                    </div>

                    <div class="space-y-4 max-h-[600px] overflow-y-auto pr-1" id="syndicatesContainer">
                        <!-- Populated JS -->
                    </div>
                </div>
            </div>
        </section>
    </main>

    <!-- BACKUP & IMPORT/EXPORT DATA MODAL -->
    <div id="backupModal" class="fixed inset-0 z-50 flex items-center justify-center modal-fade hidden p-4">
        <div class="bg-gray-900 border border-orokin-gold/40 rounded-2xl p-6 max-w-lg w-full shadow-2xl space-y-5">
            <div class="flex items-center justify-between border-b border-gray-800 pb-3">
                <div class="flex items-center space-x-2">
                    <i data-lucide="database" class="w-5 h-5 text-orokin-gold"></i>
                    <h3 class="font-orbitron font-bold text-lg text-white">Data Management & Backup</h3>
                </div>
                <button onclick="closeBackupModal()" class="text-gray-400 hover:text-white">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>

            <p class="text-xs text-gray-400">Export your complete Warframe progress to a JSON backup file or import existing data to sync across devices.</p>

            <div class="space-y-3">
                <button onclick="exportDataJSON()" class="w-full py-2.5 px-4 bg-orokin-gold/10 hover:bg-orokin-gold/20 text-orokin-gold border border-orokin-gold/40 rounded-xl font-orbitron font-semibold text-sm flex items-center justify-center space-x-2 transition">
                    <i data-lucide="download" class="w-4 h-4"></i>
                    <span>Export Progress Data (JSON)</span>
                </button>

                <div class="relative">
                    <input type="file" id="importFileInput" accept=".json" onchange="importDataJSON(event)" class="hidden">
                    <button onclick="document.getElementById('importFileInput').click()" class="w-full py-2.5 px-4 bg-cyan-950/50 hover:bg-cyan-900/50 text-lotus-cyan border border-cyan-500/40 rounded-xl font-orbitron font-semibold text-sm flex items-center justify-center space-x-2 transition">
                        <i data-lucide="upload" class="w-4 h-4"></i>
                        <span>Import Backup Data (JSON)</span>
                    </button>
                </div>
            </div>

            <div class="pt-3 border-t border-gray-800 flex justify-between items-center">
                <button onclick="resetAllData()" class="px-3 py-1.5 bg-red-950/60 hover:bg-red-900 text-red-400 border border-red-800 rounded-lg text-xs font-semibold">
                    Reset All Data
                </button>
                <button onclick="closeBackupModal()" class="px-4 py-1.5 bg-gray-800 text-gray-300 rounded-lg text-xs hover:bg-gray-700">
                    Close
                </button>
            </div>
        </div>
    </div>

    <!-- NOTIFICATION TOAST UI -->
    <div id="toastNotification" class="fixed bottom-5 right-5 z-50 bg-gray-900 border border-lotus-cyan text-white px-4 py-3 rounded-xl shadow-2xl transform translate-y-20 opacity-0 transition-all duration-300 flex items-center space-x-3 pointer-events-none">
        <i data-lucide="check-circle" class="w-5 h-5 text-lotus-cyan" id="toastIcon"></i>
        <span class="text-xs sm:text-sm font-medium" id="toastMsg">Progress saved successfully!</span>
    </div>

    <!-- FOOTER -->
    <footer class="bg-gray-950 border-t border-gray-900 py-4 mt-8 text-center text-xs text-gray-600">
        <div class="max-w-7xl mx-auto px-4 flex flex-col sm:flex-row justify-between items-center gap-2">
            <div>
                Warframe Nexus Tracker &copy; <span id="yearDisplay">2026</span> Tenno Companion Tool.
            </div>
            <div class="text-gray-500 flex items-center space-x-2">
                <span>Created for Digital Extremes Warframe Universe</span>
            </div>
        </div>
    </footer>

    <script>
        const DEFAULT_STATE = {
            profile: {
                username: "PrimeTenno",
                lastUpdated: new Date().toISOString()
            },
            warframes: [
                { id: "wf-excalibur", name: "Excalibur", type: "Base", rank: 30, mastered: true, helminth: true, bp: true, neuro: true, chassis: true, sys: true },
                { id: "wf-excalibur-prime", name: "Excalibur Prime", type: "Prime", rank: 30, mastered: true, helminth: false, bp: true, neuro: true, chassis: true, sys: true, vaulted: true },
                { id: "wf-wisp-prime", name: "Wisp Prime", type: "Prime", rank: 30, mastered: true, helminth: false, bp: true, neuro: true, chassis: true, sys: true, vaulted: false },
                { id: "wf-wisp", name: "Wisp", type: "Base", rank: 30, mastered: true, helminth: true, bp: true, neuro: true, chassis: true, sys: true },
                { id: "wf-saryn-prime", name: "Saryn Prime", type: "Prime", rank: 30, mastered: true, helminth: false, bp: true, neuro: true, chassis: true, sys: true, vaulted: true },
                { id: "wf-saryn", name: "Saryn", type: "Base", rank: 30, mastered: true, helminth: true, bp: true, neuro: true, chassis: true, sys: true },
                { id: "wf-mesaprime", name: "Mesa Prime", type: "Prime", rank: 30, mastered: true, helminth: false, bp: true, neuro: true, chassis: true, sys: true, vaulted: true },
                { id: "wf-gauss-prime", name: "Gauss Prime", type: "Prime", rank: 30, mastered: true, helminth: false, bp: true, neuro: true, chassis: true, sys: true, vaulted: false },
                { id: "wf-dante", name: "Dante", type: "Base", rank: 18, mastered: false, helminth: false, bp: true, neuro: true, chassis: false, sys: true },
                { id: "wf-jade", name: "Jade", type: "Base", rank: 0, mastered: false, helminth: false, bp: true, neuro: false, chassis: true, sys: false },
                { id: "wf-cyte09", name: "Cyte-09", type: "Base", rank: 0, mastered: false, helminth: false, bp: false, neuro: false, chassis: false, sys: false }
            ],
            weapons: [
                { id: "wp-soma-prime", name: "Soma Prime", category: "Primary", type: "Rifle", rank: 30, mastered: true, forma: 4, bp: true, partsOwned: 4, totalParts: 4 },
                { id: "wp-phenmor", name: "Phenmor", category: "Primary", type: "Incarnon Rifle", rank: 30, mastered: true, forma: 5, bp: true, partsOwned: 3, totalParts: 3 },
                { id: "wp-nataruk", name: "Nataruk", category: "Primary", type: "Bow", rank: 30, mastered: true, forma: 2, bp: true, partsOwned: 1, totalParts: 1 },
                { id: "wp-laetum", name: "Laetum", category: "Secondary", type: "Incarnon Pistol", rank: 30, mastered: true, forma: 4, bp: true, partsOwned: 3, totalParts: 3 },
                { id: "wp-epitaph", name: "Epitaph", category: "Secondary", type: "Wrist Sidearm", rank: 30, mastered: true, forma: 3, bp: true, partsOwned: 3, totalParts: 3 },
                { id: "wp-kuva-nukor", name: "Kuva Nukor", category: "Kuva/Tenet", type: "Beam Pistol", rank: 40, mastered: true, forma: 5, bp: true, valenceBonus: "60% Heat" },
                { id: "wp-tenet-arca-plasmor", name: "Tenet Arca Plasmor", category: "Kuva/Tenet", type: "Shotgun", rank: 40, mastered: true, forma: 5, bp: true, valenceBonus: "58% Toxin" },
                { id: "wp-glaive-prime", name: "Glaive Prime", category: "Melee", type: "Thrown Melee", rank: 30, mastered: true, forma: 3, bp: true, partsOwned: 3, totalParts: 3, vaulted: true },
                { id: "wp-praedos", name: "Praedos", category: "Melee", type: "Tonfa", rank: 30, mastered: true, forma: 2, bp: true, partsOwned: 3, totalParts: 3 },
                { id: "wp-mausolon", name: "Mausolon", category: "Archwing", type: "Arch-gun", rank: 30, mastered: true, forma: 1, bp: true, partsOwned: 1, totalParts: 1 }
            ],
            starchart: {
                normalNodes: 238,
                spNodes: 228,
                planets: [
                    { name: "Mercury", nodes: 12, cleared: 12, spCleared: 12 },
                    { name: "Venus", nodes: 15, cleared: 15, spCleared: 15 },
                    { name: "Earth", nodes: 18, cleared: 18, spCleared: 18 },
                    { name: "Mars", nodes: 19, cleared: 19, spCleared: 19 },
                    { name: "Phobos", nodes: 11, cleared: 11, spCleared: 11 },
                    { name: "Ceres", nodes: 15, cleared: 15, spCleared: 15 },
                    { name: "Jupiter", nodes: 16, cleared: 16, spCleared: 16 },
                    { name: "Europa", nodes: 14, cleared: 14, spCleared: 14 },
                    { name: "Saturn", nodes: 16, cleared: 16, spCleared: 16 },
                    { name: "Uranus", nodes: 14, cleared: 14, spCleared: 12 },
                    { name: "Neptune", nodes: 14, cleared: 14, spCleared: 14 },
                    { name: "Pluto", nodes: 13, cleared: 13, spCleared: 13 },
                    { name: "Sedna", nodes: 17, cleared: 17, spCleared: 16 },
                    { name: "Eris", nodes: 14, cleared: 14, spCleared: 12 },
                    { name: "Void", nodes: 15, cleared: 15, spCleared: 15 },
                    { name: "Duviri / Zariman", nodes: 10, cleared: 10, spCleared: 10 },
                    { name: "Albrecht's Laboratories", nodes: 9, cleared: 9, spCleared: 9 }
                ]
            },
            resources: [
                { id: "res-argon", name: "Argon Crystal", qty: 8, icon: "gem", rare: true },
                { id: "res-nitain", name: "Nitain Extract", qty: 42, icon: "flask-conical", rare: true },
                { id: "res-orokin-cell", name: "Orokin Cell", qty: 184, icon: "battery-charging", rare: true },
                { id: "res-forma", name: "Built Forma", qty: 14, icon: "shapes", rare: false },
                { id: "res-forma-bp", name: "Forma Blueprint", qty: 68, icon: "file-code", rare: false },
                { id: "res-catalyst", name: "Orokin Catalyst", qty: 5, icon: "zap", rare: true },
                { id: "res-reactor", name: "Orokin Reactor", qty: 3, icon: "zap-off", rare: true },
                { id: "res-umbra-forma", name: "Umbra Forma", qty: 2, icon: "shield", rare: true }
            ],
            relics: [
                { era: "Lith", radiant: 18, intact: 45 },
                { era: "Meso", radiant: 12, intact: 38 },
                { era: "Neo", radiant: 24, intact: 52 },
                { era: "Axi", radiant: 15, intact: 29 },
                { era: "Requiem", radiant: 8, intact: 16 }
            ],
            mods: [
                { id: "mod-primed-continuity", name: "Primed Continuity", type: "Primed", rank: 10, maxRank: 10 },
                { id: "mod-primed-flow", name: "Primed Flow", type: "Primed", rank: 10, maxRank: 10 },
                { id: "mod-galv-chamber", name: "Galvanized Chamber", type: "Galvanized", rank: 10, maxRank: 10 },
                { id: "mod-galv-aptitude", name: "Galvanized Aptitude", type: "Galvanized", rank: 9, maxRank: 10 },
                { id: "mod-archon-vitality", name: "Archon Vitality", type: "Archon", rank: 10, maxRank: 10 }
            ],
            arcanes: [
                { id: "arc-energize", name: "Arcane Energize", rank: 5, copies: 21, maxRank: 5 },
                { id: "arc-grace", name: "Arcane Grace", rank: 5, copies: 21, maxRank: 5 },
                { id: "arc-aegis", name: "Arcane Aegis", rank: 4, copies: 15, maxRank: 5 },
                { id: "arc-reaper", name: "Molt Augmented", rank: 5, copies: 21, maxRank: 5 }
            ],
            quests: [
                { id: "q-vor", name: "Vor's Prize", main: true, completed: true },
                { id: "q-second-dream", name: "The Second Dream", main: true, completed: true },
                { id: "q-war-within", name: "The War Within", main: true, completed: true },
                { id: "q-chains-harrow", name: "Chains of Harrow", main: true, completed: true },
                { id: "q-sacrifice", name: "The Sacrifice", main: true, completed: true },
                { id: "q-new-war", name: "The New War", main: true, completed: true },
                { id: "q-whispers", name: "Whispers in the Walls", main: true, completed: true },
                { id: "q-lotus-eaters", name: "The Lotus Eaters", main: true, completed: true },
                { id: "q-wf-1999", name: "Warframe 1999", main: true, completed: true }
            ],
            syndicates: [
                { id: "syn-ostron", name: "Ostron (Cetus)", rank: 5, maxRank: 5 },
                { id: "syn-solaris", name: "Solaris United (Fortuna)", rank: 5, maxRank: 5 },
                { id: "syn-entrati", name: "Entrati (Deimos)", rank: 5, maxRank: 5 },
                { id: "syn-holdfasts", name: "The Holdfasts (Zariman)", rank: 5, maxRank: 5 },
                { id: "syn-cavia", name: "Cavia (Sanctum Anatomica)", rank: 5, maxRank: 5 },
                { id: "syn-hex", name: "The Hex (Hollvania)", rank: 4, maxRank: 5 }
            ]
        };

        // Active Application State in Memory
        let appState = JSON.parse(JSON.stringify(DEFAULT_STATE));
        let activeFilter = 'all';
        let searchQuery = '';
        let currentWeaponType = 'all';

        window.onload = function() {
            loadFromStorage();
            initializeIcons();
            calculateMasteryXP();
            renderAllViews();
            setupSearchAndFilterListeners();
            document.getElementById('yearDisplay').innerText = new Date().getFullYear();
        };

        function saveToStorage() {
            appState.profile.lastUpdated = new Date().toISOString();
            localStorage.setItem('WF_NEXUS_TRACKER_DATA_2026', JSON.stringify(appState));
            calculateMasteryXP();
            showToast("Progress Saved Automatically!");
        }

        function loadFromStorage() {
            const saved = localStorage.getItem('WF_NEXUS_TRACKER_DATA_2026');
            if (saved) {
                try {
                    appState = JSON.parse(saved);
                } catch (e) {
                    console.error("Failed to load local storage data, reverting to default", e);
                }
            }
        }

        function initializeIcons() {
            if (window.lucide) {
                lucide.createIcons();
            }
        }

        function calculateMasteryXP() {
            let totalXp = 0;

            // Frames: 6,000 XP per max rank frame
            appState.warframes.forEach(wf => {
                if (wf.mastered) totalXp += 6000;
                else if (wf.rank > 0) totalXp += wf.rank * 200;
            });

            // Weapons: 3,000 XP (Normal Rank 30) / 4,000 XP (Kuva/Tenet Rank 40)
            appState.weapons.forEach(wp => {
                if (wp.mastered) {
                    totalXp += wp.category === 'Kuva/Tenet' ? 4000 : 3000;
                } else if (wp.rank > 0) {
                    totalXp += wp.rank * 100;
                }
            });

            // Star Chart Nodes: ~60 XP per node average
            let totalNodes = 0;
            appState.starchart.planets.forEach(p => totalNodes += (p.cleared + p.spCleared));
            totalXp += totalNodes * 62;

            // Map XP to Mastery Rank
            let mr = 0;
            let requiredXpForNext = 2500;
            let cumulativeXp = 0;

            // Mastery Rank formula simulation
            while (totalXp >= cumulativeXp + (mr < 30 ? (mr + 1) * 5000 : 147500)) {
                if (mr < 30) {
                    cumulativeXp += (mr + 1) * 5000;
                } else {
                    cumulativeXp += 147500; // Legendary ranks take fixed 147.5k XP
                }
                mr++;
            }

            let nextRankNeed = mr < 30 ? (mr + 1) * 5000 : 147500;
            let xpIntoCurrentRank = totalXp - cumulativeXp;
            let remainingForNext = nextRankNeed - xpIntoCurrentRank;

            // Update Header Display UI
            document.getElementById('mrRankBadge').innerText = mr > 30 ? `L${mr - 30}` : mr;
            document.getElementById('mrTitleBadge').innerText = mr >= 30 ? `Legendary ${mr - 30 || ''}` : `Rank ${mr}`;
            document.getElementById('mrXpDisplay').innerText = totalXp.toLocaleString();
            document.getElementById('mrNextXpDisplay').innerText = remainingForNext > 0 ? remainingForNext.toLocaleString() : 'MAX';
            document.getElementById('profileNameDisplay').innerText = appState.profile.username || "Tenno";
            
            // Standing & Focus caps based on MR
            let effMr = Math.min(mr, 30);
            document.getElementById('dailyStandingCap').innerText = ((effMr + 1) * 1000).toLocaleString();
            document.getElementById('dailyFocusCap').innerText = (250000 + (effMr * 5000)).toLocaleString();
            document.getElementById('standingMrRef').innerText = effMr;

            // Overall completion ratio calculation
            let totalCodexItems = appState.warframes.length + appState.weapons.length;
            let masteredCodexItems = appState.warframes.filter(w=>w.mastered).length + appState.weapons.filter(w=>w.mastered).length;
            let pct = totalCodexItems > 0 ? ((masteredCodexItems / totalCodexItems) * 100).toFixed(1) : "0.0";
            document.getElementById('totalCompletionPct').innerText = `${pct}%`;
        }

        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            const targetView = document.getElementById(`view-${tabId}`);
            if (targetView) targetView.classList.remove('hidden');

            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('active', 'bg-gradient-to-r', 'from-orokin-gold/20', 'to-transparent', 'text-orokin-gold', 'border-orokin-gold/40');
                btn.classList.add('text-gray-400', 'border-transparent');
            });

            const activeBtn = event ? event.currentTarget : document.querySelector(`.tab-btn[onclick*="${tabId}"]`);
            if (activeBtn) {
                activeBtn.classList.add('active', 'bg-gradient-to-r', 'from-orokin-gold/20', 'to-transparent', 'text-orokin-gold', 'border-orokin-gold/40');
                activeBtn.classList.remove('text-gray-400', 'border-transparent');
            }
            initializeIcons();
        }

        function setupSearchAndFilterListeners() {
            const searchInput = document.getElementById('globalSearchInput');
            const clearBtn = document.getElementById('clearSearchBtn');

            searchInput.addEventListener('input', (e) => {
                searchQuery = e.target.value.toLowerCase().trim();
                clearBtn.classList.toggle('hidden', searchQuery === '');
                renderAllViews();
            });

            document.querySelectorAll('.filter-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    document.querySelectorAll('.filter-btn').forEach(b => {
                        b.classList.remove('active', 'bg-gray-800', 'text-gray-300', 'border-gray-700');
                        b.classList.add('bg-gray-900', 'text-gray-400', 'border-gray-800');
                    });
                    e.currentTarget.classList.add('active', 'bg-gray-800', 'text-gray-300', 'border-gray-700');
                    activeFilter = e.currentTarget.getAttribute('data-filter');
                    renderAllViews();
                });
            });
        }

        function clearGlobalSearch() {
            document.getElementById('globalSearchInput').value = '';
            searchQuery = '';
            document.getElementById('clearSearchBtn').classList.add('hidden');
            renderAllViews();
        }

        function renderDashboard() {
            // Helminth stats
            let helminthCount = appState.warframes.filter(w => w.helminth).length;
            let helminthTotal = appState.warframes.filter(w => w.type === 'Base').length;
            document.getElementById('helminthCount').innerText = helminthCount;
            document.getElementById('helminthTotal').innerText = helminthTotal;
            document.getElementById('helminthPct').innerText = helminthTotal > 0 ? `${((helminthCount / helminthTotal) * 100).toFixed(1)}% Absorbed` : '0%';

            // Steel Path stats
            let totalSpCleared = appState.starchart.planets.reduce((acc, p) => acc + p.spCleared, 0);
            document.getElementById('spNodesCleared').innerText = totalSpCleared;
            document.getElementById('spNodesPct').innerText = `${((totalSpCleared / 238) * 100).toFixed(1)}% Cleared`;

            // Dashboard Category Grid
            const grid = document.getElementById('dashboardProgressGrid');
            const categories = [
                {
                    title: "Warframes",
                    icon: "user-check",
                    color: "orokin-gold",
                    total: appState.warframes.length,
                    mastered: appState.warframes.filter(w => w.mastered).length
                },
                {
                    title: "Primary Weapons",
                    icon: "crosshair",
                    color: "cyan-400",
                    total: appState.weapons.filter(w => w.category === 'Primary').length,
                    mastered: appState.weapons.filter(w => w.category === 'Primary' && w.mastered).length
                },
                {
                    title: "Secondary & Kuva/Tenet",
                    icon: "zap",
                    color: "purple-400",
                    total: appState.weapons.filter(w => w.category === 'Secondary' || w.category === 'Kuva/Tenet').length,
                    mastered: appState.weapons.filter(w => (w.category === 'Secondary' || w.category === 'Kuva/Tenet') && w.mastered).length
                },
                {
                    title: "Melee Weapons",
                    icon: "sword",
                    color: "red-400",
                    total: appState.weapons.filter(w => w.category === 'Melee').length,
                    mastered: appState.weapons.filter(w => w.category === 'Melee' && w.mastered).length
                },
                {
                    title: "Quests Log",
                    icon: "scroll",
                    color: "amber-400",
                    total: appState.quests.length,
                    mastered: appState.quests.filter(q => q.completed).length
                },
                {
                    title: "Faction Syndicates",
                    icon: "award",
                    color: "emerald-400",
                    total: appState.syndicates.length,
                    mastered: appState.syndicates.filter(s => s.rank >= s.maxRank).length
                }
            ];

            grid.innerHTML = categories.map(c => {
                let pct = c.total > 0 ? Math.round((c.mastered / c.total) * 100) : 0;
                return `
                    <div class="bg-gray-950/80 p-4 rounded-xl border border-gray-800 flex flex-col justify-between space-y-3">
                        <div class="flex items-center justify-between">
                            <div class="flex items-center space-x-2.5">
                                <i data-lucide="${c.icon}" class="w-5 h-5 text-${c.color}"></i>
                                <span class="font-orbitron font-semibold text-sm text-gray-200">${c.title}</span>
                            </div>
                            <span class="font-orbitron font-bold text-xs text-gray-400">${c.mastered}/${c.total}</span>
                        </div>
                        <div>
                            <div class="w-full bg-gray-800 rounded-full h-2 overflow-hidden">
                                <div class="bg-${c.color} h-2 rounded-full transition-all duration-500" style="width: ${pct}%"></div>
                            </div>
                            <div class="text-right text-[11px] text-gray-500 mt-1 font-medium">${pct}% Mastered</div>
                        </div>
                    </div>
                `;
            }).join('');

            // Craftable Items Panel
            const craftableList = document.getElementById('craftableListContainer');
            let craftableFrames = appState.warframes.filter(w => !w.mastered && w.bp && w.neuro && w.chassis && w.sys);
            let craftableWeapons = appState.weapons.filter(w => !w.mastered && w.bp && (w.partsOwned || 0) >= (w.totalParts || 1));
            let totalCraftable = craftableFrames.length + craftableWeapons.length;

            document.getElementById('craftableCountBadge').innerText = `${totalCraftable} Items Ready to Craft`;

            if (totalCraftable === 0) {
                craftableList.innerHTML = `<p class="text-xs text-gray-500 italic py-2">No unmastered items with complete sets ready in foundry.</p>`;
            } else {
                let html = '';
                craftableFrames.forEach(f => {
                    html += `
                        <div class="flex items-center justify-between p-2.5 bg-gray-950 rounded-lg border border-cyan-900/40 text-xs">
                            <div class="flex items-center space-x-2">
                                <i data-lucide="shield" class="w-4 h-4 text-cyan-400"></i>
                                <span class="font-semibold text-gray-200">${f.name}</span>
                                <span class="text-[10px] px-1.5 py-0.2 bg-cyan-950 text-cyan-400 rounded">Warframe</span>
                            </div>
                            <button onclick="markFrameMastered('${f.id}')" class="px-2.5 py-1 bg-cyan-600 hover:bg-cyan-500 text-gray-950 font-bold rounded text-[11px] transition">
                                Mark Mastered
                            </button>
                        </div>
                    `;
                });
                craftableWeapons.forEach(w => {
                    html += `
                        <div class="flex items-center justify-between p-2.5 bg-gray-950 rounded-lg border border-orokin-gold/30 text-xs">
                            <div class="flex items-center space-x-2">
                                <i data-lucide="crosshair" class="w-4 h-4 text-orokin-gold"></i>
                                <span class="font-semibold text-gray-200">${w.name}</span>
                                <span class="text-[10px] px-1.5 py-0.2 bg-yellow-950 text-orokin-gold rounded">${w.category}</span>
                            </div>
                            <button onclick="markWeaponMastered('${w.id}')" class="px-2.5 py-1 bg-orokin-gold hover:bg-yellow-400 text-gray-950 font-bold rounded text-[11px] transition">
                                Mark Mastered
                            </button>
                        </div>
                    `;
                });
                craftableList.innerHTML = html;
            }

            // Recommended MR Targets
            const recList = document.getElementById('recommendationsList');
            let recs = appState.warframes.filter(w => !w.mastered).slice(0, 3);
            recList.innerHTML = recs.map(r => `
                <div class="p-3 bg-gray-950 rounded-xl border border-gray-800 flex justify-between items-center text-xs">
                    <div>
                        <div class="font-semibold text-gray-200">${r.name}</div>
                        <div class="text-[11px] text-gray-500">Yields <strong class="text-orokin-gold">6,000 MR XP</strong> on Rank 30</div>
                    </div>
                    <button onclick="switchTab('warframes')" class="text-xs text-lotus-cyan hover:underline">View in Codex</button>
                </div>
            `).join('');
        }

        function renderStarChart() {
            const grid = document.getElementById('planetsGrid');
            grid.innerHTML = appState.starchart.planets.map((p, idx) => {
                let isNormalDone = p.cleared >= p.nodes;
                let isSpDone = p.spCleared >= p.nodes;

                return `
                    <div class="bg-gray-950 border border-gray-800 rounded-xl p-4 space-y-3">
                        <div class="flex justify-between items-center">
                            <h4 class="font-orbitron font-bold text-base text-white flex items-center gap-2">
                                <i data-lucide="disc" class="w-4 h-4 text-lotus-cyan"></i>
                                ${p.name}
                            </h4>
                            <span class="text-xs text-gray-400">${p.nodes} Nodes</span>
                        </div>

                        <!-- Normal Chart Progress -->
                        <div class="space-y-1">
                            <div class="flex justify-between text-xs">
                                <span class="text-gray-400">Normal Map</span>
                                <span class="${isNormalDone ? 'text-lotus-cyan font-bold' : 'text-gray-300'}">${p.cleared} / ${p.nodes}</span>
                            </div>
                            <input type="range" min="0" max="${p.nodes}" value="${p.cleared}" 
                                   onchange="updatePlanetCleared(${idx}, 'cleared', this.value)"
                                   class="w-full accent-cyan-400 bg-gray-800 h-1.5 rounded cursor-pointer">
                        </div>

                        <!-- Steel Path Progress -->
                        <div class="space-y-1">
                            <div class="flex justify-between text-xs">
                                <span class="text-red-400 font-semibold">Steel Path</span>
                                <span class="${isSpDone ? 'text-red-400 font-bold' : 'text-gray-300'}">${p.spCleared} / ${p.nodes}</span>
                            </div>
                            <input type="range" min="0" max="${p.nodes}" value="${p.spCleared}" 
                                   onchange="updatePlanetCleared(${idx}, 'spCleared', this.value)"
                                   class="w-full accent-red-500 bg-gray-800 h-1.5 rounded cursor-pointer">
                        </div>
                    </div>
                `;
            }).join('');
        }

        function updatePlanetCleared(planetIdx, field, val) {
            appState.starchart.planets[planetIdx][field] = parseInt(val, 10);
            saveToStorage();
            renderStarChart();
        }

        function renderWarframes() {
            const grid = document.getElementById('warframesGrid');
            
            let items = appState.warframes.filter(wf => {
                // Search filter
                if (searchQuery && !wf.name.toLowerCase().includes(searchQuery)) return false;
                // Active Category Filter
                if (activeFilter === 'mastered') return wf.mastered;
                if (activeFilter === 'unmastered') return !wf.mastered;
                if (activeFilter === 'prime') return wf.type === 'Prime';
                if (activeFilter === 'vaulted') return wf.vaulted;
                if (activeFilter === 'craftable') return !wf.mastered && wf.bp && wf.neuro && wf.chassis && wf.sys;
                return true;
            });

            if (items.length === 0) {
                grid.innerHTML = `<div class="col-span-full py-12 text-center text-gray-500">No Warframes matched your filter search criteria.</div>`;
                return;
            }

            grid.innerHTML = items.map(wf => `
                <div class="bg-gray-950 border ${wf.mastered ? 'border-orokin-gold/40' : 'border-gray-800'} rounded-xl p-4 flex flex-col justify-between space-y-3 relative overflow-hidden transition hover:border-lotus-cyan/50">
                    
                    ${wf.vaulted ? '<span class="absolute top-2 right-2 text-[10px] px-2 py-0.5 bg-red-950 text-red-400 border border-red-800 rounded">Vaulted</span>' : ''}
                    
                    <div>
                        <div class="flex items-center space-x-2">
                            <h3 class="font-orbitron font-bold text-base text-white">${wf.name}</h3>
                            <span class="text-[10px] px-2 py-0.5 rounded font-semibold ${wf.type === 'Prime' ? 'bg-orokin-gold/20 text-orokin-gold border border-orokin-gold/30' : 'bg-gray-800 text-gray-400'}">${wf.type}</span>
                        </div>
                        <div class="text-xs text-gray-400 mt-0.5">Rank ${wf.rank} / 30</div>
                    </div>

                    <!-- Blueprint / Component Checklist -->
                    <div class="grid grid-cols-4 gap-1 text-[11px] font-semibold text-center">
                        <button onclick="toggleFramePart('${wf.id}', 'bp')" class="py-1 rounded border ${wf.bp ? 'bg-cyan-950 text-cyan-400 border-cyan-500/40' : 'bg-gray-900 text-gray-600 border-gray-800'}">BP</button>
                        <button onclick="toggleFramePart('${wf.id}', 'neuro')" class="py-1 rounded border ${wf.neuro ? 'bg-cyan-950 text-cyan-400 border-cyan-500/40' : 'bg-gray-900 text-gray-600 border-gray-800'}">Neuro</button>
                        <button onclick="toggleFramePart('${wf.id}', 'chassis')" class="py-1 rounded border ${wf.chassis ? 'bg-cyan-950 text-cyan-400 border-cyan-500/40' : 'bg-gray-900 text-gray-600 border-gray-800'}">Chassis</button>
                        <button onclick="toggleFramePart('${wf.id}', 'sys')" class="py-1 rounded border ${wf.sys ? 'bg-cyan-950 text-cyan-400 border-cyan-500/40' : 'bg-gray-900 text-gray-600 border-gray-800'}">Systems</button>
                    </div>

                    <!-- Helminth & Mastery Controls -->
                    <div class="flex items-center justify-between pt-2 border-t border-gray-900 text-xs">
                        ${wf.type === 'Base' ? `
                            <button onclick="toggleHelminth('${wf.id}')" class="flex items-center space-x-1.5 px-2.5 py-1 rounded border ${wf.helminth ? 'bg-amber-950/80 text-amber-400 border-amber-600/40' : 'bg-gray-900 text-gray-500 border-gray-800'}">
                                <i data-lucide="dna" class="w-3.5 h-3.5"></i>
                                <span>${wf.helminth ? 'Subsumed' : 'Helminth'}</span>
                            </button>
                        ` : '<div></div>'}

                        <button onclick="toggleFrameMastery('${wf.id}')" class="flex items-center space-x-1.5 px-3 py-1 rounded font-orbitron font-semibold border ${wf.mastered ? 'bg-orokin-gold text-gray-950 border-orokin-lightgold shadow-md shadow-orokin-gold/20' : 'bg-gray-900 text-gray-400 border-gray-800 hover:text-white'}">
                            <i data-lucide="check" class="w-3.5 h-3.5"></i>
                            <span>${wf.mastered ? 'Mastered' : 'Mark Mastered'}</span>
                        </button>
                    </div>
                </div>
            `).join('');
        }

        function toggleFramePart(frameId, part) {
            let wf = appState.warframes.find(w => w.id === frameId);
            if (wf) {
                wf[part] = !wf[part];
                saveToStorage();
                renderWarframes();
            }
        }

        function toggleHelminth(frameId) {
            let wf = appState.warframes.find(w => w.id === frameId);
            if (wf) {
                wf.helminth = !wf.helminth;
                saveToStorage();
                renderWarframes();
            }
        }

        function toggleFrameMastery(frameId) {
            let wf = appState.warframes.find(w => w.id === frameId);
            if (wf) {
                wf.mastered = !wf.mastered;
                wf.rank = wf.mastered ? 30 : 0;
                saveToStorage();
                renderWarframes();
            }
        }

        function markFrameMastered(frameId) {
            let wf = appState.warframes.find(w => w.id === frameId);
            if (wf) {
                wf.mastered = true;
                wf.rank = 30;
                saveToStorage();
                renderAllViews();
            }
        }

        function toggleAllFramesMastery(masteredState) {
            appState.warframes.forEach(w => {
                w.mastered = masteredState;
                w.rank = masteredState ? 30 : 0;
            });
            saveToStorage();
            renderWarframes();
        }

        function renderWeapons() {
            const grid = document.getElementById('weaponsGrid');

            let items = appState.weapons.filter(wp => {
                if (searchQuery && !wp.name.toLowerCase().includes(searchQuery)) return false;
                if (currentWeaponType !== 'all' && wp.category !== currentWeaponType) return false;
                if (activeFilter === 'mastered') return wp.mastered;
                if (activeFilter === 'unmastered') return !wp.mastered;
                if (activeFilter === 'vaulted') return wp.vaulted;
                return true;
            });

            if (items.length === 0) {
                grid.innerHTML = `<div class="col-span-full py-12 text-center text-gray-500">No weapons found matching filter selection.</div>`;
                return;
            }

            grid.innerHTML = items.map(wp => `
                <div class="bg-gray-950 border ${wp.mastered ? 'border-orokin-gold/40' : 'border-gray-800'} rounded-xl p-4 flex flex-col justify-between space-y-3">
                    <div class="flex justify-between items-start">
                        <div>
                            <h3 class="font-orbitron font-bold text-base text-white">${wp.name}</h3>
                            <div class="text-xs text-gray-400 mt-0.5">${wp.type} • ${wp.category}</div>
                        </div>
                        <span class="text-xs font-semibold px-2 py-0.5 rounded bg-gray-900 border border-gray-800 text-orokin-gold">
                            ${wp.forma || 0} Forma
                        </span>
                    </div>

                    ${wp.valenceBonus ? `<div class="text-xs text-purple-400 font-semibold bg-purple-950/40 p-1.5 rounded border border-purple-800/40">Valence: ${wp.valenceBonus}</div>` : ''}

                    <div class="flex items-center justify-between pt-2 border-t border-gray-900">
                        <div class="flex items-center space-x-2 text-xs">
                            <span class="text-gray-500">Forma Count:</span>
                            <button onclick="adjustForma('${wp.id}', -1)" class="w-6 h-6 rounded bg-gray-900 hover:bg-gray-800 text-gray-300 font-bold flex items-center justify-center">-</button>
                            <span class="font-bold text-white">${wp.forma || 0}</span>
                            <button onclick="adjustForma('${wp.id}', 1)" class="w-6 h-6 rounded bg-gray-900 hover:bg-gray-800 text-gray-300 font-bold flex items-center justify-center">+</button>
                        </div>

                        <button onclick="toggleWeaponMastery('${wp.id}')" class="flex items-center space-x-1.5 px-3 py-1 rounded font-orbitron font-semibold text-xs border ${wp.mastered ? 'bg-orokin-gold text-gray-950 border-orokin-lightgold shadow-md shadow-orokin-gold/20' : 'bg-gray-900 text-gray-400 border-gray-800 hover:text-white'}">
                            <i data-lucide="check" class="w-3.5 h-3.5"></i>
                            <span>${wp.mastered ? 'Mastered' : 'Mark Mastered'}</span>
                        </button>
                    </div>
                </div>
            `).join('');
        }

        function filterWeaponType(type) {
            currentWeaponType = type;
            document.querySelectorAll('.weapon-subtab').forEach(btn => {
                btn.classList.remove('active', 'text-orokin-gold', 'bg-gray-800');
                btn.classList.add('text-gray-400');
            });
            event.currentTarget.classList.add('active', 'text-orokin-gold', 'bg-gray-800');
            event.currentTarget.classList.remove('text-gray-400');
            renderWeapons();
        }

        function toggleWeaponMastery(wpId) {
            let wp = appState.weapons.find(w => w.id === wpId);
            if (wp) {
                wp.mastered = !wp.mastered;
                wp.rank = wp.mastered ? (wp.category === 'Kuva/Tenet' ? 40 : 30) : 0;
                saveToStorage();
                renderWeapons();
            }
        }

        function markWeaponMastered(wpId) {
            let wp = appState.weapons.find(w => w.id === wpId);
            if (wp) {
                wp.mastered = true;
                wp.rank = wp.category === 'Kuva/Tenet' ? 40 : 30;
                saveToStorage();
                renderAllViews();
            }
        }

        function adjustForma(wpId, delta) {
            let wp = appState.weapons.find(w => w.id === wpId);
            if (wp) {
                wp.forma = Math.max(0, (wp.forma || 0) + delta);
                saveToStorage();
                renderWeapons();
            }
        }

        function renderInventory() {
            // Rare Resources Grid
            const resGrid = document.getElementById('resourcesGrid');
            resGrid.innerHTML = appState.resources.map((r, idx) => `
                <div class="bg-gray-950 p-3 rounded-xl border border-gray-800 flex items-center justify-between">
                    <div class="flex items-center space-x-2.5">
                        <i data-lucide="${r.icon || 'box'}" class="w-4 h-4 text-orokin-gold"></i>
                        <span class="text-xs font-semibold text-gray-200">${r.name}</span>
                    </div>

                    <div class="flex items-center space-x-1.5">
                        <button onclick="updateResourceQty(${idx}, -1)" class="w-6 h-6 rounded bg-gray-900 text-gray-400 hover:text-white flex items-center justify-center font-bold text-xs">-</button>
                        <input type="number" value="${r.qty}" onchange="setResourceQty(${idx}, this.value)" class="w-12 bg-gray-900 border border-gray-800 text-center text-xs font-bold text-orokin-gold rounded py-0.5 focus:outline-none">
                        <button onclick="updateResourceQty(${idx}, 1)" class="w-6 h-6 rounded bg-gray-900 text-gray-400 hover:text-white flex items-center justify-center font-bold text-xs">+</button>
                    </div>
                </div>
            `).join('');

            // Relics Vault List
            const relicList = document.getElementById('relicsList');
            let totalRelics = appState.relics.reduce((acc, r) => acc + r.radiant + r.intact, 0);
            document.getElementById('totalRelicsCount').innerText = `Total: ${totalRelics} Relics`;

            relicList.innerHTML = appState.relics.map((rel, idx) => `
                <div class="bg-gray-950 p-3.5 rounded-xl border border-gray-800 space-y-2">
                    <div class="flex justify-between items-center text-xs">
                        <span class="font-orbitron font-bold text-lotus-cyan">${rel.era} Era Relics</span>
                        <span class="text-gray-400 font-semibold">${rel.radiant + rel.intact} Total</span>
                    </div>

                    <div class="grid grid-cols-2 gap-3 text-xs">
                        <div class="bg-gray-900/80 p-2 rounded-lg flex justify-between items-center">
                            <span class="text-gray-400 text-[11px]">Intact</span>
                            <input type="number" value="${rel.intact}" onchange="updateRelicCount(${idx}, 'intact', this.value)" class="w-12 bg-gray-950 border border-gray-800 text-center font-bold text-gray-200 rounded py-0.5">
                        </div>
                        <div class="bg-amber-950/30 p-2 rounded-lg border border-amber-900/40 flex justify-between items-center">
                            <span class="text-amber-400 font-semibold text-[11px]">Radiant</span>
                            <input type="number" value="${rel.radiant}" onchange="updateRelicCount(${idx}, 'radiant', this.value)" class="w-12 bg-gray-950 border border-amber-800/60 text-center font-bold text-orokin-gold rounded py-0.5">
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function updateResourceQty(idx, delta) {
            appState.resources[idx].qty = Math.max(0, appState.resources[idx].qty + delta);
            saveToStorage();
            renderInventory();
        }

        function setResourceQty(idx, val) {
            appState.resources[idx].qty = Math.max(0, parseInt(val, 10) || 0);
            saveToStorage();
            renderInventory();
        }

        function updateRelicCount(idx, field, val) {
            appState.relics[idx][field] = Math.max(0, parseInt(val, 10) || 0);
            saveToStorage();
            renderInventory();
        }

        function renderModsAndArcanes() {
            // Mods Container
            const modsCont = document.getElementById('modsContainer');
            let maxMods = appState.mods.filter(m => m.rank === m.maxRank).length;
            document.getElementById('maxModsCount').innerText = `${maxMods} / ${appState.mods.length} Maxed`;

            modsCont.innerHTML = appState.mods.map((mod, idx) => `
                <div class="bg-gray-950 p-3.5 rounded-xl border border-yellow-900/30 space-y-2">
                    <div class="flex justify-between items-center text-xs">
                        <div>
                            <span class="font-semibold text-white">${mod.name}</span>
                            <span class="text-[10px] ml-2 px-1.5 py-0.2 bg-yellow-950 text-yellow-400 rounded">${mod.type}</span>
                        </div>
                        <span class="font-orbitron font-bold text-yellow-400">Rank ${mod.rank} / ${mod.maxRank}</span>
                    </div>
                    <input type="range" min="0" max="${mod.maxRank}" value="${mod.rank}" 
                           onchange="updateModRank(${idx}, this.value)" 
                           class="w-full accent-yellow-400 bg-gray-800 h-1.5 rounded cursor-pointer">
                </div>
            `).join('');

            // Arcanes Container
            const arcanesCont = document.getElementById('arcanesContainer');
            let maxArc = appState.arcanes.filter(a => a.rank === a.maxRank).length;
            document.getElementById('maxArcanesCount').innerText = `${maxArc} / ${appState.arcanes.length} Rank 5`;

            arcanesCont.innerHTML = appState.arcanes.map((arc, idx) => `
                <div class="bg-gray-950 p-3.5 rounded-xl border border-purple-900/30 space-y-2">
                    <div class="flex justify-between items-center text-xs">
                        <span class="font-semibold text-white">${arc.name}</span>
                        <span class="font-orbitron font-bold text-purple-400">Rank ${arc.rank} / ${arc.maxRank}</span>
                    </div>
                    <div class="flex items-center justify-between text-xs pt-1">
                        <span class="text-gray-500">Copies Collected: ${arc.copies} / 21</span>
                        <div class="flex items-center space-x-1.5">
                            <button onclick="updateArcaneCopies(${idx}, -1)" class="w-6 h-6 rounded bg-gray-900 text-gray-400 hover:text-white flex items-center justify-center font-bold">-</button>
                            <span class="font-bold text-purple-300 w-6 text-center">${arc.rank}</span>
                            <button onclick="updateArcaneCopies(${idx}, 1)" class="w-6 h-6 rounded bg-gray-900 text-gray-400 hover:text-white flex items-center justify-center font-bold">+</button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function updateModRank(idx, val) {
            appState.mods[idx].rank = parseInt(val, 10);
            saveToStorage();
            renderModsAndArcanes();
        }

        function updateArcaneCopies(idx, delta) {
            let arc = appState.arcanes[idx];
            arc.copies = Math.max(0, Math.min(21, arc.copies + delta));
            
            // Map copies to Arcane Rank (1, 3, 6, 10, 15, 21)
            if (arc.copies >= 21) arc.rank = 5;
            else if (arc.copies >= 15) arc.rank = 4;
            else if (arc.copies >= 10) arc.rank = 3;
            else if (arc.copies >= 6) arc.rank = 2;
            else if (arc.copies >= 3) arc.rank = 1;
            else arc.rank = 0;

            saveToStorage();
            renderModsAndArcanes();
        }

        function renderQuestsAndSyndicates() {
            // Quests List
            const questsCont = document.getElementById('questsContainer');
            let completedQuests = appState.quests.filter(q => q.completed).length;
            let qPct = Math.round((completedQuests / appState.quests.length) * 100);
            document.getElementById('questProgressPct').innerText = `${qPct}% Completed`;

            questsCont.innerHTML = appState.quests.map((q, idx) => `
                <div class="bg-gray-950 p-3 rounded-xl border border-gray-800 flex items-center justify-between text-xs">
                    <div class="flex items-center space-x-2.5">
                        <i data-lucide="${q.completed ? 'check-circle' : 'circle'}" class="w-4 h-4 ${q.completed ? 'text-orokin-gold' : 'text-gray-600'}"></i>
                        <span class="${q.completed ? 'text-gray-200 line-through' : 'text-white font-medium'}">${q.name}</span>
                    </div>
                    <button onclick="toggleQuest(${idx})" class="px-3 py-1 rounded font-semibold text-[11px] ${q.completed ? 'bg-orokin-gold/20 text-orokin-gold border border-orokin-gold/30' : 'bg-gray-900 text-gray-400 border border-gray-800'}">
                        ${q.completed ? 'Completed' : 'Mark Done'}
                    </button>
                </div>
            `).join('');

            // Faction Syndicates List
            const synCont = document.getElementById('syndicatesContainer');
            synCont.innerHTML = appState.syndicates.map((s, idx) => `
                <div class="bg-gray-950 p-4 rounded-xl border border-gray-800 space-y-2">
                    <div class="flex justify-between items-center text-xs">
                        <span class="font-orbitron font-bold text-lotus-cyan">${s.name}</span>
                        <span class="font-bold ${s.rank === s.maxRank ? 'text-orokin-gold' : 'text-gray-400'}">Rank ${s.rank} / ${s.maxRank}</span>
                    </div>
                    <input type="range" min="0" max="${s.maxRank}" value="${s.rank}" 
                           onchange="updateSyndicateRank(${idx}, this.value)" 
                           class="w-full accent-cyan-400 bg-gray-800 h-1.5 rounded cursor-pointer">
                </div>
            `).join('');
        }

        function toggleQuest(idx) {
            appState.quests[idx].completed = !appState.quests[idx].completed;
            saveToStorage();
            renderQuestsAndSyndicates();
        }

        function updateSyndicateRank(idx, val) {
            appState.syndicates[idx].rank = parseInt(val, 10);
            saveToStorage();
            renderQuestsAndSyndicates();
        }

        function renderAllViews() {
            renderDashboard();
            renderStarChart();
            renderWarframes();
            renderWeapons();
            renderInventory();
            renderModsAndArcanes();
            renderQuestsAndSyndicates();
            initializeIcons();
        }

        function editProfileName() {
            const current = appState.profile.username || "Tenno";
            const newName = prompt("Enter your Tenno Codex Alias / Username:", current);
            if (newName !== null && newName.trim() !== "") {
                appState.profile.username = newName.trim();
                saveToStorage();
                document.getElementById('profileNameDisplay').innerText = appState.profile.username;
            }
        }

        function openBackupModal() {
            document.getElementById('backupModal').classList.remove('hidden');
        }

        function closeBackupModal() {
            document.getElementById('backupModal').classList.add('hidden');
        }

        function exportDataJSON() {
            const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(appState, null, 2));
            const dlAnchorElem = document.createElement('a');
            dlAnchorElem.setAttribute("href", dataStr);
            dlAnchorElem.setAttribute("download", `Warframe_Nexus_Backup_${new Date().toISOString().slice(0,10)}.json`);
            document.body.appendChild(dlAnchorElem);
            dlAnchorElem.click();
            dlAnchorElem.remove();
            showToast("Backup file downloaded!");
        }

        function importDataJSON(event) {
            const fileReader = new FileReader();
            fileReader.onload = function(e) {
                try {
                    const parsed = JSON.parse(e.target.result);
                    if (parsed && parsed.warframes) {
                        appState = parsed;
                        saveToStorage();
                        renderAllViews();
                        closeBackupModal();
                        showToast("Progress Data Imported Successfully!");
                    } else {
                        alert("Invalid Warframe backup file format.");
                    }
                } catch (err) {
                    alert("Error parsing JSON backup file.");
                }
            };
            if (event.target.files[0]) {
                fileReader.readAsText(event.target.files[0]);
            }
        }

        function resetAllData() {
            if (confirm("Are you sure you want to reset all tracked Warframe data to default settings? This cannot be undone.")) {
                appState = JSON.parse(JSON.stringify(DEFAULT_STATE));
                saveToStorage();
                renderAllViews();
                closeBackupModal();
                showToast("All data reset to default state.");
            }
        }

        function showToast(msg) {
            const toast = document.getElementById('toastNotification');
            document.getElementById('toastMsg').innerText = msg;
            toast.classList.remove('translate-y-20', 'opacity-0');
            toast.classList.add('translate-y-0', 'opacity-100');
            
            setTimeout(() => {
                toast.classList.remove('translate-y-0', 'opacity-100');
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 2500);
        }
    </script>
</body>
</html>
```# Warframe-Tracker
Master Tracker
