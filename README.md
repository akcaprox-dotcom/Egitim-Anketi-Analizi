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
                <span class="bg-green-100 text-green-800 text-xs font-semibold px-2 py-1 rounded-full">v3.0.0 - Firebase Entegre</span>
            </div>

            <!-- Sorumluluk Reddi -->
            <div id="disclaimerSection" class="mb-4">
                <div class="bg-yellow-50 border border-yellow-300 rounded p-3 mb-3">
                    <h3 class="font-semibold text-yellow-800 mb-2 text-sm">⚠️ Veri Koruma Beyanı</h3>
                    <div class="text-xs text-yellow-700 space-y-1">
                        <p>• Verileriniz <b>Google Firebase</b> bulut altyapısında güvenli bir şekilde saklanır ve üçüncü taraflarla paylaşılmaz.</p>
                        <p>• Anket sonuçları sadece kurum yetkilileri tarafından görüntülenebilir.</p>
                        <p>• Sistem güvenliği hizmet sağlayıcıya (<b>Firebase</b>) aittir.</p>
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
                <!-- Google ile Giriş Yap butonu -->
                <div class="mb-3 flex flex-col items-center">
                    <button id="googleSignInBtn" type="button" class="flex items-center gap-2 px-4 py-2 bg-white border border-gray-300 rounded shadow hover:bg-gray-100 text-gray-700 font-semibold mb-2">
                        <img src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/google.svg" alt="Google" class="w-5 h-5"> Google ile Giriş Yap
                    </button>
                    <div id="googleUserInfo" class="text-xs text-green-700 font-medium hidden"></div>
                </div>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-auth.js"></script>
                <h3 class="text-base font-semibold text-gray-700 mb-3">Kurum ve Kişisel Bilgiler</h3>
                <!-- Kullanıcı Tipi Seçimi -->
                <div class="mb-3 flex gap-4 items-center">
                    <label class="flex items-center gap-2">
                        <input type="radio" name="userType" id="userTypeNew" value="new" checked class="accent-purple-600">
                        <span>Yeni Kullanıcı</span>
                    </label>
                    <label class="flex items-center gap-2">
                        <input type="radio" name="userType" id="userTypeExisting" value="existing" class="accent-blue-600">
                        <span>Kayıtlı Kullanıcı</span>
                    </label>
                </div>
                <!-- Yeni Kullanıcı Alanı -->
                <div class="mb-3" id="newUserArea">
                    <input type="text" id="companyName" placeholder="Kurum adınızı girin (Okul, Üniversite vb.)" 
                        class="w-full border-2 border-purple-300 rounded px-3 py-2 text-sm focus:ring-2 focus:ring-purple-500 focus:border-purple-500">
                </div>
                <!-- Kayıtlı Kullanıcı Alanı -->
                <div class="mb-3 hidden" id="existingUserArea">
                    <select id="existingCompanySelect" class="w-full border-2 border-blue-300 rounded px-3 py-2 text-sm focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                        <option value="">Kayıtlı kurum seçin...</option>
                    </select>
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
                            <button onclick="showPDFReport(true)" class="px-3 py-1 bg-red-600 text-white rounded hover:bg-red-700 text-sm" style="display:none !important">📄 PDF Göster (Filtreli)</button>
                            <button onclick="showPDFReport(false)" class="px-3 py-1 bg-gray-600 text-white rounded hover:bg-gray-700 text-sm" style="display:none !important">📄 PDF Göster (Tümü)</button>
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
                    <!-- SWOT Analizi Tablosu (Rapor Ekranı) -->
                    <div class="bg-white border rounded-lg p-4 mb-6" style="display:none">
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
                                        <td class="border border-gray-300 p-2 align-top">• Yüksek katılımcı memnuniyeti<br>• Güçlü eğitmen kadrosu<br>• Modern eğitim altyapısı</td>
                                        <td class="border border-gray-300 p-2 align-top">• Yoğun dönemlerde iletişim eksikliği<br>• Kısıtlı sosyal etkinlikler<br>• Dijital materyal eksikliği</td>
                                        <td class="border border-gray-300 p-2 align-top">• Dijitalleşme yatırımları<br>• Yeni eğitim programları<br>• Kamu destekleri</td>
                                        <td class="border border-gray-300 p-2 align-top">• Artan rekabet<br>• Ekonomik dalgalanmalar<br>• Personel değişimi</td>
                                    </tr>
                                </tbody>
                            </table>
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

        <!-- Category Detail Modal (İşletme.html'den kopyalandı) -->
        <div id="categoryDetailModal" class="modal">
            <div class="modal-content max-w-4xl bg-white shadow-2xl" style="margin: 5% auto; padding: 30px; border-radius: 20px; max-height: 80vh; overflow-y: auto; width: 90vw;">
                <div class="modal-header flex justify-between items-center mb-6 border-b pb-4">
                    <h2 class="text-2xl font-bold text-gray-800" id="categoryDetailTitle">📋 Kategori Detayları</h2>
                    <span class="close cursor-pointer text-3xl text-gray-500 hover:text-gray-700" onclick="document.getElementById('categoryDetailModal').classList.remove('show')">&times;</span>
                </div>
                <div id="categoryDetailContent"></div>
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
                    <div class="mb-4 flex flex-col sm:flex-row gap-2 items-center">
                        <input id="companySearchInput" type="text" placeholder="🔍 Kurum adı ile ara..." class="border border-gray-300 rounded px-3 py-2 text-sm w-full sm:w-64 focus:ring-2 focus:ring-blue-500 focus:border-blue-500" oninput="filterCompanyList()">
                    </div>
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
// Firebase config ve Google Sign-In logic (hastane.html ile aynı)
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
let googleUser = null;
document.addEventListener('DOMContentLoaded', function() {
    const startBtn = document.getElementById('startSurvey');
    if (startBtn) {
        // startSurvey eventini birleştir
        startBtn.addEventListener('click', function(e) {
            // Kayıtlı kullanıcı ise seçilen kurumu companyName inputuna yaz
            if (userTypeExisting && userTypeExisting.checked) {
                const selected = existingCompanySelect.value;
                if (!selected) {
                    e.preventDefault();
                    showModal('⚠️ Eksik Bilgi', 'Lütfen kayıtlı bir kurum seçin.');
                    return false;
                }
                companyNameInput.value = selected;
            }
            // Google ile giriş kontrolü
            if (!googleUser) {
                e.preventDefault();
                alert('Ankete başlamadan önce Google ile giriş yapmalısınız.');
                return false;
            }
            startSurvey();
        });
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
                        document.getElementById('firstName').value = user.displayName ? user.displayName.split(' ')[0] : '';
                        document.getElementById('lastName').value = user.displayName ? user.displayName.split(' ').slice(1).join(' ') : '';
                        userInfoDiv.textContent = `Giriş yapıldı: ${user.displayName} (${user.email})`;
                        userInfoDiv.classList.remove('hidden');
                        document.getElementById('firstName').readOnly = false;
                        document.getElementById('lastName').readOnly = false;
                    }
                })
                .catch((error) => {
                    alert('Google ile giriş başarısız: ' + error.message);
                });
        });
    }
    // Kullanıcı tipi seçimi için event listener
    const userTypeNew = document.getElementById('userTypeNew');
    const userTypeExisting = document.getElementById('userTypeExisting');
    const newUserArea = document.getElementById('newUserArea');
    const existingUserArea = document.getElementById('existingUserArea');
    const companyNameInput = document.getElementById('companyName');
    const existingCompanySelect = document.getElementById('existingCompanySelect');

    function toggleUserType() {
        if (userTypeNew.checked) {
            newUserArea.classList.remove('hidden');
            existingUserArea.classList.add('hidden');
        } else {
            newUserArea.classList.add('hidden');
            existingUserArea.classList.remove('hidden');
            // Kayıtlı kurumları yükle
            loadExistingCompanies();
        }
    }
    if (userTypeNew && userTypeExisting) {
        userTypeNew.addEventListener('change', toggleUserType);
        userTypeExisting.addEventListener('change', toggleUserType);
    }

    async function loadExistingCompanies() {
        // Firebase'den kurumları çek
        if (!window.systemData || !window.systemData.surveyData) {
            window.systemData = window.systemData || {};
            window.systemData.surveyData = await loadFromFirebase();
        }
        const companies = (window.systemData.surveyData && window.systemData.surveyData.companies) || {};
        existingCompanySelect.innerHTML = '<option value="">Kayıtlı kurum seçin...</option>';
        Object.values(companies).forEach(company => {
            existingCompanySelect.innerHTML += `<option value="${company.name}">${company.name}</option>`;
        });
    }

    // (startBtn event listener'ı yukarıda tanımlandı, burada tekrar tanımlamaya gerek yok)
    // Sayfa ilk açıldığında doğru alanı göster
    toggleUserType();
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


    // Firebase Realtime Database ayarları
    const FIREBASE_DB_URL = 'https://egitim-37c53-default-rtdb.europe-west1.firebasedatabase.app';
        // responses artık bir nesne olarak tutulacak (array değil)

        // Soru setleri
        const questions = {
                "Öğrenci": [
                    // 1. Ders İçeriği ve Öğrenme Ortamı
                    "Derslerde işlenen konular ilgi çekici ve hayatla bağlantılıdır.",
                    "Öğretmenlerimin kullandığı öğretim yöntemleri (tartışma, proje vb.) öğrenmemi kolaylaştırıyor.",
                    "Okuldaki teknolojik araçlar (akıllı tahta, bilgisayar laboratuvarı) derslere katkı sağlıyor.",
                    "Ders sırasında soru sormaktan veya fikrimi belirtmekten çekinmiyorum.",
                    "Sınavlar ve değerlendirmeler, öğrendiklerimi doğru bir şekilde ölçmektedir.",
                    // 2. Okul İklimi ve Güvenlik
                    "Okulumda kendimi fiziksel ve duygusal olarak güvende hissediyorum.",
                    "Okulda akran zorbalığı (bullying) ve taciz olayları nadiren yaşanmaktadır.",
                    "Okul yönetiminin kural ve disiplin uygulamaları adil ve tutarlıdır.",
                    "Okul binası ve çevresi temiz ve bakımlıdır.",
                    "Okulun acil durum ve güvenlik tatbikatları konusunda bilgi sahibiyim.",
                    // 3. Öğretmen Etkileşimi ve Destek
                    "Öğretmenlerim bana saygılı davranır ve bana değer verdiğini hissettirir.",
                    "Bir konuda yardıma ihtiyacım olduğunda, öğretmenime kolayca ulaşabilirim.",
                    "Öğretmenlerim, hatalarımdan ders çıkarmam için beni cesaretlendirir.",
                    "Öğretmenlerim, bireysel öğrenme farklılıklarımı dikkate alarak yardımcı olur.",
                    "Okul rehberlik servisi, eğitim ve kariyer hedeflerimde bana yol göstermektedir.",
                    // 4. Sosyal ve Kültürel Aktiviteler
                    "Okulda katılabileceğim kulüpler, spor takımları ve sanatsal etkinlikler yeterince çeşitlidir.",
                    "Okul gezileri ve saha çalışmaları dersleri daha anlamlı hale getirmektedir.",
                    "Okul, benim liderlik becerilerimi geliştirecek fırsatlar sunmaktadır.",
                    "Okul etkinlikleri, farklı öğrencilerle tanışmamı ve kaynaşmamı sağlamaktadır.",
                    "Okuldaki sosyal etkinlikler için ayrılan mekanlar ve materyaller yeterlidir.",
                    // 5. Fiziksel Olanaklar
                    "Okuldaki kütüphane ve okuma alanları ders çalışmak için uygun bir ortam sunmaktadır.",
                    "Okulun spor salonu/alanları yeterli donanıma ve bakıma sahiptir.",
                    "Okulun yemekhanesi/kantini hijyenik ve sunduğu seçenekler sağlıklıdır.",
                    "Tuvalet ve lavaboların temizlik ve hijyeni yeterli düzeydedir.",
                    "Okulun ısıtma ve havalandırma sistemleri, ders sırasında rahat hissetmemi sağlamaktadır.",
                    // 6. Karar Alma Süreçleri ve Katılım
                    "Okul yönetiminin bizimle ilgili aldığı kararlar hakkında fikrimiz sorulmaktadır.",
                    "Öğrenci konseyi/temsilciliği, öğrencilerin sesi olma görevini etkili bir şekilde yerine getirmektedir.",
                    "Okul kurallarının belirlenme sürecinde, öğrencilerin görüşleri önemsenmektedir.",
                    "Okulda kendimi önemli ve değerli bir parça olarak görüyorum.",
                    "Okulda eleştirel düşünceyi ve tartışmayı teşvik eden bir ortam vardır.",
                    // 7. Bilişim ve Dijitalleşme
                    "Okulun interneti (wi-fi) hızlı ve güvenli bir şekilde çalışmaktadır.",
                    "Okulun kullandığı dijital öğrenme platformları (LMS vb.) kolay kullanımlıdır.",
                    "Öğretmenlerimin teknolojiyi kullanma becerileri dersleri daha iyi hale getiriyor.",
                    "Okul, dijital vatandaşlık ve internet güvenliği konularında beni bilinçlendirmektedir.",
                    "E-posta, not sistemi gibi dijital iletişim araçları üzerinden bilgiye kolayca ulaşabilirim.",
                    // 8. Okul Dışı Hazırlık ve Ödevler
                    "Bana verilen ödevler, öğrenmemi destekleyecek şekilde anlamlı ve faydalıdır.",
                    "Ödevlerin zorluk seviyesi ve süresi, diğer derslerimle dengeyi korumaktadır.",
                    "Öğretmenlerim, ödevlerimi düzenli kontrol eder ve yapıcı geri bildirim verir.",
                    "Okul, üniversite/gelecek kariyerim için gereken becerileri kazanmama yardımcı olmaktadır.",
                    "Okul dışındaki özel derslere veya ek kurslara ihtiyaç duymuyorum.",
                    // 9. Çeşitlilik ve Kapsayıcılık
                    "Okulumuz farklı kültür, inanç ve geçmişten gelen öğrencileri kucaklamaktadır.",
                    "Okulda dezavantajlı öğrenciler için özel olarak tasarlanmış destek programları mevcuttur.",
                    "Öğretmenlerim, cinsiyet, ırk veya engel ayrımı yapmaksızın tüm öğrencilere eşit davranır.",
                    "Okul, farklı fikirlere ve bakış açılarına saygı duyulmasını teşvik etmektedir.",
                    "Okulda kendimi olduğum gibi kabul edilmiş hissediyorum.",
                    // 10. Genel Memnuniyet ve Tavsiye
                    "Okula gitmekten genellikle mutluyum ve isteyerek geliyorum.",
                    "Bu okulu arkadaşlarıma ve aileme tavsiye ederim.",
                    "Okulun genel atmosferi ve pozitif enerjisi yüksektir.",
                    "Okuldaki başarım ve kişisel gelişimim arasında pozitif bir ilişki görüyorum.",
                    "Genel olarak, bu okuldaki eğitim kalitesinden ve deneyiminden memnunum."
                ],
                "Öğretmen": [
                    // 1. Ders İçeriği ve Öğrenme Ortamı
                    "Müfredat içeriği, öğrencilerin yaş ve gelişim seviyelerine uygun ve günceldir.",
                    "Öğretmenler odası ve ortak çalışma alanları, verimli çalışmam için yeterli ve konforludur.",
                    "Derste kullanmamız gereken teknolojik donanımlar (akıllı tahta, projeksiyon) sorunsuz çalışmaktadır.",
                    "Sınıf mevcudu, etkili ve birebir öğrenmeyi destekleyecek ideal seviyededir.",
                    "Okulun laboratuvar, atölye ve sanatsal materyal bütçesi derslerimi zenginleştirmeye yeterlidir.",
                    // 2. Okul İklimi ve Güvenlik
                    "Okuldaki iş yükü, stres yönetimi konusunda idareden destek alıyorum.",
                    "Okul yönetiminin disiplin kurallarını uygulama konusunda tutarlı ve adildir.",
                    "Velilerle olan iletişimim, öğrenci başarısını destekleyici ve saygılı bir zeminde ilerlemektedir.",
                    "Okul, öğretmenler arasında mesleki işbirliği ve saygılı bir iletişim kültürünü teşvik etmektedir.",
                    "Okuldaki güvenlik prosedürleri, hem öğrenciler hem de personel için yeterlidir.",
                    // 3. Öğretmen Etkileşimi ve Destek
                    "Okul yönetimi, öğretmenler arası fikir alışverişini ve ortak proje geliştirmeyi desteklemektedir.",
                    "Performans değerlendirme ve geri bildirimler, adil ve gelişim odaklıdır.",
                    "Rehberlik servisi ile öğrenci sorunları ve özel ihtiyaçları konusunda etkili işbirliği yapıyorum.",
                    "İdareden aldığım idari ve lojistik destek (evrak, fotokopi vb.) yeterlidir.",
                    "Okul, benim mesleki özerkliğime ve karar verme yetkime saygı duymaktadır.",
                    // 4. Sosyal ve Kültürel Aktiviteler
                    "Öğrencilerin kulüp ve aktivite seçimlerine rehberlik etme konusunda yeterli zamanım var.",
                    "Okulun sosyal ve kültürel etkinlikleri, öğrencilerin kişisel gelişimine somut katkı sağlamaktadır.",
                    "Aktiviteler için ayrılan bütçe ve kaynaklar, etkinlik kalitesini korumaktadır.",
                    "Okul dışı gezilerin organizasyonu, öğretmenler için gereksiz iş yükü yaratmamaktadır.",
                    "Okul, öğrencilerin ilgi alanlarına yönelik yenilikçi aktivite fikirlerimi desteklemektedir.",
                    // 5. Fiziksel Olanaklar
                    "Sınıfların fiziksel düzeni (ışık, masa, sandalye) farklı öğretim yöntemlerine uygundur.",
                    "Okulun kütüphane ve kaynak materyal stoğu derslerimi destekleyecek kadar geniştir.",
                    "Okul, öğretmenlerin dinlenmesi ve hazırlanması için yeterli ve kaliteli özel alanlar sunmaktadır.",
                    "Okul binasının genel temizliği ve bakımı, çalışma motivasyonumu artırmaktadır.",
                    "Okul yönetimi, fiziksel iyileştirmeler konusunda öğretmenlerin önerilerini dikkate alır.",
                    // 6. Karar Alma Süreçleri ve Katılım
                    "Okulun eğitim politikalarının ve hedeflerinin belirlenme sürecine aktif olarak katılırım.",
                    "Bölüm toplantıları, okulun stratejik yönü hakkında fikir beyan etmem için etkili bir platformdur.",
                    "Öğretmenler olarak, müfredat ve ders materyalleri seçimi konusunda yeterli yetkiye sahibiz.",
                    "Okulda yönetimsel kararların gerekçeleri bize açık ve şeffaf bir şekilde açıklanır.",
                    "Okulun kaynak tahsisi (personel, bütçe) kararlarında adil davranıldığına inanıyorum.",
                    // 7. Bilişim ve Dijitalleşme
                    "Okulun kullandığı öğrenci yönetim sistemleri (OBS, LMS) kullanıcı dostu ve işimi kolaylaştırmaktadır.",
                    "BT (Bilgi Teknolojileri) desteği, teknik sorunlarımı hızlı ve etkili bir şekilde çözmektedir.",
                    "Dijital platformlar üzerinden velilere not ve geri bildirim gönderme sürecim sorunsuz ilerlemektedir.",
                    "Okul, derslerimde dijital teknolojileri daha etkili kullanmam için sürekli eğitimler sağlamaktadır.",
                    "Okulda öğrenci verilerinin gizliliği ve güvenliği konusunda sıkı protokoller uygulanmaktadır.",
                    // 8. Okul Dışı Hazırlık ve Ödevler
                    "Okul yönetimi, öğretmenler arası ödev yükünü dengelemeye dikkat etmektedir.",
                    "Ödev kontrolü ve geri bildirim için haftalık ders saatleri dışında yeterli zamanım kalmaktadır.",
                    "Velilerin ödevlere gereğinden fazla müdahalesi konusunda okul net bir duruş sergilemektedir.",
                    "Öğrencileri üniversite giriş sınavlarına hazırlama konusunda yeterli kaynak ve desteğe sahibiz.",
                    "Öğretmenlerin özel ders verme kuralları şeffaf ve okul politikalarıyla uyumludur.",
                    // 9. Çeşitlilik ve Kapsayıcılık
                    "Özel gereksinimli öğrenciler için okulda yeterli destek personeli (özel eğitim uzmanı) mevcuttur.",
                    "Okul, farklı kültür ve geçmişten gelen öğrencilerin uyumunu sağlamak için aktif programlar yürütmektedir.",
                    "Kapsayıcı eğitim yöntemleri konusunda düzenli olarak hizmet içi eğitim alıyorum.",
                    "Okuldaki ırk, cinsiyet veya dini ayrımcılık olaylarına karşı yönetim net ve hızlı tepki vermektedir.",
                    "Sınıf içinde sosyo-ekonomik farklılıkların yarattığı öğrenme engellerini aşmak için destekleniyorum.",
                    // 10. Genel Memnuniyet ve Motivasyon
                    "Aldığım ücret ve yan haklar, yaptığım işin sorumluluğu ve piyasa koşulları ile orantılıdır.",
                    "Okulda uzun vadeli kariyer planımı gerçekleştirebileceğime inanıyorum.",
                    "Okulun eğitim felsefesi ve değerleri, kişisel değerlerimle örtüşmektedir.",
                    "Yaptığım işin öğrenci gelişimine etkisini görmek beni motive etmektedir.",
                    "Genel olarak, bu kurumda çalışmaktan memnunum ve kurumuma bağlıyım."
                ],
                "Veli/Ebeveyn": [
                    // 1. Ders İçeriği ve Öğrenme Ortamı
                    "Okulun uyguladığı müfredat (ders içerikleri), çocuğumun gelecekteki başarısı için yeterlidir.",
                    "Okul, yabancı dil eğitimi konusunda etkili ve kalıcı bir öğrenme sağlamaktadır.",
                    "Çocuğumun okulda öğrendiği bilgileri günlük hayatta kullanabildiğini görüyorum.",
                    "Okul, eleştirel düşünme ve problem çözme gibi 21. yüzyıl becerilerini geliştirmeye odaklanmıştır.",
                    "Çocuğumun derslerdeki başarısı ve gelişim hızı konusunda düzenli bilgilendiriliyorum.",
                    // 2. Okul İklimi ve Güvenlik
                    "Çocuğumun okulda güvende olduğundan eminim.",
                    "Okul yönetimi, disiplin ve okul kuralları konusunda velilerle açık iletişim kurmaktadır.",
                    "Okulun fiziksel güvenliği (giriş/çıkış kontrolü, kamera sistemleri) yeterli seviyededir.",
                    "Okul, zorbalık ve olumsuz davranışlar karşısında hızlı ve kararlı bir tutum sergilemektedir.",
                    "Okulun sağlık hizmetleri (revir, ilk yardım) konusunda yeterli donanıma sahip olduğuna inanıyorum.",
                    // 3. Öğretmen Etkileşimi ve Destek
                    "Çocuğumun öğretmenleriyle düzenli ve yapıcı iletişim kurabiliyorum.",
                    "Çocuğumun öğretmenleri, onun bireysel ihtiyaçlarına ve öğrenme tarzına dikkat etmektedir.",
                    "Okul rehberlik servisi, eğitsel ve davranışsal konularda bize somut destek sağlamaktadır.",
                    "Öğretmenler, çocuğumun sorunlarını ve endişelerini ciddiye almaktadır.",
                    "Okul yönetimi, öğretmen ve veli arasındaki işbirliğini teşvik etmektedir.",
                    // 4. Sosyal ve Kültürel Aktiviteler
                    "Okulun sunduğu kulüp ve aktivite seçenekleri çocuğumun ilgi alanlarını desteklemektedir.",
                    "Okulun düzenlediği kültürel etkinlikler ve geziler, çocuğumun sosyal gelişimine katkı sağlamaktadır.",
                    "Okul, velilerin sosyal etkinliklere katılımını ve gönüllülüğünü teşvik etmektedir.",
                    "Okul, öğrenciler arasında takım çalışması ve liderlik becerilerini güçlendirmektedir.",
                    "Çocuğum, okul etkinliklerinde kendine güvenini artıracak fırsatlar bulmaktadır.",
                    // 5. Fiziksel Olanaklar
                    "Okul binası ve sınıflar temiz, modern ve öğrencilerin öğrenme ortamına uygundur.",
                    "Okulun kütüphanesi, çocuğumun araştırma yapması ve okuma alışkanlığı kazanması için yeterlidir.",
                    "Okulun yemek hizmetleri (sağlık, çeşitlilik ve hijyen) beklentilerimi karşılamaktadır.",
                    "Okul bahçesi ve spor alanları, çocuğumun fiziksel aktivite yapması için güvenli ve yeterlidir.",
                    "Okulun genel atmosferi, çocuğumun okula mutlu gitmesini sağlamaktadır.",
                    // 6. Karar Alma Süreçleri ve Katılım
                    "Okulun eğitim politikaları ve hedefleri konusunda veliler olarak yeterince bilgilendiriliyoruz.",
                    "Okulda, velilerin fikirlerini iletebileceği ve yönetimle doğrudan konuşabileceği düzenli platformlar mevcuttur.",
                    "Okulun Veli-Okul Aile Birliği, okulun gelişimine somut katkılar sağlamaktadır.",
                    "Okul yönetimi, önemli değişiklikler (mali, müfredat) hakkında bizi önceden bilgilendirmektedir.",
                    "Çocuğumun okul içindeki durumuyla ilgili kararlara katılımım (özel eğitim, etkinlik vb.) sağlanmaktadır.",
                    // 7. Bilişim ve Dijitalleşme
                    "Okulun kullandığı dijital iletişim platformları, öğretmenlerle hızlı iletişim kurmamı sağlamaktadır.",
                    "Çocuğumun notları ve devamsızlık bilgileri gibi verilere online olarak kolayca erişebiliyorum.",
                    "Okulun dijital ortamları, çocuğumun güvenli bir şekilde öğrenmesini desteklemektedir.",
                    "Okulun web sitesi/mobil uygulaması bilgilendirme ve duyurular için yeterli ve kullanışlıdır.",
                    "Okulun teknolojiye yaptığı yatırımlar, eğitim kalitesini artırmaktadır.",
                    // 8. Okul Dışı Hazırlık ve Ödevler
                    "Çocuğuma verilen ödevlerin miktarı ve faydası dengelidir.",
                    "Okul, çocuğumun ödev yapma sorumluluğunu kazanmasına yardımcı olmaktadır.",
                    "Okul, ek akademik ihtiyaçlar (takviye ders, etüt) konusunda yeterli destek sunmaktadır.",
                    "Okul yönetimi, veli-öğrenci-öğretmen arasındaki ödev dengesini gözetmektedir.",
                    "Okulun sunduğu eğitim, çocuğumun mezun olduktan sonraki hayatına güçlü bir temel oluşturacaktır.",
                    // 9. Çeşitlilik ve Kapsayıcılık
                    "Okul, farklı öğrenme stillerine sahip çocuklara karşı esnek ve destekleyicidir.",
                    "Okul, sosyal ve kültürel çeşitliliğin değerini vurgulayan bir ortam yaratmıştır.",
                    "Okulun kapsayıcılık politikaları (özel gereksinimli öğrenci desteği) yeterli ve etkili uygulanmaktadır.",
                    "Okul, tüm ailelerin kültürel ve dini farklılıklarına saygı göstermektedir.",
                    "Okulda, dezavantajlı öğrenciler için maliyet dışı ek destek programları sunulmaktadır.",
                    // 10. Genel Memnuniyet ve Tavsiye
                    "Bu okulu başka velilere kesinlikle tavsiye ederim.",
                    "Okulun ücret/eğitim kalitesi dengesi tatmin edicidir.",
                    "Okulun mezuniyet sonrası başarıları (üniversite yerleştirme vb.) benim için önemlidir.",
                    "Okul, çocuğumun hem akademik hem de karakter gelişimine katkı sağlamaktadır.",
                    "Genel olarak, çocuğumun bu okulda eğitim almasından ve okulun yönetiminden memnunum."
                ]
        };

        // Sistem verileri
        let systemData = {
            adminPassword: '030714',
            surveyData: null
        };

        // Kategori skorlarını hesaplama fonksiyonu
        function calculateCategoryScores(surveys) {
            const categoryScores = {
                'Öğrenci': {},
                'Öğretmen': {},
                'Veli/Ebeveyn': {}
            };
            
            // Kategori tanımları (her kategori 10 soru)
                const categories = {
                    'Öğrenci': [
                        'Ders İçeriği ve Öğrenme Ortamı',
                        'Okul İklimi ve Güvenlik',
                        'Öğretmen Etkileşimi ve Destek',
                        'Sosyal ve Kültürel Aktiviteler',
                        'Fiziksel Olanaklar',
                        'Karar Alma Süreçleri ve Katılım',
                        'Bilişim ve Dijitalleşme',
                        'Okul Dışı Hazırlık ve Ödevler',
                        'Çeşitlilik ve Kapsayıcılık',
                        'Genel Memnuniyet ve Tavsiye'
                    ],
                    'Öğretmen': [
                        'Ders İçeriği ve Öğrenme Ortamı',
                        'Okul İklimi ve Güvenlik',
                        'Öğretmen Etkileşimi ve Destek',
                        'Sosyal ve Kültürel Aktiviteler',
                        'Fiziksel Olanaklar',
                        'Karar Alma Süreçleri ve Katılım',
                        'Bilişim ve Dijitalleşme',
                        'Okul Dışı Hazırlık ve Ödevler',
                        'Çeşitlilik ve Kapsayıcılık',
                        'Genel Memnuniyet ve Motivasyon'
                    ],
                    'Veli/Ebeveyn': [
                        'Ders İçeriği ve Öğrenme Ortamı',
                        'Okul İklimi ve Güvenlik',
                        'Öğretmen Etkileşimi ve Destek',
                        'Sosyal ve Kültürel Aktiviteler',
                        'Fiziksel Olanaklar',
                        'Karar Alma Süreçleri ve Katılım',
                        'Bilişim ve Dijitalleşme',
                        'Okul Dışı Hazırlık ve Ödevler',
                        'Çeşitlilik ve Kapsayıcılık',
                        'Genel Memnuniyet ve Tavsiye'
                    ]
                };

            // Her pozisyon için kategori skorlarını hesapla
            surveys.forEach(survey => {
                const position = survey.jobType;
                if (!categoryScores[position]) return;
                
                const positionCategories = categories[position];
                if (!positionCategories) return;
                
                positionCategories.forEach((category, categoryIndex) => {
                    if (!categoryScores[position][category]) {
                        categoryScores[position][category] = {
                            'Çok Memnunum': 0,
                            'Memnunum': 0,
                            'Kararsızım': 0,
                            'Memnun Değilim': 0,
                            'Hiç Memnun Değilim': 0,
                            totalCount: 0
                        };
                    }
                    
                    // Her kategoride 10 soru var
                    const startIndex = categoryIndex * 10;
                    const endIndex = startIndex + 10;
                    
                    for (let i = startIndex; i < endIndex && i < survey.answers.length; i++) {
                        const score = survey.answers[i].score;
                        categoryScores[position][category].totalCount++;
                        
                        if (score === 5) categoryScores[position][category]['Çok Memnunum']++;
                        else if (score === 4) categoryScores[position][category]['Memnunum']++;
                        else if (score === 3) categoryScores[position][category]['Kararsızım']++;
                        else if (score === 2) categoryScores[position][category]['Memnun Değilim']++;
                        else if (score === 1) categoryScores[position][category]['Hiç Memnun Değilim']++;
                    }
                });
            });
            
            return categoryScores;
        }

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
            // Google ile giriş zorunluluğu (isletme.html ile birebir)
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
            console.log('Anket başlatma fonksiyonu çalışıyor...');
            
            // Kullanıcı tipi kontrolü
            const userTypeNew = document.getElementById('userTypeNew');
            const userTypeExisting = document.getElementById('userTypeExisting');
            let companyName = '';
            if (userTypeNew && userTypeNew.checked) {
                companyName = document.getElementById('companyName').value.trim();
            } else if (userTypeExisting && userTypeExisting.checked) {
                const existingCompanySelect = document.getElementById('existingCompanySelect');
                if (existingCompanySelect) {
                    companyName = existingCompanySelect.value.trim();
                }
            }
            const disclaimerAccepted = document.getElementById('acceptDisclaimer').checked;
            const firstName = document.getElementById('firstName').value.trim();
            const lastName = document.getElementById('lastName').value.trim();
            
            console.log('Form verileri:', { companyName, selectedJobType, disclaimerAccepted, firstName, lastName });
            
            if (!disclaimerAccepted) {
                showModal('⚠️ Uyarı', 'Devam etmek için veri koruma beyanını kabul etmelisiniz.');
                return;
            }
            // Sadece yeni kullanıcıda kurum adı zorunlu
            if (userTypeNew && userTypeNew.checked && !companyName) {
                showModal('⚠️ Eksik Bilgi', 'Lütfen kurum adını girin.');
                return;
            }
            // Kayıtlı kullanıcıda kurum seçilmediyse uyarı
            if (userTypeExisting && userTypeExisting.checked && !companyName) {
                showModal('⚠️ Eksik Bilgi', 'Lütfen kayıtlı bir kurum seçin.');
                return;
            }
            if (!selectedJobType) {
                showModal('⚠️ Eksik Bilgi', 'Lütfen rolünüzü seçin (Öğrenci, Öğretmen veya Veli/Ebeveyn).');
                return;
            }
            if (!firstName || !lastName) {
                showModal('⚠️ Eksik Bilgi', 'Lütfen adınızı ve soyadınızı girin.');
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



        // Firebase'den verileri yükle
        async function loadFromFirebase() {
            try {
                const response = await fetch(`${FIREBASE_DB_URL}/surveyData.json`);
                if (!response.ok) throw new Error('Firebase veri yükleme hatası');
                const data = await response.json();
                systemData.surveyData = data || {
                    surveyName: "Kurum Değerlendirme Anketi - Sürüm 12",
                    createdAt: new Date().toISOString(),
                    responses: {},
                    statistics: {
                        totalResponses: 0,
                        averageScore: 0,
                        lastUpdated: new Date().toISOString()
                    },
                    companies: {}
                };
                return systemData.surveyData;
            } catch (error) {
                console.error('Firebase yükleme hatası:', error);
                const defaultData = {
                    surveyName: "Kurum Değerlendirme Anketi - Sürüm 12",
                    createdAt: new Date().toISOString(),
                    responses: {},
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

        // Firebase'e PATCH ile veri kaydet (responses nesnesi olarak)
        async function saveToFirebase(patchObj) {
            try {
                const response = await fetch(`${FIREBASE_DB_URL}/surveyData.json`, {
                    method: 'PATCH',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(patchObj)
                });
                if (!response.ok) throw new Error('Firebase veri kaydetme hatası');
                return { success: true };
            } catch (error) {
                console.error('Firebase kayıt hatası:', error);
                return { success: false, error: error.message };
            }
        }

        async function createCompanyIfNotExists(companyName) {
            try {
                console.log('Kurum kontrol ediliyor:', companyName);
                if (!systemData.surveyData) {
                    systemData.surveyData = await loadFromFirebase();
                }
                const existingCompany = Object.entries(systemData.surveyData.companies || {})
                    .find(([key, company]) => company.name.toLowerCase() === companyName.toLowerCase());
                if (existingCompany) {
                    // Eski kurumda status yoksa ekle
                    if (!existingCompany[1].status) {
                        existingCompany[1].status = 'Aktif';
                        await saveToFirebase({ companies: systemData.surveyData.companies });
                    }
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
                    totalResponses: 0,
                    status: 'Aktif'
                };
                const saveResult = await saveToFirebase({ companies: systemData.surveyData.companies });
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
                // Kullanıcı tipi kontrolü
                const userTypeNew = document.getElementById('userTypeNew');
                const userTypeExisting = document.getElementById('userTypeExisting');
                let companyName = '';
                if (userTypeNew && userTypeNew.checked) {
                    companyName = document.getElementById('companyName').value.trim();
                } else if (userTypeExisting && userTypeExisting.checked) {
                    const existingCompanySelect = document.getElementById('existingCompanySelect');
                    if (existingCompanySelect) {
                        companyName = existingCompanySelect.value.trim();
                    }
                }
                const firstName = document.getElementById('firstName').value.trim() || 'Anonim';
                const lastName = document.getElementById('lastName').value.trim() || 'Kullanıcı';
                if (!companyName || !selectedJobType || !answers || answers.length === 0) {
                    showModal('❌ Hata', 'Eksik bilgi: Kurum adı, iş türü ve anket yanıtları gerekli');
                    return;
                }
                const companyResult = await createCompanyIfNotExists(companyName);
                if (!companyResult.success) {
                    showModal('❌ Hata', `Kurum işlemi başarısız: ${companyResult.error}`);
                    return;
                }
                systemData.surveyData = await loadFromFirebase();
                // Benzersiz bir key ile responses nesnesine ekle
                const responseKey = 'survey_' + Date.now() + '_' + Math.random().toString(36).substr(2, 9);
                const surveyResponse = {
                    id: responseKey,
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
                    systemData.surveyData.responses = {};
                }
                systemData.surveyData.responses[responseKey] = surveyResponse;
                // İstatistikleri güncelle
                const allResponses = Object.values(systemData.surveyData.responses);
                if (!systemData.surveyData.statistics) {
                    systemData.surveyData.statistics = {
                        totalResponses: 0,
                        averageScore: 0,
                        lastUpdated: new Date().toISOString()
                    };
                }
                systemData.surveyData.statistics.totalResponses = allResponses.length;
                systemData.surveyData.statistics.averageScore = (
                    allResponses.reduce((sum, r) => sum + parseFloat(r.averageScore), 0) / allResponses.length
                ).toFixed(2);
                systemData.surveyData.statistics.lastUpdated = new Date().toISOString();
                if (companyResult && systemData.surveyData.companies[companyResult.key]) {
                    systemData.surveyData.companies[companyResult.key].totalResponses =
                        allResponses.filter(r =>
                            r.companyName.toLowerCase() === companyName.toLowerCase()
                        ).length;
                }
                // Firebase'e responses, statistics ve companies patch olarak gönder
                const saveResult = await saveToFirebase({
                    responses: systemData.surveyData.responses,
                    statistics: systemData.surveyData.statistics,
                    companies: systemData.surveyData.companies
                });
                if (saveResult.success) {
                    document.getElementById('surveySection').innerHTML = `
                        <div class="text-center bg-green-50 p-10 rounded-lg border-2 border-green-200">
                            <div class="text-8xl mb-6">✅</div>
                            <h2 class="text-3xl font-bold text-green-800 mb-6">Anketiniz Başarıyla Kaydedildi!</h2>
                            <p class="text-green-700 mb-6 text-lg">
                                Değerli görüşleriniz için teşekkür ederiz. Anket yanıtlarınız güvenli bir şekilde <b>Firebase</b> sisteminde saklandı.
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
                    systemData.surveyData = await loadFromFirebase();
                }
                const companyEntry = Object.entries(systemData.surveyData.companies || {})
                    .find(([key, company]) => 
                        company.name.toLowerCase() === companyName.toLowerCase() && 
                        company.password === password
                    );
                if (companyEntry) {
                    // Askıya alınmışsa giriş engelle
                    if (companyEntry[1].status === 'Pasif') {
                        showModal('⛔ Askıya Alındı', 'Bu kurum şu anda askıya alınmış/dondurulmuş. Lütfen yöneticinizle iletişime geçin.');
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
                    showModal('❌ Giriş Hatası', 'Okul/kurum adı veya şifre hatalı. Lütfen yöneticinizden doğru bilgileri alın.');
                }
            } catch (error) {
                showModal('❌ Hata', 'Giriş sırasında bir hata oluştu. Lütfen tekrar deneyin.');
                console.error('Giriş hatası:', error);
            }
        }

        let filteredSurveys = null;
        function loadCompanyDashboard() {
            if (!loggedInCompany || !systemData.surveyData) return;
            document.getElementById('companyNameDisplay').textContent = loggedInCompany.name;
            const allResponses = Object.values(systemData.surveyData.responses || {});
            const companySurveys = allResponses.filter(s => 
                s.companyName && s.companyName.toLowerCase() === loggedInCompany.name.toLowerCase()
            );
            filteredSurveys = null;
            updateDashboardData(companySurveys);
        }

        function filterByDateRange() {
            if (!loggedInCompany || !systemData.surveyData) return;
            const start = document.getElementById('reportStartDate').value;
            const end = document.getElementById('reportEndDate').value;
            const allResponses = Object.values(systemData.surveyData.responses || {});
            const allSurveys = allResponses.filter(s => 
                s.companyName && s.companyName.toLowerCase() === loggedInCompany.name.toLowerCase()
            );
            console.log(`Tarih filtresi - Başlangıç: ${start}, Bitiş: ${end}`);
            console.log('Tüm anketler:', allSurveys.length);
            
            if (!start && !end) {
                filteredSurveys = null;
                console.log('Filtre temizlendi, tüm veriler gösteriliyor');
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
            console.log('Filtrelenmiş anketler:', filtered.length);
            updateDashboardData(filtered);
        }

        function updateDashboardData(surveys) {
            console.log('Dashboard güncelleniyor, anket sayısı:', surveys.length);
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
            console.log('Grafikler yeniden oluşturuluyor...');
            generateCharts(surveys);
            
            // Eğer katılımcı tablosu açıksa onu da güncelle
            const participantDetails = document.getElementById('participantDetails');
            if (participantDetails && !participantDetails.classList.contains('hidden')) {
                loadParticipantTable();
            }
        }

        function generateSimpleReport(surveys) {
            if (surveys.length === 0) {
                document.getElementById('detailedReport').innerHTML = '<p class="text-gray-500 text-center py-8 text-lg">Henüz anket verisi bulunmuyor.</p>';
                return;
            }
            // Pozisyon ve memnuniyet özetleri (eski kod)
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
            let report = `
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

            // Kategori Bazlı Özet Tablosu (Sadece Grup Özetleri)
            const memnuniyetLabels = ['5 - Çok Memnunum', '4 - Memnunum', '3 - Kararsızım', '2 - Memnun Değilim', '1 - Hiç Memnun Değilim'];
            const memnuniyetMap = {5:0, 4:1, 3:2, 2:3, 1:4};
            
            // Kategori tanımları (10'lu yeni yapı)
            const categoryDefinitions = {
                'Öğrenci': [
                    'Ders İçeriği ve Öğrenme Ortamı',
                    'Okul İklimi ve Güvenlik',
                    'Öğretmen Etkileşimi ve Destek',
                    'Sosyal ve Kültürel Aktiviteler',
                    'Fiziksel Olanaklar',
                    'Karar Alma Süreçleri ve Katılım',
                    'Bilişim ve Dijitalleşme',
                    'Okul Dışı Hazırlık ve Ödevler',
                    'Çeşitlilik ve Kapsayıcılık',
                    'Genel Memnuniyet ve Tavsiye'
                ],
                'Öğretmen': [
                    'Ders İçeriği ve Öğrenme Ortamı',
                    'Okul İklimi ve Güvenlik',
                    'Öğretmen Etkileşimi ve Destek',
                    'Sosyal ve Kültürel Aktiviteler',
                    'Fiziksel Olanaklar',
                    'Karar Alma Süreçleri ve Katılım',
                    'Bilişim ve Dijitalleşme',
                    'Okul Dışı Hazırlık ve Ödevler',
                    'Çeşitlilik ve Kapsayıcılık',
                    'Genel Memnuniyet ve Motivasyon'
                ],
                'Veli/Ebeveyn': [
                    'Ders İçeriği ve Öğrenme Ortamı',
                    'Okul İklimi ve Güvenlik',
                    'Öğretmen Etkileşimi ve Destek',
                    'Sosyal ve Kültürel Aktiviteler',
                    'Fiziksel Olanaklar',
                    'Karar Alma Süreçleri ve Katılım',
                    'Bilişim ve Dijitalleşme',
                    'Okul Dışı Hazırlık ve Ödevler',
                    'Çeşitlilik ve Kapsayıcılık',
                    'Genel Memnuniyet ve Tavsiye'
                ]
            };
            
            let detayTablo = `
                <style>
                    .survey-table {
                        width: 100%;
                        border-collapse: collapse;
                        margin-top: 20px;
                        background-color: #fff;
                        box-shadow: 0 2px 5px rgba(0,0,0,0.1);
                    }
                    .survey-table th, .survey-table td {
                        padding: 12px;
                        border: 1px solid #ddd;
                        text-align: left;
                    }
                    .survey-table th {
                        background-color: #004d99;
                        color: white;
                        font-weight: bold;
                        text-align: center;
                    }
                    .survey-table tr:nth-child(even) {
                        background-color: #f2f2f2;
                    }
                    .main-group-row {
                        background-color: #e0eaf6 !important;
                        font-weight: bold;
                        font-size: 1.1em;
                    }
                    .sub-category {
                        padding-left: 30px;
                    }
                </style>
                <div class="mt-8">
                    <table class="survey-table">
                        <thead>
                            <tr>
                                <th>Grup / Kategori</th>
                                <th>5 - Çok Memnunum</th>
                                <th>4 - Memnunum</th>
                                <th>3 - Kararsızım</th>
                                <th>2 - Memnun Değilim</th>
                                <th>1 - Hiç Memnun Değilim</th>
                            </tr>
                        </thead>
                        <tbody>`;
            
            Object.keys(questions).forEach(grup => {
                const grupSurveys = surveys.filter(s => s.jobType === grup);
                console.log(`Grup: ${grup}, Bulunan anket sayısı: ${grupSurveys.length}`);
                if (grupSurveys.length === 0) {
                    // Hiç veri yoksa bile başlık göster
                    detayTablo += `<tr class="bg-blue-50"><td colspan="${memnuniyetLabels.length + 1}" class="border p-3 font-bold text-lg text-blue-800">📊 ${grup} Kategorileri (Veri Yok)</td></tr>`;
                    return;
                }
                
                // Ana grup satırı (yüzdeli)
                const grupToplamCevap = grupSurveys.length * (questions[grup]?.length || 0);
                const grupGenelCounts = [0,0,0,0,0];
                grupSurveys.forEach(s => {
                    (s.answers||[]).forEach(a => {
                        if (memnuniyetMap[a.score] !== undefined) grupGenelCounts[memnuniyetMap[a.score]]++;
                    });
                });
                
                detayTablo += `<tr class="main-group-row">
                    <td>${grup}</td>`;
                    
                if (grupToplamCevap > 0) {
                    grupGenelCounts.forEach(c => {
                        const yuzde = ((c/grupToplamCevap)*100).toFixed(1);
                        detayTablo += `<td style="text-align: center;">${yuzde}%</td>`;
                    });
                } else {
                    grupGenelCounts.forEach(_ => detayTablo += `<td style="text-align: center;">0.0%</td>`);
                }
                detayTablo += `</tr>`;
                
                // Her kategori için ayrı satır
                const categories = categoryDefinitions[grup] || [];
                categories.forEach((categoryName, categoryIndex) => {
                    const kategoriCounts = [0,0,0,0,0];
                    let toplamKategoriCevap = 0;
                    
                    grupSurveys.forEach(s => {
                        // Her kategoride 10 soru var
                        const startIndex = categoryIndex * 10;
                        const endIndex = startIndex + 10;
                        
                        for (let i = startIndex; i < endIndex && i < s.answers.length; i++) {
                            const score = s.answers[i].score;
                            if (memnuniyetMap[score] !== undefined) {
                                kategoriCounts[memnuniyetMap[score]]++;
                                toplamKategoriCevap++;
                            }
                        }
                    });
                    
                    detayTablo += `<tr>
                        <td class="sub-category flex items-center justify-between">
                            <span>${categoryName}</span>
                            <button style="display:none" class="ml-2 px-2 py-1 bg-blue-100 text-blue-700 rounded text-xs border border-blue-200 hover:bg-blue-200 transition" onclick="showCategoryDetailModal('${grup.replace(/'/g, '')}','${categoryName.replace(/'/g, '')}')">Detay Göster</button>
                        </td>`;
                    if (toplamKategoriCevap > 0) {
                        kategoriCounts.forEach(count => {
                            detayTablo += `<td style="text-align: center;">${count}</td>`;
                        });
                    } else {
                        kategoriCounts.forEach(_ => detayTablo += `<td style="text-align: center;">0</td>`);
                    }
                    detayTablo += `</tr>`;
                });
            });
            detayTablo += `</tbody></table></div>`;
            
            // Grafik alanı ekle
            const totalResponses = surveys.length;
            const chartTitle = totalResponses > 0 ? 
                `📊 Kategori Bazlı Memnuniyet Dağılımı (${totalResponses} Katılımcı)` : 
                '📊 Kategori Bazlı Memnuniyet Dağılımı (Veri Yok)';
                
            const chartSection = `
                <div class="mt-8 bg-white border rounded-lg p-6" style="box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
                    <h3 class="text-lg font-semibold mb-4 text-gray-800">${chartTitle}</h3>
                    <div style="width: 100%; height: 400px;">
                        <canvas id="categoryChart"></canvas>
                    </div>
                    ${totalResponses === 0 ? 
                        '<p class="text-gray-500 text-center mt-4">Henüz anket verisi bulunmuyor. Grafik veriler geldiğinde otomatik olarak güncellenecektir.</p>' : 
                        '<p class="text-gray-600 text-sm text-center mt-4">Grafik tüm kategorilerden gelen cevapları birleştirerek genel memnuniyet dağılımını gösterir.</p>'
                    }
                </div>
            `;
            
            report += detayTablo + chartSection;
            
            // AI butonunu ekle
            report += `
                <div class="mt-6 flex flex-col md:flex-row gap-2 items-center justify-center">
                    <button id="aiInterpretBtn" class="bg-gradient-to-r from-blue-600 to-purple-600 text-white px-6 py-3 rounded-lg font-bold text-sm hover:from-blue-700 hover:to-purple-700 transition-all duration-300 transform hover:scale-105 shadow-lg">
                        🤖 Eğitim Raporu AI ile Yorumla
                    </button>
                </div>
            `;
            
            document.getElementById('detailedReport').innerHTML = report;
            
            // AI buton eventini ekle
            setTimeout(() => {
                const btn = document.getElementById('aiInterpretBtn');
                if (btn) btn.onclick = async function() {
                    const apiKey = 'AIzaSyDD9lwyo2viTAbHRy2nFpKZ0fUMGggS_ao';
                    btn.disabled = true;
                    btn.textContent = '🔄 AI analiz yapıyor...';
                    try {
                        // Eğitim anket özetini hazırla
                        const summary = document.getElementById('detailedReport').innerText.slice(0, 2000);
                        const prompt = `Bir eğitim uzmanı ve pedagog gibi aşağıdaki eğitim kurumu değerlendirme anket raporunu analiz et.\n\nRapor Özeti:\n${summary}\n\nAşağıdaki başlıklarla detaylı, profesyonel ve eğitim odaklı bir analiz yaz:\n\n1. Mevcut Eğitim Durumu\n2. Eğitimde Nelerin İyileştirilmesi Gerekiyor\n3. Bu Durumun Devam Etmesi Halinde Eğitim Kalitesine Etkileri\n\nHer başlık için en az 3-4 cümlelik, eğitim pedagojisine uygun, özgün ve uygulanabilir öneriler içeren bir metin oluştur.`;
                        const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent`, {
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
                        btn.textContent = '🤖 Eğitim Raporu AI ile Yorumla';
                    }
                }
            }, 500);
            
            // Grafik oluştur
            generateCategoryChart(surveys);
        }

        let categoryChartInstance = null;

        function generateCategoryChart(surveys) {
            try {
                console.log('generateCategoryChart çalışıyor, survey sayısı:', surveys ? surveys.length : 0);
                
                // Önce survey verilerinin yapısını inceleyelim
                if (surveys && surveys.length > 0) {
                    console.log('İlk survey örneği:', surveys[0]);
                    console.log('Survey keys:', Object.keys(surveys[0]));
                    if (surveys[0].answers && surveys[0].answers.length > 0) {
                        console.log('İlk answer örneği:', surveys[0].answers[0]);
                        console.log('Answer keys:', Object.keys(surveys[0].answers[0]));
                    }
                }
                
                // Mevcut grafiği temizle
                if (categoryChartInstance) {
                    categoryChartInstance.destroy();
                    categoryChartInstance = null;
                }

                const canvas = document.getElementById('categoryChart');
                if (!canvas) {
                    console.log('categoryChart canvas bulunamadı');
                    return;
                }

            // Kategori verilerini hazırla
            const categoryData = {
                'Eğitim İçeriği': [0, 0, 0, 0, 0], // [Çok Memnun, Memnun, Kararsız, Memnun Değil, Hiç Memnun Değil]
                'Eğitmen Performansı': [0, 0, 0, 0, 0],
                'Eğitim Ortamı': [0, 0, 0, 0, 0],
                'Katılımcı Memnuniyeti': [0, 0, 0, 0, 0],
                'Genel Değerlendirme': [0, 0, 0, 0, 0]
            };

            // Survey verilerinden kategori puanlarını hesapla - Daha basit yaklaşım
            if (surveys && surveys.length > 0) {
                console.log('Kategori grafiği için işlenen survey sayısı:', surveys.length);
                
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
                        
                        // Tüm kategorilere aynı puanı ekle (genel memnuniyet için)
                        Object.keys(categoryData).forEach(categoryName => {
                            categoryData[categoryName][satisfactionIndex]++;
                        });
                        
                        console.log(`Survey ${surveyIndex}: avgScore=${avgScore}, satisfactionIndex=${satisfactionIndex}`);
                    }
                });
            }

            // Toplam memnuniyet verilerini hesapla (tüm kategorilerin toplamı)
            const totalSatisfaction = [0, 0, 0, 0, 0];
            Object.values(categoryData).forEach(catData => {
                catData.forEach((count, index) => {
                    totalSatisfaction[index] += count;
                });
            });
            
            console.log('Kategori verileri:', categoryData);
            console.log('Toplam memnuniyet dağılımı:', totalSatisfaction);

            // Grafik verilerini hazırla
            const chartData = {
                labels: ['Çok Memnun', 'Memnun', 'Kararsız', 'Memnun Değil', 'Hiç Memnun Değil'],
                datasets: [{
                    label: 'Genel Memnuniyet Dağılımı',
                    data: totalSatisfaction,
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

            // Grafik ayarları (örneğinizdeki gibi)
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
                                text: 'Cevap Sayısı'
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
                                    const total = totalSatisfaction.reduce((a, b) => a + b, 0);
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
            categoryChartInstance = new Chart(ctx, config);
            
            } catch (error) {
                console.error('Kategori grafiği oluşturma hatası:', error);
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
                // responses hem dizi hem nesne olabileceği için Object.values ile normalize et
                let responsesRaw = systemData.surveyData.responses || [];
                let responses = Array.isArray(responsesRaw) ? responsesRaw : Object.values(responsesRaw);
                document.getElementById('totalCompanies').textContent = Object.keys(companies).length;
                document.getElementById('activeSurveys').textContent = Object.keys(companies).length;
                document.getElementById('totalUsers').textContent = responses.length;
                loadCompanyList(responses);
            } catch (error) {
                console.error('Admin dashboard yükleme hatası:', error);
                showModal('❌ Hata', 'Yönetici paneli yüklenirken hata oluştu.');
            }
        }

        function loadCompanyList(responses) {
            const tbody = document.getElementById('companyList');
            if (!systemData.surveyData || !systemData.surveyData.companies) {
                tbody.innerHTML = '<tr><td colspan="5" class="text-center py-4 text-gray-500">Henüz kurum kaydı bulunmuyor.</td></tr>';
                return;
            }
            const companies = systemData.surveyData.companies;
            // responses parametresi gelmezse fallback olarak eski kodu kullan
            if (!responses) {
                let responsesRaw = systemData.surveyData.responses || [];
                responses = Array.isArray(responsesRaw) ? responsesRaw : Object.values(responsesRaw);
            }
            // Şirketleri alfabetik sırala
            const sortedCompanies = Object.entries(companies).sort((a, b) => {
                const nameA = a[1].name.toLowerCase();
                const nameB = b[1].name.toLowerCase();
                return nameA.localeCompare(nameB, 'tr');
            });
            // Filtre uygula
            let search = '';
            const searchInput = document.getElementById('companySearchInput');
            if (searchInput) search = searchInput.value.trim().toLowerCase();
            const filtered = sortedCompanies.filter(([_, company]) =>
                !search || company.name.toLowerCase().includes(search)
            );
            if (filtered.length === 0) {
                tbody.innerHTML = '<tr><td colspan="5" class="text-center py-4 text-gray-500">Aramanıza uygun kurum bulunamadı.</td></tr>';
                return;
            }
            tbody.innerHTML = filtered.map(([companyKey, company]) => {
                const companySurveys = responses.filter(s =>
                    s.companyName && s.companyName.toLowerCase() === company.name.toLowerCase()
                );
                const status = company.status === 'Pasif' ? 'Pasif' : 'Aktif';
                const statusColor = status === 'Aktif' ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800';
                return `
                    <tr class="border-b hover:bg-gray-50">
                        <td class="px-4 py-3 font-medium">${company.name}</td>
                        <td class="px-4 py-3">
                            <code class="bg-gray-100 px-2 py-1 rounded text-sm">${company.password}</code>
                        </td>
                        <td class="px-4 py-3">${companySurveys.length}</td>
                        <td class="px-4 py-3">
                            <span class="px-2 py-1 rounded-full text-xs ${statusColor}">
                                ${status === 'Aktif' ? '🟢 Aktif' : '⛔ Pasif'}
                            </span>
                        </td>
                        <td class="px-4 py-3">
                            <button onclick="showAdminCompanyReport('${company.name}')" class="text-green-600 hover:text-green-800 mr-2">📊 Rapor</button>
                            <button onclick="resetCompanyPassword('${companyKey}')" class="text-orange-600 hover:text-orange-800 mr-2">🔄 Şifre</button>
                            <button onclick="toggleCompanyStatus('${companyKey}')" class="text-xs font-bold px-2 py-1 rounded ${status === 'Aktif' ? 'bg-red-500 hover:bg-red-600 text-white' : 'bg-green-500 hover:bg-green-600 text-white'}">
                                ${status === 'Aktif' ? 'Askıya Al' : 'Aktif Et'}
                            </button>
                        </td>
                    </tr>
                `;
            }).join('');
        }

// Admin: Kurum durumunu değiştir (Aktif/Pasif) -- GLOBAL SCOPE
async function toggleCompanyStatus(companyKey) {
    if (!systemData.surveyData || !systemData.surveyData.companies[companyKey]) return;
    const company = systemData.surveyData.companies[companyKey];
    company.status = company.status === 'Aktif' ? 'Pasif' : 'Aktif';

}

        // Canlı filtreleme için
        function filterCompanyList() {
            loadCompanyList();
        }

        async function resetCompanyPassword(companyKey) {
            if (!systemData.surveyData || !systemData.surveyData.companies[companyKey]) return;
            
            const newPassword = generateCompanyPassword();
            systemData.surveyData.companies[companyKey].password = newPassword;
            

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
                    systemData.surveyData = await loadFromFirebase();
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
            
            let totalScore = 0;
            let totalAnswers = 0;
            surveys.forEach(s => {
                totalScore += s.totalScore;
                totalAnswers += s.answers.length;
            });
            const avgScore = totalAnswers > 0 ? (totalScore / totalAnswers).toFixed(1) : '0.0';
            
            // Profesyonel memnuniyet yüzdesi hesaplama (50-250 puan arası)
            const minPossibleScore = totalAnswers * 1; // Her soru minimum 1 puan
            const maxPossibleScore = totalAnswers * 5; // Her soru maksimum 5 puan
            const satisfactionPercentage = totalAnswers > 0 ? 
                Math.round(((totalScore - minPossibleScore) / (maxPossibleScore - minPossibleScore)) * 100) : 0;
            
            // Pozisyon bazlı analiz
            const positionData = {};
            const positionScores = {};
            surveys.forEach(s => {
                positionData[s.jobType] = (positionData[s.jobType] || 0) + 1;
                if (!positionScores[s.jobType]) positionScores[s.jobType] = [];
                positionScores[s.jobType].push(parseFloat(s.averageScore));
            });
            
            // Kategori bazlı özet tablo verilerini hazırla
            const categoryAnalysis = calculateCategoryScores(surveys);
            
            // Genel durum analizi
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
                <body onload="window.print()">
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
                        <h3>👥 Pozisyon Bazlı Katılım</h3>
                        <table>
                            <tr><th>Pozisyon</th><th>Katılımcı Sayısı</th><th>Oran</th></tr>
                            ${Object.entries(positionData).map(([pos, count]) => 
                                `<tr><td>${pos}</td><td>${count}</td><td>%${Math.round((count/totalParticipants)*100)}</td></tr>`
                            ).join('')}
                        </table>
                    </div>
                    
                    ${Object.entries(categoryAnalysis).map(([position, categories]) => {
                        if (Object.keys(categories).length === 0) return '';
                        
                        return `<div class="section"><h3>📊 ${position} - Kategori Bazlı Memnuniyet Özeti</h3><table><tr><th>Kategori</th><th>Çok Memnunum</th><th>Memnunum</th><th>Kararsızım</th><th>Memnun Değilim</th><th>Hiç Memnun Değilim</th><th>Toplam</th></tr>${Object.entries(categories).map(([categoryName, scores]) => `<tr><td><strong>${categoryName}</strong></td><td>${scores['Çok Memnunum']} (%${scores.totalCount > 0 ? Math.round((scores['Çok Memnunum']/scores.totalCount)*100) : 0})</td><td>${scores['Memnunum']} (%${scores.totalCount > 0 ? Math.round((scores['Memnunum']/scores.totalCount)*100) : 0})</td><td>${scores['Kararsızım']} (%${scores.totalCount > 0 ? Math.round((scores['Kararsızım']/scores.totalCount)*100) : 0})</td><td>${scores['Memnun Değilim']} (%${scores.totalCount > 0 ? Math.round((scores['Memnun Değilim']/scores.totalCount)*100) : 0})</td><td>${scores['Hiç Memnun Değilim']} (%${scores.totalCount > 0 ? Math.round((scores['Hiç Memnun Değilim']/scores.totalCount)*100) : 0})</td><td>${scores.totalCount}</td></tr>`).join('')}</table></div>`;
                    }).join('')}
                    
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

        // Mevcut grafikleri saklamak için
        let existingCharts = {};

        function createEmptyCharts() {
            console.log('Boş grafikler oluşturuluyor...');
            
            // Pozisyon grafiği - boş
            const positionCtx = document.getElementById('positionChart').getContext('2d');
            existingCharts.position = new Chart(positionCtx, {
                type: 'doughnut',
                data: {
                    labels: ['Veri Yok'],
                    datasets: [{
                        data: [1],
                        backgroundColor: ['#e5e7eb']
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

            // Değerlendirme grafiği - boş
            const satisfactionCtx = document.getElementById('satisfactionChart').getContext('2d');
            existingCharts.satisfaction = new Chart(satisfactionCtx, {
                type: 'bar',
                data: {
                    labels: ['Düşük', 'Orta', 'Yüksek'],
                    datasets: [{
                        data: [0, 0, 0],
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
                        y: { beginAtZero: true, max: 1 }
                    }
                }
            });

            // Süre grafiği - boş
            const timeCtx = document.getElementById('timeChart').getContext('2d');
            existingCharts.time = new Chart(timeCtx, {
                type: 'pie',
                data: {
                    labels: ['Veri Yok'],
                    datasets: [{
                        data: [1],
                        backgroundColor: ['#e5e7eb']
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

            // Puan grafiği - boş
            const trendCtx = document.getElementById('trendChart').getContext('2d');
            existingCharts.trend = new Chart(trendCtx, {
                type: 'line',
                data: {
                    labels: ['1-2', '2-3', '3-4', '4-5'],
                    datasets: [{
                        data: [0, 0, 0, 0],
                        borderColor: '#3b82f6',
                        backgroundColor: 'transparent',
                        tension: 0.4
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: false }
                    },
                    scales: {
                        y: { beginAtZero: true, max: 1 }
                    }
                }
            });
        }

        function generateCharts(surveys) {
            console.log('generateCharts çalışıyor, anket sayısı:', surveys.length);
            
            try {
                // Mevcut grafikleri yok et
                Object.values(existingCharts).forEach(chart => {
                    if (chart) chart.destroy();
                });
                existingCharts = {};
                
                // Eğer veri yoksa boş grafikler oluştur
                if (!surveys || surveys.length === 0) {
                    createEmptyCharts();
                    return;
                }
                
                // Pozisyon grafiği
            const positionData = {};
            surveys.forEach(s => {
                positionData[s.jobType] = (positionData[s.jobType] || 0) + 1;
            });
            console.log('Pozisyon verileri:', positionData);
            
            const positionCtx = document.getElementById('positionChart').getContext('2d');
            existingCharts.position = new Chart(positionCtx, {
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
            console.log('Memnuniyet verileri:', satisfactionCounts);
            
            const satisfactionCtx = document.getElementById('satisfactionChart').getContext('2d');
            existingCharts.satisfaction = new Chart(satisfactionCtx, {
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
            console.log('Süre dağılımı verileri:', timeCounts);
            
            const timeCtx = document.getElementById('timeChart').getContext('2d');
            existingCharts.time = new Chart(timeCtx, {
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
            console.log('Puan dağılımı verileri:', scoreRanges);
            
            const trendCtx = document.getElementById('trendChart').getContext('2d');
            existingCharts.trend = new Chart(trendCtx, {
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
            
            } catch (error) {
                console.error('Grafik oluşturma hatası:', error);
                createEmptyCharts();
            }
        }

        function toggleParticipantDetails() {
            const detailsDiv = document.getElementById('participantDetails');
            const toggleBtn = document.getElementById('toggleParticipantsBtn');
            
            if (detailsDiv.classList.contains('hidden')) {
                detailsDiv.classList.remove('hidden');
                loadParticipantTable();
                // Buton metnini katılımcı sayısıyla güncelle
                const participantCount = getParticipantCount();
                toggleBtn.textContent = `📋 Katılımcıları Gizle (${participantCount})`;
            } else {
                detailsDiv.classList.add('hidden');
                toggleBtn.textContent = '📋 Katılımcıları Görüntüle';
            }
        }

        function getParticipantCount() {
            if (!loggedInCompany || !systemData.surveyData) return 0;
            
            if (filteredSurveys) {
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
            if (filteredSurveys) {
                companySurveys = filteredSurveys;
            } else {
                const allResponses = Object.values(systemData.surveyData.responses || {});
                companySurveys = allResponses.filter(s => 
                    s.companyName && s.companyName.toLowerCase() === loggedInCompany.name.toLowerCase()
                );
            }
            
            const tbody = document.getElementById('participantTableBody');
            
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

        function loadDemoData() {
            // Demo veri yükleme fonksiyonu
        }
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'981af265f22bd620',t:'MTc1ODMwNDQ1MS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>

