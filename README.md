<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Akça Pro X - Kurum Değerlendirme Anketi</title>
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
            border: 3px solid #6366f1 !important;
            background-color: #6366f1 !important;
            color: white !important;
            font-weight: bold !important;
            transform: scale(1.05) !important;
            box-shadow: 0 4px 8px rgba(99, 102, 241, 0.3) !important;
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
    <nav class="gradient-bg text-white p-3 shadow-lg">
        <div class="max-w-4xl mx-auto flex justify-between items-center">
            <div class="flex items-center gap-2">
                <div onclick="showModule('admin')" class="w-3 h-3 cursor-pointer opacity-15 hover:opacity-50 transition-opacity" title="">
                    <div class="w-3 h-3 rounded-full border border-white/30 flex items-center justify-center animate-spin" style="animation-duration: 12s;">
                        <div class="w-1 h-1 bg-white/40 rounded-full"></div>
                    </div>
                </div>
    
                <div>
                    <h1 class="text-lg font-bold">Akça Pro X</h1>
                    <p class="text-xs opacity-90">Kurum Değerlendirme Anketi</p>
                </div>
            </div>
            
            <div class="flex gap-2">
                <button onclick="showModule('survey')" class="px-3 py-1 bg-white/20 rounded text-sm hover:bg-white/30 transition-colors">📊 Anket</button>
                <button onclick="showModule('company')" class="px-3 py-1 bg-white/20 rounded text-sm hover:bg-white/30 transition-colors">🏢 Kurum Portalı</button>
            </div>
        </div>
    </nav>

    <div id="surveyModule" class="max-w-4xl mx-auto p-4">
        
        <div class="bg-white shadow-xl rounded-xl max-w-xl mx-auto p-6">
            <div class="text-center mb-6">
                <h2 class="text-xl font-bold text-gray-800 mb-1">Kurum Değerlendirme Anketi</h2>
                <p class="text-gray-600 mb-2 text-sm">Görüşleriniz bizim için değerli</p>
                <span class="bg-green-100 text-green-800 text-xs font-semibold px-2 py-1 rounded-full">v3.0.0 - JSONBin.io Entegre</span>
          
            </div>

            <div id="disclaimerSection" class="mb-4">
                <div class="bg-yellow-50 border border-yellow-300 rounded p-3 mb-3">
                    <h3 class="font-semibold text-yellow-800 mb-2 text-sm">⚠️ Veri Koruma Beyanı</h3>
                    
                    <div class="text-xs text-yellow-700 space-y-1">
                        <p>• Verileriniz JSONBin.io güvenli sisteminde saklanır ve üçüncü taraflarla paylaşılmaz.</p>
                        <p>• Anket sonuçları sadece kurum yetkilileri tarafından görüntülenebilir.</p>
                        <p>• Sistem güvenliği JSONBin.io sorumluluğundadır.</p>
     
                        <p>• Hack, veri ihlali vb. güvenlik olaylarından kaynaklanan bilgi erişimlerinin sorumluluğu Akça Pro X'e ait değildir.</p>
                    </div>
                </div>
                <label class="flex items-center space-x-2 cursor-pointer">
                    <input type="checkbox" id="acceptDisclaimer" class="w-4 h-4 text-purple-600">
         
                    <span class="text-xs font-medium">Veri koruma beyanını kabul ediyorum</span>
                </label>
            </div>

            <div id="companyInfoSection">
                <h3 class="text-base font-semibold text-gray-700 mb-3">Kurum ve Kişisel Bilgiler</h3>
    
                
                <div class="mb-3">
                    <input type="text" id="companyName" placeholder="Kurum adınızı girin (Okul, Üniversite vb.)" 
                           class="w-full border-2 border-gray-300 rounded px-3 py-2 text-sm focus:ring-2 focus:ring-purple-500 focus:border-purple-500">
      
                </div>
                
                <div class="mb-3">
                    <p class="text-xs text-gray-600 mb-2">Rolünüzü seçin:</p>
                    <div class="grid grid-cols-3 gap-2">
          
                        <button type="button" onclick="selectJobType('Öğrenci')" id="studentBtn" 
                                class="job-btn py-3 px-2 text-xs rounded border-2 border-blue-300 hover:border-blue-500 hover:bg-blue-50 transition-all duration-200 cursor-pointer font-medium bg-white text-center focus:outline-none focus:ring-2 focus:ring-blue-400">
                            <div class="text-lg mb-1">🎓</div>
   
                            <div>Öğrenci</div>
                        </button>
                        <button type="button" onclick="selectJobType('Öğretmen')" id="teacherBtn" 
                       
                            class="job-btn py-3 px-2 text-xs rounded border-2 border-green-300 hover:border-green-500 hover:bg-green-50 transition-all duration-200 cursor-pointer font-medium bg-white text-center focus:outline-none focus:ring-2 focus:ring-green-400">
                            <div class="text-lg mb-1">👨‍🏫</div>
                            <div>Öğretmen</div>
                
                        </button>
                        <button type="button" onclick="selectJobType('Veli/Ebeveyn')" id="parentBtn" 
                                class="job-btn py-3 px-2 text-xs rounded border-2 border-purple-300 hover:border-purple-500 hover:bg-purple-50 transition-all duration-200 cursor-pointer font-medium bg-white text-center focus:outline-none focus:ring-2 focus:ring-purple-400">
               
                            <div class="text-lg mb-1">👨‍👩‍👧‍👦</div>
                            <div>Veli/Ebeveyn</div>
                        </button>
                    </div>
             
                </div>
                
                <div id="selectedJobDisplay" class="text-center text-sm text-gray-600 mb-3 min-h-[20px]">
                    </div>
                
  
                <div class="grid grid-cols-2 gap-2 mb-4">
                    <input type="text" id="firstName" placeholder="Adınız (İsteğe bağlı)" 
                           class="border-2 border-gray-300 rounded px-3 py-2 text-sm focus:ring-2 focus:ring-purple-500 focus:border-purple-500">
                    <input 
type="text" id="lastName" placeholder="Soyadınız (İsteğe bağlı)" 
                           class="border-2 border-gray-300 rounded px-3 py-2 text-sm focus:ring-2 focus:ring-purple-500 focus:border-purple-500">
                </div>
                
                <button id="startSurvey" class="w-full py-3 rounded text-white font-semibold gradient-bg hover:opacity-90 transition-opacity text-sm">
  
                    📊 Anketi Başlat
                </button>
            </div>

            <div id="surveySection" class="hidden">
                <div class="flex justify-between items-center mb-6">
   
                    <span id="progressText" class="text-gray-600 font-medium">Anket İlerlemesi 0/50 Yanıtlandı</span>
                    <span id="timeElapsed" class="text-sm text-gray-500">Süre: 00:00</span>
                </div>
                <div class="w-full bg-gray-200 rounded-full h-3 mb-8">
                
                    <div id="progressBar" class="bg-purple-600 h-3 rounded-full transition-all duration-300" style="width:0%"></div>
                </div>
                <div id="questionContainer" class="space-y-6"></div>
                <button id="submitSurvey" class="hidden w-full mt-8 py-4 rounded-lg text-white font-semibold bg-green-600 hover:bg-green-700 transition-colors text-lg">
                    ✅ Anketi Tamamla
     
                </button>
            </div>
        </div>
    </div>

    <div id="companyModule" class="max-w-4xl mx-auto p-4 hidden">
        <div class="bg-white shadow-xl rounded-xl max-w-4xl mx-auto p-6">
            <div id="companyLogin" class="max-w-md mx-auto">
                <h2 class="text-3xl font-bold text-center 
