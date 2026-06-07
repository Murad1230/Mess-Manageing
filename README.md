<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>আমাদের মেস - অ্যাডভান্সড ড্যাশবোর্ড</title>
    
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,400;14..32,500;14..32,600;14..32,700&family=Hind+Siliguri:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        :root {
            --primary: #6366f1; --primary-dark: #4f46e5; --primary-light: #a5b4fc;
            --secondary: #8b5cf6; --success: #10b981; --danger: #ef4444; --warning: #f59e0b;
            --dark: #0f172a; --gray: #64748b; --light: #f8fafc;
            --card-bg: rgba(255, 255, 255, 0.95); --glass-border: rgba(255, 255, 255, 0.3);
            --shadow-sm: 0 10px 25px -5px rgba(0, 0, 0, 0.05), 0 8px 10px -6px rgba(0, 0, 0, 0.02);
            --shadow-md: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.02);
            --shadow-lg: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
            --radius: 24px; --bg-gradient: linear-gradient(135deg, #e0e7ff 0%, #f1f5f9 100%);
        }
        body.dark {
            --primary: #818cf8; --primary-dark: #6366f1; --card-bg: rgba(30, 41, 59, 0.95);
            --dark: #f1f5f9; --gray: #94a3b8; --light: #1e293b;
            --bg-gradient: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            --glass-border: rgba(255, 255, 255, 0.1);
        }
        body { font-family: 'Inter', 'Hind Siliguri', sans-serif; background: var(--bg-gradient); color: var(--dark); padding-bottom: 90px; min-height: 100vh; transition: background 0.3s, color 0.2s; }
        .container { max-width: 1400px; margin: 0 auto; padding: 20px; }
        .theme-toggle { position: fixed; bottom: 100px; right: 20px; background: var(--card-bg); border-radius: 50px; padding: 10px 15px; box-shadow: var(--shadow-md); z-index: 1000; cursor: pointer; backdrop-filter: blur(8px); border: 1px solid var(--glass-border); }
        .theme-toggle i { font-size: 1.2rem; color: var(--primary); }
        .test-data-btn { background: var(--warning); color: #1e293b; border: none; border-radius: 40px; padding: 8px 16px; font-weight: bold; cursor: pointer; margin-left: 10px; }
        .login-card { max-width: 450px; margin: 80px auto; background: var(--card-bg); backdrop-filter: blur(10px); border-radius: var(--radius); padding: 40px; box-shadow: var(--shadow-lg); border: 1px solid var(--glass-border); text-align: center; }
        .login-card h2 { background: linear-gradient(135deg, var(--primary), var(--secondary)); -webkit-background-clip: text; background-clip: text; color: transparent; font-size: 28px; margin-bottom: 30px; }
        .header { background: var(--card-bg); backdrop-filter: blur(12px); border-radius: 28px; padding: 16px 24px; display: flex; justify-content: space-between; align-items: center; margin-bottom: 24px; box-shadow: var(--shadow-sm); border: 1px solid var(--glass-border); }
        .welcome-text { font-weight: 700; font-size: 1.2rem; background: linear-gradient(135deg, var(--primary-dark), var(--secondary)); -webkit-background-clip: text; background-clip: text; color: transparent; }
        .logout-btn { background: rgba(239, 68, 68, 0.15); border: none; padding: 8px 20px; border-radius: 40px; color: var(--danger); font-weight: 600; cursor: pointer; transition: 0.2s; }
        .logout-btn:hover { background: var(--danger); color: white; }
        .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); gap: 20px; margin-bottom: 30px; }
        .stat-card { background: var(--card-bg); backdrop-filter: blur(8px); border-radius: 28px; padding: 20px; display: flex; align-items: center; gap: 16px; transition: all 0.3s cubic-bezier(0.2, 0.9, 0.4, 1.1); border: 1px solid var(--glass-border); box-shadow: var(--shadow-sm); cursor: pointer; }
        .stat-card:hover { transform: translateY(-6px); box-shadow: var(--shadow-md); }
        .stat-icon { width: 56px; height: 56px; background: linear-gradient(135deg, #e0e7ff, #c7d2fe); border-radius: 20px; display: flex; align-items: center; justify-content: center; font-size: 26px; color: var(--primary-dark); }
        .stat-info h4 { font-size: 13px; font-weight: 600; text-transform: uppercase; color: var(--gray); letter-spacing: 0.5px; }
        .stat-info h2 { font-size: 28px; font-weight: 700; margin-top: 6px; color: var(--dark); }
        .tabs { display: flex; gap: 12px; background: var(--card-bg); backdrop-filter: blur(8px); padding: 8px; border-radius: 60px; margin-bottom: 28px; overflow-x: auto; scrollbar-width: none; }
        .tab { padding: 10px 22px; border-radius: 40px; font-weight: 600; color: var(--gray); cursor: pointer; transition: 0.2s; white-space: nowrap; display: flex; align-items: center; gap: 8px; }
        .tab.active { background: var(--primary); color: white; box-shadow: 0 4px 12px rgba(99, 102, 241, 0.3); }
        .content-card { background: var(--card-bg); backdrop-filter: blur(8px); border-radius: 28px; padding: 24px; margin-bottom: 24px; border: 1px solid var(--glass-border); box-shadow: var(--shadow-sm); }
        .content-card h3 { font-size: 1.3rem; font-weight: 600; margin-bottom: 20px; display: flex; align-items: center; gap: 10px; color: var(--dark); }
        .meal-row { background: rgba(248, 250, 252, 0.8); border-radius: 20px; padding: 14px 18px; margin-bottom: 12px; display: flex; justify-content: space-between; align-items: center; }
        .toggle-btn { padding: 8px 24px; border-radius: 40px; font-weight: bold; border: none; cursor: pointer; transition: 0.2s; }
        .btn-on { background: var(--success); color: white; }
        .btn-off { background: var(--danger); color: white; }
        .table-container { overflow-x: auto; border-radius: 20px; }
        .modern-table { width: 100%; border-collapse: collapse; background: white; border-radius: 20px; overflow: hidden; }
        .modern-table th { background: #f1f5f9; padding: 14px 12px; font-weight: 600; color: var(--dark); border-bottom: 2px solid #e2e8f0; }
        .modern-table td { padding: 12px; border-bottom: 1px solid #f1f5f9; text-align:center;}
        .sheet-table { width: 100%; border-collapse: collapse; border: 1px solid #e2e8f0; border-radius: 16px; overflow: hidden; }
        .sheet-table th, .sheet-table td { border: 1px solid #e2e8f0; padding: 8px 6px; text-align: center; font-size: 12px; }
        .day-past { background-color: #dcfce7; color: #065f46; }
        .day-today { background-color: #fef08a; color: #854d0e; font-weight: bold; border: 2px solid #ca8a04; }
        .day-future { background-color: #f3f4f6; color: #9ca3af; }
        .editable-cell { cursor: pointer; transition: 0.1s; }
        .editable-cell:hover { filter: brightness(0.96); border: 2px dashed var(--primary); }
        input, select, textarea { width: 100%; padding: 12px 16px; border: 1px solid #e2e8f0; border-radius: 16px; font-family: inherit; transition: 0.2s; background: white; }
        input:focus, select:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1); }
        .btn-save { background: var(--primary); color: white; border: none; padding: 12px; border-radius: 40px; font-weight: 600; cursor: pointer; width: 100%; transition: 0.2s; margin-top: 10px;}
        .btn-save:hover { background: var(--primary-dark); transform: scale(0.98); }
        .accordion-header { background: rgba(248, 250, 252, 0.9); padding: 16px 20px; border-radius: 20px; margin-bottom: 12px; cursor: pointer; display: flex; justify-content: space-between; font-weight: 600; }
        .accordion-content { padding: 20px; background: white; border-radius: 20px; margin-top: -8px; margin-bottom: 16px; display: none; }
        .switch { position: relative; display: inline-block; width: 60px; height: 34px; }
        .switch input { opacity: 0; width: 0; height: 0; }
        .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: #ccc; transition: 0.4s; border-radius: 34px; }
        .slider:before { position: absolute; content: ""; height: 26px; width: 26px; left: 4px; bottom: 4px; background-color: white; transition: 0.4s; border-radius: 50%; }
        input:checked + .slider { background-color: var(--primary); }
        input:checked + .slider:before { transform: translateX(26px); }
        .rule-item { display: flex; align-items: center; justify-content: space-between; margin-bottom: 15px; }
        .hidden { display: none !important; }

        @media (max-width: 768px) {
            .tabs { position: fixed; bottom: 0; left: 0; width: 100%; z-index: 1000; border-radius: 0; margin: 0; padding: 8px 12px; gap: 8px; justify-content: space-around; background: var(--card-bg); border-top: 1px solid var(--glass-border); }
            .tab { flex: 1; justify-content: center; padding: 8px 12px; font-size: 12px; }
            .stats-grid { grid-template-columns: 1fr 1fr; gap: 12px; }
            .container { padding-bottom: 70px; }
        }
    </style>
</head>
<body>
<div class="container">
    <div class="theme-toggle" onclick="toggleTheme()"><i class="fas fa-moon"></i> <span id="themeText">ডার্ক</span></div>

    <div id="loginScreen" class="login-card">
        <h2><i class="fas fa-utensils"></i> মেস লগইন</h2>
        <select id="loginUser" style="margin-bottom: 15px;"></select>
        <input type="password" id="userPin" placeholder="পিন কোড" maxlength="6" style="text-align:center; letter-spacing:5px;">
        <button class="btn-save" id="loginBtn" style="margin-top:20px;">প্রবেশ করুন</button>
    </div>

    <div id="mainDashboard" class="hidden">
        <div class="header">
            <span class="welcome-text" id="welcomeText"><i class="fas fa-user-circle"></i> লোডিং...</span>
            <button class="logout-btn" onclick="location.reload()"><i class="fas fa-sign-out-alt"></i> লগআউট</button>
        </div>

        <div class="stats-grid">
            <div class="stat-card" id="balanceCard"><div class="stat-icon"><i class="fas fa-wallet"></i></div><div class="stat-info"><h4>আমার ব্যালেন্স</h4><h2 id="topBalance">৳ ০</h2></div></div>
            <div class="stat-card" id="myMealsCard"><div class="stat-icon"><i class="fas fa-utensils"></i></div><div class="stat-info"><h4>আমার মোট মিল</h4><h2 id="myTotalMeals">০ টি</h2></div></div>
            <div class="stat-card"><div class="stat-icon"><i class="fas fa-chart-line"></i></div><div class="stat-info"><h4>মেসের মোট মিল</h4><h2 id="topMeals">০ টি</h2></div></div>
            <div class="stat-card"><div class="stat-icon"><i class="fas fa-home"></i></div><div class="stat-info"><h4>ফিক্সড খরচ</h4><h2 id="topFixedCost">৳ ০</h2></div></div>
            <div class="stat-card"><div class="stat-icon"><i class="fas fa-users"></i></div><div class="stat-info"><h4>আজকের মোট মিল</h4><h2 id="topTodayTotal">0.0 টি</h2><div style="font-size:12px;">🌅 <span id="t-b">0</span> | 🌞 <span id="t-l">0</span> | 🌙 <span id="t-r">0</span></div></div></div>
        </div>

        <div class="tabs">
            <div class="tab active" onclick="switchTab('dashboard')"><i class="fas fa-home"></i> হোম</div>
            <div class="tab" onclick="switchTab('menu')"><i class="fas fa-utensils"></i> মেনু</div>
            <div class="tab" onclick="switchTab('sheet')"><i class="fas fa-table"></i> শিট</div>
            <div class="tab" onclick="switchTab('ledger')"><i class="fas fa-chart-bar"></i> লেজার</div>
            <div class="tab" onclick="switchTab('statement')"><i class="fas fa-receipt"></i> হিসাব</div>
            <div class="tab" onclick="switchTab('history')"><i class="fas fa-history"></i> ইতিহাস</div>
            <div id="managerTab" class="tab hidden" onclick="switchTab('manager')"><i class="fas fa-tasks"></i> ম্যানেজ</div>
            <div id="settingsTab" class="tab hidden" onclick="switchTab('settings')"><i class="fas fa-cog"></i> সেটিং</div>
        </div>

        <div id="sec-dashboard">
            <div id="dutyBox" class="content-card" style="background:#e0e7ff; display:none;"><h4><i class="fas fa-shopping-basket"></i> আজকের বাজারের দায়িত্ব</h4><p id="dutyText"></p></div>
            <div class="content-card" style="background:#fffbeb;"><h4><i class="fas fa-bullhorn"></i> নোটিশ</h4><p id="noticeText">লোড হচ্ছে...</p></div>
            <div class="content-card"><h3><i class="fas fa-calendar-check"></i> মিল শিডিউলার</h3>
                <input type="date" id="mealDate" onchange="loadMeals()">
                <div class="meal-row"><span>🌅 সকাল <small>(লক: আগের দিন <span id="lockB">--:--</span>)</small></span><button id="btnB" class="toggle-btn btn-on" onclick="toggleMeal('breakfast')">ON</button></div>
                <div class="meal-row"><span>🌞 দুপুর <small>(লক: <span id="lockL">--:--</span>)</small></span><button id="btnL" class="toggle-btn btn-on" onclick="toggleMeal('lunch')">ON</button></div>
                <div class="meal-row"><span>🌙 রাত <small>(লক: <span id="lockD">--:--</span>)</small></span><button id="btnD" class="toggle-btn btn-on" onclick="toggleMeal('dinner')">ON</button></div>
                <div style="margin-top:15px;"><select id="guestTime"><option value="breakfast">সকাল</option><option value="lunch">দুপুর</option><option value="dinner">রাত</option></select>
                <div style="display:flex; gap:8px; margin-top:8px;"><input type="number" id="guestCount" placeholder="সংখ্যা" value="0"><button class="btn-save" style="width:auto; margin-top:0;" onclick="saveGuest()">গেস্ট সেভ</button></div></div>
            </div>
            <div class="content-card" id="pinChangeBox"><h3><i class="fas fa-key"></i> নিজের পিন পরিবর্তন</h3><div style="display:flex; gap:8px;"><input type="password" id="oldPin" placeholder="পুরাতন পিন"><input type="password" id="newPin" placeholder="নতুন পিন"><button class="btn-save" style="width:auto; margin-top:0;" onclick="changeOwnPin()">আপডেট</button></div></div>
        </div>

        <div id="sec-menu" class="hidden"><div class="content-card"><h3>৩ দিনের ফুড মেনু</h3><div id="menuContainer" class="menu-grid"></div></div></div>

        <div id="sec-sheet" class="hidden"><div class="content-card"><h3>মাসিক মিল শিট (এডিট করতে ক্লিক)</h3><div class="table-container"><table id="sheetTable" class="sheet-table"><thead id="sheetHead"></thead><tbody id="sheetBody"></tbody></table></div></div></div>

        <div id="sec-ledger" class="hidden">
            <div class="content-card">
                <div style="display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap:10px; margin-bottom:15px;">
                    <h3 style="margin:0;">মেস লেজার রিপোর্ট</h3>
                    <div style="display:flex; gap:10px;">
                        <input type="month" id="ledgerMonth" value="" style="width:150px;">
                        <button class="btn-save" style="width:auto; background:#dc2626; margin-top:0;" onclick="downloadPDF()"><i class="fas fa-file-pdf"></i> PDF</button>
                    </div>
                </div>
                <div class="table-container"><table class="modern-table"><thead><tr><th>নাম</th><th>মোট মিল</th><th>মিল খরচ</th><th>ফিক্সড</th><th>জমা</th><th>ব্যালেন্স</th></tr></thead><tbody id="ledgerBody"></tbody></table></div>
            </div>
        </div>

        <div id="sec-statement" class="hidden"><div class="content-card"><h3>বিস্তারিত মাসিক হিসাব</h3><div id="statementSelectDiv" class="hidden"><select id="statementUser" onchange="renderStatement()"></select></div><div class="table-container"><table class="modern-table"><thead><tr><th>তারিখ</th><th>সকাল</th><th>দুপুর</th><th>রাত</th><th>গেস্ট</th><th>জমা</th></tr></thead><tbody id="statementBody"></tbody></table></div></div></div>

        <div id="sec-history" class="hidden"><div class="content-card"><h3>বাজার ইতিহাস</h3><div class="table-container"><table class="modern-table"><thead><tr><th>তারিখ</th><th>বিবরণ</th><th>টাকা</th></tr></thead><tbody id="histB"></tbody></table></div></div><div class="content-card"><h3>চালের ইতিহাস</h3><div class="table-container"><table class="modern-table"><thead><tr><th>তারিখ</th><th>বিবরণ</th><th>টাকা</th></tr></thead><tbody id="histR"></tbody></table></div></div><div class="content-card"><h3>ফিক্সড খরচ ইতিহাস</h3><div class="table-container"><table class="modern-table"><thead><tr><th>তারিখ</th><th>বিবরণ</th><th>টাকা</th></tr></thead><tbody id="histF"></tbody></table></div></div></div>

        <div id="sec-manager" class="hidden"><div class="content-card"><h3>ম্যানেজার কন্ট্রোল প্যানেল</h3>
            <div class="accordion-section"><div class="accordion-header" onclick="toggleAccordion(this)">⏰ ১. কাস্টম টাইম-লক <i class="fas fa-chevron-down"></i></div><div class="accordion-content"><div style="display:grid; grid-template-columns:1fr 1fr 1fr; gap:10px;"><div><label>সকাল</label><input type="time" id="lockTimeB" value="22:00"></div><div><label>দুপুর</label><input type="time" id="lockTimeL" value="10:00"></div><div><label>রাত</label><input type="time" id="lockTimeD" value="14:00"></div></div><button class="btn-save" onclick="saveLockTimes()">সেভ করুন</button></div></div>
            <div class="accordion-section"><div class="accordion-header" onclick="toggleAccordion(this)">📅 ২. বাজার ডিউটি <i class="fas fa-chevron-down"></i></div><div class="accordion-content"><input type="date" id="dutyDate"><div style="display:grid; gap:10px; margin-top:10px;"><select id="dutyMem1"></select><select id="dutyMem2"><option value="">-- কেউ না --</option></select></div><button class="btn-save" onclick="saveDuty()">সেভ করুন</button></div></div>
            <div class="accordion-section"><div class="accordion-header" onclick="toggleAccordion(this)">⚖️ ৩. মিল নিয়ম (সুইচ) <i class="fas fa-chevron-down"></i></div><div class="accordion-content">
                <div class="rule-item"><span>লো-মিল পেনাল্টি (৫ এর কম খেলে ২০ বিল)</span> <label class="switch"><input type="checkbox" id="rule1Toggle" checked><span class="slider"></span></label></div>
                <div class="rule-item"><span>বেস মিল চার্জ (৫০ এর কম খেলে ৫০ বিল)</span> <label class="switch"><input type="checkbox" id="rule2Toggle" checked><span class="slider"></span></label></div>
                <button class="btn-save" onclick="saveRulesToggle()">নিয়ম সেভ করুন</button>
            </div></div>
            <div class="accordion-section"><div class="accordion-header" onclick="toggleAccordion(this)">💰 ৪. খরচ ও জমা <i class="fas fa-chevron-down"></i></div><div class="accordion-content"><select id="entryType" onchange="toggleEntryFields()"><option value="deposit">মেম্বার জমা</option><option value="bazaar">তরকারি বাজার</option><option value="rice">চালের খরচ</option><option value="fixed">ফিক্সড খরচ</option></select><div id="div-deposit"><select id="depMember"></select><input type="number" id="depAmount" placeholder="জমার পরিমাণ"></div><div id="div-bazaar" class="hidden"><select id="shopper1"></select><select id="shopper2"><option value="">-- কেউ না --</option></select><input type="number" id="bazAmount" placeholder="বাজার খরচ"></div><div id="div-rice" class="hidden"><input type="text" id="riceNote" placeholder="বিবরণ"><input type="number" id="riceAmount" placeholder="চালের দাম"></div><div id="div-fixed" class="hidden"><input type="text" id="fixedNote" placeholder="বিবরণ"><input type="number" id="fixedAmount" placeholder="টাকার পরিমাণ"></div><button class="btn-save" onclick="saveEntry()">সেভ করুন</button></div></div>
            <div class="accordion-section"><div class="accordion-header" onclick="toggleAccordion(this)">📢 ৫. নোটিশ ও মেনু <i class="fas fa-chevron-down"></i></div><div class="accordion-content"><textarea id="mgrNotice" rows="2" placeholder="নোটিশ লিখুন"></textarea><div id="menuInputs"></div><button class="btn-save" onclick="saveNoticeAndMenu()">সেভ করুন</button></div></div>
            <div class="accordion-section"><div class="accordion-header" onclick="toggleAccordion(this)">⚡ ৬. সুপার অ্যাডমিন <i class="fas fa-chevron-down"></i></div><div class="accordion-content"><select id="superType" onchange="toggleSuperEdit()"><option value="">-- নির্বাচন --</option><option value="cost">মোট খরচ এডিট</option><option value="deposit">মেম্বার জমা এডিট</option><option value="meal">মিল এডিট</option><option value="reset">সব ডাটা রিসেট</option></select><div id="costDiv" class="hidden"><input type="number" id="editBaz" placeholder="বাজার"><input type="number" id="editRice" placeholder="চাল"><input type="number" id="editFix" placeholder="ফিক্সড"><button class="btn-save" style="background:var(--danger);" onclick="editCost()">আপডেট</button></div><div id="depositDiv" class="hidden"><select id="editMemDeposit"></select><p>বর্তমান জমা: ৳<span id="curDep">0</span></p><input type="number" id="newDeposit" placeholder="সঠিক জমা"><button class="btn-save" style="background:var(--danger);" onclick="editDeposit()">আপডেট</button></div><div id="mealDiv" class="hidden"><input type="date" id="editMealDate"><select id="editMealMem"></select><button class="btn-save" onclick="loadMealEdit()">লোড</button><div id="mealEditPanel"></div></div><div id="resetDiv" class="hidden"><p style="color:red;">সতর্ক: সব ডাটা মুছে যাবে!</p><button class="btn-save" style="background:var(--danger);" onclick="resetMonth()">রিসেট করুন</button></div></div></div>
            <div class="accordion-section"><div class="accordion-header" onclick="toggleAccordion(this)">🔐 ৭. ম্যানেজার মাস্টার পিন <i class="fas fa-chevron-down"></i></div><div class="accordion-content"><div style="display:grid; gap:10px;"><input type="password" id="oldMasterPin" placeholder="পুরাতন পিন"><input type="password" id="newMasterPin" placeholder="নতুন পিন"></div><button class="btn-save" onclick="changeMasterPin()">আপডেট করুন</button></div></div>
            <button class="test-data-btn" style="margin-top:20px; width:100%;" onclick="loadTestData()"><i class="fas fa-database"></i> টেস্ট ডাটা লোড করুন</button>
        </div></div>

        <div id="sec-settings" class="hidden"><div class="content-card"><h3>নতুন মেম্বার</h3><input type="text" id="newMemberName"><button class="btn-save" onclick="addMember()">যোগ করুন</button></div><div class="content-card"><h3>মেম্বার এডিট</h3><div id="memberEditList"></div><button class="btn-save" onclick="saveAllMembers()">সব সেভ করুন</button></div></div>

        <div class="content-card" style="background: linear-gradient(135deg, var(--primary), var(--secondary)); color:white; display: flex; justify-content: space-around; text-align: center;">
            <div><small>মিল রেট</small><br><strong id="liveRate">০.০০</strong></div>
            <div><small>মোট মিল খরচ</small><br><strong id="liveTotalCost">০</strong></div>
            <div><small>ক্যাশ ইন হ্যান্ড</small><br><strong id="liveCash">০</strong></div>
        </div>
    </div>
</div>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
    import { getDatabase, ref, set, update, get, onValue, remove } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

    const firebaseConfig = {
        apiKey: "AIzaSyBedhJXBSoxLvdweMlRcZ30Vt3dUPYpw20",
        authDomain: "mess-new.firebaseapp.com",
        databaseURL: "https://mess-new-default-rtdb.asia-southeast1.firebasedatabase.app",
        projectId: "mess-new"
    };
    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    let members = [], memberNames = {}, memberPins = {}, memberExceptions = {};
    let isManager = false, currentUser = "";
    let customLock = { breakfast: "22:00", lunch: "10:00", dinner: "14:00" };
    let messRules = { t1: 5, p1: 20, t2: 50, p2: 50 };
    let monthData = {};
    let selectedLedgerMonth = "";
    
    const today = new Date();
    const currentMonthKey = `${today.getFullYear()}-${String(today.getMonth()+1).padStart(2,'0')}`;
    const dateKey = `${today.getFullYear()}-${String(today.getMonth()+1).padStart(2,'0')}-${String(today.getDate()).padStart(2,'0')}`;
    
    if(document.getElementById('mealDate')) document.getElementById('mealDate').value = dateKey;
    if(document.getElementById('dutyDate')) document.getElementById('dutyDate').value = dateKey;
    if(document.getElementById('ledgerMonth')) document.getElementById('ledgerMonth').value = currentMonthKey;

    function showMsg(msg, type='success') { Swal.fire({ text: msg, icon: type, timer: 1500, showConfirmButton: false }); }

    // গ্লোবাল এন্টার বাটন লিসেনার
    document.addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            let ae = document.activeElement;
            if (ae.tagName === 'INPUT' || ae.tagName === 'SELECT') {
                if (ae.id === 'userPin') {
                    document.getElementById('loginBtn').click();
                } else {
                    let btn = ae.closest('.content-card')?.querySelector('.btn-save') || ae.closest('.accordion-content')?.querySelector('.btn-save');
                    if (btn) btn.click();
                }
            }
        }
    });

    window.toggleTheme = () => {
        document.body.classList.toggle('dark');
        const themeText = document.getElementById('themeText');
        if(document.body.classList.contains('dark')) themeText.innerText = 'লাইট';
        else themeText.innerText = 'ডার্ক';
    };

    window.loadTestData = async () => {
        if(!isManager) return;
        await set(ref(db, `records/${currentMonthKey}/totalBazaarCost`), 5000);
        await set(ref(db, `records/${currentMonthKey}/totalRiceCost`), 3000);
        await set(ref(db, `records/${currentMonthKey}/totalFixedCost`), 2000);
        await set(ref(db, `records/${currentMonthKey}/${dateKey}/Member1/deposit`), 5000);
        await set(ref(db, `records/${currentMonthKey}/${dateKey}/Member1/breakfast`), "ON");
        await set(ref(db, `records/${currentMonthKey}/${dateKey}/Member1/lunch`), "ON");
        await set(ref(db, `records/${currentMonthKey}/${dateKey}/Member1/dinner`), "ON");
        showMsg("টেস্ট ডাটা লোড হয়েছে");
    };

    document.getElementById('loginBtn').onclick = () => {
        const user = document.getElementById('loginUser').value;
        const pin = document.getElementById('userPin').value;
        get(ref(db, 'records/managerPin')).then(snap => {
            if(pin === (snap.val() || "0000")) { isManager = true; startApp(); }
            else {
                get(ref(db, `records/memberPins/${user}`)).then(s => {
                    if(pin === (s.val() || "1234")) { currentUser = user; isManager = false; startApp(); }
                    else showMsg("ভুল পিন!", "error");
                });
            }
        });
    };

    function startApp() {
        document.getElementById('loginScreen').classList.add('hidden');
        document.getElementById('mainDashboard').classList.remove('hidden');
        if(isManager) {
            document.getElementById('managerTab').classList.remove('hidden');
            document.getElementById('settingsTab').classList.remove('hidden');
            document.getElementById('statementSelectDiv').classList.remove('hidden');
            document.getElementById('welcomeText').innerHTML = '<i class="fas fa-user-shield"></i> ম্যানেজার';
            document.getElementById('myMealsCard').style.display = 'none';
            document.getElementById('pinChangeBox').style.display = 'none';
        } else {
            document.getElementById('welcomeText').innerHTML = `<i class="fas fa-user"></i> ${memberNames[currentUser] || 'মেম্বার'}`;
            document.getElementById('myMealsCard').style.display = 'flex';
            document.getElementById('statementSelectDiv').classList.add('hidden');
        }
        loadData();
    }

    function loadData() {
        onValue(ref(db, 'records/memberConfig'), snap => { memberNames = snap.val() || {}; members = Object.keys(memberNames); updateDropdowns(); renderMemberList(); renderAll(); });
        onValue(ref(db, 'records/memberPins'), snap => { memberPins = snap.val() || {}; });
        onValue(ref(db, 'records/memberExceptions'), snap => { memberExceptions = snap.val() || {}; renderAll(); });
        onValue(ref(db, 'records/customLockTimes'), snap => { if(snap.val()) customLock = snap.val(); updateLockUI(); renderAll(); });
        onValue(ref(db, `records/${currentMonthKey}/messRules`), snap => { if(snap.val()) messRules = snap.val(); updateRuleUI(); renderAll(); });
        onValue(ref(db, `records/${currentMonthKey}/noticeBoard`), snap => { if(document.getElementById('noticeText')) document.getElementById('noticeText').innerText = snap.val() || "নোটিশ নেই"; if(isManager && document.getElementById('mgrNotice')) document.getElementById('mgrNotice').value = snap.val() || ""; });
        onValue(ref(db, 'records/threeDayMenuRoutine'), snap => renderMenu(snap.val()));
        onValue(ref(db, `records/${currentMonthKey}`), snap => { monthData = snap.val() || {}; renderAll(); });
        onValue(ref(db, `records/bazaarDuty/${dateKey}`), snap => {
            let d = snap.val();
            if(d?.s1 && document.getElementById('dutyBox')) { document.getElementById('dutyText').innerHTML = `${memberNames[d.s1]} ${d.s2?'ও '+memberNames[d.s2]:''}`; document.getElementById('dutyBox').style.display = 'block'; }
            else if(document.getElementById('dutyBox')) document.getElementById('dutyBox').style.display = 'none';
        });
        onValue(ref(db, 'records/bazaarDuty'), snap => {
            let duties = snap.val() || {}; let html = '';
            Object.keys(duties).sort((a,b)=>b.localeCompare(a)).forEach(date => {
                let d = duties[date];
                if(d?.s1) html += `<tr><td>${date.split('-').reverse().join('/')}</td><td>${memberNames[d.s1]}</td><td>${d.s2?memberNames[d.s2]:'-'}</td></tr>`;
            });
            if(document.getElementById('bazaarDutyRoutineTableBody')) document.getElementById('bazaarDutyRoutineTableBody').innerHTML = html;
        });

        if(document.getElementById('ledgerMonth')) {
            document.getElementById('ledgerMonth').addEventListener('change', (e) => {
                selectedLedgerMonth = e.target.value;
                if(selectedLedgerMonth !== currentMonthKey) renderHistoricalLedger(selectedLedgerMonth);
                else renderAll();
            });
        }
    }

    // রিয়েল-টাইম ক্যালকুলেশন লজিক (ভবিষ্যতের দিনে ০ দেখাবে, আজকের দিন টাইম-লক চেক করবে)
    function getRealTimeMealCount(dayStr, uData) {
        let b = 0, l = 0, d = 0;
        let bOn = uData.breakfast !== "OFF";
        let lOn = uData.lunch !== "OFF";
        let dOn = uData.dinner !== "OFF";
        
        let now = new Date();
        let target = new Date(dayStr);
        let todayDate = new Date(today.getFullYear(), today.getMonth(), today.getDate());
        let targetDate = new Date(target.getFullYear(), target.getMonth(), target.getDate());

        if (targetDate < todayDate) {
            b = bOn ? 0.5 : 0; l = lOn ? 1.0 : 0; d = dOn ? 1.0 : 0;
        } else if (targetDate.getTime() === todayDate.getTime()) {
            b = bOn ? 0.5 : 0; // সকালের মিল আগের রাতেই লক হয়ে যায় ধরে নেওয়া হলো
            let [lH, lM] = customLock.lunch.split(':').map(Number);
            let [dH, dM] = customLock.dinner.split(':').map(Number);
            let lLock = new Date(); lLock.setHours(lH, lM, 0, 0);
            let dLock = new Date(); dLock.setHours(dH, dM, 0, 0);
            
            if(now >= lLock && lOn) l = 1.0;
            if(now >= dLock && dOn) d = 1.0;
        }
        return { b, l, d, total: b + l + d };
    }

    function renderAll() {
        if(!monthData || !members.length) return;
        let bazaar = parseFloat(monthData.totalBazaarCost) || 0;
        let rice = parseFloat(monthData.totalRiceCost) || 0;
        let fixed = parseFloat(monthData.totalFixedCost) || 0;
        let mealCost = bazaar + rice;
        
        let ownMeals = {}, guestMeals = {}, deposits = {};
        members.forEach(m => { ownMeals[m]=0; guestMeals[m]=0; deposits[m]=0; });
        let tb = 0, tl = 0, tr = 0;

        // মাসের মোট দিন বের করে ৩০/৩১ দিনের লুপ
        let year = currentMonthKey.split('-')[0];
        let month = currentMonthKey.split('-')[1];
        let daysInMonth = new Date(year, month, 0).getDate();
        let allDays = [];
        for(let i=1; i<=daysInMonth; i++) {
            allDays.push(`${year}-${month}-${String(i).padStart(2,'0')}`);
        }

        // সব দিনের ডেপোজিট যোগ করা
        Object.keys(monthData).forEach(day => {
            if(day.includes('-') && day.length>8) {
                members.forEach(m => { deposits[m] += parseFloat((monthData[day][m] || {}).deposit) || 0; });
            }
        });

        // পুরো মাসের (১-৩০) রিয়েল টাইম মিল গণনা
        allDays.forEach(dayStr => {
            members.forEach(m => {
                let u = monthData[dayStr]?.[m] || {};
                let meals = getRealTimeMealCount(dayStr, u);
                ownMeals[m] += meals.total;

                let gb = 0, gl = 0, gd = 0;
                let now = new Date();
                let targetDate = new Date(dayStr);
                let todayDate = new Date(today.getFullYear(), today.getMonth(), today.getDate());

                if (targetDate < todayDate) {
                    gb = (parseFloat(u.guest_breakfast)||0)*0.5; gl = (parseFloat(u.guest_lunch)||0)*1.0; gd = (parseFloat(u.guest_dinner)||0)*1.0;
                } else if (targetDate.getTime() === todayDate.getTime()) {
                    let [lH, lM] = customLock.lunch.split(':').map(Number);
                    let [dH, dM] = customLock.dinner.split(':').map(Number);
                    let lLock = new Date(); lLock.setHours(lH, lM, 0, 0);
                    let dLock = new Date(); dLock.setHours(dH, dM, 0, 0);

                    gb = (parseFloat(u.guest_breakfast)||0)*0.5;
                    if(now >= lLock) gl = (parseFloat(u.guest_lunch)||0)*1.0;
                    if(now >= dLock) gd = (parseFloat(u.guest_dinner)||0)*1.0;
                }
                guestMeals[m] += (gb + gl + gd);

                if (dayStr === dateKey) {
                    tb += meals.b + gb; tl += meals.l + gl; tr += meals.d + gd;
                }
            });
        });

        let billed = {}, totalBilled = 0;
        members.forEach(m => {
            let actual = ownMeals[m];
            let bill = actual;
            if(!memberExceptions[m]) {
                if(actual < messRules.t1) bill = messRules.p1;
                else if(actual < messRules.t2) bill = messRules.p2;
            }
            let total = bill + guestMeals[m];
            billed[m] = total;
            totalBilled += total;
        });

        let rate = totalBilled > 0 ? mealCost / totalBilled : 0;
        let fixedShare = members.length ? fixed / members.length : 0;

        if(selectedLedgerMonth === currentMonthKey || !selectedLedgerMonth) {
            if(document.getElementById('topMeals')) document.getElementById('topMeals').innerText = totalBilled.toFixed(1) + " টি";
            if(document.getElementById('topFixedCost')) document.getElementById('topFixedCost').innerText = "৳ " + fixed;
            if(document.getElementById('liveRate')) document.getElementById('liveRate').innerText = rate.toFixed(2);
            if(document.getElementById('liveTotalCost')) document.getElementById('liveTotalCost').innerText = mealCost;
            if(document.getElementById('topTodayTotal')) document.getElementById('topTodayTotal').innerText = (tb+tl+tr).toFixed(1) + " টি";
            if(document.getElementById('t-b')) document.getElementById('t-b').innerText = tb.toFixed(1);
            if(document.getElementById('t-l')) document.getElementById('t-l').innerText = tl.toFixed(1);
            if(document.getElementById('t-r')) document.getElementById('t-r').innerText = tr.toFixed(1);

            let ledger = '', totalDep = 0;
            members.forEach(m => {
                let actualTotal = ownMeals[m] + guestMeals[m];
                let billTotal = billed[m];
                let cost = billTotal * rate;
                let totalCost = cost + fixedShare;
                let bal = deposits[m] - totalCost;
                totalDep += deposits[m];
                let display = actualTotal === billTotal ? `<b>${actualTotal.toFixed(1)}</b>` : `${actualTotal.toFixed(1)}<br><small>(বিল: ${billTotal.toFixed(1)})</small>`;
                ledger += `<tr><td><strong>${memberNames[m]}</strong>${memberExceptions[m]?' <small>[ছাড়]</small>':''}<td>${display}<td>৳ ${cost.toFixed(1)}</td><td style="color:#a855f7;">৳ ${fixedShare.toFixed(1)}</td><td>৳ ${deposits[m]}</td><td style="color:${bal>=0?'green':'red'}">${bal.toFixed(1)}</td></tr>`;
            });
            if(document.getElementById('ledgerBody')) document.getElementById('ledgerBody').innerHTML = ledger;
            if(document.getElementById('liveCash')) document.getElementById('liveCash').innerText = (totalDep - (mealCost + fixed)).toFixed(2);

            if(!isManager && currentUser) {
                let myTotal = ownMeals[currentUser] + guestMeals[currentUser];
                let myBill = billed[currentUser];
                let myBal = deposits[currentUser] - ((myBill * rate) + fixedShare);
                if(document.getElementById('myTotalMeals')) document.getElementById('myTotalMeals').innerHTML = myTotal.toFixed(1) + " টি";
                if(document.getElementById('topBalance')) document.getElementById('topBalance').innerHTML = "৳ " + myBal.toFixed(1);
                if(document.getElementById('balanceCard')) document.getElementById('balanceCard').className = `stat-card ${myBal >= 0 ? 'green' : 'red'}`;
            }
            renderSheet(allDays);
            renderStatement(allDays);
            renderHistory(monthData);
        }
    }

    // ঐতিহাসিক লেজার (পুরানো মাসের জন্য)
    function renderHistoricalLedger(monthKeyVal) {
        get(ref(db, `records/${monthKeyVal}`)).then(snap => {
            let data = snap.val() || {};
            let bazaar = parseFloat(data.totalBazaarCost) || 0; let rice = parseFloat(data.totalRiceCost) || 0; let fixed = parseFloat(data.totalFixedCost) || 0; let mealCost = bazaar + rice;
            let own = {}, guest = {}, deposit = {};
            members.forEach(m => { own[m]=0; guest[m]=0; deposit[m]=0; });
            Object.keys(data).forEach(day => {
                if(day.includes('-') && day.length>8) {
                    members.forEach(m => {
                        let u = data[day][m] || {}; deposit[m] += parseFloat(u.deposit) || 0;
                        if(u.breakfast === "ON") own[m] += 0.5; if(u.lunch === "ON") own[m] += 1.0; if(u.dinner === "ON") own[m] += 1.0;
                        guest[m] += ((parseFloat(u.guest_breakfast)||0)*0.5 + (parseFloat(u.guest_lunch)||0) + (parseFloat(u.guest_dinner)||0));
                    });
                }
            });
            let billed = {}, totalBilled = 0;
            members.forEach(m => {
                let actual = own[m], bill = actual;
                if(!memberExceptions[m]) { if(actual < messRules.t1) bill = messRules.p1; else if(actual < messRules.t2) bill = messRules.p2; }
                let total = bill + guest[m]; billed[m] = total; totalBilled += total;
            });
            let rate = totalBilled > 0 ? mealCost / totalBilled : 0; let fixedShare = members.length ? fixed / members.length : 0;
            let ledgerHtml = '';
            members.forEach(m => {
                let actualTotal = own[m] + guest[m], billTotal = billed[m], cost = billTotal * rate, totalCost = cost + fixedShare, bal = deposit[m] - totalCost;
                let display = actualTotal === billTotal ? `<b>${actualTotal.toFixed(1)}</b>` : `${actualTotal.toFixed(1)}<br><small>বিল: ${billTotal.toFixed(1)}</small>`;
                ledgerHtml += `<tr><td><strong>${memberNames[m]}</strong>${memberExceptions[m]?' <small>[ছাড়]</small>':''}<td>${display}<td>৳ ${cost.toFixed(1)}</td><td style="color:#a855f7;">৳ ${fixedShare.toFixed(1)}</td><td>৳ ${deposit[m]}</td><td style="color:${bal>=0?'green':'red'}">${bal.toFixed(1)}</td></tr>`;
            });
            document.getElementById('ledgerBody').innerHTML = ledgerHtml;
        });
    }

    function getDayClass(dayStr) {
        let dayDate = new Date(dayStr);
        let todayDate = new Date(today.getFullYear(), today.getMonth(), today.getDate());
        if(dayDate < todayDate) return 'day-past';
        if(dayDate.getTime() === todayDate.getTime()) return 'day-today';
        return 'day-future';
    }

    function renderSheet(allDays) {
        if(!allDays) return;
        let headHtml = '<th>নাম</th>';
        allDays.forEach(day => headHtml += `<th>${day.split('-')[2]}</th>`);
        document.getElementById('sheetHead').innerHTML = headHtml;
        let bodyHtml = '';
        members.forEach(m => {
            bodyHtml += `<tr><td style="font-weight:bold;">${memberNames[m]}</td>`;
            allDays.forEach(day => {
                let u = monthData[day]?.[m] || {};
                let meals = getRealTimeMealCount(day, u);
                let cls = getDayClass(day);
                bodyHtml += `<td class="${cls} ${isManager ? 'editable-cell' : ''}" onclick="${isManager ? `editMeal('${m}','${day}')` : ''}">${meals.total.toFixed(1)}</td>`;
            });
            bodyHtml += '</tr>';
        });
        document.getElementById('sheetBody').innerHTML = bodyHtml;
    }

    window.editMeal = async (m, day) => {
        if(!isManager) return;
        let cur = monthData[day]?.[m] || {};
        let { value: v } = await Swal.fire({
            title: `${memberNames[m]} - ${day}`,
            html: `<select id="eb"><option value="ON" ${cur.breakfast!=="OFF"?'selected':''}>ON</option><option value="OFF" ${cur.breakfast==="OFF"?'selected':''}>OFF</option></select><br><br>
                   <select id="el"><option value="ON" ${cur.lunch!=="OFF"?'selected':''}>ON</option><option value="OFF" ${cur.lunch==="OFF"?'selected':''}>OFF</option></select><br><br>
                   <select id="ed"><option value="ON" ${cur.dinner!=="OFF"?'selected':''}>ON</option><option value="OFF" ${cur.dinner==="OFF"?'selected':''}>OFF</option></select>`,
            preConfirm: () => ({ breakfast: document.getElementById('eb').value, lunch: document.getElementById('el').value, dinner: document.getElementById('ed').value })
        });
        if(v) await update(ref(db, `records/${day.substring(0,7)}/${day}/${m}`), v);
    };

    function renderStatement(allDays) {
        if(!allDays) return;
        let target = isManager ? document.getElementById('statementUser').value : currentUser;
        let html = '';
        allDays.forEach(day => {
            let d = monthData[day]?.[target] || {};
            let meals = getRealTimeMealCount(day, d);
            let gb = 0, gl = 0, gd = 0;
            let targetDate = new Date(day);
            let todayDate = new Date(today.getFullYear(), today.getMonth(), today.getDate());
            
            if(targetDate < todayDate) {
                gb = (parseFloat(d.guest_breakfast)||0)*0.5; gl = (parseFloat(d.guest_lunch)||0)*1.0; gd = (parseFloat(d.guest_dinner)||0)*1.0;
            } else if (targetDate.getTime() === todayDate.getTime()) {
                let now = new Date();
                let [lH, lM] = customLock.lunch.split(':').map(Number);
                let [dH, dM] = customLock.dinner.split(':').map(Number);
                let lLock = new Date(); lLock.setHours(lH, lM, 0, 0);
                let dLock = new Date(); dLock.setHours(dH, dM, 0, 0);
                gb = (parseFloat(d.guest_breakfast)||0)*0.5;
                if(now >= lLock) gl = (parseFloat(d.guest_lunch)||0)*1.0;
                if(now >= dLock) gd = (parseFloat(d.guest_dinner)||0)*1.0;
            }
            let totalG = gb + gl + gd;
            let bHtml = d.breakfast==="OFF" ? `<span style="color:var(--danger)">OFF</span>` : `0.5`;
            let lHtml = d.lunch==="OFF" ? `<span style="color:var(--danger)">OFF</span>` : `1.0`;
            let dHtml = d.dinner==="OFF" ? `<span style="color:var(--danger)">OFF</span>` : `1.0`;

            html += `<tr><td style="font-weight:bold;">${day.split('-').reverse().join('/')}</td><td>${bHtml}</td><td>${lHtml}</td><td>${dHtml}</td><td style="color:var(--warning); font-weight:bold">${totalG > 0 ? '+'+totalG : '-'}</td><td style="color:var(--success); font-weight:bold">${parseFloat(d.deposit)||0}</td></tr>`;
        });
        document.getElementById('statementBody').innerHTML = html;
    }

    function renderHistory(data) {
        if(!data) return;
        let b='', r='', f='';
        if(data.bazaarHistory) {
            Object.values(data.bazaarHistory).sort((x,y)=>y.date.localeCompare(x.date)).forEach(h => {
                if(h.type==='rice') r += `<tr><td>${h.date}</td><td>${h.details}</td><td>৳ ${h.amount}</td></tr>`;
                else if(h.type==='fixed') f += `<tr><td>${h.date}</td><td>${h.details}</td><td>৳ ${h.amount}</td></tr>`;
                else b += `<tr><td>${h.date}</td><td>${memberNames[h.shopper1]} ${h.shopper2?'ও '+memberNames[h.shopper2]:''}</td><td>৳ ${h.amount}</td></tr>`;
            });
        }
        document.getElementById('histB').innerHTML = b || '<tr><td colspan="3">কোনো এন্ট্রি নেই</td></tr>';
        document.getElementById('histR').innerHTML = r || '<tr><td colspan="3">কোনো এন্ট্রি নেই</td></tr>';
        document.getElementById('histF').innerHTML = f || '<tr><td colspan="3">কোনো এন্ট্রি নেই</td></tr>';
    }

    function updateLockUI() {
        if(document.getElementById('lockB')) document.getElementById('lockB').innerText = customLock.breakfast;
        if(document.getElementById('lockL')) document.getElementById('lockL').innerText = customLock.lunch;
        if(document.getElementById('lockD')) document.getElementById('lockD').innerText = customLock.dinner;
        if(isManager) {
            if(document.getElementById('lockTimeB')) document.getElementById('lockTimeB').value = customLock.breakfast;
            if(document.getElementById('lockTimeL')) document.getElementById('lockTimeL').value = customLock.lunch;
            if(document.getElementById('lockTimeD')) document.getElementById('lockTimeD').value = customLock.dinner;
        }
    }

    function updateRuleUI() {
        if(isManager) {
            document.getElementById('rule1Toggle').checked = messRules.t1===5 && messRules.p1===20;
            document.getElementById('rule2Toggle').checked = messRules.t2===50 && messRules.p2===50;
        }
    }

    window.saveRulesToggle = () => {
        let newRules = { t1: 5, p1: 20, t2: 50, p2: 50 };
        if(!document.getElementById('rule1Toggle').checked) { newRules.t1 = 0; newRules.p1 = 0; }
        if(!document.getElementById('rule2Toggle').checked) { newRules.t2 = 0; newRules.p2 = 0; }
        set(ref(db, `records/${currentMonthKey}/messRules`), newRules).then(() => showMsg("নিয়ম আপডেট হয়েছে"));
    };

    function isLocked(type, date) {
        let now = new Date(), target = new Date(date);
        let todayDate = new Date(today.getFullYear(), today.getMonth(), today.getDate());
        let targetDate = new Date(target.getFullYear(), target.getMonth(), target.getDate());
        if(targetDate < todayDate) return true;
        if(type === 'breakfast') {
            let [h,m] = customLock.breakfast.split(':');
            let deadline = new Date(target); deadline.setDate(deadline.getDate()-1); deadline.setHours(h,m,0,0);
            return now >= deadline;
        }
        if(type === 'lunch') {
            let [h,m] = customLock.lunch.split(':');
            let deadline = new Date(target); deadline.setHours(h,m,0,0);
            return now >= deadline;
        }
        if(type === 'dinner') {
            let [h,m] = customLock.dinner.split(':');
            let deadline = new Date(target); deadline.setHours(h,m,0,0);
            return now >= deadline;
        }
        return false;
    }

    window.loadMeals = async () => {
        if(isManager || !currentUser) return;
        let date = document.getElementById('mealDate').value;
        if(!date) return;
        let snap = await get(ref(db, `records/${date.substring(0,7)}/${date}/${currentUser}`));
        let d = snap.val() || {};
        if(document.getElementById('btnB')) {
            document.getElementById('btnB').innerText = d.breakfast === "OFF" ? "OFF" : "ON";
            document.getElementById('btnL').innerText = d.lunch === "OFF" ? "OFF" : "ON";
            document.getElementById('btnD').innerText = d.dinner === "OFF" ? "OFF" : "ON";
            document.getElementById('btnB').className = `toggle-btn ${d.breakfast === "OFF" ? "btn-off" : "btn-on"}`;
            document.getElementById('btnL').className = `toggle-btn ${d.lunch === "OFF" ? "btn-off" : "btn-on"}`;
            document.getElementById('btnD').className = `toggle-btn ${d.dinner === "OFF" ? "btn-off" : "btn-on"}`;
        }
        if(document.getElementById('guestCount')) {
            let gt = document.getElementById('guestTime').value;
            document.getElementById('guestCount').value = d["guest_"+gt] || 0;
        }
        checkButtons();
    };

    function checkButtons() {
        let date = document.getElementById('mealDate')?.value;
        if(!date || isManager) return;
        if(document.getElementById('btnB')) {
            document.getElementById('btnB').disabled = isLocked('breakfast', date);
            document.getElementById('btnL').disabled = isLocked('lunch', date);
            document.getElementById('btnD').disabled = isLocked('dinner', date);
        }
    }

    window.toggleMeal = async (type) => {
        if(isManager || !currentUser) return;
        let date = document.getElementById('mealDate').value;
        if(isLocked(type, date)) { showMsg("সময় পার হয়েছে!", "error"); return; }
        let btn = document.getElementById('btn-' + (type==='breakfast'?'B':type==='lunch'?'L':'D'));
        if(!btn) return;
        let newState = btn.innerText === "ON" ? "OFF" : "ON";
        const confirmResult = await Swal.fire({ title: "নিশ্চিত?", text: `মিল ${newState === "ON" ? "অন" : "অফ"} করতে চান?`, icon: "question", showCancelButton: true, confirmButtonText: "হ্যাঁ", cancelButtonText: "না" });
        if(!confirmResult.isConfirmed) return;
        let updates = {};
        let [y,m,start] = date.split('-');
        let last = new Date(parseInt(y), parseInt(m), 0).getDate();
        for(let d=parseInt(start); d<=last; d++) {
            let dStr = `${y}-${m}-${String(d).padStart(2,'0')}`;
            updates[`records/${dStr.substring(0,7)}/${dStr}/${currentUser}/${type}`] = newState;
        }
        await update(ref(db), updates);
        btn.innerText = newState;
        btn.className = `toggle-btn ${newState === "ON" ? "btn-on" : "btn-off"}`;
        showMsg(newState === "ON" ? "মিল অন হয়েছে" : "মিল অফ হয়েছে");
    };

    window.saveGuest = async () => {
        if(isManager || !currentUser) return;
        let date = document.getElementById('mealDate').value;
        let slot = "guest_" + document.getElementById('guestTime').value;
        let count = parseInt(document.getElementById('guestCount').value) || 0;
        let type = document.getElementById('guestTime').value;
        if(isLocked(type, date)) { showMsg("সময় পার হয়েছে!", "error"); return; }
        await update(ref(db, `records/${date.substring(0,7)}/${date}/${currentUser}`), { [slot]: count });
        showMsg("গেস্ট মিল সেভ হয়েছে");
    };

    window.changeOwnPin = async () => {
        let old = document.getElementById('oldPin').value;
        let newp = document.getElementById('newPin').value;
        let snap = await get(ref(db, `records/memberPins/${currentUser}`));
        if(old !== (snap.val() || "1234")) return showMsg("পুরাতন পিন ভুল", "error");
        await set(ref(db, `records/memberPins/${currentUser}`), newp);
        showMsg("পিন পরিবর্তিত হয়েছে");
        document.getElementById('oldPin').value = ''; document.getElementById('newPin').value = '';
    };

    function renderMenu(menu) {
        let cont = document.getElementById('menuContainer');
        if(!cont) return;
        if(!menu || !menu.day1Date) { cont.innerHTML = '<p>কোনো মেনু নেই</p>'; return; }
        let html = '';
        for(let i=1;i<=3;i++) {
            if(menu[`day${i}Date`]) {
                html += `<div class="menu-card"><div class="menu-header" style="background:var(--primary); color:white; padding:10px; border-radius:10px 10px 0 0;">${menu[`day${i}Date`].split('-').reverse().join('/')}</div>
                        <div style="padding:10px; border-bottom:1px solid #f1f5f9;">🌅 ${menu[`day${i}B`] || '-'}</div>
                        <div style="padding:10px; border-bottom:1px solid #f1f5f9;">🌞 ${menu[`day${i}L`] || '-'}</div>
                        <div style="padding:10px;">🌙 ${menu[`day${i}D`] || '-'}</div></div>`;
            }
        }
        cont.innerHTML = html;
        if(isManager) {
            document.getElementById('menuDay1Date').value = menu.day1Date || ''; document.getElementById('menuDay1B').value = menu.day1B || ''; document.getElementById('menuDay1L').value = menu.day1L || ''; document.getElementById('menuDay1D').value = menu.day1D || '';
            document.getElementById('menuDay2Date').value = menu.day2Date || ''; document.getElementById('menuDay2B').value = menu.day2B || ''; document.getElementById('menuDay2L').value = menu.day2L || ''; document.getElementById('menuDay2D').value = menu.day2D || '';
            document.getElementById('menuDay3Date').value = menu.day3Date || ''; document.getElementById('menuDay3B').value = menu.day3B || ''; document.getElementById('menuDay3L').value = menu.day3L || ''; document.getElementById('menuDay3D').value = menu.day3D || '';
        }
    }

    // Manager Functions
    window.toggleAccordion = (header) => { let content = header.nextElementSibling; if(content.style.display === 'block') content.style.display = 'none'; else content.style.display = 'block'; };
    window.saveLockTimes = () => { set(ref(db, 'records/customLockTimes'), { breakfast: document.getElementById('lockTimeB').value, lunch: document.getElementById('lockTimeL').value, dinner: document.getElementById('lockTimeD').value }).then(()=>showMsg("টাইম-লক সেভ হয়েছে")); };
    window.saveDuty = () => { let d=document.getElementById('dutyDate').value, s1=document.getElementById('dutyMem1').value, s2=document.getElementById('dutyMem2').value; if(!s1) return showMsg("প্রধান বাজারী দিন","error"); set(ref(db,`records/bazaarDuty/${d}`),{s1,s2}).then(()=>showMsg("ডিউটি সেভ হয়েছে")); };
    window.toggleEntryFields = () => { let t=document.getElementById('entryType').value; document.querySelectorAll('[id^="div-"]').forEach(d=>d.classList.add('hidden')); document.getElementById('div-'+t).classList.remove('hidden'); };
    window.saveEntry = async () => { let type=document.getElementById('entryType').value;
        if(type==='deposit'){ let m=document.getElementById('depMember').value, amt=parseFloat(document.getElementById('depAmount').value); if(!amt) return; let snap=await get(ref(db,`records/${currentMonthKey}/${dateKey}/${m}/deposit`)); let old=parseFloat(snap.val())||0; await set(ref(db,`records/${currentMonthKey}/${dateKey}/${m}/deposit`),old+amt); showMsg("জমা হয়েছে"); }
        else if(type==='bazaar'){ let s1=document.getElementById('shopper1').value, s2=document.getElementById('shopper2').value, amt=parseFloat(document.getElementById('bazAmount').value); if(!amt) return; let snap=await get(ref(db,`records/${currentMonthKey}/totalBazaarCost`)); let old=parseFloat(snap.val())||0; await set(ref(db,`records/${currentMonthKey}/totalBazaarCost`),old+amt); await set(ref(db,`records/${currentMonthKey}/bazaarHistory/${Date.now()}`),{date:dateKey,type:'regular',shopper1:s1,shopper2:s2,amount:amt}); showMsg("বাজার সেভ হয়েছে"); }
        else if(type==='rice'){ let note=document.getElementById('riceNote').value, amt=parseFloat(document.getElementById('riceAmount').value); if(!amt) return; let snap=await get(ref(db,`records/${currentMonthKey}/totalRiceCost`)); let old=parseFloat(snap.val())||0; await set(ref(db,`records/${currentMonthKey}/totalRiceCost`),old+amt); await set(ref(db,`records/${currentMonthKey}/bazaarHistory/${Date.now()}`),{date:dateKey,type:'rice',details:note,amount:amt}); showMsg("চালের খরচ সেভ হয়েছে"); }
        else if(type==='fixed'){ let note=document.getElementById('fixedNote').value, amt=parseFloat(document.getElementById('fixedAmount').value); if(!amt) return; let snap=await get(ref(db,`records/${currentMonthKey}/totalFixedCost`)); let old=parseFloat(snap.val())||0; await set(ref(db,`records/${currentMonthKey}/totalFixedCost`),old+amt); await set(ref(db,`records/${currentMonthKey}/bazaarHistory/${Date.now()}`),{date:dateKey,type:'fixed',details:note,amount:amt}); showMsg("ফিক্সড খরচ সেভ হয়েছে"); }
        document.getElementById('depAmount').value=''; document.getElementById('bazAmount').value=''; document.getElementById('riceAmount').value=''; document.getElementById('fixedAmount').value='';
    };
    window.saveNoticeAndMenu = () => { let notice=document.getElementById('mgrNotice').value; let menu={ day1Date:document.getElementById('menuDay1Date').value, day1B:document.getElementById('menuDay1B').value, day1L:document.getElementById('menuDay1L').value, day1D:document.getElementById('menuDay1D').value, day2Date:document.getElementById('menuDay2Date').value, day2B:document.getElementById('menuDay2B').value, day2L:document.getElementById('menuDay2L').value, day2D:document.getElementById('menuDay2D').value, day3Date:document.getElementById('menuDay3Date').value, day3B:document.getElementById('menuDay3B').value, day3L:document.getElementById('menuDay3L').value, day3D:document.getElementById('menuDay3D').value }; set(ref(db,`records/${currentMonthKey}/noticeBoard`),notice); set(ref(db,'records/threeDayMenuRoutine'),menu).then(()=>showMsg("নোটিশ ও মেনু সেভ হয়েছে")); };
    window.toggleSuperEdit = () => { let t=document.getElementById('superType').value; document.querySelectorAll('[id$="Div"]').forEach(d=>d.classList.add('hidden')); if(t==='cost') document.getElementById('costDiv').classList.remove('hidden'); else if(t==='deposit'){ document.getElementById('depositDiv').classList.remove('hidden'); loadCurDeposit(); } else if(t==='meal') document.getElementById('mealDiv').classList.remove('hidden'); else if(t==='reset') document.getElementById('resetDiv').classList.remove('hidden'); };
    window.loadCurDeposit = () => { let m=document.getElementById('editMemDeposit').value; document.getElementById('curDep').innerText = monthData[dateKey]?.[m]?.deposit || 0; };
    window.editCost = async () => { let u={}; let b=document.getElementById('editBaz').value; if(b) u.totalBazaarCost=parseFloat(b); let r=document.getElementById('editRice').value; if(r) u.totalRiceCost=parseFloat(r); let f=document.getElementById('editFix').value; if(f) u.totalFixedCost=parseFloat(f); await update(ref(db,`records/${currentMonthKey}`),u); showMsg("খরচ আপডেট হয়েছে"); };
    window.editDeposit = async () => { let m=document.getElementById('editMemDeposit').value, nd=parseFloat(document.getElementById('newDeposit').value); if(isNaN(nd)) return; await set(ref(db,`records/${currentMonthKey}/${dateKey}/${m}/deposit`),nd); showMsg("জমা আপডেট হয়েছে"); };
    window.loadMealEdit = async () => { let date=document.getElementById('editMealDate').value, m=document.getElementById('editMealMem').value; let snap=await get(ref(db,`records/${date.substring(0,7)}/${date}/${m}`)); let d=snap.val()||{}; document.getElementById('mealEditPanel').innerHTML=`<div style="margin-top:10px;"><label>সকাল</label><select id="seB"><option value="ON" ${d.breakfast!=="OFF"?'selected':''}>ON</option><option value="OFF" ${d.breakfast==="OFF"?'selected':''}>OFF</option></select></div><div style="margin-top:5px;"><label>দুপুর</label><select id="seL"><option value="ON" ${d.lunch!=="OFF"?'selected':''}>ON</option><option value="OFF" ${d.lunch==="OFF"?'selected':''}>OFF</option></select></div><div style="margin-top:5px;"><label>রাত</label><select id="seD"><option value="ON" ${d.dinner!=="OFF"?'selected':''}>ON</option><option value="OFF" ${d.dinner==="OFF"?'selected':''}>OFF</option></select></div><div style="margin-top:5px;"><label>গেস্ট সকাল</label><input id="seGB" value="${d.guest_breakfast||0}"></div><div style="margin-top:5px;"><label>গেস্ট দুপুর</label><input id="seGL" value="${d.guest_lunch||0}"></div><div style="margin-top:5px;"><label>গেস্ট রাত</label><input id="seGD" value="${d.guest_dinner||0}"></div><button class="btn-save" onclick="saveMealEdit()">আপডেট</button>`; };
    window.saveMealEdit = async () => { let date=document.getElementById('editMealDate').value, m=document.getElementById('editMealMem').value; await update(ref(db,`records/${date.substring(0,7)}/${date}/${m}`),{ breakfast:document.getElementById('seB').value, lunch:document.getElementById('seL').value, dinner:document.getElementById('seD').value, guest_breakfast:parseInt(document.getElementById('seGB').value)||0, guest_lunch:parseInt(document.getElementById('seGL').value)||0, guest_dinner:parseInt(document.getElementById('seGD').value)||0 }); showMsg("মিল আপডেট হয়েছে"); };
    window.resetMonth = () => { Swal.fire({ title:"নিশ্চিত?", text:"সব ডাটা মুছে যাবে!", icon:"warning", showCancelButton:true, confirmButtonColor:"#d33", confirmButtonText:"হ্যাঁ, রিসেট" }).then(res=>{ if(res.isConfirmed) remove(ref(db,`records/${currentMonthKey}`)).then(()=>showMsg("রিসেট সম্পন্ন")); }); };
    window.changeMasterPin = async () => { let old=document.getElementById('oldMasterPin').value, newp=document.getElementById('newMasterPin').value; let snap=await get(ref(db,'records/managerPin')); if(old!==(snap.val()||"0000")) return showMsg("পুরাতন পিন ভুল","error"); await set(ref(db,'records/managerPin'),newp); showMsg("মাস্টার পিন পরিবর্তিত হয়েছে"); };
    window.addMember = async () => { let name=document.getElementById('newMemberName').value; if(!name) return; let newId="Member"+(members.length+1); await update(ref(db,'records/memberConfig'),{[newId]:name}); await set(ref(db,`records/memberPins/${newId}`),"1234"); showMsg("নতুন মেম্বার যোগ হয়েছে"); document.getElementById('newMemberName').value=''; };
    function renderMemberList() { let html=''; members.forEach(m=>{ html+=`<div class="mem-item" style="margin-bottom:15px; border:1px solid #e2e8f0; border-radius:20px; padding:12px;"><div onclick="toggleMemberAccordion('${m}')" style="cursor:pointer; display:flex; justify-content:space-between;"><span><i class="fas fa-user"></i> ${memberNames[m]}</span><i class="fas fa-chevron-down" id="icon-${m}"></i></div><div id="mem-${m}" style="display:none; margin-top:10px;"><input type="text" id="name-${m}" value="${memberNames[m]}"><input type="password" id="pin-${m}" value="${memberPins[m]||'1234'}" maxlength="4"><label><input type="checkbox" id="exempt-${m}" ${memberExceptions[m]?'checked':''}> পেনাল্টি মুক্ত</label></div></div>`; }); if(document.getElementById('memberEditList')) document.getElementById('memberEditList').innerHTML=html; }
    window.toggleMemberAccordion = (id) => { let div=document.getElementById('mem-'+id), icon=document.getElementById('icon-'+id); if(div.style.display==='none'){ div.style.display='block'; icon.style.transform='rotate(180deg)'; } else { div.style.display='none'; icon.style.transform='rotate(0deg)'; } };
    window.saveAllMembers = async () => { let c={}, e={}, p={}; members.forEach(m=>{ c[m]=document.getElementById('name-'+m).value; e[m]=document.getElementById('exempt-'+m).checked; p[m]=document.getElementById('pin-'+m).value||"1234"; }); await update(ref(db,'records'),{memberConfig:c,memberExceptions:e,memberPins:p}); showMsg("মেম্বার তথ্য সেভ হয়েছে"); };
    function updateDropdowns() { let opt=members.map(m=>`<option value="${m}">${memberNames[m]}</option>`).join(''); if(document.getElementById('loginUser')) document.getElementById('loginUser').innerHTML=opt; if(document.getElementById('depMember')) document.getElementById('depMember').innerHTML=opt; if(document.getElementById('shopper1')) document.getElementById('shopper1').innerHTML=opt; if(document.getElementById('shopper2')) document.getElementById('shopper2').innerHTML='<option value="">-- কেউ না --</option>'+opt; if(document.getElementById('dutyMem1')) document.getElementById('dutyMem1').innerHTML=opt; if(document.getElementById('dutyMem2')) document.getElementById('dutyMem2').innerHTML='<option value="">-- কেউ না --</option>'+opt; if(document.getElementById('statementUser')) document.getElementById('statementUser').innerHTML=opt; if(document.getElementById('editMemDeposit')) document.getElementById('editMemDeposit').innerHTML=opt; if(document.getElementById('editMealMem')) document.getElementById('editMealMem').innerHTML=opt; let mHtml=`<div><label>১ম দিন</label><input type="date" id="menuDay1Date"><input type="text" id="menuDay1B" placeholder="সকাল"><input type="text" id="menuDay1L" placeholder="দুপুর"><input type="text" id="menuDay1D" placeholder="রাত"></div><div><label>২য় দিন</label><input type="date" id="menuDay2Date"><input type="text" id="menuDay2B" placeholder="সকাল"><input type="text" id="menuDay2L" placeholder="দুপুর"><input type="text" id="menuDay2D" placeholder="রাত"></div><div><label>৩য় দিন</label><input type="date" id="menuDay3Date"><input type="text" id="menuDay3B" placeholder="সকাল"><input type="text" id="menuDay3L" placeholder="দুপুর"><input type="text" id="menuDay3D" placeholder="রাত"></div>`; if(document.getElementById('menuInputs')) document.getElementById('menuInputs').innerHTML=mHtml; }
    window.switchTab = (id) => { document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active')); event.currentTarget.classList.add('active'); document.querySelectorAll('[id^="sec-"]').forEach(s=>s.classList.add('hidden')); document.getElementById('sec-'+id).classList.remove('hidden'); if(id==='sheet') renderSheet(); if(id==='statement') renderStatement(); window.scrollTo(0,0); };
    window.downloadPDF = () => { let el=document.getElementById('sec-ledger').querySelector('.content-card'); if(el) html2pdf().from(el).save(); };
    setInterval(()=>{ if(document.getElementById('mealDate')) checkButtons(); },1000);
    get(ref(db,'records/memberConfig')).then(s=>{ memberNames=s.val()||{Member1:"প্রশাসক"}; members=Object.keys(memberNames); updateDropdowns(); });
</script>
</body>
</html>
