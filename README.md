<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>肺结析Pro | LungNodule AI | 폐결절 분석 시스템</title>
    <!-- 字体、图标、图表库 -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(145deg, #ecf3f8 0%, #dce5ec 100%);
            min-height: 100vh;
            padding: 20px;
            color: #1a2c3e;
        }
        .container {
            max-width: 1500px;
            margin: 0 auto;
            background: rgba(255,255,255,0.88);
            backdrop-filter: blur(2px);
            border-radius: 44px;
            box-shadow: 0 30px 45px rgba(0,0,0,0.1);
            overflow: hidden;
            padding: 24px 28px;
        }
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            margin-bottom: 28px;
            padding-bottom: 16px;
            border-bottom: 2px solid rgba(46, 109, 106, 0.2);
        }
        .title-section h1 {
            font-size: 1.9rem;
            font-weight: 800;
            background: linear-gradient(130deg, #1B5E5A, #2C9A8F);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
        }
        .lang-switch {
            display: flex;
            gap: 12px;
            background: #ffffffdd;
            padding: 6px 18px;
            border-radius: 60px;
        }
        .lang-btn {
            background: none;
            border: none;
            font-weight: 600;
            padding: 6px 18px;
            border-radius: 40px;
            cursor: pointer;
            color: #2c6e6b;
        }
        .lang-btn.active {
            background: #1f6e6b;
            color: white;
        }
        .dashboard {
            display: flex;
            gap: 24px;
            flex-wrap: wrap;
        }
        .image-panel, .analysis-panel, .report-panel {
            background: #ffffffcc;
            backdrop-filter: blur(4px);
            border-radius: 32px;
            padding: 20px;
            box-shadow: 0 8px 20px rgba(0,0,0,0.04);
        }
        .image-panel { flex: 1.2; min-width: 280px; }
        .analysis-panel { flex: 0.9; min-width: 260px; }
        .report-panel { flex: 1; min-width: 280px; }
        .upload-area {
            border: 2px dashed #80b7b2;
            border-radius: 28px;
            padding: 24px 12px;
            text-align: center;
            cursor: pointer;
            background: #fafefe;
            margin-bottom: 18px;
        }
        .upload-area i { font-size: 44px; color: #3e8a87; }
        .image-preview {
            background: #eef3f1;
            border-radius: 28px;
            text-align: center;
            min-height: 340px;
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
            justify-content: center;
        }
        .btn {
            border: none;
            padding: 10px 22px;
            border-radius: 40px;
            font-weight: 600;
            cursor: pointer;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }
        .btn-primary { background: #236e6b; color: white; box-shadow: 0 5px 12px rgba(35,110,107,0.3); }
        .btn-outline { border: 1px solid #7ba29e; background: white; }
        .stat-card { background: #eff6f4; border-radius: 24px; padding: 14px; margin-bottom: 18px; }
        .metric-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 12px;
            border-bottom: 1px solid #cce0dc;
            padding-bottom: 6px;
        }
        .risk-gauge { text-align: center; margin: 10px 0; }
        canvas#riskChart { max-width: 120px; max-height: 120px; margin: 0 auto; }
        .nodule-list { max-height: 200px; overflow-y: auto; font-size: 0.8rem; }
        .nodule-item {
            background: white;
            border-radius: 18px;
            padding: 8px 12px;
            margin-bottom: 8px;
            display: flex;
            justify-content: space-between;
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
        @media (max-width: 1000px) { .dashboard { flex-direction: column; } }
    </style>
</head>
<body>
<div class="container">
    <div class="header">
        <div class="title-section">
            <h1><i class="fas fa-lungs"></i> LungNodule AI | 폐결절 정밀분석</h1>
            <p data-key="subtitle">형태학적 필터링 기반 폐 결절 탐지 + 통계 차트 · 形态学过滤 + 智能图表报告</p>
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
                    <button class="btn btn-primary" id="analyzeBtn"><i class="fas fa-microscope"></i> <span data-key="analyzeBtn">정밀 분석 | 精准分析</span></button>
                    <button class="btn btn-outline" id="clearBtn"><i class="fas fa-eraser"></i> <span data-key="clearBtn">초기화 | 清除</span></button>
                </div>
            </div>
        </div>

        <!-- 中间分析区 + 圆环图 -->
        <div class="analysis-panel">
            <h3><i class="fas fa-chart-simple"></i> <span data-key="resultTitle">진단 지표 | 诊断指标</span></h3>
            <div class="stat-card">
                <div class="metric-item">
                    <span><i class="fas fa-microscope"></i> <span data-key="noduleCount">결절 개수 | 结节数</span></span>
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
                <canvas id="riskChart" width="120" height="120"></canvas>
            </div>
            <div>
                <strong><i class="fas fa-list-ul"></i> <span data-key="noduleDetails">결절 상세 | 结节详情</span></strong>
                <div id="noduleListPanel" class="nodule-list">
                    <div style="text-align:center; color:#65807b;"><span data-key="noDataMsg">분석 후 표시 | 分析后显示</span></div>
                </div>
            </div>
        </div>

        <!-- 右侧图表 + 文字报告 -->
        <div class="report-panel">
            <h3><i class="fas fa-chart-bar"></i> <span data-key="chartTitle">크기 분포 | 尺寸分布</span></h3>
            <canvas id="sizeChart" width="400" height="200" style="max-width:100%; background:#fefefe; border-radius: 20px;"></canvas>
            <div class="report-text" id="dynamicReport">
                <i class="fas fa-info-circle"></i> <span data-key="reportPlaceholder">분석 실행 시 상세 리포트 | 执行分析生成详细报告</span>
            </div>
            <div style="margin-top: 12px; font-size:0.7rem; background:#eef2f0; border-radius:18px; padding:6px 10px;">
                <i class="fas fa-flask"></i> <span data-key="disclaimer">* 형태학적 시뮬레이션 (참고용) | *形态学模拟分析，仅供科研参考</span>
            </div>
        </div>
    </div>
    <footer><span data-key="footer">LungNodule AI | 폐결절 보조 진단 | 智能结节分析 (형태학 기반 / 基于形态学)</span></footer>
</div>

<script>
    // ---------- 双语 ----------
    const trans = {
        zh: {
            subtitle: "형태학적 필터링 기반 폐 결절 탐지 + 통계 차트 · 形态学过滤 + 智能图表报告",
            dragText: "클릭/드래그로 CT/X-ray 업로드 | 点击或拖拽上传肺部影像",
            formatHint: "JPG/PNG 지원 | 支持JPG/PNG",
            analyzeBtn: "정밀 분석 | 精准分析",
            clearBtn: "초기화 | 清除",
            resultTitle: "진단 지표 | 诊断指标",
            noduleCount: "결절 개수 | 结节数",
            maxSize: "최대 직경 | 最大直径",
            riskLevel: "위험 지수 | 风险评分",
            noduleDetails: "결절 상세 | 结节详情",
            noDataMsg: "분석 후 표시 | 分析后显示",
            chartTitle: "크기 분포 | 尺寸分布",
            reportPlaceholder: "분석 실행 시 상세 리포트 | 执行分析生成详细报告",
            disclaimer: "* 형태학적 시뮬레이션 (참고용) | *形态学模拟分析，仅供科研参考",
            footer: "LungNodule AI | 폐결절 보조 진단 | 智能结节分析 (형태학 기반 / 基于形态学)",
            risk_low: "저위험 | 低风险",
            risk_mid: "중간 위험 | 中风险",
            risk_high: "고위험 | 高风险",
            followup: "정기 추적 관찰 권장 (6~12개월 후 재검) | 建议定期随访",
            further: "전문의 상담 및 추가 검사 권장 | 建议专科咨询",
            nodule_prefix: "결절"
        },
        ko: {
            subtitle: "형태학적 필터링 기반 폐 결절 탐지 + 통계 차트",
            dragText: "클릭/드래그로 CT/X-ray 업로드",
            formatHint: "JPG/PNG 지원",
            analyzeBtn: "정밀 분석",
            clearBtn: "초기화",
            resultTitle: "진단 지표",
            noduleCount: "결절 개수",
            maxSize: "최대 직경",
            riskLevel: "위험 지수",
            noduleDetails: "결절 상세",
            noDataMsg: "분석 후 표시",
            chartTitle: "크기 분포",
            reportPlaceholder: "분석 실행 시 상세 리포트",
            disclaimer: "* 형태학적 시뮬레이션 (참고용)",
            footer: "LungNodule AI | 폐결절 보조 진단 (형태학 기반)",
            risk_low: "저위험",
            risk_mid: "중간 위험",
            risk_high: "고위험",
            followup: "정기 추적 관찰 권장 (6~12개월 후 재검)",
            further: "전문의 상담 및 추가 검사 권장",
            nodule_prefix: "결절"
        }
    };
    let currentLang = 'zh';
    let currentImage = null;
    let canvas = document.getElementById('imageCanvas');
    let ctx = canvas.getContext('2d');
    let analysisData = { nodules: [], riskScore: 0, maxDiameter: 0 };
    let riskChartInstance = null, sizeChartInstance = null;

    function updateUIByLang() {
        document.querySelectorAll('[data-key]').forEach(el => {
            const key = el.getAttribute('data-key');
            if (trans[currentLang][key]) el.innerText = trans[currentLang][key];
        });
        if (analysisData.nodules.length) {
            renderNoduleList(analysisData.nodules);
            renderReportText(analysisData.riskScore, analysisData.maxDiameter, analysisData.nodules.length);
        } else {
            document.getElementById('noduleListPanel').innerHTML = `<div style="color:#65807b;">${trans[currentLang].noDataMsg}</div>`;
            document.getElementById('dynamicReport').innerHTML = `<i class="fas fa-info-circle"></i> ${trans[currentLang].reportPlaceholder}`;
        }
        updateMetricsUI();
        if (analysisData.riskScore > 0) updateRiskGauge(analysisData.riskScore);
        if (analysisData.nodules.length) updateSizeChart(analysisData.nodules);
        else if(sizeChartInstance) { sizeChartInstance.destroy(); sizeChartInstance=null; }
    }
    function setLanguage(lang) { currentLang = lang; updateUIByLang(); document.querySelectorAll('.lang-btn').forEach(btn => btn.classList.toggle('active', btn.getAttribute('data-lang')===lang)); }

    function updateMetricsUI() {
        document.getElementById('noduleCountValue').innerText = analysisData.nodules.length || '—';
        document.getElementById('maxSizeValue').innerText = analysisData.maxDiameter>0 ? analysisData.maxDiameter.toFixed(1)+' mm' : '—';
        let riskText = '';
        if (analysisData.riskScore < 0.35) riskText = trans[currentLang].risk_low;
        else if (analysisData.riskScore < 0.65) riskText = trans[currentLang].risk_mid;
        else riskText = trans[currentLang].risk_high;
        document.getElementById('riskLevelValue').innerText = analysisData.riskScore>0 ? `${riskText} (${(analysisData.riskScore*100).toFixed(0)}%)` : '—';
        const badgeDiv = document.getElementById('riskBadge');
        if(analysisData.nodules.length){
            let color = analysisData.riskScore<0.35?'#5cb85c':(analysisData.riskScore<0.65?'#f0ad4e':'#d9534f');
            badgeDiv.innerHTML = `<span style="background:${color}; color:white; padding:5px 12px; border-radius:40px; font-size:0.75rem;">${riskText}</span>`;
        } else badgeDiv.innerHTML = '';
    }

    function updateRiskGauge(score) {
        const canvasRisk = document.getElementById('riskChart');
        if (!canvasRisk) return;
        if (riskChartInstance) riskChartInstance.destroy();
        const ctxRisk = canvasRisk.getContext('2d');
        riskChartInstance = new Chart(ctxRisk, {
            type: 'doughnut',
            data: { datasets: [{ data: [score, 1-score], backgroundColor: ['#e67e22', '#dddddd'], borderWidth: 0, circumference: 360, rotation: -90 }] },
            options: { cutout: '70%', responsive: true, maintainAspectRatio: true, plugins: { tooltip: { enabled: false }, legend: { display: false } } }
        });
        setTimeout(() => {
            const cw = canvasRisk.width, ch = canvasRisk.height;
            ctxRisk.font = 'bold 16px "Inter"';
            ctxRisk.fillStyle = '#1f5e5a';
            ctxRisk.textAlign = 'center';
            ctxRisk.textBaseline = 'middle';
            ctxRisk.fillText(`${Math.round(score*100)}%`, cw/2, ch/2);
        }, 10);
    }

    function updateSizeChart(nodules) {
        const chartCanvas = document.getElementById('sizeChart');
        if (!chartCanvas) return;
        if (sizeChartInstance) sizeChartInstance.destroy();
        if (!nodules.length) {
            const sctx = chartCanvas.getContext('2d');
            sctx.clearRect(0,0,chartCanvas.width,chartCanvas.height);
            sctx.fillStyle = '#aaa';
            sctx.fillText(trans[currentLang].noDataMsg, 20,40);
            return;
        }
        const sizes = nodules.map(n => n.diameterMm);
        const labels = nodules.map((_,i)=> `${trans[currentLang].nodule_prefix} ${i+1}`);
        sizeChartInstance = new Chart(chartCanvas, {
            type: 'bar',
            data: { labels, datasets: [{ label: currentLang==='zh'?'结节直径 (mm)':'결절 직경 (mm)', data: sizes, backgroundColor: '#47918e', borderRadius: 8 }] },
            options: { responsive: true, maintainAspectRatio: true, scales: { y: { beginAtZero: true, title: { display: true, text: 'mm' } } } }
        });
    }

    function renderReportText(riskScore, maxDiameter, noduleCount) {
        const reportDiv = document.getElementById('dynamicReport');
        let conclusion = '', advice = '';
        if (noduleCount === 0) {
            conclusion = currentLang === 'zh' ? '🔍 未检出明确肺结节，影像特征良好。' : '🔍 명확한 폐 결절 미검출, 영상 특징 양호.';
            advice = currentLang === 'zh' ? '✅ 建议常规年度随访。' : '✅ 정기적 연간 추적 관찰 권장.';
        } else {
            if (riskScore < 0.35) {
                conclusion = currentLang === 'zh' ? `📊 检出 ${noduleCount} 个微小结节，形态规则，风险较低。` : `📊 ${noduleCount}개의 작은 결절, 형태 규칙적, 위험 낮음.`;
                advice = trans[currentLang].followup;
            } else if (riskScore < 0.65) {
                conclusion = currentLang === 'zh' ? `⚠️ 检出 ${noduleCount} 个结节，最大 ${maxDiameter.toFixed(1)} mm，部分边缘欠规则。` : `⚠️ ${noduleCount}개 결절, 최대 ${maxDiameter.toFixed(1)} mm, 일부 경계 불규칙.`;
                advice = currentLang === 'zh' ? '📌 建议短期复查 (3-6个月) 或增强CT。' : '📌 단기 추적 검사 (3~6개월) 또는 조영증강 CT 권장.';
            } else {
                conclusion = currentLang === 'zh' ? `🚨 高危结节：${noduleCount} 个，最大 ${maxDiameter.toFixed(1)} mm，形态可疑。` : `🚨 고위험 결절: ${noduleCount}개, 최대 ${maxDiameter.toFixed(1)} mm, 의심스러운 형태.`;
                advice = trans[currentLang].further;
            }
        }
        reportDiv.innerHTML = `<strong><i class="fas fa-stethoscope"></i> ${currentLang==='zh'?'影像学结论':'영상의학 결론'}</strong><br>${conclusion}<br><br>
                               <strong><i class="fas fa-clinic-medical"></i> ${currentLang==='zh'?'临床建议':'임상 권고'}</strong><br>${advice}`;
    }

    // ---------- 改进的结节检测：肺野约束、形态学严格过滤、边缘排除 ----------
    function preprocessGray(imageData, width, height) {
        let gray = new Uint8ClampedArray(width*height);
        for(let i=0;i<imageData.data.length;i+=4){
            let r=imageData.data[i], g=imageData.data[i+1], b=imageData.data[i+2];
            gray[i/4] = 0.299*r + 0.587*g + 0.114*b;
        }
        // 简单中值滤波 (3x3) 去噪
        let filtered = new Uint8ClampedArray(width*height);
        for(let y=1; y<height-1; y++){
            for(let x=1; x<width-1; x++){
                let vals = [];
                for(let dy=-1;dy<=1;dy++) for(let dx=-1;dx<=1;dx++) vals.push(gray[(y+dy)*width+(x+dx)]);
                vals.sort((a,b)=>a-b);
                filtered[y*width+x] = vals[4];
            }
        }
        // 边缘填充
        for(let y=0;y<height;y++) for(let x=0;x<width;x++) if(y===0||y===height-1||x===0||x===width-1) filtered[y*width+x]=gray[y*width+x];
        return filtered;
    }

    function findNodulesRobust(imageData, width, height) {
        let gray = preprocessGray(imageData, width, height);
        // 自适应阈值: 全局均值的1.1倍 + 局部对比度
        let sum=0; for(let v of gray) sum+=v;
        let globalMean = sum/gray.length;
        let threshold = globalMean * 1.15;  // 高于平均亮度15% (结节通常较亮)
        let binary = new Uint8Array(width*height);
        for(let i=0;i<gray.length;i++) binary[i] = gray[i] > threshold ? 1 : 0;

        // 连通组件 (4邻域)
        let labels = new Int32Array(width*height).fill(0);
        let curLabel=1, eq=[];
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
        // 并查集合并
        let parent = new Array(curLabel).fill(0).map((_,i)=>i);
        function find(x){ while(parent[x]!==x){ parent[x]=parent[parent[x]]; x=parent[x]; } return x; }
        function union(a,b){ let ra=find(a),rb=find(b); if(ra!==rb) parent[rb]=ra; }
        for(let [a,b] of eq) union(a,b);
        for(let i=1;i<curLabel;i++) find(i);
        let newMap=new Map(), finalLabel=new Int32Array(width*height), nextL=1;
        for(let i=0;i<width*height;i++){
            let l=labels[i];
            if(l===0) continue;
            let root=parent[l];
            if(!newMap.has(root)) newMap.set(root, nextL++);
            finalLabel[i]=newMap.get(root);
        }

        // 统计区域
        let regions=new Map();
        for(let i=0;i<width*height;i++){
            let lab=finalLabel[i];
            if(lab===0) continue;
            if(!regions.has(lab)) regions.set(lab, {area:0, minX:width, minY:height, maxX:0, maxY:0, sumGray:0});
            let reg=regions.get(lab);
            reg.area++;
            let x=i%width, y=Math.floor(i/width);
            reg.minX=Math.min(reg.minX,x); reg.minY=Math.min(reg.minY,y);
            reg.maxX=Math.max(reg.maxX,x); reg.maxY=Math.max(reg.maxY,y);
            reg.sumGray += gray[i];
        }

        let nodules=[];
        // 预估肺野区域: 图像中央60%区域 (排除边缘肋骨、文字)
        let lungLeft = width*0.2, lungRight = width*0.8, lungTop = height*0.2, lungBottom = height*0.8;

        for(let [lab, reg] of regions.entries()){
            let area = reg.area;
            if(area < 15 || area > 700) continue;   // 结节面积范围
            let centerX = (reg.minX+reg.maxX)/2;
            let centerY = (reg.minY+reg.maxY)/2;
            // 排除太靠近边缘的假阳性 (文字、标尺、肋骨边缘)
            if(centerX < lungLeft || centerX > lungRight || centerY < lungTop || centerY > lungBottom) continue;
            
            let w = reg.maxX-reg.minX+1, h = reg.maxY-reg.minY+1;
            let diamPx = (w+h)/2;
            let physSize = diamPx * 0.42; // mm模拟
            // 圆形度 = 面积 / (π*(半径)^2)
            let radius = diamPx/2;
            let circularity = area / (Math.PI * radius * radius);
            // 离心率 (长宽比接近1越好)
            let aspect = Math.min(w,h)/Math.max(w,h);
            // 平均灰度对比 (结节应该比周围亮)
            let avgGray = reg.sumGray / area;
            let localBg = 0;
            let samp=0;
            for(let dy=-5;dy<=5;dy+=2){
                for(let dx=-5;dx<=5;dx+=2){
                    let nx = Math.min(width-1, Math.max(0, centerX+dx));
                    let ny = Math.min(height-1, Math.max(0, centerY+dy));
                    if(Math.hypot(nx-centerX, ny-centerY) > radius+3){
                        localBg += gray[ny*width+nx];
                        samp++;
                    }
                }
            }
            let bgMean = samp>0 ? localBg/samp : globalMean;
            let contrast = avgGray / (bgMean+0.1);
            // 严格筛选: 圆形度>0.52, 长宽比>0.55, 对比度>1.15, 面积适中
            if(circularity > 0.52 && aspect > 0.55 && contrast > 1.12 && area>=15){
                nodules.push({
                    x: centerX, y: centerY, radius: radius,
                    area: area, diameterMm: physSize,
                    circularity: circularity, contrast: contrast
                });
            }
        }
        // 非极大值抑制合并过近区域
        nodules.sort((a,b)=>b.area-a.area);
        let filtered=[];
        for(let n of nodules){
            let overlap=false;
            for(let ex of filtered){
                let dist = Math.hypot(ex.x-n.x, ex.y-n.y);
                if(dist < (ex.radius+n.radius)*0.6) { overlap=true; break; }
            }
            if(!overlap) filtered.push(n);
        }
        return filtered;
    }

    async function performAnalysis() {
        if(!currentImage){
            alert(currentLang==='zh'?'먼저 이미지를 업로드하세요 | 请先上传影像':'Please upload image');
            return;
        }
        const w = canvas.width, h = canvas.height;
        let imgData = ctx.getImageData(0,0,w,h);
        let nodules = findNodulesRobust(imgData, w, h);
        let maxDia = nodules.length>0 ? Math.max(...nodules.map(n=>n.diameterMm)) : 0;
        let riskScore = 0;
        if(nodules.length>0){
            let sizeFactor = Math.min(1.0, maxDia/16.0);
            let countFactor = Math.min(1.0, nodules.length/2.5);
            let circFactor = 0;
            for(let n of nodules) circFactor += (1 - n.circularity);
            circFactor = circFactor/nodules.length;
            riskScore = 0.45*sizeFactor + 0.35*countFactor + 0.2*Math.min(1, circFactor*1.5);
            riskScore = Math.min(0.96, riskScore);
        } else { riskScore = 0.02; }
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
            ctx.lineWidth = 2.8;
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
        if(currentImage) ctx.drawImage(currentImage,0,0,canvas.width,canvas.height);
        else { ctx.fillStyle="#d9e2df"; ctx.fillRect(0,0,canvas.width,canvas.height); }
        analysisData = { nodules:[], riskScore:0, maxDiameter:0 };
        updateMetricsUI();
        document.getElementById('noduleListPanel').innerHTML = `<div style="color:#65807b;">${trans[currentLang].noDataMsg}</div>`;
        document.getElementById('dynamicReport').innerHTML = `<i class="fas fa-info-circle"></i> ${trans[currentLang].reportPlaceholder}`;
        if(sizeChartInstance) { sizeChartInstance.destroy(); sizeChartInstance=null; }
        if(riskChartInstance) riskChartInstance.destroy();
        const riskC=document.getElementById('riskChart');
        if(riskC){ let rctx=riskC.getContext('2d'); rctx.clearRect(0,0,riskC.width,riskC.height); rctx.fillStyle='#ccc'; rctx.fillText('—', riskC.width/2, riskC.height/2); }
    }

    function handleImage(file){
        if(!file) return;
        let reader = new FileReader();
        reader.onload=(e)=>{
            let img=new Image();
            img.onload=()=>{
                currentImage=img;
                let maxW=500;
                let w=img.width, h=img.height;
                if(w>maxW){ h = h*(maxW/w); w=maxW; }
                canvas.width=w; canvas.height=h;
                ctx.drawImage(img,0,0,w,h);
                analysisData={ nodules:[], riskScore:0, maxDiameter:0 };
                updateMetricsUI();
                document.getElementById('noduleListPanel').innerHTML = `<div>${trans[currentLang].noDataMsg}</div>`;
                document.getElementById('dynamicReport').innerHTML = `<i class="fas fa-info-circle"></i> ${trans[currentLang].reportPlaceholder}`;
                if(sizeChartInstance) sizeChartInstance.destroy();
                if(riskChartInstance) riskChartInstance.destroy();
                sizeChartInstance=null; riskChartInstance=null;
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
    document.getElementById('analyzeBtn').addEventListener('click', performAnalysis);
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
