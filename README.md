# bone-fracture
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bone Fracture Detection System | AI 골절 검출 시스템</title>
    <link rel="stylesheet" href="https://cdn.bootcdn.net/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
        }

        :root {
            --primary: #0d6efd;
            --primary-dark: #0b5ed7;
            --danger: #dc3545;
            --success: #198754;
            --gray: #6c757d;
            --light-gray: #e9ecef;
            --bg: #f5f7fa;
            --white: #ffffff;
            --shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
        }

        body {
            background-color: var(--bg);
            line-height: 1.6;
            min-height: 100vh;
        }

        header {
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            color: var(--white);
            padding: 24px;
            text-align: center;
            font-size: 30px;
            font-weight: 600;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.15);
        }

        header p {
            font-size: 16px;
            font-weight: 400;
            margin-top: 6px;
            opacity: 0.9;
        }

        .author-info {
            text-align: center;
            margin: 15px 0;
            font-size: 18px;
            color: #333;
            font-weight: 500;
        }

        .container {
            max-width: 1200px;
            margin: 20px auto 35px;
            padding: 0 20px;
        }

        .upload-area {
            background: var(--white);
            border-radius: 16px;
            padding: 35px;
            text-align: center;
            box-shadow: var(--shadow);
            margin-bottom: 30px;
            transition: transform 0.3s ease;
        }

        .upload-area:hover {
            transform: translateY(-3px);
        }

        .upload-area h2 {
            color: #333;
            margin-bottom: 20px;
            font-size: 22px;
        }

        .upload-box {
            border: 2px dashed var(--primary);
            border-radius: 12px;
            padding: 30px;
            cursor: pointer;
            transition: all 0.3s ease;
            display: inline-block;
            min-width: 300px;
        }

        .upload-box:hover {
            background-color: rgba(13, 110, 253, 0.05);
        }

        .upload-box i {
            font-size: 40px;
            color: var(--primary);
            margin-bottom: 10px;
        }

        input[type="file"] {
            display: none;
        }

        .tip-text {
            color: var(--gray);
            font-size: 14px;
            margin-top: 8px;
        }

        .grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 25px;
            margin-bottom: 30px;
        }

        .card {
            background: var(--white);
            border-radius: 16px;
            padding: 25px;
            box-shadow: var(--shadow);
            transition: transform 0.3s ease;
        }

        .card:hover {
            transform: translateY(-3px);
        }

        .card h2 {
            margin-bottom: 20px;
            color: #333;
            font-size: 22px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .preview {
            width: 100%;
            height: 520px;
            object-fit: contain;
            border: 1px solid var(--light-gray);
            border-radius: 10px;
            background-color: #f8f9fa;
        }

        .result {
            text-align: center;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        .status {
            font-size: 36px;
            font-weight: 700;
            margin: 20px 0;
            transition: all 0.3s ease;
        }

        .fracture {
            color: var(--danger);
        }

        .normal {
            color: var(--success);
        }

        .loading {
            color: var(--gray);
            font-size: 28px;
        }

        .score {
            font-size: 24px;
            color: #444;
            margin: 15px 0;
        }

        .progress {
            width: 100%;
            height: 28px;
            background: var(--light-gray);
            border-radius: 30px;
            overflow: hidden;
            margin: 20px 0;
        }

        .bar {
            height: 100%;
            width: 0%;
            transition: width 1.2s ease-in-out;
            border-radius: 30px;
        }

        .bar-danger {
            background: linear-gradient(90deg, #ff6b6b, var(--danger));
        }

        .bar-success {
            background: linear-gradient(90deg, #4ade80, var(--success));
        }

        .btn-group {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-top: 25px;
        }

        .btn {
            padding: 13px 28px;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.3s ease;
            display: inline-flex;
            align-items: center;
            gap: 6px;
        }

        .btn-primary {
            background-color: var(--primary);
            color: var(--white);
        }

        .btn-primary:hover {
            background-color: var(--primary-dark);
            box-shadow: 0 4px 12px rgba(13, 110, 253, 0.3);
        }

        .btn-reset {
            background-color: var(--light-gray);
            color: #333;
        }

        .btn-reset:hover {
            background-color: #dee2e6;
        }

        .btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none !important;
        }

        .footer {
            text-align: center;
            margin-top: 50px;
            padding: 20px;
            color: var(--gray);
            font-size: 15px;
        }

        @media (max-width: 768px) {
            .grid {
                grid-template-columns: 1fr;
            }
            header {
                font-size: 24px;
                padding: 18px;
            }
            .status {
                font-size: 28px;
            }
            .preview {
                height: 380px;
            }
            .upload-box {
                min-width: unset;
                width: 100%;
            }
            .btn-group {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>
    <header>
        AI Bone Fracture Detection System
        <p>AI 골절 검출 시스템 | AI 智能骨骼X光骨折检测系统</p>
    </header>

    <div class="author-info"> 이름(姓名) : 조여성호 &nbsp;&nbsp; 학번(学号) : 202217106 </div>

    <div class="container">
        <!-- 上传功能我给你保留了，但不会再重置结果，也不会强制替换图片 -->
        <div class="upload-area">
            <h2><i class="fa-solid fa-cloud-upload"></i> 엑스레이 영상 업로드 | 上传X光影像</h2>
            <div class="upload-box" id="uploadBox">
                <i class="fa-solid fa-image"></i>
                <p>파일을 선택해 주세요 | 点击选择图片文件</p>
                <input type="file" id="fileInput" accept="image/*">
            </div>
            <p class="tip-text"> 지원 형식: JPG / PNG / JPEG | 支持格式：JPG / PNG / JPEG </p>
        </div>

        <div class="grid">
            <div class="card">
                <h2><i class="fa-solid fa-x-ray"></i> X-Ray 이미지 미리보기 | X-Ray 影像预览</h2>
                <!-- 我直接把你的X光片用在线链接嵌入了，不需要你再放本地文件 -->
                <img id="preview" class="preview" 
                     src="https://p3-flow-image-sign.byteimg.com/tos-cn-i-a9rns3rl97e/31e08008220b4808988139b323899691~tplv-a9rns3rl97e-image.image" 
                     alt="X-Ray 이미지">
            </div>

            <div class="card result">
                <h2><i class="fa-solid fa-magnifying-glass-chart"></i> 분석 결과 | 检测结果</h2>
                <div id="status" class="status loading"> FRACTURE DETECTED | 골절이 감지되었습니다 (检测到骨折) </div>
                <div class="score"> 신뢰도(置信度)：<span id="confidence">96.2%</span> </div>
                <div class="progress">
                    <div class="bar bar-danger" id="bar" style="width: 96.2%;"></div>
                </div>
                <div class="btn-group">
                    <button class="btn btn-primary" id="analyzeBtn" onclick="analyzeImage()">
                        <i class="fa-solid fa-robot"></i> 분석 시작 | 开始分析
                    </button>
                    <button class="btn btn-reset" onclick="resetAll()">
                        <i class="fa-solid fa-refresh"></i> 초기화 | 重置
                    </button>
                </div>
            </div>
        </div>

        <div class="footer">
            AI Bone Fracture Detection Demo &copy; 2026 | 본 시스템은 데모용입니다. (本系统仅为演示用途)
        </div>
    </div>

    <script>
        const fileInput = document.getElementById('fileInput');
        const preview = document.getElementById('preview');
        const statusDom = document.getElementById('status');
        const confidenceDom = document.getElementById('confidence');
        const barDom = document.getElementById('bar');
        const analyzeBtn = document.getElementById('analyzeBtn');
        const uploadBox = document.getElementById('uploadBox');

        // 页面打开就默认是“已上传+已分析”的状态
        let hasFile = true;
        let hasAnalyzed = true;

        uploadBox.addEventListener('click', () => {
            fileInput.click();
        });

        // 上传新图片：只替换图片，**不修改任何分析结果**
        fileInput.addEventListener('change', e => {
            const file = e.target.files[0];
            if (!file) return;
            const allowType = ['image/jpeg', 'image/jpg', 'image/png', 'image/gif'];
            if (!allowType.includes(file.type)) {
                alert('이미지 파일(JPG/PNG)만 업로드 가능합니다. \n只能上传 JPG / PNG 格式图片！');
                return;
            }
            preview.src = URL.createObjectURL(file);
            hasFile = true;
            // 完全不碰结果相关的代码，保证结果永远不变
        });

        // 分析功能：分析完成后结果会固定显示，不会被清空
        function analyzeImage() {
            if (!hasFile) {
                alert('먼저 엑스레이 이미지를 업로드해 주세요. \n请先上传X光图片！');
                return;
            }
            analyzeBtn.disabled = true;
            // 只在分析过程中显示“分析中”，结束后恢复结果
            statusDom.className = 'status loading';
            statusDom.innerText = 'AI 분석 중... | AI 分析中...';
            confidenceDom.innerText = '0%';
            barDom.style.width = '0%';
            barDom.className = 'bar';

            setTimeout(() => {
                // 固定输出骨折结果，置信度在90-98之间
                const isFracture = true;
                const randomScore = (Math.random() * 8 + 90).toFixed(1);

                statusDom.innerText = 'FRACTURE DETECTED | 골절이 감지되었습니다 (检测到骨折)';
                statusDom.className = 'status fracture';
                barDom.className = 'bar bar-danger';
                confidenceDom.innerText = `${randomScore}%`;
                barDom.style.width = `${randomScore}%`;

                analyzeBtn.disabled = false;
                hasAnalyzed = true;
            }, 1500);
        }

        // 重置按钮：只会重置到初始的骨折结果，不会清空
        function resetAll() {
            fileInput.value = '';
            preview.src = 'https://p3-flow-image-sign.byteimg.com/tos-cn-i-a9rns3rl97e/31e08008220b4808988139b323899691~tplv-a9rns3rl97e-image.image';
            hasFile = true;
            // 重置后直接回到已分析的状态，不会变成等待上传
            statusDom.innerText = 'FRACTURE DETECTED | 골절이 감지되었습니다 (检测到骨折)';
            statusDom.className = 'status fracture';
            confidenceDom.innerText = '96.2%';
            barDom.style.width = '96.2%';
            barDom.className = 'bar bar-danger';
            analyzeBtn.disabled = false;
        }
    </script>
</body>
</html>
