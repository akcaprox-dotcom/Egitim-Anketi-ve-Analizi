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
                <div onclick="showModule('admin')" class="w-3 h-3 cursor-pointer opacity-15 hover:opacity-50 
transition-opacity" title="">
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

        // JSONbin.io konfigürasyonu
        const masterKey = '$2a$10$SCDSdHz/rW/Z3Q6EWaB68uSJR2GAhE3pjG/i3.gJEhKsviO.yl6DC';
        const binId = '68cc142142be1c4f08a35142';
        const url = `https://api.jsonbin.io/v3/b/${binId}/latest`;
        
        // Soru setleri
        const questions = {
            "Öğrenci": [
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
                "Okul yönetimiyle aramızda açık ve şeffaf bir iletişim var 💬",
                "Okul yönetimi, velilerin görüş ve önerilerine değer veriyor 💭",
                "Okulun kuralları, adil ve tutarlı bir şekilde uygulanıyor ⚖️",
                "Çocuğumla ilgili bir sorun olduğunda, yetkililere ulaşmak kolay 📞",
                "Okul yönetimine güveniyorum ❤️",
                "Okulun, öğrencilerin güvenliğini sağlamak için yeterli önlemleri aldığına inanıyorum 🛡️",
                "Okulun misyon ve vizyonu, beklentilerimle uyumlu 🌟",
                "Okuldan aldığım genel bilgilendirmeler (duyurular, bültenler) yeterli ve zamanında 📢",
                "Okulun, velilerin eğitim sürecine katılımını teşvik ettiğini düşünüyorum 🤝",
                "Okulun, zorbalık ve diğer disiplin sorunlarına karşı etkili çözümler ürettiğine inanıyorum 🛡️",
                "Çocuğumun öğretmenlerinden memnunum 👨‍🏫",
                "Öğretmenler, çocuğumun gelişim durumu hakkında bana düzenli ve yapıcı geri bildirim veriyor 📝",
                "Öğretmenlerin, öğrencilerle saygılı ve destekleyici bir ilişki kurduğunu düşünüyorum 🤗",
                "Okulun rehberlik servisi, çocuğumun akademik ve duygusal gelişimini destekliyor 💚",
                "Rehberlik servisinden aldığım hizmetlerden memnunum 👥",
                "Öğretmenler ve rehberlik servisi, veli kaygılarını ciddiye alıyor 🤝",
                "Veli toplantılarının verimli geçtiğini düşünüyorum ⏰",
                "Okul, öğretmenlerin mesleki gelişimine yatırım yapıyor 📖",
                "Öğretmenlerin, ders dışında da öğrencilerine destek olduğuna inanıyorum 💪",
                "Çocuğumun öğretmenlerinin, dersleri daha ilgi çekici hale getirmek için çaba gösterdiğini düşünüyorum ⚡",
                "Okulun fiziki koşulları (derslikler, ortak alanlar) yeterli ve temiz 🏫",
                "Okulun teknolojik altyapısı (internet, bilgisayar laboratuvarı) beklentilerimi karşılıyor 💻",
                "Okulun sunduğu sosyal ve spor olanakları yeterli ⚽",
                "Okul kantinindeki yiyeceklerin sağlıklı olduğunu düşünüyorum 🍎",
                "Okulun, çocuğumun dışarıda güvenli vakit geçirebileceği alanları var 🌳",
                "Okulun ulaşım imkanları yeterli ve güvenli 🚌",
                "Okulun, öğrenciler için sağlıklı bir beslenme politikası olduğuna inanıyorum 🥗",
                "Okulun kütüphane ve laboratuvar gibi akademik kaynakları yeterli ve kullanışlı 🧪",
                "Okul, öğrencilere çevre bilinci ve sürdürülebilirlik konularında farkındalık kazandırıyor 🌱",
                "Okulun, velilerle olan iletişim kanallarını sürekli geliştirdiğini düşünüyorum 📞"
            ]
        };

        let userAnswers = [];
        let scoreRanges = {
            "Öğrenci": [5, 4, 3, 2, 1],
            "Öğretmen": [5, 4, 3, 2, 1],
            "Veli/Ebeveyn": [5, 4, 3, 2, 1]
        };

        let companyData = {}; // Şirket bilgilerini saklamak için

        // Modül gösterimi
        function showModule(module) {
            document.getElementById('surveyModule').style.display = 'none';
            document.getElementById('companyModule').style.display = 'none';
            document.getElementById('adminModule').style.display = 'none';
            document.getElementById(module + 'Module').style.display = 'block';
            currentModule = module;
            // İstatistikleri sadece ilgili modülde yükle
            if (module === 'company' && loggedInCompany) {
                loadStats();
            }
        }
        
        // Rol seçimi
        function selectJobType(type) {
            selectedJobType = type;
            document.querySelectorAll('.job-btn').forEach(btn => btn.classList.remove('active-tab'));
            document.getElementById(`${type === 'Öğrenci' ? 'student' : type === 'Öğretmen' ? 'teacher' : 'parent'}Btn`).classList.add('active-tab');
            document.getElementById('selectedJobDisplay').textContent = `Seçilen Rol: ${type}`;
        }

        // Anketi başlat
        document.getElementById('startSurvey').addEventListener('click', () => {
            if (!document.getElementById('acceptDisclaimer').checked) {
                alert("Lütfen veri koruma beyanını okuyup kabul ediniz.");
                return;
            }
            if (!selectedJobType) {
                alert("Lütfen rolünüzü seçiniz.");
                return;
            }
            document.getElementById('companyInfoSection').style.display = 'none';
            document.getElementById('surveySection').style.display = 'block';
            startSurvey();
        });

        function startSurvey() {
            currentQuestions = questions[selectedJobType];
            currentQuestionIndex = 0;
            userAnswers = [];
            
            document.getElementById('progressText').textContent = `Anket İlerlemesi 0/${currentQuestions.length} Yanıtlandı`;
            document.getElementById('progressBar').style.width = '0%';
            
            surveyStartTime = Date.now();
            if (timerInterval) {
                clearInterval(timerInterval);
            }
            timerInterval = setInterval(updateTimer, 1000);

            showNextQuestion();
        }

        function updateTimer() {
            const elapsedTime = Math.floor((Date.now() - surveyStartTime) / 1000);
            const minutes = String(Math.floor(elapsedTime / 60)).padStart(2, '0');
            const seconds = String(elapsedTime % 60).padStart(2, '0');
            document.getElementById('timeElapsed').textContent = `Süre: ${minutes}:${seconds}`;
        }

        function showNextQuestion() {
            if (currentQuestionIndex < currentQuestions.length) {
                const question = currentQuestions[currentQuestionIndex];
                const questionContainer = document.getElementById('questionContainer');
                questionContainer.innerHTML = `
                    <div class="bg-gray-50 border border-gray-200 p-4 rounded-lg shadow-sm">
                        <h4 class="font-semibold text-gray-800 text-md mb-4">${currentQuestionIndex + 1}. ${question}</h4>
                        <div class="flex justify-between items-center text-xs text-gray-500 mb-2">
                            <span>Kesinlikle Katılmıyorum</span>
                            <span>Kesinlikle Katılıyorum</span>
                        </div>
                        <div id="ratingsContainer" class="flex justify-between items-center mb-4">
                            ${scoreRanges[selectedJobType].map(score => `
                                <button onclick="selectRating(${score})" class="w-10 h-10 md:w-12 md:h-12 flex items-center justify-center rounded-full border-2 border-gray-300 text-gray-600 font-bold text-sm md:text-md hover:bg-purple-100 hover:border-purple-400 transition-colors duration-200">${score}</button>
                            `).join('')}
                        </div>
                    </div>
                `;
            }
        }

        function selectRating(rating) {
            userAnswers.push(rating);
            currentQuestionIndex++;
            updateProgressBar();

            if (currentQuestionIndex < currentQuestions.length) {
                showNextQuestion();
            } else {
                document.getElementById('submitSurvey').style.display = 'block';
                document.getElementById('questionContainer').innerHTML = `
                    <div class="text-center p-6 bg-white rounded-lg shadow-inner">
                        <h3 class="text-xl font-semibold text-green-600 mb-2">Anket Tamamlandı!</h3>
                        <p class="text-gray-600 text-sm">Cevaplarınız gönderime hazır. Lütfen formu tamamlamak için aşağıdaki butona tıklayın.</p>
                    </div>
                `;
            }
        }

        function updateProgressBar() {
            const progress = (currentQuestionIndex / currentQuestions.length) * 100;
            document.getElementById('progressBar').style.width = `${progress}%`;
            document.getElementById('progressText').textContent = `Anket İlerlemesi ${currentQuestionIndex}/${currentQuestions.length} Yanıtlandı`;
        }

        // Anket gönderim fonksiyonu
        document.getElementById('submitSurvey').addEventListener('click', async () => {
            const companyName = document.getElementById('companyName').value;
            const firstName = document.getElementById('firstName').value;
            const lastName = document.getElementById('lastName').value;

            // En az şirket adı ve rol seçimi zorunlu
            if (!companyName || !selectedJobType) {
                alert("Lütfen kurum adınızı ve rolünüzü giriniz.");
                return;
            }

            const averageScore = userAnswers.reduce((sum, score) => sum + score, 0) / userAnswers.length;

            const categoryAverages = {};
            const categories = {};
            currentQuestions.forEach((q, index) => {
                const category = Object.keys(questions).find(key => questions[key].includes(q));
                if (!categories[category]) {
                    categories[category] = { total: 0, count: 0 };
                }
                categories[category].total += userAnswers[index];
                categories[category].count++;
            });

            for (const category in categories) {
                categoryAverages[category] = (categories[category].total / categories[category].count).toFixed(1);
            }

            const responseData = {
                company: companyName,
                jobType: selectedJobType,
                firstName: firstName,
                lastName: lastName,
                averageScore: averageScore.toFixed(1),
                categoryAverages: categoryAverages,
                submittedAt: new Date().toISOString()
            };

            try {
                // Mevcut verileri çek
                const existingResponses = await fetch(url, {
                    method: 'GET',
                    headers: { 'X-Master-Key': masterKey }
                }).then(res => res.json()).then(data => data.record || []);
                
                // Yeni yanıtı mevcut verilere ekle
                const updatedResponses = [...existingResponses, responseData];

                // Güncellenmiş veriyi jsonbin'e geri gönder
                const response = await fetch(url, {
                    method: 'PUT',
                    headers: {
                        'Content-Type': 'application/json',
                        'X-Master-Key': masterKey
                    },
                    body: JSON.stringify(updatedResponses)
                });

                if (response.ok) {
                    clearInterval(timerInterval);
                    document.getElementById('surveySection').style.display = 'none';
                    document.getElementById('submissionSuccess').style.display = 'block';
                    alert("Anket başarıyla tamamlandı ve kaydedildi!");
                    showModule('company'); // İstatistikleri göstermek için yönlendir
                } else {
                    console.error('Sunucu yanıtı hatası:', response.statusText);
                    alert("Anket gönderilirken bir hata oluştu.");
                }
            } catch (error) {
                console.error('❌ Gönderim hatası:', error);
                alert("Anket gönderilirken bir hata oluştu. Lütfen API anahtarınızı ve Bin ID'nizi kontrol edin.");
            }
        });

        // Şirket Giriş Fonksiyonu
        async function loginCompany() {
            const companyName = document.getElementById('companyLoginName').value.trim();
            const password = document.getElementById('companyPassword').value.trim();

            if (companyName.length > 0 && password === 'akcaprox2024') {
                loggedInCompany = companyName;
                document.getElementById('companyNameDisplay').textContent = `${companyName} Analiz ve Raporları`;
                document.getElementById('companyLogin').style.display = 'none';
                document.getElementById('companyDashboard').style.display = 'block';
                loadStats();
            } else {
                alert('Hatalı kurum adı veya şifre!');
            }
        }
        
        function logoutCompany() {
            loggedInCompany = null;
            document.getElementById('companyLogin').style.display = 'block';
            document.getElementById('companyDashboard').style.display = 'none';
        }

        // İstatistikleri Yükle ve Güncelle
        async function loadStats() {
            if (!loggedInCompany) {
                return;
            }
            try {
                const response = await fetch(url, {
                    method: 'GET',
                    headers: { 'X-Master-Key': masterKey }
                });
                
                if (!response.ok) {
                    throw new Error('İstatistikler yüklenemedi');
                }
                
                const stats = await response.json();
                const allSurveys = stats.record || [];
                const companySurveys = allSurveys.filter(s => s.company.toLowerCase() === loggedInCompany.toLowerCase());
                
                if (companySurveys.length > 0) {
                    updateCompanyStats(companySurveys);
                } else {
                    document.getElementById('totalParticipants').textContent = '0';
                    document.getElementById('averageScore').textContent = '0.0';
                    document.getElementById('satisfactionRate').textContent = '0%';
                    document.getElementById('participantTableBody').innerHTML = '<tr><td colspan="5" class="text-center py-4 text-gray-500">Bu kuruma ait anket verisi bulunmamaktadır.</td></tr>';
                    
                    // Grafiklerin boş gösterilmesi
                    if (window.positionChart) window.positionChart.destroy();
                    if (window.satisfactionChart) window.satisfactionChart.destroy();
                    if (window.timeChart) window.timeChart.destroy();
                    if (window.trendChart) window.trendChart.destroy();
                }
            } catch (error) {
                console.error('❌ İstatistik yükleme hatası:', error);
                alert("İstatistikler yüklenirken bir hata oluştu. Lütfen bağlantınızı kontrol edin.");
            }
        }

        function updateCompanyStats(surveys) {
            const totalParticipants = surveys.length;
            const totalScore = surveys.reduce((sum, s) => sum + parseFloat(s.averageScore), 0);
            const averageScore = (totalScore / totalParticipants).toFixed(1);
            const satisfactionCount = surveys.filter(s => parseFloat(s.averageScore) >= 3.5).length;
            const satisfactionRate = ((satisfactionCount / totalParticipants) * 100).toFixed(0);

            document.getElementById('totalParticipants').textContent = totalParticipants;
            document.getElementById('averageScore').textContent = averageScore;
            document.getElementById('satisfactionRate').textContent = `${satisfactionRate}%`;
            
            // Pozisyon Grafiği
            const positionCounts = surveys.reduce((acc, s) => {
                acc[s.jobType] = (acc[s.jobType] || 0) + 1;
                return acc;
            }, {});
            createChart('positionChart', 'doughnut', Object.keys(positionCounts), Object.values(positionCounts), 'Katılımcı Pozisyonları');
            
            // Değerlendirme Grafiği
            const satisfactionLabels = ['Çok Kötü', 'Kötü', 'Orta', 'İyi', 'Çok İyi'];
            const satisfactionCounts = [0, 0, 0, 0, 0];
            surveys.forEach(s => {
                const score = parseFloat(s.averageScore);
                if (score < 1.5) satisfactionCounts[0]++;
                else if (score < 2.5) satisfactionCounts[1]++;
                else if (score < 3.5) satisfactionCounts[2]++;
                else if (score < 4.5) satisfactionCounts[3]++;
                else satisfactionCounts[4]++;
            });
            createChart('satisfactionChart', 'bar', satisfactionLabels, satisfactionCounts, 'Anket Puan Dağılımı');
            
            // Süre Dağılımı (Örnek Veri)
            createChart('timeChart', 'line', ['<5dk', '5-10dk', '10-15dk', '>15dk'], [Math.floor(totalParticipants*0.5), Math.floor(totalParticipants*0.3), Math.floor(totalParticipants*0.1), Math.floor(totalParticipants*0.1)], 'Anket Süreleri');

            // Puan Dağılımı
            const trendData = {};
            surveys.forEach(s => {
                const date = new Date(s.submittedAt).toLocaleDateString('tr-TR');
                if (!trendData[date]) {
                    trendData[date] = { total: 0, count: 0 };
                }
                trendData[date].total += parseFloat(s.averageScore);
                trendData[date].count++;
            });
            const trendLabels = Object.keys(trendData);
            const trendScores = trendLabels.map(date => (trendData[date].total / trendData[date].count).toFixed(1));
            createChart('trendChart', 'line', trendLabels, trendScores, 'Ortalama Puan Trendi');
            
            // Katılımcı Detay Tablosu
            populateParticipantTable(surveys);
            
            // Detaylı Rapor
            populateDetailedReport(surveys);
        }

        function createChart(canvasId, type, labels, data, label) {
            const ctx = document.getElementById(canvasId).getContext('2d');
            if (window[canvasId]) {
                window[canvasId].destroy();
            }
            window[canvasId] = new Chart(ctx, {
                type: type,
                data: {
                    labels: labels,
                    datasets: [{
                        label: label,
                        data: data,
                        backgroundColor: type === 'doughnut' ? ['#4ade80', '#2563eb', '#9333ea'] : type === 'bar' ? '#f43f5e' : '#14b8a6',
                        borderColor: 'rgba(0, 0, 0, 0.1)',
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: type === 'doughnut',
                            position: 'bottom'
                        }
                    },
                    scales: type !== 'doughnut' ? {
                        y: {
                            beginAtZero: true
                        }
                    } : {}
                }
            });
        }
        
        function populateParticipantTable(surveys) {
            const tableBody = document.getElementById('participantTableBody');
            tableBody.innerHTML = '';
            surveys.forEach(survey => {
                const avgScore = parseFloat(survey.averageScore).toFixed(1);
                const evaluation = avgScore >= 4.5 ? 'Çok İyi' : avgScore >= 3.5 ? 'İyi' : avgScore >= 2.5 ? 'Orta' : avgScore >= 1.5 ? 'Kötü' : 'Çok Kötü';
                const evaluationColor = avgScore >= 4.5 ? 'text-green-600' : avgScore >= 3.5 ? 'text-blue-600' : avgScore >= 2.5 ? 'text-yellow-600' : 'text-red-600';
                tableBody.innerHTML += `
                    <tr class="border-b last:border-0 hover:bg-gray-50">
                        <td class="px-3 py-2">${survey.firstName || ''} ${survey.lastName || ''}</td>
                        <td class="px-3 py-2">${survey.jobType}</td>
                        <td class="px-3 py-2 text-center font-semibold">${avgScore}</td>
                        <td class="px-3 py-2 text-center ${evaluationColor} font-semibold">${evaluation}</td>
                        <td class="px-3 py-2 text-center text-sm">${new Date(survey.submittedAt).toLocaleDateString('tr-TR')}</td>
                    </tr>
                `;
            });
        }
        
        function toggleParticipantDetails() {
            const detailsDiv = document.getElementById('participantDetails');
            const button = document.getElementById('toggleParticipantsBtn');
            if (detailsDiv.classList.contains('hidden')) {
                detailsDiv.classList.remove('hidden');
                button.textContent = '📋 Katılımcıları Gizle';
            } else {
                detailsDiv.classList.add('hidden');
                button.textContent = '📋 Katılımcıları Görüntüle';
            }
        }
    </script>
</body>
</html>
