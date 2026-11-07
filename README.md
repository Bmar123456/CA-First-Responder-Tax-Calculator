# CA-First-Responder-Tax-Calculator
2026 California first responder tax &amp; take-home calculator. Models biweekly &amp; annual pay, pension systems (CalPERS, LAFPP, OCERS, LACERA, custom), 1.5x overtime exemption rules, 457(b) deferrals, and dual-income households.
<!DOCTYPE html>
<html lang="en" class="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0">
  <title>California First Responder Tax & Paycheck Calculator (2026)</title>
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          fontFamily: {
            sans: ['Inter', 'system-ui', '-apple-system', 'sans-serif'],
          },
          colors: {
            brand: {
              50: '#f0f9ff',
              100: '#e0f2fe',
              500: '#0ea5e9',
              600: '#0284c7',
              700: '#0369a1',
              900: '#0c4a6e',
            }
          }
        }
      }
    }
  </script>
  <style>
    body { font-family: 'Inter', system-ui, -apple-system, sans-serif; -webkit-font-smoothing: antialiased; }
    input[type=number]::-webkit-inner-spin-button, 
    input[type=number]::-webkit-outer-spin-button { 
      -webkit-appearance: none; 
      margin: 0; 
    }
    input[type=number] {
      -moz-appearance: textfield;
    }
    /* Touch & Desktop Optimized Range Sliders */
    input[type=range] {
      -webkit-appearance: none;
      width: 100%;
      height: 6px;
      background: #e2e8f0;
      border-radius: 9999px;
      outline: none;
      transition: background 0.15s ease;
    }
    .dark input[type=range] {
      background: #334155;
    }
    input[type=range]::-webkit-slider-thumb {
      -webkit-appearance: none;
      appearance: none;
      width: 18px;
      height: 18px;
      border-radius: 50%;
      background: #0284c7;
      cursor: pointer;
      border: 2px solid #ffffff;
      box-shadow: 0 1px 4px rgba(0,0,0,0.25);
      transition: transform 0.1s ease, background-color 0.15s ease;
    }
    .dark input[type=range]::-webkit-slider-thumb {
      border: 2px solid #0f172a;
      background: #38bdf8;
    }
    input[type=range]::-webkit-slider-thumb:hover,
    input[type=range]::-webkit-slider-thumb:active {
      transform: scale(1.2);
    }
    .no-scrollbar::-webkit-scrollbar {
      display: none;
    }
    .no-scrollbar {
      -ms-overflow-style: none;
      scrollbar-width: none;
    }
    @media print {
      header, .no-print { display: none !important; }
      body { background: white !important; color: black !important; }
      .print-shadow-none { box-shadow: none !important; border-color: #cbd5e1 !important; }
    }
  </style>
</head>
<body class="bg-slate-50 dark:bg-slate-950 text-slate-800 dark:text-slate-100 min-h-screen antialiased flex flex-col transition-colors duration-200">

  <!-- Top Minimalist Header -->
  <header class="bg-white dark:bg-slate-900 border-b border-slate-200/80 dark:border-slate-800 sticky top-0 z-30 shadow-xs transition-colors duration-200">
    <div class="max-w-7xl mx-auto px-3 sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-16 gap-2">
        
        <!-- Logo & Title -->
        <div class="flex items-center space-x-2.5 sm:space-x-3 min-w-0">
          <div class="w-9 h-9 sm:w-10 sm:h-10 rounded-xl bg-slate-900 dark:bg-sky-600 flex items-center justify-center text-white shadow-xs shrink-0 ring-1 ring-slate-800 dark:ring-sky-500">
            <svg class="w-5 h-5 text-sky-400 dark:text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z"/>
            </svg>
          </div>
          <div class="min-w-0">
            <div class="flex items-center space-x-1.5 sm:space-x-2 truncate">
              <h1 class="text-sm sm:text-base font-bold text-slate-900 dark:text-white tracking-tight truncate">
                CA First Responder Tax Calculator
              </h1>
              <span class="inline-flex items-center px-1.5 sm:px-2 py-0.5 rounded text-[10px] sm:text-[11px] font-semibold bg-sky-50 dark:bg-sky-950 text-sky-700 dark:text-sky-300 border border-sky-200 dark:border-sky-800 shrink-0">
                2026 Tax Rules
              </span>
            </div>
            <p class="text-[11px] text-slate-500 dark:text-slate-400 hidden sm:block truncate">CalPERS, LAFPP, OCERS, LACERA & Custom Pensions, Spouse Income, 457(b) & CA Taxes</p>
          </div>
        </div>

        <!-- Controls Toolbar: View Switcher, Day/Night Toggle, Print, Reset -->
        <div class="flex items-center space-x-1 sm:space-x-2 shrink-0">
          
          <!-- View Switcher (Split View / Biweekly Check / Annual Return) -->
          <div class="inline-flex p-0.5 sm:p-1 bg-slate-100 dark:bg-slate-800 rounded-xl border border-slate-200 dark:border-slate-700 text-xs font-medium">
            <button id="viewTabBoth" class="px-2.5 sm:px-3 py-1 sm:py-1.5 rounded-lg bg-white dark:bg-slate-700 text-slate-900 dark:text-white shadow-xs transition-all font-semibold">
              Split View
            </button>
            <button id="viewTabBiweekly" class="px-2.5 sm:px-3 py-1 sm:py-1.5 rounded-lg text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white transition-all">
              Biweekly Check
            </button>
            <button id="viewTabAnnual" class="px-2.5 sm:px-3 py-1 sm:py-1.5 rounded-lg text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white transition-all">
              Annual Return
            </button>
          </div>

          <!-- Day / Night Mode Toggle -->
          <button id="btnThemeToggle" class="p-2 text-slate-600 dark:text-slate-300 hover:text-slate-900 dark:hover:text-white rounded-lg hover:bg-slate-100 dark:hover:bg-slate-800 border border-slate-200 dark:border-slate-700 transition-colors" title="Toggle Day/Night Mode" aria-label="Toggle Day/Night Mode">
            <svg id="themeIconSun" class="w-4 h-4 hidden text-amber-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" />
            </svg>
            <svg id="themeIconMoon" class="w-4 h-4 text-slate-700" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
            </svg>
          </button>

          <!-- Print Button -->
          <button id="btnPrint" class="hidden md:inline-flex items-center px-2.5 py-1.5 text-xs font-semibold rounded-lg text-slate-700 dark:text-slate-300 bg-white dark:bg-slate-800 border border-slate-200 dark:border-slate-700 hover:bg-slate-50 dark:hover:bg-slate-700 shadow-2xs transition-colors" title="Print paycheck summary">
            <svg class="w-3.5 h-3.5 mr-1 text-slate-400 dark:text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 17h2a2 2 0 002-2v-4a2 2 0 00-2-2H5a2 2 0 00-2 2v4a2 2 0 002 2h2m2 4h6a2 2 0 002-2v-4a2 2 0 00-2-2H9a2 2 0 00-2 2v4a2 2 0 002 2zm8-12V5a2 2 0 00-2-2H9a2 2 0 00-2 2v4h10z"/></svg>
            Print
          </button>

          <!-- Reset Button -->
          <button id="btnReset" class="p-2 text-slate-400 hover:text-slate-700 dark:hover:text-slate-200 rounded-lg hover:bg-slate-100 dark:hover:bg-slate-800 border border-transparent dark:border-slate-800 transition-colors" title="Reset all fields to defaults" aria-label="Reset fields">
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
            </svg>
          </button>
        </div>
      </div>
    </div>
  </header>

  <!-- Quick Profiles Sub-Header -->
  <div class="bg-white dark:bg-slate-900 border-b border-slate-200/80 dark:border-slate-800 py-2 px-3 sm:px-6 lg:px-8 text-xs no-print transition-colors duration-200">
    <div class="max-w-7xl mx-auto flex items-center justify-between gap-3 overflow-x-auto no-scrollbar">
      <div class="flex items-center space-x-1.5 sm:space-x-2 shrink-0">
        <span class="font-semibold text-slate-700 dark:text-slate-300 whitespace-nowrap text-[11px] sm:text-xs">Quick Profiles:</span>
        <button id="presetPolice" class="px-2.5 py-1 rounded-lg bg-slate-100 dark:bg-slate-800 hover:bg-sky-50 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-200 border border-slate-200 dark:border-slate-700 font-medium whitespace-nowrap transition-colors text-[11px] sm:text-xs">
          🚓 Police Officer ($62.50/hr, 80h + 12h OT)
        </button>
        <button id="presetFire" class="px-2.5 py-1 rounded-lg bg-slate-100 dark:bg-slate-800 hover:bg-amber-50 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-200 border border-slate-200 dark:border-slate-700 font-medium whitespace-nowrap transition-colors text-[11px] sm:text-xs">
          🚒 Firefighter (56h shift, 112h + 24h OT)
        </button>
        <button id="presetParamedic" class="px-2.5 py-1 rounded-lg bg-slate-100 dark:bg-slate-800 hover:bg-emerald-50 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-200 border border-slate-200 dark:border-slate-700 font-medium whitespace-nowrap transition-colors text-[11px] sm:text-xs">
          🚑 Paramedic / EMT ($45/hr, 84h + 14h OT)
        </button>
      </div>
      <div class="hidden lg:flex items-center space-x-4 text-slate-500 dark:text-slate-400 text-[11px] shrink-0">
        <span class="inline-flex items-center"><span class="w-2 h-2 rounded-full bg-emerald-500 mr-1.5"></span>2026 457(b) Limit: $24,500</span>
        <span class="inline-flex items-center"><span class="w-2 h-2 rounded-full bg-amber-500 mr-1.5"></span>Base Pay Only Pension Rule</span>
      </div>
    </div>
  </div>

  <!-- Main Container -->
  <main class="flex-1 max-w-7xl w-full mx-auto px-3 sm:px-6 lg:px-8 py-5 sm:py-6 space-y-5 sm:space-y-6">

    <!-- Primary Pension System Selection Banner (NOW INCLUDES "OTHER / CUSTOM") -->
    <div class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-xs p-4 sm:p-5 print-shadow-none transition-colors duration-200">
      <div class="flex flex-col lg:flex-row lg:items-start lg:justify-between gap-3.5 sm:gap-4">
        <div class="space-y-2 flex-1">
          <div class="flex items-center justify-between sm:justify-start sm:space-x-3">
            <label for="pensionSystemSelect" class="text-xs font-bold uppercase tracking-wider text-slate-700 dark:text-slate-300">
              Primary Earner Pension System (Drop Down)
            </label>
            <span id="pensionBadge" class="inline-flex items-center px-2 py-0.5 rounded text-[11px] font-semibold bg-emerald-50 dark:bg-emerald-950/70 text-emerald-700 dark:text-emerald-300 border border-emerald-200 dark:border-emerald-800/80">
              <svg class="w-3 h-3 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/></svg>
              Section 218 Social Security Exempt
            </span>
          </div>

          <!-- Primary Pension Dropdown with "Other / Custom" (USER SPEC) -->
          <div class="relative max-w-2xl">
            <select id="pensionSystemSelect" class="w-full pl-3.5 pr-10 py-2.5 bg-slate-50 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-sm font-semibold text-slate-900 dark:text-white focus:outline-none focus:ring-2 focus:ring-sky-500 focus:bg-white dark:focus:bg-slate-800 transition-all appearance-none cursor-pointer shadow-2xs">
              <option value="calpers" selected>CalPERS Safety (13.75% Base Only - No Social Security)</option>
              <option value="lafpp_tier6">LAFPP Tier 6 - LAPD / LAFD (11.00% Base Only - No Social Security)</option>
              <option value="ocers_plan_v">OCERS Plan V - Safety 2.7%@57 (14.61% Base Only - No Social Security)</option>
              <option value="ocers_plan_u">OCERS Plan U - General 2.5%@67 (10.50% Base Only + Social Security)</option>
              <option value="lacera_plan_c">LACERA Plan C - Safety 2.7%@57 (15.36% Base Only - No Social Security)</option>
              <option value="lacera_plan_g">LACERA Plan G - General 2.5%@67 (9.22% Base Only + Social Security)</option>
              <option value="social_security">Social Security Only (FICA 6.2% up to $184,500 - No Pension)</option>
              <option value="other">Other / Custom Pension System (Set Your Own Numbers)</option>
            </select>
            <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-3 text-slate-500 dark:text-slate-400">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"/></svg>
            </div>
          </div>

          <!-- Primary Custom Pension Inputs Container (shown only when "Other" is selected) -->
          <div id="primaryCustomPensionContainer" class="hidden mt-3 p-3.5 rounded-xl bg-slate-50 dark:bg-slate-800/80 border border-slate-200 dark:border-slate-700 space-y-3">
            <span class="text-xs font-bold text-slate-800 dark:text-slate-200 block">Custom Pension System Configuration</span>
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 text-xs">
              <div>
                <label for="primaryCustomPensionName" class="block font-medium text-slate-700 dark:text-slate-300 mb-1">Pension System Name</label>
                <input type="text" id="primaryCustomPensionName" value="Custom Public Pension" class="w-full px-3 py-1.5 bg-white dark:bg-slate-700 border border-slate-300 dark:border-slate-600 rounded-lg font-semibold text-slate-900 dark:text-white text-xs">
              </div>
              <div>
                <label for="primaryCustomPensionRate" class="block font-medium text-slate-700 dark:text-slate-300 mb-1">Employee Contribution Rate (%)</label>
                <div class="relative">
                  <input type="number" id="primaryCustomPensionRate" value="12.00" step="0.25" min="0" max="40" class="w-full pl-3 pr-7 py-1.5 bg-white dark:bg-slate-700 border border-slate-300 dark:border-slate-600 rounded-lg font-semibold text-slate-900 dark:text-white text-xs">
                  <span class="absolute inset-y-0 right-0 pr-2.5 flex items-center text-slate-400 text-xs">%</span>
                </div>
              </div>
            </div>
            <div class="flex flex-wrap items-center gap-4 text-xs pt-1">
              <label class="flex items-center space-x-1.5 cursor-pointer text-slate-700 dark:text-slate-300">
                <input type="checkbox" id="primaryCustomPensionBaseOnly" checked class="rounded border-slate-300 dark:border-slate-600 text-sky-600 focus:ring-sky-500 w-3.5 h-3.5">
                <span>Deduct from Base Pay Only (Exempt Overtime)</span>
              </label>
              <label class="flex items-center space-x-1.5 cursor-pointer text-slate-700 dark:text-slate-300">
                <input type="checkbox" id="primaryCustomPensionSocialSecurity" class="rounded border-slate-300 dark:border-slate-600 text-sky-600 focus:ring-sky-500 w-3.5 h-3.5">
                <span>Participates in Social Security (6.2%)</span>
              </label>
            </div>
          </div>
        </div>

        <!-- Explanatory Context Pill -->
        <div id="pensionNotice" class="text-xs text-slate-600 dark:text-slate-300 bg-slate-50 dark:bg-slate-800/60 p-3 sm:p-3.5 rounded-xl border border-slate-200 dark:border-slate-700/80 lg:max-w-md">
          <p><strong class="text-slate-900 dark:text-white">California Pension Rule:</strong> Employee contribution is fixed on <span class="underline decoration-sky-400 font-semibold text-slate-900 dark:text-white">base pay only</span>. Overtime pay is strictly non-pensionable (PEPRA & California public retirement statutes), and safety members are exempt from the 6.2% Social Security tax under Section 218.</p>
        </div>
      </div>
    </div>

    <!-- Main Interactive Grid -->
    <div class="grid grid-cols-1 lg:grid-cols-12 gap-5 sm:gap-6 items-start">

      <!-- LEFT COLUMN: INPUT PARAMETERS (5 cols) -->
      <div class="lg:col-span-5 space-y-5 no-print">

        <!-- Card 1: Hourly Rate, Overtime & Biweekly Hours -->
        <div class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-xs overflow-hidden transition-colors duration-200">
          <div class="px-4 sm:px-5 py-3 border-b border-slate-100 dark:border-slate-800 bg-slate-50/60 dark:bg-slate-800/40 flex items-center justify-between">
            <h2 class="text-xs font-bold uppercase tracking-wider text-slate-900 dark:text-slate-100 flex items-center">
              <span class="w-2 h-2 rounded-full bg-sky-500 mr-2"></span>
              1. Primary Pay Rates & Hours
            </h2>
            <span class="text-[11px] text-slate-400 dark:text-slate-400 font-mono">Biweekly (26/yr)</span>
          </div>

          <div class="p-4 sm:p-5 space-y-4">
            <!-- Hourly Rate & OT Display -->
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
              <div>
                <label for="hourlyRateInput" class="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">
                  Hourly Rate ($)
                </label>
                <div class="relative">
                  <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-slate-400 dark:text-slate-400 text-sm">$</span>
                  <input type="number" id="hourlyRateInput" value="62.50" step="0.25" min="0" class="w-full pl-7 pr-3 py-2 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-sm font-semibold text-slate-900 dark:text-white focus:ring-2 focus:ring-sky-500 focus:border-sky-500 transition-all">
                </div>
                <div class="pt-2">
                  <input type="range" id="hourlyRateSlider" min="20" max="150" step="0.5" value="62.5" aria-label="Hourly rate slider">
                </div>
              </div>

              <!-- Overtime Rate (Auto 1.5x) -->
              <div>
                <label class="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">
                  Overtime Rate (1.5×)
                </label>
                <div class="py-2 px-3 bg-sky-50/80 dark:bg-sky-950/40 border border-sky-200 dark:border-sky-800 rounded-xl flex items-center justify-between">
                  <span class="text-xs font-medium text-sky-800 dark:text-sky-300">Time & Half</span>
                  <span id="otRateDisplay" class="text-sm font-bold text-sky-950 dark:text-sky-100 font-mono">$93.75/hr</span>
                </div>
                <p class="text-[10px] text-slate-400 dark:text-slate-400 mt-2 text-right">Auto-calculated (1.5 × base)</p>
              </div>
            </div>

            <!-- Hours: Base & Overtime -->
            <div class="grid grid-cols-2 gap-3">
              <div>
                <div class="flex items-center justify-between mb-1">
                  <label for="baseHoursInput" class="text-xs font-semibold text-slate-700 dark:text-slate-300">Base Hours</label>
                  <span class="text-[10px] text-slate-400 dark:text-slate-400">80h / 112h</span>
                </div>
                <input type="number" id="baseHoursInput" value="80" step="1" min="0" max="168" class="w-full px-3 py-2 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-sm font-semibold text-slate-900 dark:text-white focus:ring-2 focus:ring-sky-500 focus:border-sky-500 transition-all">
              </div>

              <div>
                <div class="flex items-center justify-between mb-1">
                  <label for="otHoursInput" class="text-xs font-semibold text-slate-700 dark:text-slate-300">Overtime Hours</label>
                  <span class="text-[10px] text-orange-600 dark:text-orange-400 font-medium">Box for OT</span>
                </div>
                <input type="number" id="otHoursInput" value="12" step="0.5" min="0" max="150" class="w-full px-3 py-2 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-sm font-semibold text-slate-900 dark:text-white focus:ring-2 focus:ring-orange-500 focus:border-orange-500 transition-all">
              </div>
            </div>

            <div class="pt-0.5">
              <div class="flex justify-between items-center text-[11px] text-slate-500 dark:text-slate-400 mb-1">
                <span>Overtime Hours Slider:</span>
                <span class="font-mono text-orange-600 dark:text-orange-400 font-semibold" id="sliderOtVal">12 hrs</span>
              </div>
              <input type="range" id="otHoursSlider" min="0" max="60" step="1" value="12" aria-label="Overtime hours slider">
            </div>

            <!-- Mini Live Gross Pay Breakdown Box -->
            <div class="bg-slate-50 dark:bg-slate-800/50 rounded-xl p-3 border border-slate-200/90 dark:border-slate-700/70 space-y-1.5 text-xs">
              <div class="flex justify-between items-center text-slate-600 dark:text-slate-300">
                <span>Base Salary (<span id="calcBaseFormula">80 hrs @ $62.50</span>):</span>
                <span id="calcBasePay" class="font-semibold text-slate-900 dark:text-white font-mono">$5,000.00</span>
              </div>
              <div class="flex justify-between items-center text-slate-600 dark:text-slate-300">
                <span>Overtime (<span id="calcOtFormula">12 hrs @ $93.75</span>):</span>
                <span id="calcOtPay" class="font-semibold text-orange-600 dark:text-orange-400 font-mono">$1,125.00</span>
              </div>
              <div class="pt-1.5 border-t border-slate-200 dark:border-slate-700 flex justify-between items-center text-sm font-bold text-slate-900 dark:text-white">
                <span>Total Primary Gross Pay:</span>
                <span id="calcTotalGross" class="text-sky-700 dark:text-sky-400 text-base font-mono">$6,125.00</span>
              </div>
            </div>
          </div>
        </div>

        <!-- Card 2: Pre-Tax Deductions (Pension, 457b, Pre-Tax Deductions) -->
        <div class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-xs overflow-hidden transition-colors duration-200">
          <div class="px-4 sm:px-5 py-3 border-b border-slate-100 dark:border-slate-800 bg-slate-50/60 dark:bg-slate-800/40 flex items-center justify-between">
            <h2 class="text-xs font-bold uppercase tracking-wider text-slate-900 dark:text-slate-100 flex items-center">
              <span class="w-2 h-2 rounded-full bg-emerald-500 mr-2"></span>
              2. Primary Pre-Tax & Pension
            </h2>
            <span class="text-[11px] text-emerald-700 dark:text-emerald-300 bg-emerald-50 dark:bg-emerald-950/60 px-2 py-0.5 rounded font-medium border border-emerald-200 dark:border-emerald-800/70">Reduces Tax Base</span>
          </div>

          <div class="p-4 sm:p-5 space-y-4">

            <!-- Primary Pension Contribution Box -->
            <div id="calpersContainer" class="p-3.5 rounded-xl bg-amber-50/80 dark:bg-amber-950/30 border border-amber-200 dark:border-amber-800/60 space-y-1.5">
              <div class="flex items-center justify-between">
                <div>
                  <span id="labelPrimaryPensionTitle" class="text-xs font-bold text-amber-950 dark:text-amber-200 flex items-center">
                    <svg class="w-4 h-4 mr-1 text-amber-700 dark:text-amber-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>
                    Pension Contribution
                  </span>
                  <span id="labelPrimaryPensionRate" class="text-[11px] text-amber-900 dark:text-amber-300">13.75% on <span class="underline decoration-amber-500 font-semibold">base pay only</span></span>
                </div>
                <div class="text-right">
                  <div id="biweeklyCalpersDisplay" class="text-sm font-bold text-amber-950 dark:text-amber-100 font-mono">$687.50</div>
                  <span class="text-[10px] text-amber-700 dark:text-amber-400">biweekly</span>
                </div>
              </div>
              <p class="text-[11px] text-amber-800 dark:text-amber-300 leading-tight">
                * Overtime earnings ($<span id="calpersOtSavings">1,125.00</span>) are exempt from pension deductions, keeping an extra <strong>$<span id="calpersSavedAmount">154.69</span></strong> in your pocket every check.
              </p>
            </div>

            <!-- Social Security notice box -->
            <div id="socialSecurityNoticeBox" class="hidden p-3.5 rounded-xl bg-slate-100 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 space-y-1">
              <div class="flex items-center justify-between text-xs font-bold text-slate-800 dark:text-slate-200">
                <span>Social Security (FICA OASDI 6.2%):</span>
                <span id="biweeklySsDisplay" class="text-slate-900 dark:text-white font-mono font-bold">$379.75</span>
              </div>
              <p class="text-[11px] text-slate-600 dark:text-slate-400">6.2% on taxable wages up to the 2026 cap ($184,500/yr).</p>
            </div>

            <!-- 457(b) Deferred Compensation -->
            <div class="space-y-2 pt-2 border-t border-slate-100 dark:border-slate-800">
              <div class="flex items-center justify-between">
                <label class="text-xs font-semibold text-slate-800 dark:text-slate-200">
                  Deferred Compensation 457(b)
                </label>
                <!-- Percentage or Dollar Toggle -->
                <div class="inline-flex p-0.5 bg-slate-100 dark:bg-slate-800 rounded-lg border border-slate-200 dark:border-slate-700 text-xs">
                  <button id="toggle457Percent" type="button" class="px-2.5 py-1 rounded-md bg-white dark:bg-slate-700 text-slate-900 dark:text-white font-semibold shadow-2xs">
                    % Percent
                  </button>
                  <button id="toggle457Dollar" type="button" class="px-2.5 py-1 rounded-md text-slate-500 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white">
                    $ Fixed
                  </button>
                </div>
              </div>

              <!-- Input row -->
              <div class="flex items-center space-x-3">
                <div class="relative flex-1">
                  <span id="unit457Prefix" class="hidden absolute inset-y-0 left-0 pl-3 flex items-center text-slate-400 text-sm">$</span>
                  <input type="number" id="input457Value" value="8" step="0.5" min="0" class="w-full pl-3 pr-8 py-2 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-sm font-semibold text-slate-900 dark:text-white focus:ring-2 focus:ring-sky-500 transition-all">
                  <span id="unit457Suffix" class="absolute inset-y-0 right-0 pr-3 flex items-center text-slate-400 text-sm">%</span>
                </div>
                <div class="text-right whitespace-nowrap">
                  <span class="text-xs text-slate-500 dark:text-slate-400 block">Biweekly 457:</span>
                  <span id="calc457Biweekly" class="text-sm font-bold text-slate-900 dark:text-white font-mono">$490.00</span>
                </div>
              </div>

              <!-- Fluctuation Note & 2026 Limits Monitor -->
              <div class="space-y-1">
                <div class="flex justify-between items-center text-[11px] text-slate-500 dark:text-slate-400">
                  <span id="note457Fluctuation" class="text-sky-700 dark:text-sky-400 font-medium truncate mr-1">
                    * 8% scales dynamically with gross pay including overtime!
                  </span>
                  <span id="annualized457Pace" class="font-mono text-slate-600 dark:text-slate-300 font-semibold shrink-0">Pace: $12,740 / yr</span>
                </div>
                <!-- 2026 Limit Bar -->
                <div class="w-full bg-slate-100 dark:bg-slate-800 rounded-full h-1.5 overflow-hidden">
                  <div id="bar457Limit" class="bg-sky-500 h-1.5 rounded-full transition-all duration-300" style="width: 52%;"></div>
                </div>
                <div class="flex justify-between text-[10px] text-slate-400 dark:text-slate-400">
                  <span>$0</span>
                  <span class="flex items-center">
                    <input type="checkbox" id="checkAge50CatchUp" class="rounded border-slate-300 dark:border-slate-600 text-sky-600 focus:ring-sky-500 mr-1 w-3 h-3">
                    <label for="checkAge50CatchUp" class="cursor-pointer text-slate-600 dark:text-slate-300">Age 50+ Catch-up ($32.5k)</label>
                  </span>
                  <span id="label457Cap">2026 Max: $24,500</span>
                </div>
              </div>
            </div>

            <!-- Additional Pre-Tax Deductions -->
            <div class="pt-2 border-t border-slate-100 dark:border-slate-800 space-y-1.5">
              <label for="inputAdditionalPretax" class="block text-xs font-semibold text-slate-800 dark:text-slate-200">
                Additional Pre-Tax Deductions (Biweekly)
              </label>
              <div class="relative">
                <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-slate-400 text-sm">$</span>
                <input type="number" id="inputAdditionalPretax" value="150.00" step="10" min="0" class="w-full pl-7 pr-3 py-2 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-sm font-semibold text-slate-900 dark:text-white focus:ring-2 focus:ring-sky-500 transition-all">
              </div>
              <p class="text-[11px] text-slate-500 dark:text-slate-400">
                Pre-tax Health / Dental / Vision (Section 125 cafeteria plan) & Healthcare FSA. Reduces Fed, State, and Medicare.
              </p>
            </div>
          </div>
        </div>

        <!-- Qualified Spousal Income Section (NOW INCLUDES "OTHER / CUSTOM") -->
        <div class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-xs overflow-hidden transition-colors duration-200">
          <div class="p-4 sm:p-5">
            <div class="flex items-start justify-between">
              <div class="flex items-start space-x-3">
                <div class="flex items-center h-5 mt-0.5">
                  <input type="checkbox" id="checkIncludeSpouse" class="w-4 h-4 rounded text-sky-600 focus:ring-sky-500 border-slate-300 dark:border-slate-700 dark:bg-slate-800 cursor-pointer">
                </div>
                <div>
                  <label for="checkIncludeSpouse" class="text-sm font-bold text-slate-900 dark:text-white cursor-pointer flex items-center">
                    Include Qualified Spouse Income
                    <span class="ml-2 inline-flex items-center px-1.5 py-0.5 rounded text-[10px] font-semibold bg-indigo-50 dark:bg-indigo-950 text-indigo-700 dark:text-indigo-300 border border-indigo-200 dark:border-indigo-800">
                      Family Total
                    </span>
                  </label>
                  <p class="text-xs text-slate-500 dark:text-slate-400 mt-0.5">
                    Composite total household income with spouse wages, CalPERS, LAFPP, OCERS, LACERA, or custom pension.
                  </p>
                </div>
              </div>
            </div>

            <!-- Collapsible Spousal Income Details Panel -->
            <div id="spousePanelContainer" class="hidden mt-4 pt-4 border-t border-slate-200 dark:border-slate-800 space-y-4">
              
              <!-- Spousal Pension Dropdown (Includes "Other / Custom") -->
              <div>
                <label for="spousePensionSystemSelect" class="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">
                  Spouse Pension / Retirement System (Drop Down)
                </label>
                <div class="relative">
                  <select id="spousePensionSystemSelect" class="w-full pl-3 pr-8 py-2 bg-slate-50 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-xs font-semibold text-slate-900 dark:text-white focus:ring-2 focus:ring-sky-500 transition-all cursor-pointer">
                    <option value="calpers" selected>CalPERS Safety (13.75% Base Only - No Social Security)</option>
                    <option value="lafpp_tier6">LAFPP Tier 6 - LAPD / LAFD (11.00% Base Only - No Social Security)</option>
                    <option value="ocers_plan_v">OCERS Plan V - Safety 2.7%@57 (14.61% Base Only - No Social Security)</option>
                    <option value="ocers_plan_u">OCERS Plan U - General 2.5%@67 (10.50% Base Only + Social Security)</option>
                    <option value="lacera_plan_c">LACERA Plan C - Safety 2.7%@57 (15.36% Base Only - No Social Security)</option>
                    <option value="lacera_plan_g">LACERA Plan G - General 2.5%@67 (9.22% Base Only + Social Security)</option>
                    <option value="social_security">Social Security Only (FICA 6.2% up to $184,500 - No Pension)</option>
                    <option value="other">Other / Custom Pension System (Set Your Own Numbers)</option>
                  </select>
                </div>

                <!-- Spouse Custom Pension Configuration (Shown when "Other" is selected) -->
                <div id="spouseCustomPensionContainer" class="hidden mt-2 p-3 rounded-xl bg-slate-50 dark:bg-slate-800/80 border border-slate-200 dark:border-slate-700 space-y-2 text-xs">
                  <span class="font-bold text-slate-800 dark:text-slate-200 block">Spouse Custom Pension Settings</span>
                  <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                    <div>
                      <label for="spouseCustomPensionName" class="block text-slate-600 dark:text-slate-400 mb-0.5">Plan Name</label>
                      <input type="text" id="spouseCustomPensionName" value="Spouse Custom Pension" class="w-full px-2.5 py-1 bg-white dark:bg-slate-700 border border-slate-300 dark:border-slate-600 rounded text-xs font-semibold">
                    </div>
                    <div>
                      <label for="spouseCustomPensionRate" class="block text-slate-600 dark:text-slate-400 mb-0.5">Contribution Rate (%)</label>
                      <input type="number" id="spouseCustomPensionRate" value="10.00" step="0.25" min="0" max="40" class="w-full px-2.5 py-1 bg-white dark:bg-slate-700 border border-slate-300 dark:border-slate-600 rounded text-xs font-semibold">
                    </div>
                  </div>
                  <div class="flex flex-wrap items-center gap-3 pt-1">
                    <label class="flex items-center space-x-1.5 cursor-pointer text-slate-700 dark:text-slate-300">
                      <input type="checkbox" id="spouseCustomPensionBaseOnly" checked class="rounded border-slate-300 dark:border-slate-600 text-sky-600 focus:ring-sky-500 w-3.5 h-3.5">
                      <span>Base Pay Only</span>
                    </label>
                    <label class="flex items-center space-x-1.5 cursor-pointer text-slate-700 dark:text-slate-300">
                      <input type="checkbox" id="spouseCustomPensionSocialSecurity" class="rounded border-slate-300 dark:border-slate-600 text-sky-600 focus:ring-sky-500 w-3.5 h-3.5">
                      <span>Social Security (6.2%)</span>
                    </label>
                  </div>
                </div>
              </div>

              <!-- Spousal Pay Rates & Hours -->
              <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                <div>
                  <label for="spouseHourlyRateInput" class="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">
                    Spouse Hourly Rate ($)
                  </label>
                  <div class="relative">
                    <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-slate-400 text-sm">$</span>
                    <input type="number" id="spouseHourlyRateInput" value="50.00" step="0.25" min="0" class="w-full pl-7 pr-3 py-2 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-sm font-semibold text-slate-900 dark:text-white focus:ring-2 focus:ring-sky-500">
                  </div>
                  <div class="pt-2">
                    <input type="range" id="spouseHourlyRateSlider" min="15" max="150" step="0.5" value="50" aria-label="Spouse hourly rate slider">
                  </div>
                </div>

                <div>
                  <label class="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">
                    Spouse OT Rate (1.5×)
                  </label>
                  <div class="py-2 px-3 bg-sky-50/80 dark:bg-sky-950/40 border border-sky-200 dark:border-sky-800 rounded-xl flex items-center justify-between">
                    <span class="text-xs font-medium text-sky-800 dark:text-sky-300">Time & Half</span>
                    <span id="spouseOtRateDisplay" class="text-sm font-bold text-sky-950 dark:text-sky-100 font-mono">$75.00/hr</span>
                  </div>
                </div>
              </div>

              <!-- Spousal Hours -->
              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label for="spouseBaseHoursInput" class="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">Spouse Base Hours</label>
                  <input type="number" id="spouseBaseHoursInput" value="80" step="1" min="0" max="168" class="w-full px-3 py-2 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-sm font-semibold text-slate-900 dark:text-white focus:ring-2 focus:ring-sky-500">
                </div>
                <div>
                  <label for="spouseOtHoursInput" class="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">Spouse OT Hours</label>
                  <input type="number" id="spouseOtHoursInput" value="6" step="0.5" min="0" max="150" class="w-full px-3 py-2 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-sm font-semibold text-slate-900 dark:text-white focus:ring-2 focus:ring-orange-500">
                </div>
              </div>

              <div class="pt-0.5">
                <div class="flex justify-between items-center text-[11px] text-slate-500 dark:text-slate-400 mb-1">
                  <span>Spouse Overtime Slider:</span>
                  <span class="font-mono text-orange-600 dark:text-orange-400 font-semibold" id="spouseSliderOtVal">6 hrs</span>
                </div>
                <input type="range" id="spouseOtHoursSlider" min="0" max="60" step="1" value="6" aria-label="Spouse overtime slider">
              </div>

              <!-- Spouse Live Gross Pay Breakdown Box -->
              <div class="bg-slate-50 dark:bg-slate-800/50 rounded-xl p-3 border border-slate-200/90 dark:border-slate-700/70 space-y-1.5 text-xs">
                <div class="flex justify-between items-center text-slate-600 dark:text-slate-300">
                  <span>Spouse Base (<span id="spouseCalcBaseFormula">80 hrs @ $50.00</span>):</span>
                  <span id="spouseCalcBasePay" class="font-semibold text-slate-900 dark:text-white font-mono">$4,000.00</span>
                </div>
                <div class="flex justify-between items-center text-slate-600 dark:text-slate-300">
                  <span>Spouse OT (<span id="spouseCalcOtFormula">6 hrs @ $75.00</span>):</span>
                  <span id="spouseCalcOtPay" class="font-semibold text-orange-600 dark:text-orange-400 font-mono">$450.00</span>
                </div>
                <div class="pt-1.5 border-t border-slate-200 dark:border-slate-700 flex justify-between items-center text-xs font-bold text-slate-900 dark:text-white">
                  <span>Total Spouse Gross Pay:</span>
                  <span id="spouseCalcTotalGross" class="text-sky-700 dark:text-sky-400 font-mono text-sm">$4,450.00</span>
                </div>
              </div>

              <!-- Spouse Pension Contribution Box -->
              <div id="spouseCalpersContainer" class="p-3 rounded-xl bg-amber-50/80 dark:bg-amber-950/30 border border-amber-200 dark:border-amber-800/60 space-y-1 text-xs">
                <div class="flex items-center justify-between">
                  <span id="labelSpousePensionTitle" class="font-bold text-amber-950 dark:text-amber-200">Spouse Pension (Base Only):</span>
                  <span id="spouseBiweeklyCalpersDisplay" class="font-mono font-bold text-amber-950 dark:text-amber-100">$550.00</span>
                </div>
                <p class="text-[11px] text-amber-800 dark:text-amber-300">
                  * Spouse OT ($<span id="spouseCalpersOtSavings">450.00</span>) exempt from pension, saving $<span id="spouseCalpersSavedAmount">61.88</span> biweekly.
                </p>
              </div>

              <!-- Spouse Social Security Notice Box -->
              <div id="spouseSocialSecurityNoticeBox" class="hidden p-3 rounded-xl bg-slate-100 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 text-xs">
                <div class="flex items-center justify-between font-bold text-slate-800 dark:text-slate-200">
                  <span>Spouse Social Security (6.2%):</span>
                  <span id="spouseBiweeklySsDisplay" class="font-mono text-slate-900 dark:text-white">$275.90</span>
                </div>
              </div>

              <!-- Spouse 457(b) / 401(k) Retirement Section -->
              <div class="space-y-2 pt-2 border-t border-slate-100 dark:border-slate-800">
                <div class="flex items-center justify-between">
                  <label class="text-xs font-semibold text-slate-800 dark:text-slate-200">
                    Spouse Deferred Comp / 401(k) / 457(b)
                  </label>
                  <div class="inline-flex p-0.5 bg-slate-100 dark:bg-slate-800 rounded-lg border border-slate-200 dark:border-slate-700 text-xs">
                    <button id="spouseToggle457Percent" type="button" class="px-2 py-0.5 rounded-md bg-white dark:bg-slate-700 text-slate-900 dark:text-white font-semibold shadow-2xs">
                      %
                    </button>
                    <button id="spouseToggle457Dollar" type="button" class="px-2 py-0.5 rounded-md text-slate-500 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white">
                      $
                    </button>
                  </div>
                </div>

                <div class="flex items-center space-x-3">
                  <div class="relative flex-1">
                    <span id="spouseUnit457Prefix" class="hidden absolute inset-y-0 left-0 pl-3 flex items-center text-slate-400 text-sm">$</span>
                    <input type="number" id="spouseInput457Value" value="6" step="0.5" min="0" class="w-full pl-3 pr-8 py-1.5 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-sm font-semibold text-slate-900 dark:text-white focus:ring-2 focus:ring-sky-500">
                    <span id="spouseUnit457Suffix" class="absolute inset-y-0 right-0 pr-3 flex items-center text-slate-400 text-sm">%</span>
                  </div>
                  <div class="text-right whitespace-nowrap">
                    <span class="text-xs text-slate-500 dark:text-slate-400 block">Deduction:</span>
                    <span id="spouseCalc457Biweekly" class="text-sm font-bold text-slate-900 dark:text-white font-mono">$267.00</span>
                  </div>
                </div>

                <div class="flex justify-between items-center text-[10px] text-slate-500 dark:text-slate-400">
                  <span id="spouseNote457Fluctuation" class="text-sky-700 dark:text-sky-400 font-medium truncate mr-1">* Scales with total pay</span>
                  <span id="spouseAnnualized457Pace" class="font-mono text-slate-600 dark:text-slate-300 font-semibold shrink-0">Pace: $6,942 / yr</span>
                </div>
                <div class="w-full bg-slate-100 dark:bg-slate-800 rounded-full h-1.5 overflow-hidden">
                  <div id="spouseBar457Limit" class="bg-sky-500 h-1.5 rounded-full transition-all duration-300" style="width: 28%;"></div>
                </div>
                <div class="flex justify-between text-[10px] text-slate-400 dark:text-slate-400">
                  <span class="flex items-center">
                    <input type="checkbox" id="spouseCheckAge50CatchUp" class="rounded border-slate-300 dark:border-slate-600 text-sky-600 focus:ring-sky-500 mr-1 w-3 h-3">
                    <label for="spouseCheckAge50CatchUp" class="cursor-pointer text-slate-600 dark:text-slate-300">Spouse 50+ ($32.5k)</label>
                  </span>
                  <span id="spouseLabel457Cap">Spouse Max: $24,500</span>
                </div>
              </div>

              <!-- Spouse Pre-Tax & Post-Tax Inputs -->
              <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 pt-2 border-t border-slate-100 dark:border-slate-800">
                <div>
                  <label for="spouseInputAdditionalPretax" class="block text-[11px] font-semibold text-slate-700 dark:text-slate-300 mb-1">
                    Spouse Pre-Tax (Health/FSA)
                  </label>
                  <div class="relative">
                    <span class="absolute inset-y-0 left-0 pl-2.5 flex items-center text-slate-400 text-xs">$</span>
                    <input type="number" id="spouseInputAdditionalPretax" value="100.00" step="10" min="0" class="w-full pl-6 pr-2 py-1.5 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-lg text-xs font-semibold text-slate-900 dark:text-white">
                  </div>
                </div>
                <div>
                  <label for="spouseInputPostTaxDeductions" class="block text-[11px] font-semibold text-slate-700 dark:text-slate-300 mb-1">
                    Spouse Post-Tax Deductions
                  </label>
                  <div class="relative">
                    <span class="absolute inset-y-0 left-0 pl-2.5 flex items-center text-slate-400 text-xs">$</span>
                    <input type="number" id="spouseInputPostTaxDeductions" value="40.00" step="5" min="0" class="w-full pl-6 pr-2 py-1.5 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-lg text-xs font-semibold text-slate-900 dark:text-white">
                  </div>
                </div>
              </div>

            </div>
          </div>
        </div>

        <!-- Card 3: Post-Tax Deductions, Alimony & Filing Status -->
        <div class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-xs overflow-hidden transition-colors duration-200">
          <div class="px-4 sm:px-5 py-3 border-b border-slate-100 dark:border-slate-800 bg-slate-50/60 dark:bg-slate-800/40 flex items-center justify-between">
            <h2 class="text-xs font-bold uppercase tracking-wider text-slate-900 dark:text-slate-100 flex items-center">
              <span class="w-2 h-2 rounded-full bg-violet-500 mr-2"></span>
              3. Taxable Deductions & Filing Status
            </h2>
            <span class="text-[11px] text-slate-400 dark:text-slate-400 font-mono">2026 Tax Rules</span>
          </div>

          <div class="p-4 sm:p-5 space-y-4">
            
            <!-- Taxable Above-the-Line Deductions (ALIMONY PAID) -->
            <div class="p-3 rounded-xl bg-slate-50 dark:bg-slate-800/60 border border-slate-200 dark:border-slate-700/80 space-y-1.5">
              <div class="flex items-center justify-between">
                <label for="inputAlimonyPaid" class="text-xs font-bold text-slate-800 dark:text-slate-200">
                  Alimony Paid / Above-the-Line Tax Deductions
                </label>
                <span class="text-[10px] text-emerald-700 dark:text-emerald-300 font-medium bg-emerald-50 dark:bg-emerald-950 px-1.5 py-0.2 rounded border border-emerald-200 dark:border-emerald-800">
                  Direct AGI Reduction
                </span>
              </div>
              <div class="relative">
                <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-slate-400 text-sm">$</span>
                <input type="number" id="inputAlimonyPaid" value="0.00" step="500" min="0" class="w-full pl-7 pr-3 py-2 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-sm font-semibold text-slate-900 dark:text-white focus:ring-2 focus:ring-sky-500 transition-all" placeholder="Annual alimony paid">
              </div>
              <p class="text-[11px] text-slate-500 dark:text-slate-400 leading-tight">
                Alimony paid under pre-2019 divorce agreements (and 100% deductible on California Form 540) reduces Adjusted Gross Income (AGI) directly.
              </p>
            </div>

            <!-- Primary Post-Tax Deductions Area -->
            <div>
              <label for="inputPostTaxDeductions" class="block text-xs font-semibold text-slate-800 dark:text-slate-200 mb-1">
                Primary Post-Tax Deductions (Biweekly)
              </label>
              <div class="relative">
                <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-slate-400 text-sm">$</span>
                <input type="number" id="inputPostTaxDeductions" value="85.00" step="5" min="0" class="w-full pl-7 pr-3 py-2 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-sm font-semibold text-slate-900 dark:text-white focus:ring-2 focus:ring-sky-500 transition-all">
              </div>
              <p class="text-[11px] text-slate-500 dark:text-slate-400 mt-1">
                Union / Association Dues (PORAC, IAFF, local union), legal defense fund, Roth 457(b), supplemental life.
              </p>
            </div>

            <!-- 2026 Filing Status Dropdown -->
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 pt-2 border-t border-slate-100 dark:border-slate-800">
              <div>
                <label for="selectFilingStatus" class="block text-xs font-semibold text-slate-800 dark:text-slate-200 mb-1">
                  2026 Filing Status
                </label>
                <select id="selectFilingStatus" class="w-full px-3 py-2 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-xs font-semibold text-slate-900 dark:text-white focus:ring-2 focus:ring-sky-500 transition-all cursor-pointer">
                  <option value="single" selected>Single</option>
                  <option value="mfj">Married Filing Jointly</option>
                  <option value="mfs">Married Filing Separately</option>
                  <option value="hoh">Head of Household</option>
                </select>
              </div>

              <!-- Children / Dependents for Credits -->
              <div>
                <label for="inputChildren" class="block text-xs font-semibold text-slate-800 dark:text-slate-200 mb-1">
                  Child Tax Credits (Under 17)
                </label>
                <div class="flex items-center space-x-2">
                  <input type="number" id="inputChildren" value="1" min="0" max="10" step="1" class="w-full px-3 py-2 bg-white dark:bg-slate-800 border border-slate-300 dark:border-slate-700 rounded-xl text-xs font-semibold text-slate-900 dark:text-white focus:ring-2 focus:ring-sky-500">
                  <span class="text-[11px] text-slate-500 dark:text-slate-400 whitespace-nowrap">$2,200/child</span>
                </div>
              </div>
            </div>

            <!-- Standard vs Itemized Deduction Toggle -->
            <div class="pt-2 border-t border-slate-100 dark:border-slate-800 flex items-center justify-between">
              <div>
                <span class="text-xs font-semibold text-slate-800 dark:text-slate-200 block">Deduction Method</span>
                <span id="deductionDisplayLabel" class="text-[11px] text-slate-500 dark:text-slate-400">2026 Standard ($16,100 Fed / $5,900 CA)</span>
              </div>
              <div class="inline-flex p-0.5 bg-slate-100 dark:bg-slate-800 rounded-lg border border-slate-200 dark:border-slate-700 text-xs">
                <button id="btnDeductionStandard" type="button" class="px-2.5 py-1 rounded-md bg-white dark:bg-slate-700 text-slate-900 dark:text-white font-semibold shadow-2xs">
                  Standard
                </button>
                <button id="btnDeductionItemized" type="button" class="px-2.5 py-1 rounded-md text-slate-500 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white">
                  Itemized
                </button>
              </div>
            </div>

            <!-- Itemized Input Container -->
            <div id="itemizedContainer" class="hidden p-3 bg-slate-50 dark:bg-slate-800 rounded-xl border border-slate-200 dark:border-slate-700 space-y-2 text-xs">
              <label for="inputItemizedAmount" class="block font-semibold text-slate-700 dark:text-slate-200">Annual Itemized Deductions ($)</label>
              <div class="relative">
                <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-slate-400">$</span>
                <input type="number" id="inputItemizedAmount" value="22000" step="500" min="0" class="w-full pl-7 pr-3 py-1.5 bg-white dark:bg-slate-700 border border-slate-300 dark:border-slate-600 rounded-lg text-sm font-semibold text-slate-900 dark:text-white">
              </div>
              <span class="text-[10px] text-slate-500 dark:text-slate-400">Qualified mortgage interest, property tax (SALT $10,000 cap), charitable gifts.</span>
            </div>

          </div>
        </div>

      </div>

      <!-- RIGHT COLUMN: RESULTS & OUTPUT SECTIONS (7 cols) -->
      <div class="lg:col-span-7 space-y-5 sm:space-y-6">

        <!-- Main KPI Summary Card: Net Take-Home & Composite Tax -->
        <div id="heroCardContainer" class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 rounded-2xl sm:rounded-3xl p-4 sm:p-6 shadow-sm dark:shadow-none text-slate-900 dark:text-white relative overflow-hidden print-shadow-none transition-colors duration-200">
          
          <div class="absolute -right-8 -bottom-8 w-48 h-48 bg-sky-500/5 dark:bg-sky-500/10 rounded-full blur-2xl pointer-events-none"></div>

          <div class="relative z-10 space-y-4 sm:space-y-5">
            <div class="flex flex-wrap items-center justify-between gap-3 border-b border-slate-100 dark:border-slate-800 pb-3 sm:pb-4">
              <div>
                <span id="heroTitleLabel" class="text-[11px] sm:text-xs uppercase tracking-wider text-sky-700 dark:text-sky-400 font-bold">
                  Net Take-Home Paycheck
                </span>
                <div class="flex items-baseline space-x-2 mt-0.5 sm:mt-1">
                  <h3 id="heroNetBiweekly" class="text-2xl sm:text-4xl font-extrabold tracking-tight text-slate-900 dark:text-white font-mono">$3,447.21</h3>
                  <span class="text-xs sm:text-sm text-slate-500 dark:text-slate-400 font-medium">/ biweekly</span>
                </div>
                <!-- Sub breakdown if spouse included -->
                <div id="heroSubNetBreakdown" class="hidden text-xs text-slate-500 dark:text-slate-400 mt-1 font-mono">
                  Primary: <span id="heroNetPrimary" class="text-slate-700 dark:text-slate-300 font-semibold">$3,447.21</span> | Spouse: <span id="heroNetSpouse" class="text-slate-700 dark:text-slate-300 font-semibold">$0.00</span>
                </div>
              </div>
              <div class="text-right">
                <span class="text-[11px] sm:text-xs uppercase tracking-wider text-slate-500 dark:text-slate-400 font-bold">Annualized Net</span>
                <p id="heroNetAnnual" class="text-lg sm:text-2xl font-bold text-emerald-600 dark:text-emerald-400 mt-0.5 sm:mt-1 font-mono">$89,627.46</p>
              </div>
            </div>

            <!-- Composite Biweekly Tax & Deductions Summary Grid -->
            <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 sm:gap-2.5">
              <div class="bg-slate-50 dark:bg-slate-800/60 rounded-xl p-2.5 sm:p-3 border border-slate-200/80 dark:border-slate-700/60">
                <span class="text-[10px] sm:text-[11px] text-slate-500 dark:text-slate-400 block font-medium">Composite Taxes</span>
                <span id="heroCompositeTaxesBiweekly" class="text-sm sm:text-base font-bold text-sky-700 dark:text-sky-300 font-mono">$1,175.29</span>
                <span id="heroEffectiveRate" class="text-[10px] text-slate-500 dark:text-slate-400 block mt-0.5">19.2% of Gross</span>
              </div>

              <div class="bg-slate-50 dark:bg-slate-800/60 rounded-xl p-2.5 sm:p-3 border border-slate-200/80 dark:border-slate-700/60">
                <span class="text-[10px] sm:text-[11px] text-slate-500 dark:text-slate-400 block font-medium">Pension (Base Only)</span>
                <span id="heroPensionBiweekly" class="text-sm sm:text-base font-bold text-amber-700 dark:text-amber-300 font-mono">$687.50</span>
                <span id="heroPensionRateLabel" class="text-[10px] text-slate-500 dark:text-slate-400 block mt-0.5">Exempts OT</span>
              </div>

              <div class="bg-slate-50 dark:bg-slate-800/60 rounded-xl p-2.5 sm:p-3 border border-slate-200/80 dark:border-slate-700/60">
                <span class="text-[10px] sm:text-[11px] text-slate-500 dark:text-slate-400 block font-medium">457(b) Retirement</span>
                <span id="hero457Biweekly" class="text-sm sm:text-base font-bold text-purple-700 dark:text-purple-300 font-mono">$490.00</span>
                <span class="text-[10px] text-slate-500 dark:text-slate-400 block mt-0.5">Pre-Tax Shelter</span>
              </div>

              <div class="bg-slate-50 dark:bg-slate-800/60 rounded-xl p-2.5 sm:p-3 border border-slate-200/80 dark:border-slate-700/60">
                <span class="text-[10px] sm:text-[11px] text-slate-500 dark:text-slate-400 block font-medium">Other Deductions</span>
                <span id="heroOtherDeductionsBiweekly" class="text-sm sm:text-base font-bold text-rose-700 dark:text-rose-300 font-mono">$235.00</span>
                <span class="text-[10px] text-slate-500 dark:text-slate-400 block mt-0.5">Health, Alimony, Post</span>
              </div>
            </div>

            <!-- Sleek Visual Paycheck Allocation Progress Bar -->
            <div>
              <div class="flex justify-between text-xs text-slate-600 dark:text-slate-300 mb-1.5 font-medium">
                <span>Where Each Gross Dollar Goes:</span>
                <span id="labelGrossDistribution" class="font-mono text-slate-700 dark:text-slate-300">100% of $6,125.00</span>
              </div>
              <div class="w-full h-3.5 bg-slate-100 dark:bg-slate-800 rounded-full flex overflow-hidden p-0.5 space-x-0.5 border border-slate-200 dark:border-slate-700/60">
                <div id="barNet" class="bg-emerald-500 rounded-l-full transition-all duration-300" style="width: 56.3%;" title="Net Take-Home Pay"></div>
                <div id="barTaxes" class="bg-sky-500 transition-all duration-300" style="width: 19.2%;" title="Composite Taxes (Fed, CA, Medicare)"></div>
                <div id="barPension" class="bg-amber-400 transition-all duration-300" style="width: 11.2%;" title="Pension Contribution"></div>
                <div id="bar457" class="bg-purple-500 transition-all duration-300" style="width: 8.0%;" title="457(b) Deferred Comp"></div>
                <div id="barOther" class="bg-rose-400 rounded-r-full transition-all duration-300" style="width: 5.3%;" title="Health & Deductions"></div>
              </div>
              <div class="flex flex-wrap gap-x-3 gap-y-1 text-[11px] text-slate-600 dark:text-slate-300 mt-2">
                <span class="inline-flex items-center"><span class="w-2.5 h-2.5 rounded-xs bg-emerald-500 mr-1.5 shrink-0"></span>Net Take-Home (<span id="pctNet">56.3%</span>)</span>
                <span class="inline-flex items-center"><span class="w-2.5 h-2.5 rounded-xs bg-sky-500 mr-1.5 shrink-0"></span>Taxes (<span id="pctTaxes">19.2%</span>)</span>
                <span class="inline-flex items-center"><span class="w-2.5 h-2.5 rounded-xs bg-amber-400 mr-1.5 shrink-0"></span>Pension (<span id="pctPension">11.2%</span>)</span>
                <span class="inline-flex items-center"><span class="w-2.5 h-2.5 rounded-xs bg-purple-500 mr-1.5 shrink-0"></span>457(b) (<span id="pct457">8.0%</span>)</span>
                <span class="inline-flex items-center"><span class="w-2.5 h-2.5 rounded-xs bg-rose-400 mr-1.5 shrink-0"></span>Deductions (<span id="pctOther">5.3%</span>)</span>
              </div>
            </div>
          </div>
        </div>

        <!-- Section A: Biweekly Paycheck Breakdown -->
        <div id="biweeklySection" class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-xs overflow-hidden print-shadow-none transition-colors duration-200">
          <div class="px-4 sm:px-5 py-3.5 border-b border-slate-100 dark:border-slate-800 bg-slate-50/50 dark:bg-slate-800/40 flex items-center justify-between">
            <h3 class="text-xs sm:text-sm font-bold text-slate-900 dark:text-white flex items-center">
              <svg class="w-4 h-4 mr-2 text-sky-600 dark:text-sky-400 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"/></svg>
              Biweekly Paycheck Breakdown (Line-by-Line)
            </h3>
            <span id="biweeklyBadgeLabel" class="text-xs font-semibold px-2.5 py-0.5 rounded-full bg-slate-100 dark:bg-slate-800 text-slate-700 dark:text-slate-300">Biweekly Check</span>
          </div>

          <div class="divide-y divide-slate-100 dark:divide-slate-800 text-xs sm:text-sm">
            
            <!-- 1. Gross Earnings -->
            <div class="p-3.5 sm:p-4 bg-slate-50/30 dark:bg-slate-800/20">
              <span class="text-xs font-bold uppercase tracking-wider text-slate-500 dark:text-slate-400 block mb-2">1. Gross Earnings</span>
              <div class="space-y-1.5">
                <div class="flex justify-between items-center">
                  <span class="text-slate-600 dark:text-slate-300">Primary Base (<span id="rowBaseHours">80</span> hrs @ $<span id="rowHourlyRate">62.50</span>/hr)</span>
                  <span id="rowBasePay" class="font-semibold text-slate-900 dark:text-white font-mono">$5,000.00</span>
                </div>
                <div class="flex justify-between items-center">
                  <span class="text-slate-600 dark:text-slate-300">Primary Overtime (<span id="rowOtHours">12</span> hrs @ $<span id="rowOtRate">93.75</span>/hr at 1.5×)</span>
                  <span id="rowOtPay" class="font-semibold text-orange-600 dark:text-orange-400 font-mono">$1,125.00</span>
                </div>
                
                <!-- Spouse Gross Line -->
                <div id="rowSpouseEarningsItem" class="hidden flex justify-between items-center pt-1 border-t border-slate-100 dark:border-slate-800">
                  <span class="text-indigo-600 dark:text-indigo-300 font-medium">Spouse Earnings (Base: <span id="rowSpouseBasePay">$4,000</span> + OT: <span id="rowSpouseOtPay">$450</span>)</span>
                  <span id="rowSpouseGrossPay" class="font-semibold text-indigo-700 dark:text-indigo-300 font-mono">$4,450.00</span>
                </div>

                <div class="flex justify-between items-center pt-1.5 border-t border-slate-200 dark:border-slate-700 font-bold text-slate-900 dark:text-white">
                  <span id="labelRowGross">Total Biweekly Gross Wages</span>
                  <span id="rowGrossPay" class="text-sky-700 dark:text-sky-400 font-bold text-sm sm:text-base font-mono">$6,125.00</span>
                </div>
              </div>
            </div>

            <!-- 2. Pre-Tax Deductions -->
            <div class="p-3.5 sm:p-4">
              <span class="text-xs font-bold uppercase tracking-wider text-emerald-600 dark:text-emerald-400 block mb-2">2. Pre-Tax Deductions</span>
              <div class="space-y-1.5">
                <div class="flex justify-between items-center">
                  <span class="text-slate-600 dark:text-slate-300 flex items-center">
                    <span id="labelRowPrimaryPension">Primary Pension (Base Only)</span>
                    <span class="ml-1.5 text-[10px] bg-amber-100 dark:bg-amber-950/70 text-amber-800 dark:text-amber-300 px-1.5 py-0.2 rounded font-medium border border-amber-200 dark:border-amber-800">OT Exempt</span>
                  </span>
                  <span id="rowCalpers" class="font-semibold text-slate-900 dark:text-white font-mono">-$687.50</span>
                </div>
                <div class="flex justify-between items-center">
                  <span class="text-slate-600 dark:text-slate-300">Primary Deferred Comp 457(b)</span>
                  <span id="row457" class="font-semibold text-slate-900 dark:text-white font-mono">-$490.00</span>
                </div>
                <div class="flex justify-between items-center">
                  <span class="text-slate-600 dark:text-slate-300">Primary Pre-Tax Health / Section 125</span>
                  <span id="rowPretaxHealth" class="font-semibold text-slate-900 dark:text-white font-mono">-$150.00</span>
                </div>

                <!-- Spouse Pre-tax Line -->
                <div id="rowSpousePretaxItem" class="hidden flex justify-between items-center pt-1 border-t border-slate-100 dark:border-slate-800 text-indigo-700 dark:text-indigo-300">
                  <span>Spouse Pre-Tax (Pension Base, 457/401k, Health)</span>
                  <span id="rowSpouseTotalPretax" class="font-semibold font-mono">-$917.00</span>
                </div>

                <div class="flex justify-between items-center pt-1.5 border-t border-slate-200 dark:border-slate-700 font-semibold text-emerald-700 dark:text-emerald-300">
                  <span>Total Pre-Tax Reductions</span>
                  <span id="rowTotalPretax" class="font-mono">-$1,327.50</span>
                </div>
              </div>
            </div>

            <!-- 3. Taxes At All Three Levels (Composite Tax Obligation) -->
            <div class="p-3.5 sm:p-4 bg-sky-50/20 dark:bg-sky-950/20">
              <div class="flex items-center justify-between mb-2">
                <span class="text-xs font-bold uppercase tracking-wider text-sky-700 dark:text-sky-300">3. Statutory Tax Withholdings (3 Levels)</span>
                <span class="text-[11px] text-slate-500 dark:text-slate-400">Taxable Wage: <strong id="rowFedTaxableWage" class="font-mono text-slate-800 dark:text-slate-200">$4,797.50</strong></span>
              </div>
              <div class="space-y-1.5">
                <div class="flex justify-between items-center">
                  <span class="text-slate-600 dark:text-slate-300">Federal Income Tax Withholding (2026 Brackets)</span>
                  <span id="rowFedTax" class="font-semibold text-slate-900 dark:text-white font-mono">-$689.42</span>
                </div>
                <div class="flex justify-between items-center">
                  <span class="text-slate-600 dark:text-slate-300">California State Income Tax (2026 Brackets)</span>
                  <span id="rowCaTax" class="font-semibold text-slate-900 dark:text-white font-mono">-$399.23</span>
                </div>
                <div class="flex justify-between items-center">
                  <span class="text-slate-600 dark:text-slate-300">Medicare Tax (Standard 1.45%)</span>
                  <span id="rowMedicareTax" class="font-semibold text-slate-900 dark:text-white font-mono">-$86.64</span>
                </div>
                <div id="rowSsItem" class="hidden flex justify-between items-center">
                  <span class="text-slate-600 dark:text-slate-300">Social Security Tax (6.2%)</span>
                  <span id="rowSsTax" class="font-semibold text-slate-900 dark:text-white font-mono">-$0.00</span>
                </div>
                <div class="flex justify-between items-center pt-1.5 border-t border-slate-200 dark:border-slate-700 font-bold text-sky-950 dark:text-sky-100">
                  <span>Composite Biweekly Tax Obligation</span>
                  <span id="rowTotalTaxes" class="text-sm font-mono text-sky-700 dark:text-sky-400">-$1,175.29</span>
                </div>
              </div>
            </div>

            <!-- 4. Post-Tax & Above-the-Line Deductions -->
            <div class="p-3.5 sm:p-4">
              <span class="text-xs font-bold uppercase tracking-wider text-violet-600 dark:text-violet-400 block mb-2">4. Post-Tax & Taxable Deductions</span>
              <div class="space-y-1.5">
                <div class="flex justify-between items-center">
                  <span class="text-slate-600 dark:text-slate-300">Primary Union Dues / Post-Tax</span>
                  <span id="rowPostTax" class="font-semibold text-slate-900 dark:text-white font-mono">-$85.00</span>
                </div>
                <div id="rowSpousePostTaxItem" class="hidden flex justify-between items-center text-indigo-700 dark:text-indigo-300">
                  <span>Spouse Post-Tax Deductions</span>
                  <span id="rowSpousePostTax" class="font-semibold font-mono">-$40.00</span>
                </div>
                <div id="rowAlimonyItem" class="hidden flex justify-between items-center text-amber-800 dark:text-amber-300">
                  <span>Alimony Paid (Biweekly Equivalent)</span>
                  <span id="rowAlimonyDeduction" class="font-semibold font-mono">-$0.00</span>
                </div>
              </div>
            </div>

            <!-- 5. Net Check Total -->
            <div class="p-3.5 sm:p-4 bg-emerald-50/60 dark:bg-emerald-950/30 flex justify-between items-center text-sm sm:text-base font-extrabold text-slate-900 dark:text-white">
              <span id="labelRowNet" class="text-emerald-950 dark:text-emerald-300 flex items-center">
                <svg class="w-5 h-5 mr-1.5 text-emerald-600 dark:text-emerald-400 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/></svg>
                Net Take-Home Paycheck:
              </span>
              <span id="rowNetPay" class="text-emerald-700 dark:text-emerald-400 text-lg sm:text-xl font-mono font-black">$3,447.21</span>
            </div>
          </div>
        </div>

        <!-- Section B: Annual Tax Estimate & Line Items -->
        <div id="annualSection" class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-xs overflow-hidden print-shadow-none transition-colors duration-200">
          <div class="px-4 sm:px-5 py-3.5 border-b border-slate-100 dark:border-slate-800 bg-slate-50/50 dark:bg-slate-800/40 flex items-center justify-between">
            <h3 class="text-xs sm:text-sm font-bold text-slate-900 dark:text-white flex items-center">
              <svg class="w-4 h-4 mr-2 text-indigo-600 dark:text-indigo-400 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/></svg>
              Annual 2026 Tax Return & Household Obligation Estimate
            </h3>
            <span class="text-xs font-semibold px-2.5 py-0.5 rounded-full bg-indigo-50 dark:bg-indigo-950/70 text-indigo-700 dark:text-indigo-300 border border-indigo-200 dark:border-indigo-800">Form 1040 & CA 540</span>
          </div>

          <div class="p-4 sm:p-5 space-y-4 text-xs sm:text-sm">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">

              <!-- Federal Tax Card -->
              <div class="p-3.5 sm:p-4 rounded-xl border border-slate-200 dark:border-slate-800 bg-slate-50/50 dark:bg-slate-800/40 space-y-2">
                <div class="flex justify-between items-center pb-2 border-b border-slate-200 dark:border-slate-700">
                  <span class="font-bold text-slate-900 dark:text-white">Federal Tax (IRS 1040)</span>
                  <span id="annFedStatusLabel" class="text-[11px] font-semibold text-sky-700 dark:text-sky-300 bg-sky-50 dark:bg-sky-950 px-2 py-0.5 rounded border border-sky-200 dark:border-sky-800">Single</span>
                </div>
                <div class="space-y-1 text-slate-600 dark:text-slate-300">
                  <div class="flex justify-between">
                    <span>Gross Annual Wages:</span>
                    <span id="annGrossWages" class="font-semibold text-slate-900 dark:text-white font-mono">$159,250</span>
                  </div>
                  <div class="flex justify-between text-emerald-700 dark:text-emerald-400">
                    <span>Pre-Tax Pension, 457(b) & Cafeteria:</span>
                    <span id="annTotalPretax" class="font-semibold font-mono">-$34,515</span>
                  </div>
                  <div id="annAlimonyRow" class="hidden flex justify-between text-amber-700 dark:text-amber-300">
                    <span>Alimony Paid (Above-the-Line):</span>
                    <span id="annAlimonyDeduction" class="font-semibold font-mono">-$0</span>
                  </div>
                  <div class="flex justify-between font-medium text-slate-800 dark:text-slate-200">
                    <span>Federal Adjusted Gross Income (AGI):</span>
                    <span id="annFedAgi" class="font-mono">$124,735</span>
                  </div>
                  <div class="flex justify-between text-slate-500 dark:text-slate-400">
                    <span>2026 Federal Standard Deduction:</span>
                    <span id="annFedStdDed" class="font-mono">-$16,100</span>
                  </div>
                  <div class="flex justify-between font-bold text-slate-900 dark:text-white pt-1 border-t border-slate-200 dark:border-slate-700">
                    <span>Federal Taxable Income:</span>
                    <span id="annFedTaxable" class="font-mono">$108,635</span>
                  </div>
                  <div class="flex justify-between pt-1">
                    <span>Tax before Credits:</span>
                    <span id="annFedTaxPreCredit" class="font-mono">$18,670</span>
                  </div>
                  <div class="flex justify-between text-emerald-700 dark:text-emerald-400">
                    <span>Child / Dependent Tax Credits:</span>
                    <span id="annFedCredits" class="font-mono">-$2,200</span>
                  </div>
                  <div class="flex justify-between font-bold text-sky-950 dark:text-sky-200 pt-1.5 border-t border-slate-200 dark:border-slate-700 text-sm">
                    <span>Total Federal Income Tax:</span>
                    <span id="annFedTaxFinal" class="font-mono font-extrabold text-sky-700 dark:text-sky-400">$16,470</span>
                  </div>
                </div>
              </div>

              <!-- California State Tax Card -->
              <div class="p-3.5 sm:p-4 rounded-xl border border-slate-200 dark:border-slate-800 bg-slate-50/50 dark:bg-slate-800/40 space-y-2">
                <div class="flex justify-between items-center pb-2 border-b border-slate-200 dark:border-slate-700">
                  <span class="font-bold text-slate-900 dark:text-white">California State Tax (FTB 540)</span>
                  <span id="annCaStatusLabel" class="text-[11px] font-semibold text-indigo-700 dark:text-indigo-300 bg-indigo-50 dark:bg-indigo-950 px-2 py-0.5 rounded border border-indigo-200 dark:border-indigo-800">Single</span>
                </div>
                <div class="space-y-1 text-slate-600 dark:text-slate-300">
                  <div class="flex justify-between">
                    <span>California AGI:</span>
                    <span id="annCaAgi" class="font-semibold text-slate-900 dark:text-white font-mono">$124,735</span>
                  </div>
                  <div class="flex justify-between text-slate-500 dark:text-slate-400">
                    <span>2026 CA Standard Deduction:</span>
                    <span id="annCaStdDed" class="font-mono">-$5,900</span>
                  </div>
                  <div class="flex justify-between font-bold text-slate-900 dark:text-white pt-1 border-t border-slate-200 dark:border-slate-700">
                    <span>CA Taxable Income:</span>
                    <span id="annCaTaxable" class="font-mono">$118,835</span>
                  </div>
                  <div class="flex justify-between pt-1">
                    <span>CA Tax before Exemption Credits:</span>
                    <span id="annCaTaxPreCredit" class="font-mono">$7,748</span>
                  </div>
                  <div class="flex justify-between text-emerald-700 dark:text-emerald-400">
                    <span>CA Exemption & Dependent Credits:</span>
                    <span id="annCaCredits" class="font-mono">-$649</span>
                  </div>
                  <div class="flex justify-between font-bold text-indigo-950 dark:text-indigo-200 pt-1.5 border-t border-slate-200 dark:border-slate-700 text-sm">
                    <span>Total California State Tax:</span>
                    <span id="annCaTaxFinal" class="font-mono font-extrabold text-indigo-700 dark:text-indigo-400">$7,099</span>
                  </div>
                  <div class="pt-2 text-[11px] text-slate-500 dark:text-slate-400 border-t border-slate-200 dark:border-slate-700 flex justify-between">
                    <span>Top CA Marginal Tax Rate:</span>
                    <strong id="annCaMarginal" class="text-slate-800 dark:text-slate-200 font-mono">9.3%</strong>
                  </div>
                </div>
              </div>

            </div>

            <!-- Composite Annual Taxes Summary -->
            <div class="bg-slate-50 dark:bg-slate-800/60 rounded-xl p-3.5 sm:p-4 border border-slate-200 dark:border-slate-700/80">
              <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-2 pb-3 border-b border-slate-200 dark:border-slate-700">
                <div>
                  <h4 class="font-bold text-slate-900 dark:text-white">Annual Composite Household Tax Obligation</h4>
                  <p class="text-xs text-slate-500 dark:text-slate-400">Combined multi-level tax obligation across all family earners</p>
                </div>
                <div class="text-right">
                  <span class="text-xs text-slate-500 dark:text-slate-400">Effective Tax Rate:</span>
                  <div id="annCombinedEffective" class="text-base font-extrabold text-slate-900 dark:text-white font-mono">16.2%</div>
                </div>
              </div>

              <div class="grid grid-cols-2 sm:grid-cols-4 gap-2.5 sm:gap-3 mt-3 text-xs">
                <div class="bg-white dark:bg-slate-900 p-2.5 rounded-lg border border-slate-200 dark:border-slate-700">
                  <span class="text-slate-400 dark:text-slate-400 block text-[10px] uppercase font-semibold">Federal Income Tax</span>
                  <span id="annSummaryFed" class="font-bold text-slate-900 dark:text-white text-sm font-mono">$16,470</span>
                </div>
                <div class="bg-white dark:bg-slate-900 p-2.5 rounded-lg border border-slate-200 dark:border-slate-700">
                  <span class="text-slate-400 dark:text-slate-400 block text-[10px] uppercase font-semibold">CA State Income Tax</span>
                  <span id="annSummaryCa" class="font-bold text-slate-900 dark:text-white text-sm font-mono">$7,099</span>
                </div>
                <div class="bg-white dark:bg-slate-900 p-2.5 rounded-lg border border-slate-200 dark:border-slate-700">
                  <span class="text-slate-400 dark:text-slate-400 block text-[10px] uppercase font-semibold">Medicare Tax (1.45%)</span>
                  <span id="annSummaryMed" class="font-bold text-slate-900 dark:text-white text-sm font-mono">$2,253</span>
                </div>
                <div class="bg-white dark:bg-slate-900 p-2.5 rounded-lg border border-slate-200 dark:border-slate-700">
                  <span id="annSummaryPensionTitle" class="text-slate-400 dark:text-slate-400 block text-[10px] uppercase font-semibold">CalPERS (13.75%)</span>
                  <span id="annSummaryPension" class="font-bold text-amber-700 dark:text-amber-400 text-sm font-mono">$17,875</span>
                </div>
              </div>
            </div>

            <!-- Tax Guide Accordion -->
            <div class="bg-sky-50/70 dark:bg-sky-950/30 rounded-xl p-3.5 sm:p-4 border border-sky-100 dark:border-sky-800/50 flex items-start space-x-3 text-xs text-sky-950 dark:text-sky-200">
              <svg class="w-5 h-5 text-sky-600 dark:text-sky-400 mt-0.5 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>
              <div class="space-y-1.5 flex-1">
                <p class="font-bold text-sky-950 dark:text-sky-100 text-sm">California & Out-of-State Public Pension Architecture:</p>
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-2.5 sm:gap-3 text-sky-900 dark:text-sky-200 mt-1">
                  <div class="bg-white/80 dark:bg-slate-900/80 p-2.5 rounded-lg border border-sky-200/60 dark:border-sky-800/60">
                    <strong class="block text-sky-950 dark:text-sky-100 mb-0.5">1. Universal "Other" Customization</strong>
                    Whether you participate in CalPERS, LAFPP, OCERS, LACERA, or an out-of-state system (FDNY, NYPD, Texas TMRS, etc.), selecting "Other" unlocks custom percentage rates, base-only toggles, and Social Security coordination.
                  </div>
                  <div class="bg-white/80 dark:bg-slate-900/80 p-2.5 rounded-lg border border-sky-200/60 dark:border-sky-800/60">
                    <strong class="block text-sky-950 dark:text-sky-100 mb-0.5">2. Base-Only Pension Exemption</strong>
                    Mandatory contributions calculate strictly from regular base salary when enabled, preserving full take-home value on all overtime and callback hours.
                  </div>
                  <div class="bg-white/80 dark:bg-slate-900/80 p-2.5 rounded-lg border border-sky-200/60 dark:border-sky-800/60">
                    <strong class="block text-sky-950 dark:text-sky-100 mb-0.5">3. Family Tax Modeling</strong>
                    Independent pension parameters for both primary and spouse ensure accurate federal, state, and payroll tax calculations for any public service household.
                  </div>
                </div>
              </div>
            </div>

          </div>
        </div>

      </div>

    </div>

  </main>

  <!-- Clean Minimalist Footer & Legal Disclaimer -->
  <footer class="mt-auto border-t border-slate-200 dark:border-slate-800 bg-white dark:bg-slate-900 py-6 text-xs text-slate-500 dark:text-slate-400 no-print transition-colors duration-200">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 space-y-3">
      <!-- Prominent Disclaimer Box -->
      <div class="p-3.5 rounded-xl bg-amber-500/10 dark:bg-amber-500/10 border border-amber-500/30 dark:border-amber-500/30 flex items-start gap-3">
        <svg class="w-5 h-5 text-amber-600 dark:text-amber-400 shrink-0 mt-0.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"/>
        </svg>
        <div class="text-slate-700 dark:text-slate-300 leading-relaxed text-xs">
          <strong class="font-semibold text-slate-900 dark:text-slate-100">Disclaimer:</strong> This calculator is for informational and educational purposes only and should not be construed as legal, tax, or financial advice. Individual payroll practices, MOU provisions, supplemental specialty pays, and personal tax scenarios vary. Please consult with a qualified tax professional (CPA, Enrolled Agent, or Tax Attorney) for advice regarding your specific tax liabilities, obligations, and filing options.
        </div>
      </div>
      <div class="flex flex-col sm:flex-row items-center justify-between gap-2 pt-1 text-[11px] text-slate-400 dark:text-slate-500">
        <p>Built for California First Responders (Law Enforcement, Firefighters, EMS). Incorporating CalPERS, LAFPP, OCERS, LACERA &amp; 2026 Tax Rules.</p>
        <p>© 2026 California First Responder Tax &amp; Pension Modeling.</p>
      </div>
    </div>
  </footer>

  <!-- Calculator Logic Script -->
  <script>
    // 2026 Tax Rates & Pension Systems Constants
    const TAX_2026 = {
      LIMIT_457_STANDARD: 24500,
      LIMIT_457_CATCHUP: 32500,
      SS_WAGE_BASE: 184500,
      SS_RATE: 0.062,
      MEDICARE_RATE: 0.0145,
      ADDITIONAL_MEDICARE_RATE: 0.009,
      ADDITIONAL_MEDICARE_THRESHOLD_SINGLE: 200000,
      ADDITIONAL_MEDICARE_THRESHOLD_MFJ: 250000,
      PAY_PERIODS_PER_YEAR: 26,

      // Pension Systems and Member Contribution Rates on Base Pay Only
      PENSION_SYSTEMS: {
        calpers: {
          name: 'CalPERS Safety (2.7% @ 57)',
          shortName: 'CalPERS (13.75%)',
          rate: 0.1375,
          hasSs: false,
          baseOnly: true,
          label: '13.75% on base pay only',
          badge: 'Section 218 SS Exempt',
          notice: 'CalPERS Safety member contribution is fixed at 13.75% on base pay only. Overtime earnings are strictly non-pensionable (PEPRA rule), and members are exempt from 6.2% Social Security under Section 218.'
        },
        lafpp_tier6: {
          name: 'LAFPP Tier 6 - LAPD / LAFD',
          shortName: 'LAFPP Tier 6 (11%)',
          rate: 0.1100,
          hasSs: false,
          baseOnly: true,
          label: '11.00% on base pay only (9% pension + 2% retiree health)',
          badge: 'Section 218 SS Exempt',
          notice: 'LAFPP Tier 6 (LAPD / LAFD) sworn members contribute a flat 11.00% on base salary only (9% toward pension + 2% toward retiree health). Overtime is exempt from pension deductions, and City of LA sworn personnel do not pay Social Security.'
        },
        ocers_plan_v: {
          name: 'OCERS Plan V - Safety 2.7% @ 57',
          shortName: 'OCERS Plan V (14.61%)',
          rate: 0.1461,
          hasSs: false,
          baseOnly: true,
          label: '14.61% on base pay only',
          badge: 'Section 218 SS Exempt',
          notice: 'OCERS Plan V Safety members (Orange County Sheriff & OCFA Firefighters) contribute 14.61% on base salary only under PEPRA. Overtime is 100% exempt from pension deductions, and members are exempt from 6.2% Social Security.'
        },
        ocers_plan_u: {
          name: 'OCERS Plan U - General 2.5% @ 67',
          shortName: 'OCERS Plan U (10.5%)',
          rate: 0.1050,
          hasSs: true,
          baseOnly: true,
          label: '10.50% on base pay only + Social Security (6.2%)',
          badge: 'Social Security + Pension',
          notice: 'OCERS Plan U General members (County general employees & attorneys) contribute 10.50% on base pay only. Overtime is non-pensionable. Plan U is coordinated with Social Security (6.2% up to $184,500).'
        },
        lacera_plan_c: {
          name: 'LACERA Plan C - Safety 2.7% @ 57',
          shortName: 'LACERA Plan C (15.36%)',
          rate: 0.1536,
          hasSs: false,
          baseOnly: true,
          label: '15.36% on base pay only',
          badge: 'Section 218 SS Exempt',
          notice: 'LACERA Safety Plan C (LA County Sheriff & Fire) member contribution is a flat 15.36% on base salary only. Overtime is 100% exempt from pension withholding, and safety members are exempt from 6.2% Social Security.'
        },
        lacera_plan_g: {
          name: 'LACERA Plan G - General 2.5% @ 67',
          shortName: 'LACERA Plan G (9.22%)',
          rate: 0.0922,
          hasSs: true,
          baseOnly: true,
          label: '9.22% on base pay only + Social Security (6.2%)',
          badge: 'Social Security + Pension',
          notice: 'LACERA General Plan G members contribute 9.22% on base salary only. Overtime is exempt from pension deductions. Members are coordinated with Social Security (6.2% up to $184,500).'
        },
        social_security: {
          name: 'Social Security Only',
          shortName: 'Social Security Only',
          rate: 0.0000,
          hasSs: true,
          baseOnly: false,
          label: 'FICA OASDI 6.2% up to $184,500 Cap',
          badge: 'Social Security Active (6.2%)',
          notice: 'Social Security FICA OASDI tax of 6.2% applies to all earnings up to $184,500/yr. Public pension deduction is $0.00.'
        }
      },

      // 2026 Federal Standard Deductions
      FED_STD_DEDUCTION: {
        single: 16100,
        mfj: 32200,
        mfs: 16100,
        hoh: 24150
      },

      // 2026 Federal Tax Brackets
      FED_BRACKETS: {
        single: [
          { max: 12400, rate: 0.10 },
          { max: 50400, rate: 0.12 },
          { max: 105700, rate: 0.22 },
          { max: 201775, rate: 0.24 },
          { max: 256225, rate: 0.32 },
          { max: 640600, rate: 0.35 },
          { max: Infinity, rate: 0.37 }
        ],
        mfj: [
          { max: 24800, rate: 0.10 },
          { max: 100800, rate: 0.12 },
          { max: 211400, rate: 0.22 },
          { max: 403550, rate: 0.24 },
          { max: 512450, rate: 0.32 },
          { max: 768700, rate: 0.35 },
          { max: Infinity, rate: 0.37 }
        ],
        mfs: [
          { max: 12400, rate: 0.10 },
          { max: 50400, rate: 0.12 },
          { max: 105700, rate: 0.22 },
          { max: 201775, rate: 0.24 },
          { max: 256225, rate: 0.32 },
          { max: 384350, rate: 0.35 },
          { max: Infinity, rate: 0.37 }
        ],
        hoh: [
          { max: 17700, rate: 0.10 },
          { max: 67450, rate: 0.12 },
          { max: 105700, rate: 0.22 },
          { max: 201750, rate: 0.24 },
          { max: 256200, rate: 0.32 },
          { max: 640600, rate: 0.35 },
          { max: Infinity, rate: 0.37 }
        ]
      },

      // 2026 California Standard Deductions
      CA_STD_DEDUCTION: {
        single: 5900,
        mfj: 11800,
        mfs: 5900,
        hoh: 11800
      },

      // 2026 California Exemption Credits
      CA_EXEMPTION_CREDITS: {
        personal_single: 158,
        personal_mfj: 316,
        personal_mfs: 158,
        personal_hoh: 158,
        dependent: 491
      },

      // 2026 California Brackets
      CA_BRACKETS: {
        single: [
          { max: 11456, rate: 0.01 },
          { max: 27157, rate: 0.02 },
          { max: 42861, rate: 0.04 },
          { max: 59498, rate: 0.06 },
          { max: 75197, rate: 0.08 },
          { max: 384109, rate: 0.093 },
          { max: 460927, rate: 0.103 },
          { max: 768213, rate: 0.113 },
          { max: Infinity, rate: 0.123 }
        ],
        mfj: [
          { max: 22912, rate: 0.01 },
          { max: 54314, rate: 0.02 },
          { max: 85722, rate: 0.04 },
          { max: 118996, rate: 0.06 },
          { max: 150394, rate: 0.08 },
          { max: 768218, rate: 0.093 },
          { max: 921854, rate: 0.103 },
          { max: 1536426, rate: 0.113 },
          { max: Infinity, rate: 0.123 }
        ],
        mfs: [
          { max: 11456, rate: 0.01 },
          { max: 27157, rate: 0.02 },
          { max: 42861, rate: 0.04 },
          { max: 59498, rate: 0.06 },
          { max: 75197, rate: 0.08 },
          { max: 384109, rate: 0.093 },
          { max: 460927, rate: 0.103 },
          { max: 768213, rate: 0.113 },
          { max: Infinity, rate: 0.123 }
        ],
        hoh: [
          { max: 22926, rate: 0.01 },
          { max: 54316, rate: 0.02 },
          { max: 70017, rate: 0.04 },
          { max: 86654, rate: 0.06 },
          { max: 102354, rate: 0.08 },
          { max: 522386, rate: 0.093 },
          { max: 626861, rate: 0.103 },
          { max: 1044771, rate: 0.113 },
          { max: Infinity, rate: 0.123 }
        ]
      }
    };

    // Application State
    const state = {
      isPercent457: true,
      spouseIsPercent457: true,
      useItemized: false,
      viewMode: 'both',
      isDark: false
    };

    // DOM Elements Mapping
    const elements = {
      pensionSystemSelect: document.getElementById('pensionSystemSelect'),
      pensionBadge: document.getElementById('pensionBadge'),
      pensionNotice: document.getElementById('pensionNotice'),
      calpersContainer: document.getElementById('calpersContainer'),
      labelPrimaryPensionTitle: document.getElementById('labelPrimaryPensionTitle'),
      labelPrimaryPensionRate: document.getElementById('labelPrimaryPensionRate'),
      socialSecurityNoticeBox: document.getElementById('socialSecurityNoticeBox'),

      // Primary Custom Pension Elements
      primaryCustomPensionContainer: document.getElementById('primaryCustomPensionContainer'),
      primaryCustomPensionName: document.getElementById('primaryCustomPensionName'),
      primaryCustomPensionRate: document.getElementById('primaryCustomPensionRate'),
      primaryCustomPensionBaseOnly: document.getElementById('primaryCustomPensionBaseOnly'),
      primaryCustomPensionSocialSecurity: document.getElementById('primaryCustomPensionSocialSecurity'),

      hourlyRateInput: document.getElementById('hourlyRateInput'),
      hourlyRateSlider: document.getElementById('hourlyRateSlider'),
      otRateDisplay: document.getElementById('otRateDisplay'),
      baseHoursInput: document.getElementById('baseHoursInput'),
      otHoursInput: document.getElementById('otHoursInput'),
      otHoursSlider: document.getElementById('otHoursSlider'),
      sliderOtVal: document.getElementById('sliderOtVal'),

      calcBaseFormula: document.getElementById('calcBaseFormula'),
      calcBasePay: document.getElementById('calcBasePay'),
      calcOtFormula: document.getElementById('calcOtFormula'),
      calcOtPay: document.getElementById('calcOtPay'),
      calcTotalGross: document.getElementById('calcTotalGross'),

      biweeklyCalpersDisplay: document.getElementById('biweeklyCalpersDisplay'),
      calpersOtSavings: document.getElementById('calpersOtSavings'),
      calpersSavedAmount: document.getElementById('calpersSavedAmount'),
      biweeklySsDisplay: document.getElementById('biweeklySsDisplay'),

      toggle457Percent: document.getElementById('toggle457Percent'),
      toggle457Dollar: document.getElementById('toggle457Dollar'),
      unit457Prefix: document.getElementById('unit457Prefix'),
      unit457Suffix: document.getElementById('unit457Suffix'),
      input457Value: document.getElementById('input457Value'),
      calc457Biweekly: document.getElementById('calc457Biweekly'),
      note457Fluctuation: document.getElementById('note457Fluctuation'),
      annualized457Pace: document.getElementById('annualized457Pace'),
      bar457Limit: document.getElementById('bar457Limit'),
      label457Cap: document.getElementById('label457Cap'),
      checkAge50CatchUp: document.getElementById('checkAge50CatchUp'),

      inputAdditionalPretax: document.getElementById('inputAdditionalPretax'),
      inputPostTaxDeductions: document.getElementById('inputPostTaxDeductions'),

      // Spouse Elements
      checkIncludeSpouse: document.getElementById('checkIncludeSpouse'),
      spousePanelContainer: document.getElementById('spousePanelContainer'),
      spousePensionSystemSelect: document.getElementById('spousePensionSystemSelect'),
      spouseCustomPensionContainer: document.getElementById('spouseCustomPensionContainer'),
      spouseCustomPensionName: document.getElementById('spouseCustomPensionName'),
      spouseCustomPensionRate: document.getElementById('spouseCustomPensionRate'),
      spouseCustomPensionBaseOnly: document.getElementById('spouseCustomPensionBaseOnly'),
      spouseCustomPensionSocialSecurity: document.getElementById('spouseCustomPensionSocialSecurity'),

      spouseHourlyRateInput: document.getElementById('spouseHourlyRateInput'),
      spouseHourlyRateSlider: document.getElementById('spouseHourlyRateSlider'),
      spouseOtRateDisplay: document.getElementById('spouseOtRateDisplay'),
      spouseBaseHoursInput: document.getElementById('spouseBaseHoursInput'),
      spouseOtHoursInput: document.getElementById('spouseOtHoursInput'),
      spouseOtHoursSlider: document.getElementById('spouseOtHoursSlider'),
      spouseSliderOtVal: document.getElementById('spouseSliderOtVal'),

      spouseCalcBaseFormula: document.getElementById('spouseCalcBaseFormula'),
      spouseCalcBasePay: document.getElementById('spouseCalcBasePay'),
      spouseCalcOtFormula: document.getElementById('spouseCalcOtFormula'),
      spouseCalcOtPay: document.getElementById('spouseCalcOtPay'),
      spouseCalcTotalGross: document.getElementById('spouseCalcTotalGross'),

      spouseCalpersContainer: document.getElementById('spouseCalpersContainer'),
      labelSpousePensionTitle: document.getElementById('labelSpousePensionTitle'),
      spouseBiweeklyCalpersDisplay: document.getElementById('spouseBiweeklyCalpersDisplay'),
      spouseCalpersOtSavings: document.getElementById('spouseCalpersOtSavings'),
      spouseCalpersSavedAmount: document.getElementById('spouseCalpersSavedAmount'),
      spouseSocialSecurityNoticeBox: document.getElementById('spouseSocialSecurityNoticeBox'),
      spouseBiweeklySsDisplay: document.getElementById('spouseBiweeklySsDisplay'),

      spouseToggle457Percent: document.getElementById('spouseToggle457Percent'),
      spouseToggle457Dollar: document.getElementById('spouseToggle457Dollar'),
      spouseUnit457Prefix: document.getElementById('spouseUnit457Prefix'),
      spouseUnit457Suffix: document.getElementById('spouseUnit457Suffix'),
      spouseInput457Value: document.getElementById('spouseInput457Value'),
      spouseCalc457Biweekly: document.getElementById('spouseCalc457Biweekly'),
      spouseNote457Fluctuation: document.getElementById('spouseNote457Fluctuation'),
      spouseAnnualized457Pace: document.getElementById('spouseAnnualized457Pace'),
      spouseBar457Limit: document.getElementById('spouseBar457Limit'),
      spouseLabel457Cap: document.getElementById('spouseLabel457Cap'),
      spouseCheckAge50CatchUp: document.getElementById('spouseCheckAge50CatchUp'),

      spouseInputAdditionalPretax: document.getElementById('spouseInputAdditionalPretax'),
      spouseInputPostTaxDeductions: document.getElementById('spouseInputPostTaxDeductions'),

      // Alimony Deduction
      inputAlimonyPaid: document.getElementById('inputAlimonyPaid'),

      selectFilingStatus: document.getElementById('selectFilingStatus'),
      inputChildren: document.getElementById('inputChildren'),
      btnDeductionStandard: document.getElementById('btnDeductionStandard'),
      btnDeductionItemized: document.getElementById('btnDeductionItemized'),
      deductionDisplayLabel: document.getElementById('deductionDisplayLabel'),
      itemizedContainer: document.getElementById('itemizedContainer'),
      inputItemizedAmount: document.getElementById('inputItemizedAmount'),

      // Hero Elements
      heroCardContainer: document.getElementById('heroCardContainer'),
      heroTitleLabel: document.getElementById('heroTitleLabel'),
      heroNetBiweekly: document.getElementById('heroNetBiweekly'),
      heroNetAnnual: document.getElementById('heroNetAnnual'),
      heroSubNetBreakdown: document.getElementById('heroSubNetBreakdown'),
      heroNetPrimary: document.getElementById('heroNetPrimary'),
      heroNetSpouse: document.getElementById('heroNetSpouse'),
      heroCompositeTaxesBiweekly: document.getElementById('heroCompositeTaxesBiweekly'),
      heroEffectiveRate: document.getElementById('heroEffectiveRate'),
      heroPensionBiweekly: document.getElementById('heroPensionBiweekly'),
      heroPensionRateLabel: document.getElementById('heroPensionRateLabel'),
      hero457Biweekly: document.getElementById('hero457Biweekly'),
      heroOtherDeductionsBiweekly: document.getElementById('heroOtherDeductionsBiweekly'),
      labelGrossDistribution: document.getElementById('labelGrossDistribution'),
      barNet: document.getElementById('barNet'),
      barTaxes: document.getElementById('barTaxes'),
      barPension: document.getElementById('barPension'),
      bar457: document.getElementById('bar457'),
      barOther: document.getElementById('barOther'),
      pctNet: document.getElementById('pctNet'),
      pctTaxes: document.getElementById('pctTaxes'),
      pctPension: document.getElementById('pctPension'),
      pct457: document.getElementById('pct457'),
      pctOther: document.getElementById('pctOther'),

      // Biweekly Line Items
      biweeklyBadgeLabel: document.getElementById('biweeklyBadgeLabel'),
      rowBaseHours: document.getElementById('rowBaseHours'),
      rowHourlyRate: document.getElementById('rowHourlyRate'),
      rowBasePay: document.getElementById('rowBasePay'),
      rowOtHours: document.getElementById('rowOtHours'),
      rowOtRate: document.getElementById('rowOtRate'),
      rowOtPay: document.getElementById('rowOtPay'),
      rowSpouseEarningsItem: document.getElementById('rowSpouseEarningsItem'),
      rowSpouseBasePay: document.getElementById('rowSpouseBasePay'),
      rowSpouseOtPay: document.getElementById('rowSpouseOtPay'),
      rowSpouseGrossPay: document.getElementById('rowSpouseGrossPay'),
      labelRowGross: document.getElementById('labelRowGross'),
      rowGrossPay: document.getElementById('rowGrossPay'),
      labelRowPrimaryPension: document.getElementById('labelRowPrimaryPension'),
      rowCalpers: document.getElementById('rowCalpers'),
      row457: document.getElementById('row457'),
      rowPretaxHealth: document.getElementById('rowPretaxHealth'),
      rowSpousePretaxItem: document.getElementById('rowSpousePretaxItem'),
      rowSpouseTotalPretax: document.getElementById('rowSpouseTotalPretax'),
      rowTotalPretax: document.getElementById('rowTotalPretax'),
      rowFedTaxableWage: document.getElementById('rowFedTaxableWage'),
      rowFedTax: document.getElementById('rowFedTax'),
      rowCaTax: document.getElementById('rowCaTax'),
      rowMedicareTax: document.getElementById('rowMedicareTax'),
      rowSsItem: document.getElementById('rowSsItem'),
      rowSsTax: document.getElementById('rowSsTax'),
      rowTotalTaxes: document.getElementById('rowTotalTaxes'),
      rowPostTax: document.getElementById('rowPostTax'),
      rowSpousePostTaxItem: document.getElementById('rowSpousePostTaxItem'),
      rowSpousePostTax: document.getElementById('rowSpousePostTax'),
      rowAlimonyItem: document.getElementById('rowAlimonyItem'),
      rowAlimonyDeduction: document.getElementById('rowAlimonyDeduction'),
      labelRowNet: document.getElementById('labelRowNet'),
      rowNetPay: document.getElementById('rowNetPay'),

      // Annual Line Items
      annFedStatusLabel: document.getElementById('annFedStatusLabel'),
      annCaStatusLabel: document.getElementById('annCaStatusLabel'),
      annGrossWages: document.getElementById('annGrossWages'),
      annTotalPretax: document.getElementById('annTotalPretax'),
      annAlimonyRow: document.getElementById('annAlimonyRow'),
      annAlimonyDeduction: document.getElementById('annAlimonyDeduction'),
      annFedAgi: document.getElementById('annFedAgi'),
      annFedStdDed: document.getElementById('annFedStdDed'),
      annFedTaxable: document.getElementById('annFedTaxable'),
      annFedTaxPreCredit: document.getElementById('annFedTaxPreCredit'),
      annFedCredits: document.getElementById('annFedCredits'),
      annFedTaxFinal: document.getElementById('annFedTaxFinal'),

      annCaAgi: document.getElementById('annCaAgi'),
      annCaStdDed: document.getElementById('annCaStdDed'),
      annCaTaxable: document.getElementById('annCaTaxable'),
      annCaTaxPreCredit: document.getElementById('annCaTaxPreCredit'),
      annCaCredits: document.getElementById('annCaCredits'),
      annCaTaxFinal: document.getElementById('annCaTaxFinal'),
      annCaMarginal: document.getElementById('annCaMarginal'),

      annCombinedEffective: document.getElementById('annCombinedEffective'),
      annSummaryFed: document.getElementById('annSummaryFed'),
      annSummaryCa: document.getElementById('annSummaryCa'),
      annSummaryMed: document.getElementById('annSummaryMed'),
      annSummaryPensionTitle: document.getElementById('annSummaryPensionTitle'),
      annSummaryPension: document.getElementById('annSummaryPension'),

      // Views
      viewTabBoth: document.getElementById('viewTabBoth'),
      viewTabBiweekly: document.getElementById('viewTabBiweekly'),
      viewTabAnnual: document.getElementById('viewTabAnnual'),
      biweeklySection: document.getElementById('biweeklySection'),
      annualSection: document.getElementById('annualSection'),

      // Theme Toggle Buttons
      btnThemeToggle: document.getElementById('btnThemeToggle'),
      themeIconSun: document.getElementById('themeIconSun'),
      themeIconMoon: document.getElementById('themeIconMoon'),

      // Presets & Buttons
      presetPolice: document.getElementById('presetPolice'),
      presetFire: document.getElementById('presetFire'),
      presetParamedic: document.getElementById('presetParamedic'),
      btnPrint: document.getElementById('btnPrint'),
      btnReset: document.getElementById('btnReset')
    };

    // Currency Formatting Helpers
    function fmt(val) {
      return '$' + Math.max(0, val).toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
    }

    function fmtNoCents(val) {
      return '$' + Math.max(0, Math.round(val)).toLocaleString('en-US');
    }

    // Theme (Day / Night Mode) Handler
    function setTheme(isDark) {
      state.isDark = isDark;
      if (isDark) {
        document.documentElement.classList.add('dark');
        elements.themeIconSun.classList.remove('hidden');
        elements.themeIconMoon.classList.add('hidden');
      } else {
        document.documentElement.classList.remove('dark');
        elements.themeIconSun.classList.add('hidden');
        elements.themeIconMoon.classList.remove('hidden');
      }
      try {
        localStorage.setItem('ca_first_responder_theme', isDark ? 'dark' : 'light');
      } catch (e) {}
    }

    // Graduated Progressive Bracket Calculation
    function calculateGraduatedTax(taxableIncome, brackets) {
      if (taxableIncome <= 0) return 0;
      let tax = 0;
      let previousMax = 0;

      for (let i = 0; i < brackets.length; i++) {
        const bracket = brackets[i];
        if (taxableIncome > bracket.max) {
          tax += (bracket.max - previousMax) * bracket.rate;
          previousMax = bracket.max;
        } else {
          tax += (taxableIncome - previousMax) * bracket.rate;
          break;
        }
      }

      // California 1% Mental Health Services Tax surcharge on taxable income over $1,000,000
      if (taxableIncome > 1000000 && brackets[0].rate < 0.05) {
        tax += (taxableIncome - 1000000) * 0.01;
      }

      return tax;
    }

    // Marginal Tax Rate Identifier
    function getMarginalRate(taxableIncome, brackets) {
      if (taxableIncome <= 0) return (brackets[0].rate * 100).toFixed(1) + '%';
      for (let i = 0; i < brackets.length; i++) {
        if (taxableIncome <= brackets[i].max) {
          let r = brackets[i].rate;
          if (taxableIncome > 1000000) r += 0.01;
          return (r * 100).toFixed(1) + '%';
        }
      }
      return '13.3%';
    }

    // Main Calculator Function
    function calculate() {
      // 1. Primary Earner Pension System Selection
      const primaryKey = elements.pensionSystemSelect.value;
      let primaryPlan = null;

      if (primaryKey === 'other') {
        elements.primaryCustomPensionContainer.classList.remove('hidden');
        const customName = elements.primaryCustomPensionName.value.trim() || 'Custom Pension';
        const customRate = Math.max(0, parseFloat(elements.primaryCustomPensionRate.value) || 0) / 100;
        const customBaseOnly = elements.primaryCustomPensionBaseOnly.checked;
        const customHasSs = elements.primaryCustomPensionSocialSecurity.checked;

        primaryPlan = {
          name: customName,
          shortName: customName,
          rate: customRate,
          hasSs: customHasSs,
          baseOnly: customBaseOnly,
          label: `${(customRate * 100).toFixed(2)}% ${customBaseOnly ? 'on base pay only' : 'on total gross'}`,
          badge: customHasSs ? 'Social Security Active' : 'Section 218 SS Exempt',
          notice: `Custom pension plan "${customName}": ${(customRate * 100).toFixed(2)}% contribution ${customBaseOnly ? 'calculated strictly from base salary (overtime exempt)' : 'calculated from total pay'}. ${customHasSs ? 'Participates in 6.2% Social Security.' : 'Exempt from 6.2% Social Security.'}`
        };
      } else {
        elements.primaryCustomPensionContainer.classList.add('hidden');
        primaryPlan = TAX_2026.PENSION_SYSTEMS[primaryKey] || TAX_2026.PENSION_SYSTEMS.calpers;
      }

      // Update primary pension card label and notice
      elements.labelPrimaryPensionTitle.innerHTML = `
        <svg class="w-4 h-4 mr-1 text-amber-700 dark:text-amber-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>
        ${primaryPlan.name}
      `;
      elements.labelPrimaryPensionRate.innerHTML = `${(primaryPlan.rate * 100).toFixed(2)}% on <span class="underline decoration-amber-500 font-semibold">${primaryPlan.baseOnly ? 'base pay only' : 'total pay'}</span>`;
      elements.pensionBadge.textContent = primaryPlan.badge;
      elements.pensionNotice.innerHTML = `<p><strong class="text-slate-900 dark:text-white">${primaryPlan.name}:</strong> ${primaryPlan.notice}</p>`;

      // 2. Primary Earnings and Rates
      const primaryHourlyRate = Math.max(0, parseFloat(elements.hourlyRateInput.value) || 0);
      const primaryOtRate = primaryHourlyRate * 1.5;
      elements.otRateDisplay.textContent = fmt(primaryOtRate) + '/hr';

      if (Math.abs(parseFloat(elements.hourlyRateSlider.value) - primaryHourlyRate) > 0.1 && primaryHourlyRate <= 150) {
        elements.hourlyRateSlider.value = primaryHourlyRate;
      }

      const primaryBaseHours = Math.max(0, parseFloat(elements.baseHoursInput.value) || 0);
      const primaryOtHours = Math.max(0, parseFloat(elements.otHoursInput.value) || 0);

      if (Math.abs(parseFloat(elements.otHoursSlider.value) - primaryOtHours) > 0.1 && primaryOtHours <= 60) {
        elements.otHoursSlider.value = primaryOtHours;
      }
      elements.sliderOtVal.textContent = `${primaryOtHours} hrs`;

      const primaryBasePay = primaryBaseHours * primaryHourlyRate;
      const primaryOtPay = primaryOtHours * primaryOtRate;
      const primaryGrossBiweekly = primaryBasePay + primaryOtPay;
      const primaryGrossAnnual = primaryGrossBiweekly * TAX_2026.PAY_PERIODS_PER_YEAR;

      elements.calcBaseFormula.textContent = `${primaryBaseHours} hrs @ $${primaryHourlyRate.toFixed(2)}`;
      elements.calcBasePay.textContent = fmt(primaryBasePay);
      elements.calcOtFormula.textContent = `${primaryOtHours} hrs @ $${primaryOtRate.toFixed(2)}`;
      elements.calcOtPay.textContent = fmt(primaryOtPay);
      elements.calcTotalGross.textContent = fmt(primaryGrossBiweekly);

      // Primary Pension or Social Security
      const primaryPensionablePay = primaryPlan.baseOnly ? primaryBasePay : primaryGrossBiweekly;
      let primaryBiweeklyCalpers = primaryPensionablePay * primaryPlan.rate;
      let primaryAnnualCalpers = primaryBiweeklyCalpers * TAX_2026.PAY_PERIODS_PER_YEAR;

      let primaryBiweeklySs = 0;
      let primaryAnnualSs = 0;
      if (primaryPlan.hasSs) {
        const ssTaxableBiweekly = Math.min(primaryGrossBiweekly, TAX_2026.SS_WAGE_BASE / TAX_2026.PAY_PERIODS_PER_YEAR);
        primaryBiweeklySs = ssTaxableBiweekly * TAX_2026.SS_RATE;
        primaryAnnualSs = Math.min(primaryGrossAnnual, TAX_2026.SS_WAGE_BASE) * TAX_2026.SS_RATE;
        elements.socialSecurityNoticeBox.classList.remove('hidden');
        elements.biweeklySsDisplay.textContent = fmt(primaryBiweeklySs);
      } else {
        elements.socialSecurityNoticeBox.classList.add('hidden');
      }

      if (primaryPlan.rate > 0) {
        elements.calpersContainer.classList.remove('hidden');
        const primarySavedFromExemptOt = primaryPlan.baseOnly ? (primaryOtPay * primaryPlan.rate) : 0;
        elements.calpersOtSavings.textContent = primaryOtPay.toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
        elements.calpersSavedAmount.textContent = primarySavedFromExemptOt.toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
        elements.biweeklyCalpersDisplay.textContent = fmt(primaryBiweeklyCalpers);
      } else {
        elements.calpersContainer.classList.add('hidden');
      }

      // Primary 457(b)
      const primaryIsAge50 = elements.checkAge50CatchUp.checked;
      const primaryMax457 = primaryIsAge50 ? TAX_2026.LIMIT_457_CATCHUP : TAX_2026.LIMIT_457_STANDARD;
      elements.label457Cap.textContent = `2026 Max: $${primaryMax457.toLocaleString()}`;

      const primaryRaw457 = Math.max(0, parseFloat(elements.input457Value.value) || 0);
      let primaryBiweekly457 = 0;
      if (state.isPercent457) {
        primaryBiweekly457 = primaryGrossBiweekly * (primaryRaw457 / 100);
        elements.note457Fluctuation.textContent = `* ${primaryRaw457}% scales dynamically with gross pay ($${primaryGrossBiweekly.toFixed(2)}) including overtime!`;
      } else {
        primaryBiweekly457 = primaryRaw457;
        elements.note457Fluctuation.textContent = `* Fixed amount of $${primaryBiweekly457.toFixed(2)} deducted per pay period`;
      }
      primaryBiweekly457 = Math.min(primaryBiweekly457, Math.max(0, primaryGrossBiweekly - primaryBiweeklyCalpers));
      const primaryAnnual457 = Math.min(primaryMax457, primaryBiweekly457 * TAX_2026.PAY_PERIODS_PER_YEAR);

      elements.calc457Biweekly.textContent = fmt(primaryBiweekly457);
      elements.annualized457Pace.textContent = `Pace: $${Math.round(primaryBiweekly457 * 26).toLocaleString()} / yr`;
      const primaryPctLimit = Math.min(100, ((primaryBiweekly457 * 26) / primaryMax457) * 100);
      elements.bar457Limit.style.width = primaryPctLimit + '%';

      // Primary Additional Pre-Tax and Post-Tax
      const primaryAdditionalPretax = Math.max(0, parseFloat(elements.inputAdditionalPretax.value) || 0);
      const primaryPostTax = Math.max(0, parseFloat(elements.inputPostTaxDeductions.value) || 0);
      const primaryTotalPretaxBiweekly = primaryBiweeklyCalpers + primaryBiweekly457 + primaryAdditionalPretax;
      const primaryTotalPretaxAnnual = primaryAnnualCalpers + primaryAnnual457 + (primaryAdditionalPretax * TAX_2026.PAY_PERIODS_PER_YEAR);

      // 3. Spouse Income Section
      const hasSpouse = elements.checkIncludeSpouse.checked;
      let spouseGrossBiweekly = 0;
      let spouseGrossAnnual = 0;
      let spouseBiweeklyCalpers = 0;
      let spouseAnnualCalpers = 0;
      let spouseBiweeklySs = 0;
      let spouseAnnualSs = 0;
      let spouseBiweekly457 = 0;
      let spouseAnnual457 = 0;
      let spouseAdditionalPretax = 0;
      let spousePostTax = 0;
      let spouseTotalPretaxBiweekly = 0;
      let spouseTotalPretaxAnnual = 0;
      let spouseBasePay = 0;
      let spouseOtPay = 0;
      let spousePlan = TAX_2026.PENSION_SYSTEMS.calpers;

      if (hasSpouse) {
        elements.spousePanelContainer.classList.remove('hidden');
        elements.rowSpouseEarningsItem.classList.remove('hidden');
        elements.rowSpousePretaxItem.classList.remove('hidden');
        elements.rowSpousePostTaxItem.classList.remove('hidden');
        elements.heroSubNetBreakdown.classList.remove('hidden');

        const spouseKey = elements.spousePensionSystemSelect.value;
        if (spouseKey === 'other') {
          elements.spouseCustomPensionContainer.classList.remove('hidden');
          const customSpouseName = elements.spouseCustomPensionName.value.trim() || 'Spouse Custom Pension';
          const customSpouseRate = Math.max(0, parseFloat(elements.spouseCustomPensionRate.value) || 0) / 100;
          const customSpouseBaseOnly = elements.spouseCustomPensionBaseOnly.checked;
          const customSpouseHasSs = elements.spouseCustomPensionSocialSecurity.checked;

          spousePlan = {
            name: customSpouseName,
            shortName: customSpouseName,
            rate: customSpouseRate,
            hasSs: customSpouseHasSs,
            baseOnly: customSpouseBaseOnly,
            label: `${(customSpouseRate * 100).toFixed(2)}% ${customSpouseBaseOnly ? 'on base pay only' : 'on total gross'}`,
            badge: customSpouseHasSs ? 'Social Security Active' : 'Section 218 SS Exempt',
            notice: `Spouse custom plan "${customSpouseName}": ${(customSpouseRate * 100).toFixed(2)}% contribution.`
          };
        } else {
          elements.spouseCustomPensionContainer.classList.add('hidden');
          spousePlan = TAX_2026.PENSION_SYSTEMS[spouseKey] || TAX_2026.PENSION_SYSTEMS.calpers;
        }

        elements.labelSpousePensionTitle.textContent = `Spouse ${spousePlan.name}:`;

        const spouseRate = Math.max(0, parseFloat(elements.spouseHourlyRateInput.value) || 0);
        const spouseOtRate = spouseRate * 1.5;
        elements.spouseOtRateDisplay.textContent = fmt(spouseOtRate) + '/hr';

        if (Math.abs(parseFloat(elements.spouseHourlyRateSlider.value) - spouseRate) > 0.1 && spouseRate <= 150) {
          elements.spouseHourlyRateSlider.value = spouseRate;
        }

        const spouseBaseH = Math.max(0, parseFloat(elements.spouseBaseHoursInput.value) || 0);
        const spouseOtH = Math.max(0, parseFloat(elements.spouseOtHoursInput.value) || 0);

        if (Math.abs(parseFloat(elements.spouseOtHoursSlider.value) - spouseOtH) > 0.1 && spouseOtH <= 60) {
          elements.spouseOtHoursSlider.value = spouseOtH;
        }
        elements.spouseSliderOtVal.textContent = `${spouseOtH} hrs`;

        spouseBasePay = spouseBaseH * spouseRate;
        spouseOtPay = spouseOtH * spouseOtRate;
        spouseGrossBiweekly = spouseBasePay + spouseOtPay;
        spouseGrossAnnual = spouseGrossBiweekly * TAX_2026.PAY_PERIODS_PER_YEAR;

        elements.spouseCalcBaseFormula.textContent = `${spouseBaseH} hrs @ $${spouseRate.toFixed(2)}`;
        elements.spouseCalcBasePay.textContent = fmt(spouseBasePay);
        elements.spouseCalcOtFormula.textContent = `${spouseOtH} hrs @ $${spouseOtRate.toFixed(2)}`;
        elements.spouseCalcOtPay.textContent = fmt(spouseOtPay);
        elements.spouseCalcTotalGross.textContent = fmt(spouseGrossBiweekly);

        // Spouse Pension / SS calculation
        const spousePensionablePay = spousePlan.baseOnly ? spouseBasePay : spouseGrossBiweekly;
        spouseBiweeklyCalpers = spousePensionablePay * spousePlan.rate;
        spouseAnnualCalpers = spouseBiweeklyCalpers * TAX_2026.PAY_PERIODS_PER_YEAR;

        if (spousePlan.hasSs) {
          const spouseSsTaxable = Math.min(spouseGrossBiweekly, TAX_2026.SS_WAGE_BASE / TAX_2026.PAY_PERIODS_PER_YEAR);
          spouseBiweeklySs = spouseSsTaxable * TAX_2026.SS_RATE;
          spouseAnnualSs = Math.min(spouseGrossAnnual, TAX_2026.SS_WAGE_BASE) * TAX_2026.SS_RATE;
          elements.spouseSocialSecurityNoticeBox.classList.remove('hidden');
          elements.spouseBiweeklySsDisplay.textContent = fmt(spouseBiweeklySs);
        } else {
          elements.spouseSocialSecurityNoticeBox.classList.add('hidden');
        }

        if (spousePlan.rate > 0) {
          elements.spouseCalpersContainer.classList.remove('hidden');
          const spouseSavedFromExemptOt = spousePlan.baseOnly ? (spouseOtPay * spousePlan.rate) : 0;
          elements.spouseCalpersOtSavings.textContent = spouseOtPay.toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
          elements.spouseCalpersSavedAmount.textContent = spouseSavedFromExemptOt.toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
          elements.spouseBiweeklyCalpersDisplay.textContent = fmt(spouseBiweeklyCalpers);
        } else {
          elements.spouseCalpersContainer.classList.add('hidden');
        }

        // Spouse 457 / 401k
        const spouseIsAge50 = elements.spouseCheckAge50CatchUp.checked;
        const spouseMax457 = spouseIsAge50 ? TAX_2026.LIMIT_457_CATCHUP : TAX_2026.LIMIT_457_STANDARD;
        elements.spouseLabel457Cap.textContent = `Spouse Max: $${spouseMax457.toLocaleString()}`;

        const spouseRaw457 = Math.max(0, parseFloat(elements.spouseInput457Value.value) || 0);
        if (state.spouseIsPercent457) {
          spouseBiweekly457 = spouseGrossBiweekly * (spouseRaw457 / 100);
          elements.spouseNote457Fluctuation.textContent = `* ${spouseRaw457}% scales with spouse gross ($${spouseGrossBiweekly.toFixed(2)})`;
        } else {
          spouseBiweekly457 = spouseRaw457;
          elements.spouseNote457Fluctuation.textContent = `* Fixed amount of $${spouseBiweekly457.toFixed(2)} deducted per pay period`;
        }
        spouseBiweekly457 = Math.min(spouseBiweekly457, Math.max(0, spouseGrossBiweekly - spouseBiweeklyCalpers));
        spouseAnnual457 = Math.min(spouseMax457, spouseBiweekly457 * TAX_2026.PAY_PERIODS_PER_YEAR);

        elements.spouseCalc457Biweekly.textContent = fmt(spouseBiweekly457);
        elements.spouseAnnualized457Pace.textContent = `Pace: $${Math.round(spouseBiweekly457 * 26).toLocaleString()} / yr`;
        const spousePctLimit = Math.min(100, ((spouseBiweekly457 * 26) / spouseMax457) * 100);
        elements.spouseBar457Limit.style.width = spousePctLimit + '%';

        spouseAdditionalPretax = Math.max(0, parseFloat(elements.spouseInputAdditionalPretax.value) || 0);
        spousePostTax = Math.max(0, parseFloat(elements.spouseInputPostTaxDeductions.value) || 0);

        spouseTotalPretaxBiweekly = spouseBiweeklyCalpers + spouseBiweekly457 + spouseAdditionalPretax;
        spouseTotalPretaxAnnual = spouseAnnualCalpers + spouseAnnual457 + (spouseAdditionalPretax * TAX_2026.PAY_PERIODS_PER_YEAR);

        elements.rowSpouseBasePay.textContent = fmtNoCents(spouseBasePay);
        elements.rowSpouseOtPay.textContent = fmtNoCents(spouseOtPay);
        elements.rowSpouseGrossPay.textContent = fmt(spouseGrossBiweekly);
        elements.rowSpouseTotalPretax.textContent = `-${fmt(spouseTotalPretaxBiweekly)}`;
        elements.rowSpousePostTax.textContent = `-${fmt(spousePostTax)}`;
      } else {
        elements.spousePanelContainer.classList.add('hidden');
        elements.rowSpouseEarningsItem.classList.add('hidden');
        elements.rowSpousePretaxItem.classList.add('hidden');
        elements.rowSpousePostTaxItem.classList.add('hidden');
        elements.heroSubNetBreakdown.classList.add('hidden');
      }

      // 4. Taxable Above-the-Line Deductions (Alimony Paid)
      const annualAlimonyPaid = Math.max(0, parseFloat(elements.inputAlimonyPaid.value) || 0);
      const biweeklyAlimonyPaid = annualAlimonyPaid / TAX_2026.PAY_PERIODS_PER_YEAR;

      if (annualAlimonyPaid > 0) {
        elements.rowAlimonyItem.classList.remove('hidden');
        elements.annAlimonyRow.classList.remove('hidden');
        elements.rowAlimonyDeduction.textContent = `-${fmt(biweeklyAlimonyPaid)}`;
        elements.annAlimonyDeduction.textContent = `-${fmtNoCents(annualAlimonyPaid)}`;
      } else {
        elements.rowAlimonyItem.classList.add('hidden');
        elements.annAlimonyRow.classList.add('hidden');
      }

      // 5. Total Household Aggregates
      const householdGrossBiweekly = primaryGrossBiweekly + spouseGrossBiweekly;
      const householdGrossAnnual = primaryGrossAnnual + spouseGrossAnnual;

      const householdPretaxBiweekly = primaryTotalPretaxBiweekly + spouseTotalPretaxBiweekly;
      const householdPretaxAnnual = primaryTotalPretaxAnnual + spouseTotalPretaxAnnual;

      const householdCalpersBiweekly = primaryBiweeklyCalpers + spouseBiweeklyCalpers;
      const householdCalpersAnnual = primaryAnnualCalpers + spouseAnnualCalpers;

      const householdSsBiweekly = primaryBiweeklySs + spouseBiweeklySs;
      const householdSsAnnual = primaryAnnualSs + spouseAnnualSs;

      const household457Biweekly = primaryBiweekly457 + spouseBiweekly457;
      const household457Annual = primaryAnnual457 + spouseAnnual457;

      const householdAdditionalPretaxBiweekly = primaryAdditionalPretax + spouseAdditionalPretax;
      const householdPostTaxBiweekly = primaryPostTax + spousePostTax;

      // 6. Filing Status & Deductions
      const filingStatus = elements.selectFilingStatus.value;
      const numChildren = Math.max(0, parseInt(elements.inputChildren.value, 10) || 0);

      const fedStdDed = TAX_2026.FED_STD_DEDUCTION[filingStatus];
      const caStdDed = TAX_2026.CA_STD_DEDUCTION[filingStatus];

      let fedDeduction = fedStdDed;
      let caDeduction = caStdDed;

      if (state.useItemized) {
        const itemizedVal = Math.max(0, parseFloat(elements.inputItemizedAmount.value) || 0);
        fedDeduction = Math.max(itemizedVal, fedStdDed);
        caDeduction = Math.max(itemizedVal, caStdDed);
        elements.deductionDisplayLabel.textContent = `Itemized: $${fedDeduction.toLocaleString()} Fed / $${caDeduction.toLocaleString()} CA`;
      } else {
        elements.deductionDisplayLabel.textContent = `2026 Standard ($${fedStdDed.toLocaleString()} Fed / $${caStdDed.toLocaleString()} CA)`;
      }

      // 7. Annual Tax Calculations (Form 1040 & CA 540)
      const annualFedAgi = Math.max(0, householdGrossAnnual - householdPretaxAnnual - annualAlimonyPaid);
      const annualCaAgi = Math.max(0, householdGrossAnnual - householdPretaxAnnual - annualAlimonyPaid);

      const annualFedTaxable = Math.max(0, annualFedAgi - fedDeduction);
      const annualCaTaxable = Math.max(0, annualCaAgi - caDeduction);

      // Federal Income Tax
      const fedBrackets = TAX_2026.FED_BRACKETS[filingStatus];
      const annualFedTaxPreCredit = calculateGraduatedTax(annualFedTaxable, fedBrackets);

      // Child Tax Credit ($2,200/child)
      const ctcPhaseoutThreshold = (filingStatus === 'mfj') ? 400000 : 200000;
      let childCredits = numChildren * 2200;
      if (annualFedAgi > ctcPhaseoutThreshold) {
        const excess = annualFedAgi - ctcPhaseoutThreshold;
        const phaseout = Math.ceil(excess / 1000) * 50;
        childCredits = Math.max(0, childCredits - phaseout);
      }
      const annualFedTaxFinal = Math.max(0, annualFedTaxPreCredit - childCredits);

      // California State Income Tax
      const caBrackets = TAX_2026.CA_BRACKETS[filingStatus];
      const annualCaTaxPreCredit = calculateGraduatedTax(annualCaTaxable, caBrackets);

      let caPersonalCredit = TAX_2026.CA_EXEMPTION_CREDITS.personal_single;
      if (filingStatus === 'mfj') caPersonalCredit = TAX_2026.CA_EXEMPTION_CREDITS.personal_mfj;
      const caDependentCredit = numChildren * TAX_2026.CA_EXEMPTION_CREDITS.dependent;
      const totalCaCredits = caPersonalCredit + caDependentCredit;
      const annualCaTaxFinal = Math.max(0, annualCaTaxPreCredit - totalCaCredits);

      // Medicare Tax (Annual)
      const primaryMedicareWages = Math.max(0, primaryGrossAnnual - (primaryAdditionalPretax * TAX_2026.PAY_PERIODS_PER_YEAR));
      const spouseMedicareWages = Math.max(0, spouseGrossAnnual - (spouseAdditionalPretax * TAX_2026.PAY_PERIODS_PER_YEAR));
      const totalMedicareWagesAnnual = primaryMedicareWages + (hasSpouse ? spouseMedicareWages : 0);

      let annualMedicare = totalMedicareWagesAnnual * TAX_2026.MEDICARE_RATE;
      const addlMedThreshold = (filingStatus === 'mfj') ? TAX_2026.ADDITIONAL_MEDICARE_THRESHOLD_MFJ : TAX_2026.ADDITIONAL_MEDICARE_THRESHOLD_SINGLE;
      if (totalMedicareWagesAnnual > addlMedThreshold) {
        annualMedicare += (totalMedicareWagesAnnual - addlMedThreshold) * TAX_2026.ADDITIONAL_MEDICARE_RATE;
      }

      // Social Security Tax (Annual)
      const annualTotalSs = primaryAnnualSs + (hasSpouse ? spouseAnnualSs : 0);

      // Composite Taxes
      const annualCompositeTaxes = annualFedTaxFinal + annualCaTaxFinal + annualMedicare + annualTotalSs;
      const annualEffectiveRate = householdGrossAnnual > 0 ? (annualCompositeTaxes / householdGrossAnnual) * 100 : 0;
      const annualNetTakeHome = householdGrossAnnual - householdPretaxAnnual - annualCompositeTaxes - (householdPostTaxBiweekly * 26) - annualAlimonyPaid;

      // 8. Biweekly Tax Withholdings
      const biweeklyFedTax = annualFedTaxFinal / TAX_2026.PAY_PERIODS_PER_YEAR;
      const biweeklyCaTax = annualCaTaxFinal / TAX_2026.PAY_PERIODS_PER_YEAR;

      const biweeklyMedicareWages = Math.max(0, householdGrossBiweekly - householdAdditionalPretaxBiweekly);
      let biweeklyMedicare = biweeklyMedicareWages * TAX_2026.MEDICARE_RATE;
      if (totalMedicareWagesAnnual > addlMedThreshold) {
        biweeklyMedicare += (Math.max(0, totalMedicareWagesAnnual - addlMedThreshold) * TAX_2026.ADDITIONAL_MEDICARE_RATE) / TAX_2026.PAY_PERIODS_PER_YEAR;
      }

      const biweeklyCompositeTaxes = biweeklyFedTax + biweeklyCaTax + biweeklyMedicare + householdSsBiweekly;
      const biweeklyEffectiveRate = householdGrossBiweekly > 0 ? (biweeklyCompositeTaxes / householdGrossBiweekly) * 100 : 0;

      // Net Take-Home Paycheck
      const biweeklyNetPay = householdGrossBiweekly - householdPretaxBiweekly - biweeklyCompositeTaxes - householdPostTaxBiweekly - biweeklyAlimonyPaid;

      // Individual net estimations for subtitle if spouse active
      if (hasSpouse && householdGrossBiweekly > 0) {
        const primaryShare = primaryGrossBiweekly / householdGrossBiweekly;
        const spouseShare = spouseGrossBiweekly / householdGrossBiweekly;
        const primaryEstNet = primaryGrossBiweekly - primaryTotalPretaxBiweekly - (biweeklyCompositeTaxes * primaryShare) - primaryPostTax;
        const spouseEstNet = spouseGrossBiweekly - spouseTotalPretaxBiweekly - (biweeklyCompositeTaxes * spouseShare) - spousePostTax;
        elements.heroNetPrimary.textContent = fmt(primaryEstNet);
        elements.heroNetSpouse.textContent = fmt(spouseEstNet);
      }

      // 9. Update DOM Outputs - Hero Section
      if (hasSpouse) {
        elements.heroTitleLabel.textContent = "Combined Household Net Take-Home";
        elements.labelRowGross.textContent = "Combined Household Gross Wages";
        elements.labelRowNet.innerHTML = `<svg class="w-5 h-5 mr-1.5 text-emerald-600 dark:text-emerald-400 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/></svg> Household Net Take-Home:`;
        elements.biweeklyBadgeLabel.textContent = "Family Household";
      } else {
        elements.heroTitleLabel.textContent = "Net Take-Home Paycheck";
        elements.labelRowGross.textContent = "Total Biweekly Gross Wages";
        elements.labelRowNet.innerHTML = `<svg class="w-5 h-5 mr-1.5 text-emerald-600 dark:text-emerald-400 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/></svg> Net Take-Home Paycheck:`;
        elements.biweeklyBadgeLabel.textContent = "Biweekly Check";
      }

      elements.heroNetBiweekly.textContent = fmt(biweeklyNetPay);
      elements.heroNetAnnual.textContent = fmt(annualNetTakeHome);
      elements.heroCompositeTaxesBiweekly.textContent = fmt(biweeklyCompositeTaxes);
      elements.heroEffectiveRate.textContent = `${biweeklyEffectiveRate.toFixed(1)}% of Gross`;
      elements.heroPensionBiweekly.textContent = fmt(householdCalpersBiweekly);
      elements.heroPensionRateLabel.textContent = (primaryPlan.rate > 0) ? `${(primaryPlan.rate * 100).toFixed(1)}% ${primaryPlan.baseOnly ? 'Base' : 'Total'}` : 'Exempts OT';
      elements.hero457Biweekly.textContent = fmt(household457Biweekly);
      elements.heroOtherDeductionsBiweekly.textContent = fmt(householdAdditionalPretaxBiweekly + householdPostTaxBiweekly + biweeklyAlimonyPaid);
      elements.labelGrossDistribution.textContent = `100% of ${fmt(householdGrossBiweekly)}`;

      // Update Bar Widths
      if (householdGrossBiweekly > 0) {
        const pNet = Math.max(0, (biweeklyNetPay / householdGrossBiweekly) * 100);
        const pTax = Math.max(0, (biweeklyCompositeTaxes / householdGrossBiweekly) * 100);
        const pPension = Math.max(0, ((householdCalpersBiweekly + householdSsBiweekly) / householdGrossBiweekly) * 100);
        const p457 = Math.max(0, (household457Biweekly / householdGrossBiweekly) * 100);
        const pOther = Math.max(0, ((householdAdditionalPretaxBiweekly + householdPostTaxBiweekly + biweeklyAlimonyPaid) / householdGrossBiweekly) * 100);

        elements.barNet.style.width = pNet.toFixed(1) + '%';
        elements.barTaxes.style.width = pTax.toFixed(1) + '%';
        elements.barPension.style.width = pPension.toFixed(1) + '%';
        elements.bar457.style.width = p457.toFixed(1) + '%';
        elements.barOther.style.width = pOther.toFixed(1) + '%';

        elements.pctNet.textContent = pNet.toFixed(1) + '%';
        elements.pctTaxes.textContent = pTax.toFixed(1) + '%';
        elements.pctPension.textContent = pPension.toFixed(1) + '%';
        elements.pct457.textContent = p457.toFixed(1) + '%';
        elements.pctOther.textContent = pOther.toFixed(1) + '%';
      }

      // 10. Update Biweekly Table Line Items
      elements.rowBaseHours.textContent = primaryBaseHours;
      elements.rowHourlyRate.textContent = primaryHourlyRate.toFixed(2);
      elements.rowBasePay.textContent = fmt(primaryBasePay);
      elements.rowOtHours.textContent = primaryOtHours;
      elements.rowOtRate.textContent = primaryOtRate.toFixed(2);
      elements.rowOtPay.textContent = fmt(primaryOtPay);
      elements.rowGrossPay.textContent = fmt(householdGrossBiweekly);

      elements.labelRowPrimaryPension.textContent = `Primary ${primaryPlan.shortName} (${primaryPlan.baseOnly ? 'Base Only' : 'Total'})`;
      elements.rowCalpers.textContent = (primaryPlan.rate > 0) ? `-${fmt(primaryBiweeklyCalpers)}` : '$0.00';
      elements.row457.textContent = `-${fmt(primaryBiweekly457)}`;
      elements.rowPretaxHealth.textContent = `-${fmt(primaryAdditionalPretax)}`;
      elements.rowTotalPretax.textContent = `-${fmt(householdPretaxBiweekly)}`;

      const biweeklyTaxableWage = Math.max(0, householdGrossBiweekly - householdPretaxBiweekly - biweeklyAlimonyPaid);
      elements.rowFedTaxableWage.textContent = fmt(biweeklyTaxableWage);

      elements.rowFedTax.textContent = `-${fmt(biweeklyFedTax)}`;
      elements.rowCaTax.textContent = `-${fmt(biweeklyCaTax)}`;
      elements.rowMedicareTax.textContent = `-${fmt(biweeklyMedicare)}`;
      
      if (householdSsBiweekly > 0) {
        elements.rowSsItem.classList.remove('hidden');
        elements.rowSsTax.textContent = `-${fmt(householdSsBiweekly)}`;
      } else {
        elements.rowSsItem.classList.add('hidden');
      }

      elements.rowTotalTaxes.textContent = `-${fmt(biweeklyCompositeTaxes)}`;
      elements.rowPostTax.textContent = `-${fmt(primaryPostTax)}`;
      elements.rowNetPay.textContent = fmt(biweeklyNetPay);

      // 11. Update Annual Section
      const statusNames = {
        single: 'Single',
        mfj: 'Married Filing Jointly',
        mfs: 'Married Filing Separately',
        hoh: 'Head of Household'
      };
      elements.annFedStatusLabel.textContent = statusNames[filingStatus];
      elements.annCaStatusLabel.textContent = statusNames[filingStatus];

      elements.annGrossWages.textContent = fmtNoCents(householdGrossAnnual);
      elements.annTotalPretax.textContent = `-${fmtNoCents(householdPretaxAnnual)}`;
      elements.annFedAgi.textContent = fmtNoCents(annualFedAgi);
      elements.annFedStdDed.textContent = `-${fmtNoCents(fedDeduction)}`;
      elements.annFedTaxable.textContent = fmtNoCents(annualFedTaxable);
      elements.annFedTaxPreCredit.textContent = fmtNoCents(annualFedTaxPreCredit);
      elements.annFedCredits.textContent = `-${fmtNoCents(childCredits)}`;
      elements.annFedTaxFinal.textContent = fmtNoCents(annualFedTaxFinal);

      elements.annCaAgi.textContent = fmtNoCents(annualCaAgi);
      elements.annCaStdDed.textContent = `-${fmtNoCents(caDeduction)}`;
      elements.annCaTaxable.textContent = fmtNoCents(annualCaTaxable);
      elements.annCaTaxPreCredit.textContent = fmtNoCents(annualCaTaxPreCredit);
      elements.annCaCredits.textContent = `-${fmtNoCents(totalCaCredits)}`;
      elements.annCaTaxFinal.textContent = fmtNoCents(annualCaTaxFinal);
      elements.annCaMarginal.textContent = getMarginalRate(annualCaTaxable, caBrackets);

      elements.annCombinedEffective.textContent = `${annualEffectiveRate.toFixed(1)}%`;
      elements.annSummaryFed.textContent = fmtNoCents(annualFedTaxFinal);
      elements.annSummaryCa.textContent = fmtNoCents(annualCaTaxFinal);
      elements.annSummaryMed.textContent = fmtNoCents(annualMedicare);
      
      let summaryPensionTitle = primaryPlan.shortName;
      if (hasSpouse && spousePlan.rate > 0) {
        summaryPensionTitle = `${primaryPlan.shortName} + ${spousePlan.shortName}`;
      }
      elements.annSummaryPensionTitle.textContent = (householdCalpersAnnual > 0) ? summaryPensionTitle : 'Social Security';
      elements.annSummaryPension.textContent = (householdCalpersAnnual > 0) ? fmtNoCents(householdCalpersAnnual) : fmtNoCents(annualTotalSs);
    }

    // View Switching Logic
    function setViewMode(mode) {
      state.viewMode = mode;
      elements.viewTabBoth.className = (mode === 'both')
        ? 'px-2.5 sm:px-3 py-1 sm:py-1.5 rounded-lg bg-white dark:bg-slate-700 text-slate-900 dark:text-white shadow-xs transition-all font-semibold'
        : 'px-2.5 sm:px-3 py-1 sm:py-1.5 rounded-lg text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white transition-all';
      elements.viewTabBiweekly.className = (mode === 'biweekly')
        ? 'px-2.5 sm:px-3 py-1 sm:py-1.5 rounded-lg bg-white dark:bg-slate-700 text-slate-900 dark:text-white shadow-xs transition-all font-semibold'
        : 'px-2.5 sm:px-3 py-1 sm:py-1.5 rounded-lg text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white transition-all';
      elements.viewTabAnnual.className = (mode === 'annual')
        ? 'px-2.5 sm:px-3 py-1 sm:py-1.5 rounded-lg bg-white dark:bg-slate-700 text-slate-900 dark:text-white shadow-xs transition-all font-semibold'
        : 'px-2.5 sm:px-3 py-1 sm:py-1.5 rounded-lg text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white transition-all';

      if (mode === 'biweekly') {
        elements.biweeklySection.classList.remove('hidden');
        elements.annualSection.classList.add('hidden');
      } else if (mode === 'annual') {
        elements.biweeklySection.classList.add('hidden');
        elements.annualSection.classList.remove('hidden');
      } else {
        elements.biweeklySection.classList.remove('hidden');
        elements.annualSection.classList.remove('hidden');
      }
    }

    // Initialization & Event Wiring
    function init() {
      let savedTheme = null;
      try {
        savedTheme = localStorage.getItem('ca_first_responder_theme');
      } catch (e) {}

      if (savedTheme) {
        setTheme(savedTheme === 'dark');
      } else if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
        setTheme(false);
      } else {
        setTheme(false);
      }

      elements.btnThemeToggle.addEventListener('click', () => {
        setTheme(!state.isDark);
      });

      // Primary Inputs
      const primaryInputs = [
        elements.pensionSystemSelect,
        elements.primaryCustomPensionName,
        elements.primaryCustomPensionRate,
        elements.primaryCustomPensionBaseOnly,
        elements.primaryCustomPensionSocialSecurity,
        elements.hourlyRateInput,
        elements.baseHoursInput,
        elements.otHoursInput,
        elements.input457Value,
        elements.checkAge50CatchUp,
        elements.inputAdditionalPretax,
        elements.inputPostTaxDeductions,
        elements.inputAlimonyPaid,
        elements.selectFilingStatus,
        elements.inputChildren,
        elements.inputItemizedAmount
      ];

      primaryInputs.forEach(input => {
        input.addEventListener('input', calculate);
        input.addEventListener('change', calculate);
      });

      // Spouse Inputs
      const spouseInputs = [
        elements.spousePensionSystemSelect,
        elements.spouseCustomPensionName,
        elements.spouseCustomPensionRate,
        elements.spouseCustomPensionBaseOnly,
        elements.spouseCustomPensionSocialSecurity,
        elements.spouseHourlyRateInput,
        elements.spouseBaseHoursInput,
        elements.spouseOtHoursInput,
        elements.spouseInput457Value,
        elements.spouseCheckAge50CatchUp,
        elements.spouseInputAdditionalPretax,
        elements.spouseInputPostTaxDeductions
      ];

      spouseInputs.forEach(input => {
        input.addEventListener('input', calculate);
        input.addEventListener('change', calculate);
      });

      // Spouse Checkbox Toggle Handler
      elements.checkIncludeSpouse.addEventListener('change', (e) => {
        if (e.target.checked) {
          if (elements.selectFilingStatus.value === 'single') {
            elements.selectFilingStatus.value = 'mfj';
          }
        } else {
          if (elements.selectFilingStatus.value === 'mfj') {
            elements.selectFilingStatus.value = 'single';
          }
        }
        calculate();
      });

      // Primary Sliders
      elements.hourlyRateSlider.addEventListener('input', (e) => {
        elements.hourlyRateInput.value = e.target.value;
        calculate();
      });

      elements.otHoursSlider.addEventListener('input', (e) => {
        elements.otHoursInput.value = e.target.value;
        calculate();
      });

      // Spouse Sliders
      elements.spouseHourlyRateSlider.addEventListener('input', (e) => {
        elements.spouseHourlyRateInput.value = e.target.value;
        calculate();
      });

      elements.spouseOtHoursSlider.addEventListener('input', (e) => {
        elements.spouseOtHoursInput.value = e.target.value;
        calculate();
      });

      // Primary 457 Toggle
      elements.toggle457Percent.addEventListener('click', () => {
        state.isPercent457 = true;
        elements.toggle457Percent.className = 'px-2.5 py-1 rounded-md bg-white dark:bg-slate-700 text-slate-900 dark:text-white font-semibold shadow-2xs';
        elements.toggle457Dollar.className = 'px-2.5 py-1 rounded-md text-slate-500 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white';
        elements.unit457Prefix.classList.add('hidden');
        elements.unit457Suffix.classList.remove('hidden');
        elements.input457Value.value = '8';
        calculate();
      });

      elements.toggle457Dollar.addEventListener('click', () => {
        state.isPercent457 = false;
        elements.toggle457Dollar.className = 'px-2.5 py-1 rounded-md bg-white dark:bg-slate-700 text-slate-900 dark:text-white font-semibold shadow-2xs';
        elements.toggle457Percent.className = 'px-2.5 py-1 rounded-md text-slate-500 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white';
        elements.unit457Prefix.classList.remove('hidden');
        elements.unit457Suffix.classList.add('hidden');
        elements.input457Value.value = '500';
        calculate();
      });

      // Spouse 457 Toggle
      elements.spouseToggle457Percent.addEventListener('click', () => {
        state.spouseIsPercent457 = true;
        elements.spouseToggle457Percent.className = 'px-2 py-0.5 rounded-md bg-white dark:bg-slate-700 text-slate-900 dark:text-white font-semibold shadow-2xs';
        elements.spouseToggle457Dollar.className = 'px-2 py-0.5 rounded-md text-slate-500 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white';
        elements.spouseUnit457Prefix.classList.add('hidden');
        elements.spouseUnit457Suffix.classList.remove('hidden');
        elements.spouseInput457Value.value = '6';
        calculate();
      });

      elements.spouseToggle457Dollar.addEventListener('click', () => {
        state.spouseIsPercent457 = false;
        elements.spouseToggle457Dollar.className = 'px-2 py-0.5 rounded-md bg-white dark:bg-slate-700 text-slate-900 dark:text-white font-semibold shadow-2xs';
        elements.spouseToggle457Percent.className = 'px-2 py-0.5 rounded-md text-slate-500 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white';
        elements.spouseUnit457Prefix.classList.remove('hidden');
        elements.spouseUnit457Suffix.classList.add('hidden');
        elements.spouseInput457Value.value = '300';
        calculate();
      });

      // Standard vs Itemized Deduction Toggles
      elements.btnDeductionStandard.addEventListener('click', () => {
        state.useItemized = false;
        elements.btnDeductionStandard.className = 'px-2.5 py-1 rounded-md bg-white dark:bg-slate-700 text-slate-900 dark:text-white font-semibold shadow-2xs';
        elements.btnDeductionItemized.className = 'px-2.5 py-1 rounded-md text-slate-500 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white';
        elements.itemizedContainer.classList.add('hidden');
        calculate();
      });

      elements.btnDeductionItemized.addEventListener('click', () => {
        state.useItemized = true;
        elements.btnDeductionItemized.className = 'px-2.5 py-1 rounded-md bg-white dark:bg-slate-700 text-slate-900 dark:text-white font-semibold shadow-2xs';
        elements.btnDeductionStandard.className = 'px-2.5 py-1 rounded-md text-slate-500 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white';
        elements.itemizedContainer.classList.remove('hidden');
        calculate();
      });

      // View Switcher Buttons
      elements.viewTabBoth.addEventListener('click', () => setViewMode('both'));
      elements.viewTabBiweekly.addEventListener('click', () => setViewMode('biweekly'));
      elements.viewTabAnnual.addEventListener('click', () => setViewMode('annual'));

      // Quick Role Presets
      elements.presetPolice.addEventListener('click', () => {
        elements.pensionSystemSelect.value = 'lafpp_tier6';
        elements.hourlyRateInput.value = '65.00';
        elements.hourlyRateSlider.value = '65.00';
        elements.baseHoursInput.value = '80';
        elements.otHoursInput.value = '14';
        elements.otHoursSlider.value = '14';
        elements.inputAdditionalPretax.value = '150';
        elements.inputPostTaxDeductions.value = '85';
        elements.inputAlimonyPaid.value = '0';
        elements.checkIncludeSpouse.checked = false;
        elements.selectFilingStatus.value = 'single';
        elements.inputChildren.value = '1';
        calculate();
      });

      elements.presetFire.addEventListener('click', () => {
        elements.pensionSystemSelect.value = 'ocers_plan_v';
        elements.hourlyRateInput.value = '52.00';
        elements.hourlyRateSlider.value = '52.00';
        elements.baseHoursInput.value = '112';
        elements.otHoursInput.value = '24';
        elements.otHoursSlider.value = '24';
        elements.inputAdditionalPretax.value = '180';
        elements.inputPostTaxDeductions.value = '110';
        elements.inputAlimonyPaid.value = '0';
        elements.checkIncludeSpouse.checked = true;
        elements.spousePensionSystemSelect.value = 'ocers_plan_u';
        elements.selectFilingStatus.value = 'mfj';
        elements.inputChildren.value = '2';
        calculate();
      });

      elements.presetParamedic.addEventListener('click', () => {
        elements.pensionSystemSelect.value = 'lacera_plan_c';
        elements.hourlyRateInput.value = '48.00';
        elements.hourlyRateSlider.value = '48.00';
        elements.baseHoursInput.value = '84';
        elements.otHoursInput.value = '14';
        elements.otHoursSlider.value = '14';
        elements.inputAdditionalPretax.value = '120';
        elements.inputPostTaxDeductions.value = '65';
        elements.inputAlimonyPaid.value = '0';
        elements.checkIncludeSpouse.checked = false;
        elements.selectFilingStatus.value = 'single';
        elements.inputChildren.value = '0';
        calculate();
      });

      elements.btnPrint.addEventListener('click', () => {
        window.print();
      });

      elements.btnReset.addEventListener('click', () => {
        elements.pensionSystemSelect.value = 'calpers';
        elements.hourlyRateInput.value = '62.50';
        elements.hourlyRateSlider.value = '62.50';
        elements.baseHoursInput.value = '80';
        elements.otHoursInput.value = '12';
        elements.otHoursSlider.value = '12';
        elements.inputAdditionalPretax.value = '150';
        elements.inputPostTaxDeductions.value = '85';
        elements.inputAlimonyPaid.value = '0';
        elements.checkIncludeSpouse.checked = false;
        elements.spousePensionSystemSelect.value = 'calpers';
        elements.selectFilingStatus.value = 'single';
        elements.inputChildren.value = '1';
        elements.checkAge50CatchUp.checked = false;
        elements.spouseCheckAge50CatchUp.checked = false;
        state.isPercent457 = true;
        state.spouseIsPercent457 = true;
        elements.toggle457Percent.click();
        elements.spouseToggle457Percent.click();
        state.useItemized = false;
        elements.btnDeductionStandard.click();
        setViewMode('both');
        calculate();
      });

      calculate();
    }

    if (document.readyState === 'loading') {
      document.addEventListener('DOMContentLoaded', init);
    } else {
      init();
    }
  </script>
</body>
</html>