mb-8">🏫 Kurum Portalı Girişi</h2>
                <div class="space-y-6">
                    <input type="text" id="companyLoginName" placeholder="Okul/Kurum Adı" 
                           class="w-full border-2 border-gray-300 rounded-lg px-4 py-4 text-base focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                   
                    <input type="password" id="companyPassword" placeholder="12 Karakterlik Şifre" 
                           class="w-full border-2 border-gray-300 rounded-lg px-4 py-4 text-base focus:ring-2 focus:ring-blue-500 focus:border-blue-500" autocomplete="off">
                    <button onclick="loginCompany()" class="w-full py-4 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors text-lg font-semibold">
                        🔐 Giriş 
Yap
                    </button>
                </div>
                <div class="mt-6 p-4 bg-blue-50 rounded-lg text-sm text-blue-700">
                    <p><strong>Not:</strong> Okul/kurum şifrenizi yöneticinizden alabilirsiniz.</p>
                </div>
  
            </div>

            <div id="companyDashboard" class="hidden">
                <div class="flex justify-between items-center mb-8">
                    <div>
                        <h2 class="text-3xl 
font-bold">Okul/Kurum Raporları</h2>
                        <p class="text-gray-600 text-lg" id="companyNameDisplay"></p>
                    </div>
                    <button onclick="logoutCompany()" class="px-6 py-3 bg-red-600 text-white rounded-lg hover:bg-red-700 font-semibold">
                        🚪 Çıkış
         
                    </button>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                    <div class="bg-gradient-to-r from-blue-500 to-blue-600 text-white p-6 rounded-lg">
                        <h3 class="text-lg 
font-semibold mb-2">Toplam Katılımcı</h3>
                        <p class="text-4xl font-bold" id="totalParticipants">0</p>
                    </div>
                    <div class="bg-gradient-to-r from-green-500 to-green-600 text-white p-6 rounded-lg">
                        <h3 
class="text-lg font-semibold mb-2">Ortalama Puan</h3>
                        <p class="text-4xl font-bold" id="averageScore">0.0</p>
                    </div>
                    <div class="bg-gradient-to-r from-purple-500 to-purple-600 text-white p-6 rounded-lg">
                        
<h3 class="text-lg font-semibold mb-2">Değerlendirme Oranı</h3>
                        <p class="text-4xl font-bold" id="satisfactionRate">0%</p>
                    </div>
                </div>

                <div class="bg-white border rounded-lg p-6">
             
                    <div class="flex justify-between items-center mb-6">
                        <h3 class="text-xl font-semibold">Anket Sonuçları</h3>
                        <button onclick="showPDFReport()" class="px-6 py-3 bg-red-600 text-white rounded-lg hover:bg-red-700 font-semibold">
                            📄 PDF 
Göster
                        </button>
                    </div>
                    
                    <div class="grid grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-3 text-sm">📊 Pozisyon</h4>
                            
                            <div style="height: 150px; position: relative;">
                                <canvas id="positionChart"></canvas>
                            </div>
                        </div>
           
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-3 text-sm">📈 Değerlendirme</h4>
                            <div style="height: 150px;
position: relative;">
                                <canvas id="satisfactionChart"></canvas>
                            </div>
                        </div>
              
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-3 text-sm">⏰ Süre Dağılımı</h4>
                            <div style="height: 150px;
position: relative;">
                                <canvas id="timeChart"></canvas>
                            </div>
                        </div>
              
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-3 text-sm">🎯 Puan Dağılımı</h4>
                            <div style="height: 150px;
position: relative;">
                                <canvas id="trendChart"></canvas>
                            </div>
                        </div>
              
                    </div>
                    
                    <div class="bg-white border rounded-lg p-4 mb-6">
                        <div 
class="flex justify-between items-center mb-4">
                            <h4 class="font-semibold text-gray-800">👥 Katılımcı Detayları</h4>
                            <button onclick="toggleParticipantDetails()" id="toggleParticipantsBtn" class="px-4 py-2 bg-blue-600 text-white text-sm rounded hover:bg-blue-700">
                            
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
                                 
                                            <th class="px-3 py-2 text-center">Değerlendirme</th>
                                            <th class="px-3 py-2 text-center">Tarih</th>
                                       
                                        </tr>
                                    </thead>
                                    <tbody id="participantTableBody">
                          
                                        </tbody>
                                </table>
             
                            </div>
                        </div>
                    </div>
                    
                    <div 
id="detailedReport" class="space-y-4"></div>
                </div>
            </div>
        </div>
    </div>

    <div id="adminModule" class="max-w-4xl mx-auto p-4 hidden">
        <div class="bg-white shadow-xl rounded-xl max-w-4xl mx-auto p-6">
            <div id="adminLogin" class="max-w-md mx-auto">
              
                <h2 class="text-3xl font-bold text-center mb-8">⚙️ Yönetici Portalı</h2>
                <div class="space-y-6">
                    <input type="password" id="adminPassword" placeholder="Yönetici Şifresi" 
                           class="w-full border-2 border-gray-300 rounded-lg px-4 py-4 text-base focus:ring-2 focus:ring-red-500 focus:border-red-500" autocomplete="off">
             
                    <button onclick="loginAdmin()" class="w-full py-4 bg-red-600 text-white rounded-lg hover:bg-red-700 transition-colors text-lg font-semibold">
                        🔐 Yönetici Girişi
                    </button>
                </div>
            </div>

         
            <div id="adminDashboard" class="hidden">
                <div class="flex justify-between items-center mb-8">
                    <h2 class="text-3xl font-bold">Sistem Yönetimi</h2>
                    <button onclick="logoutAdmin()" class="px-6 py-3 bg-red-600 text-white rounded-lg hover:bg-red-700 font-semibold">
                        
