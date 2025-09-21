<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Akça Pro X - Kurum Değerlendirme Anketi</title>
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
    <nav class="gradient-bg text-white p-3 shadow-lg sticky top-0 z-50">
        <div class="max-w-5xl mx-auto flex flex-col md:flex-row justify-between items-center gap-2 md:gap-0">
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
    <div id="surveyModule" class="max-w-5xl mx-auto p-2 md:p-4">
        <div class="bg-white shadow-xl rounded-2xl max-w-2xl mx-auto p-4 md:p-8">
            <div class="text-center mb-6">
                <h2 class="text-2xl md:text-3xl font-extrabold text-gray-800 mb-1 tracking-tight">Kurum Değerlendirme Anketi</h2>
                <p class="text-gray-600 mb-2 text-base md:text-lg">Görüşleriniz bizim için değerli</p>
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
                <h3 class="text-base font-semibold text-gray-700 mb-3">Kurum ve Kişisel Bilgiler</h3>
                <!-- Google ile Giriş Yap butonu -->
                <div class="mb-3 flex flex-col items-center">
                    <button id="googleSignInBtn" type="button" class="flex items-center gap-2 px-4 py-2 bg-white border border-gray-300 rounded shadow hover:bg-gray-100 text-gray-700 font-semibold mb-2">
                        <img src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/google.svg" alt="Google" class="w-5 h-5"> Google ile Giriş Yap
                    </button>
                    <div id="googleUserInfo" class="text-xs text-green-700 font-medium hidden"></div>
                </div>
                <div class="mb-3">
                    <input type="text" id="companyName" placeholder="Kurum adınızı girin (Okul, Üniversite vb.)" 
                        class="w-full border-2 border-purple-300 rounded px-3 py-2 text-sm focus:ring-2 focus:ring-purple-500 focus:border-purple-500">
                </div>
                <div class="mb-3">
                    <p class="text-xs text-gray-600 mb-2">Rolünüzü seçin:</p>
                    <div class="grid grid-cols-1 sm:grid-cols-3 gap-2">
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
                <div id="selectedJobDisplay" class="text-center text-sm text-gray-600 mb-3 min-h-[20px]"></div>
                <div class="grid grid-cols-2 gap-2 mb-4">
                    <input type="text" id="firstName" placeholder="Adınız" 
                        class="border-2 border-gray-300 rounded px-3 py-2 text-sm focus:ring-2 focus:ring-purple-500 focus:border-purple-500">
                    <input type="text" id="lastName" placeholder="Soyadınız" 
                        class="border-2 border-gray-300 rounded px-3 py-2 text-sm focus:ring-2 focus:ring-purple-500 focus:border-purple-500">
                </div>
                <button id="startSurvey" class="w-full py-3 rounded text-white font-semibold gradient-bg hover:opacity-90 transition-opacity text-sm">
                    📊 Anketi Başlat
                </button>
            </div>

            <!-- Anket Alanı -->
            <div id="surveySection" class="hidden">
                <div class="flex flex-col md:flex-row justify-between items-center mb-6 gap-2">
                    <span id="progressText" class="text-gray-600 font-medium">Anket İlerlemesi 0/50 Yanıtlandı</span>
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
                    <div class="flex flex-col md:flex-row justify-between items-center mb-6 gap-2">
                        <h3 class="text-xl font-semibold mb-2 md:mb-0">Anket Sonuçları</h3>
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

        // Google Sign-In logic
        let googleUser = null;
        document.addEventListener('DOMContentLoaded', function() {
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
                                document.getElementById('firstName').value = user.displayName ? user.displayName.split(' ')[0] : '';
                                document.getElementById('lastName').value = user.displayName ? user.displayName.split(' ').slice(1).join(' ') : '';
                                userInfoDiv.textContent = `Giriş yapıldı: ${user.displayName} (${user.email})`;
                                userInfoDiv.classList.remove('hidden');
                                // Ad ve soyad alanları düzenlenebilir olmalı
                                document.getElementById('firstName').readOnly = false;
                                document.getElementById('lastName').readOnly = false;
                            }
                        })
                        .catch((error) => {
                            alert('Google ile giriş başarısız: ' + error.message);
                        });
           
