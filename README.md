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
    <!-- Ana Navigasyon -->
    <nav class="gradient-bg text-white p-3 shadow-lg">
        <div class="max-w-4xl mx-auto flex justify-between items-center">
            <div class="flex items-center gap-2">
                <!-- Gizli yönetici erişimi -->
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

    <!-- Anket Modülü -->
    <div id="surveyModule" class="max-w-4xl mx-auto p-4">
        <div class="bg-white shadow-xl rounded-xl max-w-xl mx-auto p-6">
            <div class="text-center mb-6">
                <h2 class="text-xl font-bold text-gray-800 mb-1">Kurum Değerlendirme Anketi</h2>
                <p class="text-gray-600 mb-2 text-sm">Görüşleriniz bizim için değerli</p>
                <span class="bg-green-100 text-green-800 text-xs font-semibold px-2 py-1 rounded-full">v3.0.0 - JSONBin.io Entegre</span>
            </div>

            <!-- Sorumluluk Reddi -->
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

            <!-- Şirket Bilgileri -->
            <div id="companyInfoSection">
                <div class="flex flex-col items-center justify-center gap-4 p-2 sm:p-6">
                    <h3 class="text-xl sm:text-2xl font-bold text-gray-800 mb-2 text-center">Kurum ve Kişisel Bilgiler</h3>
                    <div class="w-full max-w-xs sm:max-w-md mx-auto">
                        <input type="text" id="companyName" placeholder="Kurum adınızı girin (Okul, Üniversite vb.)" 
                            class="w-full border-2 border-purple-300 rounded-xl px-4 py-3 text-base focus:ring-2 focus:ring-purple-500 focus:border-purple-500 shadow-sm mb-3">
                    </div>
                    <div class="w-full max-w-xs sm:max-w-md mx-auto">
                        <p class="text-xs text-gray-600 mb-2 font-medium">Rolünüzü seçin:</p>
                        <div class="grid grid-cols-1 sm:grid-cols-3 gap-3">
                            <button type="button" onclick="selectJobType('Öğrenci')" id="studentBtn" 
                                class="job-btn flex flex-col items-center py-4 px-2 text-base rounded-xl border-2 border-blue-300 hover:border-blue-500 hover:bg-blue-50 transition-all duration-200 cursor-pointer font-semibold bg-white text-center focus:outline-none focus:ring-2 focus:ring-blue-400 shadow-sm">
                                <div class="text-2xl mb-1">🎓</div>
                                <div>Öğrenci</div>
                            </button>
                            <button type="button" onclick="selectJobType('Öğretmen')" id="teacherBtn" 
                                class="job-btn flex flex-col items-center py-4 px-2 text-base rounded-xl border-2 border-green-300 hover:border-green-500 hover:bg-green-50 transition-all duration-200 cursor-pointer font-semibold bg-white text-center focus:outline-none focus:ring-2 focus:ring-green-400 shadow-sm">
                                <div class="text-2xl mb-1">👨‍🏫</div>
                                <div>Öğretmen</div>
                            </button>
                            <button type="button" onclick="selectJobType('Veli/Ebeveyn')" id="parentBtn" 
                                class="job-btn flex flex-col items-center py-4 px-2 text-base rounded-xl border-2 border-purple-300 hover:border-purple-500 hover:bg-purple-50 transition-all duration-200 cursor-pointer font-semibold bg-white text-center focus:outline-none focus:ring-2 focus:ring-purple-400 shadow-sm">
                                <div class="text-2xl mb-1">👨‍👩‍👧‍👦</div>
                                <div>Veli/Ebeveyn</div>
                            </button>
                        </div>
                    </div>
                    <div id="selectedJobDisplay" class="text-center text-base text-blue-600 font-semibold min-h-[24px] mb-2"></div>
                    <div class="w-full max-w-xs sm:max-w-md mx-auto grid grid-cols-1 sm:grid-cols-2 gap-3 mb-4">
                        <input type="text" id="firstName" placeholder="Adınız (İsteğe bağlı)" 
                            class="border-2 border-gray-300 rounded-xl px-4 py-3 text-base focus:ring-2 focus:ring-purple-500 focus:border-purple-500 shadow-sm">
                        <input type="text" id="lastName" placeholder="Soyadınız (İsteğe bağlı)" 
                            class="border-2 border-gray-300 rounded-xl px-4 py-3 text-base focus:ring-2 focus:ring-purple-500 focus:border-purple-500 shadow-sm">
                    </div>
                    <button id="startSurvey" class="w-full max-w-xs sm:max-w-md mx-auto py-4 rounded-xl text-white font-bold gradient-bg hover:opacity-90 transition-opacity text-lg shadow-lg">
                        📊 Anketi Başlat
                    </button>
                </div>
            </div>

            <!-- Anket Alanı -->
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

    <!-- Şirket Portalı -->
    <div id="companyModule" class="max-w-4xl mx-auto p-4 hidden">
        <div class="bg-white shadow-xl rounded-xl max-w-4xl mx-auto p-6">
            <div id="companyLogin" class="max-w-md mx-auto">
                <h2 class="text-3xl font-bold text-center mb-8">🏫 Kurum Portalı Girişi</h2>
                <div class="space-y-6">
                    <input type="text" id="companyLoginName" placeholder="Okul/Kurum Adı" 
                           class="w-full border-2 border-gray-300 rounded-lg px-4 py-4 text-base focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                    <input type="password" id="companyPassword" placeholder="12 Karakterlik Şifre" 
                           class="w-full border-2 border-gray-300 rounded-lg px-4 py-4 text-base focus:ring-2 focus:ring-blue-500 focus:border-blue-500" autocomplete="off">
                    <button onclick="loginCompany()" class="w-full py-4 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors text-lg font-semibold">
                        🔐 Giriş Yap
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
                        <h3 class="text-lg font-semibold mb-2">Toplam Katılımcı</h3>
                        <p class="text-4xl font-bold" id="totalParticipants">0</p>
                    </div>
                    <div class="bg-gradient-to-r from-green-500 to-green-600 text-white p-6 rounded-lg">
                        <h3 class="text-lg font-semibold mb-2">Ortalama Puan</h3>
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
                            📄 PDF Göster
                        </button>
                    </div>
                    
                    <!-- Grafikler Bölümü -->
                    <div class="grid grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-3 text-sm">📊 Pozisyon</h4>
                            <div style="height: 150px; position: relative;">
                                <canvas id="positionChart"></canvas>
                            </div>
                        </div>
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-3 text-sm">📈 Değerlendirme</h4>
                            <div style="height: 150px; position: relative;">
                                <canvas id="satisfactionChart"></canvas>
                            </div>
                        </div>
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-3 text-sm">⏰ Süre Dağılımı</h4>
                            <div style="height: 150px; position: relative;">
                                <canvas id="timeChart"></canvas>
                            </div>
                        </div>
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-3 text-sm">🎯 Puan Dağılımı</h4>
                            <div style="height: 150px; position: relative;">
                                <canvas id="trendChart"></canvas>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Katılımcı Detayları Bölümü -->
                    <div class="bg-white border rounded-lg p-4 mb-6">
                        <div class="flex justify-between items-center mb-4">
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
                                        <!-- Katılımcı listesi buraya yüklenecek -->
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                    
                    <div id="detailedReport" class="space-y-4"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- Yönetici Portalı -->
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

        // JSONBin.io konfigürasyonu
        const JSONBIN_CONFIG = {
            apiKey: '$2a$10$Vre/Nl1Aa1vrK2xY1NHYguabG45SOU1sMt3dnh.UJYpdBoQSdnz1.',
            accessKey: '$2a$10$SCDSdHz/rW/Z3Q6EWaB68uSJR2GAhE3pjG/i3.gJEhKsviO.yl6DC',
            binId: '68ce6bf543b1c97be9491ab1',
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
                "Okulun rehberlik servisi, çocuğumun akademik ve duygusal gelişimini destekliyor 💚",
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
                "Okulun, çocuğumun dışarıda güvenli vakit geçirebileceği alanları var 🌳",
                "Okulun ulaşım imkanları yeterli ve güvenli 🚌",
                "Okulun, öğrenciler için sağlıklı bir beslenme politikası olduğuna inanıyorum 🥗",
                "Okulun kütüphanesi ve diğer kaynakları, çocuğumun derslerine yardımcı oluyor 📚",
                "Okul, öğrencilerinin sağlığını korumak için gerekli tüm önlemleri alıyor 🏥",
                "Okulun, çocuğumun hobilerini ve ilgi alanlarını desteklediğini düşünüyorum 🎨",
                
                // Eğitimde Teknoloji ve Gelecek (10 Soru)
                "Okulun dijital eğitim platformu, çocuğumun öğrenme sürecini kolaylaştırıyor 💻",
                "Okulun, eğitimde teknolojik yenilikleri benimsediğini düşünüyorum 🚀",
                "Okulun web sitesi ve mobil uygulaması, ihtiyaç duyduğum bilgilere kolayca ulaşmamı sağlıyor 📱",
                "Okul, dijital dünyada güvenliği sağlamak için yeterli önlemleri alıyor 🔒",
                "Okulun, geleceğin mesleklerine uygun beceriler kazandırmak için çalıştığını düşünüyorum 🔮",
                "Okul, online eğitim ve veli toplantıları gibi dijital çözümleri etkili bir şekilde kullanıyor 🌐",
                "Çocuğumun, dijital okuryazarlığını geliştirmesi için okulun yeterli destek sağladığına inanıyorum 📖",
                "Okulun, sürekli olarak kendini yenileme çabalarını takdir ediyorum 🔄",
                "Okulun, geleceğin eğitim trendlerine uyum sağladığını düşünüyorum 📈",
                "Okulun dijital vizyonunun, çocuğumun eğitimine olumlu katkı sağladığına inanıyorum ✨"
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
            console.log('Seçilen rol:', jobType);
            
            // Tüm butonları sıfırla
            const allButtons = document.querySelectorAll('.job-btn');
            allButtons.forEach(btn => {
                btn.classList.remove('selected-job');
                btn.style.border = '';
                btn.style.backgroundColor = '';
                btn.style.color = '';
                btn.style.fontWeight = '';
                btn.style.transform = '';
                btn.style.boxShadow = '';
            });
            
            // Seçili butonu işaretle
            const buttonMap = {
                'Öğrenci': 'studentBtn',
                'Öğretmen': 'teacherBtn', 
                'Veli/Ebeveyn': 'parentBtn'
            };
            
            const selectedButton = document.getElementById(buttonMap[jobType]);
            if (selectedButton) {
                selectedButton.style.border = '3px solid #3b82f6';
                selectedButton.style.backgroundColor = '#3b82f6';
                selectedButton.style.color = 'white';
                selectedButton.style.fontWeight = 'bold';
                selectedButton.style.transform = 'scale(1.05)';
                selectedButton.style.boxShadow = '0 4px 12px rgba(59, 130, 246, 0.4)';
                selectedButton.classList.add('selected-job');
            }
            
            // Seçimi göster
            const displayElement = document.getElementById('selectedJobDisplay');
            if (displayElement) {
                displayElement.innerHTML = `<span class="text-blue-600 font-semibold text-lg">✓ Seçilen rol: ${jobType}</span>`;
            }
        }

        function startSurvey() {
            console.log('Anket başlatma fonksiyonu çalışıyor...');
            
            const companyName = document.getElementById('companyName').value.trim();
            const disclaimerAccepted = document.getElementById('acceptDisclaimer').checked;
            
            console.log('Form verileri:', { companyName, selectedJobType, disclaimerAccepted });
            
            if (!disclaimerAccepted) {
                showModal('⚠️ Uyarı', 'Devam etmek için veri koruma beyanını kabul etmelisiniz.');
                return;
            }
            
            if (!companyName) {
                showModal('⚠️ Eksik Bilgi', 'Lütfen kurum adını girin.');
                return;
            }
            
            if (!selectedJobType) {
                showModal('⚠️ Eksik Bilgi', 'Lütfen rolünüzü seçin (Öğrenci, Öğretmen veya Veli/Ebeveyn).');
                return;
            }
            
            // Seçilen role göre soruları al
            currentQuestions = questions[selectedJobType];
            console.log('Seçilen rol:', selectedJobType);
            console.log('Sorular:', currentQuestions);
            
            if (!currentQuestions || currentQuestions.length === 0) {
                showModal('❌ Hata', 'Seçilen rol için sorular bulunamadı. Lütfen sayfayı yenileyip tekrar deneyin.');
                return;
            }
            
            // Değişkenleri sıfırla
            currentQuestionIndex = 0;
            answers = [];
            surveyStartTime = new Date();
            
            // Anket bölümünü göster
            document.getElementById('disclaimerSection').classList.add('hidden');
            document.getElementById('companyInfoSection').classList.add('hidden');
            document.getElementById('surveySection').classList.remove('hidden');
            
            startTimer();
            displayCurrentQuestion();
            
            console.log('Anket başarıyla başlatıldı!');
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
                <div class="flex flex-col items-center justify-center bg-gradient-to-br from-green-100 to-green-50 p-8 sm:p-12 rounded-2xl border-2 border-green-300 shadow-xl">
                    <div class="text-7xl sm:text-8xl mb-4 animate-bounce">🎉</div>
                    <h3 class="text-2xl sm:text-3xl font-bold text-green-800 mb-2 text-center">Tebrikler!</h3>
                    <p class="text-green-700 mb-4 text-lg sm:text-xl text-center font-medium">Tüm soruları yanıtladınız.<br>Anketi tamamlamak için aşağıdaki butona tıklayın.</p>
                    <div class="text-base text-green-700 font-semibold mb-2">Toplam süre: ${document.getElementById('timeElapsed').textContent.split(': ')[1]}</div>
                </div>
            `;
            document.getElementById('submitSurvey').classList.remove('hidden');
            updateProgress();
        }

        // JSONBin.io API fonksiyonları
        async function createNewBin() {
            throw new Error('Sabit binId ile çalışıyor, yeni bin oluşturulamaz.');
        }

        async function loadFromJSONBin() {
            try {
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
                if (response.ok) {
                    const data = await response.json();
                    systemData.surveyData = data.record || data;
                    return systemData.surveyData;
                } else {
                    throw new Error(`API Hatası: ${response.status}`);
                }
            } catch (error) {
                console.error('JSONBin yükleme hatası:', error);
                const defaultData = {
                    surveyName: "Kurum Değerlendirme Anketi - Sürüm 12",
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
                if (!JSONBIN_CONFIG.binId) {
                    throw new Error('Sabit binId tanımlı değil!');
                }
                console.log(`JSONBin'e veri kaydediliyor... Bin ID: ${JSONBIN_CONFIG.binId}`);
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
                if (response.ok) {
                    const result = await response.json();
                    console.log('JSONBin kaydetme başarılı:', result);
                    return { success: true, data: result };
                } else {
                    const errorText = await response.text();
                    console.error('JSONBin API hatası:', response.status, errorText);
                    if (retryCount < JSONBIN_CONFIG.maxRetries && (response.status >= 500 || response.status === 429)) {
                        console.log(`${JSONBIN_CONFIG.retryDelay}ms sonra yeniden denenecek...`);
                        await new Promise(resolve => setTimeout(resolve, JSONBIN_CONFIG.retryDelay * (retryCount + 1)));
                        return await saveToJSONBin(data, retryCount + 1);
                    }
                    return { success: false, error: `API Hatası: ${response.status} - ${errorText}` };
                }
            } catch (error) {
                console.error('JSONBin bağlantı hatası:', error);
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
                console.log('Kurum kontrol ediliyor:', companyName);
                
                if (!systemData.surveyData) {
                    systemData.surveyData = await loadFromJSONBin();
                }
                
                const existingCompany = Object.entries(systemData.surveyData.companies || {})
                    .find(([key, company]) => company.name.toLowerCase() === companyName.toLowerCase());
                
                if (existingCompany) {
                    console.log('Mevcut kurum bulundu:', existingCompany[1]);
                    return { success: true, key: existingCompany[0], password: existingCompany[1].password };
                }
                
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
                
                const saveResult = await saveToJSONBin(systemData.surveyData);
                if (saveResult.success) {
                    return { success: true, key: companyKey, password: newPassword };
                } else {
                    return { success: false, error: saveResult.error };
                }
                
            } catch (error) {
                console.error('Kurum oluşturma hatası:', error);
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
                    throw new Error('Eksik bilgi: Kurum adı, iş türü ve anket yanıtları gerekli');
                }
                
                const companyResult = await createCompanyIfNotExists(companyName);
                
                if (!companyResult.success) {
                    throw new Error(`Kurum işlemi başarısız: ${companyResult.error}`);
                }
                
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
                
                if (!systemData.surveyData.responses) {
                    systemData.surveyData.responses = [];
                }
                systemData.surveyData.responses.push(surveyResponse);
                
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
                
                if (companyResult && systemData.surveyData.companies[companyResult.key]) {
                    systemData.surveyData.companies[companyResult.key].totalResponses = 
                        systemData.surveyData.responses.filter(r => 
                            r.companyName.toLowerCase() === companyName.toLowerCase()
                        ).length;
                }
                
                const saveResult = await saveToJSONBin(systemData.surveyData);
                
                if (saveResult.success) {
                    document.getElementById('surveySection').innerHTML = `
                        <div class="text-center bg-green-50 p-10 rounded-lg border-2 border-green-200">
                            <div class="text-8xl mb-6">✅</div>
                            <h2 class="text-3xl font-bold text-green-800 mb-6">Anketiniz Başarıyla Kaydedildi!</h2>
                            <p class="text-green-700 mb-6 text-lg">
                                Değerli görüşleriniz için teşekkür ederiz. Anket yanıtlarınız güvenli bir şekilde JSONBin.io sisteminde saklandı.
                            </p>
                            <div class="bg-blue-50 p-6 rounded-lg border border-blue-200 mb-6">
                                <p class="text-base text-blue-700">
                                    <strong>📊 Raporlama Bilgisi:</strong> Anket sonuçlarınız güvenli bir şekilde kaydedildi. 
                                    Kurum yöneticiniz raporları görüntüleyebilir ve analiz edebilir.
                                </p>
                            </div>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                <button onclick="showModule('company')" class="bg-blue-600 text-white px-6 py-3 rounded-lg hover:bg-blue-700 transition-colors text-lg font-semibold">
                                    🏫 Kurum Portalına Git
                                </button>
                                <button onclick="location.reload()" class="bg-purple-600 text-white px-6 py-3 rounded-lg hover:bg-purple-700 transition-colors text-lg font-semibold">
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
                showModal('⚠️ Eksik Bilgi', 'Lütfen kurum adı ve şifrenizi girin.');
                return;
            }
            
            try {
                if (!systemData.surveyData) {
                    systemData.surveyData = await loadFromJSONBin();
                }
                
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
                    showModal('❌ Giriş Hatası', 'Okul/kurum adı veya şifre hatalı. Lütfen yöneticinizden doğru bilgileri alın.');
                }
            } catch (error) {
                showModal('❌ Hata', 'Giriş sırasında bir hata oluştu. Lütfen tekrar deneyin.');
                console.error('Giriş hatası:', error);
            }
        }

        function loadCompanyDashboard() {
            if (!loggedInCompany || !systemData.surveyData) return;
            
            document.getElementById('companyNameDisplay').textContent = loggedInCompany.name;
            
            const companySurveys = systemData.surveyData.responses.filter(s => 
                s.companyName.toLowerCase() === loggedInCompany.name.toLowerCase()
            );
            
            document.getElementById('totalParticipants').textContent = companySurveys.length;
            
            if (companySurveys.length > 0) {
                let totalScore = 0;
                let totalAnswers = 0;
                companySurveys.forEach(s => {
                    totalScore += s.totalScore;
                    totalAnswers += s.answers.length;
                });
                const avgScore = totalAnswers > 0 ? (totalScore / totalAnswers).toFixed(1) : '0.0';
                document.getElementById('averageScore').textContent = avgScore;
                
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
                document.getElementById('detailedReport').innerHTML = '<p class="text-gray-500 text-center py-8 text-lg">Henüz anket verisi bulunmuyor.</p>';
                return;
            }
            
            const positionData = {};
            surveys.forEach(s => {
                positionData[s.jobType] = (positionData[s.jobType] || 0) + 1;
            });
            
            const satisfactionLevels = ['Düşük (1-2)', 'Orta (3)', 'Yüksek (4-5)'];
            const satisfactionCounts = [0, 0, 0];
            
            surveys.forEach(s => {
                const avgScore = parseFloat(s.averageScore);
                if (avgScore < 2.5) satisfactionCounts[0]++;
                else if (avgScore >= 2.5 && avgScore < 3.5) satisfactionCounts[1]++;
                else satisfactionCounts[2]++;
            });
            
            const report = `
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="bg-blue-50 p-6 rounded-lg">
                        <h4 class="font-semibold text-blue-800 mb-4 text-lg">👥 Pozisyon Dağılımı</h4>
                        ${Object.entries(positionData).map(([pos, count]) => 
                            `<div class="flex justify-between text-base mb-2">
                                <span>${pos}:</span>
                                <span class="font-semibold">${count} kişi</span>
                            </div>`
                        ).join('')}
                    </div>
                    
                    <div class="bg-green-50 p-6 rounded-lg">
                        <h4 class="font-semibold text-green-800 mb-4 text-lg">📊 Değerlendirme Seviyeleri</h4>
                        ${satisfactionLevels.map((level, i) => 
                            `<div class="flex justify-between text-base mb-2">
                                <span>${level}:</span>
                                <span class="font-semibold">${satisfactionCounts[i]} katılımcı</span>
                            </div>`
                        ).join('')}
                    </div>
                </div>
                
                <div class="mt-6 bg-gray-50 p-6 rounded-lg">
                    <h4 class="font-semibold text-gray-800 mb-3 text-lg">📈 Özet</h4>
                    <p class="text-base text-gray-700">
                        Toplam ${surveys.length} paydaş anketi tamamladı. 
                        Ortalama değerlendirme skoru ${(surveys.reduce((sum, s) => sum + parseFloat(s.averageScore), 0) / surveys.length).toFixed(1)}/5.0 olarak hesaplandı.
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
                
                document.getElementById('totalCompanies').textContent = Object.keys(companies).length;
                document.getElementById('activeSurveys').textContent = Object.keys(companies).length;
                document.getElementById('totalUsers').textContent = responses.length;
                
                loadCompanyList();
            } catch (error) {
                console.error('Admin dashboard yükleme hatası:', error);
                showModal('❌ Hata', 'Yönetici paneli yüklenirken hata oluştu.');
            }
        }

        function loadCompanyList() {
            const tbody = document.getElementById('companyList');
            
            if (!systemData.surveyData || !systemData.surveyData.companies) {
                tbody.innerHTML = '<tr><td colspan="5" class="text-center py-4 text-gray-500">Henüz kurum kaydı bulunmuyor.</td></tr>';
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
                        <td class="px-4 py-3 font-medium">${company.name}</td>
                        <td class="px-4 py-3">
                            <code class="bg-gray-100 px-2 py-1 rounded text-sm">${company.password}</code>
                        </td>
                        <td class="px-4 py-3">${companySurveys.length}</td>
                        <td class="px-4 py-3">
                            <span class="px-2 py-1 rounded-full text-xs bg-green-100 text-green-800">
                                🟢 Aktif
                            </span>
                        </td>
                        <td class="px-4 py-3">
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

        function showModal(title, content) {
            const modal = document.getElementById('modal');
            const modalContent = document.getElementById('modalContent');
            
            modalContent.innerHTML = `
                <h3 class="text-xl font-semibold mb-4">${title}</h3>
                <div class="mb-6 text-base">${content}</div>
                <button onclick="closeModal()" class="w-full bg-blue-600 text-white py-3 px-4 rounded-lg hover:bg-blue-700 font-semibold">
                    Tamam
                </button>
            `;
            
            modal.classList.add('show');
        }

        function closeModal() {
            document.getElementById('modal').classList.remove('show');
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

        function showPDFReport() {
            if (!loggedInCompany || !systemData.surveyData) return;
            
            const companySurveys = systemData.surveyData.responses.filter(s => 
                s.companyName.toLowerCase() === loggedInCompany.name.toLowerCase()
            );
            
            const pdfContent = generatePDFContent(companySurveys);
            
            const pdfWindow = window.open('', '_blank', 'width=800,height=600');
            pdfWindow.document.write(pdfContent);
            pdfWindow.document.close();
        }

        function generateAdminPDFContent(companyName, surveys) {
            const totalParticipants = surveys.length;
            
            let totalScore = 0;
            let totalAnswers = 0;
            surveys.forEach(s => {
                totalScore += s.totalScore;
                totalAnswers += s.answers.length;
            });
            const avgScore = totalAnswers > 0 ? (totalScore / totalAnswers).toFixed(1) : '0.0';
            
            // Profesyonel memnuniyet yüzdesi hesaplama (50-250 puan arası)
            // Formül: ((Alınan Puan - Minimum Puan) / (Maksimum Puan - Minimum Puan)) * 100
            const minPossibleScore = totalAnswers * 1; // Her soru minimum 1 puan
            const maxPossibleScore = totalAnswers * 5; // Her soru maksimum 5 puan
            const satisfactionPercentage = totalAnswers > 0 ? 
                Math.round(((totalScore - minPossibleScore) / (maxPossibleScore - minPossibleScore)) * 100) : 0;
            
            const positionData = {};
            const positionScores = {};
            surveys.forEach(s => {
                positionData[s.jobType] = (positionData[s.jobType] || 0) + 1;
                if (!positionScores[s.jobType]) positionScores[s.jobType] = [];
                positionScores[s.jobType].push(parseFloat(s.averageScore));
            });
            
            // Pozisyon bazlı memnuniyet yüzdeleri
            const positionSatisfaction = {};
            Object.keys(positionScores).forEach(pos => {
                const avgPosScore = positionScores[pos].reduce((a, b) => a + b, 0) / positionScores[pos].length;
                positionSatisfaction[pos] = Math.round(((avgPosScore - 1) / 4) * 100);
            });
            
            // Durum analizi
            let statusAnalysis = '';
            let recommendations = '';
            
            if (satisfactionPercentage <= 50) {
                statusAnalysis = 'Düşük Memnuniyet - Acil Müdahale Gerekli';
                recommendations = 'Acil bir eylem planı oluşturulmalıdır. Okulun fiziki koşulları ve temel iletişim kanalları gözden geçirilmelidir. Veliler, öğretmenler ve öğrencilerle düzenli toplantılar düzenlenerek çözüm süreçleri şeffaf bir şekilde paylaşılmalıdır.';
            } else if (satisfactionPercentage <= 75) {
                statusAnalysis = 'Orta Seviye Memnuniyet - İyileştirme Fırsatları';
                recommendations = 'Gelecek odaklı bir strateji belirlenmelidir. Okulun dijital dönüşüm stratejisi tüm paydaşlara net bir şekilde duyurulmalı ve bu alandaki yatırımlar artırılmalıdır. Öğretmenler için profesyonel gelişim programları hayata geçirilmelidir.';
            } else {
                statusAnalysis = 'Yüksek Memnuniyet - Sürdürülebilirlik Odaklı';
                recommendations = 'Bu başarıyı sürdürmek için düzenli nabız anketleri yapılmalı ve paydaşların beklentileri sürekli takip edilmelidir. En güçlü olduğunuz alanlarda bile sürekli iyileştirme hedefleri belirlenmelidir.';
            }
            
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
                        body { font-family: Arial, sans-serif; margin: 15px; line-height: 1.4; font-size: 12px; }
                        .header { text-align: center; border-bottom: 2px solid #333; padding-bottom: 15px; margin-bottom: 20px; }
                        .stats { display: flex; justify-content: space-between; margin-bottom: 20px; }
                        .stat-box { background: #f5f5f5; padding: 10px; border-radius: 5px; text-align: center; width: 30%; }
                        .stat-number { font-size: 1.5em; font-weight: bold; color: #333; }
                        .section { margin-bottom: 20px; }
                        .section h3 { color: #333; border-bottom: 1px solid #ddd; padding-bottom: 5px; margin-bottom: 10px; }
                        table { width: 100%; border-collapse: collapse; margin-top: 5px; font-size: 11px; }
                        th, td { border: 1px solid #ddd; padding: 5px; text-align: left; }
                        th { background-color: #f2f2f2; }
                        .analysis-box { background: #e8f4fd; padding: 10px; border-radius: 5px; margin: 10px 0; }
                        .recommendations { background: #fff3cd; padding: 10px; border-radius: 5px; margin: 10px 0; }
                        .chart-placeholder { width: 100%; height: 150px; background: #f8f9fa; border: 1px solid #ddd; display: flex; align-items: center; justify-content: center; margin: 10px 0; }
                        .footer { margin-top: 30px; text-align: center; font-size: 10px; color: #666; }
                    </style>
                </head>
                <body>
                    <div class="header">
                        <h1>📊 ${companyName}</h1>
                        <h2>Yönetici Kurum Değerlendirme Raporu</h2>
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
                            <div class="stat-number">${satisfactionPercentage}%</div>
                            <div>Genel Memnuniyet</div>
                        </div>
                    </div>
                    
                    <div class="analysis-box">
                        <h4>📈 Durum Analizi</h4>
                        <p><strong>${statusAnalysis}</strong></p>
                        <p>Genel memnuniyet oranı %${satisfactionPercentage} olarak hesaplanmıştır.</p>
                    </div>
                    
                    <div class="section">
                        <h3>👥 Pozisyon Bazlı Analiz</h3>
                        <table>
                            <tr><th>Pozisyon</th><th>Katılımcı</th><th>Memnuniyet %</th><th>Durum</th></tr>
                            ${Object.entries(positionData).map(([pos, count]) => {
                                const satisfaction = positionSatisfaction[pos] || 0;
                                const status = satisfaction <= 50 ? 'Düşük' : satisfaction <= 75 ? 'Orta' : 'Yüksek';
                                return `<tr><td>${pos}</td><td>${count}</td><td>%${satisfaction}</td><td>${status}</td></tr>`;
                            }).join('')}
                        </table>
                    </div>
                    
                    <div class="chart-placeholder">
                        <div style="text-align: center;">
                            <div style="font-size: 14px; margin-bottom: 10px;">📊 Pozisyon Dağılımı Grafiği</div>
                            ${Object.entries(positionData).map(([pos, count]) => 
                                `<div style="margin: 5px 0;">${pos}: ${count} kişi (${Math.round((count/totalParticipants)*100)}%)</div>`
                            ).join('')}
                        </div>
                    </div>
                    
                    <div class="section">
                        <h3>📈 Değerlendirme Seviyeleri</h3>
                        <table>
                            <tr><th>Seviye</th><th>Cevap Sayısı</th><th>Oran</th></tr>
                            <tr><td>Düşük (1-2)</td><td>${satisfactionCounts[0]}</td><td>${totalAnswers > 0 ? Math.round((satisfactionCounts[0]/totalAnswers)*100) : 0}%</td></tr>
                            <tr><td>Orta (3)</td><td>${satisfactionCounts[1]}</td><td>${totalAnswers > 0 ? Math.round((satisfactionCounts[1]/totalAnswers)*100) : 0}%</td></tr>
                            <tr><td>Yüksek (4-5)</td><td>${satisfactionCounts[2]}</td><td>${totalAnswers > 0 ? Math.round((satisfactionCounts[2]/totalAnswers)*100) : 0}%</td></tr>
                        </table>
                    </div>
                    
                    <div class="recommendations">
                        <h4>💡 Öneriler ve Eylem Planı</h4>
                        <p>${recommendations}</p>
                    </div>
                    
                    <div class="footer">
                        <p>Akça Pro X - Profesyonel Kurum Değerlendirme Sistemi | ${new Date().toLocaleString('tr-TR')}</p>
                    </div>
                </body>
                </html>
            `;
        }

        function generatePDFContent(surveys) {
            const companyName = loggedInCompany.name;
            const totalParticipants = surveys.length;
            
            let totalScore = 0;
            let totalAnswers = 0;
            surveys.forEach(s => {
                totalScore += s.totalScore;
                totalAnswers += s.answers.length;
            });
            const avgScore = totalAnswers > 0 ? (totalScore / totalAnswers).toFixed(1) : '0.0';
            
            // Profesyonel memnuniyet yüzdesi hesaplama (50-250 puan arası)
            // Formül: ((Alınan Puan - Minimum Puan) / (Maksimum Puan - Minimum Puan)) * 100
            const minPossibleScore = totalAnswers * 1; // Her soru minimum 1 puan
            const maxPossibleScore = totalAnswers * 5; // Her soru maksimum 5 puan
            const satisfactionPercentage = totalAnswers > 0 ? 
                Math.round(((totalScore - minPossibleScore) / (maxPossibleScore - minPossibleScore)) * 100) : 0;
            
            const positionData = {};
            const positionScores = {};
            surveys.forEach(s => {
                positionData[s.jobType] = (positionData[s.jobType] || 0) + 1;
                if (!positionScores[s.jobType]) positionScores[s.jobType] = [];
                positionScores[s.jobType].push(parseFloat(s.averageScore));
            });
            
            // Pozisyon bazlı memnuniyet yüzdeleri
            const positionSatisfaction = {};
            Object.keys(positionScores).forEach(pos => {
                const avgPosScore = positionScores[pos].reduce((a, b) => a + b, 0) / positionScores[pos].length;
                positionSatisfaction[pos] = Math.round(((avgPosScore - 1) / 4) * 100);
            });
            
            // Profesyonel durum analizi ve öneriler
            let statusAnalysis = '';
            let recommendations = '';
            let statusColor = '';
            let detailedAnalysis = '';
            
            if (satisfactionPercentage <= 50) {
                statusAnalysis = 'Düşük Memnuniyet (%0-50) - Acil Müdahale Gerekli';
                statusColor = '#dc3545';
                detailedAnalysis = 'Kurumunuz tüm paydaş gruplarında ciddi memnuniyetsizlikler yaşıyor. Bu, kurumun temel işleyişinde ve sunduğu hizmetlerde köklü sorunlar olduğuna işaret ediyor.';
                recommendations = 'Acil bir eylem planı oluşturulmalıdır. Okulun fiziki koşulları ve temel iletişim kanalları gözden geçirilmelidir. Veliler, öğretmenler ve öğrencilerle düzenli toplantılar düzenlenerek çözüm süreçleri şeffaf bir şekilde paylaşılmalıdır.';
            } else if (satisfactionPercentage <= 75) {
                statusAnalysis = 'Orta Seviye Memnuniyet (%51-75) - İyileştirme Fırsatları';
                statusColor = '#ffc107';
                detailedAnalysis = 'Kurumunuz genel olarak iyi bir performans sergiliyor, ancak mükemmeliyetten uzak. Paydaşlar genel hizmetlerden memnun olsa da, özellikle teknoloji kullanımı, sosyal gelişim ve kariyer rehberliği gibi alanlarda daha fazlasını bekliyorlar.';
                recommendations = 'Gelecek odaklı bir strateji belirlenmelidir. Okulun eğitim kalitesi ve öğrenci başarısına odaklanılmalı. Öğretmenler için profesyonel gelişim programları hayata geçirilmeli. Veli iletişimi güçlendirilmelidir.';
            } else {
                statusAnalysis = 'Yüksek Memnuniyet (%76-100) - Sürdürülebilirlik Odaklı';
                statusColor = '#28a745';
                detailedAnalysis = 'Kurumunuz tüm paydaş grupları arasında yüksek bir güven ve memnuniyet seviyesine sahip. Bu, okul yönetiminin güçlü bir vizyonu ve etkili bir iletişim stratejisi olduğunu gösteriyor. Kurum kültürünüz sağlam temeller üzerine kurulu.';
                recommendations = 'Bu başarıyı sürdürmek için düzenli nabız anketleri yapılmalı ve paydaşların beklentileri sürekli takip edilmelidir. En güçlü olduğunuz alanlarda bile sürekli iyileştirme hedefleri belirlenmelidir.';
            }
            
            // Paydaş grupları arası karşılaştırma ve ayrışma analizi
            let comparisonAnalysis = '';
            let specialScenarioAnalysis = '';
            const positions = Object.keys(positionSatisfaction);
            
            if (positions.length > 1) {
                const maxPos = positions.reduce((a, b) => positionSatisfaction[a] > positionSatisfaction[b] ? a : b);
                const minPos = positions.reduce((a, b) => positionSatisfaction[a] < positionSatisfaction[b] ? a : b);
                const diff = positionSatisfaction[maxPos] - positionSatisfaction[minPos];
                
                if (diff > 30) {
                    comparisonAnalysis = `Paydaş grupları arasında ciddi ayrışma tespit edildi. ${maxPos} grubu (%${positionSatisfaction[maxPos]}) en memnun, ${minPos} grubu (%${positionSatisfaction[minPos]}) en az memnun.`;
                    
                    // Özel senaryo kontrolü
                    if (positionSatisfaction['Öğrenci'] && positionSatisfaction['Öğrenci'] < 40 && 
                        positionSatisfaction['Öğretmen'] && positionSatisfaction['Öğretmen'] > 70) {
                        specialScenarioAnalysis = `Bu senaryo, okul içinde ciddi bir kopukluk olduğunu gösteriyor. Öğretmenler genel olarak memnunken, öğrenciler oldukça mutsuz. Acil olarak öğrencilerin sorunlarına odaklanılmalıdır. Okulun fiziksel olanakları ve rehberlik hizmetleri gözden geçirilmelidir. Öğretmenler ve öğrenciler arasında iletişim köprüsü kurulmalı.`;
                    }
                } else if (diff > 15) {
                    comparisonAnalysis = `Paydaş grupları arasında orta düzeyde fark var. ${maxPos} grubu (%${positionSatisfaction[maxPos]}) daha memnun, ${minPos} grubu (%${positionSatisfaction[minPos]}) daha az memnun.`;
                } else {
                    comparisonAnalysis = `Paydaş grupları arasında dengeli bir memnuniyet dağılımı görülmektedir. Tüm gruplar benzer seviyede memnuniyet gösteriyor.`;
                }
            }
            
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
                    <title>${companyName} - Kurum Değerlendirme Raporu</title>
                    <style>
                        body { font-family: Arial, sans-serif; margin: 15px; line-height: 1.4; font-size: 12px; }
                        .header { text-align: center; border-bottom: 2px solid #333; padding-bottom: 15px; margin-bottom: 20px; }
                        .stats { display: flex; justify-content: space-between; margin-bottom: 20px; }
                        .stat-box { background: #f5f5f5; padding: 10px; border-radius: 5px; text-align: center; width: 30%; }
                        .stat-number { font-size: 1.5em; font-weight: bold; color: #333; }
                        .section { margin-bottom: 20px; }
                        .section h3 { color: #333; border-bottom: 1px solid #ddd; padding-bottom: 5px; margin-bottom: 10px; }
                        table { width: 100%; border-collapse: collapse; margin-top: 5px; font-size: 11px; }
                        th, td { border: 1px solid #ddd; padding: 5px; text-align: left; }
                        th { background-color: #f2f2f2; }
                        .analysis-box { background: #e8f4fd; padding: 10px; border-radius: 5px; margin: 10px 0; }
                        .recommendations { background: #fff3cd; padding: 10px; border-radius: 5px; margin: 10px 0; }
                        .status-indicator { padding: 5px 10px; border-radius: 3px; color: white; font-weight: bold; display: inline-block; }
                        .chart-placeholder { width: 100%; height: 120px; background: #f8f9fa; border: 1px solid #ddd; display: flex; align-items: center; justify-content: center; margin: 10px 0; }
                        .comparison-box { background: #f0f8ff; padding: 10px; border-radius: 5px; margin: 10px 0; border-left: 4px solid #007bff; }
                        .footer { margin-top: 30px; text-align: center; font-size: 10px; color: #666; }
                    </style>
                </head>
                <body onload="window.print()">
                    <div class="header">
                        <h1>📊 ${companyName}</h1>
                        <h2>Kurum Değerlendirme Raporu</h2>
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
                            <div class="stat-number">${satisfactionPercentage}%</div>
                            <div>Genel Memnuniyet</div>
                        </div>
                    </div>
                    
                    <div class="analysis-box">
                        <h4>📈 Genel Durum Değerlendirmesi</h4>
                        <div class="status-indicator" style="background-color: ${statusColor};">${statusAnalysis}</div>
                        <p style="margin-top: 10px;"><strong>Memnuniyet Hesaplama Formülü:</strong> ((${totalScore} - ${minPossibleScore}) / (${maxPossibleScore} - ${minPossibleScore})) × 100 = %${satisfactionPercentage}</p>
                        <p style="margin-top: 5px;">${detailedAnalysis}</p>
                    </div>
                    
                    <div class="section">
                        <h3>👥 Paydaş Grupları Analizi</h3>
                        <table>
                            <tr><th>Paydaş Grubu</th><th>Katılımcı</th><th>Memnuniyet %</th><th>Değerlendirme</th></tr>
                            ${Object.entries(positionData).map(([pos, count]) => {
                                const satisfaction = positionSatisfaction[pos] || 0;
                                const status = satisfaction <= 50 ? 'Düşük' : satisfaction <= 75 ? 'Orta' : 'Yüksek';
                                const statusColor = satisfaction <= 50 ? '#dc3545' : satisfaction <= 75 ? '#ffc107' : '#28a745';
                                return `<tr><td>${pos}</td><td>${count}</td><td>%${satisfaction}</td><td style="color: ${statusColor}; font-weight: bold;">${status}</td></tr>`;
                            }).join('')}
                        </table>
                    </div>
                    
                    ${comparisonAnalysis ? `<div class="comparison-box">
                        <h4>🔍 Paydaş Grupları Karşılaştırması</h4>
                        <p>${comparisonAnalysis}</p>
                        ${specialScenarioAnalysis ? `<div style="margin-top: 10px; padding: 10px; background: #fff3cd; border-radius: 5px; border-left: 4px solid #ffc107;">
                            <strong>⚠️ Özel Durum Analizi:</strong><br>
                            ${specialScenarioAnalysis}
                        </div>` : ''}
                    </div>` : ''}
                    
                    <div class="chart-placeholder">
                        <div style="text-align: center;">
                            <div style="font-size: 14px; margin-bottom: 10px;">📊 Memnuniyet Dağılımı</div>
                            ${Object.entries(positionData).map(([pos, count]) => 
                                `<div style="margin: 3px 0; font-size: 11px;">${pos}: ${count} kişi (%${positionSatisfaction[pos] || 0} memnuniyet)</div>`
                            ).join('')}
                        </div>
                    </div>
                    
                    <div class="section">
                        <h3>📈 Yanıt Dağılımı</h3>
                        <table>
                            <tr><th>Değerlendirme Seviyesi</th><th>Yanıt Sayısı</th><th>Oran</th></tr>
                            <tr><td>Düşük Memnuniyet (1-2)</td><td>${satisfactionCounts[0]}</td><td>${totalAnswers > 0 ? Math.round((satisfactionCounts[0]/totalAnswers)*100) : 0}%</td></tr>
                            <tr><td>Orta Memnuniyet (3)</td><td>${satisfactionCounts[1]}</td><td>${totalAnswers > 0 ? Math.round((satisfactionCounts[1]/totalAnswers)*100) : 0}%</td></tr>
                            <tr><td>Yüksek Memnuniyet (4-5)</td><td>${satisfactionCounts[2]}</td><td>${totalAnswers > 0 ? Math.round((satisfactionCounts[2]/totalAnswers)*100) : 0}%</td></tr>
                        </table>
                    </div>
                    
                    <div class="section">
                        <h3>🎯 Detaylı Kategori Analizleri</h3>
                        
                        <div style="margin-bottom: 15px; padding: 10px; background: #f8f9fa; border-radius: 5px;">
                            <h4 style="color: #333; margin-bottom: 8px;">📚 1. Eğitim Kalitesi ve Akademik Gelişim</h4>
                            <p style="font-size: 11px; margin-bottom: 5px;"><strong>Tanım:</strong> Bu başlık, bir okulun eğitim kalitesini ölçer. Öğrenci başarısı, müfredatın güncelliği ve öğretmenlerin yetkinliği bu kategorinin temelini oluşturur.</p>
                            
                            ${satisfactionPercentage <= 50 ? `
                            <div style="background: #f8d7da; padding: 8px; border-radius: 3px; border-left: 4px solid #dc3545;">
                                <strong>Puan Aralığı: Düşük (%0-50)</strong><br>
                                Öğrenci ve veliler, müfredatın yetersiz olduğunu, öğretmenlerin konulara hakim olmadığını veya değerlendirme sisteminin adil olmadığını düşünüyor. Bu durum, kurumun en temel misyonunu yerine getiremediğini gösterir ve acil bir "akademik kriz" sinyali verir. Müfredat acilen gözden geçirilmeli, öğretmenler için hizmet içi eğitim programları başlatılmalı ve sınav/değerlendirme süreçleri şeffaflaştırılmalıdır.
                            </div>
                            ` : satisfactionPercentage <= 75 ? `
                            <div style="background: #fff3cd; padding: 8px; border-radius: 3px; border-left: 4px solid #ffc107;">
                                <strong>Puan Aralığı: Orta (%51-75)</strong><br>
                                Eğitim kalitesi genel olarak kabul edilebilir düzeyde, ancak teknolojik imkanların kullanımı veya bireysel öğrenme yaklaşımları gibi konularda beklentiler karşılanmıyor. Okulun gelişim potansiyeli var ancak bu tam olarak kullanılamıyor. Yaratıcı ve yenilikçi öğretim metodlarına yatırım yapılmalı, öğretmenlerin teknoloji kullanımı desteklenmeli ve öğrencilere özel yeteneklerini geliştirebilecekleri alanlar sunulmalıdır.
                            </div>
                            ` : `
                            <div style="background: #d4edda; padding: 8px; border-radius: 3px; border-left: 4px solid #28a745;">
                                <strong>Puan Aralığı: Yüksek (%76-100)</strong><br>
                                Öğrenci, öğretmen ve veliler, okulun sunduğu eğitimin kalitesinden son derece memnun. Bu, okulun eğitim alanında lider konumda olduğunu ve başarısını bir "marka değeri" olarak kullanabileceğini gösterir. Bu başarıyı sürdürmek için sürekli inovasyon teşvik edilmeli, öğretmenlerin motivasyonu ve mesleki gelişimi en üst düzeyde tutulmalıdır.
                            </div>
                            `}
                        </div>
                        
                        <div style="margin-bottom: 15px; padding: 10px; background: #f8f9fa; border-radius: 5px;">
                            <h4 style="color: #333; margin-bottom: 8px;">🏫 2. Okul Ortamı ve Olanaklar</h4>
                            <p style="font-size: 11px; margin-bottom: 5px;"><strong>Tanım:</strong> Bu başlık, fiziksel koşulların ve sosyal imkanların, öğrenci ve öğretmenlerin motivasyonunu ne kadar etkilediğini değerlendirir.</p>
                            
                            ${satisfactionPercentage <= 50 ? `
                            <div style="background: #f8d7da; padding: 8px; border-radius: 3px; border-left: 4px solid #dc3545;">
                                <strong>Puan Aralığı: Düşük (%0-50)</strong><br>
                                Okulun fiziki altyapısı yetersiz. Öğrenciler ve öğretmenler konforsuz bir ortamda çalışıyorlar. Bu, öğrenme ve öğretme motivasyonunu ciddi şekilde düşüren bir engel teşkil eder. Acil olarak altyapı iyileştirmeleri yapılmalı, temizlik ve güvenlik standartları yükseltilmeli ve temel teknolojik donanım eksiklikleri giderilmelidir.
                            </div>
                            ` : satisfactionPercentage <= 75 ? `
                            <div style="background: #fff3cd; padding: 8px; border-radius: 3px; border-left: 4px solid #ffc107;">
                                <strong>Puan Aralığı: Orta (%51-75)</strong><br>
                                Temel fiziki koşullar sağlanıyor ancak modern standartlardan uzak. Öğrenciler daha fazla spor, sanat veya teknoloji laboratuvarı gibi olanaklar bekliyor. Okul, temel ihtiyaçları karşılıyor ancak ek imkanlar sunma konusunda yetersiz kalıyor. Okulun bütçesi, yenilikçi olanaklar için ayrılmalı ve bu imkanlar okulun tanıtımında öne çıkarılmalıdır.
                            </div>
                            ` : `
                            <div style="background: #d4edda; padding: 8px; border-radius: 3px; border-left: 4px solid #28a745;">
                                <strong>Puan Aralığı: Yüksek (%76-100)</strong><br>
                                Okulun fiziksel ve teknolojik altyapısı mükemmel, temizlik ve güvenlik standartları üst düzeyde. Bu, kurumun öğrenci ve öğretmen refahına büyük önem verdiğinin ve sektör lideri olduğunun bir göstergesidir. Bu standardı korumak için düzenli bakım ve yenileme çalışmaları yapılmalı, çevreci uygulamalarla bu başarı pekiştirilmelidir.
                            </div>
                            `}
                        </div>
                        
                        <div style="margin-bottom: 15px; padding: 10px; background: #f8f9fa; border-radius: 5px;">
                            <h4 style="color: #333; margin-bottom: 8px;">💬 3. Yönetim ve İletişim</h4>
                            <p style="font-size: 11px; margin-bottom: 5px;"><strong>Tanım:</strong> Bu başlık, okul yönetiminin şeffaflığını, veli-öğretmen-öğrenci iletişimini ve yönetim yapısının güvenilirliğini ölçer.</p>
                            
                            ${satisfactionPercentage <= 50 ? `
                            <div style="background: #f8d7da; padding: 8px; border-radius: 3px; border-left: 4px solid #dc3545;">
                                <strong>Puan Aralığı: Düşük (%0-50)</strong><br>
                                Yönetim kararları şeffaf değil, veliler ve öğretmenler kendilerini dinlenmemiş hissediyor. Güvenin zayıf olduğu bir ortamda, yönetimle diğer paydaşlar arasında ciddi bir iletişim kopukluğu yaşanıyor. İletişim kanalları güçlendirilmeli, düzenli bilgilendirme toplantıları düzenlenmeli ve şikayetlerin takip edildiği şeffaf bir sistem kurulmalıdır.
                            </div>
                            ` : satisfactionPercentage <= 75 ? `
                            <div style="background: #fff3cd; padding: 8px; border-radius: 3px; border-left: 4px solid #ffc107;">
                                <strong>Puan Aralığı: Orta (%51-75)</strong><br>
                                İletişim genel olarak iyi, ancak bazı sorunlar yaşandığında yönetim süreci yavaş işliyor. Temel iletişim kurulmuş ancak bu ilişki daha yapıcı ve işbirlikçi bir seviyeye taşınmalıdır. Yönetim ekibi, geri bildirimlere daha hızlı yanıt vermeye teşvik edilmeli, karar alma süreçlerinde paydaş temsilcilerine daha fazla yer verilmelidir.
                            </div>
                            ` : `
                            <div style="background: #d4edda; padding: 8px; border-radius: 3px; border-left: 4px solid #28a745;">
                                <strong>Puan Aralığı: Yüksek (%76-100)</strong><br>
                                Okul yönetimi son derece güvenilir, şeffaf ve erişilebilir. Paydaşlar, fikirlerinin değerli olduğunu hissediyor. Bu, güçlü bir kurum kültürünün ve liderliğin en net göstergesidir. Bu güçlü iletişimi sürdürmek için düzenli "nabız anketleri" yapılmalı ve açık iletişim forumları aktif tutulmalıdır.
                            </div>
                            `}
                        </div>
                    </div>
                    
                    <div class="recommendations">
                        <h4>💡 Öneriler ve Eylem Planı</h4>
                        <p><strong>Öncelikli Aksiyonlar:</strong> ${recommendations}</p>
                        <p><strong>Takip:</strong> Bu rapor sonuçlarını 3-6 ay sonra tekrar değerlendirmek için yeni anket düzenleyiniz.</p>
                    </div>
                    
                    <div class="footer">
                        <p>Akça Pro X - Profesyonel Kurum Değerlendirme Sistemi | ${new Date().toLocaleString('tr-TR')}</p>
                        <p>Bu rapor ${totalAnswers} adet soru yanıtı analiz edilerek oluşturulmuştur.</p>
                    </div>
                </body>
                </html>
            `;
        }

        function generateCharts(surveys) {
            if (surveys.length === 0) return;
            
            // Pozisyon grafiği
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
                        backgroundColor: ['#3b82f6', '#10b981', '#f59e0b']
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: false }
                    }
                }
            });
            
            // Değerlendirme grafiği
            const satisfactionCounts = [0, 0, 0];
            surveys.forEach(s => {
                const avgScore = parseFloat(s.averageScore);
                if (avgScore < 2.5) satisfactionCounts[0]++;
                else if (avgScore < 3.5) satisfactionCounts[1]++;
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
                        legend: { display: false }
                    },
                    scales: {
                        y: { beginAtZero: true }
                    }
                }
            });
            
            // Süre dağılımı grafiği
            const timeCounts = { '0-5dk': 0, '5-10dk': 0, '10dk+': 0 };
            surveys.forEach(s => {
                const duration = s.duration || '00:00';
                const minutes = parseInt(duration.split(':')[0]) || 0;
                if (minutes <= 5) timeCounts['0-5dk']++;
                else if (minutes <= 10) timeCounts['5-10dk']++;
                else timeCounts['10dk+']++;
            });
            
            const timeCtx = document.getElementById('timeChart').getContext('2d');
            new Chart(timeCtx, {
                type: 'pie',
                data: {
                    labels: Object.keys(timeCounts),
                    datasets: [{
                        data: Object.values(timeCounts),
                        backgroundColor: ['#8b5cf6', '#06b6d4', '#f97316']
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: false }
                    }
                }
            });
            
            // Puan dağılımı grafiği
            const scoreRanges = { '1-2': 0, '2-3': 0, '3-4': 0, '4-5': 0 };
            surveys.forEach(s => {
                const avgScore = parseFloat(s.averageScore);
                if (avgScore < 2) scoreRanges['1-2']++;
                else if (avgScore < 3) scoreRanges['2-3']++;
                else if (avgScore < 4) scoreRanges['3-4']++;
                else scoreRanges['4-5']++;
            });
            
            const trendCtx = document.getElementById('trendChart').getContext('2d');
            new Chart(trendCtx, {
                type: 'line',
                data: {
                    labels: Object.keys(scoreRanges),
                    datasets: [{
                        data: Object.values(scoreRanges),
                        borderColor: '#6366f1',
                        backgroundColor: 'rgba(99, 102, 241, 0.1)',
                        fill: true
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: false }
                    },
                    scales: {
                        y: { beginAtZero: true }
                    }
                }
            });
        }

        function toggleParticipantDetails() {
            const detailsDiv = document.getElementById('participantDetails');
            const toggleBtn = document.getElementById('toggleParticipantsBtn');
            
            if (detailsDiv.classList.contains('hidden')) {
                detailsDiv.classList.remove('hidden');
                toggleBtn.textContent = '📋 Katılımcıları Gizle';
                loadParticipantTable();
            } else {
                detailsDiv.classList.add('hidden');
                toggleBtn.textContent = '📋 Katılımcıları Görüntüle';
            }
        }

        function loadParticipantTable() {
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
                const displayName = (survey.firstName && survey.lastName) ? 
                    `${survey.firstName} ${survey.lastName}` : 
                    (survey.firstName || survey.lastName || 'İsimsiz');
                
                const avgScore = parseFloat(survey.averageScore);
                let evaluation = '';
                let evaluationColor = '';
                
                if (avgScore < 2.5) {
                    evaluation = 'Düşük';
                    evaluationColor = 'text-red-600';
                } else if (avgScore < 3.5) {
                    evaluation = 'Orta';
                    evaluationColor = 'text-yellow-600';
                } else {
                    evaluation = 'Yüksek';
                    evaluationColor = 'text-green-600';
                }
                
                return `
                    <tr class="hover:bg-gray-50">
                        <td class="px-3 py-2">${displayName}</td>
                        <td class="px-3 py-2">${survey.jobType}</td>
                        <td class="px-3 py-2 text-center font-semibold">${avgScore}</td>
                        <td class="px-3 py-2 text-center ${evaluationColor} font-semibold">${evaluation}</td>
                        <td class="px-3 py-2 text-center text-sm">${new Date(survey.submittedAt).toLocaleDateString('tr-TR')}</td>
                    </tr>
                `;
            }).join('');
        }

        function loadDemoData() {
            // Demo veri yükleme fonksiyonu
        }
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'981af265f22bd620',t:'MTc1ODMwNDQ1MS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>