🚪 Çıkış
                    </button>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-4 gap-6 mb-8">
                    <div class="bg-blue-100 p-6 rounded-lg text-center">
                  
                        <h3 class="font-semibold text-blue-800 mb-2">Toplam Okul/Kurum</h3>
                        <p class="text-3xl font-bold text-blue-600" id="totalCompanies">0</p>
                    </div>
                    <div class="bg-green-100 p-6 rounded-lg text-center">
                  
                        <h3 class="font-semibold text-green-800 mb-2">Aktif Anketler</h3>
                        <p class="text-3xl font-bold text-green-600" id="activeSurveys">0</p>
                    </div>
                    <div class="bg-yellow-100 p-6 rounded-lg text-center">
                  
                        <h3 class="font-semibold text-yellow-800 mb-2">Toplam Katılımcı</h3>
                        <p class="text-3xl font-bold text-yellow-600" id="totalUsers">0</p>
                    </div>
                    <div class="bg-purple-100 p-6 rounded-lg text-center">
                  
                        <h3 class="font-semibold text-purple-800 mb-2">Sistem Durumu</h3>
                        <p class="text-sm font-bold text-purple-600">🟢 Aktif</p>
                    </div>
                </div>

                <div class="bg-white border rounded-lg p-6">
      
                    <h3 class="text-xl font-semibold mb-6">Okul/Kurum Listesi ve Yönetimi</h3>
                    <div class="overflow-x-auto">
                        <table class="w-full table-auto">
                            <thead>
     
                                <tr class="bg-gray-50">
                                    <th class="px-4 py-3 text-left">Okul/Kurum Adı</th>
                                
                                    <th class="px-4 py-3 text-left">Şifre</th>
                                    <th class="px-4 py-3 text-left">Katılımcı</th>
                                    <th class="px-4 py-3 text-left">Durum</th>
               
                                    <th class="px-4 py-3 text-left">İşlemler</th>
                                </tr>
                            </thead>
                
                            <tbody id="companyList">
                                </tbody>
                      
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>

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
        // JSONBin.io konfigürasyonu
        const JSONBIN_CONFIG = {
            apiKey: '$2a$10$SCDSdHz/rW/Z3Q6EWaB68uSJR2GAhE3pjG/i3.gJEhKsviO.yl6DC',
            binId: '68cc142142be1c4f08a35142',
            baseUrl: 'https://api.jsonbin.io/v3',
            maxRetries: 3,
            retryDelay: 1000
        };
        // Soru setleri
        const questions = {
            "Öğrenci": [
                // Okul Ortamı ve Konfor (10 Soru)
                "Okulun derslikleri ve ortak alanları (kantin, kütüphane) temiz ve düzenli 🏫",
                "Okul binasındaki ısınma, havalandırma ve aydınlatma koşulları yeterli 🌡️",
                "Okul kantinindeki yiyecek ve içeceklerin kalitesi ve çeşitliliği iyi 🍎",
                "Okul bahçesi ve spor alanları aktiviteler için güvenli ve yeterli ⚽",
                "Okul tuvaletlerinin hijyeni ve düzeninden memnunum 🚿",
                "Sınıf ortamı, derslere odaklanmamı kolaylaştırıyor 📚",
                "Okulun, öğrencilerin fiziksel ve psikolojik sağlığına önem verdiğini düşünüyorum 💚",
                "Okulun güvenli bir yer olduğuna inanıyorum 🛡️",
                "Okuldaki öğrenci dolapları ve eşya saklama alanları yeterli 🗄️",
                "Okulun sağladığı sosyal olanaklar (etkinlikler, kulüpler) yeterli ve çeşitli 🎭",
            
                // Dersler ve Eğitim Kalitesi (10 Soru)
                "Öğretmenlerim dersleri ilgi çekici ve anlaşılır bir şekilde anlatıyor 👨‍🏫",
                "Öğretmenlerim, zorlandığım konularda bana yeterli desteği sağlıyor 🤝",
                "Okulun müfredatı, gelecekteki akademik hedeflerime uygun 🎯",
                "Sınavlar ve değerlendirmeler, öğrendiklerimi doğru bir şekilde ölçüyor 📝",
                "Okulda yabancı dil öğrenme imkanları yeterli 🌍",
                "Derslerde yaratıcılığımı ve eleştirel düşünme becerilerimi kullanabiliyorum 💡",
                "Ödevler ve projeler, bilgilerimi pekiştirmeme yardımcı oluyor 📋",
                "Öğretmenlerimin bana karşı tutum ve davranışları saygılı 🤗",
                "Okulda öğrendiklerimin gerçek hayatta işime yarayacağına inanıyorum 🌟",
                "Okulda aldığım eğitimin kalitesinden memnunum ⭐",
                
                // Okul Yönetimi ve Güven (10 Soru)
                "Okul yönetiminin, öğrencilerin fikirlerine değer verdiğini düşünüyorum 💭",
                "Okul kuralları, adil ve tüm öğrenciler için eşit uygulanıyor ⚖️",
                "Sorunlarım olduğunda, okul yönetimi veya rehberlik servisine rahatlıkla başvurabiliyorum 📞",
                "Okul yönetiminin kararları açık ve anlaşılır 📢",
                "Okulda zorbalık türlerine karşı etkili önlemler alınıyor 🛡️",
                "Okulun, öğrenciler arasında saygı ve hoşgörüyü teşvik ettiğini düşünüyorum 🤝",
                "Rehberlik servisinden aldığım destekten memnunum 👥",
                "Okuldaki disiplin yönetimi, öğrencilerin gelişimini destekliyor 📈",
                "Okul yönetimine güveniyorum ❤️",
                "Okulun, öğrencilerin sosyal gelişimine katkı sağladığına inanıyorum 🌱",
                
                // Sosyal Gelişim ve Gelecek (10 Soru)
                "Okul, lise veya üniversiteye hazırlanmam için gerekli desteği sağlıyor 🎓",
                "Okuldaki kariyer rehberliği çalışmaları geleceğime yön vermeme yardımcı oluyor 🚀",
                "Okulun mezunlarının başarılı olduğunu ve bana ilham verdiğini düşünüyorum ✨",
                "Okulun, mesleki ilgi alanlarımı keşfetmem için fırsatlar sunduğuna inanıyorum 🔍",
                "Okulun sunduğu eğitim, beni geleceğe hazırlıyor 📅",
                "Okuldaki öğrenci projeleri, ekip çalışması ve liderlik becerilerimi geliştiriyor 👑",
                "Okulun, bilimsel ve sanatsal yarışmalara katılmamızı desteklediğini düşünüyorum 🏆",
                "Okulda aldığım eğitimle gurur duyuyorum 💪",
                "Gelecekte bu okulun, başarılı bir mezunu olmak için doğru yerdeyim 🎯",
                "Okulumun mezuniyetten sonra da bana destek olacağına inanıyorum 🤗",
                
                // Eğitimde Teknoloji ve Yenilenme (10 Soru)
                "Okulumuz, teknolojiyi derslerimize etkili bir şekilde entegre ediyor 💻",
                "Derslerde kullandığımız dijital araçlar (öğrenme platformları, uygulamalar vb.) kullanışlı 📱",
                "Teknolojik yenilikleri öğrenmeye ve derslerimde kullanmaya istekliyim 🎓",
                "Okul, dijital okuryazarlığımı geliştirmek için yeterli kaynak sağlıyor 📖",
                "Okulun, yeni teknolojilere yatırım yaptığını düşünüyorum 💡",
                "Okulun web sitesi ve mobil uygulaması, ders ve okul etkinlikleri hakkında beni bilgilendiriyor 📲",
                "Dijital öğrenme araçlarının, dersleri daha ilgi çekici hale getirdiğini düşünüyorum ⚡",
                "Öğretmenlerim, dijital araçları derslerde etkili bir şekilde kullanıyor 🔧",
                "Online öğrenme platformumuzun teknik altyapısı sağlam 🛠️",
                "Okulun, eğitimde yeniliklere açık bir kurum olduğunu düşünüyorum 🚀"
            ],
            "Öğretmen": [
                // Eğitim Ortamı ve Kaynaklar (10 Soru)
                "Derslerimi işlemek için gerekli olan teknolojik ve fiziki kaynaklar yeterli 💻",
                "Sınıf mevcudu, nitelikli bir eğitim vermem için uygun 👥",
                "Okulun fiziki koşulları (ısıtma, aydınlatma vb.) verimli bir çalışma ortamı sunuyor 🌡️",
                "Öğretmenler odası ve diğer sosyal alanlar yeterince konforlu 🏢",
                "Okulun, öğretmenlerin mesleki gelişimine yönelik yeterli bütçe ayırdığını düşünüyorum 💰",
                "Okulda, öğrencilerin akademik başarısını destekleyecek yeterli kaynak (kütüphane, laboratuvar) var 📚",
                "Okulun, öğretmenlerin fiziksel ve psikolojik sağlığına önem verdiğini düşünüyorum 💚",
                "Okulda, veli görüşmelerini rahatça yapabileceğim uygun ortamlar mevcut 🤝",
                "Okulun genel düzeni ve temizliği yeterli 🧹",
                "Okuldaki ders dışı etkinlikler, öğrencilerin gelişimine katkı sağlıyor 🎭",
                
                // Yönetim ve İletişim (10 Soru)
                "Okul yönetimiyle aramızda açık ve şeffaf bir iletişim var 💬",
                "Okul yönetiminin, öğretmenlerin fikirlerine ve önerilerine değer verdiğini düşünüyorum 💭",
                "Okul yönetimi, mesleki sorunlarımda bana destek oluyor 🤝",
                "Okul yönetimine güveniyorum ❤️",
                "Okulun vizyonu ve misyonu, yaptığım işe anlam katıyor 🌟",
                "Okuldaki idari süreçler (evrak işleri, planlama) verimli bir şekilde yürütülüyor 📋",
                "Okul yönetiminin kararları adil ve eşitlikçi ⚖️",
                "Okulda, diğer öğretmenlerle etkili bir iş birliği içindeyiz 👨‍🏫",
                "Okul yönetimi, başarılı çalışmalarımızı takdir ediyor 👏",
                "Okulun, öğretmenler arasında olumlu bir iş birliği kültürü oluşturduğunu düşünüyorum 🤗",
                
                // Mesleki Gelişim ve Kariyer (10 Soru)
                "Okul, mesleki gelişimim için yeterli eğitim ve seminerler sunuyor 📖",
                "Okuldaki performans değerlendirme sistemi adil ve şeffaf 📊",
                "Öğretmen olarak, okul içinde kariyer basamaklarını görebiliyorum 🚀",
                "Okulun, yeni öğretim yöntemlerini denemem için bana fırsatlar verdiğine inanıyorum 💡",
                "Mesleğimde ilerlemek için gerekli motivasyona sahibim 🔥",
                "Okulun, ulusal ve uluslararası platformlarda gelişimimi desteklediğini düşünüyorum 🌍",
                "Yaptığım işin, okulun başarısına önemli katkı sağladığını hissediyorum 🏆",
                "Okulda aldığım eğitimlerin, öğrencilerimin başarısını artırdığına inanıyorum 📈",
                "Okulun, öğretmenler için esnek ve destekleyici bir çalışma ortamı sunduğunu düşünüyorum ⚖️",
                "Mesleki gelişimim için harcadığım çabanın karşılığını alıyorum 💪",
                
                // Veli İlişkileri ve Geri Bildirim (10 Soru)
                "Velilerle olan iletişim kanalları yeterli ve etkili 📞",
                "Velilerin, okulun faaliyetlerine katılımı yeterli düzeyde 👨‍👩‍👧‍👦",
                "Velilerden gelen geri bildirimler, öğretim yöntemlerimi geliştirmeme yardımcı oluyor 📝",
                "Okul, velilerle olumlu bir iş birliği kültürü oluşturmamıza destek oluyor 🤝",
                "Veli toplantıları ve iletişim günleri verimli geçiyor ⏰",
                "Okul, velilerin eğitim sürecine dahil olması için yeterli fırsatlar sunuyor 🎯",
                "Veli beklentilerinin, okulun eğitim hedefleriyle uyumlu olduğunu düşünüyorum 🎭",
                "Veli sorunları veya şikayetleri, okul yönetimi tarafından adil bir şekilde çözülüyor ⚖️",
                "Veli iletişimimizin, öğrenci başarısını olumlu etkilediğine inanıyorum 📈",
                "Okul, velilere yönelik bilgilendirme çalışmalarını düzenli olarak yapıyor 📢",
                
                // Eğitimde Teknoloji ve Yenilenme (10 Soru)
                "Okulun dijital eğitim stratejisi açık ve anlaşılır 🎯",
                "Uzaktan eğitim platformumuz, dersleri etkili bir şekilde işlememi sağlıyor 💻",
                "Dijital araçların, öğrencilerin öğrenmesini kolaylaştırdığını düşünüyorum ⚡",
                "Okul, dijital becerilerimi geliştirmem için gerekli eğitimleri veriyor 📚",
                "Okul yönetiminin, teknolojik yeniliklere yatırım yaptığını düşünüyorum 💡",
                "Derslerde kullandığım dijital araçların teknik altyapısı sağlam 🛠️",
                "Okulun, geleceğin eğitim trendlerine uyum sağladığına inanıyorum 🚀",
                "Okulun, eğitimde sürekli yenilenmeye açık olduğunu düşünüyorum 🔄",
                "Okulun dijital dönüşüm sürecini başarılı bir şekilde yönettiğine inanıyorum 🎛️",
                "Okulun, yeni eğitim yaklaşımlarını benimsemeye istekli olduğunu düşünüyorum 🌟"
            ],
            "Veli/Ebeveyn": [
                // Eğitim Kalitesi ve Akademik Gelişim (10 Soru)
                "Çocuğumun aldığı eğitimden genel olarak memnunum 📚",
                "Okulun müfredatı, çocuğumun akademik gelişimini destekliyor 📈",
                "Öğretmenler, çocuğumun öğrenme tarzına uygun yöntemler kullanıyor 🎯",
                "Çocuğum, okulda öğrendiklerinin gerçek hayatta işe yarayacağını düşünüyor 🌟",
                "Okulun, öğrencilerinin potansiyelini en üst düzeye çıkarmak için çalıştığına inanıyorum 🚀",
                "Okulun sınav ve değerlendirme sistemi, adil ve şeffaf ⚖️",
                "Okulun, öğrenciler arasında sağlıklı bir rekabet ortamı oluşturduğunu düşünüyorum 🏆",
                "Okuldaki ders dışı kulüp ve etkinliklerin çeşitliliği yeterli 🎭",
                "Okulun, çocuğumun ilgi alanlarını keşfetmesine yardımcı olduğuna inanıyorum 🔍",
                "Okulda verilen eğitim, çocuğumu geleceğe (lise/üniversite) hazırlıyor 🎓",
                
                // Okul Yönetimi ve İletişim (10 Soru)
                "Okul yönetimiyle aramızda açık ve şeffaf bir iletişim var 💬",
                "Okul yönetimi, velilerin görüş ve önerilerine değer veriyor 💭",
                "Okulun kuralları, adil ve tutarlı bir şekilde uygulanıyor ⚖️",
                "Çocuğumla ilgili bir sorun olduğunda, yetkililere ulaşmak kolay 📞",
                "Okul yönetimi, velilerin geri bildirimlerini dikkate alıyor 👍",
                "Okulun, öğrenci-veli-okul ilişkilerini güçlendirmek için çaba gösterdiğine inanıyorum 🤝",
                "Okulda düzenlenen veli toplantıları, bilgilendirici ve verimli geçiyor 📝",
                "Okulun, velilerin eğitim sürecine aktif olarak katılması için fırsatlar sunduğunu düşünüyorum 🎯",
                "Okul yönetiminin, acil durumlarda hızlı ve etkili bir şekilde bilgilendirme yaptığını düşünüyorum 📢",
                "Okulun, velilerin beklentilerini karşılayacak düzeyde olduğuna inanıyorum ✨",
                
                // Güvenlik ve Fiziksel Ortam (10 Soru)
                "Okulun güvenli bir yer olduğuna inanıyorum ve çocuğumu okula gönül rahatlığıyla gönderiyorum 🛡️",
                "Okulun fiziki koşulları (sınıflar, tuvaletler, bahçe) temiz ve bakımlı 🧹",
                "Okul kantininde satılan ürünlerin kalitesi ve sağlığa uygunluğu yeterli 🍎",
                "Okul, öğrencilerin fiziksel ve psikolojik sağlığına önem veriyor 💚",
                "Okulun, olası zorbalık vakalarına karşı etkili önlemler aldığını düşünüyorum 🛡️",
                "Okulda, çocuğumun spor ve sosyal etkinliklere katılması için yeterli imkanlar var ⚽",
                "Okul servislerinin güvenli ve konforlu olduğuna inanıyorum 🚐",
                "Okulun, yangın ve deprem gibi acil durumlara karşı hazırlıklı olduğunu düşünüyorum 🚨",
                "Çocuğum, okulda kendisini güvende ve mutlu hissediyor ❤️",
                "Okulun, engelli öğrencilerin erişimine uygun olduğunu düşünüyorum ♿",
                
                // Teknolojik Altyapı ve İletişim (10 Soru)
                "Okulun teknolojik altyapısı (internet, bilgisayar laboratuvarı) yeterli 💻",
                "Okulun web sitesi ve mobil uygulaması, bilgiye kolayca erişmemizi sağlıyor 📲",
                "Öğretmenler, teknolojik araçları derslerde etkili bir şekilde kullanıyor 💡",
                "Okul, dijital çağın gereksinimlerine uygun bir eğitim veriyor 🚀",
                "Uzaktan eğitim platformu, ders takibini kolaylaştırıyor 🖥️",
                "Okulun, teknolojik yeniliklere yatırım yaptığını düşünüyorum 💰",
                "E-okul veya benzeri sistemler üzerinden çocuğumun not ve devamsızlık bilgilerini kolayca takip edebiliyorum 📊",
                "Okulun, öğrencilere dijital okuryazarlık becerileri kazandırmayı hedeflediğini düşünüyorum 📚",
                "Okul, veliler için teknoloji kullanımıyla ilgili bilgilendirme toplantıları yapıyor 🤝",
                "Okulun, eğitimde teknolojiyi bir araç olarak verimli bir şekilde kullandığına inanıyorum 🛠️"
            ]
        };
        // Olay dinleyicileri
        document.getElementById('startSurvey').addEventListener('click', startSurvey);
        document.getElementById('submitSurvey').addEventListener('click', submitSurvey);
        document.getElementById('acceptDisclaimer').addEventListener('change', toggleStartButton);

        function toggleStartButton() {
            const acceptDisclaimer = document.getElementById('acceptDisclaimer');
            const startButton = document.getElementById('startSurvey');
            startButton.disabled = !acceptDisclaimer.checked;
            startButton.classList.toggle('opacity-50', !acceptDisclaimer.checked);
        }

        // Fonksiyonlar
        async function startSurvey() {
            const companyName = document.getElementById('companyName').value;
            const disclaimerAccepted = document.getElementById('acceptDisclaimer').checked;
            if (!companyName || !selectedJobType || !disclaimerAccepted) {
                showModal('Lütfen tüm zorunlu alanları doldurun ve veri koruma beyanını kabul edin.');
                return;
            }
            
            // Gizleme ve gösterme işlemleri
            document.getElementById('companyInfoSection').classList.add('hidden');
            document.getElementById('disclaimerSection').classList.add('hidden');
            document.getElementById('surveySection').classList.remove('hidden');
            
            // Bin ID'yi firma adına göre ayarla
            JSONBIN_CONFIG.binId = '68cc142142be1c4f08a35142';
            
            // Anket başlatma
            answers = [];
            currentQuestionIndex = 0;
            currentQuestions = questions[selectedJobType];
            updateProgress();
            renderQuestion();
            surveyStartTime = new Date();
            startTimer();
            await connectAndFetchData();
        }

        function renderQuestion() {
            const questionContainer = document.getElementById('questionContainer');
            questionContainer.innerHTML = '';
            
            if (currentQuestionIndex >= currentQuestions.length) {
                document.getElementById('submitSurvey').classList.remove('hidden');
                return;
            }

            const questionText = currentQuestions[currentQuestionIndex];
            
            const card = document.createElement('div');
            card.className = 'bg-gray-50 border border-gray-200 rounded-lg p-5 shadow-sm fade-in question-card';
            
            card.innerHTML = `
                <p class="text-gray-800 text-lg font-medium mb-4">${currentQuestionIndex + 1}. ${questionText}</p>
                <div class="flex justify-between items-center space-x-2 text-sm font-semibold text-gray-500 mb-3">
                    <span>Kesinlikle Katılmıyorum</span>
                    <span>Kesinlikle Katılıyorum</span>
                </div>
                <div class="flex justify-between items-center gap-2">
                    <button type="button" class="flex-1 py-3 px-1 text-xs rounded-lg border-2 border-gray-300 hover:border-red-500 transition-colors duration-200 ease-in-out" onclick="selectAnswer(1)">1</button>
                    <button type="button" class="flex-1 py-3 px-1 text-xs rounded-lg border-2 border-gray-300 hover:border-orange-500 transition-colors duration-200 ease-in-out" onclick="selectAnswer(2)">2</button>
                    <button type="button" class="flex-1 py-3 px-1 text-xs rounded-lg border-2 border-gray-300 hover:border-yellow-500 transition-colors duration-200 ease-in-out" onclick="selectAnswer(3)">3</button>
                    <button type="button" class="flex-1 py-3 px-1 text-xs rounded-lg border-2 border-gray-300 hover:border-lime-500 transition-colors duration-200 ease-in-out" onclick="selectAnswer(4)">4</button>
                    <button type="button" class="flex-1 py-3 px-1 text-xs rounded-lg border-2 border-gray-300 hover:border-green-500 transition-colors duration-200 ease-in-out" onclick="selectAnswer(5)">5</button>
                </div>
            `;
            
            questionContainer.appendChild(card);
        }

        function selectAnswer(score) {
            answers.push(score);
            currentQuestionIndex++;
            updateProgress();
            renderQuestion();
        }

        function updateProgress() {
            const totalQuestions = currentQuestions.length;
            const answeredQuestions = answers.length;
            const progress = (answeredQuestions / totalQuestions) * 100;
            
            document.getElementById('progressText').textContent = `Anket İlerlemesi ${answeredQuestions}/${totalQuestions} Yanıtlandı`;
            document.getElementById('progressBar').style.width = `${progress}%`;
        }

        function startTimer() {
            timerInterval = setInterval(() => {
                const now = new Date();
                const elapsedSeconds = Math.floor((now - surveyStartTime) / 1000);
                const minutes = Math.floor(elapsedSeconds / 60).toString().padStart(2, '0');
                const seconds = (elapsedSeconds % 60).toString().padStart(2, '0');
                document.getElementById('timeElapsed').textContent = `Süre: ${minutes}:${seconds}`;
            }, 1000);
        }

        async function submitSurvey() {
            const finalScore = answers.reduce((a, b) => a + b, 0) / answers.length;
            const companyName = document.getElementById('companyName').value;
            const firstName = document.getElementById('firstName').value || 'Anonim';
            const lastName = document.getElementById('lastName').value || '';
            const surveyData = {
                companyName: companyName,
                firstName: firstName,
                lastName: lastName,
                jobType: selectedJobType,
                questions: currentQuestions,
                answers: answers,
                averageScore: finalScore.toFixed(2),
                submissionTime: new Date().toISOString(),
                surveyDuration: document.getElementById('timeElapsed').textContent
            };
            
            clearInterval(timerInterval);
            
            try {
                const binData = await fetchBinData();
                binData.surveys.push(surveyData);
                await updateBinData(binData);
                
                showModal('✅ Anketiniz başarıyla kaydedildi. Teşekkür ederiz!');
                resetSurvey();
            } catch (error) {
                showModal('❌ Anket kaydedilemedi. Lütfen internet bağlantınızı kontrol edip tekrar deneyin.');
                console.error('❌ Anket gönderme hatası:', error);
            }
        }

        function resetSurvey() {
            document.getElementById('companyInfoSection').classList.remove('hidden');
            document.getElementById('disclaimerSection').classList.remove('hidden');
            document.getElementById('surveySection').classList.add('hidden');
            document.getElementById('submitSurvey').classList.add('hidden');
            document.getElementById('companyName').value = '';
            document.getElementById('firstName').value = '';
            document.getElementById('lastName').value = '';
            document.getElementById('acceptDisclaimer').checked = false;
            document.getElementById('selectedJobDisplay').textContent = '';
            toggleStartButton();
        }

        async function fetchBinData() {
            const url = `${JSONBIN_CONFIG.baseUrl}/b/${JSONBIN_CONFIG.binId}`;
            const headers = {
                'X-Master-Key': JSONBIN_CONFIG.apiKey
            };
            
            let attempts = 0;
            while (attempts < JSONBIN_CONFIG.maxRetries) {
                try {
                    const response = await fetch(url, { headers });
                    if (!response.ok) {
                        throw new Error(`HTTP Hata! durum: ${response.status}`);
                    }
                    return await response.json();
                } catch (error) {
                    attempts++;
                    console.warn(`⚠️ Veri getirme denemesi başarısız (${attempts}/${JSONBIN_CONFIG.maxRetries}):`, error);
                    await new Promise(res => setTimeout(res, JSONBIN_CONFIG.retryDelay));
                }
            }
            throw new Error('❌ JSONBin.io\'dan veri getirilemedi.');
        }

        async function updateBinData(data) {
            const url = `${JSONBIN_CONFIG.baseUrl}/b/${JSONBIN_CONFIG.binId}`;
            const headers = {
                'Content-Type': 'application/json',
                'X-Master-Key': JSONBIN_CONFIG.apiKey
            };
            
            let attempts = 0;
            while (attempts < JSONBIN_CONFIG.maxRetries) {
                try {
                    const response = await fetch(url, {
                        method: 'PUT',
                        headers,
                        body: JSON.stringify(data)
                    });
                    if (!response.ok) {
                        throw new Error(`HTTP Hata! durum: ${response.status}`);
                    }
                    return await response.json();
                } catch (error) {
                    attempts++;
                    console.warn(`⚠️ Veri güncelleme denemesi başarısız (${attempts}/${JSONBIN_CONFIG.maxRetries}):`, error);
                    await new Promise(res => setTimeout(res, JSONBIN_CONFIG.retryDelay));
                }
            }
            throw new Error('❌ JSONBin.io\'ya veri gönderilemedi.');
        }

        function selectJobType(type) {
            selectedJobType = type;
            document.getElementById('selectedJobDisplay').textContent = `Seçilen Rol: ${type}`;
            document.querySelectorAll('.job-btn').forEach(btn => btn.classList.remove('active-tab'));
            document.getElementById(type === 'Öğrenci' ? 'studentBtn' : type === 'Öğretmen' ? 'teacherBtn' : 'parentBtn').classList.add('active-tab');
        }

        // Modal fonksiyonları
        function showModal(message) {
            const modal = document.getElementById('modal');
            document.getElementById('modalContent').innerHTML = `<p class="text-center text-gray-700">${message}</p>`;
            modal.classList.add('show');
            setTimeout(() => modal.classList.remove('show'), 3000);
        }

        // Yönetici ve Şirket Portalı Fonksiyonları
        async function connectAndFetchData() {
            try {
                const response = await fetchBinData();
                window.allSurveyData = response.surveys || [];
                updateCompanyDashboard();
                updateAdminDashboard();
            } catch (error) {
                console.error('❌ Bağlantı hatası, demo verileri yüklenecek:', error);
                //loadDemoData();
            }
        }
        
        function showModule(module) {
            document.getElementById('surveyModule').classList.add('hidden');
            document.getElementById('companyModule').classList.add('hidden');
            document.getElementById('adminModule').classList.add('hidden');
            document.getElementById(`${module}Module`).classList.remove('hidden');
            
            if (module === 'company') {
                updateCompanyDashboard();
            } else if (module === 'admin') {
                updateAdminDashboard();
            }
        }
        
        // Şirket portalı login
        function loginCompany() {
            const companyName = document.getElementById('companyLoginName').value.trim();
            const password = document.getElementById('companyPassword').value;
            
            // Şifre ile şirket adı eşleşmesi
            // Bu kısım normalde sunucu tarafında yapılır. Basit bir örnek için buraya eklenmiştir.
            const companyCredentials = {
                "Akça Pro X": "123456789012"
            };
            
            if (companyCredentials[companyName] && companyCredentials[companyName] === password) {
                loggedInCompany = companyName;
                document.getElementById('companyLogin').classList.add('hidden');
                document.getElementById('companyDashboard').classList.remove('hidden');
                document.getElementById('companyNameDisplay').textContent = loggedInCompany;
                updateCompanyDashboard();
            } else {
                showModal('Hatalı kurum adı veya şifre.');
            }
        }
        
        function logoutCompany() {
            loggedInCompany = null;
            document.getElementById('companyLogin').classList.remove('hidden');
            document.getElementById('companyDashboard').classList.add('hidden');
            document.getElementById('companyLoginName').value = '';
            document.getElementById('companyPassword').value = '';
        }
        
        function updateCompanyDashboard() {
            if (!loggedInCompany) return;
            
            const companySurveys = window.allSurveyData.filter(s => s.companyName === loggedInCompany);
            
            document.getElementById('totalParticipants').textContent = companySurveys.length;
            const avgScore = companySurveys.length > 0 ? (companySurveys.reduce((sum, s) => sum + parseFloat(s.averageScore), 0) / companySurveys.length).toFixed(1) : '0.0';
            document.getElementById('averageScore').textContent = avgScore;
            
            const positionCounts = {};
            const satisfactionScores = [];
            const timeDurations = [];
            const averageScores = [];
            
            companySurveys.forEach(survey => {
                positionCounts[survey.jobType] = (positionCounts[survey.jobType] || 0) + 1;
                satisfactionScores.push(parseFloat(survey.averageScore));
                const [minutes, seconds] = survey.surveyDuration.split('Süre: ')[1].split(':').map(Number);
                timeDurations.push(minutes * 60 + seconds);
                averageScores.push(parseFloat(survey.averageScore));
            });
            
            const positiveCount = satisfactionScores.filter(s => s >= 4).length;
            const satisfactionRate = companySurveys.length > 0 ? (positiveCount / companySurveys.length * 100).toFixed(0) : '0';
            document.getElementById('satisfactionRate').textContent = `${satisfactionRate}%`;
            
            renderCharts(positionCounts, satisfactionScores, timeDurations, averageScores);
            renderParticipantDetails(companySurveys);
            renderDetailedReport(companySurveys);
        }
        
        function renderCharts(positionData, satisfactionData, timeData, scoreData) {
            const positionCtx = document.getElementById('positionChart').getContext('2d');
            new Chart(positionCtx, {
                type: 'doughnut',
                data: {
                    labels: Object.keys(positionData),
                    datasets: [{
                        data: Object.values(positionData),
                        backgroundColor: ['#3b82f6', '#10b981', '#a855f7'],
                    }]
                },
                options: { maintainAspectRatio: false, cutout: '80%', plugins: { legend: { display: false } } }
            });

            const satisfactionCtx = document.getElementById('satisfactionChart').getContext('2d');
            const satisfactionLabels = ['Çok Düşük', 'Düşük', 'Orta', 'Yüksek', 'Çok Yüksek'];
            const satisfactionCounts = satisfactionLabels.map((label, index) => satisfactionData.filter(score => Math.round(score) === (index + 1)).length);
            new Chart(satisfactionCtx, {
                type: 'bar',
                data: {
                    labels: satisfactionLabels,
                    datasets: [{
                        label: 'Değerlendirme Puanı',
                        data: satisfactionCounts,
                        backgroundColor: ['#ef4444', '#f97316', '#eab308', '#84cc16', '#22c55e']
                    }]
                },
                options: {
                    maintainAspectRatio: false,
                    scales: { y: { beginAtZero: true, ticks: { precision: 0 } } },
                    plugins: { legend: { display: false } }
                }
            });
            
            const timeCtx = document.getElementById('timeChart').getContext('2d');
            const timeRanges = ['< 1 dk', '1-5 dk', '5-10 dk', '> 10 dk'];
            const timeCounts = [
                timeData.filter(t => t < 60).length,
                timeData.filter(t => t >= 60 && t < 300).length,
                timeData.filter(t => t >= 300 && t < 600).length,
                timeData.filter(t => t >= 600).length
            ];
            new Chart(timeCtx, {
                type: 'doughnut',
                data: {
                    labels: timeRanges,
                    datasets: [{
                        data: timeCounts,
                        backgroundColor: ['#60a5fa', '#34d399', '#facc15', '#ef4444'],
                    }]
                },
                options: { maintainAspectRatio: false, cutout: '80%', plugins: { legend: { display: false } } }
            });
            
            const trendCtx = document.getElementById('trendChart').getContext('2d');
            const scoreCounts = {};
            scoreData.forEach(score => {
                const rounded = Math.round(score * 2) / 2; // 0.5'lik dilimler
                scoreCounts[rounded] = (scoreCounts[rounded] || 0) + 1;
            });
            const labels = Object.keys(scoreCounts).sort((a,b) => parseFloat(a) - parseFloat(b));
            const data = labels.map(label => scoreCounts[label]);
            new Chart(trendCtx, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Puan Dağılımı',
                        data: data,
                        borderColor: '#a855f7',
                        tension: 0.3
                    }]
                },
                options: {
                    maintainAspectRatio: false,
                    scales: { y: { beginAtZero: true, ticks: { precision: 0 } } },
                    plugins: { legend: { display: false } }
                }
            });
        }
        
        function toggleParticipantDetails() {
            const details = document.getElementById('participantDetails');
            const btn = document.getElementById('toggleParticipantsBtn');
            const isHidden = details.classList.contains('hidden');
            if (isHidden) {
                details.classList.remove('hidden');
                btn.textContent = '📋 Katılımcıları Gizle';
            } else {
                details.classList.add('hidden');
                btn.textContent = '📋 Katılımcıları Görüntüle';
            }
        }
        
        function renderParticipantDetails(surveys) {
            const tbody = document.getElementById('participantTableBody');
            tbody.innerHTML = surveys.map(survey => {
                const avgScore = parseFloat(survey.averageScore).toFixed(1);
                let evaluation = 'Yeterli';
                let evaluationColor = 'text-gray-600';
                if (avgScore >= 4.0) { evaluation = 'Çok İyi'; evaluationColor = 'text-green-600'; }
                else if (avgScore >= 3.0) { evaluation = 'İyi'; evaluationColor = 'text-blue-600'; }
                else if (avgScore < 2.0) { evaluation = 'Çok Yetersiz'; evaluationColor = 'text-red-600'; }
                else if (avgScore < 3.0) { evaluation = 'Yetersiz'; evaluationColor = 'text-yellow-600'; }

                return `
                    <tr class="border-b border-gray-100">
                        <td class="px-3 py-2 font-semibold text-gray-800">${survey.firstName} ${survey.lastName}</td>
                        <td class="px-3 py-2 text-gray-600">${survey.jobType}</td>
                        <td class="px-3 py-2 text-center font-semibold">${avgScore}</td>
                        <td class="px-3 py-2 text-center ${evaluationColor} font-semibold">${evaluation}</td>
                        <td class="px-3 py-2 text-center text-sm">${new Date(survey.submissionTime).toLocaleDateString('tr-TR')}</td>
                    </tr>
                `;
            }).join('');
        }
        
        function renderDetailedReport(surveys) {
            const reportContainer = document.getElementById('detailedReport');
            reportContainer.innerHTML = '';
            
            const detailedScores = {};
            surveys.forEach(survey => {
                const questions = survey.questions.map((q, i) => ({ text: q, score: survey.answers[i] }));
                questions.forEach(q => {
                    if (!detailedScores[q.text]) {
                        detailedScores[q.text] = { count: 0, sum: 0 };
                    }
                    detailedScores[q.text].count++;
                    detailedScores[q.text].sum += q.score;
                });
            });
            
            const sortedQuestions = Object.keys(detailedScores).sort();
            
            sortedQuestions.forEach(questionText => {
                const avgScore = (detailedScores[questionText].sum / detailedScores[questionText].count).toFixed(2);
                const item = document.createElement('div');
                item.className = 'bg-gray-50 border rounded-lg p-4 shadow-sm';
                item.innerHTML = `
                    <p class="font-semibold text-gray-800 mb-2">${questionText}</p>
                    <div class="flex items-center gap-4">
                        <span class="text-xl font-bold text-blue-600">${avgScore}</span>
                        <div class="w-full bg-gray-200 rounded-full h-2">
                            <div class="bg-blue-600 h-2 rounded-full" style="width: ${(avgScore / 5) * 100}%"></div>
                        </div>
                    </div>
                `;
                reportContainer.appendChild(item);
            });
        }
        
        // PDF Göster fonksiyonu
        function showPDFReport() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            
            doc.text('Kurum Değerlendirme Anketi Raporu', 10, 10);
            doc.text(`Kurum Adı: ${loggedInCompany}`, 10, 20);
            
            const totalParticipants = document.getElementById('totalParticipants').textContent;
            const averageScore = document.getElementById('averageScore').textContent;
            const satisfactionRate = document.getElementById('satisfactionRate').textContent;
            
            doc.text(`Toplam Katılımcı: ${totalParticipants}`, 10, 30);
            doc.text(`Ortalama Puan: ${averageScore}`, 10, 40);
            doc.text(`Değerlendirme Oranı: ${satisfactionRate}`, 10, 50);
            
            let yOffset = 60;
            const detailedReports = document.getElementById('detailedReport').children;
            
            doc.text('Detaylı Anket Sonuçları', 10, yOffset + 10);
            yOffset += 20;
            
            Array.from(detailedReports).forEach(reportItem => {
                const questionText = reportItem.querySelector('p').textContent;
                const score = reportItem.querySelector('span').textContent;
                doc.text(`${questionText}: ${score}`, 15, yOffset);
                yOffset += 10;
            });
            
            doc.save('anket_raporu.pdf');
        }

        // Yönetici portalı login
        function loginAdmin() {
            const password = document.getElementById('adminPassword').value;
            // Yönetici şifresi basit bir örnek için buraya yazılmıştır.
            if (password === 'admin123') { 
                isAdminLoggedIn = true;
                document.getElementById('adminLogin').classList.add('hidden');
                document.getElementById('adminDashboard').classList.remove('hidden');
                updateAdminDashboard();
            } else {
                showModal('Hatalı yönetici şifresi.');
            }
        }
        
        function logoutAdmin() {
            isAdminLoggedIn = false;
            document.getElementById('adminLogin').classList.remove('hidden');
            document.getElementById('adminDashboard').classList.add('hidden');
            document.getElementById('adminPassword').value = '';
        }
        
        function updateAdminDashboard() {
            if (!isAdminLoggedIn) return;
            
            const totalCompanies = [...new Set(window.allSurveyData.map(s => s.companyName))].length;
            document.getElementById('totalCompanies').textContent = totalCompanies;
            
            document.getElementById('totalUsers').textContent = window.allSurveyData.length;
            
            const activeSurveys = window.allSurveyData.length; // Basit bir örnek
            document.getElementById('activeSurveys').textContent = activeSurveys;
            
            renderCompanyList();
        }
        
        function renderCompanyList() {
            const tbody = document.getElementById('companyList');
            tbody.innerHTML = '';
            const companyNames = [...new Set(window.allSurveyData.map(s => s.companyName))];
            
            companyNames.forEach(company => {
                const companySurveys = window.allSurveyData.filter(s => s.companyName === company);
                const participantCount = companySurveys.length;
                const avgScore = companySurveys.length > 0 ? (companySurveys.reduce((sum, s) => sum + parseFloat(s.averageScore), 0) / companySurveys.length).toFixed(1) : '0.0';
                
                const tr = document.createElement('tr');
                tr.className = 'border-b border-gray-100';
                tr.innerHTML = `
                    <td class="px-4 py-3 font-semibold text-gray-800">${company}</td>
                    <td class="px-4 py-3 text-gray-600">***</td>
                    <td class="px-4 py-3 text-center">${participantCount}</td>
                    <td class="px-4 py-3 text-sm text-green-600 font-semibold">Aktif</td>
                    <td class="px-4 py-3">
                        <button onclick="viewCompanyReport('${company}')" class="px-3 py-1 bg-blue-500 text-white text-xs rounded hover:bg-blue-600">Rapor</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }
        
        function viewCompanyReport(companyName) {
            loggedInCompany = companyName;
            showModule('company');
        }

        // Initialize on page load
        window.addEventListener('load', () => {
            toggleStartButton();
            connectAndFetchData();
        });
    </script>
</body>
</html>
