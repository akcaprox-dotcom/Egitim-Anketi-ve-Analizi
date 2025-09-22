<!--
Bu dosya, 'dosyamızhastane.html' ile birebir aynı uygulama mantığına ve işlevselliğe sahiptir.
Tüm metinler, başlıklar ve içerik kelimesi kelimesine korunmuştur.
Kodun tamamı, anket başlatma, raporlama, yönetim ve diğer tüm işlevlerle birlikte entegre edilmiştir.
-->

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
                <span class="bg-green-100 text-green-800 text-xs font-semibold px-2 py-1 rounded-full">v3.0.0 - JSONBin.io Entegre</span>
            </div>

            <!-- Sorumluluk Reddi -->
            <div id="disclaimerSection" class="mb-4">
                <div class="bg-yellow-50 border border-yellow-300 rounded-lg p-3 mb-3">
                    <h3 class="font-semibold text-yellow-800 mb-2 text-sm">⚠️ Veri Koruma Beyanı</h3>
                    <div class="text-xs text-yellow-700 space-y-1">
                        <p>• Verileriniz JSONBin.io güvenli sisteminde saklanır ve üçüncü taraflarla paylaşılmaz.</p>
                        <p>• Anket sonuçları sadece şirket yetkilileri tarafından görüntülenebilir.</p>
                        <p>• Sistem güvenliği JSONBin.io sorumluluğundadır.</p>
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
                            <button onclick="showPDFReport(true)" class="px-3 py-1 bg-red-600 text-white rounded hover:bg-red-700 text-sm">📄 PDF Göster (Filtreli)</button>
                            <button onclick="showPDFReport(false)" class="px-3 py-1 bg-gray-600 text-white rounded hover:bg-gray-700 text-sm">📄 PDF Göster (Tümü)</button>
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
                    <!-- SWOT Analizi Tablosu (Rapor Ekranı) -->
                    <div class="bg-white border rounded-lg p-4 mb-4">
                        <h4 class="font-semibold text-gray-800 mb-4 text-lg">SWOT Analizi</h4>
                        <div class="overflow-x-auto">
                            <table class="min-w-full text-sm text-center border border-gray-300">
                                <thead>
                                    <tr>
                                        <th class="bg-green-100 border border-gray-300 p-2">Güçlü Yönler</th>
                                        <th class="bg-red-100 border border-gray-300 p-2">Zayıf Yönler</th>
                                        <th class="bg-blue-100 border border-gray-300 p-2">Fırsatlar</th>
                                        <th class="bg-yellow-100 border border-gray-300 p-2">Tehditler</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr>
                                        <td class="border border-gray-300 p-2 align-top">• Yüksek çalışan memnuniyeti<br>• Güçlü ekip ruhu<br>• Modern altyapı</td>
                                        <td class="border border-gray-300 p-2 align-top">• Yoğun dönemlerde iletişim eksikliği<br>• Kısıtlı sosyal imkanlar<br>• Dijitalleşme eksikliği</td>
                                        <td class="border border-gray-300 p-2 align-top">• Dijitalleşme yatırımları<br>• Yeni pazar fırsatları<br>• Kamu destekleri</td>
                                        <td class="border border-gray-300 p-2 align-top">• Artan rekabet<br>• Ekonomik dalgalanmalar<br>• Personel değişimi</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                    
                    <!-- Katılımcı Detayları Bölümü -->
                    <div class="bg-white border rounded-lg p-4 mb-4">
                        <div class="flex justify-between items-center mb-3">
                            <h4 class="font-semibold text-gray-800">👥 Katılımcı Detayları</h4>
                            <button onclick="toggleParticipantDetails()" id="toggleParticipantsBtn" class="px-3 py-1 bg-blue-600 text-white text-sm rounded hover:bg-blue-700">
                                📋 Katılımcıları Görüntüle
                            </button>
                        </div>
                        <div id="participantDetails" class="hidden">
                            <div class="overflow-x-auto">
                                <table class="w-full table-auto text-sm">
                                    <thead>
                                        <tr class="bg-gray-100">
                                            <th class="px-3 py-2 text-left">İsim</th>
                                            <th class="px-3 py-2 text-left">Pozisyon</th>
                                            <th class="px-3 py-2 text-center">Ortalama Puan</th>
                                            <th class="px-3 py-2 text-center">Memnuniyet</th>
                                            <th class="px-3 py-2 text-center">Tarih</th>
                                        </tr>
                                    </thead>
                                    <tbody id="participantTableBody">
                                        <!-- Katılımcı listesi buraya yüklenecek -->
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                    
                    <div id="detailedReport" class="space-y-3"></div>
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

    <script>
        // Firebase config
        const firebaseConfig = {
            apiKey: "AIzaSyDp2Yh8hamXi6OTfw03MT0S4rp5CjnlAcg",
            authDomain: "akcaprox-anket.firebaseapp.com",
            projectId: "akcaprox-anket",
            storageBucket: "akcaprox-anket.appspot.com",
            messagingSenderId: "426135179922",
            appId: "1:426135179922:web:c16b3fd6fa5f3d9224cc4b",
            measurementId: "G-CD1ET7RGX1"
        };
        firebase.initializeApp(firebaseConfig);
        const auth = firebase.auth();

        // Firebase Realtime Database URL
        const FIREBASE_DB_URL = 'https://isletme-76bad-default-rtdb.europe-west1.firebasedatabase.app/';

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
                // Çalışma Ortamı & Konfor (10 Soru)
                "Çalışma alanımın temiz ve düzenli olduğunu düşünüyorum 🏭",
                "İşimi yapmam için gerekli tüm alet ve ekipmanlar yeterli ve güvenli ⚙️",
                "İş yerindeki havalandırma, aydınlatma ve ısınma koşulları yeterli 🌡️",
                "Soyunma odaları ve tuvaletler gibi sosyal alanların hijyeninden memnunum 🚿",
                "İş kıyafetlerinin rahat ve iş güvenliği standartlarına uygun olduğunu düşünüyorum 👕",
                "İşyerindeki gürültü seviyesi, çalışmamı olumsuz etkilemiyor 🔇",
                "Çalışma saatlerimin yorucu olduğunu düşünmüyorum ⏰",
                "Dinlenme molalarının yeterli uzunlukta olduğunu düşünüyorum ☕",
                "Çalışma ortamında kendimi fiziksel olarak güvende hissediyorum 🛡️",
                "İşyerindeki revir veya ilk yardım imkanları yeterli 🏥",
                
                // Yemek & Sosyal Haklar (10 Soru)
                "Şirket yemeklerinin kalitesi ve çeşitliliğinden memnunum 🍽️",
                "Yemek saatlerinin yeterli olduğunu düşünüyorum ⏱️",
                "Sağlanan servis veya ulaşım imkanları ihtiyaçlarımı karşılıyor 🚌",
                "Aldığım maaş, harcadığım emeğe göre adil 💰",
                "Şirket tarafından sunulan sosyal haklar (ikramiye, yardım vb.) yeterli 🎁",
                "Yemeklerin porsiyonları doyurucu 🥘",
                "Servis şoförlerinin tutum ve davranışları saygılı 🚐",
                "İş yerindeki sosyal etkinliklerin sayısı ve kalitesi yeterli 🎉",
                "Fazla mesai ücretlerinin adil bir şekilde ödendiğini düşünüyorum ⏳",
                "İşyeri yemekhane personelinin tutum ve davranışları nazik ve saygılı 👨‍🍳",
                
                // İş İlişkileri & Güven (10 Soru)
                "Yöneticim, işimi doğru yapmam için bana yeterli geri bildirim veriyor 👥",
                "Yöneticim, sorunlarım olduğunda bana destek oluyor 🤝",
                "İş arkadaşlarımla güçlü bir iş birliği içindeyiz 👫",
                "Yöneticim ve üst yönetimden gelen bilgiler açık ve anlaşılır 📢",
                "Şirketin hedefleri hakkında yeterince bilgilendiriliyorum 🎯",
                "İş yerinde kendimi rahatça ifade edebiliyorum 💬",
                "Yöneticimin kararlarının adil ve eşitlikçi olduğunu düşünüyorum ⚖️",
                "Şirket yönetimine güveniyorum 🤝",
                "Sorunlarım veya şikayetlerim olduğunda, yetkililere ulaşmak kolay 📞",
                "Yöneticim, iyi yaptığım işleri takdir ediyor 👏",
                
                // Sadakat & Gelecek (10 Soru)
                "Şirkete karşı güçlü bir sadakat hissediyorum ❤️",
                "Gelecek 2 yıl içinde bu şirkette çalışmaya devam etmeyi düşünüyorum 📅",
                "Şirketin geleceğinin parlak olduğunu düşünüyorum ✨",
                "Şirketin misyon ve vizyonu bana ilham veriyor 🌟",
                "İşimin, şirket başarısına önemli katkı sağladığını hissediyorum 🏆",
                "Şirket, beni daha iyi bir çalışan olmam için teşvik ediyor 📈",
                "Şirketteki pozisyonumda kariyer gelişimi için fırsatlar görüyorum 🚀",
                "Şirketin sunduğu eğitimler, kendimi geliştirmem için yeterli 📚",
                "Şirketteki performansımın adil bir şekilde değerlendirildiğini düşünüyorum 📊",
                "Şirketteki kariyer yolumun belirsiz olduğunu düşünmüyorum 🛤️",
                
                // Dijital Dönüşüm İsteği & Yenilenme (10 Soru)
                "İşimi daha kolay hale getirecek yeni teknolojilere veya araçlara açığım 💻",
                "Şirketin, iş süreçlerini dijitalleştirmesini destekliyorum 🔄",
                "Yeni teknolojileri öğrenmeye ve kendimi geliştirmeye istekliyim 🎓",
                "İşimizde kullanılan mevcut teknolojik araçlar (makine, yazılım vb.) yeterli ve güncel 🔧",
                "Şirketin yeniliklere açık bir kültürü olduğunu düşünüyorum 💡",
                "Şirket, iş süreçlerindeki verimliliği artırmak için dijital çözümleri kullanıyor ⚡",
                "Yeni teknolojik araçların kullanımına dair yeterli eğitim alıyorum 📖",
                "İşimizde dijitalleşmenin bize zaman kazandıracağını düşünüyorum ⏰",
                "Teknolojik gelişmelerin işimi daha güvenli hale getireceğine inanıyorum 🛡️",
                "Şirketin, dijital geleceğe hazırlandığını düşünüyorum 🚀"
            ],
            "Beyaz Yaka": [
                // Çalışma Ortamı & Konfor (10 Soru)
                "Çalışma alanımın (ofis, masa vb.) ergonomisi ve konforu yeterli 🪑",
                "İşimi yapmak için gerekli teknolojik donanıma (bilgisayar, yazılım vb.) sahibim 💻",
                "İş yerindeki havalandırma, aydınlatma ve ısınma koşulları yeterli 🌡️",
                "Toplantı odaları ve ortak alanların temizliği ve kullanışlılığından memnunum 🏢",
                "Esnek çalışma saatlerinin (hibrit/uzaktan) üretkenliğimi artırdığını düşünüyorum ⏰",
                "İş yerindeki gürültü seviyesi, odaklanmamı engellemiyor 🔇",
                "Çalışma saatlerimin iş-yaşam dengeme uygun olduğunu düşünüyorum ⚖️",
                "Dinlenme odaları veya kafeterya gibi sosyal alanların kalitesi yeterli ☕",
                "İş arkadaşlarımla iyi ve uyumlu bir iletişim kurabiliyorum 👥",
                "Çalışma ortamında kendimi güvende hissediyorum 🛡️",
                
                // Yemek & Sosyal Haklar (10 Soru)
                "Şirket yemeklerinin kalitesi ve çeşitliliğinden memnunum 🍽️",
                "Sağlanan servis veya ulaşım imkanları yeterli 🚌",
                "Aldığım maaş ve yan hakların sektör ortalamasının üzerinde olduğunu düşünüyorum 💰",
                "Şirket tarafından sunulan sosyal haklar (özel sağlık sigortası, primler vb.) yeterli 🏥",
                "Yemek saatlerinin, iş yüküme uygun ve esnek olduğunu düşünüyorum ⏱️",
                "Yemeklerin kalitesinin, genel refahımı artırdığını düşünüyorum 🥘",
                "Ofis içi ikramlar ve içecek seçenekleri yeterli ve çeşitli ☕",
                "İşyeri yemekhane personelinin tutum ve davranışları nazik ve saygılı 👨‍🍳",
                "Şirketin sağladığı yan haklar (hobi kulüpleri, spor imkanları vb.) memnuniyet verici 🎯",
                "Şirketin, çalışanların refahına önem verdiğini düşünüyorum 💝",
                
                // İş İlişkileri & Güven (10 Soru)
                "Yöneticim, performansımı düzenli olarak değerlendiriyor ve bana geri bildirim veriyor 📊",
                "Yöneticim, iş hedeflerimin net ve anlaşılır olmasını sağlıyor 🎯",
                "Ekip arkadaşlarım ve ben, ortak hedeflere ulaşmak için etkili bir şekilde iş birliği yapıyoruz 🤝",
                "Şirketin stratejik kararları ve hedefleri hakkında yeterince bilgilendiriliyorum 📢",
                "Yöneticim, görüş ve önerilerime değer veriyor 💭",
                "Üst yönetime güveniyorum 🤝",
                "İşyerindeki iletişim kanallarının açık ve şeffaf olduğunu düşünüyorum 💬",
                "Şirket yönetiminin kararları adil ve eşitlikçi ⚖️",
                "Sorunlarım veya şikayetlerim olduğunda, yetkililere ulaşmak kolay 📞",
                "Yöneticim, başarılarımı takdir ediyor ve beni motive ediyor 👏",
                
                // Sadakat & Kariyer (10 Soru)
                "Şirkete karşı güçlü bir sadakat hissediyorum ❤️",
                "Bu şirkette uzun vadeli bir kariyer planlıyorum 📅",
                "Şirket içi terfi ve kariyer gelişimi fırsatlarının adil ve şeffaf olduğunu düşünüyorum 🚀",
                "Şirketin, mesleki gelişimim için yeterli eğitim ve kaynak sağladığına inanıyorum 📚",
                "İşimin, kişisel becerilerimi ve yeteneklerimi geliştirmeme yardımcı olduğunu düşünüyorum 💪",
                "Şirketin misyon ve vizyonu bana ilham veriyor 🌟",
                "İşimin, şirket başarısına önemli katkı sağladığını hissediyorum 🏆",
                "Aldığım eğitimlerin kariyerime somut katkıları oldu 📈",
                "Şirketin değerleri, benim kişisel değerlerimle uyumlu 🎭",
                "Şirketin başarısı için ekstra çaba göstermeye istekliyim 💯",
                
                // Dijital Dönüşüm İsteği & Yenilenme (10 Soru)
                "Şirketin dijital dönüşüm çabalarını destekliyorum 🔄",
                "İşimi daha verimli hale getirecek yeni yazılım veya araçları kullanmaya açığım 💻",
                "İş akışlarımızı kolaylaştıracak dijital çözümlerin hayata geçirilmesini istiyorum ⚡",
                "Şirket, teknolojik yeniliklere ve güncel uygulamalara yatırım yapıyor 💡",
                "İşimizde kullanılan dijital araçların kullanımına dair yeterli eğitim alıyorum 📖",
                "Dijitalleşmenin iş güvenliğimizi ve veri gizliliğini artıracağına inanıyorum 🛡️",
                "Şirketin, dijitalleşme sürecini başarılı bir şekilde yönettiğini düşünüyorum 🎯",
                "İşimizle ilgili dijital gelişmeleri takip etmeye ve öğrenmeye istekliyim 🎓",
                "Yeni teknolojilerin, iş-yaşam dengemi daha iyi kurmama yardımcı olacağına inanıyorum ⚖️",
                "Dijital dönüşümle birlikte, şirket içinde daha fazla kariyer fırsatı doğacağını düşünüyorum 🚀"
            ],
            "Yönetim": [
                // Çalışma Ortamı & Konfor (10 Soru)
                "Çalışma alanımın, odaklanma gerektiren görevler için uygun olduğunu düşünüyorum 🎯",
                "Yönetim pozisyonunda gerekli olan tüm teknolojik ve fiziki donanımlara sahibim 💻",
                "Şirketin, yöneticilerin iş-yaşam dengesini destekleyecek bir kültürü var ⚖️",
                "Toplantı odaları ve ortak alanların kalitesi, verimli toplantılar için yeterli 🏢",
                "Hibrit/uzaktan çalışma modelinin, yönetici olarak verimliliğimi artırdığını düşünüyorum 🏠",
                "Çalışma ortamının stres seviyesi, performansımı olumsuz etkilemiyor 😌",
                "İş arkadaşlarım ve ekibimle olan iletişimim açık ve verimli 💬",
                "Çalışma ortamının yenilikçi fikirleri teşvik ettiğini düşünüyorum 💡",
                "Ekibimin, işlerini en iyi şekilde yapması için gerekli kaynaklara erişimi var 🛠️",
                "İş yeri, yöneticiler arasında network kurmak için yeterli sosyal imkanlar sunuyor 🤝",
                
                // Yemek & Sosyal Haklar (10 Soru)
                "Şirket yemeklerinin kalitesi ve çeşitliliği, üst düzey çalışanlar için yeterli 🍽️",
                "Aldığım maaş ve yan haklar paketi, piyasa standartlarında ve rekabetçi 💰",
                "Şirketin, yöneticiler için sağladığı sosyal haklar (araba, prim vb.) adil 🚗",
                "Şirketin sunduğu ek avantajlar (özel sigorta, ek emeklilik vb.) yeterli 🏥",
                "Yan hakların, şirkete olan sadakatimi artırdığını düşünüyorum ❤️",
                "Maaş ve yan haklar politikasının şeffaf olduğunu düşünüyorum 📊",
                "Şirketin yemek kalitesinin, çalışan bağlılığı üzerinde olumlu etkisi olduğuna inanıyorum 🥘",
                "İş seyahatlerindeki harcama politikaları adil ve esnek ✈️",
                "Şirketin sunduğu yan hakların, iş-yaşam dengemi korumama yardımcı olduğunu düşünüyorum ⚖️",
                "Şirketin, üst düzey çalışanlar için sağladığı sosyal imkanlardan memnunum 🎯",
                
                // İş İlişkileri & Güven (10 Soru)
                "Üst yönetimle aramızda açık ve şeffaf bir iletişim var 📢",
                "Yönetim kurulunun stratejik kararlarını destekliyor ve güveniyorum 🎯",
                "Ekibim, hedeflere ulaşmak için yeterli motivasyona ve yetkiye sahip 🔥",
                "Şirketin hedeflerini, ekibime etkili bir şekilde aktarabiliyorum 📣",
                "Şirketin, yöneticileriyle arasında güçlü bir güven ilişkisi olduğuna inanıyorum 🤝",
                "Önemli kararlar alırken, görüşlerime değer verildiğini hissediyorum 💭",
                "Şirketin, yöneticiler arasındaki rekabeti yönetme biçimi adil ⚖️",
                "Yönetici olarak, ekibimden gelen geri bildirimleri rahatlıkla kabul ediyorum 👂",
                "Şirket yönetimi, hatalardan ders çıkarmaya ve iyileştirmeye açık 🔄",
                "Yönetici olarak, şirket tarafından yeterince takdir edildiğimi düşünüyorum 👏",
                
                // Sadakat & Gelecek (10 Soru)
                "Şirkete karşı güçlü bir sadakat hissediyorum ve bu duyguyu ekibime de aktarıyorum ❤️",
                "Şirketin uzun vadeli büyüme potansiyeline güveniyorum 📈",
                "Şirketin misyon ve vizyonu, yöneticilik kariyerime yön veriyor 🌟",
                "Şirketteki pozisyonumun, sektöre yön veren bir rol oynadığını düşünüyorum 🏆",
                "Şirketin, liderlik becerilerimi geliştirmem için gerekli eğitimleri sağladığına inanıyorum 📚",
                "Şirketin geleceğinin parlak olduğunu düşünüyorum ve bu, beni motive ediyor ✨",
                "Şirket içinde kariyerimi daha üst seviyelere taşıyacak fırsatlar görüyorum 🚀",
                "Şirketin, geleceğin liderlerini yetiştirme konusunda somut adımlar attığına inanıyorum 👨‍🏫",
                "Şirkete olan bağlılığım, ekibimin bağlılığını da artırıyor 🔗",
                "Şirketin, zor zamanlarda dahi çalışanlarının arkasında duracağını düşünüyorum 🛡️",
                
                // Dijital Dönüşüm İsteği & Yenilenme (10 Soru)
                "Şirketin dijital dönüşüm stratejisinin açık ve anlaşılır olduğunu düşünüyorum 🎯",
                "Dijitalleşmenin, şirketimizin rekabet gücünü artıracağına inanıyorum 💪",
                "Yeni teknolojilerin, yönetim süreçlerini daha verimli hale getireceğini düşünüyorum ⚡",
                "Şirket, dijital dönüşüm için gerekli bütçeyi ve kaynakları ayırıyor 💰",
                "Yönetici olarak, dijital dönüşüm süreçlerini etkin bir şekilde yönetebildiğime inanıyorum 🎛️",
                "Ekibimi, dijital yenilikleri benimsemeleri için teşvik ediyorum 🚀",
                "Şirketin, kendini sürekli yenileme ve güncel kalma isteğini destekliyorum 🔄",
                "Dijital dönüşümün, iş süreçlerimizde şeffaflığı artırdığını düşünüyorum 🔍",
                "Yeni teknolojiler, karar alma süreçlerimize katkı sağlıyor 📊",
                "Şirketin, geleceğin teknolojilerine yatırım yaptığını düşünüyorum 🔮"
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
            loadDemoData();
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
                            <span class="text-xl sm:text-2xl mb-1">😞</span>
                            <span class="font-medium text-gray-700 leading-tight text-center">Hiç Memnun<br>Değilim</span>
                        </button>
                        <button onclick="selectAnswer(2)" class="answer-btn flex flex-col items-center justify-center py-3 px-2 text-xs sm:text-base rounded-xl border-2 border-orange-200 hover:border-orange-400 hover:bg-orange-50 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-orange-400 bg-gray-50 shadow-sm">
                            <span class="text-xl sm:text-2xl mb-1">😐</span>
                            <span class="font-medium text-gray-700 leading-tight text-center">Memnun<br>Değilim</span>
                        </button>
                        <button onclick="selectAnswer(3)" class="answer-btn flex flex-col items-center justify-center py-3 px-2 text-xs sm:text-base rounded-xl border-2 border-yellow-200 hover:border-yellow-400 hover:bg-yellow-50 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-yellow-400 bg-gray-50 shadow-sm col-span-2 sm:col-span-1">
                            <span class="text-xl sm:text-2xl mb-1">😊</span>
                            <span class="font-medium text-gray-700 leading-tight text-center">Kararsızım</span>
                        </button>
                        <button onclick="selectAnswer(4)" class="answer-btn flex flex-col items-center justify-center py-3 px-2 text-xs sm:text-base rounded-xl border-2 border-green-200 hover:border-green-400 hover:bg-green-50 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-green-400 bg-gray-50 shadow-sm">
                            <span class="text-xl sm:text-2xl mb-1">😄</span>
                            <span class="font-medium text-gray-700 leading-tight text-center">Memnunum</span>
                        </button>
                        <button onclick="selectAnswer(5)" class="answer-btn flex flex-col items-center justify-center py-3 px-2 text-xs sm:text-base rounded-xl border-2 border-blue-200 hover:border-blue-400 hover:bg-blue-50 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-blue-400 bg-gray-50 shadow-sm">
                            <span class="text-xl sm:text-2xl mb-1">🤩</span>
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

        // JSONBin.io API fonksiyonları
        async function createNewBin() {
            // Sabit binId kullanıldığı için yeni bin oluşturulmayacak
            throw new Error('Yeni bin oluşturma devre dışı. Sabit binId kullanılmaktadır.');
        }

        async function loadFromJSONBin() {
            try {
                // Sabit binId kullanıldığı için localStorage kontrolü kaldırıldı
                if (!JSONBIN_CONFIG.binId) {
                    throw new Error('Sabit binId tanımlı değil!');
                }
                console.log('JSONBin\'den veri yükleniyor... Bin ID:', JSONBIN_CONFIG.binId);
                const response = await fetch(`${JSONBIN_CONFIG.baseUrl}/b/${JSONBIN_CONFIG.binId}/latest`, {
                    headers: {
                        'X-Master-Key': JSONBIN_CONFIG.apiKey,
                        'X-Access-Key': JSONBIN_CONFIG.accessKey,
                        'X-Bin-Meta': 'false'
                    }
                });
                console.log('JSONBin yanıt durumu:', response.status);
                if (response.ok) {
                    const data = await response.json();
                    console.log('JSONBin verisi yüklendi:', data);
                    systemData.surveyData = data.record || data;
                    return systemData.surveyData;
                } else {
                    const errorText = await response.text();
                    console.error('JSONBin yanıt hatası:', response.status, response.statusText, errorText);
                    throw new Error(`API Hatası: ${response.status} - ${errorText}`);
                }
            } catch (error) {
                console.error('JSONBin yükleme hatası:', error);
                // Varsayılan yapı döndür
                const defaultData = {
                    surveyName: "İşletme Yönetim Anketi - Sürüm 12",
                    createdAt: new Date().toISOString(),
                    responses: [],
                    statistics: {
                        totalResponses: 0,
                        averageScore: 0,
                        lastUpdated: new Date().toISOString()
                    },
                    companies: {}
                };
                systemData.surveyData = defaultData;
                return defaultData;
            }
        }

        async function saveToJSONBin(data, retryCount = 0) {
            try {
                // Sabit binId kullanıldığı için yeni bin oluşturulmayacak
                if (!JSONBIN_CONFIG.binId) {
                    throw new Error('Sabit binId tanımlı değil!');
                }
                console.log(`JSONBin'e veri kaydediliyor... Bin ID: ${JSONBIN_CONFIG.binId} (Deneme ${retryCount + 1}/${JSONBIN_CONFIG.maxRetries + 1})`);
                const response = await fetch(`${JSONBIN_CONFIG.baseUrl}/b/${JSONBIN_CONFIG.binId}`, {
                    method: 'PUT',
                    headers: {
                        'Content-Type': 'application/json',
                        'X-Master-Key': JSONBIN_CONFIG.apiKey,
                        'X-Access-Key': JSONBIN_CONFIG.accessKey,
                        'X-Bin-Versioning': 'false'
                    },
                    body: JSON.stringify(data)
                });
                console.log('JSONBin yanıt durumu:', response.status, response.statusText);
                if (response.ok) {
                    const result = await response.json();
                    console.log('JSONBin kaydetme başarılı:', result);
                    return { success: true, data: result };
                } else {
                    const errorText = await response.text();
                    console.error('JSONBin API hatası:', response.status, response.statusText, errorText);
                    // Yeniden deneme mantığı
                    if (retryCount < JSONBIN_CONFIG.maxRetries && (response.status >= 500 || response.status === 429)) {
                        console.log(`${JSONBIN_CONFIG.retryDelay}ms sonra yeniden denenecek...`);
                        await new Promise(resolve => setTimeout(resolve, JSONBIN_CONFIG.retryDelay * (retryCount + 1)));
                        return await saveToJSONBin(data, retryCount + 1);
                    }
                    return { success: false, error: `API Hatası: ${response.status} - ${errorText}` };
                }
            } catch (error) {
                console.error('JSONBin bağlantı hatası:', error);
                // Ağ hatalarında yeniden deneme
                if (retryCount < JSONBIN_CONFIG.maxRetries) {
                    console.log(`Ağ hatası - ${JSONBIN_CONFIG.retryDelay}ms sonra yeniden denenecek...`);
                    await new Promise(resolve => setTimeout(resolve, JSONBIN_CONFIG.retryDelay * (retryCount + 1)));
                    return await saveToJSONBin(data, retryCount + 1);
                }
                return { success: false, error: `Bağlantı Hatası: ${error.message}` };
            }
        }

        async function createCompanyIfNotExists(companyName) {
            try {
                console.log('Şirket kontrol ediliyor:', companyName);
                
                if (!systemData.surveyData) {
                    console.log('Sistem verisi yükleniyor...');
                    systemData.surveyData = await loadFromJSONBin();
                }
                
                // Mevcut şirket var mı kontrol et
                const existingCompany = Object.entries(systemData.surveyData.companies || {})
                    .find(([key, company]) => company.name.toLowerCase() === companyName.toLowerCase());
                
                if (existingCompany) {
                    console.log('Mevcut şirket bulundu:', existingCompany[1]);
                    return { success: true, key: existingCompany[0], password: existingCompany[1].password };
                }
                
                // Yeni şirket oluştur
                const companyKey = companyName.toLowerCase().replace(/[^a-z0-9]/g, '').substring(0, 10) + '-' + Date.now();
                const newPassword = generateCompanyPassword();
                
                if (!systemData.surveyData.companies) {
                    systemData.surveyData.companies = {};
                }
                
                systemData.surveyData.companies[companyKey] = {
                    name: companyName,
                    password: newPassword,
                    createdAt: new Date().toISOString(),
                    totalResponses: 0,
                    status: 'aktif' // yeni şirketler varsayılan olarak aktif
                };
                
                console.log('Yeni şirket oluşturuldu:', systemData.surveyData.companies[companyKey]);
                
                // Şirket bilgilerini kaydet
                const saveResult = await saveToJSONBin(systemData.surveyData);
                if (saveResult.success) {
                    console.log('Şirket başarıyla kaydedildi');
                    return { success: true, key: companyKey, password: newPassword };
                } else {
                    console.error('Şirket kaydetme hatası:', saveResult.error);
                    return { success: false, error: saveResult.error };
                }
                
            } catch (error) {
                console.error('Şirket oluşturma hatası:', error);
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
                
                // Mevcut verileri tekrar yükle (güncel hali için)
                systemData.surveyData = await loadFromJSONBin();
                
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
                
                // Yanıtı ekle
                if (!systemData.surveyData.responses) {
                    systemData.surveyData.responses = {};
                }
                systemData.surveyData.responses[surveyResponse.id] = surveyResponse;
                
                // İstatistikleri güncelle
                if (!systemData.surveyData.statistics) {
                    systemData.surveyData.statistics = {
                        totalResponses: 0,
                        averageScore: 0,
                        lastUpdated: new Date().toISOString()
                    };
                }
                
                systemData.surveyData.statistics.totalResponses = Object.keys(systemData.surveyData.responses).length;
                systemData.surveyData.statistics.averageScore = (
                    Object.values(systemData.surveyData.responses).reduce((sum, r) => sum + parseFloat(r.averageScore), 0) / 
                    Object.keys(systemData.surveyData.responses).length
                ).toFixed(2);
                systemData.surveyData.statistics.lastUpdated = new Date().toISOString();
                
                // Şirket istatistiklerini güncelle
                if (companyResult && systemData.surveyData.companies[companyResult.key]) {
                    systemData.surveyData.companies[companyResult.key].totalResponses = 
                        Object.values(systemData.surveyData.responses).filter(r => 
                            r.companyName.toLowerCase() === companyName.toLowerCase()
                        ).length;
                }
                
                console.log('Güncellenmiş sistem verisi:', systemData.surveyData);
                
                // JSONBin'e kaydet
                const saveResult = await saveToJSONBin(systemData.surveyData);
                
                if (saveResult.success) {
                    console.log('Anket başarıyla kaydedildi');
                    
                    // Başarı mesajı göster
                    document.getElementById('surveySection').innerHTML = `
                        <div class="text-center bg-green-50 p-8 rounded-lg border-2 border-green-200">
                            <div class="text-6xl mb-4">✅</div>
                            <h2 class="text-2xl font-bold text-green-800 mb-4">Anketiniz Başarıyla Kaydedildi!</h2>
                            <p class="text-green-700 mb-4">
                                Değerli görüşleriniz için teşekkür ederiz. Anket yanıtlarınız güvenli bir şekilde JSONBin.io sisteminde saklandı.
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
                } else {
                    throw new Error(`Anket kaydedilemedi: ${saveResult.error}`);
                }
                
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
                if (!systemData.surveyData) {
                    systemData.surveyData = await loadFromJSONBin();
                }
                
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
        }

        function generateSimpleReport(surveys) {
            if (surveys.length === 0) {
                document.getElementById('detailedReport').innerHTML = '<p class="text-gray-500 text-center py-8">Henüz anket verisi bulunmuyor.</p>';
                return;
            }
            
            // Pozisyon dağılımı
            const positionData = {};
            surveys.forEach(s => {
                positionData[s.jobType] = (positionData[s.jobType] || 0) + 1;
            });
            
            // Memnuniyet dağılımı - kişi bazında hesaplama
            const satisfactionLevels = ['Düşük (1-2)', 'Orta (3)', 'Yüksek (4-5)'];
            const satisfactionCounts = [0, 0, 0];
            
            surveys.forEach(s => {
                const avgScore = parseFloat(s.averageScore);
                if (avgScore < 2.5) satisfactionCounts[0]++;
                else if (avgScore >= 2.5 && avgScore < 3.5) satisfactionCounts[1]++;
                else satisfactionCounts[2]++;
            });
            
            const report = `
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
            
            document.getElementById('detailedReport').innerHTML = report;
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
                    systemData.surveyData = await loadFromJSONBin();
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
                showModal('❌ Hata', 'Yönetici paneli yüklenirken hata oluştu.');
            }
        }

        function loadCompanyList() {
            const tbody = document.getElementById('companyList');
            if (!systemData.surveyData || !systemData.surveyData.companies) {
                tbody.innerHTML = '<tr><td colspan="5" class="text-center py-4 text-gray-500">Henüz şirket kaydı bulunmuyor.</td></tr>';
                return;
            }
            const companies = systemData.surveyData.companies;
            const responses = systemData.surveyData.responses || [];
            // Alfabetik sıralama
            const sortedEntries = Object.entries(companies).sort((a, b) => {
                const nameA = a[1].name.toLowerCase();
                const nameB = b[1].name.toLowerCase();
                return nameA.localeCompare(nameB, 'tr');
            });
            // Arama filtresi
            const searchValue = (document.getElementById('companySearchInput')?.value || '').toLowerCase();
            const filteredEntries = sortedEntries.filter(([_, company]) =>
                company.name.toLowerCase().includes(searchValue)
            );
            tbody.innerHTML = filteredEntries.length === 0
                ? '<tr><td colspan="5" class="text-center py-4 text-gray-500">Sonuç bulunamadı.</td></tr>'
                : filteredEntries.map(([companyKey, company]) => {
                    const companySurveys = responses.filter(s => 
                        s.companyName.toLowerCase() === company.name.toLowerCase()
                    );
                    const isActive = !company.status || company.status === 'aktif';
                    return `
                        <tr class="border-b hover:bg-gray-50">
                            <td class="px-4 py-2 font-medium">${company.name}</td>
                            <td class="px-4 py-2">
                                <code class="bg-gray-100 px-2 py-1 rounded text-sm">${company.password}</code>
                            </td>
                            <td class="px-4 py-2">${companySurveys.length}</td>
                            <td class="px-4 py-2">
                                <span class="px-2 py-1 rounded-full text-xs ${isActive ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800'}">
                                    ${isActive ? '🟢 Aktif' : '⛔ Pasif'}
                                </span>
                            </td>
                            <td class="px-4 py-2">
                                <button onclick="showAdminCompanyReport('${company.name}')" class="text-green-600 hover:text-green-800 mr-2">📊 Rapor</button>
                                <button onclick="resetCompanyPassword('${companyKey}')" class="text-orange-600 hover:text-orange-800 mr-2">🔄 Şifre</button>
                                <button onclick="toggleCompanyStatus('${companyKey}')" class="text-blue-600 hover:text-blue-800">${isActive ? 'Askıya Al' : 'Aktif Et'}</button>
                            </td>
                        </tr>
                    `;
                }).join('');
        }

        // Şirketi askıya al/aktifleştir fonksiyonu
        async function toggleCompanyStatus(companyKey) {
            if (!systemData.surveyData || !systemData.surveyData.companies[companyKey]) return;
            const company = systemData.surveyData.companies[companyKey];
            company.status = (!company.status || company.status === 'aktif') ? 'pasif' : 'aktif';
            const saveResult = await saveToJSONBin(systemData.surveyData);
            if (saveResult.success) {
                loadCompanyList();
                showModal('Durum Güncellendi', `${company.name} şirketi artık <b>${company.status === 'aktif' ? 'Aktif' : 'Pasif (Askıda)'}</b> durumunda.`);
            } else {
                showModal('❌ Hata', `Durum güncellenemedi: ${saveResult.error}`);
            }

        }

        // Arama kutusu için canlı filtreleme fonksiyonu
        function filterCompanyList() {
            loadCompanyList();
        }

        async function resetCompanyPassword(companyKey) {
            if (!systemData.surveyData || !systemData.surveyData.companies[companyKey]) return;
            
            const newPassword = generateCompanyPassword();
            systemData.surveyData.companies[companyKey].password = newPassword;
            
            const saveResult = await saveToJSONBin(systemData.surveyData);
            if (saveResult.success) {
                loadCompanyList();
                showModal('🔄 Şifre Yenilendi', `${systemData.surveyData.companies[companyKey].name} için yeni şifre: <code>${newPassword}</code>`);
            } else {
                showModal('❌ Hata', `Şifre yenileme sırasında hata oluştu: ${saveResult.error}`);
            }
        }

        function copyToClipboard(text) {
            navigator.clipboard.writeText(text).then(() => {
                showModal('📋 Kopyalandı', `Şifre panoya kopyalandı: ${text}`);
            });
        }

        async function showAdminCompanyReport(companyName) {
            try {
                if (!systemData.surveyData) {
                    systemData.surveyData = await loadFromJSONBin();
                }
                
                const companySurveys = systemData.surveyData.responses.filter(s => 
                    s.companyName.toLowerCase() === companyName.toLowerCase()
                );
                
                if (companySurveys.length === 0) {
                    showModal('📊 Rapor', `${companyName} için henüz anket verisi bulunmuyor.`);
                    return;
                }
                
                const pdfContent = generateAdminPDFContent(companyName, companySurveys);
                const pdfWindow = window.open('', '_blank', 'width=800,height=600');
                pdfWindow.document.write(pdfContent);
                pdfWindow.document.close();
                
            } catch (error) {
                console.error('Admin rapor hatası:', error);
                showModal('❌ Hata', 'Rapor oluşturulurken hata oluştu.');
            }
        }

        function logoutAdmin() {
            isAdminLoggedIn = false;
            document.getElementById('adminLogin').classList.remove('hidden');
            document.getElementById('adminDashboard').classList.add('hidden');
            document.getElementById('adminPassword').value = '';
        }

        function analyzeCategoryPerformance(surveys) {
            // Her pozisyon için 50 soru, 5 kategoriye bölünmüş (her kategori 10 soru)
            const categories = {
                'Çalışma Ortamı ve Konfor': { range: [0, 9], name: 'Çalışma Ortamı ve Konfor' },
                'Yemek ve Sosyal Haklar': { range: [10, 19], name: 'Yemek ve Sosyal Haklar' },
                'İş İlişkileri ve Güven': { range: [20, 29], name: 'İş İlişkileri ve Güven' },
                'Sadakat ve Kariyer': { range: [30, 39], name: 'Sadakat ve Kariyer' },
                'Dijital Dönüşüm ve Yenilenme': { range: [40, 49], name: 'Dijital Dönüşüm ve Yenilenme' }
            };
            
            const categoryResults = {};
            
            Object.keys(categories).forEach(categoryKey => {
                const category = categories[categoryKey];
                let totalScore = 0;
                let totalQuestions = 0;
                
                surveys.forEach(survey => {
                    for (let i = category.range[0]; i <= category.range[1]; i++) {
                        if (survey.answers[i]) {
                            totalScore += survey.answers[i].score;
                            totalQuestions++;
                        }
                    }
                });
                
                const avgScore = totalQuestions > 0 ? (totalScore / totalQuestions).toFixed(1) : '0.0';
                let status = 'Düşük';
                let comment = '';
                
                if (parseFloat(avgScore) >= 4.0) {
                    status = 'Yüksek';
                    if (categoryKey === 'Çalışma Ortamı ve Konfor') {
                        comment = 'Çalışanlar fiziksel ortamdan ve sağlanan olanaklardan oldukça memnun. Bu, şirketin çalışan refahına yaptığı yatırımların karşılığını aldığının bir göstergesidir. Ortamın konforu, verimliliği ve motivasyonu olumlu etkiliyor.';
                    } else if (categoryKey === 'Dijital Dönüşüm ve Yenilenme') {
                        comment = 'Çalışanlar dijitalleşmeye tamamen hazır ve bu süreci destekliyorlar. Şirketin bu alandaki yatırımları ve yenilikçi duruşu takdir görüyor. Bu, şirketin geleceğe dönük vizyonunun güçlü olduğunu gösterir.';
                    } else {
                        comment = 'Bu kategoride yüksek memnuniyet seviyesi tespit edildi. Mevcut uygulamalar başarılı ve sürdürülmeli.';
                    }
                } else if (parseFloat(avgScore) >= 3.0) {
                    status = 'Orta';
                    if (categoryKey === 'İş İlişkileri ve Güven') {
                        comment = 'İletişim genel olarak iyi ancak bazı belirsizlikler mevcut. Yöneticilerle çalışanlar arasında güçlü bir bağ var ancak geri bildirim mekanizmaları ve adalet algısı konusunda iyileştirme potansiyeli bulunuyor.';
                    } else if (categoryKey === 'Sadakat ve Kariyer') {
                        comment = 'Çalışanlar şirkete bağlı hissediyor ancak kariyer gelişim yolları net değil. Uzun vadeli planlar yapmakta zorlanıyorlar. Bu durumu tersine çevirmek için şeffaf bir kariyer planlaması ve eğitim fırsatları sunulmalı.';
                    } else {
                        comment = 'Bu kategoride orta seviye memnuniyet var. İyileştirme potansiyeli mevcut ve odaklanılması gereken alanlar belirlenebilir.';
                    }
                } else {
                    status = 'Düşük';
                    if (categoryKey === 'Yemek ve Sosyal Haklar') {
                        comment = 'Maaşlar, yan haklar ve yemek kalitesi gibi konularda ciddi bir memnuniyetsizlik var. Bu, çalışanların şirkete olan sadakatini düşürebilir ve dışarıdan yetenek çekmeyi zorlaştırabilir. Bu alan acil iyileştirme gerektiriyor.';
                    } else {
                        comment = 'Bu kategoride düşük memnuniyet tespit edildi. Acil eylem planı gerekli ve kök neden analizi yapılmalı.';
                    }
                }
                
                categoryResults[categoryKey] = {
                    name: category.name,
                    score: avgScore,
                    status: status,
                    comment: comment
                };
            });
            
            return categoryResults;
        }

        // showPDFReport(true) => filtreli, showPDFReport(false) => tümü
        function showPDFReport(filtered) {
            if (!loggedInCompany || !systemData.surveyData) return;
            let surveys;
            let dateInfo = '';
            if (filtered && filteredSurveys !== null) {
                surveys = filteredSurveys;
                const start = document.getElementById('reportStartDate').value;
                const end = document.getElementById('reportEndDate').value;
                if (start && end) dateInfo = ` - ${start} / ${end}`;
                else if (start) dateInfo = ` - ${start} sonrası`;
                else if (end) dateInfo = ` - ${end} öncesi`;
            } else {
                surveys = systemData.surveyData.responses.filter(s => s.companyName.toLowerCase() === loggedInCompany.name.toLowerCase());
            }
            const pdfContent = generatePDFContent(surveys, dateInfo);
            const pdfWindow = window.open('', '_blank', 'width=800,height=600');
            pdfWindow.document.write(pdfContent);
            pdfWindow.document.close();
        }

        function generateAdminPDFContent(companyName, surveys) {
            const totalParticipants = surveys.length;
            
            // Doğru ortalama hesaplama
            let totalScore = 0;
            let totalAnswers = 0;
            surveys.forEach(s => {
                totalScore += s.totalScore;
                totalAnswers += s.answers.length;
            });
            const avgScore = totalAnswers > 0 ? (totalScore / totalAnswers).toFixed(1) : '0.0';
            
            // Memnuniyet oranı - 4 ve 5 puan verenlerin oranı
            let highSatisfactionCount = 0;
            surveys.forEach(s => {
                s.answers.forEach(answer => {
                    if (answer.score >= 4) highSatisfactionCount++;
                });
            });
            const satisfactionRate = totalAnswers > 0 ? Math.round((highSatisfactionCount / totalAnswers) * 100) : 0;
            
            const positionData = {};
            surveys.forEach(s => {
                positionData[s.jobType] = (positionData[s.jobType] || 0) + 1;
            });
            
            const satisfactionCounts = [0, 0, 0];
            surveys.forEach(s => {
                s.answers.forEach(answer => {
                    if (answer.score <= 2) satisfactionCounts[0]++;
                    else if (answer.score === 3) satisfactionCounts[1]++;
                    else satisfactionCounts[2]++;
                });
            });
            
            return `
                <!DOCTYPE html>
                <html>
                <head>
                    <meta charset="UTF-8">
                    <title>${companyName} - Yönetici Raporu</title>
                    <style>
                        body { font-family: Arial, sans-serif; margin: 15px; line-height: 1.5; font-size: 14px; }
                        .header { text-align: center; border-bottom: 2px solid #333; padding-bottom: 15px; margin-bottom: 20px; }
                        .stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; margin-bottom: 20px; }
                        .stat-box { background: #f5f5f5; padding: 12px; border-radius: 6px; text-align: center; }
                        .stat-number { font-size: 1.8em; font-weight: bold; color: #333; }
                        .section { margin-bottom: 20px; }
                        .section h3 { color: #333; border-bottom: 1px solid #ddd; padding-bottom: 8px; font-size: 16px; }
                        table { width: 100%; border-collapse: collapse; margin-top: 8px; font-size: 13px; }
                        th, td { border: 1px solid #ddd; padding: 6px; text-align: left; }
                        th { background-color: #f2f2f2; }
                        .footer { margin-top: 30px; text-align: center; font-size: 12px; color: #666; }
                    </style>
                </head>
                <body>
                    <div class="header">
                        <h1>📊 ${companyName}</h1>
                        <h2>Yönetici Anket Raporu</h2>
                        <p>Rapor Tarihi: ${new Date().toLocaleDateString('tr-TR')}</p>
                    </div>
                    
                    <div class="stats">
                        <div class="stat-box">
                            <div class="stat-number">${totalParticipants}</div>
                            <div>Toplam Katılımcı</div>
                        </div>
                        <div class="stat-box">
                            <div class="stat-number">${avgScore}</div>
                            <div>Ortalama Puan</div>
                        </div>
                        <div class="stat-box">
                            <div class="stat-number">${satisfactionRate}%</div>
                            <div>Memnuniyet Oranı</div>
                        </div>
                    </div>
                    
                    <div class="section">
                        <h3>👥 Pozisyon Dağılımı</h3>
                        <table>
                            <tr><th>Pozisyon</th><th>Katılımcı</th><th>Yüzde</th></tr>
                            ${Object.entries(positionData).map(([pos, count]) => 
                                `<tr><td>${pos}</td><td>${count}</td><td>${Math.round((count/totalParticipants)*100)}%</td></tr>`
                            ).join('')}
                        </table>
                    </div>
                    
                    <div class="section">
                        <h3>📈 Memnuniyet Analizi</h3>
                        <table>
                            <tr><th>Seviye</th><th>Cevap Sayısı</th><th>Oran</th></tr>
                            <tr><td>Düşük (1-2)</td><td>${satisfactionCounts[0]}</td><td>${totalAnswers > 0 ? Math.round((satisfactionCounts[0]/totalAnswers)*100) : 0}%</td></tr>
                            <tr><td>Orta (3)</td><td>${satisfactionCounts[1]}</td><td>${totalAnswers > 0 ? Math.round((satisfactionCounts[1]/totalAnswers)*100) : 0}%</td></tr>
                            <tr><td>Yüksek (4-5)</td><td>${satisfactionCounts[2]}</td><td>${totalAnswers > 0 ? Math.round((satisfactionCounts[2]/totalAnswers)*100) : 0}%</td></tr>
                        </table>
                    </div>
                    
                    <div class="footer">
                        <p>Akça Pro X - Yönetici Raporu | ${new Date().toLocaleString('tr-TR')}</p>
                    </div>
                </body>
                </html>
            `;
        }

        function generatePDFContent(surveys, dateInfo = '') {
            const companyName = loggedInCompany ? loggedInCompany.name : '';
            const now = new Date();
            const dateStr = now.toLocaleDateString('tr-TR');
            const timeStr = now.toLocaleTimeString('tr-TR');
            const totalParticipants = surveys.length;
            let totalScore = 0;
            let totalAnswers = 0;
            surveys.forEach(s => {
                totalScore += s.totalScore;
                totalAnswers += s.answers.length;
            });
            const avgScore = totalAnswers > 0 ? (totalScore / totalAnswers).toFixed(1) : '0.0';
            const minPossibleScore = totalAnswers * 1;
            const maxPossibleScore = totalAnswers * 5;
            const satisfactionPercent = totalAnswers > 0 ? Math.round(((totalScore - minPossibleScore) / (maxPossibleScore - minPossibleScore)) * 100) : 0;
            // Genel durum kutusu
            let statusBox = '';
            if (satisfactionPercent < 50) {
                statusBox = `<div style='background:#fee2e2;padding:16px;border-radius:8px;margin-bottom:12px;'><b>Düşük Memnuniyet (%0-50) - Acil Müdahale Gerekli</b></div>`;
            } else if (satisfactionPercent < 80) {
                statusBox = `<div style='background:#fef9c3;padding:16px;border-radius:8px;margin-bottom:12px;'><b>Orta Memnuniyet (%51-80) - İyileştirme Gerekli</b></div>`;
            } else {
                statusBox = `<div style='background:#dcfce7;padding:16px;border-radius:8px;margin-bottom:12px;'><b>Yüksek Memnuniyet (%81-100)</b></div>`;
            }
            // Pozisyon analizi
            const positionData = {};
            surveys.forEach(s => {
                positionData[s.jobType] = (positionData[s.jobType] || 0) + 1;
            });
            // Değerlendirme dağılımı
            const satisfactionCounts = [0, 0, 0];
            surveys.forEach(s => {
                const avg = parseFloat(s.averageScore);
                if (avg < 2.5) satisfactionCounts[0]++;
                else if (avg < 3.5) satisfactionCounts[1]++;
                else satisfactionCounts[2]++;
            });
            // Yanıt dağılımı
            const answerLevels = ['Düşük Memnuniyet (1-2)', 'Orta Memnuniyet (3)', 'Yüksek Memnuniyet (4-5)'];
            const answerCounts = [0, 0, 0];
            surveys.forEach(s => {
                s.answers.forEach(a => {
                    if (a.score < 2.5) answerCounts[0]++;
                    else if (a.score < 3.5) answerCounts[1]++;
                    else answerCounts[2]++;
                });
            });
            // Kategori analizleri (örnek başlıklar)
            const businessCategories = [
                { title: '1. Çalışma Ortamı ve Konfor', desc: 'Fiziksel ortam, ekipman, hijyen ve güvenlik koşulları.' },
                { title: '2. Yemek ve Sosyal Haklar', desc: 'Yemek kalitesi, sosyal haklar ve yan haklar.' },
                { title: '3. İş İlişkileri ve Güven', desc: 'Yönetici ve ekip ilişkileri, iletişim ve güven.' },
                { title: '4. Sadakat ve Kariyer', desc: 'Şirkete bağlılık, kariyer gelişimi ve motivasyon.' },
                { title: '5. Dijital Dönüşüm ve Yenilenme', desc: 'Teknolojiye açıklık, dijitalleşme ve yenilikçilik.' }
            ];
            return `
            <html><head><title>${companyName} - İşletme Anketi Raporu</title>
            <style>
                body { font-family: Arial, sans-serif; margin: 0; padding: 0; }
                .header { text-align: center; margin-top: 24px; }
                .summary-grid { display: flex; justify-content: center; gap: 32px; margin: 24px 0; }
                .summary-box { background: #f8fafc; border-radius: 12px; padding: 24px 32px; min-width: 180px; text-align: center; font-size: 1.5rem; }
                .section { margin: 24px 0; }
                .section-title { font-size: 1.2rem; font-weight: bold; margin-bottom: 8px; }
                .table { width: 100%; border-collapse: collapse; margin-bottom: 16px; }
                .table th, .table td { border: 1px solid #e5e7eb; padding: 8px 12px; text-align: left; }
                .table th { background: #f1f5f9; }
                .highlight { font-weight: bold; color: #dc2626; }
                .info-box { background: #f1f5f9; border-radius: 8px; padding: 16px; margin-bottom: 12px; }
                .category-box { background: #fef2f2; border-radius: 8px; padding: 16px; margin-bottom: 12px; }
                .advice-box { background: #fef9c3; border-radius: 8px; padding: 16px; margin-bottom: 12px; }
                .date-info { background: #dbeafe; border-radius: 8px; padding: 12px; margin-bottom: 16px; text-align: center; font-weight: bold; color: #1e40af; }
            </style></head><body onload="window.print()">
                <div class='header'>
                    <div style='font-size:2.2rem;font-weight:bold;margin-bottom:8px;'>🏢 ${companyName}</div>
                    <div style='font-size:1.3rem;font-weight:bold;'>İşletme Değerlendirme Anketi Raporu${dateInfo}</div>
                    <div style='font-size:1rem;margin-top:4px;'>Rapor Tarihi: ${dateStr}</div>
                </div>
                ${dateInfo ? `<div class='date-info'>📅 Filtrelenmiş Rapor${dateInfo}</div>` : ''}
                <div class='summary-grid'>
                <!-- SWOT Analizi Tablosu (PDF) -->
                <div class='section'>
                    <div class='section-title'>SWOT Analizi</div>
                    <table style="width:100%;border-collapse:collapse;margin:24px 0;">
                        <tr>
                            <th style="background:#d1fae5;border:1px solid #a3a3a3;padding:10px;">Güçlü Yönler</th>
                            <th style="background:#fee2e2;border:1px solid #a3a3a3;padding:10px;">Zayıf Yönler</th>
                            <th style="background:#dbeafe;border:1px solid #a3a3a3;padding:10px;">Fırsatlar</th>
                            <th style="background:#fef9c3;border:1px solid #a3a3a3;padding:10px;">Tehditler</th>
                        </tr>
                        <tr>
                            <td style="border:1px solid #a3a3a3;padding:10px;vertical-align:top;">• Yüksek çalışan memnuniyeti<br>• Güçlü ekip ruhu<br>• Modern altyapı</td>
                            <td style="border:1px solid #a3a3a3;padding:10px;vertical-align:top;">• Yoğun dönemlerde iletişim eksikliği<br>• Kısıtlı sosyal imkanlar<br>• Dijitalleşme eksikliği</td>
                            <td style="border:1px solid #a3a3a3;padding:10px;vertical-align:top;">• Dijitalleşme yatırımları<br>• Yeni pazar fırsatları<br>• Kamu destekleri</td>
                            <td style="border:1px solid #a3a3a3;padding:10px;vertical-align:top;">• Artan rekabet<br>• Ekonomik dalgalanmalar<br>• Personel değişimi</td>
                        </tr>
                    </table>
                </div>
                    <div class='summary-box'><div style='font-size:1.1rem;'>${totalParticipants}</div>Toplam Katılımcı</div>
                    <div class='summary-box'><div style='font-size:1.1rem;'>${avgScore}</div>Ortalama Puan</div>
                    <div class='summary-box'><div style='font-size:1.1rem;'>${satisfactionPercent}%</div>Genel Memnuniyet</div>
                </div>
                <div class='section info-box'>
                    <div class='section-title'>☑️ Genel Durum Değerlendirmesi</div>
                    ${statusBox}
                    <div>Memnuniyet Hesaplama Formülü: ((Alınan Puan - Minimum Puan) / (Maksimum Puan - Minimum Puan)) × 100 = ${satisfactionPercent}%</div>
                    <div style='margin-top:8px;'>İşletmeniz için ${dateInfo ? 'seçilen tarih için' : 'tüm katılımcılarda'} genel memnuniyet düzeyi yukarıda gösterilmiştir.</div>
                </div>
                <div class='section'>
                    <div class='section-title'>👥 Katılımcı Grupları Analizi</div>
                    <table class='table'>
                        <tr><th>Katılımcı Grubu</th><th>Katılımcı</th></tr>
                        ${Object.entries(positionData).map(([pos, count]) => `<tr><td>${pos}</td><td>${count}</td></tr>`).join('')}
                    </table>
                </div>
                <div class='section'>
                    <div class='section-title'>☑️ Yanıt Dağılımı</div>
                    <table class='table'>
                        <tr><th>Değerlendirme Seviyesi</th><th>Yanıt Sayısı</th></tr>
                        ${answerLevels.map((level, i) => `<tr><td>${level}</td><td>${answerCounts[i]}</td></tr>`).join('')}
                    </table>
                </div>
                <div class='section'>
                    <div class='section-title'>📊 Detaylı Kategori Analizleri</div>
                    ${businessCategories.map(cat => `
                        <div class='category-box'>
                            <b>${cat.title}</b><br>
                            <span style='font-size:0.95rem;'>${cat.desc}</span>
                            <div style='margin-top:8px;background:#fee2e2;padding:8px;border-radius:6px;'><b>Puan Aralığı: Düşük (%0-50)</b> - Bu kategoride ciddi iyileştirme gereklidir.</div>
                        </div>
                    `).join('')}
                </div>
                <div class='section advice-box'>
                    <b>💡 Öneriler ve Eylem Planı</b><br>
                    <b>Öncelikli Aksiyonlar:</b> Çalışma ortamı, sosyal haklar ve iş ilişkileri gözden geçirilmeli.<br>
                    <b>Takip:</b> Bu rapor sonuçlarını 3-6 ay sonra tekrar değerlendirmek için yeni anket düzenleyiniz.
                </div>
                <div style='text-align:right;font-size:0.9rem;color:#888;margin-top:32px;'>Akça Pro X - Kurumsal Anket ve Raporlama Sistemi | ${dateStr} ${timeStr}<br>Bu rapor ${totalAnswers} adet soru yanıtı analiz edilerek oluşturulmuştur.${dateInfo ? `<br>Filtre: ${dateInfo}` : ''}</div>
            </body></html>
            `;
        }

        function generateCharts(surveys) {
            if (surveys.length === 0) return;
            
            // 1. Pozisyon Dağılımı Grafiği
            const positionData = {};
            surveys.forEach(s => {
                positionData[s.jobType] = (positionData[s.jobType] || 0) + 1;
            });
            
            const positionCtx = document.getElementById('positionChart').getContext('2d');
            new Chart(positionCtx, {
                type: 'doughnut',
                data: {
                    labels: Object.keys(positionData),
                    datasets: [{
                        data: Object.values(positionData),
                        backgroundColor: ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6']
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    }
                }
            });
            
            // 2. Memnuniyet Dağılımı - kişi bazında
            const satisfactionCounts = [0, 0, 0];
            surveys.forEach(s => {
                const avgScore = parseFloat(s.averageScore);
                if (avgScore < 2.5) satisfactionCounts[0]++;
                else if (avgScore >= 2.5 && avgScore < 3.5) satisfactionCounts[1]++;
                else satisfactionCounts[2]++;
            });
            
            const satisfactionCtx = document.getElementById('satisfactionChart').getContext('2d');
            new Chart(satisfactionCtx, {
                type: 'bar',
                data: {
                    labels: ['Düşük', 'Orta', 'Yüksek'],
                    datasets: [{
                        data: satisfactionCounts,
                        backgroundColor: ['#ef4444', '#f59e0b', '#10b981']
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            });
            
            // 3. Zaman Dağılımı - histogram şeklinde
            const timeRanges = ['0-2 dk', '2-4 dk', '4-6 dk', '6+ dk'];
            const timeCounts = [0, 0, 0, 0];
            
            surveys.forEach(s => {
                const [min, sec] = s.duration.split(':').map(Number);
                const totalMinutes = min + (sec / 60);
                
                if (totalMinutes <= 2) timeCounts[0]++;
                else if (totalMinutes <= 4) timeCounts[1]++;
                else if (totalMinutes <= 6) timeCounts[2]++;
                else timeCounts[3]++;
            });
            
            const timeCtx = document.getElementById('timeChart').getContext('2d');
            new Chart(timeCtx, {
                type: 'bar',
                data: {
                    labels: timeRanges,
                    datasets: [{
                        data: timeCounts,
                        backgroundColor: ['#8b5cf6', '#a855f7', '#c084fc', '#d8b4fe']
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            title: {
                                display: true,
                                text: 'Kişi Sayısı'
                            }
                        }
                    }
                }
            });
            
            // 4. Puan Dağılımı - histogram şeklinde
            const scoreRanges = ['1.0-2.0', '2.1-3.0', '3.1-4.0', '4.1-5.0'];
            const scoreCounts = [0, 0, 0, 0];
            
            surveys.forEach(s => {
                const avgScore = parseFloat(s.averageScore);
                if (avgScore <= 2.0) scoreCounts[0]++;
                else if (avgScore <= 3.0) scoreCounts[1]++;
                else if (avgScore <= 4.0) scoreCounts[2]++;
                else scoreCounts[3]++;
            });
            
            const trendCtx = document.getElementById('trendChart').getContext('2d');
            new Chart(trendCtx, {
                type: 'bar',
                data: {
                    labels: scoreRanges,
                    datasets: [{
                        data: scoreCounts,
                        backgroundColor: ['#ef4444', '#f59e0b', '#10b981', '#059669']
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            title: {
                                display: true,
                                text: 'Kişi Sayısı'
                            }
                        }
                    }
                }
            });
        }

        function showModal(title, message) {
            document.getElementById('modalContent').innerHTML = `
                <h3 class="text-lg font-semibold mb-4">${title}</h3>
                <p class="mb-4">${message}</p>
                <button onclick="closeModal()" class="w-full py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700">
                    Tamam
                </button>
            `;
            document.getElementById('modal').classList.add('show');
        }

        function closeModal() {
            document.getElementById('modal').classList.remove('show');
        }

        // Demo veri yükleme
        function toggleParticipantDetails() {
            const detailsDiv = document.getElementById('participantDetails');
            const toggleBtn = document.getElementById('toggleParticipantsBtn');
            
            if (detailsDiv.classList.contains('hidden')) {
                detailsDiv.classList.remove('hidden');
                toggleBtn.textContent = '📋 Katılımcıları Gizle';
                loadParticipantDetails();
            } else {
                detailsDiv.classList.add('hidden');
                toggleBtn.textContent = '📋 Katılımcıları Görüntüle';
            }
        }

        function loadParticipantDetails() {
            if (!loggedInCompany || !systemData.surveyData) return;
            
            const companySurveys = systemData.surveyData.responses.filter(s => 
                s.companyName.toLowerCase() === loggedInCompany.name.toLowerCase()
            );
            
            const tbody = document.getElementById('participantTableBody');
            
            if (companySurveys.length === 0) {
                tbody.innerHTML = '<tr><td colspan="5" class="text-center py-4 text-gray-500">Henüz katılımcı bulunmuyor.</td></tr>';
                return;
            }
            
            tbody.innerHTML = companySurveys.map(survey => {
                // İsim kontrolü - boş veya sadece boşluk varsa "İsimsiz" yap
                const firstName = survey.firstName && survey.firstName.trim() !== '' && survey.firstName !== 'Anonim' ? survey.firstName : '';
                const lastName = survey.lastName && survey.lastName.trim() !== '' && survey.lastName !== 'Kullanıcı' ? survey.lastName : '';
                
                let displayName = '';
                if (firstName || lastName) {
                    displayName = `${firstName} ${lastName}`.trim();
                } else {
                    displayName = 'İsimsiz';
                }
                
                // Memnuniyet seviyesi hesaplama
                const avgScore = parseFloat(survey.averageScore);
                let satisfactionLevel = '';
                let satisfactionColor = '';
                
                if (avgScore < 2.5) {
                    satisfactionLevel = 'Düşük';
                    satisfactionColor = 'text-red-600 bg-red-50';
                } else if (avgScore >= 2.5 && avgScore < 3.5) {
                    satisfactionLevel = 'Orta';
                    satisfactionColor = 'text-yellow-600 bg-yellow-50';
                } else {
                    satisfactionLevel = 'Yüksek';
                    satisfactionColor = 'text-green-600 bg-green-50';
                }
                
                // Tarih formatı
                const submitDate = new Date(survey.submittedAt).toLocaleDateString('tr-TR');
                
                return `
                    <tr class="border-b hover:bg-gray-50">
                        <td class="px-3 py-2 font-medium">${displayName}</td>
                        <td class="px-3 py-2">${survey.jobType}</td>
                        <td class="px-3 py-2 text-center font-semibold">${avgScore}/5.0</td>
                        <td class="px-3 py-2 text-center">
                            <span class="px-2 py-1 rounded-full text-xs font-semibold ${satisfactionColor}">
                                ${satisfactionLevel}
                            </span>
                        </td>
                        <td class="px-3 py-2 text-center text-gray-600">${submitDate}</td>
                    </tr>
                `;
            }).join('');
        }

        function loadDemoData() {
            console.log('Demo veri yükleniyor...');
            loadFromJSONBin().then(data => {
                console.log('Demo veri yüklendi:', data);
            }).catch(error => {
                console.error('Demo veri yükleme hatası:', error);
            });
        }
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'981a0e1e1249b657',t:'MTc1ODI5NTEwMS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
