<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>肺结析AI | LungVision Pro | 폐암 분석 시스템</title>
    <!-- 字体与图表库 -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Chart.js CDN -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(145deg, #eef2f7 0%, #dce5ec 100%);
            min-height: 100vh;
            padding: 20px;
            color: #1a2c3e;
        }

        .container {
            max-width: 1500px;
            margin: 0 auto;
            background: rgba(255,255,255,0.88);
            backdrop-filter: blur(3px);
            border-radius: 44px;
            box-shadow: 0 30px 50px rgba(0,0,0,0.12);
            overflow: hidden;
            padding: 24px 28px;
            transition: all 0.2s;
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            margin-bottom: 28px;
            padding-bottom: 14px;
            border-bottom: 2px solid rgba(46, 125, 121, 0.25);
        }

        .title-section h1 {
            font-size: 1.9rem;
            font-weight: 800;
            background: linear-gradient(130deg, #1B5E5A, #2C9A8F);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            letter-spacing: -0.5px;
        }

        .lang-switch {
            display: flex;
            gap: 12px;
            background: #ffffffdd;
            padding: 6px 18px;
            border-radius: 60px;
            backdrop-filter: blur(4px);
        }

        .lang-btn {
            background: none;
            border: none;
            font-weight: 600;
            font-size: 0.9rem;
            cursor: pointer;
            padding: 6px 18px;
            border-radius: 40px;
            transition: 0.2s;
            color: #2c6e6b;
        }

        .lang-btn.active {
            background: #1f6e6b;
            color: white;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }

        /* 三列灵活布局: 影像区 + 分析区 + 报告区 */
        .dashboard {
            display: flex;
            gap: 24px;
            flex-wrap: wrap;
        }

        .image-panel {
            flex: 1.2;
            min-width: 260px;
            background: #ffffffcc;
            border-radius: 32px;
            padding: 18px;
            backdrop-filter: blur(4px);
            box-shadow: 0 8px 20px rgba(0,0,0,0.04);
        }

        .analysis-panel {
            flex: 1;
            min-width: 260px;
            background: #ffffffcc;
            border-radius: 32px;
            padding: 18px;
            backdrop-filter: blur(4px);
        }

        .report-panel {
            flex: 1.1;
            min-width: 280px;
            background: #ffffffcc;
            border-radius: 32px;
            padding: 18px;
            backdrop-filter: blur(4px);
        }

        .upload-area {
            border: 2px dashed #80b7b2;
            border-radius: 28px;
            padding: 24px 12px;
            text-align: center;
            cursor: pointer;
            transition: 0.2s;
            background: #fafefe;
            margin-bottom: 18px;
        }

        .upload-area:hover {
            border-color: #2c8f8c;
            background: #edf6f4;
        }

        .upload-area i {
            font-size: 44px;
            color: #3e8a87;
        }

        .image-preview {
            background: #eef3f1;
            border-radius: 28px;
            text-align: center;
            min-height: 340px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }

        canvas#imageCanvas {
            max-width: 100%;
            border-radius: 24px;
            box-shadow: 0 8px 18px rgba(0,0,0,0.1);
            background: #cbdcd9;
        }

        .btn-group {
            margin-top: 18px;
            display: flex;
            gap: 12px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .btn {
            border: none;
            padding: 10px 22px;
            border-radius: 40px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.2s;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            font-size: 0.85rem;
        }

        .btn-primary {
            background: #236e6b;
            color: white;
            box-shadow: 0 5px 12px rgba(35,110,107,0.3);
        }

        .btn-primary:hover {
            background: #155e5b;
            transform: translateY(-2px);
        }

        .btn-outline {
            border: 1px solid #7ba29e;
            background: white;
        }

        .stat-card {
            background: #eff6f4;
            border-radius: 24px;
            padding: 14px;
            margin-bottom: 18px;
        }

        .metric-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 12px;
            border-bottom: 1px solid #cce0dc;
            padding-bottom: 6px;
        }

        .risk-gauge {
            text-align: center;
            margin: 10px 0;
        }

        canvas#riskChart {
            max-width: 140px;
            max-height: 140px;
            margin: 0 auto;
        }

        .nodule-list {
            max-height: 200px;
            overflow-y: auto;
            font-size: 0.8rem;
        }

        .nodule-item {
            background: white;
            border-radius: 18px;
            padding: 8px 12px;
            margin-bottom: 8px;
            display: flex;
            justify-content: space-between;
            font-weight: 500;
        }

        .report-text {
            background: #eef3f0;
            border-radius: 20px;
            padding: 14px;
            margin-top: 15px;
            font-size: 0.8rem;
        }

        footer {
            margin-top: 28px;
            text-align: center;
            font-size: 0.7rem;
            color: #5e807c;
            border-top: 1px solid rgba(0,0,0,0.05);
            padding-top: 16px;
        }

        @media (max-width: 1000px) {
            .container { padding: 16px; }
            .dashboard { flex-direction: column; }
        }
    </style>
