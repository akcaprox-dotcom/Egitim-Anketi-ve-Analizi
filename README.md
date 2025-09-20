<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Akça Pro X - Kurumsal Anket ve Raporlama Sistemi</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
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
    <nav class="gradient-bg text-white p-4 shadow-lg">
        <div class="container mx-auto flex justify-between items-center">
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
    <div id="surveyModule" class="container mx-auto p-4">
        <div class="bg-white shadow-xl rounded-xl max-w-2xl mx-auto p-6">
            <div class="text-center mb-6">
                <h2 class="text-2xl font-bold text-gray-800">Çalışan Memnuniyet Anketi</h2>
                <p class="text-gray-600 mt-2">Görüşleriniz bizim için değerli</p>
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
            <div id="companyInfoSection" class="opacity-50 pointer-events-none">
                <h3 class="text-base font-semibold text-gray-700 mb-2">Şirket ve Kişisel Bilgiler</h3>
                <input type="text" id="companyName" placeholder="Şirket adınızı girin" class="w-full border rounded-lg px-3 py-2 mb-2 text-sm focus:ring-2 focus:ring-purple-500">
                <div class="grid grid-cols-3 gap-2 mb-2">
                    <button type="button" id="blueCollar" class="py-2 text-xs rounded-lg border hover:bg-blue-50 transition-colors">👷 Mavi Yaka</button>
                    <button type="button" id="whiteCollar" class="py-2 text-xs rounded-lg border hover:bg-green-50 transition-colors">💼 Beyaz Yaka</button>
                    <button type="button" id="management" class="py-2 text-xs rounded-lg border hover:bg-purple-50 transition-colors">👔 Yönetim</button>
                </div>
                <div class="grid grid-cols-2 gap-2 mb-3">
                    <input type="text" id="firstName" placeholder="Adınız (İsteğe bağlı)" class="border rounded-lg px-3 py-2 text-sm focus:ring-2 focus:ring-purple-500">
                    <input type="text" id="lastName" placeholder="Soyadınız (İsteğe bağlı)" class="border rounded-lg px-3 py-2 text-sm focus:ring-2 focus:ring-purple-500">
                </div>
                <button id="startSurvey" class="w-full py-2 rounded-lg text-white font-semibold gradient-bg hover:opacity-90 transition-opacity text-sm">
                    📊 Anketi Başlat
                </button>
            </div>

            <!-- Anket Alanı -->
            <div id="surveySection" class="hidden">
                <div class="flex justify-between items-center mb-4">
                    <span id="progressText" class="text-gray-600">Anket İlerlemesi 0/25 Yanıtlandı</span>
                    <span id="timeElapsed" class="text-sm text-gray-500">Süre: 00:00</span>
                </div>
                <div class="w-full bg-gray-200 rounded-full h-2 mb-6">
                    <div id="progressBar" class="bg-purple-600 h-2 rounded-full transition-all duration-300" style="width:0%"></div>
                </div>
                <div id="questionContainer" class="space-y-6"></div>
                <button id="submitSurvey" class="hidden w-full mt-6 py-3 rounded-lg text-white font-semibold bg-green-600 hover:bg-green-700 transition-colors">
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
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="text-lg font-semibold">Anket Sonuçları</h3>
                        <button onclick="showPDFReport()" class="px-4 py-2 bg-red-600 text-white rounded-lg hover:bg-red-700">
                            📄 PDF Göster
                        </button>
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

        // JSONBin.io konfigürasyonu - Düzeltilmiş API bağlantısı
        const JSONBIN_CONFIG = {
            apiKey: '$2a$10$Vre/Nl1Aa1vrK2xY1NHYguabG45SOU1sMt3dnh.UJYpdBoQSdnz1.',
            binId: '68ce67cdae596e708ff4d390', // Sabit merkezi ID
            accessKey: '$2a$10$SCDSdHz/rW/Z3Q6EWaB68uSJR2GAhE3pjG/i3.gJEhKsviO.yl6DC', // X-Access-Key
            baseUrl: 'https://api.jsonbin.io/v3',
            maxRetries: 3,
            retryDelay: 1000
        };

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
            
            if (!disclaimerAccepted) {
                showModal('⚠️ Uyarı', 'Devam etmek için sorumluluk reddi beyanını kabul etmelisiniz.');
                return;
            }
            
            if (!companyName || !selectedJobType) {
                showModal('⚠️ Eksik Bilgi', 'Lütfen şirket adını girin ve iş türünüzü seçin.');
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
                    surveyName: "Çalışan Memnuniyet Anketi - Sürüm 12",
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
                    totalResponses: 0
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
                    systemData.surveyData.responses = [];
                }
                systemData.surveyData.responses.push(surveyResponse);
                
                // İstatistikleri güncelle
                if (!systemData.surveyData.statistics) {
                    systemData.surveyData.statistics = {
                        totalResponses: 0,
                        averageScore: 0,
                        lastUpdated: new Date().toISOString()
                    };
                }
                
                systemData.surveyData.statistics.totalResponses = systemData.surveyData.responses.length;
                systemData.surveyData.statistics.averageScore = (
                    systemData.surveyData.responses.reduce((sum, r) => sum + parseFloat(r.averageScore), 0) / 
                    systemData.surveyData.responses.length
                ).toFixed(2);
                systemData.surveyData.statistics.lastUpdated = new Date().toISOString();
                
                // Şirket istatistiklerini güncelle
                if (companyResult && systemData.surveyData.companies[companyResult.key]) {
                    systemData.surveyData.companies[companyResult.key].totalResponses = 
                        systemData.surveyData.responses.filter(r => 
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
                showModal('❌ Hata', 'Giriş sırasında bir hata oluştu. Lütfen tekrar deneyin.');
                console.error('Giriş hatası:', error);
            }
        }

        function loadCompanyDashboard() {
            if (!loggedInCompany || !systemData.surveyData) return;
            
            document.getElementById('companyNameDisplay').textContent = loggedInCompany.name;
            
            // Şirket anketlerini filtrele
            const companySurveys = systemData.surveyData.responses.filter(s => 
                s.companyName.toLowerCase() === loggedInCompany.name.toLowerCase()
            );
            
            // İstatistikleri güncelle
            document.getElementById('totalParticipants').textContent = companySurveys.length;
            
            if (companySurveys.length > 0) {
                // Doğru ortalama hesaplama
                let totalScore = 0;
                let totalAnswers = 0;
                companySurveys.forEach(s => {
                    totalScore += s.totalScore;
                    totalAnswers += s.answers.length;
                });
                const avgScore = totalAnswers > 0 ? (totalScore / totalAnswers).toFixed(1) : '0.0';
                document.getElementById('averageScore').textContent = avgScore;
                
                // Memnuniyet yüzdesi hesaplama - 4 ve 5 puan verenlerin oranı
                let highSatisfactionAnswers = 0;
                companySurveys.forEach(s => {
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
            
            generateSimpleReport(companySurveys);
            generateCharts(companySurveys);
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
            
            tbody.innerHTML = Object.entries(companies).map(([companyKey, company]) => {
                const companySurveys = responses.filter(s => 
                    s.companyName.toLowerCase() === company.name.toLowerCase()
                );
                
                return `
                    <tr class="border-b hover:bg-gray-50">
                        <td class="px-4 py-2 font-medium">${company.name}</td>
                        <td class="px-4 py-2">
                            <code class="bg-gray-100 px-2 py-1 rounded text-sm">${company.password}</code>
                        </td>
                        <td class="px-4 py-2">${companySurveys.length}</td>
                        <td class="px-4 py-2">
                            <span class="px-2 py-1 rounded-full text-xs bg-green-100 text-green-800">
                                🟢 Aktif
                            </span>
                        </td>
                        <td class="px-4 py-2">
                            <button onclick="showAdminCompanyReport('${company.name}')" class="text-green-600 hover:text-green-800 mr-2">📊 Rapor</button>
                            <button onclick="resetCompanyPassword('${companyKey}')" class="text-orange-600 hover:text-orange-800">🔄 Şifre</button>
                        </td>
                    </tr>
                `;
            }).join('');
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

        function showPDFReport() {
            if (!loggedInCompany || !systemData.surveyData) return;
            
            const companySurveys = systemData.surveyData.responses.filter(s => 
                s.companyName.toLowerCase() === loggedInCompany.name.toLowerCase()
            );
            
            const pdfContent = generatePDFContent(companySurveys);
            
            // Yeni pencerede PDF görünümü aç
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

        function generatePDFContent(surveys) {
            const companyName = loggedInCompany.name;
            const totalParticipants = surveys.length;
            
            // Bilimsel Memnuniyet Hesaplama Modeli
            // Minimum: 50 soru x 1 puan = 50, Maksimum: 50 soru x 5 puan = 250
            let totalScore = 0;
            let totalAnswers = 0;
            const satisfactionByPosition = {};
            
            surveys.forEach(s => {
                totalScore += s.totalScore;
                totalAnswers += s.answers.length;
                
                // Pozisyon bazlı memnuniyet hesaplama
                const positionScore = s.totalScore;
                const satisfactionPercent = Math.round(((positionScore - 50) / (250 - 50)) * 100);
                
                if (!satisfactionByPosition[s.jobType]) {
                    satisfactionByPosition[s.jobType] = { total: 0, count: 0, scores: [] };
                }
                satisfactionByPosition[s.jobType].total += satisfactionPercent;
                satisfactionByPosition[s.jobType].count++;
                satisfactionByPosition[s.jobType].scores.push(satisfactionPercent);
            });
            
            const avgScore = totalAnswers > 0 ? (totalScore / totalAnswers).toFixed(1) : '0.0';
            
            // Genel memnuniyet yüzdesi hesaplama
            const overallSatisfactionPercent = totalParticipants > 0 ? 
                Math.round(((totalScore - (totalParticipants * 50)) / (totalParticipants * 200)) * 100) : 0;
            
            // Memnuniyet oranı - 4 ve 5 puan verenlerin oranı
            let highSatisfactionCount = 0;
            surveys.forEach(s => {
                s.answers.forEach(answer => {
                    if (answer.score >= 4) highSatisfactionCount++;
                });
            });
            const satisfactionRate = totalAnswers > 0 ? Math.round((highSatisfactionCount / totalAnswers) * 100) : 0;
            
            // Kategori Bazlı Analiz - Her pozisyon için 50 soru 5 kategoriye bölünmüş
            const categoryAnalysis = analyzeCategoryPerformance(surveys);
            
            // Pozisyon dağılımı
            const positionData = {};
            surveys.forEach(s => {
                positionData[s.jobType] = (positionData[s.jobType] || 0) + 1;
            });
            
            // Memnuniyet dağılımı - her cevaba göre hesapla
            const satisfactionCounts = [0, 0, 0]; // Düşük, Orta, Yüksek
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
                    <title>${companyName} - Anket Raporu</title>
                    <style>
                        body { font-family: Arial, sans-serif; margin: 20px; line-height: 1.6; }
                        .header { text-align: center; border-bottom: 2px solid #333; padding-bottom: 20px; margin-bottom: 30px; }
                        .stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: 20px; margin-bottom: 30px; }
                        .stat-box { background: #f5f5f5; padding: 15px; border-radius: 8px; text-align: center; }
                        .stat-number { font-size: 2em; font-weight: bold; color: #333; }
                        .section { margin-bottom: 30px; }
                        .section h3 { color: #333; border-bottom: 1px solid #ddd; padding-bottom: 10px; }
                        table { width: 100%; border-collapse: collapse; margin-top: 10px; }
                        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
                        th { background-color: #f2f2f2; }
                        .footer { margin-top: 50px; text-align: center; font-size: 0.9em; color: #666; }
                        @media print { body { margin: 0; } }
                    </style>
                </head>
                <body onload="window.print()">
                    <div class="header">
                        <h1>📊 ${companyName}</h1>
                        <h2>Çalışan Memnuniyet Anketi Raporu</h2>
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
                            <tr><th>Pozisyon</th><th>Katılımcı Sayısı</th><th>Yüzde</th></tr>
                            ${Object.entries(positionData).map(([pos, count]) => 
                                `<tr><td>${pos}</td><td>${count}</td><td>${Math.round((count/totalParticipants)*100)}%</td></tr>`
                            ).join('')}
                        </table>
                    </div>
                    
                    <div class="section">
                        <h3>📈 Memnuniyet Seviyeleri</h3>
                        <table>
                            <tr><th>Seviye</th><th>Cevap Sayısı</th><th>Yüzde</th></tr>
                            <tr><td>Düşük (1-2 Puan)</td><td>${satisfactionCounts[0]}</td><td>${totalAnswers > 0 ? Math.round((satisfactionCounts[0]/totalAnswers)*100) : 0}%</td></tr>
                            <tr><td>Orta (3 Puan)</td><td>${satisfactionCounts[1]}</td><td>${totalAnswers > 0 ? Math.round((satisfactionCounts[1]/totalAnswers)*100) : 0}%</td></tr>
                            <tr><td>Yüksek (4-5 Puan)</td><td>${satisfactionCounts[2]}</td><td>${totalAnswers > 0 ? Math.round((satisfactionCounts[2]/totalAnswers)*100) : 0}%</td></tr>
                        </table>
                    </div>
                    
                    <div class="section">
                        <h3>📋 Özet ve Öneriler</h3>
                        <p><strong>Genel Değerlendirme:</strong> ${totalParticipants} çalışan anketi tamamlamıştır. Ortalama memnuniyet skoru ${avgScore}/5.0 olarak hesaplanmıştır.</p>
                        
                        <p><strong>Bilimsel Memnuniyet Analizi:</strong> Genel memnuniyet seviyesi %${overallSatisfactionPercent} olarak hesaplanmıştır.</p>
                        
                        ${overallSatisfactionPercent >= 76 ? 
                            '<div style="background: #dcfce7; padding: 15px; border-radius: 8px; border-left: 4px solid #16a34a; margin: 15px 0;"><p><strong>🎯 Senaryo 3: Yüksek Memnuniyet (%76-100)</strong></p><p><strong>Durum Değerlendirmesi:</strong> Şirketiniz çalışan bağlılığı ve memnuniyeti konusunda çok başarılı. Bu başarıyı sürdürmek kritik önemde.</p><p><strong>Öneriler:</strong></p><ul><li><strong>Sürekli Takip:</strong> Düzenli "Nabız Anketleri" ile memnuniyet seviyesi takip edilmeli</li><li><strong>Gelişim Alanları:</strong> En yüksek puan alan alanlarda bile çalışanlardan fikir toplanmalı</li><li><strong>Başarıyı Kutlama:</strong> Yüksek memnuniyet için emeği geçenler ödüllendirilmeli</li></ul></div>' :
                            overallSatisfactionPercent >= 51 ?
                            '<div style="background: #fef3c7; padding: 15px; border-radius: 8px; border-left: 4px solid #f59e0b; margin: 15px 0;"><p><strong>⚠️ Senaryo 2: Orta Memnuniyet (%51-75)</strong></p><p><strong>Durum Değerlendirmesi:</strong> İyi bir temele sahip ancak "durgunluk sendromu" yaşanıyor. Temel ihtiyaçlar karşılanmış ancak uzun vadeli gelişim konusunda belirsizlikler var.</p><p><strong>Öneriler:</strong></p><ul><li><strong>Gelişim Odaklı Yaklaşım:</strong> Kariyer gelişim programları (mentorluk, eğitim) oluşturulmalı</li><li><strong>Dijital Katılım:</strong> Dijital dönüşümle ilgili çalışanlardan fikir alınmalı</li><li><strong>Yan Hak İyileştirmesi:</strong> Yemek kalitesi, menü çeşitliliği iyileştirilmeli</li></ul></div>' :
                            '<div style="background: #fee2e2; padding: 15px; border-radius: 8px; border-left: 4px solid #dc2626; margin: 15px 0;"><p><strong>🚨 Senaryo 1: Düşük Memnuniyet (%0-50)</strong></p><p><strong>Durum Değerlendirmesi:</strong> Şirket temelden sarsılmış durumda. Tüm çalışan gruplarında ciddi sorunlar var ve acil müdahale gerekli.</p><p><strong>Öneriler:</strong></p><ul><li><strong>Acil Eylem Planı:</strong> Maaş ve sosyal haklar gibi temel konular gözden geçirilmeli</li><li><strong>İyileştirme Çalışmaları:</strong> Çalışma koşulları (havalandırma, aydınlatma, iş güvenliği) iyileştirilmeli</li><li><strong>İletişim:</strong> Yönetim şeffaf toplantılarla vizyon ve adımları paylaşmalı</li></ul></div>'
                        }
                        
                        <div style="background: #f3f4f6; padding: 15px; border-radius: 8px; margin: 15px 0;">
                            <p><strong>📊 Pozisyon Bazlı Analiz:</strong></p>
                            ${Object.entries(satisfactionByPosition).map(([position, data]) => {
                                const avgPercent = Math.round(data.total / data.count);
                                return `<p><strong>${position}:</strong> %${avgPercent} memnuniyet (${data.count} katılımcı)</p>`;
                            }).join('')}
                            
                            ${Object.keys(satisfactionByPosition).length > 1 ? 
                                (() => {
                                    const positions = Object.entries(satisfactionByPosition);
                                    const maxDiff = Math.max(...positions.map(([,data]) => Math.round(data.total / data.count))) - 
                                                   Math.min(...positions.map(([,data]) => Math.round(data.total / data.count)));
                                    
                                    if (maxDiff > 30) {
                                        return '<div style="background: #fef2f2; padding: 10px; border-radius: 6px; margin-top: 10px;"><p><strong>⚠️ Senaryo 4: Gruplar Arası Ayrışma Tespit Edildi</strong></p><p>Pozisyonlar arasında %' + maxDiff + ' fark var. Bu ciddi bir kültür çatışması işareti olabilir.</p><p><strong>Öneriler:</strong> Düşük puan alan gruplara öncelik verilmeli, gruplar arası iletişim köprüleri kurulmalı.</p></div>';
                                    }
                                    return '';
                                })() : ''
                            }
                        </div>
                        
                        <div style="background: #e0f2fe; padding: 15px; border-radius: 8px; margin: 15px 0; border-left: 4px solid #0277bd;">
                            <h4 style="color: #01579b; margin-bottom: 10px; font-size: 16px;"><strong>📋 Kategori Bazlı Detay Analiz</strong></h4>
                            <table style="width: 100%; border-collapse: collapse; margin-top: 10px; font-size: 13px;">
                                <thead>
                                    <tr style="background-color: #b3e5fc;">
                                        <th style="border: 1px solid #0277bd; padding: 8px; text-align: left; color: #01579b;">Kategori</th>
                                        <th style="border: 1px solid #0277bd; padding: 8px; text-align: center; color: #01579b;">Puan</th>
                                        <th style="border: 1px solid #0277bd; padding: 8px; text-align: center; color: #01579b;">Durum</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    ${Object.entries(categoryAnalysis).map(([key, category]) => {
                                        const statusColor = category.status === 'Yüksek' ? '#2e7d32' : 
                                                          category.status === 'Orta' ? '#f57c00' : '#d32f2f';
                                        const bgColor = category.status === 'Yüksek' ? '#e8f5e8' : 
                                                       category.status === 'Orta' ? '#fff3e0' : '#ffebee';
                                        return `
                                            <tr style="background-color: ${bgColor};">
                                                <td style="border: 1px solid #0277bd; padding: 8px; font-weight: 500;">${category.name}</td>
                                                <td style="border: 1px solid #0277bd; padding: 8px; text-align: center; font-weight: bold; color: ${statusColor};">${category.score}/5.0</td>
                                                <td style="border: 1px solid #0277bd; padding: 8px; text-align: center; font-weight: bold; color: ${statusColor};">${category.status}</td>
                                            </tr>
                                            <tr style="background-color: ${bgColor};">
                                                <td colspan="3" style="border: 1px solid #0277bd; padding: 8px; font-size: 12px; color: #424242; line-height: 1.4;">
                                                    <strong>Yorum:</strong> ${category.comment}
                                                </td>
                                            </tr>
                                        `;
                                    }).join('')}
                                </tbody>
                            </table>
                        </div>
                        <!-- İşletme için özel strateji ve aksiyon planı bölümü -->
                        <div style="background: #fff; border-radius: 8px; margin: 20px 0; padding: 24px 18px; border-left: 5px solid #6366f1; box-shadow: 0 2px 8px #e0e7ff;">
                            <h3 style="font-size: 1.4em; font-weight: bold; color: #3730a3; margin-bottom: 12px;">2. Stratejik Öneriler</h3>
                            <p style="color: #374151; margin-bottom: 12px;">Anket sonuçları, şirket genelinde güçlü ve zayıf alanları net bir şekilde ortaya koymaktadır. Özellikle <b>Maaş ve Yan Haklar</b> ile <b>Kariyer Fırsatları</b> kategorilerindeki düşük puanlar, çalışan memnuniyetsizliğinin temel kaynağına işaret etmektedir. Öte yandan, <b>Dijital Dönüşüm</b> ve <b>Çalışma Ortamı</b> gibi alanlardaki yüksek puanlar, kurumun doğru yatırımlar yaptığını ve bu başarıları bir marka değeri olarak kullanabileceğini göstermektedir.</p>
                            <ul style="margin-left: 18px; color: #374151;">
                                <li><b style="color:#dc2626;">Acil Müdahale Gerektiren Alanlar:</b> Düşük puan alan kategorilerde (Maaş ve Yan Haklar, Kariyer Fırsatları) hızlı ve şeffaf iyileştirme adımları atılmalıdır. Bu durum, çalışan devir hızını artırma ve yetenekli personeli kaybetme riskini taşımaktadır.</li>
                                <li><b style="color:#16a34a;">Güçlü Yönlerin Korunması:</b> Yüksek puan alan kategorilerdeki başarılar (Dijital Dönüşüm, Çalışma Ortamı) sürdürülmelidir. Bu alanlardaki pozitif algı, yeni yetenekleri çekmek için kullanılabilir ve iç motivasyonu pekiştirir.</li>
                                <li><b style="color:#f59e0b;">İletişimin Güçlendirilmesi:</b> Yönetim ve İletişim kategorisindeki orta düzey puanlar, daha proaktif bir yaklaşım gerektirmektedir. Düzenli "nabız anketleri" ve açık iletişim toplantıları ile çalışanların kendilerini daha fazla dinlenmiş hissetmeleri sağlanmalıdır.</li>
                            </ul>
                            <h3 style="font-size: 1.4em; font-weight: bold; color: #3730a3; margin: 24px 0 12px 0;">3. Aksiyon Planı</h3>
                            <div style="display: flex; flex-direction: column; gap: 16px;">
                                <div style="background: #eef2ff; border-left: 4px solid #6366f1; border-radius: 6px; padding: 14px 18px;">
                                    <b style="color:#3730a3;">1. Ücret ve Kariyer Gelişim Planı Hazırlığı</b>
                                    <p style="margin: 6px 0 0 0; color: #374151;">İnsan Kaynakları ve yönetim ekibi, piyasa araştırması yaparak rekabetçi bir maaş düzenlemesi ve her çalışan için net bir kariyer gelişim yolu belirleyecektir. Bu plan, önümüzdeki 3 ay içinde çalışanlarla paylaşılacaktır.</p>
                                </div>
                                <div style="background: #eef2ff; border-left: 4px solid #6366f1; border-radius: 6px; padding: 14px 18px;">
                                    <b style="color:#3730a3;">2. Nabız Anketi Uygulaması</b>
                                    <p style="margin: 6px 0 0 0; color: #374151;">İlk anketin üzerinden 6 ay geçtikten sonra daha kısa ve odaklanmış bir "nabız anketi" uygulanacaktır. Bu anket, yapılan iyileştirmelerin çalışanlar üzerindeki etkisini ölçecek ve anlık geri bildirim sağlayacaktır.</p>
                                </div>
                                <div style="background: #eef2ff; border-left: 4px solid #6366f1; border-radius: 6px; padding: 14px 18px;">
                                    <b style="color:#3730a3;">3. Liderlik Gelişim Programı</b>
                                    <p style="margin: 6px 0 0 0; color: #374151;">Yönetim ve İletişim puanlarını artırmak için tüm yöneticilere liderlik, geri bildirim verme ve iletişim becerileri üzerine kapsamlı bir eğitim programı başlatılacaktır.</p>
                                </div>
                            </div>
                        </div>
                        
                        <div style="background: #f1f8e9; padding: 15px; border-radius: 8px; margin: 15px 0; border-left: 4px solid #689f38;">
                            <h4 style="color: #33691e; margin-bottom: 10px;"><strong>🎯 Genel Değerlendirme: "Zıtlıklar Dengesi"</strong></h4>
                            <p style="font-size: 14px; line-height: 1.5; color: #424242;">
                                Bu anket sonuçları, şirketinizin bir <strong>"Zıtlıklar Dengesi"</strong> içinde olduğunu gösteriyor. 
                                Çalışma ortamı ve dijital dönüşüm gibi modern ve ileriye dönük konularda oldukça başarılısınız. 
                                Bu, şirketinizin geleceğe yatırım yaptığının ve yeniliklere açık olduğunun somut bir kanıtıdır.
                            </p>
                            <p style="font-size: 14px; line-height: 1.5; color: #424242; margin-top: 10px;">
                                Ancak, temel ve günlük yaşamı etkileyen alanlardaki düşük puanlar ciddi bir alarm işaretidir. 
                                Bu durum, çalışanların "iyi bir yer ama temel beklentilerim karşılanmıyor" şeklinde düşünmesine neden olabilir. 
                                Bu alandaki memnuniyetsizlik, diğer olumlu faktörleri gölgeleyebilir ve çalışan devir oranını artırabilir.
                            </p>
                        </div>
                        
                        <p><strong>Stratejik Öneriler ve Aksiyon Planı:</strong></p>
                        <ul>
                            <li><strong>Veri Odaklı Analiz:</strong> Düşük performans gösteren kategorilerde kök neden analizi yapılması ve SWOT analizi ile güçlü/zayıf yönlerin belirlenmesi</li>
                            <li><strong>Çalışan Deneyimi Optimizasyonu:</strong> Employee Journey Mapping ile kritik dokunma noktalarının iyileştirilmesi ve personalize çözümlerin geliştirilmesi</li>
                            <li><strong>Sürekli İyileştirme Döngüsü:</strong> Quarterly anket tekrarları, benchmark analizi ve KPI takibi ile sürekli gelişim sağlanması</li>
                            <li><strong>Best Practice Transferi:</strong> Yüksek performans gösteren departmanların başarı faktörlerinin organizasyon geneline yaygınlaştırılması</li>
                            <li><strong>Liderlik Gelişimi:</strong> Yönetici kadrosuna yönelik coaching programları ve çalışan bağlılığı eğitimlerinin planlanması</li>
                            <li><strong>İnovasyon ve Dijitalleşme:</strong> Çalışan önerilerinin değerlendirildiği inovasyon platformları ve dijital dönüşüm projelerinin hayata geçirilmesi</li>
                        </ul>
                    </div>
                    
                    <div class="footer">
                        <p>Bu rapor Akça Pro X - Kurumsal Anket ve Raporlama Sistemi tarafından oluşturulmuştur.</p>
                        <p>Rapor ID: ${Date.now()} | Oluşturulma: ${new Date().toLocaleString('tr-TR')}</p>
                    </div>
                </body>
                </html>
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
