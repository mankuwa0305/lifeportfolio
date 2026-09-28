<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>人生目的ポートフォリオ診断</title>
    
    <!-- PWA & Mobile Web App Meta Tags -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="人生ポートフォリオ">
    <meta name="theme-color" content="#4f46e5">

    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#eef2ff',
                            100: '#e0e7ff',
                            500: '#6366f1',
                            600: '#4f46e5',
                            700: '#4338ca',
                        }
                    }
                }
            }
        }
    </script>

    <!-- FontAwesome & Chart.js -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <style>
        /* Touch friendly slider customizations */
        input[type=range] {
            -webkit-appearance: none;
            width: 100%;
            background: transparent;
            touch-action: manipulation;
        }
        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none;
            height: 32px;
            width: 32px;
            border-radius: 50%;
            background: #4f46e5;
            cursor: pointer;
            margin-top: -12px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
            border: 2px solid #ffffff;
        }
        input[type=range]::-webkit-slider-runnable-track {
            width: 100%;
            height: 10px;
            cursor: pointer;
            background: #cbd5e1;
            border-radius: 5px;
        }
        *, ::before, ::after {
            box-sizing: border-box;
        }
        html, body {
            overflow-x: hidden;
            overscroll-behavior-y: none;
            -webkit-tap-highlight-color: transparent;
            width: 100%;
            margin: 0;
            padding: 0;
        }
    </style>
