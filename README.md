<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Akça Pro X - Kurumsal Anket ve Raporlama Sistemi</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Firebase App (the core Firebase SDK) -->
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
    <!-- Firebase Auth -->
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth-compat.js"></script>
    <!-- Firebase Database -->
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-database-compat.js"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .gradient-bg {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }
        .active-tab {
            border: 2px solid #6366f1;
            background-color: #eef2ff;
        }
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.5);
        }
        .modal.show {
            display: flex;
            align-items: center;
            justify-content: center;
        }
        @media print {
            .no-print { display: none !important; }
            body { background: white !important; }
        }
        
        /* Profesyonel Tablo Stilleri */
        .survey-table {
            border-collapse: collapse;
            width: 100%;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            font-size: 11px;
            background: white;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }
        .survey-table th, .survey-table td {
            border: 1px solid #a3a3a3;
            padding: 8px 12px;
            text-align: center;
        }
        .survey-table th {
            background: linear-gradient(to bottom, #f8f9fa 0%, #e9ecef 100%);
            font-weight: 600;
            color: #495057;
            border-bottom: 2px solid #6c757d;
        }
        .main-group-row {
            background: linear-gradient(to bottom, #e3f2fd 0%, #bbdefb 100%);
            font-weight: 700;
            color: #1565c0;
        }
        .sub-category {
            background: #f8f9fa;
            text-align: left !important;
            font-weight: 500;
            padding-left: 20px;
        }
        .survey-table tbody tr:nth-child(even) {
            background: #f8f9fa;
        }
        .survey-table tbody tr:hover {
            background: #e3f2fd;
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen">
    <!-- Ana Navigasyon -->
    <nav class="gradient-bg text-white p-3 shadow-lg sticky top-0 z-50">
        <div class="max-w-5xl mx-auto flex flex-col md:flex-row justify-between items-center gap-2 md:gap-0">
            <div class="flex items-center gap-2">
                <!-- Gizli yönetici erişimi -->
                <div onclick="showModule('admin')" class="w-4 h-4 cursor-pointer opacity-15 hover:opacity-50 transition-opacity" title="">
                    <div class="w-4 h-4 rounded-full border border-white/30 flex items-center justify-center animate-spin" style="animation-duration: 12s;">
                        <div class="w-1 h-1 bg-white/40 rounded-full"></div>
                    </div>
                </div>
                <div>
                    <h1 class="text-xl font-bold">Akça Pro X</h1>
                    <p class="text-sm opacity-90">Kurumsal Anket ve Raporlama Sistemi</p>
                </div>
            </div>
            <div class="flex gap-4">
                <button onclick="showModule('survey')" class="px-4 py-2 bg-white/20 rounded-lg hover:bg-white/30 transition-colors">📊 Anket</button>
                <button onclick="showModule('company')" class="px-4 py-2 bg-white/20 rounded-lg hover:bg-white/30 transition-colors">🏢 Şirket Portalı</button>
            </div>
        </div>
    </nav>

    <!-- Anket Modülü -->
    <div id="surveyModule" class="max-w-5xl mx-auto p-2 md:p-4">
        <div class="bg-white shadow-xl rounded-2xl max-w-2xl mx-auto p-4 md:p-8">
            <div class="text-center mb-6">
                <h2 class="text-2xl md:text-3xl font-extrabold text-gray-800 mb-1 tracking-tight">İşletme Yönetim Anketi</h2>
                <p class="text-gray-600 mb-2 text-base md:text-lg">Görüşleriniz bizim için değerli</p>
                <span class="bg-green-100 text-green-800 text-xs font-semibold px-2 py-1 rounded-full">v3.1.0 - Firebase Entegre</span>
            </div>

            <!-- Sorumluluk Reddi -->
            <div id="disclaimerSection" class="mb-4">
                <div class="bg-yellow-50 border border-yellow-300 rounded-lg p-3 mb-3">
                    <h3 class="font-semibold text-yellow-800 mb-2 text-sm">⚠️ Veri Koruma Beyanı</h3>
                    <div class="text-xs text-yellow-700 space-y-1">
                        <p>• Verileriniz Firebase güvenli bulut altyapısında saklanır ve üçüncü taraflarla paylaşılmaz.</p>
                        <p>• Anket sonuçları sadece şirket yetkilileri tarafından görüntülenebilir.</p>
                        <p>• Sistem güvenliği Google Firebase altyapısı sorumluluğundadır.</p>
                        <p>• Hack, veri ihlali vb. güvenlik olaylarından kaynaklanan bilgi erişimlerinin sorumluluğu Akça Pro X'e ait değildir.</p>
                    </div>
                </div>
                <label class="flex items-center space-x-2 cursor-pointer">
                    <input type="checkbox" id="acceptDisclaimer" class="w-4 h-4 text-purple-600">
                    <span class="text-xs font-medium">Veri koruma beyanını kabul ediyorum</span>
                </label>
            </div>

            <!-- Şirket Bilgileri -->
            <div id="companyInfoSection" class="">
                <h3 class="text-base font-semibold text-gray-700 mb-3">Şirket ve Kişisel Bilgiler</h3>
                <!-- Google ile Giriş Yap butonu -->
                <div class="mb-3 flex flex-col items-center">
                    <button id="googleSignInBtn" type="button" class="flex items-center gap-2 px-4 py-2 bg-white border border-gray-300 rounded shadow hover:bg-gray-100 text-gray-700 font-semibold mb-2">
                        <img src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/google.svg" alt="Google" class="w-5 h-5"> Google ile Giriş Yap
                    </button>
                    <div id="googleUserInfo" class="text-xs text-green-700 font-medium hidden"></div>
                </div>
                <div class="mb-3">
                    <input type="text" id="companyName" placeholder="Şirket adınızı girin" class="w-full border-2 border-purple-300 rounded px-3 py-2 text-sm focus:ring-2 focus:ring-purple-500 focus:border-purple-500">
                </div>
                <div class="mb-3">
                    <div class="grid grid-cols-1 sm:grid-cols-3 gap-2">
                        <button type="button" id="blueCollar" class="job-btn py-3 px-2 text-xs rounded border-2 border-blue-300 hover:border-blue-500 hover:bg-blue-50 transition-all duration-200 cursor-pointer font-medium bg-white text-center focus:outline-none focus:ring-2 focus:ring-blue-400">👷 Mavi Yaka</button>
                        <button type="button" id="whiteCollar" class="job-btn py-3 px-2 text-xs rounded border-2 border-green-300 hover:border-green-500 hover:bg-green-50 transition-all duration-200 cursor-pointer font-medium bg-white text-center focus:outline-none focus:ring-2 focus:ring-green-400">💼 Beyaz Yaka</button>
                        <button type="button" id="management" class="job-btn py-3 px-2 text-xs rounded border-2 border-purple-300 hover:border-purple-500 hover:bg-purple-50 transition-all duration-200 cursor-pointer font-medium bg-white text-center focus:outline-none focus:ring-2 focus:ring-purple-400">👔 Yönetim</button>
                    </div>
                </div>
                <div class="grid grid-cols-2 gap-2 mb-4">
                    <input type="text" id="firstName" placeholder="Adınız" class="border-2 border-gray-300 rounded px-3 py-2 text-sm focus:ring-2 focus:ring-purple-500 focus:border-purple-500">
                    <input type="text" id="lastName" placeholder="Soyadınız" class="border-2 border-gray-300 rounded px-3 py-2 text-sm focus:ring-2 focus:ring-purple-500 focus:border-purple-500">
                </div>
                <button id="startSurvey" class="w-full py-3 rounded text-white font-semibold gradient-bg hover:opacity-90 transition-opacity text-sm">
                    📊 Anketi Başlat
                </button>
            </div>

            <!-- Anket Alanı -->
            <div id="surveySection" class="hidden">
                <div class="flex flex-col md:flex-row justify-between items-center mb-6 gap-2">
                    <span id="progressText" class="text-gray-600 font-medium">Anket İlerlemesi 0/25 Yanıtlandı</span>
                    <span id="timeElapsed" class="text-sm text-gray-500">Süre: 00:00</span>
                </div>
                <div class="w-full bg-gray-200 rounded-full h-3 mb-8">
                    <div id="progressBar" class="bg-purple-600 h-3 rounded-full transition-all duration-300" style="width:0%"></div>
                </div>
                <div id="questionContainer" class="space-y-6"></div>
                <button id="submitSurvey" class="hidden w-full mt-8 py-4 rounded-xl text-white font-semibold bg-green-600 hover:bg-green-700 transition-colors text-lg">
                    ✅ Anketi Tamamla
                </button>
            </div>
        </div>
    </div>

    <!-- Şirket Portalı -->
    <div id="companyModule" class="container mx-auto p-4 hidden">
        <div class="bg-white shadow-xl rounded-xl max-w-5xl mx-auto p-6">
            <div id="companyLogin" class="max-w-md mx-auto">
                <h2 class="text-2xl font-bold text-center mb-6">🏢 Şirket Portalı Girişi</h2>
                <div class="space-y-4">
                    <input type="text" id="companyLoginName" placeholder="Şirket Adı" class="w-full border rounded-lg px-4 py-3 focus:ring-2 focus:ring-blue-500">
                    <input type="password" id="companyPassword" placeholder="12 Karakterlik Şifre" class="w-full border rounded-lg px-4 py-3 focus:ring-2 focus:ring-blue-500" autocomplete="off">
                    <button onclick="loginCompany()" class="w-full py-3 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors">
                        🔐 Giriş Yap
                    </button>
                </div>
                <div class="mt-4 p-3 bg-blue-50 rounded-lg text-sm text-blue-700">
                    <p><strong>Not:</strong> Şirket şifrenizi yöneticinizden alabilirsiniz.</p>
                </div>
            </div>

            <div id="companyDashboard" class="hidden">
                <div class="flex justify-between items-center mb-6">
                    <div>
                        <h2 class="text-2xl font-bold">Şirket Raporları</h2>
                        <p class="text-gray-600" id="companyNameDisplay"></p>
                    </div>
                    <button onclick="logoutCompany()" class="px-4 py-2 bg-red-600 text-white rounded-lg hover:bg-red-700">
                        🚪 Çıkış
                    </button>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
                    <div class="bg-gradient-to-r from-blue-500 to-blue-600 text-white p-6 rounded-lg">
                        <h3 class="text-lg font-semibold">Toplam Katılımcı</h3>
                        <p class="text-3xl font-bold" id="totalParticipants">0</p>
                    </div>
                    <div class="bg-gradient-to-r from-green-500 to-green-600 text-white p-6 rounded-lg">
                        <h3 class="text-lg font-semibold">Ortalama Puan</h3>
                        <p class="text-3xl font-bold" id="averageScore">0.0</p>
                    </div>
                    <div class="bg-gradient-to-r from-purple-500 to-purple-600 text-white p-6 rounded-lg">
                        <h3 class="text-lg font-semibold">Memnuniyet Oranı</h3>
                        <p class="text-3xl font-bold" id="satisfactionRate">0%</p>
                    </div>
                </div>

                <div class="bg-white border rounded-lg p-6">
                    <div class="flex flex-col md:flex-row justify-between items-center mb-4 gap-2">
                        <h3 class="text-lg font-semibold mb-2 md:mb-0">Anket Sonuçları</h3>
                        <div class="flex flex-col md:flex-row gap-2 items-center">
                            <input type="date" id="reportStartDate" class="border rounded px-2 py-1 text-sm" />
                            <span class="mx-1">-</span>
                            <input type="date" id="reportEndDate" class="border rounded px-2 py-1 text-sm" />
                            <button onclick="filterByDateRange()" class="px-3 py-1 bg-blue-600 text-white rounded hover:bg-blue-700 text-sm">Tarihe Göre Rapor</button>
                            <button onclick="showPDFReport(true)" class="px-3 py-1 bg-red-600 text-white rounded hover:bg-red-700 text-sm" style="display:none">📄 PDF Göster (Filtreli)</button>
                            <button onclick="showPDFReport(false)" class="px-3 py-1 bg-gray-600 text-white rounded hover:bg-gray-700 text-sm" style="display:none">📄 PDF Göster (Tümü)</button>
                        </div>
                    </div>
                    
                    <!-- Grafikler Bölümü -->
                    <div class="grid grid-cols-2 lg:grid-cols-4 gap-3 mb-4">
                        <div class="bg-gray-50 p-3 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-2 text-sm">📊 Pozisyon</h4>
                            <div style="height: 150px; position: relative;">
                                <canvas id="positionChart"></canvas>
                            </div>
                        </div>
                        <div class="bg-gray-50 p-3 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-2 text-sm">📈 Memnuniyet</h4>
                            <div style="height: 150px; position: relative;">
                                <canvas id="satisfactionChart"></canvas>
                            </div>
                        </div>
                        <div class="bg-gray-50 p-3 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-2 text-sm">⏰ Süre Dağılımı</h4>
                            <div style="height: 150px; position: relative;">
                                <canvas id="timeChart"></canvas>
                            </div>
                        </div>
                        <div class="bg-gray-50 p-3 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-2 text-sm">🎯 Puan Dağılımı</h4>
                            <div style="height: 150px; position: relative;">
                                <canvas id="trendChart"></canvas>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Katılımcı Detayları Bölümü -->
                    
                    <div id="detailedReport" class="space-y-3"></div>
    <!-- Katılımcı Listesi (Rapor Ekranı En Alt) -->
    <div id="participantListSection" class="bg-white border rounded-lg p-4 mt-6">
        <button id="toggleParticipantListBtn" class="w-full flex items-center justify-between font-semibold text-gray-800 mb-3 focus:outline-none" onclick="toggleParticipantList()">
            <span>👥 Katılımcı Listesi</span>
            <span id="participantListArrow">▼</span>
        </button>
        <div id="participantListTableWrapper" class="overflow-x-auto hidden">
            <table class="w-full table-auto text-sm">
                <thead>
                    <tr class="bg-gray-100">
                        <th class="px-3 py-2 text-left">İsim Soyisim</th>
                        <th class="px-3 py-2 text-left">Pozisyon</th>
                        <th class="px-3 py-2 text-center">Ortalama Puan</th>
                        <th class="px-3 py-2 text-center">Değerlendirme</th>
                        <th class="px-3 py-2 text-center">Tarih</th>
                    </tr>
                </thead>
                <tbody id="participantListBody">
                    <!-- Katılımcı listesi buraya yüklenecek -->
                </tbody>
            </table>
        </div>
    </div>
<script>
function toggleParticipantList() {
    const wrapper = document.getElementById('participantListTableWrapper');
    const arrow = document.getElementById('participantListArrow');
    const btn = document.getElementById('toggleParticipantListBtn');
    
    if (wrapper.classList.contains('hidden')) {
        wrapper.classList.remove('hidden');
        arrow.textContent = '▲';
        loadParticipantTable();
        // Buton metnini katılımcı sayısıyla güncelle
        const participantCount = getParticipantCount();
        btn.querySelector('span').textContent = `👥 Katılımcı Listesi (${participantCount})`;
    } else {
        wrapper.classList.add('hidden');
        arrow.textContent = '▼';
        btn.querySelector('span').textContent = '👥 Katılımcı Listesi';
    }
}

function getParticipantCount() {
    if (!loggedInCompany || !systemData.surveyData) return 0;
    
    // Filtrelenmiş veri varsa onu kullan
    if (typeof filteredSurveys !== 'undefined' && filteredSurveys !== null) {
        return filteredSurveys.length;
    } else {
        const allResponses = Object.values(systemData.surveyData.responses || {});
        return allResponses.filter(s => 
            s.companyName && s.companyName.toLowerCase() === loggedInCompany.name.toLowerCase()
        ).length;
    }
}

function loadParticipantTable() {
    if (!loggedInCompany || !systemData.surveyData) return;
    
    // Eğer tarih filtresi aktifse o verileri kullan, değilse tüm verileri kullan
    let companySurveys;
    if (typeof filteredSurveys !== 'undefined' && filteredSurveys !== null) {
        companySurveys = filteredSurveys;
    } else {
        const allResponses = Object.values(systemData.surveyData.responses || {});
        companySurveys = allResponses.filter(s => 
            s.companyName && s.companyName.toLowerCase() === loggedInCompany.name.toLowerCase()
        );
    }
    
    const tbody = document.getElementById('participantListBody');
    
    if (companySurveys.length === 0) {
        tbody.innerHTML = '<tr><td colspan="5" class="text-center py-4 text-gray-500">Henüz katılımcı bulunmuyor.</td></tr>';
        return;
    }
    
    // Puana göre yüksekten düşüğe sırala
    const sortedSurveys = [...companySurveys].sort((a, b) => 
        parseFloat(b.averageScore) - parseFloat(a.averageScore)
    );
    
    tbody.innerHTML = sortedSurveys.map(survey => {
        const displayName = (survey.firstName && survey.lastName) ? 
            `${survey.firstName} ${survey.lastName}` : 
            (survey.firstName || survey.lastName || 'İsimsiz');
        
        const avgScore = parseFloat(survey.averageScore);
        let evaluation = '';
        let evaluationColor = '';
        let evaluationIcon = '';
        
        if (avgScore >= 4.5) {
            evaluation = 'Çok Memnun';
            evaluationColor = 'text-green-600';
            evaluationIcon = '5';
        } else if (avgScore >= 3.5) {
            evaluation = 'Memnun';
            evaluationColor = 'text-green-500';
            evaluationIcon = '4';
        } else if (avgScore >= 2.5) {
            evaluation = 'Orta';
            evaluationColor = 'text-yellow-600';
            evaluationIcon = '3';
        } else if (avgScore >= 1.5) {
            evaluation = 'Düşük';
            evaluationColor = 'text-orange-600';
            evaluationIcon = '2';
        } else {
            evaluation = 'Çok Düşük';
            evaluationColor = 'text-red-600';
            evaluationIcon = '1';
        }
        
        return `
            <tr class="hover:bg-gray-50">
                <td class="px-3 py-2 font-medium">${displayName}</td>
                <td class="px-3 py-2">
                    <span class="inline-block px-2 py-1 text-xs rounded-full bg-blue-100 text-blue-800">
                        ${survey.jobType}
                    </span>
                </td>
                <td class="px-3 py-2 text-center font-semibold">${avgScore.toFixed(1)}</td>
                <td class="px-3 py-2 text-center ${evaluationColor} font-semibold">
                    <span class="inline-flex items-center gap-1">
                        <span class="inline-block w-6 h-6 rounded-full bg-gray-100 text-gray-700 text-sm font-bold flex items-center justify-center">${evaluationIcon}</span>
                        ${evaluation}
                    </span>
                </td>
                <td class="px-3 py-2 text-center text-sm text-gray-600">${new Date(survey.submittedAt).toLocaleDateString('tr-TR')}</td>
            </tr>
        `;
    }).join('');
}
</script>
                </div>
            </div>
        </div>
    </div>

    <!-- Yönetici Portalı -->
    <div id="adminModule" class="container mx-auto p-4 hidden">
        <div class="bg-white shadow-xl rounded-xl max-w-5xl mx-auto p-6">
            <div id="adminLogin" class="max-w-md mx-auto">
                <h2 class="text-2xl font-bold text-center mb-6">⚙️ Yönetici Portalı</h2>
                <div class="space-y-4">
                    <input type="password" id="adminPassword" placeholder="Yönetici Şifresi" class="w-full border rounded-lg px-4 py-3 focus:ring-2 focus:ring-red-500" autocomplete="off">
                    <button onclick="loginAdmin()" class="w-full py-3 bg-red-600 text-white rounded-lg hover:bg-red-700 transition-colors">
                        🔐 Yönetici Girişi
                    </button>
                </div>
            </div>

            <div id="adminDashboard" class="hidden">
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-2xl font-bold">Sistem Yönetimi</h2>
                    <button onclick="logoutAdmin()" class="px-4 py-2 bg-red-600 text-white rounded-lg hover:bg-red-700">
                        🚪 Çıkış
                    </button>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-6">
                    <div class="bg-blue-100 p-4 rounded-lg text-center">
                        <h3 class="font-semibold text-blue-800">Toplam Şirket</h3>
                        <p class="text-2xl font-bold text-blue-600" id="totalCompanies">0</p>
                    </div>
                    <div class="bg-green-100 p-4 rounded-lg text-center">
                        <h3 class="font-semibold text-green-800">Aktif Anketler</h3>
                        <p class="text-2xl font-bold text-green-600" id="activeSurveys">0</p>
                    </div>
                    <div class="bg-yellow-100 p-4 rounded-lg text-center">
                        <h3 class="font-semibold text-yellow-800">Toplam Katılımcı</h3>
                        <p class="text-2xl font-bold text-yellow-600" id="totalUsers">0</p>
                    </div>
                    <div class="bg-purple-100 p-4 rounded-lg text-center">
                        <h3 class="font-semibold text-purple-800">Sistem Durumu</h3>
                        <p class="text-sm font-bold text-purple-600">🟢 Aktif</p>
                    </div>
                </div>

                <div class="bg-white border rounded-lg p-6">
                    <h3 class="text-lg font-semibold mb-4">Şirket Listesi ve Yönetimi</h3>
                    <div class="mb-3 flex items-center">
                        <input type="text" id="companySearchInput" placeholder="Şirket adı ara..." class="border rounded px-3 py-2 text-sm w-full max-w-xs" oninput="filterCompanyList()">
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full table-auto">
                            <thead>
                                <tr class="bg-gray-50">
                                    <th class="px-4 py-2 text-left">Şirket Adı</th>
                                    <th class="px-4 py-2 text-left">Şifre</th>
                                    <th class="px-4 py-2 text-left">Katılımcı</th>
                                    <th class="px-4 py-2 text-left">Durum</th>
                                    <th class="px-4 py-2 text-left">İşlemler</th>
                                </tr>
                            </thead>
                            <tbody id="companyList">
                                <!-- Şirket listesi buraya yüklenecek -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal -->
    <div id="modal" class="modal">
        <div class="bg-white rounded-lg p-6 max-w-md w-full mx-4">
            <div id="modalContent"></div>
        </div>
    </div>

    <!-- AI Interpretation Modal -->
    <div id="aiInterpretationModal" class="modal">
      <div class="modal-content max-w-7xl bg-white shadow-2xl" style="margin: 2% auto; padding: 40px; border-radius: 20px; max-height: 90vh; overflow-y: auto; width: 95vw;">
        <div class="modal-header flex justify-between items-center mb-6 border-b pb-4">
          <h2 class="text-2xl font-bold text-gray-800">🤖 AI Yorum & Analiz</h2>
          <span class="close cursor-pointer text-4xl text-gray-500 hover:text-gray-700" onclick="document.getElementById('aiInterpretationModal').classList.remove('show')">&times;</span>
        </div>
        <div id="aiInterpretationContent" class="text-lg text-gray-800 leading-8 whitespace-pre-line"></div>
      </div>
    </div>

    <script>
        // Firebase config
        const firebaseConfig = {
            apiKey: "AIzaSyDp2Yh8hamXi6OTfw03MT0S4rp5CjnlAcg",
            authDomain: "akcaprox-anket.firebaseapp.com",
            projectId: "akcaprox-anket",
            storageBucket: "akcaprox-anket.appspot.com",
            messagingSenderId: "426135179922",
            appId: "1:426135179922:web:c16b3fd6fa5f3d9224cc4b",
            measurementId: "G-CD1ET7RGX1",
            databaseURL: "https://isletme-76bad-default-rtdb.europe-west1.firebasedatabase.app/"
        };
        firebase.initializeApp(firebaseConfig);
        const auth = firebase.auth();

        // Google Sign-In logic
        let googleUser = null;
        document.addEventListener('DOMContentLoaded', function() {
            // Anket başlatma butonunu startSurvey fonksiyonuna bağla
            const startBtn = document.getElementById('startSurvey');
            if (startBtn) {
                startBtn.addEventListener('click', startSurvey);
            }
            const googleBtn = document.getElementById('googleSignInBtn');
            const userInfoDiv = document.getElementById('googleUserInfo');
            if (googleBtn) {
                googleBtn.addEventListener('click', function() {
                    const provider = new firebase.auth.GoogleAuthProvider();
                    auth.signInWithPopup(provider)
                        .then((result) => {
                            const user = result.user;
                            if (user) {
                                googleUser = user;
                                // Prefill name fields
                                document.getElementById('firstName').value = user.displayName ? user.displayName.split(' ')[0] : '';
                                document.getElementById('lastName').value = user.displayName ? user.displayName.split(' ').slice(1).join(' ') : '';
                                userInfoDiv.textContent = `Giriş yapıldı: ${user.displayName} (${user.email})`;
                                userInfoDiv.classList.remove('hidden');
                                // Make name fields editable
                                document.getElementById('firstName').readOnly = false;
                                document.getElementById('lastName').readOnly = false;
                            }
                        })
                        .catch((error) => {
                            alert('Google ile giriş başarısız: ' + error.message);
                        });
                });
            }
        });

        // Anket başlatma butonuna Google ile giriş kontrolü ekle
        document.addEventListener('DOMContentLoaded', function() {
            const startBtn = document.getElementById('startSurvey');
            if (startBtn) {
                startBtn.addEventListener('click', function(e) {
                    if (!googleUser) {
                        e.preventDefault();
                        alert('Ankete başlamadan önce Google ile giriş yapmalısınız.');
                    }
                }, true);
            }
        });
        // Global değişkenler
        let currentModule = 'survey';
        let surveyStartTime = null;
        let timerInterval = null;
        let currentQuestions = [];
        let currentQuestionIndex = 0;
        let answers = [];
        let selectedJobType = '';
        let loggedInCompany = null;
        let isAdminLoggedIn = false;


        // Soru setleri
        const questions = {
            "Mavi Yaka": [
                // İş Güvenliği ve Çevre (5 Soru)
                "Çalıştığım alanda kullanılan koruyucu ekipmanlar (KKD) yeterli kalitede ve düzenli olarak sağlanmaktadır.",
                "İş kazası riski taşıyan durumlar için uygulanan güvenlik prosedürlerini net olarak biliyorum.",
                "Tehlikeli durumları veya riskleri bildirdiğimde, hızlı ve etkili bir aksiyon alınır.",
                "Fabrikanın çevreye duyarlı uygulamaları (atık yönetimi, enerji kullanımı) konusunda bilgilendiriliyorum.",
                "Makine ve teçhizatların acil durdurma mekanizmaları kolay erişilebilir durumdadır.",
                
                // Üretim Verimliliği ve Süreçler (5 Soru)
                "Üretim hattındaki iş akışı, gereksiz bekleme ve hareketleri önleyecek şekilde düzenlenmiştir.",
                "Günlük üretim hedeflerimin ne olduğunu ve bu hedeflere ulaşma durumumu biliyorum.",
                "Üretim sırasında karşılaştığım küçük sorunları/arızaları kendi yetkimle çözebilirim.",
                "Üretimi aksatan sık tekrarlanan darboğazları (bottleneck) net olarak belirleyebiliyorum.",
                "İşimi yaparken kullandığım talimatlar ve prosedürler anlaşılır ve günceldir.",
                
                // Makine/Ekipman Bakımı ve Teknoloji (5 Soru)
                "Kullandığım makinelerin/ekipmanların periyodik bakımları zamanında yapılmaktadır.",
                "Üretim sırasında kullandığım el aletleri ve yardımcı ekipmanlar kaliteli ve sağlamdır.",
                "Bir makine arızası meydana geldiğinde, teknik servis ekibi hızla müdahale etmektedir.",
                "İşimi yaparken kullanılan otomasyon/dijital sistemler, işimi kolaylaştırmaktadır.",
                "Bakım veya onarım süreçlerine görüş ve önerilerimle katkı sağlayabiliyorum.",
                
                // Kalite Yönetimi ve Kontrol (5 Soru)
                "Ürettiğim parçanın/ürünün kalite standartları ve beklentileri bana net olarak aktarılmıştır.",
                "Kalitesiz ürün/parça üretmemize neden olan temel hataları biliyorum ve önleyebilirim.",
                "Ürün/parça üzerinde yaptığım kalite kontrol testleri için doğru ekipmanlara sahibim.",
                "Bir kalite sorunu tespit ettiğimde, üretim hattını durdurma veya ürünü ayırma yetkim vardır.",
                "Kalite yönetim sistemi eğitimleri pratik uygulamalarla desteklenmektedir.",
                
                // Eğitim ve Gelişim (5 Soru)
                "İşe başladığımda aldığım oryantasyon ve işbaşı eğitimi yeterliydi.",
                "Yeni bir makine/teknoloji geldiğinde, onunla ilgili detaylı eğitim alıyorum.",
                "Geliştirmek istediğim beceriler için yönetime eğitim talebinde bulunabiliyorum.",
                "Yöneticim/Amirim, kariyer hedeflerim konusunda benimle düzenli olarak konuşur.",
                "Aldığım eğitimlerin üretimdeki verimime doğrudan katkı sağladığını düşünüyorum.",
                
                // İletişim ve İşbirliği (5 Soru)
                "Amirimden günlük iş talimatlarını net ve anlaşılır şekilde alıyorum.",
                "Diğer üretim hatları/departmanlarla (lojistik, kalite vb.) iletişimimiz sorunsuz ilerlemektedir.",
                "Departman toplantıları, işleyişteki sorunları açıkça tartışmak için uygun bir ortam sunmaktadır.",
                "Yönetimin aldığı önemli kararlar bize zamanında bildirilir.",
                "Yaptığım iş hakkında düzenli geri bildirim alıyorum.",
                
                // Çalışma Koşulları ve Sosyal Haklar (5 Soru)
                "Çalıştığım alandaki aydınlatma, havalandırma ve sıcaklık koşulları yeterlidir.",
                "Fabrikanın yemekhane hizmetlerinden ve yemek kalitesinden memnunum.",
                "Dinlenme ve sosyal alanlarımız (soyma odası, mola alanı vb.) temiz ve yeterli konfordadır.",
                "Aldığım ücret ve sosyal haklar, yaptığım işin sorumluluğu ile doğru orantılıdır.",
                "Yöneticilerim, özel ve acil durumlarımda bana destek olmaktadır.",
                
                // İş Yükü ve Zaman Yönetimi (5 Soru)
                "Günlük iş yüküm dengeli ve yönetilebilir bir seviyededir.",
                "Yapılan fazla mesai (overtime) planlamaları adil ve gerekli durumlarda yapılmaktadır.",
                "Üretimdeki aksaklıklar ve arızalar, iş saatlerimi sürekli uzatmama neden olmamaktadır.",
                "İşimi zamanında bitirmem için gerekli ekip ve kaynak desteğine sahibim.",
                "Vardiya/izin planlamaları adil ve kişisel ihtiyaçlarım dikkate alınarak yapılmaktadır.",
                
                // Liderlik ve Yönetim (5 Soru)
                "Amirim/Süpervizörüm, işimi yapmam için bana yeterli özerklik ve güven vermektedir.",
                "Yöneticim, performansımı adil ve objektif kriterlere göre değerlendirmektedir.",
                "Yöneticim, ekip içinde iyi bir atmosfer yaratılmasına katkı sağlamaktadır.",
                "Yönetimin çalışanların sorunlarına gerçekten önem verdiğini düşünüyorum.",
                "Yöneticilerimiz, bizi dinler ve fikirlerimize saygı gösterir.",
                
                // Kurumsal Bağlılık ve Motivasyon (5 Soru)
                "Fabrikamızın pazardaki yeri ve geleceği hakkında iyimserim.",
                "Fabrikada yapılan sosyal aktivitelere ve etkinliklere katılmak beni motive ediyor.",
                "Yaptığım işin firmanın genel başarısına ne kadar katkı sağladığını net olarak görüyorum.",
                "Fabrikamızda uzun vadeli çalışmayı teşvik eden bir ortam vardır.",
                "Fabrikanın misyonunu ve değerlerini benimsiyorum ve işimi buna uygun yapıyorum."
            ],
            "Beyaz Yaka": [
                // İş Güvenliği ve Çevre (5 Soru)
                "Ofis ve ortak alanlarda ergonomi ve iş güvenliği standartları (kablo düzeni, sandalye vb.) sağlanmıştır.",
                "Kendi alanımla ilgili çevre mevzuatına uyum konusunda yeterli bilgiye sahibim.",
                "Yangın ve acil durumlar için tahliye planları net ve düzenli olarak tatbikatı yapılmaktadır.",
                "Ofis ortamında stres yönetimi ve ruh sağlığı konularında destek programları mevcuttur.",
                "Fabrikanın sürdürülebilirlik hedeflerine ulaşmak için kendi işimde katkıda bulunuyorum.",
                
                // Üretim Verimliliği ve Süreçler (5 Soru)
                "Kendi departmanımdan gelen veriler, üretim süreçlerinin verimliliğini doğru ölçmektedir.",
                "Fabrikanın ERP/MRP sistemi, iş süreçlerimi hızlı ve doğru yürütmeme yardımcı olmaktadır.",
                "Departmanlar arası proje ve görev akışları (örneğin satın alma-üretim) standardize edilmiştir.",
                "İş süreçlerimde gereksiz bürokrasi ve onay mekanizmaları verimliliğimi düşürmektedir.",
                "Kendi departmanım için belirlenen hedef ve KPI'lar (Ana Performans Göstergeleri) ulaşılabilirdir.",
                
                // Makine/Ekipman Bakımı ve Teknoloji (5 Soru)
                "Kullandığım IT donanımı (bilgisayar, yazılım vb.) işimi kesintisiz ve hızlı yapmamı sağlamaktadır.",
                "Fabrikanın Bakım Yönetim Sistemi (CMMS), doğru ve güncel arıza/bakım verileri sunmaktadır.",
                "Veri güvenliği ve yedekleme sistemleri konusunda kendimi güvende hissediyorum.",
                "Üretimden gelen gerçek zamanlı veri akışı, doğru kararlar almam için yeterlidir.",
                "Departmanım, yeni ve verimli teknolojilere yatırım yapılması konusunda teşvik edilmektedir.",
                
                // Kalite Yönetimi ve Kontrol (5 Soru)
                "Fabrikanın kalite yönetim sistemi (ISO vb.) dokümantasyonu anlaşılır ve günceldir.",
                "Kendi işimin nihai ürün kalitesine etkisini (örneğin satın alınan malzeme kalitesi) net olarak görebiliyorum.",
                "Tedarikçi denetimi ve hammadde kabul süreçleri, kalitesiz girdiyi önlemede etkilidir.",
                "Müşteri şikayetleri, kök neden analizi ve Düzeltici/Önleyici Faaliyet (DÖF) için etkin kullanılmaktadır.",
                "Kalite hedeflerine ulaşmak için departmanlar arası işbirliği güçlüdür.",
                
                // Eğitim ve Gelişim (5 Soru)
                "Yöneticim, mesleki gelişimim için kurs ve seminerlere katılmamı desteklemektedir.",
                "Departmanıma özel teknik ve yönetsel eğitimler yeterli sıklıkta düzenlenmektedir.",
                "Fabrika içinde yükselme ve pozisyon değiştirme fırsatları konusunda şeffaflık vardır.",
                "Performans değerlendirme sonuçları, somut gelişim planlarına dönüştürülmektedir.",
                "Aldığım eğitimler, beni sektördeki yeni trendlere hazırlamaktadır.",
                
                // İletişim ve İşbirliği (5 Soru)
                "Üretim/Mavi yaka çalışanlarla bilgi ve veri paylaşımı hızlı ve etkili bir şekilde yapılmaktadır.",
                "Yönetim, kurumsal hedefleri tüm beyaz yakaya net ve düzenli olarak aktarmaktadır.",
                "Departmanlar arası çıkar çatışmaları adil bir şekilde çözülmektedir.",
                "Çapraz fonksiyonlu projelerde sorumluluklar net tanımlanmıştır.",
                "Görüş ve önerilerim, fabrikanın genel stratejilerini etkileme potansiyeline sahiptir.",
                
                // Çalışma Koşulları ve Sosyal Haklar (5 Soru)
                "Ofis alanlarının temizliği, bakımı ve fiziksel konforu çalışma motivasyonumu artırmaktadır.",
                "Yıllık izin planlaması adil yapılmakta ve kullanmam teşvik edilmektedir.",
                "Fabrikanın sunduğu ek sosyal haklar (özel sağlık sigortası, ulaşım vb.) tatmin edicidir.",
                "Çalışma saatlerim, özel hayatımla dengeyi sağlamama olanak vermektedir.",
                "Ofis ortamında çatışma kültürü yönetimin müdahalesiyle engellenmektedir.",
                
                // İş Yükü ve Zaman Yönetimi (5 Soru)
                "Departmanımın toplam iş yükü personel sayısıyla dengelidir.",
                "Acil durumlar dışında, fazla mesai yapma ihtiyacı nadiren ortaya çıkmaktadır.",
                "İşlerimi önceliklendirme ve zamanımı yönetme konusunda yeterli özerkliğe sahibim.",
                "Yöneticilerim, iş süreçlerini iyileştirerek gereksiz iş yükünü azaltmaya odaklanmıştır.",
                "Proje teslim tarihleri ve iş hedefleri gerçekçi ve ulaşılabilirdir.",
                
                // Liderlik ve Yönetim (5 Soru)
                "Yöneticim, görev dağılımını adil ve yetkinliklere uygun şekilde yapmaktadır.",
                "Yöneticim, stratejik düşünmeme ve büyük resmi görmeme yardımcı olmaktadır.",
                "Yönetim, hatalarımızdan ders çıkarmamıza izin veren bir öğrenme kültürü teşvik etmektedir.",
                "Fabrika yönetiminin ahlaki ve etik standartlarına güveniyorum.",
                "Yöneticim, karar alma süreçlerine aktif olarak katılmamı sağlamaktadır.",
                
                // Kurumsal Bağlılık ve Motivasyon (5 Soru)
                "Fabrikanın uzun vadeli büyüme potansiyeline inanıyorum.",
                "Fabrika içinde başarılı çalışanlar düzenli ve somut olarak tanınmakta/ödüllendirilmektedir.",
                "Fabrikanın toplumsal sorumluluk projeleri kuruma olan bağlılığımı artırmaktadır.",
                "Farklı departmanlar arası etkinlikler motivasyonu desteklemektedir.",
                "Genel olarak, kariyerimi bu firmada sürdürme konusunda kendimi motive hissediyorum."
            ],
            "Yönetim": [
                // İş Güvenliği ve Çevre (5 Soru)
                "Fabrika genelinde proaktif bir iş güvenliği kültürü oluşturulmuştur (sıfır iş kazası hedefi).",
                "Çevre ve iş güvenliği yatırımları için ayrılan bütçe yeterli ve önceliklidir.",
                "Tüm seviyelerdeki çalışanların güvenlik eğitimleri ve farkındalık seviyeleri düzenli ölçülmektedir.",
                "Fabrikanın çevre mevzuatına uyum riski düşüktür ve düzenli denetimler yapılmaktadır.",
                "Güvenlik ve çevre konularındaki KPI'lar, üst yönetim toplantılarında düzenli olarak takip edilmektedir.",
                
                // Üretim Verimliliği ve Süreçler (5 Soru)
                "Fabrikanın genel ekipman etkinliği (OEE) hedeflenen sektör standardına ulaşmıştır.",
                "Tesis düzeni (layout), malzeme akışını ve üretim verimliliğini maksimuma çıkarmaktadır.",
                "Üretimdeki kayıp/atık oranları, karlılık hedeflerimizle uyumlu ve kabul edilebilir seviyededir.",
                "Üretim planlama süreçlerimiz, müşteri taleplerine hızlı yanıt verme yeteneğimizi desteklemektedir.",
                "Fabrikamız, yalın üretim (lean manufacturing) prensiplerini etkin bir şekilde uygulamaktadır.",
                
                // Makine/Ekipman Bakımı ve Teknoloji (5 Soru)
                "Toplam Üretken Bakım (TPM) stratejimiz, arıza sürelerini minimize etmede başarılıdır.",
                "Mevcut makine parkuru ve ekipman yaşı, üretim kalitemizi ve hızımızı kısıtlamamaktadır.",
                "Yeni nesil Endüstri 4.0 teknolojilerine yatırım yapmak için stratejik planımız hazırdır.",
                "Bakım ve üretim departmanları arasındaki koordinasyon ve bütçe kullanımı verimlidir.",
                "Fabrikanın dijitalleşme ve otomasyon stratejisi, uzun vadeli büyüme hedeflerini desteklemektedir.",
                
                // Kalite Yönetimi ve Kontrol (5 Soru)
                "Tedarikçi Kalite Yönetimi (SQM) süreçlerimiz, girdi kalitesinde istikrarı sağlamaktadır.",
                "Dahili ve harici hurda/fire oranları, sektör benchmark'ları ile karşılaştırıldığında rekabetçidir.",
                "Müşteri geri bildirimleri ve şikayetleri, ürün ve süreç iyileştirmeleri için temel girdi olarak kullanılmaktadır.",
                "Fabrikamız, sektörün gerektirdiği tüm kalite sertifikasyonlarına sahiptir.",
                "Kalite departmanına ayrılan personel ve teknoloji bütçesi yeterlidir.",
                
                // Eğitim ve Gelişim (5 Soru)
                "Çalışanların mevcut yetkinlikleri ve işin gerektirdiği yetkinlikler arasındaki fark düzenli ölçülmektedir.",
                "Yönetici/Liderlik geliştirme programları, şirketin gelecekteki lider ihtiyacını karşılamaktadır.",
                "Yeni işe alım ve oryantasyon programları, işgücünün adaptasyon hızını artırmaktadır.",
                "Eğitim bütçesi, hem mavi yaka hem de beyaz yaka personelinin gelişim ihtiyacını karşılamaktadır.",
                "Çapraz eğitim programları (cross-training), esnek ve yedekli bir işgücü oluşturmaktadır.",
                
                // İletişim ve İşbirliği (5 Soru)
                "Üst yönetimden tüm personele stratejik bilgi akışı şeffaf ve etkilidir.",
                "Departmanlar arası rekabet yerine işbirliği kültürünü teşvik eden mekanizmalar mevcuttur.",
                "Kriz ve acil durum iletişim planları, tüm paydaşlara bilgi akışını güvence altına almaktadır.",
                "Yönetim kurulu kararları ve hedefler, tüm seviyelerdeki çalışan KPI'larına doğru bir şekilde yayılmıştır.",
                "Çalışanların fikirlerini üst yönetime ulaştıran kanallar etkin çalışmaktadır.",
                
                // Çalışma Koşulları ve Sosyal Haklar (5 Soru)
                "Toplam işgücü maliyeti, pazardaki rekabet avantajımızı koruyarak çalışan memnuniyetini sağlamaktadır.",
                "İnsan kaynakları politikalarımız, çeşitliliği ve eşit fırsatları desteklemektedir.",
                "Çalışanların sağlık ve refahı (wellness) için aktif programlar yürütülmektedir.",
                "Çalışan devir hızımız (turnover rate) sektör ortalamasının altındadır.",
                "Fabrikanın fiziksel koşullarına yapılan yatırımlar, üretkenliği ve işyeri güvenliğini artırmıştır.",
                
                // İş Yükü ve Zaman Yönetimi (5 Soru)
                "Üretim ve ofis personeli için uygulanan esnek çalışma modelleri, verimliliği artırmaktadır.",
                "Fazla mesai bütçesi, hedeflenen maliyet ve verimlilik dengesini korumaktadır.",
                "İş etüdü ve zaman analizleri düzenli yapılarak iş süreçleri optimize edilmektedir.",
                "Yönetici seviyesinde, iş yükü ve stres yönetimi konusunda destekleyici programlar uygulanmaktadır.",
                "Büyük projelerin zaman ve kaynak planlaması doğru yapılmaktadır.",
                
                // Liderlik ve Yönetim (5 Soru)
                "Fabrikada çalışanlara yetki verme ve sorumluluk alma kültürü teşvik edilmektedir.",
                "Yöneticilerin koçluk ve mentorluk becerileri, çalışan gelişimini desteklemektedir.",
                "Fabrika yönetiminin uzun vadeli vizyonu net ve ilham vericidir.",
                "Performans yönetim sistemi, çalışanları sadece geçmiş sonuçlara göre değil, potansiyele göre de değerlendirmektedir.",
                "Yönetim olarak, değişime hızlı adapte olma yeteneğimiz yüksektir.",
                
                // Kurumsal Bağlılık ve Motivasyon (5 Soru)
                "Kurumsal itibarımız, sektörde kalifiye personel çekme ve elde tutma konusunda avantaj sağlamaktadır.",
                "Çalışan memnuniyeti anket sonuçları, düzenli olarak aksiyon planlarına dönüştürülmektedir.",
                "Ödüllendirme ve takdir sistemi, çalışanların motivasyonunu ve bağlılığını artırmaktadır.",
                "Fabrikanın finansal performansı hakkında çalışanlarla şeffaf iletişim kurulmaktadır.",
                "Yönetim, çalışanlar arasında aidiyet duygusunu güçlendirecek etkinliklere aktif olarak destek vermektedir."
            ]
        };

        // Sistem verileri
        let systemData = {
            adminPassword: '030714',
            surveyData: null
        };

        // Sayfa yüklendiğinde
        document.addEventListener('DOMContentLoaded', function() {
            setupEventListeners();
            showModule('survey');
            // loadDemoData();
        });

        function setupEventListeners() {
            // Sorumluluk reddi checkbox
            document.getElementById('acceptDisclaimer').addEventListener('change', function() {
                const section = document.getElementById('companyInfoSection');
                if (this.checked) {
                    section.classList.remove('opacity-50', 'pointer-events-none');
                } else {
                    section.classList.add('opacity-50', 'pointer-events-none');
                }
            });

            // İş türü seçimi
            document.getElementById('blueCollar').addEventListener('click', () => selectJobType('Mavi Yaka'));
            document.getElementById('whiteCollar').addEventListener('click', () => selectJobType('Beyaz Yaka'));
            document.getElementById('management').addEventListener('click', () => selectJobType('Yönetim'));
            
            // Anket başlatma
            document.getElementById('startSurvey').addEventListener('click', startSurvey);
            
            // Anket tamamlama
            document.getElementById('submitSurvey').addEventListener('click', submitSurvey);

            // Enter tuşu ile giriş
            document.getElementById('companyPassword').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') loginCompany();
            });
            
            document.getElementById('adminPassword').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') loginAdmin();
            });
        }

        function showModule(module) {
            // Tüm modülleri gizle
            document.getElementById('surveyModule').classList.add('hidden');
            document.getElementById('companyModule').classList.add('hidden');
            document.getElementById('adminModule').classList.add('hidden');
            
            // Seçili modülü göster
            document.getElementById(module + 'Module').classList.remove('hidden');
            currentModule = module;
        }

        function selectJobType(jobType) {
            selectedJobType = jobType;
            
            // Tüm butonları sıfırla
            document.querySelectorAll('#blueCollar, #whiteCollar, #management').forEach(btn => {
                btn.classList.remove('active-tab');
            });
            
            // Seçili butonu işaretle
            const buttonMap = {
                'Mavi Yaka': 'blueCollar',
                'Beyaz Yaka': 'whiteCollar',
                'Yönetim': 'management'
            };
            
            document.getElementById(buttonMap[jobType]).classList.add('active-tab');
        }

        function startSurvey() {
            const companyName = document.getElementById('companyName').value.trim();
            const firstName = document.getElementById('firstName').value.trim();
            const lastName = document.getElementById('lastName').value.trim();
            const disclaimerAccepted = document.getElementById('acceptDisclaimer').checked;

            // Google Sign-In enforcement
            if (!googleUser) {
                showModal(
                    '🔒 Giriş Gerekli',
                    `<div class="text-2xl font-extrabold text-red-700 mb-4">Google ile Giriş Yapmalısınız</div>
                    <div class="text-base text-gray-800 mb-2">Ankete başlamadan önce kimliğinizi doğrulamanız gerekmektedir.</div>
                    <ul class="list-disc pl-6 text-base text-gray-700 mb-4">
                        <li>Yukarıdaki <b>Google ile Giriş Yap</b> butonunu kullanarak hesabınızla oturum açın.</li>
                        <li>Giriş yaptıktan sonra ad ve soyad alanlarınız otomatik doldurulacak ve düzenlenebilir olacaktır.</li>
                        <li>Gizliliğiniz korunur, bilgileriniz üçüncü kişilerle paylaşılmaz.</li>
                    </ul>
                    <div class="text-sm text-gray-500">Herhangi bir sorun yaşarsanız lütfen yöneticinizle iletişime geçin.</div>`
                );
                return;
            }

            if (!disclaimerAccepted) {
                showModal('⚠️ Uyarı', 'Devam etmek için sorumluluk reddi beyanını kabul etmelisiniz.');
                return;
            }

            if (!companyName || !selectedJobType) {
                showModal('⚠️ Eksik Bilgi', 'Lütfen şirket adını girin ve iş türünüzü seçin.');
                return;
            }

            if (!firstName || !lastName) {
                showModal('⚠️ Eksik Bilgi', 'Lütfen adınızı ve soyadınızı girin.');
                return;
            }

            currentQuestions = questions[selectedJobType];
            currentQuestionIndex = 0;
            answers = [];
            surveyStartTime = new Date();

            // Anket bölümünü göster
            document.getElementById('disclaimerSection').classList.add('hidden');
            document.getElementById('companyInfoSection').classList.add('hidden');
            document.getElementById('surveySection').classList.remove('hidden');

            startTimer();
            displayCurrentQuestion();
        }

        function startTimer() {
            timerInterval = setInterval(() => {
                const elapsed = Math.floor((new Date() - surveyStartTime) / 1000);
                const minutes = Math.floor(elapsed / 60);
                const seconds = elapsed % 60;
                document.getElementById('timeElapsed').textContent = 
                    `Süre: ${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            }, 1000);
        }

        function displayCurrentQuestion() {
            const container = document.getElementById('questionContainer');
            const question = currentQuestions[currentQuestionIndex];
            
            container.innerHTML = `
                <div class="bg-white p-3 sm:p-6 rounded-2xl border border-purple-200 shadow-md">
                    <h3 class="text-base sm:text-lg font-semibold mb-4 text-gray-800">${question}</h3>
                    <div class="grid grid-cols-2 sm:grid-cols-5 gap-2 sm:gap-3 w-full">
                        <button onclick="selectAnswer(1)" class="answer-btn flex flex-col items-center justify-center py-3 px-2 text-xs sm:text-base rounded-xl border-2 border-red-200 hover:border-red-400 hover:bg-red-50 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-red-400 bg-gray-50 shadow-sm">
                            <span class="text-xl sm:text-2xl mb-1 font-bold text-red-500">1</span>
                            <span class="font-medium text-gray-700 leading-tight text-center">Hiç Memnun<br>Değilim</span>
                        </button>
                        <button onclick="selectAnswer(2)" class="answer-btn flex flex-col items-center justify-center py-3 px-2 text-xs sm:text-base rounded-xl border-2 border-orange-200 hover:border-orange-400 hover:bg-orange-50 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-orange-400 bg-gray-50 shadow-sm">
                            <span class="text-xl sm:text-2xl mb-1 font-bold text-orange-500">2</span>
                            <span class="font-medium text-gray-700 leading-tight text-center">Memnun<br>Değilim</span>
                        </button>
                        <button onclick="selectAnswer(3)" class="answer-btn flex flex-col items-center justify-center py-3 px-2 text-xs sm:text-base rounded-xl border-2 border-yellow-200 hover:border-yellow-400 hover:bg-yellow-50 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-yellow-400 bg-gray-50 shadow-sm col-span-2 sm:col-span-1">
                            <span class="text-xl sm:text-2xl mb-1 font-bold text-yellow-600">3</span>
                            <span class="font-medium text-gray-700 leading-tight text-center">Kararsızım</span>
                        </button>
                        <button onclick="selectAnswer(4)" class="answer-btn flex flex-col items-center justify-center py-3 px-2 text-xs sm:text-base rounded-xl border-2 border-green-200 hover:border-green-400 hover:bg-green-50 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-green-400 bg-gray-50 shadow-sm">
                            <span class="text-xl sm:text-2xl mb-1 font-bold text-green-500">4</span>
                            <span class="font-medium text-gray-700 leading-tight text-center">Memnunum</span>
                        </button>
                        <button onclick="selectAnswer(5)" class="answer-btn flex flex-col items-center justify-center py-3 px-2 text-xs sm:text-base rounded-xl border-2 border-blue-200 hover:border-blue-400 hover:bg-blue-50 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-blue-400 bg-gray-50 shadow-sm">
                            <span class="text-xl sm:text-2xl mb-1 font-bold text-blue-500">5</span>
                            <span class="font-medium text-gray-700 leading-tight text-center">Çok Memnunum</span>
                        </button>
                    </div>
                </div>
            `;
            
            updateProgress();
        }

        function selectAnswer(score) {
            answers.push({
                question: currentQuestions[currentQuestionIndex],
                score: score,
                timestamp: new Date().toISOString()
            });
            
            currentQuestionIndex++;
            
            if (currentQuestionIndex < currentQuestions.length) {
                displayCurrentQuestion();
            } else {
                showSubmitButton();
            }
        }

        function updateProgress() {
            const progress = (currentQuestionIndex / currentQuestions.length) * 100;
            document.getElementById('progressBar').style.width = progress + '%';
            document.getElementById('progressText').textContent = 
                `Anket İlerlemesi ${currentQuestionIndex}/${currentQuestions.length} Yanıtlandı`;
        }

        function showSubmitButton() {
            clearInterval(timerInterval);
            document.getElementById('questionContainer').innerHTML = `
                <div class="text-center bg-green-50 p-8 rounded-lg border-2 border-green-200">
                    <div class="text-6xl mb-4">🎉</div>
                    <h3 class="text-xl font-semibold text-green-800 mb-4">Tebrikler!</h3>
                    <p class="text-green-700 mb-4">Tüm soruları yanıtladınız. Anketi tamamlamak için aşağıdaki butona tıklayın.</p>
                    <div class="text-sm text-green-600">
                        <p>Toplam süre: ${document.getElementById('timeElapsed').textContent.split(': ')[1]}</p>
                    </div>
                </div>
            `;
            document.getElementById('submitSurvey').classList.remove('hidden');
            updateProgress();
        }


        // Firebase Realtime Database API fonksiyonları (GLOBAL SCOPE)
        const FIREBASE_DB_URL = 'https://isletme-76bad-default-rtdb.europe-west1.firebasedatabase.app/';

        async function loadFromFirebase() {
            try {
                const response = await fetch(FIREBASE_DB_URL + 'surveyData.json');
                if (!response.ok) throw new Error('Firebase veri yükleme hatası');
                const data = await response.json();
                systemData.surveyData = data || { companies: {}, responses: [], statistics: {} };
                return systemData.surveyData;
            } catch (error) {
                console.error('Firebase yükleme hatası:', error);
                systemData.surveyData = { companies: {}, responses: [], statistics: {} };
                return systemData.surveyData;
            }
        }

        async function saveToFirebase(data) {
            try {
                const response = await fetch(FIREBASE_DB_URL + 'surveyData.json', {
                    method: 'PUT',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(data)
                });
                if (!response.ok) return { success: false, error: 'Firebase veri kaydetme hatası' };
                return { success: true };
            } catch (error) {
                console.error('Firebase bağlantı hatası:', error);
                return { success: false, error: error.message };
            }
        }

        async function createCompanyIfNotExists(companyName) {
            try {
                if (!systemData.surveyData) await loadFromFirebase();
                // Mevcut şirket var mı kontrol et
                const existingCompany = Object.entries(systemData.surveyData.companies || {})
                    .find(([key, company]) => company.name.toLowerCase() === companyName.toLowerCase());
                if (existingCompany) {
                    return { success: true, key: existingCompany[0], password: existingCompany[1].password };
                }
                // Yeni şirket oluştur
                const companyKey = companyName.toLowerCase().replace(/[^a-z0-9]/g, '').substring(0, 10) + '-' + Date.now();
                const newPassword = generateCompanyPassword();
                if (!systemData.surveyData.companies) systemData.surveyData.companies = {};
                systemData.surveyData.companies[companyKey] = {
                    name: companyName,
                    password: newPassword,
                    createdAt: new Date().toISOString(),
                    totalResponses: 0,
                    status: 'aktif'
                };
                const saveResult = await saveToFirebase(systemData.surveyData);
                if (saveResult.success) {
                    return { success: true, key: companyKey, password: newPassword };
                } else {
                    return { success: false, error: saveResult.error };
                }
            } catch (error) {
                return { success: false, error: error.message };
            }
        }

        function generateCompanyPassword() {
            const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
            let password = '';
            for (let i = 0; i < 12; i++) {
                password += chars.charAt(Math.floor(Math.random() * chars.length));
            }
            return password;
        }

        async function submitSurvey() {
            try {
                console.log('Anket gönderiliyor...');
                const companyName = document.getElementById('companyName').value.trim();
                const firstName = document.getElementById('firstName').value.trim() || 'Anonim';
                const lastName = document.getElementById('lastName').value.trim() || 'Kullanıcı';
                if (!companyName || !selectedJobType || !answers || answers.length === 0) {
                    throw new Error('Eksik bilgi: Şirket adı, iş türü ve anket yanıtları gerekli');
                }
                console.log('Anket verileri:', { companyName, firstName, lastName, selectedJobType, answersCount: answers.length });
                // Önce şirket oluştur/bul
                const companyResult = await createCompanyIfNotExists(companyName);
                console.log('Şirket işlem sonucu:', companyResult);
                if (!companyResult.success) {
                    throw new Error(`Şirket işlemi başarısız: ${companyResult.error}`);
                }
                // Firebase'den güncel veriyi çek (REST API)
                let data = await loadFromFirebase();
                const surveyResponse = {
                    id: 'survey_' + Date.now() + '_' + Math.random().toString(36).substr(2, 9),
                    companyName: companyName,
                    firstName: firstName,
                    lastName: lastName,
                    jobType: selectedJobType,
                    answers: answers,
                    submittedAt: new Date().toISOString(),
                    totalScore: answers.reduce((sum, answer) => sum + answer.score, 0),
                    averageScore: (answers.reduce((sum, answer) => sum + answer.score, 0) / answers.length).toFixed(2),
                    duration: document.getElementById('timeElapsed').textContent.split(': ')[1] || '00:00'
                };
                console.log('Anket yanıtı hazırlandı:', surveyResponse);
                if (!data.responses) data.responses = [];
                data.responses.push(surveyResponse);
                // İstatistikleri güncelle
                if (!data.statistics) {
                    data.statistics = {
                        totalResponses: 0,
                        averageScore: 0,
                        lastUpdated: new Date().toISOString()
                    };
                }
                data.statistics.totalResponses = data.responses.length;
                data.statistics.averageScore = (
                    data.responses.reduce((sum, r) => sum + parseFloat(r.averageScore), 0) / 
                    data.responses.length
                ).toFixed(2);
                data.statistics.lastUpdated = new Date().toISOString();
                // Şirket istatistiklerini güncelle
                if (companyResult && data.companies[companyResult.key]) {
                    data.companies[companyResult.key].totalResponses = 
                        data.responses.filter(r => 
                            r.companyName.toLowerCase() === companyName.toLowerCase()
                        ).length;
                }
                // Firebase'e kaydet (REST API)
                await saveToFirebase(data);
                systemData.surveyData = data;
                // Başarı mesajı göster
                document.getElementById('surveySection').innerHTML = `
                    <div class="text-center bg-green-50 p-8 rounded-lg border-2 border-green-200">
                        <div class="text-6xl mb-4">✅</div>
                        <h2 class="text-2xl font-bold text-green-800 mb-4">Anketiniz Başarıyla Kaydedildi!</h2>
                        <p class="text-green-700 mb-4">
                            Değerli görüşleriniz için teşekkür ederiz. Anket yanıtlarınız güvenli bir şekilde kaydedildi.
                        </p>
                        <div class="bg-blue-50 p-4 rounded-lg border border-blue-200 mb-4">
                            <p class="text-sm text-blue-700">
                                <strong>📊 Raporlama Bilgisi:</strong> Anket sonuçlarınız güvenli bir şekilde kaydedildi. 
                                Şirket yöneticiniz raporları görüntüleyebilir ve analiz edebilir.
                            </p>
                        </div>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
                            <button onclick="showModule('company')" class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition-colors">
                                🏢 Şirket Portalına Git
                            </button>
                            <button onclick="location.reload()" class="bg-purple-600 text-white px-4 py-2 rounded-lg hover:bg-purple-700 transition-colors">
                                🔄 Yeni Anket Başlat
                            </button>
                        </div>
                    </div>
                `;
            } catch (error) {
                console.error('Anket gönderme hatası:', error);
                showModal('❌ Hata', `Anket gönderilirken bir hata oluştu:<br><br><strong>Hata:</strong> ${error.message}<br><br>Lütfen sayfayı yenileyip tekrar deneyin.`);
            }
        }

        async function loginCompany() {
            const companyName = document.getElementById('companyLoginName').value.trim();
            const password = document.getElementById('companyPassword').value.trim();
            
            if (!companyName || !password) {
                showModal('⚠️ Eksik Bilgi', 'Lütfen şirket adı ve şifrenizi girin.');
                return;
            }
            
            try {
                // if (!systemData.surveyData) {
                //     systemData.surveyData = await loadFromJSONBin();
                // }
                
                // Şirket bilgilerini kontrol et
                const companyEntry = Object.entries(systemData.surveyData.companies || {})
                    .find(([key, company]) => 
                        company.name.toLowerCase() === companyName.toLowerCase() && 
                        company.password === password
                    );
                if (companyEntry) {
                    // Pasif şirket kontrolü
                    if (companyEntry[1].status === 'pasif') {
                        showModal('⛔ Askıya Alındı', 'Bu şirket askıya alınmış/dondurulmuş. Lütfen yöneticinizle iletişime geçin.');
                        return;
                    }
                    loggedInCompany = {
                        key: companyEntry[0],
                        ...companyEntry[1]
                    };
                    document.getElementById('companyLogin').classList.add('hidden');
                    document.getElementById('companyDashboard').classList.remove('hidden');
                    loadCompanyDashboard();
                } else {
                    showModal('❌ Giriş Hatası', 'Şirket adı veya şifre hatalı. Lütfen yöneticinizden doğru bilgileri alın.');
                }
            } catch (error) {
                // Eğer modal zaten açıksa (ör: pasif şirket uyarısı), tekrar hata modalı gösterme
                const modal = document.getElementById('modal');
                if (!modal.classList.contains('show')) {
                    showModal('❌ Hata', 'Giriş sırasında bir hata oluştu. Lütfen tekrar deneyin.');
                }
                console.error('Giriş hatası:', error);
            }
        }

        let filteredSurveys = null;
        function loadCompanyDashboard() {
            if (!loggedInCompany || !systemData.surveyData) return;
            document.getElementById('companyNameDisplay').textContent = loggedInCompany.name;
            const companySurveys = systemData.surveyData.responses.filter(s => 
                s.companyName.toLowerCase() === loggedInCompany.name.toLowerCase()
            );
            filteredSurveys = null;
            updateDashboardData(companySurveys);
        }

        function filterByDateRange() {
            if (!loggedInCompany || !systemData.surveyData) return;
            const start = document.getElementById('reportStartDate').value;
            const end = document.getElementById('reportEndDate').value;
            const allSurveys = systemData.surveyData.responses.filter(s => 
                s.companyName.toLowerCase() === loggedInCompany.name.toLowerCase()
            );
            if (!start && !end) {
                filteredSurveys = null;
                updateDashboardData(allSurveys);
                return;
            }
            const startDate = start ? new Date(start) : null;
            const endDate = end ? new Date(end) : null;
            const filtered = allSurveys.filter(s => {
                const d = new Date(s.submittedAt);
                if (startDate && d < startDate) return false;
                if (endDate) {
                    // Bitiş gününü de dahil et
                    const endOfDay = new Date(endDate);
                    endOfDay.setHours(23,59,59,999);
                    if (d > endOfDay) return false;
                }
                return true;
            });
            filteredSurveys = filtered;
            updateDashboardData(filtered);
        }

        function updateDashboardData(surveys) {
            document.getElementById('totalParticipants').textContent = surveys.length;
            if (surveys.length > 0) {
                let totalScore = 0;
                let totalAnswers = 0;
                surveys.forEach(s => {
                    totalScore += s.totalScore;
                    totalAnswers += s.answers.length;
                });
                const avgScore = totalAnswers > 0 ? (totalScore / totalAnswers).toFixed(1) : '0.0';
                document.getElementById('averageScore').textContent = avgScore;
                let highSatisfactionAnswers = 0;
                surveys.forEach(s => {
                    s.answers.forEach(answer => {
                        if (answer.score >= 4) highSatisfactionAnswers++;
                    });
                });
                const overallSatisfactionPercent = totalAnswers > 0 ? 
                    Math.round((highSatisfactionAnswers / totalAnswers) * 100) : 0;
                document.getElementById('satisfactionRate').textContent = overallSatisfactionPercent + '%';
            } else {
                document.getElementById('averageScore').textContent = '0.0';
                document.getElementById('satisfactionRate').textContent = '0%';
            }
            generateSimpleReport(surveys);
            generateCharts(surveys);
            
            // Eğer katılımcı tablosu açıksa onu da güncelle
            const participantWrapper = document.getElementById('participantListTableWrapper');
            if (participantWrapper && !participantWrapper.classList.contains('hidden')) {
                loadParticipantTable();
            }
            
            // Katılımcı listesini güncelle (en alt tablo)
            updateParticipantListTable(surveys);
        // Katılımcı listesini güncelleyen fonksiyon (en alt tablo)
        function updateParticipantListTable(surveys) {
            const tbody = document.getElementById('participantListBody');
            if (!tbody) return;
            tbody.innerHTML = '';
            surveys.forEach(s => {
                // İsim ve soyisim için tüm olasılıkları kontrol et
                let name = '';
                if (s.name || s.surname) {
                    name = ((s.name || '') + ' ' + (s.surname || '')).trim();
                } else if (s.firstName || s.lastName) {
                    name = ((s.firstName || '') + ' ' + (s.lastName || '')).trim();
                } else if (s.fullName) {
                    name = s.fullName;
                } else if (s.adSoyad) {
                    name = s.adSoyad;
                } else {
                    name = '-';
                }
                const job = s.jobType || s.unvan || s.title || '';
                let total = 0, count = 0;
                if (Array.isArray(s.answers)) {
                    s.answers.forEach(a => {
                        if (a && typeof a.score === 'number') {
                            total += a.score;
                            count++;
                        }
                    });
                }
                const avg = count > 0 ? (total / count).toFixed(2) : '-';
                const tarih = s.submittedAt ? new Date(s.submittedAt).toLocaleDateString('tr-TR') : '';
                tbody.innerHTML += `<tr><td class="px-3 py-2">${name}</td><td class="px-3 py-2">${job}</td><td class="px-3 py-2 text-center">${avg}</td><td class="px-3 py-2 text-center">${tarih}</td></tr>`;
            });
        }
        // Katılımcı detaylarını göster/gizle
        function toggleParticipantDetails() {
            const details = document.getElementById('participantDetails');
            if (!details) return;
            // Her açılışta tabloyu güncelle
            if (details.classList.contains('hidden')) {
                // Filtreli veri varsa onu kullan
                let surveys = (typeof filteredSurveys !== 'undefined' && filteredSurveys !== null) ? filteredSurveys : systemData.surveyData.responses.filter(s => s.companyName.toLowerCase() === loggedInCompany.name.toLowerCase());
                updateParticipantTable(surveys);
            }
            details.classList.toggle('hidden');
            const btn = document.getElementById('toggleParticipantsBtn');
            if (btn) {
                btn.textContent = details.classList.contains('hidden') ? '📋 Katılımcıları Görüntüle' : '👁️ Katılımcıları Gizle';
            }
        }
        }

        function renderParticipantList(surveys) {
            let html = `<table class="table-auto w-full text-xs"><thead><tr><th>İsim Soyisim</th><th>Ortalama Puan</th></tr></thead><tbody>`;
            surveys.forEach(s => {
                // İsim ve soyisim
                const name = (s.name || "") + " " + (s.surname || "");
                // Ortalama puan
                let total = 0, count = 0;
                if (Array.isArray(s.answers)) {
                    s.answers.forEach(a => {
                        if (a && typeof a.score === 'number') {
                            total += a.score;
                            count++;
                        }
                    });
                }
                const avg = count > 0 ? (total / count).toFixed(2) : "-";
                html += `<tr><td>${name.trim()}</td><td>${avg}</td></tr>`;
            });
            html += `</tbody></table>`;
            return html;
        }

        function generateSimpleReport(surveys) {
            if (surveys.length === 0) {
                document.getElementById('detailedReport').innerHTML = '<p class="text-gray-500 text-center py-8">Henüz anket verisi bulunmuyor.</p>';
                return;
            }

            // Grup ve kategori başlıkları
            const groups = [
                {
                    name: 'Mavi Yaka',
                    categories: [
                        'İş Güvenliği ve Çevre',
                        'Üretim Verimliliği ve Süreçler',
                        'Makine/Ekipman Bakımı ve Teknoloji',
                        'Kalite Yönetimi ve Kontrol',
                        'Eğitim ve Gelişim',
                        'İletişim ve İşbirliği',
                        'Çalışma Koşulları ve Sosyal Haklar',
                        'İş Yükü ve Zaman Yönetimi',
                        'Liderlik ve Yönetim',
                        'Kurumsal Bağlılık ve Motivasyon'
                    ]
                },
                {
                    name: 'Beyaz Yaka',
                    categories: [
                        'İş Güvenliği ve Çevre',
                        'Üretim Verimliliği ve Süreçler',
                        'Makine/Ekipman Bakımı ve Teknoloji',
                        'Kalite Yönetimi ve Kontrol',
                        'Eğitim ve Gelişim',
                        'İletişim ve İşbirliği',
                        'Çalışma Koşulları ve Sosyal Haklar',
                        'İş Yükü ve Zaman Yönetimi',
                        'Liderlik ve Yönetim',
                        'Kurumsal Bağlılık ve Motivasyon'
                    ]
                },
                {
                    name: 'Yönetim',
                    categories: [
                        'İş Güvenliği ve Çevre',
                        'Üretim Verimliliği ve Süreçler',
                        'Makine/Ekipman Bakımı ve Teknoloji',
                        'Kalite Yönetimi ve Kontrol',
                        'Eğitim ve Gelişim',
                        'İletişim ve İşbirliği',
                        'Çalışma Koşulları ve Sosyal Haklar',
                        'İş Yükü ve Zaman Yönetimi',
                        'Liderlik ve Yönetim',
                        'Kurumsal Bağlılık ve Motivasyon'
                    ]
                }
            ];
            // 1..5 puan eşlemesi: [1=Hiç Memnun Değilim, 2=Memnun Değilim, 3=Kararsızım, 4=Memnunum, 5=Çok Memnunum]
            // Tablo başlıkları düşükten yükseğe sıralanmalı (soldan sağa)
            const satisfactionLabels = ['Hiç Memnun Değilim', 'Memnun Değilim', 'Kararsızım', 'Memnunum', 'Çok Memnunum'];

            // Soru index aralıkları (örnek, gerçek indexler soru setine göre ayarlanmalı)
            const groupRanges = {
                'Mavi Yaka': [0, 49],
                'Beyaz Yaka': [50, 99],
                'Yönetim': [100, 149]
            };

            // Her grup ve kategori için frekansları hesapla
            function getCategoryIndexes(group, catIdx) {
                // Her kategori 5 soru ise:
                const start = groupRanges[group][0] + catIdx * 5;
                const end = start + 4;
                return [start, end];
            }

            // Profesyonel frekans tablosu oluştur
            let table = `<div class="overflow-x-auto mb-6">
                <table class="survey-table">
                    <thead>
                        <tr>
                            <th style="text-align: left; width: 250px;">Grup / Kategori</th>
                            ${satisfactionLabels.map(l => `<th style="width: 120px;">${l}</th>`).join('')}
                        </tr>
                    </thead>
                    <tbody>`;

            groups.forEach(group => {
                // Grup genel yüzdeleri
                const groupSurveys = surveys.filter(s => s.jobType === group.name);
                let groupCounts = [0, 0, 0, 0, 0];
                let groupTotal = 0;
                groupSurveys.forEach(s => {
                    s.answers.forEach(a => {
                        groupCounts[a.score - 1]++;
                        groupTotal++;
                    });
                });
                let groupPercents = groupCounts.map(c => groupTotal ? (c * 100 / groupTotal).toFixed(1) + '%' : '0.0%');
                table += `<tr class="main-group-row">
                    <td style="text-align: left; font-weight: 700;">${group.name}</td>
                    ${groupPercents.map(p => `<td style="font-weight: 600;">${p}</td>`).join('')}
                </tr>`;

                // Kategoriler
                group.categories.forEach((cat, catIdx) => {
                    let catCounts = [0, 0, 0, 0, 0];
                    let catTotal = 0;
                    groupSurveys.forEach(s => {
                        // Sadece ilgili kategoriye ait sorular
                        const [start, end] = getCategoryIndexes(group.name, catIdx);
                        for (let i = start; i <= end && i < s.answers.length; i++) {
                            const score = s.answers[i]?.score;
                            if (score >= 1 && score <= 5) {
                                catCounts[score - 1]++;
                                catTotal++;
                            }
                        }
                    });
                    // Kategori satırı oluştur
                    if (catTotal === 0 && groupTotal > 0) {
                        // Kategoriye ait sorular yoksa, grup sayılarını göster
                        table += `<tr>
                            <td class="sub-category">${cat}</td>
                            ${groupCounts.map(c => `<td style="text-align: center;">${c}</td>`).join('')}
                        </tr>`;
                    } else {
                        table += `<tr>
                            <td class="sub-category">${cat}</td>
                            ${catCounts.map(c => `<td style="text-align: center;">${c}</td>`).join('')}
                        </tr>`;
                    }
                });
            });
            table += '</tbody></table></div>';

            // Eski özet raporları da koru
            // Pozisyon dağılımı
            const positionData = {};
            surveys.forEach(s => {
                positionData[s.jobType] = (positionData[s.jobType] || 0) + 1;
            });
            // Memnuniyet dağılımı - kişi bazında
            const satisfactionLevels = ['Düşük (1-2)', 'Orta (3)', 'Yüksek (4-5)'];
            const satisfactionCounts = [0, 0, 0];
            surveys.forEach(s => {
                const avgScore = parseFloat(s.averageScore);
                if (avgScore < 2.5) satisfactionCounts[0]++;
                else if (avgScore >= 2.5 && avgScore < 3.5) satisfactionCounts[1]++;
                else satisfactionCounts[2]++;
            });
            const report = `
                ${table}
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="bg-blue-50 p-4 rounded-lg">
                        <h4 class="font-semibold text-blue-800 mb-3">👥 Pozisyon Dağılımı</h4>
                        ${Object.entries(positionData).map(([pos, count]) => 
                            `<div class="flex justify-between text-sm mb-1">
                                <span>${pos}:</span>
                                <span class="font-semibold">${count} kişi</span>
                            </div>`
                        ).join('')}
                    </div>
                    <div class="bg-green-50 p-4 rounded-lg">
                        <h4 class="font-semibold text-green-800 mb-3">📊 Memnuniyet Seviyeleri</h4>
                        ${satisfactionLevels.map((level, i) => 
                            `<div class="flex justify-between text-sm mb-1">
                                <span>${level}:</span>
                                <span class="font-semibold">${satisfactionCounts[i]} cevap</span>
                            </div>`
                        ).join('')}
                    </div>
                </div>
                <div class="mt-4 bg-gray-50 p-4 rounded-lg">
                    <h4 class="font-semibold text-gray-800 mb-2">📈 Özet</h4>
                    <p class="text-sm text-gray-700">
                        Toplam ${surveys.length} çalışan anketi tamamladı. 
                        Ortalama memnuniyet skoru ${(surveys.reduce((sum, s) => sum + parseFloat(s.averageScore), 0) / surveys.length).toFixed(1)}/5.0 olarak hesaplandı.
                    </p>
                </div>
            `;
            
            // Grafik alanı ekle
            const totalResponses = surveys.length;
            const chartTitle = totalResponses > 0 ? 
                `📊 Grup Bazlı Memnuniyet Dağılımı (${totalResponses} Çalışan)` : 
                '📊 Grup Bazlı Memnuniyet Dağılımı (Veri Yok)';
                
            const chartSection = `
                <div class="mt-8 bg-white border rounded-lg p-6" style="box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
                    <h3 class="text-lg font-semibold mb-4 text-gray-800">${chartTitle}</h3>
                    <div style="width: 100%; height: 400px;">
                        <canvas id="companyChart"></canvas>
                    </div>
                    ${totalResponses === 0 ? 
                        '<p class="text-gray-500 text-center mt-4">Henüz anket verisi bulunmuyor. Grafik veriler geldiğinde otomatik olarak güncellenecektir.</p>' : 
                        '<p class="text-gray-600 text-sm text-center mt-4">Grafik tüm gruplardan gelen cevapları birleştirerek genel memnuniyet dağılımını gösterir.</p>'
                    }
                </div>
            `;
            
            document.getElementById('detailedReport').innerHTML = report + chartSection;
            // AI ile Yorumla butonu ve API key inputu ekle
            document.getElementById('detailedReport').innerHTML += `
                <div class="mt-6 flex flex-col md:flex-row gap-2 items-center justify-center">
                    <button id="aiInterpretBtn" class="bg-gradient-to-r from-blue-600 to-purple-600 text-white px-6 py-3 rounded-lg font-bold text-sm hover:from-blue-700 hover:to-purple-700 transition-all duration-300 transform hover:scale-105 shadow-lg">
                        🤖 Ananliz Pro X AI ile Yorumla
                    </button>
                </div>
            `;
            // Buton eventini ekle
            setTimeout(() => {
                const btn = document.getElementById('aiInterpretBtn');
                if (btn) btn.onclick = async function() {
                    const apiKey = 'AIzaSyCJXufO8b2AMWRZpw-QctHSWgWSg2j8L1Y';
                    btn.disabled = true;
                    btn.textContent = 'AI analiz yapıyor...';
                    try {
                        // Anket özetini ve gruplama verilerini hazırla
                        const summary = document.getElementById('detailedReport').innerText.slice(0, 2000);
                        const prompt = `Bir insan kaynakları uzmanı gibi aşağıdaki anket raporunu analiz et.\n\nRapor Özeti:\n${summary}\n\nAşağıdaki başlıklarla detaylı, profesyonel ve uygulanabilir bir analiz yaz:\n\n1. Mevcut Durum\n2. Ne Yapılmalı\n3. Böyle Giderse Ne Olur\n\nHer başlık için en az 3-4 cümlelik, özgün ve açıklayıcı bir metin oluştur.\n`;
                        const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent`, {
                            method: 'POST',
                            headers: { 
                                'Content-Type': 'application/json',
                                'x-goog-api-key': apiKey
                            },
                            body: JSON.stringify({
                                contents: [{ parts: [{ text: prompt }] }]
                            })
                        });
                        if (!response.ok) throw new Error('API Hatası: ' + response.status);
                        const result = await response.json();
                        let text = (result.candidates && result.candidates[0] && result.candidates[0].content && result.candidates[0].content.parts[0].text) || 'AI yanıtı alınamadı.';
                        document.getElementById('aiInterpretationContent').innerHTML = `<pre class="whitespace-pre-wrap bg-gray-50 p-4 rounded text-sm border">${text}</pre>`;
                        document.getElementById('aiInterpretationModal').classList.add('show');
                    } catch (e) {
                        alert('AI yorumlama hatası: ' + e.message);
                    } finally {
                        btn.disabled = false;
                        btn.textContent = '🤖 AI ile Yorumla';
                    }
                }
            }, 500);
            
            // Grafik oluştur
            generateCompanyChart(surveys);
        }

        let companyChartInstance = null;

        function generateCompanyChart(surveys) {
            try {
                console.log('generateCompanyChart çalışıyor, survey sayısı:', surveys ? surveys.length : 0);
                
                // Önce survey verilerinin yapısını inceleyelim
                if (surveys && surveys.length > 0) {
                    console.log('İlk survey örneği:', surveys[0]);
                }
                
                // Mevcut grafiği temizle
                if (companyChartInstance) {
                    companyChartInstance.destroy();
                    companyChartInstance = null;
                }

                const canvas = document.getElementById('companyChart');
                if (!canvas) {
                    console.log('companyChart canvas bulunamadı');
                    return;
                }

                // Memnuniyet verilerini hazırla - Basit yaklaşım
                const satisfactionData = [0, 0, 0, 0, 0]; // [Çok Memnun, Memnun, Kararsız, Memnun Değil, Hiç Memnun Değil]

                // Survey verilerinden memnuniyet hesapla
                if (surveys && surveys.length > 0) {
                    console.log('İşletme grafiği için işlenen survey sayısı:', surveys.length);
                    
                    // Her survey için ortalama puan üzerinden memnuniyet hesapla
                    surveys.forEach((survey, surveyIndex) => {
                        const avgScore = parseFloat(survey.averageScore) || 0;
                        
                        if (avgScore > 0) {
                            // Ortalama puana göre memnuniyet seviyesi belirle
                            let satisfactionIndex;
                            if (avgScore >= 4.5) satisfactionIndex = 0; // Çok Memnun
                            else if (avgScore >= 3.5) satisfactionIndex = 1; // Memnun
                            else if (avgScore >= 2.5) satisfactionIndex = 2; // Kararsız
                            else if (avgScore >= 1.5) satisfactionIndex = 3; // Memnun Değil
                            else satisfactionIndex = 4; // Hiç Memnun Değil
                            
                            satisfactionData[satisfactionIndex]++;
                            
                            console.log(`Survey ${surveyIndex}: avgScore=${avgScore}, satisfactionIndex=${satisfactionIndex}`);
                        }
                    });
                }

                console.log('İşletme memnuniyet dağılımı:', satisfactionData);

                // Grafik verilerini hazırla
                const chartData = {
                    labels: ['Çok Memnun', 'Memnun', 'Kararsız', 'Memnun Değil', 'Hiç Memnun Değil'],
                    datasets: [{
                        label: 'İşletme Memnuniyet Dağılımı',
                        data: satisfactionData,
                        backgroundColor: [
                            '#22C55E', // Çok Memnun - Yeşil
                            '#84CC16', // Memnun - Açık Yeşil  
                            '#EAB308', // Kararsız - Sarı
                            '#F97316', // Memnun Değil - Turuncu
                            '#EF4444'  // Hiç Memnun Değil - Kırmızı
                        ],
                        borderColor: [
                            '#16A34A',
                            '#65A30D', 
                            '#CA8A04',
                            '#EA580C',
                            '#DC2626'
                        ],
                        borderWidth: 2
                    }]
                };

                // Grafik ayarları (eğitim anketindeki gibi)
                const config = {
                    type: 'bar',
                    data: chartData,
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        scales: {
                            y: {
                                beginAtZero: true,
                                title: {
                                    display: true,
                                    text: 'Çalışan Sayısı'
                                }
                            }
                        },
                        plugins: {
                            legend: {
                                display: false
                            },
                            tooltip: {
                                callbacks: {
                                    label: function(context) {
                                        const total = satisfactionData.reduce((a, b) => a + b, 0);
                                        const percentage = total > 0 ? ((context.raw / total) * 100).toFixed(1) : 0;
                                        return context.dataset.label + ': ' + context.raw + ' (' + percentage + '%)';
                                    }
                                }
                            }
                        }
                    }
                };

                // Grafiği oluştur
                const ctx = canvas.getContext('2d');
                companyChartInstance = new Chart(ctx, config);
                
            } catch (error) {
                console.error('İşletme grafiği oluşturma hatası:', error);
            }
        }

        function logoutCompany() {
            loggedInCompany = null;
            document.getElementById('companyLogin').classList.remove('hidden');
            document.getElementById('companyDashboard').classList.add('hidden');
            document.getElementById('companyLoginName').value = '';
            document.getElementById('companyPassword').value = '';
        }

        async function loginAdmin() {
            const password = document.getElementById('adminPassword').value.trim();
            
            if (password === systemData.adminPassword) {
                isAdminLoggedIn = true;
                document.getElementById('adminLogin').classList.add('hidden');
                document.getElementById('adminDashboard').classList.remove('hidden');
                await loadAdminDashboard();
            } else {
                showModal('❌ Giriş Hatası', 'Yönetici şifresi hatalı.');
            }
        }

        async function loadAdminDashboard() {
            try {
                if (!systemData.surveyData) {
                    systemData.surveyData = await loadFromFirebase();
                }
                
                const companies = systemData.surveyData.companies || {};
                const responses = systemData.surveyData.responses || [];
                
                // İstatistikleri güncelle
                document.getElementById('totalCompanies').textContent = Object.keys(companies).length;
                document.getElementById('activeSurveys').textContent = Object.keys(companies).length;
                document.getElementById('totalUsers').textContent = responses.length;
                
                // Şirket listesini yükle
                loadCompanyList();
            } catch (error) {
                console.error('Admin dashboard yükleme hatası:', error);
                showModal('❌ Hata', 'Yönetici paneli verileri yüklenirken bir hata oluştu. Lütfen tekrar deneyin.');
            }
        }

        function logoutAdmin() {
            isAdminLoggedIn = false;
            document.getElementById('adminLogin').classList.remove('hidden');
            document.getElementById('adminDashboard').classList.add('hidden');
            document.getElementById('adminPassword').value = '';
        }

        function loadCompanyList() {
            const searchTerm = document.getElementById('companySearchInput').value.trim().toLowerCase();
            const companies = systemData.surveyData.companies || {};
            const companyListEl = document.getElementById('companyList');
            companyListEl.innerHTML = '';

            Object.entries(companies).forEach(([key, company]) => {
                if (company.name.toLowerCase().includes(searchTerm)) {
                    const row = document.createElement('tr');
                    row.classList.add('hover:bg-gray-50', 'transition-colors');
                    row.innerHTML = `
                        <td class="px-4 py-2 text-left">${company.name}</td>
                        <td class="px-4 py-2 text-left">${company.password}</td>
                        <td class="px-4 py-2 text-left">${company.totalResponses || 0}</td>
                        <td class="px-4 py-2 text-left">
                            <span class="text-xs font-semibold ${company.status === 'aktif' ? 'text-green-600' : 'text-red-600'}">
                                ${company.status === 'aktif' ? 'Aktif' : 'Pasif'}
                            </span>
                        </td>
                        <td class="px-4 py-2 text-left flex gap-2">
                            <button onclick="editCompany('${key}')" class="text-blue-600 hover:underline text-sm">Düzenle</button>
                            <button onclick="toggleCompanyStatus('${key}')" class="text-xs px-2 py-1 rounded ${company.status === 'aktif' ? 'bg-red-100 text-red-700' : 'bg-green-100 text-green-700'}">
                                ${company.status === 'aktif' ? 'Pasif Yap' : 'Aktif Yap'}
                            </button>
                        </td>
                    `;
                    companyListEl.appendChild(row);
                }
            });
        }

        function editCompany(key) {
            const company = systemData.surveyData.companies[key];
            if (!company) return;

            document.getElementById('modalContent').innerHTML = `
                <h3 class="text-lg font-semibold mb-4">Şirket Bilgilerini Düzenle</h3>
                <div class="mb-4">
                    <label class="block text-sm font-medium text-gray-700 mb-1">Şirket Adı</label>
                    <input type="text" id="editCompanyName" value="${company.name}" class="w-full border rounded-lg px-4 py-2 focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="mb-4">
                    <label class="block text-sm font-medium text-gray-700 mb-1">Şifre</label>
                    <input type="text" id="editCompanyPassword" value="${company.password}" class="w-full border rounded-lg px-4 py-2 focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="mb-4">
                    <label class="block text-sm font-medium text-gray-700 mb-1">Durum</label>
                    <select id="editCompanyStatus" class="w-full border rounded-lg px-4 py-2 focus:ring-2 focus:ring-blue-500">
                        <option value="aktif" ${company.status === 'aktif' ? 'selected' : ''}>Aktif</option>
                        <option value="pasif" ${company.status === 'pasif' ? 'selected' : ''}>Pasif</option>
                    </select>
                </div>
                <div class="flex gap-2">
                    <button onclick="saveCompanyChanges('${key}')" class="flex-1 py-2 px-4 rounded-lg bg-blue-600 text-white font-semibold hover:bg-blue-700 transition-colors">
                        Kaydet
                    </button>
                    <button onclick="closeModal()" class="flex-1 py-2 px-4 rounded-lg bg-gray-200 text-gray-700 font-semibold hover:bg-gray-300 transition-colors">
                        İptal
                    </button>
                </div>
            `;
            document.getElementById('modal').classList.add('show');
        }

        function closeModal() {
            document.getElementById('modal').classList.remove('show');
        }

        async function saveCompanyChanges(key) {
            const newPassword = document.getElementById('editCompanyPassword').value.trim();
            const newStatus = document.getElementById('editCompanyStatus') ? document.getElementById('editCompanyStatus').value : null;

            if (!newPassword || !newStatus) {
                return showModal('⚠️ Eksik Bilgi', 'Lütfen tüm alanları doldurun.');
            }

            try {
                // Sadece şifre ve durum güncelle
                if (systemData.surveyData.companies[key]) {
                    systemData.surveyData.companies[key].password = newPassword;
                    systemData.surveyData.companies[key].status = newStatus;
                }

                const result = await saveToFirebase(systemData.surveyData);
                if (result.success) {
                    closeModal(); // Önce modalı kapat
                    loadCompanyList(); // Sonra tabloyu güncelle
                    showModal('✅ Başarılı', 'Şirket bilgileri başarıyla güncellendi.');
                } else {
                    showModal('❌ Hata', 'Şirket bilgileri güncellenirken bir hata oluştu. Lütfen tekrar deneyin.');
                }
            } catch (error) {
                console.error('Şirket güncelleme hatası:', error);
                showModal('❌ Hata', 'Şirket bilgileri güncellenirken bir hata oluştu. Lütfen tekrar deneyin.');
            }
        // Şirket durumunu hızlıca değiştirmek için buton
        async function toggleCompanyStatus(key) {
            const company = systemData.surveyData.companies[key];
            if (!company) return;
            // Durumu değiştir
            company.status = company.status === 'aktif' ? 'pasif' : 'aktif';

            // Anlık olarak DOM'da ilgili satırı güncelle (optimistik güncelleme)
            const companyListEl = document.getElementById('companyList');
            if (companyListEl) {
                // Tüm satırları tara, ilgili şirketi bul
                Array.from(companyListEl.children).forEach(row => {
                    if (row.children[0] && row.children[0].textContent === company.name) {
                        // Durum hücresini güncelle
                        const statusCell = row.children[3];
                        if (statusCell) {
                            statusCell.innerHTML = `<span class="text-xs font-semibold ${company.status === 'aktif' ? 'text-green-600' : 'text-red-600'}">${company.status === 'aktif' ? 'Aktif' : 'Pasif'}</span>`;
                        }
                        // Buton metnini ve rengini güncelle
                        const btn = row.querySelector('button[onclick^="toggleCompanyStatus"]');
                        if (btn) {
                            btn.textContent = company.status === 'aktif' ? 'Pasif Yap' : 'Aktif Yap';
                            btn.className = `text-xs px-2 py-1 rounded ${company.status === 'aktif' ? 'bg-red-100 text-red-700' : 'bg-green-100 text-green-700'}`;
                        }
                    }
                });
            }

            try {
                const result = await saveToFirebase(systemData.surveyData);
                // Yine de tabloyu tazele (veri tutarlılığı için)
                loadCompanyList();
                if (!result.success) {
                    showModal('❌ Hata', 'Durum güncellenemedi.');
                }
            } catch (error) {
                showModal('❌ Hata', 'Durum güncellenemedi.');
            }
        }
        }

        function generateCharts(surveys) {
                // Grafik nesnelerini globalde tut
                if (!window._charts) window._charts = {};
                // Pozisyon dağılımı grafiği
                const positionChartCtx = document.getElementById('positionChart').getContext('2d');
                const positionData = {};
                surveys.forEach(s => {
                    positionData[s.jobType] = (positionData[s.jobType] || 0) + 1;
                });
                if (window._charts.positionChart) window._charts.positionChart.destroy();
                window._charts.positionChart = new Chart(positionChartCtx, {
                    type: 'pie',
                    data: {
                        labels: Object.keys(positionData),
                        datasets: [{
                            data: Object.values(positionData),
                            backgroundColor: ['#4caf50', '#2196f3', '#ff9800'],
                            borderWidth: 0
                        }]
                    },
                    options: {
                        responsive: true,
                        plugins: {
                            legend: {
                                position: 'top',
                            },
                            tooltip: {
                                callbacks: {
                                    label: function(tooltipItem) {
                                        const label = tooltipItem.label || '';
                                        const value = tooltipItem.raw || 0;
                                        return `${label}: ${value} (${((value / surveys.length) * 100).toFixed(1)}%)`;
                                    }
                                }
                            }
                        }
                    }
                });

                // Memnuniyet grafiği
                const satisfactionChartCtx = document.getElementById('satisfactionChart').getContext('2d');
                const satisfactionData = [0, 0, 0, 0, 0];
                surveys.forEach(s => {
                    s.answers.forEach(a => {
                        satisfactionData[a.score - 1]++;
                    });
                });
                if (window._charts.satisfactionChart) window._charts.satisfactionChart.destroy();
                window._charts.satisfactionChart = new Chart(satisfactionChartCtx, {
                    type: 'bar',
                    data: {
                        labels: ['Hiç Memnun Değilim', 'Memnun Değilim', 'Kararsızım', 'Memnunum', 'Çok Memnunum'],
                        datasets: [{
                            label: 'Memnuniyet Dağılımı',
                            data: satisfactionData,
                            backgroundColor: '#4caf50',
                            borderColor: '#388e3c',
                            borderWidth: 1
                        }]
                    },
                    options: {
                        responsive: true,
                        scales: {
                            y: {
                                beginAtZero: true
                            }
                        },
                        plugins: {
                            tooltip: {
                                callbacks: {
                                    label: function(tooltipItem) {
                                        const label = tooltipItem.label || '';
                                        const value = tooltipItem.raw || 0;
                                        return `${label}: ${value} (${((value / surveys.length) * 100).toFixed(1)}%)`;
                                    }
                                }
                            }
                        }
                    }
                });

                // Süre dağılımı grafiği
                const timeChartCtx = document.getElementById('timeChart').getContext('2d');
                const timeData = {
                    labels: [], // Süre etiketleri (örneğin, 0-5 dk, 5-10 dk, ...)
                    datasets: [{
                        label: 'Katılımcı Sayısı',
                        data: [],
                        backgroundColor: '#2196f3',
                        borderColor: '#1976d2',
                        borderWidth: 1
                    }]
                };
                // Süre aralıklarını belirle
                const timeIntervals = [
                    { min: 0, max: 5 },
                    { min: 5, max: 10 },
                    { min: 10, max: 15 },
                    { min: 15, max: 20 },
                    { min: 20, max: 30 },
                    { min: 30, max: 60 },
                    { min: 60, max: 120 }
                ];
                timeIntervals.forEach(interval => {
                    const label = `${interval.min}-${interval.max} dk`;
                    timeData.labels.push(label);
                    const count = surveys.filter(s => {
                        const minutes = parseInt(s.duration.split(':')[0], 10);
                        return minutes >= interval.min && minutes < interval.max;
                    }).length;
                    timeData.datasets[0].data.push(count);
                });
                if (window._charts.timeChart) window._charts.timeChart.destroy();
                window._charts.timeChart = new Chart(timeChartCtx, {
                    type: 'bar',
                    data: timeData,
                    options: {
                        responsive: true,
                        scales: {
                            y: {
                                beginAtZero: true
                            }
                        },
                        plugins: {
                            tooltip: {
                                callbacks: {
                                    label: function(tooltipItem) {
                                        const label = tooltipItem.label || '';
                                        const value = tooltipItem.raw || 0;
                                        return `${label}: ${value} katılımcı`;
                                    }
                                }
                            }
                        }
                    }
                });

                // Puan dağılımı grafiği
                const trendChartCtx = document.getElementById('trendChart').getContext('2d');
                const trendData = {
                    labels: [], // Zaman etiketleri (günler, haftalar vb.)
                    datasets: [{
                        label: 'Ortalama Puan',
                        data: [],
                        backgroundColor: '#4caf50',
                        borderColor: '#388e3c',
                        borderWidth: 1
                    }]
                };
                // Günlük ortalama puanları hesapla
                const dailyScores = {};
                surveys.forEach(s => {
                    const date = new Date(s.submittedAt).toISOString().split('T')[0]; // Yıl-Ay-Gün formatında
                    if (!dailyScores[date]) {
                        dailyScores[date] = { total: 0, count: 0 };
                    }
                    dailyScores[date].total += parseFloat(s.averageScore);
                    dailyScores[date].count++;
                });
                // Tarihlere göre sırala
                const sortedDates = Object.keys(dailyScores).sort();
                sortedDates.forEach(date => {
                    trendData.labels.push(date);
                    const avgScore = (dailyScores[date].total / dailyScores[date].count).toFixed(2);
                    trendData.datasets[0].data.push(avgScore);
                });
                if (window._charts.trendChart) window._charts.trendChart.destroy();
                window._charts.trendChart = new Chart(trendChartCtx, {
                    type: 'line',
                    data: trendData,
                    options: {
                        responsive: true,
                        scales: {
                            y: {
                                beginAtZero: true,
                                max: 5
                            }
                        },
                        plugins: {
                            tooltip: {
                                callbacks: {
                                    label: function(tooltipItem) {
                                        const label = tooltipItem.label || '';
                                        const value = tooltipItem.raw || 0;
                                        return `${label}: ${value} puan`;
                                    }
                                }
                            }
                        }
                    }
                });
            }
    </script>
</body>
</html>
