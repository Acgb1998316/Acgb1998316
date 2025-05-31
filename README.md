<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>成菜演播厅 - 发酵智慧与菌群搭配艺术</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@3.2.31/dist/vue.global.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/pinia@2.0.14/dist/pinia.global.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/echarts@5.3.3/dist/echarts.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.1.1/css/all.min.css">
    <style>
        :root {
            --primary: #8b5a2b;
            --secondary: #d4a76a;
            --accent: #a67c52;
            --dark: #4d2e1d;
            --light: #f5e9d9;
            --success: #4caf50;
            --warning: #ff9800;
            --danger: #f44336;
            --info: #2196f3;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #f9f3e9, #f5e9d9);
            color: var(--dark);
            min-height: 100vh;
            padding: 20px;
        }
        
        .app-container {
            max-width: 1400px;
            margin: 0 auto;
        }
        
        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 20px 0;
            margin-bottom: 30px;
            border-bottom: 2px solid var(--secondary);
        }
        
        .logo {
            display: flex;
            align-items: center;
        }
        
        .logo-icon {
            font-size: 2.5rem;
            color: var(--primary);
            margin-right: 15px;
        }
        
        .logo-text {
            font-size: 1.8rem;
            font-weight: bold;
            color: var(--dark);
        }
        
        .subtitle {
            font-size: 1.2rem;
            color: var(--accent);
            font-style: italic;
            margin-top: 5px;
        }
        
        .auth-buttons {
            display: flex;
            gap: 15px;
        }
        
        .btn {
            padding: 10px 20px;
            border-radius: 30px;
            border: none;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .btn-primary {
            background: var(--primary);
            color: white;
        }
        
        .btn-outline {
            background: transparent;
            border: 2px solid var(--primary);
            color: var(--primary);
        }
        
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        
        .main-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-bottom: 40px;
        }
        
        .panel {
            background: white;
            border-radius: 15px;
            box-shadow: 0 8px 25px rgba(139, 90, 43, 0.15);
            padding: 25px;
            transition: transform 0.3s ease;
            border: 1px solid rgba(139, 90, 43, 0.1);
        }
        
        .panel:hover {
            transform: translateY(-5px);
        }
        
        .panel-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 2px solid var(--light);
        }
        
        .panel-title {
            font-size: 1.5rem;
            color: var(--primary);
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .panel-actions {
            display: flex;
            gap: 10px;
        }
        
        .icon-btn {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            background: var(--light);
            color: var(--primary);
            cursor: pointer;
            transition: all 0.3s ease;
            border: none;
        }
        
        .icon-btn:hover {
            background: var(--secondary);
            color: white;
        }
        
        .dish-showcase {
            grid-column: 1 / -1;
            height: 350px;
            position: relative;
            overflow: hidden;
            border-radius: 15px;
        }
        
        .dish-image {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.5s ease;
        }
        
        .dish-tags {
            position: absolute;
            bottom: 20px;
            left: 20px;
            display: flex;
            gap: 10px;
        }
        
        .tag {
            padding: 6px 15px;
            border-radius: 20px;
            background: rgba(255, 255, 255, 0.85);
            font-size: 0.9rem;
            font-weight: 600;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }
        
        .tag-primary {
            background: var(--primary);
            color: white;
        }
        
        .tag-warning {
            background: var(--warning);
            color: white;
        }
        
        .tag-success {
            background: var(--success);
            color: white;
        }
        
        .chart-container {
            height: 300px;
            width: 100%;
        }
        
        .ferment-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-top: 20px;
        }
        
        .ferment-item {
            background: var(--light);
            border-radius: 10px;
            padding: 15px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .ferment-item:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }
        
        .ferment-icon {
            font-size: 2rem;
            margin-bottom: 10px;
            color: var(--primary);
        }
        
        .simulator-area {
            display: grid;
            grid-template-columns: 1fr 2fr;
            gap: 20px;
            height: 400px;
        }
        
        .ingredients-panel {
            background: var(--light);
            border-radius: 12px;
            padding: 15px;
            overflow-y: auto;
        }
        
        .ingredient-list {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
        }
        
        .ingredient-item {
            background: white;
            padding: 12px;
            border-radius: 8px;
            display: flex;
            flex-direction: column;
            align-items: center;
            cursor: grab;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
        }
        
        .ingredient-icon {
            font-size: 1.8rem;
            margin-bottom: 8px;
            color: var(--accent);
        }
        
        .canvas-area {
            background: white;
            border: 2px dashed var(--secondary);
            border-radius: 12px;
            position: relative;
            overflow: hidden;
        }
        
        .draft-item {
            background: white;
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 15px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.05);
            border-left: 4px solid var(--primary);
        }
        
        .draft-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }
        
        .draft-title {
            font-weight: bold;
            color: var(--primary);
        }
        
        .draft-meta {
            font-size: 0.85rem;
            color: #777;
            margin-bottom: 10px;
        }
        
        .draft-tags {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
        }
        
        .draft-actions {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }
        
        .footer {
            text-align: center;
            padding: 30px 0;
            color: var(--accent);
            border-top: 1px solid rgba(139, 90, 43, 0.1);
            margin-top: 30px;
        }
        
        @media (max-width: 992px) {
            .main-content {
                grid-template-columns: 1fr;
            }
            
            .simulator-area {
                grid-template-columns: 1fr;
                height: auto;
            }
        }
    </style>
