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
            .no-print { display: none !important;
            }
            body { background: white !important;
            }
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
     
<p>• Hack, veri ihlali vb.
güvenlik olaylarından kaynaklanan bilgi erişimlerinin sorumluluğu Akça Pro X'e ait değildir.</p>
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
   
<divÖğrenci</div>
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
                        <h2 class="text-3xl font-bold">Okul/Kurum Raporları</h2>
         
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
                "Okuldaki öğrenci projeleri, 
ekip çalışması ve liderlik becerilerimi geliştiriyor 👑",
                "Okulun, bilimsel ve sanatsal yarışmalara katılmamızı desteklediğini düşünüyorum 🏆",
                "Okulda aldığım eğitimle gurur duyuyorum 💪",
                "Gelecekte bu okulun, başarılı bir mezunu olmak için doğru yerdeyim 🎯",
                "Okulumun mezuniyetten sonra da bana destek olacağına 
inanıyorum 🤗",
                
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
                "Okulun fiziki koşulları (ısıtma, aydınlatma 
vb.) verimli bir çalışma ortamı sunuyor 🌡️",
                "Öğretmenler odası ve diğer sosyal alanlar yeterince konforlu 🏢",
                "Okulun, öğretmenlerin mesleki gelişimine yönelik yeterli bütçe ayırdığını düşünüyorum 💰",
                "Okulda, öğrencilerin akademik başarısını destekleyecek yeterli kaynak (kütüphane, laboratuvar) var 📚",
                "Okulun, öğretmenlerin fiziksel ve psikolojik sağlığına önem verdiğini düşünüyorum 💚",
                "Okulda, veli görüşmelerini rahatça yapabileceğim uygun ortamlar mevcut 🤝",
                "Okulun genel düzeni ve temizliği yeterli 🧹",
                "Okuldaki ders dışı etkinlikler, öğrencilerin gelişimine katkı sağlıyor 🎭",
                // Yönetim ve İletişim (10 Soru)
                "Okul yönetimiyle aramızda açık ve şeffaf bir iletişim var 💬",
                "Okul 
yönetiminin, öğretmenlerin fikirlerine ve önerilerine değer verdiğini düşünüyorum 💭",
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
                "Velilerden gelen geri bildirimler, öğretim 
yöntemlerimi geliştirmeme yardımcı oluyor 📝",
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
                "Dijital araçların, 
öğrencilerin öğrenmesini kolaylaştırdığını düşünüyorum ⚡",
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
                "Çocuğum, okulda 
öğrendiklerinin gerçek hayatta işe yarayacağını düşünüyor 🌟",
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
                "Çocuğumla ilgili 
bir sorun olduğunda, yetkililere ulaşmak kolay 📞",
                "Okul yönetimine güveniyorum ❤️",
                "Okulun, öğrencilerin güvenliğini sağlamak için yeterli önlemleri aldığına inanıyorum 🛡️",
                "Okulun misyon ve vizyonu, beklentilerimle uyumlu 🌟",
                "Okuldan aldığım genel bilgilendirmeler (duyurular, bültenler) yeterli ve zamanında 📢",
                "Okulun, velilerin eğitim sürecine katılımını teşvik ettiğini düşünüyorum 🤝",
                "Okulun, zorbalık ve diğer disiplin sorunlarına karşı etkili çözümler ürettiğine inanıyorum 🛡️",
                // Öğretmenler ve Rehberlik Hizmetleri (10 Soru)
                "Çocuğumun öğretmenlerinden memnunum 👨‍🏫",
                "Öğretmenler, çocuğumun gelişim durumu hakkında bana düzenli ve yapıcı geri bildirim veriyor 📝",
                "Öğretmenlerin, öğrencilerle saygılı ve destekleyici bir ilişki kurduğunu düşünüyorum 🤗",
                "Okulun rehberlik servisi, çocuğumun akademik ve duygusal 
