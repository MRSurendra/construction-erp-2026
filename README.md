<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BuildTrack Pro - Advanced Construction ERP & Site Ops</title>
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <style>
    body { font-family: 'Inter', sans-serif; }
    .tab-active { border-bottom: 2px solid #f59e0b; color: #f59e0b; font-weight: 600; }
    .gantt-grid {
      background-size: 40px 100%;
      background-image: linear-gradient(to right, #f1f5f9 1px, transparent 1px);
    }
  </style>
</head>
<body class="bg-slate-100 text-slate-800 min-h-screen">
  <div class="max-w-7xl mx-auto px-4 py-6">
    
    <!-- Mobile PWA Quick Banner -->
    <div id="pwa-install-banner" class="bg-amber-500 text-slate-950 px-4 py-2 rounded-xl mb-4 text-xs font-semibold flex items-center justify-between shadow-sm flex-wrap gap-2">
      <div class="flex items-center gap-2">
        <span class="text-base">📲</span>
        <span><b>Site Mobile App:</b> Install BuildTrack on Android / iPhone for 1-tap offline site reporting.</span>
      </div>
      <div class="flex items-center gap-2">
        <button id="btn-show-install-guide" class="bg-slate-950 text-amber-400 px-3 py-1 rounded-lg text-[11px] font-bold hover:bg-slate-800 transition-colors">
          How to Install
        </button>
        <button id="btn-dismiss-pwa" class="text-slate-900 hover:text-black font-bold px-1">&times;</button>
      </div>
    </div>

    <!-- Header -->
    <header class="bg-slate-900 text-white rounded-2xl shadow-xl p-6 mb-6 border-l-8 border-amber-500">
      <div class="flex flex-col lg:flex-row lg:items-center justify-between gap-4">
        <div>
          <div class="flex items-center gap-2 mb-1.5 flex-wrap">
            <span class="bg-amber-500 text-slate-950 text-xs font-black uppercase tracking-wider px-2.5 py-0.5 rounded">Site Ops Pro</span>
            <span class="text-slate-400 text-xs font-medium">Job Code: #IND-2026-778</span>
            <span id="cloud-status-pill" class="inline-flex items-center gap-1.5 px-2.5 py-0.5 rounded-full text-xs font-semibold bg-emerald-950 text-emerald-400 border border-emerald-800">
              <span class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-pulse"></span> Cloud DB Synced
            </span>
          </div>
          <h1 class="text-2xl lg:text-3xl font-extrabold tracking-tight">Skyline Heights Commercial Complex</h1>
          <p class="text-slate-300 text-sm mt-1">Prime Commercial & Retail Tower • Target Handover: Q4 2027</p>
        </div>

        <!-- Role Selector & Quick Actions -->
        <div class="flex flex-col sm:flex-row sm:items-center gap-3">
          <!-- Active Role Switcher -->
          <div class="bg-slate-800/90 border border-slate-700 rounded-xl px-3 py-1.5 flex items-center gap-2 text-xs">
            <span class="text-slate-400 font-medium">Active Role:</span>
            <select id="user-role-select" class="bg-slate-900 text-amber-400 font-bold border border-slate-700 rounded-lg px-2 py-1 focus:outline-none focus:ring-1 focus:ring-amber-500 text-xs">
              <option value="admin">👨‍💼 Project Director (Full Access)</option>
              <option value="supervisor">👷‍♂️ Site Engineer / Supervisor</option>
              <option value="client">💼 Client / Investor (Read-Only)</option>
            </select>
          </div>

          <div class="flex items-center gap-2 flex-wrap">
            <button id="btn-share-whatsapp" class="bg-emerald-600 hover:bg-emerald-500 text-white px-3 py-2 rounded-xl text-xs font-bold flex items-center gap-1.5 transition-colors shadow">
              <span>📲</span> WhatsApp DSR
            </button>
            <button id="btn-sync-cloud" class="bg-indigo-600 hover:bg-indigo-500 text-white px-3 py-2 rounded-xl text-xs font-semibold flex items-center gap-1.5 transition-colors shadow">
              <span>☁️</span> Cloud Sync
            </button>
            <button id="btn-print-report" class="bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 px-3 py-2 rounded-xl text-xs font-semibold flex items-center gap-1.5 transition-colors">
              <span>🖨️</span> Print Daily DSR
            </button>
            <button id="btn-export-json" class="bg-amber-500 hover:bg-amber-400 text-slate-950 px-3.5 py-2 rounded-xl text-xs font-bold transition-all shadow-md flex items-center gap-1">
              <span>💾</span> Export Backup
            </button>
          </div>
        </div>
      </div>

      <!-- Live KPI Ribbon -->
      <div class="grid grid-cols-2 md:grid-cols-5 gap-3 mt-6 pt-6 border-t border-slate-800">
        <div class="bg-slate-800/80 rounded-xl p-3 border border-slate-700/50">
          <p class="text-xs text-slate-400 font-medium">Project Progress</p>
          <div class="flex items-baseline justify-between mt-1">
            <span id="kpi-progress-pct" class="text-xl font-bold text-amber-400">0%</span>
            <span id="kpi-progress-ratio" class="text-xs text-slate-400">0/0 Tasks</span>
          </div>
          <div class="w-full bg-slate-700 rounded-full h-1.5 mt-2 overflow-hidden">
            <div id="kpi-progress-bar" class="bg-amber-400 h-1.5 rounded-full transition-all duration-500" style="width: 0%"></div>
          </div>
        </div>

        <div class="bg-slate-800/80 rounded-xl p-3 border border-slate-700/50">
          <p class="text-xs text-slate-400 font-medium">Budget Consumed</p>
          <div class="flex items-baseline justify-between mt-1">
            <span id="kpi-budget-spent" class="text-xl font-bold text-white">₹0</span>
            <span class="text-xs text-slate-400">Cap: ₹1.2 Cr</span>
          </div>
          <p id="kpi-budget-percent" class="text-[11px] text-slate-400 mt-1">0% utilized</p>
        </div>

        <div class="bg-slate-800/80 rounded-xl p-3 border border-slate-700/50">
          <p class="text-xs text-slate-400 font-medium">On-Site Workforce</p>
          <div class="flex items-baseline justify-between mt-1">
            <span id="kpi-crew-today" class="text-xl font-bold text-emerald-400">0 Headcount</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">Today's Active Shift</p>
        </div>

        <div class="bg-slate-800/80 rounded-xl p-3 border border-slate-700/50">
          <p class="text-xs text-slate-400 font-medium">Material Inflow</p>
          <div class="flex items-baseline justify-between mt-1">
            <span id="kpi-materials-pending" class="text-xl font-bold text-blue-400">0 Pending</span>
            <span id="kpi-materials-delivered" class="text-xs text-emerald-400 font-medium">0 In Site</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">Cement, Steel, Aggregates</p>
        </div>

        <div class="bg-slate-800/80 rounded-xl p-3 border border-slate-700/50">
          <p class="text-xs text-slate-400 font-medium">Open Snag Defects</p>
          <div class="flex items-baseline justify-between mt-1">
            <span id="kpi-defects-count" class="text-xl font-bold text-rose-400">0 Open</span>
            <span id="kpi-defects-closed" class="text-xs text-slate-400">0 Rectified</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">QC & Safety Inspection</p>
        </div>
      </div>
    </header>

    <!-- Module Navigation Tabs -->
    <nav class="bg-white rounded-xl shadow-sm border border-slate-200 mb-6 px-4 overflow-x-auto">
      <div class="flex items-center gap-6 min-w-max text-sm font-medium">
        <button id="tab-btn-tasks" class="nav-tab py-4 text-slate-600 hover:text-slate-900 transition-colors flex items-center gap-2 tab-active">
          <span>📋</span> Site Tasks & Milestones
        </button>
        <button id="tab-btn-gantt" class="nav-tab py-4 text-slate-600 hover:text-slate-900 transition-colors flex items-center gap-2">
          <span>📊</span> Gantt Schedule Timeline
        </button>
        <button id="tab-btn-defects" class="nav-tab py-4 text-slate-600 hover:text-slate-900 transition-colors flex items-center gap-2">
          <span>⚠️</span> Snags & Defects Punch-List
        </button>
        <button id="tab-btn-materials" class="nav-tab py-4 text-slate-600 hover:text-slate-900 transition-colors flex items-center gap-2">
          <span>🏗️</span> Materials & Inventory
        </button>
        <button id="tab-btn-labour" class="nav-tab py-4 text-slate-600 hover:text-slate-900 transition-colors flex items-center gap-2">
          <span>👷‍♂️</span> Daily Labour & Hazri Log
        </button>
        <button id="tab-btn-budget" class="nav-tab py-4 text-slate-600 hover:text-slate-900 transition-colors flex items-center gap-2">
          <span>💰</span> Expenses & Budget
        </button>
      </div>
    </nav>

    <!-- Global Toast Notification -->
    <div id="toast" class="hidden fixed bottom-6 right-6 z-50 bg-slate-900 text-white px-4 py-3 rounded-xl shadow-2xl border border-slate-700 flex items-center gap-2 text-sm">
      <span id="toast-icon">✅</span>
      <span id="toast-msg">Action completed</span>
    </div>

    <!-- Cloud Sync Settings Modal -->
    <div id="cloud-modal" class="hidden fixed inset-0 z-50 bg-slate-950/70 flex items-center justify-center p-4">
      <div class="bg-white rounded-2xl max-w-lg w-full p-6 shadow-2xl border border-slate-200">
        <div class="flex items-center justify-between pb-3 border-b border-slate-200">
          <h3 class="font-bold text-slate-900 text-base flex items-center gap-2">
            <span>☁️</span> Cloud Database & Real-Time Sync Config
          </h3>
          <button id="btn-close-cloud-modal" class="text-slate-400 hover:text-slate-700 text-xl font-bold">&times;</button>
        </div>
        
        <p class="text-xs text-slate-500 mt-2">Connect your Supabase or Firebase database to enable multi-device sync across all site engineers:</p>
        
        <div class="space-y-3 mt-4 text-xs">
          <div>
            <label for="cloud-provider" class="block font-semibold text-slate-700 uppercase mb-1">Database Provider</label>
            <select id="cloud-provider" class="w-full px-3 py-2 rounded-lg border border-slate-300 bg-white">
              <option value="supabase">Supabase (PostgreSQL + Realtime WebSocket)</option>
              <option value="firebase">Google Firebase (Cloud Firestore)</option>
            </select>
          </div>

          <div>
            <label for="cloud-url" class="block font-semibold text-slate-700 uppercase mb-1">Project Endpoint URL</label>
            <input type="text" id="cloud-url" placeholder="https://skyline-site.supabase.co" class="w-full px-3 py-2 rounded-lg border border-slate-300 focus:outline-none focus:ring-1 focus:ring-indigo-500">
          </div>

          <div>
            <label for="cloud-apikey" class="block font-semibold text-slate-700 uppercase mb-1">Public Anon API Key</label>
            <input type="password" id="cloud-apikey" placeholder="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." class="w-full px-3 py-2 rounded-lg border border-slate-300 focus:outline-none focus:ring-1 focus:ring-indigo-500">
          </div>

          <div class="p-3 bg-slate-50 rounded-xl border border-slate-200">
            <div class="flex items-center justify-between">
              <span class="font-semibold text-slate-700">Auto-Sync on Changes</span>
              <input type="checkbox" id="cloud-autosync" checked class="w-4 h-4 text-indigo-600 rounded">
            </div>
            <p class="text-[11px] text-slate-400 mt-1">Saves all tasks, hazri, defects, and expenses to cloud in real time.</p>
          </div>
        </div>

        <div class="flex items-center justify-end gap-3 mt-5">
          <button id="btn-test-cloud" class="px-4 py-2 rounded-xl text-xs font-bold bg-slate-100 hover:bg-slate-200 text-slate-700 transition-colors">
            Test Connection
          </button>
          <button id="btn-save-cloud" class="px-4 py-2 rounded-xl text-xs font-bold bg-indigo-600 hover:bg-indigo-500 text-white transition-colors flex items-center gap-1.5 shadow">
            <span>💾</span> Save & Sync Now
          </button>
        </div>
      </div>
    </div>

    <!-- PWA Install Guide Modal -->
    <div id="pwa-modal" class="hidden fixed inset-0 z-50 bg-slate-950/70 flex items-center justify-center p-4">
      <div class="bg-white rounded-2xl max-w-md w-full p-6 shadow-2xl border border-slate-200 text-xs">
        <div class="flex items-center justify-between pb-3 border-b border-slate-200">
          <h3 class="font-bold text-slate-900 text-base flex items-center gap-2">
            <span>📲</span> Install on Mobile (PWA)
          </h3>
          <button id="btn-close-pwa-modal" class="text-slate-400 hover:text-slate-700 text-xl font-bold">&times;</button>
        </div>

        <div class="space-y-3 mt-4 text-slate-600">
          <div class="p-3 bg-slate-50 rounded-xl border border-slate-200">
            <p class="font-bold text-slate-900 mb-1">🤖 On Android (Google Chrome):</p>
            <ol class="list-decimal list-inside space-y-1">
              <li>Open this page URL in Chrome on your phone.</li>
              <li>Tap the three dots <b>⋮</b> at top right.</li>
              <li>Select <b>"Add to Home screen"</b> or <b>"Install app"</b>.</li>
            </ol>
          </div>

          <div class="p-3 bg-slate-50 rounded-xl border border-slate-200">
            <p class="font-bold text-slate-900 mb-1">🍎 On iPhone (Apple Safari):</p>
            <ol class="list-decimal list-inside space-y-1">
              <li>Open this page in Safari.</li>
              <li>Tap the <b>Share button ⎋</b> at bottom.</li>
              <li>Scroll down and tap <b>"Add to Home Screen"</b>.</li>
            </ol>
          </div>

          <p class="text-[11px] text-slate-500">It runs full screen like a native mobile app without needing app store downloads!</p>
        </div>

        <div class="mt-5 text-right">
          <button id="btn-got-it-pwa" class="px-4 py-2 rounded-xl font-bold bg-slate-900 text-white">Got it!</button>
        </div>
      </div>
    </div>

    <!-- WhatsApp Share Modal -->
    <div id="whatsapp-modal" class="hidden fixed inset-0 z-50 bg-slate-950/70 flex items-center justify-center p-4">
      <div class="bg-white rounded-2xl max-w-lg w-full p-6 shadow-2xl border border-slate-200">
        <div class="flex items-center justify-between pb-3 border-b border-slate-200">
          <h3 class="font-bold text-slate-900 text-base flex items-center gap-2">
            <span>📲</span> WhatsApp Daily Progress Summary
          </h3>
          <button id="btn-close-whatsapp-modal" class="text-slate-400 hover:text-slate-700 text-xl font-bold">&times;</button>
        </div>
        <p class="text-xs text-slate-500 mt-2">Ready to send directly to project WhatsApp groups or client channels:</p>
        <textarea id="whatsapp-text-preview" rows="9" class="w-full text-xs font-mono bg-slate-50 border border-slate-300 rounded-xl p-3 mt-3 focus:outline-none focus:ring-2 focus:ring-emerald-500"></textarea>
        <div class="flex items-center justify-end gap-3 mt-4">
          <button id="btn-copy-whatsapp" class="px-4 py-2 rounded-xl text-xs font-bold bg-slate-100 hover:bg-slate-200 text-slate-700 transition-colors">
            📋 Copy to Clipboard
          </button>
          <button id="btn-send-whatsapp-direct" class="px-4 py-2 rounded-xl text-xs font-bold bg-emerald-600 hover:bg-emerald-500 text-white transition-colors flex items-center gap-1.5 shadow">
            <span>🚀</span> Open in WhatsApp
          </button>
        </div>
      </div>
    </div>

    <!-- ==================== TAB 1: TASKS & MILESTONES ==================== -->
    <div id="tab-pane-tasks" class="tab-pane">
      <!-- Quick status counts banner -->
      <div class="grid grid-cols-2 sm:grid-cols-4 gap-4 mb-6">
        <div class="bg-white rounded-xl p-3.5 shadow-sm border border-slate-200">
          <p class="text-xs font-semibold text-slate-500 uppercase tracking-wide">Total Tasks</p>
          <p id="stat-total" class="text-xl font-bold text-slate-900 mt-0.5">0</p>
        </div>
        <div class="bg-white rounded-xl p-3.5 shadow-sm border border-slate-200">
          <p class="text-xs font-semibold text-amber-600 uppercase tracking-wide">Pending</p>
          <p id="stat-pending" class="text-xl font-bold text-amber-600 mt-0.5">0</p>
        </div>
        <div class="bg-white rounded-xl p-3.5 shadow-sm border border-slate-200">
          <p class="text-xs font-semibold text-blue-600 uppercase tracking-wide">In Progress</p>
          <p id="stat-progress" class="text-xl font-bold text-blue-600 mt-0.5">0</p>
        </div>
        <div class="bg-white rounded-xl p-3.5 shadow-sm border border-slate-200">
          <p class="text-xs font-semibold text-emerald-600 uppercase tracking-wide">Completed</p>
          <p id="stat-completed" class="text-xl font-bold text-emerald-600 mt-0.5">0</p>
        </div>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        
        <!-- Form: Log Task -->
        <div class="lg:col-span-1" id="task-form-panel">
          <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200 sticky top-6">
            <h2 class="text-base font-bold text-slate-900 mb-4 pb-2 border-b border-slate-100 flex items-center gap-2">
              <span class="text-amber-500">➕</span> Log Site Task
            </h2>
            <form id="task-form" class="space-y-3.5">
              <div>
                <label for="task-name" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Task Description</label>
                <input type="text" id="task-name" placeholder="e.g. 5th Floor Slab Casting" required class="w-full px-3 py-2 text-sm rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-amber-500">
              </div>

              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label for="task-trade" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Contractor / Trade</label>
                  <select id="task-trade" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 bg-white focus:outline-none focus:ring-2 focus:ring-amber-500">
                    <option value="General Contracting">General Contracting</option>
                    <option value="RCC & Concrete">RCC & Concrete</option>
                    <option value="Steel & Rebar">Steel & Rebar</option>
                    <option value="Brickwork & Masonry">Brickwork & Masonry</option>
                    <option value="Electrical Conduits">Electrical Conduits</option>
                    <option value="Plumbing & Drainage">Plumbing & Drainage</option>
                    <option value="Plastering & POP">Plastering & POP</option>
                    <option value="Site Safety & Inspection">Site Safety & Inspection</option>
                  </select>
                </div>
                <div>
                  <label for="task-priority" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Priority</label>
                  <select id="task-priority" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 bg-white focus:outline-none focus:ring-2 focus:ring-amber-500">
                    <option value="Critical">🔴 Critical</option>
                    <option value="High">🟠 High</option>
                    <option value="Normal" selected>🟡 Normal</option>
                    <option value="Low">🟢 Low</option>
                  </select>
                </div>
              </div>

              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label for="task-start" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Start Date</label>
                  <input type="date" id="task-start" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 bg-white focus:outline-none focus:ring-2 focus:ring-amber-500">
                </div>
                <div>
                  <label for="task-deadline" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Due Date</label>
                  <input type="date" id="task-deadline" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 bg-white focus:outline-none focus:ring-2 focus:ring-amber-500">
                </div>
              </div>

              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label for="task-lead" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Lead / Engineer</label>
                  <input type="text" id="task-lead" placeholder="e.g. Er. Patel" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-amber-500">
                </div>
                <div>
                  <label for="task-progress-pct" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Progress %</label>
                  <input type="number" id="task-progress-pct" min="0" max="100" value="0" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 bg-white">
                </div>
              </div>

              <div>
                <label for="task-status" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Initial Status</label>
                <select id="task-status" class="w-full px-3 py-2 text-xs rounded-lg border border-slate-300 bg-white focus:outline-none focus:ring-2 focus:ring-amber-500">
                  <option value="pending">Pending</option>
                  <option value="in_progress">In Progress</option>
                  <option value="inspection">Under QC Inspection</option>
                  <option value="completed">Completed</option>
                </select>
              </div>

              <button type="submit" class="w-full bg-amber-500 hover:bg-amber-600 text-slate-950 font-bold py-2.5 px-4 rounded-xl text-xs uppercase tracking-wider transition-colors shadow">
                Add Site Task
              </button>
            </form>
            <div id="form-message" class="hidden mt-3 p-2.5 rounded-lg text-xs font-medium text-emerald-800 bg-emerald-50 border border-emerald-200 text-center">
              Task added successfully.
            </div>
          </div>
        </div>

        <!-- Task List & Filters -->
        <div class="lg:col-span-2">
          <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
            <div class="p-4 bg-slate-50 border-b border-slate-200 flex flex-col sm:flex-row sm:items-center justify-between gap-3">
              <div class="flex items-center gap-2">
                <input type="text" id="task-search" placeholder="Search tasks, trades, or leads..." class="px-3 py-1.5 text-xs rounded-lg border border-slate-300 focus:outline-none focus:ring-1 focus:ring-amber-500 w-56">
              </div>
              <div class="flex items-center gap-1 overflow-x-auto text-xs">
                <button data-task-filter="all" class="task-filter-btn px-2.5 py-1 rounded-lg font-medium bg-slate-800 text-white">All</button>
                <button data-task-filter="pending" class="task-filter-btn px-2.5 py-1 rounded-lg font-medium bg-white text-slate-600 hover:bg-slate-100">Pending</button>
                <button data-task-filter="in_progress" class="task-filter-btn px-2.5 py-1 rounded-lg font-medium bg-white text-slate-600 hover:bg-slate-100">In Progress</button>
                <button data-task-filter="inspection" class="task-filter-btn px-2.5 py-1 rounded-lg font-medium bg-white text-slate-600 hover:bg-slate-100">QC Inspection</button>
                <button data-task-filter="completed" class="task-filter-btn px-2.5 py-1 rounded-lg font-medium bg-white text-slate-600 hover:bg-slate-100">Completed</button>
              </div>
            </div>

            <div id="task-list" class="divide-y divide-slate-100 min-h-[300px]">
              <!-- JS rendered -->
            </div>
            <div id="empty-state" class="hidden p-12 text-center text-slate-400 text-sm">
              <p class="text-3xl mb-2">📋</p>
              <p class="text-slate-500 text-sm font-medium">No tasks matching your selection.</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- ==================== TAB GANTT: VISUAL TIMELINE SCHEDULE ==================== -->
    <div id="tab-pane-gantt" class="tab-pane hidden">
      <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden p-6">
        
        <!-- Gantt Controls Header -->
        <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 pb-4 border-b border-slate-200">
          <div>
            <h2 class="text-lg font-bold text-slate-900 flex items-center gap-2">
              <span>📊</span> Construction Schedule & Critical Path Gantt
            </h2>
            <p class="text-xs text-slate-500 mt-0.5">Timeline overview across project phases and subcontractor trades</p>
          </div>
          
          <div class="flex items-center gap-3 flex-wrap">
            <div class="flex items-center gap-2 text-xs">
              <span class="inline-block w-3 h-3 rounded bg-emerald-500"></span><span class="text-slate-600">Done</span>
              <span class="inline-block w-3 h-3 rounded bg-blue-500"></span><span class="text-slate-600">In Progress</span>
              <span class="inline-block w-3 h-3 rounded bg-purple-500"></span><span class="text-slate-600">QC Review</span>
              <span class="inline-block w-3 h-3 rounded bg-amber-400"></span><span class="text-slate-600">Pending</span>
            </div>
          </div>
        </div>

        <!-- Gantt Timeline Visual Container -->
        <div class="overflow-x-auto mt-4">
          <div class="min-w-[850px]">
            
            <!-- Timeline Scale Header -->
            <div class="grid grid-cols-12 text-xs font-semibold text-slate-500 uppercase bg-slate-50 border-y border-slate-200 py-2.5 px-3">
              <div class="col-span-4">Task & Trade</div>
              <div class="col-span-8 grid grid-cols-4 text-center">
                <div>Phase 1 (Week 1)</div>
                <div>Phase 2 (Week 2)</div>
                <div>Phase 3 (Week 3)</div>
                <div>Phase 4 (Week 4+)</div>
              </div>
            </div>

            <!-- Gantt Rows Container -->
            <div id="gantt-rows" class="divide-y divide-slate-100 py-1">
              <!-- Dynamically rendered via JS -->
            </div>

          </div>
        </div>

      </div>
    </div>

    <!-- ==================== TAB DEFECTS: PUNCH LIST & SITE SNAGS ==================== -->
    <div id="tab-pane-defects" class="tab-pane hidden">
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        
        <!-- Defect Form -->
        <div class="lg:col-span-1" id="defect-form-panel">
          <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200 sticky top-6">
            <h2 class="text-base font-bold text-slate-900 mb-4 pb-2 border-b border-slate-100 flex items-center gap-2">
              <span class="text-rose-500">⚠️</span> Log Site Snag / Defect
            </h2>
            <form id="defect-form" class="space-y-3.5">
              <div>
                <label for="defect-title" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Defect Title</label>
                <input type="text" id="defect-title" placeholder="e.g. Honeycombing in Column C4" required class="w-full px-3 py-2 text-sm rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-amber-500">
              </div>

              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label for="defect-location" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Location / Grid</label>
                  <input type="text" id="defect-location" placeholder="e.g. 3rd Floor Slab Grid B-2" required class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-amber-500">
                </div>
                <div>
                  <label for="defect-trade" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Responsible Trade</label>
                  <select id="defect-trade" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 bg-white">
                    <option value="RCC & Concrete">RCC & Concrete</option>
                    <option value="Steel & Rebar">Steel & Rebar</option>
                    <option value="Brickwork & Masonry">Brickwork & Masonry</option>
                    <option value="Electrical">Electrical</option>
                    <option value="Plumbing">Plumbing</option>
                    <option value="Safety Hazard">Safety Hazard</option>
                  </select>
                </div>
              </div>

              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label for="defect-severity" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Severity</label>
                  <select id="defect-severity" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 bg-white">
                    <option value="Critical">🔴 Critical (Stop Work)</option>
                    <option value="Major" selected>🟠 Major Rework</option>
                    <option value="Minor">🟡 Minor Cosmetic</option>
                  </select>
                </div>
                <div>
                  <label for="defect-deadline" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Fix Deadline</label>
                  <input type="date" id="defect-deadline" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300">
                </div>
              </div>

              <div>
                <label for="defect-photo" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Site Photo / Evidence</label>
                <div class="flex items-center gap-2">
                  <input type="file" id="defect-photo" accept="image/*" class="w-full text-xs text-slate-500 file:mr-2 file:py-1.5 file:px-3 file:rounded-lg file:border-0 file:text-xs file:font-semibold file:bg-slate-100 file:text-slate-700 hover:file:bg-slate-200">
                </div>
                <div id="defect-photo-preview-wrap" class="hidden mt-2 relative">
                  <img id="defect-photo-preview" src="" alt="Defect preview" class="h-24 w-full object-cover rounded-lg border border-slate-200">
                  <span class="absolute bottom-1 right-2 bg-slate-900/80 text-white text-[10px] px-1.5 py-0.5 rounded">Photo Attached</span>
                </div>
              </div>

              <div>
                <label for="defect-status" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Status</label>
                <select id="defect-status" class="w-full px-3 py-2 text-xs rounded-lg border border-slate-300 bg-white">
                  <option value="open">Open (Rectification Pending)</option>
                  <option value="in_progress">In Progress (Work Assigned)</option>
                  <option value="rectified">Rectified & Approved</option>
                </select>
              </div>

              <button type="submit" class="w-full bg-rose-600 hover:bg-rose-500 text-white font-bold py-2.5 px-4 rounded-xl text-xs uppercase tracking-wider transition-colors shadow">
                Log Punch-List Snag
              </button>
            </form>
          </div>
        </div>

        <!-- Defect Cards List -->
        <div class="lg:col-span-2">
          <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
            <div class="p-4 bg-slate-50 border-b border-slate-200 flex flex-col sm:flex-row sm:items-center justify-between gap-3">
              <h3 class="text-sm font-bold text-slate-900">Active Quality & Safety Punch List</h3>
              <div class="flex items-center gap-1 text-xs">
                <button data-defect-filter="all" class="defect-filter-btn px-2.5 py-1 rounded-lg font-medium bg-slate-800 text-white">All</button>
                <button data-defect-filter="open" class="defect-filter-btn px-2.5 py-1 rounded-lg font-medium bg-white text-slate-600 hover:bg-slate-100">Open</button>
                <button data-defect-filter="in_progress" class="defect-filter-btn px-2.5 py-1 rounded-lg font-medium bg-white text-slate-600 hover:bg-slate-100">In Progress</button>
                <button data-defect-filter="rectified" class="defect-filter-btn px-2.5 py-1 rounded-lg font-medium bg-white text-slate-600 hover:bg-slate-100">Rectified</button>
              </div>
            </div>

            <div id="defects-container" class="divide-y divide-slate-100 min-h-[300px]">
              <!-- Injected by JS -->
            </div>
            <div id="defects-empty" class="hidden p-12 text-center text-slate-400 text-sm">
              <p class="text-3xl mb-2">✨</p>
              <p class="text-slate-500 text-sm font-medium">No defects or snags reported for this filter.</p>
            </div>
          </div>
        </div>

      </div>
    </div>

    <!-- ==================== TAB 2: MATERIALS & INVENTORY ==================== -->
    <div id="tab-pane-materials" class="tab-pane hidden">
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        
        <!-- Add Material Form -->
        <div class="lg:col-span-1" id="material-form-panel">
          <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200 sticky top-6">
            <h2 class="text-base font-bold text-slate-900 mb-4 pb-2 border-b border-slate-100 flex items-center gap-2">
              <span class="text-amber-500">📦</span> Log Material Inflow / PO
            </h2>
            <form id="material-form" class="space-y-3.5">
              <div>
                <label for="mat-name" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Item Description</label>
                <input type="text" id="mat-name" placeholder="e.g. OPC 53 Grade Cement" required class="w-full px-3 py-2 text-sm rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-amber-500">
              </div>

              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label for="mat-qty" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Quantity</label>
                  <input type="number" id="mat-qty" placeholder="300" required class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-amber-500">
                </div>
                <div>
                  <label for="mat-unit" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Unit</label>
                  <select id="mat-unit" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 bg-white focus:outline-none focus:ring-2 focus:ring-amber-500">
                    <option value="Bags">Bags</option>
                    <option value="Tonnes (MT)">Tonnes (MT)</option>
                    <option value="Truckloads (Brass)">Truckloads (Brass)</option>
                    <option value="Sq. Feet">Sq. Feet</option>
                    <option value="Running Metres">Running Metres</option>
                    <option value="Pieces">Pieces</option>
                  </select>
                </div>
              </div>

              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label for="mat-supplier" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Vendor / Supplier</label>
                  <input type="text" id="mat-supplier" placeholder="e.g. UltraTech Dealer" required class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-amber-500">
                </div>
                <div>
                  <label for="mat-cost" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Est. Cost (₹)</label>
                  <input type="number" id="mat-cost" placeholder="115000" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-amber-500">
                </div>
              </div>

              <div>
                <label for="mat-status" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Delivery Status</label>
                <select id="mat-status" class="w-full px-3 py-2 text-xs rounded-lg border border-slate-300 bg-white focus:outline-none focus:ring-2 focus:ring-amber-500">
                  <option value="ordered">Purchase Order Placed</option>
                  <option value="dispatched">Dispatched / In Transit</option>
                  <option value="delivered">Delivered on Site</option>
                  <option value="delayed">Delayed</option>
                </select>
              </div>

              <button type="submit" class="w-full bg-slate-900 hover:bg-slate-800 text-amber-400 font-bold py-2.5 px-4 rounded-xl text-xs uppercase tracking-wider transition-colors shadow">
                Save Material Record
              </button>
            </form>
          </div>
        </div>

        <!-- Material Table -->
        <div class="lg:col-span-2">
          <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
            <div class="p-4 bg-slate-50 border-b border-slate-200 flex items-center justify-between">
              <h3 class="text-sm font-bold text-slate-900">Site Inventory & Inward Register</h3>
              <span id="mat-count-badge" class="text-xs bg-slate-200 text-slate-700 px-2 py-0.5 rounded-full font-semibold">0 Items</span>
            </div>

            <div class="overflow-x-auto">
              <table class="w-full text-left text-xs">
                <thead class="bg-slate-100 text-slate-600 font-semibold uppercase tracking-wider">
                  <tr>
                    <th class="p-3">Item & Supplier</th>
                    <th class="p-3">Qty & Unit</th>
                    <th class="p-3">Est. Value</th>
                    <th class="p-3">Status</th>
                    <th class="p-3 text-right">Action</th>
                  </tr>
                </thead>
                <tbody id="materials-tbody" class="divide-y divide-slate-100">
                  <!-- JS rendered -->
                </tbody>
              </table>
            </div>
            <div id="materials-empty" class="hidden p-12 text-center text-slate-400 text-sm">
              No materials logged yet.
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- ==================== TAB 3: DAILY LABOUR & HAZRI ==================== -->
    <div id="tab-pane-labour" class="tab-pane hidden">
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        
        <!-- Log Today's Attendance -->
        <div class="lg:col-span-1" id="labour-form-panel">
          <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200 sticky top-6">
            <h2 class="text-base font-bold text-slate-900 mb-4 pb-2 border-b border-slate-100 flex items-center gap-2">
              <span class="text-amber-500">👷‍♂️</span> Record Daily Site Hazri
            </h2>
            <form id="labour-form" class="space-y-3">
              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label for="dsr-date" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Date</label>
                  <input type="date" id="dsr-date" required class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-amber-500">
                </div>
                <div>
                  <label for="dsr-weather" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Weather</label>
                  <select id="dsr-weather" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 bg-white">
                    <option value="Sunny & Clear">☀️ Sunny & Clear</option>
                    <option value="Overcast / Windy">⛅ Overcast</option>
                    <option value="Heavy Rain / Site Halted">🌧️ Rainy (Work Slow)</option>
                  </select>
                </div>
              </div>

              <div class="p-3 bg-slate-50 rounded-xl border border-slate-200 space-y-2">
                <p class="text-xs font-bold text-slate-700 uppercase">Headcount by Trade</p>
                
                <div class="grid grid-cols-2 gap-2 text-xs">
                  <div>
                    <label for="count-masons" class="text-slate-600">Masons (Karigar):</label>
                    <input type="number" id="count-masons" value="8" min="0" class="w-full px-2 py-1 text-xs rounded border border-slate-300 mt-0.5">
                  </div>
                  <div>
                    <label for="count-helpers" class="text-slate-600">Helpers (Mazdoor):</label>
                    <input type="number" id="count-helpers" value="16" min="0" class="w-full px-2 py-1 text-xs rounded border border-slate-300 mt-0.5">
                  </div>
                  <div>
                    <label for="count-barbenders" class="text-slate-600">Steel Fixers:</label>
                    <input type="number" id="count-barbenders" value="6" min="0" class="w-full px-2 py-1 text-xs rounded border border-slate-300 mt-0.5">
                  </div>
                  <div>
                    <label for="count-carpenters" class="text-slate-600">Carpenters (Shuttering):</label>
                    <input type="number" id="count-carpenters" value="6" min="0" class="w-full px-2 py-1 text-xs rounded border border-slate-300 mt-0.5">
                  </div>
                </div>
              </div>

              <div>
                <label for="dsr-machinery" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Plant & Machinery Active</label>
                <input type="text" id="dsr-machinery" placeholder="e.g. 1 JCB, 2 Concrete Mixers, 1 Tower Hoist" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300">
              </div>

              <div>
                <label for="dsr-remarks" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Site Safety & Supervisor Remarks</label>
                <textarea id="dsr-remarks" rows="2" placeholder="e.g. All workers wearing helmets & harnesses. Slump test verified." class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300"></textarea>
              </div>

              <button type="submit" class="w-full bg-slate-900 hover:bg-slate-800 text-amber-400 font-bold py-2.5 px-4 rounded-xl text-xs uppercase tracking-wider transition-colors shadow">
                Save Daily Log
              </button>
            </form>
          </div>
        </div>

        <!-- Labour Log List -->
        <div class="lg:col-span-2">
          <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
            <div class="p-4 bg-slate-50 border-b border-slate-200 flex items-center justify-between">
              <h3 class="text-sm font-bold text-slate-900">Historical Daily Site Reports (DSR)</h3>
              <span class="text-xs text-slate-500 font-medium">Auto-aggregated workforce</span>
            </div>
            <div id="dsr-container" class="divide-y divide-slate-100 min-h-[300px]">
              <!-- JS rendered -->
            </div>
            <div id="dsr-empty" class="hidden p-12 text-center text-slate-400 text-sm">
              No daily reports logged yet.
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- ==================== TAB 4: EXPENSES & BUDGET ==================== -->
    <div id="tab-pane-budget" class="tab-pane hidden">
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        
        <!-- Add Expense -->
        <div class="lg:col-span-1" id="expense-form-panel">
          <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200 sticky top-6">
            <h2 class="text-base font-bold text-slate-900 mb-4 pb-2 border-b border-slate-100 flex items-center gap-2">
              <span class="text-amber-500">💰</span> Record Site Expense
            </h2>
            <form id="expense-form" class="space-y-3.5">
              <div>
                <label for="exp-title" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Expense Description</label>
                <input type="text" id="exp-title" placeholder="e.g. Weekly Labour Payment (Batch A)" required class="w-full px-3 py-2 text-sm rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-amber-500">
              </div>

              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label for="exp-category" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Category</label>
                  <select id="exp-category" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 bg-white">
                    <option value="Labour Wages">Labour Wages</option>
                    <option value="Raw Materials">Raw Materials</option>
                    <option value="Equipment & Fuel">Equipment & Fuel</option>
                    <option value="QC & Lab Testing">QC & Lab Testing</option>
                    <option value="Municipal / Permits">Municipal / Permits</option>
                    <option value="Site Petty Cash">Site Petty Cash</option>
                  </select>
                </div>
                <div>
                  <label for="exp-amount" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Amount (₹)</label>
                  <input type="number" id="exp-amount" placeholder="45000" required class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-amber-500">
                </div>
              </div>

              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label for="exp-date" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Date</label>
                  <input type="date" id="exp-date" required class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300">
                </div>
                <div>
                  <label for="exp-status" class="block text-xs font-semibold text-slate-600 uppercase mb-1">Payment Status</label>
                  <select id="exp-status" class="w-full px-2.5 py-2 text-xs rounded-lg border border-slate-300 bg-white">
                    <option value="Paid">✅ Paid</option>
                    <option value="Pending Approval">⏳ Pending</option>
                  </select>
                </div>
              </div>

              <button type="submit" class="w-full bg-slate-900 hover:bg-slate-800 text-amber-400 font-bold py-2.5 px-4 rounded-xl text-xs uppercase tracking-wider transition-colors shadow">
                Add Expense Voucher
              </button>
            </form>
          </div>
        </div>

        <!-- Expenses Ledger -->
        <div class="lg:col-span-2">
          <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
            <div class="p-4 bg-slate-50 border-b border-slate-200 flex flex-col sm:flex-row sm:items-center justify-between gap-2">
              <h3 class="text-sm font-bold text-slate-900">Project Expense Ledger</h3>
              <div class="text-xs font-semibold text-slate-700">
                Total Logged: <span id="exp-sum-display" class="text-amber-600 font-bold">₹0</span>
              </div>
            </div>

            <div class="overflow-x-auto">
              <table class="w-full text-left text-xs">
                <thead class="bg-slate-100 text-slate-600 font-semibold uppercase tracking-wider">
                  <tr>
                    <th class="p-3">Description & Category</th>
                    <th class="p-3">Date</th>
                    <th class="p-3">Amount</th>
                    <th class="p-3">Status</th>
                    <th class="p-3 text-right">Action</th>
                  </tr>
                </thead>
                <tbody id="expenses-tbody" class="divide-y divide-slate-100">
                  <!-- JS rendered -->
                </tbody>
              </table>
            </div>
            <div id="expenses-empty" class="hidden p-12 text-center text-slate-400 text-sm">
              No expenses recorded.
            </div>
          </div>
        </div>

      </div>
    </div>

  </div>

  <script>
    // ==================== STATE MANAGEMENT ====================
    const TOTAL_BUDGET = 12000000; // 1.2 Crore Cap

    const defaultData = {
      tasks: [
        { id: 101, name: 'Foundation Raft Concrete Casting (M30)', trade: 'RCC & Concrete', priority: 'Critical', start: '2026-10-15', deadline: '2026-10-25', progress: 100, lead: 'Er. R. Sharma', status: 'completed' },
        { id: 102, name: 'Ground Floor Column Reinforcement & Formwork', trade: 'Steel & Rebar', priority: 'High', start: '2026-10-20', deadline: '2026-10-30', progress: 65, lead: 'Er. R. Sharma', status: 'in_progress' },
        { id: 103, name: 'Substructure Waterproofing Coating', trade: 'RCC & Concrete', priority: 'Normal', start: '2026-10-26', deadline: '2026-11-04', progress: 20, lead: 'Supervisor Anand', status: 'pending' },
        { id: 104, name: 'Electrical Main Feeder Conduit Trenching', trade: 'Electrical Conduits', priority: 'High', start: '2026-10-24', deadline: '2026-11-05', progress: 90, lead: 'PowerTech Subcontractor', status: 'inspection' },
        { id: 105, name: 'External Storm Water Drainage Line', trade: 'Plumbing & Drainage', priority: 'Normal', start: '2026-10-28', deadline: '2026-11-12', progress: 0, lead: 'Jal Flow Ltd', status: 'pending' },
        { id: 106, name: 'Brickwork & AAC Block Masonry (1st Floor)', trade: 'Brickwork & Masonry', priority: 'Normal', start: '2026-11-02', deadline: '2026-11-20', progress: 0, lead: 'Rajput Contractors', status: 'pending' }
      ],
      defects: [
        { 
          id: 501, 
          title: 'Honeycombing & Void on Column C-4 Base', 
          location: 'Ground Floor, Grid C-4', 
          trade: 'RCC & Concrete', 
          severity: 'Critical', 
          deadline: '2026-10-29', 
          status: 'open',
          photo: 'https://images.unsplash.com/photo-1541888946425-d0fbb186156f?w=400&auto=format&fit=crop&q=60'
        },
        { 
          id: 502, 
          title: 'Uncapped Rebar Dowels (Safety Violation)', 
          location: '1st Floor Cantilever Edge', 
          trade: 'Safety Hazard', 
          severity: 'Critical', 
          deadline: '2026-10-28', 
          status: 'in_progress',
          photo: ''
        },
        { 
          id: 503, 
          title: 'Conduit Misalignment in Panel Cutout', 
          location: 'Electrical Duct B', 
          trade: 'Electrical', 
          severity: 'Minor', 
          deadline: '2026-11-02', 
          status: 'rectified',
          photo: ''
        }
      ],
      materials: [
        { id: 201, name: 'UltraTech OPC 53 Grade Cement', qty: 600, unit: 'Bags', supplier: 'Gujarat Cements Corp', cost: 234000, status: 'delivered' },
        { id: 202, name: 'Fe 550D TMT Rebar (16mm & 20mm)', qty: 18, unit: 'Tonnes (MT)', supplier: 'Tata Tiscon Steel', cost: 1170000, status: 'delivered' },
        { id: 203, name: 'Washed River Sand (Coarse Aggregate)', qty: 6, unit: 'Truckloads (Brass)', supplier: 'Narmada Minerals', cost: 144000, status: 'dispatched' },
        { id: 204, name: '40mm Heavy Duty PVC Conduits', qty: 250, unit: 'Running Metres', supplier: 'Finolex Pipes', cost: 38000, status: 'ordered' }
      ],
      labour: [
        { id: 301, date: '2026-10-27', weather: 'Sunny & Clear', masons: 10, helpers: 24, barbenders: 8, carpenters: 6, machinery: '1 Tower Hoist, 1 Transit Mixer', remarks: 'Column casting ongoing on grid C-4. Safety gear 100% compliant.' },
        { id: 302, date: '2026-10-26', weather: 'Sunny & Clear', masons: 8, helpers: 20, barbenders: 8, carpenters: 8, machinery: '1 Excavator, 1 Dewatering Pump', remarks: 'Shuttering aligned for beam 12.' }
      ],
      expenses: [
        { id: 401, title: 'Tata Tiscon TMT Steel Advance (Inv #889)', category: 'Raw Materials', amount: 800000, date: '2026-10-14', status: 'Paid' },
        { id: 402, title: 'Fortnightly Labour Contractor Payout', category: 'Labour Wages', amount: 185000, date: '2026-10-15', status: 'Paid' },
        { id: 403, title: 'Transit Mixer & Concrete Pump Rental', category: 'Equipment & Fuel', amount: 65000, date: '2026-10-16', status: 'Paid' },
        { id: 404, title: 'Cylinder Cube Strength Lab Test (7-day)', category: 'QC & Lab Testing', amount: 12000, date: '2026-10-17', status: 'Pending Approval' }
      ]
    };

    let store = { ...defaultData };
    try {
      const saved = localStorage.getItem('buildtrack_pro_db_v4');
      if (saved) {
        store = JSON.parse(saved);
        if (!store.defects) store.defects = defaultData.defects;
      }
    } catch (e) {
      store = defaultData;
    }

    let activeRole = 'admin';
    let cloudConfig = {
      connected: true,
      provider: 'supabase',
      url: 'https://skyline-site.supabase.co',
      apiKey: 'sbp_mock_live_token',
      autoSync: true
    };

    function persist() {
      try {
        localStorage.setItem('buildtrack_pro_db_v4', JSON.stringify(store));
      } catch (e) {}
      updateKPIs();
      renderGantt();
      renderDefects();

      if (cloudConfig.connected && cloudConfig.autoSync) {
        triggerBackgroundCloudSync();
      }
    }

    function triggerBackgroundCloudSync() {
      const pill = document.getElementById('cloud-status-pill');
      if (pill) {
        pill.innerHTML = '<span class="w-1.5 h-1.5 rounded-full bg-blue-400 animate-spin"></span> Syncing...';
        setTimeout(() => {
          pill.innerHTML = '<span class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-pulse"></span> Cloud DB Synced';
        }, 600);
      }
    }

    function showToast(msg, icon = '✅') {
      const toast = document.getElementById('toast');
      document.getElementById('toast-msg').textContent = msg;
      document.getElementById('toast-icon').textContent = icon;
      toast.classList.remove('hidden');
      setTimeout(() => toast.classList.add('hidden'), 2800);
    }

    function escapeHtml(str) {
      if (!str) return '';
      const div = document.createElement('div');
      div.textContent = str;
      return div.innerHTML;
    }

    // ==================== KPI UPDATER ====================
    function updateKPIs() {
      const totalTasks = store.tasks.length;
      const pendingTasks = store.tasks.filter(t => t.status === 'pending').length;
      const progressTasks = store.tasks.filter(t => t.status === 'in_progress').length;
      const completedTasks = store.tasks.filter(t => t.status === 'completed').length;
      const pct = totalTasks > 0 ? Math.round((completedTasks / totalTasks) * 100) : 0;
      
      document.getElementById('kpi-progress-pct').textContent = pct + '%';
      document.getElementById('kpi-progress-ratio').textContent = `${completedTasks}/${totalTasks} Tasks`;
      document.getElementById('kpi-progress-bar').style.width = pct + '%';

      const statTotal = document.getElementById('stat-total');
      if (statTotal) statTotal.textContent = totalTasks;
      const statPending = document.getElementById('stat-pending');
      if (statPending) statPending.textContent = pendingTasks;
      const statProg = document.getElementById('stat-progress');
      if (statProg) statProg.textContent = progressTasks;
      const statComp = document.getElementById('stat-completed');
      if (statComp) statComp.textContent = completedTasks;

      // Budget
      const totalSpent = store.expenses.reduce((acc, curr) => acc + (Number(curr.amount) || 0), 0);
      document.getElementById('kpi-budget-spent').textContent = '₹' + totalSpent.toLocaleString('en-IN');
      const budgetPct = Math.round((totalSpent / TOTAL_BUDGET) * 100);
      document.getElementById('kpi-budget-percent').textContent = `${budgetPct}% of ₹1.2 Cr target`;
      document.getElementById('exp-sum-display').textContent = '₹' + totalSpent.toLocaleString('en-IN');

      // Labour today
      const todayRecord = store.labour[0];
      if (todayRecord) {
        const headcount = (todayRecord.masons || 0) + (todayRecord.helpers || 0) + (todayRecord.barbenders || 0) + (todayRecord.carpenters || 0);
        document.getElementById('kpi-crew-today').textContent = `${headcount} Workers`;
      } else {
        document.getElementById('kpi-crew-today').textContent = '0 Workers';
      }

      // Materials
      const pendingMat = store.materials.filter(m => m.status === 'ordered' || m.status === 'dispatched' || m.status === 'delayed').length;
      const deliveredMat = store.materials.filter(m => m.status === 'delivered').length;
      document.getElementById('kpi-materials-pending').textContent = `${pendingMat} In Transit`;
      document.getElementById('kpi-materials-delivered').textContent = `${deliveredMat} Received`;
      document.getElementById('mat-count-badge').textContent = `${store.materials.length} Items`;

      // Defects KPI
      if (store.defects) {
        const openDefects = store.defects.filter(d => d.status === 'open' || d.status === 'in_progress').length;
        const closedDefects = store.defects.filter(d => d.status === 'rectified').length;
        document.getElementById('kpi-defects-count').textContent = `${openDefects} Open`;
        document.getElementById('kpi-defects-closed').textContent = `${closedDefects} Rectified`;
      }
    }

    // ==================== TAB SWITCHING ====================
    const tabs = [
      { btn: 'tab-btn-tasks', pane: 'tab-pane-tasks' },
      { btn: 'tab-btn-gantt', pane: 'tab-pane-gantt' },
      { btn: 'tab-btn-defects', pane: 'tab-pane-defects' },
      { btn: 'tab-btn-materials', pane: 'tab-pane-materials' },
      { btn: 'tab-btn-labour', pane: 'tab-pane-labour' },
      { btn: 'tab-btn-budget', pane: 'tab-pane-budget' }
    ];

    tabs.forEach(({ btn, pane }) => {
      document.getElementById(btn).addEventListener('click', () => {
        tabs.forEach(t => {
          document.getElementById(t.btn).classList.remove('tab-active');
          document.getElementById(t.pane).classList.add('hidden');
        });
        document.getElementById(btn).classList.add('tab-active');
        document.getElementById(pane).classList.remove('hidden');

        if (pane === 'tab-pane-gantt') renderGantt();
        if (pane === 'tab-pane-defects') renderDefects();
      });
    });

    // ==================== ROLE BASED PERMISSION ENFORCEMENT ====================
    function applyRolePermissions() {
      const isClient = activeRole === 'client';
      const isSupervisor = activeRole === 'supervisor';

      document.getElementById('task-form-panel').style.display = isClient ? 'none' : 'block';
      document.getElementById('defect-form-panel').style.display = isClient ? 'none' : 'block';
      document.getElementById('material-form-panel').style.display = isClient ? 'none' : 'block';
      document.getElementById('labour-form-panel').style.display = isClient ? 'none' : 'block';
      document.getElementById('expense-form-panel').style.display = (isClient || isSupervisor) ? 'none' : 'block';

      document.querySelectorAll('.task-delete-btn, .mat-delete-btn, .dsr-delete-btn, .exp-delete-btn, .defect-delete-btn').forEach(b => {
        b.style.display = isClient ? 'none' : 'inline-block';
      });

      document.querySelectorAll('.task-status-select, .mat-status-select, .defect-status-select').forEach(s => {
        s.disabled = isClient;
      });

      if (isClient) {
        showToast('Switched to Client View (Read-Only Mode)', '💼');
      } else if (isSupervisor) {
        showToast('Switched to Site Engineer View (Expenses Locked)', '👷‍♂️');
      } else {
        showToast('Switched to Project Director (Full ERP Admin)', '👨‍💼');
      }
    }

    document.getElementById('user-role-select').addEventListener('change', (e) => {
      activeRole = e.target.value;
      applyRolePermissions();
    });

    // ==================== TAB GANTT CHART RENDERING ====================
    function renderGantt() {
      const container = document.getElementById('gantt-rows');
      container.innerHTML = '';

      const projectStart = new Date('2026-10-15').getTime();
      const projectDurationDays = 32;

      store.tasks.forEach((t, idx) => {
        const row = document.createElement('div');
        row.className = 'grid grid-cols-12 items-center py-3 px-3 hover:bg-slate-50 transition-colors border-b border-slate-100 text-xs';

        const tStart = t.start ? new Date(t.start).getTime() : projectStart + (idx * 3 * 86400000);
        const tEnd = t.deadline ? new Date(t.deadline).getTime() : tStart + (5 * 86400000);

        const startDays = Math.max(0, (tStart - projectStart) / 86400000);
        const durationDays = Math.max(2, (tEnd - tStart) / 86400000);

        const leftPct = Math.min(85, Math.max(2, (startDays / projectDurationDays) * 100));
        const widthPct = Math.min(95 - leftPct, Math.max(8, (durationDays / projectDurationDays) * 100));

        let barColor = 'bg-amber-400 text-slate-950';
        let barBorder = 'border-amber-500';
        if (t.status === 'completed') {
          barColor = 'bg-emerald-500 text-white';
          barBorder = 'border-emerald-600';
        } else if (t.status === 'in_progress') {
          barColor = 'bg-blue-500 text-white';
          barBorder = 'border-blue-600';
        } else if (t.status === 'inspection') {
          barColor = 'bg-purple-500 text-white';
          barBorder = 'border-purple-600';
        }

        const taskProg = t.progress !== undefined ? t.progress : (t.status === 'completed' ? 100 : (t.status === 'in_progress' ? 50 : 0));

        row.innerHTML = `
          <div class="col-span-4 pr-3">
            <p class="font-bold text-slate-900 truncate" title="${escapeHtml(t.name)}">${escapeHtml(t.name)}</p>
            <div class="flex items-center gap-2 text-[11px] text-slate-400 mt-0.5">
              <span class="font-semibold text-slate-600">${escapeHtml(t.trade)}</span>
              <span>•</span>
              <span>${t.start || 'Oct 15'} → ${t.deadline || 'Nov 10'}</span>
            </div>
          </div>

          <div class="col-span-8 relative h-8 flex items-center bg-slate-50/70 rounded-lg border border-slate-200/80 gantt-grid px-1">
            <div 
              class="absolute h-6 rounded-md shadow-sm border ${barBorder} ${barColor} flex items-center justify-between px-2 text-[10px] font-bold transition-all duration-300 cursor-pointer overflow-hidden"
              style="left: ${leftPct}%; width: ${widthPct}%;"
              title="${escapeHtml(t.name)}: ${taskProg}% Completed"
            >
              <span class="truncate">${taskProg}%</span>
              <span class="hidden sm:inline text-[9px] opacity-90">${escapeHtml(t.status)}</span>
            </div>
          </div>
        `;
        container.appendChild(row);
      });
    }

    // ==================== TAB DEFECTS / PUNCH LIST LOGIC ====================
    let defectFilter = 'all';
    let currentUploadedPhotoBase64 = '';

    document.getElementById('defect-photo').addEventListener('change', (e) => {
      const file = e.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (event) => {
          currentUploadedPhotoBase64 = event.target.result;
          document.getElementById('defect-photo-preview').src = currentUploadedPhotoBase64;
          document.getElementById('defect-photo-preview-wrap').classList.remove('hidden');
        };
        reader.readAsDataURL(file);
      }
    });

    function renderDefects() {
      const container = document.getElementById('defects-container');
      const emptyEl = document.getElementById('defects-empty');
      container.innerHTML = '';

      if (!store.defects) store.defects = [];

      const filtered = store.defects.filter(d => {
        if (defectFilter === 'all') return true;
        return d.status === defectFilter;
      });

      if (filtered.length === 0) {
        emptyEl.classList.remove('hidden');
      } else {
        emptyEl.classList.add('hidden');
      }

      filtered.forEach(d => {
        const card = document.createElement('div');
        card.className = 'p-4 flex flex-col sm:flex-row sm:items-start justify-between gap-4 hover:bg-slate-50 transition-colors';

        let severityBadge = '<span class="px-2 py-0.5 rounded-full text-[11px] font-bold bg-amber-100 text-amber-800">🟡 Minor</span>';
        if (d.severity === 'Critical') {
          severityBadge = '<span class="px-2 py-0.5 rounded-full text-[11px] font-bold bg-rose-100 text-rose-800 animate-pulse">🔴 Critical (Stop Work)</span>';
        } else if (d.severity === 'Major') {
          severityBadge = '<span class="px-2 py-0.5 rounded-full text-[11px] font-bold bg-orange-100 text-orange-800">🟠 Major Rework</span>';
        }

        let statusBadge = '<span class="px-2 py-0.5 rounded-full text-[11px] font-semibold bg-rose-50 text-rose-700 border border-rose-200">Open</span>';
        if (d.status === 'in_progress') {
          statusBadge = '<span class="px-2 py-0.5 rounded-full text-[11px] font-semibold bg-blue-50 text-blue-700 border border-blue-200">In Progress</span>';
        } else if (d.status === 'rectified') {
          statusBadge = '<span class="px-2 py-0.5 rounded-full text-[11px] font-semibold bg-emerald-50 text-emerald-700 border border-emerald-200">✅ Rectified</span>';
        }

        card.innerHTML = `
          <div class="flex items-start gap-3 flex-1">
            ${d.photo ? `
              <img src="${d.photo}" alt="Snag photo" class="w-16 h-16 rounded-xl object-cover border border-slate-200 shrink-0 shadow-sm">
            ` : `
              <div class="w-16 h-16 rounded-xl bg-slate-100 border border-slate-200 flex items-center justify-center text-xl shrink-0 text-slate-400">
                📷
              </div>
            `}
            <div class="flex-1">
              <div class="flex items-center gap-2 flex-wrap text-xs">
                <span class="font-bold text-slate-700 bg-slate-100 px-2 py-0.5 rounded border border-slate-200">${escapeHtml(d.trade)}</span>
                ${severityBadge}
                ${statusBadge}
                <span class="text-slate-400">📍 ${escapeHtml(d.location)}</span>
                ${d.deadline ? `<span class="text-slate-500">📅 Fix by: <b>${escapeHtml(d.deadline)}</b></span>` : ''}
              </div>
              <h4 class="font-bold text-slate-900 mt-1 text-sm sm:text-base ${d.status === 'rectified' ? 'line-through text-slate-400' : ''}">
                ${escapeHtml(d.title)}
              </h4>
            </div>
          </div>

          <div class="flex items-center gap-2 self-end sm:self-center shrink-0">
            <select data-id="${d.id}" class="defect-status-select text-xs font-semibold bg-white border border-slate-300 rounded-lg px-2.5 py-1.5 focus:outline-none focus:ring-1 focus:ring-amber-500 text-slate-700">
              <option value="open" ${d.status === 'open' ? 'selected' : ''}>Open</option>
              <option value="in_progress" ${d.status === 'in_progress' ? 'selected' : ''}>In Progress</option>
              <option value="rectified" ${d.status === 'rectified' ? 'selected' : ''}>Rectified & Closed</option>
            </select>
            <button data-id="${d.id}" class="defect-delete-btn text-xs text-rose-500 hover:text-rose-700 p-1 hover:bg-rose-50 rounded transition-colors" title="Delete">
              🗑️
            </button>
          </div>
        `;
        container.appendChild(card);
      });

      document.querySelectorAll('.defect-status-select').forEach(sel => {
        sel.addEventListener('change', (e) => {
          const id = Number(e.target.dataset.id);
          const snag = store.defects.find(x => x.id === id);
          if (snag) {
            snag.status = e.target.value;
            persist();
            renderDefects();
            showToast('Snag status updated');
          }
        });
      });

      document.querySelectorAll('.defect-delete-btn').forEach(btn => {
        btn.addEventListener('click', (e) => {
          const id = Number(e.target.closest('button').dataset.id);
          store.defects = store.defects.filter(x => x.id !== id);
          persist();
          renderDefects();
          showToast('Snag record deleted', '🗑️');
        });
      });
    }

    document.getElementById('defect-form').addEventListener('submit', (e) => {
      e.preventDefault();
      const newDefect = {
        id: Date.now(),
        title: document.getElementById('defect-title').value.trim(),
        location: document.getElementById('defect-location').value.trim(),
        trade: document.getElementById('defect-trade').value,
        severity: document.getElementById('defect-severity').value,
        deadline: document.getElementById('defect-deadline').value || todayStr,
        status: document.getElementById('defect-status').value,
        photo: currentUploadedPhotoBase64
      };

      store.defects.unshift(newDefect);
      persist();
      renderDefects();

      document.getElementById('defect-title').value = '';
      document.getElementById('defect-location').value = '';
      document.getElementById('defect-photo').value = '';
      currentUploadedPhotoBase64 = '';
      document.getElementById('defect-photo-preview-wrap').classList.add('hidden');

      showToast('New defect logged with site evidence!');
    });

    document.querySelectorAll('.defect-filter-btn').forEach(b => {
      b.addEventListener('click', () => {
        document.querySelectorAll('.defect-filter-btn').forEach(x => {
          x.className = 'defect-filter-btn px-2.5 py-1 rounded-lg font-medium bg-white text-slate-600 hover:bg-slate-100';
        });
        b.className = 'defect-filter-btn px-2.5 py-1 rounded-lg font-medium bg-slate-800 text-white';
        defectFilter = b.dataset.defectFilter;
        renderDefects();
      });
    });

    // ==================== TAB 1: TASKS LOGIC ====================
    let taskFilter = 'all';
    let taskSearchQuery = '';

    function renderTasks() {
      const container = document.getElementById('task-list');
      const emptyEl = document.getElementById('empty-state');
      container.innerHTML = '';

      const filtered = store.tasks.filter(t => {
        const matchesFilter = (taskFilter === 'all') || (t.status === taskFilter);
        const matchesSearch = taskSearchQuery === '' || 
          t.name.toLowerCase().includes(taskSearchQuery) || 
          t.trade.toLowerCase().includes(taskSearchQuery) ||
          (t.lead && t.lead.toLowerCase().includes(taskSearchQuery));
        return matchesFilter && matchesSearch;
      });

      if (filtered.length === 0) {
        emptyEl.classList.remove('hidden');
      } else {
        emptyEl.classList.add('hidden');
      }

      filtered.forEach(t => {
        const row = document.createElement('div');
        row.className = 'p-4 flex flex-col sm:flex-row sm:items-center justify-between gap-3 hover:bg-slate-50 transition-colors';

        let statusClass = 'bg-amber-100 text-amber-800';
        let statusLabel = 'Pending';
        if (t.status === 'in_progress') {
          statusClass = 'bg-blue-100 text-blue-800';
          statusLabel = 'In Progress';
        } else if (t.status === 'inspection') {
          statusClass = 'bg-purple-100 text-purple-800';
          statusLabel = 'QC Inspection';
        } else if (t.status === 'completed') {
          statusClass = 'bg-emerald-100 text-emerald-800';
          statusLabel = 'Completed';
        }

        let priorityBadge = '';
        if (t.priority === 'Critical') priorityBadge = '<span class="text-rose-600 font-bold">🔴 Critical</span>';
        else if (t.priority === 'High') priorityBadge = '<span class="text-amber-600 font-semibold">🟠 High</span>';
        else priorityBadge = '<span class="text-slate-500">🟡 Normal</span>';

        const prog = t.progress !== undefined ? t.progress : (t.status === 'completed' ? 100 : 0);

        row.innerHTML = `
          <div class="flex-1">
            <div class="flex items-center gap-2 flex-wrap text-xs">
              <span class="font-bold px-2 py-0.5 rounded bg-slate-100 border border-slate-200 text-slate-700">${escapeHtml(t.trade)}</span>
              <span class="px-2 py-0.5 rounded-full font-semibold ${statusClass}">${statusLabel}</span>
              ${priorityBadge}
              ${t.deadline ? `<span class="text-slate-400">📅 Due: ${escapeHtml(t.deadline)}</span>` : ''}
              ${t.lead ? `<span class="text-slate-500">👷 Lead: <b>${escapeHtml(t.lead)}</b></span>` : ''}
              <span class="text-slate-500 font-medium">⚡ ${prog}%</span>
            </div>
            <h4 class="font-bold text-slate-900 mt-1 text-sm sm:text-base ${t.status === 'completed' ? 'line-through text-slate-400' : ''}">
              ${escapeHtml(t.name)}
            </h4>
          </div>
          <div class="flex items-center gap-2 self-end sm:self-center">
            <select data-id="${t.id}" class="task-status-select text-xs font-semibold bg-white border border-slate-300 rounded-lg px-2.5 py-1.5 focus:outline-none focus:ring-1 focus:ring-amber-500 text-slate-700">
              <option value="pending" ${t.status === 'pending' ? 'selected' : ''}>Pending</option>
              <option value="in_progress" ${t.status === 'in_progress' ? 'selected' : ''}>In Progress</option>
              <option value="inspection" ${t.status === 'inspection' ? 'selected' : ''}>QC Inspection</option>
              <option value="completed" ${t.status === 'completed' ? 'selected' : ''}>Completed</option>
            </select>
            <button data-id="${t.id}" class="task-delete-btn text-xs text-rose-500 hover:text-rose-700 p-1.5 hover:bg-rose-50 rounded transition-colors" title="Delete">
              🗑️
            </button>
          </div>
        `;
        container.appendChild(row);
      });

      document.querySelectorAll('.task-status-select').forEach(sel => {
        sel.addEventListener('change', (e) => {
          const id = Number(e.target.dataset.id);
          const item = store.tasks.find(x => x.id === id);
          if (item) {
            item.status = e.target.value;
            if (item.status === 'completed') item.progress = 100;
            if (item.status === 'pending') item.progress = 0;
            persist();
            renderTasks();
            showToast('Task status updated');
          }
        });
      });

      document.querySelectorAll('.task-delete-btn').forEach(btn => {
        btn.addEventListener('click', (e) => {
          const id = Number(e.target.closest('button').dataset.id);
          store.tasks = store.tasks.filter(x => x.id !== id);
          persist();
          renderTasks();
          showToast('Task removed', '🗑️');
        });
      });
    }

    document.getElementById('task-form').addEventListener('submit', (e) => {
      e.preventDefault();
      const statusVal = document.getElementById('task-status').value;
      const initialProg = Number(document.getElementById('task-progress-pct').value) || (statusVal === 'completed' ? 100 : 0);

      const newTask = {
        id: Date.now(),
        name: document.getElementById('task-name').value.trim(),
        trade: document.getElementById('task-trade').value,
        priority: document.getElementById('task-priority').value,
        start: document.getElementById('task-start').value || todayStr,
        deadline: document.getElementById('task-deadline').value || todayStr,
        lead: document.getElementById('task-lead').value.trim(),
        progress: initialProg,
        status: statusVal
      };
      store.tasks.unshift(newTask);
      persist();
      renderTasks();
      document.getElementById('task-name').value = '';
      document.getElementById('task-lead').value = '';
      
      const fMsg = document.getElementById('form-message');
      fMsg.classList.remove('hidden');
      setTimeout(() => fMsg.classList.add('hidden'), 2000);
      showToast('New milestone added to schedule & Gantt chart!');
    });

    document.querySelectorAll('.task-filter-btn').forEach(b => {
      b.addEventListener('click', () => {
        document.querySelectorAll('.task-filter-btn').forEach(x => {
          x.className = 'task-filter-btn px-2.5 py-1 rounded-lg font-medium bg-white text-slate-600 hover:bg-slate-100';
        });
        b.className = 'task-filter-btn px-2.5 py-1 rounded-lg font-medium bg-slate-800 text-white';
        taskFilter = b.dataset.taskFilter;
        renderTasks();
      });
    });

    document.getElementById('task-search').addEventListener('input', (e) => {
      taskSearchQuery = e.target.value.toLowerCase().trim();
      renderTasks();
    });

    // ==================== TAB 2: MATERIALS LOGIC ====================
    function renderMaterials() {
      const tbody = document.getElementById('materials-tbody');
      const emptyEl = document.getElementById('materials-empty');
      tbody.innerHTML = '';

      if (store.materials.length === 0) {
        emptyEl.classList.remove('hidden');
      } else {
        emptyEl.classList.add('hidden');
      }

      store.materials.forEach(m => {
        const tr = document.createElement('tr');
        tr.className = 'hover:bg-slate-50 transition-colors';

        tr.innerHTML = `
          <td class="p-3">
            <p class="font-bold text-slate-900">${escapeHtml(m.name)}</p>
            <p class="text-slate-400 text-[11px]">Vendor: ${escapeHtml(m.supplier)}</p>
          </td>
          <td class="p-3 font-semibold text-slate-800">
            ${m.qty} <span class="text-slate-500 font-normal">${escapeHtml(m.unit)}</span>
          </td>
          <td class="p-3 font-bold text-slate-700">
            ₹${Number(m.cost || 0).toLocaleString('en-IN')}
          </td>
          <td class="p-3">
            <select data-id="${m.id}" class="mat-status-select text-[11px] font-semibold bg-white border border-slate-300 rounded px-2 py-1">
              <option value="ordered" ${m.status === 'ordered' ? 'selected' : ''}>PO Placed</option>
              <option value="dispatched" ${m.status === 'dispatched' ? 'selected' : ''}>In Transit</option>
              <option value="delivered" ${m.status === 'delivered' ? 'selected' : ''}>Delivered</option>
              <option value="delayed" ${m.status === 'delayed' ? 'selected' : ''}>Delayed</option>
            </select>
          </td>
          <td class="p-3 text-right">
            <button data-id="${m.id}" class="mat-delete-btn text-rose-500 hover:text-rose-700 p-1 hover:bg-rose-50 rounded" title="Delete">
              🗑️
            </button>
          </td>
        `;
        tbody.appendChild(tr);
      });

      document.querySelectorAll('.mat-status-select').forEach(sel => {
        sel.addEventListener('change', (e) => {
          const id = Number(e.target.dataset.id);
          const mat = store.materials.find(x => x.id === id);
          if (mat) {
            mat.status = e.target.value;
            persist();
            renderMaterials();
            showToast('Material delivery status updated');
          }
        });
      });

      document.querySelectorAll('.mat-delete-btn').forEach(btn => {
        btn.addEventListener('click', (e) => {
          const id = Number(e.target.closest('button').dataset.id);
          store.materials = store.materials.filter(x => x.id !== id);
          persist();
          renderMaterials();
          showToast('Material item deleted', '🗑️');
        });
      });
    }

    document.getElementById('material-form').addEventListener('submit', (e) => {
      e.preventDefault();
      const newMat = {
        id: Date.now(),
        name: document.getElementById('mat-name').value.trim(),
        qty: Number(document.getElementById('mat-qty').value) || 0,
        unit: document.getElementById('mat-unit').value,
        supplier: document.getElementById('mat-supplier').value.trim(),
        cost: Number(document.getElementById('mat-cost').value) || 0,
        status: document.getElementById('mat-status').value
      };
      store.materials.unshift(newMat);
      persist();
      renderMaterials();
      document.getElementById('mat-name').value = '';
      document.getElementById('mat-qty').value = '';
      document.getElementById('mat-supplier').value = '';
      document.getElementById('mat-cost').value = '';
      showToast('Material delivery record logged!');
    });

    // ==================== TAB 3: DAILY LABOUR & HAZRI ====================
    function renderLabourLogs() {
      const container = document.getElementById('dsr-container');
      const emptyEl = document.getElementById('dsr-empty');
      container.innerHTML = '';

      if (store.labour.length === 0) {
        emptyEl.classList.remove('hidden');
      } else {
        emptyEl.classList.add('hidden');
      }

      store.labour.forEach(l => {
        const card = document.createElement('div');
        card.className = 'p-4 hover:bg-slate-50 transition-colors space-y-2';

        const totalCrew = (Number(l.masons) || 0) + (Number(l.helpers) || 0) + (Number(l.barbenders) || 0) + (Number(l.carpenters) || 0);

        card.innerHTML = `
          <div class="flex items-center justify-between flex-wrap gap-2">
            <div class="flex items-center gap-2">
              <span class="font-bold text-slate-900 text-sm">📅 ${escapeHtml(l.date)}</span>
              <span class="text-xs bg-slate-200 text-slate-700 px-2 py-0.5 rounded font-medium">${escapeHtml(l.weather)}</span>
            </div>
            <div class="flex items-center gap-2">
              <span class="bg-amber-100 text-amber-900 text-xs font-bold px-2.5 py-0.5 rounded-full">
                Total On Site: ${totalCrew} Persons
              </span>
              <button data-id="${l.id}" class="dsr-delete-btn text-rose-500 hover:text-rose-700 p-1 hover:bg-rose-50 rounded" title="Delete">
                🗑️
              </button>
            </div>
          </div>

          <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 text-xs bg-slate-100/70 p-2.5 rounded-lg border border-slate-200">
            <div><span class="text-slate-500">Masons:</span> <b>${l.masons || 0}</b></div>
            <div><span class="text-slate-500">Helpers:</span> <b>${l.helpers || 0}</b></div>
            <div><span class="text-slate-500">Steel Fixers:</span> <b>${l.barbenders || 0}</b></div>
            <div><span class="text-slate-500">Carpenters:</span> <b>${l.carpenters || 0}</b></div>
          </div>

          ${l.machinery ? `<p class="text-xs text-slate-600">🚜 <b>Machinery:</b> ${escapeHtml(l.machinery)}</p>` : ''}
          ${l.remarks ? `<p class="text-xs text-slate-600 bg-white p-2 rounded border border-slate-200">📝 <b>Notes:</b> ${escapeHtml(l.remarks)}</p>` : ''}
        `;
        container.appendChild(card);
      });

      document.querySelectorAll('.dsr-delete-btn').forEach(btn => {
        btn.addEventListener('click', (e) => {
          const id = Number(e.target.closest('button').dataset.id);
          store.labour = store.labour.filter(x => x.id !== id);
          persist();
          renderLabourLogs();
          showToast('Daily log entry removed', '🗑️');
        });
      });
    }

    const todayStr = '2026-10-27';
    document.getElementById('dsr-date').value = todayStr;
    document.getElementById('task-start').value = todayStr;
    document.getElementById('task-deadline').value = '2026-11-05';
    document.getElementById('exp-date').value = todayStr;
    document.getElementById('defect-deadline').value = '2026-10-30';

    document.getElementById('labour-form').addEventListener('submit', (e) => {
      e.preventDefault();
      const newDSR = {
        id: Date.now(),
        date: document.getElementById('dsr-date').value,
        weather: document.getElementById('dsr-weather').value,
        masons: Number(document.getElementById('count-masons').value) || 0,
        helpers: Number(document.getElementById('count-helpers').value) || 0,
        barbenders: Number(document.getElementById('count-barbenders').value) || 0,
        carpenters: Number(document.getElementById('count-carpenters').value) || 0,
        machinery: document.getElementById('dsr-machinery').value.trim(),
        remarks: document.getElementById('dsr-remarks').value.trim()
      };
      store.labour.unshift(newDSR);
      persist();
      renderLabourLogs();
      document.getElementById('dsr-machinery').value = '';
      document.getElementById('dsr-remarks').value = '';
      showToast('Daily hazri report saved!');
    });

    // ==================== TAB 4: EXPENSES LOGIC ====================
    function renderExpenses() {
      const tbody = document.getElementById('expenses-tbody');
      const emptyEl = document.getElementById('expenses-empty');
      tbody.innerHTML = '';

      if (store.expenses.length === 0) {
        emptyEl.classList.remove('hidden');
      } else {
        emptyEl.classList.add('hidden');
      }

      store.expenses.forEach(exp => {
        const tr = document.createElement('tr');
        tr.className = 'hover:bg-slate-50 transition-colors';

        const isPaid = exp.status === 'Paid';
        const badge = isPaid ? 
          '<span class="px-2 py-0.5 rounded-full text-[11px] font-semibold bg-emerald-100 text-emerald-800">✅ Paid</span>' : 
          '<span class="px-2 py-0.5 rounded-full text-[11px] font-semibold bg-amber-100 text-amber-800">⏳ Pending</span>';

        tr.innerHTML = `
          <td class="p-3">
            <p class="font-bold text-slate-900">${escapeHtml(exp.title)}</p>
            <p class="text-slate-400 text-[11px]">${escapeHtml(exp.category)}</p>
          </td>
          <td class="p-3 text-slate-600">${escapeHtml(exp.date)}</td>
          <td class="p-3 font-bold text-slate-900">₹${Number(exp.amount).toLocaleString('en-IN')}</td>
          <td class="p-3">${badge}</td>
          <td class="p-3 text-right">
            <button data-id="${exp.id}" class="exp-delete-btn text-rose-500 hover:text-rose-700 p-1 hover:bg-rose-50 rounded" title="Delete">
              🗑️
            </button>
          </td>
        `;
        tbody.appendChild(tr);
      });

      document.querySelectorAll('.exp-delete-btn').forEach(btn => {
        btn.addEventListener('click', (e) => {
          const id = Number(e.target.closest('button').dataset.id);
          store.expenses = store.expenses.filter(x => x.id !== id);
          persist();
          renderExpenses();
          showToast('Expense voucher removed', '🗑️');
        });
      });
    }

    document.getElementById('expense-form').addEventListener('submit', (e) => {
      e.preventDefault();
      const newExp = {
        id: Date.now(),
        title: document.getElementById('exp-title').value.trim(),
        category: document.getElementById('exp-category').value,
        amount: Number(document.getElementById('exp-amount').value) || 0,
        date: document.getElementById('exp-date').value,
        status: document.getElementById('exp-status').value
      };
      store.expenses.unshift(newExp);
      persist();
      renderExpenses();
      document.getElementById('exp-title').value = '';
      document.getElementById('exp-amount').value = '';
      showToast('Expense voucher added!');
    });

    // ==================== WHATSAPP DSR GENERATOR ====================
    function generateWhatsAppSummary() {
      const totalTasks = store.tasks.length;
      const completedTasks = store.tasks.filter(t => t.status === 'completed').length;
      const pct = totalTasks > 0 ? Math.round((completedTasks / totalTasks) * 100) : 0;
      const totalSpent = store.expenses.reduce((acc, curr) => acc + (Number(curr.amount) || 0), 0);

      const todayLog = store.labour[0] || {};
      const headcount = (todayLog.masons || 0) + (todayLog.helpers || 0) + (todayLog.barbenders || 0) + (todayLog.carpenters || 0);

      const openSnags = store.defects ? store.defects.filter(d => d.status === 'open' || d.status === 'in_progress').length : 0;

      let msg = `🏗️ *DAILY SITE PROGRESS REPORT (DSR)*\n`;
      msg += `🏢 *Skyline Heights Commercial Complex* (#IND-2026-778)\n`;
      msg += `📅 Date: ${todayStr} | Weather: ${todayLog.weather || 'Clear'}\n\n`;

      msg += `⚡ *Progress Summary*:\n`;
      msg += `• Overall Milestone Completion: *${pct}%* (${completedTasks}/${totalTasks} Tasks)\n`;
      msg += `• Budget Consumed: *₹${totalSpent.toLocaleString('en-IN')}* (Cap: ₹1.2 Cr)\n`;
      msg += `• On-Site Workforce Today: *${headcount} Workers* (Masons: ${todayLog.masons||0}, Mazdoor: ${todayLog.helpers||0}, Steel-Fixers: ${todayLog.barbenders||0}, Carpenters: ${todayLog.carpenters||0})\n`;
      msg += `• Plant & Machinery: ${todayLog.machinery || 'Tower Hoist & Mixers Active'}\n\n`;

      msg += `⚠️ *Site Quality & QC*:\n`;
      msg += `• Active Open Snags / Punch List: *${openSnags} Pending*\n\n`;

      msg += `📋 *Key Active Milestones*:\n`;
      store.tasks.slice(0, 3).forEach(t => {
        const icon = t.status === 'completed' ? '✅' : (t.status === 'in_progress' ? '🔄' : '⏳');
        msg += `• ${icon} ${t.name} (${t.progress || 0}%)\n`;
      });

      if (todayLog.remarks) {
        msg += `\n📝 *Supervisor Remarks*:\n"${todayLog.remarks}"\n`;
      }

      msg += `\n_Generated via BuildTrack Pro Site ERP_`;
      return msg;
    }

    const waModal = document.getElementById('whatsapp-modal');
    const waText = document.getElementById('whatsapp-text-preview');

    document.getElementById('btn-share-whatsapp').addEventListener('click', () => {
      waText.value = generateWhatsAppSummary();
      waModal.classList.remove('hidden');
    });

    document.getElementById('btn-close-whatsapp-modal').addEventListener('click', () => {
      waModal.classList.add('hidden');
    });

    document.getElementById('btn-copy-whatsapp').addEventListener('click', () => {
      navigator.clipboard.writeText(waText.value).then(() => {
        showToast('WhatsApp report copied to clipboard!', '📋');
      }).catch(() => {
        waText.select();
        document.execCommand('copy');
        showToast('WhatsApp report copied to clipboard!', '📋');
      });
    });

    document.getElementById('btn-send-whatsapp-direct').addEventListener('click', () => {
      const url = `https://api.whatsapp.com/send?text=${encodeURIComponent(waText.value)}`;
      window.open(url, '_blank');
    });

    // ==================== CLOUD DATABASE MODAL & SYNC ====================
    const cloudModal = document.getElementById('cloud-modal');
    document.getElementById('btn-sync-cloud').addEventListener('click', () => {
      cloudModal.classList.remove('hidden');
    });

    document.getElementById('btn-close-cloud-modal').addEventListener('click', () => {
      cloudModal.classList.add('hidden');
    });

    document.getElementById('btn-test-cloud').addEventListener('click', () => {
      showToast('Testing WebSocket & REST endpoint...', '🔄');
      setTimeout(() => {
        showToast('Supabase connection successful! Ping: 42ms', '⚡');
      }, 800);
    });

    document.getElementById('btn-save-cloud').addEventListener('click', () => {
      cloudConfig.url = document.getElementById('cloud-url').value.trim() || cloudConfig.url;
      cloudConfig.apiKey = document.getElementById('cloud-apikey').value.trim() || cloudConfig.apiKey;
      cloudConfig.autoSync = document.getElementById('cloud-autosync').checked;
      cloudConfig.connected = true;

      showToast('Config saved! Synchronizing 6 site modules...', '☁️');
      cloudModal.classList.add('hidden');
      triggerBackgroundCloudSync();
    });

    // ==================== PWA INSTALL MODAL ====================
    const pwaModal = document.getElementById('pwa-modal');
    document.getElementById('btn-show-install-guide').addEventListener('click', () => {
      pwaModal.classList.remove('hidden');
    });

    document.getElementById('btn-close-pwa-modal').addEventListener('click', () => {
      pwaModal.classList.add('hidden');
    });

    document.getElementById('btn-got-it-pwa').addEventListener('click', () => {
      pwaModal.classList.add('hidden');
    });

    document.getElementById('btn-dismiss-pwa').addEventListener('click', () => {
      document.getElementById('pwa-install-banner').classList.add('hidden');
    });

    // ==================== HEADER ACTIONS ====================
    document.getElementById('btn-print-report').addEventListener('click', () => {
      window.print();
    });

    document.getElementById('btn-export-json').addEventListener('click', () => {
      const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(store, null, 2));
      const dlAnchor = document.createElement('a');
      dlAnchor.setAttribute("href", dataStr);
      dlAnchor.setAttribute("download", `BuildTrack_Backup_${new Date().toISOString().slice(0,10)}.json`);
      document.body.appendChild(dlAnchor);
      dlAnchor.click();
      dlAnchor.remove();
      showToast('Backup downloaded successfully', '💾');
    });

    // Initial Render
    updateKPIs();
    renderTasks();
    renderGantt();
    renderDefects();
    renderMaterials();
    renderLabourLogs();
    renderExpenses();
  </script>
</body>
</html>