</head>
<body>
<div class="container">
    <div class="header">
        <div class="title-section">
            <h1><i class="fas fa-lungs"></i> LungVision Pro | 폐암 통합 분석</h1>
            <p data-key="subtitle">AI 기반 영상 분석 + 자동 리포트 · AI影像分析 + 智能图文报告</p>
        </div>
        <div class="lang-switch">
            <button class="lang-btn active" data-lang="zh">🇨🇳 中文</button>
            <button class="lang-btn" data-lang="ko">🇰🇷 한국어</button>
        </div>
    </div>

    <div class="dashboard">
        <!-- 左侧影像区 -->
        <div class="image-panel">
            <div class="upload-area" id="uploadArea">
                <i class="fas fa-cloud-upload-alt"></i>
                <p data-key="dragText">클릭/드래그로 CT/X-ray 업로드 | 点击或拖拽上传肺部影像</p>
                <input type="file" id="fileInput" accept="image/jpeg,image/png,image/jpg" style="display: none;">
                <small data-key="formatHint">JPG/PNG 지원 | 支持JPG/PNG</small>
            </div>
            <div class="image-preview">
                <canvas id="imageCanvas" width="500" height="380" style="width:100%; height:auto; background:#d9e2df;"></canvas>
                <div class="btn-group">
                    <button class="btn btn-primary" id="analyzeBtn"><i class="fas fa-microscope"></i> <span data-key="analyzeBtn">분석 & 리포트 | 分析生成报告</span></button>
                    <button class="btn btn-outline" id="clearBtn"><i class="fas fa-eraser"></i> <span data-key="clearBtn">초기화 | 清除图像</span></button>
                </div>
            </div>
        </div>

        <!-- 中间分析统计数据 -->
        <div class="analysis-panel">
            <h3><i class="fas fa-chart-simple"></i> <span data-key="resultTitle">진단 지표 | 诊断指标</span></h3>
            <div class="stat-card">
                <div class="metric-item">
                    <span><i class="fas fa-microscope"></i> <span data-key="noduleCount">결절 개수 | 结节数量</span></span>
                    <span id="noduleCountValue">—</span>
                </div>
                <div class="metric-item">
                    <span><i class="fas fa-arrows-alt"></i> <span data-key="maxSize">최대 직경 | 最大直径</span></span>
                    <span id="maxSizeValue">—</span>
                </div>
                <div class="metric-item">
                    <span><i class="fas fa-chart-line"></i> <span data-key="riskLevel">위험 지수 | 风险评分</span></span>
                    <span id="riskLevelValue">—</span>
                </div>
                <div id="riskBadge" style="margin-top: 5px;"></div>
            </div>
            <div class="risk-gauge">
                <canvas id="riskChart" width="120" height="120" style="width:100px; height:100px; margin:0 auto;"></canvas>
            </div>
            <div>
                <strong><i class="fas fa-list-ul"></i> <span data-key="noduleDetails">결절 상세 | 结节详情</span></strong>
                <div id="noduleListPanel" class="nodule-list">
                    <div style="text-align:center; color:#65807b;"><span data-key="noDataMsg">분석 후 상세 표시 | 分析后显示结节</span></div>
                </div>
            </div>
        </div>

        <!-- 右侧图文报告区域 + 图表 -->
        <div class="report-panel">
            <h3><i class="fas fa-file-alt"></i> <span data-key="reportTitle">의료 영상 리포트 | 影像诊断报告</span></h3>
            <div style="margin-bottom: 12px;">
                <canvas id="sizeChart" width="400" height="200" style="max-width:100%; height:auto; background:#fefefe; border-radius: 20px;"></canvas>
            </div>
            <div class="report-text" id="dynamicReport">
                <i class="fas fa-info-circle"></i> <span data-key="reportPlaceholder">분석 실행 시 상세 리포트 생성 | 执行分析后生成结构化报告与临床建议</span>
            </div>
            <div style="margin-top: 12px; font-size:0.7rem; background:#eef2f0; border-radius:18px; padding:6px 10px;">
                <i class="fas fa-flask"></i> <span data-key="disclaimer">* 시뮬레이션 분석 (참고용) | *模拟分析仅供科研参考</span>
            </div>
        </div>
    </div>
    <footer>
        <span data-key="footer">LungVision Pro | AI 폐암 보조 진단 | 肺癌智能辅助报告系统 (데모)</span>
    </footer>
