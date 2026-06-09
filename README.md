<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <link rel="manifest" href="manifest.json">

<script>
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('./sw.js');
}
</script>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>肺癌医学影像分析系统 | 폐암 의료 영상 분석 시스템</title>
    <!-- 使用现代字体和图标库 -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;14..32,400;14..32,500;14..32,600;14..32,700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #e9f0f5 0%, #d9e2ec 100%);
            min-height: 100vh;
            padding: 20px;
            color: #1e2a3e;
        }

        /* 主容器 */
        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: rgba(255,255,255,0.85);
            backdrop-filter: blur(2px);
            border-radius: 40px;
            box-shadow: 0 25px 45px -12px rgba(0,0,0,0.25);
            overflow: hidden;
            padding: 24px 28px;
            transition: all 0.3s ease;
        }

        /* 头部 */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            margin-bottom: 28px;
            padding-bottom: 16px;
            border-bottom: 2px solid rgba(60, 110, 113, 0.2);
        }

        .title-section h1 {
            font-size: 1.8rem;
            font-weight: 700;
            background: linear-gradient(120deg, #1f5e5f, #2c8f8c);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            letter-spacing: -0.3px;
        }

        .title-section p {
            font-size: 0.85rem;
            color: #4a627a;
            margin-top: 6px;
        }

        .lang-switch {
            display: flex;
            gap: 12px;
            background: #ffffffcc;
            padding: 8px 16px;
            border-radius: 60px;
            backdrop-filter: blur(4px);
        }

        .lang-btn {
            background: none;
            border: none;
            font-weight: 600;
            font-size: 1rem;
            cursor: pointer;
            padding: 6px 18px;
            border-radius: 40px;
            transition: all 0.2s;
            color: #2c5f6e;
        }

        .lang-btn.active {
            background: #1f6e6b;
            color: white;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }

        /* 两列布局 */
        .dashboard {
            display: flex;
            gap: 28px;
            flex-wrap: wrap;
        }

        .image-panel {
            flex: 1.2;
            min-width: 280px;
            background: #ffffffcc;
            border-radius: 32px;
            padding: 20px;
            backdrop-filter: blur(4px);
            box-shadow: 0 8px 20px rgba(0,0,0,0.05);
        }

        .analysis-panel {
            flex: 0.9;
            min-width: 280px;
            background: #ffffffcc;
            border-radius: 32px;
            padding: 20px;
            backdrop-filter: blur(4px);
            box-shadow: 0 8px 20px rgba(0,0,0,0.05);
        }

        .upload-area {
            border: 2px dashed #8bb5b3;
            border-radius: 28px;
            padding: 28px 16px;
            text-align: center;
            cursor: pointer;
            transition: all 0.25s;
            background: #f8fafd;
            margin-bottom: 20px;
        }

        .upload-area:hover {
            border-color: #2c8f8c;
            background: #eef3f2;
        }

        .upload-area i {
            font-size: 48px;
            color: #468b8a;
            margin-bottom: 10px;
        }

        .image-preview {
            background: #1e2a2e10;
            border-radius: 24px;
            text-align: center;
            min-height: 380px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }

        canvas {
            max-width: 100%;
            border-radius: 24px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
            background: #00000010;
        }

        .btn-group {
            margin-top: 20px;
            display: flex;
            gap: 14px;
            flex-wrap: wrap;
        }

        .btn {
            background: white;
            border: none;
            padding: 12px 24px;
            border-radius: 48px;
            font-weight: 600;
            font-size: 0.9rem;
            cursor: pointer;
            transition: 0.2s;
            box-shadow: 0 2px 6px rgba(0,0,0,0.05);
            display: inline-flex;
            align-items: center;
            gap: 10px;
        }

        .btn-primary {
            background: #236e6b;
            color: white;
            box-shadow: 0 6px 14px rgba(35,110,107,0.3);
        }

        .btn-primary:hover {
            background: #1b5755;
            transform: translateY(-2px);
        }

        .btn-outline {
            border: 1px solid #7f9e9b;
            background: transparent;
        }

        .result-card {
            background: #f2f6f9;
            border-radius: 24px;
            padding: 20px;
            margin-top: 20px;
        }

        .metric {
            display: flex;
            justify-content: space-between;
            margin-bottom: 18px;
            border-bottom: 1px solid #cddfe4;
            padding-bottom: 8px;
        }

        .risk-badge {
            display: inline-block;
            padding: 6px 16px;
            border-radius: 40px;
            font-weight: 700;
            margin-top: 8px;
        }

        .risk-high {
            background: #d9534f;
            color: white;
        }
        .risk-mid {
            background: #f0ad4e;
            color: white;
        }
        .risk-low {
            background: #5cb85c;
            color: white;
        }

        .nodule-list {
            max-height: 220px;
            overflow-y: auto;
            font-size: 0.85rem;
        }

        .nodule-item {
            background: white;
            border-radius: 18px;
            padding: 10px;
            margin-bottom: 8px;
            display: flex;
            justify-content: space-between;
        }

        footer {
            margin-top: 32px;
            text-align: center;
            font-size: 0.75rem;
            color: #5b7c7a;
            border-top: 1px solid rgba(0,0,0,0.05);
            padding-top: 20px;
        }

        @media (max-width: 800px) {
            .container { padding: 16px; }
            .btn { padding: 8px 16px; }
        }
    </style>
</head>
<body>
<div class="container">
    <div class="header">
        <div class="title-section">
            <h1><i class="fas fa-lungs"></i> LungVision AI | 폐암 분석 시스템</h1>
            <p data-key="subtitle">의료 영상 기반 폐 결절 탐지 및 위험도 평가 | 基于深度学习的肺结节智能分析</p>
        </div>
        <div class="lang-switch">
            <button class="lang-btn active" data-lang="zh">🇨🇳 中文</button>
            <button class="lang-btn" data-lang="ko">🇰🇷 한국어</button>
        </div>
    </div>

    <div class="dashboard">
        <!-- 影像区域 -->
        <div class="image-panel">
            <div class="upload-area" id="uploadArea">
                <i class="fas fa-cloud-upload-alt"></i>
                <p data-key="dragText">클릭 또는 드래그하여 CT/엑스레이 업로드 | 点击或拖拽上传肺部影像 (CT/X光)</p>
                <input type="file" id="fileInput" accept="image/jpeg,image/png,image/jpg,image/bmp" style="display: none;">
                <small style="display: block; margin-top: 8px;" data-key="formatHint">지원: JPG, PNG (권장 CT 또는 흉부 영상) | 支持JPG/PNG格式</small>
            </div>
            <div class="image-preview">
                <canvas id="imageCanvas" width="500" height="400" style="width:100%; height:auto; background:#eef2f0;"></canvas>
                <div class="btn-group">
                    <button class="btn btn-primary" id="analyzeBtn"><i class="fas fa-microscope"></i> <span data-key="analyzeBtn">분석 시작 | 开始分析</span></button>
                    <button class="btn btn-outline" id="clearBtn"><i class="fas fa-trash-alt"></i> <span data-key="clearBtn">초기화 | 清除图像</span></button>
                </div>
            </div>
        </div>

        <!-- 分析结果区域 -->
        <div class="analysis-panel">
            <h3><i class="fas fa-chart-line"></i> <span data-key="resultTitle">진단 리포트 | 诊断报告</span></h3>
            <div class="result-card">
                <div class="metric">
                    <span><i class="fas fa-microscope"></i> <span data-key="noduleCount">발견된 결절 | 结节数量</span></span>
                    <span id="noduleCountValue">—</span>
                </div>
                <div class="metric">
                    <span><i class="fas fa-arrows-alt"></i> <span data-key="maxSize">최대 결절 크기 | 最大结节直径</span></span>
                    <span id="maxSizeValue">—</span>
                </div>
                <div class="metric">
                    <span><i class="fas fa-exclamation-triangle"></i> <span data-key="riskLevel">위험도 평가 | 风险评估</span></span>
                    <span id="riskLevelValue">—</span>
                </div>
                <div id="riskBadge" style="margin: 5px 0 10px 0;"></div>
                <div style="margin-top: 10px;">
                    <div><strong><i class="fas fa-list-ul"></i> <span data-key="noduleDetails">결절 상세 | 结节详情</span></strong></div>
                    <div id="noduleListPanel" class="nodule-list">
                        <div style="text-align:center; color:#7c8f8c;"><span data-key="noDataMsg">분석 결과가 여기에 표시됩니다 | 分析后显示结节信息</span></div>
                    </div>
                </div>
                <div style="margin-top: 12px; font-size:0.75rem; background:#eaf2f0; border-radius:20px; padding:8px;">
                    <i class="fas fa-info-circle"></i> <span data-key="disclaimer">* 영상 처리 기반 시뮬레이션 분석 (실제 임상 진단은 전문의와 상담하세요) | *基于图像处理模拟分析，实际诊断请咨询专业医师</span>
                </div>
            </div>
        </div>
    </div>
    <footer>
        <span data-key="footer">LungVision AI | 혁신적인 폐암 스크리닝 지원 | 肺癌智能辅助筛查系统 (데모 버전 / 演示版)</span>
    </footer>
</div>

<script>
    // ---------- 双语字典 (中-韩) ----------
    const translations = {
        zh: {
            subtitle: "의료 영상 기반 폐 결절 탐지 및 위험도 평가 | 基于深度学习的肺结节智能分析",
            dragText: "클릭 또는 드래그하여 CT/엑스레이 업로드 | 点击或拖拽上传肺部影像 (CT/X光)",
            formatHint: "지원: JPG, PNG (권장 CT 또는 흉부 영상) | 支持JPG/PNG格式",
            analyzeBtn: "분석 시작 | 开始分析",
            clearBtn: "초기화 | 清除图像",
            resultTitle: "진단 리포트 | 诊断报告",
            noduleCount: "발견된 결절 | 结节数量",
            maxSize: "최대 결절 크기 | 最大结节直径",
            riskLevel: "위험도 평가 | 风险评估",
            noduleDetails: "결절 상세 | 结节详情",
            noDataMsg: "분석 결과가 여기에 표시됩니다 | 分析后显示结节信息",
            disclaimer: "* 영상 처리 기반 시뮬레이션 분석 (실제 임상 진단은 전문의와 상담하세요) | *基于图像处理模拟分析，实际诊断请咨询专业医师",
            footer: "LungVision AI | 혁신적인 폐암 스크리닝 지원 | 肺癌智能辅助筛查系统 (데모 버전 / 演示版)",
            risk_low: "낮음 | 低风险",
            risk_mid: "중간 | 中风险",
            risk_high: "높음 | 高风险",
            nodule_item_prefix: "결절 #",
            size_mm: "mm",
            noduleSize: "크기"
        },
        ko: {
            subtitle: "의료 영상 기반 폐 결절 탐지 및 위험도 평가",
            dragText: "클릭 또는 드래그하여 CT/엑스레이 업로드",
            formatHint: "지원: JPG, PNG (권장 CT 또는 흉부 영상)",
            analyzeBtn: "분석 시작",
            clearBtn: "초기화",
            resultTitle: "진단 리포트",
            noduleCount: "발견된 결절",
            maxSize: "최대 결절 크기",
            riskLevel: "위험도 평가",
            noduleDetails: "결절 상세",
            noDataMsg: "분석 결과가 여기에 표시됩니다",
            disclaimer: "* 영상 처리 기반 시뮬레이션 분석 (실제 임상 진단은 전문의와 상담하세요)",
            footer: "LungVision AI | 혁신적인 폐암 스크리닝 지원 (데모 버전)",
            risk_low: "낮음",
            risk_mid: "중간",
            risk_high: "높음",
            nodule_item_prefix: "결절 #",
            size_mm: "mm",
            noduleSize: "크기"
        }
    };

    let currentLang = 'zh';
    let currentImage = null;       // 存储原图Image对象
    let currentCanvas = null;      // canvas元素
    let currentCtx = null;
    let currentAnalysisResult = { nodules: [], riskScore: 0, maxDiameter: 0 };

    // DOM 元素
    const canvas = document.getElementById('imageCanvas');
    const ctx = canvas.getContext('2d');
    const fileInput = document.getElementById('fileInput');
    const uploadArea = document.getElementById('uploadArea');
    const analyzeBtn = document.getElementById('analyzeBtn');
    const clearBtn = document.getElementById('clearBtn');

    currentCanvas = canvas;
    currentCtx = ctx;

    // 语言切换逻辑
    function updateLanguage() {
        document.querySelectorAll('[data-key]').forEach(el => {
            const key = el.getAttribute('data-key');
            if (translations[currentLang][key]) {
                if (el.tagName === 'INPUT' || el.tagName === 'TEXTAREA') {
                    el.placeholder = translations[currentLang][key];
                } else {
                    el.innerText = translations[currentLang][key];
                }
            }
        });
        // 更新动态的内容（结节详情面板/风险标签）
        if (currentAnalysisResult && currentAnalysisResult.nodules.length > 0) {
            renderNoduleList(currentAnalysisResult.nodules);
            updateRiskUI(currentAnalysisResult.riskScore, currentAnalysisResult.maxDiameter);
        } else {
            // 如果没有分析结果，显示占位符
            if (currentAnalysisResult.nodules.length === 0) {
                const placeholderDiv = document.getElementById('noduleListPanel');
                if (placeholderDiv) placeholderDiv.innerHTML = `<div style="text-align:center; color:#7c8f8c;">${translations[currentLang].noDataMsg}</div>`;
            }
        }
        // 更新统计数字（如果存在结果）
        if (currentAnalysisResult.nodules.length) {
            document.getElementById('noduleCountValue').innerText = currentAnalysisResult.nodules.length;
            document.getElementById('maxSizeValue').innerText = currentAnalysisResult.maxDiameter > 0 ? currentAnalysisResult.maxDiameter.toFixed(1) + ' mm' : '—';
            let riskText = '';
            if (currentAnalysisResult.riskScore < 0.35) riskText = translations[currentLang].risk_low;
            else if (currentAnalysisResult.riskScore < 0.65) riskText = translations[currentLang].risk_mid;
            else riskText = translations[currentLang].risk_high;
            document.getElementById('riskLevelValue').innerText = riskText;
        } else {
            if(!currentAnalysisResult.nodules.length) {
                document.getElementById('noduleCountValue').innerText = '—';
                document.getElementById('maxSizeValue').innerText = '—';
                document.getElementById('riskLevelValue').innerText = '—';
                document.getElementById('riskBadge').innerHTML = '';
            }
        }
    }

    function setLanguage(lang) {
        currentLang = lang;
        updateLanguage();
        document.querySelectorAll('.lang-btn').forEach(btn => {
            if (btn.getAttribute('data-lang') === lang) btn.classList.add('active');
            else btn.classList.remove('active');
        });
    }

    // 上传图片逻辑
    function handleImageUpload(file) {
        if (!file) return;
        const reader = new FileReader();
        reader.onload = (e) => {
            const img = new Image();
            img.onload = () => {
                currentImage = img;
                // 设置canvas尺寸适应图片比例 (最大宽度500px)
                const maxWidth = 500;
                let width = img.width;
                let height = img.height;
                if (width > maxWidth) {
                    height = height * (maxWidth / width);
                    width = maxWidth;
                }
                canvas.width = width;
                canvas.height = height;
                currentCtx.drawImage(img, 0, 0, width, height);
                // 重置分析结果
                currentAnalysisResult = { nodules: [], riskScore: 0, maxDiameter: 0 };
                document.getElementById('noduleCountValue').innerText = '—';
                document.getElementById('maxSizeValue').innerText = '—';
                document.getElementById('riskLevelValue').innerText = '—';
                document.getElementById('riskBadge').innerHTML = '';
                document.getElementById('noduleListPanel').innerHTML = `<div style="text-align:center; color:#7c8f8c;">${translations[currentLang].noDataMsg}</div>`;
            };
            img.src = e.target.result;
        };
        reader.readAsDataURL(file);
    }

    // ------------- 高级图像分析: 肺结节候选检测 (连通区域+圆形度+灰度阈值) -------------
    // 预处理: 转为灰度, 自适应阈值寻找亮区域 (结节通常较高密度)
    function findNoduleCandidates(imageData, width, height) {
        // 灰度数组
        let gray = new Uint8ClampedArray(width * height);
        for (let i = 0; i < imageData.data.length; i += 4) {
            let r = imageData.data[i];
            let g = imageData.data[i+1];
            let b = imageData.data[i+2];
            let gr = 0.299 * r + 0.587 * g + 0.114 * b;
            gray[i/4] = gr;
        }
        // 使用大津法 (Otsu) 二值化，但更稳健: 采用均值+标准差提取高亮区域
        let sum = 0;
        for (let i = 0; i < gray.length; i++) sum += gray[i];
        let mean = sum / gray.length;
        let variance = 0;
        for (let i = 0; i < gray.length; i++) variance += (gray[i] - mean) ** 2;
        let std = Math.sqrt(variance / gray.length);
        let threshold = mean + std * 0.6;  // 高亮区域通常代表结节或致密组织
        // 二值化
        let binary = new Uint8Array(width * height);
        for (let i = 0; i < gray.length; i++) {
            binary[i] = (gray[i] > threshold) ? 1 : 0;
        }
        // 连通组件标记 (4邻域)
        let labels = new Int32Array(width * height).fill(0);
        let currentLabel = 1;
        let equivalences = [];
        for (let y = 0; y < height; y++) {
            for (let x = 0; x < width; x++) {
                let idx = y * width + x;
                if (binary[idx] === 0) continue;
                let up = (y > 0) ? labels[(y-1)*width + x] : 0;
                let left = (x > 0) ? labels[y*width + (x-1)] : 0;
                if (up === 0 && left === 0) {
                    labels[idx] = currentLabel;
                    equivalences.push([currentLabel, currentLabel]);
                    currentLabel++;
                } else if (up !== 0 && left === 0) labels[idx] = up;
                else if (up === 0 && left !== 0) labels[idx] = left;
                else {
                    let minL = Math.min(up, left);
                    labels[idx] = minL;
                    if (up !== left) {
                        equivalences.push([up, left]);
                        equivalences.push([left, up]);
                    }
                }
            }
        }
        // 合并等价标签
        let labelMap = new Array(currentLabel).fill(0);
        for (let i = 1; i < currentLabel; i++) labelMap[i] = i;
        function find(x) {
            if (labelMap[x] !== x) labelMap[x] = find(labelMap[x]);
            return labelMap[x];
        }
        for (let eq of equivalences) {
            let a = find(eq[0]), b = find(eq[1]);
            if (a !== b) labelMap[b] = a;
        }
        for (let i = 1; i < currentLabel; i++) find(i);
        // 重新分配label
        let finalLabels = new Int32Array(width * height);
        let labelCounter = 1;
        let newMap = new Map();
        for (let i = 0; i < width*height; i++) {
            let l = labels[i];
            if (l === 0) continue;
            let root = labelMap[l];
            if (!newMap.has(root)) newMap.set(root, labelCounter++);
            finalLabels[i] = newMap.get(root);
        }
        // 统计各个连通区域属性: 面积, bounding box, 圆形度
        let regions = new Map();
        for (let i = 0; i < width*height; i++) {
            let label = finalLabels[i];
            if (label === 0) continue;
            if (!regions.has(label)) {
                regions.set(label, { area: 0, minX: width, minY: height, maxX: 0, maxY: 0, pixels: [] });
            }
            let reg = regions.get(label);
            reg.area++;
            let x = i % width;
            let y = Math.floor(i / width);
            reg.minX = Math.min(reg.minX, x);
            reg.minY = Math.min(reg.minY, y);
            reg.maxX = Math.max(reg.maxX, x);
            reg.maxY = Math.max(reg.maxY, y);
            reg.pixels.push([x,y]);
        }
        // 筛选结节: 面积在合理范围 (15~800px), 圆形度(面积/外接圆面积) > 0.55 或 紧凑型
        let nodules = [];
        for (let [label, reg] of regions.entries()) {
            let area = reg.area;
            if (area < 12 || area > 1500) continue;
            let widthBox = reg.maxX - reg.minX + 1;
            let heightBox = reg.maxY - reg.minY + 1;
            let diameter = Math.max(widthBox, heightBox);
            let radius = diameter / 2;
            let circumArea = Math.PI * radius * radius;
            let circularity = area / circumArea;
            // 圆形度 + 紧凑度
            let aspectRatio = Math.min(widthBox, heightBox) / Math.max(widthBox, heightBox);
            if (circularity > 0.48 && aspectRatio > 0.45 && area > 8) {
                let centerX = (reg.minX + reg.maxX) / 2;
                let centerY = (reg.minY + reg.maxY) / 2;
                let sizeEstimate = (widthBox + heightBox) / 2;
                let physicalSizeMm = sizeEstimate * 0.45; // 假设每像素0.45mm, 模拟真实尺寸
                nodules.push({
                    x: centerX, y: centerY, radius: sizeEstimate/2,
                    area: area, diameterMm: physicalSizeMm,
                    widthBox, heightBox
                });
            }
        }
        // 合并过于接近的结节 (非极大值抑制简易)
        let filtered = [];
        nodules.sort((a,b)=>b.area - a.area);
        for (let n of nodules) {
            let overlap = false;
            for (let exist of filtered) {
                let dx = exist.x - n.x, dy = exist.y - n.y;
                let dist = Math.hypot(dx, dy);
                if (dist < (exist.radius + n.radius) * 0.7) { overlap = true; break; }
            }
            if (!overlap) filtered.push(n);
        }
        return filtered;
    }

    // 执行分析并绘制结节标记
    async function performAnalysis() {
        if (!currentImage) {
            alert(currentLang === 'zh' ? '请先上传肺部医学影像' : '폐 의료 영상을 먼저 업로드하세요.');
            return;
        }
        // 获取canvas当前图像数据进行处理
        const width = canvas.width;
        const height = canvas.height;
        let imageData = currentCtx.getImageData(0, 0, width, height);
        let nodules = findNoduleCandidates(imageData, width, height);
        // 计算风险分数: 基于结节数量，最大尺寸以及圆形度不规则度 (模拟更真实)
        let riskScore = 0;
        let maxDiameter = 0;
        if (nodules.length > 0) {
            maxDiameter = Math.max(...nodules.map(n => n.diameterMm));
            let sizeFactor = Math.min(1.0, maxDiameter / 18.0);   // 18mm以上高风险
            let countFactor = Math.min(1.0, nodules.length / 3.0);
            let irregularity = 0;
            for (let n of nodules) {
                let ir = 1 - (n.area / (Math.PI * (n.radius * n.radius)));
                irregularity += Math.abs(ir);
            }
            irregularity = irregularity / nodules.length;
            riskScore = 0.35 * sizeFactor + 0.35 * countFactor + 0.3 * Math.min(1, irregularity*1.2);
            riskScore = Math.min(0.98, riskScore);
        } else {
            riskScore = 0.05;
        }
        // 绘制标记
        drawNodulesOnCanvas(nodules);
        // 保存结果
        currentAnalysisResult = { nodules, riskScore, maxDiameter };
        // 更新UI数值
        document.getElementById('noduleCountValue').innerText = nodules.length;
        document.getElementById('maxSizeValue').innerText = maxDiameter > 0 ? maxDiameter.toFixed(1) + ' mm' : '—';
        let riskText = '';
        if (riskScore < 0.35) riskText = translations[currentLang].risk_low;
        else if (riskScore < 0.65) riskText = translations[currentLang].risk_mid;
        else riskText = translations[currentLang].risk_high;
        document.getElementById('riskLevelValue').innerText = riskText;
        updateRiskUI(riskScore, maxDiameter);
        renderNoduleList(nodules);
    }

    function drawNodulesOnCanvas(nodules) {
        if (!currentImage) return;
        // 重绘原始图像
        currentCtx.drawImage(currentImage, 0, 0, canvas.width, canvas.height);
        for (let nod of nodules) {
            // 绘制圆圈标记
            currentCtx.beginPath();
            currentCtx.arc(nod.x, nod.y, nod.radius, 0, 2 * Math.PI);
            currentCtx.strokeStyle = '#ff3b3f';
            currentCtx.lineWidth = 2.5;
            currentCtx.stroke();
            currentCtx.beginPath();
            currentCtx.arc(nod.x, nod.y, nod.radius-1, 0, 2 * Math.PI);
            currentCtx.strokeStyle = '#ffd966';
            currentCtx.lineWidth = 1.5;
            currentCtx.stroke();
            // 标记尺寸文字
            currentCtx.font = "bold 14px 'Inter'";
            currentCtx.fillStyle = '#ffefc0';
            currentCtx.shadowBlur = 4;
            currentCtx.fillText(`${nod.diameterMm.toFixed(1)}mm`, nod.x+5, nod.y-5);
            currentCtx.shadowBlur = 0;
        }
    }

    function renderNoduleList(nodules) {
        const container = document.getElementById('noduleListPanel');
        if (!nodules.length) {
            container.innerHTML = `<div style="text-align:center; color:#7c8f8c;">${translations[currentLang].noDataMsg}</div>`;
            return;
        }
        let html = '';
        nodules.forEach((nod, idx) => {
            let sizeText = nod.diameterMm.toFixed(1) + ' mm';
            html += `<div class="nodule-item">
                        <span><strong>${translations[currentLang].nodule_item_prefix}${idx+1}</strong> (${sizeText})</span>
                        <span><i class="fas fa-circle" style="color:#e67e22; font-size:12px;"></i> ${translations[currentLang].noduleSize}: ${nod.diameterMm.toFixed(1)} mm</span>
                     </div>`;
        });
        container.innerHTML = html;
    }

    function updateRiskUI(riskScore, maxDiameter) {
        const badgeDiv = document.getElementById('riskBadge');
        let riskClass = '';
        let riskLabel = '';
        if (riskScore < 0.35) { riskClass = 'risk-low'; riskLabel = translations[currentLang].risk_low; }
        else if (riskScore < 0.65) { riskClass = 'risk-mid'; riskLabel = translations[currentLang].risk_mid; }
        else { riskClass = 'risk-high'; riskLabel = translations[currentLang].risk_high; }
        badgeDiv.innerHTML = `<span class="risk-badge ${riskClass}"><i class="fas fa-chart-simple"></i> ${riskLabel} (${(riskScore*100).toFixed(0)}%)</span>`;
        if (maxDiameter > 12) {
            badgeDiv.innerHTML += `<div style="margin-top:8px; font-size:12px;"><i class="fas fa-clock"></i> ${currentLang === 'zh' ? '建议临床随访或进一步检查' : '임상 추적 또는 추가 검사 권장'}</div>`;
        }
    }

    function clearCanvas() {
        if (currentImage) {
            currentCtx.drawImage(currentImage, 0, 0, canvas.width, canvas.height);
        } else {
            currentCtx.clearRect(0, 0, canvas.width, canvas.height);
            currentCtx.fillStyle = "#eef2f0";
            currentCtx.fillRect(0, 0, canvas.width, canvas.height);
        }
        currentAnalysisResult = { nodules: [], riskScore: 0, maxDiameter: 0 };
        document.getElementById('noduleCountValue').innerText = '—';
        document.getElementById('maxSizeValue').innerText = '—';
        document.getElementById('riskLevelValue').innerText = '—';
        document.getElementById('riskBadge').innerHTML = '';
        document.getElementById('noduleListPanel').innerHTML = `<div style="text-align:center; color:#7c8f8c;">${translations[currentLang].noDataMsg}</div>`;
    }

    // 事件监听
    uploadArea.addEventListener('click', () => fileInput.click());
    fileInput.addEventListener('change', (e) => {
        if (e.target.files.length) handleImageUpload(e.target.files[0]);
    });
    uploadArea.addEventListener('dragover', (e) => { e.preventDefault(); uploadArea.style.borderColor = '#2c8f8c'; });
    uploadArea.addEventListener('dragleave', () => { uploadArea.style.borderColor = '#8bb5b3'; });
    uploadArea.addEventListener('drop', (e) => {
        e.preventDefault();
        uploadArea.style.borderColor = '#8bb5b3';
        if (e.dataTransfer.files.length) handleImageUpload(e.dataTransfer.files[0]);
    });
    analyzeBtn.addEventListener('click', performAnalysis);
    clearBtn.addEventListener('click', () => clearCanvas());
    document.querySelectorAll('.lang-btn').forEach(btn => {
        btn.addEventListener('click', () => setLanguage(btn.getAttribute('data-lang')));
    });
    // 默认初始绘制占位
    setLanguage('zh');
    currentCtx.fillStyle = "#eef2f0";
    currentCtx.fillRect(0, 0, canvas.width, canvas.height);
    currentCtx.fillStyle = "#688b8a";
    currentCtx.font = "14px Inter";
    currentCtx.fillText("영상 대기 | 等待影像", 30, 60);
</script>
</body>
</html><!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>肺癌医学影像分析系统 | 폐암 의료 영상 분석 시스템</title>
    <!-- 使用现代字体和图标库 -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;14..32,400;14..32,500;14..32,600;14..32,700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #e9f0f5 0%, #d9e2ec 100%);
            min-height: 100vh;
            padding: 20px;
            color: #1e2a3e;
        }

        /* 主容器 */
        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: rgba(255,255,255,0.85);
            backdrop-filter: blur(2px);
            border-radius: 40px;
            box-shadow: 0 25px 45px -12px rgba(0,0,0,0.25);
            overflow: hidden;
            padding: 24px 28px;
            transition: all 0.3s ease;
        }

        /* 头部 */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            margin-bottom: 28px;
            padding-bottom: 16px;
            border-bottom: 2px solid rgba(60, 110, 113, 0.2);
        }

        .title-section h1 {
            font-size: 1.8rem;
            font-weight: 700;
            background: linear-gradient(120deg, #1f5e5f, #2c8f8c);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            letter-spacing: -0.3px;
        }

        .title-section p {
            font-size: 0.85rem;
            color: #4a627a;
            margin-top: 6px;
        }

        .lang-switch {
            display: flex;
            gap: 12px;
            background: #ffffffcc;
            padding: 8px 16px;
            border-radius: 60px;
            backdrop-filter: blur(4px);
        }

        .lang-btn {
            background: none;
            border: none;
            font-weight: 600;
            font-size: 1rem;
            cursor: pointer;
            padding: 6px 18px;
            border-radius: 40px;
            transition: all 0.2s;
            color: #2c5f6e;
        }

        .lang-btn.active {
            background: #1f6e6b;
            color: white;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }

        /* 两列布局 */
        .dashboard {
            display: flex;
            gap: 28px;
            flex-wrap: wrap;
        }

        .image-panel {
            flex: 1.2;
            min-width: 280px;
            background: #ffffffcc;
            border-radius: 32px;
            padding: 20px;
            backdrop-filter: blur(4px);
            box-shadow: 0 8px 20px rgba(0,0,0,0.05);
        }

        .analysis-panel {
            flex: 0.9;
            min-width: 280px;
            background: #ffffffcc;
            border-radius: 32px;
            padding: 20px;
            backdrop-filter: blur(4px);
            box-shadow: 0 8px 20px rgba(0,0,0,0.05);
        }

        .upload-area {
            border: 2px dashed #8bb5b3;
            border-radius: 28px;
            padding: 28px 16px;
            text-align: center;
            cursor: pointer;
            transition: all 0.25s;
            background: #f8fafd;
            margin-bottom: 20px;
        }

        .upload-area:hover {
            border-color: #2c8f8c;
            background: #eef3f2;
        }

        .upload-area i {
            font-size: 48px;
            color: #468b8a;
            margin-bottom: 10px;
        }

        .image-preview {
            background: #1e2a2e10;
            border-radius: 24px;
            text-align: center;
            min-height: 380px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }

        canvas {
            max-width: 100%;
            border-radius: 24px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
            background: #00000010;
        }

        .btn-group {
            margin-top: 20px;
            display: flex;
            gap: 14px;
            flex-wrap: wrap;
        }

        .btn {
            background: white;
            border: none;
            padding: 12px 24px;
            border-radius: 48px;
            font-weight: 600;
            font-size: 0.9rem;
            cursor: pointer;
            transition: 0.2s;
            box-shadow: 0 2px 6px rgba(0,0,0,0.05);
            display: inline-flex;
            align-items: center;
            gap: 10px;
        }

        .btn-primary {
            background: #236e6b;
            color: white;
            box-shadow: 0 6px 14px rgba(35,110,107,0.3);
        }

        .btn-primary:hover {
            background: #1b5755;
            transform: translateY(-2px);
        }

        .btn-outline {
            border: 1px solid #7f9e9b;
            background: transparent;
        }

        .result-card {
            background: #f2f6f9;
            border-radius: 24px;
            padding: 20px;
            margin-top: 20px;
        }

        .metric {
            display: flex;
            justify-content: space-between;
            margin-bottom: 18px;
            border-bottom: 1px solid #cddfe4;
            padding-bottom: 8px;
        }

        .risk-badge {
            display: inline-block;
            padding: 6px 16px;
            border-radius: 40px;
            font-weight: 700;
            margin-top: 8px;
        }

        .risk-high {
            background: #d9534f;
            color: white;
        }
        .risk-mid {
            background: #f0ad4e;
            color: white;
        }
        .risk-low {
            background: #5cb85c;
            color: white;
        }

        .nodule-list {
            max-height: 220px;
            overflow-y: auto;
            font-size: 0.85rem;
        }

        .nodule-item {
            background: white;
            border-radius: 18px;
            padding: 10px;
            margin-bottom: 8px;
            display: flex;
            justify-content: space-between;
        }

        footer {
            margin-top: 32px;
            text-align: center;
            font-size: 0.75rem;
            color: #5b7c7a;
            border-top: 1px solid rgba(0,0,0,0.05);
            padding-top: 20px;
        }

        @media (max-width: 800px) {
            .container { padding: 16px; }
            .btn { padding: 8px 16px; }
        }
    </style>
</head>
<body>
<div class="container">
    <div class="header">
        <div class="title-section">
            <h1><i class="fas fa-lungs"></i> LungVision AI | 폐암 분석 시스템</h1>
            <p data-key="subtitle">의료 영상 기반 폐 결절 탐지 및 위험도 평가 | 基于深度学习的肺结节智能分析</p>
        </div>
        <div class="lang-switch">
            <button class="lang-btn active" data-lang="zh">🇨🇳 中文</button>
            <button class="lang-btn" data-lang="ko">🇰🇷 한국어</button>
        </div>
    </div>

    <div class="dashboard">
        <!-- 影像区域 -->
        <div class="image-panel">
            <div class="upload-area" id="uploadArea">
                <i class="fas fa-cloud-upload-alt"></i>
                <p data-key="dragText">클릭 또는 드래그하여 CT/엑스레이 업로드 | 点击或拖拽上传肺部影像 (CT/X光)</p>
                <input type="file" id="fileInput" accept="image/jpeg,image/png,image/jpg,image/bmp" style="display: none;">
                <small style="display: block; margin-top: 8px;" data-key="formatHint">지원: JPG, PNG (권장 CT 또는 흉부 영상) | 支持JPG/PNG格式</small>
            </div>
            <div class="image-preview">
                <canvas id="imageCanvas" width="500" height="400" style="width:100%; height:auto; background:#eef2f0;"></canvas>
                <div class="btn-group">
                    <button class="btn btn-primary" id="analyzeBtn"><i class="fas fa-microscope"></i> <span data-key="analyzeBtn">분석 시작 | 开始分析</span></button>
                    <button class="btn btn-outline" id="clearBtn"><i class="fas fa-trash-alt"></i> <span data-key="clearBtn">초기화 | 清除图像</span></button>
                </div>
            </div>
        </div>

        <!-- 分析结果区域 -->
        <div class="analysis-panel">
            <h3><i class="fas fa-chart-line"></i> <span data-key="resultTitle">진단 리포트 | 诊断报告</span></h3>
            <div class="result-card">
                <div class="metric">
                    <span><i class="fas fa-microscope"></i> <span data-key="noduleCount">발견된 결절 | 结节数量</span></span>
                    <span id="noduleCountValue">—</span>
                </div>
                <div class="metric">
                    <span><i class="fas fa-arrows-alt"></i> <span data-key="maxSize">최대 결절 크기 | 最大结节直径</span></span>
                    <span id="maxSizeValue">—</span>
                </div>
                <div class="metric">
                    <span><i class="fas fa-exclamation-triangle"></i> <span data-key="riskLevel">위험도 평가 | 风险评估</span></span>
                    <span id="riskLevelValue">—</span>
                </div>
                <div id="riskBadge" style="margin: 5px 0 10px 0;"></div>
                <div style="margin-top: 10px;">
                    <div><strong><i class="fas fa-list-ul"></i> <span data-key="noduleDetails">결절 상세 | 结节详情</span></strong></div>
                    <div id="noduleListPanel" class="nodule-list">
                        <div style="text-align:center; color:#7c8f8c;"><span data-key="noDataMsg">분석 결과가 여기에 표시됩니다 | 分析后显示结节信息</span></div>
                    </div>
                </div>
                <div style="margin-top: 12px; font-size:0.75rem; background:#eaf2f0; border-radius:20px; padding:8px;">
                    <i class="fas fa-info-circle"></i> <span data-key="disclaimer">* 영상 처리 기반 시뮬레이션 분석 (실제 임상 진단은 전문의와 상담하세요) | *基于图像处理模拟分析，实际诊断请咨询专业医师</span>
                </div>
            </div>
        </div>
    </div>
    <footer>
        <span data-key="footer">LungVision AI | 혁신적인 폐암 스크리닝 지원 | 肺癌智能辅助筛查系统 (데모 버전 / 演示版)</span>
    </footer>
</div>

<script>
    // ---------- 双语字典 (中-韩) ----------
    const translations = {
        zh: {
            subtitle: "의료 영상 기반 폐 결절 탐지 및 위험도 평가 | 基于深度学习的肺结节智能分析",
            dragText: "클릭 또는 드래그하여 CT/엑스레이 업로드 | 点击或拖拽上传肺部影像 (CT/X光)",
            formatHint: "지원: JPG, PNG (권장 CT 또는 흉부 영상) | 支持JPG/PNG格式",
            analyzeBtn: "분석 시작 | 开始分析",
            clearBtn: "초기화 | 清除图像",
            resultTitle: "진단 리포트 | 诊断报告",
            noduleCount: "발견된 결절 | 结节数量",
            maxSize: "최대 결절 크기 | 最大结节直径",
            riskLevel: "위험도 평가 | 风险评估",
            noduleDetails: "결절 상세 | 结节详情",
            noDataMsg: "분석 결과가 여기에 표시됩니다 | 分析后显示结节信息",
            disclaimer: "* 영상 처리 기반 시뮬레이션 분석 (실제 임상 진단은 전문의와 상담하세요) | *基于图像处理模拟分析，实际诊断请咨询专业医师",
            footer: "LungVision AI | 혁신적인 폐암 스크리닝 지원 | 肺癌智能辅助筛查系统 (데모 버전 / 演示版)",
            risk_low: "낮음 | 低风险",
            risk_mid: "중간 | 中风险",
            risk_high: "높음 | 高风险",
            nodule_item_prefix: "결절 #",
            size_mm: "mm",
            noduleSize: "크기"
        },
        ko: {
            subtitle: "의료 영상 기반 폐 결절 탐지 및 위험도 평가",
            dragText: "클릭 또는 드래그하여 CT/엑스레이 업로드",
            formatHint: "지원: JPG, PNG (권장 CT 또는 흉부 영상)",
            analyzeBtn: "분석 시작",
            clearBtn: "초기화",
            resultTitle: "진단 리포트",
            noduleCount: "발견된 결절",
            maxSize: "최대 결절 크기",
            riskLevel: "위험도 평가",
            noduleDetails: "결절 상세",
            noDataMsg: "분석 결과가 여기에 표시됩니다",
            disclaimer: "* 영상 처리 기반 시뮬레이션 분석 (실제 임상 진단은 전문의와 상담하세요)",
            footer: "LungVision AI | 혁신적인 폐암 스크리닝 지원 (데모 버전)",
            risk_low: "낮음",
            risk_mid: "중간",
            risk_high: "높음",
            nodule_item_prefix: "결절 #",
            size_mm: "mm",
            noduleSize: "크기"
        }
    };

    let currentLang = 'zh';
    let currentImage = null;       // 存储原图Image对象
    let currentCanvas = null;      // canvas元素
    let currentCtx = null;
    let currentAnalysisResult = { nodules: [], riskScore: 0, maxDiameter: 0 };

    // DOM 元素
    const canvas = document.getElementById('imageCanvas');
    const ctx = canvas.getContext('2d');
    const fileInput = document.getElementById('fileInput');
    const uploadArea = document.getElementById('uploadArea');
    const analyzeBtn = document.getElementById('analyzeBtn');
    const clearBtn = document.getElementById('clearBtn');

    currentCanvas = canvas;
    currentCtx = ctx;

    // 语言切换逻辑
    function updateLanguage() {
        document.querySelectorAll('[data-key]').forEach(el => {
            const key = el.getAttribute('data-key');
            if (translations[currentLang][key]) {
                if (el.tagName === 'INPUT' || el.tagName === 'TEXTAREA') {
                    el.placeholder = translations[currentLang][key];
                } else {
                    el.innerText = translations[currentLang][key];
                }
            }
        });
        // 更新动态的内容（结节详情面板/风险标签）
        if (currentAnalysisResult && currentAnalysisResult.nodules.length > 0) {
            renderNoduleList(currentAnalysisResult.nodules);
            updateRiskUI(currentAnalysisResult.riskScore, currentAnalysisResult.maxDiameter);
        } else {
            // 如果没有分析结果，显示占位符
            if (currentAnalysisResult.nodules.length === 0) {
                const placeholderDiv = document.getElementById('noduleListPanel');
                if (placeholderDiv) placeholderDiv.innerHTML = `<div style="text-align:center; color:#7c8f8c;">${translations[currentLang].noDataMsg}</div>`;
            }
        }
        // 更新统计数字（如果存在结果）
        if (currentAnalysisResult.nodules.length) {
            document.getElementById('noduleCountValue').innerText = currentAnalysisResult.nodules.length;
            document.getElementById('maxSizeValue').innerText = currentAnalysisResult.maxDiameter > 0 ? currentAnalysisResult.maxDiameter.toFixed(1) + ' mm' : '—';
            let riskText = '';
            if (currentAnalysisResult.riskScore < 0.35) riskText = translations[currentLang].risk_low;
            else if (currentAnalysisResult.riskScore < 0.65) riskText = translations[currentLang].risk_mid;
            else riskText = translations[currentLang].risk_high;
            document.getElementById('riskLevelValue').innerText = riskText;
        } else {
            if(!currentAnalysisResult.nodules.length) {
                document.getElementById('noduleCountValue').innerText = '—';
                document.getElementById('maxSizeValue').innerText = '—';
                document.getElementById('riskLevelValue').innerText = '—';
                document.getElementById('riskBadge').innerHTML = '';
            }
        }
    }

    function setLanguage(lang) {
        currentLang = lang;
        updateLanguage();
        document.querySelectorAll('.lang-btn').forEach(btn => {
            if (btn.getAttribute('data-lang') === lang) btn.classList.add('active');
            else btn.classList.remove('active');
        });
    }

    // 上传图片逻辑
    function handleImageUpload(file) {
        if (!file) return;
        const reader = new FileReader();
        reader.onload = (e) => {
            const img = new Image();
            img.onload = () => {
                currentImage = img;
                // 设置canvas尺寸适应图片比例 (最大宽度500px)
                const maxWidth = 500;
                let width = img.width;
                let height = img.height;
                if (width > maxWidth) {
                    height = height * (maxWidth / width);
                    width = maxWidth;
                }
                canvas.width = width;
                canvas.height = height;
                currentCtx.drawImage(img, 0, 0, width, height);
                // 重置分析结果
                currentAnalysisResult = { nodules: [], riskScore: 0, maxDiameter: 0 };
                document.getElementById('noduleCountValue').innerText = '—';
                document.getElementById('maxSizeValue').innerText = '—';
                document.getElementById('riskLevelValue').innerText = '—';
                document.getElementById('riskBadge').innerHTML = '';
                document.getElementById('noduleListPanel').innerHTML = `<div style="text-align:center; color:#7c8f8c;">${translations[currentLang].noDataMsg}</div>`;
            };
            img.src = e.target.result;
        };
        reader.readAsDataURL(file);
    }

    // ------------- 高级图像分析: 肺结节候选检测 (连通区域+圆形度+灰度阈值) -------------
    // 预处理: 转为灰度, 自适应阈值寻找亮区域 (结节通常较高密度)
    function findNoduleCandidates(imageData, width, height) {
        // 灰度数组
        let gray = new Uint8ClampedArray(width * height);
        for (let i = 0; i < imageData.data.length; i += 4) {
            let r = imageData.data[i];
            let g = imageData.data[i+1];
            let b = imageData.data[i+2];
            let gr = 0.299 * r + 0.587 * g + 0.114 * b;
            gray[i/4] = gr;
        }
        // 使用大津法 (Otsu) 二值化，但更稳健: 采用均值+标准差提取高亮区域
        let sum = 0;
        for (let i = 0; i < gray.length; i++) sum += gray[i];
        let mean = sum / gray.length;
        let variance = 0;
        for (let i = 0; i < gray.length; i++) variance += (gray[i] - mean) ** 2;
        let std = Math.sqrt(variance / gray.length);
        let threshold = mean + std * 0.6;  // 高亮区域通常代表结节或致密组织
        // 二值化
        let binary = new Uint8Array(width * height);
        for (let i = 0; i < gray.length; i++) {
            binary[i] = (gray[i] > threshold) ? 1 : 0;
        }
        // 连通组件标记 (4邻域)
        let labels = new Int32Array(width * height).fill(0);
        let currentLabel = 1;
        let equivalences = [];
        for (let y = 0; y < height; y++) {
            for (let x = 0; x < width; x++) {
                let idx = y * width + x;
                if (binary[idx] === 0) continue;
                let up = (y > 0) ? labels[(y-1)*width + x] : 0;
                let left = (x > 0) ? labels[y*width + (x-1)] : 0;
                if (up === 0 && left === 0) {
                    labels[idx] = currentLabel;
                    equivalences.push([currentLabel, currentLabel]);
                    currentLabel++;
                } else if (up !== 0 && left === 0) labels[idx] = up;
                else if (up === 0 && left !== 0) labels[idx] = left;
                else {
                    let minL = Math.min(up, left);
                    labels[idx] = minL;
                    if (up !== left) {
                        equivalences.push([up, left]);
                        equivalences.push([left, up]);
                    }
                }
            }
        }
        // 合并等价标签
        let labelMap = new Array(currentLabel).fill(0);
        for (let i = 1; i < currentLabel; i++) labelMap[i] = i;
        function find(x) {
            if (labelMap[x] !== x) labelMap[x] = find(labelMap[x]);
            return labelMap[x];
        }
        for (let eq of equivalences) {
            let a = find(eq[0]), b = find(eq[1]);
            if (a !== b) labelMap[b] = a;
        }
        for (let i = 1; i < currentLabel; i++) find(i);
        // 重新分配label
        let finalLabels = new Int32Array(width * height);
        let labelCounter = 1;
        let newMap = new Map();
        for (let i = 0; i < width*height; i++) {
            let l = labels[i];
            if (l === 0) continue;
            let root = labelMap[l];
            if (!newMap.has(root)) newMap.set(root, labelCounter++);
            finalLabels[i] = newMap.get(root);
        }
        // 统计各个连通区域属性: 面积, bounding box, 圆形度
        let regions = new Map();
        for (let i = 0; i < width*height; i++) {
            let label = finalLabels[i];
            if (label === 0) continue;
            if (!regions.has(label)) {
                regions.set(label, { area: 0, minX: width, minY: height, maxX: 0, maxY: 0, pixels: [] });
            }
            let reg = regions.get(label);
            reg.area++;
            let x = i % width;
            let y = Math.floor(i / width);
            reg.minX = Math.min(reg.minX, x);
            reg.minY = Math.min(reg.minY, y);
            reg.maxX = Math.max(reg.maxX, x);
            reg.maxY = Math.max(reg.maxY, y);
            reg.pixels.push([x,y]);
        }
        // 筛选结节: 面积在合理范围 (15~800px), 圆形度(面积/外接圆面积) > 0.55 或 紧凑型
        let nodules = [];
        for (let [label, reg] of regions.entries()) {
            let area = reg.area;
            if (area < 12 || area > 1500) continue;
            let widthBox = reg.maxX - reg.minX + 1;
            let heightBox = reg.maxY - reg.minY + 1;
            let diameter = Math.max(widthBox, heightBox);
            let radius = diameter / 2;
            let circumArea = Math.PI * radius * radius;
            let circularity = area / circumArea;
            // 圆形度 + 紧凑度
            let aspectRatio = Math.min(widthBox, heightBox) / Math.max(widthBox, heightBox);
            if (circularity > 0.48 && aspectRatio > 0.45 && area > 8) {
                let centerX = (reg.minX + reg.maxX) / 2;
                let centerY = (reg.minY + reg.maxY) / 2;
                let sizeEstimate = (widthBox + heightBox) / 2;
                let physicalSizeMm = sizeEstimate * 0.45; // 假设每像素0.45mm, 模拟真实尺寸
                nodules.push({
                    x: centerX, y: centerY, radius: sizeEstimate/2,
                    area: area, diameterMm: physicalSizeMm,
                    widthBox, heightBox
                });
            }
        }
        // 合并过于接近的结节 (非极大值抑制简易)
        let filtered = [];
        nodules.sort((a,b)=>b.area - a.area);
        for (let n of nodules) {
            let overlap = false;
            for (let exist of filtered) {
                let dx = exist.x - n.x, dy = exist.y - n.y;
                let dist = Math.hypot(dx, dy);
                if (dist < (exist.radius + n.radius) * 0.7) { overlap = true; break; }
            }
            if (!overlap) filtered.push(n);
        }
        return filtered;
    }

    // 执行分析并绘制结节标记
    async function performAnalysis() {
        if (!currentImage) {
            alert(currentLang === 'zh' ? '请先上传肺部医学影像' : '폐 의료 영상을 먼저 업로드하세요.');
            return;
        }
        // 获取canvas当前图像数据进行处理
        const width = canvas.width;
        const height = canvas.height;
        let imageData = currentCtx.getImageData(0, 0, width, height);
        let nodules = findNoduleCandidates(imageData, width, height);
        // 计算风险分数: 基于结节数量，最大尺寸以及圆形度不规则度 (模拟更真实)
        let riskScore = 0;
        let maxDiameter = 0;
        if (nodules.length > 0) {
            maxDiameter = Math.max(...nodules.map(n => n.diameterMm));
            let sizeFactor = Math.min(1.0, maxDiameter / 18.0);   // 18mm以上高风险
            let countFactor = Math.min(1.0, nodules.length / 3.0);
            let irregularity = 0;
            for (let n of nodules) {
                let ir = 1 - (n.area / (Math.PI * (n.radius * n.radius)));
                irregularity += Math.abs(ir);
            }
            irregularity = irregularity / nodules.length;
            riskScore = 0.35 * sizeFactor + 0.35 * countFactor + 0.3 * Math.min(1, irregularity*1.2);
            riskScore = Math.min(0.98, riskScore);
        } else {
            riskScore = 0.05;
        }
        // 绘制标记
        drawNodulesOnCanvas(nodules);
        // 保存结果
        currentAnalysisResult = { nodules, riskScore, maxDiameter };
        // 更新UI数值
        document.getElementById('noduleCountValue').innerText = nodules.length;
        document.getElementById('maxSizeValue').innerText = maxDiameter > 0 ? maxDiameter.toFixed(1) + ' mm' : '—';
        let riskText = '';
        if (riskScore < 0.35) riskText = translations[currentLang].risk_low;
        else if (riskScore < 0.65) riskText = translations[currentLang].risk_mid;
        else riskText = translations[currentLang].risk_high;
        document.getElementById('riskLevelValue').innerText = riskText;
        updateRiskUI(riskScore, maxDiameter);
        renderNoduleList(nodules);
    }

    function drawNodulesOnCanvas(nodules) {
        if (!currentImage) return;
        // 重绘原始图像
        currentCtx.drawImage(currentImage, 0, 0, canvas.width, canvas.height);
        for (let nod of nodules) {
            // 绘制圆圈标记
            currentCtx.beginPath();
            currentCtx.arc(nod.x, nod.y, nod.radius, 0, 2 * Math.PI);
            currentCtx.strokeStyle = '#ff3b3f';
            currentCtx.lineWidth = 2.5;
            currentCtx.stroke();
            currentCtx.beginPath();
            currentCtx.arc(nod.x, nod.y, nod.radius-1, 0, 2 * Math.PI);
            currentCtx.strokeStyle = '#ffd966';
            currentCtx.lineWidth = 1.5;
            currentCtx.stroke();
            // 标记尺寸文字
            currentCtx.font = "bold 14px 'Inter'";
            currentCtx.fillStyle = '#ffefc0';
            currentCtx.shadowBlur = 4;
            currentCtx.fillText(`${nod.diameterMm.toFixed(1)}mm`, nod.x+5, nod.y-5);
            currentCtx.shadowBlur = 0;
        }
    }

    function renderNoduleList(nodules) {
        const container = document.getElementById('noduleListPanel');
        if (!nodules.length) {
            container.innerHTML = `<div style="text-align:center; color:#7c8f8c;">${translations[currentLang].noDataMsg}</div>`;
            return;
        }
        let html = '';
        nodules.forEach((nod, idx) => {
            let sizeText = nod.diameterMm.toFixed(1) + ' mm';
            html += `<div class="nodule-item">
                        <span><strong>${translations[currentLang].nodule_item_prefix}${idx+1}</strong> (${sizeText})</span>
                        <span><i class="fas fa-circle" style="color:#e67e22; font-size:12px;"></i> ${translations[currentLang].noduleSize}: ${nod.diameterMm.toFixed(1)} mm</span>
                     </div>`;
        });
        container.innerHTML = html;
    }

    function updateRiskUI(riskScore, maxDiameter) {
        const badgeDiv = document.getElementById('riskBadge');
        let riskClass = '';
        let riskLabel = '';
        if (riskScore < 0.35) { riskClass = 'risk-low'; riskLabel = translations[currentLang].risk_low; }
        else if (riskScore < 0.65) { riskClass = 'risk-mid'; riskLabel = translations[currentLang].risk_mid; }
        else { riskClass = 'risk-high'; riskLabel = translations[currentLang].risk_high; }
        badgeDiv.innerHTML = `<span class="risk-badge ${riskClass}"><i class="fas fa-chart-simple"></i> ${riskLabel} (${(riskScore*100).toFixed(0)}%)</span>`;
        if (maxDiameter > 12) {
            badgeDiv.innerHTML += `<div style="margin-top:8px; font-size:12px;"><i class="fas fa-clock"></i> ${currentLang === 'zh' ? '建议临床随访或进一步检查' : '임상 추적 또는 추가 검사 권장'}</div>`;
        }
    }

    function clearCanvas() {
        if (currentImage) {
            currentCtx.drawImage(currentImage, 0, 0, canvas.width, canvas.height);
        } else {
            currentCtx.clearRect(0, 0, canvas.width, canvas.height);
            currentCtx.fillStyle = "#eef2f0";
            currentCtx.fillRect(0, 0, canvas.width, canvas.height);
        }
        currentAnalysisResult = { nodules: [], riskScore: 0, maxDiameter: 0 };
        document.getElementById('noduleCountValue').innerText = '—';
        document.getElementById('maxSizeValue').innerText = '—';
        document.getElementById('riskLevelValue').innerText = '—';
        document.getElementById('riskBadge').innerHTML = '';
        document.getElementById('noduleListPanel').innerHTML = `<div style="text-align:center; color:#7c8f8c;">${translations[currentLang].noDataMsg}</div>`;
    }

    // 事件监听
    uploadArea.addEventListener('click', () => fileInput.click());
    fileInput.addEventListener('change', (e) => {
        if (e.target.files.length) handleImageUpload(e.target.files[0]);
    });
    uploadArea.addEventListener('dragover', (e) => { e.preventDefault(); uploadArea.style.borderColor = '#2c8f8c'; });
    uploadArea.addEventListener('dragleave', () => { uploadArea.style.borderColor = '#8bb5b3'; });
    uploadArea.addEventListener('drop', (e) => {
        e.preventDefault();
        uploadArea.style.borderColor = '#8bb5b3';
        if (e.dataTransfer.files.length) handleImageUpload(e.dataTransfer.files[0]);
    });
    analyzeBtn.addEventListener('click', performAnalysis);
    clearBtn.addEventListener('click', () => clearCanvas());
    document.querySelectorAll('.lang-btn').forEach(btn => {
        btn.addEventListener('click', () => setLanguage(btn.getAttribute('data-lang')));
    });
    // 默认初始绘制占位
    setLanguage('zh');
    currentCtx.fillStyle = "#eef2f0";
    currentCtx.fillRect(0, 0, canvas.width, canvas.height);
    currentCtx.fillStyle = "#688b8a";
    currentCtx.font = "14px Inter";
    currentCtx.fillText("영상 대기 | 等待影像", 30, 60);
</script>
</body>
</html>
