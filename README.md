<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <title>综合人才能力测评 · 通道定制版（120题）</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js">
    </script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js">
    </script>
    <style>
        /* ===== 全局样式 ===== */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Microsoft YaHei', 'PingFang SC', sans-serif;
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
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
        .section {
            display: none;
        }
        .section.active {
            display: block;
        }
        #result.active {
            display: grid !important;
        }

        /* 欢迎页 */
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
        .field-hint {
            font-size: 12px;
            color: #888;
            margin-top: -8px;
            margin-bottom: 10px;
        }
        .dimensions-grid {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
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
        .btn-success {
            background: #28a745;
        }
        .btn-success:hover {
            background: #218838;
            box-shadow: 0 8px 20px rgba(40, 167, 69, 0.4);
        }
        .btn-wechat {
            background: #07c160;
        }
        .btn-wechat:hover {
            background: #06ad56;
            box-shadow: 0 8px 20px rgba(7, 193, 96, 0.4);
        }

        /* 答题页 */
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

        /* ===== 结果页 ===== */
        #result {
            display: none;
            gap: 6px;
            padding: 0;
            grid-template-columns: 1fr 1fr 1fr;
            position: relative;
        }
        #result.active {
            display: grid !important;
        }
        #result .result-header {
            grid-column: 1/-1;
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

        /* 面板样式 - 所有面板统一为1列（等宽） */
        .panel {
            background: #fafbfc;
            border-radius: 6px;
            padding: 6px 10px;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.03);
            display: flex;
            flex-direction: column;
            grid-column: span 1;
        }
        .panel-warning {
            background: #fff8e7;
            border: 1px solid #ffc107;
        }
        .panel-warning h3 {
            color: #856404;
        }
        .panel-warning .analysis-box {
            border-left-color: #ffc107;
            background: #fffbf0;
        }
        .panel .analysis-box {
            border-left-color: #667eea;
        }

        .panel .suggestions-list li::before {
            content: '✓';
            position: absolute;
            left: 0;
            top: 3px;
            font-size: 11px;
            color: #28a745;
            font-weight: bold;
        }

        /* 雷达图容器 */
        .radar-container {
            display: flex;
            justify-content: center;
            align-items: center;
            flex: 1;
            min-height: 180px;
        }
        #radarChart {
            width: 100%;
            height: auto;
            max-width: 220px;
            aspect-ratio: 1/1;
            background: #fff;
            border-radius: 4px;
        }

        /* 占满三列的模块 */
        .full-width {
            grid-column: 1/-1;
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
        .tag-warning {
            background: #ffe5b4;
            color: #8a6d3b;
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

        /* ===== 结果页按钮区域 ===== */
        .result-actions {
            grid-column: 1/-1;
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

        /* ===== 分享提示 ===== */
        .share-hint {
            grid-column: 1/-1;
            background: #f0f9f0;
            border-radius: 6px;
            padding: 8px 12px;
            font-size: 12px;
            color: #1e5a3a;
            border: 1px solid #b8e0c0;
            margin-top: 4px;
            text-align: center;
            line-height: 1.6;
        }
        .share-hint strong {
            color: #07c160;
        }

        .footer-note {
            grid-column: 1/-1;
            text-align: center;
            font-size: 10px;
            color: #999;
            margin-top: 8px;
            padding-top: 6px;
            border-top: 1px solid #eee;
        }

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
            .analysis-box {
                -webkit-print-color-adjust: exact !important;
                print-color-adjust: exact !important;
            }
            .score-fill {
                -webkit-print-color-adjust: exact !important;
                print-color-adjust: exact !important;
            }
            .btn,
            .result-actions,
            .quiz-footer,
            .share-hint {
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
                grid-template-columns: 1fr 1fr 1fr !important;
            }
            .panel {
                grid-column: span 1 !important;
            }
            .full-width {
                grid-column: 1/-1 !important;
            }
            #radarChart {
                max-width: 160px;
            }
        }

        @media (max-width: 768px) {
            .dimensions-grid {
                grid-template-columns: 1fr 1fr;
            }
            #result {
                grid-template-columns: 1fr !important;
            }
            .panel {
                grid-column: 1 / -1 !important;
            }
            #radarChart {
                max-width: 200px;
            }
        }
        @media (max-width: 480px) {
            .dimensions-grid {
                grid-template-columns: 1fr 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🧠 综合人才能力测评 · 通道定制版（120题）</h1>
            <p>管理·技术·幕僚 专属题库 | MBTI·DISC·PDP·OPQ·SHL·北森·IQ·EQ</p>
            <p style="margin-top:4px;font-size:12px;opacity:0.8;">设计人：DAVY（抖音号：d190564680）</p>
        </div>
        <div class="content">
            <!-- ====== 欢迎页 ====== -->
            <div id="welcome" class="section active">
                <div class="welcome-page">
                    <h2>人才测评 · 专业版</h2>
                    <div class="intro">
                        <h3>📋 测评说明</h3>
                        <p>本测评体系由人力资源管理师<strong>DAVY</strong>设计，根据您选择的行业自动匹配考核场景，以招聘为核心导向，基于心理学、组织行为学及人才管理理论构建多维评估模型，从<strong>MBTI、DISC、PDP、OPQ、SHL、北森、IQ、EQ</strong>八大核心维度，对候选人进行系统性、动态化评估。通过定量测评工具与定性行为分析相结合的方法，构建标准化与个性化并重的评估矩阵，旨在为企业提供精准的用人决策参考，优化人才匹配效能；同时赋能个体深度认知自身优势特征及发展象限，支持职业路径规划与潜能开发。测评结果以可视化多维雷达图及诊断性报告呈现，兼具科学性与实践指导价值，适用于战略性招聘筛选、人才梯队建设及人岗动态适配等多场景应用。</p>
                        <p style="margin-top:8px;">⏱️ 时长：25-32分钟 | 📊 专业分析报告 | 📄 支持PDF下载 | 📧 一键邮件发送 | 💬 微信转发</p>
                    </div>
                    <div class="dimensions-grid">
                        <div class="dimension-card"><div class="icon">🎭</div><h4>MBTI 性格类型</h4><p>30题</p></div>
                        <div class="dimension-card"><div class="icon">📊</div><h4>DISC 行为风格</h4><p>20题</p></div>
                        <div class="dimension-card"><div class="icon">🐾</div><h4>PDP 天赋特质</h4><p>10题</p></div>
                        <div class="dimension-card"><div class="icon">💼</div><h4>OPQ 管理潜质</h4><p>10题 · 通道定制</p></div>
                        <div class="dimension-card"><div class="icon">🧠</div><h4>SHL 认知能力</h4><p>10题 · 通道定制 · 含高难度</p></div>
                        <div class="dimension-card"><div class="icon">🌟</div><h4>北森 素养情商</h4><p>20题 · 通道定制</p></div>
                        <div class="dimension-card"><div class="icon">🔍</div><h4>IQ 智力测试</h4><p>10题 · 行业定制</p></div>
                        <div class="dimension-card"><div class="icon">❤️</div><h4>EQ 情商测试</h4><p>10题 · 通道定制</p></div>
                    </div>
                    <div class="info-form">
                        <label>候选人姓名 <span class="required-star">*</span></label>
                        <input type="text" id="userName" placeholder="请填写候选人姓名">
                        <label>应聘岗位 <span class="required-star">*</span></label>
                        <input type="text" id="userDept" placeholder="如：部门经理、技术主管">
                        <label>所属行业 <span class="required-star">*</span></label>
                        <select id="industry"><option value="">-- 请选择 --</option><option value="电商">电商</option><option value="制造业">制造业</option><option value="金融">金融</option><option value="工贸">工贸</option><option value="互联网">互联网</option><option value="其他">其他</option></select>
                        <label>职业通道 <span class="required-star">*</span></label>
                        <select id="channel"><option value="">-- 请选择 --</option><option value="管理">管理通道</option><option value="技术">技术通道</option><option value="幕僚">幕僚通道</option></select>
                        <div class="field-hint">⚠️以上四项为必填，行业选择将决定IQ题目，通道选择将影响OPQ/SHL/北森/EQ题目。</div>
                    </div>
                    <button class="btn" onclick="startQuiz()">开始测评 →</button>
                </div>
            </div>

            <!-- ====== 答题页 ====== -->
            <div id="quiz" class="section">
                <div class="quiz-header">
                    <span id="moduleBadge" class="module-badge">MBTI</span>
                    <span id="progressInfo" class="progress-info">第 1 题 / 共 120 题</span>
                </div>
                <div class="progress-bar"><div id="progressFill" class="progress-fill" style="width:1%"></div></div>
                <div class="question-box"><div id="questionText" class="question-text"></div></div>
                <div id="optionsContainer" class="options-container"></div>
                <div class="quiz-footer">
                    <button id="prevBtn" class="btn btn-secondary" onclick="prevQuestion()" disabled>← 上一题</button>
                    <button id="nextBtn" class="btn" onclick="nextQuestion()" disabled>下一题 →</button>
                </div>
            </div>

            <!-- ====== 结果页 ====== -->
            <div id="result" class="section">
                <div class="result-header">
                    <h2>🎯 综合能力评估报告</h2>
                    <div class="result-user-info">
                        <span id="resultName">匿名</span> |
                        <span id="resultDept">-</span> |
                        <span id="resultIndustry">-</span> |
                        <span id="resultChannel">-</span><br>
                        测评日期：<span id="resultDate"></span>
                    </div>
                </div>

                <!-- 第一行：作答质量分析（左）、核心优势（中）、雷达图（右） -->
                <div class="panel panel-warning">
                    <h3 style="color:#856404;">⚠️ 作答质量分析</h3>
                    <div id="validityWarning" class="analysis-box" style="border-left-color:#ffc107; background:#fffbf0;"></div>
                </div>
                <div class="panel">
                    <h3>📌 核心优势与风险摘要</h3>
                    <div id="recruitSummary" class="analysis-box"></div>
                </div>
                <div class="panel">
                    <h3>📊 八大模块雷达图</h3>
                    <div class="radar-container">
                        <canvas id="radarChart" width="300" height="300"></canvas>
                    </div>
                </div>

                <!-- 八大模块综合评分（跨三列） -->
                <div class="result-section full-width"><h3>📊 八大模块综合评分</h3><div id="overallScores"></div><div id="overallInterpretation" class="analysis-box" style="margin-top:4px;"></div></div>

                <!-- MBTI、PDP、DISC 三列等宽 -->
                <div class="panel"><h3>🎭 MBTI 性格类型</h3><div id="mbtiResult" class="analysis-box"></div></div>
                <div class="panel"><h3>🐾 PDP 天赋特质</h3><div id="pdpResult" class="analysis-box"></div></div>
                <div class="panel"><h3>📊 DISC 行为风格</h3><div id="discResult" class="analysis-box"></div></div>

                <!-- OPQ、SHL、IQ -->
                <div class="panel"><h3>💼 OPQ 管理潜质（通道定制）</h3><div id="opqResult" class="analysis-box"></div></div>
                <div class="panel"><h3>🧠 SHL 认知能力（通道定制·含高难度）</h3><div id="shlResult" class="analysis-box"></div></div>
                <div class="panel"><h3>🔍 IQ 智力测试（行业定制·场景化）</h3><div id="iqResult" class="analysis-box"></div></div>

                <!-- EQ、北森、行业适配度 -->
                <div class="panel"><h3>❤️ EQ 情商测试（通道定制）</h3><div id="eqResult" class="analysis-box"></div></div>
                <div class="panel"><h3>🌟 北森 素养与情商（通道定制）</h3><div id="beisenResult" class="analysis-box"></div></div>
                <div class="panel"><h3>📈 行业与通道适配度</h3><div id="industryFit" class="analysis-box"></div></div>

                <!-- 招聘用人建议与个人发展建议 -->
                <div class="panel"><h3>💡 招聘用人建议</h3><ul id="recruitSuggestions" class="suggestions-list"></ul></div>
                <div class="panel"><h3>📚 个人发展建议</h3><ul id="personalSuggestions" class="suggestions-list"></ul></div>
                <!-- 第三列留空，网格自动填充 -->

                <!-- ===== 操作按钮 ===== -->
                <div class="result-actions">
                    <button class="btn" onclick="downloadPDF(this)">📄 下载测评结果</button>
                    <button class="btn btn-success" onclick="sendEmailReport()">📧 一键发送邮件</button>
                    <button class="btn btn-wechat" onclick="copyWechatText()">💬 复制报告摘要</button>
                    <button class="btn btn-secondary" onclick="copyResult()">📋 复制简要结果</button>
                    <button class="btn btn-secondary" onclick="restartQuiz()">🔄 重新测评</button>
                </div>

                <!-- ===== 分享提示 ===== -->
                <div class="share-hint">
                    💡 <strong>微信转发</strong>：点击「复制报告摘要」后，打开微信加 <strong>davy0910</strong> ，发送即可。<br>
                    📧 <strong>邮件发送</strong>：点击「一键发送邮件」将自动打开默认邮件客户端，收件人已预设为 <strong>davy0910@qq.com</strong>，内容包含报告摘要。
                </div>

                <div class="footer-note">本报告由综合人才能力测评系统（通道定制版，120题）生成 | 设计人：DAVY（抖音搜：d190564680）</div>
            </div>
        </div>
    </div>

    <script>
        (function() {
            'use strict';

            // ================================================================
            // 1. MBTI (30题)
            // ================================================================
            function mbtiQ(text, dim, reverse) {
                var opts = ['非常不符合', '比较不符合', '比较符合', '非常符合'];
                var scores = [1, 2, 3, 4];
                if (reverse) scores = [4, 3, 2, 1];
                return { module: 'MBTI', dimension: dim, question: text, options: opts, scores: scores };
            }
            var mbtiItems = [
                mbtiQ('在社交场合中，您更倾向于主动结识新朋友', 'EI', false),
                mbtiQ('独处时您能获得更多能量', 'EI', true),
                mbtiQ('您更喜欢与少数人深入交流，而非广泛社交', 'EI', true),
                mbtiQ('您经常成为聚会中的活跃分子', 'EI', false),
                mbtiQ('您更关注外部世界的动态和变化', 'EI', false),
                mbtiQ('您喜欢安静地思考问题', 'EI', true),
                mbtiQ('您喜欢在团队中表达自己的观点', 'EI', false),
                mbtiQ('您倾向于独立工作，不喜被打扰', 'EI', true),
                mbtiQ('您更关注事物的细节和具体事实', 'SN', false),
                mbtiQ('您经常思考未来的可能性和趋势', 'SN', true),
                mbtiQ('您更喜欢理论性、抽象性的概念', 'SN', true),
                mbtiQ('您注重实际经验和可操作的方法', 'SN', false),
                mbtiQ('您倾向于相信直觉和灵感', 'SN', true),
                mbtiQ('您做事依赖已有的成熟方案', 'SN', false),
                mbtiQ('您喜欢探索新的观念和理论', 'SN', true),
                mbtiQ('您更关注眼前的任务而非长远愿景', 'SN', false),
                mbtiQ('在做决策时，您更依赖逻辑和分析', 'TF', false),
                mbtiQ('您很在意他人的感受和情绪', 'TF', true),
                mbtiQ('您认为公平比人情更重要', 'TF', false),
                mbtiQ('您常常设身处地为他人着想', 'TF', true),
                mbtiQ('您倾向于客观评判，而非主观感受', 'TF', false),
                mbtiQ('您容易被感性的故事打动', 'TF', true),
                mbtiQ('您善于理性分析问题', 'TF', false),
                mbtiQ('您重视团队和谐胜过绝对正确', 'TF', true),
                mbtiQ('您喜欢提前制定计划并严格执行', 'JP', false),
                mbtiQ('您更享受灵活应变，而非按部就班', 'JP', true),
                mbtiQ('您倾向于把事情做完了结', 'JP', false),
                mbtiQ('您喜欢保留多种可能性，不急于定论', 'JP', true),
                mbtiQ('您很注重时间管理和日程安排', 'JP', false),
                mbtiQ('您经常在最后时刻才做决定', 'JP', true)
            ];

            // ================================================================
            // 2. DISC (20题，各维度5题，含反向)
            // ================================================================
            var discData = [
                { text: '您喜欢掌控局面，指挥他人', dim: 'D', rev: false },
                { text: '您果断，不喜欢犹豫不决', dim: 'D', rev: false },
                { text: '您敢于挑战权威和传统', dim: 'D', rev: false },
                { text: '您倾向于服从他人安排，不主动争取主导权', dim: 'D', rev: true },
                { text: '您在做决定时经常犹豫不决', dim: 'D', rev: true },
                { text: '您善于表达，充满感染力', dim: 'I', rev: false },
                { text: '您喜欢与人交往，结交新朋友', dim: 'I', rev: false },
                { text: '您在团队中常是活跃气氛的人', dim: 'I', rev: false },
                { text: '您在社交场合中倾向于保持安静', dim: 'I', rev: true },
                { text: '您不喜欢成为众人瞩目的焦点', dim: 'I', rev: true },
                { text: '您性情温和，不易与人发生冲突', dim: 'S', rev: false },
                { text: '您喜欢稳定的工作环境', dim: 'S', rev: false },
                { text: '您善于倾听，是很好的听众', dim: 'S', rev: false },
                { text: '您容易因为小事而感到烦躁', dim: 'S', rev: true },
                { text: '您喜欢经常变化和新鲜刺激', dim: 'S', rev: true },
                { text: '您做事严谨，追求准确性', dim: 'C', rev: false },
                { text: '您喜欢按规则和流程办事', dim: 'C', rev: false },
                { text: '您注重数据和事实，避免主观臆断', dim: 'C', rev: false },
                { text: '您经常粗心大意，忽略细节', dim: 'C', rev: true },
                { text: '您觉得规则可以灵活变通，不必太较真', dim: 'C', rev: true }
            ];
            var discItems = discData.map(function(d) {
                var opts = ['非常不符合', '比较不符合', '比较符合', '非常符合'];
                var scores = [1, 2, 3, 4];
                if (d.rev) scores = scores.map(s => 5 - s);
                return { module: 'DISC', dimension: d.dim, question: d.text, options: opts, scores: scores };
            });

            // ================================================================
            // 3. PDP (10题)
            // ================================================================
            function pdpQ(text, dim) {
                var opts = ['非常不符合', '比较不符合', '比较符合', '非常符合'];
                var scores = [1, 2, 3, 4];
                return { module: 'PDP', dimension: dim, question: text, options: opts, scores: scores };
            }
            var pdpItems = [
                pdpQ('您做事果断，敢于冒险', '老虎'),
                pdpQ('您喜欢掌控局面，主导事情发展', '老虎'),
                pdpQ('您善于表达，热情洋溢', '孔雀'),
                pdpQ('您喜欢社交，很容易与人打成一片', '孔雀'),
                pdpQ('您性格温和，善于倾听他人', '考拉'),
                pdpQ('您做事稳健，不喜欢冲突', '考拉'),
                pdpQ('您做事严谨，注重细节和质量', '猫头鹰'),
                pdpQ('您喜欢分析和规划，追求精确', '猫头鹰'),
                pdpQ('您善于适应不同环境，灵活变通', '变色龙'),
                pdpQ('您能在不同角色间切换自如', '变色龙')
            ];

            // ================================================================
            // 4. OPQ (10题) 按通道
            // ================================================================
            var opqByChannel = {
                '管理': [
                    { dim: '领导力', text: '您能清晰阐述愿景，并激励团队追随', rev: false },
                    { dim: '领导力', text: '您经常在团队决策中感到犹豫不决', rev: true },
                    { dim: '决策力', text: '您在复杂情境中能快速做出决断', rev: false },
                    { dim: '决策力', text: '您常常因为信息不足而推迟决策', rev: true },
                    { dim: '沟通影响', text: '您善于倾听并整合不同意见', rev: false },
                    { dim: '沟通影响', text: '您很难说服他人接受您的观点', rev: true },
                    { dim: '执行效率', text: '您注重结果，能高效执行计划', rev: false },
                    { dim: '执行效率', text: '您经常因拖延导致任务延期', rev: true },
                    { dim: '创新思维', text: '您能识别问题的本质，提出创新解法', rev: false },
                    { dim: '创新思维', text: '您更倾向于沿用旧方法，不愿尝试新事物', rev: true }
                ],
                '技术': [
                    { dim: '专业引领', text: '您对技术前沿保持高度敏感', rev: false },
                    { dim: '专业引领', text: '您很少主动学习新技术', rev: true },
                    { dim: '技术创新', text: '您勇于尝试新技术并推动落地', rev: false },
                    { dim: '技术创新', text: '您对新技术的风险过于担忧', rev: true },
                    { dim: '技术协作', text: '您乐于分享技术经验，促进团队成长', rev: false },
                    { dim: '技术协作', text: '您习惯单打独斗，不善技术交流', rev: true },
                    { dim: '问题解决', text: '您能系统性地解决复杂技术难题', rev: false },
                    { dim: '问题解决', text: '遇到技术难题时您容易放弃', rev: true },
                    { dim: '学习创新', text: '您持续学习，不断更新知识储备', rev: false },
                    { dim: '学习创新', text: '您满足于现有技能，不追求提升', rev: true }
                ],
                '幕僚': [
                    { dim: '分析能力', text: '您善于从数据中提炼关键洞察', rev: false },
                    { dim: '分析能力', text: '您对数据分析感到吃力', rev: true },
                    { dim: '战略规划', text: '您能制定清晰的长期规划', rev: false },
                    { dim: '战略规划', text: '您更关注眼前事务，缺乏长远视野', rev: true },
                    { dim: '沟通协调', text: '您能有效协调跨部门利益', rev: false },
                    { dim: '沟通协调', text: '您在跨部门沟通中经常产生误解', rev: true },
                    { dim: '制度流程', text: '您善于优化流程，提升效率', rev: false },
                    { dim: '制度流程', text: '您对现有流程感到满意，不愿改变', rev: true },
                    { dim: '创新思维', text: '您能提出新颖的解决方案', rev: false },
                    { dim: '创新思维', text: '您的想法常常保守、缺乏新意', rev: true }
                ]
            };

            function buildOPQ(channel) {
                var list = opqByChannel[channel] || opqByChannel['管理'];
                return list.map(function(item) {
                    var opts = ['非常不符合', '比较不符合', '比较符合', '非常符合'];
                    var scores = [1, 2, 3, 4];
                    if (item.rev) scores = scores.map(s => 5 - s);
                    return { module: 'OPQ', dimension: item.dim, question: item.text, options: opts, scores: scores };
                });
            }

            // ================================================================
            // 5. SHL (10题) 按通道，含3道高难度
            // ================================================================
            var shlByChannel = {
                '管理': [
                    { dim: '逻辑推理', q: '部门绩效下降，以下哪项最可能是根本原因？', opts: ['A. 市场萎缩', 'B. 团队士气低落', 'C. 流程效率低',
                            'D. 资源不足'
                        ], correct: 1 },
                    { dim: '数字推理', q: '去年销售额800万，增长25%，今年目标是多少？', opts: ['A. 900万', 'B. 1000万', 'C. 1100万',
                            'D. 1200万'
                        ], correct: 1 },
                    { dim: '言语理解', q: '“运筹帷幄”最适合形容哪种能力？', opts: ['A. 执行力', 'B. 战略规划', 'C. 沟通协调', 'D. 创新思维'],
                        correct: 1 },
                    { dim: '言语理解', q: '以下哪句话没有语病？', opts: ['A. 经过努力，使业绩提升', 'B. 公司决定对违规者处罚', 'C. 这种策略的效果如何',
                            'D. 由于他负责，所以成功'
                        ], correct: 2 },
                    { dim: '思维策略', q: '有6个不同重量的球，用天平最少称几次能按重量排序？', opts: ['A. 4次', 'B. 5次', 'C. 6次', 'D. 7次'],
                        correct: 0 },
                    { dim: '思维策略', q: '甲乙两人相向而行，速度比3:2，相遇时甲比乙多走10km，两地距离？', opts: ['A. 40km', 'B. 50km', 'C. 60km',
                            'D. 70km'
                        ], correct: 1 },
                    { dim: '数字推理', q: '预算削减10%，原预算500万，现预算为？', opts: ['A. 400万', 'B. 450万', 'C. 480万', 'D. 500万'],
                        correct: 1 },
                    { dim: '逻辑推理', q: '某公司有A、B、C三个项目，总投资额500万。A项目投资额是B的1.5倍，C项目比B多50万，则A项目投资额为？',
                        opts: ['A. 150万', 'B. 180万', 'C. 200万', 'D. 225万'], correct: 3 },
                    { dim: '数字推理', q: '一组数据：4, 9, 20, 43, ? 问号处？', opts: ['A. 76', 'B. 80', 'C. 86', 'D. 90'],
                        correct: 2 },
                    { dim: '思维策略', q: '甲乙丙三人合作完成一项工程，甲单独做需12天，乙需15天，丙需20天。若三人先合作3天，剩余由乙丙合作，还需几天？',
                        opts: ['A. 2天', 'B. 3天', 'C. 4天', 'D. 5天'], correct: 1 }
                ],
                '技术': [
                    { dim: '逻辑推理', q: '系统出现故障，以下哪项排查顺序最合理？', opts: ['A. 硬件→软件→网络', 'B. 软件→硬件→网络', 'C. 网络→硬件→软件',
                            'D. 同时检查'
                        ], correct: 0 },
                    { dim: '数字推理', q: '代码行数从5000增加到6000，增长率为？', opts: ['A. 10%', 'B. 15%', 'C. 20%', 'D. 25%'],
                        correct: 2 },
                    { dim: '数字推理', q: '服务器平均响应时间从2ms降到1.5ms，性能提升多少？', opts: ['A. 20%', 'B. 25%', 'C. 30%',
                            'D. 33%'
                        ], correct: 1 },
                    { dim: '言语理解', q: '“精益求精”最符合哪种技术态度？', opts: ['A. 追求效率', 'B. 追求质量', 'C. 追求创新', 'D. 追求稳定'],
                        correct: 1 },
                    { dim: '言语理解', q: '以下哪项表述最准确？', opts: ['A. 该方案具有可扩展性', 'B. 该方案可扩展', 'C. 该方案扩展性好',
                            'D. 该方案能够扩展'
                        ], correct: 0 },
                    { dim: '思维策略', q: '有8个外观相同的芯片，其中1个有缺陷（偏重），用天平最少称几次找出？', opts: ['A. 2次', 'B. 3次', 'C. 4次',
                            'D. 5次'
                        ], correct: 0 },
                    { dim: '逻辑推理', q: '所有工程师都懂编程，部分程序员是工程师，以下哪项正确？', opts: ['A. 所有程序员都懂编程', 'B. 有些程序员不懂编程',
                            'C. 所有工程师都是程序员', 'D. 无法确定'
                        ], correct: 0 },
                    { dim: '逻辑推理', q: '若函数f(x)=x²-4x+3，则f(f(2))的值为？', opts: ['A. 0', 'B. 1', 'C. 2', 'D. 3'],
                        correct: 1 },
                    { dim: '数字推理', q: '数列：3, 11, 25, 51, ? 问号处？', opts: ['A. 101', 'B. 103', 'C. 107', 'D. 109'],
                        correct: 2 },
                    { dim: '思维策略', q: '一个多线程程序有4个并发任务，每个任务需分别占用CPU 2s, 3s, 5s, 7s，若采用单核串行执行，总时间？若采用双核（可并行），最短时间？',
                        opts: ['A. 17s, 10s', 'B. 17s, 9s', 'C. 17s, 8s', 'D. 17s, 7s'], correct: 2 }
                ],
                '幕僚': [
                    { dim: '逻辑推理', q: '市场调研显示，消费者满意度下降，以下哪项最可能解释？', opts: ['A. 产品质量下滑', 'B. 服务态度变差', 'C. 价格上调',
                            'D. 竞争对手促销'
                        ], correct: 1 },
                    { dim: '数字推理', q: '某公司今年营收1.2亿，同比增长20%，去年营收为？', opts: ['A. 1.0亿', 'B. 1.1亿', 'C. 1.2亿',
                            'D. 1.3亿'
                        ], correct: 0 },
                    { dim: '数字推理', q: '成本从80万降至64万，降幅为？', opts: ['A. 15%', 'B. 18%', 'C. 20%', 'D. 22%'],
                        correct: 2 },
                    { dim: '言语理解', q: '“高屋建瓴”最适合形容哪种思维方式？', opts: ['A. 全局观', 'B. 细节观', 'C. 创新观', 'D. 执行观'],
                        correct: 0 },
                    { dim: '言语理解', q: '以下哪项表述最严谨？', opts: ['A. 根据数据显示，趋势明显', 'B. 数据表明，趋势显著', 'C. 数据显示出明显趋势',
                            'D. 从数据看趋势明显'
                        ], correct: 0 },
                    { dim: '思维策略', q: '有10份文件需整理，甲需4小时，乙需6小时，两人合作需多久？', opts: ['A. 2小时', 'B. 2.4小时', 'C. 3小时',
                            'D. 3.6小时'
                        ], correct: 1 },
                    { dim: '逻辑推理', q: '所有顾问都善于分析，部分分析师是顾问，以下哪项正确？', opts: ['A. 所有分析师都善于分析', 'B. 有些分析师不善于分析',
                            'C. 所有顾问都是分析师', 'D. 无法确定'
                        ], correct: 0 },
                    { dim: '逻辑推理', q: '某企业有4个部门，财务部人数是人事部的2倍，人事部是市场部的1.5倍，市场部是技术部的1.2倍。若技术部有10人，则财务部有多少人？',
                        opts: ['A. 24人', 'B. 30人', 'C. 36人', 'D. 42人'], correct: 2 },
                    { dim: '数字推理', q: '数列：5, 16, 49, 148, ? 问号处？', opts: ['A. 445', 'B. 448', 'C. 456', 'D. 460'],
                        correct: 0 },
                    { dim: '思维策略', q: '一份报告需分析5个数据表，分析师甲单独需8小时，乙需10小时，丙需12小时。若三人同时开始，2小时后乙因故退出，剩余由甲丙完成，还需几小时？',
                        opts: ['A. 1.5小时', 'B. 2小时', 'C. 2.5小时', 'D. 3小时'], correct: 2 }
                ]
            };

            function buildSHL(channel) {
                var list = shlByChannel[channel] || shlByChannel['管理'];
                return list.map(function(item) {
                    var scores = item.opts.map(function(_, idx) { return idx === item.correct ? 4 : 0; });
                    return { module: 'SHL', dimension: item.dim, question: item.q, options: item.opts, scores: scores };
                });
            }

            // ================================================================
            // 6. 北森 (20题) 按通道
            // ================================================================
            var beisenByChannel = {
                '管理': [
                    { text: '您能清晰传达复杂决策', dim: '沟通能力', rev: false },
                    { text: '您善于倾听下属意见', dim: '沟通能力', rev: false },
                    { text: '您经常因表达不清导致误解', dim: '沟通能力', rev: true },
                    { text: '您很少耐心听取反对意见', dim: '沟通能力', rev: true },
                    { text: '您乐于分享管理经验', dim: '团队合作', rev: false },
                    { text: '您能主动帮助同事解决管理难题', dim: '团队合作', rev: false },
                    { text: '您习惯独断专行，不喜协作', dim: '团队合作', rev: true },
                    { text: '您难以接受团队中的不同意见', dim: '团队合作', rev: true },
                    { text: '您能系统分析问题根源', dim: '问题解决', rev: false },
                    { text: '您能提出创新性管理方案', dim: '问题解决', rev: false },
                    { text: '您容易在复杂问题前放弃', dim: '问题解决', rev: true },
                    { text: '您很少反思管理失误', dim: '问题解决', rev: true },
                    { text: '您对新管理理念有强烈好奇心', dim: '学习能力', rev: false },
                    { text: '您能快速掌握新管理工具', dim: '学习能力', rev: false },
                    { text: '您满足于现有管理知识', dim: '学习能力', rev: true },
                    { text: '您对新管理工具缺乏兴趣', dim: '学习能力', rev: true },
                    { text: '您对工作成果高度负责', dim: '责任心', rev: false },
                    { text: '您能按时保质完成管理任务', dim: '责任心', rev: false },
                    { text: '您经常推诿管理责任', dim: '责任心', rev: true },
                    { text: '您忽视管理细节', dim: '责任心', rev: true }
                ],
                '技术': [
                    { text: '您能清晰解释技术方案', dim: '沟通能力', rev: false },
                    { text: '您善于倾听技术问题反馈', dim: '沟通能力', rev: false },
                    { text: '您常因技术术语导致沟通障碍', dim: '沟通能力', rev: true },
                    { text: '您很少主动沟通技术进展', dim: '沟通能力', rev: true },
                    { text: '您乐于分享技术代码', dim: '团队合作', rev: false },
                    { text: '您能主动协助同事解决技术难题', dim: '团队合作', rev: false },
                    { text: '您习惯独自钻研，不喜协作', dim: '团队合作', rev: true },
                    { text: '您难以接受团队中的技术分歧', dim: '团队合作', rev: true },
                    { text: '您能系统调试技术问题', dim: '问题解决', rev: false },
                    { text: '您能提出创新性技术方案', dim: '问题解决', rev: false },
                    { text: '您容易在复杂Bug前放弃', dim: '问题解决', rev: true },
                    { text: '您很少复盘技术失误', dim: '问题解决', rev: true },
                    { text: '您对新编程语言有强烈好奇心', dim: '学习能力', rev: false },
                    { text: '您能快速掌握新框架', dim: '学习能力', rev: false },
                    { text: '您满足于现有技术栈', dim: '学习能力', rev: true },
                    { text: '您对新技术缺乏兴趣', dim: '学习能力', rev: true },
                    { text: '您对技术质量高度负责', dim: '责任心', rev: false },
                    { text: '您能按时交付高质量代码', dim: '责任心', rev: false },
                    { text: '您经常拖延技术任务', dim: '责任心', rev: true },
                    { text: '您忽视代码规范', dim: '责任心', rev: true }
                ],
                '幕僚': [
                    { text: '您能精准汇报分析结论', dim: '沟通能力', rev: false },
                    { text: '您善于倾听各方意见', dim: '沟通能力', rev: false },
                    { text: '您常因表达不清导致决策失误', dim: '沟通能力', rev: true },
                    { text: '您很少主动沟通分析进展', dim: '沟通能力', rev: true },
                    { text: '您乐于分享分析工具', dim: '团队合作', rev: false },
                    { text: '您能主动协助同事完成分析任务', dim: '团队合作', rev: false },
                    { text: '您习惯独自分析，不喜协作', dim: '团队合作', rev: true },
                    { text: '您难以接受不同的分析观点', dim: '团队合作', rev: true },
                    { text: '您能系统梳理复杂问题', dim: '问题解决', rev: false },
                    { text: '您能提出创新性分析框架', dim: '问题解决', rev: false },
                    { text: '您容易在数据缺失时放弃', dim: '问题解决', rev: true },
                    { text: '您很少复盘分析失误', dim: '问题解决', rev: true },
                    { text: '您对新分析工具有强烈好奇心', dim: '学习能力', rev: false },
                    { text: '您能快速掌握新分析方法', dim: '学习能力', rev: false },
                    { text: '您满足于现有分析技能', dim: '学习能力', rev: true },
                    { text: '您对新分析技术缺乏兴趣', dim: '学习能力', rev: true },
                    { text: '您对分析质量高度负责', dim: '责任心', rev: false },
                    { text: '您能按时交付高质量分析报告', dim: '责任心', rev: false },
                    { text: '您经常拖延分析工作', dim: '责任心', rev: true },
                    { text: '您忽视数据准确性', dim: '责任心', rev: true }
                ]
            };

            function buildBeisen(channel) {
                var list = beisenByChannel[channel] || beisenByChannel['管理'];
                return list.map(function(item) {
                    var opts = ['非常不符合', '比较不符合', '比较符合', '非常符合'];
                    var scores = [1, 2, 3, 4];
                    if (item.rev) scores = scores.map(s => 5 - s);
                    return { module: '北森', dimension: item.dim, question: item.text, options: opts, scores: scores };
                });
            }

            // ================================================================
            // 7. IQ (10题) 按行业，替换数列题为场景应用题
            // ================================================================
            var iqByIndustry = {
                '电商': [
                    { dim: '逻辑推理', q: '某电商平台月活跃用户环比增长8%，若上个月为500万，本月约为？', opts: ['A. 530万', 'B. 540万', 'C. 550万',
                            'D. 560万'
                        ], correct: 1 },
                    { dim: '逻辑推理', q: '若A商品转化率高于B，B高于C，则以下哪项正确？', opts: ['A. A高于C', 'B. C高于A', 'C. 无法比较',
                            'D. 三者相同'
                        ], correct: 0 },
                    { dim: '数字推理', q: '库存周转率从6次提升到8次，提升幅度为？', opts: ['A. 25%', 'B. 30%', 'C. 33%', 'D. 40%'],
                        correct: 2 },
                    { dim: '数字推理', q: '商品定价100元，折扣15%，折后价为？', opts: ['A. 80元', 'B. 85元', 'C. 90元', 'D. 95元'],
                        correct: 1 },
                    { dim: '言语理解', q: '“爆款”在电商中最可能指？', opts: ['A. 热销商品', 'B. 高利润商品', 'C. 新上架商品', 'D. 高退货率商品'],
                        correct: 0 },
                    { dim: '言语理解', q: '以下哪项表达最符合数据分析规范？', opts: ['A. 根据数据显示，趋势明显', 'B. 数据表明，趋势显著', 'C. 数据显示出明显趋势',
                            'D. 从数据看趋势明显'
                        ], correct: 2 },
                    { dim: '思维策略', q: '有6个快递包裹，重量不同，用天平最少称几次能按重量排序？', opts: ['A. 4次', 'B. 5次', 'C. 6次', 'D. 7次'],
                        correct: 0 },
                    { dim: '思维策略', q: '甲乙两人从仓库搬货，甲单独需2小时，乙需3小时，合作需多久？', opts: ['A. 1小时', 'B. 1.2小时', 'C. 1.5小时',
                            'D. 1.8小时'
                        ], correct: 1 },
                    { dim: '逻辑推理', q: '所有爆款商品都有高销量，部分高销量商品是新品，以下哪项正确？', opts: ['A. 所有新品都是爆款', 'B. 有些新品是爆款',
                            'C. 所有爆款都是新品', 'D. 无法确定'
                        ], correct: 1 },
                    { dim: '应用分析', q: '某电商促销活动，满200减30，满300减50。客户购买一件原价280元的商品，最优支付金额为？', opts: ['A. 230元',
                            'B. 250元', 'C. 260元', 'D. 280元'
                        ], correct: 1 }
                ],
                '制造业': [
                    { dim: '逻辑推理', q: '生产线故障，以下排查顺序最合理？', opts: ['A. 原料→设备→工艺', 'B. 设备→原料→工艺', 'C. 工艺→原料→设备',
                            'D. 同时检查'
                        ], correct: 0 },
                    { dim: '逻辑推理', q: '若A产品质量合格率高于B，B高于C，则以下哪项正确？', opts: ['A. A高于C', 'B. C高于A', 'C. 无法比较',
                            'D. 三者相同'
                        ], correct: 0 },
                    { dim: '数字推理', q: '产量从1000件提高到1200件，增长率为？', opts: ['A. 10%', 'B. 15%', 'C. 20%', 'D. 25%'],
                        correct: 2 },
                    { dim: '数字推理', q: '原材料成本降低8%，原成本100元，现成本为？', opts: ['A. 88元', 'B. 90元', 'C. 92元', 'D. 96元'],
                        correct: 2 },
                    { dim: '言语理解', q: '“精益求精”最符合哪种生产理念？', opts: ['A. 追求效率', 'B. 追求质量', 'C. 追求创新', 'D. 追求稳定'],
                        correct: 1 },
                    { dim: '言语理解', q: '以下哪项表述最准确？', opts: ['A. 该方案具有可操作性', 'B. 该方案可操作', 'C. 该方案操作性好',
                            'D. 该方案能够操作'
                        ], correct: 0 },
                    { dim: '思维策略', q: '有8个零件，其中1个有缺陷（偏重），用天平最少称几次找出？', opts: ['A. 2次', 'B. 3次', 'C. 4次', 'D. 5次'],
                        correct: 0 },
                    { dim: '思维策略', q: '甲乙两人合作生产，甲效率是乙的1.5倍，若甲单独完成需12天，乙需？', opts: ['A. 15天', 'B. 16天', 'C. 18天',
                            'D. 20天'
                        ], correct: 2 },
                    { dim: '逻辑推理', q: '所有合格产品都通过检测，部分通过检测的是次品，以下哪项正确？', opts: ['A. 所有次品都合格', 'B. 有些次品合格',
                            'C. 所有合格产品都是次品', 'D. 无法确定'
                        ], correct: 1 },
                    { dim: '应用分析', q: '某工厂生产A、B两种零件，A零件每件耗材5kg，B零件每件耗材8kg，现有原料200kg，若生产A零件20件，则最多还能生产B零件多少件？',
                        opts: ['A. 10件', 'B. 12件', 'C. 14件', 'D. 16件'], correct: 1 }
                ],
                '金融': [
                    { dim: '逻辑推理', q: '投资回报率从10%升至15%，提升幅度为？', opts: ['A. 50%', 'B. 40%', 'C. 30%', 'D. 20%'],
                        correct: 0 },
                    { dim: '逻辑推理', q: '若A股票收益率高于B，B高于C，则以下哪项正确？', opts: ['A. A高于C', 'B. C高于A', 'C. 无法比较',
                            'D. 三者相同'
                        ], correct: 0 },
                    { dim: '数字推理', q: '贷款总额2000万，年利率5%，一年利息为？', opts: ['A. 50万', 'B. 80万', 'C. 100万', 'D. 120万'],
                        correct: 2 },
                    { dim: '数字推理', q: '资产从500万增至600万，增长率为？', opts: ['A. 10%', 'B. 15%', 'C. 20%', 'D. 25%'],
                        correct: 2 },
                    { dim: '言语理解', q: '“分散投资”最符合哪种策略？', opts: ['A. 降低风险', 'B. 提高收益', 'C. 增加杠杆', 'D. 短期套利'],
                        correct: 0 },
                    { dim: '言语理解', q: '以下哪项表述最严谨？', opts: ['A. 根据数据显示，风险可控', 'B. 数据表明，风险可控', 'C. 数据显示风险可控',
                            'D. 从数据看风险可控'
                        ], correct: 2 },
                    { dim: '思维策略', q: '有5种理财产品，收益不同，用比较法最少几次能按收益排序？', opts: ['A. 4次', 'B. 5次', 'C. 6次', 'D. 7次'],
                        correct: 0 },
                    { dim: '思维策略', q: '甲乙两人投资，甲本金10万，年收益8%，乙本金15万，年收益6%，一年后谁收益高？', opts: ['A. 甲', 'B. 乙',
                            'C. 一样', 'D. 无法比较'
                        ], correct: 1 },
                    { dim: '逻辑推理', q: '所有高收益产品都有高风险，部分高风险产品是保本的，以下哪项正确？', opts: ['A. 所有保本产品都高收益', 'B. 有些保本产品高收益',
                            'C. 所有高收益产品都保本', 'D. 无法确定'
                        ], correct: 1 },
                    { dim: '应用分析', q: '某基金净值从1.2元涨至1.44元，涨幅为？若该基金每年分红0.1元，则年化收益率（含分红）约为？', opts: ['A. 20%, 8.3%',
                            'B. 20%, 10%', 'C. 20%, 11.7%', 'D. 20%, 12.5%'
                        ], correct: 2 }
                ],
                '工贸': [
                    { dim: '逻辑推理', q: '供应链延误，以下哪项最可能是根本原因？', opts: ['A. 交通拥堵', 'B. 库存不足', 'C. 订单错误', 'D. 供应商产能不足'],
                        correct: 3 },
                    { dim: '逻辑推理', q: '若A供应商价格低于B，B低于C，则以下哪项正确？', opts: ['A. A低于C', 'B. C低于A', 'C. 无法比较',
                            'D. 三者相同'
                        ], correct: 0 },
                    { dim: '数字推理', q: '出口额从800万增至960万，增长率为？', opts: ['A. 10%', 'B. 15%', 'C. 20%', 'D. 25%'],
                        correct: 2 },
                    { dim: '数字推理', q: '关税提高5%，原成本400万，新成本为？', opts: ['A. 410万', 'B. 420万', 'C. 430万', 'D. 440万'],
                        correct: 1 },
                    { dim: '言语理解', q: '“供应链韧性”最可能指？', opts: ['A. 抗风险能力', 'B. 成本控制', 'C. 交货速度', 'D. 产品质量'],
                        correct: 0 },
                    { dim: '言语理解', q: '以下哪项表述最清晰？', opts: ['A. 该方案具备可行性', 'B. 该方案可行', 'C. 该方案可行性高', 'D. 该方案能够可行'],
                        correct: 0 },
                    { dim: '思维策略', q: '有7个货柜，重量不同，用天平最少称几次能按重量排序？', opts: ['A. 5次', 'B. 6次', 'C. 7次', 'D. 8次'],
                        correct: 0 },
                    { dim: '思维策略', q: '甲乙两车同时出发，甲速度60km/h，乙速度80km/h，1小时后相距？', opts: ['A. 20km', 'B. 40km', 'C. 60km',
                            'D. 80km'
                        ], correct: 0 },
                    { dim: '逻辑推理', q: '所有合规货物都通过关检，部分通过关检的是危险品，以下哪项正确？', opts: ['A. 所有危险品都合规', 'B. 有些危险品合规',
                            'C. 所有合规货物都是危险品', 'D. 无法确定'
                        ], correct: 1 },
                    { dim: '应用分析', q: '某出口企业汇率为6.5时出口一批货物收入100万美元，若汇率变为6.8，则人民币收入增加多少万元？', opts: ['A. 20万',
                            'B. 25万', 'C. 30万', 'D. 35万'
                        ], correct: 2 }
                ],
                '互联网': [
                    { dim: '逻辑推理', q: '日活用户从100万增至120万，增长率为？', opts: ['A. 10%', 'B. 15%', 'C. 20%', 'D. 25%'],
                        correct: 2 },
                    { dim: '逻辑推理', q: '若A产品用户留存率高于B，B高于C，则以下哪项正确？', opts: ['A. A高于C', 'B. C高于A', 'C. 无法比较',
                            'D. 三者相同'
                        ], correct: 0 },
                    { dim: '数字推理', q: '服务器并发数从500提升到650，提升幅度？', opts: ['A. 20%', 'B. 25%', 'C. 30%', 'D. 35%'],
                        correct: 2 },
                    { dim: '数字推理', q: 'App下载量从80万增至96万，增长率为？', opts: ['A. 10%', 'B. 15%', 'C. 20%', 'D. 25%'],
                        correct: 2 },
                    { dim: '言语理解', q: '“用户体验”最可能指？', opts: ['A. 界面友好', 'B. 加载速度', 'C. 功能丰富', 'D. 整体使用感受'],
                        correct: 3 },
                    { dim: '言语理解', q: '以下哪项表述最专业？', opts: ['A. 该算法具有可扩展性', 'B. 该算法可扩展', 'C. 该算法扩展性好',
                            'D. 该算法能够扩展'
                        ], correct: 0 },
                    { dim: '思维策略', q: '有6个服务器节点，响应时间不同，最少比较几次能按响应时间排序？', opts: ['A. 4次', 'B. 5次', 'C. 6次', 'D. 7次'],
                        correct: 0 },
                    { dim: '思维策略', q: '甲乙两人编程，甲每天写200行，乙每天写150行，合作5天总行数？', opts: ['A. 1500', 'B. 1600', 'C. 1700',
                            'D. 1800'
                        ], correct: 2 },
                    { dim: '逻辑推理', q: '所有高并发系统都需要缓存，部分需要缓存的系统是分布式的，以下哪项正确？', opts: ['A. 所有分布式系统都是高并发',
                            'B. 有些分布式系统是高并发', 'C. 所有高并发系统都是分布式', 'D. 无法确定'
                        ], correct: 1 },
                    { dim: '应用分析', q: '某App新增用户中，通过自然流量获客成本为5元/人，广告投放成本为15元/人，若自然流量转化率为10%，广告转化率为20%，总获客成本为10元/人，则广告投放占比为？',
                        opts: ['A. 30%', 'B. 40%', 'C. 50%', 'D. 60%'], correct: 2 }
                ],
                '其他': [
                    { dim: '逻辑推理', q: '项目延期，以下哪项最可能是根本原因？', opts: ['A. 需求变更', 'B. 资源不足', 'C. 沟通不畅', 'D. 技术难题'],
                        correct: 0 },
                    { dim: '逻辑推理', q: '若A方案成本低于B，B低于C，则以下哪项正确？', opts: ['A. A低于C', 'B. C低于A', 'C. 无法比较',
                            'D. 三者相同'
                        ], correct: 0 },
                    { dim: '数字推理', q: '营收从1200万增至1440万，增长率为？', opts: ['A. 10%', 'B. 15%', 'C. 20%', 'D. 25%'],
                        correct: 2 },
                    { dim: '数字推理', q: '成本从500万降至450万，降幅为？', opts: ['A. 8%', 'B. 10%', 'C. 12%', 'D. 15%'],
                        correct: 1 },
                    { dim: '言语理解', q: '“协同效应”最可能指？', opts: ['A. 合作产生额外收益', 'B. 成本降低', 'C. 效率提升', 'D. 创新加速'],
                        correct: 0 },
                    { dim: '言语理解', q: '以下哪项表述最准确？', opts: ['A. 根据调研，需求明确', 'B. 调研表明，需求明确', 'C. 调研显示需求明确',
                            'D. 从调研看需求明确'
                        ], correct: 2 },
                    { dim: '思维策略', q: '有5项任务，耗时不同，最少比较几次能按耗时排序？', opts: ['A. 4次', 'B. 5次', 'C. 6次', 'D. 7次'],
                        correct: 0 },
                    { dim: '思维策略', q: '甲乙两人合作，甲单独需10天，乙需15天，合作需？', opts: ['A. 5天', 'B. 6天', 'C. 7天', 'D. 8天'],
                        correct: 1 },
                    { dim: '逻辑推理', q: '所有优秀员工都得到认可，部分得到认可的是新人，以下哪项正确？', opts: ['A. 所有新人都是优秀员工', 'B. 有些新人是优秀员工',
                            'C. 所有优秀员工都是新人', 'D. 无法确定'
                        ], correct: 1 },
                    { dim: '应用分析', q: '某公司计划采购一批设备，预算100万，若每台设备价格2.5万，则最多可购买多少台？若需预留10%的预算作为备用金，则实际可购买台数为？',
                        opts: ['A. 40台, 36台', 'B. 40台, 35台', 'C. 40台, 34台', 'D. 40台, 33台'], correct: 0 }
                ]
            };

            function buildIQ(industry) {
                var list = iqByIndustry[industry] || iqByIndustry['其他'];
                return list.map(function(item) {
                    var scores = item.opts.map(function(_, idx) { return idx === item.correct ? 4 : 0; });
                    return { module: 'IQ', dimension: item.dim, question: item.q, options: item.opts, scores: scores };
                });
            }

            // ================================================================
            // 8. EQ (10题) 按通道，各维度2题
            // ================================================================
            var eqByChannel = {
                '管理': [
                    { dim: '自我认知', text: '您能清晰认识自己的管理风格', rev: false },
                    { dim: '自我认知', text: '您经常反思自己的管理行为', rev: false },
                    { dim: '情绪管理', text: '您能在压力下保持冷静', rev: false },
                    { dim: '情绪管理', text: '您容易因下属表现而情绪波动', rev: true },
                    { dim: '共情能力', text: '您能设身处地理解下属感受', rev: false },
                    { dim: '共情能力', text: '您对他人的情绪变化反应迟钝', rev: true },
                    { dim: '社交技能', text: '您善于建立良好的人际关系', rev: false },
                    { dim: '社交技能', text: '您在团队中经常引发冲突', rev: true },
                    { dim: '团队领导', text: '您能有效激发团队士气', rev: false },
                    { dim: '团队领导', text: '您常因管理方式不当导致员工流失', rev: true }
                ],
                '技术': [
                    { dim: '自我认知', text: '您能客观评估自己的技术能力', rev: false },
                    { dim: '自我认知', text: '您很少反思技术决策', rev: true },
                    { dim: '情绪管理', text: '您能在技术难题前保持耐心', rev: false },
                    { dim: '情绪管理', text: '您容易因技术问题而焦虑', rev: true },
                    { dim: '共情能力', text: '您能理解非技术同事的困难', rev: false },
                    { dim: '共情能力', text: '您对用户的需求缺乏敏感度', rev: true },
                    { dim: '社交技能', text: '您乐于分享技术知识', rev: false },
                    { dim: '社交技能', text: '您不善与产品经理沟通', rev: true },
                    { dim: '团队合作', text: '您能积极参与技术攻关', rev: false },
                    { dim: '团队合作', text: '您习惯于独立工作，不喜协作', rev: true }
                ],
                '幕僚': [
                    { dim: '自我认知', text: '您能准确评估自己的分析能力', rev: false },
                    { dim: '自我认知', text: '您很少反思分析过程', rev: true },
                    { dim: '情绪管理', text: '您能在数据压力下保持条理', rev: false },
                    { dim: '情绪管理', text: '您容易因数据异常而慌乱', rev: true },
                    { dim: '共情能力', text: '您能理解业务部门的焦虑', rev: false },
                    { dim: '共情能力', text: '您对非量化因素缺乏关注', rev: true },
                    { dim: '社交技能', text: '您善于向管理层汇报分析结论', rev: false },
                    { dim: '社交技能', text: '您常因表达不清导致误解', rev: true },
                    { dim: '职业素养', text: '您能坚守分析职业道德', rev: false },
                    { dim: '职业素养', text: '您有时会为了迎合而修改数据', rev: true }
                ]
            };

            function buildEQ(channel) {
                var list = eqByChannel[channel] || eqByChannel['管理'];
                return list.map(function(item) {
                    var opts = ['非常不符合', '比较不符合', '比较符合', '非常符合'];
                    var scores = [1, 2, 3, 4];
                    if (item.rev) scores = scores.map(s => 5 - s);
                    return { module: 'EQ', dimension: item.dim, question: item.text, options: opts, scores: scores };
                });
            }

            // ================================================================
            // 9. 工具函数：打乱选项顺序（同步打乱options和scores）
            // ================================================================
            function shuffleOptions(question) {
                var combined = question.options.map(function(opt, idx) {
                    return { option: opt, score: question.scores[idx] };
                });
                for (var i = combined.length - 1; i > 0; i--) {
                    var j = Math.floor(Math.random() * (i + 1));
                    var temp = combined[i];
                    combined[i] = combined[j];
                    combined[j] = temp;
                }
                question.options = combined.map(function(item) { return item.option; });
                question.scores = combined.map(function(item) { return item.score; });
                return question;
            }

            // ================================================================
            // 10. 组装题库 (共120题)
            // ================================================================
            function buildQuestions(industry, channel) {
                var opq = buildOPQ(channel);
                var shl = buildSHL(channel);
                var beisen = buildBeisen(channel);
                var iq = buildIQ(industry);
                var eq = buildEQ(channel);
                var all = [].concat(mbtiItems, discItems, pdpItems, opq, shl, beisen, iq, eq);
                all = all.map(function(q) { return shuffleOptions(q); });
                return all.sort(function() { return Math.random() - 0.5; });
            }

            // ================================================================
            // 11. 状态与核心逻辑
            // ================================================================
            var currentQuestion = 0;
            var answers = [];
            var userInfo = {};
            var questions = [];

            function showSection(id) {
                document.querySelectorAll('.section').forEach(function(s) { s.style.display = 'none';
                    s.classList.remove('active'); });
                var el = document.getElementById(id);
                el.style.display = (id === 'result') ? 'grid' : 'block';
                el.classList.add('active');
            }
            document.addEventListener('DOMContentLoaded', function() { showSection('welcome'); });

            window.startQuiz = function() {
                var name = document.getElementById('userName').value.trim();
                var dept = document.getElementById('userDept').value.trim();
                var industry = document.getElementById('industry').value;
                var channel = document.getElementById('channel').value;
                if (!name || !dept || !industry || !channel) { alert('⚠️ 请完整填写所有必填信息！'); return; }
                userInfo = { name, dept, industry, channel, date: new Date().toLocaleDateString('zh-CN') };
                questions = buildQuestions(industry, channel);
                if (questions.length !== 120) {
                    console.warn('实际题数 ' + questions.length + '，期望 120');
                }
                currentQuestion = 0;
                answers = new Array(questions.length).fill(null);
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

                var html = q.options.map(function(opt, i) {
                    return '<div class="option-item ' + (answers[currentQuestion] === i ? 'selected' : '') +
                        '" onclick="window.selectOption(' + i + ')"><div class="option-label">' + String.fromCharCode(
                        65 + i) + '</div><div class="option-content">' + opt + '</div></div>';
                }).join('');
                document.getElementById('optionsContainer').innerHTML = html;
                document.getElementById('prevBtn').disabled = (currentQuestion === 0);
                document.getElementById('nextBtn').disabled = (answers[currentQuestion] === null);
                document.getElementById('nextBtn').textContent = (currentQuestion === questions.length - 1) ? '查看结果 →' :
                    '下一题 →';
            }

            window.selectOption = function(idx) { answers[currentQuestion] = idx;
                renderQuestion(); };
            window.prevQuestion = function() { if (currentQuestion > 0) { currentQuestion--;
                    renderQuestion(); } };
            window.nextQuestion = function() {
                if (answers[currentQuestion] === null) return;
                if (currentQuestion < questions.length - 1) { currentQuestion++;
                    renderQuestion(); } else showResult();
            };

            // ================================================================
            // 12. 计分引擎（含偏差检测）
            // ================================================================
            function calculateScores() {
                var modules = ['MBTI', 'DISC', 'PDP', 'OPQ', 'SHL', '北森', 'IQ', 'EQ'];
                var result = { modules: {}, dimensions: {}, extremeCount: 0, totalQuestions: questions.length };
                modules.forEach(function(m) { result.modules[m] = { total: 0, count: 0 }; });

                questions.forEach(function(q, i) {
                    var score = q.scores[answers[i]];
                    if (answers[i] === 0 || answers[i] === 3) result.extremeCount++;
                    var module = q.module;
                    var dim = q.dimension;
                    if (!result.dimensions[module]) result.dimensions[module] = {};
                    if (!result.dimensions[module][dim]) result.dimensions[module][dim] = { total: 0, count: 0 };
                    result.dimensions[module][dim].total += score;
                    result.dimensions[module][dim].count++;
                    if (result.modules[module]) {
                        result.modules[module].total += score;
                        result.modules[module].count++;
                    }
                });

                var moduleScores = {};
                Object.keys(result.modules).forEach(function(m) {
                    var data = result.modules[m];
                    if (data.count > 0) {
                        var avg = data.total / data.count;
                        moduleScores[m] = Math.round((avg / 4) * 100);
                    } else moduleScores[m] = 0;
                });

                var dimScores = {};
                Object.keys(result.dimensions).forEach(function(mod) {
                    dimScores[mod] = {};
                    Object.keys(result.dimensions[mod]).forEach(function(dim) {
                        var data = result.dimensions[mod][dim];
                        var avg = data.total / data.count;
                        dimScores[mod][dim] = Math.round(avg * 25);
                    });
                });

                var extremeRate = Math.round((result.extremeCount / result.totalQuestions) * 100);
                return { moduleScores, dimScores, extremeRate };
            }

            function getLevel(score) {
                if (score >= 80) return { text: '优秀', cls: 'tag-excellent' };
                if (score >= 60) return { text: '良好', cls: 'tag-good' };
                return { text: '待提升', cls: 'tag-need' };
            }

            function getMBTIType(ds) {
                var EI = ds['MBTI']['EI'] || 50,
                    SN = ds['MBTI']['SN'] || 50,
                    TF = ds['MBTI']['TF'] || 50,
                    JP = ds['MBTI']['JP'] || 50;
                return (EI >= 50 ? 'E' : 'I') + (SN >= 50 ? 'S' : 'N') + (TF >= 50 ? 'T' : 'F') + (JP >= 50 ? 'J' : 'P');
            }

            function getDISCType(ds) {
                var d = ds['DISC']['D'] || 0,
                    i = ds['DISC']['I'] || 0,
                    s = ds['DISC']['S'] || 0,
                    c = ds['DISC']['C'] || 0;
                var max = Math.max(d, i, s, c);
                if (max === d) return 'D';
                if (max === i) return 'I';
                if (max === s) return 'S';
                return 'C';
            }

            function getPDPType(ds) {
                var animals = ['老虎', '孔雀', '考拉', '猫头鹰', '变色龙'];
                var max = 0,
                    best = '考拉';
                animals.forEach(function(a) { var sc = ds['PDP'][a] || 0; if (sc > max) { max = sc;
                        best = a; } });
                return best;
            }

            // ================================================================
            // 13. 绘制雷达图
            // ================================================================
            function drawRadarChart(canvas, scores) {
                var ctx = canvas.getContext('2d');
                var width = canvas.width;
                var height = canvas.height;
                var centerX = width / 2;
                var centerY = height / 2;
                var radius = Math.min(width, height) * 0.38;

                var labels = ['MBTI', 'DISC', 'PDP', 'OPQ', 'SHL', '北森', 'IQ', 'EQ'];
                var values = labels.map(function(label) { return scores[label] || 0; });
                var numAxes = labels.length;
                var angleStep = (Math.PI * 2) / numAxes;

                // 清空画布
                ctx.clearRect(0, 0, width, height);

                // 绘制背景网格（三层）
                for (var ring = 1; ring <= 3; ring++) {
                    var r = radius * (ring / 3);
                    ctx.beginPath();
                    for (var i = 0; i <= numAxes; i++) {
                        var angle = i * angleStep - Math.PI / 2;
                        var x = centerX + r * Math.cos(angle);
                        var y = centerY + r * Math.sin(angle);
                        if (i === 0) ctx.moveTo(x, y);
                        else ctx.lineTo(x, y);
                    }
                    ctx.closePath();
                    ctx.strokeStyle = '#ddd';
                    ctx.lineWidth = 0.8;
                    ctx.stroke();
                }

                // 绘制轴线
                for (var i = 0; i < numAxes; i++) {
                    var angle = i * angleStep - Math.PI / 2;
                    var x = centerX + radius * Math.cos(angle);
                    var y = centerY + radius * Math.sin(angle);
                    ctx.beginPath();
                    ctx.moveTo(centerX, centerY);
                    ctx.lineTo(x, y);
                    ctx.strokeStyle = '#ccc';
                    ctx.lineWidth = 0.8;
                    ctx.stroke();

                    // 标签
                    var labelRadius = radius * 1.08;
                    var lx = centerX + labelRadius * Math.cos(angle);
                    var ly = centerY + labelRadius * Math.sin(angle);
                    ctx.fillStyle = '#333';
                    ctx.font = '10px sans-serif';
                    ctx.textAlign = 'center';
                    ctx.textBaseline = 'middle';
                    ctx.fillText(labels[i], lx, ly);
                }

                // 绘制数据区域
                ctx.beginPath();
                for (var i = 0; i <= numAxes; i++) {
                    var idx = i % numAxes;
                    var value = Math.min(values[idx], 100);
                    var r = (value / 100) * radius;
                    var angle = i * angleStep - Math.PI / 2;
                    var x = centerX + r * Math.cos(angle);
                    var y = centerY + r * Math.sin(angle);
                    if (i === 0) ctx.moveTo(x, y);
                    else ctx.lineTo(x, y);
                }
                ctx.closePath();
                ctx.fillStyle = 'rgba(102, 126, 234, 0.25)';
                ctx.fill();
                ctx.strokeStyle = '#667eea';
                ctx.lineWidth = 2;
                ctx.stroke();

                // 绘制数据点
                for (var i = 0; i < numAxes; i++) {
                    var value = Math.min(values[i], 100);
                    var r = (value / 100) * radius;
                    var angle = i * angleStep - Math.PI / 2;
                    var x = centerX + r * Math.cos(angle);
                    var y = centerY + r * Math.sin(angle);
                    ctx.beginPath();
                    ctx.arc(x, y, 3, 0, 2 * Math.PI);
                    ctx.fillStyle = '#667eea';
                    ctx.fill();
                }
            }

            // ================================================================
            // 14. 显示结果
            // ================================================================
            function showResult() {
                var scores = calculateScores();
                var ms = scores.moduleScores,
                    ds = scores.dimScores;
                var extRate = scores.extremeRate;

                document.getElementById('resultName').textContent = userInfo.name;
                document.getElementById('resultDept').textContent = userInfo.dept;
                document.getElementById('resultIndustry').textContent = userInfo.industry;
                document.getElementById('resultChannel').textContent = userInfo.channel;
                document.getElementById('resultDate').textContent = userInfo.date;

                // ---- 作答质量分析（左） ----
                var warnHtml = '<p><strong>极端响应率：</strong>' + extRate + '%（勾选“非常符合”或“非常不符合”的比例）</p>';
                if (extRate > 60) {
                    warnHtml +=
                        '<p style="color:#dc3545;"><strong>⚠️ 社会赞许性倾向明显：</strong>建议HR在面试中交叉验证，重点考察真实场景行为。</p>';
                } else if (extRate > 40) {
                    warnHtml +=
                    '<p style="color:#856404;"><strong>📌 存在一定偏差风险：</strong>建议结合面试进一步核实。</p>';
                } else {
                    warnHtml += '<p style="color:#155724;"><strong>✅ 作答质量良好：</strong>未出现明显的社会赞许性或随意作答倾向。</p>';
                }
                document.getElementById('validityWarning').innerHTML = warnHtml;

                // ---- 核心优势与风险摘要（中） ----
                var summary = '<div class="recruit-highlight"><strong>🔍 核心特质：</strong>' + getMBTIType(ds) + ' | ' +
                    (function() { var dt = getDISCType(ds); var map = { 'D': '支配型', 'I': '影响型', 'S': '稳健型',
                            'C': '谨慎型' }; return dt + '（' + (map[dt] || '') + '）'; })() + ' | PDP-' + getPDPType(
                        ds) + '</div>';
                summary += '<div class="recruit-highlight"><strong>💼 管理潜力：</strong>' + (ms['OPQ'] >= 70 ? '较强' :
                    '一般') + '，认知能力' + (ms['SHL'] >= 70 ? '优秀' : '良好') + '。</div>';
                summary += '<div class="recruit-highlight"><strong>📈 综合评价：</strong>' + (extRate > 60 ?
                    '存在社会赞许性倾向，建议谨慎解读分数，重点参考面试表现。' :
                    '作答真实度较高，报告具有参考价值。') + '</div>';
                document.getElementById('recruitSummary').innerHTML = summary;

                // ---- 绘制雷达图（右） ----
                var canvas = document.getElementById('radarChart');
                if (canvas) {
                    drawRadarChart(canvas, ms);
                }

                // ---- 八大模块综合评分（跨三列） ----
                var overallHtml = '';
                var modNames = ['MBTI', 'DISC', 'PDP', 'OPQ', 'SHL', '北森', 'IQ', 'EQ'];
                modNames.forEach(function(m) {
                    var sc = ms[m] || 0;
                    var level = getLevel(sc);
                    overallHtml += '<div class="score-row"><div class="score-name">' + m + '</div><div class="score-bar"><div class="score-fill" style="width:' +
                        sc + '%"></div></div><div class="score-value">' + sc + '</div><span class="score-tag ' + level
                        .cls + '">' + level.text + '</span></div>';
                });
                document.getElementById('overallScores').innerHTML = overallHtml;
                document.getElementById('overallInterpretation').innerHTML = '<p>注意：OPQ/SHL/北森为通道定制，IQ为行业定制，EQ为通道定制；DISC/北森/EQ含反向计分，SHL/IQ严格计分。</p>';

                // ---- MBTI ----
                var mbtiType = getMBTIType(ds);
                var typeMap = { 'ISTJ': '检查者型', 'ISFJ': '保护者型', 'INFJ': '倡导者型', 'INTJ': '战略家型', 'ISTP': '工匠型',
                    'ISFP': '艺术家型', 'INFP': '治愈者型', 'INTP': '思想家型', 'ESTP': '实践者型', 'ESFP': '表演者型',
                    'ENFP': '激励者型', 'ENTP': '发明家型', 'ESTJ': '监督者型', 'ESFJ': '供给者型', 'ENFJ': '教育家型',
                    'ENTJ': '统帅型'
                };
                var mbtiHtml = '<p><strong>类型：' + mbtiType + '（' + (typeMap[mbtiType] || '') + '）</strong></p>';
                mbtiHtml += '<p>E/I=' + (ds['MBTI']['EI'] || 50) + '，S/N=' + (ds['MBTI']['SN'] || 50) + '，T/F=' + (ds[
                    'MBTI']['TF'] || 50) + '，J/P=' + (ds['MBTI']['JP'] || 50) + '</p>';
                document.getElementById('mbtiResult').innerHTML = mbtiHtml;

                // ---- PDP ----
                var pdpType = getPDPType(ds);
                var pdpMap = { '老虎': '支配型', '孔雀': '外向型', '考拉': '耐心型', '猫头鹰': '精确型', '变色龙': '整合型' };
                var pdpHtml = '<p><strong>优势动物：' + pdpType + '（' + pdpMap[pdpType] + '）</strong></p>';
                var animalList = ['老虎', '孔雀', '考拉', '猫头鹰', '变色龙'];
                var pdpScores = animalList.map(function(a) { return a + '=' + (ds['PDP'][a] || 0); }).join('，');
                pdpHtml += '<p>' + pdpScores + '</p>';
                document.getElementById('pdpResult').innerHTML = pdpHtml;

                // ---- DISC ----
                var discType = getDISCType(ds);
                var discMap = { 'D': '支配型', 'I': '影响型', 'S': '稳健型', 'C': '谨慎型' };
                var discHtml = '<p><strong>主导风格：' + discType + '（' + discMap[discType] + '）</strong></p>';
                discHtml += '<p>D=' + (ds['DISC']['D'] || 0) + '，I=' + (ds['DISC']['I'] || 0) + '，S=' + (ds['DISC'][
                    'S'] || 0) + '，C=' + (ds['DISC']['C'] || 0) + '</p>';
                discHtml += '<p style="font-size:10px;color:#888;">* 计分已包含反向矫正，得分反映真实倾向。</p>';
                document.getElementById('discResult').innerHTML = discHtml;

                // ---- OPQ ----
                var opqDims = Object.keys(ds['OPQ'] || {});
                var opqHtml = '<p><strong>管理潜质维度（通道定制，含反向矫正）：</strong></p>';
                opqDims.forEach(function(d) {
                    var sc = ds['OPQ'][d] || 0;
                    var level = getLevel(sc);
                    opqHtml += '<div class="score-row"><div class="score-name">' + d + '</div><div class="score-bar"><div class="score-fill" style="width:' +
                        sc + '%"></div></div><div class="score-value">' + sc + '</div><span class="score-tag ' + level
                        .cls + '">' + level.text + '</span></div>';
                });
                document.getElementById('opqResult').innerHTML = opqHtml;

                // ---- SHL ----
                var shlDims = Object.keys(ds['SHL'] || {});
                var shlHtml = '<p><strong>认知能力（通道定制，含高难度题，严格计分，正确=4分，错误=0分）：</strong></p>';
                shlDims.forEach(function(d) {
                    var sc = ds['SHL'][d] || 0;
                    var level = getLevel(sc);
                    shlHtml += '<div class="score-row"><div class="score-name">' + d + '</div><div class="score-bar"><div class="score-fill" style="width:' +
                        sc + '%"></div></div><div class="score-value">' + sc + '</div><span class="score-tag ' + level
                        .cls + '">' + level.text + '</span></div>';
                });
                document.getElementById('shlResult').innerHTML = shlHtml;

                // ---- IQ ----
                var iqDims = Object.keys(ds['IQ'] || {});
                var iqHtml = '<p><strong>智力测试（行业定制·场景化，严格计分，正确=4分，错误=0分）：</strong></p>';
                iqDims.forEach(function(d) {
                    var sc = ds['IQ'][d] || 0;
                    var level = getLevel(sc);
                    iqHtml += '<div class="score-row"><div class="score-name">' + d + '</div><div class="score-bar"><div class="score-fill" style="width:' +
                        sc + '%"></div></div><div class="score-value">' + sc + '</div><span class="score-tag ' + level
                        .cls + '">' + level.text + '</span></div>';
                });
                document.getElementById('iqResult').innerHTML = iqHtml;

                // ---- EQ ----
                var eqDims = Object.keys(ds['EQ'] || {});
                var eqHtml = '<p><strong>情商测试（通道定制，含反向矫正）：</strong></p>';
                eqDims.forEach(function(d) {
                    var sc = ds['EQ'][d] || 0;
                    var level = getLevel(sc);
                    eqHtml += '<div class="score-row"><div class="score-name">' + d + '</div><div class="score-bar"><div class="score-fill" style="width:' +
                        sc + '%"></div></div><div class="score-value">' + sc + '</div><span class="score-tag ' + level
                        .cls + '">' + level.text + '</span></div>';
                });
                document.getElementById('eqResult').innerHTML = eqHtml;

                // ---- 北森 ----
                var beiDims = Object.keys(ds['北森'] || {});
                var beiHtml = '<p><strong>素养与情商（通道定制，含反向矫正）：</strong></p>';
                beiDims.forEach(function(d) {
                    var sc = ds['北森'][d] || 0;
                    var level = getLevel(sc);
                    beiHtml += '<div class="score-row"><div class="score-name">' + d + '</div><div class="score-bar"><div class="score-fill" style="width:' +
                        sc + '%"></div></div><div class="score-value">' + sc + '</div><span class="score-tag ' + level
                        .cls + '">' + level.text + '</span></div>';
                });
                document.getElementById('beisenResult').innerHTML = beiHtml;

                // ---- 行业与通道适配 ----
                var industry = userInfo.industry,
                    channel = userInfo.channel;
                var fitMap = {
                    '电商': { '管理': ['沟通能力', '决策力', '团队合作'], '技术': ['学习能力', '问题解决', '创新思维'], '幕僚': ['分析能力',
                            '战略规划', '沟通协调'
                        ] },
                    '制造业': { '管理': ['责任心', '执行效率', '团队合作'], '技术': ['专业引领', '问题解决', '学习能力'], '幕僚': ['制度流程',
                            '分析能力', '沟通协调'
                        ] },
                    '金融': { '管理': ['决策力', '领导力', '沟通能力'], '技术': ['逻辑推理', '数据分析', '学习能力'], '幕僚': ['战略规划',
                            '分析能力', '沟通能力'
                        ] },
                    '工贸': { '管理': ['执行效率', '沟通协调', '团队合作'], '技术': ['专业引领', '问题解决', '学习能力'], '幕僚': ['沟通协调',
                            '分析能力', '制度流程'
                        ] },
                    '互联网': { '管理': ['创新思维', '团队合作', '决策力'], '技术': ['学习能力', '创新思维', '问题解决'], '幕僚': ['战略规划',
                            '分析能力', '沟通能力'
                        ] },
                    '其他': { '管理': ['团队合作', '沟通能力', '责任心'], '技术': ['学习能力', '问题解决', '责任心'], '幕僚': ['分析能力',
                            '沟通能力', '责任心'
                        ] }
                };
                var needed = (fitMap[industry] && fitMap[industry][channel]) || ['团队合作', '沟通能力', '责任心'];
                var matchCount = 0,
                    matchDetails = [];
                needed.forEach(function(need) {
                    var sc = (ds['北森'] && ds['北森'][need]) || (ds['OPQ'] && ds['OPQ'][need]) || 0;
                    if (sc >= 70) { matchCount++;
                        matchDetails.push(need + '(' + sc + '分)'); }
                });
                var fitLevel = matchCount >= 2 ? '高度匹配' : (matchCount >= 1 ? '部分匹配' : '匹配度较低');
                var fitHtml = '<p><strong>行业：' + industry + ' | 通道：' + channel + '</strong></p>';
                fitHtml += '<p>关键特质：<strong>' + needed.join('、') + '</strong></p>';
                fitHtml += '<p>匹配度：<strong>' + fitLevel + '</strong>（' + (matchDetails.length ? matchDetails.join('、') :
                    '无') + '）</p>';
                if (matchCount >= 2) fitHtml += '<p>✅ 高度契合，建议重点关注。</p>';
                else if (matchCount >= 1) fitHtml += '<p>📌 部分契合，可针对性面试验证。</p>';
                else fitHtml += '<p>⚠️ 偏差较大，建议审慎评估。</p>';
                document.getElementById('industryFit').innerHTML = fitHtml;

                // ---- 招聘建议（左） ----
                var rec = [];
                rec.push('• 结合“作答质量分析”，若存在偏差预警，建议在面试中增加行为事件访谈（BEI）。');
                rec.push('• MBTI类型' + mbtiType + '，DISC风格' + discType + '，可初步判断其沟通与决策模式。');
                if (ms['OPQ'] < 60) rec.push('• 管理潜质一般，建议入职后安排导师带教或管理基础培训。');
                if (ms['SHL'] < 60) rec.push('• 认知能力偏弱，建议在面试中增加案例分析环节，进一步验证。');
                if (ms['IQ'] < 60) rec.push('• IQ得分偏低，建议进一步评估学习能力和逻辑思维。');
                if (ms['EQ'] < 60) rec.push('• EQ得分偏低，建议关注其情绪管理和人际交往能力。');
                if (matchCount >= 2) rec.push('• 行业与通道匹配度高，建议加快录用流程。');
                else rec.push('• 匹配度一般，建议综合面试表现及背景调查再做决定。');
                document.getElementById('recruitSuggestions').innerHTML = rec.map(function(s) { return '<li>' + s +
                    '</li>'; }).join('');

                // ---- 个人发展建议（右） ----
                var dev = [];
                dev.push('【短期】发挥' + pdpType + '特质，主动承担相关挑战性任务。');
                if (ms['SHL'] < 60) dev.push('• 加强逻辑与量化思维训练，如参加数据分析课程。');
                if (ms['IQ'] < 60) dev.push('• 强化基础逻辑和数学能力，可通过在线平台练习。');
                if (ms['EQ'] < 60) dev.push('• 提升情绪管理与人际沟通技巧，可阅读相关书籍或参加培训。');
                dev.push('【中期】参与跨部门项目，提升沟通与协调能力。');
                dev.push('【长期】根据行业趋势，制定3年职业跃迁计划。');
                if (industry === '电商') dev.push('• 学习数字化运营与私域流量策略。');
                else if (industry === '制造业') dev.push('• 研究精益生产与智能制造。');
                else if (industry === '金融') dev.push('• 考取CFA/FRM等专业资质。');
                else dev.push('• 持续关注行业前沿，拓宽管理视野。');
                document.getElementById('personalSuggestions').innerHTML = dev.map(function(s) { return '<li>' + s +
                    '</li>'; }).join('');

                // 保存结果数据
                window._reportData = {
                    userInfo: userInfo,
                    scores: scores,
                    mbtiType: mbtiType,
                    discType: discType,
                    pdpType: pdpType,
                    discMap: discMap,
                    pdpMap: pdpMap,
                    ms: ms,
                    ds: ds,
                    extRate: extRate,
                    needed: needed,
                    matchCount: matchCount,
                    fitLevel: fitLevel
                };

                showSection('result');
                window.resultData = { scores: scores, userInfo: userInfo };
            }

            // ================================================================
            // 15. PDF下载
            // ================================================================
            window.downloadPDF = function(btn) {
                if (!window.resultData) { alert('请先完成测评！'); return; }
                var orig = btn.innerHTML;
                btn.innerHTML = '⏳ 生成中...';
                btn.disabled = true;
                var el = document.getElementById('result');
                setTimeout(function() {
                    html2canvas(el, { scale: 2, useCORS: true, logging: false, backgroundColor: '#ffffff',
                            height: el.scrollHeight, windowHeight: el.scrollHeight, windowWidth: el
                            .scrollWidth })
                        .then(function(canvas) {
                            var pdf = new jspdf.jsPDF('p', 'mm', 'a4');
                            var w = pdf.internal.pageSize.getWidth(),
                                h = pdf.internal.pageSize.getHeight();
                            var m = 8,
                                cw = w - m * 2,
                                ch = h - m * 2;
                            var ratio = Math.min(cw / canvas.width, ch / canvas.height);
                            var fw = canvas.width * ratio,
                                fh = canvas.height * ratio;
                            pdf.addImage(canvas.toDataURL('image/jpeg', 0.95), 'JPEG', (w - fw) / 2, (h - fh) / 2,
                                fw, fh);
                            pdf.save('测评报告_' + userInfo.name + '.pdf');
                            btn.innerHTML = '✅ 下载成功';
                            setTimeout(function() { btn.innerHTML = orig;
                                btn.disabled = false; }, 2000);
                        }).catch(function() { btn.innerHTML = orig;
                            btn.disabled = false;
                            alert('下载失败，请重试。'); });
                }, 300);
            };

            // ================================================================
            // 16. 复制结果（简要）
            // ================================================================
            window.copyResult = function() {
                var text = '综合人才测评报告（通道定制版，120题）\n姓名：' + userInfo.name + ' 岗位：' + userInfo.dept + ' 通道：' + userInfo
                    .channel + '\n';
                var ms = window.resultData.scores.moduleScores;
                Object.keys(ms).forEach(function(m) { text += m + '：' + ms[m] + '分\n'; });
                text += '\n注：OPQ/SHL/北森为通道定制，IQ为行业定制，EQ为通道定制；DISC/北森/EQ含反向计分，SHL/IQ严格计分。';
                var ta = document.createElement('textarea');
                ta.value = text;
                document.body.appendChild(ta);
                ta.select();
                document.execCommand('copy');
                document.body.removeChild(ta);
                alert('✅ 已复制简要报告。');
            };

            // ================================================================
            // 17. 复制微信摘要
            // ================================================================
            window.copyWechatText = function() {
                if (!window._reportData) {
                    alert('⚠️ 请先完成测评！');
                    return;
                }
                var r = window._reportData;
                var u = r.userInfo;
                var text = '📊 综合人才测评报告摘要（120题版）\n\n';
                text += '姓名：' + u.name + '\n';
                text += '岗位：' + u.dept + '\n';
                text += '行业：' + u.industry + '\n';
                text += '通道：' + u.channel + '\n';
                text += '日期：' + u.date + '\n\n';
                text += '【八大模块评分】\n';
                var mods = ['MBTI', 'DISC', 'PDP', 'OPQ', 'SHL', '北森', 'IQ', 'EQ'];
                mods.forEach(function(m) {
                    var sc = r.ms[m] || 0;
                    var lv = getLevel(sc);
                    text += '  ' + m + '：' + sc + '分（' + lv.text + '）\n';
                });
                text += '\n【性格/风格】\n';
                text += 'MBTI：' + r.mbtiType + '\n';
                text += 'DISC：' + r.discType + '（' + (r.discMap[r.discType] || '') + '）\n';
                text += 'PDP：' + r.pdpType + '（' + (r.pdpMap[r.pdpType] || '') + '）\n\n';
                text += '【适配度】' + r.fitLevel + '\n';
                text += '【极端响应率】' + r.extRate + '%\n\n';
                text += '【招聘建议】\n';
                var recs = document.querySelectorAll('#recruitSuggestions li');
                if (recs.length) {
                    recs.forEach(function(li) {
                        text += '  • ' + li.textContent.trim() + '\n';
                    });
                } else {
                    text += '  • 建议结合面试综合评估。\n';
                }
                text += '\n【个人发展建议】\n';
                var devs = document.querySelectorAll('#personalSuggestions li');
                if (devs.length) {
                    devs.forEach(function(li) {
                        text += '  • ' + li.textContent.trim() + '\n';
                    });
                }
                text += '\n---\n完整报告请查看PDF附件。\n测评系统由DAVY设计（抖音号：d190564680）';

                var ta = document.createElement('textarea');
                ta.value = text;
                document.body.appendChild(ta);
                ta.select();
                document.execCommand('copy');
                document.body.removeChild(ta);
                alert('✅ 报告摘要已复制，请打开微信，添加davy0910为好友，发送即可。');
            };

            // ================================================================
            // 18. 邮件发送（mailto）
            // ================================================================
            window.sendEmailReport = function() {
                if (!window._reportData) {
                    alert('⚠️ 请先完成测评，生成报告后再发送。');
                    return;
                }
                var r = window._reportData;
                var u = r.userInfo;
                var body = '测评报告摘要（120题版）：\n\n';
                body += '姓名：' + u.name + '\n';
                body += '岗位：' + u.dept + '\n';
                body += '行业：' + u.industry + '\n';
                body += '通道：' + u.channel + '\n';
                body += '日期：' + u.date + '\n\n';
                body += '八大模块评分：\n';
                var mods = ['MBTI', 'DISC', 'PDP', 'OPQ', 'SHL', '北森', 'IQ', 'EQ'];
                mods.forEach(function(m) {
                    var sc = r.ms[m] || 0;
                    var lv = getLevel(sc);
                    body += '  ' + m + '：' + sc + '分（' + lv.text + '）\n';
                });
                body += '\nMBTI类型：' + r.mbtiType + '\n';
                body += 'DISC风格：' + r.discType + '（' + (r.discMap[r.discType] || '') + '）\n';
                body += 'PDP特质：' + r.pdpType + '（' + (r.pdpMap[r.pdpType] || '') + '）\n\n';
                body += '行业适配度：' + r.fitLevel + '\n';
                body += '极端响应率：' + r.extRate + '%\n\n';
                body += '招聘建议摘要：\n';
                var recs = document.querySelectorAll('#recruitSuggestions li');
                if (recs.length) {
                    recs.forEach(function(li) {
                        body += '  • ' + li.textContent.trim() + '\n';
                    });
                } else {
                    body += '  • 建议结合面试综合评估。\n';
                }
                body += '\n个人发展建议摘要：\n';
                var devs = document.querySelectorAll('#personalSuggestions li');
                if (devs.length) {
                    devs.forEach(function(li) {
                        body += '  • ' + li.textContent.trim() + '\n';
                    });
                }
                body += '\n---\n完整报告请查看附件PDF（请先下载）。\n';
                body += '本报告由综合人才能力测评系统（120题版）生成。';

                var subject = encodeURIComponent('综合人才测评报告 - ' + u.name);
                var mailto = 'mailto:davy0910@qq.com?subject=' + subject + '&body=' + encodeURIComponent(body);
                window.open(mailto, '_blank');
            };

            // ================================================================
            // 19. 重新测评
            // ================================================================
            window.restartQuiz = function() { currentQuestion = 0;
                answers = [];
                window.resultData = null;
                window._reportData = null;
                showSection('welcome'); };

            // ================================================================
            // 20. 键盘快捷键
            // ================================================================
            document.addEventListener('keydown', function(e) {
                var qa = document.getElementById('quiz').classList.contains('active');
                if (!qa) return;
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