</head>
<body>
    <div id="app">
        <div class="app-container">
            <header>
                <div class="logo">
                    <div class="logo-icon">
                        <i class="fas fa-mortar-pestle"></i>
                    </div>
                    <div>
                        <div class="logo-text">成菜演播厅</div>
                        <div class="subtitle">从一道菜的风味出发，反推发酵智慧与菌群搭配艺术</div>
                    </div>
                </div>
                
                <div class="auth-buttons">
                    <button class="btn btn-outline">
                        <i class="fas fa-user"></i> 登录
                    </button>
                    <button class="btn btn-primary">
                        <i class="fas fa-cloud-upload-alt"></i> 发布配方
                    </button>
                </div>
            </header>
            
            <div class="main-content">
                <!-- 成菜演播视图 -->
                <div class="panel dish-showcase">
                    <img src="https://images.unsplash.com/photo-1555939594-58d7cb561ad1?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80" 
                         alt="发酵美食展示" class="dish-image">
                    <div class="dish-tags">
                        <div class="tag tag-primary">酸辣味型</div>
                        <div class="tag tag-warning">推荐午餐</div>
                        <div class="tag tag-success">益生菌丰富</div>
                    </div>
                    
                    <div class="panel-actions">
                        <button class="icon-btn">
                            <i class="fas fa-expand"></i>
                        </button>
                        <button class="icon-btn">
                            <i class="fas fa-volume-up"></i>
                        </button>
                        <button class="icon-btn">
                            <i class="fas fa-book-open"></i>
                        </button>
                    </div>
                </div>
                
                <!-- 味型结构雷达图 -->
                <div class="panel">
                    <div class="panel-header">
                        <h2 class="panel-title">
                            <i class="fas fa-chart-radar"></i> 味型结构雷达图
                        </h2>
                        <div class="panel-actions">
                            <button class="icon-btn">
                                <i class="fas fa-cog"></i>
                            </button>
                        </div>
                    </div>
                    
                    <div class="chart-container" ref="radarChart"></div>
                </div>
                
                <!-- 反推调味图谱 -->
                <div class="panel">
                    <div class="panel-header">
                        <h2 class="panel-title">
                            <i class="fas fa-project-diagram"></i> 反推调味图谱
                        </h2>
                        <div class="panel-actions">
                            <button class="icon-btn">
                                <i class="fas fa-expand"></i>
                            </button>
                        </div>
                    </div>
                    
                    <div class="chart-container" ref="backtraceChart"></div>
                    
                    <div class="ferment-grid">
                        <div class="ferment-item">
                            <div class="ferment-icon">
                                <i class="fas fa-seedling"></i>
                            </div>
                            <div>原料层</div>
                        </div>
                        <div class="ferment-item">
                            <div class="ferment-icon">
                                <i class="fas fa-bacteria"></i>
                            </div>
                            <div>菌种层</div>
                        </div>
                        <div class="ferment-item">
                            <div class="ferment-icon">
                                <i class="fas fa-flask"></i>
                            </div>
                            <div>调味料形成</div>
                        </div>
                        <div class="ferment-item">
                            <div class="ferment-icon">
                                <i class="fas fa-utensils"></i>
                            </div>
                            <div>味感贡献</div>
                        </div>
                        <div class="ferment-item">
                            <div class="ferment-icon">
                                <i class="fas fa-clock"></i>
                            </div>
                            <div>发酵时间轴</div>
                        </div>
                        <div class="ferment-item">
                            <div class="ferment-icon">
                                <i class="fas fa-thermometer-half"></i>
                            </div>
                            <div>温湿度控制</div>
                        </div>
                    </div>
                </div>
                
                <!-- 调味配器交互台 -->
                <div class="panel">
                    <div class="panel-header">
                        <h2 class="panel-title">
                            <i class="fas fa-blender"></i> 调味配器交互台
                        </h2>
                        <div class="panel-actions">
                            <button class="icon-btn">
                                <i class="fas fa-redo"></i>
                            </button>
                        </div>
                    </div>
                    
                    <div class="simulator-area">
                        <div class="ingredients-panel">
                            <h3 style="margin-bottom: 15px;">可用配料</h3>
                            <div class="ingredient-list">
                                <div class="ingredient-item" draggable="true">
                                    <div class="ingredient-icon">
                                        <i class="fas fa-pepper-hot"></i>
                                    </div>
                                    <div>辣椒酱</div>
                                </div>
                                <div class="ingredient-item" draggable="true">
                                    <div class="ingredient-icon">
                                        <i class="fas fa-apple-alt"></i>
                                    </div>
                                    <div>苹果醋</div>
                                </div>
                                <div class="ingredient-item" draggable="true">
                                    <div class="ingredient-icon">
                                        <i class="fas fa-jar"></i>
                                    </div>
                                    <div>味噌</div>
                                </div>
                                <div class="ingredient-item" draggable="true">
                                    <div class="ingredient-icon">
                                        <i class="fas fa-cheese"></i>
                                    </div>
                                    <div>豆腐乳</div>
                                </div>
                                <div class="ingredient-item" draggable="true">
                                    <div class="ingredient-icon">
                                        <i class="fas fa-dna"></i>
                                    </div>
                                    <div>乳酸菌</div>
                                </div>
                                <div class="ingredient-item" draggable="true">
                                    <div class="ingredient-icon">
                                        <i class="fas fa-dna"></i>
                                    </div>
                                    <div>酵母菌</div>
                                </div>
                            </div>
                        </div>
                        
                        <div class="canvas-area" id="mixerCanvas">
                            <div style="position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); color: #d4a76a; text-align: center;">
                                <i class="fas fa-arrow-down" style="font-size: 2rem; margin-bottom: 10px;"></i>
                                <p>拖拽配料到此处开始调配</p>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- 创新组合记录器 -->
                <div class="panel">
                    <div class="panel-header">
                        <h2 class="panel-title">
                            <i class="fas fa-lightbulb"></i> 创新组合记录器
                        </h2>
                        <div class="panel-actions">
                            <button class="icon-btn">
                                <i class="fas fa-plus"></i>
                            </button>
                        </div>
                    </div>
                    
                    <div class="draft-list">
                        <div class="draft-item">
                            <div class="draft-header">
                                <div class="draft-title">凉拌三色豆腐乳拼盘</div>
                                <div class="draft-status">草稿</div>
                            </div>
                            <div class="draft-meta">
                                <span>最后更新: 2023-06-15 14:30</span>
                            </div>
                            <div class="draft-tags">
                                <span class="tag">酸辣型</span>
                                <span class="tag">豆腐乳</span>
                                <span class="tag">乳酸菌</span>
                            </div>
                            <div class="draft-actions">
                                <button class="btn btn-outline" style="padding: 6px 12px;">
                                    <i class="fas fa-edit"></i> 编辑
                                </button>
                                <button class="btn btn-primary" style="padding: 6px 12px;">
                                    <i class="fas fa-share-alt"></i> 发布
                                </button>
                            </div>
                        </div>
                        
                        <div class="draft-item">
                            <div class="draft-header">
                                <div class="draft-title">韩式泡菜炖豆腐</div>
                                <div class="draft-status" style="color: var(--success);">已发布</div>
                            </div>
                            <div class="draft-meta">
                                <span>发布于: 2023-06-10 09:15</span>
                                <span>| 点赞: 124</span>
                            </div>
                            <div class="draft-tags">
                                <span class="tag">鲜辣型</span>
                                <span class="tag">泡菜</span>
                                <span class="tag">酵母菌</span>
                            </div>
                            <div class="draft-actions">
                                <button class="btn btn-outline" style="padding: 6px 12px;">
                                    <i class="fas fa-chart-bar"></i> 分析
                                </button>
                                <button class="btn btn-primary" style="padding: 6px 12px;">
                                    <i class="fas fa-copy"></i> 复制
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="footer">
                <p>成菜演播厅 © 2023 | 探索发酵智慧与菌群搭配的艺术</p>
            </div>
        </div>
    </div>

    <script>
        const { createApp } = Vue;
        const { createPinia } = Pinia;
        
        const app = createApp({
            mounted() {
                this.initRadarChart();
                this.initBacktraceChart();
                this.setupDragAndDrop();
            },
            methods: {
                initRadarChart() {
                    const chartDom = this.$refs.radarChart;
                    const chart = echarts.init(chartDom);
                    
                    const option = {
                        title: {
                            text: '菜品味型分析',
                            left: 'center'
                        },
                        tooltip: {},
                        legend: {
                            data: ['当前菜品', '标准值'],
                            bottom: 10
                        },
                        radar: {
                            indicator: [
                                { name: '酸度', max: 100 },
                                { name: '甜度', max: 100 },
                                { name: '咸度', max: 100 },
                                { name: '鲜度', max: 100 },
                                { name: '辣度', max: 100 },
                                { name: '发酵感', max: 100 },
                                { name: '清爽度', max: 100 }
                            ],
                            radius: '65%',
                            splitNumber: 5,
                            axisName: {
                                color: '#333',
                                backgroundColor: 'none',
                                borderRadius: 3,
                                padding: [3, 5]
                            }
                        },
                        series: [{
                            type: 'radar',
                            data: [
                                {
                                    value: [85, 30, 65, 90, 75, 95, 60],
                                    name: '当前菜品',
                                    symbol: 'circle',
                                    symbolSize: 8,
                                    lineStyle: {
                                        width: 2,
                                        color: '#d4a76a'
                                    },
                                    areaStyle: {
                                        color: 'rgba(212, 167, 106, 0.3)'
                                    }
                                },
                                {
                                    value: [70, 50, 70, 75, 65, 80, 70],
                                    name: '标准值',
                                    symbol: 'circle',
                                    symbolSize: 8,
                                    lineStyle: {
                                        width: 2,
                                        color: '#8b5a2b'
                                    },
                                    areaStyle: {
                                        color: 'rgba(139, 90, 43, 0.1)'
                                    }
                                }
                            ]
                        }]
                    };
                    
                    chart.setOption(option);
                    
                    // 响应窗口大小变化
                    window.addEventListener('resize', function() {
                        chart.resize();
                    });
                },
                
                initBacktraceChart() {
                    const chartDom = this.$refs.backtraceChart;
                    const chart = echarts.init(chartDom);
                    
                    const option = {
                        title: {
                            text: '发酵链路图谱',
                            left: 'center'
                        },
                        tooltip: {
                            trigger: 'item',
                            triggerOn: 'mousemove'
                        },
                        animation: false,
                        series: [
                            {
                                type: 'sankey',
                                layout: 'none',
                                emphasis: {
                                    focus: 'adjacency'
                                },
                                data: [
                                    { name: '黄豆' },
                                    { name: '米曲霉' },
                                    { name: '盐水' },
                                    { name: '发酵' },
                                    { name: '味噌' },
                                    { name: '鲜味' },
                                    { name: '咸味' },
                                    { name: '发酵感' }
                                ],
                                links: [
                                    { source: '黄豆', target: '发酵', value: 10 },
                                    { source: '米曲霉', target: '发酵', value: 8 },
                                    { source: '盐水', target: '发酵', value: 6 },
                                    { source: '发酵', target: '味噌', value: 12 },
                                    { source: '味噌', target: '鲜味', value: 7 },
                                    { source: '味噌', target: '咸味', value: 5 },
                                    { source: '味噌', target: '发酵感', value: 8 }
                                ],
                                lineStyle: {
                                    color: 'gradient',
                                    curveness: 0.5
                                }
                            }
                        ]
                    };
                    
                    chart.setOption(option);
                    
                    // 响应窗口大小变化
                    window.addEventListener('resize', function() {
                        chart.resize();
                    });
                },
                
                setupDragAndDrop() {
                    const canvas = document.getElementById('mixerCanvas');
                    const draggables = document.querySelectorAll('.ingredient-item');
                    
                    draggables.forEach(draggable => {
                        draggable.addEventListener('dragstart', (e) => {
                            e.dataTransfer.setData('text/plain', draggable.innerText);
                            draggable.classList.add('dragging');
                        });
                        
                        draggable.addEventListener('dragend', () => {
                            draggable.classList.remove('dragging');
                        });
                    });
                    
                    canvas.addEventListener('dragover', (e) => {
                        e.preventDefault();
                        canvas.style.borderColor = '#8b5a2b';
                        canvas.style.backgroundColor = 'rgba(139, 90, 43, 0.05)';
                    });
                    
                    canvas.addEventListener('dragleave', () => {
                        canvas.style.borderColor = '#d4a76a';
                        canvas.style.backgroundColor = '';
                    });
                    
                    canvas.addEventListener('drop', (e) => {
                        e.preventDefault();
                        const data = e.dataTransfer.getData('text/plain');
                        
                        // 创建新的元素添加到画布
                        const newElement = document.createElement('div');
                        newElement.className = 'ingredient-item';
                        newElement.style.position = 'absolute';
                        newElement.style.left = (e.offsetX - 40) + 'px';
                        newElement.style.top = (e.offsetY - 40) + 'px';
                        newElement.style.cursor = 'move';
                        
                        // 找到对应的图标
                        const iconMap = {
                            '辣椒酱': 'fa-pepper-hot',
                            '苹果醋': 'fa-apple-alt',
                            '味噌': 'fa-jar',
                            '豆腐乳': 'fa-cheese',
                            '乳酸菌': 'fa-dna',
                            '酵母菌': 'fa-dna'
                        };
                        
                        newElement.innerHTML = `
                            <div class="ingredient-icon">
                                <i class="fas ${iconMap[data] || 'fa-question'}"></i>
                            </div>
                            <div>${data}</div>
                        `;
                        
                        canvas.appendChild(newElement);
                        
                        // 重置画布样式
                        canvas.style.borderColor = '#d4a76a';
                        canvas.style.backgroundColor = '';
                        
                        // 添加拖动功能
                        this.makeDraggable(newElement);
                    });
                },
                
                makeDraggable(element) {
                    let pos1 = 0, pos2 = 0, pos3 = 0, pos4 = 0;
                    
                    element.onmousedown = dragMouseDown;
                    
                    function dragMouseDown(e) {
                        e.preventDefault();
                        // 获取鼠标位置
                        pos3 = e.clientX;
                        pos4 = e.clientY;
                        document.onmouseup = closeDragElement;
                        // 调用函数移动元素
                        document.onmousemove = elementDrag;
                    }
                    
                    function elementDrag(e) {
                        e.preventDefault();
                        // 计算新位置
                        pos1 = pos3 - e.clientX;
                        pos2 = pos4 - e.clientY;
                        pos3 = e.clientX;
                        pos4 = e.clientY;
                        // 设置元素新位置
                        element.style.top = (element.offsetTop - pos2) + "px";
                        element.style.left = (element.offsetLeft - pos1) + "px";
                    }
                    
                    function closeDragElement() {
                        // 停止移动
                        document.onmouseup = null;
                        document.onmousemove = null;
                    }
                }
            }
        });
        
        const pinia = createPinia();
        app.use(pinia);
        
        app.mount('#app');
    </script>
</body>
</html>