gelişimini destekliyor 💚",
                "Rehberlik servisinden aldığım hizmetlerden memnunum 👥",
                "Öğretmenler ve rehberlik servisi, veli kaygılarını ciddiye alıyor 🤝",
                "Veli toplantılarının verimli geçtiğini düşünüyorum ⏰",
                "Okul, öğretmenlerin mesleki gelişimine yatırım yapıyor 📖",
                "Öğretmenlerin, ders dışında da öğrencilerine destek olduğuna inanıyorum 💪",
                "Çocuğumun öğretmenlerinin, dersleri daha ilgi çekici hale getirmek için çaba gösterdiğini düşünüyorum ⚡",
                // Okul Ortamı ve Olanaklar (10 Soru)
                "Okulun fiziki koşulları (derslikler, ortak alanlar) yeterli ve temiz 🏫",
                "Okulun teknolojik altyapısı (internet, bilgisayar laboratuvarı) beklentilerimi karşılıyor 💻",
                "Okulun sunduğu sosyal ve spor olanakları yeterli ⚽",
                "Okul kantinindeki yiyeceklerin sağlıklı olduğunu düşünüyorum 🍎",
                "Okulun, çocuğumun dışarıda güvenli vakit geçirebileceği 
alanları var 🌳",
                "Okulun ulaşım imkanları yeterli 🚌",
                "Okulun temizlik ve hijyen standartları yeterli 🧹",
                "Okulun genel mimari yapısı ve estetiği hoşuma gidiyor 🏛️",
                "Okulun, engelli öğrencilere yönelik erişim imkanları yeterli ♿",
                "Okulun, çevreci uygulamalar (geri dönüşüm vb.) konusunda duyarlı olduğuna inanıyorum ♻️"
            ]
        };

        // Veritabanı işlemleri
        async function fetchBinData() {
            try {
                const response = await fetch(`${JSONBIN_CONFIG.baseUrl}/b/${JSONBIN_CONFIG.binId}/latest`, {
                    headers: {
                        'X-Master-Key': JSONBIN_CONFIG.apiKey
                    }
                });
                if (!response.ok) {
                    throw new Error(`HTTP Hata: ${response.status}`);
                }
                const data = await response.json();
                console.log('✅ Veri başarıyla çekildi:', data.record);
                return data.record;
            } catch (error) {
                console.error('❌ Veri çekme hatası:', error);
                showModal(`❌ Bağlantı Hatası: Veriler alınamadı. Hata: ${error.message}`);
                return [];
            }
        }

        async function updateBinData(data) {
            let retries = 0;
            while (retries < JSONBIN_CONFIG.maxRetries) {
                try {
                    const response = await fetch(`${JSONBIN_CONFIG.baseUrl}/b/${JSONBIN_CONFIG.binId}`, {
                        method: 'PUT',
                        headers: {
                            'Content-Type': 'application/json',
                            'X-Master-Key': JSONBIN_CONFIG.apiKey
                        },
                        body: JSON.stringify(data)
                    });
                    if (!response.ok) {
                        throw new Error(`HTTP Hata: ${response.status}`);
                    }
                    const updatedData = await response.json();
                    console.log('✅ Veri başarıyla güncellendi:', updatedData);
                    return updatedData;
                } catch (error) {
                    console.error(`❌ Veri güncelleme hatası. ${retries + 1}. deneme:`, error);
                    retries++;
                    if (retries < JSONBIN_CONFIG.maxRetries) {
                        await new Promise(resolve => setTimeout(resolve, JSONBIN_CONFIG.retryDelay));
                    } else {
                        throw error;
                    }
                }
            }
        }

        // Genel yardımcı fonksiyonlar
        function showModule(moduleName) {
            document.getElementById('surveyModule').style.display = 'none';
            document.getElementById('companyModule').style.display = 'none';
            document.getElementById('adminModule').style.display = 'none';
            document.getElementById(`${moduleName}Module`).style.display = 'block';
            currentModule = moduleName;
        }

        function showModal(content) {
            document.getElementById('modalContent').innerHTML = content;
            document.getElementById('modal').classList.add('show');
        }

        function hideModal() {
            document.getElementById('modal').classList.remove('show');
        }

        // Anket Modülü
        document.getElementById('startSurvey').addEventListener('click', () => {
            const acceptDisclaimer = document.getElementById('acceptDisclaimer').checked;
            const companyName = document.getElementById('companyName').value.trim();

            if (!acceptDisclaimer) {
                showModal('<p class="text-lg font-bold text-red-600 mb-2">Hata!</p><p>Devam etmek için veri koruma beyanını kabul etmelisiniz.</p><button onclick="hideModal()" class="mt-4 px-4 py-2 bg-blue-600 text-white rounded">Tamam</button>');
                return;
            }

            if (!companyName || !selectedJobType) {
                showModal('<p class="text-lg font-bold text-red-600 mb-2">Hata!</p><p>Lütfen tüm zorunlu alanları doldurun: Kurum Adı ve Rolünüz.</p><button onclick="hideModal()" class="mt-4 px-4 py-2 bg-blue-600 text-white rounded">Tamam</button>');
                return;
            }

            // Sayfa geçişini yönet
            document.getElementById('companyInfoSection').style.display = 'none';
            document.getElementById('surveySection').style.display = 'block';
            document.getElementById('submitSurvey').style.display = 'block';
            
            startSurvey(selectedJobType);
        });

        function selectJobType(type) {
            selectedJobType = type;
            const jobButtons = document.querySelectorAll('.job-btn');
            jobButtons.forEach(btn => {
                btn.classList.remove('active-tab');
            });

            document.getElementById(`${type.toLowerCase().replace(/[\/ÇçĞğIıİiÖöŞşÜü]/g, (match) => {
                switch(match) {
                    case '/': return '-';
                    case 'Ç': case 'ç': return 'c';
                    case 'Ğ': case 'ğ': return 'g';
                    case 'I': case 'ı': return 'i';
                    case 'İ': case 'i': return 'i';
                    case 'Ö': case 'ö': return 'o';
                    case 'Ş': case 'ş': return 's';
                    case 'Ü': case 'ü': return 'u';
                }
            })}Btn`).classList.add('active-tab');
            document.getElementById('selectedJobDisplay').textContent = `Seçilen Rol: ${type}`;
        }
        
        function startSurvey(jobType) {
            currentQuestions = questions[jobType];
            answers = [];
            currentQuestionIndex = 0;
            surveyStartTime = new Date();
            startTimer();
            renderQuestion();
        }

        function renderQuestion() {
            if (currentQuestionIndex >= currentQuestions.length) {
                // Anket bitti, tamamla
                document.getElementById('submitSurvey').style.display = 'block';
                document.getElementById('questionContainer').innerHTML = `
                    <div class="text-center p-6 bg-green-100 text-green-800 rounded-lg">
                        <p class="font-bold text-lg mb-2">Anket Tamamlandı!</p>
                        <p class="text-sm">Tüm soruları yanıtladınız. Lütfen anketi tamamla butonuna basın.</p>
                    </div>
                `;
                return;
            }

            const questionText = currentQuestions[currentQuestionIndex];
            const container = document.getElementById('questionContainer');
            container.innerHTML = `
                <div class="bg-gray-50 p-6 rounded-lg shadow-inner animate-fade-in-up">
                    <h4 class="font-semibold text-gray-700 text-lg mb-4">Soru ${currentQuestionIndex + 1}:</h4>
                    <p class="text-gray-900 text-base font-medium mb-4">${questionText}</p>
                    <div class="grid grid-cols-2 md:grid-cols-5 gap-3 mt-4" data-question-index="${currentQuestionIndex}">
                        <button class="choice-btn px-4 py-3 border-2 border-gray-300 rounded-lg text-sm transition-all duration-200 hover:scale-105 hover:border-red-500" data-value="1">Kesinlikle Katılmıyorum</button>
                        <button class="choice-btn px-4 py-3 border-2 border-gray-300 rounded-lg text-sm transition-all duration-200 hover:scale-105 hover:border-orange-500" data-value="2">Katılmıyorum</button>
                        <button class="choice-btn px-4 py-3 border-2 border-gray-300 rounded-lg text-sm transition-all duration-200 hover:scale-105 hover:border-yellow-500" data-value="3">Kararsızım</button>
                        <button class="choice-btn px-4 py-3 border-2 border-gray-300 rounded-lg text-sm transition-all duration-200 hover:scale-105 hover:border-blue-500" data-value="4">Katılıyorum</button>
                        <button class="choice-btn px-4 py-3 border-2 border-gray-300 rounded-lg text-sm transition-all duration-200 hover:scale-105 hover:border-green-500" data-value="5">Kesinlikle Katılıyorum</button>
                    </div>
                </div>
            `;
            updateProgressBar();
            updateProgressText();
        }

        document.getElementById('questionContainer').addEventListener('click', (e) => {
            if (e.target.classList.contains('choice-btn')) {
                const value = parseInt(e.target.dataset.value);
                const questionIndex = parseInt(e.target.closest('.grid').dataset.question-index);
                answers[questionIndex] = value;
                
                // Seçilen butona animasyon ekle ve diğerlerini eski haline getir
                const buttons = e.target.parentNode.querySelectorAll('.choice-btn');
                buttons.forEach(btn => btn.classList.remove('scale-105', 'border-red-500', 'border-orange-500', 'border-yellow-500', 'border-blue-500', 'border-green-500', 'font-bold'));
                e.target.classList.add('scale-105', 'font-bold');
                
                // Renk ekle
                if (value === 1) e.target.classList.add('border-red-500');
                if (value === 2) e.target.classList.add('border-orange-500');
                if (value === 3) e.target.classList.add('border-yellow-500');
                if (value === 4) e.target.classList.add('border-blue-500');
                if (value === 5) e.target.classList.add('border-green-500');

                currentQuestionIndex++;
                setTimeout(renderQuestion, 300); // 300ms sonra yeni soruyu göster
            }
        });
        
        document.getElementById('submitSurvey').addEventListener('click', async () => {
            if (answers.length < currentQuestions.length) {
                showModal('<p class="text-lg font-bold text-red-600 mb-2">Hata!</p><p>Lütfen tüm soruları yanıtlayın.</p><button onclick="hideModal()" class="mt-4 px-4 py-2 bg-blue-600 text-white rounded">Tamam</button>');
                return;
            }
            
            stopTimer();

            showModal('<div class="flex flex-col items-center"><svg class="animate-spin h-8 w-8 text-blue-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path></svg><p class="mt-4 text-center">Anket sonuçlarınız gönderiliyor...</p></div>');
            
            try {
                const binData = await fetchBinData();
                
                const firstName = document.getElementById('firstName').value.trim() || 'Anonim';
                const lastName = document.getElementById('lastName').value.trim() || '';
                const companyName = document.getElementById('companyName').value.trim();

                const newSurvey = {
                    id: Date.now(),
                    companyName: companyName,
                    jobType: selectedJobType,
                    firstName: firstName,
                    lastName: lastName,
                    questions: currentQuestions,
                    answers: answers,
                    surveyDuration: Math.floor((new Date() - surveyStartTime) / 1000), // saniye cinsinden
                    submittedAt: new Date().toISOString()
                };

                binData.surveys.push(newSurvey);
                await updateBinData(binData);

                showModal('<p class="text-lg font-bold text-green-600 mb-2">Başarılı!</p><p>Anket yanıtlarınız başarıyla kaydedildi. Katılımınız için teşekkür ederiz.</p><button onclick="window.location.reload()" class="mt-4 px-4 py-2 bg-blue-600 text-white rounded">Anasayfaya Dön</button>');
            } catch (error) {
                showModal('<p class="text-lg font-bold text-red-600 mb-2">Hata!</p><p>Anket gönderilirken bir hata oluştu. Lütfen tekrar deneyin. Sorun devam ederse, bir yöneticiye danışın.</p><button onclick="hideModal()" class="mt-4 px-4 py-2 bg-blue-600 text-white rounded">Tamam</button>');
            }
        });

        function updateProgressBar() {
            const progress = (currentQuestionIndex / currentQuestions.length) * 100;
            document.getElementById('progressBar').style.width = `${progress}%`;
        }

        function updateProgressText() {
            const progressText = document.getElementById('progressText');
            progressText.textContent = `Anket İlerlemesi ${currentQuestionIndex}/${currentQuestions.length} Yanıtlandı`;
        }

        function startTimer() {
            surveyStartTime = new Date();
            timerInterval = setInterval(updateTimer, 1000);
        }

        function stopTimer() {
            clearInterval(timerInterval);
        }

        function updateTimer() {
            const now = new Date();
            const elapsed = Math.floor((now - surveyStartTime) / 1000);
            const minutes = Math.floor(elapsed / 60);
            const seconds = elapsed % 60;
            document.getElementById('timeElapsed').textContent = `Süre: ${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
        }
        
        // Şirket Portalı
        document.getElementById('companyModule').addEventListener('click', async () => {
            if (loggedInCompany) {
                await loadCompanyDashboard();
            }
        });

        async function loginCompany() {
            const name = document.getElementById('companyLoginName').value.trim();
            const password = document.getElementById('companyPassword').value.trim();

            if (!name || !password) {
                showModal('<p class="text-lg font-bold text-red-600 mb-2">Hata!</p><p>Lütfen kurum adı ve şifre girin.</p><button onclick="hideModal()" class="mt-4 px-4 py-2 bg-blue-600 text-white rounded">Tamam</button>');
                return;
            }

            showModal('<div class="flex flex-col items-center"><svg class="animate-spin h-8 w-8 text-blue-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path></svg><p class="mt-4 text-center">Giriş yapılıyor...</p></div>');
            
            try {
                const binData = await fetchBinData();
                const company = binData.companies.find(c => c.name === name && c.password === password);

                hideModal();

                if (company) {
                    loggedInCompany = company;
                    document.getElementById('companyLogin').style.display = 'none';
                    document.getElementById('companyDashboard').style.display = 'block';
                    document.getElementById('companyNameDisplay').textContent = `${company.name} Raporu`;
                    await loadCompanyDashboard();
                } else {
                    showModal('<p class="text-lg font-bold text-red-600 mb-2">Hata!</p><p>Kurum adı veya şifre yanlış.</p><button onclick="hideModal()" class="mt-4 px-4 py-2 bg-blue-600 text-white rounded">Tamam</button>');
                }
            } catch (error) {
                hideModal();
                showModal('<p class="text-lg font-bold text-red-600 mb-2">Hata!</p><p>Giriş yapılırken bir hata oluştu. Lütfen tekrar deneyin.</p><button onclick="hideModal()" class="mt-4 px-4 py-2 bg-blue-600 text-white rounded">Tamam</button>');
            }
        }

        function logoutCompany() {
            loggedInCompany = null;
            document.getElementById('companyLogin').style.display = 'block';
            document.getElementById('companyDashboard').style.display = 'none';
            document.getElementById('companyLoginName').value = '';
            document.getElementById('companyPassword').value = '';
        }
        
        async function loadCompanyDashboard() {
            if (!loggedInCompany) return;

            const binData = await fetchBinData();
            const companySurveys = binData.surveys.filter(s => s.companyName === loggedInCompany.name);
            const totalParticipants = companySurveys.length;
            const totalScore = companySurveys.reduce((sum, s) => sum + s.answers.reduce((qSum, ans) => qSum + ans, 0), 0);
            const totalQuestions = companySurveys.length > 0 ? companySurveys[0].questions.length : 0;
            const averageScore = totalParticipants > 0 ? (totalScore / (totalParticipants * totalQuestions)).toFixed(1) : '0.0';
            const satisfactionRate = totalParticipants > 0 ? ((companySurveys.filter(s => s.answers.reduce((qSum, ans) => qSum + ans, 0) / totalQuestions >= 3).length / totalParticipants) * 100).toFixed(0) : '0';

            document.getElementById('totalParticipants').textContent = totalParticipants;
            document.getElementById('averageScore').textContent = averageScore;
            document.getElementById('satisfactionRate').textContent = `${satisfactionRate}%`;

            renderCharts(companySurveys);
            renderParticipantDetails(companySurveys);
            renderDetailedReports(companySurveys);
        }

        function renderCharts(surveys) {
            const positionCounts = {};
            const satisfactionCounts = { 'Yetersiz': 0, 'Orta': 0, 'İyi': 0, 'Çok İyi': 0, 'Mükemmel': 0 };
            const timeCounts = { '0-5 dk': 0, '5-10 dk': 0, '10-15 dk': 0, '15+ dk': 0 };
            const scoreCounts = {};

            surveys.forEach(survey => {
                // Position Chart
                positionCounts[survey.jobType] = (positionCounts[survey.jobType] || 0) + 1;

                // Satisfaction Chart
                const avgScore = survey.answers.reduce((sum, ans) => sum + ans, 0) / survey.answers.length;
                if (avgScore < 2.5) satisfactionCounts['Yetersiz']++;
                else if (avgScore < 3.5) satisfactionCounts['Orta']++;
                else if (avgScore < 4.0) satisfactionCounts['İyi']++;
                else if (avgScore < 4.5) satisfactionCounts['Çok İyi']++;
                else satisfactionCounts['Mükemmel']++;
                
                // Time Chart
                if (survey.surveyDuration <= 300) timeCounts['0-5 dk']++;
                else if (survey.surveyDuration <= 600) timeCounts['5-10 dk']++;
                else if (survey.surveyDuration <= 900) timeCounts['10-15 dk']++;
                else timeCounts['15+ dk']++;

                // Score Chart
                const totalScore = survey.answers.reduce((sum, ans) => sum + ans, 0);
                scoreCounts[totalScore] = (scoreCounts[totalScore] || 0) + 1;
            });
            
            // Clear existing charts
            const charts = ['positionChart', 'satisfactionChart', 'timeChart', 'trendChart'];
            charts.forEach(id => {
                const canvas = document.getElementById(id);
                if (canvas) {
                    const chart = Chart.getChart(canvas);
                    if (chart) chart.destroy();
                }
            });

            // Position Chart
            new Chart(document.getElementById('positionChart'), {
                type: 'pie',
                data: {
                    labels: Object.keys(positionCounts),
                    datasets: [{
                        data: Object.values(positionCounts),
                        backgroundColor: ['#3B82F6', '#10B981', '#6366F1'],
                        hoverOffset: 4
                    }]
                }
            });

            // Satisfaction Chart
            new Chart(document.getElementById('satisfactionChart'), {
                type: 'doughnut',
                data: {
                    labels: Object.keys(satisfactionCounts),
                    datasets: [{
                        data: Object.values(satisfactionCounts),
                        backgroundColor: ['#EF4444', '#F97316', '#FACC15', '#3B82F6', '#10B981'],
                        hoverOffset: 4
                    }]
                }
            });

            // Time Chart
            new Chart(document.getElementById('timeChart'), {
                type: 'bar',
                data: {
                    labels: Object.keys(timeCounts),
                    datasets: [{
                        label: 'Katılımcı Sayısı',
                        data: Object.values(timeCounts),
                        backgroundColor: '#6366F1',
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    scales: {
                        y: { beginAtZero: true }
                    }
                }
            });

            // Score Chart
            const sortedScores = Object.keys(scoreCounts).map(Number).sort((a,b) => a-b);
            new Chart(document.getElementById('trendChart'), {
                type: 'line',
                data: {
                    labels: sortedScores,
                    datasets: [{
                        label: 'Puan Dağılımı',
                        data: sortedScores.map(score => scoreCounts[score]),
                        borderColor: '#9333ea',
                        backgroundColor: 'rgba(147, 51, 234, 0.2)',
                        fill: true,
                        tension: 0.4
                    }]
                },
                options: {
                    responsive: true,
                    scales: {
                        y: { beginAtZero: true }
                    }
                }
            });
        }
        
        function renderParticipantDetails(surveys) {
            const tbody = document.getElementById('participantTableBody');
            tbody.innerHTML = surveys.map(survey => {
                const avgScore = (survey.answers.reduce((sum, ans) => sum + ans, 0) / survey.answers.length).toFixed(1);
                let evaluation = 'Yetersiz';
                let evaluationColor = 'text-red-600';

                if (avgScore >= 4.5) { evaluation = 'Mükemmel'; evaluationColor = 'text-green-600'; }
                else if (avgScore >= 4.0) { evaluation = 'Çok İyi'; evaluationColor = 'text-blue-600'; }
                else if (avgScore >= 3.5) { evaluation = 'İyi'; evaluationColor = 'text-purple-600'; }
                else if (avgScore >= 2.5) { evaluation = 'Orta'; evaluationColor = 'text-orange-600'; }

                return `
                    <tr class="border-b hover:bg-gray-50 cursor-pointer" onclick="showDetailedReport(${survey.id})">
                        <td class="px-3 py-2 text-left">${survey.firstName} ${survey.lastName}</td>
                        <td class="px-3 py-2 text-left">${survey.jobType}</td>
                        <td class="px-3 py-2 text-center font-semibold">${avgScore}</td>
                        <td class="px-3 py-2 text-center ${evaluationColor} font-semibold">${evaluation}</td>
                        <td class="px-3 py-2 text-center text-sm">${new Date(survey.submittedAt).toLocaleDateString('tr-TR')}</td>
                    </tr>
                `;
            }).join('');
        }
        
        function renderDetailedReports(surveys) {
            // Soru başlıklarını al
            const questionText = surveys.length > 0 ? surveys[0].questions : [];
            const container = document.getElementById('detailedReport');
            container.innerHTML = `
                <div class="bg-white border rounded-lg p-6 mb-6">
                    <h4 class="text-xl font-semibold mb-4">Soru Bazlı Ortalama Yanıtlar</h4>
                    <div id="questionAverages" class="space-y-4"></div>
                </div>
            `;
            const averagesContainer = document.getElementById('questionAverages');
            
            questionText.forEach((q, index) => {
                const questionScores = surveys.map(s => s.answers[index]);
                const validScores = questionScores.filter(s => !isNaN(s));
                const average = validScores.length > 0 ? (validScores.reduce((sum, score) => sum + score, 0) / validScores.length).toFixed(2) : 'N/A';
                
                let barColor = 'bg-gray-300';
                if (average >= 4.5) barColor = 'bg-green-500';
                else if (average >= 3.5) barColor = 'bg-blue-500';
                else if (average >= 2.5) barColor = 'bg-orange-500';
                else if (average < 2.5) barColor = 'bg-red-500';

                averagesContainer.innerHTML += `
                    <div class="border-b pb-4">
                        <p class="font-medium text-sm text-gray-700 mb-2">${index + 1}. ${q}</p>
                        <div class="flex items-center space-x-2">
                            <div class="w-full bg-gray-200 rounded-full h-2.5">
                                <div class="${barColor} h-2.5 rounded-full" style="width: ${((average / 5) * 100).toFixed(0)}%"></div>
                            </div>
                            <span class="text-sm font-bold">${average}</span>
                        </div>
                    </div>
                `;
            });
        }
        
        function showDetailedReport(id) {
            // Bu fonksiyon PDF raporu oluşturmak için gerekli veriyi toplayacak
            // Şu an için demo amaçlı boş bırakıldı
            alert(`Katılımcı ID: ${id} için detaylı rapor gösterimi.`);
        }
        
        function toggleParticipantDetails() {
            const detailsDiv = document.getElementById('participantDetails');
            const button = document.getElementById('toggleParticipantsBtn');
            if (detailsDiv.classList.contains('hidden')) {
                detailsDiv.classList.remove('hidden');
                button.textContent = 'Katılımcıları Gizle';
            } else {
                detailsDiv.classList.add('hidden');
                button.textContent = 'Katılımcıları Görüntüle';
            }
        }
        
        // Yönetici Modülü
        async function loginAdmin() {
            const password = document.getElementById('adminPassword').value.trim();

            if (password !== 'AkcaProXAdmin2025') {
                showModal('<p class="text-lg font-bold text-red-600 mb-2">Hata!</p><p>Yönetici şifresi yanlış.</p><button onclick="hideModal()" class="mt-4 px-4 py-2 bg-blue-600 text-white rounded">Tamam</button>');
                return;
            }
            
            isAdminLoggedIn = true;
            document.getElementById('adminLogin').style.display = 'none';
            document.getElementById('adminDashboard').style.display = 'block';
            
            await loadAdminDashboard();
        }
        
        function logoutAdmin() {
            isAdminLoggedIn = false;
            document.getElementById('adminLogin').style.display = 'block';
            document.getElementById('adminDashboard').style.display = 'none';
        }
        
        async function loadAdminDashboard() {
            const binData = await fetchBinData();
            
            const totalCompanies = binData.companies.length;
            const totalUsers = binData.surveys.length;
            
            document.getElementById('totalCompanies').textContent = totalCompanies;
            document.getElementById('activeSurveys').textContent = totalCompanies; // Her şirketin bir anketi var varsayımıyla
            document.getElementById('totalUsers').textContent = totalUsers;
            
            renderCompanyList(binData.companies);
        }
        
        function renderCompanyList(companies) {
            const tbody = document.getElementById('companyList');
            tbody.innerHTML = companies.map(company => {
                const participantCount = binData.surveys.filter(s => s.companyName === company.name).length;
                const status = 'Aktif'; // Her zaman aktif varsayımıyla
                return `
                    <tr class="border-b">
                        <td class="px-4 py-3">${company.name}</td>
                        <td class="px-4 py-3 text-sm text-gray-500">${company.password}</td>
                        <td class="px-4 py-3 text-center">${participantCount}</td>
                        <td class="px-4 py-3 text-green-600 font-semibold">${status}</td>
                        <td class="px-4 py-3 space-x-2">
                            <button onclick="deleteCompany('${company.name}')" class="px-4 py-2 bg-red-600 text-white text-sm rounded hover:bg-red-700 transition-colors">Sil</button>
                        </td>
                    </tr>
                `;
            }).join('');
        }
        
        async function deleteCompany(companyName) {
            const confirmDelete = confirm(`${companyName} adlı kurumu ve tüm verilerini silmek istediğinize emin misiniz? Bu işlem geri alınamaz.`);
            if (!confirmDelete) return;
            
            showModal('<div class="flex flex-col items-center"><svg class="animate-spin h-8 w-8 text-red-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path></svg><p class="mt-4 text-center">Siliniyor...</p></div>');
            
            try {
                const binData = await fetchBinData();
                binData.companies = binData.companies.filter(c => c.name !== companyName);
                binData.surveys = binData.surveys.filter(s => s.companyName !== companyName);
                
                await updateBinData(binData);
                
                hideModal();
                showModal('<p class="text-lg font-bold text-green-600 mb-2">Başarılı!</p><p>Kurum ve verileri başarıyla silindi.</p><button onclick="window.location.reload()" class="mt-4 px-4 py-2 bg-blue-600 text-white rounded">Sayfayı Yenile</button>');
            } catch (error) {
                hideModal();
                showModal('<p class="text-lg font-bold text-red-600 mb-2">Hata!</p><p>Silme işlemi sırasında bir hata oluştu.</p><button onclick="hideModal()" class="mt-4 px-4 py-2 bg-blue-600 text-white rounded">Tamam</button>');
            }
        }
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'981af265f22bd620',t:'MTc1ODMwNDQ1MS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c)}}(window));</script>
</body>
</html>
