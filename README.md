<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>人生目的ポートフォリオ診断</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Chart.js -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700;900&display=swap');
    body {
      font-family: 'Noto Sans JP', sans-serif;
      background-color: #f8fafc;
      color: #0f172a;
    }
    input[type=range]::-webkit-slider-thumb {
      height: 24px;
      width: 24px;
      border-radius: 50%;
      background: #2563eb;
      cursor: pointer;
      -webkit-appearance: none;
      margin-top: -8px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.2);
    }
    input[type=range]::-webkit-slider-runnable-track {
      width: 100%;
      height: 8px;
      cursor: pointer;
      background: #cbd5e1;
      border-radius: 4px;
    }
  </style>
</head>
<body class="min-h-screen flex flex-col justify-between">

  <!-- Header (スリム化・コンパクト化) -->
  <header class="bg-slate-900 text-white py-2.5 px-4 shadow-md sticky top-0 z-50">
    <div class="max-w-2xl mx-auto flex items-center justify-between">
      <div class="flex items-center space-x-2">
        <div class="bg-blue-600 p-1.5 rounded-lg flex items-center justify-center">
          <i data-lucide="compass" class="w-4 h-4 text-white"></i>
        </div>
        <span class="text-sm font-bold tracking-wide">人生目的ポートフォリオ</span>
      </div>
      <span class="text-xs text-slate-400 font-medium">Life Portfolio Diagnostic</span>
    </div>
  </header>

  <!-- Main Content Container -->
  <main class="flex-grow max-w-2xl w-full mx-auto p-4 sm:p-6">

    <!-- Screen 1: Start Screen -->
    <div id="screen-start" class="bg-white rounded-2xl p-6 sm:p-8 shadow-sm border border-slate-200 text-center mt-4">
      <div class="w-16 h-16 bg-blue-50 rounded-full flex items-center justify-center mx-auto mb-4 text-blue-600">
        <i data-lucide="pie-chart" class="w-8 h-8"></i>
      </div>
      <h1 class="text-2xl font-black text-slate-900 mb-3">人生目的ポートフォリオ診断</h1>
      <p class="text-sm text-slate-700 leading-relaxed mb-6">
        価値観や人生の目的を可視化し、現状の「エネルギー投資」とのギャップを特定するための対話ツールです。
      </p>
      
      <div class="bg-slate-50 p-4 rounded-xl text-left text-xs text-slate-700 space-y-2 mb-6 border border-slate-200">
        <div class="flex items-center text-slate-900 font-bold mb-1">
          <i data-lucide="check-circle-2" class="w-4 h-4 mr-1 text-blue-600"></i> 診断の流れ
        </div>
        <p>1. 20個の問いに直感で答える（重要度と満足度）</p>
        <p>2. 現在割いている「6カテゴリーのエネルギー配分」を入力（合計100%）</p>
        <p>3. 「目的シェア vs エネルギー配分」のギャップを分析</p>
      </div>

      <button id="btn-start" onclick="startDiagnostic()" class="w-full py-4 bg-blue-600 hover:bg-blue-700 active:scale-[0.99] text-white font-bold text-base rounded-xl shadow-lg transition-all flex items-center justify-center">
        <span>診断を開始する</span>
        <i data-lucide="arrow-right" class="w-5 h-5 ml-2"></i>
      </button>
    </div>

    <!-- Screen 2: Questions (20項目シャッフル出題) -->
    <div id="screen-questions" class="hidden space-y-4">
      <!-- Progress Bar & Indicator -->
      <div class="bg-white p-3 rounded-xl shadow-sm border border-slate-200 flex items-center justify-between">
        <div class="flex items-center space-x-2">
          <span id="cat-badge" class="px-2.5 py-1 rounded-full text-xs font-bold bg-blue-100 text-blue-800">
            カテゴリ
          </span>
          <span id="q-number" class="text-xs font-bold text-slate-700">1 / 20</span>
        </div>
        <div class="w-28 sm:w-40 bg-slate-200 h-2 rounded-full overflow-hidden">
          <div id="progress-bar" class="bg-blue-600 h-full w-0 transition-all duration-300"></div>
        </div>
      </div>

      <!-- Question Card -->
      <div class="bg-white rounded-2xl p-5 sm:p-7 shadow-sm border border-slate-200">
        <!-- Question Header Icon & Label -->
        <div class="flex items-center justify-center mb-3">
          <div id="q-icon-box" class="w-12 h-12 rounded-xl flex items-center justify-center text-xl">
            <!-- Icon rendered by JS -->
          </div>
        </div>
        
        <h2 id="q-text" class="text-base sm:text-lg font-bold text-center text-slate-900 leading-snug mb-6 min-h-[3rem] flex items-center justify-center">
          質問文がここに入ります
        </h2>

        <!-- Slider 1: Importance (重要度) -->
        <div class="mb-6 bg-slate-50 p-4 rounded-xl border border-slate-200">
          <div class="flex justify-between items-center mb-2">
            <label class="text-xs sm:text-sm font-bold text-slate-900 flex items-center">
              <i data-lucide="star" class="w-4 h-4 mr-1 text-amber-500 fill-amber-500"></i>
              重要度（どれくらい大切か）
            </label>
            <span id="val-imp" class="text-lg font-black text-blue-600">5</span>
          </div>
          <input type="range" id="slider-imp" min="1" max="10" value="5" oninput="updateSliderVal('imp')" class="w-full">
          <div class="flex justify-between text-[10px] text-slate-500 font-bold mt-1">
            <span>1: あまり重視しない</span>
            <span>10: 最も重要</span>
          </div>
        </div>

        <!-- Slider 2: Satisfaction (満足度) -->
        <div class="mb-6 bg-slate-50 p-4 rounded-xl border border-slate-200">
          <div class="flex justify-between items-center mb-2">
            <label class="text-xs sm:text-sm font-bold text-slate-900 flex items-center">
              <i data-lucide="smile" class="w-4 h-4 mr-1 text-emerald-500"></i>
              現状満足度（どの程度満たされているか）
            </label>
            <span id="val-sat" class="text-lg font-black text-blue-600">5</span>
          </div>
          <input type="range" id="slider-sat" min="1" max="10" value="5" oninput="updateSliderVal('sat')" class="w-full">
          <div class="flex justify-between text-[10px] text-slate-500 font-bold mt-1">
            <span>1: 全く不満</span>
            <span>10: 非常に満足</span>
          </div>
        </div>

        <!-- Navigation Buttons -->
        <div class="flex space-x-3 pt-2">
          <button id="btn-prev" onclick="prevQuestion()" class="w-1/3 py-3 bg-slate-100 hover:bg-slate-200 text-slate-700 font-bold text-sm rounded-xl transition flex items-center justify-center">
            <i data-lucide="arrow-left" class="w-4 h-4 mr-1"></i> 戻る
          </button>
          <button id="btn-next" onclick="nextQuestion()" class="w-2/3 py-3 bg-blue-600 hover:bg-blue-700 active:scale-[0.99] text-white font-bold text-sm rounded-xl shadow-md transition flex items-center justify-center">
            <span id="btn-next-text">次へ進む</span>
            <i data-lucide="arrow-right" class="w-4 h-4 ml-1"></i>
          </button>
        </div>
      </div>
    </div>

    <!-- Screen 3: Energy Allocation (STEP 2: 5%刻みエネルギー配分) -->
    <div id="screen-energy" class="hidden space-y-4">
      <div class="bg-blue-900 text-white rounded-2xl p-5 shadow-sm">
        <span class="bg-blue-600 text-xs font-bold px-2.5 py-1 rounded-full uppercase tracking-wider">STEP 2</span>
        <h2 class="text-lg sm:text-xl font-bold mt-2">現在の「エネルギー配分」</h2>
        <p class="text-xs text-blue-100 mt-1 leading-relaxed">
          現在、日々の時間・情熱・意識などのリソースを6カテゴリーにどれくらい割いているか、直感で入力してください（合計100%）。
        </p>
      </div>

      <!-- 100% Status Card -->
      <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm flex items-center justify-between sticky top-14 z-40">
        <div>
          <div class="text-xs font-bold text-slate-500">合計配分</div>
          <div id="energy-total-text" class="text-2xl font-black text-slate-900">0% / 100%</div>
        </div>
        <div id="energy-status-badge" class="px-3 py-1.5 rounded-full text-xs font-bold bg-amber-100 text-amber-800 flex items-center">
          <i data-lucide="alert-circle" class="w-4 h-4 mr-1"></i> 残り 100%
        </div>
      </div>

      <!-- Energy Inputs List -->
      <div id="energy-input-list" class="space-y-3">
        <!-- Rendered by JS -->
      </div>

      <!-- Submit Energy Button -->
      <button id="btn-submit-energy" onclick="finishDiagnostic()" disabled class="w-full py-4 bg-slate-300 text-slate-500 cursor-not-allowed font-bold text-base rounded-xl shadow-md transition flex items-center justify-center">
        <span>診断結果を見る</span>
        <i data-lucide="check" class="w-5 h-5 ml-2"></i>
      </button>
    </div>

    <!-- Screen 4: Results -->
    <div id="screen-results" class="hidden space-y-6">
      
      <!-- Top Title -->
      <div class="bg-slate-900 text-white p-5 rounded-2xl shadow-sm text-center">
        <h2 class="text-xl font-black">人生目的ポートフォリオ診断結果</h2>
        <p class="text-xs text-slate-300 mt-1">目的シェア（理想） vs エネルギー配分（現状）</p>
      </div>

      <!-- Chart Card -->
      <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-200">
        <h3 class="text-sm font-bold text-slate-900 mb-3 flex items-center">
          <i data-lucide="bar-chart-3" class="w-4 h-4 mr-1.5 text-blue-600"></i>
          目的シェア（理想） vs エネルギー（現状） 比較
        </h3>
        <div class="relative w-full h-64 sm:h-72">
          <canvas id="resultChart"></canvas>
        </div>
      </div>

      <!-- Category Comparison List -->
      <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-200 space-y-4">
        <h3 class="text-sm font-bold text-slate-900 flex items-center">
          <i data-lucide="layers" class="w-4 h-4 mr-1.5 text-blue-600"></i>
          カテゴリー別 詳細ギャップ分析
        </h3>
        <div id="cat-comparison-list" class="space-y-3">
          <!-- Rendered by JS -->
        </div>
      </div>

      <!-- Key Insights / Gap Analysis Card -->
      <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-200 space-y-3">
        <h3 class="text-sm font-bold text-slate-900 flex items-center">
          <i data-lucide="compass" class="w-4 h-4 mr-1.5 text-blue-600"></i>
          重要なギャップ（注力すべきポイント）
        </h3>
        <div id="gap-insights" class="space-y-2">
          <!-- Rendered by JS -->
        </div>
      </div>

      <!-- Action Buttons -->
      <div class="flex flex-col sm:flex-row gap-3 pt-2">
        <button onclick="window.print()" class="w-full sm:w-1/2 py-3.5 bg-slate-800 hover:bg-slate-900 text-white font-bold text-sm rounded-xl shadow-md transition flex items-center justify-center">
          <i data-lucide="printer" class="w-4 h-4 mr-2"></i> 結果を印刷 / PDF保存
        </button>
        <button onclick="restartDiagnostic()" class="w-full sm:w-1/2 py-3.5 bg-blue-600 hover:bg-blue-700 text-white font-bold text-sm rounded-xl shadow-md transition flex items-center justify-center">
          <i data-lucide="rotate-ccw" class="w-4 h-4 mr-2"></i> 再度診断する
        </button>
      </div>

    </div>

  </main>

  <!-- Footer -->
  <footer class="text-center py-4 text-xs text-slate-500 border-t border-slate-200 bg-white">
    © Wealth Management Advisory - Life Portfolio Diagnostic
  </footer>

  <!-- App Script -->
  <script>
    // 6 Categories Master Data
    const CATEGORIES = {
      family: { name: "家族・人間関係", color: "#ef4444", icon: "heart", bg: "bg-red-50", text: "text-red-600" },
      asset:  { name: "事業・資産形成", color: "#2563eb", icon: "trending-up", bg: "bg-blue-50", text: "text-blue-600" },
      health: { name: "健康・ウェルビーイング", color: "#10b981", icon: "activity", bg: "bg-emerald-50", text: "text-emerald-600" },
      life:   { name: "趣味・ライフスタイル", color: "#f59e0b", icon: "plane", bg: "bg-amber-50", text: "text-amber-600" },
      growth: { name: "自己探求・精神的成長", color: "#8b5cf6", icon: "book-open", bg: "bg-purple-50", text: "text-purple-600" },
      legacy: { name: "社会貢献・レガシー", color: "#06b6d4", icon: "award", bg: "bg-cyan-50", text: "text-cyan-600" }
    };

    // 20 Questions Data
    const QUESTIONS_MASTER = [
      { id: 1, cat: "family", text: "パートナーや家族と深い信頼関係を築き、十分な時間を過ごすこと", icon: "heart" },
      { id: 2, cat: "family", text: "子供や次世代の成長・教育を支援し、見守ること", icon: "users" },
      { id: 3, cat: "family", text: "親族や家族の絆を深める行事やイベントを大切にすること", icon: "home" },

      { id: 4, cat: "asset", text: "ビジネスや事業で成果を出し、経済的な成長を続けること", icon: "briefcase" },
      { id: 5, cat: "asset", text: "資産を効率的に運用・保全し、将来への経済的自由を確保すること", icon: "line-chart" },
      { id: 6, cat: "asset", text: "自分の専門性やビジネス上のインフルエンスを高めること", icon: "award" },

      { id: 7, cat: "health", text: "身体の健康を維持し、高いエネルギーで日々を過ごすこと", icon: "activity" },
      { id: 8, cat: "health", text: "心のリフレッシュやストレス管理のための十分な静寂を持つこと", icon: "sun" },
      { id: 9, cat: "health", text: "食生活や睡眠、運動などの質の高いライフスタイル習慣を保つこと", icon: "smile" },

      { id: 10, cat: "life", text: "趣味や関心のある分野（スポーツ、車、芸術等）を追求すること", icon: "compass" },
      { id: 11, cat: "life", text: "新しい景色や文化に触れる旅行・体験を楽しむこと", icon: "plane" },
      { id: 12, cat: "life", text: "心地よい住環境や所有物、上質な時間空間を享受すること", icon: "coffee" },

      { id: 13, cat: "growth", text: "新しい知識やスキル、学びを継続的に吸収すること", icon: "book-open" },
      { id: 14, cat: "growth", text: "自らの哲学や美意識、内面的な精神性を深めること", icon: "feather" },
      { id: 15, cat: "growth", text: "未知の分野や新しいプロジェクトへ勇敢に挑戦すること", icon: "zap" },

      { id: 16, cat: "legacy", text: "社会や地域コミュニティ、文化の発展に寄与すること", icon: "globe" },
      { id: 17, cat: "legacy", text: "寄付やフィルアンソロピー活動を通じて他者を支援すること", icon: "gift" },
      { id: 18, cat: "legacy", text: "自分の意志や価値観、資産を次世代にレガシーとして受け継ぐこと", icon: "shield" },

      { id: 19, cat: "family", text: "信頼できる親友や仲間と本音で高め合える関係性を持つこと", icon: "user-check" },
      { id: 20, cat: "asset", text: "事業承継やファミリーオフィス等の体制を盤石に整えること", icon: "building" }
    ];

    // App State Variables
    let shuffledQuestions = [];
    let currentIndex = 0;
    let userAnswers = {}; // { qId: { imp: number, sat: number } }
    let energyAllocations = { family: 15, asset: 25, health: 15, life: 15, growth: 15, legacy: 15 }; // Default sum=100%
    let myChartInstance = null;

    // Initialize App
    window.addEventListener('DOMContentLoaded', () => {
      lucide.createIcons();
    });

    // Start Diagnostic Function
    function startDiagnostic() {
      // Shuffle Questions Order
      shuffledQuestions = [...QUESTIONS_MASTER].sort(() => Math.random() - 0.5);
      currentIndex = 0;
      userAnswers = {};

      document.getElementById('screen-start').classList.add('hidden');
      document.getElementById('screen-questions').classList.remove('hidden');
      renderQuestion();
    }

    // Render Current Question
    function renderQuestion() {
      const q = shuffledQuestions[currentIndex];
      const cat = CATEGORIES[q.cat];

      // Update Header Badges
      const catBadge = document.getElementById('cat-badge');
      catBadge.textContent = cat.name;
      catBadge.className = `px-2.5 py-1 rounded-full text-xs font-bold ${cat.bg} ${cat.text}`;

      document.getElementById('q-number').textContent = `${currentIndex + 1} / ${shuffledQuestions.length}`;
      document.getElementById('progress-bar').style.width = `${((currentIndex + 1) / shuffledQuestions.length) * 100}%`;

      // Update Icon
      const iconBox = document.getElementById('q-icon-box');
      iconBox.className = `w-12 h-12 rounded-xl flex items-center justify-center ${cat.bg} ${cat.text}`;
      iconBox.innerHTML = `<i data-lucide="${q.icon}" class="w-6 h-6"></i>`;

      // Question Text
      document.getElementById('q-text').textContent = q.text;

      // Restore Saved Answer or Set Default
      const existing = userAnswers[q.id] || { imp: 5, sat: 5 };
      document.getElementById('slider-imp').value = existing.imp;
      document.getElementById('slider-sat').value = existing.sat;
      document.getElementById('val-imp').textContent = existing.imp;
      document.getElementById('val-sat').textContent = existing.sat;

      // Prev Button State
      document.getElementById('btn-prev').style.visibility = currentIndex === 0 ? 'hidden' : 'visible';

      // Next Button Label
      document.getElementById('btn-next-text').textContent = (currentIndex === shuffledQuestions.length - 1) ? "エネルギー入力へ" : "次へ進む";

      lucide.createIcons();
    }

    // Update Slider Value Label
    function updateSliderVal(type) {
      if (type === 'imp') {
        document.getElementById('val-imp').textContent = document.getElementById('slider-imp').value;
      } else {
        document.getElementById('val-sat').textContent = document.getElementById('slider-sat').value;
      }
    }

    // Save Current Answer
    function saveCurrentAnswer() {
      const q = shuffledQuestions[currentIndex];
      userAnswers[q.id] = {
        imp: parseInt(document.getElementById('slider-imp').value, 10),
        sat: parseInt(document.getElementById('slider-sat').value, 10)
      };
    }

    // Next Question Button
    function nextQuestion() {
      saveCurrentAnswer();
      if (currentIndex < shuffledQuestions.length - 1) {
        currentIndex++;
        renderQuestion();
      } else {
        // Questions completed -> Move to Energy Allocation Screen
        document.getElementById('screen-questions').classList.add('hidden');
        document.getElementById('screen-energy').classList.remove('hidden');
        renderEnergyScreen();
      }
    }

    // Previous Question Button
    function prevQuestion() {
      saveCurrentAnswer();
      if (currentIndex > 0) {
        currentIndex--;
        renderQuestion();
      }
    }

    // Render STEP 2 Energy Screen
    function renderEnergyScreen() {
      const listContainer = document.getElementById('energy-input-list');
      listContainer.innerHTML = '';

      Object.keys(CATEGORIES).forEach(catKey => {
        const cat = CATEGORIES[catKey];
        const val = energyAllocations[catKey] || 0;

        const card = document.createElement('div');
        card.className = "bg-white p-4 rounded-xl border border-slate-200 shadow-sm space-y-2";
        card.innerHTML = `
          <div class="flex justify-between items-center">
            <div class="flex items-center space-x-2">
              <span class="p-1.5 rounded-lg ${cat.bg} ${cat.text}">
                <i data-lucide="${cat.icon}" class="w-4 h-4"></i>
              </span>
              <span class="text-sm font-bold text-slate-900">${cat.name}</span>
            </div>
            <span class="text-lg font-black text-slate-900" id="energy-val-${catKey}">${val}%</span>
          </div>
          <div class="flex items-center space-x-3">
            <button onclick="adjustEnergy('${catKey}', -5)" class="w-10 h-10 bg-slate-100 hover:bg-slate-200 active:bg-slate-300 text-slate-800 rounded-lg font-black text-lg flex items-center justify-center shadow-sm">
              -
            </button>
            <input type="range" min="0" max="100" step="5" value="${val}" id="energy-range-${catKey}" oninput="onEnergySliderChange('${catKey}')" class="flex-grow">
            <button onclick="adjustEnergy('${catKey}', 5)" class="w-10 h-10 bg-slate-100 hover:bg-slate-200 active:bg-slate-300 text-slate-800 rounded-lg font-black text-lg flex items-center justify-center shadow-sm">
              +
            </button>
          </div>
        `;
        listContainer.appendChild(card);
      });

      lucide.createIcons();
      updateEnergyTotal();
    }

    // Adjust Energy Allocation Button +/-
    function adjustEnergy(catKey, delta) {
      let current = energyAllocations[catKey] || 0;
      let next = Math.min(100, Math.max(0, current + delta));
      energyAllocations[catKey] = next;
      document.getElementById(`energy-range-${catKey}`).value = next;
      document.getElementById(`energy-val-${catKey}`).textContent = `${next}%`;
      updateEnergyTotal();
    }

    // Energy Slider Event
    function onEnergySliderChange(catKey) {
      const val = parseInt(document.getElementById(`energy-range-${catKey}`).value, 10);
      energyAllocations[catKey] = val;
      document.getElementById(`energy-val-${catKey}`).textContent = `${val}%`;
      updateEnergyTotal();
    }

    // Update Energy Total & Validate 100%
    function updateEnergyTotal() {
      const total = Object.values(energyAllocations).reduce((a, b) => a + b, 0);
      const totalText = document.getElementById('energy-total-text');
      const badge = document.getElementById('energy-status-badge');
      const btn = document.getElementById('btn-submit-energy');

      totalText.textContent = `${total}% / 100%`;

      if (total === 100) {
        totalText.className = "text-2xl font-black text-emerald-600";
        badge.className = "px-3 py-1.5 rounded-full text-xs font-bold bg-emerald-100 text-emerald-800 flex items-center";
        badge.innerHTML = `<i data-lucide="check-circle" class="w-4 h-4 mr-1"></i> ぴったり100%`;
        btn.disabled = false;
        btn.className = "w-full py-4 bg-blue-600 hover:bg-blue-700 active:scale-[0.99] text-white font-bold text-base rounded-xl shadow-lg transition flex items-center justify-center cursor-pointer";
      } else {
        totalText.className = "text-2xl font-black text-amber-600";
        const diff = 100 - total;
        badge.className = "px-3 py-1.5 rounded-full text-xs font-bold bg-amber-100 text-amber-800 flex items-center";
        badge.innerHTML = `<i data-lucide="alert-circle" class="w-4 h-4 mr-1"></i> ${diff > 0 ? `残り ${diff}%` : `${Math.abs(diff)}% 超過`}`;
        btn.disabled = true;
        btn.className = "w-full py-4 bg-slate-300 text-slate-500 cursor-not-allowed font-bold text-base rounded-xl shadow-md transition flex items-center justify-center";
      }
      lucide.createIcons();
    }

    // Finish Diagnostic -> Show Results
    function finishDiagnostic() {
      document.getElementById('screen-energy').classList.add('hidden');
      document.getElementById('screen-results').classList.remove('hidden');

      // Calculate Category Importance Sum & Share %
      let catImpSum = { family: 0, asset: 0, health: 0, life: 0, growth: 0, legacy: 0 };
      let grandTotalImp = 0;

      QUESTIONS_MASTER.forEach(q => {
        const ans = userAnswers[q.id] || { imp: 5, sat: 5 };
        catImpSum[q.cat] += ans.imp;
        grandTotalImp += ans.imp;
      });

      // Purpose Share (%)
      let purposeShare = {};
      Object.keys(CATEGORIES).forEach(catKey => {
        purposeShare[catKey] = grandTotalImp > 0 ? Math.round((catImpSum[catKey] / grandTotalImp) * 100) : 0;
      });

      // Render Chart
      renderChart(purposeShare, energyAllocations);

      // Render Detailed Comparison List
      renderComparisonList(purposeShare, energyAllocations);

      // Render Gap Insights
      renderGapInsights(purposeShare, energyAllocations);
    }

    // Render Comparison Chart (Chart.js Bar Chart)
    function renderChart(purposeShare, energy) {
      const ctx = document.getElementById('resultChart').getContext('2d');
      if (myChartInstance) myChartInstance.destroy();

      const labels = Object.keys(CATEGORIES).map(k => CATEGORIES[k].name);
      const purposeData = Object.keys(CATEGORIES).map(k => purposeShare[k]);
      const energyData = Object.keys(CATEGORIES).map(k => energy[k]);

      myChartInstance = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: labels,
          datasets: [
            {
              label: '目的シェア (理想 %)',
              data: purposeData,
              backgroundColor: '#2563eb',
              borderRadius: 6
            },
            {
              label: 'エネルギー配分 (現状 %)',
              data: energyData,
              backgroundColor: '#f59e0b',
              borderRadius: 6
            }
          ]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: { position: 'top', labels: { font: { family: 'Noto Sans JP', size: 11 } } }
          },
          scales: {
            y: { beginAtZero: true, max: 50, ticks: { callback: v => v + '%' } },
            x: { ticks: { font: { family: 'Noto Sans JP', size: 10 } } }
          }
        }
      });
    }

    // Render Detailed Comparison List
    function renderComparisonList(purposeShare, energy) {
      const container = document.getElementById('cat-comparison-list');
      container.innerHTML = '';

      Object.keys(CATEGORIES).forEach(catKey => {
        const cat = CATEGORIES[catKey];
        const pVal = purposeShare[catKey];
        const eVal = energy[catKey];
        const gap = eVal - pVal; // positive = excess energy, negative = insufficient energy

        let gapBadge = '';
        if (Math.abs(gap) <= 5) {
          gapBadge = `<span class="px-2 py-0.5 rounded text-[11px] font-bold bg-slate-100 text-slate-700">調和</span>`;
        } else if (gap > 5) {
          gapBadge = `<span class="px-2 py-0.5 rounded text-[11px] font-bold bg-amber-100 text-amber-800">エネルギー過多 (+${gap}%)</span>`;
        } else {
          gapBadge = `<span class="px-2 py-0.5 rounded text-[11px] font-bold bg-blue-100 text-blue-800">エネルギー不足 (${gap}%)</span>`;
        }

        const item = document.createElement('div');
        item.className = "bg-slate-50 p-3.5 rounded-xl border border-slate-200 space-y-2";
        item.innerHTML = `
          <div class="flex justify-between items-center">
            <div class="flex items-center space-x-2">
              <span class="p-1 rounded ${cat.bg} ${cat.text}">
                <i data-lucide="${cat.icon}" class="w-4 h-4"></i>
              </span>
              <span class="text-xs font-bold text-slate-900">${cat.name}</span>
            </div>
            ${gapBadge}
          </div>
          <div class="grid grid-cols-2 gap-2 text-xs">
            <div class="bg-white p-2 rounded border border-slate-200 text-center">
              <span class="text-slate-500 block text-[10px]">目的シェア (理想)</span>
              <span class="text-sm font-black text-blue-600">${pVal}%</span>
            </div>
            <div class="bg-white p-2 rounded border border-slate-200 text-center">
              <span class="text-slate-500 block text-[10px]">エネルギー (現状)</span>
              <span class="text-sm font-black text-amber-600">${eVal}%</span>
            </div>
          </div>
        `;
        container.appendChild(item);
      });

      lucide.createIcons();
    }

    // Render Gap Insights (Focus Areas)
    function renderGapInsights(purposeShare, energy) {
      const container = document.getElementById('gap-insights');
      container.innerHTML = '';

      // Calculate gaps for all categories
      let gaps = Object.keys(CATEGORIES).map(catKey => {
        return {
          catKey: catKey,
          name: CATEGORIES[catKey].name,
          pVal: purposeShare[catKey],
          eVal: energy[catKey],
          gap: energy[catKey] - purposeShare[catKey] // positive = excess, negative = deficit
        };
      });

      // Sort by absolute gap descending
      gaps.sort((a, b) => Math.abs(b.gap) - Math.abs(a.gap));

      gaps.slice(0, 3).forEach(g => {
        if (Math.abs(g.gap) < 5) return;

        const isDeficit = g.gap < 0;
        const card = document.createElement('div');
        card.className = `p-3.5 rounded-xl border ${isDeficit ? 'bg-blue-50/60 border-blue-200' : 'bg-amber-50/60 border-amber-200'} space-y-1`;
        
        card.innerHTML = `
          <div class="flex items-center justify-between text-xs font-bold">
            <span class="${isDeficit ? 'text-blue-900' : 'text-amber-900'}">${g.name}</span>
            <span class="${isDeficit ? 'text-blue-700' : 'text-amber-700'}">${isDeficit ? `理想に対し ${Math.abs(g.gap)}% 不足` : `理想に対し +${g.gap}% 投入集中`}</span>
          </div>
          <p class="text-xs text-slate-700 leading-relaxed">
            ${isDeficit 
              ? `「${g.name}」は人生の目的シェア(${g.pVal}\%)が高い一方で、現在割けているエネルギー(${g.eVal}%)が不足しています。優先的にリソースをシフトする余地があります。`
              : `「${g.name}」には目的シェア(${g.pVal}\%)以上に多くのエネルギー(${g.eVal}%)が費やされています。効率化や仕組み化でエネルギーを解放できる可能性があります。`}
          </p>
        `;
        container.appendChild(card);
      });

      if (container.children.length === 0) {
        container.innerHTML = `<div class="text-xs text-slate-500 text-center py-2">全体として目的シェアとエネルギー配分が非常によく調和しています。</div>`;
      }
    }

    // Restart Diagnostic
    function restartDiagnostic() {
      document.getElementById('screen-results').classList.add('hidden');
      document.getElementById('screen-start').classList.remove('hidden');
    }
  </script>
</body>
</html>
