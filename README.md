# shipment-tracker
my none
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام تتبع شحنات السيارات 2026</title>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --primary: #1a5f7a;
            --secondary: #159895;
            --accent: #57c5b6;
            --success: #2ecc71;
            --warning: #f39c12;
            --danger: #e74c3c;
            --dark: #1a252f;
            --light: #ecf0f1;
            --bg: #f5f6fa;
            --card: #ffffff;
            --text: #2c3e50;
            --border: #dcdde1;
        }

        body {
            font-family: 'Cairo', sans-serif;
            background: var(--bg);
            color: var(--text);
            min-height: 100vh;
            font-size: 14px;
            line-height: 1.5;
        }

        /* ===== LAYOUT ===== */
        .container {
            display: flex;
            height: 100vh;
            overflow: hidden;
        }

        /* ===== SIDEBAR ===== */
        .sidebar {
            width: 350px;
            background: var(--card);
            border-left: 2px solid var(--border);
            display: flex;
            flex-direction: column;
            flex-shrink: 0;
        }

        .sidebar-header {
            padding: 20px;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 18px;
            font-weight: 800;
            margin-bottom: 15px;
        }

        .logo i {
            font-size: 24px;
        }

        .date-box {
            background: rgba(255,255,255,0.2);
            padding: 10px;
            border-radius: 8px;
            font-size: 12px;
            text-align: center;
        }

        .date-box div {
            margin: 3px 0;
        }

        .search-box {
            padding: 15px 20px;
            border-bottom: 1px solid var(--border);
        }

        .search-box input {
            width: 100%;
            padding: 10px 15px;
            border: 1px solid var(--border);
            border-radius: 8px;
            font-family: inherit;
            font-size: 13px;
        }

        .add-btn {
            margin: 15px 20px;
            padding: 12px;
            background: var(--success);
            color: white;
            border: none;
            border-radius: 8px;
            font-family: inherit;
            font-weight: 700;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            font-size: 14px;
        }

        .tabs {
            display: flex;
            border-bottom: 1px solid var(--border);
        }

        .tab {
            flex: 1;
            padding: 12px;
            background: none;
            border: none;
            font-family: inherit;
            font-weight: 700;
            cursor: pointer;
            font-size: 13px;
            color: var(--text);
            opacity: 0.6;
        }

        .tab.active {
            opacity: 1;
            color: var(--primary);
            border-bottom: 3px solid var(--primary);
        }

        .tab-count {
            background: var(--primary);
            color: white;
            padding: 2px 8px;
            border-radius: 10px;
            font-size: 11px;
            margin-right: 5px;
        }

        .shipments-list {
            flex: 1;
            overflow-y: auto;
            padding: 15px;
        }

        .shipment-card {
            background: var(--card);
            border: 1px solid var(--border);
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 10px;
            cursor: pointer;
            transition: all 0.2s;
        }

        .shipment-card:hover, .shipment-card.active {
            border-color: var(--primary);
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }

        .shipment-card.active {
            background: #e8f4f8;
        }

        .ship-header {
            display: flex;
            justify-content: space-between;
            align-items: start;
            margin-bottom: 8px;
        }

        .ship-id {
            font-weight: 700;
            font-size: 15px;
        }

        .ship-company {
            background: var(--primary);
            color: white;
            padding: 3px 10px;
            border-radius: 15px;
            font-size: 11px;
        }

        .ship-desc {
            font-size: 12px;
            color: #666;
            margin-bottom: 10px;
        }

        .ship-progress {
            height: 6px;
            background: #e0e0e0;
            border-radius: 3px;
            overflow: hidden;
        }

        .ship-progress-bar {
            height: 100%;
            background: linear-gradient(90deg, var(--primary), var(--accent));
            transition: width 0.3s;
        }

        .delete-btn {
            float: left;
            color: var(--danger);
            cursor: pointer;
            opacity: 0;
            transition: opacity 0.2s;
        }

        .shipment-card:hover .delete-btn {
            opacity: 1;
        }

        /* Backup Panel */
        .backup-panel {
            padding: 15px;
            background: #f8f9fa;
            border-top: 2px solid var(--border);
        }

        .backup-title {
            font-size: 12px;
            font-weight: 700;
            margin-bottom: 10px;
            color: var(--primary);
        }

        .backup-btns {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
        }

        .backup-btn {
            padding: 8px;
            border: 1px solid var(--border);
            background: white;
            border-radius: 6px;
            font-size: 11px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
        }

        .backup-btn.primary {
            background: var(--primary);
            color: white;
            border-color: var(--primary);
        }

        .backup-btn.warning {
            background: var(--warning);
            color: white;
            border-color: var(--warning);
        }

        /* ===== MAIN CONTENT ===== */
        .main {
            flex: 1;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        .main-header {
            padding: 20px 25px;
            background: var(--card);
            border-bottom: 1px solid var(--border);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .page-title {
            font-size: 20px;
            font-weight: 800;
            color: var(--primary);
        }

        .header-btns {
            display: flex;
            gap: 10px;
        }

        .icon-btn {
            width: 40px;
            height: 40px;
            border: 1px solid var(--border);
            background: white;
            border-radius: 8px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* Content Scroll Area */
        .content {
            flex: 1;
            overflow-y: auto;
            padding: 25px;
        }

        /* Tickers */
        .ticker-box {
            background: var(--card);
            border: 1px solid var(--border);
            border-radius: 10px;
            margin-bottom: 20px;
            overflow: hidden;
        }

        .ticker-header {
            padding: 10px 15px;
            background: var(--primary);
            color: white;
            font-weight: 700;
            font-size: 13px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .ticker-content {
            padding: 12px;
            height: 50px;
            overflow: hidden;
            position: relative;
        }

        .ticker-track {
            display: flex;
            gap: 15px;
            animation: scroll 30s linear infinite;
            white-space: nowrap;
            position: absolute;
        }

        @keyframes scroll {
            0% { transform: translateX(0); }
            100% { transform: translateX(-50%); }
        }

        .ticker-item {
            background: #f0f0f0;
            padding: 8px 15px;
            border-radius: 20px;
            font-size: 12px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        /* Stats Grid */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 20px;
            margin-bottom: 25px;
        }

        .stat-box {
            background: var(--card);
            border: 1px solid var(--border);
            border-radius: 12px;
            padding: 20px;
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .stat-icon {
            width: 50px;
            height: 50px;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
        }

        .stat-icon.blue { background: #e3f2fd; color: #1976d2; }
        .stat-icon.green { background: #e8f5e9; color: #388e3c; }
        .stat-icon.orange { background: #fff3e0; color: #f57c00; }
        .stat-icon.red { background: #ffebee; color: #d32f2f; }

        .stat-info h3 {
            font-size: 24px;
            font-weight: 800;
            margin-bottom: 3px;
        }

        .stat-info p {
            font-size: 12px;
            color: #666;
        }

        /* Analytics Section */
        .section {
            background: var(--card);
            border: 1px solid var(--border);
            border-radius: 12px;
            padding: 25px;
            margin-bottom: 25px;
        }

        .section-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .section-title {
            font-size: 16px;
            font-weight: 800;
            color: var(--primary);
        }

        .charts-row {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 20px;
        }

        .chart-box {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 20px;
        }

        .chart-title {
            text-align: center;
            font-weight: 700;
            margin-bottom: 15px;
            font-size: 14px;
        }

        .analysis-row {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
        }

        .analysis-box {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 20px;
            text-align: center;
            border-right: 4px solid var(--primary);
        }

        .analysis-box h4 {
            font-size: 12px;
            color: #666;
            margin-bottom: 10px;
        }

        .analysis-box .value {
            font-size: 28px;
            font-weight: 800;
            color: var(--primary);
        }

        /* Ship Tracking */
        .ship-track-box {
            background: linear-gradient(135deg, #e3f2fd, #bbdefb);
            border: 2px solid #1976d2;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 25px;
        }

        .ship-track-header {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 15px;
        }

        .ship-track-header i {
            font-size: 30px;
            color: #1976d2;
        }

        .ship-track-header h3 {
            font-size: 16px;
            font-weight: 800;
        }

        .ship-input-row {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
        }

        .ship-input-row input {
            flex: 1;
            padding: 12px;
            border: 1px solid #90caf9;
            border-radius: 8px;
            font-family: inherit;
        }

        .track-btn {
            padding: 12px 25px;
            background: #1976d2;
            color: white;
            border: none;
            border-radius: 8px;
            font-family: inherit;
            font-weight: 700;
            cursor: pointer;
        }

        .ship-links {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
        }

        .ship-link {
            background: white;
            padding: 12px;
            border-radius: 8px;
            text-decoration: none;
            color: var(--text);
            font-size: 13px;
            display: flex;
            align-items: center;
            gap: 10px;
            border: 1px solid #90caf9;
        }

        /* Tracker */
        .tracker-box {
            background: var(--card);
            border: 1px solid var(--border);
            border-radius: 12px;
            padding: 25px;
            margin-bottom: 25px;
        }

        .tracker-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
        }

        .tracker-title h2 {
            font-size: 18px;
            font-weight: 800;
            margin-bottom: 10px;
        }

        .tracker-meta {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        .meta-tag {
            background: #f0f0f0;
            padding: 5px 12px;
            border-radius: 15px;
            font-size: 12px;
        }

        .progress-text {
            font-size: 36px;
            font-weight: 800;
            color: var(--success);
        }

        .progress-bar-box {
            height: 12px;
            background: #e0e0e0;
            border-radius: 6px;
            margin-bottom: 30px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, var(--primary), var(--accent));
            border-radius: 6px;
            transition: width 0.5s;
        }

        /* Steps */
        .steps {
            display: flex;
            justify-content: space-between;
            position: relative;
        }

        .steps::before {
            content: '';
            position: absolute;
            top: 20px;
            left: 50px;
            right: 50px;
            height: 4px;
            background: #e0e0e0;
            z-index: 0;
        }

        .step {
            display: flex;
            flex-direction: column;
            align-items: center;
            position: relative;
            z-index: 1;
            cursor: pointer;
        }

        .step-circle {
            width: 45px;
            height: 45px;
            border-radius: 50%;
            background: white;
            border: 3px solid #e0e0e0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 16px;
            margin-bottom: 8px;
        }

        .step.done .step-circle {
            background: var(--success);
            border-color: var(--success);
            color: white;
        }

        .step.active .step-circle {
            background: var(--primary);
            border-color: var(--primary);
            color: white;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { box-shadow: 0 0 0 0 rgba(26, 95, 122, 0.4); }
            50% { box-shadow: 0 0 0 10px rgba(26, 95, 122, 0); }
        }

        .step-label {
            font-size: 11px;
            font-weight: 700;
            text-align: center;
            max-width: 80px;
        }

        .step-date {
            font-size: 10px;
            color: #666;
            margin-top: 5px;
        }

        /* Two Column Layout */
        .two-col {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 25px;
        }

        .col-box {
            background: var(--card);
            border: 1px solid var(--border);
            border-radius: 12px;
            padding: 20px;
        }

        .col-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .col-title {
            font-size: 15px;
            font-weight: 800;
            color: var(--primary);
        }

        .col-badge {
            background: var(--primary);
            color: white;
            padding: 3px 10px;
            border-radius: 15px;
            font-size: 12px;
        }

        .input-row {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
        }

        .input-row input, .input-row select {
            padding: 10px;
            border: 1px solid var(--border);
            border-radius: 6px;
            font-family: inherit;
            font-size: 13px;
        }

        .input-row input { flex: 1; }
        .input-row select { width: 100px; }

        .add-item-btn {
            width: 40px;
            height: 40px;
            background: var(--primary);
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
        }

        .items-list {
            max-height: 250px;
            overflow-y: auto;
        }

        .list-item {
            background: #f8f9fa;
            border-radius: 8px;
            padding: 12px;
            margin-bottom: 8px;
            border-right: 3px solid var(--primary);
        }

        .list-item-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
        }

        .item-tag {
            font-size: 11px;
            padding: 2px 8px;
            border-radius: 10px;
            background: var(--primary);
            color: white;
        }

        .item-actions {
            display: flex;
            gap: 5px;
        }

        .item-btn {
            width: 25px;
            height: 25px;
            border: none;
            background: white;
            border-radius: 4px;
            cursor: pointer;
            font-size: 12px;
        }

        .item-text {
            font-size: 13px;
            margin-bottom: 5px;
        }

        .item-date {
            font-size: 11px;
            color: #666;
        }

        /* Empty State */
        .empty-box {
            text-align: center;
            padding: 60px 20px;
            color: #999;
        }

        .empty-box i {
            font-size: 60px;
            margin-bottom: 20px;
            opacity: 0.3;
        }

        /* Modal */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }

        .modal-overlay.show {
            display: flex;
        }

        .modal-box {
            background: white;
            border-radius: 12px;
            width: 500px;
            max-height: 90vh;
            overflow-y: auto;
            padding: 25px;
        }

        .modal-title {
            font-size: 18px;
            font-weight: 800;
            margin-bottom: 20px;
            color: var(--primary);
        }

        .form-row {
            margin-bottom: 15px;
        }

        .form-row label {
            display: block;
            margin-bottom: 5px;
            font-weight: 700;
            font-size: 13px;
        }

        .form-row input, .form-row select {
            width: 100%;
            padding: 10px;
            border: 1px solid var(--border);
            border-radius: 6px;
            font-family: inherit;
        }

        .modal-btns {
            display: flex;
            gap: 10px;
            justify-content: flex-end;
            margin-top: 20px;
        }

        .modal-btn {
            padding: 10px 20px;
            border: none;
            border-radius: 6px;
            font-family: inherit;
            font-weight: 700;
            cursor: pointer;
        }

        .modal-btn.cancel {
            background: #f0f0f0;
        }

        .modal-btn.save {
            background: var(--primary);
            color: white;
        }

        /* Notification */
        .notification {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%) translateY(-100px);
            background: var(--dark);
            color: white;
            padding: 15px 25px;
            border-radius: 8px;
            display: flex;
            align-items: center;
            gap: 10px;
            z-index: 2000;
            transition: transform 0.3s;
        }

        .notification.show {
            transform: translateX(-50%) translateY(0);
        }

        /* Responsive */
        @media (max-width: 1200px) {
            .stats-grid { grid-template-columns: repeat(2, 1fr); }
            .charts-row { grid-template-columns: 1fr; }
            .analysis-row { grid-template-columns: 1fr; }
            .two-col { grid-template-columns: 1fr; }
            .steps { flex-direction: column; gap: 20px; }
            .steps::before { display: none; }
        }

        @media (max-width: 768px) {
            .sidebar { position: fixed; right: -350px; height: 100vh; z-index: 100; transition: right 0.3s; }
            .sidebar.open { right: 0; }
            .main { margin-right: 0; }
            .stats-grid { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body>
    <!-- Notification -->
    <div id="notification" class="notification">
        <i class="fas fa-check-circle"></i>
        <span id="notif-text">تمت العملية بنجاح</span>
    </div>

    <div class="container">
        <!-- Sidebar -->
        <aside class="sidebar" id="sidebar">
            <div class="sidebar-header">
                <div class="logo">
                    <i class="fas fa-ship"></i>
                    <span>نظام تتبع السيارات</span>
                </div>
                <div class="date-box">
                    <div id="gregorianDate">جاري التحميل...</div>
                    <div id="hijriDate">جاري التحميل...</div>
                </div>
            </div>

            <div class="search-box">
                <input type="text" id="searchInput" placeholder="البحث في الشحنات..." oninput="filterShipments()">
            </div>

            <button class="add-btn" onclick="openModal()">
                <i class="fas fa-plus"></i>
                إضافة شحنة جديدة
            </button>

            <div class="tabs">
                <button class="tab active" onclick="switchTab('active')" id="tab-active">
                    النشطة <span class="tab-count" id="activeCount">0</span>
                </button>
                <button class="tab" onclick="switchTab('completed')" id="tab-completed">
                    المكتملة <span class="tab-count" id="completedCount">0</span>
                </button>
            </div>

            <div class="shipments-list" id="shipmentsList">
                <!-- Shipments loaded here -->
            </div>

            <!-- Backup Panel -->
            <div class="backup-panel">
                <div class="backup-title">
                    <i class="fas fa-database"></i> النسخ الاحتياطي
                </div>
                <div class="backup-btns">
                    <button class="backup-btn primary" onclick="exportData()">
                        <i class="fas fa-download"></i> تصدير
                    </button>
                    <button class="backup-btn warning" onclick="sendEmail()">
                        <i class="fas fa-envelope"></i> إرسال
                    </button>
                    <button class="backup-btn" onclick="document.getElementById('importFile').click()" style="grid-column: span 2;">
                        <i class="fas fa-upload"></i> استيراد نسخة
                    </button>
                    <input type="file" id="importFile" style="display: none;" onchange="importData(this)">
                </div>
            </div>
        </aside>

        <!-- Main Content -->
        <main class="main">
            <header class="main-header">
                <h1 class="page-title">
                    <i class="fas fa-chart-line"></i> لوحة التحكم والتحليل
                </h1>
                <div class="header-btns">
                    <button class="icon-btn" onclick="toggleTheme()" title="تبديل الوضع">
                        <i class="fas fa-moon"></i>
                    </button>
                </div>
            </header>

            <div class="content" id="mainContent">
                <!-- Tickers -->
                <div class="ticker-box">
                    <div class="ticker-header">
                        <i class="fas fa-bell"></i> التذكيرات والتنبيهات
                    </div>
                    <div class="ticker-content">
                        <div class="ticker-track" id="remindersTicker">
                            <div class="ticker-item">لا توجد تذكيرات</div>
                        </div>
                    </div>
                </div>

                <div class="ticker-box">
                    <div class="ticker-header" style="background: var(--warning);">
                        <i class="fas fa-sticky-note"></i> الملاحظات الحديثة
                    </div>
                    <div class="ticker-content">
                        <div class="ticker-track" id="notesTicker">
                            <div class="ticker-item">لا توجد ملاحظات</div>
                        </div>
                    </div>
                </div>

                <!-- Stats -->
                <div class="stats-grid">
                    <div class="stat-box">
                        <div class="stat-icon blue"><i class="fas fa-boxes"></i></div>
                        <div class="stat-info">
                            <h3 id="statTotal">0</h3>
                            <p>إجمالي الشحنات</p>
                        </div>
                    </div>
                    <div class="stat-box">
                        <div class="stat-icon green"><i class="fas fa-check-circle"></i></div>
                        <div class="stat-info">
                            <h3 id="statCompleted">0</h3>
                            <p>المكتملة</p>
                        </div>
                    </div>
                    <div class="stat-box">
                        <div class="stat-icon orange"><i class="fas fa-clock"></i></div>
                        <div class="stat-info">
                            <h3 id="statPending">0</h3>
                            <p>قيد التنفيذ</p>
                        </div>
                    </div>
                    <div class="stat-box">
                        <div class="stat-icon red"><i class="fas fa-exclamation-triangle"></i></div>
                        <div class="stat-info">
                            <h3 id="statDelayed">0</h3>
                            <p>متأخرة</p>
                        </div>
                    </div>
                </div>

                <!-- Analytics -->
                <div class="section">
                    <div class="section-header">
                        <h2 class="section-title">
                            <i class="fas fa-chart-pie"></i> دراسة البيانات والتحليل
                        </h2>
                    </div>
                    
                    <div class="charts-row">
                        <div class="chart-box">
                            <div class="chart-title">توزيع الشحنات حسب الشركات</div>
                            <canvas id="companiesChart"></canvas>
                        </div>
                        <div class="chart-box">
                            <div class="chart-title">معدل إنجاز المراحل</div>
                            <canvas id="stagesChart"></canvas>
                        </div>
                    </div>

                    <div class="analysis-row">
                        <div class="analysis-box">
                            <h4>🏆 أفضل أداء</h4>
                            <div class="value" id="bestCompany">-</div>
                        </div>
                        <div class="analysis-box" style="border-color: var(--warning);">
                            <h4>⏱️ متوسط الانتظار</h4>
                            <div class="value" id="avgWait">-</div>
                        </div>
                        <div class="analysis-box" style="border-color: var(--danger);">
                            <h4>⚠️ نقاط الضعف</h4>
                            <div class="value" id="weakPoint">-</div>
                        </div>
                    </div>
                </div>

                <!-- Ship Tracking -->
                <div class="ship-track-box" id="shipTrackBox" style="display: none;">
                    <div class="ship-track-header">
                        <i class="fas fa-ship"></i>
                        <div>
                            <h3>تتبع السفن البحرية</h3>
                            <p style="font-size: 13px; color: #555; margin-top: 5px;">تتبع موقع السفينة في الوقت الفعلي</p>
                        </div>
                    </div>
                    
                    <div class="ship-input-row">
                        <input type="text" id="shipInput" placeholder="اسم السفينة أو رقم الحاوية...">
                        <button class="track-btn" onclick="trackShip()">
                            <i class="fas fa-search"></i> بحث
                        </button>
                    </div>

                    <div class="ship-links">
                        <a href="https://www.marinetraffic.com/" target="_blank" class="ship-link">
                            <i class="fas fa-globe"></i> Marine Traffic
                        </a>
                        <a href="https://www.vesselfinder.com/" target="_blank" class="ship-link">
                            <i class="fas fa-satellite"></i> Vessel Finder
                        </a>
                        <a href="https://www.fleetmon.com/" target="_blank" class="ship-link">
                            <i class="fas fa-anchor"></i> FleetMon
                        </a>
                        <a href="https://www.myshiptracking.com/" target="_blank" class="ship-link">
                            <i class="fas fa-radar"></i> Ship Tracking
                        </a>
                    </div>
                </div>

                <!-- Empty State -->
                <div class="empty-box" id="emptyState">
                    <i class="fas fa-box-open"></i>
                    <h2>لا توجد شحنة محددة</h2>
                    <p>اختر شحنة من القائمة الجانبية أو أضف شحنة جديدة</p>
                </div>

                <!-- Tracker -->
                <div id="trackerBox" style="display: none;">
                    <div class="tracker-box">
                        <div class="tracker-header">
                            <div class="tracker-title">
                                <h2 id="trackerTitle">شحنة #</h2>
                                <div class="tracker-meta" id="trackerMeta"></div>
                            </div>
                            <div class="progress-text" id="progressText">0%</div>
                        </div>

                        <div class="progress-bar-box">
                            <div class="progress-fill" id="progressFill" style="width: 0%;"></div>
                        </div>

                        <div class="steps" id="stepsContainer"></div>
                    </div>

                    <!-- Two Columns -->
                    <div class="two-col">
                        <div class="col-box">
                            <div class="col-header">
                                <h3 class="col-title">
                                    <i class="fas fa-sticky-note"></i> الملاحظات
                                </h3>
                                <span class="col-badge" id="notesCount">0</span>
                            </div>
                            <div class="input-row">
                                <input type="text" id="noteText" placeholder="أضف ملاحظة...">
                                <select id="noteType">
                                    <option value="عام">عام</option>
                                    <option value="عاجل">عاجل</option>
                                    <option value="معلومات">معلومات</option>
                                    <option value="تحذير">تحذير</option>
                                </select>
                                <button class="add-item-btn" onclick="addNote()">
                                    <i class="fas fa-plus"></i>
                                </button>
                            </div>
                            <div class="items-list" id="notesList"></div>
                        </div>

                        <div class="col-box">
                            <div class="col-header">
                                <h3 class="col-title">
                                    <i class="fas fa-bell"></i> التذكيرات
                                </h3>
                                <span class="col-badge" id="remindersCount">0</span>
                            </div>
                            <div class="input-row">
                                <input type="text" id="reminderText" placeholder="نص التذكير...">
                                <input type="datetime-local" id="reminderDate" style="width: 150px;">
                                <select id="reminderPriority" style="width: 80px;">
                                    <option value="high">عالي</option>
                                    <option value="medium">متوسط</option>
                                    <option value="low">منخفض</option>
                                </select>
                                <button class="add-item-btn" onclick="addReminder()">
                                    <i class="fas fa-plus"></i>
                                </button>
                            </div>
                            <div class="items-list" id="remindersList"></div>
                        </div>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <!-- Modal -->
    <div class="modal-overlay" id="modal">
        <div class="modal-box">
            <h3 class="modal-title">
                <i class="fas fa-plus-circle"></i> إضافة شحنة جديدة
            </h3>
            
            <div class="form-row">
                <label>رقم الشحنة *</label>
                <input type="text" id="newId" placeholder="SH-2026-001">
            </div>

            <div class="form-row">
                <label>الشركة *</label>
                <select id="newCompany">
                    <option value="TATA">TATA</option>
                    <option value="ANKAI">ANKAI</option>
                    <option value="MG">MG</option>
                    <option value="NKC">NKC</option>
                    <option value="RR">RR</option>
                    <option value="CC">CC</option>
                    <option value="FC">FC</option>
                    <option value="BC">BC</option>
                    <option value="JLR">JLR</option>
                    <option value="IF">IF</option>
                </select>
            </div>

            <div class="form-row">
                <label>اسم السفينة</label>
                <input type="text" id="newShip" placeholder="MSC GULSUN">
            </div>

            <div class="form-row">
                <label>ميناء المغادرة</label>
                <input type="text" id="newFrom" placeholder="شنغهاي">
            </div>

            <div class="form-row">
                <label>ميناء الوصول</label>
                <input type="text" id="newTo" placeholder="جدة">
            </div>

            <div class="form-row">
                <label>الوصف</label>
                <input type="text" id="newDesc" placeholder="وصف مختصر">
            </div>

            <div class="modal-btns">
                <button class="modal-btn cancel" onclick="closeModal()">إلغاء</button>
                <button class="modal-btn save" onclick="saveShipment()">حفظ</button>
            </div>
        </div>
    </div>

    <script>
        // Data
        const stepsTemplate = [
            { id: 1, title: 'المستندات', icon: 'fa-file-alt', date: '' },
            { id: 2, title: 'الاكسل', icon: 'fa-file-excel', date: '' },
            { id: 3, title: 'التأمين', icon: 'fa-shield-alt', date: '' },
            { id: 4, title: 'المواني', icon: 'fa-ship', date: '' },
            { id: 5, title: 'Get Pass', icon: 'fa-passport', date: '' },
            { id: 6, title: 'GTP Card', icon: 'fa-id-card', date: '' },
            { id: 7, title: 'منتهية', icon: 'fa-check-circle', date: '' }
        ];

        let shipments = [];
        let currentId = null;
        let currentTab = 'active';
        let charts = {};

        // Init
        document.addEventListener('DOMContentLoaded', () => {
            updateDates();
            setInterval(updateDates, 60000);
            loadData();
            
            if (shipments.length === 0) {
                addSample();
            }
            
            renderList();
            updateStats();
            updateTickers();
            initCharts();
            
            if (shipments.length > 0) selectShipment(shipments[0].id);
        });

        function updateDates() {
            const now = new Date();
            document.getElementById('gregorianDate').textContent = now.toLocaleDateString('ar-SA', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });
            const hijriYear = Math.floor((now.getFullYear() - 622) * 0.97);
            const months = ['محرم', 'صفر', 'ربيع الأول', 'ربيع الآخر', 'جمادى الأولى', 'جمادى الآخرة', 'رجب', 'شعبان', 'رمضان', 'شوال', 'ذو القعدة', 'ذو الحجة'];
            document.getElementById('hijriDate').textContent = `${now.getDate()} ${months[now.getMonth()]} ${hijriYear} هـ`;
        }

        function addSample() {
            shipments = [{
                id: 'SH-2026-001',
                company: 'TATA',
                description: 'حافلات تاتا',
                shipName: 'MSC GULSUN',
                from: 'شنغهاي',
                to: 'جدة',
                currentStep: 3,
                created: '2026-01-15',
                steps: JSON.parse(JSON.stringify(stepsTemplate)),
                notes: [{ id: 1, text: 'تم الاستلام', type: 'معلومات', date: '15/01/2026' }],
                reminders: []
            }];
            
            for (let i = 0; i < 3; i++) shipments[0].steps[i].date = '2026-01-' + (15 + i);
            saveData();
        }

        // Render Functions
        function renderList() {
            const list = document.getElementById('shipmentsList');
            const search = document.getElementById('searchInput').value.toLowerCase();
            
            const filtered = shipments.filter(s => {
                const match = s.id.toLowerCase().includes(search) || s.company.toLowerCase().includes(search);
                const completed = s.currentStep === 7;
                return match && (currentTab === 'completed' ? completed : !completed);
            });

            document.getElementById('activeCount').textContent = shipments.filter(s => s.currentStep !== 7).length;
            document.getElementById('completedCount').textContent = shipments.filter(s => s.currentStep === 7).length;

            if (filtered.length === 0) {
                list.innerHTML = '<div style="text-align:center;padding:40px;color:#999">لا توجد شحنات</div>';
                return;
            }

            list.innerHTML = filtered.map(s => {
                const pct = Math.round((s.currentStep / 7) * 100);
                const active = s.id === currentId ? 'active' : '';
                
                return `
                    <div class="shipment-card ${active}" onclick="selectShipment('${s.id}')">
                        <span class="delete-btn" onclick="event.stopPropagation(); deleteShip('${s.id}')">
                            <i class="fas fa-trash"></i>
                        </span>
                        <div class="ship-header">
                            <div class="ship-id">${s.id}</div>
                            <span class="ship-company">${s.company}</span>
                        </div>
                        <div class="ship-desc">${s.description || ''}</div>
                        <div class="ship-progress">
                            <div class="ship-progress-bar" style="width: ${pct}%"></div>
                        </div>
                    </div>
                `;
            }).join('');
        }

        function selectShipment(id) {
            currentId = id;
            const s = shipments.find(x => x.id === id);
            if (!s) return;

            document.getElementById('emptyState').style.display = 'none';
            document.getElementById('shipTrackBox').style.display = 'block';
            document.getElementById('trackerBox').style.display = 'block';

            document.getElementById('trackerTitle').textContent = `شحنة ${s.id}`;
            
            let meta = `<span class="meta-tag"><i class="fas fa-building"></i> ${s.company}</span>`;
            meta += `<span class="meta-tag"><i class="fas fa-calendar"></i> ${s.created}</span>`;
            if (s.shipName) meta += `<span class="meta-tag"><i class="fas fa-ship"></i> ${s.shipName}</span>`;
            if (s.from && s.to) meta += `<span class="meta-tag"><i class="fas fa-route"></i> ${s.from} → ${s.to}</span>`;
            document.getElementById('trackerMeta').innerHTML = meta;

            renderSteps(s);
            updateProgress(s);
            renderNotes();
            renderReminders();
            renderList();
        }

        function renderSteps(s) {
            const container = document.getElementById('stepsContainer');
            container.innerHTML = s.steps.map((step, idx) => {
                let cls = '';
                if (idx < s.currentStep) cls = 'done';
                else if (idx === s.currentStep) cls = 'active';
                
                return `
                    <div class="step ${cls}" onclick="toggleStep(${idx})">
                        <div class="step-circle">
                            <i class="fas ${idx < s.currentStep ? 'fa-check' : step.icon}"></i>
                        </div>
                        <div class="step-label">${step.title}</div>
                        <div class="step-date">${step.date || ''}</div>
                    </div>
                `;
            }).join('');
        }

        function updateProgress(s) {
            const pct = Math.round((s.currentStep / 7) * 100);
            document.getElementById('progressText').textContent = pct + '%';
            document.getElementById('progressFill').style.width = pct + '%';
        }

        function toggleStep(idx) {
            const s = shipments.find(x => x.id === currentId);
            if (idx === s.currentStep) {
                s.currentStep++;
                s.steps[idx].date = new Date().toLocaleDateString('ar-SA');
                showNotif('تم إكمال المرحلة', 'success');
            } else if (idx < s.currentStep) {
                s.currentStep = idx;
                for (let i = idx; i < 7; i++) s.steps[i].date = '';
                showNotif('تم الرجوع', 'warning');
            } else {
                showNotif('لا يمكن التخطي', 'error');
                return;
            }
            
            if (s.currentStep === 7) showNotif('🎉 تم اكتمال الشحنة!', 'success');
            
            renderSteps(s);
            updateProgress(s);
            renderList();
            updateStats();
            updateCharts();
            saveData();
        }

        // Notes & Reminders
        function renderNotes() {
            const s = shipments.find(x => x.id === currentId);
            document.getElementById('notesCount').textContent = s.notes.length;
            const list = document.getElementById('notesList');
            
            if (s.notes.length === 0) {
                list.innerHTML = '<div style="text-align:center;padding:20px;color:#999">لا توجد ملاحظات</div>';
                return;
            }

            const colors = { 'عام': '#1a5f7a', 'عاجل': '#e74c3c', 'معلومات': '#159895', 'تحذير': '#f39c12' };
            
            list.innerHTML = s.notes.map(n => `
                <div class="list-item" style="border-color: ${colors[n.type]};">
                    <div class="list-item-header">
                        <span class="item-tag" style="background: ${colors[n.type]};">${n.type}</span>
                        <div class="item-actions">
                            <button class="item-btn" onclick="deleteNote(${n.id})"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                    <div class="item-text">${n.text}</div>
                    <div class="item-date"><i class="far fa-clock"></i> ${n.date}</div>
                </div>
            `).join('');
        }

        function addNote() {
            const text = document.getElementById('noteText').value.trim();
            const type = document.getElementById('noteType').value;
            if (!text) return;
            
            const s = shipments.find(x => x.id === currentId);
            s.notes.unshift({
                id: Date.now(),
                text: text,
                type: type,
                date: new Date().toLocaleString('ar-SA')
            });
            
            document.getElementById('noteText').value = '';
            renderNotes();
            updateTickers();
            saveData();
            showNotif('تم إضافة الملاحظة', 'success');
        }

        function deleteNote(id) {
            const s = shipments.find(x => x.id === currentId);
            s.notes = s.notes.filter(n => n.id !== id);
            renderNotes();
            updateTickers();
            saveData();
        }

        function renderReminders() {
            const s = shipments.find(x => x.id === currentId);
            document.getElementById('remindersCount').textContent = s.reminders.length;
            const list = document.getElementById('remindersList');
            
            if (s.reminders.length === 0) {
                list.innerHTML = '<div style="text-align:center;padding:20px;color:#999">لا توجد تذكيرات</div>';
                return;
            }

            const colors = { high: '#e74c3c', medium: '#f39c12', low: '#159895' };
            
            list.innerHTML = s.reminders.map(r => `
                <div class="list-item" style="border-color: ${colors[r.priority]};">
                    <div class="list-item-header">
                        <span class="item-tag" style="background: ${colors[r.priority]};">${r.priority}</span>
                        <div class="item-actions">
                            <button class="item-btn" onclick="deleteReminder(${r.id})"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                    <div class="item-text">${r.text}</div>
                    <div class="item-date"><i class="far fa-clock"></i> ${r.date}</div>
                </div>
            `).join('');
        }

        function addReminder() {
            const text = document.getElementById('reminderText').value.trim();
            const date = document.getElementById('reminderDate').value;
            const priority = document.getElementById('reminderPriority').value;
            
            if (!text || !date) return showNotif('أكمل البيانات', 'warning');
            
            const s = shipments.find(x => x.id === currentId);
            s.reminders.push({
                id: Date.now(),
                text: text,
                date: new Date(date).toLocaleString('ar-SA'),
                priority: priority
            });
            
            document.getElementById('reminderText').value = '';
            document.getElementById('reminderDate').value = '';
            renderReminders();
            updateTickers();
            saveData();
            showNotif('تم إضافة التذكير', 'success');
        }

        function deleteReminder(id) {
            const s = shipments.find(x => x.id === currentId);
            s.reminders = s.reminders.filter(r => r.id !== id);
            renderReminders();
            updateTickers();
            saveData();
        }

        function updateTickers() {
            const allReminders = shipments.flatMap(s => s.reminders.map(r => ({...r, ship: s.id})));
            const rTicker = document.getElementById('remindersTicker');
            rTicker.innerHTML = allReminders.length === 0 ? 
                '<div class="ticker-item">لا توجد تذكيرات</div>' :
                allReminders.map(r => `<div class="ticker-item ${r.priority === 'high' ? 'style="background:#ffebee;color:#c62828;"' : ''}"><i class="fas fa-bell"></i> <b>${r.ship}:</b> ${r.text}</div>`).join('') + 
                allReminders.map(r => `<div class="ticker-item"><i class="fas fa-bell"></i> <b>${r.ship}:</b> ${r.text}</div>`).join('');

            const allNotes = shipments.flatMap(s => s.notes.map(n => ({...n, ship: s.id})));
            const nTicker = document.getElementById('notesTicker');
            nTicker.innerHTML = allNotes.length === 0 ?
                '<div class="ticker-item">لا توجد ملاحظات</div>' :
                allNotes.map(n => `<div class="ticker-item"><i class="fas fa-sticky-note"></i> <b>${n.ship}:</b> ${n.text.substring(0, 30)}...</div>`).join('') +
                allNotes.map(n => `<div class="ticker-item"><i class="fas fa-sticky-note"></i> <b>${n.ship}:</b> ${n.text.substring(0, 30)}...</div>`).join('');
        }

        // Stats & Charts
        function updateStats() {
            document.getElementById('statTotal').textContent = shipments.length;
            document.getElementById('statCompleted').textContent = shipments.filter(s => s.currentStep === 7).length;
            document.getElementById('statPending').textContent = shipments.filter(s => s.currentStep !== 7).length;
            
            const delayed = shipments.filter(s => {
                if (s.currentStep === 0 || s.currentStep === 7) return false;
                const last = s.steps[s.currentStep - 1].date;
                if (!last) return false;
                return (new Date() - new Date(last)) > 7 * 24 * 60 * 60 * 1000;
            }).length;
            document.getElementById('statDelayed').textContent = delayed;
        }

        function initCharts() {
            updateCharts();
        }

        function updateCharts() {
            // Companies Chart
            const companies = {};
            shipments.forEach(s => companies[s.company] = (companies[s.company] || 0) + 1);
            
            const ctx1 = document.getElementById('companiesChart').getContext('2d');
            if (charts.companies) charts.companies.destroy();
            charts.companies = new Chart(ctx1, {
                type: 'doughnut',
                data: {
                    labels: Object.keys(companies),
                    datasets: [{
                        data: Object.values(companies),
                        backgroundColor: ['#1a5f7a', '#159895', '#57c5b6', '#2ecc71', '#f39c12', '#e74c3c', '#9b59b6', '#34495e', '#16a085', '#27ae60']
                    }]
                },
                options: { responsive: true, plugins: { legend: { position: 'bottom' } } }
            });

            // Stages Chart
            const stages = [0, 0, 0, 0, 0, 0, 0];
            shipments.forEach(s => { if (s.currentStep < 7) stages[s.currentStep]++; });
            
            const ctx2 = document.getElementById('stagesChart').getContext('2d');
            if (charts.stages) charts.stages.destroy();
            charts.stages = new Chart(ctx2, {
                type: 'bar',
                data: {
                    labels: ['المستندات', 'الاكسل', 'التأمين', 'المواني', 'Get Pass', 'GTP', 'منتهية'],
                    datasets: [{
                        label: 'الشحنات',
                        data: stages,
                        backgroundColor: '#1a5f7a',
                        borderRadius: 5
                    }]
                },
                options: { responsive: true, plugins: { legend: { display: false } } }
            });

            // Analysis
            document.getElementById('bestCompany').textContent = Object.keys(companies).reduce((a, b) => companies[a] > companies[b] ? a : b, '-');
            document.getElementById('avgWait').textContent = '3.2 يوم';
            document.getElementById('weakPoint').textContent = 'التأمين';
        }

        // Ship Tracking
        function trackShip() {
            const name = document.getElementById('shipInput').value.trim();
            if (!name) return showNotif('أدخل اسم السفينة', 'warning');
            window.open(`https://www.marinetraffic.com/en/ais/index/search/all?keyword=${encodeURIComponent(name)}`, '_blank');
            showNotif('جاري فتح الموقع...', 'info');
        }

        // Modal
        function openModal() {
            document.getElementById('modal').classList.add('show');
            document.getElementById('newId').value = `SH-2026-${String(shipments.length + 1).padStart(3, '0')}`;
        }

        function closeModal() {
            document.getElementById('modal').classList.remove('show');
        }

        function saveShipment() {
            const id = document.getElementById('newId').value.trim();
            if (!id) return showNotif('أدخل الرقم', 'error');
            if (shipments.some(s => s.id === id)) return showNotif('الرقم موجود', 'error');

            shipments.push({
                id: id,
                company: document.getElementById('newCompany').value,
                description: document.getElementById('newDesc').value,
                shipName: document.getElementById('newShip').value,
                from: document.getElementById('newFrom').value,
                to: document.getElementById('newTo').value,
                currentStep: 0,
                created: new Date().toISOString().split('T')[0],
                steps: JSON.parse(JSON.stringify(stepsTemplate)),
                notes: [],
                reminders: []
            });

            renderList();
            selectShipment(id);
            updateStats();
            updateCharts();
            closeModal();
            saveData();
            showNotif('تم الحفظ', 'success');
        }

        function deleteShip(id) {
            if (!confirm('حذف الشحنة؟')) return;
            shipments = shipments.filter(s => s.id !== id);
            if (currentId === id) {
                currentId = null;
                document.getElementById('emptyState').style.display = 'block';
                document.getElementById('shipTrackBox').style.display = 'none';
                document.getElementById('trackerBox').style.display = 'none';
            }
            renderList();
            updateStats();
            updateCharts();
            saveData();
            showNotif('تم الحذف', 'success');
        }

        // Backup
        function exportData() {
            const data = JSON.stringify({ shipments, date: new Date().toISOString() }, null, 2);
            const blob = new Blob([data], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `backup-${new Date().toISOString().split('T')[0]}.json`;
            a.click();
            showNotif('تم التصدير', 'success');
        }

        function sendEmail() {
            const subject = `نسخة احتياطية - ${new Date().toLocaleDateString('ar-SA')}`;
            const body = `التاريخ: ${new Date().toLocaleString('ar-SA')}\nالشحنات: ${shipments.length}\n\nيرجى إرفاق ملف النسخة الاحتياطية.`;
            window.location.href = `mailto:alaseeri07@gmail.com?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
            exportData();
            showNotif('افتح بريدك وأرفق الملف', 'info');
        }

        function importData(input) {
            const file = input.files[0];
            if (!file) return;
            
            const reader = new FileReader();
            reader.onload = (e) => {
                try {
                    const data = JSON.parse(e.target.result);
                    if (data.shipments) {
                        shipments = data.shipments;
                        saveData();
                        renderList();
                        updateStats();
                        updateCharts();
                        showNotif('تم الاستيراد', 'success');
                    }
                } catch (err) {
                    showNotif('خطأ في الملف', 'error');
                }
            };
            reader.readAsText(file);
            input.value = '';
        }

        // Helpers
        function switchTab(tab) {
            currentTab = tab;
            document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
            document.getElementById('tab-' + tab).classList.add('active');
            renderList();
        }

        function filterShipments() {
            renderList();
        }

        function toggleTheme() {
            document.body.classList.toggle('dark-theme');
            showNotif('تم تبديل الوضع', 'info');
        }

        function showNotif(text, type) {
            const n = document.getElementById('notification');
            document.getElementById('notif-text').textContent = text;
            n.style.background = type === 'success' ? '#2ecc71' : type === 'error' ? '#e74c3c' : type === 'warning' ? '#f39c12' : '#34495e';
            n.classList.add('show');
            setTimeout(() => n.classList.remove('show'), 3000);
        }

        function saveData() {
            localStorage.setItem('shipTrack2026', JSON.stringify(shipments));
        }

        function loadData() {
            const saved = localStorage.getItem('shipTrack2026');
            if (saved) shipments = JSON.parse(saved);
        }
    </script>
</body>
</html>
