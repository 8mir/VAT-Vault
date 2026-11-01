<!DOCTYPE html>
<html lang="en" dir="ltr" class="dark">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>VAT Vault — Smart VAT Invoice Manager | فواتيرك الضريبية بذكاء</title>
  <meta name="description" content="macOS & iOS application for automated VAT invoice management using SwiftUI, SwiftData, and Apple Vision OCR." />
  
  <!-- Google Fonts: Inter, Tajawal & JetBrains Mono -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600&family=Tajawal:wght@400;500;700;800;900&display=swap" rel="stylesheet">
  
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          fontFamily: {
            sans: ['Inter', 'Tajawal', 'sans-serif'],
            mono: ['JetBrains Mono', 'monospace'],
          },
          colors: {
            vault: {
              bg: '#07080a',
              card: 'rgba(18, 20, 26, 0.72)',
              border: 'rgba(255, 255, 255, 0.08)',
              accent: '#10b981',
              accentSoft: 'rgba(16, 185, 129, 0.14)',
              blue: '#3b82f6'
            }
          }
        }
      }
    }
  </script>

  <style>
    :root {
      color-scheme: dark;
    }
    html {
      scroll-behavior: smooth;
    }
    html[lang="en"] body {
      font-family: 'Inter', sans-serif;
    }
    html[lang="ar"] body {
      font-family: 'Tajawal', sans-serif;
    }
    body {
      background-color: #07080a;
      color: #f3f4f6;
      background-image: 
        radial-gradient(circle at 50% -10%, rgba(16, 185, 129, 0.14), transparent 45%),
        radial-gradient(circle at 85% 30%, rgba(59, 130, 246, 0.08), transparent 40%);
      overflow-x: hidden;
    }
    .glass-panel {
      background: rgba(18, 20, 26, 0.72);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      border: 1px solid rgba(255, 255, 255, 0.08);
    }
    .glass-card {
      background: linear-gradient(180deg, rgba(255,255,255,0.04) 0%, rgba(255,255,255,0.015) 100%);
      backdrop-filter: blur(16px);
      -webkit-backdrop-filter: blur(16px);
      border: 1px solid rgba(255, 255, 255, 0.08);
      transition: all 0.25s cubic-bezier(0.16, 1, 0.3, 1);
    }
    .glass-card:hover {
      border-color: rgba(16, 185, 129, 0.32);
      transform: translateY(-2px);
    }
    .mac-shadow {
      box-shadow: 0 30px 90px -20px rgba(0, 0, 0, 0.85), 0 0 0 1px rgba(255, 255, 255, 0.09);
    }
    @keyframes scanline {
      0% { top: 6%; opacity: 0.9; }
      50% { top: 90%; opacity: 1; }
      100% { top: 6%; opacity: 0.9; }
    }
    .ocr-laser {
      position: absolute;
      left: 0;
      right: 0;
      height: 2px;
      background: linear-gradient(90deg, transparent, #10b981, #34d399, #10b981, transparent);
      box-shadow: 0 0 18px 3px rgba(16, 185, 129, 0.65);
      animation: scanline 3.2s ease-in-out infinite;
    }
    @keyframes floatPulse {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.03); }
    }
    .bubble-item {
      animation: floatPulse 5s ease-in-out infinite;
    }
    @keyframes dashFlow {
      to { stroke-dashoffset: -40; }
    }
    .flow-line {
      stroke-dasharray: 6 6;
      animation: dashFlow 1.8s linear infinite;
    }
    /* Enable smooth grab scroll for containers */
    .draggable-container {
      cursor: grab;
      overflow-x: auto;
      user-select: none;
      scrollbar-width: thin;
    }
    .draggable-container:active {
      cursor: grabbing;
    }
    ::-webkit-scrollbar { width: 8px; height: 8px; }
    ::-webkit-scrollbar-track { background: #07080a; }
    ::-webkit-scrollbar-thumb { background: #1f2430; border-radius: 9999px; }
  </style>
</head>

<body class="antialiased selection:bg-emerald-500/30 selection:text-emerald-200">

  <!-- Top Navigation Bar -->
  <header class="sticky top-0 z-50 border-b border-white/5 bg-[#07080a]/80 backdrop-blur-xl">
    <div class="max-w-6xl mx-auto px-4 sm:px-6 h-16 flex items-center justify-between">
      <a href="#" class="flex items-center gap-3 group">
        <div class="w-9 h-9 rounded-xl bg-gradient-to-br from-emerald-400/20 to-emerald-600/20 border border-emerald-500/30 flex items-center justify-center shadow-inner">
          <svg class="w-5 h-5 text-emerald-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M9 14l6-6m-5.5.5h.01m4.99 5h.01M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16l3.5-2 3.5 2 3.5-2 3.5 2z" />
          </svg>
        </div>
        <div class="flex items-center gap-2">
          <span class="font-bold tracking-tight text-white text-lg">VAT Vault</span>
          <span class="text-[11px] font-mono px-2 py-0.5 rounded-full bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">v1.0</span>
        </div>
      </a>

      <nav class="hidden md:flex items-center gap-7 text-sm text-zinc-400">
        <a href="#preview-gif-section" class="hover:text-white transition" data-i18n="nav_preview">Workflow</a>
        <a href="#ocr-section" class="hover:text-white transition" data-i18n="nav_ocr">Vision OCR</a>
        <a href="#quarters-section" class="hover:text-white transition" data-i18n="nav_quarters">Quarters</a>
        <a href="#privacy-section" class="hover:text-white transition" data-i18n="nav_privacy">Privacy</a>
        <a href="#toolkit-section" class="hover:text-white transition" data-i18n="nav_features">Toolkit</a>
        <a href="#faq-section" class="hover:text-white transition" data-i18n="nav_faq">FAQ</a>
      </nav>

      <div class="flex items-center gap-2">
        <!-- Language Toggle Button -->
        <button onclick="toggleLanguage()" id="langToggleBtn"
                class="inline-flex items-center gap-1.5 px-3 py-1.5 rounded-xl text-xs font-mono font-semibold bg-emerald-500/10 hover:bg-emerald-500/20 border border-emerald-500/30 text-emerald-300 transition">
          <svg class="w-3.5 h-3.5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M3 5h12M9 3v2m1.048 9.5A18.022 18.022 0 016.412 9m6.088 9h7M11 21l5-10 5 10M12.751 5C11.783 10.77 8.07 15.61 3 18.129" />
          </svg>
          <span id="langToggleText">العربية</span>
        </button>

        <a href="https://github.com/8mir/VAT-Vault" target="_blank" rel="noopener"
           class="hidden sm:inline-flex items-center gap-2 px-3.5 py-1.5 rounded-xl text-xs font-medium bg-white/5 hover:bg-white/10 border border-white/10 text-zinc-200 transition">
          <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.53 1.032 1.53 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0112 6.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.202 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.943.359.309.678.92.678 1.855 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0022 12.017C22 6.484 17.522 2 12 2z"/>
          </svg>
          <span>GitHub</span>
        </a>

        <a href="#download-section" data-i18n="nav_download"
           class="px-4 py-1.5 rounded-xl text-xs font-bold bg-emerald-500 hover:bg-emerald-400 text-zinc-950 transition shadow-lg shadow-emerald-500/20">
          Download
        </a>
      </div>
    </div>
  </header>

  <!-- Hero Section -->
  <section class="pt-14 pb-10 px-4 sm:px-6">
    <div class="max-w-4xl mx-auto text-center">
      <div class="inline-flex items-center gap-2 px-3.5 py-1 rounded-full bg-white/[0.04] border border-white/10 text-xs font-mono text-zinc-300 mb-6">
        <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
        <span>FOR MACOS &amp; IOS · SWIFTUI · VISION OCR · SWIFTDATA</span>
      </div>

      <h1 class="text-4xl sm:text-6xl font-extrabold tracking-tight text-white leading-[1.15] mb-5">
        <span data-i18n="hero_title_1">Your VAT invoices,</span>
        <span class="bg-gradient-to-r from-emerald-400 via-teal-300 to-sky-400 bg-clip-text text-transparent" data-i18n="hero_title_2">lighter &amp; smarter.</span>
      </h1>

      <p class="text-base sm:text-xl text-zinc-400 max-w-2xl mx-auto leading-relaxed mb-8" data-i18n="hero_subtitle">
        VAT Vault captures purchase and sales receipts, extracts dates, merchants, and tax totals automatically via Apple Vision OCR, and organizes them by financial quarters in one tap.
      </p>

      <!-- Download Hero Buttons -->
      <div class="flex flex-wrap items-center justify-center gap-3.5 mb-4">
        <a href="https://github.com/8mir/VAT-Vault/releases" target="_blank" rel="noopener"
           class="flex items-center gap-3 px-6 py-3.5 rounded-2xl bg-white text-zinc-950 font-bold text-sm hover:bg-zinc-200 transition shadow-xl">
          <svg class="w-5 h-5 shrink-0" fill="currentColor" viewBox="0 0 24 24">
            <path d="M18.71 19.5c-.83 1.24-1.71 2.45-3.05 2.47-1.34.03-1.77-.79-3.29-.79-1.53 0-2 .77-3.27.82-1.31.05-2.3-1.32-3.14-2.53C4.25 17 2.94 12.45 4.7 9.39c.87-1.52 2.43-2.48 4.12-2.51 1.28-.02 2.5.87 3.29.87.78 0 2.26-1.07 3.81-.91.65.03 2.47.26 3.64 1.98-.09.06-2.17 1.28-2.15 3.81.03 3.02 2.65 4.03 2.68 4.04-.03.07-.42 1.44-1.38 2.83M15.97 6.33c.64-.78 1.08-1.86.96-2.94-.93.04-2.06.62-2.72 1.4-.58.68-1.1 1.79-.96 2.84 1.04.08 2.08-.52 2.72-1.3z"/>
          </svg>
          <div class="text-start">
            <div data-i18n="btn_mac_download">Download for macOS</div>
            <div class="text-[11px] font-mono font-normal text-zinc-600">Apple Silicon &amp; Intel · Universal</div>
          </div>
        </a>

        <a href="https://github.com/8mir/VAT-Vault" target="_blank" rel="noopener"
           class="flex items-center gap-3 px-6 py-3.5 rounded-2xl glass-card text-white font-bold text-sm hover:bg-white/10 transition">
          <svg class="w-5 h-5 text-emerald-400 shrink-0" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4" />
          </svg>
          <div class="text-start">
            <div data-i18n="btn_source_code">View Source Code</div>
            <div class="text-[11px] font-mono font-normal text-zinc-400">GitHub · 8mir/VAT-Vault</div>
          </div>
        </a>
      </div>
    </div>
  </section>

  <!-- Interactive Live macOS App Window -->
  <section class="px-4 sm:px-6 pb-16">
    <div class="max-w-5xl mx-auto">
      <div class="glass-panel rounded-3xl mac-shadow overflow-hidden border border-white/10">
        
        <!-- macOS Window Titlebar -->
        <div class="h-12 px-4 bg-zinc-950/80 border-b border-white/5 flex items-center justify-between select-none">
          <div class="flex items-center gap-2">
            <span class="w-3 h-3 rounded-full bg-[#ff5f57]"></span>
            <span class="w-3 h-3 rounded-full bg-[#febc2e]"></span>
            <span class="w-3 h-3 rounded-full bg-[#28c840]"></span>
            <span class="mx-3 text-xs font-mono text-zinc-400 hidden sm:inline">VAT Vault — SwiftData Local Store</span>
          </div>

          <!-- Spotlight Search Bar -->
          <div class="flex items-center gap-2 px-3 py-1 rounded-lg bg-white/[0.04] border border-white/10 text-xs text-zinc-400 w-56 sm:w-72">
            <svg class="w-3.5 h-3.5 text-zinc-500 shrink-0" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/>
            </svg>
            <input id="liveSearchInput" type="text" placeholder="Search merchant or TRN number..." 
                   class="bg-transparent border-none outline-none text-xs text-zinc-200 placeholder-zinc-500 w-full" />
            <kbd class="text-[10px] font-mono px-1.5 py-0.5 rounded bg-white/5 text-zinc-400">⌘K</kbd>
          </div>

          <div class="text-[11px] font-mono text-emerald-400 flex items-center gap-1.5">
            <span class="w-2 h-2 rounded-full bg-emerald-400"></span>
            <span>OCR READY</span>
          </div>
        </div>

        <!-- App Body -->
        <div class="grid grid-cols-1 lg:grid-cols-12 min-h-[470px]">
          
          <!-- Sidebar -->
          <aside class="lg:col-span-3 bg-zinc-950/40 border-b lg:border-b-0 lg:border-e border-white/5 p-4 flex flex-col justify-between">
            <div>
              <div class="text-[11px] font-mono uppercase tracking-wider text-zinc-500 mb-2.5 px-2" data-i18n="sidebar_quarters_title">Financial Quarters · 2026</div>
              <div class="space-y-1" id="quarterButtons">
                <button onclick="selectQuarter('ALL')" data-q="ALL" class="q-btn w-full flex items-center justify-between px-3 py-2 rounded-xl text-xs font-bold bg-emerald-500/15 text-emerald-300 border border-emerald-500/30 transition">
                  <span data-i18n="q_all">All Quarters (Annual)</span>
                  <span class="font-mono text-[11px]">100%</span>
                </button>
                <button onclick="selectQuarter('Q1')" data-q="Q1" class="q-btn w-full flex items-center justify-between px-3 py-2 rounded-xl text-xs text-zinc-400 hover:bg-white/5 transition">
                  <span data-i18n="q_1">First Quarter (Q1)</span>
                  <span class="font-mono text-[11px]" data-i18n="q_1_months">Jan – Mar</span>
                </button>
                <button onclick="selectQuarter('Q2')" data-q="Q2" class="q-btn w-full flex items-center justify-between px-3 py-2 rounded-xl text-xs text-zinc-400 hover:bg-white/5 transition">
                  <span data-i18n="q_2">Second Quarter (Q2)</span>
                  <span class="font-mono text-[11px]" data-i18n="q_2_months">Apr – Jun</span>
                </button>
                <button onclick="selectQuarter('Q3')" data-q="Q3" class="q-btn w-full flex items-center justify-between px-3 py-2 rounded-xl text-xs text-zinc-400 hover:bg-white/5 transition">
                  <span data-i18n="q_3">Third Quarter (Q3)</span>
                  <span class="font-mono text-[11px]" data-i18n="q_3_months">Jul – Sep</span>
                </button>
                <button onclick="selectQuarter('Q4')" data-q="Q4" class="q-btn w-full flex items-center justify-between px-3 py-2 rounded-xl text-xs text-zinc-400 hover:bg-white/5 transition">
                  <span data-i18n="q_4">Fourth Quarter (Q4)</span>
                  <span class="font-mono text-[11px]" data-i18n="q_4_months">Oct – Dec</span>
                </button>
              </div>
            </div>

            <!-- Storage / Engine Status Box -->
            <div class="mt-6 p-3.5 rounded-2xl bg-white/[0.03] border border-white/5">
              <div class="flex items-center justify-between text-xs mb-1.5">
                <span class="text-zinc-400" data-i18n="ocr_accuracy_label">OCR Extraction Accuracy</span>
                <span class="font-mono text-emerald-400 font-bold">99.4%</span>
              </div>
              <div class="w-full h-1.5 bg-zinc-800 rounded-full overflow-hidden mb-2">
                <div class="h-full bg-gradient-to-r from-emerald-400 to-teal-500 w-[99%]"></div>
              </div>
              <div class="text-[11px] text-zinc-500 font-mono">VNRecognizeTextRequest · On-Device</div>
            </div>
          </aside>

          <!-- Main Interactive Content -->
          <main class="lg:col-span-9 p-5 sm:p-6 flex flex-col justify-between bg-gradient-to-b from-white/[0.01] to-transparent">
            
            <div>
              <div class="flex flex-wrap items-center justify-between gap-4 pb-5 border-b border-white/5">
                <div>
                  <div class="text-xs font-mono text-emerald-400 mb-1" id="activeQuarterLabel">CONSOLIDATED VAT SUMMARY · ALL QUARTERS 2026</div>
                  <div class="flex items-baseline gap-3">
                    <span class="text-3xl sm:text-4xl font-extrabold font-mono text-white" id="totalVatDisplay">184.550</span>
                    <span class="text-sm font-mono text-zinc-400" data-i18n="currency_vat_label">BHD Reclaimable VAT</span>
                  </div>
                  <p class="text-xs text-zinc-400 mt-1">
                    <span data-i18n="net_total_label">Total Net Amount (Excl. VAT):</span>
                    <span id="totalNetDisplay" class="font-mono text-zinc-200">1,845.500</span>
                    <span data-i18n="currency_short">BHD</span> ·
                    <span data-i18n="invoice_count_label">Invoices:</span>
                    <span id="invoiceCountDisplay" class="font-mono text-emerald-400">6</span>
                  </p>
                </div>

                <div class="flex items-center gap-2.5">
                  <button onclick="simulateNewScan()" id="scanBtn"
                          class="flex items-center gap-2 px-4 py-2.5 rounded-xl bg-emerald-500 hover:bg-emerald-400 text-zinc-950 font-bold text-xs transition shadow-lg shadow-emerald-500/20">
                    <svg class="w-4 h-4 shrink-0" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2.2">
                      <path stroke-linecap="round" stroke-linejoin="round" d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z"/>
                    </svg>
                    <span id="scanBtnText" data-i18n="scan_btn_text">Scan New Invoice (OCR)</span>
                  </button>
                </div>
              </div>

              <!-- Four Quick Metric Cards inside App -->
              <div class="grid grid-cols-2 sm:grid-cols-4 gap-3 my-5">
                <div class="p-3.5 rounded-2xl bg-white/[0.03] border border-white/5">
                  <div class="text-[11px] text-zinc-400" data-i18n="metric_1_title">Supported VAT Rates</div>
                  <div class="text-lg font-bold font-mono text-white mt-0.5">10% / 15%</div>
                  <div class="text-[10px] text-emerald-400 font-mono mt-0.5" data-i18n="metric_1_sub">Auto-Detected</div>
                </div>
                <div class="p-3.5 rounded-2xl bg-white/[0.03] border border-white/5">
                  <div class="text-[11px] text-zinc-400" data-i18n="metric_2_title">Storage Engine</div>
                  <div class="text-lg font-bold text-white mt-0.5" data-i18n="metric_2_val">Local Vault</div>
                  <div class="text-[10px] text-sky-400 font-mono mt-0.5">SwiftData + iCloud</div>
                </div>
                <div class="p-3.5 rounded-2xl bg-white/[0.03] border border-white/5">
                  <div class="text-[11px] text-zinc-400" data-i18n="metric_3_title">Avg. Scan Speed</div>
                  <div class="text-lg font-bold font-mono text-white mt-0.5">0.4s</div>
                  <div class="text-[10px] text-zinc-400 font-mono mt-0.5">Apple Neural Engine</div>
                </div>
                <div class="p-3.5 rounded-2xl bg-white/[0.03] border border-white/5">
                  <div class="text-[11px] text-zinc-400" data-i18n="metric_4_title">Tax Export</div>
                  <div class="text-lg font-bold font-mono text-white mt-0.5">CSV · PDF</div>
                  <div class="text-[10px] text-amber-400 font-mono mt-0.5" data-i18n="metric_4_sub">Audit-Ready Returns</div>
                </div>
              </div>

              <!-- Invoices Table inside Mockup (Draggable/Scrollable) -->
              <div class="draggable-container overflow-x-auto pb-2" id="dragTableScroll">
                <table class="w-full text-start text-xs whitespace-nowrap">
                  <thead>
                    <tr class="text-zinc-500 border-b border-white/5 font-mono text-[11px]">
                      <th class="pb-2.5 font-normal text-start pe-4" data-i18n="th_merchant">Merchant / Vendor</th>
                      <th class="pb-2.5 font-normal text-start px-4" data-i18n="th_trn">Tax Reg. No (TRN)</th>
                      <th class="pb-2.5 font-normal text-start px-4" data-i18n="th_date">Date</th>
                      <th class="pb-2.5 font-normal text-start px-4" data-i18n="th_quarter">Quarter</th>
                      <th class="pb-2.5 font-normal text-start px-4" data-i18n="th_net">Net Amount</th>
                      <th class="pb-2.5 font-normal text-emerald-400 text-start ps-4" data-i18n="th_vat">VAT Amount</th>
                    </tr>
                  </thead>
                  <tbody id="invoiceTableBody" class="divide-y divide-white/5 font-mono">
                    <!-- Rendered dynamically by JS -->
                  </tbody>
                </table>
              </div>
            </div>

            <div class="pt-4 mt-4 border-t border-white/5 flex flex-wrap items-center justify-between gap-2 text-[11px] text-zinc-500">
              <span data-i18n="interactive_hint">Try it live — the app window above is interactive! Switch quarters or click 'Scan New Invoice'.</span>
              <span class="font-mono text-zinc-400" data-i18n="export_shortcut">↵ Instant Export</span>
            </div>
          </main>
        </div>
      </div>
    </div>
  </section>

  <!-- ANIMATED WORKFLOW SHOWCASE -->
  <section id="preview-gif-section" class="py-16 px-4 sm:px-6 border-t border-white/5">
    <div class="max-w-5xl mx-auto">
      <div class="text-center max-w-2xl mx-auto mb-10">
        <div class="text-xs font-mono uppercase tracking-widest text-emerald-400 mb-2">LIVE ANIMATED WORKFLOW</div>
        <h2 class="text-3xl sm:text-4xl font-extrabold text-white" data-i18n="gif_sec_title">From camera roll to tax return in seconds.</h2>
        <p class="text-zinc-400 text-sm sm:text-base mt-2.5" data-i18n="gif_sec_desc">
          Watch how VAT Vault scans receipts, extracts line items via Vision OCR, and groups transactions into quarterly dashboards automatically.
        </p>
      </div>

      <div class="glass-panel rounded-3xl p-4 sm:p-6 mac-shadow border border-white/10">
        <div class="flex items-center justify-between px-3 pb-3 mb-3 border-b border-white/5 text-xs font-mono text-zinc-400">
          <div class="flex items-center gap-2">
            <span class="w-2.5 h-2.5 rounded-full bg-emerald-400 animate-ping"></span>
            <span class="w-2.5 h-2.5 rounded-full bg-emerald-400 -ms-4"></span>
            <span>VAT-Vault-Preview.gif · Native SwiftUI Animation</span>
          </div>
          <span class="text-emerald-400">60 FPS · Metal Accelerated</span>
        </div>

        <div class="relative rounded-2xl overflow-hidden bg-zinc-950/90 border border-white/5">
          <img id="repoAnimatedPreview" 
               src="./demo.gif" 
               alt="VAT Vault Animated Preview" 
               class="w-full h-auto object-cover rounded-2xl hidden"
               onload="this.classList.remove('hidden'); document.getElementById('svgAnimatedShowcase').classList.add('mt-4');" />

          <div id="svgAnimatedShowcase" class="p-6 sm:p-10 bg-gradient-to-br from-zinc-950 via-[#0b1016] to-zinc-950">
            <svg viewBox="0 0 900 320" class="w-full h-auto select-none" fill="none" xmlns="http://www.w3.org/2000/svg">
              <defs>
                <linearGradient id="emeraldGrad" x1="0%" y1="0%" x2="100%" y2="100%">
                  <stop offset="0%" stop-color="#10b981" stop-opacity="0.35"/>
                  <stop offset="100%" stop-color="#0ea5e9" stop-opacity="0.1"/>
                </linearGradient>
                <linearGradient id="beamGrad" x1="0%" y1="0%" x2="100%" y2="0%">
                  <stop offset="0%" stop-color="#10b981" stop-opacity="0"/>
                  <stop offset="50%" stop-color="#34d399" stop-opacity="1"/>
                  <stop offset="100%" stop-color="#10b981" stop-opacity="0"/>
                </linearGradient>
              </defs>

              <g transform="translate(30, 30)">
                <rect width="210" height="260" rx="18" fill="#11141c" stroke="rgba(255,255,255,0.12)" stroke-width="1.5"/>
                <text x="22" y="34" fill="#10b981" font-family="JetBrains Mono, monospace" font-size="11" font-weight="600">01. RECEIPT INPUT</text>
                <rect x="22" y="52" width="120" height="10" rx="5" fill="#272e3f"/>
                <rect x="22" y="72" width="165" height="8" rx="4" fill="#1e2433"/>
                <rect x="22" y="90" width="140" height="8" rx="4" fill="#1e2433"/>
                <rect x="22" y="118" width="165" height="32" rx="8" fill="rgba(16,185,129,0.12)" stroke="rgba(16,185,129,0.4)"/>
                <text x="34" y="138" fill="#6ee7b7" font-family="JetBrains Mono, monospace" font-size="11">TRN: 200019482700002</text>
                <rect x="22" y="164" width="165" height="32" rx="8" fill="rgba(56,189,248,0.12)" stroke="rgba(56,189,248,0.35)"/>
                <text x="34" y="184" fill="#7dd3fc" font-family="JetBrains Mono, monospace" font-size="11">VAT 10%: 42.000 BHD</text>
                <text x="22" y="232" fill="#ffffff" font-family="JetBrains Mono, monospace" font-size="14" font-weight="700">TOTAL: 462.000 BHD</text>
                <rect x="10" y="45" width="190" height="3" fill="url(#beamGrad)">
                  <animate attributeName="y" values="45;235;45" dur="2.8s" repeatCount="indefinite"/>
                </rect>
              </g>

              <path d="M 245 160 L 335 160" stroke="#10b981" stroke-width="2.5" class="flow-line"/>
              <circle cx="290" cy="160" r="5" fill="#34d399">
                <animate attributeName="cx" values="245;335" dur="1.4s" repeatCount="indefinite"/>
              </circle>

              <g transform="translate(340, 50)">
                <rect width="220" height="220" rx="22" fill="url(#emeraldGrad)" stroke="#10b981" stroke-width="1.5"/>
                <circle cx="110" cy="85" r="34" fill="rgba(16,185,129,0.18)" stroke="#34d399" stroke-width="1.5">
                  <animate attributeName="r" values="31;37;31" dur="2.2s" repeatCount="indefinite"/>
                </circle>
                <text x="110" y="90" text-anchor="middle" fill="#ffffff" font-family="JetBrains Mono, monospace" font-size="13" font-weight="700">VISION</text>
                <text x="110" y="148" text-anchor="middle" fill="#e2e8f0" font-family="Inter, sans-serif" font-size="13" font-weight="700">Apple Vision OCR</text>
                <text x="110" y="170" text-anchor="middle" fill="#6ee7b7" font-family="JetBrains Mono, monospace" font-size="11">VNRecognizeTextRequest</text>
                <text x="110" y="192" text-anchor="middle" fill="#94a3b8" font-family="JetBrains Mono, monospace" font-size="10">On-Device · 0.4s</text>
              </g>

              <path d="M 565 160 L 655 160" stroke="#38bdf8" stroke-width="2.5" class="flow-line"/>
              <circle cx="610" cy="160" r="5" fill="#38bdf8">
                <animate attributeName="cx" values="565;655" dur="1.4s" repeatCount="indefinite"/>
              </circle>

              <g transform="translate(660, 30)">
                <rect width="210" height="260" rx="18" fill="#11141c" stroke="rgba(255,255,255,0.12)" stroke-width="1.5"/>
                <text x="22" y="34" fill="#38bdf8" font-family="JetBrains Mono, monospace" font-size="11" font-weight="600">03. SWIFTDATA VAULT</text>
                <rect x="22" y="56" width="166" height="36" rx="8" fill="#181d29"/>
                <text x="34" y="79" fill="#cbd5e1" font-family="JetBrains Mono, monospace" font-size="11">Q1 · 50.500 BHD</text>
                <rect x="22" y="102" width="166" height="36" rx="8" fill="#181d29"/>
                <text x="34" y="125" fill="#cbd5e1" font-family="JetBrains Mono, monospace" font-size="11">Q2 · 54.000 BHD</text>
                <rect x="22" y="148" width="166" height="36" rx="8" fill="#181d29"/>
                <text x="34" y="171" fill="#cbd5e1" font-family="JetBrains Mono, monospace" font-size="11">Q3 · 21.050 BHD</text>
                <rect x="22" y="194" width="166" height="44" rx="10" fill="rgba(16,185,129,0.18)" stroke="#10b981"/>
                <text x="34" y="220" fill="#ffffff" font-family="JetBrains Mono, monospace" font-size="12" font-weight="700">Q4 · +59.000 BHD ✓</text>
              </g>
            </svg>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Section 1: SMART VISION OCR -->
  <section id="ocr-section" class="py-20 px-4 sm:px-6 border-t border-white/5">
    <div class="max-w-5xl mx-auto grid grid-cols-1 lg:grid-cols-12 gap-10 items-center">
      <div class="lg:col-span-5 space-y-4">
        <div class="text-xs font-mono uppercase tracking-widest text-emerald-400">VISION OCR ENGINE</div>
        <h2 class="text-3xl sm:text-4xl font-extrabold text-white leading-tight" data-i18n="ocr_sec_title">
          One snap. Every detail in place.
        </h2>
        <p class="text-zinc-400 text-sm sm:text-base leading-relaxed" data-i18n="ocr_sec_desc">
          No more typing receipt numbers into messy spreadsheets. Powered by Apple Vision, VAT Vault reads Arabic and English paper or PDF invoices, extracting vendor names, dates, subtotals, and VAT in milliseconds.
        </p>
        <ul class="space-y-2.5 pt-2 text-sm text-zinc-300">
          <li class="flex items-center gap-2.5">
            <span class="w-5 h-5 rounded-full bg-emerald-500/20 text-emerald-400 flex items-center justify-center text-xs shrink-0">✓</span>
            <span data-i18n="ocr_bullet_1">Automatic multi-format date parsing &amp; financial quarter assignment</span>
          </li>
          <li class="flex items-center gap-2.5">
            <span class="w-5 h-5 rounded-full bg-emerald-500/20 text-emerald-400 flex items-center justify-center text-xs shrink-0">✓</span>
            <span data-i18n="ocr_bullet_2">TRN / VAT Registration ID extraction and checksum validation</span>
          </li>
          <li class="flex items-center gap-2.5">
            <span class="w-5 h-5 rounded-full bg-emerald-500/20 text-emerald-400 flex items-center justify-center text-xs shrink-0">✓</span>
            <span data-i18n="ocr_bullet_3">Smart separation between net taxable subtotal and VAT amount</span>
          </li>
        </ul>
      </div>

      <div class="lg:col-span-7">
        <div class="glass-panel rounded-3xl p-6 relative overflow-hidden">
          <div class="ocr-laser"></div>
          <div class="flex items-center justify-between text-xs font-mono text-zinc-400 mb-4 pb-3 border-b border-white/10">
            <span>RECEIPT_IMG_4092.HEIC</span>
            <span class="text-emerald-400">● PARSING TOKENS...</span>
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div class="p-4 rounded-2xl bg-zinc-950/80 border border-white/5 font-mono text-xs space-y-2 text-zinc-400 text-left" dir="ltr">
              <div class="text-[10px] text-zinc-500 uppercase" data-i18n="raw_tokens_label">RAW CAPTURED TEXT</div>
              <div class="p-2 rounded bg-white/[0.03] text-zinc-200">AL-NOOR TECH SUPPLIES W.L.L</div>
              <div class="p-2 rounded bg-white/[0.03]">VAT REG: <span class="text-sky-400">200019482700002</span></div>
              <div class="p-2 rounded bg-white/[0.03]">DATE: <span class="text-amber-300">2026-10-18 14:22</span></div>
              <div class="p-2 rounded bg-white/[0.03]">SUBTOTAL: 420.000 BHD</div>
              <div class="p-2 rounded bg-emerald-500/10 border border-emerald-500/30 text-emerald-300">VAT (10%): 42.000 BHD</div>
              <div class="p-2 rounded bg-white/[0.03] font-bold text-white">TOTAL: 462.000 BHD</div>
            </div>

            <div class="p-4 rounded-2xl bg-emerald-950/15 border border-emerald-500/20 flex flex-col justify-between">
              <div>
                <div class="text-[10px] font-mono text-emerald-400 uppercase mb-3" data-i18n="swiftdata_model_label">PARSED ENTITY · SwiftData @Model</div>
                <div class="space-y-3 text-xs">
                  <div class="flex justify-between border-b border-white/5 pb-2">
                    <span class="text-zinc-400" data-i18n="parsed_merchant_key">Merchant:</span>
                    <span class="font-bold text-white" data-i18n="parsed_merchant_val">Al-Noor Tech Supplies</span>
                  </div>
                  <div class="flex justify-between border-b border-white/5 pb-2">
                    <span class="text-zinc-400" data-i18n="parsed_q_key">Auto Quarter:</span>
                    <span class="font-mono px-2 py-0.5 rounded bg-emerald-500/20 text-emerald-300">Q4 · 2026</span>
                  </div>
                  <div class="flex justify-between border-b border-white/5 pb-2">
                    <span class="text-zinc-400" data-i18n="parsed_net_key">Net Subtotal:</span>
                    <span class="font-mono text-zinc-200">420.000 BHD</span>
                  </div>
                  <div class="flex justify-between pb-1">
                    <span class="text-zinc-400" data-i18n="parsed_vat_key">VAT Amount:</span>
                    <span class="font-mono font-bold text-emerald-400">+42.000 BHD</span>
                  </div>
                </div>
              </div>

              <div class="mt-4 pt-3 border-t border-white/10 flex items-center justify-between text-[11px] font-mono text-zinc-400">
                <span>Confidence: 99.8%</span>
                <span class="text-emerald-400">✓ Saved to Vault</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Section 2: QUARTER SPACE MAP -->
  <section id="quarters-section" class="py-20 px-4 sm:px-6 border-t border-white/5">
    <div class="max-w-5xl mx-auto">
      <div class="text-center max-w-2xl mx-auto mb-12">
        <div class="text-xs font-mono uppercase tracking-widest text-sky-400 mb-2">QUARTERLY VISUAL MAP</div>
        <h2 class="text-3xl sm:text-4xl font-extrabold text-white" data-i18n="map_title">See where your tax went.</h2>
        <p class="text-zinc-400 text-sm sm:text-base mt-3" data-i18n="map_desc">
          Every expense category and financial quarter becomes an interactive bubble. Bigger bubble, larger reclaimable VAT.
        </p>
      </div>

      <div class="glass-panel rounded-3xl p-6 sm:p-10">
        <div class="flex flex-wrap items-center justify-center gap-6 sm:gap-10 py-4">
          <div class="bubble-item w-44 h-44 sm:w-52 sm:h-52 rounded-full bg-gradient-to-br from-emerald-500/25 to-teal-600/10 border border-emerald-400/30 flex flex-col items-center justify-center text-center p-4 shadow-2xl">
            <span class="text-xs text-emerald-300 font-bold" data-i18n="bubble_1_title">Tech &amp; Hardware</span>
            <span class="text-2xl sm:text-3xl font-extrabold font-mono text-white my-1">84.200</span>
            <span class="text-[11px] font-mono text-zinc-400" data-i18n="bubble_1_sub">BHD VAT · 45%</span>
          </div>
          <div class="bubble-item w-36 h-36 sm:w-44 sm:h-44 rounded-full bg-gradient-to-br from-sky-500/25 to-blue-600/10 border border-sky-400/30 flex flex-col items-center justify-center text-center p-4" style="animation-delay: 1s;">
            <span class="text-xs text-sky-300 font-bold" data-i18n="bubble_2_title">Print &amp; Materials</span>
            <span class="text-xl sm:text-2xl font-extrabold font-mono text-white my-1">52.150</span>
            <span class="text-[11px] font-mono text-zinc-400" data-i18n="bubble_2_sub">BHD VAT · 28%</span>
          </div>
          <div class="bubble-item w-32 h-32 sm:w-36 sm:h-36 rounded-full bg-gradient-to-br from-purple-500/25 to-indigo-600/10 border border-purple-400/30 flex flex-col items-center justify-center text-center p-3" style="animation-delay: 2s;">
            <span class="text-xs text-purple-300 font-bold" data-i18n="bubble_3_title">Cloud Services</span>
            <span class="text-lg sm:text-xl font-extrabold font-mono text-white my-0.5">29.800</span>
            <span class="text-[10px] font-mono text-zinc-400" data-i18n="bubble_3_sub">BHD · 16%</span>
          </div>
          <div class="bubble-item w-28 h-28 sm:w-32 sm:h-32 rounded-full bg-gradient-to-br from-amber-500/20 to-orange-600/10 border border-amber-400/30 flex flex-col items-center justify-center text-center p-3" style="animation-delay: 0.5s;">
            <span class="text-[11px] text-amber-300 font-bold" data-i18n="bubble_4_title">Operations</span>
            <span class="text-base sm:text-lg font-extrabold font-mono text-white my-0.5">18.400</span>
            <span class="text-[10px] font-mono text-zinc-400" data-i18n="bubble_4_sub">BHD · 11%</span>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Section 3: SAFE BY DESIGN -->
  <section id="privacy-section" class="py-20 px-4 sm:px-6 border-t border-white/5">
    <div class="max-w-5xl mx-auto">
      <div class="mb-10">
        <div class="text-xs font-mono uppercase tracking-widest text-emerald-400 mb-2">SAFE BY DESIGN</div>
        <h2 class="text-3xl sm:text-4xl font-extrabold text-white" data-i18n="safe_title">Two rules. No surprises.</h2>
        <p class="text-zinc-400 text-sm sm:text-base mt-2" data-i18n="safe_desc">Your financial records and receipts never leave your device to any external cloud server.</p>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div class="glass-card rounded-3xl p-7">
          <div class="w-10 h-10 rounded-2xl bg-emerald-500/15 border border-emerald-500/30 flex items-center justify-center text-emerald-400 font-mono font-bold mb-5">01</div>
          <h3 class="text-xl font-bold text-white mb-2" data-i18n="rule_1_title">100% On-Device Vision OCR Processing</h3>
          <p class="text-sm text-zinc-400 leading-relaxed mb-5" data-i18n="rule_1_desc">
            Receipts and PDFs are processed directly on Apple Silicon Neural Engine without uploading sensitive invoices to third-party APIs.
          </p>
          <div class="text-xs font-mono px-3 py-2 rounded-xl bg-white/[0.03] border border-white/5 text-emerald-300">
            Zero Cloud API Calls · 100% Offline Capable
          </div>
        </div>

        <div class="glass-card rounded-3xl p-7">
          <div class="w-10 h-10 rounded-2xl bg-sky-500/15 border border-sky-500/30 flex items-center justify-center text-sky-400 font-mono font-bold mb-5">02</div>
          <h3 class="text-xl font-bold text-white mb-2" data-i18n="rule_2_title">Original Receipts Preserved with Instant Undo</h3>
          <p class="text-sm text-zinc-400 leading-relaxed mb-5" data-i18n="rule_2_desc">
            High-res receipt attachments are stored alongside structured SwiftData records, and deleted items go to temporary Trash first.
          </p>
          <div class="text-xs font-mono px-3 py-2 rounded-xl bg-white/[0.03] border border-white/5 text-sky-300">
            SwiftData Persistence · Instant Audit Trail
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Section 4: EVERYTHING IN ONE PLACE -->
  <section id="toolkit-section" class="py-20 px-4 sm:px-6 border-t border-white/5">
    <div class="max-w-5xl mx-auto">
      <div class="mb-12">
        <div class="text-xs font-mono uppercase tracking-widest text-emerald-400 mb-2">EVERYTHING IN ONE PLACE</div>
        <h2 class="text-3xl sm:text-4xl font-extrabold text-white" data-i18n="toolkit_title">Small app. Whole accounting toolkit.</h2>
      </div>

      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
        <div class="glass-card rounded-2xl p-5">
          <div class="text-emerald-400 font-bold text-sm mb-1.5" data-i18n="feat_1_title">Smart Vision OCR</div>
          <p class="text-xs text-zinc-400 leading-relaxed" data-i18n="feat_1_desc">Instant extraction of merchant names, dates, and tax totals from photos &amp; PDFs.</p>
        </div>
        <div class="glass-card rounded-2xl p-5">
          <div class="text-emerald-400 font-bold text-sm mb-1.5" data-i18n="feat_2_title">Quarterly Grouping (Q1–Q4)</div>
          <p class="text-xs text-zinc-400 leading-relaxed" data-i18n="feat_2_desc">Automatic sorting into the right fiscal quarter the moment a transaction date is read.</p>
        </div>
        <div class="glass-card rounded-2xl p-5">
          <div class="text-emerald-400 font-bold text-sm mb-1.5" data-i18n="feat_3_title">Duplicate Invoice Guard</div>
          <p class="text-xs text-zinc-400 leading-relaxed" data-i18n="feat_3_desc">Smart alerts if the same receipt is scanned twice based on TRN, date, and total.</p>
        </div>
        <div class="glass-card rounded-2xl p-5">
          <div class="text-emerald-400 font-bold text-sm mb-1.5" data-i18n="feat_4_title">Native Glassmorphic UI</div>
          <p class="text-xs text-zinc-400 leading-relaxed" data-i18n="feat_4_desc">Crafted in SwiftUI with dark translucent panels that feel right at home on macOS &amp; iOS.</p>
        </div>
        <div class="glass-card rounded-2xl p-5">
          <div class="text-emerald-400 font-bold text-sm mb-1.5" data-i18n="feat_5_title">One-Click Tax Reports</div>
          <p class="text-xs text-zinc-400 leading-relaxed" data-i18n="feat_5_desc">Generate clean CSV tables and PDF summaries ready for quarterly VAT return filings.</p>
        </div>
        <div class="glass-card rounded-2xl p-5">
          <div class="text-emerald-400 font-bold text-sm mb-1.5" data-i18n="feat_6_title">Free &amp; Open Source</div>
          <p class="text-xs text-zinc-400 leading-relaxed" data-i18n="feat_6_desc">No ads, no subscriptions, and full transparency with complete source code on GitHub.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- Section 5: FAQ -->
  <section id="faq-section" class="py-20 px-4 sm:px-6 border-t border-white/5">
    <div class="max-w-3xl mx-auto">
      <h2 class="text-3xl font-extrabold text-white mb-8" data-i18n="faq_heading">Questions.</h2>
      
      <div class="space-y-4">
        <div class="glass-card rounded-2xl p-6">
          <h3 class="font-bold text-white text-base mb-2" data-i18n="faq_1_q">Does VAT Vault require an internet connection to scan invoices?</h3>
          <p class="text-sm text-zinc-400 leading-relaxed" data-i18n="faq_1_a">No. It uses Apple's built-in Vision framework on macOS and iOS, meaning text recognition and VAT extraction happen 100% offline at native speed.</p>
        </div>

        <div class="glass-card rounded-2xl p-6">
          <h3 class="font-bold text-white text-base mb-2" data-i18n="faq_2_q">Which platforms does VAT Vault run on?</h3>
          <p class="text-sm text-zinc-400 leading-relaxed" data-i18n="faq_2_a">It runs natively on Mac (Apple Silicon and Intel) as well as iPhone and iPad thanks to its unified SwiftUI and SwiftData architecture.</p>
        </div>

        <div class="glass-card rounded-2xl p-6">
          <h3 class="font-bold text-white text-base mb-2" data-i18n="faq_3_q">Why does macOS show a warning on first launch?</h3>
          <p class="text-sm text-zinc-400 leading-relaxed" data-i18n="faq_3_a">Because the app is open-source and distributed directly via GitHub. You can bypass Gatekeeper in one second using the Terminal command shown below.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- Section 6: Download & Terminal Command -->
  <section id="download-section" class="py-20 px-4 sm:px-6 border-t border-white/5">
    <div class="max-w-4xl mx-auto glass-panel rounded-3xl p-8 sm:p-12 text-center">
      <h2 class="text-3xl sm:text-5xl font-extrabold text-white mb-3" data-i18n="cta_title">Get VAT Vault.</h2>
      <p class="text-zinc-400 text-sm sm:text-base mb-8" data-i18n="cta_subtitle">Free and open-source for macOS and iOS.</p>

      <div class="flex flex-wrap justify-center gap-4 mb-10">
        <a href="https://github.com/8mir/VAT-Vault/releases" target="_blank" rel="noopener"
           data-i18n="cta_btn_dmg"
           class="px-7 py-4 rounded-2xl bg-emerald-500 hover:bg-emerald-400 text-zinc-950 font-extrabold text-sm transition shadow-lg shadow-emerald-500/25">
          Download for macOS · DMG
        </a>
        <a href="https://github.com/8mir/VAT-Vault" target="_blank" rel="noopener"
           data-i18n="cta_btn_github"
           class="px-7 py-4 rounded-2xl bg-white/5 hover:bg-white/10 border border-white/10 text-white font-bold text-sm transition">
          Open on GitHub ↗
        </a>
      </div>

      <div class="max-w-xl mx-auto text-start bg-zinc-950/90 border border-white/10 rounded-2xl p-4">
        <div class="flex items-center justify-between text-xs text-zinc-400 mb-2">
          <span data-i18n="terminal_hint">To run on macOS after dragging to Applications, run once in Terminal:</span>
          <button onclick="copyCommand()" id="copyBtn" class="text-emerald-400 hover:underline font-mono text-[11px]" data-i18n="copy_btn">Copy Command</button>
        </div>
        <pre dir="ltr" class="font-mono text-xs text-emerald-300 overflow-x-auto text-left bg-white/[0.02] p-3 rounded-xl border border-white/5"><code>xattr -cr /Applications/VAT-Vault.app</code></pre>
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer class="py-10 px-4 sm:px-6 border-t border-white/5 text-xs text-zinc-500">
    <div class="max-w-5xl mx-auto flex flex-col sm:flex-row items-center justify-between gap-4">
      <div class="flex items-center gap-2">
        <span class="font-bold text-zinc-300">VAT Vault</span>
        <span data-i18n="footer_built">· Built with SwiftUI, SwiftData &amp; Vision OCR</span>
      </div>
      <div class="flex items-center gap-5">
        <a href="https://github.com/8mir/VAT-Vault" target="_blank" rel="noopener" class="hover:text-zinc-300 transition">GitHub</a>
        <a href="#preview-gif-section" class="hover:text-zinc-300 transition" data-i18n="nav_preview">Workflow</a>
        <a href="#ocr-section" class="hover:text-zinc-300 transition" data-i18n="nav_features">Toolkit</a>
        <a href="#faq-section" class="hover:text-zinc-300 transition" data-i18n="nav_faq">FAQ</a>
      </div>
    </div>
  </footer>

  <!-- Scripts -->
  <script>
    const translations = {
      en: {
        nav_preview: "Workflow",
        nav_ocr: "Vision OCR",
        nav_quarters: "Quarters",
        nav_privacy: "Privacy",
        nav_features: "Toolkit",
        nav_faq: "FAQ",
        nav_download: "Download",
        hero_title_1: "Your VAT invoices,",
        hero_title_2: "lighter & smarter.",
        hero_subtitle: "VAT Vault captures purchase and sales receipts, extracts dates, merchants, and tax totals automatically via Apple Vision OCR, and organizes them by financial quarters in one tap.",
        btn_mac_download: "Download for macOS",
        btn_source_code: "View Source Code",
        search_placeholder: "Search merchant or TRN number...",
        sidebar_quarters_title: "Financial Quarters · 2026",
        q_all: "All Quarters (Annual)",
        q_1: "First Quarter (Q1)",
        q_1_months: "Jan – Mar",
        q_2: "Second Quarter (Q2)",
        q_2_months: "Apr – Jun",
        q_3: "Third Quarter (Q3)",
        q_3_months: "Jul – Sep",
        q_4: "Fourth Quarter (Q4)",
        q_4_months: "Oct – Dec",
        ocr_accuracy_label: "OCR Extraction Accuracy",
        currency_vat_label: "BHD Reclaimable VAT",
        net_total_label: "Total Net Amount (Excl. VAT):",
        currency_short: "BHD",
        invoice_count_label: "Invoices:",
        scan_btn_text: "Scan New Invoice (OCR)",
        scanning_progress: "Parsing via Vision OCR...",
        metric_1_title: "Supported VAT Rates",
        metric_1_sub: "Auto-Detected",
        metric_2_title: "Storage Engine",
        metric_2_val: "Local Vault",
        metric_3_title: "Avg. Scan Speed",
        metric_4_title: "Tax Export",
        metric_4_sub: "Audit-Ready Returns",
        th_merchant: "Merchant / Vendor",
        th_trn: "Tax Reg. No (TRN)",
        th_date: "Date",
        th_quarter: "Quarter",
        th_net: "Net Amount",
        th_vat: "VAT Amount",
        interactive_hint: "Try it live — the app window above is interactive! Switch quarters or click 'Scan New Invoice'.",
        export_shortcut: "↵ Instant Export",
        gif_sec_title: "From camera roll to tax return in seconds.",
        gif_sec_desc: "Watch how VAT Vault scans receipts, extracts line items via Vision OCR, and groups transactions into quarterly dashboards automatically.",
        ocr_sec_title: "One snap. Every detail in place.",
        ocr_sec_desc: "No more typing receipt numbers into messy spreadsheets. Powered by Apple Vision, VAT Vault reads Arabic and English paper or PDF invoices, extracting vendor names, dates, subtotals, and VAT in milliseconds.",
        ocr_bullet_1: "Automatic multi-format date parsing & financial quarter assignment",
        ocr_bullet_2: "TRN / VAT Registration ID extraction and checksum validation",
        ocr_bullet_3: "Smart separation between net taxable subtotal and VAT amount",
        raw_tokens_label: "RAW CAPTURED TEXT",
        swiftdata_model_label: "PARSED ENTITY · SwiftData @Model",
        parsed_merchant_key: "Merchant:",
        parsed_merchant_val: "Al-Noor Tech Supplies",
        parsed_q_key: "Auto Quarter:",
        parsed_net_key: "Net Subtotal:",
        parsed_vat_key: "VAT Amount:",
        map_title: "See where your tax went.",
        map_desc: "Every expense category and financial quarter becomes an interactive bubble. Bigger bubble, larger reclaimable VAT.",
        bubble_1_title: "Tech & Hardware",
        bubble_1_sub: "BHD VAT · 45%",
        bubble_2_title: "Print & Materials",
        bubble_2_sub: "BHD VAT · 28%",
        bubble_3_title: "Cloud Services",
        bubble_3_sub: "BHD · 16%",
        bubble_4_title: "Operations",
        bubble_4_sub: "BHD · 11%",
        safe_title: "Two rules. No surprises.",
        safe_desc: "Your financial records and receipts never leave your device to any external cloud server.",
        rule_1_title: "100% On-Device Vision OCR Processing",
        rule_1_desc: "Receipts and PDFs are processed directly on Apple Silicon Neural Engine without uploading sensitive invoices to third-party APIs.",
        rule_2_title: "Original Receipts Preserved with Instant Undo",
        rule_2_desc: "High-res receipt attachments are stored alongside structured SwiftData records, and deleted items go to temporary Trash first.",
        toolkit_title: "Small app. Whole accounting toolkit.",
        feat_1_title: "Smart Vision OCR",
        feat_1_desc: "Instant extraction of merchant names, dates, and tax totals from photos & PDFs.",
        feat_2_title: "Quarterly Grouping (Q1–Q4)",
        feat_2_desc: "Automatic sorting into the right fiscal quarter the moment a transaction date is read.",
        feat_3_title: "Duplicate Invoice Guard",
        feat_3_desc: "Smart alerts if the same receipt is scanned twice based on TRN, date, and total.",
        feat_4_title: "Native Glassmorphic UI",
        feat_4_desc: "Crafted in SwiftUI with dark translucent panels that feel right at home on macOS & iOS.",
        feat_5_title: "One-Click Tax Reports",
        feat_5_desc: "Generate clean CSV tables and PDF summaries ready for quarterly VAT return filings.",
        feat_6_title: "Free & Open Source",
        feat_6_desc: "No ads, no subscriptions, and full transparency with complete source code on GitHub.",
        faq_heading: "Questions.",
        faq_1_q: "Does VAT Vault require an internet connection to scan invoices?",
        faq_1_a: "No. It uses Apple's built-in Vision framework on macOS and iOS, meaning text recognition and VAT extraction happen 100% offline at native speed.",
        faq_2_q: "Which platforms does VAT Vault run on?",
        faq_2_a: "It runs natively on Mac (Apple Silicon and Intel) as well as iPhone and iPad thanks to its unified SwiftUI and SwiftData architecture.",
        faq_3_q: "Why does macOS show a warning on first launch?",
        faq_3_a: "Because the app is open-source and distributed directly via GitHub. You can bypass Gatekeeper in one second using the Terminal command shown below.",
        cta_title: "Get VAT Vault.",
        cta_subtitle: "Free and open-source for macOS and iOS.",
        cta_btn_dmg: "Download for macOS · DMG",
        cta_btn_github: "Open on GitHub ↗",
        terminal_hint: "To run on macOS after dragging to Applications, run once in Terminal:",
        copy_btn: "Copy Command",
        copied_btn: "Copied ✓",
        footer_built: "· Built with SwiftUI, SwiftData & Vision OCR",
        qLabels: {
          ALL: "CONSOLIDATED VAT SUMMARY · ALL QUARTERS 2026",
          Q1: "VAT SUMMARY · FIRST QUARTER (Q1 - 2026)",
          Q2: "VAT SUMMARY · SECOND QUARTER (Q2 - 2026)",
          Q3: "VAT SUMMARY · THIRD QUARTER (Q3 - 2026)",
          Q4: "VAT SUMMARY · FOURTH QUARTER (Q4 - 2026)"
        }
      },
      ar: {
        nav_preview: "العرض المتحرك",
        nav_ocr: "المسح الذكي OCR",
        nav_quarters: "الأرباع المالية",
        nav_privacy: "الخصوصية",
        nav_features: "المميزات",
        nav_faq: "الأسئلة",
        nav_download: "تحميل التطبيق",
        hero_title_1: "فواتيرك الضريبية،",
        hero_title_2: "أخف وأذكى.",
        hero_subtitle: "يلتقط VAT Vault فواتير المشتريات والمبيعات، يستخرج التواريخ وأسماء المتاجر ومبالغ الضريبة تلقائياً عبر محرك Apple Vision OCR، ويُرتبها حسب الأرباع المالية بلمسة واحدة.",
        btn_mac_download: "تحميل لنظام macOS",
        btn_source_code: "استعراض الكود المصدري",
        search_placeholder: "ابحث باسم المتجر أو الرقم الضريبي...",
        sidebar_quarters_title: "الأرباع المالية · 2026",
        q_all: "كل الأرباع (السنوي)",
        q_1: "الربع الأول (Q1)",
        q_1_months: "يناير - مارس",
        q_2: "الربع الثاني (Q2)",
        q_2_months: "أبريل - يونيو",
        q_3: "الربع الثالث (Q3)",
        q_3_months: "يوليو - سبتمبر",
        q_4: "الربع الرابع (Q4)",
        q_4_months: "أكتوبر - ديسمبر",
        ocr_accuracy_label: "دقة الاستخراج OCR",
        currency_vat_label: "د.ب (BHD) ضريبة مستردة",
        net_total_label: "إجمالي الفواتير الصافية قبل الضريبة:",
        currency_short: "د.ب",
        invoice_count_label: "عدد الفواتير:",
        scan_btn_text: "مسح فاتورة جديدة (OCR)",
        scanning_progress: "جاري قراءة الفاتورة عبر Vision OCR...",
        metric_1_title: "نسبة الضريبة المعتمدة",
        metric_1_sub: "كشف تلقائي",
        metric_2_title: "حالة الأرشفة",
        metric_2_val: "محفوظ محلياً",
        metric_3_title: "متوسط زمن المسح",
        metric_4_title: "تصدير التقارير",
        metric_4_sub: "جاهز للإقرار الضريبي",
        th_merchant: "المتجر / المورد",
        th_trn: "الرقم الضريبي (TRN)",
        th_date: "التاريخ",
        th_quarter: "الربع",
        th_net: "المبلغ الصافي",
        th_vat: "الضريبة (VAT)",
        interactive_hint: "جرّب بنفسك — النافذة أعلاه تفاعلية بالكامل! اضغط على الأرباع المالية أو زر \"مسح فاتورة جديدة\".",
        export_shortcut: "↵ للتصدير الفوري",
        gif_sec_title: "من صورة الفاتورة إلى الإقرار الضريبي في ثوانٍ.",
        gif_sec_desc: "شاهد كيف يقوم VAT Vault بمسح الإيصالات واستخراج البيانات عبر Vision OCR وتصنيفها داخل الأرباع المالية تلقائياً.",
        ocr_sec_title: "لقطة واحدة. كل تفصيلة في مكانها.",
        ocr_sec_desc: "لا مزيد من إدخال الأرقام يدوياً في جداول معقدة. يعتمد VAT Vault على إطار Apple Vision لقراءة الفواتير الورقية والإلكترونية بالعربية والإنجليزية، واستخلاص اسم المورد، التاريخ، الإجمالي، وقيمة الضريبة في أجزاء من الثانية.",
        ocr_bullet_1: "التعرف التلقائي على صيغ التواريخ المتعددة وتحديد الربع المالي",
        ocr_bullet_2: "استخراج الرقم الضريبي (TRN / VAT ID) والتحقق من صحته",
        ocr_bullet_3: "فصل ذكي بين المبلغ الخاضع للضريبة والضريبة المضافة",
        raw_tokens_label: "النص الخام الملتقط · RAW TEXT",
        swiftdata_model_label: "الكائن المستخرج · SwiftData @Model",
        parsed_merchant_key: "المتجر:",
        parsed_merchant_val: "شركة النور للتقنية",
        parsed_q_key: "الربع المالي التلقائي:",
        parsed_net_key: "الصافي:",
        parsed_vat_key: "قيمة الضريبة:",
        map_title: "شاهد أين تذهب ضرائبك.",
        map_desc: "يتحول كل قطاع مصروفات وربع مالي إلى فقاعة تفاعلية؛ كلما كبرت الفقاعة زاد حجم الضريبة المستحقة أو المستردة بداخلها.",
        bubble_1_title: "معدات وأجهزة تقنية",
        bubble_1_sub: "د.ب ضريبة · 45%",
        bubble_2_title: "مواد خام ومطبوعات",
        bubble_2_sub: "د.ب ضريبة · 28%",
        bubble_3_title: "خدمات سحابية",
        bubble_3_sub: "د.ب · 16%",
        bubble_4_title: "مصاريف تشغيلية",
        bubble_4_sub: "د.ب · 11%",
        safe_title: "قاعدتان ذهبيتان. بدون مفاجآت.",
        safe_desc: "بياناتك المالية وفواتيرك لا تغادر جهازك أبداً إلى أي خادم خارجي.",
        rule_1_title: "المعالجة بالكامل على الجهاز (On-Device OCR)",
        rule_1_desc: "تتم قراءة الصور والملفات عبر شريحة Apple Silicon مباشرة داخل جهازك دون إرسال صور الفواتير إلى أي واجهة برمجية سحابية خارجية.",
        rule_2_title: "الفواتير الأصلية محفوظة دائماً مع إمكانية التراجع",
        rule_2_desc: "تُحفظ نسخة من صورة الفاتورة الأصلية بجانب البيانات المستخرجة في قاعدة بيانات SwiftData، وعند الحذف تنتقل لسلة المهملات المؤقتة أولاً.",
        toolkit_title: "تطبيق خفيف. أدوات محاسبية متكاملة.",
        feat_1_title: "المسح البصري الذكي (Vision OCR)",
        feat_1_desc: "التقاط فوري لأسماء المتاجر والتواريخ والإجماليات من الصور وملفات PDF.",
        feat_2_title: "تجميع الأرباع المالية (Q1–Q4)",
        feat_2_desc: "فرز آلي لكل فاتورة في الربع السنوي المناسب بمجرد قراءة تاريخ المعاملة.",
        feat_3_title: "كشف الفواتير المكررة",
        feat_3_desc: "تنبيه ذكي في حال رفع نفس الفاتورة مرتين بناءً على الرقم الضريبي والتاريخ والمبلغ.",
        feat_4_title: "واجهة Glassmorphic أصلية",
        feat_4_desc: "مصمم بلغة SwiftUI ليبدو جزءاً أصيلاً من نظام macOS و iOS بالوضع الداكن.",
        feat_5_title: "تصدير تقارير الضريبة",
        feat_5_desc: "توليد ملفات CSV وملخصات PDF جاهزة للتقديم في الإقرارات الضريبية الفصلية.",
        feat_6_title: "مفتوح المصدر ومجاني",
        feat_6_desc: "بدون إعلانات أو اشتراكات شهرية، مع توفر الكود الكامل على GitHub.",
        faq_heading: "أسئلة شائعة.",
        faq_1_q: "هل يحتاج التطبيق إلى اتصال بالإنترنت لقراءة الفواتير؟",
        faq_1_a: "لا. يعتمد التطبيق على مكتبة Apple Vision المدمجة في نظام macOS و iOS، مما يعني أن قراءة النصوص واستخراج الضريبة يتم محلياً بنسبة 100% وبسرعة فائقة.",
        faq_2_q: "ما الأنظمة التي يعمل عليها VAT Vault؟",
        faq_2_a: "يعمل على أجهزة Mac (بمعالجات Apple Silicon و Intel) بالإضافة إلى أجهزة iPhone و iPad بفضل بناء المشروع باستخدام SwiftUI و SwiftData.",
        faq_3_q: "لماذا تظهر رسالة تنبيه من macOS عند فتح التطبيق لأول مرة؟",
        faq_3_a: "لأن التطبيق مفتوح المصدر ويتم تحميله مباشرة من GitHub؛ يمكنك تخطي تنبيه Gatekeeper بسهولة عبر تنفيذ أمر واحد في الـ Terminal كما هو موضح في قسم التحميل بالأسفل.",
        cta_title: "احصل على VAT Vault.",
        cta_subtitle: "مجاني ومفتوح المصدر لنظامي macOS و iOS.",
        cta_btn_dmg: "تحميل نسخة macOS · DMG",
        cta_btn_github: "فتح المشروع على GitHub ↗",
        terminal_hint: "لتشغيل التطبيق مباشرة على macOS بعد نقله لمجلد Applications:",
        copy_btn: "نسخ الأمر (Copy)",
        copied_btn: "تم النسخ ✓",
        footer_built: "· صُمم وطُوّر باستخدام SwiftUI & Vision OCR",
        qLabels: {
          ALL: "ملخص الضريبة المجمّع · جميع الأرباع 2026",
          Q1: "ملخص الضريبة · الربع الأول (Q1 - 2026)",
          Q2: "ملخص الضريبة · الربع الثاني (Q2 - 2026)",
          Q3: "ملخص الضريبة · الربع الثالث (Q3 - 2026)",
          Q4: "ملخص الضريبة · الربع الرابع (Q4 - 2026)"
        }
      }
    };

    const invoices = [
      { merchant_ar: "شركة النور للتقنية", merchant_en: "Al-Noor Tech Supplies", trn: "200019482700002", date: "2026-02-14", quarter: "Q1", net: 320.000, vat: 32.000 },
      { merchant_ar: "مطبعة الخليج الحديثة", merchant_en: "Gulf Modern Press", trn: "200055391000003", date: "2026-03-22", quarter: "Q1", net: 185.000, vat: 18.500 },
      { merchant_ar: "أبل ستور - معدات مكتبية", merchant_en: "Apple Store — Office Gear", trn: "200099123400001", date: "2026-05-10", quarter: "Q2", net: 540.000, vat: 54.000 },
      { merchant_ar: "خدمات أمازون السحابية AWS", merchant_en: "Amazon Web Services AWS", trn: "200077211900004", date: "2026-07-19", quarter: "Q3", net: 210.500, vat: 21.050 },
      { merchant_ar: "مؤسسة البحرين للتوريدات", merchant_en: "Bahrain Supply Corp", trn: "200033817200002", date: "2026-10-04", quarter: "Q4", net: 390.000, vat: 39.000 },
      { merchant_ar: "مركز التصميم الرقمي", merchant_en: "Digital Design Studio", trn: "200061900400005", date: "2026-10-25", quarter: "Q4", net: 200.000, vat: 20.000 }
    ];

    const extraScanPool = [
      { merchant_ar: "شركة الاتصالات المتقدمة", merchant_en: "Advanced Telecom Co.", trn: "200088441200001", date: "2026-11-01", quarter: "Q4", net: 95.000, vat: 9.500 },
      { merchant_ar: "قرطاسية ومستلزمات المكتب", merchant_en: "Office Stationery Hub", trn: "200044198200009", date: "2026-06-12", quarter: "Q2", net: 64.000, vat: 6.400 },
      { merchant_ar: "استوديو الإبداع للطباعة", merchant_en: "Creative Print Studio", trn: "200011938400007", date: "2026-08-29", quarter: "Q3", net: 150.000, vat: 15.000 }
    ];

    let currentLang = 'en';
    let currentQuarter = 'ALL';
    let searchQuery = '';

    const candidateGifPaths = [
      "./demo.gif",
      "./preview.gif",
      "./vat-vault.gif",
      "./assets/demo.gif",
      "./assets/preview.gif",
      "./screenshots/demo.gif",
      "https://raw.githubusercontent.com/8mir/VAT-Vault/main/demo.gif",
      "https://raw.githubusercontent.com/8mir/VAT-Vault/main/preview.gif",
      "https://raw.githubusercontent.com/8mir/VAT-Vault/main/assets/demo.gif"
    ];

    function tryLoadRepoAnimation() {
      const imgEl = document.getElementById('repoAnimatedPreview');
      let idx = 0;
      function tryNext() {
        if (idx >= candidateGifPaths.length) return;
        const testImg = new Image();
        const url = candidateGifPaths[idx++];
        testImg.onload = () => {
          imgEl.src = url;
          imgEl.classList.remove('hidden');
          document.getElementById('svgAnimatedShowcase').classList.add('mt-4');
        };
        testImg.onerror = tryNext;
        testImg.src = url;
      }
      tryNext();
    }

    function applyLanguage(lang) {
      currentLang = lang;
      const htmlEl = document.documentElement;
      htmlEl.setAttribute('lang', lang);
      htmlEl.setAttribute('dir', lang === 'ar' ? 'rtl' : 'ltr');

      document.getElementById('langToggleText').textContent = lang === 'ar' ? 'English' : 'العربية';
      document.getElementById('liveSearchInput').placeholder = translations[lang].search_placeholder;

      document.querySelectorAll('[data-i18n]').forEach(el => {
        const key = el.getAttribute('data-i18n');
        if (translations[lang][key]) {
          el.textContent = translations[lang][key];
        }
      });

      document.getElementById('activeQuarterLabel').textContent = translations[lang].qLabels[currentQuarter];
      renderInvoices();
    }

    function toggleLanguage() {
      const nextLang = currentLang === 'en' ? 'ar' : 'en';
      applyLanguage(nextLang);
    }

    function renderInvoices() {
      const tbody = document.getElementById('invoiceTableBody');
      const filtered = invoices.filter(item => {
        const matchesQ = (currentQuarter === 'ALL' || item.quarter === currentQuarter);
        const nameAr = item.merchant_ar.toLowerCase();
        const nameEn = item.merchant_en.toLowerCase();
        const q = searchQuery.toLowerCase();
        const matchesS = nameAr.includes(q) || nameEn.includes(q) || item.trn.includes(q);
        return matchesQ && matchesS;
      });

      tbody.innerHTML = filtered.map(inv => {
        const displayName = currentLang === 'ar' ? inv.merchant_ar : inv.merchant_en;
        return `
          <tr class="hover:bg-white/[0.03] transition">
            <td class="py-3 font-sans font-bold text-white pe-4">${displayName}</td>
            <td class="py-3 text-zinc-400 px-4">${inv.trn}</td>
            <td class="py-3 text-zinc-400 px-4">${inv.date}</td>
            <td class="py-3 px-4"><span class="px-2 py-0.5 rounded bg-white/5 text-zinc-300 text-[10px]">${inv.quarter}</span></td>
            <td class="py-3 text-zinc-300 px-4">${inv.net.toFixed(3)}</td>
            <td class="py-3 text-emerald-400 font-bold ps-4">+${inv.vat.toFixed(3)}</td>
          </tr>
        `;
      }).join('');

      const totalNet = filtered.reduce((acc, cur) => acc + cur.net, 0);
      const totalVat = filtered.reduce((acc, cur) => acc + cur.vat, 0);

      document.getElementById('totalNetDisplay').textContent = totalNet.toFixed(3);
      document.getElementById('totalVatDisplay').textContent = totalVat.toFixed(3);
      document.getElementById('invoiceCountDisplay').textContent = filtered.length;
    }

    function selectQuarter(q) {
      currentQuarter = q;
      document.querySelectorAll('.q-btn').forEach(btn => {
        if (btn.getAttribute('data-q') === q) {
          btn.className = "q-btn w-full flex items-center justify-between px-3 py-2 rounded-xl text-xs font-bold bg-emerald-500/15 text-emerald-300 border border-emerald-500/30 transition";
        } else {
          btn.className = "q-btn w-full flex items-center justify-between px-3 py-2 rounded-xl text-xs text-zinc-400 hover:bg-white/5 transition";
        }
      });

      document.getElementById('activeQuarterLabel').textContent = translations[currentLang].qLabels[q];
      renderInvoices();
    }

    function simulateNewScan() {
      const btn = document.getElementById('scanBtn');
      const btnText = document.getElementById('scanBtnText');
      btnText.textContent = translations[currentLang].scanning_progress;
      btn.disabled = true;

      setTimeout(() => {
        const randomNum = Math.floor(Math.random() * 90 + 10);
        const nextItem = extraScanPool.shift() || {
          merchant_ar: "فاتورة مشتريات جديدة #" + randomNum,
          merchant_en: "New Purchase Receipt #" + randomNum,
          trn: "2000" + Math.floor(10000000000 + Math.random() * 89999999999),
          date: "2026-11-02",
          quarter: currentQuarter === 'ALL' ? 'Q4' : currentQuarter,
          net: 120.000,
          vat: 12.000
        };
        invoices.unshift(nextItem);
        renderInvoices();
        btnText.textContent = translations[currentLang].scan_btn_text;
        btn.disabled = false;
      }, 650);
    }

    function copyCommand() {
      navigator.clipboard.writeText('xattr -cr /Applications/VAT-Vault.app');
      const btn = document.getElementById('copyBtn');
      btn.textContent = translations[currentLang].copied_btn;
      setTimeout(() => { btn.textContent = translations[currentLang].copy_btn; }, 2000);
    }

    document.getElementById('liveSearchInput').addEventListener('input', (e) => {
      searchQuery = e.target.value;
      renderInvoices();
    });

    // Enable Grab-and-Drag scrolling for tables and overflow elements
    const slider = document.getElementById('dragTableScroll');
    let isDown = false;
    let startX;
    let scrollLeft;

    slider.addEventListener('mousedown', (e) => {
      isDown = true;
      slider.classList.add('active');
      startX = e.pageX - slider.offsetLeft;
      scrollLeft = slider.scrollLeft;
    });
    slider.addEventListener('mouseleave', () => {
      isDown = false;
    });
    slider.addEventListener('mouseup', () => {
      isDown = false;
    });
    slider.addEventListener('mousemove', (e) => {
      if(!isDown) return;
      e.preventDefault();
      const x = e.pageX - slider.offsetLeft;
      const walk = (x - startX) * 2; // scroll-fast multiplier
      slider.scrollLeft = scrollLeft - walk;
    });

    applyLanguage('en');
    tryLoadRepoAnimation();
  </script>
</body>
</html>