</div>

<script>
    // ------------------- 中韩双语字典 ------------------
    const trans = {
        zh: {
            subtitle: "AI 기반 영상 분석 + 자동 리포트 · AI影像分析 + 智能图文报告",
            dragText: "클릭/드래그로 CT/X-ray 업로드 | 点击或拖拽上传肺部影像",
            formatHint: "JPG/PNG 지원 | 支持JPG/PNG",
            analyzeBtn: "분석 & 리포트 | 分析生成报告",
            clearBtn: "초기화 | 清除图像",
            resultTitle: "진단 지표 | 诊断指标",
            noduleCount: "결절 개수 | 结节数量",
            maxSize: "최대 직경 | 最大直径",
            riskLevel: "위험 지수 | 风险评分",
            noduleDetails: "결절 상세 | 结节详情",
            noDataMsg: "분석 후 상세 표시 | 分析后显示结节",
            reportTitle: "의료 영상 리포트 | 影像诊断报告",
            reportPlaceholder: "분석 실행 시 상세 리포트 생성 | 执行分析后生成结构化报告与临床建议",
            disclaimer: "* 시뮬레이션 분석 (참고용) | *模拟分析仅供科研参考",
            footer: "LungVision Pro | AI 폐암 보조 진단 | 肺癌智能辅助报告系统 (데모)",
            risk_low: "저위험 | 低风险",
            risk_mid: "중간 위험 | 中风险",
            risk_high: "고위험 | 高风险",
            followup: "정기 추적 관찰 권장 (6~12개월 후 재검) | 建议定期随访 (6-12个月复查)",
            further: "조직 검사 또는 전문의 상담 권장 | 建议进一步检查或专科咨询",
            good: "양호한 패턴 | 影像表现较良好",
            nodule_prefix: "결절"
        },
        ko: {
            subtitle: "AI 기반 영상 분석 + 자동 리포트",
            dragText: "클릭/드래그로 CT/X-ray 업로드",
            formatHint: "JPG/PNG 지원",
            analyzeBtn: "분석 & 리포트",
            clearBtn: "초기화",
            resultTitle: "진단 지표",
            noduleCount: "결절 개수",
            maxSize: "최대 직경",
            riskLevel: "위험 지수",
            noduleDetails: "결절 상세",
            noDataMsg: "분석 후 상세 표시",
            reportTitle: "의료 영상 리포트",
            reportPlaceholder: "분석 실행 시 상세 리포트 생성",
            disclaimer: "* 시뮬레이션 분석 (참고용)",
            footer: "LungVision Pro | AI 폐암 보조 진단 (데모)",
            risk_low: "저위험",
            risk_mid: "중간 위험",
            risk_high: "고위험",
            followup: "정기 추적 관찰 권장 (6~12개월 후 재검)",
            further: "조직 검사 또는 전문의 상담 권장",
            good: "양호한 패턴",
            nodule_prefix: "결절"
        }
    };
    let currentLang = 'zh';
    let currentImage = null;
    let canvas = document.getElementById('imageCanvas');
    let ctx = canvas.getContext('2d');
    let analysisData = { nodules: [], riskScore: 0, maxDiameter: 0 };
    let riskChartInstance = null;
    let sizeChartInstance = null;

    // 更新语言
    function updateUIByLang() {
        document.querySelectorAll('[data-key]').forEach(el => {
            const key = el.getAttribute('data-key');
            if (trans[currentLang][key]) {
                if (el.innerText !== undefined) el.innerText = trans[currentLang][key];
                else el.textContent = trans[currentLang][key];
            }
        });
        // 刷新结节列表显示 (如果有数据)
        if (analysisData.nodules.length > 0) {
            renderNoduleList(analysisData.nodules);
            renderReportText(analysisData.riskScore, analysisData.maxDiameter, analysisData.nodules.length);
        } else {
            document.getElementById('noduleListPanel').innerHTML = `<div style="text-align:center; color:#65807b;">${trans[currentLang].noDataMsg}</div>`;
            if(!analysisData.nodules.length) document.getElementById('dynamicReport').innerHTML = `<i class="fas fa-info-circle"></i> ${trans[currentLang].reportPlaceholder}`;
        }
        updateMetricsUI();
        updateRiskGauge(analysisData.riskScore);
        if(analysisData.nodules.length) updateSizeChart(analysisData.nodules);
        else if(sizeChartInstance) { sizeChartInstance.destroy(); sizeChartInstance = null; }
    }

    function setLanguage(lang) {
        currentLang = lang;
        updateUIByLang();
        document.querySelectorAll('.lang-btn').forEach(btn => {
            btn.classList.toggle('active', btn.getAttribute('data-lang') === lang);
        });
    }

    // 显示指标数值
    function updateMetricsUI() {
        document.getElementById('noduleCountValue').innerText = analysisData.nodules.length ? analysisData.nodules.length : '—';
        document.getElementById('maxSizeValue').innerText = analysisData.maxDiameter > 0 ? analysisData.maxDiameter.toFixed(1) + ' mm' : '—';
        let riskText = '';
        if (analysisData.riskScore < 0.35) riskText = trans[currentLang].risk_low;
        else if (analysisData.riskScore < 0.65) riskText = trans[currentLang].risk_mid;
        else riskText = trans[currentLang].risk_high;
        document.getElementById('riskLevelValue').innerText = analysisData.riskScore > 0 ? `${riskText} (${(analysisData.riskScore*100).toFixed(0)}%)` : '—';
        const badgeDiv = document.getElementById('riskBadge');
        if(analysisData.nodules.length){
            let cls = analysisData.riskScore<0.35?'risk-low-badge':(analysisData.riskScore<0.65?'risk-mid-badge':'risk-high-badge');
            badgeDiv.innerHTML = `<span style="background:${analysisData.riskScore<0.35?'#5cb85c':(analysisData.riskScore<0.65?'#f0ad4e':'#d9534f')}; color:white; padding:5px 12px; border-radius:40px; font-size:0.75rem;">${riskText}</span>`;
        } else badgeDiv.innerHTML = '';
    }

    // 风险仪表盘 (圆环图)
    function updateRiskGauge(score) {
        const canvasRisk = document.getElementById('riskChart');
        if (!canvasRisk) return;
        const ctxRisk = canvasRisk.getContext('2d');
        if (riskChartInstance) riskChartInstance.destroy();
        riskChartInstance = new Chart(ctxRisk, {
            type: 'doughnut',
            data: {
                datasets: [{
                    data: [score, 1 - score],
                    backgroundColor: ['#e67e22', '#dddddd'],
                    borderWidth: 0,
                    circumference: 360,
                    rotation: -90,
                }]
            },
            options: {
                cutout: '70%',
                responsive: true,
                maintainAspectRatio: true,
                plugins: { tooltip: { enabled: false }, legend: { display: false } }
            }
        });
        // 中间加文字
        let midText = score > 0 ? `${Math.round(score * 100)}%` : '?';
        ctxRisk.font = 'bold 16px "Inter"';
        ctxRisk.fillStyle = '#1a3e3b';
        ctxRisk.textAlign = 'center';
        ctxRisk.textBaseline = 'middle';
        // 在canvas绘制中心文字 (需要延迟)
        setTimeout(() => {
            const centerX = canvasRisk.width/2, centerY = canvasRisk.height/2;
            ctxRisk.font = 'bold 18px "Inter"';
            ctxRisk.fillStyle = '#1f5e5a';
            ctxRisk.clearRect(0,0,canvasRisk.width,canvasRisk.height);
            riskChartInstance.draw();
            ctxRisk.font = 'bold 17px "Inter"';
            ctxRisk.fillStyle = '#1f5e5a';
            ctxRisk.fillText(midText, centerX, centerY);
        }, 10);
    }

    // 结节大小分布柱状图
    function updateSizeChart(nodules) {
        const chartCanvas = document.getElementById('sizeChart');
        if(!chartCanvas) return;
        if(sizeChartInstance) sizeChartInstance.destroy();
        if(!nodules.length){
            const ctxBar = chartCanvas.getContext('2d');
            ctxBar.clearRect(0,0,chartCanvas.width,chartCanvas.height);
            ctxBar.fillStyle = '#ccc';
            ctxBar.fillText(trans[currentLang].noDataMsg, 20,50);
            return;
        }
        const sizes = nodules.map(n => n.diameterMm);
        const labels = nodules.map((_, idx) => `${trans[currentLang].nodule_prefix} ${idx+1}`);
        sizeChartInstance = new Chart(chartCanvas, {
            type: 'bar',
            data: {
                labels: labels,
                datasets: [{
                    label: currentLang === 'zh' ? '结节直径 (mm)' : '결절 직경 (mm)',
                    data: sizes,
                    backgroundColor: '#47918e',
                    borderRadius: 12,
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: true,
                plugins: { legend: { position: 'top' } },
                scales: { y: { beginAtZero: true, title: { display: true, text: 'mm' } } }
            }
        });
    }

    // 生成文本报告
    function renderReportText(riskScore, maxDiameter, noduleCount) {
        const reportDiv = document.getElementById('dynamicReport');
        let advice = '';
        let summary = '';
        if (noduleCount === 0) {
            summary = currentLang === 'zh' ? '🔍 未发现明确肺结节，影像学表现大致正常。' : '🔍 명확한 폐 결절이 발견되지 않았으며, 영상 소견 정상 범위입니다.';
            advice = currentLang === 'zh' ? '✅ 建议常规年度体检。' : '✅ 정기적인 연간 건강검진을 권장합니다.';
        } else {
            if (riskScore < 0.35) {
                summary = currentLang === 'zh' ? `📊 发现 ${noduleCount} 个微小结节，形态规则，风险较低。` : `📊 ${noduleCount}개의 작은 결절, 형태 규칙적이며 위험 낮음.`;
                advice = trans[currentLang].followup;
            } else if (riskScore < 0.65) {
                summary = currentLang === 'zh' ? `⚠️ 发现 ${noduleCount} 个结节，最大直径 ${maxDiameter.toFixed(1)} mm，部分边缘不规则。` : `⚠️ ${noduleCount}개의 결절, 최대 직경 ${maxDiameter.toFixed(1)} mm, 일부 경계 불규칙.`;
                advice = currentLang === 'zh' ? '📌 建议短期复查 (3-6个月) 或增强CT评估。' : '📌 단기 추적 검사 (3~6개월) 또는 조영증강 CT 권장.';
            } else {
                summary = currentLang === 'zh' ? `🚨 高危结节：${noduleCount} 个，最大 ${maxDiameter.toFixed(1)} mm，形态可疑 (分叶/毛刺征象)。` : `🚨 고위험 결절: ${noduleCount}개, 최대 ${maxDiameter.toFixed(1)} mm, 의심스러운 형태 (분엽/가시).`;
                advice = trans[currentLang].further;
            }
        }
        const reportHtml = `<strong><i class="fas fa-stethoscope"></i> ${currentLang === 'zh' ? '影像学结论' : '영상의학 결론'}</strong><br>${summary}<br><br>
                            <strong><i class="fas fa-clinic-medical"></i> ${currentLang === 'zh' ? '临床建议' : '임상 권고사항'}</strong><br>${advice}<br>
                            ${noduleCount>0? `<hr style="margin:8px 0"><small><i class="fas fa-chart-simple"></i> ${currentLang === 'zh' ? '最大结节尺寸分布见柱状图' : '최대 결절 크기 분포 차트 참조'}</small>` : ''}`;
        reportDiv.innerHTML = reportHtml;
    }

    // ---------- 图像结节检测算法 (保持与原功能一致，增强稳定) ----------
    function findNodulesAdvanced(imageData, width, height) {
        let gray = new Uint8ClampedArray(width*height);
        for(let i=0;i<imageData.data.length;i+=4){
            let r=imageData.data[i], g=imageData.data[i+1], b=imageData.data[i+2];
            gray[i/4] = 0.299*r + 0.587*g + 0.114*b;
        }
        let sum=0; for(let v of gray) sum+=v;
        let mean = sum/gray.length;
        let variance=0; for(let v of gray) variance+=(v-mean)**2;
        let std = Math.sqrt(variance/gray.length);
        let threshold = mean + std*0.55;
        let binary = new Uint8Array(width*height);
        for(let i=0;i<gray.length;i++) binary[i] = gray[i] > threshold ? 1 : 0;
        let labels = new Int32Array(width*height).fill(0);
        let curLabel=1;
        let eq=[];
        for(let y=0;y<height;y++){
            for(let x=0;x<width;x++){
                let idx=y*width+x;
                if(binary[idx]===0) continue;
                let up=y>0?labels[(y-1)*width+x]:0;
                let left=x>0?labels[y*width+(x-1)]:0;
                if(up===0 && left===0) { labels[idx]=curLabel; eq.push([curLabel,curLabel]); curLabel++; }
                else if(up!==0 && left===0) labels[idx]=up;
                else if(up===0 && left!==0) labels[idx]=left;
                else { let m=Math.min(up,left); labels[idx]=m; if(up!==left){ eq.push([up,left]); eq.push([left,up]); } }
            }
        }
        let parent = new Array(curLabel).fill(0).map((_,i)=>i);
        function find(x){ while(parent[x]!==x){ parent[x]=parent[parent[x]]; x=parent[x]; } return x; }
        function union(a,b){ let ra=find(a),rb=find(b); if(ra!==rb) parent[rb]=ra; }
        for(let [a,b] of eq) union(a,b);
        for(let i=1;i<curLabel;i++) find(i);
        let newLabelMap=new Map();
        let finalLabel=new Int32Array(width*height);
        let nextLabel=1;
        for(let i=0;i<width*height;i++){
            let l=labels[i];
            if(l===0) continue;
            let root=parent[l];
            if(!newLabelMap.has(root)) newLabelMap.set(root, nextLabel++);
            finalLabel[i]=newLabelMap.get(root);
        }
        let regions=new Map();
        for(let i=0;i<width*height;i++){
            let lab=finalLabel[i];
            if(lab===0) continue;
            if(!regions.has(lab)) regions.set(lab,{area:0,minX:width,minY:height,maxX:0,maxY:0});
            let reg=regions.get(lab);
            reg.area++;
            let x=i%width, y=Math.floor(i/width);
            reg.minX=Math.min(reg.minX,x); reg.minY=Math.min(reg.minY,y);
            reg.maxX=Math.max(reg.maxX,x); reg.maxY=Math.max(reg.maxY,y);
        }
        let nodules=[];
        for(let [lab,reg] of regions.entries()){
            let area=reg.area;
            if(area<10 || area>1300) continue;
            let w=reg.maxX-reg.minX+1, h=reg.maxY-reg.minY+1;
            let diameterPx = (w+h)/2;
            let physSize = diameterPx * 0.42; // mm模拟
            let roundness = area / (Math.PI * (diameterPx/2)*(diameterPx/2));
            if(roundness>0.45 && area>8 && w/h<2.2 && h/w<2.2){
                nodules.push({
                    x: (reg.minX+reg.maxX)/2, y: (reg.minY+reg.maxY)/2,
                    radius: diameterPx/2, area: area,
                    diameterMm: physSize, widthBox:w, heightBox:h
                });
            }
        }
        nodules.sort((a,b)=>b.area-a.area);
        let filtered=[];
        for(let n of nodules){
            let overlap=false;
            for(let ex of filtered){
                let dx=ex.x-n.x, dy=ex.y-n.y;
                if(Math.hypot(dx,dy)<(ex.radius+n.radius)*0.7) {overlap=true; break;}
            }
            if(!overlap) filtered.push(n);
        }
        return filtered;
    }

    async function runAnalysis() {
        if(!currentImage){
            alert(currentLang==='zh'?'먼저 이미지를 업로드하세요 | 请先上传影像':'Please upload an image first');
            return;
        }
        const w = canvas.width, h = canvas.height;
        let imgData = ctx.getImageData(0,0,w,h);
        let nodules = findNodulesAdvanced(imgData, w, h);
        let maxDia = nodules.length>0 ? Math.max(...nodules.map(n=>n.diameterMm)) : 0;
        let riskScore = 0;
        if(nodules.length>0){
            let sizeFactor = Math.min(1.0, maxDia/18.0);
            let countFactor = Math.min(1.0, nodules.length/3.5);
            let irrSum=0;
            for(let n of nodules){
                let idealArea = Math.PI * (n.radius*n.radius);
                let ir = 1 - (n.area/idealArea);
                irrSum += Math.abs(ir);
            }
            let irregularity = irrSum/nodules.length;
            riskScore = 0.4*sizeFactor + 0.4*countFactor + 0.2*Math.min(1, irregularity*1.3);
            riskScore = Math.min(0.97, riskScore);
        } else { riskScore = 0.03; }
        analysisData = { nodules, riskScore, maxDiameter: maxDia };
        drawNodulesOnCanvas(nodules);
        updateMetricsUI();
        updateRiskGauge(riskScore);
        renderNoduleList(nodules);
        updateSizeChart(nodules);
        renderReportText(riskScore, maxDia, nodules.length);
    }

    function drawNodulesOnCanvas(nodules) {
        if(!currentImage) return;
        ctx.drawImage(currentImage, 0, 0, canvas.width, canvas.height);
        for(let nod of nodules){
            ctx.beginPath();
            ctx.arc(nod.x, nod.y, nod.radius+2, 0, 2*Math.PI);
            ctx.strokeStyle = '#e63946';
            ctx.lineWidth = 2.5;
            ctx.stroke();
            ctx.beginPath();
            ctx.arc(nod.x, nod.y, nod.radius-0.5, 0, 2*Math.PI);
            ctx.strokeStyle = '#ffbc6e';
            ctx.lineWidth = 1.8;
            ctx.stroke();
            ctx.font = "bold 13px 'Inter'";
            ctx.fillStyle = '#fff1cf';
            ctx.shadowBlur=3;
            ctx.fillText(`${nod.diameterMm.toFixed(1)}mm`, nod.x+5, nod.y-5);
            ctx.shadowBlur=0;
        }
    }

    function renderNoduleList(nodules) {
        const container = document.getElementById('noduleListPanel');
        if(!nodules.length) { container.innerHTML = `<div style="color:#65807b;">${trans[currentLang].noDataMsg}</div>`; return; }
        let html='';
        nodules.forEach((n,i)=>{
            html+=`<div class="nodule-item"><span>${trans[currentLang].nodule_prefix} ${i+1}</span><span>${n.diameterMm.toFixed(1)} mm</span></div>`;
        });
        container.innerHTML = html;
    }

    function clearAll() {
        if(currentImage){
            ctx.drawImage(currentImage,0,0,canvas.width,canvas.height);
        } else {
            ctx.fillStyle = "#d9e2df";
            ctx.fillRect(0,0,canvas.width,canvas.height);
        }
        analysisData = { nodules:[], riskScore:0, maxDiameter:0 };
        updateMetricsUI();
        document.getElementById('noduleListPanel').innerHTML = `<div style="text-align:center; color:#65807b;">${trans[currentLang].noDataMsg}</div>`;
        document.getElementById('dynamicReport').innerHTML = `<i class="fas fa-info-circle"></i> ${trans[currentLang].reportPlaceholder}`;
        if(sizeChartInstance) { sizeChartInstance.destroy(); sizeChartInstance=null; }
        if(riskChartInstance) riskChartInstance.destroy();
        const riskC=document.getElementById('riskChart');
        if(riskC) { let rctx=riskC.getContext('2d'); rctx.clearRect(0,0,riskC.width,riskC.height); rctx.fillStyle='#ccc'; rctx.fillText('—', riskC.width/2, riskC.height/2); }
    }

    // 上传图片逻辑
    function handleImage(file){
        if(!file) return;
        let reader = new FileReader();
        reader.onload=(e)=>{
            let img=new Image();
            img.onload=()=>{
                currentImage=img;
                let maxW=500;
                let w=img.width, h=img.height;
                if(w>maxW){ h = h * (maxW/w); w = maxW; }
                canvas.width=w; canvas.height=h;
                ctx.drawImage(img,0,0,w,h);
                clearAll(); // reset data, keep image
                analysisData={ nodules:[], riskScore:0, maxDiameter:0 };
                updateMetricsUI();
                document.getElementById('noduleListPanel').innerHTML = `<div>${trans[currentLang].noDataMsg}</div>`;
                document.getElementById('dynamicReport').innerHTML = `<i class="fas fa-info-circle"></i> ${trans[currentLang].reportPlaceholder}`;
            };
            img.src=e.target.result;
        };
        reader.readAsDataURL(file);
    }

    // 事件绑定
    const uploadAreaDiv = document.getElementById('uploadArea');
    const fileInputElem = document.getElementById('fileInput');
    uploadAreaDiv.addEventListener('click',()=>fileInputElem.click());
    fileInputElem.addEventListener('change',(e)=> { if(e.target.files.length) handleImage(e.target.files[0]); });
    uploadAreaDiv.addEventListener('dragover',(e)=>e.preventDefault());
    uploadAreaDiv.addEventListener('drop',(e)=>{ e.preventDefault(); if(e.dataTransfer.files.length) handleImage(e.dataTransfer.files[0]); });
    document.getElementById('analyzeBtn').addEventListener('click', runAnalysis);
    document.getElementById('clearBtn').addEventListener('click', ()=>{ if(currentImage){ctx.drawImage(currentImage,0,0,canvas.width,canvas.height);} clearAll(); });
    document.querySelectorAll('.lang-btn').forEach(btn=>btn.addEventListener('click',()=>setLanguage(btn.getAttribute('data-lang'))));
    setLanguage('zh');
    ctx.fillStyle = "#d9e2df";
    ctx.fillRect(0,0,canvas.width,canvas.height);
    ctx.fillStyle = "#516e6b";
    ctx.font = "14px Inter";
    ctx.fillText("대기 | 等待影像", 30,50);
</script>
</body>
</html>
