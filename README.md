<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <title>T12职业优势测评系统 - 招聘专业版</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js">
    </script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js">
    </script>
    <style>
        /* ---------- 全局重置 ---------- */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Microsoft YaHei', 'PingFang SC', system-ui, -apple-system, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 12px;
        }
        .container {
            max-width: 1000px;
            margin: 0 auto;
            background: #fff;
            border-radius: 16px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.25);
            overflow: hidden;
        }
        .header {
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            color: #fff;
            padding: 24px 16px;
            text-align: center;
        }
        .header h1 {
            font-size: 22px;
            margin-bottom: 4px;
        }
        .header p {
            font-size: 13px;
            opacity: 0.9;
        }
        .content {
            padding: 20px 16px;
        }

        /* ---------- 页面切换 ---------- */
        .section {
            display: none;
        }
        .section.active {
            display: block;
        }
        #result.active {
            display: grid !important;
        }

        /* ---------- 欢迎页 ---------- */
        .welcome-page {
            text-align: center;
        }
        .welcome-page h2 {
            color: #1e3c72;
            font-size: 20px;
            margin-bottom: 14px;
        }
        .welcome-page .intro {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 16px;
            margin-bottom: 18px;
            text-align: left;
            line-height: 1.7;
            color: #333;
            font-size: 14px;
        }
        .welcome-page .intro h3 {
            color: #2a5298;
            margin-bottom: 4px;
            font-size: 15px;
        }
        .info-form {
            text-align: left;
            margin-bottom: 18px;
        }
        .info-form label {
            display: block;
            margin-bottom: 4px;
            color: #333;
            font-weight: 500;
            font-size: 14px;
        }
        .info-form input,
        .info-form select {
            width: 100%;
            padding: 12px 14px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 15px;
            margin-bottom: 12px;
            transition: border-color 0.3s;
            background: #fff;
            -webkit-appearance: none;
            appearance: none;
        }
        .info-form input:focus,
        .info-form select:focus {
            outline: none;
            border-color: #667eea;
        }
        .info-form .required-star {
            color: #dc3545;
            margin-left: 2px;
        }
        .info-form .field-hint {
            font-size: 12px;
            color: #888;
            margin-top: -8px;
            margin-bottom: 10px;
        }
        .dimensions-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin: 18px 0;
        }
        .dimension-card {
            background: linear-gradient(135deg, #f5f7fa 0%, #e8ecf1 100%);
            border-radius: 10px;
            padding: 14px 8px;
            text-align: center;
        }
        .dimension-card .icon {
            font-size: 26px;
            margin-bottom: 2px;
        }
        .dimension-card h4 {
            color: #1e3c72;
            font-size: 14px;
            margin-bottom: 2px;
        }
        .dimension-card p {
            font-size: 12px;
            color: #666;
        }

        /* ---------- 按钮 ---------- */
        .btn {
            display: inline-block;
            padding: 12px 28px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #fff;
            border: none;
            border-radius: 30px;
            font-size: 15px;
            cursor: pointer;
            transition: all 0.3s;
            text-align: center;
            min-height: 44px;
            line-height: 1.2;
        }
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(102, 126, 234, 0.4);
        }
        .btn-secondary {
            background: #e0e0e0;
            color: #333;
        }
        .btn-secondary:hover {
            background: #d0d0d0;
            box-shadow: none;
        }
        .btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none !important;
        }

        /* ---------- 答题页 ---------- */
        .quiz-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 14px;
            flex-wrap: wrap;
            gap: 6px;
        }
        .module-badge {
            padding: 4px 12px;
            background: #e8f4fd;
            color: #2a5298;
            border-radius: 20px;
            font-size: 13px;
            font-weight: 500;
        }
        .progress-info {
            font-size: 13px;
            color: #666;
        }
        .progress-bar {
            height: 6px;
            background: #e8e8e8;
            border-radius: 4px;
            margin-bottom: 18px;
            overflow: hidden;
        }
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #667eea, #764ba2);
            border-radius: 4px;
            transition: width 0.3s ease;
        }
        .question-box {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 18px;
            margin-bottom: 18px;
            min-height: 70px;
            display: flex;
            align-items: center;
        }
        .question-text {
            font-size: 16px;
            color: #333;
            line-height: 1.6;
            font-weight: 500;
        }
        .options-container {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-bottom: 22px;
        }
        .option-item {
            display: flex;
            align-items: flex-start;
            padding: 12px 14px;
            border: 2px solid #e8e8e8;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s;
        }
        .option-item:hover {
            border-color: #667eea;
            background: #f8f9ff;
        }
        .option-item.selected {
            border-color: #667eea;
            background: linear-gradient(135deg, #f0f3ff 0%, #f8f9ff 100%);
        }
        .option-label {
            width: 28px;
            height: 28px;
            border-radius: 50%;
            background: #f0f0f0;
            color: #888;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 12px;
            font-weight: 600;
            flex-shrink: 0;
            font-size: 14px;
        }
        .option-item.selected .option-label {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: #fff;
        }
        .option-content {
            flex: 1;
            font-size: 14px;
            color: #444;
            line-height: 1.5;
            padding-top: 2px;
        }
        .quiz-footer {
            display: flex;
            justify-content: space-between;
            gap: 10px;
        }
        .quiz-footer .btn {
            flex: 1;
            padding: 10px 16px;
            font-size: 14px;
            min-height: 44px;
        }

        /* ==========================================================
           结果页 - 两栏网格紧凑布局（响应式）
           ========================================================== */
        #result {
            display: none;
            gap: 6px;
            padding: 0;
            grid-template-columns: 1fr 1fr;
        }
        #result.active {
            display: grid !important;
        }

        #result .result-header {
            grid-column: 1 / -1;
            text-align: center;
            background: linear-gradient(135deg, #f0f3ff 0%, #e8ecff 100%);
            border-radius: 8px;
            padding: 10px 12px;
            margin-bottom: 2px;
        }
        #result .result-header h2 {
            font-size: 18px;
            margin-bottom: 2px;
        }
        #result .result-header .result-user-info {
            font-size: 12px;
            color: #666;
            line-height: 1.4;
        }

        #result .result-section {
            background: #fafbfc;
            border-radius: 6px;
            padding: 6px 10px;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.03);
            page-break-inside: avoid;
            display: flex;
            flex-direction: column;
        }

        /* T12排行（第4个）和个人发展建议（第13个）占两栏 */
        #result .result-section:nth-of-type(4) {
            grid-column: 1 / -1;
        }
        #result .result-section:nth-of-type(13) {
            grid-column: 1 / -1;
        }

        .result-section h3 {
            font-size: 13px;
            margin-bottom: 4px;
            padding-bottom: 3px;
            border-bottom: 1px solid #e9ecef;
            display: flex;
            align-items: center;
            color: #1e3c72;
        }
        .result-section h3::before {
            content: '';
            width: 3px;
            height: 14px;
            background: linear-gradient(180deg, #667eea, #764ba2);
            border-radius: 2px;
            margin-right: 6px;
        }

        .score-row {
            display: flex;
            align-items: center;
            margin-bottom: 3px;
            font-size: 11px;
            flex-wrap: wrap;
        }
        .score-name {
            width: 70px;
            font-size: 11px;
            color: #333;
            flex-shrink: 0;
        }
        .score-bar {
            flex: 1;
            height: 14px;
            background: #f0f0f0;
            border-radius: 7px;
            margin: 0 6px;
            overflow: hidden;
            min-width: 40px;
        }
        .score-fill {
            height: 100%;
            border-radius: 7px;
            background: linear-gradient(90deg, #667eea, #764ba2);
            transition: width 0.8s ease;
        }
        .score-value {
            width: 28px;
            text-align: right;
            font-size: 12px;
            font-weight: 600;
            color: #2a5298;
            flex-shrink: 0;
        }
        .score-tag {
            font-size: 9px;
            padding: 1px 5px;
            border-radius: 6px;
            margin-left: 4px;
            flex-shrink: 0;
        }

        .tag-excellent {
            background: #d4edda;
            color: #155724;
        }
        .tag-good {
            background: #fff3cd;
            color: #856404;
        }
        .tag-need {
            background: #f8d7da;
            color: #721c24;
        }

        /* T12卡片 - 5列（桌面） */
        .t12-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 4px;
        }
        .t12-card {
            background: #f8f9fa;
            border-radius: 4px;
            padding: 4px 2px;
            text-align: center;
        }
        .t12-card .t12-name {
            font-size: 10px;
            color: #333;
            font-weight: 500;
            margin-bottom: 1px;
        }
        .t12-card .t12-score {
            font-size: 16px;
            font-weight: 700;
            background: linear-gradient(135deg, #667eea, #764ba2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        .t12-card .t12-desc {
            font-size: 8px;
            color: #888;
            line-height: 1.1;
        }

        .analysis-box {
            background: #fff;
            border-radius: 4px;
            padding: 6px 8px;
            font-size: 11px;
            color: #444;
            line-height: 1.5;
            border-left: 2px solid #667eea;
            flex: 1;
        }
        .analysis-box p {
            margin-bottom: 2px;
        }
        .analysis-box strong {
            color: #2a5298;
        }

        .suggestions-list {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        .suggestions-list li {
            padding: 3px 0 3px 18px;
            position: relative;
            font-size: 11px;
            line-height: 1.4;
            border-bottom: none;
        }
        .suggestions-list li::before {
            content: '✓';
            position: absolute;
            left: 0;
            top: 3px;
            font-size: 11px;
            color: #28a745;
            font-weight: bold;
        }
        .suggestions-list li:last-child {
            border-bottom: none;
        }

        .recruit-highlight {
            background: #e8f4fd;
            border-radius: 4px;
            padding: 5px 8px;
            margin: 3px 0;
            border-left: 2px solid #2a5298;
            font-size: 11px;
            line-height: 1.4;
        }
        .recruit-highlight strong {
            color: #1e3c72;
        }

        .result-actions {
            grid-column: 1 / -1;
            display: flex;
            gap: 8px;
            margin-top: 12px;
            flex-wrap: wrap;
        }
        .result-actions .btn {
            flex: 1;
            min-width: 80px;
            font-size: 13px;
            padding: 10px 12px;
            min-height: 44px;
        }
        .footer-note {
            grid-column: 1 / -1;
            text-align: center;
            font-size: 10px;
            color: #999;
            margin-top: 8px;
            padding-top: 6px;
            border-top: 1px solid #eee;
        }

        /* ---------- 打印样式 ---------- */
        @media print {
            body {
                background: #fff !important;
                padding: 0 !important;
            }
            .container {
                box-shadow: none !important;
                border-radius: 0 !important;
            }
            .header {
                -webkit-print-color-adjust: exact !important;
                print-color-adjust: exact !important;
            }
            .dimension-card,
            .option-item,
            .result-header,
            .analysis-box,
            .t12-card {
                -webkit-print-color-adjust: exact !important;
                print-color-adjust: exact !important;
            }
            .score-fill {
                -webkit-print-color-adjust: exact !important;
                print-color-adjust: exact !important;
            }
            .btn,
            .result-actions,
            .quiz-footer {
                display: none !important;
            }
            .section {
                display: block !important;
            }
            #welcome {
                display: none !important;
            }
            #quiz {
                display: none !important;
            }
            #result {
                display: grid !important;
                gap: 4px;
            }
            #result .result-section {
                page-break-inside: avoid;
            }
            #result .result-section:nth-of-type(4) {
                page-break-before: avoid;
            }
            #result .result-section:nth-of-type(13) {
                page-break-before: avoid;
            }
        }

        /* ---------- 响应式：平板/手机 ---------- */
        @media (max-width: 768px) {
            body {
                padding: 8px;
            }
            .content {
                padding: 16px 12px;
            }
            .header h1 {
                font-size: 20px;
            }
            .dimensions-grid {
                grid-template-columns: 1fr 1fr;
                gap: 8px;
            }
            .dimension-card {
                padding: 12px 6px;
            }
            .dimension-card .icon {
                font-size: 22px;
            }
            .dimension-card h4 {
                font-size: 13px;
            }
            .dimension-card p {
                font-size: 11px;
            }

            /* 结果页：单栏 */
            #result {
                grid-template-columns: 1fr;
                gap: 8px;
            }
            #result .result-section {
                grid-column: 1 !important;
            }
            .t12-grid {
                grid-template-columns: repeat(4, 1fr);
            }
            .result-section h3 {
                font-size: 14px;
            }
            .score-name {
                width: 60px;
                font-size: 11px;
            }
            .score-bar {
                height: 16px;
            }
            .score-value {
                width: 28px;
                font-size: 12px;
            }
            .btn {
                font-size: 14px;
                padding: 10px 20px;
            }
            .question-text {
                font-size: 15px;
            }
            .option-content {
                font-size: 14px;
            }
            .quiz-footer .btn {
                font-size: 13px;
                padding: 10px 12px;
            }
            .result-actions .btn {
                font-size: 13px;
                padding: 10px 10px;
                min-width: 60px;
            }
        }

        @media (max-width: 480px) {
            .header h1 {
                font-size: 18px;
            }
            .header p {
                font-size: 12px;
            }
            .content {
                padding: 12px 10px;
            }
            .dimensions-grid {
                grid-template-columns: 1fr 1fr;
                gap: 6px;
            }
            .dimension-card {
                padding: 10px 4px;
            }
            .dimension-card .icon {
                font-size: 20px;
            }
            .dimension-card h4 {
                font-size: 12px;
            }
            .dimension-card p {
                font-size: 10px;
            }
            .info-form input,
            .info-form select {
                font-size: 14px;
                padding: 10px 12px;
            }
            .btn {
                font-size: 13px;
                padding: 10px 16px;
                min-height: 40px;
            }
            .question-text {
                font-size: 14px;
            }
            .option-content {
                font-size: 13px;
            }
            .option-item {
                padding: 10px 12px;
            }
            .option-label {
                width: 24px;
                height: 24px;
                font-size: 12px;
                margin-right: 8px;
            }
            .quiz-footer .btn {
                font-size: 12px;
                padding: 8px 10px;
                min-height: 40px;
            }
            .result-actions .btn {
                font-size: 12px;
                padding: 8px 8px;
                min-width: 50px;
                min-height: 40px;
            }
            /* 结果页单栏已经是默认 */
            .t12-grid {
                grid-template-columns: repeat(3, 1fr);
            }
            .score-name {
                width: 50px;
                font-size: 10px;
            }
            .score-bar {
                height: 12px;
                margin: 0 4px;
            }
            .score-value {
                width: 24px;
                font-size: 11px;
            }
            .score-tag {
                font-size: 8px;
                padding: 1px 4px;
            }
            .result-section h3 {
                font-size: 13px;
            }
            .analysis-box {
                font-size: 11px;
                padding: 4px 6px;
            }
            .suggestions-list li {
                font-size: 10px;
                padding: 2px 0 2px 16px;
            }
            .suggestions-list li::before {
                font-size: 10px;
                top: 2px;
            }
            .recruit-highlight {
                font-size: 10px;
                padding: 4px 6px;
            }
            .footer-note {
                font-size: 9px;
            }
        }

        @media (max-width: 380px) {
            .t12-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            .dimensions-grid {
                grid-template-columns: 1fr 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🏆 T12职业优势测评系统</h1>
            <p>招聘甄选专业版 | 素质·性格·EQ·IQ 四维综合评估</p>
            <p style="margin-top:4px;font-size:12px;opacity:0.8;">设计人：DAVY（抖音号：d190564680）</p>
        </div>

        <div class="content">
            <!-- 欢迎页 -->
            <div id="welcome" class="section">
                <div class="welcome-page">
                    <h2>招聘人才测评</h2>
                    <div class="intro">
                        <h3>📋 测评说明</h3>
                        <p>本测评体系由人力资源管理师<strong>DAVY</strong>设计，以招聘场景为核心导向，基于心理学、组织行为学及人才管理理论构建多维评估模型，从<strong>职业素质（含专业技能、岗位适配性、职业素养等细分维度）、职业性格（融合MBTI、Big Five等人格理论框架）、情商EQ（涵盖情绪感知、自我调节、人际互动与社会适应等能力层级）、智商IQ（聚焦逻辑推理、创造性思维与复杂问题解决能力）</strong>四大核心维度，对候选人的职业潜力进行系统性、动态化评估。通过定量测评工具与定性行为分析相结合的方法，构建标准化与个性化并重的评估矩阵，旨在为企业提供精准的用人决策参考，优化人才匹配效能；同时赋能个体深度认知自身优势特征及发展象限，支持职业路径规划与潜能开发。测评结果以可视化多维雷达图及诊断性报告呈现，兼具科学性与实践指导价值，适用于战略性招聘筛选、人才梯队建设及人岗动态适配等多场景应用。</p>
                        <p style="margin-top:8px;">⏱️ 本次测评时长：25-35分钟 | 📊 专业分析报告 | 📄 下载测评结果</p>
                    </div>
                    <div class="dimensions-grid">
                        <div class="dimension-card"><div class="icon">💼</div><h4>职业素质</h4><p>30题 · 管理能力</p></div>
                        <div class="dimension-card"><div class="icon">🎭</div><h4>职业性格</h4><p>30题 · T12特质</p></div>
                        <div class="dimension-card"><div class="icon">❤️</div><h4>情商EQ</h4><p>20题 · 情绪管理</p></div>
                        <div class="dimension-card"><div class="icon">🧠</div><h4>智商IQ</h4><p>20题 · 逻辑思维</p></div>
                    </div>
                    <div class="info-form">
                        <label>候选人姓名 <span class="required-star">*</span></label>
                        <input type="text" id="userName" placeholder="请填写候选人姓名">
                        <label>应聘岗位 <span class="required-star">*</span></label>
                        <input type="text" id="userDept" placeholder="如：生产经理、运营经理等">
                        <label>所属行业 <span class="required-star">*</span></label>
                        <select id="industry">
                            <option value="">-- 请选择行业 --</option>
                            <option value="制造业">制造业</option>
                            <option value="电商零售">电商零售</option>
                            <option value="互联网">互联网</option>
                            <option value="金融">金融</option>
                            <option value="医疗健康">医疗健康</option>
                            <option value="教育">教育</option>
                            <option value="其他">其他</option>
                        </select>
                        <div class="field-hint">⚠️ 以上两项为必填项，一项为选择项，您选择行业的时候要慎重，不同行业测评倾向不同，可能会影响最终结果</div>
                    </div>
                    <button class="btn" onclick="startQuiz()">开始测评 →</button>
                </div>
            </div>

            <!-- 答题页 -->
            <div id="quiz" class="section">
                <div class="quiz-header">
                    <span id="moduleBadge" class="module-badge">职业素质</span>
                    <span id="progressInfo" class="progress-info">第 1 题 / 共 100 题</span>
                </div>
                <div class="progress-bar"><div id="progressFill" class="progress-fill" style="width:1%"></div></div>
                <div class="question-box"><div id="questionText" class="question-text"></div></div>
                <div id="optionsContainer" class="options-container"></div>
                <div class="quiz-footer">
                    <button id="prevBtn" class="btn btn-secondary" onclick="prevQuestion()" disabled>← 上一题</button>
                    <button id="nextBtn" class="btn" onclick="nextQuestion()" disabled>下一题 →</button>
                </div>
            </div>

            <!-- 结果页 -->
            <div id="result" class="section">
                <div class="result-header">
                    <h2>🎯 候选人职业优势评估报告</h2>
                    <div class="result-user-info">
                        <span id="resultName">匿名</span> |
                        <span id="resultDept">-</span> |
                        <span id="resultIndustry">-</span><br>
                        测评日期：<span id="resultDate"></span>
                    </div>
                </div>

                <div class="result-section"><h3>📌 候选人核心优势摘要</h3><div id="recruitSummary" class="analysis-box"></div></div>
                <div class="result-section"><h3>📊 四维综合评估</h3><div id="overallScores"></div><div id="overallInterpretation" class="analysis-box" style="margin-top:4px;"></div></div>
                <div class="result-section"><h3>🎯 领导力风格画像</h3><div id="leadershipStyle" class="analysis-box"></div></div>
                <div class="result-section"><h3>🏅 T12职业优势排行</h3><p style="font-size:10px;color:#888;margin-bottom:2px;">分数范围：30~70分</p><div id="t12Grid" class="t12-grid"></div></div>
                <div class="result-section"><h3>📌 优势与关键发展领域</h3><div id="strengthWeakness" class="analysis-box"></div></div>
                <div class="result-section"><h3>🔬 四大职业领域分析</h3><div id="f4Analysis" class="analysis-box"></div></div>
                <div class="result-section"><h3>💼 职业素质细分</h3><div id="qualityScores"></div></div>
                <div class="result-section"><h3>🎭 性格画像分析</h3><div id="personalityAnalysis" class="analysis-box"></div></div>
                <div class="result-section"><h3>❤️ 情商(EQ)深度解析</h3><div id="eqScores"></div><div id="eqInterpretation" class="analysis-box" style="margin-top:4px;"></div></div>
                <div class="result-section"><h3>🧠 智商(IQ)认知能力</h3><div id="iqAnalysis" class="analysis-box"></div></div>
                <div class="result-section"><h3>📈 行业适配度与岗位匹配</h3><div id="industryFit" class="analysis-box"></div></div>
                <div class="result-section"><h3>💡 招聘用人建议</h3><ul id="recruitSuggestions" class="suggestions-list"></ul></div>
                <div class="result-section"><h3>📚 个人发展建议</h3><ul id="personalSuggestions" class="suggestions-list"></ul></div>

                <div class="result-actions">
                    <button class="btn" onclick="downloadPDF(this)">📄 下载测评结果</button>
                    <button class="btn btn-secondary" onclick="copyResult()">📋 复制测评结果</button>
                    <button class="btn btn-secondary" onclick="restartQuiz()">🔄 重新测评</button>
                </div>
                <div class="footer-note">本报告由T12职业优势测评系统自动生成 | 设计人：DAVY（抖音号：d190564680）</div>
            </div>
        </div>
    </div>

    <script>
        (function() {
            'use strict';

            // ============================================================
            // 题库（100题）
            // ============================================================
            var questions = [
                // ---- 职业素质 (30题) ----
                { module: '职业素质', dimension: '战略思维', question: '面对复杂多变的市场环境，您通常如何制定部门或团队的发展规划？', options: ['主要依据上级指示和历史经验制定', '会分析1-2个主要方向后快速决策', '系统分析内外部环境，制定3年以上规划并分解落地', '不仅制定规划，还会建立动态调整机制和多套预案'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '战略思维', question: '当公司战略方向发生重大调整时，您的第一反应是：', options: ['等待上级明确具体安排后再行动', '先做好手头工作，看情况再说', '快速理解战略意图，主动调整团队工作重点', '深入分析战略调整背后的逻辑，主动寻找新的机会点'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '战略思维', question: '在评估一个新项目或业务机会时，您最看重的是：', options: ['短期能否快速见效、产生业绩', '是否符合当前团队的能力范围', '长期战略价值与短期收益的平衡', '能否构建核心竞争力和护城河'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '战略思维', question: '对于行业趋势和竞争对手动态，您通常：', options: ['很少主动关注，专注做好本职工作', '偶尔看看行业新闻，了解大概情况', '定期收集分析行业信息，形成自己的判断', '建立系统化的情报收集机制，定期输出竞争分析报告'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '战略思维', question: '当现有业务模式遇到瓶颈时，您倾向于：', options: ['继续优化现有模式，争取做到最好', '做一些小的调整和改进', '探索1-2个新的业务方向', '推动系统性的业务转型和创新'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '决策执行', question: '在信息不充分的情况下需要做出重要决策时，您通常：', options: ['等待更多信息，推迟决策', '凭经验和直觉快速决策', '收集关键信息后权衡利弊做出决策', '构建决策框架，识别关键假设，做出有预案的决策'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '决策执行', question: '对于重要决策的落地执行，您的工作方式是：', options: ['布置下去后等待结果汇报', '定期询问进度，遇到问题再介入', '制定详细计划，跟进关键节点', '建立执行监控体系，及时发现偏差并调整'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '决策执行', question: '当执行过程中遇到重大阻碍时，您会：', options: ['向上级汇报，等待指示', '尝试几种常规方法解决', '深入分析原因，调整策略继续推进', '快速识别本质问题，调动资源突破性解决'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '决策执行', question: '对于工作中的优先级判断，您的做法是：', options: ['按时间顺序，先来先做', '按上级要求的紧急程度安排', '按重要性和紧急性四象限分类处理', '聚焦高价值事项，主动淘汰低价值工作'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '决策执行', question: '面对多个相互冲突的目标和需求时，您：', options: ['感到困惑，不知道先做哪个', '尽量兼顾，哪个都不想放弃', '明确优先级，有取有舍', '找到多方共赢的创新解决方案'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '团队管理', question: '在团队目标设定方面，您通常：', options: ['根据上级要求分解给团队成员', '与团队成员商量后确定目标', '设定有挑战性的目标，激发团队潜能', '建立目标管理体系，将个人目标与组织目标对齐'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '团队管理', question: '对于团队成员的绩效反馈，您的做法是：', options: ['年底统一评价，平时很少反馈', '出问题时才会指出问题', '定期一对一沟通，给予反馈和指导', '建立持续反馈机制，及时认可优秀表现，建设性指出不足'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '团队管理', question: '当团队成员之间出现矛盾冲突时，您会：', options: ['尽量回避，希望他们自己解决', '简单调解，各打五十大板', '了解情况后公正判断，协调解决', '深入分析冲突根源，引导双方达成共识并预防未来冲突'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '团队管理', question: '在人才培养和梯队建设方面，您：', options: ['主要关注业绩，人才培养是HR的事', '会指导几个表现好的骨干员工', '有计划地培养团队核心成员', '建立系统化的人才培养体系，打造人才梯队'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '团队管理', question: '对于团队中表现不佳的员工，您通常：', options: ['忍着不说，实在不行就换人', '私下提醒几次，不行就放弃', '明确指出问题，设定改进期限', '诊断问题根源，提供针对性支持，建立改进计划'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '沟通协调', question: '在跨部门协作中，您通常：', options: ['有事才联系，平时很少沟通', '遇到问题时协调解决', '主动沟通，建立良好的协作关系', '建立跨部门协作机制，推动协同增效'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '沟通协调', question: '向上级汇报工作时，您的风格是：', options: ['上级问什么答什么', '汇报工作进展和结果', '不仅汇报结果，还分析问题和建议', '结构化汇报，提供多个方案并给出建议'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '沟通协调', question: '在表达不同意见或提出反对观点时，您：', options: ['很少提出不同意见，避免冲突', '有不同意见会直接说出来', '会选择合适的时机和方式表达', '既能坚持原则，又能让对方欣然接受'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '沟通协调', question: '对于重要的沟通，您通常：', options: ['想到什么说什么', '会前大概想一下要说什么', '会前做充分准备，理清思路', '不仅准备内容，还会预判对方反应，设计沟通策略'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '沟通协调', question: '当您的意见被上级或重要客户否定时，您会：', options: ['心里不服，但表面接受', '坚持自己的观点，继续争取', '先接受，再找机会慢慢沟通', '反思对方的考量，调整方案后再次沟通'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '问题解决', question: '遇到复杂问题时，您的思维方式是：', options: ['凭经验找解决方案', '分析问题原因后找对策', '用结构化方法拆解分析问题', '系统性思考，识别根本原因和系统性解决方案'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '问题解决', question: '对于工作中的风险，您的态度是：', options: ['尽量避免风险，走稳妥路线', '风险不可避免，走一步看一步', '识别主要风险，做好应对准备', '建立风险管理体系，主动管理和利用风险'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '问题解决', question: '当问题原因不明确时，您会：', options: ['凭直觉判断原因', '尝试几个可能的解决方案', '收集数据，分析找出原因', '设计验证方案，科学定位根因'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '问题解决', question: '对于重复性出现的问题，您倾向于：', options: ['每次出现就解决一次', '总结经验，下次更快解决', '制定标准流程，减少重复发生', '从系统层面优化，彻底根除问题'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '问题解决', question: '面对突发危机事件时，您通常：', options: ['感到紧张，不知如何应对', '按上级指示处理', '快速反应，控制局面', '冷静应对，有序处理，同时总结经验完善预案'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '成就导向', question: '对于工作目标，您的态度是：', options: ['完成基本要求就行', '努力完成既定目标', '主动设定更高的目标挑战自己', '追求卓越，持续突破自我极限'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '成就导向', question: '面对工作中的困难和挫折，您：', options: ['容易气馁，想放弃', '坚持一下，实在不行就调整目标', '迎难而上，想办法克服困难', '把困难视为成长机会，越挫越勇'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '成就导向', question: '对于工作成果的标准，您通常：', options: ['过得去就行', '达到合格标准', '追求高质量完成', '精益求精，追求极致'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '成就导向', question: '在工作中学习和自我提升方面，您：', options: ['工作够用就行，很少主动学习', '遇到问题时才去学习', '有计划地学习提升自己', '持续学习，将学习融入日常工作'], scores: [1, 2, 3, 4] },
                { module: '职业素质', dimension: '成就导向', question: '当取得不错的成绩时，您会：', options: ['感到满足，可以放松一下了', '保持现状，维持这个水平', '总结经验，争取做得更好', '快速归零，设定更高的目标'], scores: [1, 2, 3, 4] },

                // ---- 职业性格 (30题) ----
                { module: '职业性格', dimension: '开拓型', question: '在团队或组织中，您更倾向于：', options: ['跟随大家的节奏，配合完成工作', '在明确的框架内做好自己的事', '主动推动事情进展，影响他人', '引领方向，开创新局面'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '开拓型', question: '面对新的市场机会或业务领域，您的第一反应是：', options: ['风险太大，还是稳妥点好', '可以了解一下，但不急于行动', '很感兴趣，愿意尝试探索', '兴奋不已，立刻行动抢占先机'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '开拓型', question: '在压力和挑战面前，您通常：', options: ['感到焦虑，希望压力小一些', '可以承受，但不主动寻求', '喜欢有挑战的工作，能激发斗志', '享受高压状态，越有挑战越兴奋'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '管控型', question: '在工作中，您更关注：', options: ['工作氛围是否融洽', '大家是否开心满意', '任务是否按时完成', '结果是否达到标准，过程是否可控'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '管控型', question: '对于规则和制度，您的态度是：', options: ['规则是死的，人是活的，可以灵活处理', '大的原则要遵守，细节可以灵活', '严格按规则办事，确保公平公正', '不仅遵守规则，还会主动完善制度体系'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '管控型', question: '作为管理者，您的风格更偏向：', options: ['民主型，大家商量着来', '支持型，帮助员工成长', '指导型，告诉员工怎么做', '指挥型，明确目标和要求，严格执行'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '交际型', question: '在社交场合中，您通常：', options: ['不太主动，等待别人来搭话', '熟悉的人会聊得比较多', '主动与人交流，容易结识新朋友', '是人群中的焦点，善于活跃气氛'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '交际型', question: '在与人沟通时，您更擅长：', options: ['倾听和理解对方', '一对一深入交流', '在小组中表达观点', '在大型场合演讲或主持'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '交际型', question: '对于人脉和社交资源，您的看法是：', options: ['朋友不在多，在于精', '有几个知心朋友就够了', '多认识人挺好的，机会更多', '主动经营人脉，广泛建立社交网络'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '服务型', question: '在工作中，您更看重：', options: ['自己的工作是否轻松', '个人的成就和发展', '团队的整体表现', '客户和他人是否满意'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '服务型', question: '当他人需要帮助时，您通常：', options: ['忙自己的事，没太注意', '别人开口了才会帮忙', '看到了会主动帮忙', '总是设身处地为他人着想，主动提供支持'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '服务型', question: '对于客户投诉或不满，您的第一反应是：', options: ['觉得客户太挑剔', '想办法尽快打发走', '耐心倾听，解决问题', '感谢客户反馈，视为改进机会'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '亲和型', question: '在团队中，您更像：', options: ['独来独往的独行侠', '做好自己事的普通成员', '乐于助人的好伙伴', '维系团队感情的粘合剂'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '亲和型', question: '对于团队氛围，您认为：', options: ['工作嘛，把事做好就行', '氛围还挺重要的，但不能影响工作', '好的氛围能提升工作效率', '氛围是最重要的，人开心了什么都好办'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '亲和型', question: '当团队发生冲突时，您通常：', options: ['事不关己，高高挂起', '看看情况再说', '会出面调解一下', '主动协调，帮助大家化解矛盾'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '事务型', question: '在工作中，您更擅长：', options: ['开创性的工作', '与人打交道的工作', '按流程执行的事务性工作', '精细化的流程管理和优化'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '事务型', question: '对于工作的条理性和规范性，您：', options: ['比较随意，怎么方便怎么来', '大的方面有条理，细节无所谓', '喜欢井井有条，按部就班', '追求完美的秩序和规范'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '事务型', question: '面对大量琐碎的事务性工作，您：', options: ['感到烦躁，不想做', '能做但不喜欢', '耐心细致地完成', '享受把繁杂事务理清楚的过程'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '操作型', question: '在动手实操方面，您：', options: ['不太擅长，更喜欢动脑', '一般水平，能用就行', '手比较巧，做得又快又好', '非常享受动手操作的过程'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '操作型', question: '学习新技能或新工具时，您更喜欢：', options: ['先看理论，理解原理再动手', '边学边练，慢慢摸索', '直接上手，在实践中学习', '动手拆解研究，彻底搞明白'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '操作型', question: '对于具体的执行落地，您更关注：', options: ['方向对不对', '整体方案好不好', '具体步骤清不清楚', '每一个操作细节是否到位'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '技能型', question: '在专业领域，您的追求是：', options: ['够用就行', '达到平均水平以上', '成为领域内的专家', '追求技术/专业的极致境界'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '技能型', question: '对于专业深度和广度，您更倾向于：', options: ['什么都懂一点，但不深', '有一两个比较擅长的领域', '在某个领域有很深的造诣', '既是专家又有广博的知识面'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '技能型', question: '当遇到专业难题时，您：', options: ['找别人帮忙解决', '查资料试试看', '深入研究，直到搞懂', '非常享受攻克技术难题的过程'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '规划型', question: '做事情时，您通常：', options: ['走一步看一步', '有个大概想法就开始做', '先做详细计划再行动', '不仅有计划，还有备选方案和风险预案'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '规划型', question: '对于未来，您的思考方式是：', options: ['想太多没用，过好当下', '偶尔会想想未来', '经常思考未来的发展方向', '有清晰的长远规划，并在逐步实现'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '规划型', question: '面对复杂的项目，您更擅长：', options: ['具体执行某个环节', '带领小团队完成任务', '统筹协调多个方面', '顶层设计和整体规划'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '发明型', question: '对于新想法和创意，您：', options: ['不太感兴趣，还是老办法靠谱', '听听可以，但不会轻易尝试', '经常有新想法，愿意尝试', '点子特别多，喜欢创新和发明'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '发明型', question: '遇到问题时，您更倾向于：', options: ['用成熟的方法解决', '在现有方法基础上改进', '寻找新的解决方案', '创造全新的方法和思路'], scores: [1, 2, 3, 4] },
                { module: '职业性格', dimension: '发明型', question: '您更喜欢的工作类型是：', options: ['按部就班的常规工作', '有一定变化的工作', '需要创意的工作', '充满未知和探索的工作'], scores: [1, 2, 3, 4] },

                // ---- 情商EQ (20题) ----
                { module: '情商EQ', dimension: '自我认知', question: '您对自己的情绪变化：', options: ['不太敏感，情绪来了自己也没察觉', '事后回想能意识到当时的情绪', '情绪发生时能立刻意识到', '不仅能察觉，还能理解情绪产生的原因'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '自我认知', question: '对于自己的优点和不足，您：', options: ['不太清楚，别人怎么说就怎么信', '大概知道一些，但不全面', '比较了解自己的长短处', '有清晰的自我认知，知道自己的边界和潜力'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '自我认知', question: '当受到批评或负面评价时，您的第一反应是：', options: ['感到受伤或愤怒，急于辩解', '心里不舒服，但表面接受', '冷静思考对方说得有没有道理', '感谢反馈，深入反思并寻求改进'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '自我认知', question: '您对自己的情绪触发点（容易引发强烈情绪的事情）：', options: ['完全不清楚，莫名就发火了', '知道一些，但说不清楚', '比较清楚自己的情绪触发点', '非常清楚，并能主动管理'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '情绪管理', question: '当感到愤怒或烦躁时，您通常：', options: ['忍不住发脾气或摔东西', '忍着，但脸色很难看', '能控制住不发作，但心里难受', '能有效调节，快速平复情绪'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '情绪管理', question: '面对压力和焦虑时，您：', options: ['被情绪淹没，无法正常工作', '受影响较大，效率下降', '能基本维持正常工作状态', '能将压力转化为动力，保持高效'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '情绪管理', question: '遇到挫折或失败时，您恢复的速度：', options: ['需要很长时间才能走出来', '需要一段时间调整', '比较快就能恢复', '很快就能调整好，甚至越挫越勇'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '情绪管理', question: '对于自己的情绪表达，您：', options: ['要么压抑要么爆发，没有中间状态', '不太会表达，别人常常猜不透', '能适当地表达自己的情绪', '能恰如其分地表达，既真实又不伤人'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '自我激励', question: '在没有外部监督的情况下，您：', options: ['很容易偷懒或拖延', '需要 deadlines 才能推动', '能自我驱动完成工作', '有很强的内在动力，持续追求卓越'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '自我激励', question: '面对长期目标，您：', options: ['热情来得快去得也快', '能坚持一段时间，但容易放弃', '能坚持到完成目标', '不仅能坚持，还能保持热情和专注'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '共情能力', question: '与他人交流时，您：', options: ['更关注自己要说什么', '听对方说话，但容易走神', '认真倾听，理解对方的意思', '不仅听内容，还能感受对方的情绪'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '共情能力', question: '当别人向您倾诉烦恼时，您通常：', options: ['觉得对方太矫情，这点小事也说', '给出建议，告诉对方应该怎么做', '表示理解和安慰', '先共情接纳，再根据需要提供帮助'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '共情能力', question: '您能准确感知他人的情绪变化吗？', options: ['很迟钝，别人生气了都不知道', '比较熟悉的人能感觉到', '大部分时候能感知到', '非常敏感，能捕捉细微的情绪变化'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '共情能力', question: '对于与自己观点不同的人，您：', options: ['难以理解，觉得对方不对', '可以接受不同观点，但不认同', '能理解对方为什么这么想', '能站在对方角度，感同身受'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '人际管理', question: '在人际关系中，您通常：', options: ['比较被动，等待别人主动', '与人相处融洽，但不主动经营', '主动维护关系，人缘不错', '是人际网络的核心，善于经营人脉'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '人际管理', question: '处理人际冲突时，您更擅长：', options: ['回避冲突，能躲就躲', '妥协让步，维持表面和平', '据理力争，赢了再说', '寻找双赢方案，化解矛盾'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '人际管理', question: '在团队中，您对团队氛围的影响是：', options: ['没什么影响，小透明一个', '偶尔会影响大家的情绪', '能带动积极的氛围', '是团队的情绪领袖，能感染所有人'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '人际管理', question: '说服他人接受您的观点时，您：', options: ['很难说服别人', '靠道理和逻辑说服', '能找到对方的利益点来说服', '既能晓之以理，又能动之以情'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '人际管理', question: '对于不喜欢的人，您：', options: ['表现得很明显，甚至针锋相对', '尽量避开，少打交道', '保持表面客气', '能专业地合作，不因个人好恶影响工作'], scores: [1, 2, 3, 4] },
                { module: '情商EQ', dimension: '人际管理', question: '在给予他人反馈时，您：', options: ['要么不说，说了就很难听', '只说好话，怕得罪人', '直接说，但尽量客气', '既能坦诚指出问题，又不让对方感到被攻击'], scores: [1, 2, 3, 4] },

                // ---- 智商IQ (20题) ----
                { module: '智商IQ', dimension: '逻辑推理', question: '所有的玫瑰都是花，有些花会很快凋谢，所以：', options: ['所有玫瑰都会很快凋谢', '有些玫瑰会很快凋谢', '玫瑰不会很快凋谢', '无法确定玫瑰是否会很快凋谢'], scores: [1, 2, 1, 4] },
                { module: '智商IQ', dimension: '逻辑推理', question: '甲比乙高，乙比丙矮，丙比丁高，丁比戊高。以下哪项一定正确？', options: ['甲比丙高', '丙比戊高', '乙比丁高', '甲比戊高'], scores: [1, 4, 1, 2] },
                { module: '智商IQ', dimension: '逻辑推理', question: '某公司有100名员工，其中60人会英语，50人会日语，20人两种都会。那么两种都不会的有多少人？', options: ['10人', '20人', '30人', '40人'], scores: [1, 1, 4, 1] },
                { module: '智商IQ', dimension: '逻辑推理', question: '如果天下雨，地面就会湿。现在地面是湿的，所以：', options: ['一定下雨了', '一定没下雨', '可能下雨了', '不可能下雨'], scores: [1, 1, 4, 1] },
                { module: '智商IQ', dimension: '逻辑推理', question: '找规律：2, 6, 12, 20, 30, ?', options: ['38', '40', '42', '44'], scores: [1, 1, 4, 1] },
                { module: '智商IQ', dimension: '数字推理', question: '找规律：1, 1, 2, 3, 5, 8, ?', options: ['11', '12', '13', '14'], scores: [1, 1, 4, 1] },
                { module: '智商IQ', dimension: '数字推理', question: '找规律：3, 9, 27, 81, ?', options: ['162', '243', '324', '405'], scores: [1, 4, 1, 1] },
                { module: '智商IQ', dimension: '数字推理', question: '一件商品先涨价20%，再降价20%，最终价格与原价相比：', options: ['不变', '涨了4%', '降了4%', '降了2%'], scores: [1, 1, 4, 1] },
                { module: '智商IQ', dimension: '数字推理', question: '一项工程，甲单独做需要10天，乙单独做需要15天。两人合作需要多少天完成？', options: ['5天', '6天', '7天', '8天'], scores: [1, 4, 1, 1] },
                { module: '智商IQ', dimension: '数字推理', question: '找规律：1, 4, 9, 16, 25, ?', options: ['30', '35', '36', '49'], scores: [1, 1, 4, 1] },
                { module: '智商IQ', dimension: '空间想象', question: '一个正方体有6个面，分别涂成红、黄、蓝、绿、白、黑。已知红色对面是蓝色，黄色对面是绿色，那么白色对面是：', options: ['红色', '黄色', '黑色', '绿色'], scores: [1, 1, 4, 1] },
                { module: '智商IQ', dimension: '空间想象', question: '将一张正方形纸对折两次后，在中间剪一个小圆孔，展开后纸上有几个孔？', options: ['1个', '2个', '4个', '8个'], scores: [1, 1, 4, 1] },
                { module: '智商IQ', dimension: '空间想象', question: '从不同角度看同一个立方体，看到的数字分别是1对面是6，2对面是5，3对面是4。请问数字1旁边不可能是哪个数字？', options: ['2', '3', '5', '6'], scores: [1, 1, 1, 4] },
                { module: '智商IQ', dimension: '言语理解', question: '"水落石出"这个成语的意思是：', options: ['水干了石头就露出来了', '事情的真相完全暴露', '石头很重会沉到水底', '水和石头的关系'], scores: [2, 4, 1, 1] },
                { module: '智商IQ', dimension: '言语理解', question: '以下哪个词与其他三个不同类？', options: ['电脑', '手机', '电视机', '洗衣机'], scores: [1, 1, 1, 4] },
                { module: '智商IQ', dimension: '言语理解', question: '"虚心使人进步，骄傲使人落后"这句话告诉我们的道理是：', options: ['要谦虚不要骄傲', '进步和落后是相对的', '骄傲的人一定会落后', '虚心的人一定会进步'], scores: [4, 1, 2, 2] },
                { module: '智商IQ', dimension: '思维策略', question: '有9个外观相同的球，其中一个略重。用天平最少称几次一定能找出那个重球？', options: ['1次', '2次', '3次', '4次'], scores: [1, 4, 2, 1] },
                { module: '智商IQ', dimension: '思维策略', question: '一根绳子烧完需要1小时，粗细不均匀。如何用它来计时半小时？', options: ['量出一半长度烧', '从两端同时烧', '看表计时', '无法做到'], scores: [1, 4, 1, 1] },
                { module: '智商IQ', dimension: '思维策略', question: '有5个人排队，甲不能站在第一个，乙不能站在最后一个，共有多少种排法？', options: ['72种', '78种', '96种', '120种'], scores: [1, 4, 1, 1] },
                { module: '智商IQ', dimension: '思维策略', question: '一个池塘里的睡莲每天面积增加一倍，20天长满整个池塘。请问第几天长满一半？', options: ['第10天', '第15天', '第19天', '第20天'], scores: [1, 1, 4, 1] }
            ];

            // 打乱选项顺序
            (function() {
                function rotateArray(arr, shift) {
                    var n = arr.length;
                    shift = ((shift % n) + n) % n;
                    if (shift === 0) return arr.slice();
                    return arr.slice(-shift).concat(arr.slice(0, -shift));
                }
                questions.forEach(function(q, i) {
                    var shift = (i * 3 + 1) % 4;
                    if (shift > 0) {
                        q.options = rotateArray(q.options, shift);
                        q.scores = rotateArray(q.scores, shift);
                    }
                });
            })();

            // ============================================================
            // 状态变量
            // ============================================================
            var currentQuestion = 0;
            var answers = new Array(questions.length).fill(null);
            var userInfo = {};

            var t12Dimensions = {
                '开拓型': { desc: '开创进取，引领变革', field: '开拓影响' },
                '管控型': { desc: '掌控全局，严格执行', field: '开拓影响' },
                '交际型': { desc: '善于沟通，影响他人', field: '开拓影响' },
                '服务型': { desc: '客户至上，贴心服务', field: '服务关系' },
                '亲和型': { desc: '和谐友善，团队凝聚', field: '服务关系' },
                '规划型': { desc: '运筹帷幄，深谋远虑', field: '研发策划' },
                '发明型': { desc: '创新思维，突破常规', field: '研发策划' },
                '事务型': { desc: '严谨细致，井井有条', field: '事物执行' },
                '操作型': { desc: '动手能力强，实操落地', field: '事物执行' },
                '技能型': { desc: '专业精深，技术专家', field: '事物执行' }
            };

            var f4Fields = {
                '开拓影响': '开拓影响领域：战略控制、资源整合、推动变革',
                '服务关系': '服务关系领域：人际互动、客户服务、团队协作',
                '研发策划': '研发策划领域：抽象创新、战略规划、创意设计',
                '事物执行': '事物执行领域：流程管理、具体执行、操作落地'
            };

            var leadershipStyles = {
                '开拓型': '【开拓型领导者】- 愿景驱动，敢于冒险，适合变革与增长。需注意平衡风险。',
                '管控型': '【管控型领导者】- 结果导向，纪律严明，擅长体系化运营。需增强灵活性。',
                '亲和型': '【亲和型领导者】- 以人为本，善于凝聚团队。需强化决策果断性。',
                '专家型': '【专家型领导者】- 专业精深，逻辑严谨，适合技术/质量领域。需提升人际影响力。'
            };

            // ============================================================
            // 核心函数
            // ============================================================

            function showSection(id) {
                var sections = document.querySelectorAll('.section');
                sections.forEach(function(s) {
                    s.style.display = 'none';
                    s.classList.remove('active');
                });
                var el = document.getElementById(id);
                el.classList.add('active');
                if (id === 'result') {
                    el.style.display = 'grid';
                } else {
                    el.style.display = 'block';
                }
            }

            document.addEventListener('DOMContentLoaded', function() {
                showSection('welcome');
            });

            window.startQuiz = function() {
                var name = document.getElementById('userName').value.trim();
                var dept = document.getElementById('userDept').value.trim();
                var industry = document.getElementById('industry').value;
                var errMsg = '';
                if (!name) errMsg += '姓名/测评人 ';
                if (!dept) errMsg += '部门/岗位 ';
                if (!industry) errMsg += '所属行业 ';
                if (errMsg) {
                    alert('⚠️ 以下信息为必填项，请完整填写后再开始测评：\n' + errMsg);
                    if (!name) document.getElementById('userName').focus();
                    else if (!dept) document.getElementById('userDept').focus();
                    else document.getElementById('industry').focus();
                    return;
                }

                userInfo.name = name;
                userInfo.dept = dept;
                userInfo.industry = industry;
                userInfo.date = new Date().toLocaleDateString('zh-CN');

                showSection('quiz');
                renderQuestion();
            };

            function renderQuestion() {
                var q = questions[currentQuestion];
                document.getElementById('moduleBadge').textContent = q.module;
                var progress = ((currentQuestion + 1) / questions.length) * 100;
                document.getElementById('progressFill').style.width = progress + '%';
                document.getElementById('progressInfo').textContent = '第 ' + (currentQuestion + 1) + ' 题 / 共 ' + questions
                    .length + ' 题';
                document.getElementById('questionText').textContent = q.question;

                var optionsHtml = q.options.map(function(opt, i) {
                    return '<div class="option-item ' + (answers[currentQuestion] === i ? 'selected' : '') +
                        '" onclick="window.selectOption(' + i + ')">' +
                        '<div class="option-label">' + String.fromCharCode(65 + i) + '</div>' +
                        '<div class="option-content">' + opt + '</div>' +
                        '</div>';
                }).join('');
                document.getElementById('optionsContainer').innerHTML = optionsHtml;

                document.getElementById('prevBtn').disabled = (currentQuestion === 0);
                document.getElementById('nextBtn').disabled = (answers[currentQuestion] === null);
                document.getElementById('nextBtn').textContent = (currentQuestion === questions.length - 1) ? '查看结果 →' :
                    '下一题 →';
            }

            window.selectOption = function(index) {
                answers[currentQuestion] = index;
                renderQuestion();
            };
            window.prevQuestion = function() {
                if (currentQuestion > 0) { currentQuestion--;
                    renderQuestion(); }
            };
            window.nextQuestion = function() {
                if (answers[currentQuestion] === null) return;
                if (currentQuestion < questions.length - 1) {
                    currentQuestion++;
                    renderQuestion();
                } else {
                    showResult();
                }
            };

            // ==================== 评分引擎 ====================
            function calculateScores() {
                var result = {
                    modules: { '职业素质': 0, '职业性格': 0, '情商EQ': 0, '智商IQ': 0 },
                    moduleCounts: { '职业素质': 0, '职业性格': 0, '情商EQ': 0, '智商IQ': 0 },
                    dimensions: {},
                    t12: {},
                    f4: { '开拓影响': 0, '服务关系': 0, '研发策划': 0, '事物执行': 0 },
                    f4Counts: { '开拓影响': 0, '服务关系': 0, '研发策划': 0, '事物执行': 0 }
                };

                questions.forEach(function(q, i) {
                    var score = q.scores[answers[i]];
                    var module = q.module;
                    var dimension = q.dimension;

                    result.modules[module] += score;
                    result.moduleCounts[module]++;

                    if (!result.dimensions[dimension]) {
                        result.dimensions[dimension] = 0;
                        result.dimensions[dimension + '_count'] = 0;
                    }
                    result.dimensions[dimension] += score;
                    result.dimensions[dimension + '_count']++;

                    if (module === '职业性格') {
                        if (!result.t12[dimension]) {
                            result.t12[dimension] = { score: 0, count: 0 };
                        }
                        result.t12[dimension].score += score;
                        result.t12[dimension].count++;
                    }
                });

                // 百分制
                Object.keys(result.modules).forEach(function(m) {
                    result.modules[m] = Math.round((result.modules[m] / (result.moduleCounts[m] * 4)) * 100);
                });

                // T12线性映射
                Object.keys(result.t12).forEach(function(t) {
                    var avg = result.t12[t].score / result.t12[t].count;
                    result.t12[t] = Math.round(30 + avg * 10);
                });

                // F4
                Object.keys(t12Dimensions).forEach(function(t) {
                    if (result.t12[t]) {
                        var field = t12Dimensions[t].field;
                        result.f4[field] += result.t12[t];
                        result.f4Counts[field]++;
                    }
                });
                Object.keys(result.f4).forEach(function(f) {
                    if (result.f4Counts[f] > 0) {
                        result.f4[f] = Math.round(result.f4[f] / result.f4Counts[f]);
                    }
                });

                // 维度百分制
                Object.keys(result.dimensions).forEach(function(d) {
                    if (!d.endsWith('_count')) {
                        var count = result.dimensions[d + '_count'];
                        result.dimensions[d] = Math.round((result.dimensions[d] / (count * 4)) * 100);
                    }
                });

                return result;
            }

            function getScoreLevel(score) {
                if (score >= 80) return { text: '优秀', class: 'tag-excellent' };
                if (score >= 60) return { text: '良好', class: 'tag-good' };
                return { text: '待提升', class: 'tag-need' };
            }

            function getT12Level(score) {
                if (score >= 65) return { text: '优势', class: 'tag-excellent' };
                if (score >= 50) return { text: '一般', class: 'tag-good' };
                return { text: '待发展', class: 'tag-need' };
            }

            function renderScoreBar(name, score) {
                var level = getScoreLevel(score);
                return '<div class="score-row"><div class="score-name">' + name + '</div><div class="score-bar"><div class="score-fill" style="width:' +
                    score + '%"></div></div><div class="score-value">' + score + '</div><span class="score-tag ' + level
                    .class + '">' + level.text + '</span></div>';
            }

            // ==================== 显示结果 ====================
            function showResult() {
                var scores = calculateScores();

                document.getElementById('resultName').textContent = userInfo.name;
                document.getElementById('resultDept').textContent = userInfo.dept;
                document.getElementById('resultIndustry').textContent = userInfo.industry;
                document.getElementById('resultDate').textContent = userInfo.date;

                var sortedT12 = Object.entries(scores.t12).sort(function(a, b) { return b[1] - a[1]; });
                var top3 = sortedT12.slice(0, 3);
                var bottom3 = sortedT12.slice(-3).reverse();

                // ---- 候选人核心优势摘要 ----
                var industry = userInfo.industry;
                var fitMap = {
                    '制造业': ['管控型', '事务型', '技能型'],
                    '电商零售': ['开拓型', '交际型', '服务型'],
                    '互联网': ['开拓型', '发明型', '规划型'],
                    '金融': ['管控型', '规划型', '技能型'],
                    '医疗健康': ['服务型', '技能型', '管控型'],
                    '教育': ['服务型', '交际型', '规划型'],
                    '其他': ['亲和型', '交际型', '服务型']
                };
                var needed = fitMap[industry] || ['亲和型', '交际型', '服务型'];
                var matchCount = 0,
                    matchDetails = [];
                sortedT12.forEach(function(item) {
                    if (needed.includes(item[0])) {
                        matchCount++;
                        matchDetails.push(item[0] + '(' + item[1] + '分)');
                    }
                });
                var fitLevel = matchCount >= 2 ? '高度匹配' : (matchCount >= 1 ? '部分匹配' : '匹配度较低');

                var summary = '<div class="recruit-highlight"><strong>🔍 候选人核心优势：</strong> ' +
                    top3.map(function(item) { return item[0] + '（' + item[1] + '分）'; }).join('、') +
                    '</div>';
                summary += '<div class="recruit-highlight"><strong>💼 管理潜力评估：</strong> ' +
                    (scores.modules['职业素质'] >= 80 ? '具备较强的管理能力，可胜任主管及以上岗位。' :
                        scores.modules['职业素质'] >= 60 ? '具备一定的管理基础，可通过培养提升。' :
                        '管理经验尚浅，建议从基层管理岗位开始锻炼。') +
                    '</div>';
                summary += '<div class="recruit-highlight"><strong>📈 岗位匹配建议：</strong> ' +
                    '根据行业（' + industry + '）需求，候选人的核心特质与岗位的匹配度为 <strong>' + fitLevel + '</strong>，';
                if (matchCount >= 2) summary += '建议重点考察其管理潜力和执行力。';
                else if (matchCount >= 1) summary += '可安排进一步面试，关注其学习能力和适应力。';
                else summary += '建议审慎评估，或考虑其他更匹配的人选。';
                summary += '</div>';
                document.getElementById('recruitSummary').innerHTML = summary;

                // ---- 四维综合评估 ----
                var overallHtml = '';
                var moduleNames = { '职业素质': '职业素质', '职业性格': '职业性格', '情商EQ': '情商EQ', '智商IQ': '智商IQ' };
                Object.keys(scores.modules).forEach(function(m) {
                    overallHtml += renderScoreBar(moduleNames[m], scores.modules[m]);
                });
                document.getElementById('overallScores').innerHTML = overallHtml;

                var overallText = '';
                var q = scores.modules['职业素质'];
                if (q >= 80) overallText += '💼 职业素质优秀，具备出色的管理能力。';
                else if (q >= 60) overallText += '💼 职业素质良好，部分维度有提升空间。';
                else overallText += '💼 职业素质需加强，建议系统学习管理知识。';
                var p = scores.modules['职业性格'];
                if (p >= 80) overallText += ' 🎭 性格特质鲜明，优势突出。';
                else if (p >= 60) overallText += ' 🎭 性格特征均衡，可进一步挖掘核心优势。';
                else overallText += ' 🎭 性格特质有待清晰，建议进行职业锚探索。';
                var eq = scores.modules['情商EQ'];
                if (eq >= 80) overallText += ' ❤️ 情商极高，是卓越领导者的重要基石。';
                else if (eq >= 60) overallText += ' ❤️ 情商良好，可继续提升情绪管理与人际影响力。';
                else overallText += ' ❤️ 情商是当前短板，建议重点修炼。';
                var iq = scores.modules['智商IQ'];
                if (iq >= 80) overallText += ' 🧠 认知能力优秀，善于逻辑分析与策略思考。';
                else if (iq >= 60) overallText += ' 🧠 认知能力良好，可进一步训练系统性思维。';
                else overallText += ' 🧠 认知能力需加强，建议多进行逻辑训练。';
                document.getElementById('overallInterpretation').innerHTML = '<p>' + overallText + '</p>';

                // ---- 领导力风格 ----
                var style = identifyLeadershipStyle(sortedT12);
                document.getElementById('leadershipStyle').innerHTML = '<p>' + leadershipStyles[style] +
                    '</p><p style="margin-top:2px;">基于您的 T12 特质组合，您更倾向于 <strong>' + style + '</strong> 领导风格。</p>';

                // ---- T12职业优势排行 ----
                var t12Html = '';
                sortedT12.forEach(function(item) {
                    var name = item[0],
                        score = item[1];
                    t12Html += '<div class="t12-card"><div class="t12-name">' + name + '</div><div class="t12-score">' +
                        score + '</div><div class="t12-desc">' + (t12Dimensions[name] ? t12Dimensions[name].desc :
                        '') + '</div></div>';
                });
                document.getElementById('t12Grid').innerHTML = t12Html;

                // ---- 优势与关键发展领域 ----
                var swHtml = '<p><strong>🌟 核心优势（TOP3）</strong></p>';
                top3.forEach(function(item) {
                    swHtml += '<p>• <strong>' + item[0] + '</strong>（' + item[1] + '分）—— ' + (t12Dimensions[item[0]] ?
                        t12Dimensions[item[0]].desc : '') + '</p>';
                });
                swHtml += '<p style="margin-top:4px;"><strong>⚡ 关键发展领域（Bottom3）</strong></p>';
                bottom3.forEach(function(item) {
                    swHtml += '<p>• <strong>' + item[0] + '</strong>（' + item[1] + '分）—— 建议有意识地锻炼或寻求互补合作。</p>';
                });
                document.getElementById('strengthWeakness').innerHTML = swHtml;

                // ---- 四大职业领域分析 ----
                var sortedF4 = Object.entries(scores.f4).sort(function(a, b) { return b[1] - a[1]; });
                var topField = sortedF4[0][0];
                var f4Html = '<p>您的职业优势领域分布如下：</p>';
                sortedF4.forEach(function(item, idx) {
                    var field = item[0],
                        score = item[1];
                    var level = getT12Level(score);
                    f4Html += '<p><strong>' + (idx + 1) + '. ' + field + '（' + score + '分 - ' + level.text +
                        '）</strong><br><span style="font-size:10px;color:#666;">' + f4Fields[field] + '</span></p>';
                });
                f4Html += '<p style="margin-top:2px;"><strong>✨ 核心优势领域：' + topField + '</strong></p>';
                document.getElementById('f4Analysis').innerHTML = f4Html;

                // ---- 职业素质细分 ----
                var qualityDims = ['战略思维', '决策执行', '团队管理', '沟通协调', '问题解决', '成就导向'];
                var qualityHtml = '';
                qualityDims.forEach(function(dim) {
                    if (scores.dimensions[dim]) qualityHtml += renderScoreBar(dim, scores.dimensions[dim]);
                });
                document.getElementById('qualityScores').innerHTML = qualityHtml;

                // ---- 性格画像分析 ----
                var personalityHtml = '<p><strong>🌟 显著优势特质（TOP3）</strong></p>';
                top3.forEach(function(item) {
                    personalityHtml += '<p>• <strong>' + item[0] + '</strong>（' + item[1] +
                        '分）<br><span style="font-size:10px;color:#666;">' + (t12Dimensions[item[0]] ? t12Dimensions[
                            item[0]].desc : '') + '</span></p>';
                });
                personalityHtml += '<p style="margin-top:4px;"><strong>⚖️ 待发展特质（Bottom3）</strong></p>';
                bottom3.forEach(function(item) {
                    personalityHtml += '<p>• <strong>' + item[0] + '</strong>（' + item[1] +
                        '分）<br><span style="font-size:10px;color:#666;">' + (t12Dimensions[item[0]] ? t12Dimensions[
                            item[0]].desc : '') + '</span></p>';
                });
                document.getElementById('personalityAnalysis').innerHTML = personalityHtml;

                // ---- 情商(EQ)深度解析 ----
                var eqDims = ['自我认知', '情绪管理', '自我激励', '共情能力', '人际管理'];
                var eqHtml = '';
                eqDims.forEach(function(dim) {
                    if (scores.dimensions[dim]) eqHtml += renderScoreBar(dim, scores.dimensions[dim]);
                });
                document.getElementById('eqScores').innerHTML = eqHtml;
                var eqScore = scores.modules['情商EQ'];
                var eqInterp = '';
                if (eqScore >= 80) eqInterp =
                    '您的情商能力非常突出，尤其在自我认知和人际管理方面表现出色。这使您能够在复杂人际环境中游刃有余，是卓越领导者的重要特质。';
                else if (eqScore >= 60) eqInterp =
                    '您的情商处于良好水平，具备基本的情绪感知和关系处理能力。建议在共情能力和冲突管理方面进行刻意练习，可显著提升领导效能。';
                else eqInterp =
                    '您的情商能力有待提升，建议从自我觉察开始，学习情绪调节技巧，并多参与团队协作活动，逐步增强人际敏感度。';
                document.getElementById('eqInterpretation').innerHTML = '<p>' + eqInterp + '</p>';

                // ---- 智商(IQ)认知能力 ----
                var iqScore = scores.modules['智商IQ'];
                var iqLevel = getScoreLevel(iqScore);
                var iqHtml = '<p><strong>IQ综合评价：</strong>' + iqScore + '分，' + iqLevel.text + '水平。</p><p>本次测评从逻辑推理、数字推理、空间想象、言语理解、思维策略等维度评估。';
                if (iqScore >= 80) iqHtml += '您的认知能力优秀，善于结构化分析和解决复杂问题。';
                else if (iqScore >= 60) iqHtml += '您的认知能力良好，可加强系统性思维和量化分析训练。';
                else iqHtml += '建议多进行逻辑思维训练，如案例分析、数据分析等。';
                iqHtml += '</p>';
                document.getElementById('iqAnalysis').innerHTML = iqHtml;

                // ---- 行业适配度与岗位匹配 ----
                var fitHtml = '<p><strong>您所选行业：' + industry + '</strong></p>';
                fitHtml += '<p>该行业通常需要的关键特质：<strong>' + needed.join('、') + '</strong></p>';
                fitHtml += '<p>您的匹配度：<strong>' + fitLevel + '</strong>（匹配项：' + (matchDetails.length ? matchDetails.join(
                    '、') : '无') + '）</p>';
                if (matchCount >= 2) fitHtml += '<p>✅ 您的特质与行业需求高度契合，建议聚焦优势领域发展。</p>';
                else if (matchCount >= 1) fitHtml += '<p>📌 您的部分特质符合行业需求，可加强短板领域的锻炼，提升综合竞争力。</p>';
                else fitHtml += '<p>⚠️ 您的特质与行业主流需求偏差较大，建议重新审视职业方向或主动调整能力结构。</p>';
                document.getElementById('industryFit').innerHTML = fitHtml;

                // ---- 招聘用人建议 ----
                var recruitSuggestions = [];
                recruitSuggestions.push('• 根据候选人的四维得分，重点关注其优势领域（' + top3[0][0] + '、' + top3[1][0] + '）在岗位中的应用。');
                if (scores.modules['情商EQ'] < 60) {
                    recruitSuggestions.push(
                        '• 候选人情商低于平均，建议在面试中增加情境模拟，考察其应对压力和人际交往的真实表现。'
                    );
                }
                if (scores.modules['职业素质'] < 60) {
                    recruitSuggestions.push('• 候选人职业素质有待提升，建议入职后安排系统性的管理培训，并配备导师带教。');
                }
                if (matchCount >= 2) {
                    recruitSuggestions.push('• 候选人与行业匹配度高，建议快速推进录用流程，避免人才流失。');
                } else if (matchCount === 1) {
                    recruitSuggestions.push('• 候选人部分特质匹配，建议在录用后安排针对性强的轮岗或项目历练，加速能力补齐。');
                } else {
                    recruitSuggestions.push('• 候选人匹配度较低，若仍考虑录用，建议调整岗位定位或设定明确的试用期考核目标。');
                }
                recruitSuggestions.push('• 建议结合公司文化和团队现状，综合判断候选人的长期适配性。');
                document.getElementById('recruitSuggestions').innerHTML = recruitSuggestions.map(function(s) { return '<li>' +
                        s + '</li>'; }).join('');

                // ---- 个人发展建议 ----
                var personalSuggestions = generatePersonalSuggestions(scores);
                document.getElementById('personalSuggestions').innerHTML = personalSuggestions.map(function(s) { return '<li>' +
                        s + '</li>'; }).join('');

                showSection('result');
                window.resultData = { scores: scores, userInfo: userInfo, sortedT12: sortedT12, sortedF4: sortedF4,
                    style: style };
            }

            function identifyLeadershipStyle(sortedT12) {
                var top = sortedT12.slice(0, 3).map(function(item) { return item[0]; });
                if (top.includes('开拓型') && top.includes('管控型')) return '开拓型';
                if (top.includes('管控型') && top.includes('事务型')) return '管控型';
                if (top.includes('亲和型') && top.includes('服务型')) return '亲和型';
                if (top.includes('技能型') || top.includes('规划型')) return '专家型';
                return '管控型';
            }

            function generatePersonalSuggestions(scores) {
                var list = [];
                var topT12 = Object.entries(scores.t12).sort(function(a, b) { return b[1] - a[1]; })[0];
                var bottomT12 = Object.entries(scores.t12).sort(function(a, b) { return a[1] - b[1]; })[0];
                var ind = userInfo.industry;

                list.push('【短期（1-3个月）】');
                list.push('• 聚焦核心优势（' + topT12[0] + '），主动承担相关任务，积累成功案例。');
                if (bottomT12[1] < 50) {
                    list.push('• 针对短板（' + bottomT12[0] + '），每天花15分钟进行专项练习（如沟通、逻辑、情绪记录等）。');
                }
                list.push('• 完成1次360度反馈，获取同事、下属、上级的真实评价，明确自我认知偏差。');
                list.push('• 阅读1本与领导力相关的经典书籍（如《高效能人士的七个习惯》《领导力21法则》）。');

                list.push('【中期（3-12个月）】');
                if (scores.modules['情商EQ'] < 60) {
                    list.push('• 参加情商或沟通技巧培训，每月至少进行一次演讲或主持，提升表达能力。');
                }
                if (scores.modules['职业素质'] < 60) {
                    list.push('• 参与跨部门项目或公司级项目，锻炼系统思维和决策能力。');
                }
                list.push('• 建立个人"优势-挑战"日志，每周复盘关键事件，提炼经验。');
                list.push('• 寻找一位资深导师，每月进行1次职业发展对话，获取反馈和指导。');

                list.push('【长期（1-3年）】');
                if (ind === '制造业') {
                    list.push('• 深入精益生产、智能制造、供应链管理等领域，考取相关认证（如六西格玛黑带）。');
                } else if (ind === '电商零售') {
                    list.push('• 深耕数字化运营、私域流量、数据驱动决策，关注新零售趋势。');
                } else if (ind === '互联网') {
                    list.push('• 关注技术前沿（AI、区块链等），提升产品思维和敏捷管理能力。');
                } else if (ind === '金融') {
                    list.push('• 强化风险管理、合规治理、金融科技应用，考取CFA/FRM等证书。');
                } else if (ind === '医疗健康') {
                    list.push('• 关注医疗政策、患者体验、数字化转型，提升医院或机构管理能力。');
                } else if (ind === '教育') {
                    list.push('• 关注教育政策、课程创新、教育科技，打造个人教学品牌。');
                } else {
                    list.push('• 根据行业趋势，制定3年职业发展规划，明确目标岗位所需能力。');
                }
                list.push('• 系统学习战略管理、财务分析、组织行为学等进阶课程（如MBA或专业认证）。');
                list.push('• 逐步向更高管理岗位靠拢，承担更大责任，锻炼综合领导能力。');
                list.push('• 建立个人品牌，在专业社群或行业会议中分享见解，扩大影响力。');

                return list;
            }

            // ==================== PDF下载 & 复制 & 重启 ====================
            window.downloadPDF = function(btn) {
                if (!window.resultData) { alert('请先完成测评！'); return; }
                var originalText = btn.innerHTML;
                btn.innerHTML = '⏳ 正在生成PDF...';
                btn.disabled = true;
                var resultEl = document.getElementById('result');
                html2canvas(resultEl, {
                    scale: 1.8,
                    useCORS: true,
                    logging: false,
                    backgroundColor: '#ffffff',
                    height: resultEl.scrollHeight,
                    windowHeight: resultEl.scrollHeight,
                    windowWidth: resultEl.scrollWidth,
                }).then(function(canvas) {
                    var imgData = canvas.toDataURL('image/jpeg', 0.92);
                    var pdf = new jspdf.jsPDF('p', 'mm', 'a4');
                    var pdfWidth = pdf.internal.pageSize.getWidth();
                    var pdfHeight = pdf.internal.pageSize.getHeight();
                    var imgWidth = canvas.width,
                        imgHeight = canvas.height;
                    var ratio = pdfWidth / imgWidth;
                    var imgPdfHeight = imgHeight * ratio;
                    if (imgPdfHeight <= pdfHeight) {
                        var yOffset = (pdfHeight - imgPdfHeight) / 2;
                        pdf.addImage(imgData, 'JPEG', 0, yOffset, pdfWidth, imgPdfHeight);
                    } else {
                        var pageHeightPx = pdfHeight / ratio;
                        var totalPages = Math.ceil(imgHeight / pageHeightPx);
                        for (var i = 0; i < totalPages; i++) {
                            if (i > 0) pdf.addPage();
                            var srcY = i * pageHeightPx;
                            var srcH = Math.min(pageHeightPx, imgHeight - srcY);
                            var tempCanvas = document.createElement('canvas');
                            tempCanvas.width = imgWidth;
                            tempCanvas.height = srcH;
                            var ctx = tempCanvas.getContext('2d');
                            ctx.drawImage(canvas, 0, srcY, imgWidth, srcH, 0, 0, imgWidth, srcH);
                            var pageData = tempCanvas.toDataURL('image/jpeg', 0.92);
                            var pageImgH = srcH * ratio;
                            var yOff = (pdfHeight - pageImgH) / 2;
                            pdf.addImage(pageData, 'JPEG', 0, yOff, pdfWidth, pageImgH);
                        }
                    }
                    var pageCount = pdf.internal.getNumberOfPages();
                    for (var p = 1; p <= pageCount; p++) {
                        pdf.setPage(p);
                        pdf.setFontSize(10);
                        pdf.setTextColor(150, 150, 150);
                        pdf.text('第 ' + p + ' 页 / 共 ' + pageCount + ' 页', pdfWidth / 2, pdfHeight - 8, { align: 'center' });
                    }
                    var fileName = 'T12测评报告_' + userInfo.name + '_' + userInfo.date.replace(/\//g, '-') + '.pdf';
                    pdf.save(fileName);
                    btn.innerHTML = '✅ 下载成功';
                    btn.style.background = 'linear-gradient(135deg,#28a745 0%,#20c997 100%)';
                    setTimeout(function() {
                        btn.innerHTML = originalText;
                        btn.style.background = '';
                        btn.disabled = false;
                    }, 2500);
                }).catch(function(err) {
                    alert('生成PDF失败，请重试。错误：' + err.message);
                    btn.innerHTML = originalText;
                    btn.disabled = false;
                });
            };

            window.copyResult = function() {
                var text = generateResultText();
                var ta = document.createElement('textarea');
                ta.value = text;
                document.body.appendChild(ta);
                ta.select();
                document.execCommand('copy');
                document.body.removeChild(ta);
                alert('✅ 测评结果已复制到剪贴板！');
            };

            function generateResultText() {
                var data = window.resultData;
                if (!data) return '暂无数据';
                var scores = data.scores,
                    userInfo = data.userInfo,
                    sortedT12 = data.sortedT12,
                    sortedF4 = data.sortedF4;
                var lines = [];
                lines.push('========================================');
                lines.push('  T12职业优势测评报告（招聘甄选版）');
                lines.push('  设计人：DAVY（抖音号：d190564680）');
                lines.push('========================================\n');
                lines.push('【候选人】' + userInfo.name + ' | ' + userInfo.dept + ' | ' + userInfo.industry + ' | ' + userInfo
                    .date);
                lines.push('\n【四维综合】');
                lines.push('职业素质：' + scores.modules['职业素质'] + '分');
                lines.push('职业性格：' + scores.modules['职业性格'] + '分');
                lines.push('情商EQ：' + scores.modules['情商EQ'] + '分');
                lines.push('智商IQ：' + scores.modules['智商IQ'] + '分');
                lines.push('\n【T12优势排行】');
                sortedT12.forEach(function(item, idx) {
                    lines.push((idx + 1) + '. ' + item[0] + '：' + item[1] + '分 - ' + (t12Dimensions[item[0]] ?
                        t12Dimensions[item[0]].desc : ''));
                });
                lines.push('\n【四大领域】');
                sortedF4.forEach(function(item) {
                    lines.push(item[0] + '：' + item[1] + '分');
                });
                lines.push('\n【领导力风格】' + data.style);
                lines.push('\n【招聘建议】');
                var recruitList = document.getElementById('recruitSuggestions').querySelectorAll('li');
                recruitList.forEach(function(li) {
                    lines.push('• ' + li.textContent);
                });
                lines.push('\n【个人发展建议】');
                var personalList = document.getElementById('personalSuggestions').querySelectorAll('li');
                personalList.forEach(function(li) {
                    lines.push('• ' + li.textContent);
                });
                lines.push('\n========================================');
                lines.push('  报告生成时间：' + new Date().toLocaleString());
                lines.push('========================================');
                return lines.join('\n');
            }

            window.restartQuiz = function() {
                currentQuestion = 0;
                answers = new Array(questions.length).fill(null);
                window.resultData = null;
                showSection('welcome');
            };

            // 键盘快捷键
            document.addEventListener('keydown', function(e) {
                var quizActive = document.getElementById('quiz').classList.contains('active');
                if (!quizActive) return;
                if (e.key >= '1' && e.key <= '4') {
                    var idx = parseInt(e.key) - 1;
                    if (idx < questions[currentQuestion].options.length) window.selectOption(idx);
                } else if (e.key === 'ArrowLeft') window.prevQuestion();
                else if (e.key === 'ArrowRight' || e.key === 'Enter') {
                    if (answers[currentQuestion] !== null) window.nextQuestion();
                }
            });

        })();
    </script>
</body>
</html>
