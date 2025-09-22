<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Akça Pro X - Kurum Değerlendirme Anketi</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
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
                            <button onclick="showPDFReport(true)" class="px-3 py-1 bg-red-600 text-white rounded hover:bg-red-700 text-sm" style="display:none">📄 PDF Göster (Filtreli)</button>
                            <button onclick="showPDFReport(false)" class="px-3 py-1 bg-gray-600 text-white rounded hover:bg-gray-700 text-sm" style="display:none">📄 PDF Göster (Tümü)</button>
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


        // Firebase Realtime Database ayarları
        const FIREBASE_DB_URL = 'https://akcaprox-anket-default-rtdb.europe-west1.firebasedatabase.app';
        // responses artık bir nesne olarak tutulacak (array değil)

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
            
            const companyName = document.getElementById('companyName').value.trim();
            const disclaimerAccepted = document.getElementById('acceptDisclaimer').checked;
            const firstName = document.getElementById('firstName').value.trim();
            const lastName = document.getElementById('lastName').value.trim();
            
            console.log('Form verileri:', { companyName, selectedJobType, disclaimerAccepted, firstName, lastName });
            
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

            // Detaylı memnuniyet dağılımı tablosu (başlıkta yüzdelik, sorularda rakam)
            const memnuniyetLabels = ['Çok Memnunum', 'Memnun', 'Kararsızım', 'Memnun Değilim', 'Hiç Memnun Değilim'];
            const memnuniyetMap = {5:0, 4:1, 3:2, 2:3, 1:4};
            const groups = Object.keys(questions);
            let detayTablo = `<div class="overflow-x-auto mt-8"><table class="min-w-full text-xs text-center border border-gray-300 bg-white"><thead><tr><th class="border p-2">Grup / Soru</th>${memnuniyetLabels.map(l=>`<th class="border p-2">${l}</th>`).join('')}</tr></thead><tbody>`;
            groups.forEach(grup => {
                // Grup başlığı için yüzdelik dağılım
                const grupSurveys = surveys.filter(s => s.jobType === grup);
                const toplamCevap = grupSurveys.length * (questions[grup]?.length || 0);
                const grupCounts = [0,0,0,0,0];
                grupSurveys.forEach(s => {
                    (s.answers||[]).forEach(a => {
                        if (memnuniyetMap[a.score] !== undefined) grupCounts[memnuniyetMap[a.score]]++;
                    });
                });
                detayTablo += `<tr class="bg-gray-100 font-bold"><td class="border p-2">${grup}</td>`;
                if (toplamCevap > 0) {
                    grupCounts.forEach(c => {
                        const yuzde = ((c/toplamCevap)*100).toFixed(1);
                        detayTablo += `<td class="border p-2">${yuzde}%</td>`;
                    });
                } else {
                    grupCounts.forEach(_ => detayTablo += `<td class="border p-2">0.0%</td>`);
                }
                detayTablo += `</tr>`;
                // Her soru için cevap sayısı
                questions[grup].forEach((soru, idx) => {
                    const counts = [0,0,0,0,0];
                    grupSurveys.forEach(s => {
                        if (s.answers && s.answers[idx] && s.answers[idx].score) {
                            const score = s.answers[idx].score;
                            if (memnuniyetMap[score] !== undefined) counts[memnuniyetMap[score]]++;
                        }
                    });
                    detayTablo += `<tr><td class="border p-2 text-left">${soru.replace(/<[^>]+>/g, '').replace(/\s*\p{Emoji_Presentation}/gu, '').slice(0,60)}${soru.length>60?'...':''}</td>`;
                    counts.forEach(c => detayTablo += `<td class="border p-2">${c}</td>`);
                    detayTablo += `</tr>`;
                });
            });
            detayTablo += `</tbody></table></div>`;
            report += detayTablo;
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
                    s.companyName.toLowerCase() === company.name.toLowerCase()
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
    // JSONBin kaldırıldı, burada bir işlem yapılmıyor (Firebase kullanılıyor)
    if (saveResult.success) {
        loadCompanyList();
    } else {
        showModal('❌ Hata', 'Durum güncellenemedi: ' + saveResult.error);
    }
}

        // Canlı filtreleme için
        function filterCompanyList() {
            loadCompanyList();
        }

        async function resetCompanyPassword(companyKey) {
            if (!systemData.surveyData || !systemData.surveyData.companies[companyKey]) return;
            
            const newPassword = generateCompanyPassword();
            systemData.surveyData.companies[companyKey].password = newPassword;
            
            // JSONBin kaldırıldı, burada bir işlem yapılmıyor (Firebase kullanılıyor)
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
            const educationCategories = [
                { title: '1. Eğitim İçeriği ve Kalitesi', desc: 'Eğitim programının kapsamı, güncelliği ve uygulama yeterliliği.' },
                { title: '2. Eğitmen Performansı', desc: 'Eğitmenlerin bilgi düzeyi, iletişimi ve katılımcı ile etkileşimi.' },
                { title: '3. Fiziksel ve Dijital Ortam', desc: 'Eğitim ortamının konforu, teknik altyapı ve materyal kalitesi.' },
                { title: '4. Katılımcı Memnuniyeti', desc: 'Katılımcıların genel memnuniyeti, beklenti karşılanması ve öneriler.' },
                { title: '5. Genel Değerlendirme ve Tavsiye', desc: 'Eğitimin genel başarısı, tekrar tercih etme ve tavsiye etme eğilimleri.' }
            ];
            return `
            <html><head><title>${companyName} - Eğitim Anketi Raporu</title>
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
                    <div style='font-size:2.2rem;font-weight:bold;margin-bottom:8px;'>🏫 ${companyName}</div>
                    <div style='font-size:1.3rem;font-weight:bold;'>Eğitim Değerlendirme Anketi Raporu${dateInfo}</div>
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
                            <td style="border:1px solid #a3a3a3;padding:10px;vertical-align:top;">• Yüksek katılımcı memnuniyeti<br>• Güçlü eğitmen kadrosu<br>• Modern eğitim altyapısı</td>
                            <td style="border:1px solid #a3a3a3;padding:10px;vertical-align:top;">• Yoğun dönemlerde iletişim eksikliği<br>• Kısıtlı sosyal etkinlikler<br>• Dijital materyal eksikliği</td>
                            <td style="border:1px solid #a3a3a3;padding:10px;vertical-align:top;">• Dijitalleşme yatırımları<br>• Yeni eğitim programları<br>• Kamu destekleri</td>
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
                    <div style='margin-top:8px;'>Eğitiminiz için ${dateInfo ? 'seçilen tarih için' : 'tüm katılımcılarda'} genel memnuniyet düzeyi yukarıda gösterilmiştir.</div>
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
                    ${educationCategories.map(cat => `
                        <div class='category-box'>
                            <b>${cat.title}</b><br>
                            <span style='font-size:0.95rem;'>${cat.desc}</span>
                            <div style='margin-top:8px;background:#fee2e2;padding:8px;border-radius:6px;'><b>Puan Aralığı: Düşük (%0-50)</b> - Bu kategoride ciddi iyileştirme gereklidir.</div>
                        </div>
                    `).join('')}
                </div>
                <div class='section advice-box'>
                    <b>💡 Öneriler ve Eylem Planı</b><br>
                    <b>Öncelikli Aksiyonlar:</b> Eğitim içeriği, eğitmen performansı ve ortam koşulları gözden geçirilmeli.<br>
                    <b>Takip:</b> Bu rapor sonuçlarını 3-6 ay sonra tekrar değerlendirmek için yeni anket düzenleyiniz.
                </div>
                <div style='text-align:right;font-size:0.9rem;color:#888;margin-top:32px;'>Akça Pro X - Kurum Değerlendirme Anketi | ${dateStr} ${timeStr}<br>Bu rapor ${totalAnswers} adet soru yanıtı analiz edilerek oluşturulmuştur.${dateInfo ? `<br>Filtre: ${dateInfo}` : ''}</div>
            </body></html>
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