</head>
<body class="bg-slate-100 text-slate-900 font-sans min-h-screen flex flex-col antialiased select-none overflow-x-hidden w-full">

    <!-- Header / App Bar -->
    <header id="app-header" class="bg-indigo-700 text-white sticky top-0 z-50 shadow-sm py-2 px-4 w-full">
        <div class="w-full flex items-center justify-between">
            <div class="flex items-center space-x-1.5">
                <i class="fa-solid fa-compass text-amber-300 text-sm"></i>
                <h1 class="font-bold text-xs tracking-wide opacity-90">人生ポートフォリオ</h1>
            </div>
            <button onclick="toggleHistoryModal()" class="text-[11px] bg-indigo-800 hover:bg-indigo-900 px-2.5 py-1 rounded-full flex items-center space-x-1 border border-indigo-400/40 text-white font-medium">
                <i class="fa-solid fa-clock-rotate-left text-[10px]"></i>
                <span>履歴</span>
            </button>
        </div>
    </header>

    <!-- Main Content Container (横幅一杯レイアウト) -->
    <main class="flex-1 w-full p-4 pb-20 box-border overflow-x-hidden">

        <!-- STEP 1: Welcome & Intro View -->
        <section id="view-intro" class="space-y-6 text-center py-4 w-full">
            <div class="bg-white rounded-3xl p-6 shadow-md border border-slate-200 space-y-4 w-full">
                <div class="w-20 h-20 bg-indigo-100 rounded-3xl flex items-center justify-center mx-auto text-indigo-600 text-4xl shadow-inner">
                    <i class="fa-solid fa-bullseye"></i>
                </div>
                <h2 class="text-2xl font-extrabold text-slate-900">あなたの「人生の目的」を<br>可視化しましょう</h2>
                <p class="text-sm text-slate-700 leading-relaxed font-medium">
                    バイアスを防ぐため全20項目をランダムな順序で出題！「重要度」と「現在の満足度」を診断し、構成比とギャップを分析します。
                </p>
                <div class="pt-2">
                    <button onclick="startDiagnosis()" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-4 px-6 rounded-2xl shadow-lg shadow-indigo-200 active:scale-95 transition text-lg flex items-center justify-center space-x-2">
                        <span>診断をスタートする</span>
                        <i class="fa-solid fa-arrow-right"></i>
                    </button>
                </div>
            </div>
        </section>

        <!-- STEP 2: Questionnaire Wizard View -->
        <section id="view-wizard" class="hidden space-y-4 w-full">
            <!-- Progress Bar -->
            <div class="bg-white px-4 py-3 rounded-2xl shadow-sm border border-slate-200 space-y-2 w-full">
                <div class="flex justify-between items-center text-sm font-bold text-slate-700">
                    <span id="wizard-category-name" class="text-indigo-700 font-extrabold text-base">カテゴリー</span>
                    <span id="wizard-progress-text" class="text-slate-800 text-sm">1 / 20 項目</span>
                </div>
                <div class="w-full bg-slate-200 rounded-full h-3 overflow-hidden">
                    <div id="wizard-progress-bar" class="bg-indigo-600 h-3 rounded-full transition-all duration-300" style="width: 5%"></div>
                </div>
            </div>

            <!-- Question Card -->
            <div id="question-card" class="bg-white rounded-3xl p-5 shadow-md border border-slate-200 space-y-5 text-center w-full">
                
                <!-- Category Badge & Title Area -->
                <div class="flex flex-col items-center space-y-2">
                    <span id="item-category-badge" class="inline-block bg-indigo-100 text-indigo-800 text-xs font-extrabold px-3 py-1 rounded-full border border-indigo-200">
                        カテゴリー名
                    </span>
                    
                    <!-- Visual Illustration Icon -->
                    <div id="item-icon-container" class="w-16 h-16 rounded-2xl flex items-center justify-center text-2xl shadow-xs my-1 border">
                        <i id="item-icon" class="fa-solid fa-heart"></i>
                    </div>

                    <div class="space-y-1">
                        <h3 id="item-title" class="text-2xl font-black text-slate-900 tracking-tight">項目タイトル</h3>
                        <p id="item-desc" class="text-sm text-slate-600 font-bold leading-snug px-2">項目の補足説明</p>
                    </div>
                </div>

                <!-- Slider 1: Importance -->
                <div class="space-y-3 bg-slate-50 p-4 rounded-2xl border border-slate-200 text-left w-full">
                    <div class="flex justify-between items-center">
                        <label class="text-sm font-extrabold text-slate-900 flex items-center space-x-1.5">
                            <i class="fa-solid fa-star text-amber-500 text-base"></i>
                            <span>重要度（どれくらい重視するか）</span>
                        </label>
                        <span id="val-importance" class="text-xl font-black text-indigo-700 bg-white px-3.5 py-1 rounded-xl border border-slate-300 shadow-xs">5</span>
                    </div>
                    <input type="range" id="input-importance" min="1" max="10" value="5" step="1" oninput="updateSliderVal('importance')">
                    <div class="flex justify-between text-xs text-slate-600 font-bold px-1">
                        <span>1: 低い</span>
                        <span>5: 普通</span>
                        <span>10: 非常に高い</span>
                    </div>
                </div>

                <!-- Slider 2: Satisfaction -->
                <div class="space-y-3 bg-slate-50 p-4 rounded-2xl border border-slate-200 text-left w-full">
                    <div class="flex justify-between items-center">
                        <label class="text-sm font-extrabold text-slate-900 flex items-center space-x-1.5">
                            <i class="fa-solid fa-face-smile text-emerald-600 text-base"></i>
                            <span>現在の満足度（満たされているか）</span>
                        </label>
                        <span id="val-satisfaction" class="text-xl font-black text-emerald-700 bg-white px-3.5 py-1 rounded-xl border border-slate-300 shadow-xs">5</span>
                    </div>
                    <input type="range" id="input-satisfaction" min="1" max="10" value="5" step="1" oninput="updateSliderVal('satisfaction')">
                    <div class="flex justify-between text-xs text-slate-600 font-bold px-1">
                        <span>1: 不満</span>
                        <span>5: 普通</span>
                        <span>10: 大満足</span>
                    </div>
                </div>
            </div>

            <!-- Controls -->
            <div class="flex items-center justify-between gap-3 pt-1 w-full">
                <button id="btn-prev" onclick="prevQuestion()" class="flex-1 bg-white hover:bg-slate-100 border-2 border-slate-300 text-slate-800 font-extrabold py-4 px-4 rounded-2xl text-base transition flex items-center justify-center space-x-1 shadow-sm active:scale-95">
                    <i class="fa-solid fa-chevron-left text-sm"></i>
                    <span>前へ</span>
                </button>
                <button id="btn-next" onclick="nextQuestion()" class="flex-2 bg-indigo-600 hover:bg-indigo-700 text-white font-extrabold py-4 px-6 rounded-2xl text-lg shadow-md shadow-indigo-200 transition flex items-center justify-center space-x-2 active:scale-95">
                    <span id="btn-next-text">次へ進む</span>
                    <i class="fa-solid fa-chevron-right text-sm"></i>
                </button>
            </div>
        </section>

        <!-- STEP 3: Results View (画面幅いっぱいのフルサイズレイアウト) -->
        <section id="view-result" class="hidden space-y-5 w-full box-border">
            
            <!-- Result Title Header & Top Action Bar -->
            <div class="bg-gradient-to-r from-indigo-700 to-indigo-900 text-white p-5 rounded-3xl shadow-lg space-y-2 relative w-full box-border">
                <div class="flex justify-between items-start">
                    <div>
                        <div class="text-[11px] opacity-90 uppercase tracking-widest font-extrabold">Diagnosis Result</div>
                        <h2 class="text-xl font-black mt-0.5">人生目的ポートフォリオ分析</h2>
                    </div>
                    <!-- 履歴ボタン -->
                    <button onclick="toggleHistoryModal()" class="text-[11px] bg-white/20 hover:bg-white/30 px-3 py-1.5 rounded-full flex items-center space-x-1.5 border border-white/30 text-white font-bold backdrop-blur-xs">
                        <i class="fa-solid fa-clock-rotate-left text-[10px]"></i>
                        <span>履歴</span>
                    </button>
                </div>
                <p class="text-xs text-indigo-100 font-medium">あなたの意識の重きと現状のバランス結果です。</p>
            </div>

            <!-- TOP TYPE COMMENT SECTION -->
            <div id="top-type-card" class="bg-amber-50 border-2 border-amber-300 p-5 rounded-3xl space-y-3 shadow-sm w-full box-border">
                <!-- JS Dynamic Inject -->
            </div>

            <!-- Ranking Table: Share & Average Importance -->
            <div class="bg-white p-5 rounded-3xl shadow-sm border border-slate-200 space-y-4 w-full box-border">
                <h3 class="font-extrabold text-base text-slate-900 flex items-center space-x-2">
                    <i class="fa-solid fa-list-ol text-indigo-600"></i>
                    <span>人生目的のシェア（重要度順位）</span>
                </h3>

                <!-- Doughnut Chart -->
                <div class="relative w-48 h-48 mx-auto my-2">
                    <canvas id="doughnutChart"></canvas>
                </div>

                <!-- Ranking Table -->
                <div class="w-full overflow-x-auto">
                    <table class="w-full text-left text-sm">
                        <thead>
                            <tr class="border-b-2 border-slate-200 text-slate-700 font-extrabold text-xs">
                                <th class="py-2.5 px-1 text-center whitespace-nowrap">順位</th>
                                <th class="py-2.5 px-2 whitespace-nowrap">カテゴリー</th>
                                <th class="py-2.5 px-1 text-right whitespace-nowrap">平均重要度</th>
                                <th class="py-2.5 px-1 text-right whitespace-nowrap">シェア</th>
                            </tr>
                        </thead>
                        <tbody id="ranking-table-body" class="divide-y divide-slate-200">
                            <!-- JS Dynamic Inject -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- Radar Chart: Importance vs Satisfaction -->
            <div class="bg-white p-5 rounded-3xl shadow-sm border border-slate-200 space-y-3 w-full box-border">
                <h3 class="font-bold text-base text-slate-800 flex items-center space-x-2">
                    <i class="fa-solid fa-chart-radar text-indigo-500"></i>
                    <span>重要度 vs 満足度バランス</span>
                </h3>
                <div class="relative w-full max-w-sm mx-auto p-2">
                    <canvas id="radarChart"></canvas>
                </div>
            </div>

            <!-- Priority Top 3 Gaps -->
            <div class="bg-white p-5 rounded-3xl shadow-sm border border-slate-200 space-y-4 w-full box-border">
                <div>
                    <h3 class="font-bold text-base text-slate-800 flex items-center space-x-2">
                        <i class="fa-solid fa-triangle-exclamation text-rose-500"></i>
                        <span>最優先で対処すべき領域 TOP 3</span>
                    </h3>
                    <p class="text-xs text-slate-500 mt-1 font-medium">「重要度が高く満足度が低い」ギャップの大きい具体項目です。</p>
                </div>

                <div id="gap-items-container" class="space-y-3 w-full">
                    <!-- JS Dynamic Inject -->
                </div>
            </div>

            <!-- Action Plan Input -->
            <div class="bg-indigo-50 border border-indigo-100 p-5 rounded-3xl space-y-3 w-full box-border">
                <h3 class="font-bold text-base text-indigo-900 flex items-center space-x-2">
                    <i class="fa-solid fa-pen-to-square text-indigo-600"></i>
                    <span>今後のアクションメモ</span>
                </h3>
                <textarea id="action-plan-memo" rows="3" class="w-full text-sm p-3 rounded-2xl border border-indigo-200 focus:outline-none focus:ring-2 focus:ring-indigo-500 bg-white box-border" placeholder="例: 今週末に今後のキャリアについて整理する時間を作る..."></textarea>
                <button onclick="saveCurrentResult()" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-3.5 rounded-2xl text-sm shadow-md shadow-indigo-200 transition flex items-center justify-center space-x-2 active:scale-95">
                    <i class="fa-solid fa-floppy-disk"></i>
                    <span>診断結果とメモを保存する</span>
                </button>
            </div>

            <!-- Restart Button -->
            <div class="pt-2 w-full">
                <button onclick="restartDiagnosis()" class="w-full bg-white hover:bg-slate-50 text-slate-600 font-bold py-3.5 border border-slate-200 rounded-2xl text-sm transition">
                    再診断を行う
                </button>
            </div>
        </section>

    </main>

    <!-- History Modal -->
    <div id="modal-history" class="fixed inset-0 bg-slate-900/40 backdrop-blur-xs z-50 hidden flex items-end sm:items-center justify-center p-0 sm:p-4">
        <div class="bg-white w-full max-w-md rounded-t-3xl sm:rounded-3xl p-5 space-y-4 max-h-[80vh] flex flex-col shadow-2xl">
            <div class="flex justify-between items-center border-b border-slate-100 pb-3">
                <h3 class="font-bold text-base text-slate-800 flex items-center space-x-2">
                    <i class="fa-solid fa-clock-rotate-left text-indigo-600"></i>
                    <span>診断履歴一覧</span>
                </h3>
                <button onclick="toggleHistoryModal()" class="text-slate-400 hover:text-slate-600 p-1">
                    <i class="fa-solid fa-xmark text-xl"></i>
                </button>
            </div>
            
            <div id="history-list" class="flex-1 overflow-y-auto space-y-3 pr-1">
                <!-- JS Dynamic Inject -->
            </div>
        </div>
    </div>

    <script>
        // 20 Standard Items with Matching Visual Icons & Colors
        const DIAGNOSIS_ITEMS = [
            { id: 1, category: "家族・人間関係", title: "パートナーシップ", desc: "配偶者やパートナーとの信頼・深い絆", icon: "fa-heart", iconBg: "bg-rose-100", iconColor: "text-rose-600", borderColor: "border-rose-200" },
            { id: 2, category: "家族・人間関係", title: "家族・子供", desc: "子供の成長支援や親・親族との良好な関係", icon: "fa-people-roof", iconBg: "bg-orange-100", iconColor: "text-orange-600", borderColor: "border-orange-200" },
            { id: 3, category: "家族・人間関係", title: "友人・コミニュティ", desc: "心から信頼できる友人や仲間とのつながり", icon: "fa-user-group", iconBg: "bg-pink-100", iconColor: "text-pink-600", borderColor: "border-pink-200" },

            { id: 4, category: "仕事・キャリア", title: "やりがい・達成感", desc: "仕事を通じた成長や自己実現の感触", icon: "fa-trophy", iconBg: "bg-amber-100", iconColor: "text-amber-600", borderColor: "border-amber-200" },
            { id: 5, category: "仕事・キャリア", title: "成果・社会的評価", desc: "仕事上の実績や周囲からの評価・信頼", icon: "fa-award", iconBg: "bg-blue-100", iconColor: "text-blue-600", borderColor: "border-blue-200" },
            { id: 6, category: "仕事・キャリア", title: "働き方・自由度", desc: "時間や場所の柔軟性、ワークライフバランス", icon: "fa-sliders", iconBg: "bg-teal-100", iconColor: "text-teal-600", borderColor: "border-teal-200" },

            { id: 7, category: "お金・資産", title: "収入の安定性・向上", desc: "生活を維持・発展させる継続的な収入", icon: "fa-wallet", iconBg: "bg-emerald-100", iconColor: "text-emerald-600", borderColor: "border-emerald-200" },
            { id: 8, category: "お金・資産", title: "資産形成・投資", desc: "将来に向けた貯蓄・投資・不動産などの蓄え", icon: "fa-chart-line", iconBg: "bg-green-100", iconColor: "text-green-600", borderColor: "border-green-200" },
            { id: 9, category: "お金・資産", title: "経済的自由", desc: "お金の心配をせず選択できる自由の度合い", icon: "fa-piggy-bank", iconBg: "bg-cyan-100", iconColor: "text-cyan-600", borderColor: "border-cyan-200" },

            { id: 10, category: "健康・ライフスタイル", title: "身体的健康・体力", desc: "病気の予防、十分な体力・エネルギー", icon: "fa-heart-pulse", iconBg: "bg-red-100", iconColor: "text-red-600", borderColor: "border-red-200" },
            { id: 11, category: "健康・ライフスタイル", title: "メンタル・心の平穏", desc: "ストレスの少なさ、高い精神的安心感", icon: "fa-spa", iconBg: "bg-purple-100", iconColor: "text-purple-600", borderColor: "border-purple-200" },
            { id: 12, category: "健康・ライフスタイル", title: "住環境・生活水準", desc: "快適で心地よい住まいや日々の暮らし", icon: "fa-house-user", iconBg: "bg-indigo-100", iconColor: "text-indigo-600", borderColor: "border-indigo-200" },

            { id: 13, category: "社会貢献・影響力", title: "社会・他者への貢献", desc: "困っている人の支援や社会課題への取り組み", icon: "fa-hand-holding-heart", iconBg: "bg-violet-100", iconColor: "text-violet-600", borderColor: "border-violet-200" },
            { id: 14, category: "社会貢献・影響力", title: "知識・体験の発信", desc: "自分の経験や学びを他者に共有し役立てる", icon: "fa-bullhorn", iconBg: "bg-sky-100", iconColor: "text-sky-600", borderColor: "border-sky-200" },
            { id: 15, category: "社会貢献・影響力", title: "次世代への継承", desc: "未来の世代に良い価値観や環境を残す", icon: "fa-seedling", iconBg: "bg-lime-100", iconColor: "text-lime-700", borderColor: "border-lime-200" },

            { id: 16, category: "趣味・レジャー", title: "自己啓発・学び", desc: "新しいスキルの習得や興味のある分野の勉強", icon: "fa-graduation-cap", iconBg: "bg-blue-100", iconColor: "text-blue-700", borderColor: "border-blue-200" },
            { id: 17, category: "趣味・レジャー", title: "趣味・創作活動", desc: "夢中になれるスポーツ・芸術・クラフトなど", icon: "fa-palette", iconBg: "bg-fuchsia-100", iconColor: "text-fuchsia-600", borderColor: "border-fuchsia-200" },
            { id: 18, category: "趣味・レジャー", title: "旅行・非日常体験", desc: "新しい場所や文化に触れる探訪体験", icon: "fa-plane-departure", iconBg: "bg-sky-100", iconColor: "text-sky-600", borderColor: "border-sky-200" },
            { id: 19, category: "趣味・レジャー", title: "余暇・リフレッシュ", desc: "心身をリラックスさせ自由を満たす時間", icon: "fa-mug-hot", iconBg: "bg-amber-100", iconColor: "text-amber-700", borderColor: "border-amber-200" },
            { id: 20, category: "趣味・レジャー", title: "美意識・自分磨き", desc: "ファッション、美容、感性を磨く時間", icon: "fa-wand-magic-sparkles", iconBg: "bg-pink-100", iconColor: "text-pink-600", borderColor: "border-pink-200" }
        ];

        // Personality / Life Type Descriptions
        const TYPE_DESCRIPTIONS = {
            "家族・人間関係": {
                typeName: "ファミリー＆リレーションシップタイプ",
                icon: "fa-people-roof",
                color: "text-rose-600",
                badgeBg: "bg-rose-100",
                traits: "人生において身近な人との深い絆や人間関係の温かさを最も重視する傾向があります。",
                advice: "周囲を大切にする素晴らしい姿勢を持つ反面、自分の時間を後回しにしがちです。自分自身のケアも大切にしましょう。"
            },
            "仕事・キャリア": {
                typeName: "キャリア＆セルフアクチュアルタイプ",
                icon: "fa-briefcase",
                color: "text-blue-600",
                badgeBg: "bg-blue-100",
                traits: "自己実現や挑戦、社会的達成感を何よりのモチベーションとするパッション溢れるタイプです。",
                advice: "高い目標に向かって進む力が強みですが、健康面やプライベートとのバランスが崩れていないか定期的に点検しましょう。"
            },
            "お金・資産": {
                typeName: "フィナンシャル・インデペンデンスタイプ",
                icon: "fa-sack-dollar",
                color: "text-emerald-600",
                badgeBg: "bg-emerald-100",
                traits: "将来の安心や選択の自由を得るために、経済的基盤の確立に重点を置く堅実派タイプです。",
                advice: "資産形成は手段であり目的ではありません。「貯めた先で何を楽しみたいか」という具体的な目的意識を忘れずに備えましょう。"
            },
            "健康・ライフスタイル": {
                typeName: "ウェルビーイング＆ライフイノベーションタイプ",
                icon: "fa-heart-pulse",
                color: "text-amber-600",
                badgeBg: "bg-amber-100",
                traits: "心身の健康と快適な日常の維持こそがすべての幸福の土台であると捉える聡明なタイプです。",
                advice: "安定した土台の上で「今後新しく挑戦したいこと」にも一歩を踏み出すと、さらに生活に彩りが生まれます。"
            },
            "社会貢献・影響力": {
                typeName: "ソーシャルインパクト＆コントリビューションタイプ",
                icon: "fa-hand-holding-heart",
                color: "text-indigo-600",
                badgeBg: "bg-indigo-100",
                traits: "自分だけでなく他者や社会へ良い価値を提供したいという高い利他精神を持つタイプです。",
                advice: "社会貢献の継続には自身の持続可能性（経済・健康のゆとり）が不可欠です。まずは自分の身の回りの満たしを大切に。"
            },
            "趣味・レジャー": {
                typeName: "クリエイティブ＆レジャーライフタイプ",
                icon: "fa-palette",
                color: "text-purple-600",
                badgeBg: "bg-purple-100",
                traits: "日々の好奇心、非日常の体験、自分だけの表現や学びを通じて人生の豊かさを追求するタイプです。",
                advice: "インプットや楽しむ時間を大切にしつつ、その体験を日常や仕事のアイデアに還元していくと更なるシナジーを生むことができます。"
            }
        };

        let currentIndex = 0;
        let shuffledItems = [];
        let userScores = {};
        let radarChartInstance = null;
        let doughnutChartInstance = null;

        // Reset scores
        DIAGNOSIS_ITEMS.forEach(item => {
            userScores[item.id] = { importance: 5, satisfaction: 5 };
        });

        // App Init
        window.addEventListener('DOMContentLoaded', () => {
            loadSavedPlan();
        });

        function shuffleArray(array) {
            const arr = [...array];
            for (let i = arr.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [arr[i], arr[j]] = [arr[j], arr[i]];
            }
            return arr;
        }

        function startDiagnosis() {
            shuffledItems = shuffleArray(DIAGNOSIS_ITEMS);

            document.getElementById('app-header').classList.remove('hidden');
            document.getElementById('view-intro').classList.add('hidden');
            
            const wizardView = document.getElementById('view-wizard');
            wizardView.classList.remove('hidden');

            const resultView = document.getElementById('view-result');
            resultView.classList.add('hidden');

            currentIndex = 0;
            renderQuestion();
        }

        function renderQuestion() {
            const item = shuffledItems[currentIndex];
            const total = shuffledItems.length;

            document.getElementById('wizard-category-name').textContent = item.category;
            document.getElementById('wizard-progress-text').textContent = `${currentIndex + 1} / ${total} 項目`;
            document.getElementById('wizard-progress-bar').style.width = `${((currentIndex + 1) / total) * 100}%`;

            document.getElementById('item-category-badge').textContent = item.category;
            document.getElementById('item-title').textContent = item.title;
            document.getElementById('item-desc').textContent = item.desc;

            const iconContainer = document.getElementById('item-icon-container');
            const iconElem = document.getElementById('item-icon');
            
            iconContainer.className = `w-16 h-16 rounded-2xl flex items-center justify-center text-2xl shadow-xs my-1 border ${item.iconBg} ${item.borderColor}`;
            iconElem.className = `fa-solid ${item.icon} ${item.iconColor}`;

            const currentScore = userScores[item.id] || { importance: 5, satisfaction: 5 };
            
            document.getElementById('input-importance').value = currentScore.importance;
            document.getElementById('val-importance').textContent = currentScore.importance;

            document.getElementById('input-satisfaction').value = currentScore.satisfaction;
            document.getElementById('val-satisfaction').textContent = currentScore.satisfaction;

            document.getElementById('btn-prev').style.visibility = (currentIndex === 0) ? 'hidden' : 'visible';
            document.getElementById('btn-next-text').textContent = (currentIndex === total - 1) ? '結果を見る' : '次へ進む';
        }

        function updateSliderVal(type) {
            const val = document.getElementById(`input-${type}`).value;
            document.getElementById(`val-${type}`).textContent = val;
            const item = shuffledItems[currentIndex];
            userScores[item.id][type] = parseInt(val, 10);
        }

        function prevQuestion() {
            if (currentIndex > 0) {
                currentIndex--;
                renderQuestion();
            }
        }

        function nextQuestion() {
            const item = shuffledItems[currentIndex];
            userScores[item.id].importance = parseInt(document.getElementById('input-importance').value, 10);
            userScores[item.id].satisfaction = parseInt(document.getElementById('input-satisfaction').value, 10);

            if (currentIndex < shuffledItems.length - 1) {
                currentIndex++;
                renderQuestion();
            } else {
                showResults();
            }
        }

        function calculateResults() {
            const categories = ["家族・人間関係", "仕事・キャリア", "お金・資産", "健康・ライフスタイル", "社会貢献・影響力", "趣味・レジャー"];
            const categoryStats = {};

            categories.forEach(cat => {
                categoryStats[cat] = { totalImp: 0, totalSat: 0, count: 0 };
            });

            DIAGNOSIS_ITEMS.forEach(item => {
                const s = userScores[item.id];
                if (categoryStats[item.category]) {
                    categoryStats[item.category].totalImp += s.importance;
                    categoryStats[item.category].totalSat += s.satisfaction;
                    categoryStats[item.category].count += 1;
                }
            });

            let sumAvgImp = 0;
            const categoryAvgs = {};

            categories.forEach(cat => {
                const stat = categoryStats[cat];
                const avgImp = stat.count > 0 ? (stat.totalImp / stat.count) : 0;
                const avgSat = stat.count > 0 ? (stat.totalSat / stat.count) : 0;
                categoryAvgs[cat] = { avgImp, avgSat };
                sumAvgImp += avgImp;
            });

            const categoryList = categories.map(cat => {
                const { avgImp, avgSat } = categoryAvgs[cat];
                const sharePercent = sumAvgImp > 0 ? (avgImp / sumAvgImp) * 100 : 0;

                return {
                    name: cat,
                    avgImportance: parseFloat(avgImp.toFixed(1)),
                    avgSatisfaction: parseFloat(avgSat.toFixed(1)),
                    sharePercent: parseFloat(sharePercent.toFixed(1)),
                    gap: parseFloat((avgImp - avgSat).toFixed(1))
                };
            });

            categoryList.sort((a, b) => b.avgImportance - a.avgImportance);

            const itemGaps = DIAGNOSIS_ITEMS.map(item => {
                const s = userScores[item.id];
                return {
                    ...item,
                    importance: s.importance,
                    satisfaction: s.satisfaction,
                    gap: s.importance - s.satisfaction
                };
            }).sort((a, b) => b.gap - a.gap);

            return {
                sortedCategories: categoryList,
                topGapItems: itemGaps.slice(0, 3)
            };
        }

        function showResults() {
            document.getElementById('app-header').classList.add('hidden');
            document.getElementById('view-wizard').classList.add('hidden');

            const resultView = document.getElementById('view-result');
            resultView.classList.remove('hidden');

            window.scrollTo({ top: 0, behavior: 'smooth' });

            const results = calculateResults();
            const sortedCats = results.sortedCategories;

            // Top Type Comment
            const topCategory = sortedCats[0];
            const typeInfo = TYPE_DESCRIPTIONS[topCategory.name] || {};

            const typeCard = document.getElementById('top-type-card');
            typeCard.innerHTML = `
                <div class="flex items-center space-x-2">
                    <span class="text-xs font-bold ${typeInfo.color} ${typeInfo.badgeBg} px-2.5 py-1 rounded-full flex items-center space-x-1">
                        <i class="fa-solid ${typeInfo.icon}"></i>
                        <span>トップシェア (1位)</span>
                    </span>
                    <span class="text-xs font-extrabold text-slate-500">${topCategory.sharePercent}% の重き</span>
                </div>
                <h3 class="text-base font-bold text-slate-800">${typeInfo.typeName || topCategory.name}</h3>
                <p class="text-xs text-slate-600 leading-relaxed font-medium">${typeInfo.traits || ''}</p>
                <div class="bg-white/80 p-3 rounded-2xl border border-amber-200/60 text-xs text-amber-900 space-y-1">
                    <div class="font-bold flex items-center space-x-1 text-amber-700">
                        <i class="fa-solid fa-lightbulb"></i>
                        <span>今後のアドバイス</span>
                    </div>
                    <p class="text-[11px] leading-relaxed">${typeInfo.advice || ''}</p>
                </div>
            `;

            // Ranking Table
            const tbody = document.getElementById('ranking-table-body');
            tbody.innerHTML = '';

            sortedCats.forEach((cat, index) => {
                const tr = document.createElement('tr');
                tr.className = index === 0 ? "bg-indigo-50/50 font-bold" : "hover:bg-slate-50";
                tr.innerHTML = `
                    <td class="py-2.5 px-1 text-center text-slate-400 font-bold">${index + 1}</td>
                    <td class="py-2.5 px-2 text-slate-800 whitespace-nowrap">${cat.name}</td>
                    <td class="py-2.5 px-1 text-right text-indigo-600 font-extrabold whitespace-nowrap">${cat.avgImportance} <span class="text-[10px] font-normal text-slate-400">/10</span></td>
                    <td class="py-2.5 px-1 text-right text-slate-700 font-bold whitespace-nowrap">${cat.sharePercent}%</td>
                `;
                tbody.appendChild(tr);
            });

            // Top 3 Gaps
            const gapContainer = document.getElementById('gap-items-container');
            gapContainer.innerHTML = '';

            results.topGapItems.forEach(item => {
                const card = document.createElement('div');
                card.className = "p-3.5 rounded-2xl bg-slate-50 border border-slate-200 space-y-2 w-full box-border";
                card.innerHTML = `
                    <div class="flex justify-between items-start">
                        <div>
                            <span class="text-[10px] font-bold text-slate-500 bg-slate-200/60 px-2 py-0.5 rounded-md">${item.category}</span>
                            <h4 class="font-bold text-sm text-slate-800 mt-1">${item.title}</h4>
                        </div>
                        <span class="text-xs font-bold text-rose-500 bg-rose-50 border border-rose-200 px-2 py-0.5 rounded-lg whitespace-nowrap">
                            ギャップ +${item.gap}
                        </span>
                    </div>
                    <div class="flex items-center space-x-4 text-xs text-slate-600 pt-1 border-t border-slate-200/60 font-medium">
                        <span>重要度: <strong class="text-indigo-600">${item.importance}</strong></span>
                        <span>満足度: <strong class="text-emerald-600">${item.satisfaction}</strong></span>
                    </div>
                `;
                gapContainer.appendChild(card);
            });

            renderCharts(sortedCats);
        }

        function renderCharts(sortedCats) {
            const labels = sortedCats.map(c => c.name);
            const importanceAvg = sortedCats.map(c => c.avgImportance);
            const satisfactionAvg = sortedCats.map(c => c.avgSatisfaction);
            const shares = sortedCats.map(c => c.sharePercent);

            if (radarChartInstance) radarChartInstance.destroy();
            if (doughnutChartInstance) doughnutChartInstance.destroy();

            // Doughnut Chart
            const ctxDoughnut = document.getElementById('doughnutChart').getContext('2d');
            doughnutChartInstance = new Chart(ctxDoughnut, {
                type: 'doughnut',
                data: {
                    labels: labels,
                    datasets: [{
                        data: shares,
                        backgroundColor: [
                            '#4f46e5', '#3b82f6', '#10b981', '#f59e0b', '#ec4899', '#8b5cf6'
                        ],
                        borderWidth: 2,
                        borderColor: '#ffffff'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { display: false } },
                    cutout: '65%'
                }
            });

            // Radar Chart
            const ctxRadar = document.getElementById('radarChart').getContext('2d');
            radarChartInstance = new Chart(ctxRadar, {
                type: 'radar',
                data: {
                    labels: labels,
                    datasets: [
                        {
                            label: '重要度 (目標)',
                            data: importanceAvg,
                            backgroundColor: 'rgba(99, 102, 241, 0.2)',
                            borderColor: 'rgba(99, 102, 241, 1)',
                            borderWidth: 2,
                            pointBackgroundColor: 'rgba(99, 102, 241, 1)'
                        },
                        {
                            label: '満足度 (現状)',
                            data: satisfactionAvg,
                            backgroundColor: 'rgba(16, 185, 129, 0.2)',
                            borderColor: 'rgba(16, 185, 129, 1)',
                            borderWidth: 2,
                            pointBackgroundColor: 'rgba(16, 185, 129, 1)'
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: true,
                    aspectRatio: 1.1,
                    scales: {
                        r: {
                            min: 0,
                            max: 10,
                            ticks: { stepSize: 2, display: false },
                            pointLabels: { font: { size: 10, weight: 'bold' } }
                        }
                    },
                    plugins: {
                        legend: { position: 'bottom', labels: { boxWidth: 10, font: { size: 11 } } }
                    }
                }
            });
        }

        // Local Storage Operations
        function saveCurrentResult() {
            const memo = document.getElementById('action-plan-memo').value;
            const results = calculateResults();

            const record = {
                id: Date.now(),
                date: new Date().toLocaleDateString('ja-JP', { year: 'numeric', month: '2-digit', day: '2-digit', hour: '2-digit', minute: '2-digit' }),
                scores: userScores,
                topCategory: results.sortedCategories[0]?.name || "-",
                actionMemo: memo
            };

            let history = JSON.parse(localStorage.getItem('life_portfolio_history') || '[]');
            history.unshift(record);
            localStorage.setItem('life_portfolio_history', JSON.stringify(history));

            alert('診断結果とメモを保存しました！');
        }

        function loadSavedPlan() {
            let history = JSON.parse(localStorage.getItem('life_portfolio_history') || '[]');
            if (history.length > 0) {
                document.getElementById('action-plan-memo').value = history[0].actionMemo || "";
            }
        }

        function restartDiagnosis() {
            if (confirm('診断をやり直しますか？')) {
                DIAGNOSIS_ITEMS.forEach(item => {
                    userScores[item.id] = { importance: 5, satisfaction: 5 };
                });
                startDiagnosis();
            }
        }

        function toggleHistoryModal() {
            const modal = document.getElementById('modal-history');
            if (modal.classList.contains('hidden')) {
                renderHistoryList();
                modal.classList.remove('hidden');
            } else {
                modal.classList.add('hidden');
            }
        }

        function renderHistoryList() {
            const listContainer = document.getElementById('history-list');
            let history = JSON.parse(localStorage.getItem('life_portfolio_history') || '[]');

            if (history.length === 0) {
                listContainer.innerHTML = `<p class="text-xs text-slate-400 text-center py-8">保存された診断履歴はありません。</p>`;
                return;
            }

            listContainer.innerHTML = history.map((item) => `
                <div class="p-3 bg-slate-50 rounded-2xl border border-slate-100 space-y-1.5 text-xs">
                    <div class="flex justify-between items-center font-bold text-slate-700">
                        <span>📅 ${item.date}</span>
                        <span class="text-indigo-600 bg-indigo-50 px-2 py-0.5 rounded-md">1位: ${item.topCategory}</span>
                    </div>
                    ${item.actionMemo ? `<p class="text-slate-500 bg-white p-2 rounded-xl border border-slate-100 italic">"${item.actionMemo}"</p>` : ''}
                    <div class="flex justify-end pt-1">
                        <button onclick="deleteHistory(${item.id})" class="text-[10px] text-rose-500 hover:text-rose-700 font-semibold">
                            削除
                        </button>
                    </div>
                </div>
            `).join('');
        }

        function deleteHistory(id) {
            let history = JSON.parse(localStorage.getItem('life_portfolio_history') || '[]');
            history = history.filter(item => item.id !== id);
            localStorage.setItem('life_portfolio_history', JSON.stringify(history));
            renderHistoryList();
        }
    </script>
</body>
</html>
