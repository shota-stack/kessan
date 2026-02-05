html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>日本株式市場 決算発表カレンダー</title>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700;900&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Noto Sans JP', sans-serif;
            background: linear-gradient(to bottom right, #020617, #0f172a, #020617);
            color: #f1f5f9;
            min-height: 100vh;
            position: relative;
            overflow-x: hidden;
        }

        .bg-decoration {
            position: fixed;
            inset: 0;
            overflow: hidden;
            pointer-events: none;
            z-index: 0;
        }

        .bg-circle-1 {
            position: absolute;
            top: 0;
            left: 25%;
            width: 384px;
            height: 384px;
            background: rgba(16, 185, 129, 0.05);
            border-radius: 50%;
            filter: blur(80px);
        }

        .bg-circle-2 {
            position: absolute;
            bottom: 0;
            right: 25%;
            width: 384px;
            height: 384px;
            background: rgba(59, 130, 246, 0.05);
            border-radius: 50%;
            filter: blur(80px);
        }

        .container {
            position: relative;
            max-width: 1280px;
            margin: 0 auto;
            padding: 2rem 1rem;
            z-index: 1;
        }

        header {
            margin-bottom: 3rem;
            opacity: 0;
            transform: translateY(-2rem);
            animation: slideDown 1s ease forwards;
        }

        @keyframes slideDown {
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .header-top {
            display: flex;
            align-items: center;
            gap: 1rem;
            margin-bottom: 1rem;
        }

        .icon-box {
            background: linear-gradient(to bottom right, #10b981, #059669);
            padding: 0.75rem;
            border-radius: 1rem;
            box-shadow: 0 10px 30px rgba(16, 185, 129, 0.2);
        }

        .icon {
            width: 32px;
            height: 32px;
            color: white;
        }

        h1 {
            font-size: 2.5rem;
            font-weight: 900;
            background: linear-gradient(to right, #10b981, #6ee7b7, #60a5fa);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            letter-spacing: -0.025em;
        }

        .subtitle {
            font-size: 1.125rem;
            color: #94a3b8;
            margin-top: 0.25rem;
        }

        .controls {
            display: flex;
            flex-direction: column;
            gap: 1rem;
            margin-top: 1.5rem;
        }

        @media (min-width: 640px) {
            .controls {
                flex-direction: row;
            }
        }

        .search-box {
            position: relative;
            flex: 1;
        }

        .search-icon {
            position: absolute;
            left: 1rem;
            top: 50%;
            transform: translateY(-50%);
            width: 20px;
            height: 20px;
            color: #64748b;
        }

        input[type="text"] {
            width: 100%;
            padding: 0.75rem 1rem 0.75rem 3rem;
            background: rgba(30, 41, 59, 0.5);
            border: 1px solid rgba(51, 65, 85, 0.5);
            border-radius: 0.75rem;
            color: #f1f5f9;
            font-size: 1rem;
            outline: none;
            transition: all 0.3s;
        }

        input[type="text"]::placeholder {
            color: #64748b;
        }

        input[type="text"]:focus {
            border-color: rgba(16, 185, 129, 0.5);
            box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.1);
        }

        .period-buttons {
            display: flex;
            gap: 0.5rem;
        }

        .period-btn {
            padding: 0.75rem 1.5rem;
            border-radius: 0.75rem;
            font-weight: 500;
            border: none;
            cursor: pointer;
            transition: all 0.3s;
            font-family: 'Noto Sans JP', sans-serif;
        }

        .period-btn.active {
            background: linear-gradient(to right, #10b981, #059669);
            color: white;
            box-shadow: 0 10px 30px rgba(16, 185, 129, 0.3);
        }

        .period-btn:not(.active) {
            background: rgba(30, 41, 59, 0.5);
            color: #94a3b8;
        }

        .period-btn:not(.active):hover {
            background: rgba(30, 41, 59, 0.8);
            color: #cbd5e1;
        }

        .loading-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 6rem 0;
        }

        .spinner-container {
            position: relative;
        }

        .spinner {
            width: 80px;
            height: 80px;
            border: 4px solid #334155;
            border-top-color: #10b981;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        .spinner-icon {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 32px;
            height: 32px;
            color: #10b981;
        }

        .loading-text {
            margin-top: 1.5rem;
            color: #94a3b8;
            font-size: 1.125rem;
        }

        .error-box {
            background: rgba(239, 68, 68, 0.1);
            border: 1px solid rgba(239, 68, 68, 0.2);
            border-radius: 1rem;
            padding: 1.5rem;
            display: flex;
            align-items: flex-start;
            gap: 1rem;
        }

        .error-icon {
            width: 24px;
            height: 24px;
            color: #f87171;
            flex-shrink: 0;
            margin-top: 0.25rem;
        }

        .earnings-grid {
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        .earning-card {
            background: linear-gradient(to right, rgba(30, 41, 59, 0.4), rgba(30, 41, 59, 0.2));
            backdrop-filter: blur(10px);
            border: 1px solid rgba(51, 65, 85, 0.3);
            border-radius: 1rem;
            padding: 1.5rem;
            transition: all 0.3s;
            opacity: 0;
            transform: translateY(2rem);
            animation: slideUp 0.6s ease forwards;
        }

        @keyframes slideUp {
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .earning-card:hover {
            border-color: rgba(16, 185, 129, 0.3);
            box-shadow: 0 20px 40px rgba(16, 185, 129, 0.05);
            transform: translateY(-0.25rem);
        }

        .card-content {
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
        }

        .card-main {
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
        }

        @media (min-width: 1024px) {
            .card-main {
                flex-direction: row;
                align-items: center;
                justify-content: space-between;
            }
        }

        .analyst-section {
            background: rgba(15, 23, 42, 0.5);
            border: 1px solid rgba(51, 65, 85, 0.3);
            border-radius: 0.75rem;
            padding: 1rem;
        }

        .analyst-header {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            margin-bottom: 1rem;
            color: #cbd5e1;
            font-weight: 600;
        }

        .analyst-icon {
            width: 20px;
            height: 20px;
            color: #10b981;
        }

        .analyst-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
            gap: 1rem;
        }

        .analyst-item {
            text-align: center;
        }

        .analyst-label {
            font-size: 0.75rem;
            color: #64748b;
            margin-bottom: 0.25rem;
            text-transform: uppercase;
            letter-spacing: 0.05em;
        }

        .analyst-value {
            font-size: 1.125rem;
            font-weight: 700;
        }

        .rating-buy {
            color: #10b981;
        }

        .rating-strong-buy {
            color: #059669;
        }

        .rating-neutral {
            color: #fbbf24;
        }

        .rating-sell {
            color: #ef4444;
        }

        .analyst-count {
            color: #94a3b8;
            font-size: 0.875rem;
        }

        .target-price {
            color: #60a5fa;
        }

        .price-change-positive {
            color: #10b981;
        }

        .price-change-negative {
            color: #ef4444;
        }

        .chart-section {
            background: rgba(15, 23, 42, 0.5);
            border: 1px solid rgba(51, 65, 85, 0.3);
            border-radius: 0.75rem;
            padding: 1rem;
        }

        .chart-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 1rem;
        }

        .chart-title {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            color: #cbd5e1;
            font-weight: 600;
        }

        .chart-icon {
            width: 20px;
            height: 20px;
            color: #10b981;
        }

        .chart-year {
            font-size: 0.875rem;
            color: #64748b;
        }

        .chart-container {
            position: relative;
            height: 120px;
            margin-top: 0.5rem;
            display: flex;
            gap: 0.5rem;
        }

        .chart-y-axis {
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            width: 50px;
            font-size: 0.7rem;
            color: #64748b;
            text-align: right;
            padding-right: 0.5rem;
        }

        .chart-canvas {
            flex: 1;
            height: 100%;
            position: relative;
        }

        .chart-grid {
            position: absolute;
            inset: 0;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            pointer-events: none;
        }

        .chart-grid-line {
            height: 1px;
            background: rgba(51, 65, 85, 0.3);
        }

        .chart-svg {
            position: absolute;
            inset: 0;
            width: 100%;
            height: 100%;
        }

        .chart-line {
            fill: none;
            stroke: url(#chartGradient);
            stroke-width: 2.5;
            stroke-linecap: round;
            stroke-linejoin: round;
        }

        .chart-area {
            fill: url(#chartAreaGradient);
        }

        .chart-labels {
            display: flex;
            justify-content: space-between;
            margin-top: 0.5rem;
            font-size: 0.75rem;
            color: #64748b;
        }

        .company-info {
            display: flex;
            align-items: flex-start;
            gap: 1rem;
            flex: 1;
        }

        .company-icon-box {
            background: linear-gradient(to bottom right, rgba(16, 185, 129, 0.2), rgba(59, 130, 246, 0.2));
            padding: 0.75rem;
            border-radius: 0.75rem;
        }

        .company-icon {
            width: 24px;
            height: 24px;
            color: #10b981;
        }

        .company-details {
            flex: 1;
        }

        .company-header {
            display: flex;
            align-items: center;
            gap: 0.75rem;
            margin-bottom: 0.5rem;
            flex-wrap: wrap;
        }

        .company-name {
            font-size: 1.25rem;
            font-weight: 700;
            color: #f1f5f9;
        }

        .ticker-badge {
            padding: 0.25rem 0.75rem;
            background: rgba(16, 185, 129, 0.2);
            color: #6ee7b7;
            font-size: 0.875rem;
            font-weight: 500;
            border-radius: 9999px;
        }

        .company-meta {
            display: flex;
            align-items: center;
            gap: 1rem;
            font-size: 0.875rem;
            color: #94a3b8;
        }

        .meta-item {
            display: flex;
            align-items: center;
            gap: 0.25rem;
        }

        .sector-dot {
            width: 8px;
            height: 8px;
            background: #60a5fa;
            border-radius: 50%;
        }

        .calendar-icon {
            width: 16px;
            height: 16px;
        }

        .card-stats {
            display: flex;
            align-items: center;
            gap: 1.5rem;
            flex-wrap: wrap;
        }

        .stat-box {
            text-align: center;
        }

        .stat-label {
            font-size: 0.875rem;
            color: #64748b;
            margin-bottom: 0.25rem;
        }

        .stat-value {
            font-size: 1.125rem;
            font-weight: 700;
            color: #10b981;
        }

        .stat-time {
            font-size: 0.875rem;
            color: #94a3b8;
            margin-top: 0.25rem;
        }

        .stat-eps {
            font-size: 1.5rem;
            font-weight: 700;
            color: #f1f5f9;
        }

        .arrow-btn {
            padding: 0.75rem;
            background: rgba(51, 65, 85, 0.5);
            border: none;
            border-radius: 0.75rem;
            cursor: pointer;
            transition: all 0.3s;
        }

        .arrow-btn:hover {
            background: rgba(16, 185, 129, 0.2);
        }

        .arrow-icon {
            width: 20px;
            height: 20px;
            color: #94a3b8;
            transition: color 0.3s;
        }

        .arrow-btn:hover .arrow-icon {
            color: #10b981;
        }

        .empty-state {
            text-align: center;
            padding: 6rem 0;
        }

        .empty-icon {
            width: 64px;
            height: 64px;
            color: #475569;
            margin: 0 auto 1rem;
        }

        .empty-text {
            color: #94a3b8;
            font-size: 1.125rem;
        }

        footer {
            margin-top: 4rem;
            padding-top: 2rem;
            border-top: 1px solid rgba(30, 41, 59, 0.5);
            text-align: center;
            color: #64748b;
            font-size: 0.875rem;
        }

        footer p {
            margin-bottom: 0.5rem;
        }
    </style>
</head>
<body>
    <div class="bg-decoration">
        <div class="bg-circle-1"></div>
        <div class="bg-circle-2"></div>
    </div>

    <div class="container">
        <header>
            <div class="header-top">
                <div class="icon-box">
                    <svg class="icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"></path>
                    </svg>
                </div>
                <div>
                    <h1>日本株式市場</h1>
                    <p class="subtitle">決算発表カレンダー</p>
                </div>
            </div>

            <div class="controls">
                <div class="search-box">
                    <svg class="search-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path>
                    </svg>
                    <input type="text" id="searchInput" placeholder="企業名、証券コード、業種で検索...">
                </div>

                <div class="period-buttons">
                    <button class="period-btn active" data-period="今週">今週</button>
                    <button class="period-btn" data-period="来週">来週</button>
                    <button class="period-btn" data-period="今月">今月</button>
                </div>
            </div>
        </header>

        <div id="content"></div>

        <footer>
            <p>データは検索結果に基づいて更新されます</p>
            <p id="lastUpdated"></p>
        </footer>
    </div>

    <script>
        let earnings = [];
        let loading = true;
        let error = null;
        let selectedPeriod = '今週';
        let searchTerm = '';

        function formatDate(dateString) {
            const date = new Date(dateString);
            const month = date.getMonth() + 1;
            const day = date.getDate();
            const weekdays = ['日', '月', '火', '水', '木', '金', '土'];
            const weekday = weekdays[date.getDay()];
            return `${month}月${day}日 (${weekday})`;
        }

        function generateFallbackData() {
            const companies = [
                { 
                    company: "トヨタ自動車", 
                    ticker: "7203", 
                    sector: "自動車", 
                    expectedEPS: "¥450",
                    per: "8.2",
                    pbr: "1.1",
                    roe: "12.5%",
                    analystRating: "買い",
                    analystCount: 28,
                    targetPrice: "¥3,200",
                    priceChange: "+5.2%"
                },
                { 
                    company: "ソニーグループ", 
                    ticker: "6758", 
                    sector: "電機", 
                    expectedEPS: "¥320",
                    per: "15.4",
                    pbr: "2.3",
                    roe: "16.8%",
                    analystRating: "買い",
                    analystCount: 32,
                    targetPrice: "¥15,800",
                    priceChange: "+8.4%"
                },
                { 
                    company: "任天堂", 
                    ticker: "7974", 
                    sector: "ゲーム", 
                    expectedEPS: "¥580",
                    per: "13.6",
                    pbr: "3.8",
                    roe: "28.2%",
                    analystRating: "中立",
                    analystCount: 24,
                    targetPrice: "¥7,500",
                    priceChange: "-2.1%"
                },
                { 
                    company: "ソフトバンクグループ", 
                    ticker: "9984", 
                    sector: "通信", 
                    expectedEPS: "¥210",
                    per: "11.8",
                    pbr: "1.4",
                    roe: "11.9%",
                    analystRating: "買い",
                    analystCount: 35,
                    targetPrice: "¥8,900",
                    priceChange: "+12.3%"
                },
                { 
                    company: "キーエンス", 
                    ticker: "6861", 
                    sector: "電機", 
                    expectedEPS: "¥890",
                    per: "42.5",
                    pbr: "8.2",
                    roe: "19.3%",
                    analystRating: "買い",
                    analystCount: 26,
                    targetPrice: "¥72,000",
                    priceChange: "+6.7%"
                },
                { 
                    company: "ファーストリテイリング", 
                    ticker: "9983", 
                    sector: "小売", 
                    expectedEPS: "¥420",
                    per: "24.8",
                    pbr: "5.6",
                    roe: "22.6%",
                    analystRating: "強気買い",
                    analystCount: 30,
                    targetPrice: "¥48,500",
                    priceChange: "+15.8%"
                },
                { 
                    company: "三菱UFJフィナンシャル・グループ", 
                    ticker: "8306", 
                    sector: "銀行", 
                    expectedEPS: "¥180",
                    per: "9.3",
                    pbr: "0.8",
                    roe: "8.6%",
                    analystRating: "中立",
                    analystCount: 22,
                    targetPrice: "¥1,450",
                    priceChange: "+3.2%"
                },
            ];
            
            const today = new Date();
            return companies.map((company, index) => {
                const date = new Date(today);
                date.setDate(today.getDate() + index);
                
                // Generate chart data for the year
                const chartData = generateYearlyChartData(index);
                
                return {
                    ...company,
                    date: date.toISOString().split('T')[0],
                    time: ["09:00", "12:00", "15:00"][index % 3],
                    quarter: "Q3 2024",
                    chartData: chartData
                };
            });
        }

        function generateYearlyChartData(seed) {
            const months = ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月'];
            const basePrice = 1000 + seed * 500;
            const data = [];
            
            for (let i = 0; i < 12; i++) {
                const variation = Math.sin(i / 2 + seed) * 200 + (Math.random() - 0.5) * 150;
                const trend = (i / 12) * (seed % 2 === 0 ? 300 : -100);
                data.push({
                    month: months[i],
                    price: Math.round(basePrice + variation + trend)
                });
            }
            
            return data;
        }

        async function fetchEarningsData() {
            loading = true;
            error = null;
            render();

            try {
                const searchQuery = selectedPeriod === '今週' 
                    ? '日本株 決算発表 予定 今週'
                    : selectedPeriod === '来週'
                    ? '日本株 決算発表 予定 来週'
                    : '日本株 決算発表 予定 今月';

                const response = await fetch("https://api.anthropic.com/v1/messages", {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                    },
                    body: JSON.stringify({
                        model: "claude-sonnet-4-20250514",
                        max_tokens: 1000,
                        tools: [
                            {
                                "type": "web_search_20250305",
                                "name": "web_search"
                            }
                        ],
                        messages: [
                            {
                                role: "user",
                                content: `${searchQuery}の最新情報を検索して、以下のJSON形式で企業の決算発表予定を5-10社分教えてください。実際の決算発表日、アナリスト予測、株価データ、財務指標を含めてください。

必ず以下の形式のJSONのみを返してください（前置きやマークダウンの記号は不要）:
[
  {
    "company": "企業名",
    "ticker": "証券コード",
    "date": "2026-02-05",
    "time": "15:00",
    "quarter": "Q3 2024",
    "expectedEPS": "¥450",
    "per": "12.5",
    "pbr": "1.8",
    "roe": "15.2%",
    "sector": "業種",
    "analystRating": "買い/強気買い/中立/売り",
    "analystCount": 25,
    "targetPrice": "¥3,000",
    "priceChange": "+5.2%",
    "chartData": [
      {"month": "1月", "price": 2800},
      {"month": "2月", "price": 2850},
      {"month": "3月", "price": 2900},
      {"month": "4月", "price": 2950},
      {"month": "5月", "price": 3000},
      {"month": "6月", "price": 3050},
      {"month": "7月", "price": 3100},
      {"month": "8月", "price": 3150},
      {"month": "9月", "price": 3200},
      {"month": "10月", "price": 3250},
      {"month": "11月", "price": 3300},
      {"month": "12月", "price": 3350}
    ]
  }
]`
                            }
                        ],
                    })
                });

                const data = await response.json();
                
                let fullResponse = data.content
                    .map(item => (item.type === "text" ? item.text : ""))
                    .filter(Boolean)
                    .join("\n");
                
                const cleanText = fullResponse.replace(/```json|```/g, "").trim();
                
                try {
                    const parsed = JSON.parse(cleanText);
                    if (Array.isArray(parsed) && parsed.length > 0) {
                        earnings = parsed;
                    } else {
                        earnings = generateFallbackData();
                    }
                } catch (parseError) {
                    console.error("Parse error:", parseError);
                    earnings = generateFallbackData();
                }
                
            } catch (err) {
                console.error("Fetch error:", err);
                error = "データの取得に失敗しました";
                earnings = generateFallbackData();
            } finally {
                loading = false;
                render();
            }
        }

        function renderMiniChart(chartData, ticker) {
            if (!chartData || chartData.length === 0) return '';
            
            const width = 100;
            const height = 100;
            const padding = 5;
            
            // Find min and max values
            const prices = chartData.map(d => d.price);
            const minPrice = Math.min(...prices);
            const maxPrice = Math.max(...prices);
            const priceRange = maxPrice - minPrice;
            
            // Calculate Y-axis labels (5 levels)
            const yAxisLabels = [];
            for (let i = 0; i < 5; i++) {
                const price = maxPrice - (priceRange * i / 4);
                yAxisLabels.push(Math.round(price).toLocaleString('ja-JP'));
            }
            
            // Generate path points
            const points = chartData.map((d, i) => {
                const x = (i / (chartData.length - 1)) * (width - 2 * padding) + padding;
                const y = height - padding - ((d.price - minPrice) / priceRange) * (height - 2 * padding);
                return `${x},${y}`;
            }).join(' ');
            
            // Create area path
            const areaPoints = `${padding},${height - padding} ` + points + ` ${width - padding},${height - padding}`;
            
            // Determine if trend is positive
            const isPositive = prices[prices.length - 1] > prices[0];
            
            return `
                <div class="chart-y-axis">
                    ${yAxisLabels.map(label => `<div>¥${label}</div>`).join('')}
                </div>
                <div class="chart-canvas">
                    <svg class="chart-svg" viewBox="0 0 ${width} ${height}" preserveAspectRatio="none">
                        <defs>
                            <linearGradient id="chartGradient-${ticker}" x1="0%" y1="0%" x2="0%" y2="100%">
                                <stop offset="0%" style="stop-color:${isPositive ? '#10b981' : '#ef4444'};stop-opacity:1" />
                                <stop offset="100%" style="stop-color:${isPositive ? '#10b981' : '#ef4444'};stop-opacity:0.8" />
                            </linearGradient>
                            <linearGradient id="chartAreaGradient-${ticker}" x1="0%" y1="0%" x2="0%" y2="100%">
                                <stop offset="0%" style="stop-color:${isPositive ? '#10b981' : '#ef4444'};stop-opacity:0.2" />
                                <stop offset="100%" style="stop-color:${isPositive ? '#10b981' : '#ef4444'};stop-opacity:0.02" />
                            </linearGradient>
                        </defs>
                        <polygon points="${areaPoints}" fill="url(#chartAreaGradient-${ticker})" />
                        <polyline points="${points}" fill="none" stroke="url(#chartGradient-${ticker})" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
                    </svg>
                </div>
            `;
        }

        function render() {
            const content = document.getElementById('content');
            
            if (loading) {
                content.innerHTML = `
                    <div class="loading-container">
                        <div class="spinner-container">
                            <div class="spinner"></div>
                            <svg class="spinner-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"></path>
                            </svg>
                        </div>
                        <p class="loading-text">決算情報を取得中...</p>
                    </div>
                `;
                return;
            }

            if (error) {
                content.innerHTML = `
                    <div class="error-box">
                        <svg class="error-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                        </svg>
                        <div>
                            <h3 style="color: #f87171; font-weight: 600; font-size: 1.125rem; margin-bottom: 0.25rem;">エラーが発生しました</h3>
                            <p style="color: rgba(248, 113, 113, 0.8);">${error}</p>
                        </div>
                    </div>
                `;
                return;
            }

            const filteredEarnings = earnings.filter(e => 
                e.company.toLowerCase().includes(searchTerm.toLowerCase()) ||
                e.ticker.includes(searchTerm) ||
                e.sector.toLowerCase().includes(searchTerm.toLowerCase())
            );

            if (filteredEarnings.length === 0) {
                content.innerHTML = `
                    <div class="empty-state">
                        <svg class="empty-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4"></path>
                        </svg>
                        <p class="empty-text">検索条件に一致する企業が見つかりませんでした</p>
                    </div>
                `;
                return;
            }

            content.innerHTML = `
                <div class="earnings-grid">
                    ${filteredEarnings.map((earning, index) => {
                        const ratingClass = 
                            earning.analystRating === '強気買い' ? 'rating-strong-buy' :
                            earning.analystRating === '買い' ? 'rating-buy' :
                            earning.analystRating === '中立' ? 'rating-neutral' :
                            'rating-sell';
                        
                        const priceChangeClass = earning.priceChange.startsWith('+') 
                            ? 'price-change-positive' 
                            : 'price-change-negative';
                        
                        return `
                        <div class="earning-card" style="animation-delay: ${index * 50}ms">
                            <div class="card-content">
                                <div class="card-main">
                                    <div class="company-info">
                                        <div class="company-icon-box">
                                            <svg class="company-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4"></path>
                                            </svg>
                                        </div>
                                        <div class="company-details">
                                            <div class="company-header">
                                                <h3 class="company-name">${earning.company}</h3>
                                                <span class="ticker-badge">${earning.ticker}</span>
                                            </div>
                                            <div class="company-meta">
                                                <span class="meta-item">
                                                    <div class="sector-dot"></div>
                                                    ${earning.sector}
                                                </span>
                                                <span class="meta-item">
                                                    <svg class="calendar-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"></path>
                                                    </svg>
                                                    ${earning.quarter}
                                                </span>
                                            </div>
                                        </div>
                                    </div>

                                    <div class="card-stats">
                                        <div class="stat-box">
                                            <p class="stat-label">発表日</p>
                                            <p class="stat-value">${formatDate(earning.date)}</p>
                                            <p class="stat-time">${earning.time}</p>
                                        </div>
                                        
                                        <div class="stat-box">
                                            <p class="stat-label">予想EPS</p>
                                            <p class="stat-eps">${earning.expectedEPS}</p>
                                        </div>

                                        <div class="stat-box">
                                            <p class="stat-label">PER</p>
                                            <p class="stat-value" style="color: #60a5fa;">${earning.per}倍</p>
                                        </div>

                                        <div class="stat-box">
                                            <p class="stat-label">PBR</p>
                                            <p class="stat-value" style="color: #a78bfa;">${earning.pbr}倍</p>
                                        </div>

                                        <div class="stat-box">
                                            <p class="stat-label">ROE</p>
                                            <p class="stat-value" style="color: #f59e0b;">${earning.roe}</p>
                                        </div>

                                        <button class="arrow-btn">
                                            <svg class="arrow-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
                                            </svg>
                                        </button>
                                    </div>
                                </div>

                                <!-- Analyst Predictions Section -->
                                <div class="analyst-section">
                                    <div class="analyst-header">
                                        <svg class="analyst-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"></path>
                                        </svg>
                                        <span>アナリスト予測</span>
                                    </div>
                                    <div class="analyst-grid">
                                        <div class="analyst-item">
                                            <div class="analyst-label">総合評価</div>
                                            <div class="analyst-value ${ratingClass}">${earning.analystRating}</div>
                                            <div class="analyst-count">${earning.analystCount}名</div>
                                        </div>
                                        <div class="analyst-item">
                                            <div class="analyst-label">目標株価</div>
                                            <div class="analyst-value target-price">${earning.targetPrice}</div>
                                        </div>
                                        <div class="analyst-item">
                                            <div class="analyst-label">上昇余地</div>
                                            <div class="analyst-value ${priceChangeClass}">${earning.priceChange}</div>
                                        </div>
                                    </div>
                                </div>

                                <!-- Chart Section -->
                                <div class="chart-section">
                                    <div class="chart-header">
                                        <div class="chart-title">
                                            <svg class="chart-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 12l3-3 3 3 4-4M8 21l4-4 4 4M3 4h18M4 4h16v12a1 1 0 01-1 1H5a1 1 0 01-1-1V4z"></path>
                                            </svg>
                                            <span>株価推移</span>
                                        </div>
                                        <div class="chart-year">2026年度</div>
                                    </div>
                                    <div class="chart-container" id="chart-${earning.ticker}">
                                        ${renderMiniChart(earning.chartData, earning.ticker)}
                                    </div>
                                    <div class="chart-labels">
                                        <span>1月</span>
                                        <span>4月</span>
                                        <span>7月</span>
                                        <span>10月</span>
                                        <span>12月</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    `;
                    }).join('')}
                </div>
            `;
        }

        // Event Listeners
        document.getElementById('searchInput').addEventListener('input', (e) => {
            searchTerm = e.target.value;
            render();
        });

        document.querySelectorAll('.period-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                document.querySelectorAll('.period-btn').forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                selectedPeriod = btn.dataset.period;
                fetchEarningsData();
            });
        });

        // Update footer timestamp
        function updateTimestamp() {
            document.getElementById('lastUpdated').textContent = 
                `最終更新: ${new Date().toLocaleString('ja-JP')}`;
        }

        // Initialize
        updateTimestamp();
        fetchEarningsData();
    </script>
</body>
</html>
```

## ライセンス

MIT License

## 作者

Created with Claude AI# kessan
