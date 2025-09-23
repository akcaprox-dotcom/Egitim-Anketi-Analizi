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
                        <th class="px-3 py-2 text-left">Ünvan</th>
                        <th class="px-3 py-2 text-center">Yanıt Ortalaması</th>
                        <th class="px-3 py-2 text-center">Anket Tarihi</th>
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
    if (wrapper.classList.contains('hidden')) {
        wrapper.classList.remove('hidden');
        arrow.textContent = '▲';
    } else {
        wrapper.classList.add('hidden');
        arrow.textContent = '▼';
    }
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
                        'Çalışma Ortamı',
                        'Yemek ve Sosyal Haklar',
                        'İş İlişkileri',
                        'Sadakat ve Gelecek',
                        'Dijitalleşme ve Yenilik',
                        'Genel Memnuniyet'
                    ]
                },
                {
                    name: 'Beyaz Yaka',
                    categories: [
                        'Çalışma Ortamı',
                        'Yönetim ve Liderlik',
                        'İş Yükü ve Dengesi',
                        'İç İletişim',
                        'Kariyer Gelişimi',
                        'Ücret ve Yan Haklar',
                        'Takdir ve Geri Bildirim',
                        'İş Süreçleri',
                        'Kurum Kültürü',
                        'Genel İş Memnuniyeti'
                    ]
                },
                {
                    name: 'Yönetim',
                    categories: [
                        'Finansal Performans ve Operasyonel Verimlilik',
                        'Pazarlama ve Marka Yönetimi',
                        'İnsan Kaynakları Yönetimi',
                        'Müşteri/Çalışan İlişkileri ve Kalite Kontrol',
                        'Teknolojik Altyapı ve Gelecek Vizyonu',
                        'Genel Yönetim Memnuniyeti'
                    ]
                }
            ];
            const satisfactionLabels = ['Çok Memnunum', 'Memnun', 'Kararsızım', 'Memnun Değilim', 'Hiç Memnun Değilim'];

            // Soru index aralıkları (örnek, gerçek indexler soru setine göre ayarlanmalı)
            const groupRanges = {
                'Mavi Yaka': [0, 49],
                'Beyaz Yaka': [50, 99],
                'Yönetim': [100, 149]
            };

            // Her grup ve kategori için frekansları hesapla
            function getCategoryIndexes(group, catIdx) {
                // Her kategori 10 soru ise:
                const start = groupRanges[group][0] + catIdx * 10;
                const end = start + 9;
                return [start, end];
            }

            // Frekans tablosu oluştur
            let table = `<div class="overflow-x-auto"><table class="min-w-full text-xs text-center border border-gray-300 mb-6">
                <thead>
                    <tr class="bg-gray-100">
                        <th class="px-2 py-1">Grup / Soru</th>
                        ${satisfactionLabels.map(l => `<th class="px-2 py-1">${l}</th>`).join('')}
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
                table += `<tr class="font-bold bg-gray-50"><td>${group.name}</td>${groupPercents.map(p => `<td>${p}</td>`).join('')}</tr>`;

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
                    // DÜZELTME: Kategori toplamı 0 ise, grup toplamından paylaştır
                    if (catTotal === 0 && groupTotal > 0) {
                        // Kategoriye ait sorular yoksa, grup yüzdelerini göster
                        table += `<tr><td>${cat}</td>${groupCounts.map(c => `<td>${c}</td>`).join('')}</tr>`;
                    } else {
                        table += `<tr><td>${cat}</td>${catCounts.map(c => `<td>${c}</td>`).join('')}</tr>`;
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
